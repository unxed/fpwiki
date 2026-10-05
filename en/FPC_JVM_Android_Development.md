# FPC JVM Android Development

[![Android robot.svg](https://wiki.freepascal.org/images/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

This article applies to [Android](</Category:Android> "Category:Android") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page gives you an introduction for developing Android applications using the [JVM Port](<FPC_JVM.md> "FPC JVM") of the FPC compiler. For native development for ARM/Android, please see [here](<Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android"). 

## Contents

  * 1 Prerequisites
  * 2 Using Eclipse
  * 3 Using Lazarus
    * 3.1 Preparations
      * 3.1.1 Patching Lazarus
      * 3.1.2 Patching FPC
      * 3.1.3 Generate a Key Store
    * 3.2 Structure of an Android application
    * 3.3 Building an Android application
      * 3.3.1 Summary
      * 3.3.2 Used Directories
      * 3.3.3 Directory layout
      * 3.3.4 The manifest file
      * 3.3.5 The resource files
      * 3.3.6 Generate the resource package
      * 3.3.7 The resource identifiers
      * 3.3.8 Compiling the source
      * 3.3.9 Generating the package
      * 3.3.10 Deploying the package
    * 3.4 Finding errors



# Prerequisites

You need to have the [Android SDK](<http://developer.android.com/sdk/index.html>) installed (at least version 10 should be sufficient). For more information about installing the SDK please refer to the [Android sites](<http://developer.android.com/sdk/installing.html>). 

You also need the JDK (either the Oracle one or OpenJDK) and the JRE. To be more precise: the applications _jarsigner_ and _java_ must be available and depending on the method you choose below _javac_ needs to be available as well. 

At last you also need the FPC JVM compiler and the Jasmin assembler for which you should follow the guidelines written [here](<FPC_JVM.md> "FPC JVM"). 

# Using Eclipse

See <https://macpgmr.github.io/pba/PbaStatus.html>

# Using Lazarus

## Preparations

You should use a trunk version of Lazarus as this already contains a few extensions for language features used and implemented in the JVM port. 

Also to simplyfy things a bit you might want to patch Lazarus and the compiler driver "fpc": 

### Patching Lazarus

Apply the following patch to add support for the _jvm_ platform and the _android_ and _arm_ operating systems (this patch is not yet reported, because first the JVM port should be merged back into FPC's trunk): 
    
    
    --- ide/frames/compiler_codegen_options.pas	2011-12-24 17:43:38.000000000 +0000
    +++ /mnt/data/apps/lazarus/0.9.31/ide/frames/compiler_codegen_options.pas	2011-12-25 18:22:32.593788084+0000
    @@ -166,6 +166,8 @@
           Add('morphos');
           Add('embedded');
           Add('symbian');
    +      Add('java');
    +      Add('android');
         end;
         ItemIndex := 0;
       end;
    @@ -183,6 +185,7 @@
           Add('powerpc');
           Add('sparc');
           Add('x86_64');
    +      Add('jvm');
         end;
         ItemIndex := 0;
       end;
    

### Patching FPC

While every platform in Free Pascal has it's own compiler binary (e.g. _ppc386_ , _ppcarm_ , etc.) there is the binary _fpc_ which selects the correct compiler by interpreting the _-Pxxx_ argument. To enable support for the _jvm_ platform the source code of the _fpc_ binary needs to be adjusted. For this you can use any source for this binary that's younger than February 2010 (as this is where the last change was made). You can also simply download the source code file from FPC's trunk ([here](<http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/compiler/utils/fpc.pp?view=markup>)) for this and compile that as there are no dependencies besides unit _SysUtils_. 

Here is the patch: 
    
    
    Index: compiler/utils/fpc.pp
    ===================================================================
    --- compiler/utils/fpc.pp	(Revision 19886)
    +++ compiler/utils/fpc.pp	(Arbeitskopie)
    @@ -214,7 +214,9 @@
                                  cpusuffix:='ia64'
                                else if processorstr='x86_64' then
                                  cpusuffix:='x64'
    -                           else
    +                           else if processorstr='jvm' then
    +                             cpusuffix:='jvm'
    +                           else                           
                                  error('Illegal processor type "'+processorstr+'"');
     
     {$ifndef darwin}
    

After you compiled the _fpc_ binary you can replace your old one with it and copy the _ppcjvm_ to the directory where all other compilers are located. These are for the different platforms: 

  * Windows: _%fpcdir%\bin\i386-win32\_ (or _x86_64-win64_ if you have a native 64-Bit installation) 
  * Linux: _$fpcdir/lib/fpc/$fpcversion/_ (depending on your installation _$fpcdir_ could be _/_ , _/usr_ or some directory in your home directory) 
  * Mac OS X: **WRITE ME**



Also you should copy the compiled units from your FPC JVM snapshot to the corresponding _units_ directory of your installation: 

  * Windows: from _%fpcjvm%\units\jvm-android_ to _%fpcdir%\units\jvm-android_
  * Linux: from _$fpcjvm/units/jvm-android_ to _$fpcdir/lib/fpc/$fpcversion/units/jvm-android_
  * Mac OS X: **WRITE ME**



Also if you don't want to copy the _jasmin.jar_ to the same directory as the _ppcjvm_ , you should adjust your _fpc.cfg_ compiler by adding the following lines: 
    
    
    #IFDEF CPUJVM
    -FD{Directory of jasmin.jar}
    #ENDIF
    

### Generate a Key Store

Only signed Android packages can be deployed on any Android device (including the emulator) without exception. The only difference to Windows Phone and iOS is that it is sufficient if you sign them yourself (even if you deploy them to the Android market). This is done by creating a keystore which is later utilized by the _jarsigner_ tool to sign the package. 

Using the following command you can create such a key store if you haven't one yet: 
    
    
    keytool -genkey -v -keystore /path/to/YourKeyStore.keystore -alias YourAlias -keyalg RSA -validity 10000

You can freely chose the location and name of the keystore file as well as the alias name. But you need both of the to sign an Android package. The tool will then ask you some questions and for the passwords for keystore and alias (you can use the same password for both or different ones). 

_Note:_ You should also follow the guidelines written on the [Android developer sites](<http://developer.android.com/guide/publishing/app-signing.html>) so that your application might be approved for the Android market. 

## Structure of an Android application

An Android applications consists of the source code of the applications, some resources like images and XML files which describe the layout of the GUI (similar to LFMs) and a manifest file which describes options like the minimum SDK version or which activity is the main one. 

This leads to another difference to e.g. Lazarus applications: an Android applications consists of at least one so called _Activity_ , which can be thought of as a combination of Lazarus' _TForm_ and _TApplication_. Every activity that can be started must be defined in the manifest file (otherwise an exception occurs if one tries to start that activity). Activities are started using so called _intents_ which tell the activity why they are started and are also used to send parameters to the activity. Thus the main project file of an Android is not a program file, but an unit which will contain at least one class that derives from _android.app.Activity_ or one of its subclasses (like _android.app.ListActivity_). An activity then consists of a view which contain - depending on their class - one or more additional views which all together make up the GUI of an Android application. These views can either be instantiated at runtime by creating the view classes or by _inflating_ a layout that is part of the resources. Handling events (e.g. reacting on the click of a button) is always done by specifying a class instance that implements the corresponding listener interface (Java does not support procedure/method variables). 

## Building an Android application

### Summary

For building an Android applications multiple tools are necessary. First the resources need to be converted to binaries and added to a "resource package". Also the IDs they have in this resource package need to be added to a resource class (usally called _R_). Then the source code needs to be compiled (in our case using the FPC JVM compiler). After that the generated *.class files need to be converted to the Dalvik's DEX format and combined with the resource package and the manifest these build a unsigned package (extension APK). This package then needs to be signed (using _jarsigner_) and can then be installed on the emulator or a device or (hopefully) be deployed to the Android market. 

### Used Directories

To simplyfy the given commandlines the following conventions are used: 

  * _$platform_ gives the _platforms/android-VERSION/_ directory of the Android SDK 
  * _$platformtools_ gives the _platform-tools_ directory of the Android SDK 
  * _$tools_ gives the _tools_ directory of the Android SDK 



### Directory layout

To avoid clutter in the project's directory the following directory layout is suggested: 
    
    
    -\
     |- bin (will contain the package files and the *.dex file)
     |  |
     |  \- classes (contains the compiled *.class files and FPC's PPU files
     |
     |- gen (this will contain the generated R.java file for the resources)
     |
     |- res (here the resources will be located)
     |  |
     |  |- drawable(-hdpi/-ldpi/-mdpi) (directories containing icons, images, etc)
     |  |
     |  |- layout (layouts for the GUI)
     |  |
     |  |- menu (layouts for the menus (context menu, options menu))
     |  |
     |  \- values (containing for example strings used by the application)
     |
     \- src (the source code of your application)
    

### The manifest file

A basic manifest file looks like the following: 
    
    
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="org.myname.myapp"
          android:versionCode="1"
          android:versionName="1.0">
        <uses-sdk 
            android:minSdkVersion="9"
            android:targetSdkVersion="9" />
        <application android:label="@string/app_name" android:icon="@drawable/icon">
            <activity android:name="TMyAppActivity"
                      android:label="@string/app_name">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
            <activity android:name="TSomeOtherActivity" />
        </application>
    </manifest>

The attribute _package_ gives the class path of your application. This will be used by the generation of the _R.java_ and you should also use this at least for the _$namespace_ option in units containing activities. The attributes containing "@" are references to resources in the _res_ directory. Hereby the _drawable_ is replaced by _drawable-mdpi_ , _drawable-hdpi_ or _drawable-ldpi_ depending of the screen size at runtime. The value _@string/app_name_ refers to the string _app_name_ in the _res/values/strings.xml_ file. _TMyAppActivity_ is the main activity which will be invoked when started by the launcher. _TSomeOtherActivity_ is for example some second application window. Both need to derive from _android.app.Activity_ or one of its subclasses. Having _targetSdkVersion_ and _minSdkVersion_ set will avoid that compatibilty modules are used if you have a newer SDK version installed as well. 

### The resource files

The exact contents of your resource directory will depend on your application, but some generic advices can be given: 

The _drawable_ directories can contain a _icon.png_ in different sizes (ldpi: 36x36, mdpi: 48x48, hdpi: 72x72). 

For the contents of the layouts in the _menu_ and _layout_ directories I advice you to read the [Android documentation](<http://developer.android.com/guide/topics/ui/declaring-layout.html>). 

The _values_ directory contains a _strings.xml_ which looks like the following: 
    
    
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <string name="app_name">My App</string>
    </resources>

### Generate the resource package

To generate the resource package and the R.java you can use the following command: 
    
    
    $platformtools/aapt p -f -M AndroidManifest.xml -F bin/myapp.ap_ -I $platform/android.jar -S res -m -J gen

This will take all resources from the directory given by _-S_ , the manifest file given by _-M_ and the _android.jar_ given by _-I_ to generate the resource package given by _-F_ and the _R.java_ of which the base directory is given by _-J_ (the remaining class path is extracted from the manifest). 

### The resource identifiers

The generated _R.java_ file (located in _gen/org/myname/myapp_ according to above shown manifest file) will look like the following (supposing that there is a layout for a dialog in _res/layouts_ , two layouts for menus in _res/menu_ and two more strings in _res/values/strings.xml_): 
    
    
    /* AUTO-GENERATED FILE.  DO NOT MODIFY.
    *
    * This class was automatically generated by the
    * aapt tool from the resource data it found.  It
    * should not be modified by hand.
    */
     
    package org.myname.myapp;
     
    public final class R {
        public static final class attr {
        }
        public static final class drawable {
            public static final int icon=0x7f020000;
        }
        public static final class id {
            public static final int dialog_name_cancel=0x7f060002;
            public static final int dialog_name_name=0x7f060000;
            public static final int dialog_name_ok=0x7f060001;
        }
        public static final class layout {
            public static final int dialog_name=0x7f030000;
        }
        public static final class menu {
            public static final int myapp_context=0x7f050000;
            public static final int myapp_options=0x7f050001;
        }
        public static final class string {
            public static final int app_name=0x7f040000;
            public static final int dialog_name_cancel=0x7f040001;
            public static final int dialog_name_ok=0x7f040002;
        }
    }

This class then needs to be converted to Pascal code in a unit I decided to call _src/resources.pas_ (this needs to be done everytime you recreate the resources, because ID values might change then). The unit then looks like this: 
    
    
    unit Resources;
     
    {$mode objfpc}{$H+}
    {$modeswitch unicodestrings}
    {$namespace org.myname.myapp}
     
    interface
     
    type
      R = class sealed
      public
        type
          attr = class sealed
     
          end;
     
          drawable = class sealed
          public
            const
              icon = $7f020000;
          end;
     
          id = class sealed
          public
            const
              dialog_name_cancel = $7f060002;
              dialog_name_name = $7f060000;
              dialog_name_ok = $7f060001;
          end;
     
          layout = class sealed
          public
            const
              dialog_name = $7f030000;
          end;
     
          menu = class sealed
          public
            const
              myapp_context = $7f050000;
              myapp_options = $7f050001;
          end;
     
          strings = class sealed
          public
            const
              app_name = $7f040000;
              dialog_name_cancel = $7f040001;
              dialog_name_ok = $7f040002;
          end;
      end;
     
    implementation
     
    end.

You could also use global constants, but this way it looks more like the usual Android development. 

### Compiling the source

If you've followed the advice above regarding adjusting FPC and Lazarus you should be able to simply compile the code using Lazarus and setting the target to _jvm-android_. The unit output directory should also be set to _bin/classes_. 

If you did not follow the advice you can create a script or a makefile containg the following command (note: you can not simply change the compiler, because Lazarus then does not know the target "android"): 
    
    
    /path/to/ppcjvm -n -Tandroid -Fu/path/to/android-rtl -FEbin/classes -FD/directory/of/jasmin-jar myapp.lpr

You can then add this script, makefile or also the command itself to Lazarus' pre or post build commands (Project => Project Settings => Compilation) and uncheck all checkboxes for the compilation command. For error locations you can also check the "FPC" checkbox next to "look for messages" for your command. 

After the source is compiled you need to convert it to the format understood by Dalvik. For this you also need to copy all *.class files provided in the _units/jvm-android_ directory and subdirectories to the _bin/classes_ directory (it should be possible to put the RTL files into a JAR file as well, but this was not tested by me). Then you simply execute the following command: 
    
    
    $platformtools/dx --dex --output=bin/classes.dex bin/classes

### Generating the package

Now you need to generate first a unsigned package out of the resources and the dex file. This is done using the following command: 
    
    
    $tools/apkbuilder bin/myapp-unsigned.apk -u -z bin/myapp.ap_ -f myapp/classes.dex

The file given with _-z_ is the resource package generated earlier. 

_Note:_ This tools will print a message that it is deprecated (which is true), but for now it still works. Until it is removed a solution for this needs to be found (maybe by developing a custom application around the Java class which does the hard work and which is not deprecated). 

After the unsigned package is created it needs to be signed so it can be loaded to the emulator or device. For this the following command suffices: 
    
    
    jarsigner -keystore /path/to/YourKeyStore.keystore -signedjar bin/myapp.apk bin/myapp-unsigned.apk YourAlias

The values _/path/to/YourKeyStore.keystore_ and _YourAlias_ are the same which you created earlier. You are then prompted for the passwords of the two. 

### Deploying the package

Using the _adb_ tool the package can be directly installed to a running emulator or connected device (for more infos on the latter see [here](<http://developer.android.com/guide/developing/device.html>)). 

For the emulator use the following command: 
    
    
    $tools/adb -e install -r bin/myapp.apk

For the device the following: 
    
    
    $tools/adb -d install -r bin/myapp.apk

In both cases the _-r_ tells _adb_ to reinstall the application (thus application data like databases is kept). 

You can uninstall an application (together with its user data) using the following command: 
    
    
    $tools/adb -e uninstall org.myname.myapp

Use _-d_ instead of _-e_ if you want to do this on a connected device. 

_Hint:_ The uninstall command gives a good reason why you should use different namespaces for your application. 

## Finding errors

During the development of an Android application you might often see a dialog that informs you that your application has crashed. You can then use the _$tools/adb_ tool with the "logcat" command which will cat the log of the device or emulator to your console (you need to use Ctrl+C to close it again). There you'll see the name of the Exception class, its message and a stacktrace. You can also use the tool _$tools/ddms_ which is a GUI application and represents the log to you in a colored way (depending on the severity of the log message). It also allows taking screenshots. 

You can also write log messages to that log yourself by using the static methods of the class _android.utils.Log_. 

Common errors: 

  * using a functionality that is not part of the running version: the current Android unit is generated for SDK version 14. As the compiler does not know what e.g. version 10 might have contained or not contained you are responsible yourself that you only use methods and classes that are available in the corresponding SDK version. Possible exceptions are "FieldNotFoundException" or "ClassDefNotFoundException"

---

_Source: [https://wiki.freepascal.org/FPC_JVM_Android_Development](https://web.archive.org/web/20190609094630/https://wiki.freepascal.org/FPC_JVM_Android_Development)_
