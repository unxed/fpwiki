# Android Build APK

│ **[English (en)](<../en/Android_Build_APK.md>)** │  **русский (ru)** │

Данная статья освещает способ как скомпилировать проект на FPC и загрузить его на андроид устройство. 

Подразумевается, что проект являет собой NDK библиотеку, которая загрузается в Java коде. Так же, данный способ не привязан к какому либо расширению IDE (например Android4Laz) и может использовать отдельно для произвольных проектов. 

## Contents

  * 1 Требования
    * 1.1 Версия JRE/JDK
  * 2 Сборка
  * 3 Смотри также



## Требования

  * FPC (с готовыми [Android rtl](<Android.md> "Android/ru") библиотеками).
  * JRE (Java-Run Time) - нужен для JDK, а так же, для запуска "java" машины (для исполнения утилит входящийх в состав Android SDK)
  * JDK (необходим для установки **javac**), JDK потребует установки JRE.
  * Android NDK
  * Android SDK



Никаких дополнительнех средств сборкт (например ant) не требуется 

### Версия JRE/JDK

На данный момент (зима 2017) официально доступна для скачивания только версия 8 (1.8). Ранее можно было скачать версию 7. Многие компоненты Android SDK привязаны к конкретной версии JDK. 

## Сборка

0\. Предполагается, что весь проект лежит в некоторой папке, например "mydemo". А все скрипты и утилиты для сборки под android находятся в "mydemo\android". Это имеет значение только на следующем шаге, где указан ключ -FE. 

1\. необходимо скомпилировать проект (библиотеку). Командная строка (windows)может выглядеть таким образом. 
    
    
    C:\FPC\3.1.1\bin\i386-win32\fpc.exe ^
     -Tandroid -Parm -MDelphi ^
     -Scghi -CX -O3 -Xs -XX -l -vbq ^
     -FUlib\arm-android ^
     -oandroid\mydemo.so ^
     -FE.\android\files\lib\armeabi\ ^
     -CpARMv6 -CfVFPv2 -Xd -XParm-linux-androideabi- ^
     -FDC:\android\ndk-r11c\toolchains\arm-linux-androideabi-4.9\prebuilt\windows\bin  ^
     -FuC:\android\ndk-r11c\platforms\android-9\arch-arm\usr\lib ^
     mydemo.lpr
    

На что следует обратить внимание. Ключи 

  * -FD - указывает путь, где хранятся утилиты сборки
  * -XParm-linux-androideabi- - указывает префикс для утилит сборки
  * -Fu - указывающие на библиотеки NDK - необходимы для компоновки
  * -FE - указывает путь, куда следует записать созданную библиотеку. Конечная директория в данном случае указана как "lib\armeabi". Этот путь будет сохранится в самом apk. Он должен быть именно таким, т.к. он привязан к целевой архитектуре (arm). Для arm64 или arm7 путь должен быть иным. Все сторонние динамические библиотеки для ARM архитектуры, так же должны находится в папке lib\armeabi.



2\. подготовить файл-манифест AndroidManifest.xml 

3\. создать .apk файл 
    
    
    set buildtoolsdir=C:\android\sdk-windows\build-tools\23.0.3
    set APP_NAME=testapp
    set APK_SDK_PLATFORM=C:\android\sdk-windows\platforms\android-11
    
    rem -S res - fails, if "res" directory doesn't exist
    rem enerate-dependencies is needed to generate R.java file
    %buildtoolsdir%\aapt p -v -f -M AndroidManifest.xml ^
      -F bin\%APP_NAME%.ap_ ^
      --generate-dependencies ^
      -I %APK_SDK_PLATFORM%\android.jar -S res -m -J gen ^
      files
    

