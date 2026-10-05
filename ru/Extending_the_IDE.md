# Extending the IDE

│ **[English (en)](<../en/Extending_the_IDE.md>)** │  **русский (ru)** │

## Contents

  * 1 Расширение IDE
    * 1.1 Обзор
    * 1.2 Пакеты и зависимости Makefile
  * 2 Пункты меню
    * 2.1 Добавление пункта меню
  * 3 Сочетания клавиш
  * 4 Конфигурационные файлы
    * 4.1 Загрузка/Сохранение настроек
  * 5 Компоненты
    * 5.1 Расширение существующих компонентов
    * 5.2 Написание компонент
      * 5.2.1 Предоставление новому компоненту иконки для палитры компонентов
      * 5.2.2 Скрытие компонента в палитре компонентов
    * 5.3 Написание редактора компонента
    * 5.4 Написание редакторов свойств
      * 5.4.1 Сокрытие свойства в инспекторе объектов
      * 5.4.2 Поиск редактора свойств, используемого для свойства
  * 6 Формы
  * 7 Дизайнер
    * 7.1 Написание дизайнера-посредника
    * 7.2 Создание нового уникального имени компонента
    * 7.3 Выбор компонента в конструкторе/инспекторе объектов
    * 7.4 Получение файла модуля, дизайнера формы
    * 7.5 Сохранение модуля, дизайнера формы
    * 7.6 Сохранение формы дизайнера как кода Паскаль
  * 8 Обработчики событий
    * 8.1 События дизайнера
      * 8.1.1 Примеры для событий дизайнера
        * 8.1.1.1 Пример: когда [на родителя] помещается новый компонент, перемещать элемент управления в дочерний элемент
      * 8.1.2 Отключение обработчика мыши в [режиме] дизайнера
    * 8.2 Получение уведомлений при изменении дизайнера форм
    * 8.3 События проекта
    * 8.4 Другие события IDE
  * 9 Проект
    * 9.1 Текущий проект
    * 9.2 Все модули текущего проекта
      * 9.2.1 Все модули инспектора проекта
      * 9.2.2 Все используемые модули проекта
    * 9.3 Файлы .lpr, .lpi и .lps проекта
    * 9.4 Имя исполняемого / целевого файла проекта
    * 9.5 Добавление своего собственного типа проекта
    * 9.6 Добавление своих собственных типов файлов
      * 9.6.1 Добавление нового типа файла
      * 9.6.2 Добавление нового типа формы
    * 9.7 Сохранение пользовательских значений в [файлах] lpi/lps
  * 10 Пакеты
    * 10.1 Поиск во всех пакетах
    * 10.2 Поиск в пакетах с [определенным] именем
    * 10.3 Получение имени файла lpk установленного пакета
    * 10.4 Установка пакетов
    * 10.5 Открытие файла пакета (lpk)
    * 10.6 Поиск пакета(-ов) модуля
  * 11 Окна
    * 11.1 Добавление нового закрепляемого окна IDE
      * 11.1.1 Как зарегистрировать закрепляемое окно IDE
      * 11.1.2 Отображение окна IDE
      * 11.1.3 Заметки о IDEWindowCreators и SimpleLayoutStorage
  * 12 CodeTools
    * 12.1 Добавление директивы ресурса в файл
    * 12.2 Получение путей поиска для модулей и включаемых файлов
  * 13 Редактор исходного кода
    * 13.1 Активный редактор исходного кода
  * 14 SynEdit
    * 14.1 Получение настроек для TSynEdit
    * 14.2 Получение настроек для подсветки SynEdit
  * 15 Справка
    * 15.1 Добавление справки для исходников
    * 15.2 Добавление строк в окно сообщений
  * 16 Окно настроек IDE
    * 16.1 Открытие параметров в определенном кадре
  * 17 Прочее
    * 17.1 Добавление макроса
    * 17.2 Отладка IDE
    * 17.3 См. также



  


# Расширение IDE

## Обзор

Lazarus IDE поддерживает несколько типов плагинов: 

Компоненты
    это элементы, найденные в палитре компонентов. Например, TButton (находится на странице Standard) можно использовать для создания кнопок.

Редакторы компонентов
    при двойном щелчке по компоненту в Дизайнере вызывается редактор компонентов. Редактор компонента также может добавить дополнительный элемент во всплывающее меню Дизайнера (доступ к нему осуществляется по щелчку правой кнопкой мыши на компоненте в конструкторе).

Редакторы свойств
    Каждая строка в разделе сетки Инспектора объектов использует редактор свойств, чтобы разрешить установку или изменение значения этого свойства.

Эксперты
    Эксперты охватывают все другие типы плагинов. Они могут регистрировать новые пункты меню, регистрировать ярлыки или расширять другие функции IDE.

Есть два возможных способа добавить свои собственные плагины в IDE Lazarus: 

  1. Напишите пакет, установите его и зарегистрируйте ваши плагины в процедуре 'Register' модуля.
  2. Расширьте код Lazarus и отправьте свой svn diff в список рассылки Lazarus.



## Пакеты и зависимости Makefile

Следующие шаги необходимы, если добавляются новые зависимости пакета. 

  * Создание/Обновление Makefile для пакета через Package editor / More ... / Create Makefile.
  * Исправление всех зависимостей. См. tools/updatemakefiles.lpi



# Пункты меню

Вы можете добавлять новые пункты меню, скрывать или перемещать существующие и добавлять подменю, разделы. Для ваших собственных форм вы можете зарегистрировать TMainMenu/TPopupMenu, чтобы другие пакеты могли расширять вашу форму дальше. Модуль **MenuIntf** пакета IDEIntf содержит всю регистрацию для меню и множество стандартных пунктов меню самой IDE. 

## Добавление пункта меню

Одиночный элемент меню, такой как [, например,] 'View Object Inspector', называется **TIDEMenuCommand**. Вы можете создать его с помощью **RegisterIDEMenuCommand** , который имеет две формы с большим количеством параметров: 
    
    
    function RegisterIDEMenuCommand(Parent: TIDEMenuSection;
                                    const Name, Caption: string;
                                    const OnClickMethod: TNotifyEvent = nil;
                                    const OnClickProc: TNotifyProcedure = nil;
                                    const Command: TIDECommand = nil;
                                    const ResourceName: String = ''
                                    ): TIDEMenuCommand; overload;
    function RegisterIDEMenuCommand(const Path, Name, Caption: string;
                                    const OnClickMethod: TNotifyEvent = nil;
                                    const OnClickProc: TNotifyProcedure = nil;
                                    const Command: TIDECommand = nil;
                                    const ResourceName: String = ''
                                    ): TIDEMenuCommand; overload;
    

Разница между этими двумя [функциями] только в том, как вы указываете родительский раздел меню. Вы можете указать раздел меню прямо или косвенно через путь. Многие стандартные разделы можно найти в модуле _MenuIntf_. Например, раздел **mnuTools** , который является [пунктом] меню _Tools_ главной панели IDE. Он содержит раздел **itmSecondaryTools** , рекомендуемый для сторонних инструментов. 

