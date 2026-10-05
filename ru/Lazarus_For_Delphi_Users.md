# Lazarus For Delphi Users

│ **[English (en)](<../en/Lazarus_For_Delphi_Users.md>)** │  **русский (ru)** │

Эта статья предназначена для тех, кто интересуется Lazarus и уже знает Delphi. Здесь описаны различия между ними. 

## Contents

  * 1 Delphi -> Lazarus
    * 1.1 Самые большие различия между Lazarus и Delphi
    * 1.2 С чего начать перенос проекта Delphi в Lazarus
    * 1.3 Поддержка Юникода
  * 2 Delphi IDE -> Lazarus IDE
    * 2.1 Проекты
    * 2.2 Среда Lazarus не бывает "пустой"
    * 2.3 Редактор исходного кода
      * 2.3.1 Пример для автозавершения локальной переменной
      * 2.3.2 Пример автозавершения назначения события
      * 2.3.3 Пример автозавершения вызова процедуры
      * 2.3.4 "Автозавершение слова" Ctrl+W
      * 2.3.5 Поддержка Include-файлов
    * 2.4 Designer
      * 2.4.1 Инспектор объектов
      * 2.4.2 Пакеты
  * 3 VCL -> LCL
    * 3.1 TControl.Font/TControl.ParentFont
    * 3.2 Control Dragging/Docking
    * 3.3 TEdit/TCustomEdit
    * 3.4 TDBImage
    * 3.5 (необязательно) TSplitter -> TPairSplitter
    * 3.6 TCustomTreeView/TTreeView
    * 3.7 Сообщения / События
  * 4 См. также



## Delphi -> Lazarus

Lazarus - среда быстрой разработки приложений (RAD), как и Delphi. Это значит, что она состоит из библиотеки визуальных компонентов и среды разработки (IDE). Библиотека компонентов Lazarus (LCL) похожа на библиотеку визуальных компонентов Delphi (VCL). Большинство модулей, классов и свойств Lazarus обладают такими же именами и функциями, как их аналоги в Delphi. Это делает относительно лёгким перенос приложений Delphi в Lazarus. Однако Lazarus **не является** 'клоном Delphi с открытым кодом', поэтому не следует ожидать 100%-ной совместимости. 

### Самые большие различия между Lazarus и Delphi

  * код Lazarus полностью открыт
  * Lazarus написан кроссплатформенным способом
  * Lazarus использует компилятор [Free Pascal](<../en/Free_Pascal.md> "Free Pascal") (FPC)



FPC работает на 15 платформах. Однако не все пакеты и библиотеки FPC портированы на все платформы, поэтому Lazarus работает на: 

  * Linux (i386, x86_64)
  * FreeBSD (i386)
  * macOS (powerpc, i386, x86_64)
  * Windows (i386, x86_64)



**Работа над Lazarus, как и над этим текстом, не закончена. Мы всё время ищем новых разработчиков и технических писателей...**

### С чего начать перенос проекта Delphi в Lazarus

В меню **Сервис** выберите команду **Преобразовать проект Delphi в проект Lazarus**. Не стоит ждать, что так будет преобразовано абсолютно всё, тем не менее, это хорошее начало. Учтите, что средства Lazarus выполняют, в основном, преобразования в одном направлении. Если вам надо сохранить совместимость с Delphi (компилировать проект и в Delphi, и в Lazarus) рассмотрите использование [XDev Toolkit](<../en/XDev_Toolkit.md> "XDev Toolkit"). 

### Поддержка Юникода

Delphi до версии 2007 не поддерживала Юникод, а использовала кодировку Windows ANSI. Юникод в кодировке UTF-16 поддерживается в Delphi с версии 2009. 

В Lazarus поддержка Юникод реализована раньше в кодировке UTF-8 (см. [Поддержка Юникод в LCL](<LCL_Unicode_Support.md> "LCL Unicode Support/ru")). 

## Delphi IDE -> Lazarus IDE

### Проекты

