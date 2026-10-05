# Qt5 Interface

│ **[English (en)](<../en/Qt5_Interface.md> "Qt5 Interface")** │  **русский (ru)** │    
****

[![Qt logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Qt_logo_2013.svg/50px-Qt_logo_2013.svg.png)](</File:Qt_logo_2013.svg>)

Эта статья относится только к [Qt5 widgetset](</Category:Qt> "Category:Qt").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Введение
  * 2 Windows
    * 2.1 Запуск и развертывание
  * 3 Linux
    * 3.1 Lazarus
    * 3.2 libqt5pas
      * 3.2.1 Проблема с версией?
    * 3.3 Системы, использующие Wayland
    * 3.4 Цвета Qt5
    * 3.5 Почему Qt5, а не GTK?
  * 4 macOS (64-бит)
  * 5 Пример проекта
  * 6 Other Interfaces
    * 6.1 Platform specific Tips
    * 6.2 Interface Development Articles



## Введение

Этот интерфейс основан на Qt 5 (тестируется Qt 5.6.2). Для получения документации, исправлений и загрузки перейдите на [Project](<http://qt-project.org/Qt>) (установщик доступен на [странице загрузки 5.6.2](<https://download.qt.io/new_archive/qt/5.6/5.6.2/>)). Lazarus с интерфейсом Qt5 (qt5-lcl) можно использовать в Windows 32/64, Linux x32/x64/arm, macOS x64 (Cocoa). Наборы виджетов Qt5 были доступны в Lazarus, начиная со стабильной версии 1.8. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** С недавних пор российские IP стали баниться. Такова печальная реальность. Поэтому лучше используйте прокси и скачивайте [оффлайн инсталлятор](<https://www.qt.io/offline-installers>). **Устанавливать** с него необходимо **при отключенном интернете** и именно по той же причине

Большинство современных дистрибутивов Linux имеют подходящий Qt5 в своих стандартных репозиториях, однако выпуски с долгосрочной поддержкой имеют проблемы из-за их возраста. Ubuntu 16.04 имеет слишком старый QT5.5, а 18.04 содержит проблемную версию libQt5Pas, которую необходимо заменить, см. ниже. 

* * *

Также подробнее об установке "с нуля" под Windows и Linux можно почитать [здесь](<https://github.com/zoltanleo/fpc_lazarus_notes/blob/main/README.md#installing_qt_lazarus>) (подойдет также для qt6) 

* * *

## Windows

Бинарный файл _Q5Pas1.dll_ доступен здесь 

  * x86: <https://gitlab.com/freepascal.org/lazarus/binaries/-/raw/main/i386-win32/qt5/Qt5Pas1.dll?inline=false>



а вот готовой 64-битной версии нет. 

Сборка основана на MinGW, поэтому вы можете использовать библиотеку MinGW Qt (например, qt-opensource-windows-x86-mingw492-5.6.2.exe). 

Если вам нужно собрать проект **cbindings** , вам понадобится **MinGW**. Вы можете использовать **MinGW** из пакета Qt (qt-opensource-windows-x86-mingw492-5.6.2.exe) — это необязательный компонент установки пакета Qt. 

### Запуск и развертывание

Проект должен быть развернут с использованием _Q5Pas1.dll_. 

Файлы .dll Qt5, которые необходимо развернуть, см. в руководствах по Qt5: <https://doc.qt.io/qt-5/windows-deployment.html>. 

Для пакета на основе MinGW потребуется следующий набор библиотек DLL для запуска одного проекта FORM. 

Эти библиотеки DLL поставляются с пакетом Qt5 (можно найти в C:\QtQt5.6.2\5.6\mingw49_32\bin). 

    Qt5Core.dll
    Qt5PrintSupport.dll
    Qt5Widgets.dll
    Qt5Gui.dll
    Qt5Network.dll
    libstdc++-6.dll
    libwinpthread-1.dll
    libgcc_s_dw2-1.dll

Для сборок, основанных на MSVC, а не на MinGW, может потребоваться другой набор dll(не Qt5). Обратите внимание, что [Qt5Pas1.dll](<https://gitlab.com/freepascal.org/lazarus/binaries/-/raw/main/i386-win32/qt5/Qt5Pas1.dll?inline=false>) собран для Qt5-5.6.2, но Qt5Pas1.dll можно использовать с любым Qt5 > 5.6.2. 

## Linux

### Lazarus

[![](https://wiki.freepascal.org/images/4/46/SelectQt5.png)](</File:SelectQt5.png>)

[](</File:SelectQt5.png> "Enlarge")

Image of Qt5 selected

Lazarus «из коробки» в Linux использует и создает приложения для GTK2. Он не помечен как имеющий зависимость от Qt5, поэтому, если вы хотите создавать приложения Qt5 или собирать версию Lazarus для Qt5, вам, вероятно, потребуется установить «libQt5Pas-dev», используя репозиторий вашего дистрибутива: 

  * Fedora, Mageia - sudo dnf install qt5pas-devel <enter>
  * Ubuntu, Debian — sudo apt install libqt5pas-dev <enter>



это добавит необходимые библиотеки Qt5 в качестве зависимостей, обычно около 50 МБ в системе, в которой нет существующих потребностей Qt5. Если вы хотите использовать версию Lazarus для Qt5, выполните сборку из исходного кода с помощью такой команды, как 
    
    
     make bigide LCL_PLATFORM=qt5 <enter>
    

но обратите внимание, что по умолчанию GTK2 Lazarus с удовольствием создаст для вас приложения Qt5. 

В Lazarus, чтобы выбрать сборку приложения Qt5, сделайте так: Projects --> ProjectOptions --> Additions and Overrides, нажмите Set "LCLWidgetType" и выберите Qt5 для текущего режима сборки. А еще лучше добавить специальный режим сборки для Qt5, может быть, релиз и отладку. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** если вы устанавливаете Lazarus, используя версию Lazarus вашего дистрибутива, вам может потребоваться специально запросить компоненты Qt5 LCL. Если вы устанавливаете из исходного кода, вы получите намного больше с самого начала.

Не забудьте, что при релизе вашего приложения оно должно быть помечено как зависимое от libqt5pas1. 

### libqt5pas

_libqt5pas_ — это библиотека, связывающая Qt5 и ваше приложение Lazarus. Более новые дистрибутивы (U20.04, Debian Buster и т.д.) имеют рабочие версии, доступные в их репозиториях. Отметьте свое приложение как зависимое от libqt5pas1 и все будет в порядке. Вам, как разработчику, использующему Qt5, также понадобится libqt5pas-dev, как упоминалось выше. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** если вы используете версию Lazarus более позднюю, чем 2.2.0, вам (и вашим конечным пользователям) потребуется libQt5Pas выше или равной 1.2.10. Дистрибутивам потребуется некоторое время, чтобы обновить версию, которую они распространяют, поэтому либо создайте свою собственную, либо используйте неофициальные <https://github.com/davidbannon/libqt5pas/releases/latest> debs и rpms, как указано ниже. Помните, что вашим конечным пользователям тоже нужна версия 1.2.10!

Использование вашего репозитория дистрибутивов - 

  * Fedora, Mageia - sudo dnf install qt5pas<enter>
  * Ubuntu, Debian - sudo apt install libqt5pas1 <enter>



  


#### Проблема с версией?

Версия libQt5Pas вызовет проблемы у следующих людей, помните, что это влияет на вас, как на разработчика, и на ваших конечных пользователей. 

  * Пользователи очень старых дистрибутивов Linux, таких как Ubuntu 16.04 - жаль, нет решения, не используйте Qt5, если вам нужна поддержка таких операционных систем.
  * Старые дистрибутивы Linux, такие как U18.04, официально поддерживаются до 2023 года, но используют неподходящий libQt5Pas. Вы увидите сбой, если ваше приложение использует TMemo. Замените существующий libQt5Pas.
  * Текущие дистрибутивы LTS, такие как 20.04 (и, возможно, 22.04, Debian Bullseye и т. д.), будут иметь libQt5Pas более ранней, чем 1.2.10, и это проблема, если вы используете версию Lazarus более позднюю, чем 2.2.0. Опять же, вы можете заменить существующий libQt5Pas



**Замена своего libQt5Pas?**

Два варианта: используйте Deb- или RPM-пакеты из <https://github.com/davidbannon/libqt5pas/releases/latest> — вы должны убедиться, что ваш менеджер пакетов не возражает, или даже заменить ваш новый сияющий файл другим из предпочитаемого дистрибутива. Пишите на форуме, если вам нужно что-то кроме 64-битных Deb и RPM (Signing, 32bit, Pacman и т.д.). Установите, например, с помощью 
    
    
     sudo apt install ./libqt5pas1_2.10-0_amd64.deb <enter> 
    

Как разработчику, вам также понадобится dev-пакет, удалите существующий, если он есть. 

Во-вторых, в исходниках Lazarus есть код для создания собственной библиотеки, вам понадобится gcc и несколько других вещей, задокументированных в README.txt. Затем скопируйте новую библиотеку поверх существующей libQt5Pas1 (конечно, как root) 

_Примечание 1_ Эти два пакета deb созданы для Ubuntu 18.04 (или более поздней версии), они совершенно не помогут в Ubuntu 16.04, так как его основные библиотеки Qt5 слишком устарели. 

_Примечание 2_ Ubuntu будет постоянно перезаписывать эту пару обновленных пакетов с помощью инструмента «автоматического обновления». Вы должны занести пакеты в черный список, чтобы средство автоматического обновления не испортило вашу хорошую работу. 

### Системы, использующие Wayland

Некоторые дистрибутивы используют Wayland по умолчанию (например, Fedora по умолчанию с рабочим столом GNOME), а QT5 не работает с Wayland. Вы получите сообщение вида: 
    
    
    QSocketNotifier: Can only be used with threads started with QThread
    [FORMS.PP] ExceptionOccurred
    ....
    

Выйдите из системы, на экране входа щелкните маленький значок шестеренки, выберите «GNOME on XOrg». Войдите снова. 

Есть, видимо, плагин Wayland для Qt5, для смелых - <https://wayland.freedesktop.org/qt5.html>

### Цвета Qt5

В Linux иногда приложения Qt5 наследуют используемые цвета от рабочего стола/ОС. Однако это не всегда работает и, по-видимому, зависит от дистрибутива/рабочего стола. С нынешней модой на темные темы более надежным подходом является использование qt5ct. qt5ct присутствует в репозитории большинства дистрибутивов, и его можно быстро и легко установить. Когда вы запускаете его, он открывается на вкладке «Внешний вид», для темной темы выберите «Палитра = Пользовательская», «Цветовая схема» = «Темная». Нажмите «Применить» и «ОК», чтобы закрыть приложение. 

[![qt5ct dark.png](https://wiki.freepascal.org/images/f/f1/qt5ct_dark.png)](</File:qt5ct_dark.png>)

Вы заметили сообщение вверху «Приложение настроено неправильно»? Его беспокоит то, что вы не добавили настройку переменной среды, которая укажет вашему приложению использовать только что установленные цвета. Вы можете задать его только для одного приложения, установив его в командной строке приложения: 
    
    
    QT_QPA_PLATFORMTHEME=qt5ct myapplication

Это может быть добавлено в файл рабочего стола приложения или, возможно, вы отредактируете пункт меню. Альтернативой является добавление его в /etc/environment (очевидно, без имени приложения), чтобы оно автоматически устанавливалось для всех приложений, вам нужно выйти из системы и снова войти. Добавление записи в собственный .bashrc не совсем уместно, поскольку приложения с графическим интерфейсом, запускаемые, например, из меню, не будут об этом уведомлены. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** В Debian при установке qt5ct автоматически добавляется QT_QPA_PLATFORMTHEME в переменные среды.

### Почему Qt5, а не GTK?

Хороший вопрос! Большинство приложений Linux Lazarus создаются с использованием GTK2 по умолчанию. Различные дистрибутивы Linux предоставляют все меньше и меньше приложений GTK2, обычно «обновляя» их до GTK3. Тем не менее, интерфейс Lazarus GTK3 все еще нуждается в некоторой доработке, и он еще не готов для производственных систем. До конца 2019 года это не имело большого значения, GTK2 работает нормально. Но Ubuntu19.10 не имеет библиотек GTK2, установленных по умолчанию, и Xfce объявил, что его следующий выпуск будет свободен от GTK2, по-видимому, многие дистрибутивы хотят исключить GTK2 из своих наборов по умолчанию. Хотя вы все еще можете установить GTK2, он довольно большой.... 

Qt5 — очевидное решение, оно уже установлено в таких дистрибутивах, как Fedora 30 plus, это относительно легкое дополнение к Ubuntu, всего 48 мегабайт по сравнению с 445 мегабайтами GTK2. Если вы пометите свое приложение как зависимое от libqt5pas, оно будет установлено одновременно с другими файлами qt5, если это необходимо. Ручная установка libqt5pas с помощью вашего менеджера пакетов (при условии, что он разрешает зависимости!) также будет работать. 

Очевидно, что дистрибутивы на основе Qt, использующие Plasma или KDE в качестве рабочего стола, потребуют только _libqt5pas_. 

## macOS (64-бит)

Следующие шаги были протестированы с macOS 10.13.6 (High Sierra), инструментами командной строки Xcode 10.0.0 (Xcode не требуется), Lazarus 1.8.4 и минимальной версией Qt 5.6.2 (Qt 5.12.xx тоже работает без проблем). 

После установки Qt 5.6.2 (см. выше) откройте окно терминала и выполните следующие команды (пути могут отличаться): 
    
    
    export LazarusDir=/Developer/Lazarus
    export QtDir=~/Qt5.6.2
    cd $LazarusDir/lcl/interfaces/qt5/cbindings
    export PATH=$QtDir/5.6/clang_64/bin:$PATH
    qmake
    

Если **qmake** завершается со следующей ошибкой: 
    
    
    xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance
    

это **не означает** , что вы должны установить Xcode. См. обходной путь [Qt без Xcode](<https://gist.github.com/shoogle/750a330c851bd1a924dfe1346b0b4a08>) и попробуйте снова запустить `qmake`. (Зависимость от Xcode, кажется, исправлена ​​в Qt 5.9.4 или, возможно, ранее.) 

Продолжайте процесс сборки с помощью следующих команд: 
    
    
    make
    make install
    

Может быть полезно добавить символические ссылки в Qt5Pas.framework и другие файлы *.framework (чтобы не требовалось никаких изменений в пути): 
    
    
    cd /Library/Frameworks/
    sudo ln -s $QtDir/5.6/clang_64/lib/*.framework .
    

Помните, что Qt5 является 64-битным, поэтому в Lazarus в разделеTools - Options - Environment вам нужно изменить исполняемый файл компилятора с `/usr/local/bin/ppc386` на `/usr/local/bin/ppcx64`. При использовании ppc386 вы позже получите следующие ошибки при компиляции ваших проектов Lazarus: 
    
    
    Error: linker: Undefined symbols for architecture i386:
    Error: linker: "_QAbstractButton_click", referenced from:
    [...]
    ld: symbol(s) not found for architecture i386
    

Наконец, создайте новый проект Lazarus (или откройте существующий), откройте Project - Project options - Compiler options - Additions and overrides, установите `LCLWidgetType:=qt5`. Теперь вы сможете скомпилировать свой проект с набором виджетов Qt5. 

## Пример проекта

См. [Первые шаги с интерфейсом Lazarus Qt5](<https://tondrej.blogspot.com/2018/04/first-steps-with-lazarus-qt5-interface.html>) для примера проекта для Win32, Win64, macOS (64-разрядная версия) и Linux (64 бит). 

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<../en/Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<../en/Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<../en/Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<../en/Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<../en/Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<../en/Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<../en/Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<../en/GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<../en/GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<../en/GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<../en/fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<../en/Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<../en/Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<../en/iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<../en/FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<../en/Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<../en/macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<../en/WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<../en/Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<../en/Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<../en/Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<../en/LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<../en/LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<../en/Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset

---

_Source: [https://wiki.freepascal.org/Qt5_Interface/ru](https://web.archive.org/web/20250219053431/https://wiki.freepascal.org/Qt5_Interface/ru)_