Следующий [код] регистрирует новую команду меню с именем 'MyTool', с заголовком 'Start my tool' и при нажатии выполняет процедуру _StartMyTool_ : 
    
    
    procedure StartMyTool;
    begin
      ... выполняется при нажатии на пункт меню ...
    end;
    
    procedure Register;
    begin
      RegisterIDEMenuCommand(itmSecondaryTools, 'MyTool','Start my tool',nil,@StartMyTool);
    end;
    

Если вы хотите вызвать метод вместо процедуры, используйте параметр **OnClickMethod**. 

Этот пункт меню не имеет сочетания клавиш. Если вы хотите [назначить ему] сочетание клавиш, вы должны создать **TIDECommand** и передать его в параметре **Command**. Например: 
    
    
    uses ... IDECommands, MenuIntf;
    ...
    var
      Key: TIDEShortCut;
      Cat: TIDECommandCategory;
      CmdMyTool: TIDECommand;
    begin
      // регистрация сочетаний клавиш IDE и пункта меню
      Key := IDEShortCut(VK_UNKNOWN,[],VK_UNKNOWN,[]);
      Cat:=IDECommandList.FindCategoryByName(CommandCategoryToolMenuName);
      CmdMyTool := RegisterIDECommand(Cat,'Start my tool', 'Starts my tool to do some stuff', Key, nil, @StartMyTool);
      RegisterIDEMenuCommand(itmSecondaryTools, 'MyTool', 'Start my tool', nil, nil, CmdMyTool);
    end;
    

# Сочетания клавиш

Все сочетания клавиш (ключи, которые вызывают какое-либо действие) зарегистрированы в IDECommandList. Вот пример того, как найти все команды, которые реагируют на клавиши `Ctrl`+`D` или две комбинации клавиш, начинающиеся с `Ctrl`+`D`: 
    
    
    uses
      Classes, SysUtils, LCLProc, IDECommands;
    
    procedure ListCtrlD;
    // список всех команд IDE с комбинациями клавиш <Ctrl>+<D>
    var
      i: integer;
      Cmd: TIDECommand;
      Commands: TFPList;
    begin
      Commands:=IDECommandList.FindCommandsByShortCut(IDEShortCut(VK_D,[],VK_UNKNOWN,[]));
      try
        for i:=0 to Commands.Count-1 do begin
          Cmd:=TIDECommand(Commands[i]);
          writeln('Cmd: ',Cmd.Name,
            ' A=',dbgs(Cmd.ShortcutA.Shift1),'-',Cmd.ShortcutA.Key1,
            ',',dbgs(Cmd.ShortcutA.Shift2),'-',Cmd.ShortcutA.Key2,
            ' B=',dbgs(Cmd.ShortcutB.Shift1),'-',Cmd.ShortcutB.Key1,
            ',',dbgs(Cmd.ShortcutB.Shift2),'-',Cmd.ShortcutB.Key2
            );
        end;
      finally
        Commands.Free;
      end;
    end;
    

# Конфигурационные файлы

## Загрузка/Сохранение настроек

Все конфигурационные файлы IDE хранятся в одном каталоге в виде XML-файлов. Пакеты также могут добавлять свои собственные файлы туда. Каталог [primary config path](<../en/pcp.md> "pcp") можно прочитать с помощью: 
    
    
    uses LazIDEIntf;
    ...
      Directory := LazarusIDE.GetPrimaryConfigPath;
    

Пакеты могут создавать свои собственные файлы XML с помощью: 
    
    
    uses
      ..., LCLProc, BaseIDEIntf, LazConfigStorage;
    
    const
      Version = 1;
    var
      Config: TConfigStorage;
      SomeValue: String;
      SomeInteger: Integer;
      SomeBoolean: Boolean;
      FileVersion: LongInt;
    begin
      SomeValue:='Default';
      SomeInteger:=3;
      SomeBoolean:=true;
    
      // сохранение настроек
      try
        Config:=GetIDEConfigStorage('mysettings.xml',false);
        try
          // сохраняем номер версии, чтобы будущие расширения могли обрабатывать старые файлы конфигурации
          Config.SetDeleteValue('Path/To/The/Version',Version,0);
          // сохраняем строковую переменную "SomeValue"
          // если SomeValue имеет значение по умолчанию, запись не сохраняется,
          // поэтому сохраняются только [значения,] отличающиеся от значения по умолчанию.
          // Таким образом, xml остается коротким, и значения по умолчанию могут измениться в будущем.
          Config.SetDeleteValue('Path/To/Some/Value',SomeValue,'Default');
          Config.SetDeleteValue('Path/To/Some/Integer',SomeInteger,3);
          Config.SetDeleteValue('Path/To/Some/Boolean',SomeBoolean,true);
          // далее [по коду] еще overloads-инструкции для SetDeleteValue, 
          // которые [всплывают в редакторе кода по нажатию] <Ctrl>+<Space> 
        finally
          Config.Free;
        end;
    
      except
        on E: Exception do begin
          DebugLn(['Saving mysettings.xml failed: ',E.Message]);
        end;
      end;
    
      // загрузка настроек
      try
        Config:=GetIDEConfigStorage('mysettings.xml',true);
        try
          // чтение версии конфигурации
          FileVersion:=Config.GetValue('Path/To/The/Version',0);
          // чтение строковой переменной "SomeValue". Если запись не существует, используется
          // значение по умолчанию
          SomeValue:=Config.GetValue('Path/To/Some/Value','Default');
          SomeInteger:=Config.GetValue('Path/To/Some/Integer',3);
          SomeBoolean:=Config.GetValue('Path/To/Some/Boolean',true);
        finally
          Config.Free;
        end;
      except
        on E: Exception do begin
          DebugLn(['Loading mysettings.xml failed: ',E.Message]);
        end;
      end;
    end;
    

На заметку: 

  * Каждый плагин IDE должен использовать свой собственный файл конфигурации. Особенно вы не должны использовать стандартные файлы опций IDE, такие как [editoroptions.xml](</index.php?title=editoroptions.xml&action=edit&redlink=1> "editoroptions.xml \(page does not exist\)") или [environmentmentoptions.xml](</index.php?title=environmentmentoptions.xml&action=edit&redlink=1> "environmentmentoptions.xml \(page does not exist\)").
  * GetIDEConfigStorage при загрузке проверяет, существует ли файл и, если нет, копирует шаблон из вторичного каталога конфигурации.
  * Начиная с версии 0.9.31 вы можете использовать эту функцию для загрузки файла вне основного каталога конфигурации.



# Компоненты

## Расширение существующих компонентов