Главным в приложении Delphi является файл .dpr. Файл .dpr также содержит главный код программы Delphi и является местом, где среда разработки Delphi хранит сведения о соответствующих ключах компилятора и нахождении модулей. В приложении Lazarus есть файл .lpr, который также является основным файлом кода проекта на языке Pascal. Тем не менее, главным в проекте Lazarus является файл .lpi (Lazarus Project Information), который создаётся вместе с файлом .lpr. Все данные проекта (ключи компилятора, пути к модулям и т.п.) хранятся в файле .lpi. Поэтому важнейшим является файл .lpi. На большинстве платформ двойной щелчок по файлу .lpi открывает проект в среде разработки Lazarus. 

Delphi хранит пути к модулям проекта в файле .dpr. Пример: 
    
    
    unit1 in 'path/Unit1.pas';
    

Такие пути со словом 'in' специфичны для Delphi, и не работают в Lazarus, поэтому не используйте их. Вместо этого в параметрах проекта (**Проект- >Параметры проекта...**) на странице **Параметры компилятора- >Пути** задайте пути к модулям, находящимся за пределами каталога проекта. Большинство путей к модулям задаются автоматически после добавления зависимости от какого-либо пакета. Например, во все стандартные проекты Lazarus LCL по умолчанию добавляется зависимость от пакета LCL, поэтому в каждом новом проекте Lazarus LCL (**Проект- >Создать проект...->Приложение**) пути ко всем модулям LCL известны без вашего участия. 

Delphi хранит параметры компилятора в файле .dpr (например, {$APPTYPE CONSOLE}). Lazarus игнорирует такую запись. Вместо этого используйте страницу **Параметры компилятора** в диалоге **Параметры проекта**. 

### Среда Lazarus не бывает "пустой"

В среде Lazarus всегда открыт проект. Единственный способ "закрыть" проект - выход из Lazarus или открытие другого проекта. Это связано с тем, что проект Lazarus - ещё и "сеанс работы". Сведения о сеансе (например, параметры редактора) также хранятся в файле .lpi, и при повторном открытии проекта состояние редактора восстанавливается. 

**Пример.** Вы отлаживаете приложение и расставили несколько точек останова и закладок. Вы можете в любой момент сохранить проект, закрыть Lazarus или открыть другой проект. При повторном открытии проекта (даже на другом компьютере) все точки останова, закладки, открытые файлы, положение курсора, история переходов будут восстановлены. 

### Редактор исходного кода

В меню Сервис -> Параметры -> Редактор -> Комбинации клавиш могут быть заданы практически любые комбинации клавиш. 

Среда разработки Lazarus располагает множеством инструментов для работы с кодом. Многие из них выглядят и работают так же, как и в Delphi. Но есть одно важное отличие: Lazarus не использует компилятор для получения сведений о коде, а работает с ним напрямую. Это даёт ряд важных преимуществ: 

Редактор работает с "комментариями". В Delphi комментарии это просто место между фрагментами кода. Здесь не действуют никакие средства работы с кодом, и комментарии смещаются при автоматической вставке нового кода. В Lazarus можно найти объявление даже в закоментированном коде. Это не очень надёжно, однако работает в большинстве случаев. При вставке нового кода среда разработки использует некоторую эвристику, чтобы сохранить комментарий и код вместе. К примеру, строка `"c: char; // comment"` разделяться не будет. 

Дельфийское «Автозавершение кода» (`Ctrl` \+ `Space`) в Lazarus называется «Автозавершение идентификатора». Термин Lazarus'овское «Автозавершение кода» - это функция, объединяющая «Автоматическое завершение класса» (как в Delphi), «Автозавершение локальной переменной» и «Автозавершение назначения события». Все они вызываются по `Ctrl` \+ `Space` \+ `C`, что подразумевает, что среда IDE определяет положение курсора. 

#### Пример для автозавершения локальной переменной

Предположим, вы только что создали новый метод и написали оператор "i:=3;" 
    
    
    procedure TForm1.DoSomething;
    begin
      i := 3;
    end;
    

Наведите курсор на идентификатор «i» и нажмите `Ctrl`+`⇧ Shift`+`C`, чтобы получить: 
    
    
    procedure TForm1.DoSomething;
    var i: Integer;
    begin
      i := 3;
    end;
    

#### Пример автозавершения назначения события

Приятной особенностью инспектора объектов является автоматическое создание методов. Заодно, вы можете получить редактор исходного кода для создания событий.  
Например: 
    
    
    Button1.OnClick:=
    

