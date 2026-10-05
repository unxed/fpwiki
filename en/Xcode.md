# Xcode

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/Xcode.md> "Xcode/ru")** │    
****

## Contents

  * 1 Overview
  * 2 Installing Command Line Tools: Mavericks 10.9 and higher
    * 2.1 Install Xcode (includes Command Line Tools)
    * 2.2 Install the standalone Command Line Tools package only
  * 3 Installing Command Line Tools: Lion 10.7 or Mountain Lion 10.8
  * 4 Legacy Information
    * 4.1 Xcode 7.0
    * 4.2 Xcode 5.0
  * 5 External links



## Overview

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** You do NOT need to install the Xcode IDE in order to get all the build utilities on which the Free Pascal Compiler depends, but you DO need to install the Command Line Tools. How to do so is explained below. You WILL need to install Xcode if: (1) you want access to the SDKs for **iOS** , **iPadOS** , watchOS and tvOS; or (2) to validate and upload apps to the Mac App Store; or (3) to [notarize](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+") apps for distribution outside of the Mac App Store.

Xcode is an integrated development environment for macOS containing a suite of software development tools developed by Apple for developing software for macOS, iOS, iPadOS, watchOS, and tvOS. 

The standalone Command Line Tools Package is a small self-contained package available for download separately from Xcode and that allows you to do command line development in macOS. It consists of the macOS SDK and command-line tools which are installed in the /Library/Developer/CommandLineTools directory. 

## Installing Command Line Tools: Mavericks 10.9 and higher

You can use either of the following methods to install the command line tools on your system: 

### Install Xcode (includes Command Line Tools)

If you install Xcode, then there is no need to install the command line tools. Xcode comes bundled with all the command line tools. Mavericks 10.9 and later includes shims or wrapper executables. These shims, installed in `/usr/bin/`, can map any tool included in `/usr/bin/` to the corresponding one inside Xcode. xcrun is one of such shims, which allows you to find or run any tool inside Xcode from the command line. Use it to invoke any tool within Xcode from the command line. For example: 
    
    
    $ xcrun dwarfdump --uuid  MyApp.app/Contents/MacOS/MyApp 
    UUID: 4CED1202-EB68-3991-A036-7146FB4D4E17 (x86_64) MyApp.app/Contents/MacOS/MyApp
    

However, you will need to edit your `/etc/fpc.cfg` file to add this line: 
    
    
    -XR/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk
    

in the "Linking" section which is located towards then end of the file to enable FPC to find the macOS SDK. 

**Note that** this is not the officially supported method nor is it the recommended method for accessing the command line tools and that when you subsequently install FPC, its installer will warn you that the standalone command line tools have not been installed. 

### Install the standalone Command Line Tools package only

You can install the standalone Command Line Tools package by opening a Terminal and running the following commands: 
    
    
    sudo xcode-select --install
    sudo xcodebuild -license accept
    

macOS comes bundled with xcode-select, a command-line tool that is installed in `/usr/bin/`. It allows you to manage the active developer directory for Xcode and other BSD development tools. See its man page for more information. 

In Mavericks 10.9 and later, software update notifies you when new versions of the command-line tools are available for update. 

**Note that** installing the standalone command line tools, and not also installing Xcode, will only provide you with the SDK for macOS (you will not have the SDKs for **iOS** , **iPadOS** , watchOS or tvOS). 

## Installing Command Line Tools: Lion 10.7 or Mountain Lion 10.8

You will need to sign up for a free Apple developer account to login and access the Command Line Tools download. Go to <https://developer.apple.com/downloads/> , log in, search for Command Line Tools and download the appropriate file. Mount the DMG file you downloaded and run the package installer. 

## Legacy Information

### Xcode 7.0

  * If after updating to Xcode 7 your projects fail to compile and you are using FPC 2.6.4 (or earlier), [change your debug info](<Debugger_Setup.md> "Debugger Setup") type from "Automatic" to "Dwarf". Apple's assembler no longer supports .Stabs debug information (which is used by default on i386). 
    * If your project is using packages you might need to switch these packages debug info to Dwarf as well. There're two ways to do that.



    

    1\. Change each package debug info. Open Project -> Project Inspector. For each package listed under "Required Packages", double-click on a package (to open package dialog), select "Options" and change debug info type under "Debugging" section 

    or
    2\. You can add the setting as a custom option for all packages used in the project. As show in the example on [this page](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options"). But instead of using "-dSomeValue" put "-gw"

  * Alternatively you could download the previous version 6.4 
    * Download Xcode 6.4, and change the folder name to **Xcode6.4** before copy to /Application
    * So you have two Xcode applications now, so switch your command line tools to using the ones from **Xcode6.4** :


    
    
    # sudo xcode-select -switch /Applications/Xcode6.4.app
    

  * more to come for ARM (iOS) target.



### Xcode 5.0

Apple removed "gdb" from it's binary utilities. At the time, Lazarus was only able to use "gdb" as an external debugger. "Gdb" should be installed from a third party. See [GDB on macOS](<GDB_on_OS_X_Mavericks_or_newer_and_Xcode_5_or_newer.md> "GDB on OS X Mavericks or newer and Xcode 5 or newer"). 

## External links

  * [Apple: Technical Note TN2339](<https://developer.apple.com/library/archive/technotes/tn2339/_index.html>) Building from the Command Line with Xcode FAQ.

---

_Source: [https://wiki.freepascal.org/Xcode](https://web.archive.org/web/20240214173310/https://wiki.freepascal.org/Xcode)_
