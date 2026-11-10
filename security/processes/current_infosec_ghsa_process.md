# InfoSec GHSA Process

This page provides the steps that the EDK II InfoSec team will perform to process a GitHub Security Advisory submission. The documentation was derived from the two draft proposals listed below. Over time, more of the draft content will become part of the current process.

[Process for GHSA](proposals/ghsa_github_security_advisories_process_draft.md)

[Infosec GHSA Process Proposal](proposals/infosec_ghsa_process_proposal_draft.md)

```admonish important
Due to the high number of GHSA submissions, the triage and mitigation timelines may be extended. Please work with the maintainers to ensure that all patches are applied to the private GHSA fork and reviewed as soon as possible.

Due to this higher-than-normal submission rate, additional meetings are being scheduled on a weekly basis to triage and mitigate valid reports.
```

## Role Definitions

| Role | Description |
|------|-------------|
| **Reporter** | The person or team submitting the GHSA. |
| **InfoSec Team** | Members of the `tianocore/tianocore-infosec-team`. |
| **InfoSec Admins** | A small group of the **InfoSec Team** that have access to newly reported GHSAs. |
| **Package Maintainer** | The person listed in MAINTAINERS.txt that is responsible for the code impacted by the reported GHSA. |
| **Package Reviewers** | The people listed in MAINTAINERS.txt that review changes for the code impacted by the reported GHSA. |
| **Tianocore Admins** | Tianocore members belonging to the Admin team. |
| **Tianocore Stewards** | Provide strategic direction and administration for the Tianocore project. |

## Stages

The following are the stages that a vulnerability goes through until it is patched. To ensure that GHSAs progress through each of these steps, monthly meetings are scheduled to track and manage the progress. Additional meetings will be scheduled when needed to focus on larger groups of issues.

### Stage: Report

A Security Advisory begins when a **Reporter** reports a vulnerability.