Поместите курсор за оператором присваивания ":=" и нажмите `Ctrl`+`⇧ Shift`+`C`. 

* * *

Прим.перев.: при этом сформированный код будет выглядеть так: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Button1.OnClick:=@Button1Click;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
    
    end;
    

#### Пример автозавершения вызова процедуры

Предположим, вы только что написали оператор "DoSomething(Width);" 
    
    
    procedure SomeProcedure;
    var
      Width: integer;
    begin
      Width:=3;
      DoSomething(Width);
    end;
    

Наведите курсор на идентификатор «DoSomething» и нажмите `Ctrl`+`⇧ Shift`+`C`, чтобы получить: 
    
    
    procedure DoSomething(aWidth: LongInt);
    begin
    
    end;
    
    procedure SomeProcedure;
    var
      Width: integer;
    begin
      Width:=3;
      DoSomething(Width);
    end;
    

#### "Автозавершение слова" Ctrl+W

Он работает аналогично «Автозавершению идентификатора», но работает не для идентификаторов паскалей, а для всех слов. Позволяет выбрать все слова во всех открытых файлах, начиная с одинаковых букв. 

#### Поддержка Include-файлов

Delphi не поддерживал их, поэтому вы, вероятно, еще не создавали много включаемых файлов. Но у включаемых файлов есть большое преимущество: они позволяют писать (не)зависимый от платформы код, не запутывая ваш код IFDEF'ами. Например: переход по методу, автозавершение класса, объявление find, ... все работает с include-файлами. 

Есть много вариантов функций кода. 

### Designer

\- см. руководство пользователя 

#### Инспектор объектов

В IDE Delphi и IDE Lazarus'а Инспектор объектов используется для редактирования свойств компонента, назначения событий и т.д. Ниже приведены некоторые незначительные отличия, которые следует отметить при использовании: 

  1. Начиная с Delphi 5, существует Дерево Объектов(Object Treeview), которое можно использовать для навигации и выбора объектов в соответствии с иерархией в дополнение к традиционному выпадающему списку в Инспекторе объектов. В Lazarus это является частью Инспектора объектов и используется вместо раскрывающегося списка по умолчанию, вы можете выбрать его из контекстного меню "Show Component Tree"(Показать дерево компонентов), чтобы использовать/не использовать.
  2. В Delphi двойной щелчок на пустом событии автоматически создаст его и откроет редактор исходного кода в этой позиции, в Lazarus справа от выбранного выпадающего меню есть кнопка, которая выполняет это действие.
  3. В Delphi вы должны вручную удалить имя события в редакторе, чтобы удалить привязку к нему, в Lazarus вы можете нажать на раскрывающийся список и выбрать "(None)"(Нет).
  4. Двойной щелчок по обычным свойствам, таким как логическое значение, не изменит его значение (как это произошло бы случае с Events(Событием)), вы должны выбрать его из выпадающего списка. А чтобы открыть их с прилагаемой формой редактора, вы должны нажать кнопку «...» справа от раскрывающегося меню.



#### Пакеты

  * Может ли Lazarus установить и использовать пакеты Delphi?



    Нет, потому что они требуют магии компилятора Delphi.

  * Должны ли они быть специально сделанными для Lazarus?



    Да.
    Создайте новый пакет, сохраните его в каталоге с исходниками пакета (обычно это тот же каталог файла .dpk), добавьте LCL как требуемый пакет и, наконец, добавьте файлы .pas. Теперь вы можете установить его или использовать в своих проектах. Есть некоторые [ различия между пакетами Lazarus и Delphi](<../en/packages.md> "packages").

## VCL -> LCL

Хотя VCL и LCL служат большей части одной и той же цели - объектно-ориентированной иерархии компонентов, особенно ориентированной на быструю разработку приложений, они не идентичны. Например, в то время как VCL предоставляет много невизуальных компонентов, LCL пытается предоставлять только визуальные, в то время как большинство невизуальных компонентов (таких как доступ к базе данных) предоставляются с FCL, включенным в [Free Pascal](<../en/Free_Pascal.md> "Free Pascal"). 

Кроме того, многие элементы управления, которые представлены в VCL, могут отсутствовать в LCL, или наоборот, или даже если элементы управления существуют в обоих, они не являются клонами, и при переносе необходимо внести изменения в приложения, компоненты и элементы управления. 

