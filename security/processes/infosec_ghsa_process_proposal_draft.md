# Infosec GHSA Process Proposal

**Status: draft for community review, not adopted policy.**

This proposal describes how EDK II participants investigate, remediate and
disclose reported vulnerabilities through GitHub Security Advisories (GHSAs).

## Principles

- The advisory and its appropriately restricted linked records are the official
  record for decisions, responsibilities and disclosure commitments.
- A report describes a potential vulnerability. A CVE identifier is not required
  to begin investigation and does not, by itself, confirm a vulnerability.
- Acceptance, private-fork creation, development start, review approval,
  publication and upstream merge are separate events.
- Keep unpublished reports and patches in authorized private channels until
  the agreed disclosure window.
- Publication and upstream merge do not establish that downstream products
  have received or deployed a fix.

## Roles and responsibilities

The Coordinator confirms each participant's agreement to do the work and
arranges the required access. One person may perform multiple roles, but a
patch author cannot provide their own independent review approval.

Use GitHub's [advisory credits](https://docs.github.com/en/code-security/repository-security-advisories/creating-a-repository-security-advisory#about-credits-for-repository-security-advisories)
to acknowledge contributions. Credit invitations are not work assignments,
patch approvals or grants of private-advisory access.

### Reporter

- Submit the report, affected configurations and supporting evidence.
- Answer investigation questions and, where possible, validate the proposed fix.
- Coordinate disclosure and attribution with the Coordinator. Credit the Finder
  separately when the person who discovered the issue is not the Reporter.

### Coordinator

- Own the case from receipt through closure or publication and merge follow-through.
- Find the Analyst, Developer and independent Reviewers; confirm their participation.
- Be the point of contact for reporter updates and disclosure commitments.
- Record decisions, assignments, blockers and agreed schedules in the advisory.
- Arrange permissions with an authorized repository or organization owner.
- Escalate stalled work and coordinate schedule changes.

### Analyst

- Assess project applicability, security impact, affected revisions and evidence.
- Investigate possible duplicates using the original reports and underlying
  mechanism, not merely a shared package or CVE.
- Record findings, uncertainties and a recommendation to accept, investigate
  further or close. Hand the evidence to the Developer.

