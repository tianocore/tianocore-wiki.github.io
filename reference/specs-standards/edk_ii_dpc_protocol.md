# Deferred Procedure Call Protocol

This document introduces the Deferred Procedure Call (DPC) protocol, including the protocol interface, design intension
and code analysis in edk2 network drivers. There is also some discussion about the assumptions, limitations and rules
that user must know before consuming the DPC protocol.

## What is DPC?

The Deferred Procedure Call Protocol provides a method for an UEFI Driver to queue a DPC Procedure when the caller is
executing at elevated TPLs, and dispatch it at a later time and at a specified TPL level.

DPC is typically used by UEFI Drivers that have event notification functions that execute at high TPL levels, and
require the use of services that must be executed at lower TPL levels. The event notification function can queue a DPC
Procedure with this protocol, and the DPC Procedure can be invoked when the TPL is lowered to a level that is less than
or equal to the TPL level required for the DPC Procedure to run correctly.

The DPC protocol is not a standardized protocol in UEFI specification; it is an edk2 specific implementation and defined
in *MdeModulePkg/Include/Protocol/Dpc.h*.

The DPC protocol only has 2 interfaces: `QueueDpc()` and `DispatchDpc()`.

```c
typedef
EFI_STATUS
(EFIAPI *EFI_DPC_QUEUE_DPC)(
  IN EFI_DPC_PROTOCOL   *This,
  IN EFI_TPL            DpcTpl,
  IN EFI_DPC_PROCEDURE  DpcProcedure,
  IN VOID               *DpcContext    OPTIONAL
  );
```

The function `QueueDpc()` queues the DPC Procedure specified by *DpcProcedure* to be execute at a later time when the
TPL level is at or below the TPL level specified by *DpcTpl*. When *DpcProcedure* is invoked, the one parameter
specified by *DpcContext* is passed to *DpcProcedure*. This function is required to maintain a unique queue for every
legal EFI_TPL value. The DPC Procedure specified by *DpcProcedure* and *DpcContext* is placed at the end of the DPC
queue specified by *DpcTpl*.

The DPC driver does not take the responsibility to invoke the queued DPCs. Instead, the UEFI driver or application which
calls `QueueDpc()` is responsible for calling `DispatchDpc()` to dispatch them from lower TPL levels at an appropriate
time.

```c
typedef
EFI_STATUS
(EFIAPI *EFI_DPC_DISPATCH_DPC)(
  IN EFI_DPC_PROTOCOL  *This
  );
```

The function `DispatchDpc()` dispatches all the previously queued DPCs whose TPL level is greater than or equal to the
current TPL level. DPCs with the highest TPL level are dispatched before DPCs with lower TPL levels. Within a single TPL
level, the DPCs are dispatched in the order that they were queued by QueueDpc().

## What problem does DPC solve?

### UEFI iSCSI Layout

The DPC protocol is designed to solve the TPL issue in UEFI network stack. Take the iSCSI for example, the UEFI SCSI
stack lays on top of the TCP network stack, as shown in the following picture.