утилита создаёт файл начальный .apk файл (-F bin\%APP_NAME%.ap_ - расширение указано как "ap_" намеренно, т.к. это не итоговый файл), а так же R.java файл в папке "gen" (-J gen). При создании apk (по сути .zip) файла, будут включена папка "files". (на шаге №1 в неё был скопилированы библиотека "-FE.\android\files\lib\armeabi\", таким образом "lib\armeabi\mydemo.so" попадёт в apk файл) "-S" указывает папку со стандартными андроид ресурсами (иконки и layout xml-ы), папка будет скопирована в .apk 

Структура сгенерированного .apk файла будет выглядеть так: 
    
    
    AndroidManifest.xml 
    resources.arsc
    res
    lib
    lib\armeabi
    ...все другие папки/файлы которые были расположены в "files"
    

Если у программы есть какие-либо ресусрные файлы, то они должны быть добавлены в "files" непосредственно ПЕРЕД вызовом "aapt". Копировать их в "files" тоже не обязательно (как и использовать папку "files"), т.к. можно указать несколько папок-источников, например так: 
    
    
    %buildtoolsdir%\aapt ... ^
      dir1 dir2 dir3
     
    

4\. скомпилировать Java код основной программы 
    
    
    set jdkbindir=C:\Program Files\Java\jdk1.7.0_25\bin
    set androidjar=C:\android\sdk-windows\platforms\android-11\android.jar 
    
    "%jdkbindir%\javac.exe" ^
      .\zengl\android\ZenGL.java ^
      .\zengl\demo01\Demo01Activity.java ^
      -cp "%androidjar%" ^
      -d .\classes
    

Важно - указать "-cp" (class-path) для используемой Андроид-СДК. Компилятор java, как и сам jdk, ничего об андроид системе не знают. А компилируемая программа, будет использовать классы SDK. По-этому если class-path не указать, то будут сыпаться ошибки о том, что не объявлен класс и т.д. -d - указывает путь, куда положить скомпилированные .class файлы, они необходимы на следующем шаге. 

  * На дальнейшем шаге скомпилированный код, должен быть преобразован в Dalvik. Возможно, что версия "dx.jar" ожидаен классы версии 1.7, а не 1.8 (которую вы можете использовать). В этом случае, необходимо javac необходимо указать версию совместимости:


    
    
    set jdkbindir=C:\Program Files\Java\jdk1.8.0_25\bin
    set androidjar=C:\android\sdk-windows\platforms\android-11\android.jar 
    
    "%jdkbindir%\javac.exe" ^
      .\zengl\android\ZenGL.java ^
      .\zengl\demo01\Demo01Activity.java ^
      -cp "%androidjar%" ^
      -d .\classes -target 1.7 -source 1.7
    

5\. сконвертировать Java код в dalvik 
    
    
    SET DX_PATH=C:\android\sdk-windows\build-tools\23.0.3\lib
    
    java -Djava.ext.dirs=%DX_PATH%\ -jar %DX_PATH%\dx.jar --dex --verbose --output=.\bin\classes.dex .\classes
    

Конвертирация java-класс файлов в далвик файлы. 

6\. создать не подписанный .apk файл, добавив dalvik класс-файлы 
    
    
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
    

На момент написания скрипта, отдельной утилиты для добавления classes.dex в .apk файл не было... Но здесь происходит вызов вызов ApkBuilderMain (через вызов java машины). Единственное что он делает, это добавляет classes.dex (созданный на шаге №5) в корневую папку существующего .apk файла (созданного на шаге №3 .ap_) и записывает в файл -unsigned.apk. Обычная zip утилита (например 7z), скорей всего, справилась бы не хуже. 

7\. Подписать .apk файл. Внимание! данный скрипт генерирует новый само-подписанный ключ каждый раз. Программа загружаемая на Google Play должна использовать настоящий ключ (это так же важно при дальнейшем обновлении программы) 
    
    
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
    

7.1 проверка подписи. (шаг опциональный) 
    
    
    set jdkbindir=C:\Program Files\Java\jdk1.7.0_25\bin
    set APP_NAME=testapp
    
    REM Signing the APK with a debug key
    "%jdkbindir%\jarsigner" -verify -verbose -certs bin\%APP_NAME%-unaligned.apk
    

8\. Выровнять результирующий .apk. (Выравнивние должно происходить ПОСЛЕ подписи, т.к. подпись делает apk файл не выровнянным) 
    
    
    set APP_NAME=testapp
    set buildtoolsdir=C:\android\sdk-windows\build-tools\23.0.3
    
    REM Align the final APK package
    %buildtoolsdir%\zipalign -v 4 bin\%APP_NAME%-unaligned.apk bin\%APP_NAME%.apk
    

По завершению 8-ого шага, результирующий .apk файл может быть загружен непосредственно на устройство. 

9\. Загрузка любой .apk файл можно загрузить на устройство, если оно подлюкчено по USB и на нём разрешены права разработчика. В состав SDK входит утилита adb, которая позволяет удалить приложение и установить новый .apk. (Если приложение уже установлено, то новый .apk загружен не будет, и предварительно его нужно удалять с помощью uninstall) 
    
    
    C:\android\sdk-windows\platform-tools\adb.exe uninstall zengl.demo01
    C:\android\sdk-windows\platform-tools\adb.exe install bin\testapp.apk
    

## Смотри также

  * [Кросс-компилятор](<Android.md> "Android/ru") инструкции по сборке кросс-компилятора на Андроид

---

_Source: [https://wiki.freepascal.org/Android_Build_APK/ru](https://web.archive.org/web/20250516145557/https://wiki.freepascal.org/Android_Build_APK/ru)_
