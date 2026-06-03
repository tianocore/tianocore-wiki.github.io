# 1. What is ECC tool?

ECC is a python tool which helps to detect coding style issues.
It reports errors for the codes which don't follow [EDK II C Coding Standards
Specification](https://tianocore-docs.github.io/edk2-CCodingStandardsSpecification/draft/).

## 2. Where is the ECC tool?

ECC tool is located in edk2/BaseTools/Source/Python/Ecc.

## 3. How to run ECC tool?

Steps to run the ECC tool:

* 1). Enter edk2 directory, run: **edksetup.bat**** (**on Windows**)
      **Enter edk2 directory, run: **source edksetup.sh**** (**on Linux**)

* 2). Then in the edk2 directory, you can type "Ecc" to run the ECC tool directly**.

* 3). If you meet the following errors:**
  * Error 1:

    ```text
    import antlr3
    ImportError: No module named antlr3
    ```

    This error may be met when you run the ECC tool with Python 2.x, then ECC depends on antlr V3.0.1, you can download
    it from [https://www.antlr3.org/download/Python/](https://www.antlr3.org/download/Python/).

    After downloading and extracting it, you can enter the antlr tool directory and run:

    `C:\Python27\python.exe setup.py install` (_on Windows_)

    `python setup.py install` (_on Linux_)

  * Error 2:

    ```text
    import antlr4 as antlr
    ModuleNotFoundError: No module named 'antlr4'
    ```

    This error may be met when you run the ECC tool with Python 3.x, then ECC depends on antlr4, you can install it through
    the following command.

    `py -3 -m pip install antlr4-python3-runtime==4.7.1` (_on Windows_)

    `sudo python3 -m pip install antlr4-python3-runtime==4.7.1` (_on Linux_)

* 4). You can type "Ecc -h/Ecc --help" to get the help info of the ECC tool.

* 5). Common usage model:

  `Ecc -c <config_file> -e <exception file> -t <to-be-scanned directory> -r <result CSV file>`

  > Notes: Please use the full path when specifying the target directory to scan.
  >
  > `config.ini` and `exception.xml` are in the `edk2/BaseTools/Source/Python/Ecc` directory.
  >
> `config.ini` is the configuration file of the ECC tool. If the config file is not specified when running ECC, it will
use the one in the `Edk2/BaseTools/Source/Python/Ecc` directory by default.
  > `exception.xml` is used to skip some specific coding style issues.

  For example, to run ECC to check the coding style in MdePkg:

`Ecc –c D:/AWORK/edk2/BaseTools/Source/Python/Ecc/config.ini -e D:/AWORK/edk2/BaseTools/Source/Python/Ecc/exception.xml
-t D:/AWORK/edk2/MdePkg -r MdePkgECC.csv`

```admonish note
When running ECC for a specific sub-dir, it may report some errors such as some library instances are not used,
but when running ECC for the whole project, these errors are gone. We can ignore such kind of errors when running ECC
for a specific sub-dir.
```

* 6). You may need to maintain the config.ini and exception.xml files by yourself for your project.

* a) If you want to skip to check some sub-dir or file, you can add them to the SkipDirList, SkipFileList part in the
config.ini.
  A list for skip dirs when scanning source code: `SkipDirList = BUILD, ..., TEST\TEST`
  A list for skip files when scanning source code: `SkipFileList = .gitignore,...`

  * b) If you want to skip a specific ECC error, you can add them to the exception.xml file.
  The mapping relationship between exception format and ECC error is like below.
  ![ECC Exception](../../images/eccexception.png)
