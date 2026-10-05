# Installing Lazarus on macOS

│ **[English (en)](<../en/Installing_Lazarus_on_macOS.md>)** │  **русский (ru)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

Установка Lazarus на Mac не особенно сложна, но очень важно выполнить установку в правильном порядке. Пропуск шагов почти наверняка приведет к плачу Ярославны. Вкратце, вот что вы должны сделать - 

  1. Загрузить и установить Xcode.
  2. Установить глобальные инструменты командной строки для Xcode.
  3. Установить Free Pascal Compiler и исходники FPC
  4. Установить Lazarus
  5. Настроить LLDB - поставляемый (и подписанный) Apple отладчик от Lazarus.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** если вы устанавливаете версии Lazarus до 2.0.0, вам почти наверняка понадобится gdb, см. раздел [Legacy](<../en/Legacy_Information__Installing_Lazarus_on_Mac.md> "Legacy Information: Installing Lazarus on Mac").

  


## Contents

  * 1 Установка
    * 1.1 Шаг 1. Загрузка Xcode (необязательно).
    * 1.2 Шаг 2. Инструменты командной строки Xcode
    * 1.3 Шаг 3 Бинарные файлы FPC и исходный код FPC
    * 1.4 Установка Lazarus
    * 1.5 Настройка отладчика
      * 1.5.1 Дополнительная информация по использованию LLDB
      * 1.5.2 Установка LazDebuggerFpLLdb
  * 2 Cocoa 64-бит в сравнении с Carbon 32-бит
  * 3 Матрица совместимости FPC + Lazarus
  * 4 Установка невыпущенных версий Lazarus IDE
    * 4.1 Lazarus Fixes 2.2
      * 4.1.1 Загрузка исходного кода Lazarus Fixes 2.2
      * 4.1.2 Обновление исходного кода Lazarus Fixes 2.2
      * 4.1.3 Сборка исходного кода Lazarus Fixes 2.2
    * 4.2 Разрабатываемая версия Лазаруса
      * 4.2.1 Скачивание исходного кода разрабатываемой версии Lazarus
      * 4.2.2 Обновление исходников версии Lazarus для разработки
      * 4.2.3 Сборка исходного кода версии Lazarus для разработки
    * 4.3 Что делает аргумент make bigide?
  * 5 Установка незарелизенных версий FPC
    * 5.1 Установка из исходников
      * 5.1.1 FPC веросия для разработчиков
      * 5.1.2 FPC Fixes 3.2
  * 6 Известные проблемы и решения
    * 6.1 Lazarus IDE v2.2.0 - проблема пересборки
      * 6.1.1 Следующие ошибки пересборки не возникают на компьютерах Intel
    * 6.2 Lazarus IDE - Unable to "run without debugging"
    * 6.3 Обновление с Mojave (10.14) до Catalina (10.15)
    * 6.4 Сборка компилятора FPC, начиная с Mojave (10.14) и далее
    * 6.5 ЧаВо по установке на Mac
  * 7 Деинсталляция Lazarus и Free Pascal
  * 8 Устаревшая информация
  * 9 См. также
  * 10 Внешние ссылки



## Установка

В подробных инструкциях предполагается, что на вашем Mac установлена последняя версия macOS, последняя версия Xcode от Apple и последняя версия Lazarus. Внизу страницы, под устаревшей документацией, вы увидите более старую информацию, которая может иметь значение, если вы используете старые компоненты. Вы можете помочь, заменив устаревшую информацию, либо удалив ее, либо, если это может помочь кому-то, работающему с унаследованным проектом, переместить ее в конец страницы. 

В общем, речь идет об использовании набора виджетов **Carbon** и **Cocoa**. Хотя Carbon все еще можно считать немного более стабильным, на момент выпуска 2.0.0 64-битное Cocoa очень близко [к стабильному] и, безусловно, его следует учитывать. Carbon намеренно (Apple) ограничен 32 битами, и мы знаем, что следующая версия MacOSX, вероятно, не будет его поддерживать. 

### Шаг 1. Загрузка Xcode (необязательно).

