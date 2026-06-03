# Commit Signature Format

The [commit message](commit_message_format.md) should include signatures for those involved with creating and reviewing
the code.

This format is also documented in [Contributions.txt](https://github.com/tianocore/edk2/raw/master/Contributions.txt)
which may be available in the source tree as well.

## Name format

* Your Name &lt;<your.email@host.com>&gt;
  * This is your real name and email address
  * Quote (") your name if it contains a comma
    * "Last, First" &lt;<your.email@host.com>&gt;

## Where to add signatures

* All signatures should be at the end of the commit log message
* Following the message, there should be one blank line and then the signatures
* There should only be one signature per line
* Each new signature should be added following to the end of the signature list

## Signed-off-by

* Authors should use Signed-off-by (See example below)
* If you've received the code from a trusted source, and are forwarding it along, please add a Signed-off-by line for
  yourself to indicate that you know this code to be usable by our community.

## Reviewed-by

* For code reviewers use Reviewed-by (See example below)
* Reviewers should publicly indicate they have reviewed the code by replying to the code review email with:
  1. Review comments (optional if you have no suggested changes.)
2. Your Reviewed-by signature line, exactly as it should appear in the commit. (The committer will add this before
committing the change to source control.)
  * You should omit your Reviewed-by signature if you want to re-review the change after your suggested changes are
    made.

## Format Strictness

Please strictly adhere to this format for signatures to enable this
data to be stripped by automated tools when required.

## Example

This is a sample [commit message](commit_message_format.md), including signatures:

Package/Module: Short one line description of change

Several lines of
description for the
change.

Signed-off-by: Contributor Name <contributor@email.server>
Reviewed-by: Reviewer Name <reviewer@reviewer-email.server>

## See Also

* [Code-Style](../coding-standards/code_style.md)
  [SubmittingPatches](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/Documentation/SubmittingPatches?id=f6f94e2ab1b33f0082ac22d71f66385a60d8157f#n297)
