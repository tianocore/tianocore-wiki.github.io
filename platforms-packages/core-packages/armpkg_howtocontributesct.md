# ArmPkg HowToContributeSct

## How To contribute to SCT

Pre-requirements:

\- Having an account on Github

\- Being a member of the UEFI Forum. The membership list is available
here: [https://uefi.org/members](https://uefi.org/members)

\- Requesting access to UEFI-SCT Github project:
[https://github.com/UEFI/UEFI-SCT](https://github.com/UEFI/UEFI-SCT)

1.Fork the Github repository
![](contribute-sct-1.png)

After forking the repository, it should appear under your own Github
account:
![](contribute-sct-2.png)

2\. Clone the repository on your host machine:

    git clone https://github.com/oliviermartin/UEFI-SCT.git

3\. Create your branch

    git checkout -b om/installsct-fix-backup origin/master

After making your first change, you can see the state of your local GIT
repository:

    $ git status
    On branch om/installsct-fix-backup
    Your branch is up-to-date with 'github-olivier/master'.

    Changes not staged for commit:
      (use "git add ..." to update what will be committed)
      (use "git checkout -- ..." to discard changes in working directory)

        modified:   Application/InstallSct/InstallSct.c

    no changes added to commit (use "git add" and/or "git commit -a")

    $ git diff
    diff --git a/Application/InstallSct/InstallSct.c b/Application/InstallSct/InstallSct.c
    index cb9f209..d28e720 100644
    --- a/Application/InstallSct/InstallSct.c
    +++ b/Application/InstallSct/InstallSct.c
    @@ -432,35 +432,6 @@ BackupTests (
         }

         //
    -    // Exist. Need to remove or backup
    -    //
    -    if (StriCmp (InputBuffer, L"a") == 0) {
    -      //
    -      // Backup all
    -      //
    -      Status = BackupDirFile (TmpName);
    -      if (EFI_ERROR (Status)) {
    -        FreePool (TmpName);
    -        return Status;
    -      }
    -
    -      FreePool (TmpName);
    -      continue;
    -    } else if (StriCmp (InputBuffer, L"l") == 0) {
    -      //
    -      // Remove all
    -      //
    -      Status = RemoveDirFile (TmpName);
    -      if (EFI_ERROR (Status)) {
    -        FreePool (TmpName);
    -        return Status;
    -      }
    -
    -      FreePool (TmpName);
    -      continue;
    -    }
    -
    -    //
         // User must input a selection
         //
         while (TRUE) {

Once happy by your changes, you can add them to your future GIT commit:

    git add Application/InstallSct/InstallSct.c

And commit it!

    git commit

After having completed all your changes, you need to push them to the
your GIT repository:

    $ git push github-olivier om/installsct-fix-backup
    Username for 'https://github.com': oliviermartin
    Password for 'https://oliviermartin@github.com':
    Counting objects: 39, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (19/19), done.
    Writing objects: 100% (19/19), 2.92 KiB | 0 bytes/s, done.
    Total 19 (delta 13), reused 0 (delta 0)
    To https://github.com/oliviermartin/UEFI-SCT.git
     * [new branch]      om/installsct-fix-backup -> om/installsct-fix-backup

The new branch should now be visible into your fork of UEFI-SCT
project:
![](contribute-sct-3.png)

To propose your changes to the UEFI-SCT project to be merged, send a
pull-request by clicking on 'Compare & pull request'.
![](contribute-sct-4.png)

After sending the pull request, the request should appear into the
'UEFI-SCT' project 'pull request' list.
![](contribute-sct-5.png)

Details of the 'pull-request' can be shown by clicking on the entry:
![](contribute-sct-6.png)

User can review changes and leave comments:
![](contribute-sct-7.png)
