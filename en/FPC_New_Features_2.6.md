# FPC New Features 2.6.2

│ **English (en)** │    
****

## Contents

  * 1 About this page
  * 2 Darwin/Mac OS X
    * 2.1 Support for specifying and querying the deployment version
  * 3 New Features from other versions



## About this page

Below you can find a list of new features introduced since the [previous release](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0"), along with some background information and examples. 

## Darwin/Mac OS X

### Support for specifying and querying the deployment version

  * **Overview** : Support has been added to specify the minimum deployment target version of Mac OS X/iOS via a command line parameter, and for determining the specified deployment target version via a macro
  * **More information**
    * Command line parameters: _-WM10.4_ or _-WM10.5.3_ and similar for Mac OS X targets, _-WP4.1_ or _-WP5.0.1_ and similar for the iPhoneOS/iPhoneSimulator targets.
    * If no command line parameter with a deployment version is specified, the compiler will look at the _MACOSX_DEPLOYMENT_TARGET_ resp. _IPHONEOS_DEPLOYMENT_TARGET_ environment variables to get the deployment target version
    * if the deployment target version has not been specified using any of the previous methods, a default will be used (Mac OS X/PPC: 10.3; Mac OS X/PPC64/i386: 10.4; Mac OS X/x86-64: 10.5; iOS/iPhoneSimulator: 3.0)
    * Apart from passing the deployment target version number to the linker and linking in the appropriate startup code, the compiler will also define a symbol called _MAC_OS_X_VERSION_MIN_REQUIRED_ resp. _IPHONE_OS_VERSION_MIN_REQUIRED_ containing a value equivalent to the one defined by Apple's _Availability.h_ /_AvailabilityMacros.h_ , i.e. 1043 for Mac OS X 10.4.3, and 40102 for iPhoneOS 4.1.2. Note that this is not a constant that can be used in Pascal expressions, it's only for use by preprocessor statements.
  * **Example** :


    
    
    procedure CGImageSourceUpdateDataProvider( isrc: CGImageSourceRef; provider: CGDataProviderRef; final: CBool ); cdecl; 
      {$if MAC_OS_X_VERSION_MIN_REQUIRED >= 1040}external;{$else}weakexternal;{$endif}
    

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

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | 2.6.2 | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/FPC_New_Features_2.6.2](https://web.archive.org/web/20240913181059/https://wiki.freepascal.org/FPC_New_Features_2.6.2)_
