# EDK II git svn

If you have commit access, then you should consider using git-svn to
interface with the EDK II svn repository.

If you do not have commit access, then you should instead just clone one
of the git mirrors listed on the
[Source
Control](../contribution-guides/source_control.md) page.

## git-svn Overview

By using git-svn, you can utilize a large portion of the git feature
set.

With git-svn, you will be able to download all svn changes into a local
git repository, and commit new changes.

There are probably much better git-svn tutorials on the web. (The [git
website](https://git-scm.org) is probably a good place to start.) But,
here is some brief information...

### Create a new EDK II tree with git-svn

This will download **all** changes from svn (this will take a very long
time!):

    bash$ git svn clone -s https://svn.code.sf.net/p/edk2/code edk2

You can also download a snapshot git-svn tree:
[https://sourceforge.net/projects/edk2/files/Git/](https://sourceforge.net/projects/edk2/files/Git/)

The snapshots are named edk2-git-svn-\*.zip. They contain the git
repository along with the svn data.

1. Unzip the file into an empty directory
2. Run 'git reset --hard' in that directory

### Commit changes to your local tree

When using git, your changes are first committed to your local tree. The
simplest way to commit all current changes in your tree is:

    bash$ git commit -a

But, if you just want to commit changes to a single file, then use:

    bash$ git add path/to/file
    bash$ git commit

When working with your local tree, you can probably find much better
tutorials on the web. (The [git website](https://git-scm.org) is probably
a good place to start.)

### Publishing changes to svn

As mentioned above your git commits are stored locally in the git
repository within your source tree.

To publish (commit) your changes to svn, you'll use the git svn dcommit
command.

dcommit will check in all the local commits you've made into svn 1-by-1.
Here is a recommended sequence of steps for committing your changes to
svn:

This will download all recent changes from svn and rebase your local
changes to occur after the new remote changes:

    bash$ git svn rebase

Highly recommended for new git-svn users! Run dcommit with --dry-run to
make sure the number of changes you expect will be be committed:

    bash$ git svn dcommit --dry-run

Commit all of your changes to svn:

    bash$ git svn dcommit

## See also

- [EDK2 git](edk2_git.md)
