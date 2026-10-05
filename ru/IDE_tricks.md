# IDE tricks

│ **[English (en)](<../en/IDE_tricks.md>)** │  **русский (ru)** │

## Contents

  * 1 Создание нового файла с помощью [функции] "Открыть файл"
  * 2 Настройка нового модуля / формы
  * 3 Макросы IDE
  * 4 Директивы IDE
  * 5 Получение параметров командной строки компилятора, созданных в среде IDE
  * 6 Closing all editor files except one
  * 7 Component palette
    * 7.1 Finding a component in the palette
    * 7.2 Open the package of a component in the palette
    * 7.3 Find the source declaration of a component in the palette
  * 8 My application freezes my linux desktop while debugging
    * 8.1 Using a second X session
    * 8.2 Using VNC
    * 8.3 Debug the application on the second server
  * 9 Compiling the IDE fast
  * 10 DebugLn of the IDE or a LCL application
  * 11 Finding the source file of an IDE window
  * 12 Opening a Terminal Windows from the IDE
  * 13 Object Inspector: Events: Frames: Jump to the source of an inherited event
  * 14 Events (Method properties) in the Object Inspector



# Создание нового файла с помощью [функции] "Открыть файл"

Вы можете создать новый файл и сохранить его, или вы можете создать новый файл с именем файла и типом файла за один шаг: Просто откройте файл (`ctrl`+`O`) и выберите несуществующий файл. Например: unit1.pas. IDE спросит вас, нужно ли его создавать. 

# Настройка нового модуля / формы

Начиная с версии 0.9.27 вы можете щелкнуть правой кнопкой мыши на кнопке быстрого запуска 'New unit' (New form) и установить тип файла, который должен быть создан. Вы можете зарегистрировать больше типов файлов через IDEIntf или пакет шаблонов проектов. 

# Макросы IDE

