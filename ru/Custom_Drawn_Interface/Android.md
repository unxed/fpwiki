# Custom Drawn Interface/Android

│ **[English (en)](<../../en/Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")** │  **[español (es)](</Custom_Drawn_Interface/Android/es> "Custom Drawn Interface/Android/es")** │  **[日本語 (ja)](</Custom_Drawn_Interface/Android/ja> "Custom Drawn Interface/Android/ja")** │  **русский (ru)** │    
****

Вернуться к [Custom Drawn Interface](<../../en/Custom_Drawn_Interface.md> "Custom Drawn Interface")

## Contents

  * 1 Архитектура
  * 2 Roadmap
  * 3 Использование Android SDK, эмулятора и телефона
  * 4 Программирование для Андроид
  * 5 Настройка Free Pascal Compiler для Android
    * 5.1 Используйте готовый образ Virtual Box с FPC и Lazarus
    * 5.2 Как самому сделать компилятор в Windows?
    * 5.3 Создание вашего компилятора в Linux
    * 5.4 Использование пре-компилятора в Linux
  * 6 Компиляция примера LCL Android приложения
  * 7 How to create an LCL Android Application
    * 7.1 Step 1 - Creating the LPI
    * 7.2 Step 2 - Building the library
    * 7.3 Step 3 - Create the Android project structure
    * 7.4 Building an LCL Android application with debug info
  * 8 Oh no! My LCL Android application doesn't work
    * 8.1 The Pascal executable was compiled for a wrong architecture, operating system and/or widgetset
    * 8.2 My Pascal application crashed. How to get a stacktrace?
    * 8.3 The indispensable build options
  * 9 Free Pascal Bugs on Android Support
  * 10 Development Notes
    * 10.1 The misterious JNI Crash
    * 10.2 NDK Libraries available in Android 2.2 (API level 8)
    * 10.3 Native text drawing
    * 10.4 Flood of garbage collector messages when the caret is visible
    * 10.5 Why the caret of a TEdit keeps blinking after clicking on the form



## Архитектура

LCL-CustomDrawn-Android использует маленькое Java приложение, которое сообщается с нашей Pascal библиотекой, отправляет ей все события и выполняет все команды от неё. Общение обеспечивается JNI, так как Google поддерживает это. Элементы управления не являются родными, вместо этого они нарисованы в TRawImage+TLazIntfImage+TLazCanvas и события, и рисование сокращается с помощью LazRegions. Рисование делается в Pascal, используя jnigraphics для рисования на Bitmap Java объекте, что в последствии сокращает активность Java на внешнем виде. 

Наши предыдущие и недолгая попытка написать LCL интерфейс для Android (LCL-Android), который использовал не стандартный метод использования родной исполняемой системы, чтобы сообщаться с Java при помощи "труб". Мы отказались от этой системы, потому что это было не продумано и не поддерживалось Google. Это считается устаревшим и люди должны вместо этого использовать LCL-CustomDrawn-Android. Этот старый интерфейс был описан в [Android Interface](<../../en/Android_Interface.md> "Android Interface"). 

## Roadmap

  1. ~~Build the set of[Lazarus Custom Drawn Controls](<../../en/Lazarus_Custom_Drawn_Controls.md> "Lazarus Custom Drawn Controls")~~
  2. ~~Initial bindings for the Android APIs~~
  3. ~~Create an application to automatically generate the bindings~~
  4. ~~Start the new widgetset~~
  5. ~~Implement support for JNI~~
  6. ~~Merge the Lazarus Custom Drawn Controls into the LCL and use them to implement all basic controls~~
  7. ~~Add text support for Android~~
  8. ~~Implement basic window client area scrolling~~
  9. Add mobile emulator for desktop platforms
  10. Improve the mobile scrolling
  11. Add text support in X11
  12. Add DPI awareness and adaptation in the LCL
  13. Add non-native text support via pasfreetype



## Использование Android SDK, эмулятора и телефона

[Android Interface/Using the Android SDK, Emulator and Phones/ru](<../Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones.md> "Android Interface/Using the Android SDK, Emulator and Phones/ru")

## Программирование для Андроид

Смотрите раздел [Android Programming/ru](<../Android_Programming.md> "Android Programming/ru"), который также включает в себя важные для программирования особенности телефонов. 

## Настройка Free Pascal Compiler для Android

### Используйте готовый образ Virtual Box с FPC и Lazarus

Самый быстрый способ настройки Free Pascal и Lazarus для разработки под Андроид - просто использовать заранее настроенный образ для виртуальной машины который уже содержит Mageia Linux 1, KDE, Lazarus 0.9.31, Subversion, Free Pascal для обоих Linux: x86-linux и arm-linux (targetting Android), Android SDK, Android NDK, Java, Ant, Gtk2, и все его разрабатываемые пакеты, GNU debugger (GDB) и все остальное необходимое для создания приложений для Андроид в Lazarus. 

Загрузить образ виртуальной машины: <https://sourceforge.net/projects/p-tools/files/Lazarus_for_Android_VM_Image/snapshot_30_dez_2011/>

Образ виртуальной машины состоит из двух файлов: “Android Devel VM.vbox” и “Android Devel VM.hdd”. Оба файла должны находиться в одной папке и следует отметить то, что source forge архив и его надо разархивировать. После распаковки образ будет весить 5,2GB. Для использования этой виртуальной машины, нужно установить Virtual Box и при помощи этой программы открыть .vbox файл. После этого можно использовать эту Linux виртуальную машину для создания Android приложений. Процесс создания приложения включает в себя первое открытие Lazarus и создания приложения в Lazarus, примеры проектов доступны в ~/Programas/lazarus/example/androidlcl/ и после идите в терминал linux и перейдите в ~/Programas/lazarus/example/androidlcl/android и выполните команду “ant debug”, которая генерирует APK файл в ~/Programas/ lazarus/example/androidlcl/android/bin/LCLExample-debug.apk. Виртуальная машина имеет два пользователя: “root” и “pascaldev” и у обоих из них пароль: “root”. 

Обратим внимание, что виртуальная машина уже имеет встроенный код приложения Lazarus, но также можно это приложение обновить. Но это предполагает определенный риск т.к. версия из интернета может содержать проблемы. 

### Как самому сделать компилятор в Windows?

**Шаг 1** -Установить Android SDK и NDK 

Загрузите с сайта Google и установите SDK и NDK для Windows. 

Проверьте путь к binutils в установленном NDK. Например для NDK r7 установленном в C:\Programas\android-ndk-r7\ путь будет такой: C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin 

Также проверьте путь к библиотекам NDK. Например, для NDK r7 установленном в C:\Programas\android-ndk-r7\ путь будут такой: C:\Programas\android-ndk-r7\platforms\android-8\arch-arm\usr 

**Шаг 2** \- Скачайте FPC 2.5.1 от 19 января. Создайте каталог для хранения исходного кода FPC и з Create a folder to store the FPC trunk source code and создайте рабочую копию ревизии 16790 

**Шаг 3** \- Установите FPC 2.4.4 

Самый простой способ получить хороший FPC 2.4.4 это установить Lazarus 0.9.30.2. Для примера, предположим что вы установили эту версию в C:\Programas\lazarusfpc244 

**Шаг 4** \- Создание скрипта и его написание 

Создайте выходную папку, например: C:\Programas\fpcandroid\output 

Далее создайте файл, называющийся "build.bat"(без кавычек) в корневой папке FPC, например C:\Programas\fpcandroid и скопируйте в неё: 
    
    
    SET PATH=C:\Programas\lazarusfpc244\fpc\2.4.4\bin\i386-win32\
    make crossinstall CPU_TARGET=arm OS_TARGET=linux CROSSBINDIR=C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin OPT=-dFPC_ARMEL BINUTILSPREFIX=arm-linux-androideabi- INSTALL_PREFIX=C:\Programas\fpcandroid\output
    pause

Далее, запустите этот файл, и вы должны иметь FPC установлен в C:\Programas\fpcandroid\output 

**Шаг 5** \- Скопируйте некоторые cross-binutils в bin 

Вы должны иметь FPC bin'овские файлы in C:\Programas\fpcandroid\output\bin\i386-win32 

Теперь возьмите 3 файла: 

C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin\arm-linux-androideabi-as.exe C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin\arm-linux-androideabi-ld.exe C:\Programas\android-ndk-r7\toolchains\arm-linux-androideabi-4.4.3\prebuilt\windows\bin\arm-linux-androideabi-strip.exe 

Скопируйте и переименуйте файлы: 

C:\Programas\fpcandroid\output\bin\i386-win32\arm-linux-as.exe C:\Programas\fpcandroid\output\bin\i386-win32\arm-linux-ld.exe C:\Programas\fpcandroid\output\bin\i386-win32\arm-linux-strip.exe 

**Шаг 6** \- Соедините в настройках Lazarus FPC 

Допустим, у вас есть следующий Lazarus C:\Programas\lazarus31 с установленным внутри FPC: C:\Programas\lazarus31\fpc\2.5.1\bin\i386-win32 и C:\Programas\lazarus31\fpc\2.5.1\units\ 

Сейчас скопируйте C:\Programas\fpcandroid\output\units\arm-linux в C:\Programas\lazarus31\fpc\2.5.1\units\ 

Скопируйте все .exe файлы из C:\Programas\fpcandroid\output\bin\i386-win32\ в C:\Programas\lazarus31\fpc\2.5.1\bin\i386-win32 

Сейчас Вы можете собрать пример проекта androidlcltest.lpi используя режим Windows-Release build 

### Создание вашего компилятора в Linux

Просмотрите [Setup_Cross_Compile_For_ARM](<../../en/Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM") и убедитесь, что используете опцию OPT="-dFPC_ARMEL" для создания компилятора. 

### Использование пре-компилятора в Linux

Пре-компилятор был сделан для удобства пользователей. Следующие шаги были испытаны в Mandriva Linux 2010.0 и 2010.1: 

**Необходимые окружающие среды**

  * Последняя и стабильная FPC, установленная с помощью RPM / DEB / TAR пакетов



**Step 1** \- Install the cross-binutils 

For Mandriva Linux the RPM package containing arm-linux-as, arm-linux-ld, etc, which are the cross-binutils can be found here: <http://rpm.pbone.net/index.php3/stat/4/idpl/14252825/dir/mandriva_2010/com/cross-arm-binutils-2.20.51.0.4-2mnb2.i586.rpm.html>

Just download the RPM package and install it using: 
    
    
    rpm -ivh cross-arm-binutils-2.20.51.0.4-2mnb2.i586.rpm 
    

In Mandriva Linux 2010.0 the dependencies won't match, as the package is for 2010.1, but one can simply ignore this problem and it works fine using --nodeps: 
    
    
    rpm -ivh --nodeps cross-arm-binutils-2.20.51.0.4-2mnb2.i586.rpm 
    

For other distributions use the corresponding package, or else read the instructions for building the cross-binutils yourself at [Setup_Cross_Compile_For_ARM](<../../en/Setup_Cross_Compile_For_ARM.md> "Setup Cross Compile For ARM")

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

## Компиляция примера LCL Android приложения

**Шаг 1 - Загрузите исходный код**

Исходный код примера находится внутри Lazarus источник в lazarus/examples/androidlcl/androidlcltest.lpi 

**Шаг 2 - Сборка проекта с использованием Lazarus IDE**

Configuring Lazarus itself to use the new compiler should not normally be necessary because fpc should be able to find the symlink created, but if you have trouble in this part you can try to hard code the compiler path to use the new crosscompiler. To hardcode the compiler path in cause of trouble go to the menu "Tools->Options" and change the "Compiler Path" to "/usr/bin/ppcrossarm" 

But it is necessary to configure the LPI file to build the example. Open the Project Options dialog of the androidlcltest.lpi project and go to the section Paths and change the libraries path to reflect the correct paths of where your Android NDK was installed and where your jnigraphics.so, libc.so, etc, libraries are located for your target Android API level. For LCL-CustomDrawn-Android API level 8 is the best choice, because it supports Android 2.2+ like the LCL. The image bellow shows where to configure this: 

[![Configure androidlcl libraries.jpg](https://wiki.freepascal.org/images/b/b0/Configure_androidlcl_libraries.jpg)](</File:Configure_androidlcl_libraries.jpg>)

**Step 3 - Build the APK**

Before going to the command line to build the APK open the file lazarus/examples/androidlcl/android/local.properties and change it to reflect your Android SDK path. The file says you should not modify it, but ignore that and modify it anyway. Here is how the file looks like: 
    
    
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

**Step 4 - Install the APK in your phone and run it**

You can install via ADB or by copying the file to the sdcard. You should see this: 

[![Custom drawn android test1.png](https://wiki.freepascal.org/images/6/6a/Custom_drawn_android_test1.png)](</File:Custom_drawn_android_test1.png>)

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

### Building an LCL Android application with debug info

It is useful to add another Android build mode which has debug info. Use all of the same options as shown above, except for the debug information: 

[![Android project options 4.png](https://wiki.freepascal.org/images/1/11/Android_project_options_4.png)](</File:Android_project_options_4.png>)

## Oh no! My LCL Android application doesn't work

There are various reasons why the app may not work. The most important thing to do when an app doesn't work is to open the logcat and see what the log says. This can be done by running this command line command: 
    
    
    ./adb logcat
    

And then test one of the hipotesis in the next subsections. 

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

### The indispensable build options

  * -Xd This build option is indispensable when cross-compiling from x86-linux to Android or else FPC will try to link the application against for example /usr/lib/libc.so instead of the libc.so in the NDK, even if you specify a -Fl library path
  * -CpARMV6 This build option is indispensable because code generated for older ARM versions is unsupported by Android and might crash in some devices. See <http://groups.google.com/group/android-ndk/browse_thread/thread/ba542483f062a828>
  * -dAndroid in the package LCLBase.lpk
  * -Parm -Tlinux for the process and target operating system
  * -Fl should contain the path to the NDK directory with the target link shared object, such as libc.so, liblog.so, libjni.so, etc



## Free Pascal Bugs on Android Support

The following summarizes the state of Android Support in Free Pascal: 

  * The pre-compiled snapshot for x86-linux -> Android is known to work very well and it was built on 19th January 2011: <https://sourceforge.net/projects/p-tools/files/Free%20Pascal%20for%20ARM/>
  * On February this revision [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=16981](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=16981>) introduced this bug: <http://mantis.freepascal.org/view.php?id=18833>
  * In the end of 2011 this revision [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19036](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&revision=19036>) broke linking Android projects



We are in contact with FPC developers to fix the situation as can be seen in: <http://mantis.freepascal.org/view.php?id=20726>

## Development Notes

### The misterious JNI Crash

See the thread: <http://groups.google.com/group/android-ndk/browse_thread/thread/ba542483f062a828>

The problem was that vm^^.GetEnv would crash in HTC Wildfire, Alcatel and in the emulator with SIGILL but not in Xperia Mini, HTC Desire HD, Motorola Atrix and other smartphones. 

The answer was that one needs to specify -CpARMV6 when building because older instructions might fail in some devices 

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

When clicking on the form the LCL focus will refuse to move from the Edit to the Form because this behavior seams to be hard coded in the LCL, although maybe we might change this for Android. So even while we set focus into the form, the LCL refuses to do that and keeps its focus in the Edit, thus we have a focus missmatch. This might eventually be fixed in the future.

---

_Source: [https://wiki.freepascal.org/Custom_Drawn_Interface/Android/ru](https://web.archive.org/web/20250122112013/https://wiki.freepascal.org/Custom_Drawn_Interface/Android/ru)_
