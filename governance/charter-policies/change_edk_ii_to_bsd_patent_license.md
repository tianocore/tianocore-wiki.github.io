# Change EDK II to BSD+Patent License

This change is based on the following emails:

  [https://lists.01.org/pipermail/edk2-devel/2019-February/036260.html](https://lists.01.org/pipermail/edk2-devel/2019-February/036260.html)
  [https://lists.01.org/pipermail/edk2-devel/2018-October/030385.html](https://lists.01.org/pipermail/edk2-devel/2018-October/030385.html)

RFCs with detailed process for the license change:

* V3:
  [https://lists.01.org/pipermail/edk2-devel/2019-March/038116.html](https://lists.01.org/pipermail/edk2-devel/2019-March/038116.html)
* V2:
  [https://lists.01.org/pipermail/edk2-devel/2019-March/037669.html](https://lists.01.org/pipermail/edk2-devel/2019-March/037669.html)
* V1:
  [https://lists.01.org/pipermail/edk2-devel/2019-March/037500.html](https://lists.01.org/pipermail/edk2-devel/2019-March/037500.html)

## GitHub Issue

[https://github.com/tianocore/edk2/issues/7666](https://github.com/tianocore/edk2/issues/7666)

## Patch Reviews

The patch series for review has been posted on the following branch using
7ed72121b7 as the base for the patch series.

  [https://github.com/mdkinney/edk2/tree/Bug_1373_BsdPatentLicense_V3](https://github.com/mdkinney/edk2/tree/Bug_1373_BsdPatentLicense_V3)

The commits in patch series can be viewed here:

  [https://github.com/mdkinney/edk2/commits/Bug_1373_BsdPatentLicense_V3](https://github.com/mdkinney/edk2/commits/Bug_1373_BsdPatentLicense_V3)

The patch series has one patch per package along with a few patches
to update the license information in the root of the edk2 repository
as described in the RFC V3.

Due to the size of the patch series, the preference is to not send the
patch emails.  Instead, please perform code reviews using content
from the branch.

All EDK II package maintainers and package reviewers should provide
review feedback for their packages.  The critical part of the review
is:

1) Any changes that cause build breaks or logic changes.  These code
   changes are intended to only modify license contents in comment
   blocks.
2) Any file that has been changed to BSD+Patent, but should remain
   with the current license.
3) Any file that that has not changed to BSD+Patent, but should be
   changed to BSD+Patent.

Feedback and Reviewed-by emails should identify the patch the feedback
applies using the patch summary listed below.  The goal is to complete
all reviews to support the commit of these patches on April 9, 2019.

The following table provide a summary of the patches, the list of reviewers,
and the Reviewed-by status.

| Patch  | Reviewers | Reviewed-by |
| ------ | --------- | ----------- |
| [edk2: Remove Contributions.txt and update Readme.md](https://github.com/mdkinney/edk2/commit/eece5f8a6e64470be95a80a1f12f6f1597ee7eec)                     | Andrew Fish <afish@apple.com>  Laszlo Ersek <lersek@redhat.com>  Leif Lindholm <leif.lindholm@linaro.org>                    | DONE |
| [OvmfPkg: Change License.txt from 2-Clause BSD to BSD+Patent](https://github.com/mdkinney/edk2/commit/224dce1ae5db1239108e55c243fa681d86f8ac1e)             | Jordan Justen <jordan.l.justen@intel.com>  Laszlo Ersek <lersek@redhat.com>  Ard Biesheuvel <ard.biesheuvel@linaro.org>        | DONE |
| [StdLibPrivateInternalFiles: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/f3cbc2ffc74c478551d0ebbe821ab84eb0549f69) | Daryl McDaniel <edk2-lists@mc2research.org>  Jaben Carsey <jaben.carsey@intel.com>                                               | DONE |
| [StdLib: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/845b94504469b5c5d3fa34cc71f535bbc2f0a1e1)                     | Daryl McDaniel <edk2-lists@mc2research.org>  Jaben Carsey <jaben.carsey@intel.com>                                               | DONE |
| [AppPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/e55c8532c980ea081dc3bf1f669c7a37872cc073)                     | Daryl McDaniel <edk2-lists@mc2research.org>  Jaben Carsey <jaben.carsey@intel.com>                                               | DONE |
| [Vlv2TbltDevicePkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/a6df2af909f86d1c2910bf9adbb7491e49e0ae7a)          | Zailiang Sun <zailiang.sun@intel.com>  Yi Qian <yi.qian@intel.com>                                                               | DONE |
| [Vlv2DeviceRefCodePkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/6360b3f3afc62afcad1de75af1d4136a689ca8fb)       | Zailiang Sun <zailiang.sun@intel.com>  Yi Qian <yi.qian@intel.com>                                                               | DONE |
| [UefiCpuPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/a67328cbb41d4669d7d73e05ed4c6cbc6f1d80f6)                 | Eric Dong <eric.dong@intel.com>  Ray Ni <ray.ni@intel.com>  Laszlo Ersek <lersek@redhat.com>                                   | DONE |
| [StandaloneMmPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/248d91c9080e88dd9f9783f85ed36d627ceca22b)            | Achin Gupta <achin.gupta@arm.com>  Jiewen Yao <jiewen.yao@intel.com>  Supreeth Venkatesh <supreeth.venkatesh@arm.com>          | DONE |
| [SourceLevelDebugPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/415927330c683efeb6495100d8e4a04780890e83)        | Hao Wu <hao.a.wu@intel.com>                                                                                                        | DONE |
| [SignedCapsulePkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/4cd6c3f31f601291f7170dbd82661dd92ef2b356)           | Jiewen Yao <jiewen.yao@intel.com>  Chao Zhang <chao.b.zhang@intel.com>                                                           | DONE |
| [ShellPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/9ffa4947d31faedb5624466794818b337223fa2a)                   | Jaben Carsey <jaben.carsey@intel.com>  Ray Ni <ray.ni@intel.com>                                                                 | DONE |
| [ShellBinPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/fdb651095579234f576e12927a83a7cc7ec68ced)                | Jaben Carsey <jaben.carsey@intel.com>  M: Ray Ni <ray.ni@intel.com>  Leif Lindholm <leif.lindholm@linaro.org>  Ard Biesheuvel <ard.biesheuvel@linaro.org> | DONE |
| [SecurityPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/dc5f8d93d9c4fce863577748eef475f8d8e951bc)                | Chao Zhang <chao.b.zhang@intel.com>  Jiewen Yao <jiewen.yao@intel.com>  Jian Wang <jian.j.wang@intel.com>                      | DONE |
| [QuarkSocPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/841d42a913e8bb31956c3fe22621aa9f6ca2c8ad)                | Kelly Steele <kelly.steele@intel.com>                                                                                              | DONE |
| [QuarkPlatformPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/75c3756e1875140129c3e907263a4aa1f1f0417d)           | Kelly Steele <kelly.steele@intel.com>                                                                                              | DONE |
| [PcAtChipsetPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/657491e5a3b346e74a44fc6e83cf15d44d9a2488)             | Ray Ni <ray.ni@intel.com>                                                                                                          | DONE |
| [OvmfPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/4ac687248f163e96a98944205c93e635c7f4a904)                    | Jordan Justen <jordan.l.justen@intel.com>  Laszlo Ersek <lersek@redhat.com>  Ard Biesheuvel <ard.biesheuvel@linaro.org>        | DONE |
| [OptionRomPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/521b8139c7bd180b634a1ceaea4ef6200458074c)               | Ray Ni <ray.ni@intel.com>                                                                                                          | DONE |
| [Omap35xxPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/ec339f8320c88843a6c36d9eeae0c2bf757224ff)                | Leif Lindholm <leif.lindholm@linaro.org>  Ard Biesheuvel <ard.biesheuvel@linaro.org>                                             | DONE |
| [Nt32Pkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/ad1bf1a53a95f76cbdddfa57e183b42d6f477b52)                    | Ray Ni <ray.ni@intel.com>  Hao Wu <hao.a.wu@intel.com>                                                                           | DONE |
| [NetworkPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/aa99b9adeb10f7f389322007b5e1fc1e2f25a95d)                 | Siyuan Fu <siyuan.fu@intel.com>  Jiaxin Wu <jiaxin.wu@intel.com>                                                                 | DONE |
| [MdePkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/88bdc989d76b46a5ae6608e3c5d44b8b9302dc32)                     | Liming Gao <liming.gao@intel.com>                                                                                                  | DONE |
| [MdeModulePkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/5628b5e5c734fb8f4dfe5aaac4543e65d0c31bb0)               | Jian J Wang <jian.j.wang@intel.com>  Hao Wu <hao.a.wu@intel.com>  Ray Ni <ray.ni@intel.com>  Star Zeng <star.zeng@intel.com> | DONE |
| [IntelSiliconPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/505e36838dbe0e9a67b170cff5e278a40d99d4e4)            | Ray Ni <ray.ni@intel.com>  Rangasai V Chaganty <rangasai.v.chaganty@intel.com>                                                   | DONE |
| [IntelFspWrapperPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/e1fecb94e72b15c2dab1e2da36cd33ca52bec90f)         | Chasel Chiu <chasel.chiu@intel.com>  Nate DeSimone <nathaniel.l.desimone@intel.com>  Star Zeng <star.zeng@intel.com>           | DONE |
| [IntelFspPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/d7347d7ef09d3c27abcafc15fc382bae1516f066)                | Chasel Chiu <chasel.chiu@intel.com>  Nate DeSimone <nathaniel.l.desimone@intel.com>  Star Zeng <star.zeng@intel.com>           | DONE |
| [IntelFsp2WrapperPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/487741dd60dff6f5d16cc3f0b17507c992748fc1)        | Chasel Chiu <chasel.chiu@intel.com>  Nate DeSimone <nathaniel.l.desimone@intel.com>  Star Zeng <star.zeng@intel.com>           | DONE |
| [IntelFsp2Pkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/0e8d264fafa07af7df1250b561a4d18508fed969)               | Chasel Chiu <chasel.chiu@intel.com>  Nate DeSimone <nathaniel.l.desimone@intel.com>  Star Zeng <star.zeng@intel.com>           | DONE |
| [IntelFrameworkPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/f4835e95086c5a22f4a279972eb31845f0179545)          | Liming Gao <liming.gao@intel.com>                                                                                                  | DONE |
| [IntelFrameworkModulePkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/d9af7c1d3b71f2161e0f95f81079d00a09c6e33d)    | Liming Gao <liming.gao@intel.com>                                                                                                  | DONE |
| [FmpDevicePkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/5a3c8a14ce6b96730b3b550a74b00d67f2847f5d)               | Liming Gao <liming.gao@intel.com>                                                                                                  | DONE |
| [FatPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/87d9ee37c5d42e458fdae759a36254d70cc67d4b)                     | Ray Ni <ray.ni@intel.com>                                                                                                          | DONE |
| [EmulatorPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/f336d7d7de20ff8948959bfdf0e9e33ff53fb5a6)                | Jordan Justen <jordan.l.justen@intel.com>  Andrew Fish <afish@apple.com>  Ray Ni <ray.ni@intel.com>                            | DONE |
| [EmbeddedPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/5ea93a60884c8d16b9f2201593683c6413600e00)                | Leif Lindholm <leif.lindholm@linaro.org>  Ard Biesheuvel <ard.biesheuvel@linaro.org>                                             | DONE |
| [DynamicTablesPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/2afa7c0820691a129d3e176550acbefdd0958964)           | Sami Mujawar <Sami.Mujawar@arm.com>  Alexei Fedorov <Alexei.Fedorov@arm.com>                                                     | |
| [CryptoPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/4768af6404d42da3540247f111595ab78bde1c54)                  | Ting Ye <ting.ye@intel.com>  Gang Wei <gang.wei@intel.com>  Jian Wang <jian.j.wang@intel.com>                                  | DONE |
| [CorebootPayloadPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/9aecbee0a3e8fbe785ca9c581953f462760b7fa4)         | Maurice Ma <maurice.ma@intel.com>  Prince Agyeman <prince.agyeman@intel.com>  Benjamin You <benjamin.you@intel.com>            | DONE |
| [CorebootModulePkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/e7716450b8cb54822d8a09520ce67a311736a2d7)          | Maurice Ma <maurice.ma@intel.com>  Prince Agyeman <prince.agyeman@intel.com>  Benjamin You <benjamin.you@intel.com>            | DONE |
| [BeagleBoardPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/04fb04e0aa59de68fe2930febcd395c50ea49b1e)             | Leif Lindholm <leif.lindholm@linaro.org>  Ard Biesheuvel <ard.biesheuvel@linaro.org>                                             | DONE |
| [ArmVirtPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/c910bd4e916eac58a54288212afc61c30a653e0b)                 | Laszlo Ersek <lersek@redhat.com>  Ard Biesheuvel <ard.biesheuvel@linaro.org>                                                     | DONE |
| [ArmPlatformPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/6f98d2f3052fd5d1fa90b7c5cc0ccc4e4e6d5fd6)             | Leif Lindholm <leif.lindholm@linaro.org>  Ard Biesheuvel <ard.biesheuvel@linaro.org>                                             | DONE |
| [ArmPkg: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/1ce858bf27300f8b2c48170b6788769df7ac7a75)                     | Leif Lindholm <leif.lindholm@linaro.org>  Ard Biesheuvel <ard.biesheuvel@linaro.org>                                             | DONE |
| [BaseTools: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/0fb7bd552bdba9c3f97c70e1fc672ae8f3be32d5)                  | Bob Feng <bob.c.feng@intel.com>  Liming Gao <liming.gao@intel.com>  Yonghong Zhu <yonghong.zhu@intel.com>                      | DONE |
| [edk2: Replace BSD License with BSD+Patent License](https://github.com/mdkinney/edk2/commit/bc617332858610c65cbb26f0c2be72038f7c6a0e)                       | Andrew Fish <afish@apple.com>  Laszlo Ersek <lersek@redhat.com>  Leif Lindholm <leif.lindholm@linaro.org>                      | DONE |
| [edk2: Change License.txt from 2-Clause BSD to BSD+Patent](https://github.com/mdkinney/edk2/commit/8ee83c5dcd798caf0b2e9e0bfb33601de5c6ab79)                | Andrew Fish <afish@apple.com>  Laszlo Ersek <lersek@redhat.com>  Leif Lindholm <leif.lindholm@linaro.org>                      | DONE |
| [edk2: Add License-History.txt](https://github.com/mdkinney/edk2/commit/17a33094a9a2fd0f5db89c89e60a58a2f195dbdf)                                           | Andrew Fish <afish@apple.com>  Laszlo Ersek <lersek@redhat.com>  Leif Lindholm <leif.lindholm@linaro.org>                      | DONE |

## RFC V3

 This RFC follows up on the proposal from Mark Doran to change the
EDK II Project to a BSD+Patent License.

 <https://lists.01.org/pipermail/edk2-devel/2019-February/036260.html>

The review period for this license change is 30 days.  If there is no
unresolved feedback on April 9, 2019, then commits of the license change
patches will begin on April 9, 2019.

  **Please provide feedback on the proposal by Monday April 8, 2019.**

Feedback can be sent to edk2-devel at lists.01.org, the EDK II community
manager or any of the EDK II stewards.

* Leif Lindholm   `<leif.lindholm at linaro.org>`    Steward
* Andrew Fish     `<afish at apple.com>`             Steward
* Laszlo Ersek    `<lersek at redhat.com>`           Steward
* Michael Kinney  `<michael.d.kinney at intel.com>`  Steward

The goal is to convert all of the files in the edk2 repository that are
currently covered by the 2-Clause BSD License and the TianoCore
Contribution Agreement to a BSD+Patent License.

I will be following up with pointers to public GitHub branches that
contain the set of changes to the edk2 repository for review.

The proposal is to perform this change to edk2/master in the steps listed
below. The license change will not be applied to any of the other existing
branches in the edk2 repository.

1) Add a License-History.txt file to the root of the edk2 repository that
   contains the 2-Clause BSD License and the TianoCore Contribution
   Agreement along with the details on the change to the BSD+Patent License.

2) Change License.txt in the root of the edk2 repository from a 2-Clause
   BSD License to the BSD+Patent License. The following is the link to the
   BSD+Patent License and the new License.txt file contents.

   [https://opensource.org/licenses/BSDplusPatent](https://opensource.org/licenses/BSDplusPatent)

   ```
   Redistribution and use in source and binary forms, with or without
   modification, are permitted provided that the following conditions are met:

   1. Redistributions of source code must retain the above copyright notice,
      this list of conditions and the following disclaimer.

   2. Redistributions in binary form must reproduce the above copyright notice,
      this list of conditions and the following disclaimer in the documentation
      and/or other materials provided with the distribution.

   Subject to the terms and conditions of this license, each copyright holder
   and contributor hereby grants to those receiving rights under this license
   a perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable
   (except for failure to satisfy the conditions of this license) patent
   license to make, have made, use, offer to sell, sell, import, and otherwise
   transfer this software, where such license applies only to those patent
   claims, already acquired or hereafter acquired, licensable by such copyright
   holder or contributor that are necessarily infringed by:

   (a) their Contribution(s) (the licensed copyrights of copyright holders and
       non-copyrightable additions of contributors, in source or binary form)
       alone; or

   (b) combination of their Contribution(s) with the work of authorship to
       which such Contribution(s) was added by such copyright holder or
       contributor, if, at the time the Contribution is added, such addition
       causes such combination to be necessarily infringed. The patent license
       shall not apply to any other combinations which include the
       Contribution.

   Except as expressly stated above, no rights or licenses from any copyright
   holder or contributor is granted under this license, whether expressly, by
   implication, estoppel or otherwise.

   DISCLAIMER

   THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
   AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
   IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
   ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDERS OR CONTRIBUTORS BE
   LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
   CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
   SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
   INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
   CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
   ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
   POSSIBILITY OF SUCH DAMAGE.
   ```

3) Change all files currently covered by a 2-Clause BSD License and the
   TianoCore Contribution Agreement to a BSD+Patent License using the
   following SPDX-License-Identifier statement:

       SPDX-License-Identifier: BSD-2-Clause-Patent

   The use of SPDX-License-Identifier statement is based on the following:

        https://01.org/blogs/jc415/2018/open-source-hacks-one-question-interviews-open-source-experts-how-use-spdx-headers

4) Update Readme.md in the root of the edk2 repository to state that content
   is covered by a BSD+Patent License.  Also state that the BSD+Patent License
   is the preferred license for the EDK II project.

   a) Move the portions of Contributions.txt in the root of the edk2 repository
      Readme.md in the root of edk2 repository that describe how to contribute
      along with the commit message format.

   b) Add the following to Readme.md in the root of edk2 repository:

     ```
     # Developer Certificate of Origin

     Your change description should use the standard format for a
     commit message, and must include your `Signed-off-by` signature.

     In order to keep track of who did what, all patches contributed must
     include a statement that to the best of the contributor's knowledge
     they have the right to contribute it under the specified license.

     The test for this is as specified in the [Developer's Certificate of
     Origin (DCO) 1.1](https://developercertificate.org/). The contributor
     certifies compliance by adding a line saying

       Signed-off-by: Developer Name <developer@example.org>

     where `Developer Name` is the contributor's real name, and the email
     address is one the developer is reachable through at the time of
     contributing.

     Developer's Certificate of Origin 1.1

     By making a contribution to this project, I certify that:

      (a) The contribution was created in whole or in part by me and I
         have the right to submit it under the open source license
         indicated in the file; or

      (b) The contribution is based upon previous work that, to the best
         of my knowledge, is covered under an appropriate open source
         license and I have the right under that license to submit that
         work with modifications, whether created in whole or in part
         by me, under the same open source license (unless I am
         permitted to submit under a different license), as indicated
         in the file; or

      (c) The contribution was provided directly to me by some other
         person who certified (a), (b) or (c) and I have not modified
         it.

      (d) I understand and agree that this project and the contribution
         are public and that a record of the contribution (including all
         personal information I submit with it, including my sign-off) is
         maintained indefinitely and may be redistributed consistent with
         this project or the open source license(s) involved.
     ```

5) Remove the Contributions.txt file from the root of the edk2 repository
   that contains the TianoCore Contribution Agreement.

6) Update all documentation to state that content submitted under the
   BSD+Patent License no longer requires the Tianocore Contribution
   Agreement which means the following line is not required in commit
   messages for changes to files that are covered by a BSD+Patent License.

       Contributed-under: TianoCore Contribution Agreement 1.1

7) Create Wiki page(s) that provide the details of the BSD+Patent License
   change and provides the status of the license change for each TianoCore
   repository and package.

Once the conversion of the edk2 repository is complete, work will begin
on the other repositories in the TianoCore project.

## License-History.txt

 ```
                               License-History.txt
                              ===================

This file contains the history of license change and contributor's agreement
changes.

Unless otherwise noted in a specific file, the EDK2 project is now licensed
under the terms listed in the License.txt file.  Terms under which Contributions
made prior to the move to the License.txt formulation are shown below.  Those
terms require notice of the terms themselves be preserved and presented with the
contributions.  This file serves that preservation purpose as a matter of
documenting the history of the project.

Key Dates
----------
* August 3, 2017

  Update the TianoCore Contribution Agreement from Version 1.0
  to Version 1.1 to cover open source documentation associated
  with the TianoCore project.

  Version 1.0 covers source code files.  Version 1.1 is a
  backwards compatible extension that adds support for document
  files in both source form and compiled form.

  References:
      https://opensource.org/licenses/BSD-2-Clause
      Complete text of TianoCore Contribution Agreement 1.0 included below
      Complete text of TianoCore Contribution Agreement 1.1 included below

  Proposals (RFCs):
      https://lists.01.org/pipermail/edk2-devel/2017-March/008654.html

  TianoCore GitHub Issue:
      https://github.com/tianocore/edk2/issues/7056

* April 9, 2019

  Replace BSD 2-Clause License with BSD + Patent License removing the need for
  the TianoCore Contribution Agreement.

  References:
      https://opensource.org/licenses/BSD-2-Clause
      Complete text of TianoCore Contribution Agreement 1.0 included below
      Complete text of TianoCore Contribution Agreement 1.1 included below
      https://opensource.org/licenses/BSDplusPatent

  Proposals (RFCs):
      https://lists.01.org/pipermail/edk2-devel/2019-February/036260.html
      https://lists.01.org/pipermail/edk2-devel/2019-March/037500.html

  TianoCore GitHub Issue:
      https://github.com/tianocore/edk2/issues/7666

--------------------------------------------------------------------------------
License.txt: BSD 2-Clause License
--------------------------------------------------------------------------------
    Redistribution and use in source and binary forms, with or without
    modification, are permitted provided that the following conditions
    are met:

    * Redistributions of source code must retain the above copyright
      notice, this list of conditions and the following disclaimer.
    * Redistributions in binary form must reproduce the above copyright
      notice, this list of conditions and the following disclaimer in
      the documentation and/or other materials provided with the
      distribution.

    THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
    "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
    LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
    FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
    COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
    INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
    BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
    LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
    CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
    LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
    ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
    POSSIBILITY OF SUCH DAMAGE.
--------------------------------------------------------------------------------

--------------------------------------------------------------------------------
Contributions.txt: TianoCore Contribution Agreement 1.1
--------------------------------------------------------------------------------
    ======================
    = Code Contributions =
    ======================

    To make a contribution to a TianoCore project, follow these steps.
    1. Create a change description in the format specified below to
       use in the source control commit log.
    2. Your commit message must include your "Signed-off-by" signature,
       and "Contributed-under" message.
    3. Your "Contributed-under" message explicitly states that the
       contribution is made under the terms of the specified
       contribution agreement.  Your "Contributed-under" message
       must include the name of contribution agreement and version.
       For example: Contributed-under: TianoCore Contribution Agreement 1.1
       The "TianoCore Contribution Agreement" is included below in
       this document.
    4. Submit your code to the TianoCore project using the process
       that the project documents on its web page.  If the process is
       not documented, then submit the code on development email list
       for the project.
    5. It is preferred that contributions are submitted using the same
       copyright license as the base project. When that is not possible,
       then contributions using the following licenses can be accepted:
       * BSD (2-clause): https://opensource.org/licenses/BSD-2-Clause
       * BSD (3-clause): https://opensource.org/licenses/BSD-3-Clause
       * MIT: https://opensource.org/licenses/MIT
       * Python-2.0: https://opensource.org/licenses/Python-2.0
       * Zlib: https://opensource.org/licenses/Zlib

       For documentation:
       * FreeBSD Documentation License
         https://www.freebsd.org/copyright/freebsd-doc-license.html

       Contributions of code put into the public domain can also be
       accepted.

       Contributions using other licenses might be accepted, but further
       review will be required.

    =====================================================
    = Change Description / Commit Message / Patch Email =
    =====================================================

    Your change description should use the standard format for a
    commit message, and must include your "Signed-off-by" signature
    and the "Contributed-under" message.

    == Sample Change Description / Commit Message =

    === Start of sample patch email message ===

    From: Contributor Name <contributor@example.com>
    Subject: [Repository/Branch PATCH] Module: Brief-single-line-summary

    Full-commit-message

    Contributed-under: TianoCore Contribution Agreement 1.1
    Signed-off-by: Contributor Name <contributor@example.com>
    ---

    An extra message for the patch email which will not be considered part
    of the commit message can be added here.

    Patch content inline or attached

    === End of sample patch email message ===

    === Notes for sample patch email ===

    * The first line of commit message is taken from the email's subject
      line following [Repository/Branch PATCH]. The remaining portion of the
      commit message is the email's content until the '---' line.
    * git format-patch is one way to create this format

    === Definitions for sample patch email ===

    * "Repository" is the identifier of the repository the patch applies.
      This identifier should only be provided for repositories other than
      'edk2'. For example 'edk2-BuildSpecification' or 'staging'.
    * "Branch" is the identifier of the branch the patch applies. This
      identifier should only be provided for branches other than 'edk2/master'.
      For example 'edk2/UDK2015', 'edk2-BuildSpecification/release/1.27', or
      'staging/edk2-test'.
    * "Module" is a short identifier for the affected code or documentation. For
      example 'MdePkg', 'MdeModulePkg/UsbBusDxe', 'Introduction', or
      'EDK II INF File Format'.
    * "Brief-single-line-summary" is a short summary of the change.
    * The entire first line should be less than ~70 characters.
    * "Full-commit-message" a verbose multiple line comment describing
      the change.  Each line should be less than ~70 characters.
    * "Contributed-under" explicitly states that the contribution is
      made under the terms of the contribution agreement. This
      agreement is included below in this document.
    * "Signed-off-by" is the contributor's signature identifying them
      by their real/legal name and their email address.

    ========================================
    = TianoCore Contribution Agreement 1.1 =
    ========================================

    INTEL CORPORATION ("INTEL") MAKES AVAILABLE SOFTWARE, DOCUMENTATION
    ("DOCUMENTATION"), INFORMATION AND/OR OTHER MATERIALS FOR USE IN THE
    TIANOCORE OPEN SOURCE PROJECT (COLLECTIVELY "CONTENT"). USE OF THE CONTENT
    IS GOVERNED BY THE TERMS AND CONDITIONS OF THIS AGREEMENT BETWEEN YOU AND
    INTEL AND/OR THE TERMS AND CONDITIONS OF LICENSE AGREEMENTS OR NOTICES
    INDICATED OR REFERENCED BELOW. BY USING THE CONTENT, YOU AGREE THAT YOUR
    USE OF THE CONTENT IS GOVERNED BY THIS AGREEMENT AND/OR THE TERMS AND
    CONDITIONS OF ANY APPLICABLE LICENSE AGREEMENTS OR NOTICES INDICATED OR
    REFERENCED BELOW. IF YOU DO NOT AGREE TO THE TERMS AND CONDITIONS OF THIS
    AGREEMENT AND THE TERMS AND CONDITIONS OF ANY APPLICABLE LICENSE
    AGREEMENTS OR NOTICES INDICATED OR REFERENCED BELOW, THEN YOU MAY NOT
    USE THE CONTENT.

    Unless otherwise indicated, all Content (except Documentation) made available
    on the TianoCore site is provided to you under the terms and conditions of the
    BSD License ("BSD"). A copy of the BSD License is available at
    https://opensource.org/licenses/bsd-license.php
    or when applicable, in the associated License.txt file.

    Unless otherwise indicated, all Documentation made available on the
    TianoCore site is provided to you under the terms and conditions of the
    FreeBSD Documentation License ("FreeBSD"). A copy of the license is
    available at https://www.freebsd.org/copyright/freebsd-doc-license.html or,
    when applicable, in the associated License.txt file.

    Certain other content may be made available under other licenses as
    indicated in or with such Content (for example, in a License.txt file).

    You accept and agree to the following terms and conditions for Your
    present and future Contributions submitted to TianoCore site. Except
    for the license granted to Intel hereunder, You reserve all right,
    title, and interest in and to Your Contributions.

    == SECTION 1: Definitions ==
    * "You" or "Contributor" shall mean the copyright owner or legal
      entity authorized by the copyright owner that is making a
      Contribution hereunder. All other entities that control, are
      controlled by, or are under common control with that entity are
      considered to be a single Contributor. For the purposes of this
      definition, "control" means (i) the power, direct or indirect, to
      cause the direction or management of such entity, whether by
      contract or otherwise, or (ii) ownership of fifty percent (50%)
      or more of the outstanding shares, or (iii) beneficial ownership
      of such entity.
    * "Contribution" shall mean any original work of authorship,
      including any modifications or additions to an existing work,
      that is intentionally submitted by You to the TianoCore site for
      inclusion in, or documentation of, any of the Content. For the
      purposes of this definition, "submitted" means any form of
      electronic, verbal, or written communication sent to the
      TianoCore site or its representatives, including but not limited
      to communication on electronic mailing lists, source code
      control systems, and issue tracking systems that are managed by,
      or on behalf of, the TianoCore site for the purpose of
      discussing and improving the Content, but excluding
      communication that is conspicuously marked or otherwise
      designated in writing by You as "Not a Contribution."

    == SECTION 2: License for Contributions ==
    * Contributor hereby agrees that redistribution and use of the
      Contribution in source and binary forms, with or without
      modification, are permitted provided that the following
      conditions are met:
    ** Redistributions of source code must retain the Contributor's
       copyright notice, this list of conditions and the following
       disclaimer.
    ** Redistributions in binary form must reproduce the Contributor's
       copyright notice, this list of conditions and the following
       disclaimer in the documentation and/or other materials provided
       with the distribution.
    * Disclaimer. None of the names of Contributor, Intel, or the names
      of their respective contributors may be used to endorse or
      promote products derived from this software without specific
      prior written permission.
    * Contributor grants a license (with the right to sublicense) under
      claims of Contributor's patents that Contributor can license that
      are infringed by the Contribution (as delivered by Contributor) to
      make, use, distribute, sell, offer for sale, and import the
      Contribution and derivative works thereof solely to the minimum
      extent necessary for licensee to exercise the granted copyright
      license; this patent license applies solely to those portions of
      the Contribution that are unmodified. No hardware per se is
      licensed.
    * EXCEPT AS EXPRESSLY SET FORTH IN SECTION 3 BELOW, THE
      CONTRIBUTION IS PROVIDED BY THE CONTRIBUTOR "AS IS" AND ANY
      EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO,
      THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A
      PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
      CONTRIBUTOR BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
      SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT
      NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
      LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
      HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
      CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
      OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THE
      CONTRIBUTION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH
      DAMAGE.

    == SECTION 3: Representations ==
    * You represent that You are legally entitled to grant the above
      license. If your employer(s) has rights to intellectual property
      that You create that includes Your Contributions, You represent
      that You have received permission to make Contributions on behalf
      of that employer, that Your employer has waived such rights for
      Your Contributions.
    * You represent that each of Your Contributions is Your original
      creation (see Section 4 for submissions on behalf of others).
      You represent that Your Contribution submissions include complete
      details of any third-party license or other restriction
      (including, but not limited to, related patents and trademarks)
      of which You are personally aware and which are associated with
      any part of Your Contributions.

    == SECTION 4: Third Party Contributions ==
    * Should You wish to submit work that is not Your original creation,
      You may submit it to TianoCore site separately from any
      Contribution, identifying the complete details of its source
      and of any license or other restriction (including, but not
      limited to, related patents, trademarks, and license agreements)
      of which You are personally aware, and conspicuously marking the
      work as "Submitted on behalf of a third-party: [named here]".

    == SECTION 5: Miscellaneous ==
    * Applicable Laws. Any claims arising under or relating to this
      Agreement shall be governed by the internal substantive laws of
      the State of Delaware or federal courts located in Delaware,
      without regard to principles of conflict of laws.
    * Language. This Agreement is in the English language only, which
      language shall be controlling in all respects, and all versions
      of this Agreement in any other language shall be for accommodation
      only and shall not be binding. All communications and notices made
      or given pursuant to this Agreement, and all documentation and
      support to be provided, unless otherwise noted, shall be in the
      English language.
--------------------------------------------------------------------------------

--------------------------------------------------------------------------------
Contributions.txt: TianoCore Contribution Agreement 1.0
--------------------------------------------------------------------------------
    ======================
    = Code Contributions =
    ======================

    To make a contribution to a TianoCore project, follow these steps.
    1. Create a change description in the format specified below to
       use in the source control commit log.
    2. Your commit message must include your "Signed-off-by" signature,
       and "Contributed-under" message.
    3. Your "Contributed-under" message explicitly states that the
       contribution is made under the terms of the specified
       contribution agreement.  Your "Contributed-under" message
       must include the name of contribution agreement and version.
       For example: Contributed-under: TianoCore Contribution Agreement 1.0
       The "TianoCore Contribution Agreement" is included below in
       this document.
    4. Submit your code to the TianoCore project using the process
       that the project documents on its web page.  If the process is
       not documented, then submit the code on development email list
       for the project.
    5. It is preferred that contributions are submitted using the same
       copyright license as the base project. When that is not possible,
       then contributions using the following licenses can be accepted:
       * BSD (2-clause): https://opensource.org/licenses/BSD-2-Clause
       * BSD (3-clause): https://opensource.org/licenses/BSD-3-Clause
       * MIT: https://opensource.org/licenses/MIT
       * Python-2.0: https://opensource.org/licenses/Python-2.0
       * Zlib: https://opensource.org/licenses/Zlib

       Contributions of code put into the public domain can also be
       accepted.

       Contributions using other licenses might be accepted, but further
       review will be required.

    =====================================================
    = Change Description / Commit Message / Patch Email =
    =====================================================

    Your change description should use the standard format for a
    commit message, and must include your "Signed-off-by" signature
    and the "Contributed-under" message.

    == Sample Change Description / Commit Message =

    === Start of sample patch email message ===

    From: Contributor Name <contributor@example.com>
    Subject: [PATCH] CodeModule: Brief-single-line-summary

    Full-commit-message

    Contributed-under: TianoCore Contribution Agreement 1.0
    Signed-off-by: Contributor Name <contributor@example.com>
    ---

    An extra message for the patch email which will not be considered part
    of the commit message can be added here.

    Patch content inline or attached

    === End of sample patch email message ===

    === Notes for sample patch email ===

    * The first line of commit message is taken from the email's subject
      line following [PATCH]. The remaining portion of the commit message
      is the email's content until the '---' line.
    * git format-patch is one way to create this format

    === Definitions for sample patch email ===

    * "CodeModule" is a short idenfier for the affected code.  For
      example MdePkg, or MdeModulePkg UsbBusDxe.
    * "Brief-single-line-summary" is a short summary of the change.
    * The entire first line should be less than ~70 characters.
    * "Full-commit-message" a verbose multiple line comment describing
      the change.  Each line should be less than ~70 characters.
    * "Contributed-under" explicitely states that the contribution is
      made under the terms of the contribtion agreement.  This
      agreement is included below in this document.
    * "Signed-off-by" is the contributor's signature identifying them
      by their real/legal name and their email address.

    ========================================
    = TianoCore Contribution Agreement 1.0 =
    ========================================

    INTEL CORPORATION ("INTEL") MAKES AVAILABLE SOFTWARE, DOCUMENTATION,
    INFORMATION AND/OR OTHER MATERIALS FOR USE IN THE TIANOCORE OPEN SOURCE
    PROJECT (COLLECTIVELY "CONTENT"). USE OF THE CONTENT IS GOVERNED BY THE
    TERMS AND CONDITIONS OF THIS AGREEMENT BETWEEN YOU AND INTEL AND/OR THE
    TERMS AND CONDITIONS OF LICENSE AGREEMENTS OR NOTICES INDICATED OR
    REFERENCED BELOW. BY USING THE CONTENT, YOU AGREE THAT YOUR USE OF THE
    CONTENT IS GOVERNED BY THIS AGREEMENT AND/OR THE TERMS AND CONDITIONS
    OF ANY APPLICABLE LICENSE AGREEMENTS OR NOTICES INDICATED OR REFERENCED
    BELOW. IF YOU DO NOT AGREE TO THE TERMS AND CONDITIONS OF THIS
    AGREEMENT AND THE TERMS AND CONDITIONS OF ANY APPLICABLE LICENSE
    AGREEMENTS OR NOTICES INDICATED OR REFERENCED BELOW, THEN YOU MAY NOT
    USE THE CONTENT.

    Unless otherwise indicated, all Content made available on the TianoCore
    site is provided to you under the terms and conditions of the BSD
    License ("BSD"). A copy of the BSD License is available at
    https://opensource.org/licenses/bsd-license.php
    or when applicable, in the associated License.txt file.

    Certain other content may be made available under other licenses as
    indicated in or with such Content. (For example, in a License.txt file.)

    You accept and agree to the following terms and conditions for Your
    present and future Contributions submitted to TianoCore site. Except
    for the license granted to Intel hereunder, You reserve all right,
    title, and interest in and to Your Contributions.

    == SECTION 1: Definitions ==
    * "You" or "Contributor" shall mean the copyright owner or legal
      entity authorized by the copyright owner that is making a
      Contribution hereunder. All other entities that control, are
      controlled by, or are under common control with that entity are
      considered to be a single Contributor. For the purposes of this
      definition, "control" means (i) the power, direct or indirect, to
      cause the direction or management of such entity, whether by
      contract or otherwise, or (ii) ownership of fifty percent (50%)
      or more of the outstanding shares, or (iii) beneficial ownership
      of such entity.
    * "Contribution" shall mean any original work of authorship,
      including any modifications or additions to an existing work,
      that is intentionally submitted by You to the TinaoCore site for
      inclusion in, or documentation of, any of the Content. For the
      purposes of this definition, "submitted" means any form of
      electronic, verbal, or written communication sent to the
      TianoCore site or its representatives, including but not limited
      to communication on electronic mailing lists, source code
      control systems, and issue tracking systems that are managed by,
      or on behalf of, the TianoCore site for the purpose of
      discussing and improving the Content, but excluding
      communication that is conspicuously marked or otherwise
      designated in writing by You as "Not a Contribution."

    == SECTION 2: License for Contributions ==
    * Contributor hereby agrees that redistribution and use of the
      Contribution in source and binary forms, with or without
      modification, are permitted provided that the following
      conditions are met:
    ** Redistributions of source code must retain the Contributor's
       copyright notice, this list of conditions and the following
       disclaimer.
    ** Redistributions in binary form must reproduce the Contributor's
       copyright notice, this list of conditions and the following
       disclaimer in the documentation and/or other materials provided
       with the distribution.
    * Disclaimer. None of the names of Contributor, Intel, or the names
      of their respective contributors may be used to endorse or
      promote products derived from this software without specific
      prior written permission.
    * Contributor grants a license (with the right to sublicense) under
      claims of Contributor's patents that Contributor can license that
      are infringed by the Contribution (as delivered by Contributor) to
      make, use, distribute, sell, offer for sale, and import the
      Contribution and derivative works thereof solely to the minimum
      extent necessary for licensee to exercise the granted copyright
      license; this patent license applies solely to those portions of
      the Contribution that are unmodified. No hardware per se is
      licensed.
    * EXCEPT AS EXPRESSLY SET FORTH IN SECTION 3 BELOW, THE
      CONTRIBUTION IS PROVIDED BY THE CONTRIBUTOR "AS IS" AND ANY
      EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO,
      THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A
      PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
      CONTRIBUTOR BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
      SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT
      NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
      LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
      HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
      CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
      OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THE
      CONTRIBUTION, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH
      DAMAGE.

    == SECTION 3: Representations ==
    * You represent that You are legally entitled to grant the above
      license. If your employer(s) has rights to intellectual property
      that You create that includes Your Contributions, You represent
      that You have received permission to make Contributions on behalf
      of that employer, that Your employer has waived such rights for
      Your Contributions.
    * You represent that each of Your Contributions is Your original
      creation (see Section 4 for submissions on behalf of others).
      You represent that Your Contribution submissions include complete
      details of any third-party license or other restriction
      (including, but not limited to, related patents and trademarks)
      of which You are personally aware and which are associated with
      any part of Your Contributions.

    == SECTION 4: Third Party Contributions ==
    * Should You wish to submit work that is not Your original creation,
      You may submit it to TianoCore site separately from any
      Contribution, identifying the complete details of its source
      and of any license or other restriction (including, but not
      limited to, related patents, trademarks, and license agreements)
      of which You are personally aware, and conspicuously marking the
      work as "Submitted on behalf of a third-party: [named here]".

    == SECTION 5: Miscellaneous ==
    * Applicable Laws. Any claims arising under or relating to this
      Agreement shall be governed by the internal substantive laws of
      the State of Delaware or federal courts located in Delaware,
      without regard to principles of conflict of laws.
    * Language. This Agreement is in the English language only, which
      language shall be controlling in all respects, and all versions
      of this Agreement in any other language shall be for accommodation
      only and shall not be binding. All communications and notices made
      or given pursuant to this Agreement, and all documentation and
      support to be provided, unless otherwise noted, shall be in the
      English language.
--------------------------------------------------------------------------------
```
