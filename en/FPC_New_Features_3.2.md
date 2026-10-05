# FPC New Features 3.2.2

## Contents

  * 1 About this page
  * 2 New compiler targets
    * 2.1 Support for macOS/AArch64
    * 2.2 Language
      * 2.2.1 Record methods assigned to method variables
  * 3 Units
    * 3.1 SQLdb
      * 3.1.1 MySQL 8.0 support
    * 3.2 Classes
      * 3.2.1 Naming of Threads
  * 4 New Features from other versions



## About this page

FPC 3.2.2 has been released on May 20th, 2021. 

Below you can find a list of new features introduced since the [previous release](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0"), along with some background information and examples. 

A list of changes that may break existing code can be found at [User Changes 3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2"). 

## New compiler targets

### Support for macOS/AArch64

  * **Overview** : The compiler can now target macOS running on AArch64
  * **Notes** : The Darwin/AArch64 target corresponds to macOS/AArch64. Generating code for iOS/AArch64 requires a [different command line parameter](<User_Changes_3.2.md> "User Changes 3.2.2") compared to previous versions.
  * **More information** : [Build instructions](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers")
  * **svn** : 45762



### Language

#### Record methods assigned to method variables

  * **Overview** : ?
  * **Notes** : 
    * Delphi-compatibility
  * **Example** : <https://gitlab.com/freepascal.org/fpc/source/-/blob/main/tests/tbs/tb0681.pp>
  * **svn** : 47794



## Units

#### SQLdb

##### MySQL 8.0 support

  * **Overview** : Support for MySQL 8.0 has been implemented.
  * **svn:** 48692



#### Classes

##### Naming of Threads

  * **Overview** : _TThread.NameThreadForDebugging_ has been implemented.
  * **Notes** : Delphi compatible, currently implemented for Windows, Linux and Android. Read documentation as every platform has its own restrictions.
  * **svn:** 45160, 45206, 45233



## New Features from other versions

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | 3.2.2 | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/FPC_New_Features_3.2.2](https://web.archive.org/web/20241226175221/https://wiki.freepascal.org/FPC_New_Features_3.2.2)_
