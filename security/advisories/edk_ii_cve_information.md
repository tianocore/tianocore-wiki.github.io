# EDK II CVE Information

[CVE-2025-3770](https://www.cvedetails.com/cve/CVE-2025-3770/)

* Bugzilla: N/A
* [GHSA-vx5v-4gg6-6qxr](https://github.com/tianocore/edk2/security/advisories/GHSA-vx5v-4gg6-6qxr)
* Stable Tag where fixed: [202508](https://github.com/tianocore/edk2/releases/tag/edk2-stable202508)
* Commit(s) where fixed: [PR #11372](https://github.com/tianocore/edk2/pull/11372): available August 4, 2025

***

[CVE-2025-2296](https://www.cvedetails.com/cve/CVE-2025-2296/)

* Bugzilla: N/A
* [GHSA-6pp6-cm5h-86g5](https://github.com/tianocore/edk2/security/advisories/GHSA-6pp6-cm5h-86g5)
* Stable Tag where fixed: [202505](https://github.com/tianocore/edk2/releases/tag/edk2-stable202505)
* Commit(s) where fixed: [PR #10628](https://github.com/tianocore/edk2/pull/10628): available January 21, 2025

***

[CVE-2025-2295](https://www.cvedetails.com/cve/CVE-2025-2295/)

* Bugzilla: N/A
* [GHSA-8522-69fh-w74x](https://github.com/tianocore/edk2/security/advisories/GHSA-8522-69fh-w74x)
* Stable Tag where fixed: [202505](https://github.com/tianocore/edk2/releases/tag/edk2-stable202505)
* Commit(s) where fixed: [PR #10863](https://github.com/tianocore/edk2/pull/10863): available March 18, 2025

***

[CVE-2024-38805](https://www.cvedetails.com/cve/CVE-2024-38805/)

* Bugzilla: N/A
* [GHSA-p7wp-52j7-6r5x](https://github.com/tianocore/edk2/security/advisories/GHSA-p7wp-52j7-6r5x)
* Stable Tag where fixed: [202505](https://github.com/tianocore/edk2/releases/tag/edk2-stable202505)
* Commit(s) where fixed: [PR #11042](https://github.com/tianocore/edk2/pull/11042): available March 18, 2025

***

[CVE-2024-38798](https://www.cvedetails.com/cve/CVE-2024-38798/)

* Bugzilla: N/A
* [GHSA-q2c6-37h5-7cwf](https://github.com/tianocore/edk2/security/advisories/GHSA-q2c6-37h5-7cwf)
* Stable Tag where fixed: [202511](https://github.com/tianocore/edk2/releases/tag/edk2-stable202511)
* Commit(s) where fixed: [Push #10964](https://github.com/tianocore/edk2/pull/10964): available October 9, 2025

***

[CVE-2024-38797](https://www.cvedetails.com/cve/CVE-2024-38797/)

* Bugzilla: N/A
* [GHSA-4wjw-6xmf-44xf](https://github.com/tianocore/edk2/security/advisories/GHSA-4wjw-6xmf-44xf)
* Stable Tag where fixed: [202505](https://github.com/tianocore/edk2/releases/tag/edk2-stable202505)
* Commit(s) where fixed: [Push #10928](https://github.com/tianocore/edk2/pull/10928): available April 8, 2025

***

[CVE-2024-38796](https://nvd.nist.gov/vuln/detail/CVE-2024-38796)

* Bugzilla: N/A
* [GHSA-xpcr-7hjq-m6qm](https://github.com/tianocore/edk2/security/advisories/GHSA-xpcr-7hjq-m6qm)
* Stable Tag where fixed: [202411](https://github.com/tianocore/edk2/releases/tag/edk2-stable202411)
* Commit(s) where fixed: [Push #5659](https://github.com/tianocore/edk2/pull/5659): available September 30, 2024

***

[CVE-2024-1298](https://nvd.nist.gov/vuln/detail/CVE-2024-1298)

* Bugzilla: N/A
* [GHSA-chfw-xj8f-6m53](https://github.com/tianocore/edk2/security/advisories/GHSA-chfw-xj8f-6m53)
* Stable Tag where fixed: [202405](https://github.com/tianocore/edk2/releases/tag/edk2-stable202405)
* Commit(s) where fixed: [Push #6249](https://github.com/tianocore/edk2/pull/6249): available May 26, 2024
* Note: Binarly public issue

***

[CVE-2023-45237](https://nvd.nist.gov/vuln/detail/CVE-2023-45237)

* Bugzilla: [BZ 4542](https://bugzilla.tianocore.org/show_bug.cgi?id=4542) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204542%22))
* [GHSA-hc6x-cw6p-gj7h](https://github.com/tianocore/edk2/security/advisories/GHSA-hc6x-cw6p-gj7h)
* Stable Tag where fixed: [202405](https://github.com/tianocore/edk2/releases/tag/edk2-stable202405)
* Commit(s) where fixed: [Push #5582](https://github.com/tianocore/edk2/pull/5582): available WW21 (12 applicable
  commits for Intel-based platforms)
* **Important**: Due to new DEPEXs added in NetworkPkg to DxeNetLib.inf (gEfiRngProtocolGuid) and TcpDxe.inf
  (gEfiHash2ServiceBindingProtocolGuid), please ensure your platform has RngDxe.inf and Hash2CryptoDxe.inf included in
  your FDF/DSC files for full Network functionality.
* Note: NetworkPkg Bug 09
* Note: Adds new platform dependency (See [Update
  Note](https://github.com/tianocore/edk2/releases/tag/edk2-stable202405) and [Edk2 Devel
  #119227](https://edk2.groups.io/g/devel/message/119227))

***

[CVE-2023-45236](https://nvd.nist.gov/vuln/detail/CVE-2023-45236)

* Bugzilla: [BZ 4541](https://bugzilla.tianocore.org/show_bug.cgi?id=4541) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204541%22))
* [GHSA-hc6x-cw6p-gj7h](https://github.com/tianocore/edk2/security/advisories/GHSA-hc6x-cw6p-gj7h)
* Stable Tag where fixed: [202405](https://github.com/tianocore/edk2/releases/tag/edk2-stable202405)
* Commit(s) where fixed: [Push #5582](https://github.com/tianocore/edk2/pull/5582): available WW21 (12 applicable
  commits for Intel-based platforms)
* **Important**: Due to new DEPEXs added in NetworkPkg to DxeNetLib.inf (gEfiRngProtocolGuid) and TcpDxe.inf
  (gEfiHash2ServiceBindingProtocolGuid), please ensure your platform has RngDxe.inf and Hash2CryptoDxe.inf included in
  your FDF/DSC files for full Network functionality.
* Note: NetworkPkg Bug 08
* Note: Adds new platform dependency (See [Update
  Note](https://github.com/tianocore/edk2/releases/tag/edk2-stable202405) and [Edk2 Devel
  #119227](https://edk2.groups.io/g/devel/message/119227))

***

[CVE-2023-45235](https://nvd.nist.gov/vuln/detail/CVE-2023-45235)

* Bugzilla: [BZ 4540](https://bugzilla.tianocore.org/show_bug.cgi?id=4540) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204540%22))
* [GHSA-hc6x-cw6p-gj7h](https://github.com/tianocore/edk2/security/advisories/GHSA-hc6x-cw6p-gj7h)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: [Push #5352](https://github.com/tianocore/edk2/pull/5352): available WW06
* Note: NetworkPkg Bug 07

***

[CVE-2023-45234](https://nvd.nist.gov/vuln/detail/CVE-2023-45234)

* Bugzilla: [BZ 4539](https://bugzilla.tianocore.org/show_bug.cgi?id=4539) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204539%22))
* [GHSA-hc6x-cw6p-gj7h](https://github.com/tianocore/edk2/security/advisories/GHSA-hc6x-cw6p-gj7h)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: [Push #5352](https://github.com/tianocore/edk2/pull/5352): available WW06
* Note: NetworkPkg Bug 06

***

[CVE-2023-45233](https://nvd.nist.gov/vuln/detail/CVE-2023-45233)

* Bugzilla: [BZ 4538](https://bugzilla.tianocore.org/show_bug.cgi?id=4538) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204538%22))
* [GHSA-hc6x-cw6p-gj7h](https://github.com/tianocore/edk2/security/advisories/GHSA-hc6x-cw6p-gj7h)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: [Push #5352](https://github.com/tianocore/edk2/pull/5352): available WW06
* Note: NetworkPkg Bug 05

***

[CVE-2023-45232](https://nvd.nist.gov/vuln/detail/CVE-2023-45232)

* Bugzilla: [BZ 4537](https://bugzilla.tianocore.org/show_bug.cgi?id=4537) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204537%22))
* [GHSA-hc6x-cw6p-gj7h](https://github.com/tianocore/edk2/security/advisories/GHSA-hc6x-cw6p-gj7h)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: [Push #5352](https://github.com/tianocore/edk2/pull/5352): available WW06
* Note: NetworkPkg Bug 04

***

[CVE-2023-45231](https://nvd.nist.gov/vuln/detail/CVE-2023-45231)

* Bugzilla: [BZ 4536](https://bugzilla.tianocore.org/show_bug.cgi?id=4536) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204536%22))
* [GHSA-hc6x-cw6p-gj7h](https://github.com/tianocore/edk2/security/advisories/GHSA-hc6x-cw6p-gj7h)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: [Push #5352](https://github.com/tianocore/edk2/pull/5352): available WW06
* Note: NetworkPkg Bug 03

***

[CVE-2023-45230](https://nvd.nist.gov/vuln/detail/CVE-2023-45230)

* Bugzilla: [BZ 4535](https://bugzilla.tianocore.org/show_bug.cgi?id=4535) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204535%22))
* [GHSA-hc6x-cw6p-gj7h](https://github.com/tianocore/edk2/security/advisories/GHSA-hc6x-cw6p-gj7h)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: [Push #5352](https://github.com/tianocore/edk2/pull/5352): available WW06
* Note: NetworkPkg Bug 02

***

[CVE-2023-45229](https://nvd.nist.gov/vuln/detail/CVE-2023-45229)

* Bugzilla: [BZ 4534](https://bugzilla.tianocore.org/show_bug.cgi?id=4534) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204534%22))
* [GHSA-hc6x-cw6p-gj7h](https://github.com/tianocore/edk2/security/advisories/GHSA-hc6x-cw6p-gj7h)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: [Push #5352](https://github.com/tianocore/edk2/pull/5352): available WW06
* Note: NetworkPkg Bug 01

***

[CVE-2022-36765](https://nvd.nist.gov/vuln/detail/CVE-2022-36765)

* Bugzilla: [BZ 4166](https://bugzilla.tianocore.org/show_bug.cgi?id=4166) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204166%22))
* [GHSA-ch4w-v7m3-g8wx](https://github.com/tianocore/edk2/security/advisories/GHSA-ch4w-v7m3-g8wx)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: [Push #5252](https://github.com/tianocore/edk2/pull/5252): available January 16
* Note: HOB issue

***

[CVE-2022-36764](https://nvd.nist.gov/vuln/detail/CVE-2022-36764)

* Bugzilla: [BZ 4118](https://bugzilla.tianocore.org/show_bug.cgi?id=4118) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204118%22))
* [GHSA-4hcq-p8q8-hj8j](https://github.com/tianocore/edk2/security/advisories/GHSA-4hcq-p8q8-hj8j)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: Both [Push #5264](https://github.com/tianocore/edk2/pull/5264) and [Push
  #5273](https://github.com/tianocore/edk2/pull/5273) (last 3 commits)
* Note: TCG related

***

[CVE-2022-36763](https://nvd.nist.gov/vuln/detail/CVE-2022-36763)

* Bugzilla: [BZ 4117](https://bugzilla.tianocore.org/show_bug.cgi?id=4117) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%204117%22))
* [GHSA-xvv8-66cq-prwr](https://github.com/tianocore/edk2/security/advisories/GHSA-xvv8-66cq-prwr)
* Stable Tag where fixed: [202402](https://github.com/tianocore/edk2/releases/tag/edk2-stable202402)
* Commit(s) where fixed: Both [Push #5264](https://github.com/tianocore/edk2/pull/5264) and [Push
  #5273](https://github.com/tianocore/edk2/pull/5273) (last 3 commits)
* Note: TCG related

***

[CVE-2021-38578](https://nvd.nist.gov/vuln/detail/CVE-2021-38578)

* Bugzilla: [BZ 3387](https://bugzilla.tianocore.org/show_bug.cgi?id=3387) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%203387%22))
* Stable Tag where fixed: [202211](https://github.com/tianocore/edk2/releases/tag/edk2-stable202211)
* Commit(s) where fixed:
  [cab1f02565d3b29081dd21afb074f35fdb4e1fd6](https://github.com/tianocore/edk2/commit/cab1f02565d3b29081dd21afb074f35fdb4e1fd6)

***

[CVE-2021-38576](https://nvd.nist.gov/vuln/detail/CVE-2021-38576)

* Bugzilla: [BZ 3499](https://bugzilla.tianocore.org/show_bug.cgi?id=3499) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%203499%22))
* Stable Tag where fixed: [202302](https://github.com/tianocore/edk2/releases/tag/edk2-stable202302)
* Commit(s) where fixed: 1. [Push #1968](https://github.com/tianocore/edk2/pull/1968): sample code in SecurityPkg for
TcgPlatformDxe/PEI, 2. [Push #2034](https://github.com/tianocore/edk2/pull/2034): OvmfPkg support for disabling the TPM
2 platform hierarchy, (Note: There is also an example platform implementation available in [edk2-platforms](https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/MinPlatformPkg/Tcg/Library/PeiDxeTpmPlatformHierarchyLib))

***

[CVE-2021-38575](https://nvd.nist.gov/vuln/detail/CVE-2021-38575)

* Bugzilla: [BZ 3356](https://bugzilla.tianocore.org/show_bug.cgi?id=3356) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%203356%22))
* Stable Tag where fixed: [202108](https://github.com/tianocore/edk2/releases/tag/edk2-stable202108)
* Commit(s) where fixed: [Push #1698](https://github.com/tianocore/edk2/pull/1698)

***

[CVE-2021-28213](https://nvd.nist.gov/vuln/detail/CVE-2021-28213)

* Bugzilla: [BZ 1866](https://bugzilla.tianocore.org/show_bug.cgi?id=1866) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%201866%22))
* Stable Tag where fixed: [201905](https://github.com/tianocore/edk2/releases/tag/edk2-stable201905)
* Commit(s) where fixed:
  [d55d9d0664366efe731db461e14c6fc380fca776](https://github.com/tianocore/edk2/commit/d55d9d0664366efe731db461e14c6fc380fca776)
  (removed NetworkPkg/IpSecDxe driver per [BZ 1697](https://bugzilla.tianocore.org/show_bug.cgi?id=1697) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%201697%22)))

***

[CVE-2021-28211](https://nvd.nist.gov/vuln/detail/CVE-2021-28211)

* Bugzilla: [BZ 1816](https://bugzilla.tianocore.org/show_bug.cgi?id=1816) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%201816%22))
* Stable Tag where fixed: [202011](https://github.com/tianocore/edk2/releases/tag/edk2-stable202011)
* Commit(s) where fixed:
  [6aeaea14e97f2a36f07ccd4fd2ffb971d68b3b0a](https://github.com/tianocore/edk2/pull/1138/commits/6aeaea14e97f2a36f07ccd4fd2ffb971d68b3b0a)

***

[CVE-2021-28210](https://nvd.nist.gov/vuln/detail/CVE-2021-28210)

* Bugzilla: [BZ 1743](https://bugzilla.tianocore.org/show_bug.cgi?id=1743) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%201743%22))
* Stable Tag where fixed: [202011](https://github.com/tianocore/edk2/releases/tag/edk2-stable202011)
* Commit(s) where fixed: [Push #1137](https://github.com/tianocore/edk2/pull/1137)

***

[CVE-2019-14587](https://nvd.nist.gov/vuln/detail/CVE-2019-14587)

* Bugzilla: [BZ 1989](https://bugzilla.tianocore.org/show_bug.cgi?id=1989) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%201989%22))
* Stable Tag where fixed: [202002](https://github.com/tianocore/edk2/releases/tag/edk2-stable202002)
* Commit(s) where fixed:
  [e36d5ac7d10a6ff5becb0f52fdfd69a1752b0d14](https://github.com/tianocore/edk2/commit/e36d5ac7d10a6ff5becb0f52fdfd69a1752b0d14)

***

[CVE-2019-14586](https://nvd.nist.gov/vuln/detail/CVE-2019-14586)

* Bugzilla: [BZ 1995](https://bugzilla.tianocore.org/show_bug.cgi?id=1995) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%201995%22))
* Stable Tag where fixed: [202002](https://github.com/tianocore/edk2/releases/tag/edk2-stable202002)
* Commit(s) where fixed:
  [c32be82e99ef272e7fa742c2f06ff9a4c3756613](https://github.com/tianocore/edk2/commit/c32be82e99ef272e7fa742c2f06ff9a4c3756613)

***

[CVE-2019-14584](https://nvd.nist.gov/vuln/detail/CVE-2019-14584)

* Bugzilla: [BZ 1914](https://bugzilla.tianocore.org/show_bug.cgi?id=1914) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%201914%22))
* Stable Tag where fixed: [202011](https://github.com/tianocore/edk2/releases/tag/edk2-stable202011)
* Commit(s) where fixed:
  [26442d11e620a9e81c019a24a4ff38441c64ba10](https://github.com/tianocore/edk2/commit/26442d11e620a9e81c019a24a4ff38441c64ba10)

***

[CVE-2019-14575](https://nvd.nist.gov/vuln/detail/CVE-2019-14575)

* Bugzilla: [BZ 1608](https://bugzilla.tianocore.org/show_bug.cgi?id=1608) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%201608%22))
* Stable Tag where fixed: [202002](https://github.com/tianocore/edk2/releases/tag/edk2-stable202002)
* Commit(s) where fixed: BZ [Comment 60](https://bugzilla.tianocore.org/show_bug.cgi?id=1608#c60) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%201608%22)) is “Pushed
  fbb9607223...c230c002ac” with [10
  results](https://github.com/search?q=repo%3Atianocore%2Fedk2+CVE-2019-14575&type=commits) from search:

1.
[c230c002accc4281ccc57bba7153a9b2d9b9ccd3](https://github.com/tianocore/edk2/commit/c230c002accc4281ccc57bba7153a9b2d9b9ccd3)
2.
[cb30c8f25162e6d8142c6b098f14c1e4e7f125ce](https://github.com/tianocore/edk2/commit/cb30c8f25162e6d8142c6b098f14c1e4e7f125ce)
3.
[fbb96072233b5eaecf4d229cbee47b13dcab39e1](https://github.com/tianocore/edk2/commit/fbb96072233b5eaecf4d229cbee47b13dcab39e1)
4.
[5cd8be6079ea7e5638903b2f3da0f4c10ec7f1da](https://github.com/tianocore/edk2/commit/5cd8be6079ea7e5638903b2f3da0f4c10ec7f1da)
5.
[c13742b180095e5181e41dffda954581ecbd9b9c](https://github.com/tianocore/edk2/commit/c13742b180095e5181e41dffda954581ecbd9b9c)
6.
[b1c11470598416c89c67b75c991fd0773bcbab9d](https://github.com/tianocore/edk2/commit/b1c11470598416c89c67b75c991fd0773bcbab9d)
7.
[a83dbf008cc73406cbdc0d5ac3164cc19fff6683](https://github.com/tianocore/edk2/commit/a83dbf008cc73406cbdc0d5ac3164cc19fff6683)
8.
[adc6898366298d1f64b91785e50095527f682758](https://github.com/tianocore/edk2/commit/adc6898366298d1f64b91785e50095527f682758)
9.
[929d1a24d12822942fd4f9fa83582e27f92de243](https://github.com/tianocore/edk2/commit/929d1a24d12822942fd4f9fa83582e27f92de243)
10.
[9e569700901857d0ba418ebdd30b8086b908688c](https://github.com/tianocore/edk2/commit/9e569700901857d0ba418ebdd30b8086b908688c)

***

[CVE-2019-14563](https://nvd.nist.gov/vuln/detail/CVE-2019-14563)

* Bugzilla: [BZ 2001](https://bugzilla.tianocore.org/show_bug.cgi?id=2001) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%202001%22))
* Stable Tag where fixed: [202011](https://github.com/tianocore/edk2/releases/tag/edk2-stable202011)
* Commit(s) where fixed:
  [322ac05f8bbc1bce066af1dabd1b70ccdbe28891](https://github.com/tianocore/edk2/commit/322ac05f8bbc1bce066af1dabd1b70ccdbe28891)

***

[CVE-2019-14562](https://nvd.nist.gov/vuln/detail/CVE-2019-14562)

* Bugzilla: [BZ 2215](https://bugzilla.tianocore.org/show_bug.cgi?id=2215) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%202215%22))
* Stable Tag where fixed: [202008](https://github.com/tianocore/edk2/releases/tag/edk2-stable202008)
* Commit(s) where fixed:
  [0b143fa43e92be15d11e22f80773bcb1b2b0608f](https://github.com/tianocore/edk2/commit/0b143fa43e92be15d11e22f80773bcb1b2b0608f)

***

[CVE-2019-14559](https://nvd.nist.gov/vuln/detail/CVE-2019-14559)

* Bugzilla: [BZ 2031](https://bugzilla.tianocore.org/show_bug.cgi?id=2031) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%202031%22))
* Stable Tag where fixed: [202002](https://github.com/tianocore/edk2/releases/tag/edk2-stable202002)
* Commit(s) where fixed:
  [1d3215fd24f47eaa4877542a59b4bbf5afc0cfe8](https://github.com/tianocore/edk2/commit/1d3215fd24f47eaa4877542a59b4bbf5afc0cfe8)

***

[CVE-2019-14553](https://nvd.nist.gov/vuln/detail/CVE-2019-14553)

* Bugzilla: [BZ 960](https://bugzilla.tianocore.org/show_bug.cgi?id=960) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%20960%22))
* Stable Tag where fixed: [201911](https://github.com/tianocore/edk2/releases/tag/edk2-stable201911)
* Commit(s) where fixed: BZ [Comment 47](https://bugzilla.tianocore.org/show_bug.cgi?id=960#c47) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%20960%22)) is “Pushed as commit
  range b15646484eaf..e2fc50812895” with [8
  results](https://github.com/search?q=repo%3Atianocore%2Fedk2+CVE-2019-14553&type=commits) from search:

1.
[e2fc50812895b17e8b23f5a9c43cde29531b200f](https://github.com/tianocore/edk2/commit/e2fc50812895b17e8b23f5a9c43cde29531b200f)
2.
[703e7ab21ff8fda9ababf7751d59bd28ad5da947](https://github.com/tianocore/edk2/commit/703e7ab21ff8fda9ababf7751d59bd28ad5da947)
3.
[2ca74e1a175232cc201798e27437700adc7fb07e](https://github.com/tianocore/edk2/commit/2ca74e1a175232cc201798e27437700adc7fb07e)
4.
[8d16ef8269b2ff373d8da674e59992adfdc032d3](https://github.com/tianocore/edk2/commit/8d16ef8269b2ff373d8da674e59992adfdc032d3)
5.
[1e72b1fb2ec597caedb5170079bb213f6d67f32a](https://github.com/tianocore/edk2/commit/1e72b1fb2ec597caedb5170079bb213f6d67f32a)
6.
[2ac41c12c0d4b3d3ee8f905ab80da019e784de00](https://github.com/tianocore/edk2/commit/2ac41c12c0d4b3d3ee8f905ab80da019e784de00)
7.
[eb520d94dba7369d1886cd5522d5a2c36fb02209](https://github.com/tianocore/edk2/commit/eb520d94dba7369d1886cd5522d5a2c36fb02209)
8.
[31efec82796cb950e99d1622aa9c0eb8380613a0](https://github.com/tianocore/edk2/commit/31efec82796cb950e99d1622aa9c0eb8380613a0)

***

[CVE-2017-5731](https://nvd.nist.gov/vuln/detail/CVE-2017-5731)

* Bugzilla: [BZ 686](https://bugzilla.tianocore.org/show_bug.cgi?id=686) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%20686%22))
* Stable Tag where fixed: Pre-Stable Tags: Edk2-master (2018), UDK2018, UDK2017, UDK2015
* Commit(s) where fixed: BZ [Comment 10](https://bugzilla.tianocore.org/show_bug.cgi?id=686#c10) ([GitHub Issue](https://github.com/tianocore/edk2/issues?q=is%3Aissue%20in%3Atitle%20%22Bugzilla%20Bug%20686%22)) is “Fix it in
   edk2 master
         2ec7953d49677142c5f7552e9e3d96fb406ba0c4..041d89bc0f0119df37a5fce1d0f16495ff905089
   edk2 UDK2018
     fb72f6fd6f1c4130f0d0037f33a5153fe9fdb322..96c32854ad69cb7cc983165926d58049f7ab27cc
   edk2 UDK2017
     167e6e48af8dfd558aa3c7497959092d58b26d54..1d707a02d86e5f43cf0ed2cd43f7583a8d7a39db
   edk2 UDK2015
     ee9ec6e6426f8f36bb9cd1301eb836959ef1412e..551888b06a1987b9db5040e10cdde5be34236653
with [3 results](https://github.com/search?q=repo%3Atianocore%2Fedk2+CVE-2017-5731&type=commits) from search:

1.
[041d89bc0f0119df37a5fce1d0f16495ff905089](https://github.com/tianocore/edk2/commit/041d89bc0f0119df37a5fce1d0f16495ff905089)
2.
[684db6da64bc7b5faee4e1174e801c245f563b5c](https://github.com/tianocore/edk2/commit/684db6da64bc7b5faee4e1174e801c245f563b5c)
3.
[2ec7953d49677142c5f7552e9e3d96fb406ba0c4](https://github.com/tianocore/edk2/commit/2ec7953d49677142c5f7552e9e3d96fb406ba0c4)

***

[CVE-2014-8271](https://nvd.nist.gov/vuln/detail/CVE-2014-8271),
[CERT CC VU# 533140](https://www.kb.cert.org/vuls/id/533140)

* Bugzilla: Pre-BZ, [Tianocore SA
  17](https://tianocore-docs.github.io/SecurityAdvisory/draft/buffer_overflow_in_variable_reclaim.html)
* Stable Tag where fixed: Pre-Stable Tags: UDK2015 +
* Commit(s) where fixed: Originally:
  [https://sourceforge.net/p/edk2/code/16280/](https://sourceforge.net/p/edk2/code/16280/),
[https://github.com/tianocore/edk2/commit/6ebffb67c8eca68cf5eb36bd308b305ab84fdd99](https://github.com/tianocore/edk2/commit/6ebffb67c8eca68cf5eb36bd308b305ab84fdd99)

***

[CVE-2014-4860](https://nvd.nist.gov/vuln/detail/CVE-2014-4860), [CERT CC VU# 552286](https://www.kb.cert.org/vuls/id/552286)

* Bugzilla: Pre-BZ, [Tianocore SA
  15](https://tianocore-docs.github.io/SecurityAdvisory/draft/buffer_overflows_in_capsule_update.html)
* Stable Tag where fixed: Pre-Stable Tags: UDK2015 +
* Commit(s) where fixed: Originally:
  [https://sourceforge.net/p/edk2/code/15137](https://sourceforge.net/p/edk2/code/15137),
  [https://github.com/tianocore/edk2/commit/ff284c56a11a9a9b32777c91bc069093d5b5d8a9](https://github.com/tianocore/edk2/commit/ff284c56a11a9a9b32777c91bc069093d5b5d8a9)

***

[CVE-2014-4859](https://nvd.nist.gov/vuln/detail/CVE-2014-4859), [CERT CC](https://www.kb.cert.org/vuls/id/552286)
[VU# 552286](https://www.kb.cert.org/vuls/id/552286)

* Bugzilla: Pre-BZ,
[Tianocore SA 15](https://tianocore-docs.github.io/SecurityAdvisory/draft/buffer_overflows_in_capsule_update.html)
* Stable Tag where fixed: Pre-Stable Tags: UDK2015 +
* Commit(s) where fixed: Originally:
  [https://sourceforge.net/p/edk2/code/15136](https://sourceforge.net/p/edk2/code/15136),
[https://github.com/tianocore/edk2/commit/3a1966c4e2f04374178872b064c3a8e42a0eb776](https://github.com/tianocore/edk2/commit/3a1966c4e2f04374178872b064c3a8e42a0eb776)
