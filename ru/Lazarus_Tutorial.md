# Lazarus Tutorial

│ **[Deutsch (de)](</Lazarus_Tutorial/de> "Lazarus Tutorial/de")** │  **[English (en)](<../en/Lazarus_Tutorial.md> "Lazarus Tutorial")** │  **[español (es)](</Lazarus_Tutorial/es> "Lazarus Tutorial/es")** │  **[suomi (fi)](</Lazarus_Tutorial/fi> "Lazarus Tutorial/fi")** │  **[français (fr)](</Lazarus_Tutorial/fr> "Lazarus Tutorial/fr")** │  **[magyar (hu)](</Lazarus_Tutorial/hu> "Lazarus Tutorial/hu")** │  **[italiano (it)](</Lazarus_Tutorial/it> "Lazarus Tutorial/it")** │  **[日本語 (ja)](</Lazarus_Tutorial/ja> "Lazarus Tutorial/ja")** │  **[македонски (mk)](</Lazarus_Tutorial/mk> "Lazarus Tutorial/mk")** │  **[Nederlands (nl)](</Lazarus_Tutorial/nl> "Lazarus Tutorial/nl")** │  **[português (pt)](</Lazarus_Tutorial/pt> "Lazarus Tutorial/pt")** │  **русский (ru)** │  **[slovenčina (sk)](</Lazarus_Tutorial/sk> "Lazarus Tutorial/sk")** │  **[shqip (sq)](</Lazarus_Tutorial/sq> "Lazarus Tutorial/sq")** │  **[中文（中国大陆） (zh_CN)](</Lazarus_Tutorial/zh_CN> "Lazarus Tutorial/zh CN")** │  **[中文（臺灣） (zh_TW)](</Lazarus_Tutorial/zh_TW> "Lazarus Tutorial/zh TW")** │    
****

Lazarus - это бесплатный инструмент разработки с открытым исходным кодом для [Free Pascal compiler](<../en/FPC.md> "FPC"), который, в свою очередь, также является бесплатным и с открытым исходным кодом. [Интегрированная среда разработки](<../en/IDE.md> "IDE") Lazarus (IDE, см. [Скриншоты](<../en/Screenshots.md> "Screenshots"))) - это среда программирования для создания автономных графических и консольных приложений. 

В настоящее время Lazarus работает на [Linux](<../en/Linux.md> "Linux"), macOS, FreeBSD и Windows и предоставляет настраиваемый редактор исходного кода и среду создания визуальных форм вместе с менеджером пакетов, отладчиком и полной интеграцией графического интерфейса с Free Pascal [Compiler](<../en/Compiler.md> "Compiler") , 

## Contents

  * 1 Обзор
  * 2 Начнем - Ваша первая Lazarus-программа!
    * 2.1 Вторая попытка.
  * 3 Редактор
    * 3.1 Главное меню
    * 3.2 Кнопочная панель
  * 4 Палитра Компонентов
  * 5 Отладчик
    * 5.1 Настройки
    * 5.2 Кнопки
    * 5.3 Трассировка
  * 6 Файлы Lazarus
  * 7 Авторство и изменения



## Обзор

