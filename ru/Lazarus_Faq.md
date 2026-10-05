# Lazarus Faq

│ **[العربية (ar)](</Lazarus_Faq/ar> "Lazarus Faq/ar")** │  **[Deutsch (de)](</Lazarus_Faq/de> "Lazarus Faq/de")** │  **[English (en)](<../en/Lazarus_Faq.md> "Lazarus Faq")** │  **[español (es)](</Lazarus_Faq/es> "Lazarus Faq/es")** │  **[français (fr)](</Lazarus_Faq/fr> "Lazarus Faq/fr")** │  **[magyar (hu)](</Lazarus_Faq/hu> "Lazarus Faq/hu")** │  **[italiano (it)](</Lazarus_Faq/it> "Lazarus Faq/it")** │  **[日本語 (ja)](</Lazarus_Faq/ja> "Lazarus Faq/ja")** │  **[한국어 (ko)](</Lazarus_Faq/ko> "Lazarus Faq/ko")** │  **[português (pt)](</Lazarus_Faq/pt> "Lazarus Faq/pt")** │  **русский (ru)** │  **[slovenčina (sk)](</Lazarus_Faq/sk> "Lazarus Faq/sk")** │  **[中文（中国大陆） (zh_CN)](</Lazarus_Faq/zh_CN> "Lazarus Faq/zh CN")** │  **[中文（臺灣） (zh_TW)](</Lazarus_Faq/zh_TW> "Lazarus Faq/zh TW")** │    
****

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Этот FAQ может быть **устаревшим** в некоторых частях.

## Contents

  * 1 Общее
    * 1.1 Почему получаемые бинарные файлы очень большие?
    * 1.2 УСТАРЕЛО: Почему компоновка такая медленная в Windows?
    * 1.3 Мне нужен ppc386.cfg или fpc.cfg?
    * 1.4 Как я могу скомпилировать lazarus?
    * 1.5 Как собирать LCL проекты без Lazarus-а?
    * 1.6 Какая версия FPC требуется?
    * 1.7 Я не могу скомпилировать Lazarus
    * 1.8 При компиляции проекта возникает ошибка:
      * 1.8.1 "Cannot find Unit interfaces". Как можно это исправить?
    * 1.9 Когда я пытаюсь откомпилировать проект Delphi в lazarus, я получаю сообщение об ошибке
      * 1.9.1 Ошибка на строке :{$R *.DFM}. Как мне решить эту проблему?
      * 1.9.2 Ошибка: 'Identifier not found LazarusResources'.
    * 1.10 При обращении к событиям объектов, например OnClick объекта button, я получаю следующую ошибку: ERROR unit not found: stdCtrls
    * 1.11 Как внедрять содержимое небольшого файла в исполняемый файл? Как внедрять ресурсы?
    * 1.12 Что означают различные расширения файлов, используемые Lazarus?
    * 1.13 Когда я пишу _var mytext: text;_ , чтобы объявить текстовый файл, я получаю ошибку "Unit1.pas(32,15) Error: Error in type definition". Как можно это исправить?
    * 1.14 Я получаю ошибку при использовании Printer.BeginDoc
    * 1.15 Почему TForm.ClientWidth/ClientHeight - это то же самое, что и TForm.Width/Height?
  * 2 Отладка
    * 2.1 Как увидеть отладочные сообщения?
    * 2.2 Как мне просмотреть значение свойств?
    * 2.3 Почему отладчик не показывает некоторые переменные или структуры (выдавая ошибки "no such symbol"/"incomplete type")
    * 2.4 Как я могу проводить отладку компонентов и пакетов FCL с помощью Lazarus
  * 3 Contributing / Making Changes to Lazarus
    * 3.1 Я создал патч для пристыковки окна сообщений IDE к окну "Редактор исходного кода" (снизу)
    * 3.2 Я исправил/улучшил Lazarus. Как добавить мои изменения в официальный код Lazarus?
    * 3.3 How can I become a Lazarus developer and access management in the SVN and bug-tracker?
  * 4 Где объявлены ...
    * 4.1 Константы виртуальных клавиш
  * 5 Использование среды разработки
    * 5.1 Как я могу использовать "завершение идентификаторов"?
  * 6 Linux
    * 6.1 Как выполнить отладку в Linux без IDE?
    * 6.2 I can debug now but ddd does not find my sources or complains that they contain no code. Whats that?
    * 6.3 I receive an error during the linking that states /usr/bin/ld can't find -l<some lib>
    * 6.4 How can I convert a kylix 2 project into a lazarus project?
    * 6.5 When compiling lazarus the compiler can not find a unit. e.g.: gtkint.pp(17,16) Fatal: Can't find unit GLIB
    * 6.6 I have installed the binary version, but when compiling a simple project, lazarus gives: Fatal: Can't find unit CONTROLS
    * 6.7 Lazarus компилируется, но компоновка прерывается с ошибкой: libgdk-pixbuf not found
    * 6.8 Я использую SuSE, при компиляции получаю ошибку: "/usr/bin/ld: cannot find -lgtk Error: Error while linking"
    * 6.9 Лазарус после установки компонента падает с ошибкой runtime error 211
    * 6.10 Когда я запускаю программу, использующую потоки (threads), я получаю сообщение об ошибке "runtime error 232"
    * 6.11 У меня Ubuntu Breezy/Mandriva KDE3 и шрифты в Lazarus IDE выглядят слишком большими
    * 6.12 Как подключать и использовать сторонние файлы ресурсов (*.rc) в GTK приложении?
    * 6.13 I have Ubuntu and I cannot compile for Gtk2 due to missing libraries
    * 6.14 How can I compile a program for Gtk2?
    * 6.15 I get this message: "[WARNING] ** Multibyte character encodings (like UTF8) are not supported at the moment."
  * 7 Windows
    * 7.1 When I cycle the compiler, I get:The name specified is not recognized as an internal or external command, operable program or batch file.>& was unexpected at this time.
    * 7.2 When I cycle the compiler, I get: make[3]: ./ppc1.exe: Command not found
    * 7.3 When I try to make Lazarus I get:
      * 7.3.1 make.exe: * * * interfaces: No such file or directory (ENOENT). Stop.make.exe: * * * [interfaces_all] Error 2
      * 7.3.2 makefile:27: *** You need the GNU utils package to use this Makefile. Stop.
    * 7.4 How can I give my program an XP look like lazarus has?
    * 7.5 Когда я запускаю Windows программу, созданную в Lazarus-е, открывается консольное (DOS) окно
  * 8 Mac OS X
    * 8.1 Why does compiling a project fail with 'unknown section attribute: no_dead_strip'?
  * 9 Лицензирование
    * 9.1 Могу ли я создавать коммерческие приложения, используя Lazarus?
    * 9.2 Могу ли использовать в коммерческих приложениях дополнительные компоненты Lazarus?
    * 9.3 Как я могу узнать, что компонент является частью LCL?
    * 9.4 Могу ли я создавать коммерческие плагины для Lazarus?



