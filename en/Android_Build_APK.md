# Android Build APK

│ **English (en)** │  **[русский (ru)](<../ru/Android_Build_APK.md>)** │

This article covers a way to compile a project in FPC and download it to an Android device. It is assumed that the project is an NDK library that is loaded in Java code. Also, this method is not tied to any IDE extension (for example Android4Laz) and can be used separately for arbitrary projects. 

## Contents

  * 1 Requirements
    * 1.1 JRE / JDK version
  * 2 Assembly
  * 3 See also



## Requirements

  * FPC (with ready-made Android rtl libraries).
  * JRE (Java-Run Time) - needed for the JDK, as well as to run the "java" machine (for executing the utilities included in the Android SDK)
  * JDK (required to install javac ), JDK will require JRE to be installed.
  * Android NDK
  * Android SDK



No additional build tools (e.g. ant) ​​are required 

### JRE / JDK version

At the moment (winter 2017) only version 8 (1.8) is officially available for download. Previously, you could download version 7. Many components of the Android SDK are tied to a specific version of the JDK. 

## Assembly

0\. It is assumed that the entire project is in a folder, for example "mydemo". And all scripts and utilities for building under android are located in "mydemo \ android". This only matters in the next step where the -FE switch is specified. 

1\. you need to compile the project (library). The command line (windows) might look like this. 
    
    
    C:\FPC\3.1.1\bin\i386-win32\fpc.exe ^
    -Tandroid -Parm -MDelphi ^
    -Scghi -CX -O3 -Xs -XX -l -vbq ^
    -FUlib\arm-android ^
    -oandroid\mydemo.so ^
    -FE.\Android\files\lib\armeabi\ ^
    -CpARMv6 ^ 
    -CfVFPv2 ^
    -Xd ^
    -XParm-linux-androideabi- ^
    -FDC:\android\ndk-r11c\toolchains\arm-linux-androideabi-4.9\prebuilt\windows\bin ^
    -FuC:\android\ndk-r11c\platforms\android-9\arch-arm\usr\lib ^
    mydemo.lpr
    

What you should pay attention to. Keys: 

  * -FD - specifies the path where build utilities are stored
  * -XParm-linux-androideabi- - specifies the prefix for the build utilities
  * -Fu - pointing to NDK libraries - required for linking
  * -FE - specifies the path where the created library should be written. The destination directory in this case is listed as "lib \ armeabi". This path will be saved in the apk itself. It should be just that, because it is tied to the target architecture (arm). For arm64 or arm7, the path must be different. All third-party dynamic link libraries for ARM architecture should also be located in the lib \ armeabi folder.



2\. Prepare the AndroidManifest.xml manifest file 

3\. Create the .apk file 
    
    
    set buildtoolsdir=C:\android\sdk-windows\build-tools\23.0.3
    set APP_NAME=testapp
    set APK_SDK_PLATFORM=C:\android\sdk-windows\platforms\android-11
    rem -S res - fails, if "res" directory doesn't exist
    rem generate-dependencies is needed to generate R.java file
    
    
    
    %buildtoolsdir%\aapt p -v -f -M AndroidManifest.xml ^
     -F bin\%APP_NAME% .ap_ ^
     --generate-dependencies ^
     -I%APK_SDK_PLATFORM%\android.jar -S res -m -J gen ^
     files
    