Lazarus это бесплатный инструмент разработки с открытым кодом, предназначенный для также бесплатного компилятора с открытым кодом Free Pascal (object pascal). IDE Lazarus ([screenshot](<http://lazarus-ccr.sourceforge.net/kbdata/lazide.jpg>)) это стабильная богатая возможностями среда разработки для создания самостоятельных графических и консольных приложений. В настоящее время она работает на Linux, FreeBSD и Win32 и предоставляет настраиваемый редактор кода и визуальную среду создания форм вместе с менеджером пакетов, отладчиком и GUI полноcтью интегрированным с компилятором FreePascal. 

## Начнем - Ваша первая Lazarus-программа!

(Благодарим [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")) 

Найдите, установите([Installing Lazarus](<../en/Installing_Lazarus.md> "Installing Lazarus")) и запустите Lazarus, который кроме того содержит и компилятор FreePascal. 

На экране появятся несколько окон: главное окно вверху, Инспектор Объектов (Object Inspector) слева, занимаюший большую часть экрана Редактор Кода Lazarus (Lazarus Source Editor), и готовое к использованию окно Form1 поверх окна Редактора. 

В главном окне сверху, под строкой меню располагается строка вкладок. Если вкладка 'Standard' еще не выбрана, выберите ее, щелкнув на ней левой кнопкой мыши. Затем найдите иконку Button (прямоугольник с текстом 'Ok' на нем) и щелкните на ней мышкой. Затем щелкните в окне Form1, где-нибудь слева от середины. Появится затененный прямоугольник с надписью 'Button1'. Вновь щелкните на иконке Button на вкладке Standard и щелкните на Form1 где-нибудь справа от центра: появится прямоугольник с надписью 'Button2'. 

Теперь щелкните на Button1 чтобы выбрать ее. Инспектор Объектов отобразит свойства объекта Button1. Не далеко от верхнего края располагается свойство с именем 'Caption', в котором отображается значение 'Button1'. Щелкните в этой строке и измените 'Button1' на 'Нажми меня' (_прим.пер.: в оригинальном тексте 'Press'. Далее будет приводиться только русский текст надписей, а оригинальный можно посмотреть на странице с английским текстом_). Если вы нажмете клавишу ENTER или щелкнете в другой строке, то увидите, что надпись на первой кнопке Form1 изменилась на 'Нажми меня'. Теперь щелкните в Инспекторе объектов на вкладке Events (События) и вы увидите различные события, на которые может реагировать кнопка. Среди них OnClick, OnEnter, OnExit и так далее. Щелкните в строке справа от OnClick: появится маленькая кнопка с троеточием (...). Если вы ее нажмете, то автоматически перенесетесь в Редактор Кода и курсор окажется в участке кода, начинающегося с: 
    
    
     procedure TForm1.Button1Click(Sender: TObject);
     begin
       {здесь наберите:}  Button1.caption := 'Нажми еще раз';
       {Редактор уже вставил завершение процедуры}
     end;
    

Нажмите F12 для переключения от Редактора Кода к окну формы Form1. 

Теперь отредактируйте свойства кнопки2 (Button2): щелкните на Button2 для отображения ее свойств в Инспекторе Объектов. Измените свойство Caption на 'Выход' вместо 'Button2'. Теперь перейдите на вкладку событий (Events) и щелкните в строке OnClick. Щелкните на кнопке с троеточием и перенеситесь в Редактор Кода, в тело другой процедуры: 
    
    
     procedure TForm1.Button2Click(Sender: TObject);
     begin
     {здесь наберите:}   Close;
     {Редактор уже вставил завершение процедуры} 
     end;
    

Теперь нажмите F12 чтобы увидеть форму Form1 вновь. Теперь вы можете попытаться скомпилировать. Простейшим способом сделать это является выбор в главном меню пункта 'Run' а в появившемся подменю пункта 'Run'. Вы также можете просто нажать клавишу F9. Сначала произойдет компиляция, а затем (если все в порядке) линковка и запуск вашей программы. 

Промелькнет несколько текстовых окон и будут выведены различные сообщения компилятора, а потом вновь появится окно формы Form1, но уже без точечной сетки; это и есть реальное главное окно вашего приложения, и оно ожидает нажатия кнопок или любого другого вашего действия. 

Попробуйте щелкнуть на кнопке 'Нажми меня'. Вы увидите, что надпись на ней сменится на 'Нажми еще раз'. Если вы нажмете еще раз, то на кнопке так и останется надпись 'Нажми еще раз'!! 

Теперь щелкните на кнопке с надписью 'Выход'. Окно закроется и программа завершится. Вновь появится окно формы Form1 с точечной сеткой, готовое для редактирования. 

Теперь (и вообще почаще) вы можете сохранить свою работу выбрав последовательно пункты меню Project > Save Project As > имя_вашего_файла.pas 

### Вторая попытка.

Вновь откройте сохраненный вами проект. 

На форме Form1 щелкните на кнопке 'Нажми меня' (Button1) чтобы выбрать ее. В Инспекторе Объектов перейдите на вкладку событий (Events), щелкните на строке справа от события OnClick, щелкните на кнопке с многоточием, чтобы перенестись к соответствующему участку кода в Редакторе. 

Измените код, на приведенный ниже: 
    
    
     procedure TForm1.Button1Click(Sender: TObject);
    {Используем свойство Tag, устанавливая его в положения 0 или 1}
     begin
       if Button1.tag =0 then
       begin
         Button1.caption := 'Нажми еще раз';
         Button1.tag := 1
       end else
       begin
         Button1.caption := 'Нажми меня';
         Button1.tag := 0
       end
     end;
    

Сохраните проект, перекомпилируйте и запустите. Левая кнопка теперь циклически меняет свой текст с одного сообщения на другое. 

Сделай дело - гуляй смело! 

Если вы хотите писать консольные или программы с текстовым интерфейсом на Pascal (например, если вы следуете стандартному курсу изучения программирования на Pascal, или вам нужно написать программу для работы в командном режиме или системную программу) вы также можете использовать Lazarus для написания, компилирования и выполнения ваших программ. Это прекрасная среда для программирования на Pascal. Смотри [Console Mode Pascal](<../en/Console_Mode_Pascal.md> "Console Mode Pascal"). 

## Редактор

Когда вы запускаете Lazarus в первый раз, на вашем рабочем столе появляется набор несвязанных 'плавающих' окошек. 

Первое, расположенное в самом верху рабочего стола имеет название **Lazarus Editor vXXXXXX - project1** (название зависит от используемой версии и названия открытого проекта). Это главное окно управления проектом и оно содержит Главное Меню и Палитру Компонентов (Component Palette). 

[![Lazmain.jpg](https://wiki.freepascal.org/images/2/2d/Lazmain.jpg)](</File:Lazmain.jpg>)

Строкой ниже располагается Главное Меню с обычными пунктами File, Edit, Search, View и некоторыми специфичными для Lazarus. Ниже Слева располагается набор кнопок, предоставляющих быстрый доступ к некоторым функциям главного меню, и справа - Палитра Компонентов. _(Примечание переводчика: если при установке вы выбираете русский язык, то весь интерфейс будет на русском, то есть пункты меню, названия окон и вкладок, а также подсказки. В связи с этим далее все названия указаны на русском, а в скобках (на английском) языке. В любой момент можно переключить язык интерфеса в пункте меню**Окружение**. )_

Под окном редактора Lazarus слева располагается окно **[Инспектора Объектов](<http://lazarus-ccr.sourceforge.net/kbdata/objinsp.jpg>)** , а справа **[Редактор Исходного Кода (Lazarus Source Editor)](<http://lazarus-ccr.sourceforge.net/kbdata/editor.jpg>)**. Может быть и другое окно меньшего размера, озаглавленное **[Form1](<http://lazarus-ccr.sourceforge.net/kbdata/blankform.jpg>)** , расположенное поверх Редактора Исходного Кода. Если его в данный момент не видно, то можно переключиться к нему, нажав клавишу **F12** , которая позволяет переключаться между Редактором Исходного Кода и Окном Формы. Окно формы это то место, где вы разрабатываете графический интерфейс вашей программы, а в Редакторе Исходного Кода отображается разрабатываемый вами Pascal-код вашего приложения. Использование Инспектора Объектов детально обсуждается ниже после описания Палитры Компонентов. 

Когда вы начинаете новый проект (или впервые запускаете Lazarus) по умолчанию создается стандартная форма, состоящая из прямоугольника с точечной сеткой для более легкого позиционирования различных компонентов на форме и системная строка сверху, которая обычно содержит кнопки **Свернуть** , **Развернуть** и **Закрыть**. Если вы щелкните мышкой в любом месте формы, вы увидите ее свойства в Инспекторе Объектов у левого края экрана. 

Другие окна, которые могут появиться в процессе работы: **[Инспектор проектов](<http://lazarus-ccr.sourceforge.net/kbdata/projinsp.jpg>)** , содержащий сведения о файлах, включенных в проект, и позволяющий вам добавлять и удалять файлы из проекта; Окно **Messages** (**Сообщения**), отображающее сообщения компилятора, ошибки и отчеты по вашему проекту; если Lazarus был запущен из терминала, то это окно остается запущенным и в нем отображаются подробные сообщения компилятора. 

### Главное меню

For a detailed description of every menu item which is accessible from the Lazarus IDE Main Menu, please refer to the [Main menu/ru](<Main_menu.md> "Main menu/ru") page. 

### Кнопочная панель

Маленькая панель в левой верхней части основного окна, слева от палитры компонентов, имеет набор кнопок, повторяющих наиболее часто применяемые выборы основного меню: 

    **Создать модуль** , **Открыть** (со стрелкой вниз для отображения списка недавно использованных файлов), **Сохранить** , **Сохранить все** , **Создать форму** , **Переключить Форма/Модуль** (т.е. показать либо форму, либо модуль исходного кода), **Показать модули** , **Показать формы** , **Запуск** (т.е. компиляция и выполнение), **Пауза** , **Шаг со входом** , **Шаг в обход** (последние два – функции отладчика).

## Палитра Компонентов

Это панель инструментов с вкладками, показывающая множество иконок, представляющих обычно применяемые компоненты при построении форм. 

Каждая вкладка представляет собой набор иконок, составляющих функциональную группу компонентов. Самая левая иконка на каждой вкладке в виде стрелочки называется Средством Выбора. 

Если навести курсор мыши на иконку палитры компонентов без нажатия, появится название данной компоненты. Заметьте, что каждое название начинается с 'T', что означает 'Тип', а точнее 'Класс' компоненты. При выборе компоненты для размещения на форме, Class добавится в секцию **type** раздела **interface** модуля (обычно в виде части на TForm1), и **instance** (образец) этого класса добавится в секцию **var** (обычно как переменная Form1). Все **Methods** (методы), разработанные для формы или ее компонент (процедуры или функции) будут помещены в раздел **implementation** модуля. 

В приведенном ниже списке компонентов можно найти связи с файлами, которые содержат описания модулей, в которых они найдены. Если нужно узнать о свойствах конкретного компонента, зачастую следует обратить внимание на Наследование этого компонента и просмотреть свойства базового типа, из которого он произведен. Например, для изучения TMaskEdit очень полезно просмотреть TCustomMaskEdit. 

**ВКЛАДКИ** (их имена понятны и не требуют разъяснений): 

  * **[Standard](<http://lazarus-ccr.sourceforge.net/index.php?wiki=StdCtrls>)**

[![Component Palette Standart.png](https://wiki.freepascal.org/images/4/4d/Component_Palette_Standart.png)](</File:Component_Palette_Standart.png>)

    _Наиболее часто применяемые компоненты:_ [TMainMenu](<http://lazarus-ccr.sourceforge.net/index.php?wiki=MenuUnit>), [TPopupMenu](<http://lazarus-ccr.sourceforge.net/index.php?wiki=MenuUnit>), [TButton](<http://lazarus-ccr.sourceforge.net/index.php?wiki=ButtonsTxt>), TLabel, TEdit, TMemo, TToggleBox, TCheckBox, TRadioButton, TListBox, TComboBox, TScrollBar, TGroupBox, TStaticText, [TRadioGroup](<http://lazarus-ccr.sourceforge.net/index.php?wiki=ExtCtrls>), TCheckGroup, [TPanel](<http://lazarus-ccr.sourceforge.net/index.php?wiki=ExtCtrls>), TActionList

  * **[Additional](<http://lazarus-ccr.sourceforge.net/index.php?wiki=ExtCtrls>)**

[![Component Palette Additional.png](https://wiki.freepascal.org/images/7/7d/Component_Palette_Additional.png)](</File:Component_Palette_Additional.png>)

    _Дополнительные, часто используемые:_ [TBitBtn](<http://lazarus-ccr.sourceforge.net/index.php?wiki=ButtonsTxt>), [TSpeedButton](<http://lazarus-ccr.sourceforge.net/index.php?wiki=ButtonsTxt>), [TImage](<http://lazarus-ccr.sourceforge.net/index.php?wiki=ExtCtrls>), TShape, TBevel, TPaintBox, TNotebook, TLabeledEdit, TSplitter, [TMaskEdit](<http://lazarus-ccr.sourceforge.net/index.php?wiki=MaskEdit>), TCheckListBox, [TScrollBox](<http://lazarus-ccr.sourceforge.net/index.php?wiki=FormsTxt>), [TApplicationProperties](<http://lazarus-ccr.sourceforge.net/index.php?wiki=FormsTxt>), TStringGrid, TDrawGrid, TPairSplitter

  * **[Common Controls](<http://lazarus-ccr.sourceforge.net/index.php?wiki=ComCtrls>)**

[![Component Palette Common Controls.png](https://wiki.freepascal.org/images/4/4a/Component_Palette_Common_Controls.png)](</File:Component_Palette_Common_Controls.png>)

    TTrackBar, TProgressBar, TTreeView, TListView, TStatusBar, TToolBar, TUpDown, TPageControl, TImageList

  * **[Dialogs](<http://lazarus-ccr.sourceforge.net/index.php?wiki=DialogsTxt>)**

[![Component Palette Dialogs.png](https://wiki.freepascal.org/images/7/72/Component_Palette_Dialogs.png)](</File:Component_Palette_Dialogs.png>)

    TOpenDialog, TSaveDialog, TSelectDirectoryDialog, TColorDialog, TFontDialog, TOpenPictureDialog, TSavePictureDialog, TCalendarDialog, TCalculatorDialog

Некоторые полезные процедуры или функции группы **[Dialog](<http://lazarus-ccr.sourceforge.net/index.php?wiki=DialogExamples>)** не размещены на Палитре, но легко применяются путем прямого вызова из исходной программы. 

Несколько хороших примеров использования компонентов находятся в папке $LazarusPath/lazarus/examples исходной установки. Многие программы показывают способы применения диалогов и других компонентов без использования IDE и палитры компонентов, при наличии файла определения отдельной формы: все компоненты явно и полно определены в основной программе. Другие программы примеров полностью используют IDE. 

Некоторые примеры не будут сначала работоспособны: понадобится настройка путей и разрешений для файлов и папок. При желании скомпилировать пример убедитесь, что для файлов и папок установлены разрешения read/write/execute, или скопируйте файлы в папку с подходящими разрешениями. 

Попробуйте выполнить программу 'testall' для просмотра меню доступных компонентов совместно с маленькими примерами тестовых форм для большинства из них; затем просмотрите исходный код для понимания принципов их работы! 

  * **Misc**

[![Component Palette Misc.png](https://wiki.freepascal.org/images/8/8e/Component_Palette_Misc.png)](</File:Component_Palette_Misc.png>)

    [TColorButton](<http://lazarus-ccr.sourceforge.net/index.php?wiki=DialogsTxt>), TSpinEdit, TArrow, TCalendar, TEditButton, TFileNameEdit, TDirectoryEdit, TDateEdit, TCalcEdit, TFileListBox

  * **Data Controls**

[![Component Palette DataControls.png](https://wiki.freepascal.org/images/6/62/Component_Palette_DataControls.png)](</File:Component_Palette_DataControls.png>)

    _Компоненты работы с данными, которые в основном копируют стандартную и дополнительную группы, но применительно к базам данных:_ TDBNavigation, TDBText, TDBEdit, TDBMemo, TDBImage, TDBListBox,TDBComboBox, TDBCheckBox, TDBRadioGroup, TDBCalendar, TDBGroupBox, TdbGrid

  * **Data Access**

[![Component Palette DataAccess.png](https://wiki.freepascal.org/images/c/cc/Component_Palette_DataAccess.png)](</File:Component_Palette_DataAccess.png>)

    TDatasource

  * **System**

[![Component Palette System.png](https://wiki.freepascal.org/images/f/fe/Component_Palette_System.png)](</File:Component_Palette_System.png>)

    [TTimer](<http://lazarus-ccr.sourceforge.net/index.php?wiki=ExtCtrls>), TIdleTimer, TProcess

  * **SynEdit**

[![Component Palette SynEdit.png](https://wiki.freepascal.org/images/2/22/Component_Palette_SynEdit.png)](</File:Component_Palette_SynEdit.png>)

    _Группа компонентов для сопряжения с другими языками и сервисными программами. SynEdit – продвинутый редактор для Borland Delphi, Kylix and C++Builder. Он поддерживает подцветку синтаксиса и завершение кода, экспорт в html, tex и rtf. Полное управление VCL/CLX, считая, что это не оболочка для управления Microsoft Windows, не нужна библиотека времени выполнения; это делает SynEdit кроссплатформенным компонентом. Также планируется совместимость с FreePascal, и SynEdit является компонентом редактирования в Lazarus IDE._ См. [synedit at sourceforge](<http://synedit.sourceforge.net>). TSynEdit, TSynAutoComplete, TSynExporterHTML, TSynMacroRecorder, TSynMemo, TSynPasSyn, TSynCppSyn, TSynJavaSyn, TSynPerlSyn, TSynHTMLSyn, TSynXMLSyn, TSynLFMSyn, TSynUNIXShellScriptSyn, TSynCssSyn, TSynPHPSyn, TSynTeXSyn, TSynSQLSyn, TSynMultiSyn

Для работы с палитрой нужно открыть форму (если форма не открыта, выбрать Файл -> Создать Форму). Щелкните на иконке нужного компонента на нужной вкладке Палитры, затем щелкните на форме, и этот компонент появится на ней. После этого можно выбрать новый компонент щелчком мыши, переместить его в желаемое место и подогнать размеры. Эти установки можно сделать также с помощью Инспектора Объектов, регулируя значения соответствующих свойств данного компонента. 

При установке дополнительных компонентов, разработанных самостоятельно, или полученных со стороны, появятся дополнительные вкладки с соответствующими иконками на Палитре компонентов. Эти новые компоненты могут быть выбраны и использованы на формах, как и те, что поставлены по умолчанию. 

## Отладчик

Писал и редактировал: Romlo 

### Настройки

Чтобы пользоваться отладчиком, должны быть установлены галочки: 

проект > параметры компилятора > вкладка "Связывание" >

  * галочка "Выдавать номера строк в ошибках времени исполнения"


  * галочка "Генерировать отладочную информацию для GDB (замедляет сборку)"



### Кнопки

Управление отладкой производится вот этими пятью кнопками: 

[![Buttons debugger.PNG](https://wiki.freepascal.org/images/b/b9/Buttons_debugger.PNG)](</File:Buttons_debugger.PNG>)

  * Зеленый треугольник - "Запуск" - скомпилировать и запустить приложение под отладчиком.
  * Вертикальные палочки - "Пауза" - здесь серые, а вообще синие - это пауза, можно приостановить приложение, например, при трассировке.
  * Квадратик - "Останов" - здесь серый, а вообще красный - закрыть приложение. Это если оно повисло (напр. в бесконечном цикле).



Остальные две кнопки используются при трассировке программы, про которую мы поговорим подробнее. 

### Трассировка

Трассировать программу - значит выполнять её шаг за шагом, построчно. Для трассировки какого-либо участка программы, нужно установить в нужном месте "точку останова". Для этого надо найти начало участка программы в редакторе кода, а затем щёлкнуть по его номеру слева. При этом сама строчка выделится красным, а рядом с номером появится красный кружок. 

[![str debugger.PNG](https://wiki.freepascal.org/images/8/8c/str_debugger.PNG)](</File:str_debugger.PNG>)

После нажатия на "запуск", знак вопроса в кружке должен смениться галочкой. Если вместо галочки там крестик, значит, точка останова не была установлена по каким-то причинам. В этом случае лучше ещё раз проверить настройки и перезапустить Lazarus. Если же всё получилось, продолжим. Далее, нужно вызвать то событие, при котором начнёт выполняться данный участок кода (то есть если вы поставили точку останова в начале button1_click, например, то теперь вам надо нажать эту кнопку). При этом должен показаться редактор кода. Ранее красная строчка теперь станет серой, а галочка в кружке сменится стрелочкой: 

[![str2 debugger.PNG](https://wiki.freepascal.org/images/7/7c/str2_debugger.PNG)](</File:str2_debugger.PNG>)

Вот теперь и вспомним про те две кнопки, что мы не рассмотрели. первая называется "Шаг со входом", вторая - "Шаг в обход". Смысл первой заключается в том, что если сейчас вы "стоите" на строчке с какой-то процедурой или функцией, то нажав эту кнопку, вы перейдете на эту процедуру и будете выполнять пошагово уже её. Когда она закончится, вы вернётесь обратно, но уже на следующую строку. Как несложно догадаться, нажав на вторую кнопку, вы не зайдёте в процедуру, она просто выполнится, а вы перейдёте на следующую строку. 

Отдельно надо сказать, что "Шаг со входом" не работает, если функция, на которой вы "стоите", экспортирована из библиотеки (например, в WinAPI-функцию вы зайти не сможете). 

Также имеется возможность просматривать содержимое переменных, для этого надо просто навести на неё мышку прямо в редакторе кода, во всплывающей подсказке будет указан адрес переменной и её содержимое: 

[![var debugger.png](https://wiki.freepascal.org/images/4/41/var_debugger.png)](</File:var_debugger.png>)

Ну и в дополнение к сказанному добавлю, что чтобы продолжить выполнение программы в обычном режиме (если вам надоело её трассировать), достаточно нажать кнопку "запуск". Ну а чтобы убрать точку останова, надо закрыть приложение и ещё раз щёлкнуть по номеру строки, на которой эта точка стоит. ну и после этого заново откомпилировать приложение. 

## Файлы Lazarus
    
    
       (Благодарим Kevin Whitefoot.)
       (ДОбавления от Giuseppe Ridinò, [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc") и Tom Lisjac)
    

Когда вы выполняете сохранение на самом деле вы сохраняете два файла: 
    
    
      xxx.pas и yyy.lpr 
    

(Вы сохраняете и больше, но это те файлы, которым вы даете имена). Файл проекта (lpr) и файл модуля (pas) должны иметь разные имена, потому что Lazarus присваивает имя модулю (в исходном коде) также, как и имя файла модуля, а программе по имени файла проекта (это необходимо сделать иначе компилятор может впоследствии не найти модуль по ссылке на него в файле проекта). Во избежание противоречий (ошибок) следует изменить все упоминания Unit1 на xxx. 

Итак, если вы сохранили проект под именем **again** , то попытка сохранить again.pas и again.lpr приведет к ошибке, потому что имена модуля и программы одинаковы, что приводит к ошибке двойного именования. 

Ниже приведен пример именования: 

e:/lazarus/kj/lazhello: 
    
    
    total 4740  free 76500
    -rwxrwxrwx   1 kjwh     root  4618697 Mar 24 11:19 again.exe
    -rw-rw-rw-   1 kjwh     root     3002 Mar 24 11:21 again.lpi
    -rw-rw-rw-   1 kjwh     root      190 Mar 24 11:18 again.lpr
    -rw-rw-rw-   1 kjwh     root      506 Mar 24 11:08 againu.lfm
    -rw-rw-rw-   1 kjwh     root      679 Mar 24 11:08 againu.lrs
    -rw-rw-rw-   1 kjwh     root      677 Mar 24 11:08 againu.pas
    -rw-rw-rw-   1 kjwh     root     2124 Mar 24 11:08 againu.ppu
    -rwxrwxrwx   1 kjwh     root      335 Mar 24 11:07 ppas.bat
    

Заметьте, что появилось более двух файлов, как следовало бы ожидать. 

Ниже приведена краткая справка по каждому файлу: 

**again.exe:** Основной исполняемый файл программы. Win32 добавляет расширение "exe". Linux этого не делает. В Linux это файл будет иметь большой размер вследствие того, что включает отладочную информацию. Запустите утилиту "strip" чтобы удалить ее и значительно снизить размер исполняемого файла. 

**again.lpi:** Это основной файл проекта Lazarus (Lazarus Project Information); эквивалент основного файла приложения в Delphi с расширением .dpr. Он сохраняется в XML формате. 

**again.lpr:** Исходный код основной программы. Не смотря на специфичноое для Lazarus расширение на самом деле это обычный Pascal-код. Он содержит строку Uses, помогающую компилятору найти все необходимые модули Отметим, что программа называется не аналогично имени данного файла. 

**againu.lfm:** Это файл, в котором Lazarus хранит описание формы. Lazarus использует его для создания файла ресурсов, который включает секцию инициализации модуля againu.pas. Файл Delphi с расширением .dfm может быть преобразован в lfm-формат в IDE Lazarus из главного меню: Tools->Convert DFM file. 

**again.lrs:** Это автоматически генерируемый файл ресурсов. Заметьте, что это не файл ресурсов Windows. 

**againu.pas:** Модуль, содержащий код формы. 

**again.ppu:** Это скомпилированный модуль. 

**ppas.bat:** Это простой скрипт, связывающий программу для создания выполняемого файла. Если компиляция успешна, он удаляется компилятором. 

## Авторство и изменения

Эта страница была импортирована из epikwiki [версии](<http://lazarus-ccr.sourceforge.net/index.php?wiki=LazarusTutorial>). 

  * Created initial page and template. T. Lisjac - 11/04/2003 [ VlxAdmin](</User:Tom> "User:Tom")
  * Inserted a note containing instructions for writing your first Lazarus Program. Suggest an administrator places it in the appropriate place on the Tutorial menu. 3/09/2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Per above, moved Chris's writeup to the main body of the tutorial [ VlxAdmin](</User:Tom> "User:Tom")
  * Began to insert text describing the Lazarus Editor - more to follow! 24 Mar 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added some more to Lazarus Editor section of Tutorial. 25 Mar 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added screenshots and revised some of the page formatting [ VlxAdmin](</User:Tom> "User:Tom") 3/25/2004
  * Moved some of kwhitefoot's comments into Tutorial section. Formatting not quite right, but have to go to bed now! 26 Mar 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Formatted, added credits and comments. Removed original notes. [ VlxAdmin](</User:Tom> "User:Tom") 3/26/2004
  * More material added to Editor section of tutorial. 26 Mar 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * More material added describing the Main Menu. Renamed 'Hello World' to 'Getting Started' and moved it to nearer the top. 31 March 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Inserted section on Run sub-menu. Some general editing (eg ended each entry with a period to ensure consistency). 9 Apr 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Inserted a new section on How to get started with MySQL in FPC/Lazarus. 13 Apr 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Deleted the section on MySQL from here: it has been copied to Lazarus Database section of tutorial. 14 Apr 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added some more to the description of the Editor Main Menu. 18 Apr 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added section on Environment sub-menu. 19 Apr 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added section on Components sub-menu. 4 May 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Adding Tools sub-menu description (incomplete). 7 May 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added some screenshots to Menu descriptions. 9 May 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Fixed a bit in Environment Options - thanks VincentSnijders. 14 May 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * More additions to Tools sub-menu. 19 May 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added a section on the Button Bar and started work on The Component Palette. 20 May 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Posted a description file for the StdCtrls unit of the LCL, in the hope that people will add comments. 26 May 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Edited the StdCtrls file, removing a lot of repetitive material and doing some formatting. It is still far too long. 28 May 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Expanding on the Components Palette. 5 June 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added a lot to the DialogExamples page. 10 June 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Considerable revision of the StdCtrls page, hopefully making it clearer and encouraging people to contribute - particularly in the 'Description' sections. 14 June 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added pages for Menus and Dialogs units (linked to Component Palette description) - please feel free to add to these pages. 14 June 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added page for Common Controls (linked to Component Palette). 16 June 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added MaskEdit page (linked to Component Palette). 17 June 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added Buttons, ExtCtrls pages (linked to Component Palette). 17 June 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Edited MainMenu component description page. 23 June 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Some additions to Common Controls. 28 June 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * A new tutorial on Text-mode Pascal programming has been added. 5 July 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Minor changes to ComCtrls, ExtCtrls, Environment Menu. 10 July [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Added FormsTxt, component description page for Component Palette. 20 July 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Some corrections to ConsoleModePascal. 21 July 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")
  * Some small changes to ComponentPalette. 22 July 2004 [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc")

---

_Source: [https://wiki.freepascal.org/Lazarus_Tutorial/ru](https://web.archive.org/web/20240613201325/https://wiki.freepascal.org/Lazarus_Tutorial/ru)_