Xcode — это загрузка 12ГБ архива, которая займет после распаковки 16ГБ дискового пространства. Имеет смысл заружать и устанавливать полную среду разработки Xcode, **только если** вам нужны: 

  * SDK для **iOS** , **iPadOS** , watchOS и tvOS; или
  * для проверки и загрузки приложений в Mac App Store; или
  * [нотариально заверять](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+/ru") приложения для распространения за пределами Mac App Store.



Xcode 11.3.1 для использования в macOS 10.14 Mojave теперь необходимо устанавливать, загрузив его из [Apple Developer Connection](<http://developer.apple.com/>) (ADC), для чего требуется бесплатная регистрация. Xcode 12.4.x для использования в macOS 10.15 Catalina и более поздних версиях можно установить из магазина приложений [Mac App store](<https://apps.apple.com/us/genre/mac-developer-tools/id12002?mt=12>). Обратите внимание, что вы должны сначала переместить все старые версии Xcode из папки «Приложения» в корзину или переименовать приложение Xcode (например, Xcode.app в Xcode_1014.app). Затем вы можете выбрать, какую версию Xcode использовать с помощью утилиты командной строки `xcode-select`. Откройте Applications > Utilities > Terminal (Приложения > Инструменты > Терминал) и введите` man xcode-select` для показа страниц руководства по этой утилите. 

**Старые системы:**

Инструменты разработчика Xcode можно установить с исходных установочных дисков macOS или более новой копии, загруженной из [Apple Developer Connection](<http://developer.apple.com/>) (ADC), для чего требуется бесплатная регистрация. Загрузите файл Xcode, он окажется в вашем каталоге загрузок в виде zip-файла. Нажмите на нее. Он разархивируется в каталог Downloads(Загрузки). Вам может повезти, а может быть и нет. Другие пользователи увидят путь к нему, но не смогут его использовать. А это - непорядок. Чтобы исправить это недоразумение, я переместил свой, а затем указал `xcode-select`, куда он был перемещен (в терминале) - 
    
    
    mv Downloads/Xcode.app /Developer/.
    sudo xcode-select -s /Developer/Xcode.app/Contents/Developer
    

### Шаг 2. Инструменты командной строки Xcode

Это раздел показан как отдельный шаг, потому что это действительно отдельный шаг. Не путайте эти автономные инструменты командной строки с внутренними инструментами командной строки Xcode, о которых графический интерфейс Xcode сообщит вам, что они уже установлены, если вы установили полный пакет Xcode на шаге 1. FPC не может использовать эти внутренние инструменты командной строки Xcode без изменений конфигурации ( подробности см. в [Xcode](<../en/Xcode.md> "Xcode")). 

Сделайте следующее, это быстро и легко для всех версий macOS до Catalina 10.15 включительно: 
    
    
    sudo xcode-select --install
    sudo xcodebuild -license accept
    

  
Для Big Sur 11.x и более поздних версий вам нужно ввести только первую из двух приведенных выше команд, если вы также не установили полный пакет Xcode. Если вы установили только инструменты командной строки, вам не следует вводить команду _xcodebuild_. 

Если у вас возникли проблемы с установкой инструментов командной строки с использованием этого метода командной строки (например, программа установки зависает при «поиске программного обеспечения»), вы также можете загрузить и установить пакет инструментов командной строки, зайдя на [Apple Developer Site](<https://developer.apple.com/download/more/?=for%20Xcode>) и загрузив и установив образ диска _Command Line Tools for Xcode_. 

### Шаг 3 Бинарные файлы FPC и исходный код FPC

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Для установки Apple Silicon/AArch64/M1, если вы устанавливаете FPC 3.2.2 (Lazarus 2.2RCx или 2.2.0 или более поздние версии), вам не нужно компилировать собственную версию FPC для Apple Silicon, поскольку FPC 3.2.2 для macOS универсальный двоичный файл, содержащий исполняемые файлы Intel и aarch64. Если вы устанавливаете более раннюю, чем 3.2.2, версию FPC, обратитесь к [этим инструкциям](<../en/macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers") для создания собственного компилятора Apple Silicon Free Pascal после установки 64-разрядного двоичного файла Intel и исходных пакетов для FPC. 

Загрузите и установите официальный пакет двоичных файлов FPC и отдельный пакет исходного кода FPC из [области файлов Lazarus IDE](<https://sourceforge.net/projects/lazarus/files/>). 

Когда вы доберетесь до области файлов Lazarus IDE: 

  1. Выберите правильную версию вашей операционной системы. Подавляющее большинство пользователей Mac теперь должны выбирать 64-битные пакеты в каталоге Lazarus macOS x86-64. Каждый компьютер Mac с конца 2006 года поддерживает 64-разрядную версию. Тот факт, что Apple полностью отказалась от 32-битной поддержки в macOS 10.15 Catalina (выпущенной в октябре 2019 года), является еще одной причиной для выбора 64-битных пакетов.
  2. Выберите версию Lazarus, которую вы хотите установить, и вам будут представлены два бинарных и исходных пакета FPC для загрузки.



Эти установочные пакеты создаются разработчиками FPC/Lazarus и отслеживают официальные выпуски. Поскольку эти установочные пакеты не одобрены Apple, вам нужно, удерживая нажатой клавишу Control, щелкнуть пакет, выбрать «Открыть» и подтвердить, что вы хотите установить его от неизвестного разработчика (Unknown Developer). 

На этом этапе вы можете попробовать простой и быстрый тест FPC — [Тестирование установки FPC](<../en/Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler"). 

### Установка Lazarus

Крайне важно, чтобы совместимый компилятор Free Pascal (FPC) и его исходный код был установлен _перед установкой_ Lazarus IDE. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Для установки на Apple Silicon/AArch64 обратитесь к инструкциям Lazarus Fixes 2.2 или Lazarus development, чтобы создать собственную IDE Lazarus. Перейдите к инструкциям по сборке и используйте загрузку исходного кода Lazarus Fixes 2.2/Lazarus development по своему усмотрению. 

Загрузите и установите Lazarus IDE из [области файлов Lazarus IDE](<https://sourceforge.net/projects/lazarus/files/>). Когда вы доберетесь до этой области файлов, выберите правильную версию вашей операционной системы. Подавляющее большинство пользователей Mac теперь должны выбирать 64-битные пакеты в каталоге Lazarus macOS x86-64. Каждый компьютер Mac с конца 2006 года поддерживает 64-разрядную версию. Тот факт, что Apple полностью отказалась от 32-битной поддержки в macOS 10.15 Catalina (выпущенной в октябре 2019 года), является еще одной причиной для выбора 64-битных пакетов. 

### Настройка отладчика

В версиях Lazarus 1.8.4 и более ранних версиях вам нужно было использовать gdb в качестве отладчика, медленно устанавливать и тяжело подписывать. Начиная с Lazarus 2.0.0 вы можете (и должны) использовать LLDB, отладчик, предоставляемый Apple, подпись [для которого] не требуется. 

Предполагая, что вы установили все необходимое и запустили Lazarus, остается только настроить отладчик. Если вы не сделаете этого сейчас, Lazarus попытается использовать GDB и потерпит неудачу. 

Сначала нажмите Tools(Инструменты)->Options(Параметры)->Debugger(Отладчик). В правом верхнем углу открытого окна есть надпись "Debugger type and path"(Тип и путь отладчика), вы должны задать оба [значения]. Выберите "LLDB debugger (with fpdebug) (Beta)"(Отладчик LLDB (с помощью fpdebug) (Beta)). 

[![Set Debugger2.png](https://wiki.freepascal.org/images/d/d5/Set_Debugger2.png)](</File:Set_Debugger2.png>)

Если его нет в списке выбора, см. ниже (Установка LazDebuggerFpLLdb). Обратите внимание, что изображение выше имеет путь к LLDB, который может отличаться от вашего, в зависимости от того, где были установлены инструменты XCode, в моем случае /Library. Сохраните эти настройки, и теперь вы можете попытаться скомпилировать практически пустую программу, которую Lazarus любезно предоставил для вас (щелкните маленький зеленый треугольник в верхнем левом углу). 

Далее вы видите загадочный вопрос, см. изображение ниже. Выберите "Debug Format"(формат отладки) из предложенных - 

[![Set Dwarf.png](https://wiki.freepascal.org/images/0/03/Set_Dwarf.png)](</File:Set_Dwarf.png>)

Martin_fr (человек, который дал нам этот интерфейс между Lazarus и LLDB) предлагает вам использовать "dwarf3". Затем вам нужно ввести свой пароль: MacOSX хитроумна, потому что одно приложение мешает другому. В этом случае это хорошо! 

#### Дополнительная информация по использованию LLDB

В [этой ветке](<https://forum.lazarus.freepascal.org/index.php/topic,42869.0.html>) форума появляется много информации об использовании LLDB. Вот еще несколько самоцветов от Martin_fr: 

В случае непредвиденных проблем стоит попробовать "dwarf with sets" взамен используемому "dwarf3". 

Параметр "debug info" влияет только на модули непосредственно в вашем проекте. Однако модули в пакетах также могут иметь отладочную информацию. Это может быть 

  * установка в пакете
  * для многих, но не для всех пакетов в меню Tools(Сервис) > Configure build Lazarus(Настройка сборки Lazarus)
  * project settings (настройки проекта) > [Дополнения и переопределения](<../en/IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")



Если вы измените настройки для пакета, вы также можете проверить, в какой пакет вы планируете перейти. Пакет, в который вы не входите, не требует отладочной информации. Если вы используете тип из пакета (такой, как TForm из LCL), то достаточно, чтобы ваш модуль (в котором вы объявляли переменную и(или) использовали переменную для включения типа) имел отладочную информацию. Уменьшение количества пакетов с отладочной информацией (в том числе тех, которые по умолчанию имеют отладочную информацию) может сократить время запуска отладчиков. 

Также, возможно, стоит сравнить (не проверено) время запуска отладчиков для тех же настроек, только изменив флажок "use external debug info" (использовать внешнюю информацию отладки). 

Это должно быть установлено только в вашем проекте. Если установлен в вашем проекте, это повлияет на все пакеты. (Если он установлен в пакете, он ничего не сделает/по крайней мере, не должен ...) 

#### Установка LazDebuggerFpLLdb

Если вы установились из исходного кода и [при этом] использовали параметр bigide [утилиты make], то правильный отладчик будет установлен, как пакет, и [сразу будет] готов к работе. Однако, если вы установили другим способом, [отладчика] там может и не оказаться. На главном экране IDE выберите Packages(Пакеты) -> Install/Uninstall Packages (Установить/удалить пакеты). Show - это два списка пакетов, слева - список уже установленных, справа - доступных для установки. Ищите LazDebuggerFpLldb (именно его, есть некоторые схожие названия, но менее подходящие пакеты). Если он справа, щелкните его, нажмите "Install Selection" (Установить выделенное), а затем "Save and rebuild IDE" (Сохранить и пересобрать IDE). Это займет немного времени, IDE выключится и перезапустится, и все должно быть хорошо. Теперь вернитесь на страницу и продолжите настройку отладчика. 

## Cocoa 64-бит в сравнении с Carbon 32-бит

Использование 64-битной платформы Cocoa от Apple теперь, несомненно, является будущим для Mac. 32-разрядная платформа Apple Carbon, хотя больше не разрабатывается, но работает почти так же, как и ожидалось. Однако мы бы порекомендовали сначала попробовать Cocoa, поскольку Apple прекратила поддержку 32-разрядных приложений и 32-разрядной платформы Carbon в macOS 10.15 Catalina (октябрь 2019 г.) . 

Можно собрать Cocoa-версию Lazarus IDE, начиная с версии 2.0.0 и выше. Также можно собрать Carbon-версию Lazarus IDE (если вы не используете macOS 10.15 Catalina или более позднюю версию) и использовать ее для создания 64-битных двоичных файлов Cocoa. 

Чтобы создавать приложения Cocoa в Carbon или Cocoa IDE, вам необходимо установить Target для 64-битного процессора и выбрать набор виджетов Cocoa: 

  * Откройте свой проект с помощью Lazarus и в меню выберите Project > Project Options.
  * На панели "Config and Target" (Конфигурация и цель) установите для "Target CPU family"(Целевое семейство ЦП) значение "x86_64" (Intel) или "aarch64" (Apple M1).
  * На панели "Config and Target" (Конфигурация и цель), если "Current LCL widgetset"(Текущий набор виджетов LCL) не установлен для Cocoa, нажмите "Select another LCL widgetset" (Выбрать другой набор виджетов LCL), который перенесет вас на панель "Additions and Overrides" (Дополнения и перекрытия), где вы можете нажать выпадающее меню "Set LCLWidgetType"(Присвоить "LCLWidgetType") и установите значение "Cocoa"
  * Убедитесь, что в разделе Tools (Сервис) > Options(Настройки) (> Preferences (Параметры) для Lazarus в версии 2.2.0 и более поздних версиях) для параметра "Compiler Executable" (Исполнимый файл компилятора) установлено значение «/usr/local/bin/fpc», чтобы получить 64-разрядные приложения.
  * Теперь скомпилируйте свой проект и, пожалуйста, сообщите о любых проблемах, с которыми вы столкнулись.



## Матрица совместимости FPC + Lazarus

Не всякая комбинация [версий] Lazarus и Free Pascal совместима с любой установкой macOS. Пожалуйста, обратитесь к следующей таблице, чтобы найти правильную версию для вашей среды разработки: 

Lazarus Compatibility Matrix | Lazarus 1.8.x  | Lazarus 2.0.y  | Lazarus 2.0.8 | Lazarus 2.0.10  | Lazarus 2.0.12 | Lazarus 2.2.y   
---|---|---|---|---|---|---  
| FPC 3.0.4  | FPC 3.2.0  | FPC 3.2.2   
PPC processors   
Mac OS X 10.4 (Tiger)‡ | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Mac OS X 10.5 (Leopard)‡ | Not tested | Not tested | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Intel processors   
Mac OS X 10.4 (Tiger)‡ | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Mac OS X 10.5 (Leopard)  | Not tested | Compatible^ | Not tested | Compatible^**† | Not tested | Not tested | Not tested   
Mac OS X 10.6 (Snow Leopard)  | Compatible | Compatible^^ | Not tested | Not tested | Not tested | Not tested | Not tested   
Mac OS X 10.7 (Lion)  | Compatible | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
OS X 10.8 (Mountain Lion)  | Compatible^^ | Compatible | Compatible**# | Compatible**# | Compatible**# | Compatible# | Compatible   
OS X 10.9 (Mavericks)  | Compatible^^ | Compatible | Compatible**† | Compatible**† | Not tested | Not tested | Not tested   
OS X 10.10 (Yosemite)  | Compatible^^ | Compatible | Compatible**† | Compatible**† | Not tested | Not tested | Not tested   
OS X 10.11 (El Capitan)  | Compatible^^ | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Comptaible   
macOS 10.12 (Sierra)  | Compatible^^ | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Compatible   
macOS 10.13 (High Sierra)  | Not tested | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Comptaible   
macOS 10.14 (Mojave)  | Not tested | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Compatible   
macOS 10.15 (Catalina)  | Not tested | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Compatible   
macOS 11 (Big Sur)  | Not tested | Compatible | Compatible***† | Compatible† | Compatible† | Compatible† | Compatible   
macOS 12 (Monterey)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible   
macOS 13 (Ventura)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible   
macOS 14 (Sonoma)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible   
Apple Silicon M series processors   
macOS 11 (Big Sur)  | Not tested | Not tested | Not tested | Compatible†† | Compatible†† | Compatible††† | Compatible*   
macOS 12 (Monterey)  | Not tested | Not tested | Not tested | Compatible†† | Compatible†† | Compatible††† | Compatible*   
macOS 13 (Ventura)  | Not tested | Not tested | Not tested | Compatible†† | Compatible†† | Compatible††† | Compatible*   
macOS 14 (Sonoma)  | Not tested | Not tested | Not tested | Compatible†† | Compatible†† | Compatible††† | Compatible*   
  
**x** = 0, 2 or 4; **y** = 0, 2, 4 or 6 

^ Carbon interface compiles - Cocoa does not. 

^^ [Restrictions](<../en/GDB_on_OS_X_Mavericks_and_Xcode_5.md> "GDB on OS X Mavericks and Xcode 5") apply to debugging with [gdb](<../en/GDB.md> "GDB"). 

* Lazarus 2.2.0 installs universal binaries for FPC 3.2.2, but an Intel Lazarus IDE binary which you can use or recompile the IDE from within itself for a native aarch64 version. 

** See [Installing Lazarus 2.0.8, 2.0.10 with FPC 3.2.0 for macOS 10.10 and earlier](<../en/Legacy_Information__Installing_Lazarus_on_Mac.md> "Legacy Information: Installing Lazarus on Mac") for instructions. 

*** See [Installing Lazarus 2.0.8 with FPC 3.2.0 for macOS 10.11+](<../en/Legacy_Information__Installing_Lazarus_on_Mac.md> "Legacy Information: Installing Lazarus on Mac") for instructions. 

# Cannot run without debugging in the IDE. Can run compiled application outside of the IDE. See [Issue #37324](<https://bugs.freepascal.org/view.php?id=37324>). Choose the gdb debugger, change timeout option to false or click through five "timeout" dialogs to run with debugging in the IDE. 

† Cannot "run without debugging" in the IDE. Can run compiled application outside of the IDE. See [Lazarus IDE - Unable to "run without debugging"](<../en/Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") for workaround. See [Issue #36780](<https://bugs.freepascal.org/view.php?id=36780>). 

†† You need to compile a native aarch64 version of FPC 3.3.1 (trunk) and Lazarus 2.0.12 from source to support an Apple Silicon M series processor. Refer to [these instructions](<../en/macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers") for FPC and [these instructions](<../en/Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") for the Lazarus IDE. 

††† After installing FPC 3.2.2, you need to compile a native aarch64 version of Lazarus from source to support an Apple Silicon M series processor. Refer to [these instructions](<../en/Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") for compiling the Lazarus IDE. 

‡ See the [legacy version of this compatibility matrix](</Template:Compatibility_matrix_of_Lazarus_for_Mac_Legacy> "Template:Compatibility matrix of Lazarus for Mac Legacy") for recommended installs on very old versions of macOS. 

## Установка невыпущенных версий Lazarus IDE

[Прим.перев](</User:Zoltanleo> "User:Zoltanleo"): настоятельно советую для НЕрелизных сборок компилятора и Лазаруса использовать [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe/ru")

### Lazarus Fixes 2.2

  * [Fixes included in fixes_2_2](<../en/Lazarus_2.md> "Lazarus 2.2 fixes branch")



Есть ряд причин, по которым вам может быть лучше использовать нерелизную версию Lazarus, в частности, fixes_2_2. Особенно: 

  * Вам почти наверняка нужно ориентироваться на 64-битный Cocoa, macOS 10.15 Catalina и более поздние версии вообще не поддерживают 32-битный Carbon.
  * Набор виджетов Cocoa постоянно улучшался, а интерфейс отладчика lldb быстро улучшался, начиная с версии 2.0.0.
  * Если на вашем Mac установлен процессор Apple M1, отладчик в fixes_2_2 теперь работает для aarch64.
  * Fixes_2_2 — более безопасный и стабильный вариант, чем ветка разработки, но все равно получает новые функции намного быстрее, чем релизная версия.



Предварительно скомпилированные двоичные файлы на основе fixes_2_2 недоступны в SourceForge, поэтому мы загружаем и собираем их из исходного кода. Сначала немного медленный, но очень надежный и отличный тест установки вашего компилятора! Вам понадобится git, который включен во все последние версии инструментов командной строки Xcode, которые вы уже должны были установить (см. [Инструменты командной строки Xcode](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS/ru") выше). 

Касательно svn или git: инструменты командной строки Xcode 11.4 в macOS 10.15 больше не устанавливают svn, только git. Вы можете установить Subversion через fink, порты или brew. 

**Additional notes for building a native aarch64 Lazarus IDE for an Apple Silicon M1 processor Mac**

Убедитесь, что вы скомпилировали собственный компилятор Free Pascal aarch64 (как это сделать, см. в разделе Поддержка Apple Silicon). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Выпуск FPC 3.2.2 включает в себя универсальные двоичные файлы, которые изначально будут работать как на aarch64 (64-разрядная версия ARM — Apple Silicon M1), так и на x86_64 (64-разрядная версия Intel), поэтому вам больше не нужно компилировать свои собственные из исходного кода.

При сборке Lazarus IDE измените CPU_TARGET в приведенных ниже инструкциях с x86_64 (64-разрядная версия Intel) на aarch64 (64-разрядная версия ARM — Apple Silicon M1). 

#### Загрузка исходного кода Lazarus Fixes 2.2

Создайте каталог для Lazarus и загрузите текущую версию исправлений: 
    
    
    cd
    mkdir -p bin/lazarus
    cd bin/lazarus
    

С использованием git: 
    
    
    git clone -b fixes_2_2 <https://gitlab.com/freepascal.org/lazarus/lazarus.git> laz_fixes
    

С использованием svn: 
    
    
    svn checkout <https://github.com/fpc/Lazarus/branches/fixes_2_2> laz_fixes
    

В зависимости от вашего интернет-соединения и загруженности сервера это может занять от нескольких секунд до нескольких минут. 

#### Обновление исходного кода Lazarus Fixes 2.2

Чтобы поддерживать установку fixes_2_2 в актуальном состоянии, достаточно просто: 

С использованием git: 
    
    
    cd ~/bin/lazarus/laz_fixes
    git clean -f -d
    git pull
    

С использованием svn: 
    
    
    cd ~/bin/lazarus/laz_fixes
    svn clean --remove-unversioned
    svn update
    

#### Сборка исходного кода Lazarus Fixes 2.2
    
    
    cd laz_fixes
    make distclean all LCL_PLATFORM=cocoa CPU_TARGET=x86_64 bigide
    //-wait some time...
    open startlazarus.app --args "--pcp=~/.laz_fixes"
    

  * Примечание. Я передаю параметр для использования каталога конфигурации, основанного на имени фактического каталога установки. Это упрощает написание некоторых сценариев.


  * В более старых версиях macOS, поддерживающих 32-битные приложения, замените приведенную выше строку make на "_make LCL_PLATFORM=carbon CPU_TARGET=i386 bigide_ " и настройте свой проект, как указано в разделе [Carbon в сравнении с Cocoa](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS/ru") выше.



Возможно, вы захотите поместить небольшой скрипт в каталог $HOME/bin и даже указать путь к нему (очень UNIX!) 
    
    
    #!/bin/bash
    LAZDIR="laz_fixes"
    cd ~/bin/lazarus/"$LAZDIR"
    open ~/bin/lazarus/"$LAZDIR"/lazarus.app --args "--pcp=~/.$LAZDIR"
    

### Разрабатываемая версия Лазаруса

  * [Примечания к разрабатываемой версии Lazarus](<../en/Lazarus_2.4.md> "Lazarus 2.4.0 release notes") \- Примечание: работа в процессе



Предварительно скомпилированные двоичные файлы, основанные на версии для разработки (известной как "trunk" в SVN; теперь известна как "main" в GIT) среды разработки Lazarus недоступны в SourceForge, поэтому можно загрузить исходный код версии для разработки с помощью _svn_ или _git_ и собрать IDE Lazarus. Вам понадобится svn (до macOS 10.15 — Catalina) или git (после macOS 10.14 — Mojave), который включен в автономные инструменты командной строки Xcode, которые вы уже должны были установить (см. Инструменты командной строки Xcode выше). 

  


  * When working with an Apple Silicon M1 processor Mac ensure you use the FPC 3.2.2 or later as it provides a universal binary with both a native aarch64 and Intel compiler. If you must use an earlier FPC than 3.2.2 see [Apple Silicon Support](<../en/macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers") for how to do this. When building the Lazarus IDE, change the **CPU_TARGET** in the instructions below from **x86_64** (Intel 64 bit) to **aarch64** (ARM 64 bit - Apple Silicon M1).



#### Скачивание исходного кода разрабатываемой версии Lazarus
    
    
    cd
    mkdir -p bin/lazarus
    cd bin/lazarus
    

С использованием git: 
    
    
    git clone -b main <https://gitlab.com/freepascal.org/lazarus/lazarus.git> laz_main
    

С использованием svn: 
    
    
    svn checkout --depth files <https://github.com/fpc/Lazarus/branches> all
    cd all
    svn update --set-depth infinity main
    

Приведенный выше вызов svn более сложен из-за бага во многих версиях Subversion. В качестве обходного пути рекомендуется использование утилиты Альфреда (он же Don Alfredo на [форуме](<../en/Forum.md> "Forum")), который является создателем [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe/ru"). 

#### Обновление исходников версии Lazarus для разработки

Чтобы обновить существующий исходный код версии Lazarus для разработки, следует сделать следующее: 

С использованием git: 
    
    
    cd ~/bin/lazarus/laz_main
    git clean -f -d
    git pull
    

С использованием svn: 
    
    
    cd ~/bin/lazarus/laz_trunk
    svn cleanup --remove-unversioned
    svn update
    

#### Сборка исходного кода версии Lazarus для разработки
    
    
    make distclean all LCL_PLATFORM=cocoa CPU_TARGET=x86_64 bigide
    //-wait some time...
    open startlazarus.app --args "--pcp=~/.laz_dev"
    

### Что делает аргумент make bigide?

Аргумент _bigide_ `make` добавляет в Lazarus кучу пакетов, которые многие находят полезными и без которых не могут обойтись. Пакеты, которые добавляются: 

  * cairocanvas
  * chmhelp
  * datetimectrls
  * externhelp
  * fpcunit
  * fpdebug
  * instantfpc
  * jcf2
  * lazcontrols
  * lazdebuggers

| 

  * lclextensions
  * leakview
  * macroscript
  * memds
  * onlinepackagemanager
  * pas2js
  * PascalScript
  * printers
  * projecttemplates
  * rtticontrols

| 

  * sdf
  * sqldb
  * synedit
  * tachart
  * tdbf
  * todolist
  * turbopower_ipro
  * virtualtreeview

  
---|---|---  
  
Приведенный выше список взят из _`[Lazarus source directory]/IDE/Makefile.fpc`_ и может быть изменен. 

Обратите внимание: если вы не скомпилировали свою собственную Lazarus IDE с аргументом bigide, вы можете установить любой из этих пакетов самостоятельно, используя диалоговое окно Lazarus IDE [_Package > Install/Uninstall Packages..._](<../en/IDE_Window__Installed_Packages.md> "IDE Window: Installed Packages")

## Установка незарелизенных версий FPC

### Установка из исходников

В настоящее время существует две незарелизенные ветки компилятора Free Pascal: ветка разработки (trunk) и ветка Fixes 3.2, которая включает дополнительные исправления выпущенной версии 3.2.2. Разработчики и те, кто любит находиться на переднем крае и тестировать новые функции и исправления, выберут версию для разработчиков; а обычные пользователи, которые хотят использовать стабильную ветку с некоторыми дополнительными исправлениями, начиная с последней версии выпуска, выберут ветку Fixes. Приведенные ниже инструкции охватывают обе эти ветви. 

Исходный код хранится в системе контроля версий под названием _git_ : 

  * macOS 10.5 и выше уже содержат git-клиент командной строки, если вы установили утилиты командной строки Xcode.


  * Вам также необходимо установить последнюю релизную версию компилятора Free Pascal (3.2.2 по состоянию на март 2022 г.), чтобы иметь возможность успешно скомпилировать разрабатываемую (trunk) версию.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** При сборке нативного компилятора Free Pascal под aarch64 (ppca64) для процессора Apple Silicon M1 Mac **измените CPU_TARGET** в приведенных ниже инструкциях с x86_64 (64-разрядная версия Intel) на aarch64 (64-разрядная версия ARM — Apple Silicon M1) и **измените любую ссылку** с ppcx64 на ppca64

#### FPC веросия для разработчиков

  * [Новые функции FPC trunk](<../en/FPC_New_Features_Trunk.md> "FPC New Features Trunk")
  * [Изменения для ползователей FPC trunk](<../en/User_Changes_Trunk.md> "User Changes Trunk") \- может сломать существующий код.



Обратите внимание, что поскольку версия FPC для разработки (известная как "trunk" в SVN; теперь известная как `main` в GIT) по определению все еще находится в стадии разработки, некоторые функции могут измениться до того, как они попадут в релизную версию. 

Создайте каталог, в который вы хотите поместить исходный код (например, fpc_main в вашем домашнем каталоге). Вам не нужно быть root, чтобы сделать это. Это может сделать любой нормальный пользователь. Откройте Applications > Utilities > Terminal и выполните следующие действия: 
    
    
     []$ git clone -b main https://gitlab.com/freepascal.org/fpc/source.git fpc_main
    

Это создаст каталог с именем `fpc_main` и загрузит в него основной исходный код FPC. 

Чтобы впоследствии обновить локальный репозиторий исходного кода последними изменениями исходного кода, вы можете просто сделать: 
    
    
     []$ cd
     []$ cd fpc_main
     []$ git clean -f -d
     []$ git pull
    

Чтобы собрать и установить FPC (выделенный текст должен быть на одной строке): 
    
    
     []$ cd
     []$ cd fpc_main
     []$ make distclean all FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

Вам также потребуется обновить ссылки для компилятора в `/usr/local/bin`, которые будут указывать на предыдущую версию FPC. Например: 
    
    
     []$ cd /usr/local/bin
     []$ sudo rm ppc386
     []$ sudo rm ppcx64
     []$ sudo ln -s /usr/local/lib/fpc/3.3.1/ppc386
     []$ sudo ln -s /usr/local/lib/fpc/3.3.1/ppcx64
    

Обратите внимание, что вам потребуется создать новый `ppc386` компилятор, если вы хотите продолжить компилировать 32-битные приложения, заменив эти строки (это невозможно после Xcode 11.3.1 и macOS 10.14.6 Mojave из-за удаления Apple'ом 32-битных фреймворков): 
    
    
     []$ make clean all FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

с этими двумя строками: 
    
    
     []$ make clean all FPC=/usr/local/lib/fpc/3.2.2/ppc386 OS_TARGET=darwin CPU_TARGET=i386 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppc386 OS_TARGET=darwin CPU_TARGET=i386
    

#### FPC Fixes 3.2

Создайте каталог, в который вы хотите поместить исходный код (например, `fpc_fixes32` в вашем домашнем каталоге). Вам не нужно быть root, чтобы сделать это. Это может сделать любой нормальный пользователь. Откройте Applications > Utilities > Terminal и выполните следующие действия: 
    
    
     []$ git clone -b fixes_3_2 https://gitlab.com/freepascal.org/fpc/source.git fpc_fixes32
    

Это создаст каталог с именем `fpc_fixes32` и загрузит в него исходный код FPC. 

Чтобы впоследствии обновить локальную копию исходного кода репозитория последними изменениями исходного кода, вы можете просто сделать: 
    
    
     []$ cd fpc_fixes32
     []$ git clean -f -d
     []$ git pull
    

Чтобы собрать и установить FPC (выделенный текст должен быть на одной строке): 
    
    
     []$ cd
     []$ cd fpc_fixes32
     []$ make distclean all FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

Вам также потребуется обновить ссылки для компилятора в `/usr/local/bin`, которые будут указывать на предыдущую версию FPC. Например: 
    
    
     []$ cd /usr/local/bin
     []$ sudo rm ppc386
     []$ sudo rm ppcx64
     []$ sudo ln -s /usr/local/lib/fpc/fixes-3.2/ppc386
     []$ sudo ln -s /usr/local/lib/fpc/fixes-3.2/ppcx64
    

Обратите внимание, что вам потребуется создать новый `ppc386` компилятор, если вы хотите продолжить компилировать 32-битные приложения, заменив эти строки (это невозможно после Xcode 11.3.1 и macOS 10.14.6 Mojave из-за удаления Apple'ом 32-битных фреймворков): 
    
    
     []$ make clean all FPC=/usr/local/lib/fpc/3.2.2/ppc386 OS_TARGET=darwin CPU_TARGET=i386 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppc386 OS_TARGET=darwin CPU_TARGET=i386
    

## Известные проблемы и решения

### Lazarus IDE v2.2.0 - проблема пересборки

Любая попытка пересобрать IDE Lazarus из самой себя завершается ошибкой с правами доступа, поскольку файлы и каталоги принадлежат пользователю с идентификатором 503, в то время как большинство пользователей обнаружат, что их идентификатор равен 501. Решение состоит в том, чтобы открыть Applications > Utilities> Terminal и: 
    
    
    cd /Applications
    sudo chown -R your_username Lazarus
    

#### Следующие ошибки пересборки не возникают на компьютерах Intel

Следующая ошибка заключается в том, что компилятор ресурсов «fpcres» может быть не найден. Решение состоит в том, чтобы создать файл `.fpc.cfg` (обратите внимание на начальную точку) в вашем домашнем каталоге и добавить в него следующие строки: 
    
    
    #include /etc/fpc.cfg
    -FD/usr/local/bin
    

чтобы "fpcres" можно было найти там, где он был установлен. 

Следующая ошибка указывает на неподдерживаемую целевую архитектуру `-Paarch64` с советом вместо этого вызвать драйвер компилятора "fpc", не взирая на то, что мы уже делаем. Решение состоит в том, чтобы перейти в Lazarus > Preferences > Compiler к исполняемому файлу компилятора, для которого установлено значение `/usr/local/bin/fpc`, и изменить его на `/usr/local/lib/fpc/3.2.2/ppca64`. 

Теперь среда IDE будет успешно пересобрана, но не перезапустится автоматически. Попытка затем запустить Lazarus с помощью значка`startlazarus.app` или значка `lazarus.app` в `/Applications/Lazarus` по-прежнему будет приводить к запуску исходного двоичного файла Intel lazarus. Правильный бинарный файл aarch64 можно найти в `~/.lazarus/bin/aarch64-darwin/lazarus`. Таким образом, окончательное решение — открыть Application > Utilities > Terminal и: 
    
    
    mv /Applications/Lazarus/lazarus /Applications/Lazarus/lazarus.old
    mv  ~/.lazarus/bin/aarch64-darwin/lazarus /Applications/Lazarus/
    

Затем вы можете запустить IDE с помощью значков `startLazarus.app` или `lazarus.app`. Когда вы перезапустите lazarus, стоит вернуться в Lazarus > Preferences > Compiler к исполняемому файлу компилятора, для которого установлено значение `/usr/local/lib/fpc/3.2.2/ppca64`, и изменить его обратно на `/usr/local/bin/fpc`, чтобы вы могли скомпилировать двоичные файлы как для aarch64, так и для Intel, чтобы, например, создать [Universal Binary](<../en/macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers"), который будет работать как на машинах Intel, так и на машинах aarch64. 

### Lazarus IDE - Unable to "run without debugging"

Если вы используете Lazarus IDE 2.0.10 и пункт меню «Выполнить > Запустить без отладки» не работает с диалоговым окном, подобным: 

[![run dialog fail.png](https://wiki.freepascal.org/images/1/19/run_dialog_fail.png)](</File:run_dialog_fail.png>)

затем вам необходимо исправить исходный код Lazarus 2.0.10 ([Issue #37324](<https://bugs.freepascal.org/view.php?id=37324>) и [Issue #36780](<https://bugs.freepascal.org/view.php?id=36780>)). В частности, исправьте `../ide/main.pp`, как показано ниже (неисправленные строки показаны первыми, исправленные строки показаны вторыми): 
    
    
     7243,7245c7243,7245
     <     if RunAppBundle
     <         and FileExistsUTF8(Process.Executable)
     <     and FileExistsUTF8('/usr/bin/open') then
     ---
     >     if RunAppBundle then
     >     //    and FileExistsUTF8(Process.Executable)
     >     //and FileExistsUTF8('/usr/bin/open') then
    

и перекомпилируйте Lazarus IDE. 

Кроме того, вы можете не исправлять исходный код и просто перекомпилировать Lazarus 2.0.10 с FPC 3.0.4. 

Аналогичные действия по исправлению и перекомпиляции или просто перекомпиляции с FPC 3.0.4 необходимо выполнить для Lazarus 2.0.8, если он был скомпилирован с FPC 3.2.0. 

### Обновление с Mojave (10.14) до Catalina (10.15)

  * Выполните `sudo xcode-select --install`



  * Чтобы Lazarus мог найти файл `crt1.10.5.o`, измените в `/etc/fpc.cfg` -Fl после '#ifdef cpux86_64' с


    
    
    -Fl/Applications/Xcode.app/Contents/Developer/Toolchain...
    

на 
    
    
    -Fl/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/lib/
    

### Сборка компилятора FPC, начиная с Mojave (10.14) и далее

  * Чтобы найти файл crt1.10.5.o при сборке более поздней версии Free Pascal Compiler из исходного кода, необходимо указать:


    
    
     OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
    

в командной строке make, потому что FPC игнорирует файл конфигурации `/etc/fpc.cfg` во время сборки. 

### ЧаВо по установке на Mac

  * См. [Mac Installation FAQ](<../en/Mac_Installation_FAQ.md> "Mac Installation FAQ"), чтобы узнать о решениях других распространенных проблем, которые могут возникнуть во время (и после) установки Lazarus и Free Pascal на macOS.



## Деинсталляция Lazarus и Free Pascal

Обратитесь к [Удаление Lazarus в macOS](<../en/Uninstalling_Lazarus_on_macOS.md> "Uninstalling Lazarus on macOS") для получения информации о вариантах удаления. 

## Устаревшая информация

See [Legacy Information](<../en/Legacy_Information__Installing_Lazarus_on_Mac.md> "Legacy Information: Installing Lazarus on Mac") for details of: 

  * Installing Lazarus on Mac OS X 10.4 (Tiger), Mac OS X 10.5 (Leopard), OS X 10.8 (Mountain Lion)
  * Installing Lazarus 2.0.8, 2.0.10 with FPC 3.2.0 for macOS 10.10 and earlier
  * Installing Lazarus 2.0.8 with FPC 3.2.0 for macOS 10.11+
  * Installing Lazarus on PowerPC-based Macs
  * Old Xcode versions
  * Installing the gdb debugger
  * Compatibility matrix for Lazarus 1.0.0 through 1.6.4 and FPC 2.6.0 through 3.0.2.



# См. также

  * [Xcode](<../en/Xcode.md> "Xcode").
  * [Other macOS installation options](<../en/Other_macOS_installation_options.md> "Other macOS installation options").
  * [Installing multiple Lazarus versions](<../en/Multiple_Lazarus.md> "Multiple Lazarus").
  * [Mac Portal](<../en/Portal_Mac.md> "Portal:Mac") for an overview of development for macOS with Lazarus and Free Pascal.
  * [Mac Installation FAQ](<../en/Mac_Installation_FAQ.md> "Mac Installation FAQ") for solutions to the most frequent problems that may arise during (and after) installation of Lazarus and Free Pascal on macOS.



# Внешние ссылки

  * [Lazarus daily development version download](<https://sourceforge.net/projects/macos-lazarus-snapshots/files/intel/>) (Intel x86_64)
  * [Lazarus + FPC daily development version downloads](<https://sourceforge.net/projects/macos-lazarus-snapshots/files/arm64/>) (M1, ARM64, aarch64)
  * [GIT Succinctly](<https://www.syncfusion.com/succinctly-free-ebooks/git>) \- Free Ebook (PDF, MOBI, EPUB) from SyncFusion.
  * [Uninstall Xcode Command Line Tools](<https://mac.install.guide/commandlinetools/6.html>)
  * [Reinstall Xcode Command Line Tools](<https://mac.install.guide/commandlinetools/7.html>)

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus_on_macOS/ru](https://web.archive.org/web/20241209233316/https://wiki.freepascal.org/Installing_Lazarus_on_macOS/ru)_