Ниже приведена попытка для пользователя Delphi дать достаточно полное описание основных различий или несовместимостей между ними. Она охватывает различия в первую очередь особенно для VCL D4, хотя иногда также D5, D6 или D7; и с текущим LCL, как в CVS. Таким образом, оно может не всегда соответствовать версии Delphi, к которой вы привыкли, или полностью соответствовать текущему LCL, который у вас есть. Если вы видите неточности между нижеследующим и LCL, как в CVS, или ваш Delphi не стесняется добавлять и изменять, чтобы сделать его максимально полным для всех людей. 

### TControl.Font/TControl.ParentFont

В VCL довольно распространено и нормально использовать определенное имя шрифта и свойства шрифта, такие как жирный шрифт и курсив для элементов управления, и ожидать, что это значение всегда будет соблюдаться. Далее предоставляется свойство TControl.ParentFont, которое гарантирует, что элемент управления всегда будет следовать за шрифтом его родителя. Опять же подразумеваемое предположение, что эти значения будут всегда соблюдаться, даже независимо от настроек внешнего вида Windows. 

Это не всегда верно в LCL, да и не может быть. LCL, являющийся кросс-платформой/кросс-интерфейсом по своей природе, предпочитает сбалансированный подход и вместо этого всегда будет пытаться использовать собственные настройки Рабочего стола/Внешнего вида инструментария или Темы для любых виджетов. Например, если используется интерфейс GTK, а тема gtk предоставляет определенный шрифт для кнопок, то кнопки LCL всегда будут пытаться использовать этот шрифт. 

Это означает, что большинство элементов управления LCL не имеют того же уровня управления проектированием, который часто ожидается в VCL, скорее, только те пользовательские элементы управления, которые нарисованы Canvas вместо назначенного интерфейса, могут постоянно изменяться таким образом независимо от используемого интерфейса. 

### Control Dragging/Docking

В VCL большинство (Win)Controls реализуют методы и функции обратного вызова для обработки перетаскивания и закрепления элементов управления, например, перетаскивая элемент управления с одной панели и закрепляя его на другой панели во время выполнения. 

Эта функциональность в настоящее время не реализована/не завершена в LCL, хотя в настоящее время она находится на начальных этапах планирования и должна в конечном итоге поддерживать некоторый уровень совместимости для этого типа поведения, если не точно таким же образом. 

В настоящее время это означает, что ни один элемент управления не будет наследовать/использовать следующие функции, процедуры, свойства или события TControl: 
    
    
    Protected
      function GetDockEdge(MousePos: TPoint): TAlign;
      function GetDragImages: TDragImageList;
      function GetFloating: Boolean;
      function GetFloatingDockSiteClass: TWinControlClass;
      procedure DoEndDrag(Target:TObject); X, Y: Integer);
      procedure DockTrackNoTarget(Source: TDragDockObject; X, Y: Integer);
      procedure DoEndDock(Target: TObject; X, Y: Integer);
      procedure DoDock(NewDockSite: TWinControl; var ARect: TRect);
      procedure DoStartDock(var DragObject: TDragObject);
      procedure DragCanceled;
      procedure DragOver(Source: TObject; X, Y: Integer; State: TDragState;
                        var Accept: Boolean);
      procedure DoEndDrag(Target: TObject; X, Y: Integer);
      procedure DoStartDrag(var DragObject: TDragObject);
      procedure DrawDragDockImage(DragDockObject: TDragDockObject);
      procedure EraseDragDockImage(DragDockObject: TDragDockObject);
      procedure PositionDockRect(DragDockObject: TDragDockObject);
      procedure SetDragMode(Value: TDragMode);
      property DragKind: TDragKind;
      property DragCursor: TCursor;
      property DragMode: TDragMode;
      property OnDragDrop: TDragDropEvent;
      property OnDragOver: TDragOverEvent;
      property OnEndDock: TEndDragEvent;
      property OnEndDrag: TEndDragEvent;
      property OnStartDock: TStartDockEvent;
      property OnStartDrag: TStartDragEvent;
    public
      function Dragging: Boolean;
      function ManualDock(NewDockSite: TWinControl; DropControl: TControl;
                         ControlSide: TAlign): Boolean;
      function ManualFloat(ScreenPos: TRect): Boolean;
      function ReplaceDockedControl(Control: TControl; NewDockSite: TWinControl;
                          DropControl: TControl; ControlSide: TAlign): Boolean;
      procedure BeginDrag(Immediate: Boolean; Threshold: Integer);
      procedure Dock(NewDockSite: TWinControl; ARect: TRect);
      procedure DragDrop(Source: TObject; X, Y: Integer);
      procedure EndDrag(Drop: Boolean);
      property DockOrientation: TDockOrientation;
      property Floating: Boolean;
      property FloatingDockSiteClass: TWinControlClass;
      property HostDockSite: TWinControl;
      property LRDockWidth: Integer;
      property TBDockHeight: Integer;
      property UndockHeight: Integer;
      property UndockWidth: Integer;
    