Источник: [[1]](<http://lazarus.freepascal.org/index.php/topic,17733.msg98425.html#msg98425>) Это обычная настройка существующих компонентов LCL, например, если вы хотите добавить свойства. 

Добавьте необходимые свойства с соответствующими методами установки и получения и полями хранения. 

Затем создайте их экземпляр «вручную», предоставив компонент-владелец (для последующей обработки), и установите [свойство] Parent и, возможно, [свойства] Top и Left и любые другие значения по умолчанию, необходимые для вашего компонента при инициализации. 

Когда вы убедитесь, что ваш настроенный компонент отлажен, добавьте вызов `RegisterComponent()`, чтобы прикрепить его к палитре компонентов IDE и избавьте вас от необходимости выполнять код установки вручную (посмотрите на исходник любого компонента, чтобы узнать, как это сделать). 

Вот простой пример: 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, StdCtrls;
    
    type
      TEdX = class(TEdit)
      private
        FNewIntProp: integer;
        FNewStrProp: string;
        procedure SetNewIntProp(AValue: integer);
        procedure SetNewStrProp(AValue: string);
      public
        constructor Create(theComponent: TComponent); override;
        property NewIntProp: integer read FNewIntProp write SetNewIntProp;
        property NewStrProp: string read FNewStrProp write SetNewStrProp;
      end;
    
      TForm1 = class(TForm)
        procedure FormCreate(Sender: TObject);
      private
        edX: TEdX;
      public
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      edX:= TEdX.Create(Self);
      edX.Parent:= Self;
      edX.Caption:= IntToStr(edX.NewIntProp);
      Caption:= edX.NewStrProp;
    end;
    
    {$R *.lfm}
    
    { TEdX }
    
    procedure TEdX.SetNewIntProp(AValue: integer);
    begin
      if FNewIntProp=AValue then Exit;
      FNewIntProp:=AValue;
      // добавьте свой код здесь
    end;
    
    procedure TEdX.SetNewStrProp(AValue: string);
    begin
      if FNewStrProp=AValue then Exit;
      FNewStrProp:=AValue;
      // добавьте свой код здесь
    end;
    
    constructor TEdX.Create(theComponent: TComponent);
    begin
      inherited Create(theComponent);
      FNewStrProp:= 'new string property value';
      FNewIntProp:= 99;
    end;
    
    end.
    

## Написание компонент

_Основная статья:[How To Write Lazarus Component](<How_To_Write_Lazarus_Component.md> "How To Write Lazarus Component/ru")_

Вы можете создавать новые компоненты через редактор пакетов. 

Создайте или откройте пакет, нажмите **add**(добавить), нажмите **New Component** (Новый компонент), заполните пункты: Ancestor Type(Тип предка) = TButton, New class name (Имя нового класса) = TMyButton, palette page (страница палитры) = Misc, Unit file name (имя файла модуля) = mybutton.pas (этот файл будет создан), имя модуля MyButton, и нажмите кнопку **ok**. 

### Предоставление новому компоненту иконки для палитры компонентов

Создайте файл изображения в формате .bmp, .xpm или .png с тем же именем, что и класс компонента. Например, _tmybutton.png_ и сохраните его в исходном каталоге пакета. Изображение может быть создано любой графической программой (например, GIMP [или IncScape]) и должно быть не больше 24x24 пикселей. Затем преобразуйте изображение в файл ресурсов с помощью инструмента lazres, который можно найти в каталоге lazarus/tools: 
    
    
    ~/lazarus/tools/lazres mybutton.lrs tmybutton.png
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Лаззарус trunk/1.4+? использует файлы ресурсов .res взамен .lrs-файлов
    
    
    ~/lazarus/tools/lazres mybutton.res tmybutton.png
    

Это создаст include-файл паскаля, который используется в разделе initialization mybutton.pas: 
    
    
    initialization
      {$I mybutton.lrs}
      // или в более новой версии Lazarus - синтаксис docheck
      {$R mybutton.res}
    

Установите пакет. 

### Скрытие компонента в палитре компонентов

Пакет IDEIntf, модуль componentreg: 
    
    
    IDEComponentPalette.FindComponent('TButton').Visible:=false;
    

## Написание редактора компонента

Редактор компонента обрабатывает такие вещи, как двойной щелчок по компоненту в конструкторе или добавление пунктов меню при щелчке правой кнопкой мыши по компоненту. Написать редактор компонентов легко. Посмотрите модуль componententeditors.pas пакета IDEIntf для [просмотра] кучи примеров. Например, чтобы показать редактор при двойном щелчке, см. TCheckListBoxComponentEditor. 

## Написание редакторов свойств

Каждый тип свойства (integer, string, TFont, ...) в Инспекторе объектов нуждается в редакторе свойств. Если у вашего компонента есть свойство _Password_ типа string, вы можете определить редактор свойств для вашего конкретного класса компонента со именем свойства _Password_ типа string. Модуль propedits.pp пакета IDEIntf содержит многие стандартные редакторы свойств, используемые самой IDE. Например, TStringPropertyEditor является редактором свойств по умолчанию для всех строк, в то время как TComponentNamePropertyEditor является более конкретным и обрабатывает только TComponent.Name. Смотрите [исходный код] модуля для [получения] множества примеров. 

### Сокрытие свойства в инспекторе объектов

Существует специальный редактор свойств для сокрытия [свойств]: 
    
    
      RegisterPropertyEditor(TypeInfo(TAnchorSide), TControl, 'AnchorSideLeft', THiddenPropertyEditor);
    

### Поиск редактора свойств, используемого для свойства
    
    
    uses Classes, TypInfo;
    var
      Instance: TPersistent; // ваш экземпляр
      PropInfo: PPropInfo;
      EdClass: TPropertyEditorClass;
    begin
      PropInfo:=GetPropInfo(Instance.ClassType,'Name'); // замените Name именем свойства, которое вы ищете
      GetEditorClass(PropInfo, Instance);
    end;
    

# Формы

IDE позволяет легко создавать потомков форм и фреймов. Эти формы могут иметь новые published-компоненты и новые published-методы. Но, когда вы добавляете новые published-свойства, они не доступны через инспектор объектов. Они работают только во время выполнения [приложения]. Методы в IDE являются поддельными, они не могут быть выполнены. В среде IDE можно создавать компоненты, поскольку их классы зарегистрированы в палитре компонентов. Ваши новые свойства существуют только в вашем исходном коде. IDE пока не может их подделать. Вот почему вы должны зарегистрировать новый класс. Это означает [, что вы должны]: 

  * Создать пакет времени разработки (или расширить существующий)
  * Ваша форма должна быть в модуле этого пакета


    
    
    ...
    interface
    
    uses FormEditingIntf, Forms;
    
    type
      TYourForm = class(TForm)
      private
        FYourProperty: integer;
      published
        property YourProperty: integer read FYourProperty write FYourProperty;
      end;
    
    procedure Register;
    
    implementation
    
    procedure Register;
    begin
     FormEditingHook.RegisterDesignerBaseClass(TYourForm);
    end;
    ...
    

  * Проверить [наличие процедуры] "Register" в редакторе пакетов для этого модуля.
  * Установить пакет в IDE



# Дизайнер

## Написание дизайнера-посредника

Стандартный конструктор позволяет визуально редактировать элементы управления LCL, тогда как все остальные отображаются в виде значков (например, TOpenDialog или TDataSource). Чтобы визуально редактировать элемент управления не [из библиотеки] LCL, вам нужно создать **designer mediator** (дизайнер-посредник). Он может быть использован для разработки веб-страниц, диаграмм UML или других наборов виджетов, таких как [fpGUI](<../en/fpGUI.md> "fpGUI"). Полный пример приведен в examples/designnonlcl/. 

  * Установите пакет с примерами examples/designnonlcl/notlcldesigner.lpk и перезапустите IDE. Это зарегистрирует дизайнер-посредник для компонентов TMyWidget и добавит новые компоненты TMyButton, TMyGroupBox в палитру компонентов.
  * Откройте проект с примерами examples/designnonlcl/project/NonLCL1.lpi.
  * Откройте unit1.pas и покажите дизайнер форм (F12). Вы должны увидеть компоненты в виде красных прямоугольников, которые можно выделять, перемещать и изменять их размер, как элементы управления LCL.



## Создание нового уникального имени компонента
    
    
    uses FormEditingIntf;
    
    ...
    
    NewName:=FormEditingHook.CreateUniqueComponentName(AComponent);
    
    // Или, если вам нужно создать имя перед созданием компонента:
    
    NewName:=FormEditingHook.CreateUniqueComponentName(ComponentClassName,OwnerComponent);
    // ComponentClassName будет использоваться в качестве префикса без начального T.
    // OwnerComponent является новым владельцем нового компонента.
    

## Выбор компонента в конструкторе/инспекторе объектов
    
    
    uses propedits;
    ..
    GlobalDesignHook.SelectOnlyThis(AComponent);
    

## Получение файла модуля, дизайнера формы
    
    
    uses ProjectIntf, LazIDEIntf, ComponentEditors;
    ...
    // открываем файл
    if LazarusIDE.DoOpenEditorFile(Filename,-1,-1,[ofAddToRecent])<>mrOk then exit;
    // получаем интерфейс файла
    aFile:=LazarusIDE.ActiveProject.FindFile(Filename,[]);
    // получаем дизайнер
    aDesigner:=TComponentEditorDesigner(LazarusIDE.GetDesignerWithProjectFile(AFile,true));
    if aDesigner=nil then 
      exit; // этот модуль не имеет ресурса (форма, фрейм, модуль данных и т.д.)
    // получаем форму
    aForm:=aDesigner.Form;
    

## Сохранение модуля, дизайнера формы
    
    
    uses LazIDEIntf;
    ...
    if LazarusIDE.DoSaveEditorFile(Filename,[])<>mrOk then exit;
    

## Сохранение формы дизайнера как кода Паскаль

  * Используйте функцию **FormEditingHook.SaveComponentAsPascal** модуля _FormEditingfIntf_. См. _examples/pascalstream/CopyAsPasPkg/copyaspasdemounit1.pas_.



# Обработчики событий

В IDE есть несколько событий, для которых плагины могут добавлять свои собственные обработчики. 

## События дизайнера

В propedits.pp есть объект "GlobalDesignHook", который поддерживает несколько событий для проектирования. Каждое событие вызывает список обработчиков. Обработчики по умолчанию добавляются в среду IDE. Вы можете добавить свои собственные обработчики с помощью методов AddHandlerXXX и RemoveHandlerXXX. Они будут вызваны перед обработчиками по умолчанию. 

Примеры: 

  * Добавление вашего обработчика (это обычно делается в конструкторе вашего объекта):


    
    
     GlobalDesignHook.AddHandlerPersistentAdded(@YourOnPersistentAdded);
    

  * Удаление вашего обработчика:


    
    
     GlobalDesignHook.RemoveHandlerPersistentAdded(@YourOnPersistentAdded);
    

  * Вы можете удалить все обработчики одновременно. Например, это хорошая идея - добавить эту строку в деструктор вашего объекта:


    
    
     GlobalDesignHook.RemoveAllHandlersForObject(Self);
    

Обработчики GlobalDesignHook: 

  * Поиск корня 
    * ChangeLookupRoot - Вызывается при изменении "LookupRoot". "LookupRoot" является объектом-владельцем выбранных компонентов. Обычно это TForm.
  * Методы 
    * CreateMethod
    * GetMethodName
    * GetMethods
    * MethodExists
    * RenameMethod
    * ShowMethod
    * MethodFromAncestor
    * ChainCall
  * Компоненты 
    * GetComponent
    * GetComponentName
    * GetComponentNames
    * GetRootClassName
    * AddClicked - Вызывается, когда пользователь выбирает класс компонента и щелкает дизайнер, чтобы добавить новый компонент. Вы можете изменить класс компонента и родительский элемент.
    * ComponentRenamed - Вызывается, когда компонент был переименован
  * Объекты TPersistent 
    * PersistentAdded - Вызывается при добавлении нового TPersistent в LookupRoot
    * PersistentDeleting - Вызывается до освобождения TPersistent.
    * DeletePersistent - Вызывается IDE для удаления TPersistent.
    * GetSelectedPersistents - Вызывается при получении текущего выбора TPersistent.
    * SetSelectedPersistents - Вызывается при настройке текущего выбора TPersistent.
    * GetObject
    * GetObjectName
    * GetObjectNames
  * Изменение 
    * Modified
    * Revert
    * RefreshPropertyValues
  * Выбор 
    * SetSelection
    * GetSelection



### Примеры для событий дизайнера

#### Пример: когда [на родителя] помещается новый компонент, перемещать элемент управления в дочерний элемент
    
    
    ...
    type
      TYourDesignerExtension = class
      private
        function AddClicked(ADesigner: TIDesigner;
                 MouseDownComponent: TComponent; Button: TMouseButton;
                 Shift: TShiftState; X, Y: Integer;
                 var AComponentClass: TComponentClass;
                 var NewParent: TComponent): boolean of object;
      public
        constructor Create;
        destructor Destroy; override;
      end;
    
    ...
    constructor TYourDesignerExtension.Create;
    begin
      // регистрация вашего обработчика
      GlobalDesignHook.AddHandlerAddClicked(@AddClicked);
    end;
    
    destructor TYourDesignerExtension.Destroy;
    begin
      // удаление вашего обработчика
      GlobalDesignHook.RemoveHandlerAddClicked(@AddClicked);
    end;
    
    function TYourDesignerExtension.AddClicked(ADesigner: TIDesigner;
                 MouseDownComponent: TComponent; Button: TMouseButton;
                 Shift: TShiftState; X, Y: Integer;
                 var AComponentClass: TComponentClass;
                 var NewParent: TComponent): boolean of object;
    begin
      // в этом примере TYourControl - это пользовательский элемент управления с ChildControl.
      // Всякий раз, когда пользователь помещает элемент управления на TYourControl, вместо добавления
      // нового элемента управления на TYourControl, [этот] новый элемент управления добавляется как дочерний элемент
      // [имеющегося] ChildControl.
      // создание 
      if MouseDownComponent is TYourControl then
        NewParent:=TYourControl(MouseDownComponent).ChildControl;
      Result:=true;
    end;
    

### Отключение обработчика мыши в [режиме] дизайнера

Обычно дизайнер ловит все события мыши компонента. Если ваш пользовательский элемент управления должен обрабатывать дескриптор событий мыши, вы можете либо установить **csDesignInteractive** **ControlStyle** , либо вы можете точно управлять им через сообщение _CM_DESIGNHITTEST_ в вашем элементе управления: 
    
    
    type
      TYourControl = class(TCustomControl)
      protected
        procedure CMDesignHitTest(var Message: TLMessage); message CM_DESIGNHITTEST;
      end;
    ...
    procedure TYourControl.CMDesignHitTest(var Message: TLMessage);
    var
      p: TSmallPoint;
      aShiftState: TShiftState;
    begin
      aShiftState:=KeysToShiftState(PtrUInt(Message.WParam));
      p:=TSmallPoint(longint(Message.LParam));
      debugln(['TForm1.CMDesignHitTest ShiftState=',dbgs(aShiftState),' x=',p.x,' y=',p.y]);
      if SkipDesignerMouseHandler then
        Message.Result:=1; // теперь дизайнер вызывает обычные методы MouseUp, MouseMove и MouseDown.
    end;
    

## Получение уведомлений при изменении дизайнера форм

Каждая разработанная форма LCL имеет конструктор типа TIDesigner. В среде IDE создаются конструкторы типа TComponentEditorDesigner, определенные в компонентных компонентах модуля IDEIntf. Например: 
    
    
    procedure TYourAddOn.OnDesignerModified(Sender: TObject);
    var
      IDEDesigner: TComponentEditorDesigner;
    begin
      IDEDesigner:=TComponentEditorDesigner(Sender);
      ...
    end;
    
    procedure TYourAddOn.ConnectDesignerForm(Form1: TCustomForm);
    var
      IDEDesigner: TComponentEditorDesigner;
    begin
      IDEDesigner:=TComponentEditorDesigner(Form1.Designer);
      IDEDesigner.AddHandlerModified(@OnDesignerModified);
    end;
    

## События проекта

Эти события определены в модуле LazIDEIntf. 

  * **LazarusIDE.AddHandlerOnProjectClose** : вызывается перед закрытием проекта
  * **LazarusIDE.AddHandlerOnProjectOpened** : вызывается после полного открытия проекта (например, все необходимые пакеты были загружены, модули были открыты в редакторе исходного кода)
  * **LazarusIDE.AddHandlerOnSavingAll** : вызывается перед тем, как IDE сохраняет все
  * **LazarusIDE.AddHandlerOnSavedAll** : вызывается после того, как IDE сохранил все
  * **LazarusIDE.AddHandlerOnProjectBuilding** : вызывается перед тем, как IDE создаст проект
  * **LazarusIDE.AddHandlerOnProjectDependenciesCompiling** : вызывается перед тем, как IDE компилирует пакетные зависимости проекта
  * **LazarusIDE.AddHandlerOnProjectDependenciesCompiled** : вызывается после компиляции зависимостей пакета IDE проекта
  * **LazarusIDE.AddHandlerOnProjectBuildingFinished** : вызывается после того, как IDE собрал проект (успешно или нет) (начиная с 1.5)



## Другие события IDE

**uses** _LazIDEIntf;_

Вызовите их в процедуре регистрации вашего пакета: 

  * **LazarusIDE.AddHandlerOnIDERestoreWindows** : вызывается, когда IDE восстанавливает свои окна (перед открытием первого проекта)
  * **LazarusIDE.AddHandlerOnIDEClose** : вызывается, когда IDE закрывается (после запроса закрытия, поэтому больше нет интерактивности)
  * **LazarusIDE.AddHandlerOnQuickSyntaxCheck** : вызывается при выполнении пункта меню или ярлыка для быстрой проверки синтаксиса
  * **LazarusIDE.AddHandlerOnLazarusBuilding** : вызывается перед тем, как IDE собирает Lazarus IDE (начиная с 1.5)
  * **LazarusIDE.AddHandlerOnLazarusBuildingFinished** : вызывается после того, как IDE собирает Lazarus IDE (начиная с 1.5)
  * **LazarusIDE.AddHandlerGetFPCFrontEndParams** : вызывается, когда IDE получает параметры клиентского инструмента интерфейса 'fpc' (начиная с 1.5)
  * **LazarusIDE.AddHandlerGetFPCFrontEndPath** : вызывается, когда IDE получает параметры клиентского инструмента интерфейса 'fpc' (начиная с 1.5)
  * **LazarusIDE.AddHandlerOnUpdateIDEComponentPalette** : ?
  * **LazarusIDE.AddHandlerOnUpdateComponentPageControl** : ?
  * **LazarusIDE.AddHandlerOnShowDesignerFormOfSource** : вызывается после показа конструктора формы для редактора кода (AEditor может быть nil!) (начиная с 1.5)
  * **LazarusIDE.AddHandlerOnShowSourceOfActiveDesignerForm** : вызывается после показа кода формы дизайнера (начиная с 1.5)
  * **LazarusIDE.AddHandlerOnChangeToolStatus** : вызывается, когда IDEToolStatus изменяется (например, itNone-> itBuilder и т. д.) (начиная с 1.5)



Вызовите их в разделе инициализации одного из модулей: 

  * **AddBootHandler**
    * AddBootHandler(**libhTransferMacrosCreated** ,@YourProc): вызывается после создания IDEMacros (начиная с версии 1.3)
    * AddBootHandler(**libhEnvironmentOptionsLoaded** ,@YourProc): вызывается после загрузки параметров среды (начиная с версии 1.3)



# Проект

## Текущий проект

Текущий основной проект может быть получен [при помощи] LazarusIDE.ActiveProject. (модуль LazIDEIntf) 

## Все модули текущего проекта

Обратите внимание, что термин «юниты проекта» неоднозначен. 

  * Инспектор проектов показывает список файлов .lpi. Проект может не использовать все эти модули на всех платформах, и, возможно, разработчик забыл добавить все используемые модули.
  * Модули из использованных пакетов не являются модулями проекта.



### Все модули инспектора проекта

Чтобы перебрать все паскалевские модули, перечисленные в инспекторе проектов, вы можете использовать, например: 
    
    
    uses 
      LCLProc, FileUtil, LazIDEIntf, ProjectIntf;
    
    procedure ListProjectUnits;
    var
      LazProject: TLazProject;
      i: Integer;
      LazFile: TLazProjectFile;
    begin
      LazProject:=LazarusIDE.ActiveProject;
      if LazProject<>nil then
        for i:=0 to LazProject.FileCount-1 do
        begin
          LazFile:=LazProject.Files[i];
          if LazFile.IsPartOfProject
          and FilenameIsPascalUnit(LazFile.Filename)
          then
            debugln(LazFile.Filename);
        end;
    end;
    

### Все используемые модули проекта

Чтобы найти все используемые в настоящее время модули проекта/пакета, вы можете использовать следующее. Обратите внимание, что в среде IDE необходимо проанализировать все модули, поэтому это может занять некоторое время. 
    
    
    uses 
      LazLoggerBase, LazIDEIntf, ProjectIntf;
    
    procedure ListProjectUnits;
    var
      LazProject: TLazProject;
      i: Integer;
    begin
      LazProject:=LazarusIDE.ActiveProject;
      if LazProject<>nil then 
      begin
        Units:=LazarusIDE.FindUnitsOfOwner(LazProject,[fuooListed,fuooUsed]); // добавьте fuooPackages, чтобы включить модули из пакетов
        try
          for i:=0 to Units.Count-1 do
            debugln('Filename=',Units[i]);
        finally
          Units.Free;
        end;
      end;
    end;
    

## Файлы .lpr, .lpi и .lps проекта
    
    
    uses 
      LCLProc, FileUtil, ProjectIntf, LazIDEIntf;
    var
      LazProject: TLazProject;
    begin
      LazProject:=LazarusIDE.ActiveProject;
      // у каждого проекта есть файл .lpi:
      DebugLn(['Файл проекта lpi: ',LazProject.ProjectInfoFile]);
    
      // если информация о сеансе проекта хранится в отдельном файле .lps:
      if LazProject.SessionStorage<>pssNone then
        DebugLn(['Файл проекта lps: ',LazProject.ProjectSessionFile]);
    
      // Если у проекта есть файл .lpr, то он является основным исходным файлом:
      if (LazProject.MainFile<>nil)
      and (CompareFileExt(LazProject.MainFile.Filename,'lpr')=0) then
        DebugLn(['Проект имеет файл lpr: ',LazProject.MainFile.Filename]);
    end;
    

## Имя исполняемого / целевого файла проекта

Существует макрос $(TargetFile), который можно использовать в путях и внешних инструментах. Вы можете запросить макрос в коде: 
    
    
    uses MacroIntf;
    
    function MyGetProjectTargetFile: string;
    begin
      Result:='$(TargetFile)';
      if not IDEMacros.SubstituteMacros(Result) then
        raise Exception.Create('невозможно получить целевой файл проекта');
    end;
    

См. здесь для более полной информации по макросам: [Макросы IDE в путях и именах файлов](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames/ru"). 

## Добавление своего собственного типа проекта

Вы можете добавлять [свои] пункты в диалоговое окно 'New ...': 

  * Смотрите модуль ProjectIntf пакета IDEIntf.
  * См. [Шаблоны проекта](<Project_Templates.md> "Project Templates/ru")



## Добавление своих собственных типов файлов

Вы можете добавлять [свои] пункты в диалоговое окно 'New ...': 

  * Смотрите модуль ProjectIntf пакета IDEIntf.
  * Выбрав базовый класс TFileDescPascalUnit для обычных модулей или TFileDescPascalUnitWithResource для новой формы/модуля данных.



### Добавление нового типа файла
    
    
    uses ProjectIntf;
    ...
      { TFileDescText }
    
      TFileDescMyText = class(TProjectFileDescriptor)
      public
        constructor Create; override;
        function GetLocalizedName: string; override;
        function GetLocalizedDescription: string; override;
      end;
    ...
    
    procedure Register;
    
    implementation
    
    procedure Register;
    begin
      RegisterProjectFileDescriptor(TFileDescMyText.Create,FileDescGroupName);
    end;
    
    { TFileDescMyText }
    
    constructor TFileDescMyText.Create;
    begin
      inherited Create;
      Name:='MyText'; // не переводите эту [строку]
      DefaultFilename:='text.txt';
      AddToProject:=false;
    end;
    
    function TFileDescText.GetLocalizedName: string;
    begin
      Result:='My Text'; // замените это строкой из resourcestring
    end;
    
    function TFileDescText.GetLocalizedDescription: string;
    begin
      Result:='Пустой текстовый файл';
    end;
    

### Добавление нового типа формы
    
    
    uses ProjectIntf;
    
    ...
      TFileDescPascalUnitWithMyForm = class(TFileDescPascalUnitWithResource)
      public
        constructor Create; override;
        function GetInterfaceUsesSection: string; override;
        function GetLocalizedName: string; override;
        function GetLocalizedDescription: string; override;
      end;
    ...
    
    procedure Register;
    
    implementation
    
    procedure Register;
    begin
      RegisterProjectFileDescriptor(TFileDescPascalUnitWithMyForm.Create,FileDescGroupName);
    end;
    
    { TFileDescPascalUnitWithMyForm }
    
    constructor TFileDescPascalUnitWithMyForm.Create;
    begin
      inherited Create;
      Name:='MyForm'; // не переводите эту [строку]
      ResourceClass:=TMyForm;
      UseCreateFormStatements:=true;
    end;
    
    function TFileDescPascalUnitWithMyForm.GetInterfaceUsesSection: string;
    begin
      Result:='Classes, SysUtils, MyWidgetSet';
    end;
    
    function TFileDescPascalUnitWithForm.GetLocalizedName: string;
    begin
      Result:='MyForm'; // замените это строкой из resourcestring
    end;
    
    function TFileDescPascalUnitWithForm.GetLocalizedDescription: string;
    begin
      Result:='Создать новую MyForm из примера пакета NotLCLDesigner';
    end;
    

## Сохранение пользовательских значений в [файлах] lpi/lps

Вы можете хранить свои собственные данные с помощью пар имя/значение в CustomData и CustomSessionData. 
    
    
    LazarusIDE.ActiveProject.CustomData['Name']:=Value; // хранится в lpi - x-platform, общие данные
    LazarusIDE.ActiveProject.CustomSessionData['Name']:=Value; // хранится в lpi - x-platform, общие данные
    

# Пакеты

Вы можете выполнять различные действия с [пакетами](<Lazarus_Packages.md> "Lazarus Packages/ru"): 

## Поиск во всех пакетах

Переберите все пакеты, загруженные в IDE (начиная с 0.9.29). 
    
    
    uses PackageIntf;
    ...
    for i:=0 to PackageEditingInterface.GetPackageCount-1 do
      writeln(PackageEditingInterface.GetPackages(i).Name);
    

## Поиск в пакетах с [определенным] именем
    
    
    uses PackageIntf;
    ...
    var
      Pkg: TIDEPackage;
    begin
      Pkg:=PackageEditingInterface.FindPackageWithName('LCL');
      if Pkg<>nil then 
        ...
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** FindPackageWithName не открывает редактор пакетов. Для этого используйте DoOpenPackageWithName.

## Получение имени файла lpk установленного пакета
    
    
    uses PackageIntf;
    ...
    var
      Pkg: TIDEPackage;
    begin
      Pkg:=PackageEditingInterface.FindPackageWithName('LCL');
      if Pkg<>nil then 
        LPKFilename:=Pkg.Filename;
    end;
    

## Установка пакетов

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Устанавливайте только пакеты с плагинами IDE. Установка других пакетов может привести к нестабильной работе среды IDE.
    
    
    uses PackageIntf, contnrs;
    ...
      PkgList:=TObjectList.create(true);
      try
        Pkg:=TLazPackageID.Create;
        Pkg.Name:='Cody';
        PkgList.Add(Pkg);
        // проверьте, может ли IDE найти cody.lpk и все зависимости
        // Среда IDE покажет некоторые предупреждения/подтверждения, если что-то выглядит странно.
        if not PackageEditingInterface.CheckInstallPackageList(PkgList,[]) then
          exit;
        // в этом примере мы [это] уже проверили, поэтому пропустите предупреждения
        // и пересоберите IDE
        if PackageEditingInterface.InstallPackages(PkgList,[piiifSkipChecks,piiifRebuildIDE])<>mrOK then
          exit;
      finally
        PkgList.Free;
      end;
    

## Открытие файла пакета (lpk)
    
    
    uses PackageIntf, FileUtil;
    ...
    var
      pkg: TIDEPackage;
    begin
      if PackageEditingInterface.DoOpenPackageFile(LPKFilename,[pofAddToRecent],false)<>mrOk then
        exit;
      Pkg:=PackageEditingInterface.FindPackageWithName(ExtractFilenameOnly(LPKFilename));
      ...
    end;
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Среда IDE автоматически закрывает неиспользуемые пакеты в режиме ожидания. Никогда не храните ссылку на TIDEPackage.

## Поиск пакета(-ов) модуля

У вас есть имя файла модуля, например '/home/user/unit.pas', и [если] вы хотите знать, какому пакету(-ам) и проекту(-ам) он принадлежит, используйте это: 
    
    
    uses Classes, PackageIntf, ProjectIntf;
    ...
    var
      Owners: TFPList;
      i: Integer;
      o: TObject;
    begin
      Owners:=PackageEditingInterface.GetPossibleOwnersOfUnit('/full/path/of/unit.pas',[]);
      if Owners=nil then begin
        // модуль не связан напрямую с проектом/пакетом
        // возможно модуль был по какой-то причине не добавлен, но доступен
        // поиск по всем путям модулей всех проектов/пакетов
        // Осторожно: это может привести к ложным попаданиям
        Owners:=PackageEditingInterface.GetPossibleOwnersOfUnit('/full/path/of/unit.pas',
          [piosfExcludeOwned,piosfIncludeSourceDirectories]);
      end;
      if Owners=nil then exit;
      try
        for i:=0 to Owners.Count-1 do begin
          o:=TObject(Owners[i]);
          if o is TIDEPackage then begin
            writeln('Owner is package ',TIDEPackage(o).Name);
          end else if o is TLazProject then begin
            writeln('Owner is project ',TLazProject(o).ProjectInfoFile);
          end;
        end;
      finally
        Owners.Free;
      end;
    

# Окна

Есть четыре основных типа окон IDE. 

  * основная панель IDE - это Application.MainForm. Оно всегда присутствует.
  * плавающие / закрепляемые окна, такие как Редактор исходного кода, Инспекторы объектов и Сообщения.
  * модальные формы, такие как диалог поиска, диалоги настроек и вопросы.
  * подсказки и заполнение форм



## Добавление нового закрепляемого окна IDE

Что такое закрепляемое окно IDE: Окна, такие как Редактор исходного кода или Инспектор объектов, являются плавающими окнами, которые можно закрепить, если установлен пакет закрепления, а его состояние, положение и размер сохраняются и восстанавливаются при следующем запуске IDE. Для восстановления окна IDE нужен создатель, как определено в модуле **IDEWindowIntf** пакета IDEIntf. Каждое закрепляемое окно должно иметь уникальное имя. Не используйте общие имена, такие как 'FileBrowser', потому что это будет легко конфликтовать с другими пакетами. И не используйте короткие имена, такие как 'XYZ', потому что создатель отвечает за все формы, начинающиеся с этого имени. 

### Как зарегистрировать закрепляемое окно IDE

Не забудьте выбрать длинное уникальное имя, которое является действительным идентификатором Паскаля. Ваше окно может иметь любой заголовок, который вы хотите. 
    
    
    uses SysUtils, IDEWindowIntf;
    ...
    var MyIDEWindow: TMyIDEWindow = nil; 
    
    procedure CreateMyIDEWindow(Sender: TObject; aFormName: string; var AForm: TCustomForm; DoDisableAutoSizing: boolean);
    begin
      // проверяем имя
      if CompareText(aFormName,MyIDEWindowName)<>0 then exit;
      // создаем форму, если это еще не сделано, и отключаем автомасштабирование
      IDEWindowCreators.CreateForm(MyIDEWindow,TMyIDEWindowm,DoDisableAutosizing,Application);
      ... инициируем окно ...
      AForm:=MyIDEWindow;
    end;
    
    procedure Register;
    begin
      IDEWindowCreators.Add('MyIDEWindow',@CreateMyIDEWindow,nil,'100','50%','+300','+20%');
      // стандартная граница формы - это:
      // Left=100, Top=50% от [величины] Screen.Height, Width=300, Height=20% от [величины] Screen.Height
      // когда IDE требуется экземпляр этого окна, она вызывает процедуру CreateMyIDEWindow.
    end;
    

### Отображение окна IDE

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Не используйте [процедуру] Show.

Используйте: 
    
    
    IDEWindowCreators.ShowForm(MyIDEWindow,false);
    

Это будет работать с [механизмом] пристыковки. Система пристыковки может обернуть форму в стыковочный узел. Параметр BringToFront указывает системе закрепления сделать форму и все ее родительские сайты видимыми и вывести сайт верхнего уровня вперед. 

### Заметки о IDEWindowCreators и SimpleLayoutStorage

IDEWindowCreators.SimpleLayoutStorage просто хранит BoundsRect и WindowState всех форм, которые когда-то были открыты. Он используется как запасной вариант, если не установлен док-мастер. Он сохраняет состояние, даже если установлен DockMaster, поэтому, когда dockmaster удален, границы форм восстанавливаются. 

IDEWindowCreators используется всеми присоединяемыми формами для регистрации и отображения форм. При показе формы Создатель проверяет, установлен ли IDEDockMaster, и передает ему отображение. Если IDEDockMaster не установлен, он просто показывает форму. IDEDockMaster может использовать информацию в IDEWindowCreators для создания форм по именам и получить представление о том, где разместить форму при ее первом отображении. Для более подробной информации смотрите пакеты AnchorDockingDsgn и EasyDockMgDsgn. 

# CodeTools

CodeTools - это пакет, предоставляющий огромное количество функций для анализа, поиска и изменения исходных текстов, а также некоторые основные функции для других языков. Вы можете использовать их также и без IDE. Рекомендуется сначала прочитать о CodeTools в целом, прежде чем использовать их в IDE: [Codetools](<../en/Codetools.md> "Codetools"). 

Перед вызовом любой из функций CodeTools в IDE вы должны зафиксировать текущие изменения редактора исходного кода в буферах CodeTools: 
    
    
    uses LazIDEIntf;
    ...
      // сохраняем изменения в редакторе исходного кода в codetools
      LazarusIDE.SaveSourceEditorChangesToCodeCache(-1); // -1: фиксируем все редакторы исходного кода
    

## Добавление директивы ресурса в файл

Это добавляет {$R example.res} к модулю паскаля: 
    
    
    procedure AddResourceDirectiveToPascalSource(const Filename: string);
    var
      ExpandedFilename: String;
      CodeBuf: TCodeBuffer;
    begin
      // убеждаемся, что имя файла обрезано [от концевых пробелов] и содержит полный путь
      ExpandedFilename:=CleanAndExpandFilename(Filename);
      
      // сохраняем изменения в редакторе исходного кода в codetools
      LazarusIDE.SaveSourceEditorChangesToCodeCache(-1);
    
      // загружаем файл
      CodeBuf:=CodeToolBoss.LoadFile(ExpandedFilename,true,false);
    
      // добавляем директиву ресурса
      if not CodeToolBoss.AddResourceDirective(CodeBuf,'example.res') then
        LazarusIDE.DoJumpToCodeToolBossError;
    end;
    

Codetools также предоставляет такие функции, как FindResourceDirective и RemoveDirective. 

## Получение путей поиска для модулей и включаемых файлов

В среде IDE существует множество различных путей поиска из проектов, пакетов, каталога fpc и lazarus, и существует множество типов путей: до или после разрешения макросов, с унаследованными путями поиска или без них, как относительными или абсолютными путями. Все файлы в каталоге имеют одинаковый набор путей поиска. Вы можете получить пути поиска для каждого каталога, полностью решенного путем запроса codetools: 
    
    
    uses CodeToolManager;
    ...
    
    Dir:=''; // пустой каталог для новых файлов и имеет те же настройки, что и каталог проекта
    
    // Получение путей поиска для включаемых файлов:
    Path:=CodeToolBoss.GetIncludePathForDirectory(Dir);
    
    // Получение путей поиска для модулей:
    // Этот путь поиска передается компилятору.
    // Он содержит выходные каталоги пакетов, но не каталоги с исходниками пакетов.
    Path:=CodeToolBoss.GetUnitPathForDirectory(Dir);
    
    // Могут быть дополнительные пути поиска модулей только для IDE (не передаются компилятору)
    Path:=CodeToolBoss.GetSrcPathForDirectory(Dir);
    
    // Полный путь поиска содержит также все исходные пути пакета для модулей:
    Path:=CodeToolBoss.GetCompleteSrcPathForDirectory(Dir);
    

# Редактор исходного кода

## Активный редактор исходного кода
    
    
    uses SrcEditorIntf;
    ...
    Editor:=SourceEditorManagerIntf.ActiveEditor;
    if Editor=nil then exit;
    Filename:=Editor.FileName;
    ScreenPos:=Editor.CursorScreenXY;
    TextPos:=Editor.CursorTextXY;
    

# SynEdit

## Получение настроек для TSynEdit

Если у вас есть диалог с использованием TSynEdit, и вы хотите использовать тот же шрифт и настройки, которые использует редактор исходного кода: 
    
    
    uses SrcEditorIntf;
    ...
    SourceEditorManagerIntf.GetEditorControlSettings(ASynEdit);
    

## Получение настроек для подсветки SynEdit

Если у вас есть диалог, использующий TSynEdit с подсветкой, и вы хотите использовать те же цвета, что и для подсветки редактора исходного кода для этого языка, используйте: 
    
    
    uses SrcEditorIntf;
    ...
    SourceEditorManagerIntf.GetHighlighterSettings(ASynHighlighter);
    

См. пример: TSQLStringsPropertyEditorDlg.Create in the unit [SQLStringsPropertyEditorDlg](<http://svn.freepascal.org/svn/lazarus/trunk/components/sqldb/sqlstringspropertyeditordlg.pas>). 

# Справка

## Добавление справки для исходников

Сначала создаем THelpDatabase: 
    
    
    HelpDB := TFPDocHTMLHelpDatabase(
      HelpDatabases.CreateHelpDatabase('ANameOfYourChoiceForTheDatabase',
      TFPDocHTMLHelpDatabase,true));
    HelpDB.DefaultBaseURL := 'http://your.help.org/';
    
    FPDocNode := THelpNode.CreateURL(HelpDB,
      'Package1 - A new package',
      'file://index.html');
    HelpDB.TOCNode := THelpNode.Create(HelpDB,FPDocNode);// один раз как оглавление TOC
    DirectoryItem := THelpDBISourceDirectory.Create(FPDocNode,'$(PkgDir)/lcl',
      '*.pp;*.pas',false);// и один раз как обычная страница
    HelpDB.RegisterItem(DirectoryItem);
    

## Добавление строк в окно сообщений
    
    
    unit IDEMsgIntf;
    ...
    var Dir: String;
    begin
      Dir:=GetCurrentDir;
      IDEMessagesWindow.BeginBlock;
      IDEMessagesWindow.AddMsg('unit1.pas(30,4) Error: Identifier not found "a"',Dir,0);
      IDEMessagesWindow.EndBlock;
    end;
    

# Окно настроек IDE

## Открытие параметров в определенном кадре
    
    
    uses LazIDEIntf;
    ...
    LazarusIDE.DoOpenIDEOptions(TYourIDEOptionsFrame);
    

# Прочее

## Добавление макроса

Вы можете добавить свои собственные макросы IDE: 
    
    
    uses MacroIntf, MacroDefIntf;
    
    procedure Register;
    begin
      // регистрация макроса с фиксированным значением:
      IDEMacros.Add(TTransferMacro.Create('MyMacro1','Value','Description',nil,[]));
    end;
    
    // регистрация макроса с динамическим значением требует метода объекта
    procedure TForm1.Init;
    begin
      IDEMacros.Add(TTransferMacro.Create('MyMacro','Value','Description',@OnResolveMyMacro,[]));
    end;
    
    function TForm1.OnResolveMyMacro(const s: string; const Data: PtrInt;
      var Abort: boolean): string;
    // s - это параметр. Например, $MyMacro(parameter)
    // Data  - это экземпляр запроса. Может быть nil.
    // Если есть ошибка, установите Abort в true.
    // Результирующая строка может содержать макросы. Например, Result:='$(FPCVer)/$(TargetOS)';
    begin
      Result:='MyValue';
    end;
    

## Отладка IDE

  * Скомпилируйте IDE как обычно, например, с помощью Tools/Build Lazarus. Убедитесь, что вы установили отладочные флаги, такие как -g или -gw2. Лучше всего использовать профиль "Debug IDE", который содержит много флагов, полезных для отладки.
  * Откройте проект ide\lazarus.lpi. Вы не можете напрямую скомпилировать этот проект.
  * Установите точки останова и пр. И запустите проект.



## См. также

  * [Creating IDE Help](<../en/Creating_IDE_Help.md> "Creating IDE Help")
  * [Help protocol](<../en/Help_protocol.md> "Help protocol")
  * [How To Write Lazarus Component](<../en/How_To_Write_Lazarus_Component.md> "How To Write Lazarus Component")

---

_Source: [https://wiki.freepascal.org/Extending_the_IDE/ru](https://web.archive.org/web/20250114102301/https://wiki.freepascal.org/Extending_the_IDE/ru)_
