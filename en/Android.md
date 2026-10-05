# Android

│ **English (en)** │  **[русский (ru)](<../ru/Android.md>)** │

All Articles about Android are listed in [Portal:Android](<Portal_Android.md> "Portal:Android"). 

[![Android robot.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

This article applies to [Android](</Category:Android> "Category:Android") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Android target
  * 2 Android NDK
  * 3 Download FPC for Android
    * 3.1 Release version
    * 3.2 Development version
  * 4 Building cross compiler
  * 5 Compiling programs
  * 6 Notes
  * 7 Known issues
  * 8 Freeze during shared library initialization on Android prior 4.1
  * 9 See also
  * 10 External links



## Android target

Support for the Android target is present starting from FPC 3.0. 

Supported CPUs: 

  * ARM (`CPU_TARGET=arm`)
  * x86 (`CPU_TARGET=i386`)
  * MIPS (`CPU_TARGET=mipsel`)
  * ARM64 (`CPU_TARGET=aaarch64`) - in FPC 3.2+
  * x86-64 (`CPU_TARGET=x86_64`) - in FPC 3.2+
  * [JVM](<FPC_JVM.md> "FPC JVM") (not discussed on this page)



## Android NDK

You need to download and install Android NDK in order to get the cross binutils that compile programs for the android target on your current machine. 

Download Android NDK using this link: <https://developer.android.com/ndk/downloads/index.html>

Extract it to some folder. 

## Download FPC for Android

### Release version

If you develop for Android using a Windows machine you can download a setup package which contains cross-compilers for Android (ARM, i386, MIPS). After installation of the cross-compilers you can build binaries for Android without any additional steps. 

For other host platforms you need to download the FPC sources archive and build a cross-compiler as described below. 

Download the latest release cross-compilers and FPC sources here: <http://freepascal.org/download.html>

### Development version

The development (trunk) version of FPC may contain new features or bug fixes compared to the latest release version. 

Check out the latest trunk sources of FPC using the following command: 
    
    
    git clone https://gitlab.com/freepascal.org/fpc/source.git fpcsrc
    

Now you have the latest compiler sources in the `fpcsrc` sub-folder. 

## Building cross compiler

You need to have a working installation of the latest version of FPC for your host platform in order to create the cross-compiler. 

This tutorial describes how to build a cross-compiler for the **`arm-android`** target on Windows. But the tutorial can be applied to any system with small obvious modifications. 

Use the similar approach to build a **`aarch64-android`** or **`i386-android`** or **`x86_64-android`** or **`mipsel-android`** cross-compiler. 

Let's assume the following paths: 

  * Android NDK path: `C:\Program Files\Android SDK\android-ndk-r12b`
  * FPC svn sources path: `C:\Develop\FPC\fpcsrc`
  * The cross-compiler installation path: `C:\Develop\FPC\pp`



Add the path to Android binutils to the PATH environment variable. In this tutorial the path is: `C:\Program Files\Android SDK\android-ndk-r12b\toolchains\arm-linux-androideabi-4.9\prebuilt\windows\bin`

Open the Command line prompt and `cd` to the root of FPC sources folder: `C:\Develop\FPC\fpcsrc`

Execute the following command: 
    
    
    make clean crossall crossinstall OS_TARGET=android CPU_TARGET=arm INSTALL_PREFIX=C:\Develop\FPC\pp
    

After that you should have the cross-compiler and units installed in folder `C:\Develop\FPC\pp` By default all units are built with -O2 optimization switch. Also software FPU emulation is used for ARM CPU by default. If you need to specify different compiler options for building, use the `CROSSOPT` parameter. For example if you wish to build units with hardware FPU support and ARMv7a CPU use the following command: 
    
    
    make clean crossall crossinstall OS_TARGET=android CPU_TARGET=arm CROSSOPT="-Cparmv7a -Cfvfpv3" INSTALL_PREFIX=C:\Develop\FPC\pp
    

Now create a new `fpc.cfg` file in folder `C:\Develop\FPC\pp\bin\i386-win32` and paste into it the following text: 
    
    
    -FuC:\Develop\FPC\pp\units\$FPCTARGET\*
    
    #ifdef android
    #ifdef cpuarm
    -FlC:\Program Files\Android SDK\android-ndk-r12b\platforms\android-21\arch-arm\usr\lib
    #endif
    #ifdef cpuaarch64
    -FlC:\Program Files\Android SDK\android-ndk-r12b\platforms\android-21\arch-arm64\usr\lib
    #endif
    #ifdef cpui386
    -FlC:\Program Files\Android SDK\android-ndk-r12b\platforms\android-21\arch-x86\usr\lib
    #endif
    #ifdef cpux86_64
    -FlC:\Program Files\Android SDK\android-ndk-r12b\platforms\android-21\arch-x86_64\usr\lib
    #endif
    #ifdef cpumips32
    -FlC:\Program Files\Android SDK\android-ndk-r12b\platforms\android-21\arch-mips\usr\lib
    #endif
    #endif
    

## Compiling programs

Now, when the cross-compiler is ready, you can compile programs for the Android target: 
    
    
    C:\Develop\FPC\pp\bin\i386-win32\ppcrossarm -Tandroid testprog.pas
    

## Notes

  * The compiler does NOT define **`LINUX`** during compilation! It defines **`UNIX`** and **`ANDROID`** though.  
(Background: Android is not completely Linux compatible, e.g. a few syscalls and default library functions e.g. in pthreads, ... are missing, and it generally behaves different in a few ways. This makes the amount of defines manageable by avoiding `if defined(linux) and not defined(android)`. Actually, the list of things that are different is only growing).


  * Shared libraries do not have **`argc/argv`** available (i.e. set to 0 or nil) because of Android.


  * The **System** unit contains some useful [Android-specific functions](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/rtl/android/sysandroidh.inc?view=markup>).


  * The compiler and RTL do special processing for JNI shared libraries. The compiler treats a library as a JNI library only if the library exports the **`JNI_OnLoad`** function. Therefore, if you are creating a JNI shared library, always export `JNI_OnLoad`, even if it is empty.


  * For JNI shared libraries standard output and error output are redirected to the Android system log. Therefore you can use `writeln()` to write to the system log.


  * By default FPC builds PIC enabled shared libraries and executables for the Android target. PIC shared libraries work on any Android version (tested even on Android 1.5). But PIC executables can run only on Android 4.1+. To run executables on older versions of Android you need to disable PIC by passing the **`-Cg-`** switch to the compiler.



## Known issues

  * Shared libraries compiled with FPC 3.0 does not work on Android 6.0+ and executables does not run on Android 5.0+. Starting from Android 6.0 (Marshmallow), it is required to build PIC compatible shared libraries. Starting from Android 5.0 (Lollipop), it is required to build PIC compatible executables (PIE).**  
PIC support for the Android target is present starting from FPC 3.0.2.**  
NOTE: By default FPC 3.0.2+ builds PIC enabled shared libraries and executables for the Android target. PIC shared libraries work on any Android version (tested even on Android 1.5). But PIC executables can run only on Android 4.1+. To run executables on older versions of Android you need to disable PIC by passing the `-Cg-` switch to the compiler.
  * Shared libraries compiled with FPC 3.0.x does not work on Android 8.0+ when an application targets API 26+. This is caused by usage of syscalls which are blocked by SECCOMP starting from Android 8.  
**This issue is fixed in FPC 3.2.**
  * If you use the **cwstring** unit in a JNI shared library, your Java application will hang when loading the library on Android 4.0 or earlier. This also applies to any unit which dynamically loads shared libraries during initialization. See more information below.  
**This issue is fixed in FPC 3.2.**
  * The **clocale** unit is not implemented in FPC 3.0.  
**It is implemented in FPC 3.2.**
  * The **netdb** unit is unusable in FPC 3.0 due to missing configuration of DNS servers.  
**This issue is fixed in FPC 3.2.**
  * The `Now` function always returns a GMT time in FPC 3.0.  
**This issue is fixed in FPC 3.2.**
  * The following functions return unusable paths in FPC 3.0: `GetTempDir, GetAppConfigDir, GetAppConfigFile, GetUserDir`.  
**This issue is fixed in FPC 3.2.**



## Freeze during shared library initialization on Android prior 4.1

If your shared library contains a unit which performs dynamic loading of other shared libraries using `LoadLibaray()` or `dlopen()`, your library may fail to load on Android 4.0 or earlier. The library loading will freeze in such case. One of the units, which can trigger the issue is **cwstring**. 

Due to a bug in Android versions prior to 4.1, the `dlopen()` API function does not support recursive calls. When a shared library is being loaded by JVM, using `dlopen()`, the library initialization is executed, including initialization code of all units in the library. If a unit initialization code calls `dlopen()` \- it simply hangs, since the initial `dlopen()` is not finished yet and recursion is not allowed. 

**This issue is fixed in FPC 3.2.0** : To workaround this issue the compiler uses the following trick: If a library exports `JNI_OnLoad()`, then no unit initialization is performed during library load. The initialization is called when JVM has loaded the library and then calls `JNI_OnLoad()`. 

Therefore, if you are creating a JNI shared library, always export `JNI_OnLoad`, even if it is empty. 

Also you can trigger this bug if you have a JNI shared library, which use other shared library, created using FPC. The loading of this second library may hang. To workaround this, you need to export an empty `JNI_OnLoad()` function in the second library. Then you need to call `JNI_OnLoad()` from the initialization of the first library. 

## See also

  * [Android tutorial](<Android_tutorial.md> "Android tutorial")
  * You can use the [pas2jni](<pas2jni.md> "pas2jni") utility to easily create JNI shared libraries from your existing code and use it in Android native Java applications.
  * [Cross compiling](<Cross_compiling.md> "Cross compiling") for cross compiling from Linux, macOS and Windows along with other useful cross-compilation information.



## External links

  * [Tutorial "Lazarus and Android"](<http://sourceforge.net/projects/laztoapk/files/Lazarus%20and%20Android.pdf/download>) (PDF file)
  * [How to create Android apps with Lazarus](<https://pascaldelphilazarus.wordpress.com/2014/06/02/how-to-create-android-apps-with-lazarus/>)
  * [How to setup cross-compilation to Android](<https://forum.lazarus.freepascal.org/index.php/topic,32553.0.html>) (Lazarus Forum)

---

_Source: [https://wiki.freepascal.org/Android](https://web.archive.org/web/20241210022435/https://wiki.freepascal.org/Android)_
