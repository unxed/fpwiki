# Android Programming

From Free Pascal wiki

(Redirected from [Android Interface/Android Programming](</index.php?title=Android_Interface/Android_Programming&redirect=no> "Android Interface/Android Programming"))

****English (en)**** | [**日本語 (ja)**](</Android_Programming/ja> "Android Programming/ja") | [**한국어 (ko)**](</Android_Programming/ko> "Android Programming/ko") | [**Русский (ru)**](<../../ru/Android_Programming.md> "Android Programming/ru") | [**‪中文(中国大陆)‬ (zh_CN)**](</Android_Programming/zh_CN> "Android Programming/zh CN")

See also [Custom Drawn Interface/Android](<../Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")

## Contents

  * 1 How to...
    * 1.1 Build the NDK OpenGL example
    * 1.2 Read the screen metrics
  * 2 Details about Android devices
    * 2.1 Smartphones
    * 2.2 Tablets
    * 2.3 List of devices ordered by processor compatibility (ARMv6 vs ARMv7)
    * 2.4 Playing sounds and videos on the phones
  * 3 Android file system
  * 4 Creating a new Java Android application
    * 4.1 Showing/hiding the virtual keyboard
  * 5 Android JNI
    * 5.1 Table of Java types and Pascal JNI types
    * 5.2 Example 1: Creating a Java object and calling a Java method
    * 5.3 Example 2: Calling a static method
  * 6 References

  
---  
  
  
Knowing general Android programming can be very useful to help develop the Lazarus Android Interface. 

##  How to...

###  Build the NDK OpenGL example

Just follow these steps: 

**Step 1** \- Download and install the Android NDK, Android SDK and Ant. More information here: [Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones](<Using_the_Android_SDK,_Emulator_and_Phones.md> "Android Interface/Using the Android SDK, Emulator and Phones")

**Step 2** \- Install the pre-compiled FPC cross-compiler. Instructions here: [Android_Interface#Using_the_pre-compiled_compiler](<../Android_Interface.md> "Android Interface")

**Step 3** \- Download the latest lazarus-ccr sourceforge code: 
    
    
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr lazarus-ccr

or if you think it is too big you can download just the folder lazarus-ccr/bindings/android-ndk 

**Step 4** \- Building the Pascal Library 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** In the steps below, the directory /home/felipe/Programas or ~/Programas is used. Please adjust if your paths are different.

Open the project lazarus-ccr/bindings/android-ndk/examples/opengltest/opengltest.lpi 

Go to Project->Project Options->Paths and where is written "Libraries -Fl" you should see a value like this: 
    
    
    /home/felipe/Programas/android-ndk-r5/platforms/android-9/arch-arm/usr/lib
    

Change it to the correct path in your system which points to the NDK and to the library folder for your minimal required Android API Level 

Now build the project using Lazarus. 

**Step 5** \- Configuring the build environment 

Open the file opengltest/local.properties. Change this line to point to your SDK location: 
    
    
    sdk.dir=/home/felipe/Programas/android-sdk-linux
    

**Step 6** \- Buildings the APK 

Open a terminal and type these commands: 
    
    
    cd lazarus-ccr/bindings/ndk/examples/opengltest/android
    ant debug

The APK file will be placed in opengltest/android/bin/ 

**Step 7** \- Install the APK 

In this step, if you get error messages about permissions read: [Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones#Recognition_of_devices_under_Linux](<Using_the_Android_SDK,_Emulator_and_Phones.md> "Android Interface/Using the Android SDK, Emulator and Phones")

The command to install our APK in the phone is: 
    
    
    cd opengltest/android
    ~/Programas/android-sdk-linux/platform-tools/adb install bin/OpenGLNDKTest-debug.apk

You should see something like: 
    
    
    2395 KB/s (107299 bytes in 0.043s)
           pkg: /data/local/tmp/OpenGLNDKTest-debug.apk
    Success
    

If you get an error message that it is already installed you can install with this command: 
    
    
    ~/Programas/android-sdk-linux/platform-tools/adb uninstall com.pascal.opengltest

And then you can run adb logcat to see what the log says while you start it in the phone via its newly added icon: 
    
    
    ~/Programas/android-sdk-linux/platform-tools/adb logcat

###  Read the screen metrics

First fill a TDisplayMetrics with the correct info: 
    
    
    uses androidutil;
     
    var
      MyDisplayMetrics: TDisplayMetrics;
      Str: string;
      //
      lHeight, lWidth: Integer;
      xdpi, ydpi, lScreenSize: Single;
    begin
      // ..
      // Objects
     
      MyDisplayMetrics := TDisplayMetrics.Create;
      Activity.getWindowManager().getDefaultDisplay().getMetrics(MyDisplayMetrics);

And then you can read it from the TDisplayMetrics: 
    
    
      lHeight := MyDisplayMetrics.heightPixels();
      lWidth := MyDisplayMetrics.widthPixels();
      xdpi := MyDisplayMetrics.xdpi();
      ydpi := MyDisplayMetrics.ydpi();
      lScreenSize := sqrt(sqr(lHeight / ydpi) + sqr(lWidth / xdpi));
      ldensity := MyDisplayMetrics.density();
      ldensityDpi := MyDisplayMetrics.densityDpi();
      scaledDensity := MyDisplayMetrics.scaledDensity();

Note that lots of devices lie about the xdpi and ydpi, so don't trust the lScreenSize calculated above. Smartphones might report even 10 inches of screen size, while the correct is around 4. 

##  Details about Android devices

Since there are so many Android devices, it can be useful to keep track of some information about them. See also in the wikipedia Comparison of Android Devices [[1]](<http://en.wikipedia.org/wiki/Comparison_of_Android_devices>) and details about the processors in [[2]](<https://wiki.mozilla.org/Mobile/Platforms/Android#System_Requirements>). 

###  Smartphones

Manufacturer  |  Model  |  Android API Name (Build.Model)  |  Processor  |  Android version  |  Multi-touch  |  Comments   
---|---|---|---|---|---|---  
HTC | Wildfire | HTC Wildfire | armv6 | 2.1=>2.2 | Yes | \-   
Sony Erricson | Xperia X10 | X10i | armv7 | 1.6=>2.1 | No | \-   
  
###  Tablets

Manufacturer  |  Model  |  Android API Name (Build.Model)  |  Processor  |  Android version  |  Multi-touch  |  Comments   
---|---|---|---|---|---|---  
Toshiba | Folio 100 Tablet | TOSHIBA_FOLIO_AND_A | armv7 | 2.2 | Yes | \-   
  
###  List of devices ordered by processor compatibility (ARMv6 vs ARMv7)

**List of ARMv7 devices:**

  * Advent Vega (P10AN01) 
  * Dell Streak, Streak 7 
  * HTC Desire 
  * HTC Desire Z (T-Mobile G2) 
  * HTC Desire HD 
  * HTC Droid Incredible 
  * HTC EVO 4G, EVO Shift 4G 
  * HTC Glacier (T-Mobile myTouch 4G) 
  * HTC Inspire 4G 
  * HTC Nexus One 
  * HTC Thunderbolt 4G 
  * Huawei Ideos S7 
  * LG Optimus Z 
  * Motorola Atrix 4G 
  * Motorola Bravo 
  * Motorola Cliq 2 - untested 
  * Motorola Defy 
  * Motorola Droid 2, Droid 2 Global 
  * Motorola Droid Pro (Motorola PRO) 
  * Motorola Droid X 
  * Motorola Xoom 
  * POV Mobii Tegra Tablet 
  * Samsung Continuum (i400) 
  * Samsung Galaxy S (i9000, Captivate, Fascinate, Vibrant, Epic 4G) 
  * Samsung Galaxy Tab 
  * Sharp IS03 
  * Sony Ericsson Xperia X10 
  * Toshiba AS100 
  * Viewsonic gTablet 
  * Acer Liquid E 
  * Acer Liquid (Liquid A1) 
  * Archos 101 Internet Tablet 
  * Motorola Charm 
  * Motorola Droid (Milestone) 
  * Samsung Galaxy S 4G 
  * Samsung Nexus S 



**List of ARMv6 processors:**

  * Android SDK emulator 
  * Asus Garmin nuvifone A50 (T-Mobile Garminfone) 
  * Augen GENTouch 78 Tablet 
  * Coby Kyros Internet Tablet (MID7015) 
  * Geeksphone One, Geeksphone Zero 
  * HTC Aria 
  * HTC ChaCha 
  * HTC Dream (T-Mobile G1, Android Dev Phone 1) 
  * HTC Droid Eris 
  * HTC Espresso (T-Mobile myTouch 3G Slide) 
  * HTC Hero (T-Mobile G2 Touch) 
  * HTC Legend 
  * HTC Magic (T-Mobile myTouch 3G, T-Mobile G1 Touch) 
  * HTC Salsa 
  * HTC Tattoo 
  * HTC Wildfire 
  * Huawei Ascend 
  * Huawei Ideos U8150-B (T-Mobile Comet) 
  * Huawei U8110 (T-Mobile Pulse Mini) 
  * Huawei U8230 
  * LG Ally (Apex) (LG VS740) 
  * LG GW620 (Eve, InTouch Max, LinkMe) 
  * LG Optimus, Optimus M, Optimus T, Optimus S, Optimus V 
  * LG Vortex 
  * LG P500 
  * MAG iMiTO iM7 
  * vMAG iMiTO iM7S 
  * Motorola Backflip 
  * Motorola Citrus 
  * Motorola Cliq (MB200) 
  * Motorola Dext 
  * Motorola Devour 
  * Motorola i1 
  * Motorola Spice XT300 
  * Motorola Quench XT5 XT502 
  * Pandigital Novel 
  * Samsung GT-S5570 Galaxy Mini 
  * Samsung i5500 Galaxy 5 (Corby) 
  * Samsung i5700 Galaxy Portal (Spica) 
  * Samsung i5800 Galaxy 3 
  * Samsung i7500 Galaxy 
  * Samsung Intercept 
  * Samsung M900 Moment 
  * Samsung S5830 Galaxy Ace 
  * Samsung Transform 
  * Sanyo ZIO M6000 
  * Sony Ericsson Xperia X8 
  * Sony Ericsson Xperia X10 Mini 
  * Sony Ericsson Xperia X10 Mini Pro 
  * Superpad 10.2" Tablet PC 
  * Viewsonic ViewPad 7 Tablet 
  * Velocity Micro T103 Cruz tablet 
  * Vodafone 845 
  * ZTE Blade 



###  Playing sounds and videos on the phones

Each phone comes with different codecs installed, which means that the native Media Player will be able to play different formats in different phones. Below is a table showing which formats play in the native Media Player of each phone: 

Telephone  |  Android  |  webm - VP8  |  m4v - H.264  |  ogv - Theora  |  mp4 - H.264  |  mov - H.264  |  avi - RLE  |  mpg - MPEG-1  |  wmv - WM9  |  3gp - MPEG-4   
---|---|---|---|---|---|---|---|---|---|---  
Emulator | 1.6 | x | x | x | x | x | x | x | x | OK   
Emulator | 2.1 | x | x | x | x | x | x | x | x | OK   
Nexus One | 2.2 | x | OK | x | OK | x | x | x | x | OK   
HTC Desire | 1.6 | x | OK | x | OK | x | x | x | OK | OK   
Toshiba Folio 100 | 2.2 | x | OK | x | OK | x | x | x | OK | OK   
Xperia X10 | 2.1 | x | OK | x | x | x | x | x | OK | OK   
Xperia X8 | 2.2 | x | OK | x | x | x | x | x | x | OK   
HTC Wildfire | 2.1 | x | OK | x | x | x | x | x | x | OK   
HTC Desire HD | 2.2 | x | OK | x | OK | x | x | x | x | OK   
Galaxy Tab | 2.2 | x | OK | x | x | x | x | x | x | OK   
myPhone A210 | 1.6 | x | OK | x | x | x | x | x | x | OK   
Motorola Milestone | 2.1 | x | OK | x | x | x | x | x | OK | ?   
Milestone 2 | 2.2 | x | OK | x | x | x | x | x | sound-only | OK   
  
##  Android file system

Android recommends to use getApplicationContext().getFilesDir() to obtain the data directory for the application and this routine in my HTC Wildfire returns /data/data/<package name>

The native libraries of the application are placed in /data/data/<package>/lib 

A good place where we have read and write access and we can create new files and even run files is /data/data/<package name>

##  Creating a new Java Android application

This info can be useful for helping implement LCL-CustomDrawn-Android 

Generic instructions here: <http://developer.android.com/guide/developing/other-ide.html>

###  Showing/hiding the virtual keyboard

See here: <http://android-codes-examples.blogspot.com/2011/11/show-or-hide-soft-keyboard-on-opening.html>

To show it: 
    
    
    InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
    imm.toggleSoftInput(InputMethodManager.SHOW_FORCED,0);

and for hiding the keyboard: 
    
    
    InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
    imm.hideSoftInputFromWindow(singleedittext.getWindowToken(),0);

##  Android JNI

Using JNI (Java Native Interface) you can call most Android SDK APIs and also other Java libraries. 

###  Table of Java types and Pascal JNI types

All of the Java types except interfaces can be represented in JNI without problems. For each Java type there is a corresponding VM Type Signature which is utilized in routines such as javaEnvRef^^.GetMethodID. There is also the JNI native type, which is the type declared in the unit lazarus/lcl/interfaces/customdrawn/android/jni.pas and should be the type utilized in JNI code written in Pascal. The Pascal equivalent is provided just for comparison, as one should use the types from jni.pas when writing JNI code. 

Java type  |  VM signature  |  Type in jni.pas  |  Pascal equivalent  |  Description   
---|---|---|---|---  
boolean | Z | jboolean | Boolean |   
byte | B | jbyte | Byte |   
char | C | jchar | Word |   
short | S | jshort | Shortint |   
int | I | jint | Integer |   
long | J | jlong | Int64 |   
float | F | jfloat | Single |   
double | D | jdouble | Double |   
a class, ie: String or InputStream | L<full class name> ie: Ljava/lang/String; | jobject | N/A |   
Array, ie: int[] | [<type-name> ie: [I | N/A |   
A method type | (arg-types)ref-type | JMethodID | N/A |   
void | V | N/A | procedure |   
  
###  Example 1: Creating a Java object and calling a Java method

Let's say that you want to make Pascal JNI code equivalent to the following Java code: 
    
    
      HttpGet javaRequest = new HttpGet();
      URI myURI = new URI("http://magnifier.sourceforge.net/");
      javaRequest.setURI(myURI);

First you need to get all Class IDs involved in the process, then get all method IDs which we will use and then make the calls. If the call has parameters, then pass it in an array. The following Pascal native code is equivalent to that Java code above: 
    
    
    uses jni, customdrawnint;
     
    var
      javaClass_HttpGet, javaClass_URI: jclass;
      javaMethod_HttpGet_new, javaMethod_HttpGet_setURI,
        javaMethod_URI_new: jmethodid;
      javaRequest, javaURI: jobject;
      javaString: jstring;
      lParams: array[0..2] of JValue;
      lNativeString: PChar;
    begin
      DebugLn(':>LoadHTMLPageViaJNI');
      // First call FindClass for all required classes
      javaClass_HttpGet := javaEnvRef^^.FindClass(javaEnvRef,
        'org/apache/http/client/methods/HttpGet');
      javaClass_URI := javaEnvRef^^.FindClass(javaEnvRef,
        'java/net/URI');
     
      // Now all Method IDs
      DebugLn(':LoadHTMLPageViaJNI 1');
      javaMethod_HttpGet_new := javaEnvRef^^.GetMethodID(javaEnvRef, javaClass_HttpGet, '<init>', '()V');
      javaMethod_HttpGet_setURI := javaEnvRef^^.GetMethodID(javaEnvRef, javaClass_HttpGet, 'setURI', '(Ljava/net/URI;)V');
      javaMethod_URI_new := javaEnvRef^^.GetMethodID(javaEnvRef, javaClass_URI, '<init>', '(Ljava/lang/String;)V');
     
      // Create a new instance for the HTTP request object
      // HttpGet javaRequest = new HttpGet();
      javaRequest := javaEnvRef^^.NewObject(javaEnvRef,
        javaClass_HttpGet,
        javaMethod_HttpGet_new);
     
      DebugLn(':LoadHTMLPageViaJNI 4 javaRequest='+IntToHex(PtrInt(javaRequest), 8));
      // Add a URI for the request object
      // URI javaURI = new URI("http://magnifier.sourceforge.net/");
      lParams[0].l := javaEnvRef^^.NewStringUTF(javaEnvRef, 'http://magnifier.sourceforge.net');
      javaURI := javaEnvRef^^.NewObjectA(javaEnvRef,
        javaClass_URI, javaMethod_URI_new, @lParams[0]);
      javaEnvRef^^.DeleteLocalRef(javaEnvRef, lParams[0].l);
      // javaRequest.setURI(javaURI);
      lParams[0].l := javaURI;
      javaEnvRef^^.CallVoidMethodA(javaEnvRef, javaRequest,
        javaMethod_HttpGet_setURI, @lParams[0]);

###  Example 2: Calling a static method

Let's say that you want to call the following static method located in the Bass class: <http://jerome.jouvie.free.fr/nativebass/javadoc/org/jouvieje/bass/Bass.html>
    
    
      public static native int BASS_StreamCreateFile(String file, long offset, long length, int flags);

First you need to get all Class IDs involved in the process, then get the static method ID and then make the call with the parameter in the array: 
    
    
    uses jni, customdrawnint;
     
    var
      javaClass_Bass: jclass;
      javaMethod_Bass_BASS_StreamCreateFile: jmethodid;
      javaString: jstring;
      lParams: array[0..3] of JValue;
      lRetInt: jint;
    begin
      // First call FindClass for all required classes
      javaClass_Bass := javaEnvRef^^.FindClass(javaEnvRef, 'org/jouvieje/bass/Bass');
     
      // Now all Method IDs
      javaMethod_Bass_BASS_StreamCreateFile := javaEnvRef^^.GetStaticMethodID(javaEnvRef, javaClass_Bass, 'BASS_StreamCreateFile', '(Ljava/lang/String;JJI)I');
     
      // Make the call
      lParams[0].l := javaEnvRef^^.NewStringUTF(javaEnvRef, '/path/to/file');
      lParams[1].j := 0; // offset
      lParams[2].j := 200; // length
      lParams[3].i := 0; // flags
      lRetInt := javaEnvRef^^.CallStaticIntMethodA(javaEnvRef, javaClass_Bass, javaMethod_Bass_BASS_StreamCreateFile, @lParams[0]);
      javaEnvRef^^.DeleteLocalRef(javaEnvRef, lParams[0].l);

##  References

  * <http://developer.android.com/guide/developing/other-ide.html>
  * [Tutorial "Lazarus and Android"](<https://dl.dropboxusercontent.com/u/3753548/Lazarus%20and%20Android.pdf>) (PDF file)

---

_Source: [https://wiki.freepascal.org/Android_Interface/Android_Programming](https://web.archive.org/web/20140925104701/https://wiki.freepascal.org/Android_Interface/Android_Programming)_
