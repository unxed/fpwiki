# LAMW

│ **English (en)** │  **[polski (pl)](</LAMW/pl> "LAMW/pl")** │  **[русский (ru)](<../ru/LAMW.md> "LAMW/ru")** │ 

[![Android robot.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

This article applies to [Android](</Category:Android> "Category:Android") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![LAMW](https://wiki.freepascal.org/images/b/b3/lamw_git_small_circle.png)](<https://github.com/jmpessoa/lazandroidmodulewizard> "LAMW")

**LAMW** : Lazarus Android Module Wizard is a wizard to create JNI Android loadable module (.so) and Android Apk using Lazarus/Free Pascal. 

  
**Features:**

    Native Android GUI 

    

    AppCompat and Material Design supported!

    RAD! Form designer and drag&drop component development model! 

    

    More than 140 components!
    Github Page: <https://github.com/jmpessoa/lazandroidmodulewizard>

  


## Contents

  * 1 Get Lazarus for Android
    * 1.1 Laz4Android 2.0.12 (Windows)
    * 1.2 LAMW Manager (Linux and Windows)
    * 1.3 Install LAMW using fpcupdeluxe (Linux and Windows)
    * 1.4 Do It Yourself! (Windows)
  * 2 Infrastructure
    * 2.1 Get Java JDK
    * 2.2 Get Android NDK
    * 2.3 Get Ant builder
    * 2.4 Get Gradle builder
    * 2.5 Get Android SDK
  * 3 Using LAMW
    * 3.1 Configure Paths
    * 3.2 Create and Run your first Android Apk
  * 4 Other references



## Get Lazarus for Android

### Laz4Android 2.0.12 (Windows)

    All cross-android compilers already installed!

    arm-android/aarch64-android/i386-android/x86_64-android/jvm-android

  


How to

    Install * [Laz4Android 2.0.12](<http://sourceforge.net/projects/laz4android/files/?source=navbar>)

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Install here: "C:\laz4android2.0.12" (not "Program Files" !!!)

    Install * [LAMW](<https://github.com/jmpessoa/lazandroidmodulewizard/archive/master.zip>)

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Download LAMW and unzip it in some folder (ex. "C:\laz4android2.0.12\components")

    

    Packages installations order/sequence:

    

    

    tfpandroidbridge_pack.lpk (in "..../android_bridges" folder)
    lazandroidwizardpack.lpk (in ""..../android_wizard" folder)
    amw_ide_tools.lpk (in "..../ide_tools" folder)

Go to Infrastructure.

### LAMW Manager (Linux and Windows)

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** All in One! LAMW Manage produces a complete Lazarus for Android environment by automating the step Infrastructure!

    Install * [LAMW Manager Installer for Linux](<https://github.com/dosza/LAMWManager-linux>)
    Install * [LAMW Manager Installer for Windows](<https://github.com/dosza/LAMWManager-win>)

### Install LAMW using fpcupdeluxe (Linux and Windows)

    

    [How to install on Linux (FPCUPdeluxe + LAMW)](<LAMW_install_linux_fpcupdeluxe.md>)
    [How to install on Windows (FPCUPdeluxe + LAMW)](<LAMW_install_windows_fpcupdeluxe.md>)

### Do It Yourself! (Windows)

    Install * [Lazarus 2.0.12](<https://sourceforge.net/projects/lazarus/files/Lazarus%20Windows%2064%20bits/Lazarus%202.0.12/lazarus-2.0.12-fpc-3.2.0-win64.exe/download>)

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Install here: "C:\lazarus2.0.12" (not "Program Files" !!!)

    Install * [LAMW](<https://github.com/jmpessoa/lazandroidmodulewizard/archive/master.zip>)

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Download LAMW and unzip it in some folder (ex. "C:\lazarus2.0.12\components")

    

    Packages installations order/sequence:

    

    

    tfpandroidbridge_pack.lpk (in "..../android_bridges" folder)
    lazandroidwizardpack.lpk (in ""..../android_wizard" folder)
    amw_ide_tools.lpk (in "..../ide_tools" folder)

    Install * [FPC source code (trunk)](<https://gitlab.com/freepascal.org/fpc/source/-/archive/main/source-main.zip>)

    

    Download FPC source code and unzip it in some folder and point up the source path in next step

  


    Go to Lazarus menu "Tools" >> "[LAMW] Android Module Wizard" >> "Build FPC Cross Android"

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** repeat the "Build and install" process once for each architecture

    

    

    (x) Armv7a + Soft (android 32 bits << tested!)

    

    

    

    Build

    

    

    

    Install

    

    

    (x) Aarch64 (android 64 bits << tested!)

    

    

    

    Build

    

    

    

    Install

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** After "build" and "install" the cross-compilers and after to do all Infrastructure go to Using LAMW and try to create your first [New] LAMW project!

    If you get an error "Fatal: Cannot find unit system used by fcllaz of package FCL" when trying "Run" >> "Build" your project then go to "fpc.cfg" (ex. "C:\lazarus2.0.12\fpc\3.2.0\bin") and:

    

    **change:**

    

    

    #searchpath for units and other system dependent things

    

    

    -FuC:\lazarus2.0.12\fpc\$FPCVERSION/units/$fpctarget

    

    

    -FuC:\lazarus2.0.12\fpc\$FPCVERSION/units/$fpctarget/*

    

    

    -FuC:\lazarus2.0.12\fpc\$FPCVERSION/units/$fpctarget/rtl

    

    **to:**

    

    

    #searchpath for units and other system dependent things

    

    

    -FuC:\lazarus2.0.12\fpc\3.2.0/units/$fpctarget

    

    

    -FuC:\lazarus2.0.12\fpc\3.2.0/units/$fpctarget/*

    

    

    -FuC:\lazarus2.0.12\fpc\3.2.0/units/$fpctarget/rtl

  


    And go to Lazarus IDE menu "Tools" >> "Options" >> "Environment" >> [FPC Source]

    

    **change:**

    

    

    $(LazarusDir)fpc\$(FPCVer)\source

    

    **to:**

    

    

    $(LazarusDir)fpc\3.2.0\source

  


## Infrastructure

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Only for non-users of LAMW Manager!

### Get Java JDK

    Install * [Java JDK 8](<http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>)

### Get Android NDK

    Install * [r19c](<https://github.com/android/ndk/wiki/Unsupported-Downloads>)

### Get Ant builder

    Install * [Ant](<http://ant.apache.org/bindownload.cgi>)

    

    Simply extract the zip file to a convenient location...

### Get Gradle builder

    Install * [Gradle 6.6.1](<https://gradle.org/next-steps/?version=6.6.1&format=bin>)

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Use the option "extract here" to produce the folder "gradle-6.6.1" in a convenient location...

### Get Android SDK

    Install * [r25.2.5-windows](<https://dl.google.com/android/repository/tools_r25.2.5-windows.zip>)

    Install * [r25.2.5-linux](<https://dl.google.com/android/repository/tools_r25.2.5-linux.zip>)

    Install * [r25.2.5-macosx](<https://dl.google.com/android/repository/tools_r25.2.5-macosx.zip>)

How to

    unpacked/install to a "sdk" folder

    open a command line terminal and go to folder "sdk/tools"

    run the command >> "android update sdk" to open a GUI "SDK Manager"

    

    Go to [Tools] and keep "as is"

    

    

    Android SDK Tools (installed)
    (x) Android SDK Platform-Tools
    (x) Android SDK Build-Tools 29.0.3 (and others more recent)

    

    Go to [Android R] and uncheck all!

    

    Go to [Android 10 API 29] uncheck all and check only

    

    

    (x)SDK Platform

    

    Go to [Extras] and check:

    

    

    (x)Android Support Repository

    

    

    (x)Google USB Drive (Windows only...)

    

    

    (x)Google Repository

    

    

    (x)Google Play Services

    

    [Install 7 package!]

  


## Using LAMW

### Configure Paths

    Lazarus IDE menu "Tools" >> "[LAMW] Android Module Wizard" >> "Paths Settings ..."

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** MacOs: path to Java JDK auto setting!

### Create and Run your first Android Apk

How to

    From Lazarus IDE select "Project" >> "New Project"

    From displayed dialog select "[LAMW] GUI Android Module" and "Ok"

    Fill the form/dialog fields and "Ok"

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Path to Workspace" is your projects folder

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Accept "default" options! (but pay attention to the * signage)

    "Save" unit1 as/where suggested

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Search your project folder... you will find many treasures there! (look for lazarus project in ".../jni" folder)

    From Lazarus IDE select "Run" >> "Build"

    

    **Success!** Your system is up to produce your first Android Apk!

  


    Configure your phone device to [debug mode](<https://developer.android.com/studio/debug/dev-options>) and plug it to the computer usb port

  


    From Lazarus IDE select "Run" >> "[LAMW] Build Apk and Run"

    

    **Congratulations!** You are now an Android Developer!

## Other references

[Tutorial: My First "Hello Word" App](<https://github.com/jmpessoa/lazandroidmodulewizard/blob/master/docs/AppHelloWorld.md>)

---

_Source: [https://wiki.freepascal.org/LAMW](https://web.archive.org/web/20231129032052/https://wiki.freepascal.org/LAMW)_