At least one **Project Analyst** must participate: an Analyst listed as a
maintainer, reviewer or steward in the current upstream `Maintainers.txt`, or
a verified member of the
[Tianocore InfoSec team](https://github.com/orgs/tianocore/teams/tianocore-infosec-team).
Prefer relevant component expertise, but a package match is not mandatory.
External researchers may also contribute analysis and receive Analyst credits.
Project Analyst is a process qualification, not a separate GitHub credit type.

### Remediation Developer

- Accept the work and record the actual development-start date.
- Develop the patch and document affected and fixed revisions.
- Record tests, results, platforms, configurations and known limitations.
- Follow the patch through private review, publication and upstream merge.

### Remediation Reviewer

- Provide relevant technical expertise during development.
- Independently review the patch, security implications and regression risks.
- Record testing and explicit approval or outstanding review requirements.

### Remediation Verifier

- Test the vulnerability or fix and record the revisions, configurations and results.
- This function may be performed by a Reviewer, Reporter or another qualified
  participant; it does not require an additional person or replace review approval.

### Package Maintainer

- Help resolve ownership, review and integration blockers.
- Provide independent additional or final approval when two Reviewers cannot
  be found, with the substitution recorded.
- Shepherd the reviewed patch into the upstream primary branch.

### Security Advisory Writer

- Prepare the advisory's affected versions, description, references, CVE and credits.
- Coordinate public-branch preparation and advisory publication.
- Confirm publication permissions before the agreed handoff.
- The Coordinator or an authorized maintainer may perform this function; it is
  not a separate GitHub credit type.

### Decision authority and participant discovery

An authorized Package Maintainer or InfoSec lead makes acceptance and closure
decisions based on the Project Analyst's recommendation, and approves any
exception to the CVE-before-publication requirement. The Coordinator records
the decision and approver. These decisions do not replace required patch reviews.

Use [Maintainers.txt](https://github.com/tianocore/edk2/blob/master/Maintainers.txt)
to find contacts for the affected component. Involve InfoSec or the Tianocore
Stewards when scope or ownership is unclear. Confirm expertise, availability and
private access rather than treating a directory entry as an assignment.

## Records and timing

Record significant decisions in the private advisory discussion or an
appropriately restricted linked record. Include the responsible person,
decision, supporting evidence and actual event date, with a time zone,
preferably UTC. Keep proposed targets separate from actual events.

At development kickoff, the Coordinator records and communicates:

1. The Developer's agreement and actual development-start date.
2. The embargo-start date and disclosure date agreed with the Reporter.
3. Development and testing/review targets.
4. Responsible participants and the next reporter update.

| Expectation | Starting point or condition |
|---|---|
| 180-day embargo | Embargo start agreed with the Reporter |
| Development ideally completed within 90 days | Actual development kickoff |
| At least a further 90 days for testing and feedback | Planned interval before disclosure |
| Publication handoff within one day | Agreed publication window after readiness approval |
| 24 hours for public review | Public upstream PR and patch are available |
| Upstream merge promptly after public review | Review period elapsed and required approvals/checks satisfied |

Plan development and testing together within the agreed embargo. If development
and embargo start on different dates, record both. If the schedule cannot meet
these expectations, the Coordinator and maintainer coordinate a revised plan
with the Reporter and record the agreement before changing committed dates.

Advisory acceptance, fork creation and CVE assignment do not start these clocks.
Record the relevant start events explicitly.

## Workflow

### Report and triage

The Reporter submits through
[EDK2's Security page](https://github.com/tianocore/edk2/security).

![Report a vulnerability](../../images/infosec_ghsa_report_a_vulnerability.png)
![Submit vulnerability details](../../images/infosec_ghsa_submit_vulnerability_details.png)

For reports received through VINCE or another channel, coordinate a private
GitHub record with the Reporter and preserve the original reference. Reuse an
existing advisory rather than creating a duplicate.

Acknowledge receipt, identify the Coordinator and assign a Project Analyst.
Keep the report in triage while the initial assessment is pending.

### Assessment and acceptance

The Project Analyst records project applicability, impact, affected revisions,
reproduction evidence, possible duplicates and remaining uncertainties.
Request additional information where needed.

The Coordinator obtains and records the acceptance decision under the authority
defined above. An authorized participant accepts the report through GitHub.
For a maintainer-created draft, explicitly record the decision to proceed.

For this process, **GitHub draft means Accepted, not yet published**. Acceptance
means proceeding with coordinated investigation and remediation, not that every
technical claim is established. Record unresolved questions and next actions.

### Private fork and development

An authorized participant checks whether a temporary private fork already exists
and creates one if needed. Use one advisory fork, with multiple branches where
necessary, and verify collaborator access.

If "Start a temporary private fork" is unavailable, contact the
[EDKII InfoSec team lead](https://github.com/orgs/tianocore/teams/tianocore-infosec-team?query=role%3Amaintainer).

![Create a temporary private fork](../../images/infosec_ghsa_create_temporary_fork.png)

Link the fork to the advisory. The Developer accepts the work, and the Coordinator
records the kickoff, reviewers and schedule described above.

Develop and test the patch privately. Prepare the applicable package security-fix
record, such as [NetworkPkg/SecurityFixes.yaml](https://github.com/tianocore/edk2/blob/master/NetworkPkg/SecurityFixes.yaml),
as part of the confidential change. Involve Reviewers early and escalate blockers
through the Coordinator.

### Fix review

The Developer opens a pull request against the private fork's primary branch,
including the patch, test results and known limitations.

![Private-fork review target](../../images/infosec_ghsa_targetable_branch.jpg)

Private-fork pipelines may be unavailable. Run applicable checks locally,
including PatchCheck and Uncrustify where relevant, and record results.
The Coordinator asks the Reporter to validate the fix and records their response
or that validation remains unavailable.

Review completes when both Reviewers approve, or a maintainer supplies the
documented additional/final approval when two Reviewers cannot be found.
Every required approver must be independent of the patch's author.

### Publication readiness

The Coordinator records readiness after confirming:

- Required independent reviews and test evidence are complete.
- Affected/fixed revisions, advisory text, references and credits are checked.
- A CVE is assigned, or an authorized Package Maintainer or InfoSec lead has
  approved a documented exception.
- The applicable package security-fix record is prepared.
- Disclosure timing is coordinated with the Reporter.
- An authorized publisher and maintainer are available for the public handoff.
- The publication and public-review schedule is recorded.

### Publication and public review

Within the agreed disclosure window, the Advisory Writer or an authorized
maintainer prepares `security-advisory/<cve-id-lowercase>/advisory`.
For an approved CVE exception, agree and record an alternative branch name.

The Developer targets that branch with the reviewed change. Preserve private
review evidence for authorized participants before closing superseded PRs.
The authorized participant merges the change into the public advisory branch,
and the Coordinator or authorized publisher publishes the GHSA with its
references and credits. Coordinate these actions within the one-day handoff.

![Publish the advisory](../../images/infosec_ghsa_publish_advisory.png)

Open the upstream primary-branch PR when the public patch is available. Link
the advisory and allow 24 hours for community review from PR and patch
availability. Resolve outstanding CI failures and review requirements.

### Upstream completion

The maintainer merges the reviewed change into the upstream primary branch
promptly after the public-review period and required checks are complete.
Record the PR, actual merge commit and date; a branch name or its existence
does not establish integration.

The Coordinator confirms publication and all required upstream fix and backport
merges with the maintainer and records final sign-off. Identify remaining
limitations and downstream follow-up explicitly. Upstream completion does not
mean all downstream products have received or deployed the fix.

### Closure without remediation

At any open stage, the Project Analyst may recommend closure. The Coordinator
obtains the disposition decision under the authority defined above, records
the rationale and supporting evidence, and informs the Reporter.
For duplicates, link the canonical advisory while preserving access controls.

An authorized participant closes the report on GitHub. Missing information or
an unassigned CVE is not, by itself, grounds for rejection. Closure does not mean
the issue was fixed or prevent the Reporter from publishing their own account.