Вот эти классы не существуют/непригодны для использования - 
    
    
    TDragImageList = class(TCustomImageList)
    TDockZone = class
    TDockTree = class(TInterfacedObject, IDockManager)
    TDragObject = class(TObject)
    TBaseDragControlObject = class(TDragObject)
    TDragControlObject = class(TBaseDragControlObject)
    TDragDockObject = class(TBaseDragControlObject)
    

а вот эти функции также непригодны / несовместимы - 
    
    
    function FindDragTarget(const Pos: TPoint;
                             AllowDisabled: Boolean) : TControl;
    procedure CancelDrag;
    function IsDragObject(sender: TObject): Boolean;
    

Запуск менеджера пристыковки окон описан здесь: [Anchor Docking](<../en/Anchor_Docking.md> "Anchor Docking")

### TEdit/TCustomEdit

Элементы управления Edit, хотя они и функционируют в основном как в LCL, так и в VCL, имеют некоторые особенности, которые необходимо учитывать при преобразовании: 

  1. Из-за ограничений в интерфейсах TEdit.PasswordChar работает пока не во всех интерфейсах (хотя со временем может и будет), вместо этого следует использовать TCustomEdit.EchoMode emPassword в том случае, если текст события должен быть скрыт.
  2. Событие Drag/Dock еще не реализовано. Для получения дополнительной информации см. предыдущий раздел  Control Dragging/Docking.
  3. Свойства шрифта обычно игнорируются для согласованности интерфейса, для подробного объяснения, что и как, пожалуйста, см.  TControl.Font/TControl.ParentFont



### TDBImage

Delphi и Lazarus имеют элемент управления [TDBImage](<../en/TDBImage.md> "TDBImage"), который показывает изображения, хранящиеся в поле базы данных. В текущих стабильных версиях (1.2.4) Lazarus хранит информацию о типе изображения в поле базы данных перед фактическими данными изображения. См. процедуру _TDBImage.UpdateData_. Это означает, что Delphi и более старые реализации Lazarus не совместимы. 

