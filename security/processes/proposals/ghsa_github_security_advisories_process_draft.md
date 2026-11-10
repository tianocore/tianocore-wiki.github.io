# Process for GHSA (GitHub Security Advisory)

## Private Vulnerability Reporting – Reporter enters a probable security issue

* If security issue only GHSR (GitHub Security Report) - Security Policy to describe the procedure to report security
  issue (Sean B completed)

## Validate that it is a security issue - Infosec Team will determine if report is a security issue. This may require
the enlistment of subject matter experts - If not deemed security issue, ask reporter to submit Bugzilla

* If the report is determined to be a security issue
  * GHSA Created - Infosec Team may create the GHSA (if from Bugzilla) but typically this is created by the reporter
* Add infosec team - Infosec add the team members, Maintainers, reviewers and submitter (need Infosec team group -
completed)
  * CVSS Scoring - Infosec Team with assistance from submitter set the CVSS Score
  * Assign CWEs - Infosec Team assigns appropriate CWEs
  * Allocate CVE # - Infosec Team allocates CVE# to reference issue
  * Add private fork - Infosec Team creates private fork for patch work to be completed
* Proposed Patch created or exists
  * Developer pushes branch to private fork (DevName-FixDesc-version)
  * Developer submits Pull Request to private fork
  * Developer Leaves comment using the @ mentions to alert Maintainers & Reviewers
  * All discussion takes place within the GHSA.
  * Discussion should use @ mentions to tag people / teams for reviews or comments
  * If conversation is needed for documentation Comments need to be at the Advisory comments not file or line
  * Submitter must add Maintainers & Reviewers to Pull Request
    * Maintainers, Reviewers and Infosec Team - All parties evaluate Pull Request
  * Validate Fix complete - Infosec Team, SME(s)
  * Level of Testing required to consider complete - Infosec Team defines the level of testing necessary to validate.
  * Close unused Pull Requests
* Embargo period established - Infosec Team establishes the embargo time period
  * 60 Day under normal circumstances.
  * Exception process is possible based on external factors
* Embargo Period Ends
* GHSA PR (Pull Request) Created - GHSA Info is publicly visible at this point
  * Merged to Main branch within 1 day – under normal circumstances
* This means maintainer (and/or from infosec participant or community manager or steward) will sign-off via pull request
(and avoid patch email review)
* To ensure no clerical/formatting overhead recommend running local CI linting tools while in embargo prior to making
public
  * Publish GHSA
* CVE Details Updated - Infosec team updates CVE Detail information and submits to Mitre and make CVE public