[Макросы IDE в путях и именах файлов](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames/ru")

# Директивы IDE

[IDE directives](<../en/IDE_directives.md> "IDE directives")

# Получение параметров командной строки компилятора, созданных в среде IDE

Вы можете скопировать параметры из Project -> Compiler Options -> Show Options. Здесь пути относятся к каталогу проекта. Таким образом, в большинстве случаев вы можете скопировать их без настройки. 

Параметры также сохраняются в файле *.compiled. Например, если ваш проект называется test1.lpi, то создается test1.compiled. Это простой текстовый XML-файл, поэтому вы можете просто скопировать параметры и настроить пути для компиляции на другом компьютере. Файл помещается в тот же каталог, где создается исполняемый файл. 

Это работает так же и для пакетов. 

Таким образом, вы можете скомпилировать свой (надеюсь, работающий и свободный от ошибок) код за пределами IDE lazarus. 

# Closing all editor files except one

Under gtk (available for Linux, macOS and FreeBSD) the source editor pages have a close button right to their page name. Press the Ctrl key while clicking on the button, closes all files except the clicked one. 

# Component palette

## Finding a component in the palette

You know the component name, or part of it, but you don't know on which page it was? This tool finds it: Right click on any component in the palette to open the popup menu. Select 'View all' from the menu; this will pop open a dialog(also you can achieve it by pressing Ctrl+Alt+P). Enter the (partial) component name in the 'Find' box to filter the list. 

## Open the package of a component in the palette

Right click on the component to open the popup menu, then choose open package. 

## Find the source declaration of a component in the palette

Right click on the component to open the popup menu, then choose open unit. 

# My application freezes my linux desktop while debugging

X (your desktop) can freeze, when an application that grabbed the mouse is stopped by gdb (the debugger). 

## Using a second X session

You can start a second X by: 
    
    
      X :1 &
    

with Ctrl-Alt-F7 you switch to :0 and with Ctrl-Alt-F8 you switch to :1 after that you can start a second gnome session by: 
    
    
      gnome-session --display=:1 &
    

## Using VNC

You can use vncserver/client by installing tightvncserver/realvncserver Start the server with: 
    
    
      vncserver :1
    

AFAIK, a session is also started. You can connect to the vncserver with vncviewer. 

## Debug the application on the second server

In Lazarus, in the run parameters for your project, check "use display" and enter 
    
    
     :1
    

Now your application will run on the second server, so when it is being debugged, only the second server will freeze (but that won't affect you since you are debugging on the first). 

# Compiling the IDE fast

Working on Lazarus itself needs rebuilding the IDE many times. If you use the following tricks and have enough memory and a recent cpu, you should be able to recompile the IDE in a few seconds. 

  * Put the Lazarus sources on a fast harddisk. Not on a slow network filesystem.
  * Install only needed packages.
  * Set USESVN2REVISIONINC=0 to skip the update of the revision.inc.



# DebugLn of the IDE or a LCL application

The IDE writes many useful hints via debugln. You can get these this way: 

  * Windows: start Lazarus with the command line parameter _\--debug-log=filename.txt_.
  * Linux/BSD/macOS/Solaris: just start Lazarus in a terminal.



This is a general LCL feature. 

# Finding the source file of an IDE window

  * Open the IDE window.
  * Press Ctrl+Shift+F1 to open the help editor.
  * Remember the window classname. Close the window.
  * Use [Find In Files](<../en/IDE_Window__Find_in_files.md> "IDE Window: Find in files") to find the source file of the class.



# Opening a Terminal Windows from the IDE

Its often handy to be able to open a terminal in your current project's working directory. Maybe to use a revision control system like git or svn or for a host of other possible reasons. Here is how to add a menu item to Lazarus's Tool's menu. Thanks to taazz and other contributors to <https://forum.lazarus.freepascal.org/index.php/topic,42151.msg294206.html#msg294206>

  * Click Tools, Configure External Tools
  * Click 'Add' to add a new menu item (under Tools).
  * Set Title to (eg) "Open a Terminal"
  * Set Program File Name to the command on your system that opens a terminal, some suggestions below.
  * Set Parameters to what ever your terminal expects on the command line to open a specific directory and the Lazarus var that points there, $projPath().



Those last two vary quite a lot between systems, if you don't know the first, try looking at a list of running programs, the word 'terminal' is likely to feature. For the second, once you know the terminal's name, look up its man page or google. 

Some examples include - 

O.S.  | Program File Name  | Parameters   
---|---|---  
Ubuntu Mate | mate-terminal | \--tab --working-directory=$projPath()   
xfce | exo-open | \--launch TerminalEmulator --working-directory $ProjPath()   
KDE | konsole | \--working-dir $projPath()   
Mac (see below) | /User/$HOME/bin/ttab | cd $projPath()   
Windows | ? | ?   
  
On the Mac, where everything is so much easier, all you have to do is - 

  1. Install <https://www.npmjs.com/package/ttab>, its only 665 lines, use the path you installed it to instead of the example shown in the table above.
  2. Run it so you can tick its security to access the Accessibility feature.
  3. Then set the Lazarus Tools settings as described.
  4. Then, from within Lazarus, Tools->Open Terminal, then similar to 3) tick to allow Lazarus to access the Accessibility thingo.



# Object Inspector: Events: Frames: Jump to the source of an inherited event

The Object Inspector shows the events of inherited events as _ClassName.MethodName_. Double clicking will create a new event. Ctrl+Mouse click on the combobox will jump directly to the inherited method body, without creating a new method. 

# Events (Method properties) in the Object Inspector

Events (Method properties) are special properties, because they need as value a code address pointer, which does not exist at design time. That's why Lazarus uses the same trick as the Delphi IDE: Every method value can be type casted to **TMethod** , which contains _Data_ (the object or class pointer) and the _Code_ (the address pointer). At runtime both are set. But at designtime the IDE sets Data to a special key value for its internal lookup table and sets Code to nil. This means: 

  * At runtime when the program loads the .lfm file the real method (Data+Code) is used
  * At designtime a method value can either be a real method (Data<>nil and Code<>nil) or a fake method (Data<>nil,Code=nil)
  * The compiler has for Delphi compatibility some specials about comparing method values.
  * The = and the <> operator only checks the Code, not the Data.



Do not use: 
    
    
    if OnMyEvent = NewValue then exit; // wrong, because it compares only Code
    

Because this only compares the Code. Use instead 
    
    
    if CompareMem(@FOnMyEvent, @NewValue, SizeOf(TMethod)) then exit; // correct, checking both Data and Code
    

Here is an example when the = operator fails: 
    
    
    type
      TMyClass = class
      private
        FOnClick: TNotifyEvent;
        procedure SetOnClick(Value: TNotifyEvent);
      public
        procedure Click(Sender: TObject);
        property OnClick: TNotifyEvent read FOnClick write SetOnClick;
      end;
    
    procedure TMyClass.SetOnClick(Value: TNotifyEvent);
    begin
      if Value = FOnClick then exit; // bug, checks only for Code, not for data
      FOnClick := Value;
    end;
    
    var
      a, b: TMyClass;
    begin
      a := TMyClass.Create;
      b := TMyClass.Create;
      a.OnClick := @a.Click;
      a.OnClick := @b.Click; // same code, but different data, a.OnClick still points to @a.Click;
    end.

---

_Source: [https://wiki.freepascal.org/IDE_tricks/ru](https://web.archive.org/web/20250208183355/https://wiki.freepascal.org/IDE_tricks/ru)_