В текущей стабильной версии Lazarus (1.2.0+) реализованы изменения, позволяющие использовать поведение, совместимое с Delphi. Пожалуйста, см. [Lazarus_1.2.0_release_notes#TDBImage](<../en/Lazarus_1.2.md> "Lazarus 1.2.0 release notes") для получения подробной информации о том, как активировать это. 

Текущий транк Lazarus'а возвращается к Delphi-совместимому поведению при чтении полей базы данных в формате Delphi в TDBImage. 

### (необязательно) TSplitter -> TPairSplitter

**Пожалуйста, улучшите меня!**

Теперь в LCL есть элемент управления [TSplitter](<TSplitter.md> "TSplitter/ru"), поэтому нет необходимости его преобразовывать. 

Тем не менее, если вы хотите, здесь приводится объяснение: 

Следующее в основном основано на вопросах пользователя [ Vincent Snijders](</User:Vincent> "User:Vincent") в списках рассылки и ответов [Andrew Johnson](<http://lazarus-ccr.sourceforge.net/index.php?wiki=AndrewJohnson>): 

В VCL элементы управления "Splitting" - это ползунок, который можно перетаскивать между двумя компонентами для предоставления большего или меньшего пространства одному из них за счет другого; выполняется [посредством компонента] TSplitter. Например, в IDE Delphi его можно увидеть между закрепленным Code Explorer и Source Viewer. 

LCL предоставляет свой собственный элемент управления, называемый [TPairSplitter](<TPairSplitter.md> "TPairSplitter/ru"), который служит для тех же целей, однако он не совместим с Delphi, поэтому в случае переноса кода потребуется исправление "поврежденного" кода VCL или DFM-файлов Delphi, даже если используется много общего между двумя этими компонентами. 

Так в чем именно различия?

Ну, самые большие различия в том, что VCL TSplitter не имеет [размещенных на нем] дочерних элементов, вместо этого он помещается между двумя правильно выровненными элементами управления и позволяет изменять их размер во время выполнения независимо от его собственного размера. На каждой стороне должно быть два элемента управления, чтобы что-нибудь сделать. Простым примером будет форма с выровненной по левому краю панелью, выровненным по левому краю разделителем и второй выровненной по клиенту панелью. Во время выполнения вы можете затем изменить размер, заданный для каждой панели, перетаскивая за специальный ползунок, предоставляемый элементом управления Splitter. 

Однако с точки зрения LCL, TPairSplitter - это особый вид управления с двумя панелями, и он может быть полезен только в том случае, если элементы управления для разделения находятся на этих панелях, но он все равно будет выполнять разбиение между этими панелями, независимо от того, что расположено на них. Таким образом, по аналогии с предыдущим примером, вы получите форму с выровненным клиентом TPairSplitter, выровненным по панели клиентом с левой стороны и выровненным по панели клиентом с правой стороны. 

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): иными словами, TPairSplitter - это готовый контейнер, разделенный TSplitter на две половины (в зависимости от значения свойства SplitterType на левую/правую или верхнюю/нижнюю), каждая из которых уже имеет привязку alClient, готова принимать на себя дочерние элементы и меняет свои размеры при перемещении ползунка разделителя. 

* * *

Другое важное отличие состоит в том, что в VCl, поскольку TSplitter является собственным TControl, положение сохраняется относительно других элементов управления при изменении размера. Поэтому, например, клиентская панель будет расти, в то время как другие панели этого делать не будут, таким образом, позиция разделения является относительна к выравниванию разделяемых элементов управления. 

В LCL, поскольку боковые панели отделены друг от друга, у TPairSplitter есть свойство Position, которое является абсолютным по отношению к верхнему или левому краям [родительского элемента]. Поэтому при изменении размера фактическая позиция не изменяется в зависимости от содержимого, отсюда, необходимо установить обратный вызов, чтобы обеспечить сохранение соотношения при изменении размера, если это важно. 

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): проще говоря, Position - это позиция ползунка TPairSplitter относительно левого/верхнего края контейнера (в зависимости от значения SplitterType = pstHorizontal/pstVertical соответственно). 

* * *

Например, если правая сторона вертикального разбиения должна иметь поведение, аналогичное alClient, необходимо добавить обратный вызов изменения размера формы, который выполняет что-то вроде: 
    
    
     PairSplitter.Position := PairSplitter.Width - PairSplitter.Position;
    

Итак, как я могу преобразовать существующий код, использующий TSplitter, в TPairSplitter?

Если разделитель и элементы управления создаются внутри фактической функции (например, при создании формы), преобразование не должно быть слишком сложным, в первую очередь реорганизуйте код, чтобы создать элементы управления в порядке новой иерархии, и установите родительские элементы дочерних элементов управления для разделения на левую/верхнюю и правую/нижнюю части PairSplitter. Пример изменений: 

**VCL** | **LCL**  
---|---  
      
    
    var 
      BottomPanel: TPanel;
      VerticalSplitter: TSplitter;
      LeftPanel: TPanel;
      HorizontalSplitter: TSplitter;
      MainPanel: TPanel;
    
    begin
      BottomPanel:= TPanel.Create(Self);
      with (BottomPanel) do
      begin
        Parent:= Self;
        Height:= 75;
        Align:= alBottom;
      end;
    
      VerticalSplitter:= TSplitter.Create(Self);
      with (VerticalSplitter) do
      begin
        Parent:= Self;
        Align:= alBottom;
      end;
    
      HorizontalSplitter:= TSplitter.Create(Self);
      with (HorizontalSplitter) do
      begin
        Parent:= Self;
        align:= alLeft;
      end;
    
      LeftPanel:= TPanel.Create(Self);
      with (LeftPanel) do
      begin
        Parent:= Self;
        Width:= 125;
        Align:= alLeft;
      end;
    
      MainPanel:= TPanel.Create(Self);
      with (MainPanel) do
      begin
        Parent:= Self;
        Align:= alClient;
        Caption:= 'Привет';
      end;
    end;
    

