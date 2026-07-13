# How to report a Security Issue

```admonish important
The latest tracking and update of security issues for EDKII can be found at [GHSA-GitHub-Security-Advisories-Process](ghsa_github_security_advisories_process_draft.md).
```

At present the repository tracked by Tianocore Infosec includes the main EDKII repository
([https://github.com/tianocore/edk2](https://github.com/tianocore/edk2)). For issues found in repos like
([https://github.com/tianocore/edk2-platforms](https://github.com/tianocore/edk2-platforms)) recommend reaching out to
the respective component who is named in the subdirectory - you can find a list of relevant companies in this domain at
([https://uefi.org/security](https://uefi.org/security)), for example.

Also, it is encouraged to attach a patch that mitigates the issues with the bug report, if possible.

## How Security Issues are Evaluated

When a Tianocore Security Issue is entered, the issue is evaluated by the **Infosec** group to determine if the issue is
a security issue or not. If it is not deemed to be a security issue, then the issue is converted to a standard issue and
follows the normal issue resolution process. If the issue is confirmed to be a security issue, then the priority,
severity, and impact of the issue is assessed by the **Infosec** group. Discussions, resolution, and patches are
completed within GHSA. A date for public disclose is determined, and on that date the issue is made public and added to
the list of Security Advisories.

If you believe you have found a security vulnerability (per [MITRE’s definition](https://cwe.mitre.org/documents/glossary/index.html#Vulnerability)),
please submit the report as an [EDK II security advisory](https://github.com/tianocore/edk2/security/advisories/new).
Please include the requested information (as much as you can provide) to help us better understand the nature and scope
of the possible issue.

The following will help us triage and advance the report more quickly:

- Proof-of-concept or exploit code
- Proposed patch

Priority will be given to reports that include proof-of-concept or exploit code. EDK II Infosec may decide to reduce the
Severity of the report to Low if a proof-of-concept or exploit code is not included in the report. Alternatively, EDK II
Infosec may decide not to accept issues without this exploitability insight.

Please analyze AI-generated results before submitting a report. Use the capabilities of AI models to generate
proof-of-concept code and suggested patches, then include them in the report.

The outputs of tools are not sufficient. Although automation tools and fuzzers and scanners are common trade practice
in the security community, they often produce many results for further investigation and can yield many false positives.
Reports from automated tools or scans must include additional analysis to demonstrate the exploitability of the
vulnerability.

If you are interested in being involved in the evaluation of Tianocore Security Issues, then please send an email
request to join the Tianocore Infosec group to the Tianocore Community Manager or one of the Tianocore Stewards.

```admonish warning
Never send any details related to a security issue in email.
```

Also, Tianocore Infosec team members should only share details of unmitigated issues within the draft GHSA. Any sharing
of unmitigated issues on un-encrypted email or open source prior to embargo expiry may lead to removal from the Infosec
group.

Now that Tianocore is a CNA [https://cve.mitre.org/cve/cna.html](https://cve.mitre.org/cve/cna.html), namely
[https://www.cvedetails.com/product/64326/Tianocore-Edk2.html?vendor_id=19679](https://www.cvedetails.com/product/64326/Tianocore-Edk2.html?vendor_id=19679),
CVE issuance will be a “Must” for Tianocore content and “May” for downstream derivatives of Tianocore (open or closed).
We request that the reporter perform the initial CVSS calculation. Recommend using
[https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:L/I:H/A:L](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:L/AC:L/PR:L/UI:N/S:U/C:L/I:H/A:L).
If reporter doesn’t wish to grade, then Infosec will propose a grade and share w/ reporter prior to applying the
grading.

The Tianocore Infosec team uses the following flow to evaluate items
![](https://github.com/jwang36/tianocore.github.io/raw/master/security/flowchart.svg?sanitize=true)

## Security Advisories

List of current EDK II Security Advisories can be found at:

- **[Published GHSAs](https://github.com/tianocore/edk2/security/advisories?state=published)**
- **[Security Advisory Log Gitbook](https://tianocore-docs.github.io/SecurityAdvisory/draft/)** (no longer maintained)

List of all Third Party EDK II Security Advisories can be found at:

- **[Third Party Security Advisory Log](https://www.tianocore.org/tianocore-wiki.github.io/security/advisories/third_party_security_advisories.html)**