![UEFI SCSI
Layout](../../Projects/NetworkPkg/Images/Uefi_SCSI_Layout.png "UEFI
iSCSI Layout")

The network stack from MNP to TCP is asynchronous, that is, all data transmitting and receiving are conveyed by tokens.
Each token has an event which associates with a notify function. When the transmitting or receiving is done, the event
is signaled with the corresponding I/O status and optionally the data buffer.

The SCSI stack and block I/O drivers can be asynchronous or synchronous from UEFI Specification, we only discuss the
synchronous situation here.

The UEFI iSCSI driver provides the linkage between the asynchronous network stack and the synchronous SCSI stack. The
UEFI iSCSI driver is implemented using a synchronous solution, to avoid the performance hit by polling the network cards
in periodical timers for receiving frames.

### TPL issue in iSCSI

UEFI Specification states that all UEFI network stack protocols and their service binding protocols should be called
with TPL <= TPL_CALLBACK (see Table 24. TPL Restrictions in UEFI Specification v2.6).

Above TPL restrictions, plus the asynchronous interfaces and some specific nature of network, make the UEFI network
stack driver can only lock itself at TPL_CALLBACK. Take the ARP driver for example. Usually it will response the
received ARP requests with ARP replies. The TPL of notify function to parse the received ARP requests should be >=
TPL_CALLBACK because it need to access shared structures in ARP driver. When the ARP request is parsed and an ARP reply
should be sent out, Mnp->Transmit() would be called to transmit the ARP reply. The TPL for calling the Mnp->Transmit()
should be <= TPL_CALLBACK according to the TPL restrictions defined in the UEFI specification. As a result, these two
restrictions make the ARP driver can only run at TPL_CALLBACK.

UEFI Specification also states the Simple File System protocol should be called with TPL <= TPL_CALLBACK. Take FAT
driver as the example, it will lock itself at TPL_CALLBACK when accessing the shared structures, or in critical section
such as reading block, writing block, etc.

Now the problem comes out: once the Simple File System driver is controlling the iSCSI stack, and attempting to read a
block, iSCSI can NOT receive data from network stack any more. Actually, the data could be received into the buffer pool
in MNP driver when iSCSI is polling the network. However, as all the tokens of the upper network stack drivers are
created with TPL_CALLBACK, the notify function won't have the chance to run even though the receive event is already
signaled. In the other words, the synchronous data read function in iSCSI driver will continue polling the network
device at TPL_CALLBACK, and waiting the receive event's notify function, which is also a TPL_CALLBACK event, to be
execute first. As a result, the whole iSCSI stack falls into a deadlock, no packet could be delivered to the upper layer
drivers.

### How DPC solve the problem

The edk2 network stack solves this deadlock problem by introducing the DPC protocol. Whenever a driver want to call the
asynchronous transmit or receive function, it creates an event with TPL_NOTIFY instead of TPL_CALLBACK. The notify
function won't really process the packet, it only queues a new DPC Procedure and return. Later, the DPC Procedure will
be dispatched at TPL_CALLBACK, and complete the whole task of the packet processing. With this technique, the deadlock
is avoided by using a high level TPL event, and the packets are still processed in the required TPL_CALLBACK level.

![ARP Receive](../../Projects/NetworkPkg/Images/DPC_ARP_Receive.png "Use
DPC in ARP driver")

Still take the receive path of ARP for example. The ARP driver creates an event as the MNP receive token, whose notify
function is *ArpOnFrameRcvd()* and notify TPL set to *TPL_NOTIFY*. When the *Mnp->Poll()* receives a new packet and
signals this receive event, the *ArpOnFrameRcvd()* will execute immediately (because it's a TPL_NOTIFY event, which
could interrupt any network task whose TPL <= TPL_CALLBACK). The *ArpOnFrameRcvd()* calls `QueueDpc()` to queue a new
DPC Procedure *ArpOnFrameRcvdDpc()* at TPL_CALLBACK and return. Later, the *Mnp->Poll()* calls the `DispatchDpc()`, to
dispatch the DPC Procedure queued by the notify function of the receive token's event, which finally executes the
*ArpOnFrameRcvdDpc()* in this case.

## How does DPC relate to UEFI network stack

DPC protocol is not a standardized solution in UEFI specification. It adds additional complexity to the UEFI network
drivers since the code logic must be carefully arranged to queue and dispatch the DPCs to ensure the stack running
correctly (see the "Rules, limitations and assumptions" below). However, in order to solve the TPL lock issue we
discussed before, almost all edk2 network stack drivers adopt the DPC solution in the asynchronous interface, including
the transmitting and receiving path.

Although DPC protocol is designed to solve a particular problem in the UEFI network stack, it's not saying it can only
be used in the network stack. Any UEFI drivers and applications could also use the DPC protocol for solving similar
problem as long as needed.

## Library Interface (DpcLib)

Edk2 also provides a library interface DpcLib, which is a simple encapsulation of the DPC protocol. The library wraps
the 2 interfaces and accepts same parameters except the "*This*" pointer as the protocol interface. The library
interface is more user-friendly since it reduces a *LocateProtocol()* call from the user.

```ini
[LibraryClasses]
  # MdeModulePkg/Include/Library/DpcLib.h
  DpcLib|MdeModulePkg/Library/DxeDpcLib/DxeDpcLib.inf
```

The following code fragment shows a simple example of using the DpcLib.

```c
#include <Library/DpcLib.h>

/**
  DPC Procedure.
**/
VOID
EFIAPI
EventHandlerDpc (
  IN VOID      *Context
  )
{
  //
  // We are running at TPL_CALLBACK.
  // Data is really processed in this function.
  //
  ...
}

/**
  Event notify function.
**/
VOID
EFIAPI
EventHandler (
  IN EFI_EVENT Event,
  IN VOID      *Context
  )
{
  //
  // We are running at TPL_NOTIFY.
  // Queue EventHandlerDpc as a DPC Procedure at TPL_CALLBACK
  //
  QueueDpc (TPL_CALLBACK, EventHandlerDpc, Context);
}

/*
  Sample code to create event, signal event and dispatch the DPC Procedure.

  This function must be called at the TPL level <= TPL_CALLBACK.
*/
VOID
CodeSample (
  VOID
  )
{
  EFI_EVENT   Event;
  VOID        *Context;

  //
  // Create a TPL_NOTIFY event.
  //
  gBS->CreateEvent (
         EVT_NOTIFY_SIGNAL,
         TPL_NOTIFY,
         EventHandler,
         Context,
         &Event
         );

  //
  // Signal the event.
  //
  gBS->SignalEvent (Event);

  //
  // Dispatch the DPC Procedure.
  //
  DispatchDpc();
}
```

## Assumptions, Limitations and Rules when using DPC

The module writer must be familiar with the matters below about DPC before using it.

### DispatchDpc() can be Nested

If the *DispatchDpc()* is called again in a dispatched DPC Procedure, it will first execute the next DPC Procedure in
the queue, then return to the original code.

The writer of the DPC Procedure must realize that the function may be interrupted when it calls the *DispatchDpc()*
interface, or any other interfaces which eventually lead to a *DispatchDpc()*, such as to transmit a network packet,
which finally calls *Mnp->Transmit()* with a *DispatchDpc()* in it.

```c
/*
  Consider we have queued 3 DPC Procedures with same TPL level as below.
  DPC Queue: DpcFun_A -> DpcFun_B -> DpcFun_C
*/
DpcFun_A()
{
  Fun_A_Part1;
  DispatchDpc();
  Fun_A_Part2;
}

DpcFun_B()
{
  Fun_B_Part1;
  QueueDpc(TPL_CALLBACK, DpcFun_A, NULL);   // Add DpcFun_A() to the end of the DPC queue.
  Fun_B_Part2;
  DispatchDpc();
  Fun_B_Part3;
}

DpcFun_C()
{
  Fun_C;
}

/*
  Now if someone calls DispatchDpc(), the code flow will now looks as below:
*/
DispatchDpc()
{
  DpcFun_A()                  // 1st DPC Procedure in the queue.
  {
    Fun_A_Part1;
    DispatchDpc()
    {
      DpcFun_B()              // 2nd DPC Procedure in the queue.
      {
        Fun_B_Part1;
        QueueDpc(TPL_CALLBACK, DpcFun_A, NULL);   // Add DpcFun_A() to the end of the queue.
        Fun_B_Part2;
        DispatchDpc()
        {
          DpcFun_C()          // 3rd DPC Procedure in the queue.
          {
            Fun_C;
          }
          DpcFun_A()          // 4th DPC Procedure in the queue.
          {
            Fun_A_Part1;
            DispatchDpc();    // No more DPC Procedure now.
            Fun_A_Part2;
          }
        }
        Fun_B_Part3;
      }
    }
    Fun_A_Part2;
  }
}
```

### Never Poll an I/O device in a DPC Procedure

The writer should avoid polling the I/O device in a DPC Procedure, especially in the data read/receive path.

Take the network device for example. If there is already a network frame in the network device's receive ring, the
*Mnp->Poll()* will pick it from the device, signal event to queue a new DPC Procedure, and call *DispatchDpc()* to
deliver the packet to upper layer driver. This means if a DPC Procedure polls the network, a new network packet will be
delivered to the upper layer driver, which may finally run into the original DPC Procedure again. It will bring lots of
complexities to the upper layer driver, like a serial of nested DPC call, or an infinite recursion of
receive-poll-receive...

```c
MnpPoll()
{
  ...
  MnpReceive()
  {
    ...
    MnpInstanceDeliverPacket()
    {
      ...
      //
      // Signal event of IP driver's receive token, which will queue a new DPC
      // Procedure immediately, see Ip4OnFrameReceived() and Ip6OnFrameReceived()
      //
      gBS->SignalEvent (RxToken->Event);
    }
  }
  DispatchDpc ();
}
```

### Choose a proper place to dispatch the DPC

The dispatching of previous queued DPCs is not guaranteed by the firmware or the DPC driver. Instead, **the UEFI driver
or application that calls *QueueDpc()* is responsible for calling *DispatchDpc()* to dispatch it**.

* UEFI applications that call QueueDpc() must call DispatchDpc() before they exit to guarantee that all queued DPCs have
  been dispatched.
* UEFI drivers that call QueueDpc() must call DispatchDpc() before they unload to guarantee that all queued DPCs have
  been dispatched
* UEFI drivers the follow the EFI Driver Model and call QueueDpc() using a device specific context must call
  DispatchDpc() from their EFI Driver Binding Stop() function to guarantee that all DPCs for that device are dispatched
  before the device specific context structures are freed.

However, it's not encouraged to queue a lot of DPCs and dispatch them together before exiting. As we discussed before,
multiple DPCs in the queue will result in nested DPC calls, and bring additional complexity to the upper layer driver.
**In Edk2 network stack, it is required to call the QueueDpc() and DispatchDpc() always in pairs**, or in other words,
always try to call DispatchDpc() immediately after a SignalEvent() which may queue a new DPC procedure, otherwise the
upper layer drivers will malfunction. For example, a particular implementation of the MNP driver want to receive
multiple packets in the MnpPoll(). So it calls gBS->SignalEvent() for every received packet, then uses a single
DispatchDpc() to dispatch them as below.

```c
// Bad example
MnpPoll()
{
  while (...) {
    MnpReceive()
    {
      MnpInstanceDeliverPacket()
      {
        gBS->SignalEvent (RxToken->Event);
      }
    }
  }
  DispatchDpc ();    // Dispatch several DPC procedures together - NOT good
}
```

The above implementation will result in nested DPC calls as we discussed before, and cause a failure in TCP/UDP driver.
The correct practice is to dispatch the DPC procedure inside the while loop, so these DPC procedures will be dispatched
separately.

```c
// Good example
MnpPoll()
{
  while (...) {
    MnpReceive()
    {
      MnpInstanceDeliverPacket()
      {
        gBS->SignalEvent (RxToken->Event);
      }
    }
    DispatchDpc ();
  }
}
```

Finally, **make sure the *DispatchDpc()* is called at a right TPL level** so it won't miss any previously queued DPCs.
Remember that the *DispatchDpc()* dispatches all the DPCs that is greater than or equal to the current TPL value.

## Related Pages

[NetworkPkg Getting Started Guide](../../platforms-packages/core-packages/networkpkg_getting_started_guide.md)