The utility creates an initial .apk file (-F bin\%APP_NAME% .ap_ - the extension is specified as "ap_" intentionally, since this is not a final file), as well as a R.java file in the "gen" folder (-J gen). When creating an apk (essentially a .zip) file, the "files" folder will be included. (at step # 1, the library "-FE.\android\files\lib\armeabi\" was copied into it, so "lib\armeabi\mydemo.so" will get into the apk file) "-S" indicates the folder with standard android resources (icons and layout xml-s), the folder will be copied to .apk The structure of the generated .apk file will look like this: 
    
    
    AndroidManifest.xml 
    resources.arsc
    res
    lib
    lib\armeabi
    ... all other folders/files that were located in "files"
    

If the program has any resource files, they must be added to "files" immediately BEFORE calling "aapt". It is not necessary to copy them to "files" either (as well as to use the "files" folder). you can specify several source folders, for example: 
    
    
    %buildtoolsdir%\aapt ... ^
      dir1 dir2 dir3
    
    

4\. Compile Java code of the main program 
    
    
    set jdkbindir=C:\Program Files\Java\jdk1.7.0_25\bin
    set androidjar=C:\android\sdk-windows\platforms\android-11\android.jar 
    
    
    
    "%jdkbindir%\javac.exe" ^
      .\zengl\android\ZenGL.java ^
      .\zengl\demo01\Demo01Activity.java ^
      -cp "%androidjar%" ^
      -d.\classes
    

It is important to specify "-cp" (class-path) for the used Android-SDK. The java compiler, like jdk itself, does not know anything about the android system. And the compiled program will use the SDK classes. Therefore, if the class-path is not specified, then errors will appear that the class is not declared, etc. -d - specifies the path where to put the compiled .class files, they are needed in the next step. 

In a further step, the compiled code must be converted to Dalvik. It is possible that version "dx.jar" is expected for classes version 1.7, not 1.8 (which you can use). In this case, you need javac to specify the compatibility version: 
    
    
    set jdkbindir= C:\Program Files\Java\jdk1.8.0_25\bin
    set androidjar=C:\android\sdk-windows\platforms\android-11\android.jar 
    
    
    
    "%jdkbindir%\javac.exe" ^
      .\zengl\android\ZenGL.java ^
      .\zengl\demo01\Demo01Activity.java ^
      -cp "%androidjar%" ^
      -d.\classes -target 1.7 -source 1.7
    

5\. Convert Java code to dalvik 
    
    
    SET DX_PATH=C:\android\sdk-windows\build-tools\23.0.3\lib
    java -Djava.ext.dirs=%DX_PATH%\ -jar %DX_PATH%\dx.jar --dex --verbose --output=.\bin\classes.dex .\classes
    

Converting java class files to dalwick files. 

6\. Create an unsigned .apk file by adding dalvik class files 
    
    
    set jdkdir=C:\Program Files\Java\jdk1.7.0_25\bin
    set ANDROID_HOME=C:\android\sdk-windows
    set APK_PROJECT_PATH=.
    set APP_NAME=testapp
    
    
    
    del %APK_PROJECT_PATH%\bin\%APP_NAME%-unsigned.apk
    "%jdkdir%\java" -classpath %ANDROID_HOME%\tools\lib\sdklib.jar ^
     com.android.sdklib.build.ApkBuilderMain ^
     %APK_PROJECT_PATH%\bin\%APP_NAME%-unsigned.apk -v -u ^
     -z %APK_PROJECT_PATH%\bin\%APP_NAME%.ap_ ^
     -f %APK_PROJECT_PATH%\bin\classes.dex
    

At the time of writing the script, there was no separate utility for adding classes.dex to the .apk file ... But here ApkBuilderMain is called (via a call to the java machine). The only thing it does is add classes.dex (created in step # 5) to the root folder of the existing .apk file (created in step # 3 .ap_) and write it to the -unsigned.apk file. A regular zip utility (like 7z) would probably do just as well. 

7\. Sign the .apk file. Attention! this script generates a new self-signed key every time. The program downloaded on Google Play must use the real key (this is also important for further program updates) 
    
    
    set jdkbindir=C:\Program Files\Java\jdk1.7.0_25\bin
    set APP_NAME=testapp
    
    
    
    REM Generating on the fly a debug key
    "%jdkbindir%\keytool" -genkeypair -v -keystore bin\LCLDebugBKKey.keystore -alias LCLDebugBKKey ^
     -keyalg RSA -validity 10000 ^
     -dname "cn=Havefunsoft, o=company, c=US" ^
     -storepass "123456" -keypass "123456" -keysize 2048
    
    
    
    REM Signing the APK with a debug key
    del bin\%APP_NAME%-unaligned.apk
    "%jdkbindir%\jarsigner" -verbose ^
      -sigalg SHA1withRSA ^
      -digestalg SHA1 ^
      -keystore bin\LCLDebugBKKey.keystore -keypass 123456 -storepass 123456 ^
      -signedjar bin\%APP_NAME%-unaligned.apk bin\%APP_NAME%-unsigned.apk LCLDebugBKKey
    

7.1 signature verification. (optional step) 
    
    
    set jdkbindir=C:\Program Files\Java\jdk1.7.0_25\bin
    set APP_NAME=testapp
    
    
    
    REM Signing the APK with a debug key
    "%jdkbindir%\jarsigner" -verify -verbose -certs bin\%APP_NAME%-unaligned.apk
    

8\. Align the resulting .apk. (Alignment should occur AFTER the signature, since the signature makes the apk file unaligned) 
    
    
    set APP_NAME=testapp
    set buildtoolsdir=C:\android\sdk-windows\build-tools\23.0.3
    
    
    
    REM Align the final APK package
    %buildtoolsdir%\zipalign -v 4 bin\%APP_NAME%-unaligned.apk bin\%APP_NAME%.apk
    

On completion of the 8th step, the resulting .apk file can be downloaded directly to the device. 

9\. Downloading any .apk file can be downloaded to the device if it is connected via USB and developer rights are allowed on it. The SDK includes an adb utility that allows you to uninstall the application and install a new .apk. (If the application is already installed, then the new .apk will not be loaded, and you must first uninstall it using uninstall) 
    
    
    C:\android\sdk-windows\platform-tools\adb.exe uninstall zengl.demo01
    C:\android\sdk-windows\platform-tools\adb.exe install bin\testapp.apk
    

## See also

  * [Cross compiler instructions](<Android.md> "Android") for building cross compiler on Android.

---

_Source: [https://wiki.freepascal.org/Android_Build_APK](https://web.archive.org/web/20240425021556/https://wiki.freepascal.org/Android_Build_APK)_