| 
    
    
    var
      BottomPanel: TPanel;
      VerticalSplitter: TPairSplitter;
      LeftPanel: TPanel;
      HorizontalSplitter: TPairSplitter;
      MainPanel: TPanel;
    
    begin
      VerticalSplitter:= TPairSplitter.Create(Self);
      with (VerticalSplitter) do
      begin
        Parent:= Self;
        Align:= alClient;
        Width:= Self.Width;
        Height:= Self.Height;
        SplitterType:= pstVertical;
        Position:= Height - 75;
        Sides[0].Width:= Width;
        Sides[0].Height:= Position;
      end;
    
      HorizontalSplitter:= TPairSplitter.Create(Self);
      with (HorizontalSplitter) do
      begin
        Parent:= VerticalSplitter.Sides[0];
        Width:= Self.Width;
        Height:= VerticalSplitter.Position;
        align:= alClient;
        SplitterType:= pstHorizontal;
        Position:= 125;
      end;
    
      LeftPanel:= TPanel.Create(Self);
      with (LeftPanel) do
      begin
        Parent:= HorizontalSplitter.Sides[0];
        Align:= alClient;
      end;
    
      MainPanel:= TPanel.Create(Self);
      with (MainPanel) do
      begin
        Parent:= HorizontalSplitter.Sides[1];
        Align:= alClient;
        Caption:= 'Привет';
      end;
    
      BottomPanel:= TPanel.Create(Self);
      with (BottomPanel) do
      begin
        Parent:= VerticalSplitter.Sides[1];
        Align:= alClient;
      end;
    end;
      
  
Таким образом, как вы видите, [преобразование] справедливо для большей [части кода] с [сохранением] иерархии элементов управления. И если вы знакомы со [структурой] DFM-файлов, изменения, необходимые для преобразования DFM -> LFM, должны быть очевидны из выше приведенного, поскольку они являются такими же изменениями в Parent/Owner и т.д. 

Таким образом, приведенный выше пример будет что-то вроде - 

**Delphi DFM** **(посторонние значения удалены)** | **Lazarus LFM** **(большинство свойств width, height... и т.д. удалены)**  
---|---  
      
    
    object VerticalSplitter: TSplitter
      Height = 3
      Cursor = crVSplit
      Align = alBottom
    end
    object HorizontalSplitter: TSplitter
      Width = 3
      Align = alLeft
    end
    object BottomPanel: TPanel
      Height = 75
      Align = alBottom
    end
    object LeftPanel: TPanel
      Width = 125
      Align = alLeft
    end
    object MainPanel: TPanel
      Align = alClient
    end
    

| 
    
    
    object VerticalSplitter: TPairSplitter
      Align = alClient
      SplitterType = pstVertical
      Position = 225
      Height = 300
      Width = 400
      object Pairsplitterside1: TPairSplitterIde
        object HorizontalSplitter: TPairSplitter
          Align = alClient
          Position = 125
          object Pairsplitterside3: TPairSplitterIde
            Width = 125
            object LeftPanel: TPanel
              Align = alClient
              Width = 125
            end
          end
          object Pairsplitterside4: TPairSplitterIde
            object MainPanel: TPanel
              Align = alClient
            end
          end
        end
      end
      object Pairsplitterside2: TPairSplitterIde
        object BottomPanel: TPanel
          Align = alClient
          Height = 75
        end
      end
    end
      
  
### TCustomTreeView/TTreeView

И VCL, и LCL предоставляют компонент TCustomTreeView/TTreeView, используемый для древовидных списков данных с несколькими узлами, расширенного выбора и списков изображений, и хотя фактические функции сопоставимы, не все свойства полностью совместимы. Основные различия заключаются в следующем - 

