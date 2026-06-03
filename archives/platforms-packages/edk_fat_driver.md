# EDK FAT Driver

fat-driver Project home
[https://sourceforge.net/projects/edk-fat-driver/](https://sourceforge.net/projects/edk-fat-driver/)

Please read the news on the home page regarding the SITE
TRANSITION—DOWNTIME January 21. Very important information regarding
coming changes.

Request project membership/role \| Watch project Summary Project for the
Fat32 Driver Category sample-driver License Other Owner(s)

Welcome to the FAT-Driver PROJECT, a project for the FAT32 File System
Driver, which is based on Microsoft's FAT32 File System Driver
Specification. The Fat Driver is separate from the EDK project only
because the terms of use of the code are unique, requiring us to keep it
in a separate code repository (the terms are basically that code
developed using the FAT32 Specification must be associated with EFI).

This project is a subproject of the EDK project, and is only used to
house the FAT32 code respository. All development tools in the EDK
project are integrated with this project, so come here only to update
the CVS repository or to get access to the latest code snapshots. Use
the EDK project to post issues, join mailing lists, participate in
discussion forums, etc. In short, consider them one project with two
code repositories. FAT-Driver News

The FAT-Driver development team is pleased to announce a new "Enhanced"
FAT-Driver. This project will now be used to develop and maintain both
drivers. From this point on, the original FAT-Driver will be refererred
to as FAT-Driver while the new driver will go by the Enhanced-FAT-Driver
name. Both drivers will be using the same Project Tracer and each have
their own module name when creating/modifying TASKs or ENHANCEMENTs. The
enhanced-fat-driver is also published under the same license as the
fat-driver. To view more specific information about the
enhanced-fat-driver, go to the Enhanced-FAT-Driver section. Working with
the FAT32 Code

**The FAT-Driver project is sponsored by Intel and released under the
BSD license with one additional term. Before using the code please read
the license below, paying special attention to the "Additional Terms"
section:**

To download code, view the directory of code snapshots and choose the
version (Official Release or Development Version) you are interested in
and simply click on it to start the download process. Join this project
if you want to contribute code changes (see the next section for
guidance). Enhanced-FAT-Driver

The Enhanced-FAT-Driver is a new implementation of the FAT32 filesystem.
It is a near complete rewrite of the original FAT-Driver with over 80%
of the code being specific just to the Enhanced-FAT-Driver. The new
driver is faster and takes up only 66% of the original FAT-Driver's
memory footprint. What makes the Enhanced-FAT-Driver better?

`* Better Architecture`
`o More modular design thanks to object oriented and hierarchical design`
`o Logic directory approach used in place of the physical directory`
`o Easier to maintain and debug`
`o 80% of the code was completely rewritten to make it smaller and faster`
`* Higher Performance`
`o A cache mechanism to cache recently opened directories`
`o A more efficient 8.3 name generation algorithm`
`o 10x, 100x or even 1000x acceleration when creating thousands files/directories`
`* Smaller Size`
`o Simplified the controller name from "FAT File System[FAT16] 200MB" to "FAT File System"`
`o Succinct coding style`
`o 2/3 the image size of the original FAT-Driver`

So why are there two FAT-Driver implementations in the fat-driver
project?

`* Publish the enhanced FAT implementation so that people can try the new implementation with size and performance benefits`
`* Keep the original FAT driver for backward compatibility or avoid potential risk for mature products`

Which driver should I use?

`* Enhanced-FAT-Driver`
`o When you need the fastest FAT filesystem driver`
`o When space is crucial`
`* FAT-Driver`
`o When you need stability to avoid possible risk of a new implementation`
`o When you prefer to keep the original behavior, such as 8.3 name generation algorithm, complete controller name f`nor each volume`

What does this mean to you?

Not much. The source code will be housed in the same repository so there
is no need to checkout multiple projects to work on these drivers. The
Project Tracker configuration is setup to allow for concurrent DEFECT,
ENHANCEMENT and TASK management for both projects. The only real concern
for you is to figure out which driver to use when. Joining the
FAT-Driver Project

To join the FAT-Driver Project simply click on the "Request Project
Membership/Role" link above and choose "Observer" as a requested role
(that's the default role for all new project members). Upon receipt of
your request, the FAT-Driver Maintainer will approve your project
membership. Once in the project you'll primarily use the CVS repository
located here, or access the FAT32-specific documents located here.
Managing Changes to the FAT-Driver Source Code

Since the FAT32 Driver Code is actually part of the EDK, changes to it
are managed within the EDK issue management system. To make it simple
for you, the following links have been developed to help the
communication between the two projects. From these links you can enter a
FAT32 issue, query open FAT32 issues, access the DEV mailing list
archive and download the FAT32 source code.

`* Enhanced FAT-Driver`
`o Latest Enhanced-FAT-Driver Source Code Snapshot (Development Version)`
`o Latest Enhanced-FAT-Driver Source Code Snapshot (Official Release)`
`o Query of Open and Active Enhanced-FAT-Driver Project Defects`
`* FAT-Driver`
`o Latest FAT-Driver Source Code Snapshot (Development Version)`
`o Latest FAT-Driver Source Code Snapshot (Official Release)`
`o Query of Open and Active FAT-Driver Project Defects`
`* General`
`o Query of All FAT-Driver Project Defects`
`o Archives of the EDK "Dev" Mailing List`

Other Common Links and Downloads

`* Request for a FAT-Driver Project Role (initial or promotion)`
`* Link to the EDK Project Homepage`[EDK](../reference/efidevkit.md)
`* Link to Microsoft's FAT32 System Driver Specification`[Link](https://msdn.microsoft.com/en-us/windows/hardware/gg463080)
`* Link to Microsoft's license agreement`[Link](https://msdn.microsoft.com/en-us/windows/hardware/gg463080.aspx)

Project Points of Contact

-

-
