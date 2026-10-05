# Xcode

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

Эта статья относится только к [iOS](</Category:iOS> "Category:iOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/Xcode.md>)** │  **русский (ru)** │

## Contents

  * 1 Обзор
  * 2 Установка инструментов командной строки(CLT): Mavericks 10.9 и новее
    * 2.1 Установка Xcode (включая Command Line Tools)
    * 2.2 Установка только отдельного пакета инструментов командной строки(CLT)
  * 3 Установка Command Line Tools: Lion 10.7 или Mountain Lion 10.8
  * 4 LУстаревшая информация
    * 4.1 Xcode 7.0
    * 4.2 Xcode 5.0
  * 5 Внешние ссылки



## Обзор

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Вам НЕ НУЖНО устанавливать Xcode IDE, чтобы получить все утилиты сборки, от которых зависит компилятор Free Pascal, но вам НЕОБХОДИМО установить Command Line Tools (CLT - инструменты командной строки). Как это сделать, объясняется ниже. Вам НЕОБХОДИМО установить Xcode, если: (1) вы хотите получить доступ к SDK для **iOS** , **iPadOS** , watchOS и tvOS; или (2) для проверки и загрузки приложений в Mac App Store; или (3) для [нотариального заверения приложений](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+/ru") для распространения за пределами Mac App Store.

Xcode — это интегрированная среда разработки для macOS, содержащая набор инструментов разработки программного обеспечения, разработанных Apple для разработки программного обеспечения для macOS, iOS, iPadOS, watchOS и tvOS. 

Автономный пакет инструментов командной строки (Command Line Tools - CLT) — это небольшой автономный пакет, доступный для загрузки отдельно от Xcode, который позволяет вам выполнять разработку командной строки в macOS. Он состоит из macOS SDK и инструментов командной строки, которые устанавливаются в каталоге /Library/Developer/CommandLineTools. 

## Установка инструментов командной строки(CLT): Mavericks 10.9 и новее

Вы можете использовать любой из следующих методов для установки инструментов командной строки в вашей системе: 

### Установка Xcode (включая Command Line Tools)

Если вы устанавливаете Xcode, то нет необходимости устанавливать инструменты командной строки (CLT). Xcode поставляется в комплекте со всеми инструментами командной строки. Mavericks 10.9 и более поздние версии включают прослойки или исполняемые файлы-оболочки. Эти прослойки, установленные в `/usr/bin/`, могут отображать любой инструмент, включенный в `/usr/bin/`, на соответствующий внутри Xcode. **xcrun** — одна из таких прослоек, позволяющая найти или запустить любой инструмент внутри Xcode из командной строки. Используйте его для вызова любого инструмента в Xcode из командной строки. Например: 
    
    
    $ xcrun dwarfdump --uuid  MyApp.app/Contents/MacOS/MyApp 
    UUID: 4CED1202-EB68-3991-A036-7146FB4D4E17 (x86_64) MyApp.app/Contents/MacOS/MyApp
    

Однако вам нужно будет отредактировать файл `/etc/fpc.cfg`, чтобы добавить эту строку: 
    
    
    -XR/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk
    

в разделе "Linking"(Связывание), расположенном ближе к концу файла, чтобы FPC мог найти SDK для macOS. 

**Обратите внимание** , что это не официально поддерживаемый метод и не рекомендуемый метод для доступа к инструментам командной строки, и что когда вы впоследствии установите FPC, его установщик предупредит вас, что автономные инструменты командной строки не были установлены. 

### Установка только отдельного пакета инструментов командной строки(CLT)

Вы можете установить отдельный пакет инструментов командной строки ((CLT)), открыв терминал и выполнив следующие команды: 
    
    
    sudo xcode-select --install
    sudo xcodebuild -license accept
    

macOS поставляется в комплекте с xcode-select, инструментом командной строки, который устанавливается в `/usr/bin/` . Он позволяет вам управлять активным каталогом разработчика для Xcode и других инструментов разработки BSD. См. его справочную страницу для получения дополнительной информации. 

В Mavericks 10.9 и более поздних версиях обновление программного обеспечения уведомляет вас, когда для обновления доступны новые версии инструментов командной строки. 

**Обратите внимание** , что при установке автономных инструментов командной строки (а не при установке Xcode) вы получите только SDK для macOS (у вас не будет SDK для **iOS** , **iPadOS** , watchOS или tvOS). 

## Установка Command Line Tools: Lion 10.7 или Mountain Lion 10.8

Вам нужно будет зарегистрировать бесплатную учетную запись разработчика Apple, чтобы войти в систему и получить доступ к загрузке инструментов командной строки. Перейдите на [эту страницу](<https://developer.apple.com/downloads/>), войдите в систему, найдите инструменты командной строки и загрузите соответствующий файл. Смонтируйте загруженный файл DMG и запустите установщик пакета. 

## LУстаревшая информация

### Xcode 7.0

  * Если после обновления до Xcode 7 ваши проекты не компилируются и вы используете FPC 2.6.4 (или более раннюю версию), [измените информацию об отладке](<../en/Debugger_Setup.md> "Debugger Setup") с "Automatic" на "Dwarf". Ассемблер Apple больше не поддерживает отладочную информацию .Stabs (которая используется по умолчанию на i386). 
    * If your project is using packages you might need to switch these packages debug info to Dwarf as well. There're two ways to do that.



    

    1\. Измените информацию об отладке каждого пакета. Откройте Project -> Project Inspector. Для каждого пакета, указанного в разделе "Required Packages"(Обязательные пакеты), дважды щелкните пакет (чтобы открыть диалоговое окно пакета), выберите "Options"(Параметры) и измените тип отладочной информации в разделе "Debugging"(Отладка). 

    или
    2\. Вы можете добавить настройку в качестве пользовательской опции для всех пакетов, используемых в проекте. Как показано в примере на [этой странице](<../en/IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options"). Но вместо "-dSomeValue" поставьте "-gw"

  * В качестве альтернативы вы можете загрузить предыдущую версию 6.4. 
    * Загрузите Xcode 6.4 и измените имя папки на **Xcode6.4** перед копированием в /Application.
    * Итак, теперь у вас есть два приложения Xcode, поэтому переключите инструменты командной строки на использование инструментов из **Xcode6.4** :


    
    
    # sudo xcode-select -switch /Applications/Xcode6.4.app
    

  * больше двигаться в сторону целевого процессора ARM (iOS).



### Xcode 5.0

Apple удалила "gdb" из своих бинарных утилит. В то время Lazarus мог использовать только "gdb" в качестве внешнего отладчика. "Gdb" должен быть установлен от стороннего разработчика. См. [GDB в macOS](<../en/GDB_on_OS_X_Mavericks_or_newer_and_Xcode_5_or_newer.md> "GDB on OS X Mavericks or newer and Xcode 5 or newer"). 

## Внешние ссылки

  * [Apple: Technical Note TN2339](<https://developer.apple.com/library/archive/technotes/tn2339/_index.html>) Building from the Command Line with Xcode FAQ.

---

_Source: [https://wiki.freepascal.org/Xcode/ru](https://web.archive.org/web/20250215131122/https://wiki.freepascal.org/Xcode/ru)_