**Список не полон, но обновлен для включения в него функции TCustomTreeView Mark и protected-методов**

  1. LCL предоставляет TCustomTreeView.Options, набор опций, которые можно установить в элементе управления, чтобы изменить его поведение и внешний вид. Вот эти опции: 
     * tvoAllowMultiselect - включает режим выбора нескольких узлов, эквивалентный включению TCustomTreeView.MultiSelect в VCL D6
     * tvoAutoExpand - автоматическое раскрытие узлов, что эквивалентно включению TCustomTreeView.AutoExpand
     * tvoAutoInsertMark - обновляет предосмотр Drag при перемещении мышью.
     * tvoAutoItemHeight - автоматически регулирует высоту элемента.
     * tvoHideSelection - не помечать выбранный элемент.
     * tvoHotTrack - использовать Hot Tracking, что эквивалетно включению TCustomTreeview.HotTrack
     * tvoKeepCollapsedNodes - сохранять дочерние узлы при их сжатии/сворачивании
     * tvoReadOnly - сделать Treeview доступным только для чтения, эквивалентно включению TCustomTreeview.ReadOnly
     * tvoRightClickSelect - разрешить использовать правую кнопку мыши для выбора узлов, что эквивалентно включению TCustomTreeView.RightClickSelect
     * tvoRowSelect - разрешить выбор строк, что эквивалентно включению TCustomTreeView.RowSelect
     * tvoShowButtons - показывать кнопки, что эквивалентно включению TCustomTreeView.ShowButtons
     * tvoShowLines - показывать линии узлов, что эквивалентные включению TCustomTreeView.ShowLines
     * tvoShowRoot - показывать корневые элементы, что эквивалентные включению TCustomTreeView.ShowRoot
     * tvoShowSeparators - показывать разделитель
     * tvoToolTips - показывать подсказки для отдельных узлов
  2. LCL предоставляет дополнительные свойства: 
     * Событие TCustomTreeView.OnSelectionChange
     * TCustomTreeView.DefaultItems, для количества элементов [дерева] по умолчанию
     * TCustomTreeView.ExpandSignType для определения знака, используемого в раскрывающихся/сворачивающихся узлах
  3. Хотя большинство событий OnDrag/OnDock доступны в LCL, они не работают. Для получения дополнительной информации см. предыдущий раздел [Control Dragging/Docking](<Lazarus_For_Delphi_Users.md> "Lazarus For Delphi Users/ru").



### Сообщения / События

Порядок и частота сообщений и событий (OnShow, OnActivate, OnEnter, ...) отличаются от VCL и зависят от [ widgetset](<../en/Widgetset.md> "Widgetset"). LCL предоставляет подмножество WinAPI-подобных сообщений, чтобы упростить перенос компонентов Delphi, но почти все сообщения LCL работают несколько иначе, чем аналоги VCL/WinAPI. Большая часть кода Delphi, использующего сообщения WinAPI, использует их, потому что VCL не имеет функции или по соображениям скорости. Такие вещи редко работают одинаково в LCL, поэтому их нужно проверять вручную. Вот почему сообщения LCL называются, например, LM_SIZE вместо WM_SIZE (модуль Lmessages). 

**Замечание по обработке пользовательских сообщений!** Начиная с версии 0.9.26 (декабрь 2008г.), способ обработки пользовательских сообщений WinApi (например, WM_HOTKEY, WM_SYSCOMMAND) отличается от способа обработки этих сообщений в Delphi. В настоящее время вы не можете обработать их с помощью директивы **message** или переопределением метода _WndProc_ формы. Единственный способ обработать их в форме - это самому перехватить сообщение Windows _windowproc_. Подробнее читайте здесь: [ Processing non-user messages in your window](<../en/Win32/64_Interface.md> "Win32/64 Interface")

## См. также

  * [ Code Conversion Guide (from Delphi & Kylix)](<Code_Conversion_Guide.md> "Code Conversion Guide/ru")
  * [Compile With Delphi](<Compile_With_Delphi.md> "Compile With Delphi/ru")

---

_Source: [https://wiki.freepascal.org/Lazarus_For_Delphi_Users/ru](https://web.archive.org/web/20250516144206/https://wiki.freepascal.org/Lazarus_For_Delphi_Users/ru)_
