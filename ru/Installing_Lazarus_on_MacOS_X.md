# Installing Lazarus on MacOS X

[![Logo OSX.png](https://wiki.freepascal.org/images/7/7a/Logo_OSX.png)](</File:Logo_OSX.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ [**English (en)**](<../en/Installing_Lazarus_on_MacOS_X.md> "Installing Lazarus on MacOS X") │  [**한국어 (ko)**](</Installing_Lazarus_on_MacOS_X/ko> "Installing Lazarus on MacOS X/ko") │  **русский (ru)** │    
****

Установка Lazarus на Mac не особенно сложна, но очень важно выполнить установку в правильном порядке. Пропуск шагов почти наверняка приведет к плачу Ярославны. Вкратце, вот что вы должны сделать - 

  1. Загрузить и установить Xcode.
  2. Установить глобальные инструменты командной строки для Xcode.
  3. Установить Free Pascal Compiler и исходники FPC
  4. Установить Lazarus
  5. Настроить LLDB - поставляемый (и подписанный) Apple отладчик от Lazarus.



[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** если вы устанавливаете версии Lazarus до 2.0.0, вам почти наверняка понадобится gdb, см. раздел Legacy.

  


## Contents

  * 1 Установка
    * 1.1 Шаг 1. Загрузка Xcode.
    * 1.2 Шаг 2. Инструменты командной строки Xcode
    * 1.3 Шаг 3 FPC, исходники FPC
      * 1.3.1 Source Forge
      * 1.3.2 Другие параметры FPC
    * 1.4 Установка Lazarus
    * 1.5 Настройка отладчика
      * 1.5.1 Дополнительная информация по использованию LLDB
      * 1.5.2 Установка LazDebuggerFpLLdb
  * 2 Совместимость
  * 3 Carbon и Cocoa
  * 4 Загрузка исходников через svn
  * 5 Загрузка стабильных исходников
  * 6 Информация, утратившая актуальность
    * 6.1 Установка gdb
      * 6.1.1 Часть I установки GDB: собственно, установка
      * 6.1.2 Часть II установки GDB: расскажите Lazarus о запуске GDB
    * 6.2 Совместимость с XCode
      * 6.2.1 Совместимость с Xcode 5.0 и более новыми версиями (Mac OS X 10.8, 10.9, 10.10 и 10.11)
      * 6.2.2 Совместимость с Xcode 4.3+ (Mac OS X 10.7/10.8)
      * 6.2.3 Совместимость с Xcode 3.2.x (Mac OS X 10.6)
      * 6.2.4 Совместимость с Xcode 3.1.x (Mac OS X 10.5)
      * 6.2.5 Совместимость с Xcode 2.x (Mac OS X 10.4)
    * 6.3 Установка пакетов / релиз-версий
      * 6.3.1 Установка из образов дисков
    * 6.4 Установка из исходников / Девелоперские версии
      * 6.4.1 Загрузка и установка компилятора
  * 7 Деинсталляция Lazarus и Free Pascal
    * 7.1 Удаление с использованием fink
    * 7.2 Удаление из образов дисков или исходников
      * 7.2.1 Папка настроек Lazarus .lazarus в домашнем каталоге
  * 8 Известные проблемы
  * 9 См. также
    * 9.1 Другие интерфейсы
      * 9.1.1 Платформозависимые советы
      * 9.1.2 Mac How в статьях
      * 9.1.3 Статьи по разработке интерфейсов



## Установка

В подробных инструкциях предполагается, что на вашем Mac установлена последняя версия macOS, последняя версия Xcode от Apple и последняя версия Lazarus. Внизу страницы, под устаревшей документацией, вы увидите более старую информацию, которая может иметь значение, если вы используете старые компоненты. Вы можете помочь, заменив устаревшую информацию, либо удалив ее, либо, если это может помочь кому-то, работающему с унаследованным проектом, переместить ее в конец страницы. 

В общем, речь идет об использовании набора виджетов **Carbon** и **Cocoa**. Хотя Carbon все еще можно считать немного более стабильным, на момент выпуска 2.0.0 64-битное Cocoa очень близко [к стабильному] и, безусловно, его следует учитывать. Carbon намеренно (Apple) ограничен 32 битами, и мы знаем, что следующая версия MacOSX, вероятно, не будет его поддерживать. 

### Шаг 1. Загрузка Xcode.

Вам нужны инструменты Apple Developer, которые являются частью среды разработки Xcode. Они могут быть установлены с оригинальных установочных дисков MacOS или более новой копии, загруженной с Apple Developer Connection (ADC), для которой требуется бесплатная регистрация: [ссылка](<http://developer.apple.com/>). Загрузите файл Xcode, он окажется в вашем каталоге Downloads в виде zip-файла. На сентябрь 2017 года это Xcode_9_beta_6.xip, около 5Гб. Кликните по нему. Он разархивируется в ваш каталог Downloads. Возможно, вы будете довольны этим, а может и нет. Другие пользователи увидят путь к нему, но не смогут его использовать. И это непорядок. Поэтому я переместил мой [экземпляр] и затем сказал xcode - выбери, куда он был перемещен (в терминале) - 
    
    
    mv Downloads/Xcode-beta.app /Developer/.
    sudo xcode-select -s /Developer/Xcode-beta.app/Contents/Developer
    

### Шаг 2. Инструменты командной строки Xcode

Это показано здесь в качестве отдельного шага, потому что на самом деле это отдельный шаг в дополнение к Шагу 1. Не путайте это с внутренними инструментами командной строки Xcode, которые, по словам графического интерфейса Xcode, уже установлены. Lazarus не может использовать эти инструменты внутренней командной строки Xcode, поэтому сделайте следующее (это быстро и легко) - 
    
    
    xcode-select --install
    

Начиная с macOS Mojave, вам также необходимо установить заголовочные файлы SDK: 
    
    
    open /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg
    

### Шаг 3 FPC, исходники FPC

Получите и установите FPC и исходники FPC. Перед установкой Lazarus необходимо установить совместимый fpc (и исходный код), у вас есть несколько вариантов. 

#### Source Forge

Получить версию релиза можно по ссылке <https://sourceforge.net/projects/lazarus/files>. Эти двоичные установочные комплекты созданы разработчиками FPC/Lazarus и отслеживают формальные выпуски. Так как эти установочные комплекты не одобрены Apple, вам нужно удерживать нажатой клавишу Control, щелкнуть пакет и выбрать Open(Открыть) и подтвердить, что вы хотите установить его от неизвестного разработчика (то есть того, кто не платит Apple откат). 

Для получения официальных/более подробных инструкций для FPC перейдите по адресу [[1]](<http://freepascal.stack.nl/down/i386/macosx-netherlands.html>) ([прим.перев.](</index.php?title=Zoltanleo&action=edit&redlink=1> "Zoltanleo \(page does not exist\)"): в оригинале битая ссылка) 

Вы можете применить простой и быстрый тест fpc на этом этапе: [Testing_FPC_installation](<../en/Installing_Lazarus_on_Linux.md>). 

#### Другие параметры FPC

Вы также можете рассмотреть возможность использования fpcupdeluxe для установки fpc и, возможно, Lazarus, [fpcupdeluxe](<../en/fpcupdeluxe.md> "fpcupdeluxe")

Кроме того, вы можете использовать [fink](<http://finkproject.org/>), менеджер пакетов для macOS для установки fpc. Обратите внимание, что на момент написания (в начале 2019 года) fink предлагает Lazarus 1.8.4, и в целом он согласен с тем, что требуются версии более поздние, чем 2.0.0 (т.е. не выпускаемые), поэтому, вероятно, они не подходят для разработки Cocoa. Дополнительным бонусом fink является простота установки, а также чистое удаление огромного количества других пакетов программного обеспечения с открытым исходным кодом, включая [FreePascal crosscompilers](<../en/Cross_compiling.md> "Cross compiling") для многих процессоров и систем. Выбор для lazarus - Carbon'ный вид lazarus (предпочитаемый большинством), gtk2, Qt4 и Qt5: 
    
    
    $ fink install lazarus-aqua
    // или
    $ fink install lazarus-gtk2
    // или
    $ fink install lazarus-qt4
    // или
    $ fink install lazarus-qt5
    

Вам будет задан вопрос, устанавливать ли ряд зависимостей, включая компилятор FreePascal, исходные коды lazarus. Просто нажмите Enter и сразу же идите пить кофе. Сборка всех пакетов может занять некоторое время, особенно на старых Mac. 

После установки Lazarus можно запустить из папки /Applications/Fink/. Фактические детали fpc и lazarus находятся в подкаталогах /sw 

Для любого пакета lazarus от fink эти наборы виджетов поддерживаются для вашей программы в macOS: carbon (Aqua), gtk2, qt4 (Aqua), qt5 (Aqua), nogui, win32, win64 и wince. 

Если вы установите fpc из fink, у вас все получится, скачав исходный код Lazarus и скомпилировав его, как описано ниже. Однако, возможно, не стоит смешивать fink fpc с установкой бинарников Lazarus от sourceforge. 

### Установка Lazarus

На момент написания статьи (начало 2019 года) существует ряд причин, по которым вам может быть лучше использовать еще не выпущенную версию Lazarus, а именно fixes_2_0. Особенности - 

  * Вам почти наверняка понадобится Cocoa, скорее всего, следующая версия MacOSX не будет поддерживать 32-битный Carbon.
  * Вы почти наверняка захотите использовать lldb вместо gdb - гораздо проще установить.
  * Cocoa постоянно улучшается, и интерфейс lldb быстро улучшается, начиная с 2.0.0.
  * Fixes_2_0 безопаснее и стабильнее, чем trunk, но получает новые вещи намного быстрее, чем release версия.



Предварительно скомпилированные двоичные файлы, основанные на fixes_2_0, недоступны в SourceForge, но их можно получить из [fpcupdeluxe](<../en/fpcupdeluxe.md> "fpcupdeluxe"). Но здесь мы обсудим загрузку исходного кода и сборку. Немного медленный изначально, но очень надежный и отличный тест установки вашего компилятора! Вам понадобится svn (он не был предварительно установлен на ранних версиях MacOS, используйте brew или fink для установки). 
    
    
    cd ~
    mkdir -p bin/lazarus
    cd bin/lazarus
    svn checkout https://svn.freepascal.org/svn/lazarus/branches/fixes_2_0 laz_fixes
    // ожидаем какое-то время ....
    cd laz_fixes
    make LCL_PLATFORM=cocoa CPU_TARGET=x86_64 bigide
    // ожидаем какое-то время .....
    open startlazarus.app --args "--pcp=~/.laz_fixes"
    

  * Примечание. Я передаю параметр для использования каталога конфигурации, основанного на имени фактического каталога установки. Это делает некоторые сценарии легкими.


  * В настоящее время также вполне возможно (и, возможно, безопаснее на ощупь) создать 32-битную Carbon Lazarus IDE и использовать ее для создания 64-битных приложений Cocoa. Замените упомянутую выше строку на "_make LCL_PLATFORM=carbon CPU_TARGET=i386 bigide_ " и настройте свой проект, как указано в разделе [Carbon и Cocoa](<Installing_Lazarus_on_MacOS_X.md> "Installing Lazarus on MacOS X/ru") ниже.



Возможно, вы захотите поместить небольшой скрипт в каталог $HOME/bin и даже указать путь к нему (очень unix!) 
    
    
    #!/bin/bash
    LAZDIR="laz_fixes"
    cd ~/bin/lazarus/"$LAZDIR"
    open ~/bin/lazarus/"$LAZDIR"/lazarus.app --args "--pcp=~/.$LAZDIR"
    

### Настройка отладчика

В версиях Lazarus 1.8.4 и более ранних версиях вам нужно было использовать gdb в качестве отладчика, медленно устанавливать и тяжело подписывать. Начиная с Lazarus 2.0.0 вы можете (и должны) использовать LLDB, отладчик, предоставляемый Apple, подпись [для которого] не требуется. 

Предполагая, что вы установили все необходимое и запустили Lazarus, остается только настроить отладчик. Если вы не сделаете этого сейчас, Lazarus попытается использовать GDB и потерпит неудачу. 

Сначала нажмите Tools(Инструменты)->Options(Параметры)->Debugger(Отладчик). В правом верхнем углу открытого окна есть надпись "Debugger type and path"(Тип и путь отладчика), вы должны задать оба [значения]. Выберите "LLDB debugger (with fpdebug) (Beta)"(Отладчик LLDB (с помощью fpdebug) (Beta)). 

[![Set Debugger.png](https://wiki.freepascal.org/images/8/8e/Set_Debugger.png)](</File:Set_Debugger.png>)

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

## Совместимость

Не всякая комбинация [версий] Lazarus и Free Pascal совместима с любой установкой macOS. Пожалуйста, обратитесь к следующей таблице, чтобы найти правильную версию для вашей среды разработки: 

Lazarus Compatibility Matrix  | Lazarus 1.0.0  | Lazarus 1.0.2  | Lazarus 1.0.4  | Lazarus 1.0.6  | Lazarus 1.0.8  | Lazarus 1.0.10  | Lazarus 1.0.12  | Lazarus 1.0.14  | Lazarus 1.2.0   
---|---|---|---|---|---|---|---|---|---  
| Free Pascal (FPC) 2.6.0  |  FPC 2.6.2   
PPC processors   
Mac OS X 10.4 (Tiger)  | Incompatible | Incompatible | N/A | Compatible | Incompatible | Incompatible | Compatible* | Compatible* | Not tested   
Mac OS X 10.5 (Leopard)  | Compatible | Compatible | N/A | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible**   
Intel processors   
Mac OS X 10.4 (Tiger)  | Incompatible | Incompatible | Compatible | Compatible | Incompatible | Incompatible | With hack only | Not tested | Not tested   
Mac OS X 10.5 (Leopard)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Not tested | Not tested   
Mac OS X 10.6 (Snow Leopard)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible   
Mac OS X 10.7 (Lion)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible   
Mac OS X 10.8 (Mountain Lion)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible   
Mac OS X 10.9 (Mavericks)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible* | Compatible* | Compatible*   
Mac OS X 10.10 (Yosemite)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
Mac OS X 10.11 (El Capitan)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS X 10.12 (Sierra)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS X 10.13 (High Sierra)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS X 10.14 (Mojave)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
| Lazarus 1.2.2  | Lazarus 1.2.4  | Lazarus 1.2.6  | Lazarus 1.4.0  | Lazarus 1.4.2  | Lazarus 1.4.4  | Lazarus 1.6.0  | Lazarus 1.6.2  | Lazarus 1.6.4  | Lazarus 1.8.x  | Lazarus 2.0.y   
---|---|---|---|---|---|---|---|---|---|---|---  
|  FPC 2.6.4  |  FPC 3.0.0  | FPC 3.0.2  | FPC 3.0.4  | FPC 3.0.4   
PPC processors   
Mac OS X 10.4 (Tiger)  | Compatible | N/A | Compatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Mac OS X 10.5 (Leopard)  | Compatible | Compatible** | Compatible | Compatible** | Compatible** | Compatible** | Not tested | Not tested | Not tested | Not tested | Incompatible   
Intel processors   
Mac OS X 10.4 (Tiger)  | Not tested | Not tested | Not tested | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Mac OS X 10.5 (Leopard)  | Compatible | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
Mac OS X 10.6 (Snow Leopard)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible* | Not tested   
Mac OS X 10.7 (Lion)  | Compatible | Compatible | Compatible | Not tested | Not tested | Not tested | Compatible | Compatible | Compatible | Not tested | Not tested   
Mac OS X 10.8 (Mountain Lion)  | Compatible | Compatible | Compatible | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Not tested | Not tested   
Mac OS X 10.9 (Mavericks)  | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Not tested | Not tested   
Mac OS X 10.10 (Yosemite)  | Not tested | Not tested | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Not tested | Not tested   
Mac OS X 10.11 (El Capitan)  | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible* | Compatible* | Compatible* | Compatible* | Not tested | Not tested   
macOS X 10.12 (Sierra)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible* | Compatible* | Compatible* | Compatible* | Compatible***   
macOS X 10.13 (High Sierra)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS X 10.14 (Mojave)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible***   
  
*[Restrictions](<../en/GDB_on_OS_X_Mavericks_and_Xcode_5.md> "GDB on OS X Mavericks and Xcode 5") apply to debugging with [GDB](<../en/GDB.md> "GDB"). **Not available as pre-built installer. [Compiling from source](<../en/Installing_Lazarus_on_MacOS_X.md> "Installing Lazarus on MacOS X") required. ***[LLDB](<../en/lldb.md> "lldb") debugger required  
x = 0, 2 or 4; y = 0 or 2 

## Carbon и Cocoa

В настоящее время (начало 2019 года) Lazarus, использующий Cocoa, удовлетворит потребности многих пользователей. Carbon работает почти так же, как и ожидалось, но вам рекомендуется сначала попробовать Cocoa, особенно если учесть, что Apple указала релизы после того, как Mojave не будет поддерживать 32bit Carbon. Альтернативы включают QT и GTK2 +, которые требуют дополнительных библиотек, и GTK2 + на Mac, кажется, привлекают мало внимания в эти дни. У QT на Mac есть несколько сильных сторонников на форуме. 

Однако, мы должны предполагать, что **Cocoa** \- это будущее Mac. Возможно собрать версию Lazarus Cocoa на релизе 2.0.0 и выше. Можно также сделать сборку Lazarus для Carbon и использовать ее для создания 64-битных двоичных файлов Cocoa. В Carbon IDE вам нужно установить цель для 64-битного процессора и указать набор виджетов Cocoa: 

  * Откройте свой проект с помощью Lazarus и нажмите Project/ProjectOptions
  * На панели "Config and Target"(Конфигурация и цель) установите для "Target CPU family"(Семейство целевых процессоров) значение "x86_64".
  * На панели "Additions and Overrides"(Дополнения и переопределения) нажмите "Set LCLWidgetType"(Задать LCLWidgetType) и установите значение "Cocoa"
  * Раньше по какой-то причине Lazarus постоянно устанавливал компилятор в "/usr/local/bin/ppc386", что приводило к [созданию] 32-битных приложений. Убедитесь, что в меню Tools(Сервис)->Options(Параметры) для "Compiler Executable"(Исполняемый файл компилятора) установлено значение "/usr/local/bin/fpc", чтобы получить 64-разрядные приложения.
  * Теперь скомпилируйте ваш проект - и, пожалуйста, сообщите о любых проблемах, с которыми вы столкнулись.



## Загрузка исходников через svn

Исходники хранятся в системе управления версиями, которая называется subversion, или коротко **svn** : 

  * [macOS] 10.5 и выше уже содержат клиентов SVN. Пользователи более ранних версий должны установить SVN для macOS. Хороший пакет предоставлен [Martin Ott](<http://www.codingmonkeys.de/mbo/>). Вы также можете использовать Fink. Клиенты SVN с графическим интерфейсом пользователя (GUI) доступны на [Versiontracker](<http://www.versiontracker.com>). Довольно удобным клиентом, который интегрируется в Finder, является [SCPlugin](<http://scplugin.tigris.org>).



Создайте каталог, куда вы хотели бы поместить исходники. Для этого вам не нужно быть пользователем root. Это может сделать любой обычный пользователь. Сначала создайте каталог для fpc (например, /Users/username/freepascal), затем откройте терминал и выполните следующие действия: 
    
    
     []$ svn co http://svn.freepascal.org/svn/fpc/trunk fpc
    

Это создаст каталог с именем 'fpc', который впоследствии можно будет использовать в IDE. Подсказка: чтобы загрузить/обновить последние изменения, вы можете просто сделать 
    
    
     []$ cd /Users/username/freepascal/fpc
     []$ svn up
    

Соберите fpc 
    
    
     []$ make clean all
     []$ sudo make install
    

Затем загрузите lazarus 
    
    
     []$ svn co http://svn.freepascal.org/svn/lazarus/trunk lazarus
    

Это создаст каталог с именем 'lazarus'. Чтобы обновить последние изменения: 
    
    
     []$ cd /Users/username/freepascal/lazarus
     []$ svn up
    

Соберите lazarus 
    
    
     []$ make clean all
    

Затем запустите Lazarus либо из командной строки: 
    
    
     open lazarus.app
    

, либо дважды щелкните в окне Finder. 

## Загрузка стабильных исходников

Исходный код стабильных версий доступен в архивах [Zip - GZip](<http://sourceforge.net/projects/lazarus/files/Lazarus%20Zip%20_%20GZip/Lazarus>). Это, например, возможность для пользователей, которые хотят запускать последние официальные версии Lazarus на «неподдерживаемых» компьютерах, например, Маки на основе PPC. При условии, что вы установили последнюю стабильную версию FPC вместе с исходными текстами, достаточно скачать и распаковать zip-архив. Переместите его в нужное место (например, в вашу папку /Developer), откройте окно терминала и измените путь оболочки к этой папке (самый простой способ сделать это - набрать "cd ", а затем перетащить значок только что распакованного Lazarus папку до позиции после этой команды в окне терминала). При необходимости нажмите <Enter>, затем 
    
    
    []$ make clean bigide
    

и снова <Enter>. Впоследствии FreePascal будет использовать make-файл в этой папке для сборки среды разработки Lazarus. Это может занять несколько минут, но процесс полностью автоматизирован. Поэтому, возможно, вы захотите поискать чашку кофе, пока FPC работает. Если компилятор готов, он выводит количество обработанных строк и требуемое время на вывод оболочки. 

Последний шаг - щелкните правой кнопкой мыши значок пакета Lazarus и выберите "Show Package Contents"(Показать содержимое пакета) в контекстном меню. Затем перетащите исполняемый файл "lazarus" в папку "Contents/MacOS" комплекта. Вот и все. 

# Информация, утратившая актуальность

### Установка gdb

Начиная с версии Lazarus 2.0.0, это совершенно необязательно. IDE теперь поддерживает LLDB напрямую. См. [здесь](<http://forum.lazarus-ide.org/index.php/topic,42869.0.html>) и [здесь](<http://wiki.lazarus.freepascal.org/Lazarus_2.0.0_release_notes#LLDB_based_Debugger_.28New.29>). 

#### Часть I установки GDB: собственно, установка

Запуск gdb может быть немного утомительным, но в конце концов стоит того, если используется Lazarus моложе v2.0, или имеются какие-то особые требования для gdb. У вас, возможно, нет! 

Этот шаг следует выполнить после установки XCode и перед установкой FPC/Lazarus. 

  1. Установите fink или brew и используйте их для [установки gdb](<../en/GDB_on_OS_X_Mavericks_and_Xcode_5.md>)
  2. Самостоятельно [подпишите](<https://sourceware.org/gdb/wiki/BuildingOnDarwin>) двоичный файл gdb, используя инструменты, уже установленные на вашем Mac'е. Это утомительная часть. Инструкции понятны, за исключением того, что, возможно, они могли бы упомянуть [следующее]: после того, как вы установили связку ключей ’System’, вам нужно нажать ‘Create’(Создать) и ‘Done’(Готово) на следующем экране. Затем перейдите и найдите сертификат в вашем общем списке сертификатов, чтобы выполнить следующую инструкцию “using the contextual menu ”(с помощью контекстного меню). Не беспокойтесь о шаге “2.1.4 Disable starting the debuggee…”(Отключить запуск отладчика), мы обратимся к этому после установки и работы самого Lazarus.
  3. Как только Lazarus будет установлен и запущен, укажите ему, как он должен запускать GDB, это шаг 5.



[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** : по состоянию на апрель 2018 года могут [возникнуть проблемы](<https://forum.lazarus.freepascal.org/index.php/topic,40874.0.html>) с новейшими GDB и High Sierra. Согласно [этому сообщению](<https://stackoverflow.com/questions/49001329/gdb-doesnt-work-on-macos-high-sierra-10-13-3>), на High Sierra GDB 8.1 не работает, но работает GDB 8.0.1

#### Часть II установки GDB: расскажите Lazarus о запуске GDB

Опять же, этот шаг не обязателен для Lazarus v2.0 и новее. См. подробности [здесь](<http://wiki.lazarus.freepascal.org/Lazarus_2.0.0_release_notes#LLDB_based_Debugger_.28New.29>). Если вы устанавливаете (и используете) gdb, вы должны сообщить Lazarus о том, как вызывать gdb. Очевидно, что этот шаг происходит после установки Lazarus. 

Хорошо, теперь у вас должна быть возможность запустить Lazarus, он должен отображать различные части и пути, которые ему нужны (включая gdb) на маленьком начальном экране. Там вы получите предупреждение, если решите не использовать отладчик. Перейдите к IDE, и теперь мы должны сказать Lazarus, чтобы gdb запретил открывать оболочку, [как отмечают](<http://forum.lazarus.freepascal.org/index.php/topic,37310.0.html>) некоторые участники форума. В Lazarus выберите Tools(Сервис)->Options(Параметры)->Debugger(Отладчик)->General(Общее). В “Debugger specific Options”(Опции, специфичные для отладчика) есть поле, помеченное как Debugger_Startup_Options(Параметры запуска отладчика). Введите "--eval-command=set startup-with-shell off" (включая кавычки) и нажмите клавишу <Enter>, нажмите «ОК». Обратите внимание, что вы не можете скопировать и вставить в это поле (##$!). Смотрите скриншот. 

[![Debug Startup Options 01.png](https://wiki.freepascal.org/images/6/6e/Debug_Startup_Options_01.png)](</File:Debug_Startup_Options_01.png>)

### Совместимость с XCode

Информация о старых версиях FPC, Lazarus и Apple Developer Tools. 

#### Совместимость с Xcode 5.0 и более новыми версиями (Mac OS X 10.8, 10.9, 10.10 и 10.11)

Как и Xcode 4.3+, Xcode 5 и последующие версии больше не устанавливают инструменты командной строки по умолчанию. Однако они требуются FPC/Lazarus. В отличие от Xcode 4, опция установки больше не доступна в Xcode 5 через вкладку Downloads. Кроме того, вкладка "Locations"(Местоположения) диалогового окна настроек может сбивать с толку, поскольку в ней упоминается, что установлены инструменты командной строки: 

[![Default location of command line tools in Xcode 5.](https://wiki.freepascal.org/images/c/cf/xcode5_misleading_screenshot.png)](</File:xcode5_misleading_screenshot.png> "Default location of command line tools in Xcode 5.") [![Installation of command line tools with the xcode-select command part 1...](https://wiki.freepascal.org/images/7/75/xcode_select.png)](</File:xcode_select.png> "Installation of command line tools with the xcode-select command part 1...") [![...and part 2.](https://wiki.freepascal.org/images/1/1e/command_line_tools_installation_on_mavericks.png)](</File:command_line_tools_installation_on_mavericks.png> "...and part 2.")

Первое диалоговое окно показывает, что по умолчанию инструменты командной строки устанавливаются только внутри самого пакета Xcode.app. Чтобы установить их глобально, как того требует FPC/Lazarus, выполните в терминале следующую команду: 
    
    
    xcode-select --install
    

В зависимости от вашей версии XCode альтернативные шаги для установки инструментов разработчика командной строки могут заключаться в том, чтобы загрузить их непосредственно в окне настроек XCode (см. Скриншоты выше) или загрузить их с [developer.apple.com с сайта Apple](<http://developer.apple.com>)(пожалуйста, позаботьтесь об установке инструментов командной строки, которые точно соответствуют вашей версии XCode). 

  * Вы можете скомпилировать и установить обычный GDB. См. [GDB on OS X Mavericks and Xcode 5](<../en/GDB_on_OS_X_Mavericks_and_Xcode_5.md> "GDB on OS X Mavericks and Xcode 5").



#### Совместимость с Xcode 4.3+ (Mac OS X 10.7/10.8)

Xcode 4.3 и более поздние версии больше не устанавливают инструменты командной строки по умолчанию. А это требуется FPC/Lazarus. 

Чтобы установить их вручную, откройте Xcode, перейдите в Preferences(Настройки), выберите "Downloads"(Загрузки) и установите "Command Line Tools"(Инструменты командной строки). После этого FPC установится и будет работать правильно. 

[![install command line tools 1.png](https://wiki.freepascal.org/images/a/a5/install_command_line_tools_1.png)](</File:install_command_line_tools_1.png>) [![install command line tools 2.png](https://wiki.freepascal.org/images/d/dd/install_command_line_tools_2.png)](</File:install_command_line_tools_2.png>) [![install command line tools 3.png](https://wiki.freepascal.org/images/d/d0/install_command_line_tools_3.png)](</File:install_command_line_tools_3.png>)

Необходимо будет переустанавливать инструменты командной строки после каждого обновления XCode (особенно, когда это - новая основная версия, установщик может удалить старые копии инструментов командной строки), а также после переключения на новое программное обеспечение системы (то же самое). Вы также можете загрузить инструменты командной строки с [Apple Developer](<https://developer.apple.com/download/more/?=command%20line%20tools>). 

#### Совместимость с Xcode 3.2.x (Mac OS X 10.6)

FPC 2.6.0: существует проблема при компиляции динамических библиотек с FPC под Mac OS X 10.6 из-за ошибки в компоновщике Xcode 3.2.x. Эта ошибка была исправлена в Xcode 4. Вы можете обойти эту ошибку в Xcode 3.2.x, используя параметр командной строки -k-no_order_inits при компиляции динамической библиотеки. 

#### Совместимость с Xcode 3.1.x (Mac OS X 10.5)

Большинство версий Lazarus и FPC совместимы с Xcode 3.1 (см. таблицу [ Совместимость](<Installing_Lazarus_on_MacOS_X.md> "Installing Lazarus on MacOS X/ru") для более подробной информации). Интеграционный комплект XCode предоставляется в качестве дополнительной установки, позволяющей разрабатывать с помощью Free Pascal непосредственно из XCode. Однако этого не требуется для разработки на Lazarus. 

#### Совместимость с Xcode 2.x (Mac OS X 10.4)

Большинство версий Lazarus и FPC совместимы с Xcode 2.3 или новее (см. [ совместимость](<Installing_Lazarus_on_MacOS_X.md> "Installing Lazarus on MacOS X/ru") для более подробной информации). Интеграционный комплект XCode предоставляется в качестве дополнительной установки, позволяющей разрабатывать с помощью Free Pascal непосредственно из XCode. Однако он не требуется для разработки на Lazarus. Xcode 2.0 был совместим с Lazarus 0.9.x, но больше не поддерживается новыми версиями Lazarus IDE. 

## Установка пакетов / релиз-версий

### Установка из образов дисков

Загрузите три образа диска (файлы .dmg) для fpc, fpcsrc и lazarus по одной из следующих ссылок: 

  * [Intel-based Macs](<http://sourceforge.net/projects/lazarus/files/Lazarus%20Mac%20OS%20X%20i386/>)
  * [PowerPC-based Macs](<http://sourceforge.net/projects/lazarus/files/Lazarus%20Mac%20OS%20X%20powerpc/>)



Откройте каждый образ диска и установите его в следующем порядке: 

  1. fpc
  2. fpcsrc
  3. Lazarus



После установки приложение Lazarus можно найти в /Developer/lazarus/, исходные файлы FPC в /usr/local/share/fpcsrc. 

Если при попытке скомпилировать пустую форму появляется сообщение об ошибке "Can't find unit Interfaces used by Project1"(Не удается найти интерфейс устройства, используемый Project1), проверьте следующие параметры в Lazarus (должны быть установлены по умолчанию): 

Environment Options 
    
    
      Lazarus directory: /Developer/lazarus
      Compiler path: /usr/local/bin/ppc386 (PowerPC Macs: /usr/local/bin/ppcppc)
      FPC Source: /usr/local/share/fpcsrc
    

Project | Options 
    
    
      All paths blank
      LCL Widget Type: default (Carbon beta)
    

Project | Inspector 
    
    
      Required Packages
         LCL
    
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** Различные версии Lazarus зависят от конкретных версий компилятора FreePascal и не будут работать с другими версиями.

Другая распространенная проблема заключается в том, что могут отличаться версии fpc и fpcsrc. 

Это самый простой способ установить Lazarus на Mac OS X. 

## Установка из исходников / Девелоперские версии

Вам нужна версия последнего стабильного релиза FPC, чтобы скомпилировать версию для разработки. 

### Загрузка и установка компилятора

Загрузите и установите [пакеты FPC](<https://sourceforge.net/project/showfiles.php?group_id=89339>). 

Существует две версии компилятора для разработки: 2.6.x - стабильная версия без новых функций - только исправления ошибок. Нестабильная версия 2.7.x поставляется с множеством новых функций, но иногда и с ошибками. Лучше всего скачать и установить fpc 2.6.x. Некоторые ежедневные снапшоты можно найти [здесь](<http://snapshots.lazarus.shikami.org/lazarus/>). Имейте в виду, что это - снапшоты, и вам может не повезти, и вы получите версию с ошибками. Вероятность около 1:30. Поэтому, если версия содержит ошибку, попробуйте [скачать снапшот в] другой день или используйте релизные пакеты. 

# Деинсталляция Lazarus и Free Pascal

## Удаление с использованием fink

Полная деинсталляция всех пакетов fpc и lazarus выполняется с помощью: 
    
    
    fink purge --recursive fpc-config
    

Если вы хотите сохранить изменения в файле настроек /sw/etc/fpc.cfg, выполните: 
    
    
    fink remove --recursive fpc-config
    

Для удаления папки настроек Lazarus и файлов в вашем домашнем каталоге, смотрите ниже ([Папка настроек Lazarus .lazarus в домашнем каталоге](<Installing_Lazarus_on_MacOS_X.md> "Installing Lazarus on MacOS X/ru")). 

## Удаление из образов дисков или исходников

Обычно вы удаляете приложение в OS X, просто перетаскивая его из папки Applications(Программы) в корзину. Но поскольку Lazarus и Free Pascal являются инструментами разработки, они устанавливаются в несколько папок, которые вы обычно не видите в Finder. 

Вы можете скопировать и сохранить приведенные ниже команды в файл uninstallLaz.sh и запустить его, если вам нужно удалить Lazarus и Free Pascal. Обычно вы можете устанавливать более новые версии Lazarus и Free Pascal поверх более старых версий, но, как и в большинстве программ, неплохо было бы очистить все перед установкой новой версии. Этот список команд также должен дать вам представление о том, где находятся различные части Lazarus и Free Pascal. 
    
    
    bin=/usr/local/bin
    share=/usr/local/share
    lib=/usr/local/lib
    receipts=/Library/Receipts
    private=/private/etc
    
    rm -fv $bin/ppcppc
    rm -fv $bin/ppc386
    rm -fv $bin/bin2obj
    rm -fv $bin/data2inc
    rm -fv $bin/delp
    rm -fv $bin/fd2pascal
    rm -fv $bin/fpc
    rm -fv $bin/fpcmake
    rm -fv $bin/fpcmkcfg
    rm -fv $bin/fpcsubst
    rm -fv $bin/fpdoc
    rm -fv $bin/fprcp
    rm -fv $bin/h2pas
    rm -fv $bin/h2paspp
    rm -fv $bin/makeskel
    rm -fv $bin/mkxmlrpc
    rm -fv $bin/plex
    rm -fv $bin/postw32
    rm -fv $bin/ppdep
    rm -fv $bin/ppudump
    rm -fv $bin/ppufiles
    rm -fv $bin/ppumove
    rm -fv $bin/ptop
    rm -fv $bin/pyacc
    rm -fv $bin/rstconv
    rm -fv $bin/unitdiff
    rm -r $private/lazarus
    rm -r $bin/lazarus
    rm -r $share/lazarus
    rm -r $share/fpcsrc
    rm -r $share/doc/fpc-2.2.2
    rm -r $share/examples/fpc-2.2.2
    
    rm -r $lib/fpc
    
    rm -r $receipts/lazarus-*.pkg
    rm -r $receipts/fpcsrc-*.pkg
    rm -r $receipts/fpc-*.pkg
    
    rm -fv /etc/fpc.cfg
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** это предполагает, что у вас установлена версия 2.2.2 Free Pascal. Если у вас другая версия, измените две соответствующие команды, чтобы указать свою версию.

Чтобы запустить этот скрипт, перейдите в каталог, где он хранится, и введите: 
    
    
    chmod +x uninstallLaz.sh
    sudo ./uninstallLaz.sh
    

Затем введите свой пароль, когда будет запрошено. 

### Папка настроек Lazarus .lazarus в домашнем каталоге

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** Lazarus также создает папку .lazarus в вашем домашнем каталоге, где хранятся настройки среды и список недавно открытых проектов и файлов. Вы можете оставить эту папку в покое, если хотите, чтобы новая версия Lazarus, которую вы устанавливаете, использовала ваши старые настройки. MacOS обычно не показывает файлы или папки, имена которых начинаются с точки (.)

. 

Чтобы увидеть эту папку и ее содержимое, введите в окне терминала следующее: 
    
    
    cd ~/
    ls -al
    cd .lazarus
    ls
    

Чтобы удалить ее, выполните следующие команды: 
    
    
    cd ~/
    rm -rv .lazarus
    

# Известные проблемы

  * FPC 2.4.4 имеет ошибку. Вы не можете скомпилировать IDE с флагом проверки диапазона **-Cr**.
  * В OS X 10.4 вы должны вручную удалить любую предыдущую версию перед установкой нового dmg. Удалите следующие файлы и папки: 
    * /Developer/lazarus
    * /Library/Receipts/lazarus.pkg
    * /etc/lazarus
    * /usr/local/bin/lazbuild



# См. также

  * [Mac Installation FAQ](<../en/Mac_Installation_FAQ.md> "Mac Installation FAQ")



## Другие интерфейсы

  * [Lazarus known issues (things that will never be fixed)](<../en/Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<../en/Win32/64_Interface.md> "Win32/64 Interface") \- The winapi interface for Windows 95/98/Me/2K/XP/Vista, but not CE
  * [Windows CE Interface](<../en/Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<../en/Carbon_Interface.md> "Carbon Interface") \- The Carbon interface for Mac OS X
  * [Cocoa Interface](<../en/Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa interface for Mac OS X
  * [Qt Interface](<../en/Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, Mac OS X, Windows, and Linux-based PDAs
  * [Qt5 Interface](<../en/Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, Mac OS X, Windows, and Linux-based PDAs
  * [GTK1 Interface](<../en/GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, Mac OS X (X11), Windows
  * [GTK2 Interface](<../en/GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, Mac OS X (X11), Windows
  * [GTK3 Interface](<../en/GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, Mac OS X (X11), Windows
  * [fpGUI Interface](<../en/fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<../en/Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Платформозависимые советы

  * [OS X Programming Tips](<../en/OS_X_Programming_Tips.md> "OS X Programming Tips") \- Установка Lazarus, полезные инструменты, команды Unix и многое другое ...
  * [WinCE Programming Tips](<../en/WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Использование телефонного API, отправка SMS-сообщений и многое другое ...
  * [Windows Programming Tips](<../en/Windows_Programming_Tips.md> "Windows Programming Tips") \- Советы по программированию настольных приложений Windows.



### Mac How в статьях

  * [Mac How](<https://www.macforbeginners.com/articles/>) \- Если у вас возникли проблемы в решении некоторых серьезных проблем ([прим.перев.](</index.php?title=User:Zoltanleo&action=edit&redlink=1> "User:Zoltanleo \(page does not exist\)"): поправил ссылку на похожую, поскольку в оригинальной статье ссылка заблокирована спам-фильтром и ведет на левый сайт).



### Статьи по разработке интерфейсов

  * [Carbon interface internals](<../en/Carbon_interface_internals.md> "Carbon interface internals") \- Если вы хотите помочь улучшить интерфейс Carbon
  * [Windows CE Development Notes](<../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- Для карманных ПК и смартфонов
  * [Adding a new interface](<../en/Adding_a_new_interface.md> "Adding a new interface") \- Как добавить новый интерфейс набора виджетов
  * [LCL Defines](<../en/LCL_Defines.md> "LCL Defines") \- Выбор правильных опций для перекомпиляции LCL
  * [LCL Internals](<../en/LCL_Internals.md> "LCL Internals") \- Некоторая информация о внутренней работе LCL

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus_on_MacOS_X/ru](https://web.archive.org/web/20190823163613/https://wiki.freepascal.org/Installing_Lazarus_on_MacOS_X/ru)_
