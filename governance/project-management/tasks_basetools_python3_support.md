# Tasks Basetools Python3 Support

This task updating the existing Python-base BaseTools to run under
both Python 2.7 and Python 3.0.

## Current Status

Not started.

## Directory list

* [ ] AutoGen
* [ ] Common
* [ ] CommonDataClass
* [ ] Workspace
* [ ] Table
* [ ] build
* [ ] BPDG
* [ ] Eot
* [ ] GenFds
* [ ] GenPatchPcdTable
* [ ] PatchPcdValue
* [ ] Trim
* [ ] UPT

## Task Overview

* Difficulty: Complex
* Language: Python
* Suggested by:
* Required completion date: End of year, 2020

Issues:

* Individual tools use shared modules that will need to be ported prior to

## porting individual tools

* Current sources contain depricated content
* Files need to conform to the Python Coding Style Guideline and pass pylint

## (static analysis tool)

* Several standard Python module functions are overridden for handling long

## file paths

* Several filenames are duplicated in different directory trees
* Some imports are ambiguous

Location of Source Code:

`[git@github.com:tianocore/edk2.git BaseTools/Source/Python]`

Requirements:

* Update content to conform to Coding Style (see below)&gt;
* Modules must pass pylint - a custom pylint RC file is available.
* Python files must start with the standard header
* Update code, replacing deprecated modules with newer modules.
* First line python code statement in each file must be:

  ```from``__future__``import``absolute_import`

Tools
-----

* All tools are 32-bit versions of the tools, in order to create Win32

`executables.`

Python 2.7.x

* must be able to run with 2.7.3 or later Python 3.x.x
* must be able to run with 3.4.x or later pip
* recommended version is 8.1.1
* used to install other modules logilab-common
* 1.2.0 or later logilab-astng
* 0.24.3 or later pylint
* 1.5.5 or later astroid
* 1.4.5 or later uuid
* 1.20 pywin32
* 217 or later antlr-python-runtime
* 3.0.1 ONLY cx-Freeze
* 4.3.4 or later

Coding Style
------------

[Python PEP-8 Coding Style Guidelines](https://www.python.org/dev/peps/pep-0008/ "wikilink")

[Python PEP-257 Docstring Conventions](https://www.python.org/dev/peps/pep-0257/ "wikilink")

### EDK II specific changes to PEP-8 and Pylint

* Maximum line length for code: 100 characters
* Maximum line length for comments and docstrings: 72 characters
* Regular Expressions allow camel case: \[a-zA-Z\_\]\[a-zA-Z0-9\_\]{2,30}$
* Const Regular Expressions: ((\[gA-Z\_\]\[a-zA-Z0-9\_\]\*)|(\_\_.\*\_\_))$
* min-similarity-lines=10
* ignored-classes=SQLObject
* generated-members=REQUEST,acl\_users,aq\_parent
* dummy-variables-rgx=\_|dummy
* max-args=12
* max-locals=25
* max-returns=10
* max-branches=25
* max-statements=100
* max-attributes=15
* min-public-methods=0
* deprecated-modules=regsub,string,TERMIOS,Bastion,rexec,optparse
* Python 2.7.x requires all files to be endcoded on disk as ASCII, not UTF-8.

BaseTools Source Tree
=====================

All modules must be able to execute from source with both Python 2.7.x and Python 3.x.x.

The following two files should be removed:

`Common\MigrationUtilities.py`
`Common\PyUtility.pyd`