This is done by submitting a new EDK II GHSA via this link [New Edk2 Security Advisory Draft](https://github.com/tianocore/edk2/security/advisories/new). A new EDK II Platforms GHSA can be created via this link [New Edk2-Platforms Security Advisory Draft](https://github.com/tianocore/edk2-platforms/security/advisories/new). Complete instructions for filing a new draft GHSA can be found on the [How to report a Security Issue](reporting_security_issues.md) page.

Alternatively, a report may be submitted via [VINCE](https://www.kb.cert.org/vince/). In that case, the reporter should be directed to the GitHub security page to submit the security issue for internal tracking.

### Stage: Intake

A member of the Tianocore **InfoSec Admins** will periodically review the list of new GHSA drafts to perform basic intake actions.

* Add the `tianocore/tianocore-infosec-team` to the contributors list
* Add the **Package Maintainer** to the contributors list
* Adjust the scope and package information to help with filtering
* Create the private GHSA fork
* Add a comment asking the **Package Maintainer** to review the report to ensure it is a valid security issue

The person performing the intake of a new GHSA draft may also decide to include a request for review and specifically reference one or more package maintainers.

### Stage: Triage

It is up to the **Package Maintainer** to determine if the reported issue is a vulnerability. The **Package Maintainer** may also request guidance or input from the **InfoSec Team**. Once a vulnerability is confirmed, the remediation process should begin.

If the submission is not found to be a vulnerability, the **Package Maintainer** must communicate this to the **InfoSec Team** for further action. The **InfoSec Team** will work with the submitter to close the issue. The **Reporter** is allowed to (and probably will) write up public documentation and submit a fix to the public repository.

### Stage: Fix

All work must be done on a private GHSA fork. If one does not exist, contact the **InfoSec Team** to create the fork. DO NOT proceed with a mitigation without a private fork to use for development.

The **Package Maintainer** should review any mitigations provided by the **Reporter** and provide their review feedback. In some cases, it may be beneficial for the **Reporter** and **Package Maintainer** to directly collaborate on the mitigation. If a mitigation was not provided, it is up to the **Package Maintainer** to provide a mitigation or work with the **InfoSec Team** to identify an appropriate developer to develop the mitigation. The status and timeline for the mitigation should be tracked in the GHSA comments.

The **Package Maintainer** may wish to include other developers and reviewers in the development of the mitigation. This should be coordinated with the **InfoSec Team** to ensure appropriate access to the GHSA private fork.

### Stage: Review

Once the mitigation is implemented, the **Package Maintainer** should notify the **InfoSec Team** and determine a set of required reviewers. Ideally, this is two or more of the **Package Reviewers** as well as the **Reporter** of the vulnerability. The review will be considered complete when the **Package Reviewers** or **Package Maintainer** have accepted the patch. If two **Package Reviewers** cannot be found, then representatives from the **InfoSec Team** or a **Package Maintainer** may act as the additional or final approver.

In some cases, a **Package Maintainer** may not exist or may not be responsive to requests. In this case, the **InfoSec Team** should attempt to find an alternate representative to provide a fix. The representative could be from the **InfoSec Team** or a subject matter expert who is willing to volunteer. The **InfoSec Team** may also need to perform a similar search to identify additional reviewers. A pool of subject matter experts should be maintained by the **InfoSec Team** to simplify the search in the future. If no representatives can be identified, the **InfoSec Team** should reach out to one of the **Tianocore Stewards** for help with the search. The option exists for the **Tianocore Stewards** to review and approve the mitigation once all required changes are made.

Once the review is complete, the status of the patch should be updated in the comments of the GHSA. The review and feedback should be integrated as soon as possible to allow for the maximum amount of validation time.

Pipelines and GitHub workflows do not work in private GHSA forks. To reduce the work that will need to take place in public, the **Package Maintainer** should run as many CI checks locally as they can (formatting, etc.).

Examples: PatchCheck, Uncrustify, Unit Tests, etc.

Additional **InfoSec Team** review steps:
* Review and finalize the CVSS score
* Ensure that the correct CWEs have been assigned
* Allocate a CVE

Once the review process is complete, the **Reporter** should be encouraged to verify that the mitigation is correct and complete.

### Stage: Embargo

The **InfoSec Team** should work together to determine an appropriate disclosure date. The embargo period is 60 days under normal circumstances, but it may change based on several factors. The embargo period begins as soon as the proposed mitigation has completed the review process and is approved. In some cases, the **InfoSec Team** and **Reporter** may need to negotiate the date to meet the expectations of both sides. Once an appropriate embargo period is determined, the embargo information and end date should be captured in the GHSA and clearly communicated to the **Reporter**.

During the embargo period, additional patch validation should continue. Any issues with the mitigation should be communicated and any additional fixes integrated. Whenever possible, fixes should be provided by the validation team that identified the failures with the mitigation. Depending on the number and type of failures discovered, the embargo period may need to be renegotiated.

The embargo period should also be used by the **InfoSec Team** to finalize the wording of the GHSA as well as the CVSS score. This will allow the **InfoSec Team** and the **Reporter** to agree on the content of the GHSA prior to publication and reduce the potential for last-minute changes during the publication process.

### Stage: Publish

The **InfoSec Team** should first ensure that the **Package Maintainer** and **Package Reviewers** are available to support the GHSA publishing process. In some cases, one or more of the **Tianocore Stewards** may be required to support the GHSA publication process if some participants are not available.

Once the embargo period is complete and the team is ready to publish, the following steps should be completed within 24 hours.

A **Package Maintainer** or **Tianocore Admins** must create a branch that follows the format `security-advisory/cve-xxxx-xxxx/advisory` on the destination repository. This special advisory branch will be used as a means to publish the mitigation so that the community can be notified and provide comments. This is also the branch that will be used to generate the PR to the ***master*** branch.

The following steps should be done by the **Package Maintainer** or a member of the **InfoSec Team**.
1. Create a PR from the GHSA private fork to the newly created advisory branch.
  * This step may be done by any developer with access to the GHSA private fork.
2. Close the original PR targeting the repository's default branch. Confirm that only the PR targeting the advisory branch remains open, then use **Merge pull request(s)** on the GHSA page to merge the mitigation into the advisory branch.
  * At this point the PR in the GHSA private fork that was targeting the ***master*** branch can be closed.
3. Create a PR from the advisory branch to ***master***.
  * The **Package Maintainer** may be required to make additional modifications to pass CI. Running CI locally prior to this point will reduce the chance that additional changes will be required.
4. Get reviewer approvals.
5. Merge the PR to ***master***.

Once the PR is merged into the ***master*** branch, the GHSA should be published by a member of the **InfoSec Team**.
