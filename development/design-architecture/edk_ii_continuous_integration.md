# EDK II Continuous Integration

Summary of pre-commit and post-commit Continuous Integration services that
improve the quality of commits made to EDK II repositories.  The sections below
list the Continuous Integration services that are active and plans for future
enhancements and extensions to these services.

## Phase 1 (edk2 repository only) - Activated November 11, 2019

1) Use a combination of GitHub, Azure Pipelines, Mergify, and edk2-pytool features.
   * GitHub Pull Requests + Labels, Branch Protections,  Notifications
   * Mergify Pull Request Rules with auto commit if all checks pass
   * 3 pre-commit jobs in Azure Pipelines (PatchCheck, Windows/VS, Linux/GCC).
     Goal is to complete all pre-commits check in under 10 minutes.
   * 2 post-commit jobs in Azure Pipelines (Windows/VS, Linux/GCC).  Post commit
     status provided at top of `edk2/master` [Readme.md](https://github.com/tianocore/edk2/blob/master/Readme.md).
   * [EDK II Pytool Library](https://github.com/tianocore/edk2-pytool-library)
   * [EDK II Pytool Extensions](https://github.com/tianocore/edk2-pytool-extensions)
   * [GitHub Issue](https://github.com/tianocore/edk2/issues/8241)
   * Original RFC Proposals
     * [https://edk2.groups.io/g/rfc/message/93](https://edk2.groups.io/g/rfc/message/93)
     * [https://edk2.groups.io/g/devel/message/46607](https://edk2.groups.io/g/devel/message/46607)
2) Enable the following pre-commit checks
     [PatchCheck](https://github.com/tianocore/edk2/blob/e92b155740cdbf10a85ed8f37f69da0991fc8275/BaseTools/Scripts/PatchCheck.py)
     [CharEncodingCheck](https://github.com/tianocore/edk2/tree/e92b155740cdbf10a85ed8f37f69da0991fc8275/.pytool/Plugin/CharEncodingCheck)
     [CompilerPlugin](https://github.com/tianocore/edk2/tree/e92b155740cdbf10a85ed8f37f69da0991fc8275/.pytool/Plugin/CompilerPlugin)
     [DependencyCheck](https://github.com/tianocore/edk2/tree/e92b155740cdbf10a85ed8f37f69da0991fc8275/.pytool/Plugin/DependencyCheck)
     [DscCompleteCheck](https://github.com/tianocore/edk2/tree/e92b155740cdbf10a85ed8f37f69da0991fc8275/.pytool/Plugin/DscCompleteCheck)
     [GuidCheck](https://github.com/tianocore/edk2/tree/e92b155740cdbf10a85ed8f37f69da0991fc8275/.pytool/Plugin/GuidCheck)
     [LibraryClassCheck](https://github.com/tianocore/edk2/tree/e92b155740cdbf10a85ed8f37f69da0991fc8275/.pytool/Plugin/LibraryClassCheck)
     [SpellCheck](https://github.com/tianocore/edk2/tree/e92b155740cdbf10a85ed8f37f69da0991fc8275/.pytool/Plugin/SpellCheck)
3) TianoCore EDK II Maintainers Team permissions reduced from 'Write" to "Triage"
4) EDK II Maintainers must use GitHub pull request with 'push' label to request
   a branch to be strict rebase merged into `edk2/master`.  If all checks pass,
   then the patches in the pull request are automatically added to `edk2/master`.
   If any check fails, then email notifications are sent and details of the
   failure are available through Azure Pipelines test results.
5) Personal builds available to all EDK II developers using a GitHub pull
   request without the 'push' label set.  If all checks pass, then a notification
   email is sent and the pull request is closed.  If any checks fails, then
   email notifications are sent and the details of the failure are available
   through Azure Pipelines test results.
6) GitHub References
   * [GitHub](https://github.com/)
   * [GitHub Labels](https://docs.github.com/en/github/managing-your-work-on-github/about-labels)
   * [GitHub Protected Branches](https://docs.github.com/en/github/administering-a-repository/about-protected-branches)
   * [GitHub
     Notifications](https://docs.github.com/en/github/receiving-notifications-about-activity-on-github/about-notifications)
   * [Watch a GitHub
     repository](https://docs.github.com/en/github/receiving-notifications-about-activity-on-github/watching-and-unwatching-repositories)
   * [Create a GutHub fork](https://docs.github.com/en/github/getting-started-with-github/fork-a-repo)
   * [Create a GitHub pull
     request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request)
7) GitHub Command line Utility (`gh`) to perform GitHub operations
   * [GitHub CLI Download](https://cli.github.com/)
   * [GitHub CLI Manual](https://cli.github.com/manual/gh)
8) Azure Pipelines References
   * [Azure Piplelines GitHub App](https://github.com/marketplace/azure-pipelines)
   * [Azure Pipelines TianoCore edk2-ci Project](https://dev.azure.com/tianocore/edk2-ci)
   * [Azure Pipelines TianoCore edk2-ci Pipelines](https://dev.azure.com/tianocore/edk2-ci/_build)
   * [Azure Pipelines](https://azure.microsoft.com/en-us/services/devops/pipelines)
   * [Azure Pipelines YAML Schema](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema)
9) Mergify References
   * [Mergify GitHub App](https://github.com/apps/mergify)
   * [Mergify Documentation](https://doc.mergify.com)

[EDK II Continuous Integration Administration](../../governance/project-management/edk_ii_continuous_integration_administration.md)

## Proposed Pre-Commit Checks in Phase 2

* Verify no non-ASCII characters in modified files
* Verify no binary files in set of modified files

## Proposed Pre-Commit Checks in Phase 3

* Run ECC on modified files
* Verify modified modules/libs build
* Run available Host Based tests against modified modules/libs

## Post Commit Checks

* Build all modules/libs/platforms that consume modified content

## Daily Builds

* Build critical packages
* Build critical platforms
* Verify that Doxygen Documentation can be generated

## Weekly Builds

* Build all packages
* Build all platforms
* Publish Doxygen Documentation to a web site

## Release Builds

* Same as weekly builds
* Full regression testing
* Publishes binary files to release pages

## Possible Continuous Integration Actions

* PatchCheck.py
* Verify no non-ASCII characters in modified source files
* Verify no binary files in set of modified files
* Verify Package Dependency rules in modified files
* Verify modified modules/libs build
* Run Host Based tests against modified modules/libs
* cppcheck
* ECC (EFI Code Checker)
  * Difficult to address all warnings/issues/false positives reported
  * May need to maintain an exception list
* Static Analysis against modified modules/libs
  * CLANG static analysis
  * Coverity: [https://scan.coverity.com/](https://scan.coverity.com/)
  * Difficult to address all warnings/issues/false positives reported
  * May need to maintain an exception list
* pyflakes (for python sources)
* pylama (for python sources)
* Generate Package Documentation (Doxygen based)
* Build all Packages/Platforms for all supported CPU architectures
  (IA32, X64, ARM, AARCH64, EBC), all supported tool chains (VS2015, VS2017,
  GCC5, XCODE5), and all supported build targets (DEBUG, RELEASE, NOOPT).
  Needs further discussion on required coverage.
* Boot platforms to UEFI Shell
* Run UEFI SCTs and collect results for platforms
* Linaro LAVA CI
  * [https://validation.linaro.org/static/docs/v2](https://validation.linaro.org/static/docs/v2)
    [https://validation.linaro.org/static/docs/v2/lava_ci.html#continuous-integration](https://validation.linaro.org/static/docs/v2/lava_ci.html#continuous-integration)
  * [https://lkft.linaro.org](https://lkft.linaro.org)
* Boot platforms to OS(s)
* Integration/Regression tests that require full OS boots
* Build binary releases of components (e.g. UEFI Shell, OVMF)

## Possible Platform Testing Actions

* OVMF
  * IA32 DXE boot tests
  * X64 DXE boot tests
  * ArmVirt QEMU boot tests
  * S3 enabled tests
  * SMM_REQUIRE enabled tests
  * Boot using both KVM and QEMU environments
  * HTTPS Boot Tests
  * UEFI Secure Boot Sanity Testing
    * Enroll UEFI Secure Boot Keys
    * UEFI Secure Boot testing of "unsigned" images
    * UEFI Secure Boot testing of "signed but not recognized" images
    * UEFI Secure Boot testing of "signed and accepted" images
    * UEFI Secure Boot testing of "signed but blacklisted" images
  * Project that offers a python script that automates communicating with the
    UEFI shell over the emulated serial port. Used in downstream package builds,
    for packaging a pre-enrolled variable store template file.
    * [https://github.com/puiterwijk/qemu-ovmf-secureboot](https://github.com/puiterwijk/qemu-ovmf-secureboot)

## Evaluations and Supporting Background Materials

* Background
  * [https://en.wikipedia.org/wiki/Continuous_integration](https://en.wikipedia.org/wiki/Continuous_integration)
* GitHub Continuous Integration services
    [https://github.com/marketplace/category/continuous-integration](https://github.com/marketplace/category/continuous-integration)
* Jenkins Evaluation
* GitLab Evaluation
  * Contacts
    * Laszlo Ersek <lersek@redhat.com>
    * Philippe Mathieu-Daudé <philmd@redhat.com>
  * [https://gitlab.com/philmd/edk2/pipelines](https://gitlab.com/philmd/edk2/pipelines)
* Azure Pipeline Evaluation for CPU Archs, tool chain tags, and build targets
  * Contacts
    * Michael D Kinney <michael.d.kinney@intel.com>
  * [https://github.com/mdkinney/edk2-ci](https://github.com/mdkinney/edk2-ci)
    [https://dev.azure.com/mikekinney/edk2-ci/_build?definitionId=1](https://dev.azure.com/mikekinney/edk2-ci/_build?definitionId=1)
* Azure Pipelines Evaluation with HBFA integration
  * Contacts
    * Sean Brogan <sean.brogan@microsoft.com>
    [https://github.com/spbrogan/edk2-staging/tree/edk2-stuart-ci-latest](https://github.com/spbrogan/edk2-staging/tree/edk2-stuart-ci-latest)
  * To work with this branch and run tests immediately, all you need to do is:

```
pip install --upgrade -r requirements.txt
stuart_setup -c .\CISettings.py
stuart_update -c .\CISettings.py
stuart_ci_build -c .\CISettings.py --Tool_Chain VS2017
```

* Branch is monitored for CI and PR gating in the following Azure build pipeline.
    [https://dev.azure.com/tianocore/edk2-ci-play/_build?definitionId=12](https://dev.azure.com/tianocore/edk2-ci-play/_build?definitionId=12)
* Results show that this CI process is running build CI and DSC checking and
    automatically running a host-based unit test and all of the results are
    visible in a single view.
    [https://dev.azure.com/tianocore/edk2-ci-play/_build/results?buildId=216&view=ms.vss-test-web.build-test-results-tab](https://dev.azure.com/tianocore/edk2-ci-play/_build/results?buildId=216&view=ms.vss-test-web.build-test-results-tab)
    [https://dev.azure.com/tianocore/edk2-ci-play/_build/results?buildId=215&view=ms.vss-test-web.build-test-results-tab](https://dev.azure.com/tianocore/edk2-ci-play/_build/results?buildId=215&view=ms.vss-test-web.build-test-results-tab)
* Depends on pip installable tool from the following TianoCore repos
  * [https://github.com/tianocore/edk2-pytool-extensions](https://github.com/tianocore/edk2-pytool-extensions)
  * [https://github.com/tianocore/edk2-pytool-library](https://github.com/tianocore/edk2-pytool-library)
* Documentation for edk2-pytools
    [https://github.com/tianocore/edk2-pytool-library/tree/master/docs](https://github.com/tianocore/edk2-pytool-library/tree/master/docs)
    [https://github.com/tianocore/edk2-pytool-extensions/tree/master/docs](https://github.com/tianocore/edk2-pytool-extensions/tree/master/docs)
