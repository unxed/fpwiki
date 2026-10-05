# Custom Drawn Interface/Using the Android SDK, Emulator and Phones

│ **[English (en)](<../../en/Custom_Drawn_Interface/Using_the_Android_SDK,_Emulator_and_Phones.md> "Custom Drawn Interface/Using the Android SDK, Emulator and Phones")** │  **русский (ru)** │    
****

  
Go back to [Custom Drawn Interface/Android](<../../en/Custom_Drawn_Interface/Android.md> "Custom Drawn Interface/Android")

## Contents

  * 1 Использование Android SDK
    * 1.1 Как настроить Ant
      * 1.1.1 Настройка Ant в Mandriva Linux
      * 1.1.2 Общие ошибки Ant
    * 1.2 Распознавание устройств в Linux
  * 2 Использование эмулятора
    * 2.1 Установка эмулятора
    * 2.2 Построка(Building) образа эмулятора с поддержкой карты памяти
    * 2.3 Запуск образа эмулятора
    * 2.4 Копирование файлов на эмулятор
    * 2.5 Установка приложений в эмуляторе
  * 3 Использование телефона
    * 3.1 Установка файлового менеджера
    * 3.2 Samsung Galaxy S



## Использование Android SDK

Загрузка и установка Android SDK очень простая. Нужно всего лишь перейти по ссылке: <http://developer.android.com/sdk/index.html>

### Как настроить Ant

При использовании 'ant debug' для построения проекта Андроид может быть получено много ошибок о неудовлетворенных зависимостях. В Linux это может быть решено установкой пакетов которые имеют эти зависимости, это будет менятся от дистрибутива к дистрибутиву. 

#### Настройка Ant в Mandriva Linux

Эти ошибки: 
    
    
    /usr/bin/build-classpath: error: Could not find jaxp_parser_impl Java extension for this JVM
    /usr/bin/build-classpath: error: Could not find xml-commons-apis Java extension for this JVM
    

Могут быть исправленны, соответственно, с: 
    
    
    urpmi xerces-j2
    urpmi xml-commons-jaxp
    

#### Общие ошибки Ant

Эта ошибка: 
    
    
     /home/felipe/Programas/lazarus-ccr/examples/androidlcl/android/build.xml:46: taskdef class com.android.ant.SetupTask cannot be found
    

означает, что нужно установить корректные пути к SDK в вашем файле проекта. 

### Распознавание устройств в Linux

В отличие от Windows, где нужно установить новый набор драйверов почти для каждого телефона, в Linux, Android устройства распознаются как базовые Android устройства (независимо от того, будь то телефон, планшет или ноутбук), и они готовы к использованию без установки дополнительных драйверов. Один из вопросов, однако, который часто возникает, это то что устройства не полностью распознаются автоматически и возникает сообщение об ошибке "?????? no permissions". В этом случае команда 'adb devices' для устройства будет выглядеть следующим образом: 
    
    
    [felipe@localhost ~]$ cd Programas/android-sdk-linux_x86/tools/
    [felipe@localhost tools]$ ./adb devices
    List of devices attached 
    ????????????    no permissions
    

Чтобы побороть эту проблему можно воспользоваться следующим решением: 

**Шаг первый** \- Сперва получите идентификатор устройства воспользовавшись утилитой **lspci** от имени **root'** a (или через команду **sudo**): 
    
    
    [root@localhost Programas]# lsusb
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 001 Device 003: ID 04e8:681c Samsung Electronics Co., Ltd 
    Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 006 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 007 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 008 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    

В данном случае идентификатор - **04e8**

**Шаг второй** \- Далее, находясь под root'ом (или через sudo) отредактируйте файл /etc/udev/rules.d/51-android.rules 

Если этот файл не существует то создайте его и добавьте следующую строку в файл: 
    
    
    SUBSYSTEM=="usb|usb_device", SYSFS{idVendor}=="04e8", SYMLINK+="android_adb", MODE="0666"
    

Замените идентификатор (idVendor) в соответствии с вашим устройством. 

**Шаг третий** \- Сделайте рестарт службы udev 

В Ubuntu это делается так: **sudo services udev restart**. В Mandriva нет службы udev, так что единственным путем решения будет просто перезагрузить компьютер. После этого ваше android-устройство должно быть полностью опознано и команда 'adb devices' должна заработать как и команда 'adb logcat'. 
    
    
    [felipe@localhost tools]$ ./adb devices
    * daemon not running. starting it now on port 5037 *
    * daemon started successfully *
    List of devices attached 
    1000c31696b6    device
    