## Общее

### Почему получаемые бинарные файлы очень большие?

Бинарные файлы очень большие из-за того, что включают в себя много отладочной информации для использования в отладчик [gdb](<http://www.gnu.org/software/gdb>) (GNU Debugger). Компилятор имеет настройку для удаления отладочной информации из исполняемого файла (-Xs), но из-за ошибки в компиляторе (версия 2.0.2 и ниже) она не работает корректно. Ошибка исправлена в версиях компилятора 2.0.4 и выше. 

Вы можете использовать программу "strip" для удаления отладочной информации из исполняемых файлов. Она находится в каталоге Lazarus'а: lazarus\pp\bin\i386-win32\\. 

Наберите "strip --strip-all <путь к исполняемому файлу>" в командной строке. 

Если Вы хотите сделать Вашу программу очень маленькой, то Вы можете попробовать использовать [UPX](<http://upx.sourceforge.net/>). UPX - это очень хороший exe-упаковщик. It includes no memory overhead due to in-place decompression. И также имеет очень быструю распаковку (~10 МБ/сек на Pentium 133). 

Для использования upx наберите "upx <путь к исполняемому файлу>" в командной строке. 

После использования strip и upx простая GUI программа на Lazarus'е получается: 

  * ~ 700Кб на Linux
  * ~ 420Кб на Windows



Для более детальной информации о недостатках использования UPX обратитесь к статье [ Размер имеет значение](<Size_Matters.md> "Size Matters/ru"). Также нужно отметить, что "hello world" программы lazarus уже включают в себя множество возможностей, таких как: 

  * Библиотеку работы с XML
  * Библиотеки для обработки файлов рисунков типа png, xpm, bmp и ico
  * Практически все виджеты Lazarus Component Library (LCL)
  * Все Runtime библиотеки Free Pascal



Всё это делает приложение большим, но это так же и включает всё, что может потребоваться нетривиальному приложению. 

Начальный размер исполняемого файла Lazarus велик, однако растёт в дальнейшем довольно медленно. Проект на С++ (это для примера, но относится и к другим языкам / инструментальные средства тоже), сначала очень маленький (типа "Hello world"), но быстро растет по экспоненте, когда Вы добавляете дополнительные возможности, чтобы написать нетривиальное приложение. 

  
[![Lazarus vs cpp.png](https://wiki.freepascal.org/images/d/de/Lazarus_vs_cpp.png)](</File:Lazarus_vs_cpp.png>)

  
**Краткое руководство по уменьшению размера Lazarus/FPC приложения** _(проверено в Lazarus 0.9.26)_

  * 1\. Project|Compiler Options|Code|Smart Linkable (-CX) -> Поставить галочку
  * 2\. Project|Compiler Options|Linking|Debugging|Display Line Numbers in Run-time ErrorBacktraces (-gl) -> Убрать галочку
  * 3\. Project|Compiler Options|Linking|Debugging|Strip Symbols From Executable (-Xs) -> Поставить галочку
  * 4\. Project|Compiler Options|Linking|Link Style|Link Smart (-XX) -> Поставить галочку



Самые важные элементы, мне кажется, 2 и 3. Для простого приложения выполнимый размер должен теперь составить 1-3 Мбайта вместо 15-20 Мбайт. В этом пункте Вы можете также попробовать: Project|Compiler Options|Code|Optimizations|smaller, вместо Faster -> Поставить галочку (Предупреждение: это может уменьшить производительность), 

  * 5\. (Опционально) Запустите UPX <путь к исполняемому файлу> для сжатия вашего бинарника дополнительно в 2-3 раза (Предупреждение: как сказано выше, есть недостатки в использовании UPX).



### УСТАРЕЛО: Почему компоновка такая медленная в Windows?

Эта проблема свойственна лишь старым версиям: FPC 2.2 и Lazarus 0.9.24. Пожалуйста обновите Лазарус. Всё написанное здесь относится **только** к старым версиям. 

В общем, компоновка на Windows занимает больше времени, чем на других платформах, потому что GNU компоновщик под эту операционную систему очень медленный. FPC (2.2) использует этот компоновщик при сборке программ. Эта проблема возникает только на Windows и только на слабых компьютерах (с процессором частотой менее 1ГГц и/или оперативной памятью меньше 128 Мегабайт) 

Также, если вы используете умную компоновку (smartlinking), общее время компиляции так же увеличиться. Подробнее об этом можно прочитать здесь: [File size and smartlinking](<../en/File_size_and_smartlinking.md> "File size and smartlinking")

Для решения этой проблемы, был разработан внутренний компоновщик. Он есть во всех версиях FPC, начиная с 2.2. Его использование позволило в значительной степени сократить общее время компиляции. Внутренний компоновщик используется только при компиляции для Windows системы. 

### Мне нужен ppc386.cfg или fpc.cfg?

Вам нужен только fpc.cfg. В нем указаны пути, по которым компилятор будет искать библиотеки. 

### Как я могу скомпилировать lazarus?

Нет ничего проще :) 
    
    
    $ cd lazarus
    $ make clean all
    

### Как собирать LCL проекты без Lazarus-а?

  * В тех случаях, когда использование графической среды невозможно, вы можете воспользоваться утилитой командной строки: lazbuild.



Эта утилита используется для сборки проектов и lazarus-пакетов (packages). 

  * Если вам нужно собрать LCL приложение без среды разработки и не используя lazbuild, вам необходимо добавить следующие строки в файл _fpc.cfg_


    
    
      # строчки начинающиеся с # можно не добавлять
      # это комментарии
      # пути для других модулей и компонентов 
     -Fu{Путь_к_лазарусу}/lcl/units/{целевая_система}
     -Fu{Путь_к_лазарусу}/components/units/{целевая_система}
    

    Где {целевая_система} это системный префикс, указывающий для какой системы располагаются модули. Обычно этот префикс представляет собой пару имён "Процессор-ОСь", например i386-win32, i386-linux, i386-darwin.:

После добавления этих строк в конфигурационный файл, вызовете команду: fpc myproject.lpr myproject.lpr это имя основного файла проекта (модуль начинающийся с "program" или "library"). Но имя может быть другим, т.к. Лазарус не принуждает Вас использовать расширение .lpr. 

Кроме того, если Ваш проект использует какие-либо особые настройки, вы можете получить командную строку для компиляции, используя меню в Лазарусе: _Проект- >Параметры проекта...->Показать параметры_ (параметров может быть очень много, удобно скопировать их в отдельный скрипт файл .bat или .sh); 

### Какая версия FPC требуется?

Версия 2.4.0 для всех операционных систем. 

### Я не могу скомпилировать Lazarus

  1. Проверьте версию компилятора
  2. Проверьте версию (fpc)библиотек, они должны быть той же версии
  3. Проверьте путь установки компилятора на наличие в нем пробелов. Пробелов в нем не должно быть!
  4. Проверьте наличие файла fpc.cfg, а не старого файла ppc386.cfg
  5. Проверьте также OS-dependent FAQs



### При компиляции проекта возникает ошибка:

#### "Cannot find Unit interfaces". Как можно это исправить?

Ошибка возникает потомучто компилятор не может найти 'interfaces.ppu' **или** он найден, но он поврежден, неправильной версии или просрочен. 

Этот модуль может находиться в {LazarusDir}\lcl\units\\{TargetCPU}-{TargetOS}\\{LCLWidgetSet}\interfaces.ppu. Например: /home/username/lazarus/lcl/units/i386-linux/gtk/interfaces.ppu. 

Убедитесь, что он только здесь. Если находятся несколько версий interfaces.ppu, то вы возможно получите неверную конфигурацию (к примеру добавлен каталог lcl в список путей поиска). Удалите все interfaces.ppu, оставьте только тот что в каталоге указаном выше. 

Если вы выберете другой widgetset, то при пересборке lazarus, понадобится пересобрать LCL для этого widgetset. 

Если ошибка возникает несмотря на то что 'interfaces.ppu' на месте - значит используется, иная compiler/rtl для компиляции проекта нежели для компиляции Lazarus IDE. Можно сделать одно из следующего: 

  * Пересобрать LCL (или полностью Lazarus) с компилятором выбраным в Environmnent Options. Для этого можно кликнуть Tools -> Build Lazarus. Перед этим проверьте текущие настройки в Tools -> Configure Build Lazarus.
  * Поменять компилятор в Environment Options на тот что используется для компиляции Lazarus. Посмотрите внимательно также в Environment Options что используются корректный путь к каталогу Лазаря (Lazarus Directory) и к исходникам FPC. Убедитесь что есть только только один файл конфигурации fpc.cfg - он должен располагаться /etc/ для Linux/Unix или вместе с компилятором fpc под Windows. Попробуйте запустить "fpc -vt bogus" для того чтобы увидеть какой fpc.cfg используется на вашей системе. Мешающие копии обычно появляются при обновлении компилятора; они могут находится в домашнем каталоге или вместе с пересобраным новым компилятором. УДАЛИТЕ ИХ!!
  * Можно также попробовать сменить текущий widgetset проекта. Например, примерный проект "objectinspector" поставляемый с Лазарем поумолчанию использует gtk. Компиляция этого проекта наверняка выдаст "Can't find unit interfaces" под Windows. Смена widgetset на default(Win32) в Project | Compiler Options... | LCL Widget Type (various) должно исправить это.



### Когда я пытаюсь откомпилировать проект Delphi в lazarus, я получаю сообщение об ошибке

#### Ошибка на строке :{$R *.DFM}. Как мне решить эту проблему?

Lazarus (а точнее - Linux) не знает о таком понятии, как "ресурсы", и не может использовать некоторые связанные с ними понятия, пришедшие из Delphi/Win32. Однако, Lazarus использует методы, обеспечивающие совместимость с этими понятиями. Вы сможете использовать формы Delphi (файлы .dfm), если выполните следующие шаги: 

  * Используйте текстовые версии .dfm-файлов. D5 и более поздние версии используют такие файлы по умолчанию. Если у вас более старые файлы, выполните следующее: нажмите ALT-F12 для просмотра кода формы в виде текста и скопируйте/вставьте текст. Если у вас есть текстовый .dfm-файл, просто скопируйте его содержание в .lfm-файл.
  * Создайте файл с помощью lazres (в меню lazarus/tools) следующей командой: lazres yourform.lrs yourform.lfm
  * Добавьте следующую строку в секцию initialization:


    
    
         initialization
         {$I yourform.lrs}
    

Пожалуйста, помните, что не все свойства объектов, описанные в dfm-файлах, поддерживаются Lazarus, и часть из них может вызвать падения IDE. 

Примечание: Вы не получите этой ошибки, начиная с версии Lazarus 0.9.29 (SVN) при использовании FreePascal 2.4.0 и выше. Компилятор этой версии умеет включать такие ресурсы в исполняемый файл на всех платформах. Тем не менее, проблема несовместимости отдельных свойств от этого не исчезает. 

#### Ошибка: 'Identifier not found LazarusResources'.

При создании формы Lazarus автоматически добавляет некоторые необходимые модули в секцию uses вашего модуля, содержащего форму. После конвертации из Delphi в модуле могут отсутствовать необходимые ссылки. В данном конкретном случае вам необходимо добавить в секцию uses модуль LResources. 

### При обращении к событиям объектов, например OnClick объекта button, я получаю следующую ошибку: ERROR unit not found: stdCtrls

Убедитесь (Project -> Project Inspector),что ваш проект зависит от пакета 'LCL' и что вы установили исходники FPC. 

Lazarus - это IDE (среда разработки) и библиотека визуальных компонентов LCL. Все другие вещи, как IO, Database, FCL и RTL предоставляются FPC. IDE нужны пути ко всем исходникам. 

Пути к исходникам FPC могут быть установлены через: Environment -> Environment Options -> Files -> FPC source directory (Окружение -> Параметры -> Файлы -> Каталог исходного кода FPC). 

### Как внедрять содержимое небольшого файла в исполняемый файл? Как внедрять ресурсы?

Например, 
    
    
    /your/lazarus/path/tools/lazres sound.lrs sound1.wav sound2.wav ...
    

создаст sound.lrs из sound1.wav и sound2.wav. 

Потом включите его *после* lrs-файла формы: 
    
    
    ...
    initialization
    {$i unit1.lrs} // this is main resource file (first)
    {$i sound.lrs} // user defined resource file
    
    end.
    

В Вашей программе эти ресурсы можно использовать следующим образом: 
    
    
    Sound1AsString:=LazarusResources.Find('sound1').Value;
    

### Что означают различные расширения файлов, используемые Lazarus?

Глава [Lazarus Tutorial#The Lazarus files](<../en/Lazarus_Tutorial.md> "Lazarus Tutorial") разъясняет назначение некторых расширений. Вот их краткий список: 

`*.lpi`
    файл с информацией о проекте Lazarus (в формате XML; содержит настройки, относящиеся к конкретному проекту)
`*.lpr`
    программный файл Lazarus; содержит основной Pascal-код программы
`*.lfm`
    файл формы Lazarus; содержит информацию обо всех объектах, размещённых на форме (хранится в специальном текстовом формате; связанные с объектами формы действия хранятся в одноимённом `*.pas`-файле)
`*.pas` или `*.pp`
    модуль с Pascal-кодом (обычно связан с формой в одноимённом `*.lfm`-файле)
`*.lrs`
    файл ресурсов Lazarus (это генерируемый файл; не является файлом ресурсом Windows).
    Этот файл может быть создан с помощью утилиты lazres (в каталоге Lazarus/Tools) путё вызова из командной строки: lazres myfile.lrs myfile.lfm
`*.ppu`
    скомпилированный модуль
`*.lpk`
    информационный файл для пакета Lazarus (в формате XML; содержит настройки, относящиеся к конкретному пакету)

### Когда я пишу _var mytext: text;_ , чтобы объявить текстовый файл, я получаю ошибку "Unit1.pas(32,15) Error: Error in type definition". Как можно это исправить?

Класс TControl содержит свойство [Text](<http://lazarus-ccr.sourceforge.net/docs/lcl/controls/tcontrol.text.html> "doc:lcl/controls/tcontrol.text.html"). В методе формы используется тип[Text](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/text.html> "doc:rtl/system/text.html") из модуля system. Вы можете использовать тип [TextFile](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/text.html> "doc:rtl/system/text.html"), который всего лишь является другим названием типа Text, или можете добавить модуль при объявлении (см. в примере): 
    
    
      var
      MyTextFile: TextFile;
      MyText: System.Text;
    

Сходный конфликт имен существует и при связывании и закрытии текстового файла. TForm имеет методы _assign_ и [Close](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tcustomform.close.html> "doc:lcl/forms/tcustomform.close.html"). Вместо них вы можете использовать [AssignFile](<http://lazarus-ccr.sourceforge.net/docs/rtl/objpas/assignfile.html> "doc:rtl/objpas/assignfile.html") и [CloseFile](<http://lazarus-ccr.sourceforge.net/docs/rtl/objpas/closefile.html> "doc:rtl/objpas/closefile.html"), или же добавлять имя модуля _System_ (System.Close, System.Assign). 

### Я получаю ошибку при использовании Printer.BeginDoc

Модуль Printers должен быть добавлен в секцию uses. 

Пакет Printer4Lazarus должен быть добавлен в зависимости Вашего проекта в IDE: Project|Project Inspector|Add|New Requirement|Package Name: 

Если пакета Printer4Lazarus нет в списке пакетов, то Вам нобходимо установить его. Пакет является частью установки Lazarus и может быть найден по следующему пути: [каталог установки lazarus]\components\printers 

Если Вы используете стандартный путь для установки Lazarus'а, то [каталог установки larazus] находится: 

  * Windows: c:\lazarus
  * Linux: /usr/lib/lazarus



Данное решение также применимо, если Вы получаете исключения при использовании Printer.Printers 

### Почему TForm.ClientWidth/ClientHeight - это то же самое, что и TForm.Width/Height?

TForm.Width/Height не включают границ окна, поскольку не существует способа получить размер этих границ на всех платформах. Без надёжного способа LCL будет перемещать формы по всему экрану или бесконечно изменять их размер. 

В конечном итоге, когда появится надёжный способ получения размера и позиции окна вместе с его границами для всех платформ, это будет изменено. Для сохранения совместимости со старыми LCL-формами, будет введён номер версии и использованы некоторые другие дополнительные методы. 

## Отладка

### Как увидеть отладочные сообщения?

В модуле LCLProc в LCL есть две процедуры для вывода отладочных сообщений. Они называются: 

  * **DebugLn:** которая работает также, как WriteLn, но принимает только строки.
  * **DbgOut:** которая работает также, как Write, но принимает только строки.



В обычных условиях сообщения выводятся в stdout. Если stdout закрыт (например когда приложение {$AppType Gui} или откомпилировано с ключом -WG под Windows), сообщения не выводятся никуда. 

Отладочные сообщение могут также выводится в файл. Код инициализации модуля LCLProc проверяет командую строку Lazarus.exe's на предмет наличия ключа '--debug-log=<file>'. Если этот ключ присутствует - весь последующий отладочный вывод направляется в <file>. 

Если этого ключа нет, проверяется существование системной переменной окружения xxx_debuglog, где xxx - имя файла программы без расширения. Для Lazarus это будет lazarus_debuglog. Если такая переменная окружения существует, файл указанный в ней будет использован для вывода отладочных сообщений. Пример: если вы сделаете: 
    
    
    set lazarus_debuglog=c:\lazarus\debug.txt
    

то отладочные сообщения будут выводится в c:\lazarus\debug.txt. 

Так как это реализовано в lclproc, любое приложение использующее lclproc может использовать этот механизм вывода отладочных сообщений. 

Отладка Lazarus-а
    Наиболее полезно для Windows: Если вы хотите выводить сообщения в консоль, добавьте {$APPTYPE console} в lazarus.pp ; После чего перекомпилируйте Lazarus.

### Как мне просмотреть значение свойств?

Вам нужно использовать самую последнюю версию FPC из исходников (2.5.1) или релиз 2.4.0. Любая версия позднее указанных, так же подойдёт. 

Если Вы скомпилируете приложение, использую ключ -gw (отладочная информация dwarf), Вы сможете просмотреть значения свойств. 

**Внимание:** это возможно, только для тех свойст, которые напрямую связан с членом класса, директива "read" указывает на переменную, а не метод. 

Если свойство возвращает значение через функцию, то достаточно опасно проверять её значение. Риск заключается в том, что для проверки её значения требуется вызвать процедуру или функцию, что может привести к изменению других значений и/или данных. А это значит, что данные изменились в режиме отладки, и дальнейшее исполнение программы, будет отличаться от работы без отладчика. 

Проверка свойства по результату функции (как описана выше), ещё не реализована. 

### Почему отладчик не показывает некоторые переменные или структуры (выдавая ошибки "no such symbol"/"incomplete type")

Для решения проблем с: \- свойствами  
\- динамическими массивами  
\- переменными во вложенных процедурах  
\- "no such symbol in context"  
\- "incomplete type"  


смотри здесь [GDB Debugger Tips](<../en/GDB_Debugger_Tips.md> "GDB Debugger Tips")

### Как я могу проводить отладку компонентов и пакетов FCL с помощью Lazarus

Компоненты и классы FCL скомпилированны по умолчанию без отладночной информации. Как результат - gdb не моэет получить доступ к методам и свойствам этих объектов. Для пересборки компонентов FCL необходимо включить ключ компилятора "-gl" для генерации отладочной информации. 

Этот пример предпологает, что у вас Linux дистрибутив с установленным FPC в папке /usr/local/ и вам необходимо включить отладочную информацию для пакета fcl-db. По аналогии с пакетом fcl-db, используемом в данном примере, вы можете применить эти команды для ЛЮБЫХ пакетов, содержащихся в дистрибутиве. 

В начале, вам необходимо найти путь к установленному FPC проверив ваш конфигурационный файл FPC. Это файл (fpc.cfg) расположен /etc/fpc.cfg. Просмотрите содержимое fpc.cfg определить папку установки. Обратите внимание на строки, начинающиеся с -Fu в fpc.cfg: 
    
    
    -Fu/usr/local/lib/fpc/$fpcversion/units/$fpctarget/*
    

При создании скрипта для установки модулей в папку INSTALL_PATH/lib/fpc/$fpcversion/units/$fpctarget/, вы должны быть уверены, что /usr/local это путь установки FPC, и он должен быть присвоем INSTALL_PREFIX, в противном случае Make-скрипт установи модули в неправильную папку или вообще завершится с ошибкой. 

**Step 1** : Открыть терминал и набрать терминале  
**Step 2** : cd /user/local/share/src/fpc-2.3.1/fpc/fcl-db/  
**Step 3** : sudo make clean all install INSTALL_PREFIX=/usr/local OPT=-gl  


Замечание: Параметр INSTALL_PREFIX правильно указан для установки модулей. В примере ниже /usr/local - это путь по умолчанию для fpc в Linux, но он может сильно отличаться в других операционных системах 
    
    
    make clean all install INSTALL_PREFIX=/usr/local OPT=-gl
    

В конце, после пересборки любого FCL пакета, вам возможно необходимо будет пересобрать LCL. 

## Contributing / Making Changes to Lazarus

### Я создал патч для пристыковки окна сообщений IDE к окну "Редактор исходного кода" (снизу)

Такие патчи не будут приняты, так как они реализуют лишь малую часть требуемой функциональности стыковки (docking). Цель состоит в создании полноценного менеджера стыковки и его использовании. Полноценный менеджер стыковки(dock manager) может соединять все окна и позволяет пользователю определять, как их стыковать (должно ли окно сообщений быть над или под окном кода ... или вообще быть отделено от него). К примеру: 
    
    
    +-------------------++--+
    |menu               ||  |
    +-------------------+|  |
    +--++---------------+|  |
    |PI|| Source Editor ||CE|
    +--+|               ||  |
    +--+|               ||  |
    |  |+---------------++--+
    |OI|+-------------------+
    |  ||messages           |
    +--++-------------------+
    

Менеджер стыковки может сохранить это расположение и восстановить его при следующем старте. Предпочтительно, если менеджер может работать не только с окнами, но и со страницами редактора кода. Менеджер стыковки не требует использования drag&drop. Все патчи реализующие стыковку без менеджера стыковки усложняют реализацию настоящего менеджера стыковки и потому будут отклонены. 

В качестве временного решения можно использовать это расширение IDE: [Manual Docker](<../en/Manual_Docker.md> "Manual Docker")

### Я исправил/улучшил Lazarus. Как добавить мои изменения в официальный код Lazarus?

Создайте патч и пришлите его разработчикам. Более подробную информацию смотрите здесь [Creating A Patch/ru](<Creating_A_Patch.md> "Creating A Patch/ru"). 

### How can I become a Lazarus developer and access management in the SVN and bug-tracker?

First of all: you must learn about Lazarus, to prove your knowledge and skill. Start by reading the [wiki articles](<../en/Lazarus_Documentation.md> "Lazarus Documentation"), read the Lazarus source code, giving a look at the [Lazarus Bug-Tracker](<http://www.lazarus.freepascal.org/mantis>), fix some bugs, and if you think you are ready, contact the developers on the [mailing list](<http://www.mail-archive.com/lazarus@miraclec.com>). 

## Где объявлены ...

### Константы виртуальных клавиш

Константы виртуальных клавиш (VK_UP, VK_ESCAPE и т.д ) объявлены в [LCLType](<http://lazarus-ccr.sourceforge.net/docs/lcl/lcltype> "doc:lcl/lcltype"). Добавьте LCLtype в **uses** секцию. 

## Использование среды разработки

### Как я могу использовать "завершение идентификаторов"?

Чтобы вызвать окно завершения идентификатора нажмите [ctrl][space] (по умолчанию для Windows и Linux). 

Вы можете настроить автоматическое появление этого окошка в пункте меню _Окружение- >Редактор->Code Tools->Автоматические функции_

## Linux

### Как выполнить отладку в Linux без IDE?

Прежде всего потребуется отладчик. gdb это стандартный отладчик под линукс, имеющий несколько GUI-интерфейсов. Наиболее распространённый интерфейс - ddd - является частью большинства популярных дистрибутивов. Для компиляции lazarus/lcl с информацией для отладчика вам нужно использовать следующие команды для запуска отладочной сессии: 
    
    
     $ make clean; make OPT=-dDEBUG
     $ ddd lazarus
    

Однако, следует отметить что ddd не такой удобный как например отладчик Lazarus. Особенно если он используется для просмотра значений имеющихся переменных, учитывая что ddd/gdb регистрозависимы, а Pascal - регистронезависим. Поэтому, чтобы видеть значения переменных, необходимо набирать их имена в верхнем регистре. Для получения более подробной информации обратитесь к fpc-manuals. 

### I can debug now but ddd does not find my sources or complains that they contain no code. Whats that?

This is a path-related problem with either gdb or ddd. You can avoid this by 

  * Use the "Change directory" command from the ddd menu and choose the directory where the sources are located. The drawback of this method is that you now can't use the source of the program you started with (e.g. lazarus). Thus it may be neccessary to change the directory multiple times.
  * In ddd goto [Edit] [gdb-settings] and set the search-path
  * Create a $(HOME)/.gdbinit file like:


    
    
         directory /your/path/to/lazarus
         directory /your/path/to/lazarus/lcl
         directory /your/path/to/lazarus/lcl/include
    

### I receive an error during the linking that states /usr/bin/ld can't find -l<some lib>

**Package Based Distributions**
    You need to install the package that provides the lib<somelib>.so or lib<somelib>.a files. Dynamic libs under linux have the extension .so, while static libs have the extension .a. On some Linux distro's you have installed the package (rpm, deb) <packagename> which provides <some lib>, but you also need the development package (rpm, deb), normally called <packagename>-dev, which contains the .a (static lib) and/or the .so (dynamic lib).
    Some distributions have commands to find which package contains a file:
    **Mandriva**
    
    
    []$ urpmf lib<somelib>.so
    

    will list all packages containing the file named lib<somelib>.so, you'll have to install those ending in -devel

    **Debian**

    install the apt-file utility (apt-get install apt-file) then
    
    
    []$ apt-file search lib<somelib>.so
    

    will list all packages containing the file named lib<somelib>.so, you'll have to install those ending in -dev

  


**Source Based Distributions and Manual Compilation (LFS)**
    Make sure that there is a lib<somelib>.a in the path, and that it contains the right version. To let the linker find the dynamic library, create a symlink called lib<some lib>.so to lib<some lib><version>-x,y.so if necessary (and/or for static lib; lib<some lib>.a to lib<some lib><version>-x,y.a).

**FreeBSD**
    As source based distro's, and also make sure you have -Fl/usr/local/lib in your fpc.cfg and/or Lazarus library path. Keep in mind that GTK1.2 has "gtk12" as package name under FreeBSD. (same for glib) NOTE: This has changed as of late. Newest ports have gtk-12 and glib-12 as well. You might stumble on this problem, since FPC requires the "-less" ones, you will need to symlink them like this:
    
    
    []# cd /usr/local/lib && ln -s libglib-12.so libglib12.so
    []# cd /usr/X11R6/lib && ln -s libgtk-12.so libgtk12.so
    []# cd /usr/X11R6/lib && ln -s libgdk-12.so libgdk12.so
    

**NetBSD**
    As source based distro's, and also make sure you have -Fl/usr/pkg/lib in your fpc.cfg and/or Lazarus library path

### How can I convert a kylix 2 project into a lazarus project?

Nearly the same way as converting a Kylix project into a Delphi/VCL project. 

The LCL (Lazarus Component Library) tries to be compatible to Delphi's VCL. Kylix's CLX tries to be QT compatible. Here are some general hints: 

  * Rename all used CLX Q-units like QForms, QControls, QGraphics, ... into their VCL counterparts: Forms, Controls, Graphics, ...
  * Add LResources to the uses section of every form source
  * Rename or copy all .xfm files to .lfm files.
  * Rename or copy .dpr file to .lpr file.
  * Add "Interfaces" to the uses section in the .lpr file.
  * Remove {$R *.res} directive
  * Remove {$R *.xfm} directive
  * Add {$mode objfpc}{$H+} or {$mode delphi}{$H+} directive to .pas and .lpr files
  * Add an initialization section to the end of each form source and add an include directive for the .lrs file (lazarus resource file):


    
    
     initialization
       {$I unit1.lrs}
    

    The .lrs files can be created via the lazres tool in: (lazarusdir)/tools/lazres.
    For example: ./lazres unit1.lrs unit1.lfm

  * Fix the differences. The LCL does not yet support every property of the VCL and the CLX is not fully VCL compatible.


  * To make it more platform independant, reduce unit libc (which is deprecated) references and substitute with native FPC units like baseunix/unix as much as possible. This will be necessary to support other targets than linux/x86 (including OS X, FreeBSD and Linux/x86_64)



### When compiling lazarus the compiler can not find a unit. e.g.: gtkint.pp(17,16) Fatal: Can't find unit GLIB

1\. Check a clean rebuild: do a 'make clean all' 

2\. Check if the compiler has the correct version (2.0.4 or higher) 

3\. Check if the compiler is using the right config file. The normal installation creates /etc/fpc.cfg. But fpc also searches for ~/.ppc386.cfg, ~/.fpc.cfg, /etc/ppc386.cfg and it uses only the first it finds. 

    **Hint:** You can see which config file is used with 'ppc386 -vt bogus'
    Remove any ppc386.cfg as it is really obsolete.

4\. Check if the config file (/etc/fpc.cfg) contains the right paths to your fpc libs. There must be three lines like this: 
    
    
       -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget
       -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/rtl
       -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/*
    

    The first part of these paths (/usr/lib/fpc) depends on your system. On some systems this can be for example /usr/local/lib/fpc/... .
    **Hint:** You can see your searchpaths with 'ppc386 -vt bogus'

5\. Check that the config file (/etc/fpc.cfg) does not contain search paths to the lcl source files (.pp, .pas): 
    
    
     forbidden: -Fu(lazarus_source_directory)/lcl
     forbidden: -Fu(lazarus_source_directory)/lcl/interfaces/gtk
    

    If you want to add the lcl for all your fpc projects, make sure that the two paths look like the following and are placed after the above fpc lib paths:
    
    
     -Fu(lazarus_source_directory)/lcl/units/$fpctarget
     -Fu(lazarus_source_directory)/lcl/units/$fpctarget/gtk
    

6\. Check if the missing unit (glib.ppu) exists in your fpc lib directory. For example the gtk.ppu can be found in /usr/lib/fpc/$fpcversion/units/i386-linux/gtk/. If it does not exists, the fpc lib is corrupt and should be reinstalled. 

7\. Check if the sources are in a NFS mounted directory. In some cases the NFS updates created files incorrectly. Please, try to move the sources into a non NFS directory and compile there. 

8\. If you are still not succeeded try to use samplecfg script as follows: 

_#_ cd /usr/lib/fpc/_version_ / 

_#_ sudo ./samplecfg /usr/lib/fpc/_\$version_ /etc 

Note! Do not put - / - after etc because if you do that the system will create a file - /etc/fpc.cfg/fpc.cfg. In fact we want that samplecfg make a file - /etc/fpc.cfg - not the folder /etc/fpc.cfg. 

### I have installed the binary version, but when compiling a simple project, lazarus gives: Fatal: Can't find unit CONTROLS

Probably you are using a newer fpc package, than that used for building the lazarus binaries. The best solution is to download the sources and compile lazarus manually. You can download the source snapshot or get the source via svn: 
    
    
     $ bash
     $ svn checkout <http://svn.freepascal.org/svn/lazarus/trunk> lazarus
     $ cd lazarus
     $ make clean all
    

Make sure that lazarus get the new source directory: Environment->General Options->Files->Lazarus Directory Top 

### Lazarus компилируется, но компоновка прерывается с ошибкой: libgdk-pixbuf not found

Для решения проблемы нужно установить библиотеку gdk-pixbuf library для gtk1.x: 

Библиотека gdk-pixbuf может быть найдена: 

RPM пакет: [http://rpmfind.net/linux/rpm2html/search.php?query=gdk-pixbuf&submit=Search+...&system=&arch=](<http://rpmfind.net/linux/rpm2html/search.php?query=gdk-pixbuf&submit=Search+...&system=&arch=>)

Debian пакет: libgdk-pixbuf-dev 

Исходники: <ftp://ftp.gnome.org/pub/gnome/unstable/sources/gdk-pixbuf/>

  
**Ubuntu 8.10:**

Если вы собираете Lazarus с GTK 2.0 вы можете получить ошибку "libgdk-pixbuf2.0 not found" . Для решения проблемы просто установите пакет libgtk2.0-dev, используя команду apt следующим образом (используйте sudo при необходимости): 
    
    
    apt-get install libgtk2.0-dev
    

### Я использую SuSE, при компиляции получаю ошибку: "/usr/bin/ld: cannot find -lgtk Error: Error while linking"

Ранние версии SuSE (до SuSE 11) устанавливали gtk библиотеки devel в директорию /opt/gnome/lib (или /opt/gnome/lib64 для 64-й версии), что не является общепринятым путём для библиотек. 

Для решения проблемы, вы можете добавить этот путь библиотеки в конфигурационный файл FPC (/etc/fpc.cfg), следующей строкой: 
    
    
    -Fl/opt/gnome/lib.
    

### Лазарус после установки компонента падает с ошибкой runtime error 211

После установки компонента Лазарус падает со следующей ошибкой: 
    
    
    Threading has been used before cthreads was initialized.
    Make cthreads one of the first units in your uses clause.
    Runtime error 211 at $0066E188
    

Как это исправить? 

Установленный компонент использует потоки. FPC не включает поддержку многопоточности автоматически на *nix системах, по-этому её необходимо включать вручную. Её включение происходит использованием модуля cthreads. Любое приложение, использующее такой компонент должна использовать этот модуль, причём он должен быть первым подключаемым модулем в программе. Лазарус так же не является исключением. 

Подключить модуль cthreads можно двумя способами: 

1) Откройте пакет. В редакторе пакетов нажмите на _Параметры (Options)_. Во вкладке _Использование (Usage)_ добавьте настройку _Пользовательские (custom)_ и запишите **-dUseCThreads**. Пересоберите Лазарус. В этом случае модуль cthreads подключиться автоматически для unix систем. 

2) Чтобы не изменять пакет, можно добавить директиву компиляци при сборке самого Лазаруса. Откройте меню Сервис(Tools)->Параметры сборки Lazarus(Configure "build Lazarus)". В диалоге Параметры "Cборки Lazarus"("build Lazarus") в поле "Параметры:"("Options:") впишите -Facthreads и нажмите кнопку "OK". После этого добавьте пакет и пересоберите среду. 

_Совет:_ Предыдущая копия Лазаруса (исполнительный файл, который при запуске на выдавал ошибку), скорее всего, находится в той же папке что и текущая версия, но с расширением .old. 

См.также:[Модули, необходимые для мультипоточных приложений](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial/ru")

### Когда я запускаю программу, использующую потоки (threads), я получаю сообщение об ошибке "runtime error 232"

Полное сообщение выглядит так: 
    
    
    This binary has no thread support compiled in.
    Recompile the application with a thread-driver in the program uses
    clause before other units using thread.
    Runtime error 232
    

**Решение** : Добавьте модуль cthreads в секцию uses главного модуля вашей программы (обычно это .lpr-файл). 

### У меня Ubuntu Breezy/Mandriva KDE3 и шрифты в Lazarus IDE выглядят слишком большими

Попробуйте следующее: Создайте файл с именем «.gtkrc.mine» в домашней директории (если он не существует) и внесите в него данный текст: 
    
    
    style "default-text" {
           fontset = "-*-arial-medium-r-normal--*-100-*-*-*-*-iso8859-1,\
                      -*-helvetica-medium-r-normal--*-100-*-*-*-*-*-*"
    }
    
    class "GtkWidget" style "default-text"
    

Если это не сработает, попробуйте создать ссылку .gtkrc на .gtkrc.mine. Данный способ был опробован в Xubuntu 7.10 и Mandriva 2009.0 KDE3 

**Примечание:** Если Lazarus был скомпилирован с использованием библиотеки Gtk1.2, то настройка шрифтов в Gtk2 не будет влиять на отображение текста в нём. 

### Как подключать и использовать сторонние файлы ресурсов (*.rc) в GTK приложении?

Вариант 1. Переименуйте файл ресурсов (rc) _мой_ресурс.rc_ в _имя_программы.gtkrc_ и поместите в папку с исполняемым файлом программы. 

Вариант 2. Подключите модуль _GtkInt_ в секцию _uses_ исходного кода проекта (*.lpr), и допишите код 
    
    
    {$IFDEF LCLGtk} 
      GTKWidgetSet.SetRCFilename('имя_вашего_файла_ресурса');
    {$ENDIF LCLGtk}
    

перед вызовом _Application.Initialize_. 

Вариант 3. Используя модуль _gtk2_ вызовите метод _gtk_rc_parse('имя_файла_ресурса')_ а также _gtk_rc_reparse_all_. 

### I have Ubuntu and I cannot compile for Gtk2 due to missing libraries

Ubuntu has a problem with not creating all the symbolic links that you'll need even when the libraries are installed. Make sure that all missing libraries when trying to link for Gtk2 have their appropriate links. For instance, you might need to do: 
    
    
    cd /usr/lib
    sudo ln -s libgdk-x11-2.0.so.0 libgtk-x11-2.0.so
    

Make sure that the [whatever].so symbolic links are created and point to the actual libraries. 

### How can I compile a program for Gtk2?

At the moment, the Gtk2 compiled IDE is a little unstable, but you can compile software for Gtk2 using the Gtk1 IDE. 

To start with recompile LCL for Gtk2. Go to the menu "Tools"->"Configure Build Lazarus" and set LCL to clean+build and everything else to none. 

Now click Ok and go to the menu "Tools"->"Build Lazarus" 

Now you can compile your software with Gtk2 going on the Compiler options and changing the widgetset to Gtk2. 

### I get this message: "[WARNING] ** Multibyte character encodings (like UTF8) are not supported at the moment."

Since revision 10535 (0.9.21) this message doesn't exist anymore. Previously it was used to warn that a UTF-8 encoding was used. The internal keyhandling routines for the gtk1 widgetset couldn't handle such encoding for keypresses, with the result that keypresses with for instance accented chars were not or wrong detected. 

(original text for older versions of lazarus)  
~~This warning message indicates that your locale enconding is set to utf-8. If you are using Gtk 1 this can be a serious problem and prevent the correct working of Lazarus or software created with Lazarus.~~

~~To work around this, just change your locale to a non utf-8 before executing the program on the command line, like this:~~

~~
    
    
    export LC_CTYPE="pt_BR"
    export LANG="pt_BR"
    export LANGUAGE="pt_BR"
    ./lazarus

~~~~~~

~~Substitute pt_BR with the locale for your country. You can create a script to automate this.~~

## Windows

### When I cycle the compiler, I get:The name specified is not recognized as an internal or external command, operable program or batch file.>& was unexpected at this time.

In the compiler directory there is an OS2 scriptfile named make.cmd. Different versions of Windows also see this as a script file, so remove it since what is needed for OS2 becomes a hindrance on Windows. 

### When I cycle the compiler, I get: make[3]: ./ppc1.exe: Command not found

I don't know why but somehow make has lost its path. Try to cycle with a basedir set like: make cycle BASEDIR=your_fpc_source_dir_herecompiler 

### When I try to make Lazarus I get:

#### make.exe: * * * interfaces: No such file or directory (ENOENT). Stop.make.exe: * * * [interfaces_all] Error 2

You need to upgrade your make. 

#### makefile:27: *** You need the GNU utils package to use this Makefile. Stop.

Make sure you didn't install FPC in a path with spaces in the name. The Makefile doesn't support it. 

  


### How can I give my program an XP look like lazarus has?

Project -> Project Options -> Check 'Use manifest to enables themes'. 

### Когда я запускаю Windows программу, созданную в Lazarus-е, открывается консольное (DOS) окно

Укажите параметр -WG (Windows графическое приложение) в командной строке компилятора или установите флажок 
    
    
    Проект->Опции проекта->Параметры компилятора -> Связывание -> Графическое приложение Win32 
    

англ: 
    
    
    Project->Project Options-> Compiler Options -> Linking -> Win32 GUI application.
    

## Mac OS X

### Why does compiling a project fail with 'unknown section attribute: no_dead_strip'?

Dead code stripping is not supported by the assembler and linker before Xcode 1.5 (available for Mac OS X 10.3.9). Disable the compiler options 

  * Code > Unit style > Smart linkable (-CX)
  * and Linking > Link Style > Link smart (-XX)



## Лицензирование

### Могу ли я создавать коммерческие приложения, используя Lazarus?

Да, библиотека LCL разрабатывается под лицензией LGPL, что позволяет использовать её без открытия кода вашего приложения. Однако, модификации и расширения LCL должны распространяться с исходным кодом. Сам Lazarus, как IDE, использует лицензию GPL. Отметим, что LCL - это код, содержащийся в файлах из каталога "lcl", прочий код не подпадает под действие указанной лицензии. 

### Могу ли использовать в коммерческих приложениях дополнительные компоненты Lazarus?

В составе Lazarus есть дополнительные компоненты, разработанные участниками сообщества. Некоторые из этих компонентов распространяются под лицензиями, отличными от лицензии самого Lazarus. Если вы используете такие компоненты, вы должны уточнить их лицензию. Обычно необходимое пояснение приводится в исходном коде файлов соответствующего пакета. Большинство дополнительных компонентов от сторонних разработчиков можно найти в подкаталоге "components" основного каталога Lazarus. 

### Как я могу узнать, что компонент является частью LCL?

Все модули LCL размещаются в подкаталоге "lcl". Также доступен [список модулей](<http://lazarus-ccr.sourceforge.net/docs/lcl/>), входящих в LCL. Если в вашем коде вызываются модули, которых нет в этом списке, вероятно, вы используете компонент, не являющийся частью LCL. 

### Могу ли я создавать коммерческие плагины для Lazarus?

Да, the IDEIntf part of the IDE is licensed under the LGPL with the same exception, so that shared data structures in this part will not force you to license your plug-in or design-time package under the GPL. You are free to choose a plug-in of any license; we don't want to limit your choice. Therefore non-GPL compatible plug-ins are allowed. Note that it's not allowed to distribute a precompiled Lazarus with these non-GPL-compatible plugins included statically; however, we do not see this as a severe limitation, since recompiling Lazarus is easy.

---

_Source: [https://wiki.freepascal.org/Lazarus_Faq/ru](https://web.archive.org/web/20250516150800/https://wiki.freepascal.org/Lazarus_Faq/ru)_
