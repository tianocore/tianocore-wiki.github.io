# Security Advisory Process

Security Advisory Process

Now using Security reporting through the following:
[Reporting Security Issues](reporting_security_issues.md)

Below for archive reference only

------------------------------------------------------------------------

- **IMPORTANT:** All mail sent via this reflector are TianoCore
  [Code Contributions](../../development/contribution-guides/code_contributions.md)

- **IMPORTANT:** Please consider encrypting your message. We support
  S/MIME. If you do not have encryption available, describe the issue in
  generalities to establish the converstation. We can then work out some
  mutually agreeable method of encrypting the details in subsequent
  private conversations.

- **IMPORTANT:** Please indicate in your initial mail if you are asking
  for any embargo on the information in the mail or on fixes associated
  with the report (ahead of a security conference, for example). We do
  not make assurances that all such requests will be granted.

- Common practice in the software community when managing a security
  issue is:

1. Receive or discover an issue.
2. Rank the issue using DREAD or CVSS or ...
3. If the issue is low risk, simply provide a fix via the site's
    non-security bug fixing methods
4. Describe the issue,its vulnerabilities, and the fix to the site's
    trusted users
5. Embargo the fix from the open site to give time for the users to
    review and apply the fix and provide the fixed update to their
    customers.
6. After the embargo is complete, the complete vulnerability is
    described on the site.

------------------------------------------------------------------------

We are aware that BIOS/Boot firmware is rarely updated by consumers or
by enterprise IT departments. This makes the embargo generally
impractical. It also makes the complete vulnerability description
counterproductive. So, this site operates under the following modified
version:

- Modified Steps:

1. *same as above*
2. *same as above*
3. *same as above*
4. Describe the issue, its vulernabilities, and the fix to the site's
    trusted users. The users have 1 week to provide comments and request
    an embargo.
5. If reasonable rationale is provided for an embargo, the embargo
    occurs (as in 5 above).
6. The issue is fixed in the public tree, but only the normal bug
    description is provided.
7. A list of all issues fixed in designated files are provided via a
    pdf on this page.

(As noted above, the embargo in step 5 may also be requested as a part
of the initial issue report.)

Designated files (step 7) are indicated by a comment in the header that
the file may process untrusted input. These files are so designated
because they protect the assets in the tianocore.org threat model. See
([https://www.uefi.org/sites/default/files/resources/Intel-UEFI-ThreatModel.pdf](https://www.uefi.org/sites/default/files/resources/Intel-UEFI-ThreatModel.pdf))
for information on the assets. If you believe other files should be so
designated, please use the report button to provide the path. The
"untrusted input" header comment is also used to indicate that the files
so marked have had focused security reviews and may require security
reviews when changes are made.

As implied in step 4 above, tianocore.org maintains a list of trusted
users who receive early notification of vulnerabilities so that
implementations using tianocore.org code may be upgraded and distributed
prior to open notification. By joining the list, you agree to limit
disclosure of any issues reported by the reflector to those necessary to
fix and manage the issue. Failure to effectively manage this
confidential information may result in your removal from the reflector.
The list is managed by the trusted users on the list itself. Those
managing the list reserve the right to limit the size and membership of
the list.
