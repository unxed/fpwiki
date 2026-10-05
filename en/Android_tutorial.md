# Android tutorial

│ **English (en)** │  **[русский (ru)](<../ru/Android_tutorial.md>)** │

[![Android robot.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

This article applies to [Android](</Category:Android> "Category:Android") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Windows
    * 1.1 Prepare folder
    * 1.2 Install JDK
    * 1.3 Install SDK
    * 1.4 Install NDK
    * 1.5 Install Laz4Android
    * 1.6 Rebuild Laz4Android IDE
    * 1.7 Compile the demo "androidlcl"
    * 1.8 Deployment
    * 1.9 Links
  * 2 Linux
    * 2.1 Prepare folder
    * 2.2 Install OpenJDK
    * 2.3 Install SDK
    * 2.4 Install NDK
    * 2.5 Build FPC 2.7.1
    * 2.6 Rebuild Lazarus IDE
    * 2.7 Links



# Windows

This is a quick tutorial on how to create android apps with Lazarus. 

In this tutorial assume used versions JDK 1.6u45 + SDK 22.2.1 + ndk-r9 + Laz4Android1.1-41139-FPC2.7.1 

## Prepare folder

Create on C:\ a folder called "Android" 

## Install JDK

1.6u45 (i have installed jdk-6u45-windows-x64.exe) arm-linux-androideabi/bin <http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html#jdk-6u45-oth-JPR>

It will be installed in to C:\Program Files\Java\\. 

## Install SDK

Go to <http://developer.android.com/sdk/index.html>

DOWNLOAD FOR OTHER PLATFORMS > SDK Tools Only 

installer_r22.2.1-windows.exe (Recommended) 

Install sdk (installer_r22.2.1-windows) into C:\Android\android-sdk 

Copy at "C:\Android\android-sdk\tools" the file "apkbuilder.bat" (can be found here 

<https://github.com/ACSOP/android_sdk/raw/master/apkbuilder/etc/apkbuilder.bat> ) 

Now update the SDK [WIN START > SDK Manager ] 

add android 4.0 (API 14) 

add android 2.2 (API 8] 

Install packages. 

## Install NDK

<http://developer.android.com/tools/sdk/ndk/index.html>

Download only the file android-ndk-r9-windows-x86.zip 

Unzip android-ndk-r9-windows-x86.zip into C:\Android\android-ndk-r9 

  


## Install Laz4Android

Download it from [https://skydrive.live.com/?cid=89ae6b50650182c6&id=89AE6B50650182C6!149](<https://skydrive.live.com/?cid=89ae6b50650182c6&id=89AE6B50650182C6!149>)

New version can download from here:[[1]](<http://sourceforge.net/projects/laz4android/>)

Unzip Laz4Android1.1-41139-FPC2.7.1.7z into C:\Android\laz4android 

Copy 

C:\Android\android-ndk-r9\toolchains\arm-linux-androideabi-4.6\prebuilt\windows\bin\arm-linux-androideabi-*.exe 

to 

C:\Android\laz4android\fpc\2.7.1\bin\i386-win32\\* 

Edit the file C:\Android\laz4android\build.bat like this : 
    
    
    SET FPC_BIN_PATH=C:\Android\laz4android
    

Run build.bat (doubleclick) -> It will compile & build lazarus. 

## Rebuild Laz4Android IDE

Run C:\Android\laz4android\lazarus.exe 

Select "Start IDE" 

select "Package" "Install/Uninstall Packages" 

select customdrawn 0.0 > install selection and hit save and rebuild IDE , continue 

  


## Compile the demo "androidlcl"

Copy C:\Android\laz4android\examples\androidlcl at C:\Android\Projects\androidlcl 

Run "C:\Android\laz4android\lazarus.exe" 

File > open "C:\Android\Projects\androidlcl\androidlcltest.lpr" 

Project > project options 

Compiler options > select "Release TAndroid" 

Paths >

Libraries -Ll : C:\Android\android-ndk-r9\platforms\android-8\arch-arm\usr\lib;C:\Android\android-ndk-r9\toolchains\arm-linux-androideabi-4.6\prebuilt\windows\lib\gcc\arm-linux-androideabi\4.6 

Target file name : android\libs\armeabi\liblclapp.so 

Code generation 
    
    
     Target OS -T android
     Target CPU -P arm
     Target processor default
    

Linking : 
    
    
     Enable strip symbols (-Xs)
    

Other : -dANDROID -Xd -CpARMV6 -FLlibdl.so 

Ide macro values : macro name : LCLWidgetType macro value : customdrawn 

Now Run > Compile this will generate this library : C:\Android\Projects\androidlcl\android\libs\armeabi\liblclapp.so [5.073 kb] 

  


## Deployment

  * Edit with a text editor



C:\Android\Projects\androidlcl\android\generate_debug_key.bat 

Modify the first 6 lines like this : 
    
    
    REM Adjust these paths to yours
    SET PATH=C:\Android\android-sdk\tools;C:\Android\android-sdk\build-tools\17.0.0;C:\Android\android-sdk\platform-tools\;C:\Progra~1\Java\jdk1.6.0_45\bin
    SET APP_NAME=lcltest
    SET ANDROID_HOME=C:\Android\android-sdk
    SET APK_SDK_PLATFORM=C:\Android\android-sdk\platforms\android-8
    SET APK_PROJECT_PATH=C:\Android\Projects\androidlcl\android
    

Run "generate_debug_key.bat" 

Use as password : "senhas" 

This will produce the file : 

C:\Android\Projects\androidlcl\android\bin\LCLDebugKey.keystore 

  


  * Edit with a text editor



C:\Android\Projects\androidlcl\android\build_debug_apk.bat 

Modify the first 6 lines like this : 
    
    
    REM Adjust these paths to yours
    SET PATH=C:\Android\android-sdk\tools;C:\Android\android-sdk\build-tools\17.0.0;C:\Android\android-sdk\platform-tools\;C:\Progra~1\Java\jdk1.6.0_45\bin
    SET APP_NAME=lcltest
    SET ANDROID_HOME=C:\Android\android-sdk
    SET APK_SDK_PLATFORM=C:\Android\android-sdk\platforms\android-8
    SET APK_PROJECT_PATH=C:\Android\Projects\androidlcl\android
    

  
I had to change mine to this on sdk version 22: 
    
    
    SET APK_BUILD_TOOLS=C:\development\android-sdk\build-tools\22.0.1
    SET PATH=C:\development\android-sdk\tools;C:\development\android-sdk\platform-tools\;C:\Program Files\Java\jdk1.7.0_79\bin;%APK_BUILD_TOOLS%
    SET APP_NAME=androidlcl
    SET ANDROID_HOME=C:\development\android-sdk
    SET APK_SDK_PLATFORM=C:\development\android-sdk\platforms\android-22
    SET APK_PROJECT_PATH=C:\Android\Projects\androidlcl\android
    
    
    and these lines:
    
    REM Convert the java bytecode to dalvik bytecode
    ::REM For older SDKs: call dx --dex --verbose --output=%APK_PROJECT_PATH%\bin\classes.dex %APK_PROJECT_PATH%\bin\classes
    ::REM For older SDKs:  call java -Djava.ext.dirs=%ANDROID_HOME%\platform-tools\lib\ -jar %ANDROID_HOME%\platform-tools\lib\dx.jar --dex --verbose --output=%APK_PROJECT_PATH%\bin\classes.dex %APK_PROJECT_PATH%\bin\classes
    call java -Djava.ext.dirs=%ANDROID_HOME%\platform-tools\lib\;%APK_BUILD_TOOLS%\lib\ -jar %APK_BUILD_TOOLS%\lib\dx.jar --dex --verbose --output=%APK_PROJECT_PATH%\bin\classes.dex %APK_PROJECT_PATH%\bin\classes
    

  
run "build_debug_apk.bat" answer at the questions and give the password "senhas" when asked 

This will generate 

C:\Android\Projects\androidlcl\android\bin\lcltest.apk [1.549 kb] 

  


  * Create an Android Virtual Device



START > AVD Manager 

New > AVD name > give a name 

Device 5.1 WVGA 

Target Android 4 API 14 

Hit OK 

Once created select the virtual device and hit "start" > "Launch" wait a couple of minutes. 

  


  * Edit with a text editor



C:\Android\Projects\androidlcl\android\adb_install.bat 

Modify like this : 
    
    
    REM Adjust these paths to yours
    SET PATH=C:\Android\android-sdk\tools;C:\Android\android-sdk\build-tools\17.0.0;C:\Android\android-sdk\platform-tools\;C:\Progra~1\Java\jdk1.6.0_45\bin
    
    adb uninstall com.pascal.lcltest
    adb install bin\lcltest.apk
    
    pause
    

Run "adb_install.bat" . This will install the "LCL Test" into your Virtual device 

  


  * Debugging



Edit with a text editor the file 

C:\Android\Projects\androidlcl\android\adb_install.bat 

like this : 
    
    
    REM Adjust these paths to yours
    SET PATH=C:\Android\android-sdk\tools;C:\Android\android-sdk\build-tools\17.0.0;C:\Android\android-sdk\platform-tools\;C:\Progra~1\Java\jdk1.6.0_45\bin
    
    adb logcat > std.txt
    
    pause
    

By running this you will get a debug file here : 

C:\Android\Projects\androidlcl\android\std.txt 

Enjoy ! 

## Links

  * [Original forum topic](<http://forum.lazarus.freepascal.org/index.php/topic,22414.msg141964.html?>)
  * [Lazarus and Android PDF tutorial with images](<http://sourceforge.net/projects/laztoapk/files/Lazarus%20and%20Android.pdf/download>)



# Linux

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Work in progress...

This is a quick tutorial on how to create android apps with Lazarus. 

In this tutorial assume used versions are Ubuntu 13.10, OpenJDK 1.7.0_51, SDK 22.2.1, ndk-r9, Laz4Android1.1-41139-FPC2.7.1 

## Prepare folder

Create directory ~\Android 

## Install OpenJDK
    
    
    sudo apt-get install openjdk-7-jdk
    

## Install SDK

Go to <http://developer.android.com/sdk/index.html>

DOWNLOAD FOR OTHER PLATFORMS > SDK Tools Only 

android-sdk_r22.6.2-linux.tgz (Recommended) 

Extract SDK () into ~\Android\android-sdk 
    
    
    tar -zxvf android-sdk_r22.6.2-linux.tgz
    

  
Now update the SDK (Start ~\Android\android-sdk\tools\android) 

add android 4.0 (API 14) 

add android 2.2 (API 8] 

Install packages. 

## Install NDK

<http://developer.android.com/tools/sdk/ndk/index.html>

Download only the file android-ndk-r9d-linux-x86.tar.bz2 

Extract android-ndk-r9d-linux-x86.tar.bz2 into ~\Android\android-ndk-r9 
    
    
    tar -jxvf android-ndk-r9d-linux-x86.tar.bz2
    

## Build FPC 2.7.1

Install latest FPC distribution package: 
    
    
    sudo apt-get install fpc
    

Download latest FPC trunk from SVN: 
    
    
    svn co http://svn.freepascal.org/svn/fpc/trunk/ ~/Android/fpcsrc
    

Build FPC trunk for ARM to ~/Android/fpc: 
    
    
    make crossinstall CPU_TARGET=arm OS_TARGET=linux \
    CROSSBINDIR=~/Android/android-ndk-r9/toolchains/arm-linux-androideabi-4.8/prebuilt/linux-x86_64/arm-linux-androideabi/bin OPT=-dFPC_ARMEL INSTALL_PREFIX=~/Android/fpc
    

  


## Rebuild Lazarus IDE

Run ~\Android\lazarus 

Select "Start IDE" 

select "Package" "Install/Uninstall Packages" 

select customdrawn 0.0 > install selection and hit save and rebuild IDE , continue 

  


## Links

  * [Android Programming with Lazarus through Custom Drawn Interface](<http://pascalgeek.blogspot.cz/2013/10/android-programming-with-lazarus.html>)

---

_Source: [https://wiki.freepascal.org/Android_tutorial](https://web.archive.org/web/20230201003035/https://wiki.freepascal.org/Android_tutorial)_
