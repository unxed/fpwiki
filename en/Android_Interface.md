# Custom Drawn Interface/Android

[![Android robot.svg](https://wiki.freepascal.org/images/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

This article applies to [Android](</Category:Android> "Category:Android") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

WARNING: THIS INFORMATION IS DATED AND NEEDS UPDATING 

│ **English (en)** │


Go back to [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface")

## Contents

  * 1 Architecture
  * 2 Roadmap
  * 3 Using the Android SDK, Emulator and Phones
  * 4 Android Programming
  * 5 Configuring the Free Pascal Compiler for Android
    * 5.1 Use the ready Virtual Box Image with FPC and Lazarus
    * 5.2 Building the compiler yourself in Windows
    * 5.3 Using the pre-compiled compiler in Windows
    * 5.4 Building the compiler yourself in Linux
    * 5.5 Using the pre-compiled compiler in Linux
  * 6 Install the Java JDK and the Android SDK and NDK
  * 7 Compiling the example LCL Android Application
  * 8 How to create an LCL Android Application
    * 8.1 Step 1 - Creating the LPI
    * 8.2 Step 2 - Building the library
    * 8.3 Step 3 - Create the Android project structure
    * 8.4 Step 4 - Creating the APK package
      * 8.4.1 Step 4 - Creating the APK package in Linux
      * 8.4.2 Step 4 - Creating the APK package in Windows
    * 8.5 Building an LCL Android application with debug info
  * 9 Oh no! My LCL Android application doesn't work
    * 9.1 The Pascal executable was compiled for a wrong architecture, operating system and/or widgetset
    * 9.2 My Pascal application crashed. How to get a stacktrace?
    * 9.3 The indispensable build options
    * 9.4 Wrong package name
  * 10 Free Pascal Bugs on Android Support
    * 10.1 FPC4Android Branches
      * 10.1.1 FPC4Android 2.5.1
      * 10.1.2 FPC4Android 2.6.1
  * 11 Android SDK and NDK bugs
    * 11.1 Android NDK for Windows broken linker
    * 11.2 Error while calling DX: Trouble processing: bad class file magic (cafebabe)
  * 12 Development Notes
    * 12.1 The mysterious JNI Crash (SIGILL - illegal instruction)
    * 12.2 NDK Libraries available in Android 2.2 (API level 8)
    * 12.3 Native text drawing
    * 12.4 Flood of garbage collector messages when the caret is visible
    * 12.5 Why the caret of a TEdit keeps blinking after clicking on the form
    * 12.6 Android ComboBox
    * 12.7 Using the Native ListView Dialog
    * 12.8 LazDeviceAPIs



## Architecture

LCL-CustomDrawn-Android utilizes a minimal Java application which communicates with our Pascal library and sends all events to it and also obeys commands from it. The communication is done via JNI as supported by Google. The controls are not native, instead they are all drawing using TRawImage+TLazIntfImage+TLazCanvas and events and the painting are clipped using LazRegions. The drawing itself is done in Pascal using jnigraphics to draw on a Bitmap Java object which is then drawn by the minimal Java activity on a SurfaceView. 

Our previous and short-lived attempt to write an LCL Interface for Android (LCL-Android) utilized a non-standard method of using native executables which communicated with a Java machine via Pipes. This method was abandoned because it was considered unsupported by Google. It is considered obsolete and people should use LCL-CustomDrawn-Android instead. This older interface was documented in [Android Interface](<Android_Interface.md> "Android Interface"). 

## Roadmap

Part 1, until March 2012: 

  1. ~~Build the set of[Lazarus Custom Drawn Controls](<Lazarus_Custom_Drawn_Controls.md> "Lazarus Custom Drawn Controls")~~
  2. ~~Initial bindings for the Android APIs~~
  3. ~~Create an application to automatically generate the bindings~~
  4. ~~Start the new widgetset~~
  5. ~~Implement support for JNI~~
  6. ~~Merge the Lazarus Custom Drawn Controls into the LCL and use them to implement all basic controls~~
  7. ~~Add text support for Android~~
  8. ~~Implement basic window client area scrolling~~
  9. Add mobile emulator for desktop platforms 
  10. Improve the mobile scrolling 
  11. ~~Add text support in X11~~
  12. ~~Add DPI awareness and adaptation in the LCL~~
  13. ~~Add non-native text support via pasfreetype~~



Part 2: April, May, June 2012: 

  1. ~~TButton improve the Android visual~~
  2. TBitBtn 
  3. ~~TRadioButton finish the native look~~
  4. Calendar dialog -> Problematic as the LCL has no calendar dialog, it just opens a normal form and puts a calendar on it, so we need a TCDCalendar widget 
  5. MaskEdit 
  6. ~~Improve TComboBox~~
  7. TMemo 
  8. TListView as a list with columns 
  9. ~~TMainMenu~~
  10. Database with sqlite 
  11. Barcode reading 
  12. Windows add-on installer 



## Using the Android SDK, Emulator and Phones

[Custom Drawn Interface/Using the Android SDK, Emulator and Phones](<Custom_Drawn_Interface/Using_the_Android_SDK,_Emulator_and_Phones.md> "Custom Drawn Interface/Using the Android SDK, Emulator and Phones")

## Android Programming

See [Android Programming](<Android_Programming.md> "Android Programming"), also includes caracteristics of the various phones relevant for programming. 

## Configuring the Free Pascal Compiler for Android

NOTE:   
---  
The cross-compiler instructions below are probably obsolete, since support for the [Android target](<Android.md> "Android") is present in the trunk (development) version 2.7.1 of FPC. Here are up to date instructions: [building a cross-compiler for Android target](<Android.md> "Android").   
  
### Use the ready Virtual Box Image with FPC and Lazarus

The fastest way to configure Free Pascal and Lazarus for Android development is simply using a pre-configured virtual machine image which already includes Mageia Linux 1, KDE, Lazarus 0.9.31, Subversion, Free Pascal for both x86-linux and arm-linux (targetting Android), the Android SDK, the Android NDK, Java, Ant, Gtk2 and all it's development packages, the GNU debugger (GDB), subversion and everything else necessary for building Android applications with Lazarus. 

The download like for the virtual machine image is: <https://sourceforge.net/projects/p-tools/files/Lazarus_for_Android_VM_Image/snapshot_30_dez_2011/> (warning: the VirtualBox image was created using the Mac OS X default Zip utility and not all zip applications can handle it. The Mac OS X default zip application can, and [PeaZip](<http://peazip.sourceforge.net/>) too and [WinRAR](<http://win-rar.com/download.html>) too. Both 7-Zip and the Windows 7 default unzipper fail. 

The virtual machine image is composed by two files: “Android Devel VM.vbox” and “Android Devel VM.hdd”. Both need to be located in the same folder and also note that the source forge download is a zipped file, so it needs to be unzipped. After unzipping the VM image has 5,2GB. To use this virtual machine one should install Virtual Box and then use it to open the .vbox file. Then one can already use this Linux virtual machine to build Android applications. The process of building an application involves first opening Lazarus and building the application in Lazarus, for example the example project available on ~/Programas/lazarus/example/androidlcl/ and then going into the Linux terminal, navigating to ~/Programas/lazarus/example/androidlcl/android and issuing the command “ant debug” which will generate the APK file in ~/Programas/ lazarus/example/androidlcl/android/bin/LCLExample-debug.apk. There are two users in the virtual machine: “root” and “pascaldev” and both of them use the password “root”. 

Note that the virtual machine contains the Lazarus source code from when it was created, but one can also use svn update to get the latest code. Note that this involves some risks, however, as the trunk version might ocasionally contain problems. 

### Building the compiler yourself in Windows

**Step 1** \- Install the Android SDK and NDK 

Download from Google and install both the SDK and the NDK for Windows. 

Check your path to the binutils installed by the NDK. For example, for NDK r7 installed in C:\Programas\android-ndk-r7\ the path is C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin 

Also check your path to the libraries installed by the NDK. For example, for NDK r7 installed in C:\Programas\android-ndk-r7\ the path is C:\Programas\android-ndk-r7\platforms\android-8\arch-arm\usr 

**Step 2** \- Download FPC 2.5.1 from January 19th 

Create a folder to store the FPC trunk source code and checkout revision 16790 

Or if you want support for database and many other nice fixes use the branch fpc4android from here: 

svn co <https://p-tools.svn.sourceforge.net/svnroot/p-tools/fpc4android> fpc4android 

**Step 3** \- Install FPC 2.4.4 

An easy way to obtain a good FPC 2.4.4 is installing Lazarus 0.9.30.2. For example, let's say you install this Lazarus version in C:\Programas\lazarusfpc244 

**Step 4** \- Create the build script and built it 

Create the output directory, for example: C:\Programas\fpcandroid\output 

Next create a file called build.bat in the fpc root folder, in our example C:\Programas\fpcandroid and copy to it: 
    
    
    SET PATH=C:\Programas\lazarusfpc244\fpc\2.4.4\bin\i386-win32\
    make crossinstall CPU_TARGET=arm OS_TARGET=linux CROSSBINDIR=C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin OPT=-dFPC_ARMEL BINUTILSPREFIX=arm-linux-androideabi- INSTALL_PREFIX=C:\Programas\fpcandroid\output
    pause

Next run this batch script and you should have FPC installed into C:\Programas\fpcandroid\output 

**Step 5** \- Copy some cross-binutils into the bin dir 

You now should have a FPC binary in C:\Programas\fpcandroid\output\bin\i386-win32 

Now get these 3 files: 

C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin\arm-linux-androideabi-as.exe C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin\arm-linux-androideabi-ld.exe C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin\arm-linux-androideabi-strip.exe 

And copy and then rename to have: 

C:\Programas\fpcandroid\output\bin\i386-win32\arm-linux-as.exe C:\Programas\fpcandroid\output\bin\i386-win32\arm-linux-ld.exe C:\Programas\fpcandroid\output\bin\i386-win32\arm-linux-strip.exe 

**Step 6** \- Merge this into a Lazarus FPC setup 

Let's say you have the following Lazarus C:\Programas\lazarus31 with the following fpc installed inside it: C:\Programas\lazarus31\fpc\2.6.1\bin\i386-win32 and C:\Programas\lazarus31\fpc\2.6.1\units\ 

Now create a directory C:\Programas\lazarus31\fpc\2.5.1\units\arm-linux and copy C:\Programas\fpcandroid\output\units\arm-linux there 

Copy all .exe files from C:\Programas\fpcandroid\output\bin\i386-win32\ into C:\Programas\lazarus31\fpc\2.6.1\bin\i386-win32 

(! copy the exe-files to FPC 2.6.1 !) 

Now you can already build the example project androidlcltest.lpi using the Windows-Release build mode 

### Using the pre-compiled compiler in Windows

**Step 1** \- Download and unzip the pre-compiled compiler 

The download is available here: <https://sourceforge.net/projects/p-tools/files/Free%20Pascal%20for%20ARM/Windows/>

**Step 2** \- Merge it into the FPC installation 

Let's say you have the following Lazarus C:\Programas\lazarus31 with the following fpc installed inside it: C:\Programas\lazarus31\fpc\2.6.1\bin\i386-win32 and C:\Programas\lazarus31\fpc\2.6.1\units\ 

Now create a directory C:\Programas\lazarus31\fpc\2.5.1\units\arm-linx and copy fpc4android_2_5_1_26_mar_2012\units\arm-linux there 

Copy all .exe files from fpc4android_2_5_1_26_mar_2012\bin\i386-win32\ into C:\Programas\lazarus31\fpc\2.6.1\bin\i386-win32 (the version here should be of the installed Windows compiler, not the version of the cross-compiler, so might be 2.6.0 or 2.7.1 or something else) 

**Step 3** \- Install the Java JDK and the Android SDK and NDK 

If you haven't done so already, then install the Java JDK and the Android SDK and NDK. I recommend the JDK 6 (JDK 7 doesnt work!), latest SDK and the NDK version 7. The Windows NDK 7b has a serious bug: [[1]](<https://groups.google.com/group/android-ndk/browse_thread/thread/d8c88bd58ad6df87/5396d1223f0c65b1?hl=pl&lnk=gst&q=Felipe+Monteiro+de+Carvalho#5396d1223f0c65b1>) Some people also report that 7c works fine, and I tested that 8c works fine. 

  * Java JDK (!WARNING! Download JDK version 6! Version 7 does not work!!!): <http://www.oracle.com/technetwork/java/javase/downloads/index.html>
  * Android SDK Download (!WARNING! Instead of clicking in the first download link that you see, go to "Use an existing IDE"!): <http://developer.android.com/sdk/index.html> !WARNING! The SDKs r21 and superior removed the nice apkbuilder.bat script which is utilized in our build system in Windows. If you want to use our Windows build scripts, download the SDK r20 from this link: <http://dl.google.com/android/installer_r20-windows.exe>
  * Android NDK version 7 download link: <http://dl.google.com/android/ndk/android-ndk-r7-windows.zip>



Now you can already build the example project androidlcltest.lpi using the Windows-Release build mode and these instructions: [Custom Drawn Interface/Android#Compiling_the_example_LCL_Android_Application](<Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")

### Building the compiler yourself in Linux

See [Setup_Cross_Compile_For_ARM](<Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM") and make sure to use the option OPT="-dFPC_ARMEL" for building the compiler. 

### Using the pre-compiled compiler in Linux

A pre-compiled compiler is provided for convenience for users. The following steps were tested in Mandriva Linux 2010.0 and 2010.1: 

**Required Environment**

  * The latest stable FPC installed in the system via the RPM / DEB / TAR package 



**Step 1** \- Install the cross-binutils 

For Mandriva Linux the RPM package containing arm-linux-as, arm-linux-ld, etc, which are the cross-binutils can be found here: <http://rpm.pbone.net/index.php3/stat/4/idpl/14252825/dir/mandriva_2010/com/cross-arm-binutils-2.20.51.0.4-2mnb2.i586.rpm.html>

Just download the RPM package and install it using: 
    
    
    rpm -ivh cross-arm-binutils-2.20.51.0.4-2mnb2.i586.rpm 
    

In Mandriva Linux 2010.0 the dependencies won't match, as the package is for 2010.1, but one can simply ignore this problem and it works fine using --nodeps: 
    
    
    rpm -ivh --nodeps cross-arm-binutils-2.20.51.0.4-2mnb2.i586.rpm 
    

For other distributions use the corresponding package, or else read the instructions for building the cross-binutils yourself at [Setup_Cross_Compile_For_ARM](<Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM")

**Step 2** \- Configure the cross-binutils 

The assembler needs a parameter to tell it which ARM ABI to use. A choice which works good is EABI-5, which is compatible with all Android devices available as of Jan 2011. To set this, we will rename the original assembler and substitute it with a shell script which passes the desired parameter. These commands will do it: 
    
    
    su
    mv /usr/bin/arm-linux-as /usr/bin/arm-linux-as_org
    gedit /usr/bin/arm-linux-as
    

Now paste into the editor this code: 
    
    
    #!/bin/sh
    /usr/bin/arm-linux-as_org -meabi=5 $@
    

And don't forget to then make it executable: 
    
    
    chmod 755 /usr/bin/arm-linux-as
    

**Step 3** \- Install the Free Pascal Cross-Compiler 

At this point the pre-compiled FPC can be download from here: <http://sourceforge.net/projects/p-tools/files/Free%20Pascal%20for%20ARM/>

Then use these commands to install the pre-compiled Free Pascal cross-compiler into /usr: 
    
    
    [felipe@localhost Programas]$ ls -l
    total 20664
    -rw-rw-r--  1 felipe felipe 17098552 2010-10-25 08:17 fpc-2.5.1.arm-linux.tar.gz
    [felipe@localhost Programas]$ su
    Password: 
    [root@localhost Programas]# cp fpc-2.5.1.arm-linux.tar.gz /usr/
    [root@localhost Programas]# cd /usr/
    [root@localhost usr]# tar -xvf fpc-2.5.1.arm-linux.tar.gz 
    [root@localhost usr]# ln -s /usr/lib/fpc/2.5.1/ppcrossarm /usr/bin/ppcrossarm
    

**Step 4** \- Verify if your Cross-Compiler works 

If you made no errors in the previous steps, it should work, so try to call it like this: 
    
    
    [felipe@localhost Programas]$ /usr/bin/ppcrossarm
    

If this command works and fpc shows its options, then you configured the cross-compiler correctly, if not, then try to find out if your symbolic link points to a correct location with this command: 

[felipe@localhost Programas]$ ls -ls /usr/bin/ppc* 

Now we are ready to compile Android applications using the Lazarus IDE! Configuring the fpc.cfg file isn't necessary, the old compiler will automatically find the new compiler and it's object files. 

## Install the Java JDK and the Android SDK and NDK

If you haven't done so already, then install the Java JDK and the Android SDK and NDK. I recommend the JDK 6 (JDK 7 doesnt work!), latest SDK and the NDK version 7. The Windows NDK 7b has a serious bug: [[2]](<https://groups.google.com/group/android-ndk/browse_thread/thread/d8c88bd58ad6df87/5396d1223f0c65b1?hl=pl&lnk=gst&q=Felipe+Monteiro+de+Carvalho#5396d1223f0c65b1>) Some people also report that 7c works fine, and I tested that 8c works fine. 

  * Java JDK (Choose version 6!): <http://www.oracle.com/technetwork/java/javase/downloads/index.html>
  * Android SDK Download (Choose Use an existing IDE!): <http://developer.android.com/sdk/index.html>
  * Android NDK version 7 download link: <http://dl.google.com/android/ndk/android-ndk-r7-windows.zip>



Now you can already build the example project androidlcltest.lpi using the Windows-Release build mode and these instructions: [Custom Drawn Interface/Android#Compiling_the_example_LCL_Android_Application](<Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")

## Compiling the example LCL Android Application

**Step 1 - Download the source code**

The source code of the example is located inside the Lazarus sources in lazarus/examples/androidlcl/androidlcltest.lpi 

**Step 2 - Make sure you have the Java JDK and the Android SDK and NDK installed**

Read here: [Custom_Drawn_Interface/Android#Install_the_Java_JDK_and_the_Android_SDK_and_NDK](<Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")

**Step 3 - Build the project using the Lazarus IDE**

Configuring Lazarus itself to use the new compiler should not normally be necessary because fpc should be able to find the symlink created, but if you have trouble in this part you can try to hard code the compiler path to use the new crosscompiler. To hardcode the compiler path in cause of trouble go to the menu "Tools->Options" and change the "Compiler Path" to "/usr/bin/ppcrossarm" 

But it is necessary to configure the LPI file to build the example. Open the Project Options dialog of the androidlcltest.lpi project and go to the section Paths and change the libraries path to reflect the correct paths of where your Android NDK was installed and where your jnigraphics.so, libc.so, etc, libraries are located for your target Android API level. For LCL-CustomDrawn-Android API level 8 is the best choice, because it supports Android 2.2+ like the LCL. The image below shows where to configure this: 

[![Configure androidlcl libraries.jpg](https://wiki.freepascal.org/images/b/b0/Configure_androidlcl_libraries.jpg)](</File:Configure_androidlcl_libraries.jpg>)

The actual lazarus release contains a error, they create a .so.so - file instead of a .so - file. So you have to change this manually in lazarus. 

**Step 4 - Build the APK**

If you are using Windows, use these instructions to build the APK: [Custom Drawn Interface/Android#Step_4_-_Creating_the_APK_package_in_Windows](<Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")

If you are using Linux follow these instructions instead: Before going to the command line to build the APK open the file lazarus/examples/androidlcl/android/local.properties and change it to reflect your Android SDK path. The file says you should not modify it, but ignore that and modify it anyway. Here is how the file looks like: 
    
    
    # This file is automatically generated by Android Tools.
    # Do not modify this file -- YOUR CHANGES WILL BE ERASED!
    # 
    # This file must *NOT* be checked in Version Control Systems,
    # as it contains information specific to your local configuration.
    
    # location of the SDK. This is only used by Ant
    # For customization when using a Version Control System, please read the
    # header note.
    sdk.dir=/home/felipe/Programas/android-sdk-linux
    

After configuring that one, go to the command line and issue these commands: 
    
    
     cd lazarus/examples/androidlcl/android
     ant debug
    

The APK will be located in lazarus/examples/androidlcl/android/bin 

**Step 5 - Install the APK in your phone and run it**

You can install via ADB or by copying the file to the sdcard. You should see this: 

[![lcl android 30 mar.png](https://wiki.freepascal.org/images/3/3e/lcl_android_30_mar.png)](</File:lcl_android_30_mar.png>)

## How to create an LCL Android Application

To create a new LCL-CustomDrawn-Android application simply copy all of the file structure and build and java files from the example project called "androidlcl". This example can be obtained from the Lazarus source tree in lazarus/examples/androidlcl 

Then you will need to modify the build files to change them to your new project name and your new Java package name. 

### Step 1 - Creating the LPI

You need a separate LPI at the moment for the Android version of the application but all the rest of the code can be shared. Create it using the template for a "Library" and then adapt the code from the example located in the lazarus source code in lazarus/examples/androidlcl 

You need to adapt the exported JNI method names to your Android Package Name. 

### Step 2 - Building the library

First of all, build the Pascal executable without debug information. This debug information is not so useful in Android and makes the executable much bigger. Open the menu Project->Project Options and set the build mode, widgetset, architecture and operating system targets, as shown in these screenshots: 

[![Android project options 1.png](https://wiki.freepascal.org/images/6/64/Android_project_options_1.png)](</File:Android_project_options_1.png>)

[![Android project options 2.png](https://wiki.freepascal.org/images/f/f7/Android_project_options_2.png)](</File:Android_project_options_2.png>)

[![Android project options 3.png](https://wiki.freepascal.org/images/9/9f/Android_project_options_3.png)](</File:Android_project_options_3.png>)

### Step 3 - Create the Android project structure

Besides the LCL project building, such as configuring a proper LPI and having a library main project file, one also needs to add all of the android project structure. Simply copy from the example project in lazarus/examples/androidlcl and adapt it to the new project. Things to change are the path to the SDK, the package name. The package name needs to be updated in the build files, in the directory structure android/src/packagename, in the activity java source code and also in the main library pascal source in the exports section. 

### Step 4 - Creating the APK package

#### Step 4 - Creating the APK package in Linux

To create the APK package in Linux simply go to the directory "android" inside the project folder and issue the command "ant debug". The package will be in android/bin/MyProject.apk 

` cd myproject/android ant debug `

Ant and all dependencies must be installed using the package management of your linux distribution. 

#### Step 4 - Creating the APK package in Windows

**Pre-requisites** : 

  * Java JDK installed: <http://www.oracle.com/technetwork/java/javase/downloads/index.html>
  * Android SDK Installed 



**Instructions** : 

In Windows we provide a APK build system which uses only the tools provided in the Android SDK and therefore does not require ANT. 

First it is necessary to create a debug key to use. To do this use this script: 
    
    
    lazarus/examples/androidlcl/android/generate_debug_key.bat
    

which can be copied from the example application to your own project folder. 

it contains following: 
    
    
    REM Adjust these paths to yours
    SET PATH=C:\Programme\Android\android-sdk\tools;C:\Programme\Android\android-sdk\platform-tools\;C:\Programme\Java\jdk1.6.0_38\bin
    SET APP_NAME=androidlcltest
    SET ANDROID_HOME=C:\Programme\Android\android-sdk
    SET APK_SDK_PLATFORM=C:\Programme\Android\android-sdk\platforms\android-9
    SET APK_PROJECT_PATH=C:\Programme\lazarus\examples\androidlcl\android
    
    mkdir bin
    
    keytool --help
    
    keytool -genkey -v -keystore bin\LCLDebugKey.keystore -alias LCLDebugKey -keyalg RSA -validity 10000
    
    REM call and pause together allow us to see the results in the end
    pause
    
    

You have to fix everything after SET to your own paths. Don't forget adjusting the android version from APK_SDK_PLATFORM! 

After generating the key, use the other script: 
    
    
    lazarus/examples/androidlcl/android/build_debug_apk.bat
    

To build the APK file. Also remember to edit this batch file to reflect your paths. 

This script contains the following: 
    
    
    REM Adjust these paths to yours
    SET PATH=C:\Programme\Android\android-sdk\tools;C:\Programme\Android\android-sdk\platform-tools\;C:\Programme\Java\jdk1.6.0_38\bin
    SET APP_NAME=androidlcltest
    SET ANDROID_HOME=C:\Programme\Android\android-sdk
    SET APK_SDK_PLATFORM=C:\Programme\Android\android-sdk\platforms\android-9
    SET APK_PROJECT_PATH=C:\lazarus\examples\androidlcl\android 
    
    REM Create necessary directory Structure
    mkdir bin
    mkdir bin\classes
    mkdir gen
    mkdir gen\com
    mkdir gen\com\pascal
    mkdir gen\com\pascal\lcltest
    mkdir raw
    mkdir raw\lib
    mkdir raw\lib\armeabi
    
    REM Cleanup
    del bin\%APP_NAME%.ap_
    del bin\%APP_NAME%.apk
    del raw\lib\armeabi\*.so
    
    REM More directory preparation
    copy libs\armeabi\*.so raw\lib\armeabi\ 
    
    REM Resource compilation
    call aapt p -v -f -M AndroidManifest.xml -F bin\%APP_NAME%.ap_ -I %APK_SDK_PLATFORM%\android.jar -S res -m -J gen raw
    
    REM Java compiler
    call javac -verbose -classpath %APK_SDK_PLATFORM%\android.jar -d bin\classes src\com\pascal\lcltest\LCLActivity.java
    
    REM DX to convert the java bytecode to dalvik bytecode
    call dx --dex --verbose --output=%APK_PROJECT_PATH%\bin\classes.dex %APK_PROJECT_PATH%\bin\classes 
    
    REM It seams that dx calls echo off
    @echo on
    REM Now build the unsigned APK
    del %APK_PROJECT_PATH%\bin\%APP_NAME%-unsigned.apk
    call apkbuilder %APK_PROJECT_PATH%\bin\%APP_NAME%-unsigned.apk -v -u -z %APK_PROJECT_PATH%\bin\%APP_NAME%.ap_ -f %APK_PROJECT_PATH%\bin\classes.dex
    
    REM Generating on the fly a debug key
    rem keytool -genkey -v -keystore bin\LCLDebugKey.keystore -alias LCLDebugKey -keyalg RSA -validity 10000 -dname NAME -storepass senhas  -keypass senhas
    
    REM Signing the APK with a debug key
    del bin\%APP_NAME%-unaligned.apk
    
    
    
    REM Change keypass and storepass pw                        here|         and here|
    jarsigner -verbose -keystore bin\LCLDebugKey.keystore -keypass senhas -storepass senhas -signedjar bin\%APP_NAME%-unaligned.apk  bin\%APP_NAME%-unsigned.apk LCLDebugKey
    
    REM Align the final APK package
    zipalign -v 4 bin\%APP_NAME%-unaligned.apk bin\%APP_NAME%.apk 
    
    REM call and pause together allow us to see the results in the end
    pause
    

Don't forget changing the set-variables in the top AND change the pw (see above where). 

### Building an LCL Android application with debug info

It is useful to add another Android build mode which has debug info. Use all of the same options as shown above, except for the debug information: 

[![Android project options 4.png](https://wiki.freepascal.org/images/1/11/Android_project_options_4.png)](</File:Android_project_options_4.png>)

## Oh no! My LCL Android application doesn't work

There are various reasons why the app may not work. The most important thing to do when an app doesn't work is to open the logcat and see what the log says. This can be done by running this command line command: 
    
    
    ./adb logcat

And then test one of the hypotheses in the next subsections. 

### The Pascal executable was compiled for a wrong architecture, operating system and/or widgetset

This is the leading cause of executables not running. Always verify that you compiled the program to the "android" widgetset, "linux" operating system and "arm" architecture. This is done by going into the menu Project->Project Options. Then set these in the dialog all options as explained in this section: 

[How_to_build_an_LCL_Android_application](</index.php?title=How_to_build_an_LCL_Android_application&action=edit&redlink=1> "How to build an LCL Android application \(page does not exist\)")

To check if you library is arm-linux or not, execute the following commands: 
    
    
    cd libs/armeabi
    ls -l
    file liblclapp.so
    ldd liblclapp.so

If it mentions anything like 80386 or gtk, then your program was compiled for linux-x86 and gtk2, not for Android like it should. 

### My Pascal application crashed. How to get a stacktrace?

If a Pascal application crashes you should use the command "adb logcat" to obtain the stack 

Read also this thread in the Google NDK group about converting addresses to lines: <http://groups.google.com/group/android-ndk/msg/2ed2286e85f4ccdd>? 

### The indispensable build options

  * -Xd This build option is indispensable when cross-compiling from x86-linux to Android or else FPC will try to link the application against for example /usr/lib/libc.so instead of the libc.so in the NDK, even if you specify a -Fl library path 
  * -CpARMV6 This build option is indispensable because code generated for older ARM versions is unsupported by Android and might crash in some devices. See <http://groups.google.com/group/android-ndk/browse_thread/thread/ba542483f062a828>
  * -dAndroid in the package LCLBase.lpk 
  * -Parm -Tlinux for the process and target operating system 
  * -Fl should contain the path to the NDK directory with the target link shared object, such as libc.so, liblog.so, libjni.so, etc 



### Wrong package name

If you change the package name of the project, then you need to adapt the main project file to inform the widgetset of the change. In the example project this is done in the main project file, androidlcltest.lpr in this part: 
    
    
    CDWidgetset.ActivityClassName := 'com/pascal/lcltest/LCLActivity';
    

If your new package name is com.pascal.myapp, then you should change the command above to this: 
    
    
    CDWidgetset.ActivityClassName := 'com/pascal/myapp/LCLActivity';
    

## Free Pascal Bugs on Android Support

The following summarizes the state of Android Support in Free Pascal: 

  * The pre-compiled snapshot for x86-linux -> Android is known to work very well and it was built on 19th January 2011: <https://sourceforge.net/projects/p-tools/files/Free%20Pascal%20for%20ARM/>
  * On February this revision [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=16981](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=16981>) introduced this bug: <http://mantis.freepascal.org/view.php?id=18833>
  * In the end of 2011 this revision [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19036](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19036>) broke linking Android projects 



We are in contact with FPC developers to fix the situation as can be seen in: <http://mantis.freepascal.org/view.php?id=20726>

### FPC4Android Branches

Because of FPC bugs related to Android support I have rolled my own branches. 

#### FPC4Android 2.5.1

FPC4Android 2.5.1 has the following fixes applied to it: 

<http://bugs.freepascal.org/view.php?id=21208> <http://bugs.freepascal.org/view.php?id=21341> <http://bugs.freepascal.org/view.php?id=19559>

And I have created a pre-compiled x86-linux -> android-arm cross-compiler and hosted it here (it is the -v2 cross-compiler); 

<https://sourceforge.net/projects/p-tools/files/Free%20Pascal%20for%20ARM/>

To download the source execute: 
    
    
    svn co <https://p-tools.svn.sourceforge.net/svnroot/p-tools/fpc4android> fpc4android
    

#### FPC4Android 2.6.1

This branch is not finished, it lacks patches to fix Android support, patches are welcome. SVN link: 
    
    
    svn co <https://p-tools.svn.sourceforge.net/svnroot/p-tools/fpc4android_2_6> fpc4android_2_6
    

## Android SDK and NDK bugs

### Android NDK for Windows broken linker

The linker in android-ndk-r7b (path C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin\arm-linux-androideabi-ld.exe) crashes for no reason while linking LCL apps =( The linker from android-ndk-r7 and r5 work correctly. 

See <http://groups.google.com/group/android-ndk/browse_thread/thread/d8c88bd58ad6df87>

### Error while calling DX: Trouble processing: bad class file magic (cafebabe)

If while using the build script you get the following errors: 
    
    
    D:\djsystem\djpalm\android>call dx --dex --verbose --output=D:\djsystem\djpalm\a
    ndroid\bin\classes.dex D:\djsystem\djpalm\android\bin\classes
    processing D:\djsystem\djpalm\android\bin\classes\.\com\pascal\djpalm\LCLActivit
    y$1.class...
    
    trouble processing:
    bad class file magic (cafebabe) or version (0033.0000)
    ...while parsing com/pascal/djpalm/LCLActivity$1.class
    ...while processing com/pascal/djpalm/LCLActivity$1.class
    processing D:\djsystem\djpalm\android\bin\classes\.\com\pascal\djpalm\LCLActivit
    y$2.class...
    
    trouble processing:
    bad class file magic (cafebabe) or version (0033.0000)
    ...while parsing com/pascal/djpalm/LCLActivity$2.class
    ...while processing com/pascal/djpalm/LCLActivity$2.class
    processing D:\djsystem\djpalm\android\bin\classes\.\com\pascal\djpalm\LCLActivit
    y$3.class...
    

This probably means that you are using JDK 7. Unfortunatelly the current Android SDK r20 is not compatible with JDK 7, only with JDK 5 and 6. Please install JDK 6 and use that instead. See: 

  * <http://stackoverflow.com/questions/8538891/bad-class-file-magic-when-using-dx-bat>
  * <http://www.oracle.com/technetwork/java/javase/downloads/jdk6-downloads-1637591.html>



## Development Notes

### The mysterious JNI Crash (SIGILL - illegal instruction)

See the thread: <http://groups.google.com/group/android-ndk/browse_thread/thread/ba542483f062a828>

The problem was that vm^^.GetEnv would crash in HTC Wildfire, Alcatel and in the emulator with SIGILL but not in Xperia Mini, HTC Desire HD, Motorola Atrix and other smartphones. 

The answer was that one needs to specify -CpARMV6 when building because older instructions might fail in some devices. Also try restarting the emulator or running on a different device. 

### NDK Libraries available in Android 2.2 (API level 8)

LCL-CustomDrawn targets Android 2.2+ and in this API level the following libraries are supported by the NDK: 

  1. libc.so 
  2. libdl.so (linker) 
  3. OpenGL ES 1 
  4. OpenGL ES 2 
  5. libjnigraphics.so 
  6. liblog.so 
  7. libm.so 
  8. libz.so 
  9. libthread_db.so 
  10. libstdc++.so 



LCL-CustomDrawn-Android uses libjnigraphics.so and liblog.so at the moment. 

### Native text drawing

Native text drawing is a bit convoluted in Android and the documentation is quite bad too. Some gotchas already found: 

  * It draws text starting on a Y coordinate which represents the baseline, which is the bottom of strings like "abc", but not the bottom of strings like "qg". See <http://code.google.com/p/android/issues/detail?id=393>
  * There is no documentation about what the size in Paint.setTextSize represents. Some people say it is pixels, but to me it looks something else because it seams to differ across devices. 



But there are also good things about it: 

  * It can draw on a transparent bitmap and use alpha transparency to implement anti-aliasing and subpixel rendering. The text gets quite beaultiful. 
  * It can draw on the ARGB 32-bits non-premultiplied format which is a pretty good one 



### Flood of garbage collector messages when the caret is visible

Something like this: 
    
    
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 188 objects / 6664 bytes in 94ms
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 170 objects / 6160 bytes in 78ms
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 162 objects / 5904 bytes in 82ms
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 162 objects / 5856 bytes in 78ms
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 162 objects / 5872 bytes in 80ms
    D/dalvikvm( 2825): GC_EXPLICIT freed 371 objects / 18480 bytes in 91ms
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 162 objects / 5896 bytes in 78ms
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 162 objects / 5896 bytes in 78ms
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 162 objects / 5896 bytes in 81ms
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 162 objects / 5920 bytes in 78ms
    D/dalvikvm( 6364): GC_EXTERNAL_ALLOC freed 162 objects / 5920 bytes in 83ms
    

In my investigation this happens probably because the TCDEdit will request a full control invalidate on each timer tick, which currently will main a full form invalidate, so the GC needs to collect the bitmaps and other paint objects when painting that often. 

### Why the caret of a TEdit keeps blinking after clicking on the form

When clicking on the form the LCL focus will refuse to move from the Edit to the Form because this behavior seems to be hard coded in the LCL, although maybe we might change this for Android. So even while we set focus into the form, the LCL refuses to do that and keeps its focus in the Edit, thus we have a focus mismatch. This might eventually be fixed in the future. 

### Android ComboBox

For adding Android combobox support a new method was added to LCLIntf along with a global variable to store the callback of this dialog: 
    
    
    var
      OnShowSelectItemDialogResult: TOnShowSelectItemDialogResult;
     
    function ShowSelectItemDialog(const AItems: TStrings): Boolean; override;

This method will run a native item selection dialog, if the platform has one, or result false without doing anything otherwise. 

[![Android combobox.png](https://wiki.freepascal.org/images/8/82/Android_combobox.png)](</File:Android_combobox.png>)

### Using the Native ListView Dialog

To allow the user to select items from a list more easily we offer a routine which launch dialog with the native ListView. 
    
    
    {$ifdef LCLCustomDrawn}
    uses customdrawnint, lclintf;
    {$endif}
     
    procedure TForm2.Button3Click(Sender: TObject);
    begin
    {$ifdef LCLCustomDrawn}
      LCLIntf.OnListViewDialogResult := @MyOnListViewDialogResult;
      CDWidgetSet.ShowListViewDialog('Dialog Title',
        ['Title1', 'Title2', 'Title3', 'Title4', 'Title5', 'Title6'],
        ['Descr1', 'Descr2', 'Descr3', 'Descr4', 'Descr5', 'Descr6']);
    {$endif}
    end;
     
    procedure TForm2.MyOnListViewDialogResult(ASelectedItem: Integer);
    begin
      DebugLn(Format('[MyOnListViewDialogResult] ASelectedItem=%d', [ASelectedItem]));
    end;

LCLIntf.OnListViewDialogResult will be called with the selected item, with a zero-based index, or -1 if the dialog was cancelled by the user. One way to cancel the dialog is when the user pressed the back button. 

Note that there are 2 more parameters in the end of this routine with default values, which allow choosing the color for the odd and the other for the even rows. The color is an integer of 32-bits in the ARGB format, don't use TColor here! 
    
    
    procedure TCDWidgetSet.ShowListViewDialog(ATitle: string; ATitles,
      ADescriptions: array of string;
      AColorOddRow: jint = $ff292C29; AColorEvenRow: jint = $ff424542);

[![Android ListView.png](https://wiki.freepascal.org/images/2/2c/Android_ListView.png)](</File:Android_ListView.png>)

### LazDeviceAPIs

See: [LazDeviceAPIs](<LazDeviceAPIs.md> "LazDeviceAPIs")

---

_Source: [https://wiki.freepascal.org/Android_Interface](https://web.archive.org/web/20170920040157/https://wiki.freepascal.org/Android_Interface)_