## Использование эмулятора

### Установка эмулятора

### Построка(Building) образа эмулятора с поддержкой карты памяти

Шаг 1: Запустите приложение C:\Program Files\android-sdk-windows\tools\android.bat 

Шаг 2: Создайте новый образ, нажмите кнопку New как на изображении ниже. 

[![Android new image.PNG](https://wiki.freepascal.org/images/c/c6/Android_new_image.PNG)](</File:Android_new_image.PNG>)

Шаг 3: Установите название и параметры образа, размер карты памяти и поддрержку аппаратного обеспечения как на рисунке ниже. 

[![Android new image dialog.PNG](https://wiki.freepascal.org/images/8/8a/Android_new_image_dialog.PNG)](</File:Android_new_image_dialog.PNG>)

### Запуск образа эмулятора

Шаг 1: Запустите приложение C:\Program Files\android-sdk-windows\tools\android.bat 

Шаг 2: Выберите образ 

Шаг 3: Нажмите на кнопку "start", как на изображении ниже. 

[![Android run image.PNG](https://wiki.freepascal.org/images/e/e1/Android_run_image.PNG)](</File:Android_run_image.PNG>)

### Копирование файлов на эмулятор

Вы можете использовать скрипт похожий на этот для копирования файлов с вашего жоского диска на эмулятор. 

You can use a script similar to this one to copy files from your hard-drive to the emulator. Обратите внимание, что образ должен быть запущен и что он должен поддерживать карты памяти. 
    
    
    c:
    cd "Program Files"
    cd android-sdk-windows
    cd tools
    adb push C:\Mypath\Somefile.extension /sdcard
    pause
    

### Установка приложений в эмуляторе

Приложения для Андроид должны быть упакованы в файловом фомате .apk. Больше информации о том как создать .apk файл, который вы можете установить на эмуляторе/телефоне, смотрите Android Asset Packaging Tool (aapt). После упаковки файла, он может быть установлен в эмуляторе используя следуюзий вызов adb: 
    
    
    c:
    cd "Program Files"
    cd android-sdk-windows
    cd tools
    adb install C:\Mypath\MyApp.apk
    pause
    

## Использование телефона

Установка драйверов, получение надежного способа передачи исполняемых файлов и отладка ПО на разных телефонах с Андроид можеть быть проблеммой сама по себе. 

### Установка файлового менеджера

Удивительно, Андроид телефоны поставляются без файлового менеджера. Однако, очень просто загрузить один из Android Market. Я предлагаю следующие инструкции: 

1\. Подключите телефон к WiFi или Мобильной сети. 

2\. Нажмите на иконку "Market" на рабочем столе Андроид. Будет необходимо войти в аккаунт Google для доступа к нему. 

3\. Нажмите на кнопку поиска и введите "oi". 

4\. Найдите и установите "OI File Manager" (бесплатную версию). 

### Samsung Galaxy S

Первое что нужно сделать при использовании Samsung Galaxy S это отказаться от инструкций Samsung'a. Они утверждают что Вам необходимо поставить Kies, их ужасно гадкий набор программ для ПК весом в 130 мегабайт установка которого все время сбоит потому что первое что она делает это пытается безуспешно соединиться с серверами в Южной Корее для того что бы скачать ещё больше файлов. Вместо этого следуйте инструкциям ниже: 

Шаг первый. Зайдите в Настройки телефона -> Об Устройстве -> Параметеры USB. Смените с "KIES" на "Накопитель". 

Шаг второй. Скачайте и установите 19 мегабайт USB драйверов от Samsung по ссылке: [http://drivers.softpedia.com/dyn-postdownload.php?p=96692&t=4&i=1](<http://drivers.softpedia.com/dyn-postdownload.php?p=96692&t=4&i=1>)

Шаг третий. Подсоедините кабель телефона к ПК. 

Шаг четвертый. Опустите верхнее меню телефона вниз и подмонтируйте SD-карту.

---

_Source: [https://wiki.freepascal.org/Custom_Drawn_Interface/Using_the_Android_SDK%2C_Emulator_and_Phones/ru](https://web.archive.org/web/20250418101928/https://wiki.freepascal.org/Custom_Drawn_Interface/Using_the_Android_SDK%2C_Emulator_and_Phones/ru)_
