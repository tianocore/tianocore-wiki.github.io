# EDK2 git

Note: If you are simply looking for an
[EDK II git repository](../contribution-guides/source_control.md), then visit the
[Source
Control](../contribution-guides/source_control.md) page.

The [EDK II](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II/) project uses the subversion
source control system. But, as [git](https://git-scm.org) is a source
control system often used by open-source projects these days, the
question of whether git can be used with EDK II occasionally comes up.
This page will attempt to document how contributors can use git with EDK
II, as well as other EDK II git information.

## Contributors without svn commit access

EDK II uses [package maintainers](../../platforms-packages/core-packages/edkii_packages.md) to own reviewing and
committing contributions for a package. This means that most
contributors don't really need to interface with svn if they prefer to
use git.

In this scenario, the contributor can clone from one of the EDK II
[git
mirrors](../contribution-guides/source_control.md). They can then:

1. Make their changes in git as usual
2. Use git format-patch / send-email as normal to publish the changes
    to
    [edk2-devel](../../community/communications/mailing_lists.md)

Of course, when the package maintainer commits your change, your
authorship of the commit will be lost, as svn does not maintain separate
author/committer metadata.

## Helping convince EDK II to convert to git

If you would prefer for EDK II to use git, then one of the most helpful
things you can do is mention it on
[edk2-devel](../../community/communications/mailing_lists.md).
Silence will generally favor and the Status quo / staying on svn.

Other than this, your best bet is to use the
[git
mirrors](../contribution-guides/source_control.md) of svn to make it clear that this is how you prefer to work.

Use of git send-email, pushing your tree to a publicly accessible
location and mentioning that your changes are available in a branch,
will help raise awareness of the usefulness of git as well.

## Discussions of converting to git

Git is mentioned occasionally on
[edk2-devel](../../community/communications/mailing_lists.md).

In February 2013 there was such a
[discussion](https://sourceforge.net/p/edk2/mailman/message/30431526/),
and in general people expressed a desire to use git for EDK II. Here is
some of the information from that email thread:

- David & Jordan from Intel would prefer git
- Andrew from Apple uses git-svn, and wouldn't mind if the project move
  to git
- Cameron from Apple does **not** want to use git
- ARM uses git
- Linaro uses git
  - Maintains their own git-svn
    [repository](https://git.linaro.org/gitweb?p=mirror/edk2/edk2.git;a=summary)
- Brian from SGI mentioned they would prefer git
- Laszlo from Red Hat uses git
- Several EDK II Google Summer of Code projects used git

## See Also

- [EDK II
  git-svn](edk_ii_git_svn.md) (For
  [package
  owners](../../platforms-packages/core-packages/edkii_packages.md) / svn committers)
