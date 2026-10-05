# How To Write Lazarus Component

│ **[English (en)](<../en/How_To_Write_Lazarus_Component.md>)** │  **русский (ru)** │

  
Это руководство по созданию компонентов. 

## Contents

  * 1 Шаг 1: Создание пакета
  * 2 Шаг 2: Создание модуля
    * 2.1 Создание нового модуля
    * 2.2 Добавление существующего модуля
  * 3 Шаг 3: Создание значков для пакета
    * 3.1 Использование редактора изображений Lazarus
    * 3.2 Использование lazres
      * 3.2.1 Создание lrs-файла
      * 3.2.2 Создание res-файла
    * 3.3 Использование glazres
  * 4 Перекомпиляция пакетов
  * 5 Удаление пакетов
  * 6 Усовершенствование mycom.pas
  * 7 Использование встроенных (визуальных) компонентов
  * 8 Использование пользовательской процедуры рисования
  * 9 Интеграция компонента с IDE
    * 9.1 Редакторы свойств
    * 9.2 Редакторы компонентов
  * 10 Отладка компонента в Design-time
  * 11 См. также



## Шаг 1: Создание пакета

  * В меню IDE Lazarus нажмите "Package" > "New package" для запуска диспетчера пакетов.



[![package menu.png](https://wiki.freepascal.org/images/f/fb/package_menu.png)](</File:package_menu.png>)

  * Появится диалоговое окно "Save". Выберите папку и имя файла и нажмите "Save". Если программа IDE предложит использовать строчные имена файлов, нажмите "да".


  * Поздравляем: вы только что создали свой первый пакет!



[![Package Maker](https://wiki.freepascal.org/images/3/37/How_to_write_lazarus_component_package_maker.png)](</File:How_to_write_lazarus_component_package_maker.png> "Package Maker")

## Шаг 2: Создание модуля

Вы можете создать новый модуль или использовать существующий файл. Оба варианта описаны ниже. 

### Создание нового модуля

  * Используйте кнопку **Add > New component**.  




[![package new component.png](https://wiki.freepascal.org/images/6/60/package_new_component.png)](</File:package_new_component.png>)  


  * Выберите компонент, [например такой] как TComboBox.
  * Выберите _customcontrol1.pas_ в качестве **Unit File Name** и _CustomControl1_ в качестве **Unit Name**.
  * Теперь вы можете задать иконку компоненту и определить, на какой палитре компонент должен появиться позже в Lazarus-IDE.
  * Нажмите **OK**[, чтобы создать новый компонент].  



    
    
    unit CustomControl1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs, StdCtrls;
    
    type
      TCustomControl1 = class(TComboBox)
      private
        { Private declarations }
      protected
        { Protected declarations }
      public
        { Public declarations }
      published
        { Published declarations }
      end;
    
    procedure Register;
    
    implementation
    
    procedure Register;
    begin
      RegisterComponents('Standard', [TCustomControl1]);
    end;
    
    end.
    

  * Установите пакет, кликнув кнопку **Use - > Install** вверху редактора пакетов.  




[![package install.png](https://wiki.freepascal.org/images/2/2a/package_install.png)](</File:package_install.png>)  


  * Теперь IDE спросит вас, должна ли она быть пересобрана. Ответьте "Да".  




[![package rebuild.png](https://wiki.freepascal.org/images/6/67/package_rebuild.png)](</File:package_rebuild.png>)  


  * Перезапустите Lazarus[, если он не сделал этого сам,] и посмотрите на свой новый компонент в палитре компонентов. Поздравляем: вы только что установили свой первый пакет с вашим первым компонентом.  




[![package installed.png](https://wiki.freepascal.org/images/7/79/package_installed.png)](</File:package_installed.png>)  


![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если вы не видите свой новый компонент в палитре компонентов, скорее всего, вы используете не пересобранную версию Lazarus. Вы можете установить, где Lazarus пересобирается, используя [пункт меню] Tools -> Options -> Files -> Lazarus directory. 

Вместо прямого вызова Lazarus вы также можете использовать startlazarus, который запускает недавно созданный Lazarus, например исполняемый файл Lazarus в каталоге ~ / .lazarus, если у вас нет доступа на запись в каталог, в который был установлен Lazarus.

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo") 23:36, 1 December 2018 (CET): В корневом каталоге Lazarus'а после пересборки обычно содержится три исполняемых файла: lazarus (текущий запускаемый файл), startlazarus (лаучер для lazarus) и lazarus.old (старая версия исполняемого файла, существующая после последней удачной пересборки среды). В примечании, вероятнее всего, речь идет о них. 

* * *

### Добавление существующего модуля

Если у вас уже есть модуль, вы можете добавить его в пакет: 

[![package existing unit.png](https://wiki.freepascal.org/images/a/a4/package_existing_unit.png)](</File:package_existing_unit.png>)  


  * Нажмите кнопку **Add** , перейдите к вкладке **Add Files**. В [колонке] file name модуля добавьте ваш существующий файл. Нажмите **Add files to package**. Если диспетчер пакетов жалуется, что модуль находится вне заданных к модулям путей, нажмите "Да", чтобы добавить каталог в пути к модулям.
  * Нажмите кнопку **Add** снова, перейдите на вкладку **Add Files** , найдите файл _.lrs_ и нажмите "OK" (см. Шаг 3 о создании файла иконки).
  * Снова нажмите кнопку **Add** , перейдите к вкладке **New Requirement** [(новая зависимость)]. В имени пакета выберите **LCL** и нажмите OK.



Конечный результат должен выглядеть следующим образом:  


[![Package Maker](https://wiki.freepascal.org/images/d/d9/How_to_write_lazarus_component_package_maker_done.png)](</File:How_to_write_lazarus_component_package_maker_done.png> "Package Maker")  


  * Щелкните по дереву файлов в диспетчере пакетов. В свойствах файла убедитесь, что установлен флажок _Register unit_.
  * Нажмите кнопку Options. Перейдите на вкладку IDE Integration. В опции Package Type убедитесь, что выбраны [режимы] Designtime и Runtime.
  * Нажмите кнопку Compile, чтобы проверить, файлы компилируются без ошибок.
  * Нажмите кнопку Install. Lazarus автоматически пересоберется и перезагрузится.



Компонент создан и готов к использованию:  


[![Component Created](https://wiki.freepascal.org/images/3/3c/How_to_write_lazarus_component_created.png)](</File:How_to_write_lazarus_component_created.png> "Component Created")  


## Шаг 3: Создание значков для пакета

Вы должны создать PNG-файлы размером 24x24 пикселя в виде значков. Если для Lazarus 1.8+ вы хотите, чтобы значки палитры масштабировались при более высоких разрешениях экрана, вы также должны предоставлять значки 36x36 и 48x48 пикселей. Имена более крупных значков требуют добавления [суффиксов] "_150" и "_200" к имени файла, соответственно [увеличению разрешения] ("_150" для 150%, "_200" для 200%). 

FPC способен использовать стандартные файлы ресурсов `.rc` или скомпилированные ресурсы `.res`, [начиная] с версии fpc 2.6. См. [FPC ресурсы](<Lazarus_Resources.md> "Lazarus Resources/ru"). [О файлах] `.lrs`: см.ниже. 

### Использование редактора изображений Lazarus

Вы можете использовать [Lazarus Image Editor](<../en/Lazarus_Image_Editor.md> "Lazarus Image Editor") для создания изображений в .lrs формате. 

  


* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo") 17:13, 3 December 2018 (CET): Поскольку старшие версии Лазаруса (насколько я помню, после v.1.6.х точно) позволяют задавать иконку изображения в png-формате, то удобнее всего для их создания использовать бесплатный векторный редактор [InkScape](<https://inkscape.org/ru>), "родным" для которого является формат SVG. Среди его преимуществ: 

  * работа со слоями
  * поддержка альфа-канала
  * предосмотр иконок в разных разрешениях (в файле настроек можно задать даже нестандартные значения)
  * вменяемый и оперативный саппорт (можно напрямую пообщаться с разработчиками в [чате](<https://inkscape.org/ru/community/discussion>))
  * экспорт изображений в png-формат



* * *

### Использование lazres

`lazres` обычно находится в каталоге инструментов Lazarus. 

Вам может понадобиться скомпилировать [lazres](<../en/lazres.md> "lazres") при первом использовании. Просто откройте _lazres.lpi_ в среде IDE и нажмите **run > build** в меню. 

#### Создание lrs-файла

Для создания lrs-файла запустите: 
    
    
    ~/lazarus/tools/lazres samplepackage.lrs TMyCom.png
    

или, если доступны изображения с высоким разрешением 
    
    
    ~/lazarus/tools/lazres samplepackage.lrs TMyCom.png TMyCom_150.pgn TMyCom_200.png
    

где 

  * _samplepackage_ \- имя вашего пакета
  * _TMyCom_ \- имя вашего компонента. Имя изображения должно совпадать с именем вашего компонента!



Вы можете добавить несколько изображений в файл lrs, добавив имя файла изображения в конец [команды]. Например: 
    
    
    ~/lazarus/tools/lazres samplepackage.lrs TMyCom.png TMyOtherCom.png ...
    

  
**Пример**

Ниже приведен образец полученного файла _samplepackage.lrs_. 
    
    
    LazarusResources.Add('TMyCom','PNG',[
      #137'PNG'#13#10#26#10#0#0#0#13'IHDR'#0#0#0#24#0#0#0#24#8#2#0#0#0'o'#21#170#175
      +#0#0#0#4'gAMA'#0#0#177#143#11#252'a'#5#0#0#0'|IDAT8O'#237#212#209#10#192' '#8
      +#5'P'#247#231#251's'#215#138#133#164#166'\'#220#195'`'#209'c'#157'L'#173#131
      +#153#169'd4'#168'dP'#137'r_'#235'5'#136'@Zmk'#16'd9'#144#176#232#164'1'#247
      +'I'#8#160'IL'#206'C'#179#144#12#199#140'.'#134#244#141'~'#168#247#209'S~;'#29
      +'V+'#196#201'^'#10#15#150'?'#255#18#227#206'NZ>42'#181#159#226#144#15'@'#201
      +#148#168'e'#224'7f<@4'#130'u_YD'#23#213#131#134'Q]'#158#188#135#0#0#0#0'IEND'
      +#174'B`'#130
    ]);
    

Обязательно включите свой файл ресурсов в исходник нового компонента, добавив нижеследующее в модуль вашего компонента и включив "LResources" в раздел uses 
    
    
    initialization
      {$I samplepackage.lrs}
    

  


#### Создание res-файла

lazres также может создать скомпилированный файл ресурсов, просто укажите файл с расширением .res, например: 
    
    
    ~/lazarus/tools/lazres samplepackage.res TMyCom.png
    

или, для иконок с высоким разрешением: 
    
    
    ~/lazarus/tools/lazres samplepackage.res TMyCom.png TMyCom_150.png TMyCom_200.png
    

В этом случае вместо включения файла lrs в разделе инициализации просто укажите файл ресурсов в любом месте исходного файла компонента 
    
    
      {$R samplepackage.res}
    

### Использование glazres

**GLazRes** это графическая версия `lazres`, которая может собирать файлы в файл ресурсов Lazarus (.lrs). Его можно найти в каталоге инструментов установки Lazarus. 

## Перекомпиляция пакетов

Вам нужно пересобирать пакет каждый раз, когда вы вносите изменения в файл _mycom.pas_. Чтобы пересобрать пакет, откройте файл _samplepackage.lpk_ в диспетчере пакетов и нажмите кнопку Install. 

## Удаление пакетов

  * Чтобы удалить установленные компоненты: в меню IDE выберите Package > Configure installed packages. На следующем рисунке показан инструмент Installed Packages.


    
    
    [![Installed Components](https://wiki.freepascal.org/images/0/0e/How_to_write_lazarus_component_installed_components.png)](</File:How_to_write_lazarus_component_installed_components.png> "Installed Components")
    

  * Выберите пакет, который вы хотите удалить, и нажмите Uninstall selection.



Если что-то пойдет не так с пакетом (например, каталог пакета удаляется без деинсталляции [пакета в первую очередь]), Lazarus может не разрешить вам удалять пакеты. Чтобы устранить проблему, в меню IDE выберите Tools > Build Lazarus. Lazarus пересоберет все пакеты и перезапустит их. Теперь вы можете удалить проблемные пакеты. 

## Усовершенствование mycom.pas

  * Код в _mycom.pas_ выше дает вам основы того, что вам нужно для создания компонента. Далее приведена усовершенствованная версия с некоторыми советами о том, как писать процедуры и события для компонентов.
  * _OnChange2_ показывает, как создавать события.
  * _OnSample_ показывает, как создавать пользовательские события.
  * _MyText_ и _MyText2_ показывает различные способы записи свойств.
  * Вы можете использовать TComboBox вместо TCustomComboBox в качестве базового класса, который [объявляет] публичными все свойства как TComboBox.
  * Если TCustomComboBox используется в качестве базового класса, вы заметите, что в Инспекторе объектов IDE будет отсутствовать множество свойств и событий. Чтобы добавить эти свойства и события, просто скопируйте и вставьте свойства, перечисленные ниже [комментария] _// properties from TComboBox_. Этот список свойств можно получить из объявления TComboBox в модуле StdCtrls. Исключите [из списка] любое свойство, которое вы хотите [определить] самостоятельно.


    
    
    unit mycom;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, StdCtrls, Forms, Dialogs,
      LCLType,LCLIntf,lresources,LCLProc;
    
    type
    
      TSampleEvent = procedure(MyText: String) of Object;
    
      TMyCom = class (TCustomComboBox)
      private
        FMyText: String;
        FOnChange2: TNotifyEvent;
        FOnSample: TSampleEvent;
      public
        constructor Create(TheOwner: TComponent); override;
        procedure CreateWnd; override;
        procedure Change; override;
      protected
        function GetMyText2: String;
        procedure SetMyText2(MyText: String);
      published
        property MyText: String read FMyText write FMyText;
        property MyText2: String read GetMyText2 write SetMyText2;
        property OnChange2: TNotifyEvent read FOnChange2 write FOnChange2;
        property OnSample: TSampleEvent read FOnSample write FOnSample;
        
        // properties from TComboBox
        property Align;
        property Anchors;
        property ArrowKeysTraverseList;
        property AutoComplete;
        property AutoCompleteText;
        property AutoDropDown;
        property AutoSelect;
        property AutoSize;
        property BidiMode;
        property BorderSpacing;
        property CharCase;
        property Color;
        property Ctl3D;
        property Constraints;
        property DragCursor;
        property DragMode;
        property DropDownCount;
        property Enabled;
        property Font;
        property ItemHeight;
        property ItemIndex;
        property Items;
        property ItemWidth;
        property MaxLength;
        property OnChange;
        property OnChangeBounds;
        property OnClick;
        property OnCloseUp;
        property OnContextPopup;
        property OnDblClick;
        property OnDragDrop;
        property OnDragOver;
        property OnDrawItem;
        property OnEndDrag;
        property OnDropDown;
        property OnEditingDone;
        property OnEnter;
        property OnExit;
        property OnGetItems;
        property OnKeyDown;
        property OnKeyPress;
        property OnKeyUp;
        property OnMeasureItem;
        property OnMouseDown;
        property OnMouseMove;
        property OnMouseUp;
        property OnStartDrag;
        property OnSelect;
        property OnUTF8KeyPress;
        property ParentBidiMode;
        property ParentColor;
        property ParentCtl3D;
        property ParentFont;
        property ParentShowHint;
        property PopupMenu;
        property ReadOnly;
        property ShowHint;
        property Sorted;
        property Style;
        property TabOrder;
        property TabStop;
        property Text;
        property Visible;    
      end;
    
    procedure Register;
    
    implementation
    
    procedure Register;
    begin
      RegisterComponents('Sample',[TMyCom]);
    end;
    
    constructor TMyCom.Create(TheOwner: TComponent);
    begin
      inherited Create(TheOwner);
      Self.Style := csDropDownList;
    end;
    
    procedure TMyCom.CreateWnd;
    begin
      inherited CreateWnd;
      Items.Assign(Screen.Fonts);
    end;
    
    procedure TMyCom.Change;
    begin
      inherited;
      if Assigned(FOnChange2) then FOnChange2(Self);
      if Assigned(FOnSample) then FOnSample(FMyText);
    end;
    
    function TMyCom.GetMyText2: String;
    begin
      Result:=FMyText;
    end;
    
    procedure TMyCom.SetMyText2(MyText: String);
    begin
      FMyText:=MyText;
    end;
    
    initialization
      {$I samplepackage.lrs}
    
    end.
    

Также вы заметите, что в Инспекторе объектов IDE существуют некоторые необъявленные и, возможно, нежелательные элементы. Чтобы удалить те из них, которые вам не нужны, вы можете переобъявить их в разделе _Published_ как простые переменные. Например: 
    
    
    Published
    ... 
        property Height: Integer;
        property Width:  Integer;
    ...
    

## Использование встроенных (визуальных) компонентов

Можно использовать стандартные компоненты, встроенные в ваши собственные компоненты (посмотрите для примера на TLabeledEdit или TButtonPanel). 

Предположим, вы хотите создать пользовательскую панель с TLabel на ней. С помощью описанных выше шагов можно создать базовый пакет и исходные файлы. Теперь сделайте следующее для добавления TLabel в компонент: 

  * Добавьте приватный атрибут (поле) для компонента label (`FEmbeddedLabel: TLabel;`).
  * Добавьте published-свойство только для чтения для компонента label (`property EmbeddedLabel: TLabel read FEmbeddedLabel;`)
  * Создайте label в конструкторе (переопределенном) компонента (`FEmbeddedLabel: = TLabel.Create(self);`)
  * Задайте родительский элемент компонента (`FEmbeddedLabel.Parent: = self;`)
  * Если встроенный компонент не является 'субкомпонентом' по умолчанию (например, TBoundLabel, TPanelBitBtn и т.д.), то добавьте вызов в SetSubComponent. Это необходимо для IDE, чтобы она знала, что она также должна хранить свойства встроенного компонента. По умолчанию TLabel не является субкомпонентом, поэтому необходимо добавить вызов метода (`FEmbeddedLabel.SetSubComponent(true);`).



В итоге вы получите что-то вроде этого (показаны только основные части): 
    
    
    TEnhancedPanel = class(TCustomControl)
    private
      { Новые аттрибуты для встроенного label }
      FEmbeddedLabel: TLabel;
      
    public
      { Конструктор должен быть переопределен, чтобы label могла быть создана}
      constructor Create(AOwner: TComponent); override;
      
    published
      { Сделаем label видимым в IDE }
      property EmbeddedLabel: TLabel read FEmbeddedLabel;
    end;
    
    implementation
    
    constructor TEnhancedPanel.Create(AOwner: TComponent);
    begin
      inherited Create(AOwner);
    
      // Устанавливаем дефолтовые width и height
      with GetControlClassDefaultSize do
        SetInitialBounds(0, 0, CX, CY);
    
      // Добавляем встроенный label
      FEmbeddedLabel := TLabel.Create(Self); // Добавляем встроенный label
      FEmbeddedLabel.Parent := self;         // Показываем label на panel
      FEmbeddedLabel.SetSubComponent(true);  // Говорим IDE сохранять измененные свойства
      FLabel.Name := 'EmbeddedLabel';        
      FLabel.Caption := 'Howdy World!';
    
      // Убеждаемся, что встроенная label не может быть выделена/удалена средствами IDE
      FLabel.ControlStyle := FLabel.ControlStyle - [csNoDesignSelectable];
      
      // Задаем другие свойства, если необходимо
      //...
      
    end;
    

## Использование пользовательской процедуры рисования

Вы всегда можете переопределить компонент внутри вашей программы. Например, это реализует пользовательскую процедуру Paint для TLabel: 
    
    
    type
      TMyLabel = class(TLabel)
        protected
          procedure Paint; override;
      end;
    {...}
    implementation
    {...}
    procedure TMyLabel.Paint;
    begin
      // ваш код для реализации Paint, например
      Canvas.TextOut(0,0,Caption);
    end;
    

Теперь вы можете создать MyLabel внутри вашей программы во время выполнения с этой переопределенной процедурой Paint вместо стандартной. 

Для большинства компонентов и для большинства методов было бы полезно вызвать унаследованную процедуру внутри него: 
    
    
    procedure TMyLabel.Paint;
    begin
    
      inherited Paint;   /////////////////////
    
      // ваш код для реализации Paint, например
      Canvas.TextOut(0,0,Caption);
    end;
    

Однако унаследованное поведение нежелательно в этом случае, поскольку второе действие записи будет перекрывать первый (унаследованный). 

## Интеграция компонента с IDE

### Редакторы свойств

Редакторы свойств предоставляют настраиваемые диалоги для редактирования свойств в инспекторе объектов. Для большинства свойств, таких как строки, строковые списки, изображения, перечисленные типы и другие, уже есть редакторы свойств по умолчанию, но если у пользовательского компонента есть особый вид свойства, для изменения свойства может потребоваться настраиваемый диалог. 

Каждый редактор свойств - это класс, который должен происходить из TPropertyEditor или одного из его потомков и реализовывать методы этого базового класса. Они должны быть зарегистрированы в процедуре 'Register', используя функцию RegisterPropertyEditor из модуля PropEdits. Это стандарт для имен редакторов свойств с именем свойства, за которым следует [слово] 'Property', например [имя] TFieldProperty для редактора свойств свойства TField. 
    
    
      TPropertyEditor = class
      public
        function  AllEqual: Boolean; Virtual;
        function  AutoFill: Boolean; Virtual;
        procedure Edit; Virtual;     // Активируется двойным щелчком по значению свойства
        procedure ShowValue; Virtual; // Активируется щелчком по элементу управления значения свойства
        function  GetAttributes: TPropertyAttributes; Virtual;
        function  GetEditLimit: Integer; Virtual;
        function  GetName: ShortString; Virtual;
        procedure GetProperties(Proc: TGetPropEditProc); Virtual;
        function  GetHint(HintType: TPropEditHint; x, y: integer): String; Virtual;
        function  GetDefaultValue: AnsiString; Virtual;
        procedure GetValues(Proc: TGetStrProc); Virtual;
        procedure SetValue(const NewValue: AnsiString); Virtual;
        procedure UpdateSubProperties; Virtual;
        function  SubPropertiesNeedsUpdate: Boolean; Virtual;
        function  IsDefaultValue: Boolean; Virtual;
        function  IsNotDefaultValue: Boolean; Virtual;
        // ... укорочено
      end;
    

Хорошим примером для редактора свойств является редактор свойств TFont. 

Один из наиболее распространенных случаев для редакторов свойств - это свойства, которые являются классами. Поскольку классы имеют много полей и могут иметь различные форматы, Lazarus не может иметь [универсальное] поле редактирования в инспекторе объектов, способное их редактировать, [как это сделано,] например, для строк и числовых типов. 

Для классов соглашение состоит в том, чтобы в поле значений постоянно отображалось имя класса в круглых скобках, например "(TFont)", а кнопка "..." показывала диалог для редактирования этого класса. Это поведение, за исключением диалога, реализуется стандартным редактором свойств для классов, называемых TClassPropertyEditor, который может быть унаследован при создании редакторов свойств для классов: 
    
    
    TClassPropertyEditor = class(TPropertyEditor)
    public
      constructor Create(Hook: TPropertyEditorHook; APropCount: Integer); Override;
      function GetAttributes: TPropertyAttributes; Override;
      procedure GetProperties(Proc: TGetPropEditProc); Override;
      function GetValue: AnsiString; Override;
      property SubPropsTypeFilter: TTypeKinds Read FSubPropsTypeFilter
                                             Write SetSubPropsTypeFilter
                                           Default tkAny;
    end;
    

Возвращаясь к примеру TFont, наследование от TClassPropertyEditor уже предлагает часть желаемого поведения, а затем классу TFontPropertyEditor необходимо реализовать отображение диалога в методе Edit и установить атрибуты для редактора: 
    
    
      TFontPropertyEditor = class(TClassPropertyEditor)
      public
        procedure Edit; Override;
        function  GetAttributes: TPropertyAttributes; Override;
      end;
    
    procedure TFontPropertyEditor.Edit;
    var 
      FontDialog: TFontDialog;
    begin
      FontDialog := TFontDialog.Create(NIL);
      try
        FontDialog.Font    := TFont(GetObjectValue(TFont));
        FontDialog.Options := FontDialog.Options + [fdShowHelp, fdForceFontExist];
        if FontDialog.Execute then SetPtrValue(FontDialog.Font);
      finally
        FontDialog.Free;
      end;
    end;
    
    function TFontPropertyEditor.GetAttributes: TPropertyAttributes;
    begin
      Result := [paMultiSelect, paSubProperties, paDialog, paReadOnly];
    end;
    

### Редакторы компонентов

Редакторы компонентов управляют поведением компонентов по щелчку правой кнопкой мыши и двойному щелчку в дизайнере форм. 

Каждый редактор компонентов - это класс, который должен происходить от TComponentEditor или одного из его потомков и реализовывать методы из этого базового класса. Он должен быть зарегистрирован в процедуре 'Register', использущей функцию RegisterComponentEditor из модуля ComponentEditors. Это стандарт для именования редакторов компонентов с именем компонента, за которым следует [слово] 'Editor', например TStringGridComponentEditor для редактора свойств компонента TStringGrid. Хотя редакторы пользовательских компонентов должны быть основаны на TComponentEditor, большинство его методов на самом деле [берут начало] от предка, поэтому необходимо также знать TBaseComponentEditor: 
    
    
      TBaseComponentEditor = class
      protected
      public
        constructor Create(AComponent: TComponent;
                           ADesigner: TComponentEditorDesigner); Virtual;
        procedure Edit; Virtual; Abstract;
        procedure ExecuteVerb(Index: Integer); Virtual; Abstract;
        function  GetVerb(Index: Integer): String; Virtual; Abstract;
        function  GetVerbCount: Integer; Virtual; Abstract;
        procedure PrepareItem(Index: Integer; const AnItem: TMenuItem); Virtual; Abstract;
        procedure Copy; Virtual; Abstract;
        function  IsInInlined: Boolean; Virtual; Abstract;
        function  GetComponent: TComponent; Virtual; Abstract;
        function  GetDesigner: TComponentEditorDesigner; Virtual; Abstract;
        function  GetHook(out Hook: TPropertyEditorHook): Boolean; Virtual; Abstract;
        procedure Modified; Virtual; Abstract;
      end;
    

Наиболее важным методом редактора компонентов является [метод] Edit, который вызывается по двойному щелчку на компоненте. Когда вызывается контекстное меню для компонента, запрашивается метод GetVerbCount и GetVerb для создания меню. Если выбран один из глаголов (что означает пункты меню в этом случае), вызывается ExecuteVerb. Существует редактор компонентов по умолчанию (TDefaultEditor), который реализует Edit для поиска свойств компонента для наиболее подходящего для редактирования. Обычно он выбирает событие, которое будет редактироваться, путем добавления скелета кода в редактор кода и установки курсора в это место. 

Другими важными методами из TBaseComponentEditor являются: 

  * **ExecuteVerb(Index)** \- Выполняет один из дополнительных пунктов меню, расположенных во всплывающем меню правой кнопки мыши;
  * **GetVerb** – Возвращает имя каждого дополнительного пункта всплывающего меню. Обратите внимание, что редактор компонентов должен размещать специальные символы заголовка элемента, такие как &, для создания ускорителя клавиатуры и "-" для создания разделителя;
  * **GetVerbCount** – Возвращает количество элементов, которые будут добавлены во всплывающее меню. Индекс для подпрограмм GetVerb и ExecuteVerb равен нулю, начиная от 0 до GetVerbCount - 1;
  * **PrepareItem** – Вызывается для каждого глагола [(GetVerbCount и GetVerb)] после создания пункта меню. Позволяет настраивать элемент меню, например, добавляя подэлементы, добавляя флажок или даже скрывая его, устанавливая Visible в false;
  * **Copy** \- Вызывается, когда компонент копируется в буфер обмена. Данные компонента при использовании Lazarus'ом всегда будут [только] добавляться и не могут быть изменены. Этот метод предназначен для добавления различных типов из буфера обмена при вставке компонента в другие приложения, но не будет влиять на вставку в Lazarus'е.



Простым и интересным примером является редактор компонентов TCheckListBox, который создает диалоговое окно для редактирования. Более удобный, чем реализация всех методов из TBaseComponentEditor, [он] наследуется от TComponentEditor, и это то, что делает TCheckListBoxEditor. Этот базовый класс добавляет пустые реализации для большинства методов и некоторые умолчательные для других. Для [метода] Edit он вызывает ExecuteVerb(0), поэтому, если первый пункт [popup menu] будет сопоставлен действию двойного щелчка мыши, что является условным обозначением для редактора, то нет необходимости в [ручной] реализации [кода метода] Edit. Это основное действие для двойного щелчка и первого элемента меню часто является диалогом, и для TCheckListBox это также реализуется [так]: 
    
    
      TCheckListBoxComponentEditor = class(TComponentEditor)
      protected
        procedure DoShowEditor;
      public
        procedure ExecuteVerb(Index: Integer); override;
        function  GetVerb(Index: Integer): String; override;
        function  GetVerbCount: Integer; override;
      end;
    
    procedure TCheckGroupComponentEditor.DoShowEditor;
    var
      Dlg: TCheckGroupEditorDlg;
    begin
      Dlg := TCheckGroupEditorDlg.Create(NIL);
      try
        // .. укорочено
        Dlg.ShowModal;
        // .. укорочено
      finally
        Dlg.Free;
      end;
    end;
    
    procedure TCheckGroupComponentEditor.ExecuteVerb(Index: Integer);
    begin
      case Index of
        0: DoShowEditor;
      end;
    end;
    
    function TCheckGroupComponentEditor.GetVerb(Index: Integer): String;
    begin
      Result := 'CheckBox Editor...';
    end;
    
    function TCheckGroupComponentEditor.GetVerbCount: Integer;
    begin
      Result := 1;
    end;
    

## Отладка компонента в Design-time

Чтобы отловить ошибки времени разработки во (вновь созданном) _dbgcomponent_ : 

  * откройте проект `C:\lazarus\ide\lazarus.lpi`;
  * запустите проект;
  * установите точку останова в _dbgcomponent_ в главном (1-ом) приложении;
  * используйте паскалевский код _dbgcomponent_ во втором приложении;
  * выполнить пошагово код компонента времени разработки в режиме отладки; делайте все, что необходимо;



## См. также

  * [Extending the IDE](<Extending_the_IDE.md> "Extending the IDE/ru")
  * [Lazarus Packages](<Lazarus_Packages.md> "Lazarus Packages/ru") \- A guide for creating a package under Lazarus
  * [Components and Code examples](<Components_and_Code_examples.md> "Components and Code examples/ru")
  * [Lazarus Components](<../en/Lazarus_Components.md> "Lazarus Components")
  * [IDE Window: Add to Package](</index.php?title=IDE_Window:_Add_to_Package&action=edit&redlink=1> "IDE Window: Add to Package \(page does not exist\)")
  * [IDE Window: Create new package component](<../en/IDE_Window__Create_new_package_component.md> "IDE Window: Create new package component")
  * [Создание визуальных компонент для Lazarus](<http://freepascal.ru/article/lazarus/20050530152619>) \- статья Сергея Смирнова с русскоязычного ресурса



Все вопросы вы можете оставлять также [на этой](<http://forum.lazarus.freepascal.org/index.php/topic,5789.0.html>) странице

---

_Source: [https://wiki.freepascal.org/How_To_Write_Lazarus_Component/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/How_To_Write_Lazarus_Component/ru)_
