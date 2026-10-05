# Android Programming

│ **[English (en)](<../en/Android_Programming.md>)** │  **русский (ru)** │

Смотрите также [Custom Drawn Interface/Android](<../en/Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")

## Contents

  * 1 Как...
    * 1.1 Построить NDK (Android Native Development Kit) OpenGL
    * 1.2 Получить параметры экрана
  * 2 Подробная информация об устройствах с Android
    * 2.1 Смартфоны
    * 2.2 Планшеты
    * 2.3 Списки устройств сгруппированные по процессорам (ARMv6 vs ARMv7)
    * 2.4 Воспроизведение Аудио и Видео на телефонах
  * 3 Файловая система Android
  * 4 Создание нового Java Приложения для Android
    * 4.1 Показать / Скрыть виртуальную клавиатуру
  * 5 Ссылки



Общие знания о программировании под Android могут очень помочь в разработке интерфейса Lazarus для Android. 

## Как...

### Построить NDK (Android Native Development Kit) OpenGL

Просто выполните следующие действия: 

**Шаг 1** \- Скачайте и установите Android NDK, Android SDK и Ant. Больше информации тут: [Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones/ru](<Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones.md> "Android Interface/Using the Android SDK, Emulator and Phones/ru")

**Шаг 2** \- Установите pre-compiled FPC cross-compiler. Инструкции здесь: [Android_Interface#Using_the_pre-compiled_compiler](<../en/Android_Interface.md> "Android Interface")

**Шаг 3** \- Загрузите последнюю версию lazarus-ccr sourceforge code: 
    
    
    svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr> lazarus-ccr
    

или если вы думаете что это слишком много, вы можете скачать только каталог lazarus-ccr/bindings/android-ndk 

**Шаг 4** \- Постройте Pascal Library 

Откройте проект lazarus-ccr/bindings/android-ndk/examples/opengltest/opengltest.lpi 

Откройте пункт меню Project->Project Options->Paths и где написано "Libraries -Fl" вы должны увидеть следующее значение: 
    
    
    /home/felipe/Programas/android-ndk-r5/platforms/android-9/arch-arm/usr/lib
    

Измените его на правильный путь, который указывает на библиотеку NDK с минимальной необходимой версией Android API Level 

Теперь постройте проект используя Lazarus. 

**Шаг 5** \- Настройка среды сборки 

Откройте файл opengltest/local.properties и в строке: 
    
    
    sdk.dir=/home/felipe/Programas/android-sdk-linux
    

Измените этот путь на местоположение SDK 

**Шаг 6** \- Постройка APK 

Откройте консоль и введите следущие команды: 
    
    
    cd lazarus-ccr/bindings/ndk/examples/opengltest/android
    ant debug
    

Файл APK будет помещен в opengltest/android/bin/ 

**Шаг 7** \- Установка APK 

Если на этом шаге вы получите сообщение об ошибке прав доступа, читайте: [Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones#Recognition_of_devices_under_Linux](<../en/Android_Interface/Using_the_Android_SDK,_Emulator_and_Phones.md> "Android Interface/Using the Android SDK, Emulator and Phones")

Команда, чтобы установить APK на телефоне: 
    
    
    cd opengltest/android
    ~/Programas/android-sdk-linux/platform-tools/adb install bin/OpenGLNDKTest-debug.apk 
    2395 KB/s (107299 bytes in 0.043s)
           pkg: /data/local/tmp/OpenGLNDKTest-debug.apk
    Success
    

Если вы получили сообщение об ошибке, что APK уже установлен, вы можете установить его с помощью команды: 
    
    
    ~/Programas/android-sdk-linux/platform-tools/adb uninstall com.pascal.opengltest
    

После можно запустить _adb logcat_ чтобы увидеть журнал, в то время как вы запустите его в телефон через недавно добавленную иконку: 
    
    
    ~/Programas/android-sdk-linux/platform-tools/adb logcat
    

### Получить параметры экрана

Вначале нужно заполнить TDisplayMetrics: 
    
    
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
    

После вы можете получить информацию о дисплее из TDisplayMetrics: 
    
    
      lHeight := MyDisplayMetrics.heightPixels();
      lWidth := MyDisplayMetrics.widthPixels();
      xdpi := MyDisplayMetrics.xdpi();
      ydpi := MyDisplayMetrics.ydpi();
      lScreenSize := sqrt(sqr(lHeight / ydpi) + sqr(lWidth / xdpi));
      ldensity := MyDisplayMetrics.density();
      ldensityDpi := MyDisplayMetrics.densityDpi();
      scaledDensity := MyDisplayMetrics.scaledDensity();
    

Обратите внимание, что многие устройства врут о xdpi и ydpi, поэтому не стоит доверять lScreenSize, рассчитанному выше. Смартфоны могут сообщить о размере экрана 10 дюймов, хотя их настоящий размер может быть около 4. 

## Подробная информация об устройствах с Android

Поскольку существует так много Android устройств, эта информация может быть полезно для отслеживания информации о них. Смотрите также в википедии [Сравнение устройств с Android](<http://ru.wikipedia.org/wiki/Сравнение_устройств_с_Android>) и подробную информацию о процессорах в [Android#System_Requirements](<https://wiki.mozilla.org/Mobile/Platforms/Android#System_Requirements>). 

### Смартфоны

**Производитель** | **Модель** | **Android API Name (Build.Model)** | **Процессор** | **Версия Android** | **Мультитач** | **Комментарий**  
---|---|---|---|---|---|---  
HTC | Wildfire | HTC Wildfire | armv6 | 2.1=>2.2 | Да | \-   
Sony Erricson | Xperia X10 | X10i | armv7 | 1.6=>2.1 | Нет | \-   
  
### Планшеты

**Производитель** | **Модель** | **Android API Name (Build.Model)** | **Процессор** | **Версия Android** | **Мультитач** | **Комментарий**  
---|---|---|---|---|---|---  
Toshiba | Folio 100 Tablet | TOSHIBA_FOLIO_AND_A | armv7 | 2.2 | Yes | \-   
  
### Списки устройств сгруппированные по процессорам (ARMv6 vs ARMv7)

**Список устройств на ARMv7:**

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



**Список устройств на ARMv6:**

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



### Воспроизведение Аудио и Видео на телефонах

Каждый телефон поставляется со своим набором кодеков, поэтому на разных телефонах родной Media Player может воспроизводить разные форматы. Ниже приведена таблица, показывающая, какие форматы воспроизводит Media Player на каждом телефоне: 

**Телефон** | **Android** | **webm - VP8** | **m4v - H.264** | **ogv - Theora** | **mp4 - H.264** | **mov - H.264** | **avi - RLE** | **mpg - MPEG-1** | **wmv - WM9** | **3gp - MPEG-4**  
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
Milestone 2 | 2.2 | x | OK | x | x | x | x | x | только звук | OK   
  
## Файловая система Android

Рекомендуется использовать getApplicationContext().getFilesDir() чтобы получить путь к каталогу для хранения данных приложения. В HTC Wildfire эта функция возвращает /data/data/<package name>

Библиотеки приложения находятся в /data/data/<package>/lib 

В папке /data/data/<package name> есть разрешение на чтение и запись, можно создавать новые файлы и даже запускать их. 

## Создание нового Java Приложения для Android

Эта информация может быть полезна для реализации LCL-CustomDrawn-Android 

Общие инструкции здесь: [Managing Projects from the Command Line](<http://developer.android.com/tools/projects/projects-cmdline.html>)

### Показать / Скрыть виртуальную клавиатуру

Чтобы показать: 
    
    
     InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
     imm.toggleSoftInput(InputMethodManager.SHOW_FORCED,0);
    

и чтобы спрятать клавиатуру: 
    
    
     InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
     imm.hideSoftInputFromWindow(singleedittext.getWindowToken(),0);
    

## Ссылки

  * [Managing Projects from the Command Line](<http://developer.android.com/tools/projects/projects-cmdline.html>)
  * [Show or Hide soft keyboard on opening a dialog or activity in android](<http://android-codes-examples.blogspot.com/2011/11/show-or-hide-soft-keyboard-on-opening.html>)

---

_Source: [https://wiki.freepascal.org/Android_Programming/ru](https://web.archive.org/web/20250117043423/https://wiki.freepascal.org/Android_Programming/ru)_
