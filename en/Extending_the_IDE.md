# Extending the IDE

│ **[Deutsch (de)](</Extending_the_IDE/de> "Extending the IDE/de")** │  **English (en)** │  **[español (es)](</Extending_the_IDE/es> "Extending the IDE/es")** │  **[français (fr)](</Extending_the_IDE/fr> "Extending the IDE/fr")** │  **[日本語 (ja)](</Extending_the_IDE/ja> "Extending the IDE/ja")** │  **[русский (ru)](<../ru/Extending_the_IDE.md> "Extending the IDE/ru")** │  **[slovenčina (sk)](</Extending_the_IDE/sk> "Extending the IDE/sk")** │  **[中文（中国大陆） (zh_CN)](</Extending_the_IDE/zh_CN> "Extending the IDE/zh CN")** │    
****

## Contents

  * 1 Extending the IDE
    * 1.1 Overview
    * 1.2 Packages and Makefile Dependencies
  * 2 Menu items
    * 2.1 Adding a menu item
  * 3 Shortcuts
  * 4 Config files
    * 4.1 Load/Save settings
  * 5 Components
    * 5.1 Extending existing components
    * 5.2 Writing components
      * 5.2.1 Giving a new component an icon for the component palette
      * 5.2.2 Hiding a component in the component palette
    * 5.3 Writing component editors
    * 5.4 Writing property editors
      * 5.4.1 Hiding a property in the Object Inspector
      * 5.4.2 Find out what property editor is used for a property
  * 6 Forms
  * 7 Designer
    * 7.1 Writing a designer mediator
    * 7.2 Create a new unique component name
    * 7.3 Select a component in the designer/object inspector
    * 7.4 Get unit, designer, form of a file
    * 7.5 Save unit, designer form
    * 7.6 Save designer form as Pascal
  * 8 Event handlers
    * 8.1 Designer events
      * 8.1.1 Examples for designer events
        * 8.1.1.1 Example: When dropping a new component move control to a child
      * 8.1.2 Disabling the designer mouse handler
    * 8.2 Getting notified when a designer form is modified
    * 8.3 Project events
    * 8.4 Other IDE events
  * 9 Project
    * 9.1 Current Project
    * 9.2 All units of current project
      * 9.2.1 All units of the project inspector
      * 9.2.2 All used units of project
    * 9.3 The .lpr, .lpi and .lps file of a project
    * 9.4 The executable / target file name of a project
    * 9.5 Add your own project type
    * 9.6 Add your own file type
      * 9.6.1 Add a new file type
      * 9.6.2 Add a new form type
    * 9.7 Add new file
    * 9.8 Store custom values in lpi/lps
      * 9.8.1 Store custom filenames in lpi/lps
  * 10 Packages
    * 10.1 Search in all packages
    * 10.2 Search a package with a name
    * 10.3 Get lpk filename of an installed package
    * 10.4 Install packages
    * 10.5 Open a package file (lpk)
    * 10.6 Find the package(s) of a unit
    * 10.7 Find the required package(s) of a project/package
  * 11 Windows
    * 11.1 Adding a new dockable IDE window
      * 11.1.1 How to register a dockable IDE window
      * 11.1.2 Showing an IDE window
      * 11.1.3 Notes about IDEWindowCreators and SimpleLayoutStorage
  * 12 CodeTools
    * 12.1 Adding a resource directive to a file
    * 12.2 Getting the search paths for units and include files
  * 13 Source Editor
    * 13.1 Active source editor
    * 13.2 Find source editor
    * 13.3 Edit source
  * 14 SynEdit
    * 14.1 Getting the settings for a TSynEdit
    * 14.2 Getting the settings for a SynEdit highlighter
  * 15 Help
    * 15.1 Adding help for sources
    * 15.2 Adding lines to the messages window
  * 16 IDE options frame
    * 16.1 Open options in a specific frame
  * 17 Miscellaneous
    * 17.1 Adding a macro
    * 17.2 Query an IDE macro
    * 17.3 Debugging the IDE
    * 17.4 Adding a parser for external tool
    * 17.5 See also



# Extending the IDE

## Overview

The Lazarus IDE supports several types of plugins: 

Components
    These are the items found on the Component Palette. For example, TButton (found on the Standard page) can be used to create Buttons.

Component Editors
    When you double-click a component in the Designer, a component editor is invoked. The component editor may also add an extra item to the Designer's popup menu (accessed when you right-click on a component in the Designer).

Property Editors
    Each row in the grid section of the Object Inspector uses a property editor to allow that property's value to be set or altered.

Experts
    Experts encompass all other plugin types. They might register new menu item(s), register short cuts, or extend other IDE features.

  
There are two possible ways to add your own plugins to the Lazarus IDE: 

  1. Write a package, install it and register your plugin(s) in the 'Register' procedure of a unit.
  2. Extend the Lazarus code, and send your svn diff to the Lazarus mailing list.



## Packages and Makefile Dependencies

The following steps are necessary, if adding new package dependencies. 

  * Create/Update the Makefile for the package via Package editor / More ... / Create Makefile.
  * Fix all dependencies. See tools/updatemakefiles.lpi



# Menu items

You can add new menu items, hide or move existing ones and add sub menus, sections. For your own forms you can register the TMainMenu/TPopupMenu so that other packages can extend your form further. The unit **MenuIntf** of the package IDEIntf contains all registration for menus and many standard menu items of the IDE itself. 

## Adding a menu item

A single menu item like 'View Object Inspector' is called a **TIDEMenuCommand**. You can create one with **RegisterIDEMenuCommand** , which has two forms with lots of parameters: 
    
    
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
    

The difference between those two is only how you specify the parent menu section. You can give the menu section directly or indirectly via the path. Many standard sections can be found in the _MenuIntf_ unit. For example the section **mnuTools** , which is the _Tools_ menu of the main IDE bar. It contains a section **itmSecondaryTools** , the recommended section for third party tools. 

The following registers a new menu command, with the name 'MyTool', the caption 'Start my tool' and when clicked executes the procedure _StartMyTool_ : 
    
    
    procedure StartMyTool;
    begin
      ...executed when menu item is clicked...
    end;
    
    procedure Register;
    begin
      RegisterIDEMenuCommand(itmSecondaryTools, 'MyTool','Start my tool',nil,@StartMyTool);
    end;
    

If you want to call a method instead of a procedure use the **OnClickMethod** parameter. 

This menu item does not have a shortcut. If you want a shortcut you must create a **TIDECommand** and pass it in the **Command** parameter. For example: 
    
    
    uses ... IDECommands, MenuIntf;
    ...
    var
      Key: TIDEShortCut;
      Cat: TIDECommandCategory;
      CmdMyTool: TIDECommand;
    begin
      // register IDE shortcut and menu item
      Key := IDEShortCut(VK_UNKNOWN,[],VK_UNKNOWN,[]);
      Cat:=IDECommandList.FindCategoryByName(CommandCategoryToolMenuName);
      CmdMyTool := RegisterIDECommand(Cat,'Start my tool', 'Starts my tool to do some stuff', Key, nil, @StartMyTool);
      RegisterIDEMenuCommand(itmSecondaryTools, 'MyTool', 'Start my tool', nil, nil, CmdMyTool);
    end;
    

# Shortcuts

All shortcuts (keys that invoke some action) are registered in the IDECommandList. Here is an example how to find all commands that react on key Ctrl-D or two key combinations starting with Ctrl-D: 
    
    
    uses
      Classes, SysUtils, LCLProc, IDECommands;
    
    procedure ListCtrlD;
    // list all idecommands with shortcut Ctrl-D
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
    

# Config files

## Load/Save settings

All configuration files of the IDE are stored in one directory as xml files. Packages can add their own files there too. The [primary config path](<pcp.md> "pcp") directory can be read with: 
    
    
    uses LazIDEIntf;
    ...
      Directory := LazarusIDE.GetPrimaryConfigPath;
    

Packages can create their own xml files with: 
    
    
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
    
      // save settings
      try
        Config:=GetIDEConfigStorage('mysettings.xml',false);
        try
          // store the version number so future extensions can handle old config files
          Config.SetDeleteValue('Path/To/The/Version',Version,0);
          // store string variable "SomeValue"
          // if SomeValue has the default value the entry is not stored,
          // so only the differences to the default are stored.
          // This way the xml is kept short and defaults may change in future.
          Config.SetDeleteValue('Path/To/Some/Value',SomeValue,'Default');
          Config.SetDeleteValue('Path/To/Some/Integer',SomeInteger,3);
          Config.SetDeleteValue('Path/To/Some/Boolean',SomeBoolean,true);
          // there are more overloads for SetDeleteValue, find out with Ctrl+Space
        finally
          Config.Free;
        end;
    
      except
        on E: Exception do begin
          DebugLn(['Saving mysettings.xml failed: ',E.Message]);
        end;
      end;
    
      // load settings
      try
        Config:=GetIDEConfigStorage('mysettings.xml',true);
        try
          // read the version of the config
          FileVersion:=Config.GetValue('Path/To/The/Version',0);
          // read a string variable "SomeValue". If the entry does not exist, use
          // the Default
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
    

Notes: 

  * Every IDE plugin should use its own config file. Especially you must not use the standard IDE option files like [editoroptions.xml](</index.php?title=editoroptions.xml&action=edit&redlink=1> "editoroptions.xml \(page does not exist\)") or [environmentoptions.xml](</index.php?title=environmentoptions.xml&action=edit&redlink=1> "environmentoptions.xml \(page does not exist\)").
  * GetIDEConfigStorage when loading checks if the file exists and if not it will copy the template from the secondary config directory.
  * Since 0.9.31 you can use this function to load a file outside the primary config directory.



# Components

## Extending existing components

Source: [[1]](<http://lazarus.freepascal.org/index.php/topic,17733.msg98425.html#msg98425>) It is straightforward to customise existing LCL components, e.g. if you want to add properties. 

Add the properties you need with appropriate setter and getter methods and storage fields. 

Then instantiate them 'by hand', giving an owner component (to handle later destruction), and set Parent and possibly Top and Left and any other defaults your component needs when initialized. 

When you are satisfied that your customised component is debugged, add a RegisterComponent() call to stick it on the IDE Component Palette and save you having to do the setup code 'by hand' (look at any component's source to see how to do this). 

Here is a simple example: 
    
    
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
      // add custom code here
    end;
    
    procedure TEdX.SetNewStrProp(AValue: string);
    begin
      if FNewStrProp=AValue then Exit;
      FNewStrProp:=AValue;
      // add custom code here
    end;
    
    constructor TEdX.Create(theComponent: TComponent);
    begin
      inherited Create(theComponent);
      FNewStrProp:= 'new string property value';
      FNewIntProp:= 99;
    end;
    
    end.
    

## Writing components

_Main article:[How To Write Lazarus Component](<How_To_Write_Lazarus_Component.md> "How To Write Lazarus Component")_

You can create new components via the package editor. 

Create or open a package, click on **add** , click on **New Component** , fill in the items: Ancestor Type = TButton, New class name = TMyButton, palette page = Misc, Unit file name = mybutton.pas (this file will be created), unit name MyButton and click **ok**. 

### Giving a new component an icon for the component palette

Create an image file of the format .bmp, .xpm or .png with the same name as the component class. For example _tmybutton.png_ and save it in the package source directory. The image can be created by any graphic program (e.g. gimp) and should be no bigger than 24x24 pixel. Then convert the image to a resource file with the lazres tool, which can be found in the lazarus/tools directory: 
    
    
    ~/lazarus/tools/lazres mybutton.lrs tmybutton.png
    # note: Laz trunk/1.4+? uses .res resource files instead of .lrs files:
    ~/lazarus/tools/lazres mybutton.res tmybutton.png
    

This creates an pascal include file, which is used in the initialization section of mybutton.pas: 
    
    
    initialization
      {$I mybutton.lrs}
      // or in newer Lazarus version - to docheck syntax
      {$R mybutton.res}
    

Install the package. 

### Hiding a component in the component palette

Package IDEIntf, unit componentreg: 
    
    
    IDEComponentPalette.FindComponent('TButton').Visible:=false;
    

## Writing component editors

A component editor handles things like double clicking on a component in a designer or adding menu items when right clicking on a component. Writing a component editor is easy. See the unit componenteditors.pas of the package IDEIntf for lots of examples. For instance to show an editor when double clicking see TCheckListBoxComponentEditor. 

## Writing property editors

Each type of property (integer, string, TFont, ...) in the Object Inspector needs a property editor. If your component has a property _Password_ of type string, then you can define a property editor for your specific component class, with a property named _Password_ of type string. The unit propedits.pp of the package IDEIntf contains many of the standard property editors used by the IDE itself. For example the TStringPropertyEditor is the default property editor for all strings, while TComponentNamePropertyEditor is more specific and handles only TComponent.Name. See the unit for plenty of examples. 

### Hiding a property in the Object Inspector

There is a special property editor for hiding: 
    
    
      RegisterPropertyEditor(TypeInfo(TAnchorSide), TControl, 'AnchorSideLeft', THiddenPropertyEditor);
    

### Find out what property editor is used for a property
    
    
    uses Classes, TypInfo, PropEdits;
    var
      Instance: TPersistent; // your instance
      PropInfo: PPropInfo;
      EdClass: TPropertyEditorClass;
    begin
      PropInfo:=GetPropInfo(Instance.ClassType,'Name'); // replace Name with the property name you are searching for
      GetEditorClass(PropInfo, Instance);
    end;
    

# Forms

The IDE allows to easily create descendants of forms and frames. These forms can have new published components and new published methods. But you when you add new published properties, they are not accessible via the Object Inspector. They work only at runtime. The methods in the IDE are fake, they can not be executed. The IDE can create the components, because their classes are registered in the component palette. Your new properties only exist in your source code. The IDE can not fake them yet. That's why you have to register the new class. That means: 

  * create a design time package (or extend an existing one)
  * your form must be in a unit of this package


    
    
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
    

  * check the "Register" in the package editor for this unit.
  * Install the package in the IDE



# Designer

## Writing a designer mediator

The standard designer allows to visually edit LCL controls, while all others are shown as icons(like TOpenDialog or TDataSource). To visually edit non LCL control, you need to create a **designer mediator**. This can be used to design webpages, UML diagrams or other widgetsets like [fpGUI](<fpGUI.md> "fpGUI"). There is a complete example in examples/designnonlcl/. 

  * Install the example package examples/designnonlcl/notlcldesigner.lpk and restart the IDE. This will register the designer mediator for TMyWidget components and add the new components TMyButton, TMyGroupBox to the component palette.
  * Open the the example project examples/designnonlcl/project/NonLCL1.lpi.
  * Open the unit1.pas and show the designer form (F12). You should see the components as red rectangles, which can be selected, moved and resized like LCL controls.



## Create a new unique component name
    
    
    uses FormEditingIntf;
    
    ...
    
    NewName:=FormEditingHook.CreateUniqueComponentName(AComponent);
    
    // Or if you need to create the name before creating the component:
    
    NewName:=FormEditingHook.CreateUniqueComponentName(ComponentClassName,OwnerComponent);
    // ComponentClassName will be used as prefix, without a leading T.
    // OwnerComponent is the new owner of the new component.
    

## Select a component in the designer/object inspector
    
    
    uses propedits;
    ..
    GlobalDesignHook.SelectOnlyThis(AComponent);
    

## Get unit, designer, form of a file
    
    
    uses ProjectIntf, LazIDEIntf, ComponentEditors;
    ...
    // open file
    if LazarusIDE.DoOpenEditorFile(Filename,-1,-1,[ofAddToRecent])<>mrOk then exit;
    // get file interface
    aFile:=LazarusIDE.ActiveProject.FindFile(Filename,[]);
    // get designer
    aDesigner:=TComponentEditorDesigner(LazarusIDE.GetDesignerWithProjectFile(AFile,true));
    if aDesigner=nil then 
      exit; // this unit has no resource (form, frame, datamodule, etc.)
    // get form
    aForm:=aDesigner.Form;
    

## Save unit, designer form
    
    
    uses LazIDEIntf;
    ...
    if LazarusIDE.DoSaveEditorFile(Filename,[])<>mrOk then exit;
    

## Save designer form as Pascal

  * Use the function **FormEditingHook.SaveComponentAsPascal** of unit _FormEditingfIntf_. See _examples/pascalstream/CopyAsPasPkg/copyaspasdemounit1.pas_.



# Event handlers

There are several events in the IDE, for which plugins can add their own handlers. 

## Designer events

In propedits.pp there is a "GlobalDesignHook" object, which maintains several events for designing. Each event calls a list of handlers. The default handlers are added by the IDE. You can add your own handlers with the AddHandlerXXX and RemoveHandlerXXX methods. They will be called before the default handlers. 

Examples: 

  * Adding your handler (this is normally done in the constructor of your object):


    
    
     GlobalDesignHook.AddHandlerPersistentAdded(@YourOnPersistentAdded);
    

  * Removing your handler:


    
    
     GlobalDesignHook.RemoveHandlerPersistentAdded(@YourOnPersistentAdded);
    

  * You can remove all handlers at once. For example, it is a good idea to add this line in the destructor of your object:


    
    
     GlobalDesignHook.RemoveAllHandlersForObject(Self);
    

The handlers of GlobalDesignHook: 

  * Lookup root 
    * ChangeLookupRoot - Called when the "LookupRoot" changed. The "LookupRoot" is the owner object of the currently selected components. Normally this is a TForm.
  * Methods 
    * CreateMethod
    * GetMethodName
    * GetMethods
    * MethodExists
    * RenameMethod
    * ShowMethod
    * MethodFromAncestor
    * ChainCall
  * Components 
    * GetComponent
    * GetComponentName
    * GetComponentNames
    * GetRootClassName
    * AddClicked - Called when the user selected a component class and clicked on the designer to add a new component. You can change the component class and the parent.
    * ComponentRenamed - Called when a component was renamed
  * TPersistent objects 
    * PersistentAdded - Called when a new TPersistent was added to the LookupRoot
    * PersistentDeleting - Called before a TPersistent is freed.
    * DeletePersistent - Called by the IDE to delete a TPersistent.
    * GetSelectedPersistents - Called when getting the current selection of TPersistent.
    * SetSelectedPersistents - Called when setting the current selection of TPersistent.
    * GetObject
    * GetObjectName
    * GetObjectNames
  * Modifing 
    * Modified
    * Revert
    * RefreshPropertyValues
  * Selection 
    * SetSelection
    * GetSelection



### Examples for designer events

#### Example: When dropping a new component move control to a child
    
    
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
      // register your handler
      GlobalDesignHook.AddHandlerAddClicked(@AddClicked);
    end;
    
    destructor TYourDesignerExtension.Destroy;
    begin
      // remove your handler:
      GlobalDesignHook.RemoveHandlerAddClicked(@AddClicked);
    end;
    
    function TYourDesignerExtension.AddClicked(ADesigner: TIDesigner;
                 MouseDownComponent: TComponent; Button: TMouseButton;
                 Shift: TShiftState; X, Y: Integer;
                 var AComponentClass: TComponentClass;
                 var NewParent: TComponent): boolean of object;
    begin
      // in this example TYourControl is a custom control with a ChildControl.
      // Whenever the user drops a control on the TYourControl instead of adding
      // the new control to the TYourControl, the new control is added as child
      // of ChildControl.
      // created 
      if MouseDownComponent is TYourControl then
        NewParent:=TYourControl(MouseDownComponent).ChildControl;
      Result:=true;
    end;
    

### Disabling the designer mouse handler

Normally the designer catches all mouse events of a component. If your custom control should handle the mouse events handle you can either set the **csDesignInteractive** **ControlStyle** or you can fine control it via the message _CM_DESIGNHITTEST_ in your control: 
    
    
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
        Message.Result:=1; // now the designer calls the normal MouseUp, MouseMove and MouseDown methods.
    end;
    

## Getting notified when a designer form is modified

Every designed LCL form has a Designer of type TIDesigner. The IDE creates designers of type TComponentEditorDesigner defined in the IDEIntf unit componenteditors. For example: 
    
    
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
    

## Project events

These events are defined in unit LazIDEIntf. 

  * **LazarusIDE.AddHandlerOnProjectClose** : called before a project is closed
  * **LazarusIDE.AddHandlerOnProjectOpened** : called after the project was completely opened (for example all required packages were loaded, units were opened in the source editor)
  * **LazarusIDE.AddHandlerOnSavingAll** : called before IDE saves everything
  * **LazarusIDE.AddHandlerOnSavedAll** : called after IDE saved everything
  * **LazarusIDE.AddHandlerOnProjectBuilding** : called before IDE builds the project
  * **LazarusIDE.AddHandlerOnProjectDependenciesCompiling** : called before IDE compiles package dependencies of project
  * **LazarusIDE.AddHandlerOnProjectDependenciesCompiled** : called after IDE compiled package dependencies of project
  * **LazarusIDE.AddHandlerOnProjectBuildingFinished** : called after IDE built the project (successful or not) (since 1.5)
  * **LazarusIDE.AddHandlerOnLoadSaveCustomData** : called after loading/before saving a project's CustomData or a project file's CustomData. On save the CustomData is cloned to a temporary tree, so it is free for altering before save. Use this (since 2.3)



## Other IDE events

uses LazIDEIntf; 

Call these in the Register procedure of your package: 

  * **LazarusIDE.AddHandlerOnIDERestoreWindows** : called when IDE is restores its windows (before opening the first project)
  * **LazarusIDE.AddHandlerOnIDEClose** : called when IDE is shutting down (after closequery, so no more interactivity)
  * **LazarusIDE.AddHandlerOnQuickSyntaxCheck** : called when the menu item or the shortcut for Quick syntax check is executed
  * **LazarusIDE.AddHandlerOnLazarusBuilding** : // called before IDE builds Lazarus IDE (since 1.5)
  * **LazarusIDE.AddHandlerOnLazarusBuildingFinished** : // called after IDE builds Lazarus IDE (since 1.5)
  * **LazarusIDE.AddHandlerGetFPCFrontEndParams** : // called when the IDE gets the parameters of the 'fpc' front end tool (since 1.5)
  * **LazarusIDE.AddHandlerGetFPCFrontEndPath** : // called when the IDE gets the path of the 'fpc' front end tool (since 1.5)
  * **LazarusIDE.AddHandlerOnUpdateIDEComponentPalette** : ?
  * **LazarusIDE.AddHandlerOnUpdateComponentPageControl** : ?
  * **LazarusIDE.AddHandlerOnShowDesignerFormOfSource** : // called after showing a designer form for code editor (AEditor can be nil!) (since 1.5)
  * **LazarusIDE.AddHandlerOnShowSourceOfActiveDesignerForm** : // called after showing a code of designer form (since 1.5)
  * **LazarusIDE.AddHandlerOnChangeToolStatus** : //called when IDEToolStatus has changed (e.g. itNone->itBuilder etc.) (since 1.5)



Call these in the initialization section of one of your units: 

  * **AddBootHandler**
    * AddBootHandler(**libhTransferMacrosCreated** ,@YourProc): called after IDEMacros were created (since 1.3)
    * AddBootHandler(**libhEnvironmentOptionsLoaded** ,@YourProc): called after environment options were loaded (since 1.3)



# Project

## Current Project

The current main project can be obtained by LazarusIDE.ActiveProject. (unit LazIDEIntf) 

## All units of current project

Note that the term "units of project" is ambiguous. 

  * The project inspector shows the list of the .lpi file. The project might not use all these units on all platforms and maybe the developer forgot to add all used units.
  * Units from used packages are not project units.



### All units of the project inspector

To iterate through all pascal units listed in the project inspector you can use for example: 
    
    
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
    

### All used units of project

To find all currently used units of a project/package you can use the following. Note that the IDE needs to parse all units, so it can take some time. 
    
    
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
        Units:=LazarusIDE.FindUnitsOfOwner(LazProject,[fuooListed,fuooUsed]); // add fuooPackages to include units from packages
        try
          for i:=0 to Units.Count-1 do
            debugln('Filename=',Units[i]);
        finally
          Units.Free;
        end;
      end;
    end;
    

## The .lpr, .lpi and .lps file of a project
    
    
    uses 
      LCLProc, FileUtil, ProjectIntf, LazIDEIntf;
    var
      LazProject: TLazProject;
    begin
      LazProject:=LazarusIDE.ActiveProject;
      // every project has a .lpi file:
      DebugLn(['Project'' lpi file: ',LazProject.ProjectInfoFile]);
    
      // if the project session information is stored in a separate .lps file:
      if LazProject.SessionStorage<>pssNone then
        DebugLn(['Project'' lps file: ',LazProject.ProjectSessionFile]);
    
      // If the project has a .lpr file it is the main source file:
      if (LazProject.MainFile<>nil)
      and (CompareFileExt(LazProject.MainFile.Filename,'lpr')=0) then
        DebugLn(['Project has lpr file: ',LazProject.MainFile.Filename]);
    end;
    

## The executable / target file name of a project

There is a macro $(TargetFile), which can be used in paths and external tools. You can query the macro in code: 
    
    
    uses MacroIntf;
    
    function MyGetProjectTargetFile: string;
    begin
      Result:='$(TargetFile)';
      if not IDEMacros.SubstituteMacros(Result) then
        raise Exception.Create('unable to retrieve target file of project');
    end;
    

See here for more macros: [IDE Macros in paths and filenames](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames"). 

## Add your own project type

You can add items to the 'New ...' dialog: 

  * See the unit ProjectIntf of the package IDEIntf.
  * See the [Project Templates](<Project_Templates.md> "Project Templates")



  


## Add your own file type

You can add items to the 'New ...' dialog: 

  * See the unit ProjectIntf of the package IDEIntf.
  * Choose a base class TFileDescPascalUnit for normal units or TFileDescPascalUnitWithResource for a new form/datamodule.



### Add a new file type
    
    
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
      Name:='MyText'; // do not translate this
      DefaultFilename:='text.txt';
      AddToProject:=false;
    end;
    
    function TFileDescText.GetLocalizedName: string;
    begin
      Result:='My Text'; // replace this with a resourcestring
    end;
    
    function TFileDescText.GetLocalizedDescription: string;
    begin
      Result:='An empty text file';
    end;
    

### Add a new form type
    
    
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
      Name:='MyForm'; // do not translate this
      ResourceClass:=TMyForm;
      UseCreateFormStatements:=true;
    end;
    
    function TFileDescPascalUnitWithMyForm.GetInterfaceUsesSection: string;
    begin
      Result:='Classes, SysUtils, MyWidgetSet';
    end;
    
    function TFileDescPascalUnitWithForm.GetLocalizedName: string;
    begin
      Result:='MyForm'; // replace this with a resourcestring
    end;
    
    function TFileDescPascalUnitWithForm.GetLocalizedDescription: string;
    begin
      Result:='Create a new MyForm from example package NotLCLDesigner';
    end;
    

## Add new file

Adding a new file to the project: uses LazIDEIntf, ProjectIntf; 
    
    
    LazarusIDE.DoNewEditorFile(FileDescriptorUnit,_,_ ,[nfIsPartOfProject,nfOpenInEditor]);
    

The FileDescriptorUnit generates a default code and a default filename. The new file is opened in the source editor and it is not yet saved to disk. 

You can create a new file with specific filename and custom source: 
    
    
    LazarusIDE.DoNewEditorFile(FileDescriptorUnit,NewFilename,NewSource,[nfIsPartOfProject,nfOpenInEditor]);
    

Keep in mind that the file is not saved to disk. This is done when the user triggers a _Save_. 

Alternatively you can create a file via codetools or directly on disk (e.g. TFileStream) and open that in the source editor: 
    
    
     LazarusIDE.DoOpenEditorFile(Filename,-1,-1,[ofAddToRecent]);
    

## Store custom values in lpi/lps

You can store your own data via name/value pairs in CustomData and CustomSessionData. 
    
    
    LazarusIDE.ActiveProject.CustomData['Name']:=Value; // stored in lpi - x-platform, shared data
    LazarusIDE.ActiveProject.CustomSessionData['Name']:=Value; // stored in lps - only this machine
    

### Store custom filenames in lpi/lps

Filenames should be stored relative in the lpi and some care must be taken because platform specifics. Here is an example from the pas2jsdsgn package: 

Hook the OnLoadSaveCustomData handler: 
    
    
    uses
      LazIDEIntf, ProjectIntf, AvgLvlTree;
    
    ...
      // in registration
      LazarusIDE.AddHandlerOnLoadSaveCustomData(@OnLoadSaveCustomData);
    
    ...
    procedure TPJSController.OnLoadSaveCustomData(Sender: TObject; Load: boolean;
      CustomData: TStringToStringTree; PathDelimChanged: boolean);
    var
      fn: String;
      aProject: TLazProject;
    begin
      if Sender is TLazProject then
        begin
        aProject:=TLazProject(Sender);
        if CustomData[PJSProjectWebBrowser]='1' then
          begin
          fn:=CustomData[PJSProjectHTMLFile];
          if fn<>'' then
            begin
            if Load then
              aProject.ConvertFromLPIFilename(fn)
            else
              aProject.ConvertToLPIFilename(fn);
            CustomData[PJSProjectHTMLFile]:=fn;
            end;
          end;
        end;
      if PathDelimChanged then ;
    end;
    

# Packages

You can perform various actions with [Packages](<Lazarus_Packages.md> "Lazarus Packages"): 

## Search in all packages

Iterate all packages loaded in the IDE (since 0.9.29). 
    
    
    uses PackageIntf;
    ...
    for i:=0 to PackageEditingInterface.GetPackageCount-1 do
      writeln(PackageEditingInterface.GetPackages(i).Name);
    

## Search a package with a name
    
    
    uses PackageIntf;
    ...
    var
      Pkg: TIDEPackage;
    begin
      Pkg:=PackageEditingInterface.FindPackageWithName('LCL');
      if Pkg<>nil then 
        ...
    end;
    

Note: FindPackageWithName does not open the package editor. For that use DoOpenPackageWithName. 

## Get lpk filename of an installed package
    
    
    uses PackageIntf;
    ...
    var
      Pkg: TIDEPackage;
    begin
      Pkg:=PackageEditingInterface.FindPackageWithName('LCL');
      if Pkg<>nil then 
        LPKFilename:=Pkg.Filename;
    end;
    

## Install packages

**Note** : Only install packages with IDE plugins. Installing other packages can make the IDE unstable. 
    
    
    uses PackageIntf, contnrs;
    ...
      PkgList:=TObjectList.create(true);
      try
        Pkg:=TLazPackageID.Create;
        Pkg.Name:='Cody';
        PkgList.Add(Pkg);
        // check if the IDE can find the cody.lpk and all dependencies
        // The IDE will prompt some warnings/confirmations if something looks strange.
        if not PackageEditingInterface.CheckInstallPackageList(PkgList,[]) then
          exit;
        // in this example we have checked already, so skip the warnings
        // and rebuild the IDE
        if PackageEditingInterface.InstallPackages(PkgList,[piiifSkipChecks,piiifRebuildIDE])<>mrOK then
          exit;
      finally
        PkgList.Free;
      end;
    

## Open a package file (lpk)
    
    
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

**Warning:** The IDE automatically closes unused packages on idle. Never keep a reference to an TIDEPackage.

## Find the package(s) of a unit

You have the file name of a unit, e.g. '/home/user/unit.pas', and you want to know to which package(s) and project(s) it belongs use this: 
    
    
    uses Classes, PackageIntf, ProjectIntf;
    ...
    var
      Owners: TFPList;
      i: Integer;
      o: TObject;
    begin
      Owners:=PackageEditingInterface.GetPossibleOwnersOfUnit('/full/path/of/unit.pas',[]);
      if Owners=nil then begin
        // unit is not directly associated with a project/package
        // maybe the unit was for some reason not added, but is reachable
        // search in all unit paths of all projects/packages
        // Beware: This can lead to false hits
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
    

## Find the required package(s) of a project/package

You have a project or a package and you want to know which package(s) are required use this: 
    
    
    uses Classes, PackageIntf, ProjectIntf;
    ...
    var
      lPkgList: TFPList;
      i: Integer;
    begin
      PackageEditingInterface.GetRequiredPackages(LazarusIDE.ActiveProject, lPkgList, [pirNotRecursive]);
      try
        for i := 0 to lPkgList.Count - 1 do begin
          writeln('required package is ' + TIDEPackage(lPkgList[i]).Filename);
        end;
      finally
        lPkgList.Free;
      end;
    ...
    

# Windows

There are basically four types of IDE windows. 

  * the main IDE bar is the Application.MainForm. It is always present.
  * floating/dockable windows like the Source Editors, the Object Inspectors and Messages.
  * the modal forms, like the find dialog, options dialogs and questions.
  * hints and completion forms



## Adding a new dockable IDE window

What is a dockable IDE window: Windows like the Source Editor or the Object Inspector are floating windows, that can be docked if a docking package is installed, and its state, position and size is stored and restored on next IDE start. In order to restore a window the IDE needs a creator as defined in the unit **IDEWindowIntf** of the package IDEIntf. Each dockable window must have a unique name. Do not use generic names like 'FileBrowser' because this will easily clash with other packages. And don't use short names like 'XYZ', because the creator is responsible for all forms beginning with this name. 

### How to register a dockable IDE window

Remember to choose a long unique name that is a valid pascal identifier. Your window can have any caption you want. 
    
    
    uses SysUtils, IDEWindowIntf;
    ...
    var MyIDEWindow: TMyIDEWindow = nil; 
    
    procedure CreateMyIDEWindow(Sender: TObject; aFormName: string; var AForm: TCustomForm; DoDisableAutoSizing: boolean);
    begin
      // check the name
      if CompareText(aFormName,MyIDEWindowName)<>0 then exit;
      // create the form if not already done and disable autosizing
      IDEWindowCreators.CreateForm(MyIDEWindow,TMyIDEWindowm,DoDisableAutosizing,Application);
      ... init the window ...
      AForm:=MyIDEWindow;
    end;
    
    procedure Register;
    begin
      IDEWindowCreators.Add('MyIDEWindow',@CreateMyIDEWindow,nil,'100','50%','+300','+20%');
      // the default boundsrect of the form is:
      // Left=100, Top=50% of Screen.Height, Width=300, Height=20% of Screen.Height
      // when the IDE needs an instance of this window it calls the procedure CreateMyIDEWindow.
    end;
    

### Showing an IDE window

**Do not use Show**. Use: 
    
    
    IDEWindowCreators.ShowForm(MyIDEWindow,false);
    

This will work with docking. The docking system might wrap the form into a docking site. The BringToFront parameter tells the docking system to make the form and all its parent sites visible and bring the top level site to the front. 

### Notes about IDEWindowCreators and SimpleLayoutStorage

The IDEWindowCreators.SimpleLayoutStorage simply stores the BoundsRect and WindowState of all forms that were once opened. It is used as fallback if no dockmaster is installed. It stores the state even if a DockMaster is installed, so that when the dockmaster is uninstalled the forms bounds are restored. 

The IDEWindowCreators is used by all dockable forms to register themselves and to show forms. When showing a form the Creator checks if a IDEDockMaster is installed and will delegate the showing to it. If no IDEDockMaster is installed it simply shows the form. The IDEDockMaster can use the information in the IDEWindowCreators to create forms by names and get an idea where to place a form when showing it for the first time. For more details see the packages AnchorDockingDsgn and EasyDockMgDsgn. 

# CodeTools

The CodeTools is a package providing a vast amount of functions to parse, search and change pascal sources and some basic functions for other languages as well. You can use them without the IDE too. It is recommended that you read first about the CodeTools in general before using them in the IDE: [Codetools](<Codetools.md> "Codetools"). 

Before calling any of the CodeTools functions in the IDE you should commit the current changes of the source editor to the CodeTools buffers: 
    
    
    uses LazIDEIntf;
    ...
      // save changes in source editor to codetools
      LazarusIDE.SaveSourceEditorChangesToCodeCache(-1); // -1: commit all source editors
    

## Adding a resource directive to a file

This adds a {$R example.res} to a pascal unit: 
    
    
    procedure AddResourceDirectiveToPascalSource(const Filename: string);
    var
      ExpandedFilename: String;
      CodeBuf: TCodeBuffer;
    begin
      // make sure the filename is trimmed and contains a full path
      ExpandedFilename:=CleanAndExpandFilename(Filename);
      
      // save changes in source editor to codetools
      LazarusIDE.SaveSourceEditorChangesToCodeCache(-1);
    
      // load the file
      CodeBuf:=CodeToolBoss.LoadFile(ExpandedFilename,true,false);
    
      // add the resource directive
      if not CodeToolBoss.AddResourceDirective(CodeBuf,'example.res') then
        LazarusIDE.DoJumpToCodeToolBossError;
    end;
    

The codetools provides also functions like FindResourceDirective and RemoveDirective. 

## Getting the search paths for units and include files

There are many different search paths in the IDE from projects, packages, the fpc and lazarus directory and there are many types of paths: before or after resolving macros, with or without inherited search paths, as relative or absolute paths. All files in a directory share the same set of search paths. You can get the search paths for each directory fully resolved by asking the codetools: 
    
    
    uses CodeToolManager;
    ...
    
    Dir:=''; // the empty directory is for new files and has the same settings as the project directory
    
    // Getting the search paths for include files:
    Path:=CodeToolBoss.GetIncludePathForDirectory(Dir);
    
    // Getting the search paths for units:
    // This search path is passed to the compiler.
    // It contains the package output directories, but not the package source directories.
    Path:=CodeToolBoss.GetUnitPathForDirectory(Dir);
    
    // There can be additional unit search paths for the IDE only (not passed to the compiler)
    Path:=CodeToolBoss.GetSrcPathForDirectory(Dir);
    
    // The complete search path contains also all package source paths for units:
    Path:=CodeToolBoss.GetCompleteSrcPathForDirectory(Dir);
    

# Source Editor

## Active source editor
    
    
    uses SrcEditorIntf;
    ...
    Editor:=SourceEditorManagerIntf.ActiveEditor;
    if Editor=nil then exit;
    Filename:=Editor.FileName;
    ScreenPos:=Editor.CursorScreenXY; // glyph/character and line, both 1-based
    TextPos:=Editor.CursorTextXY; // byte and line, both 1-based
    

## Find source editor

You can search for a disk file using the expanded filename. Keep in mind, that newly created files usually have filenames without directory. 
    
    
    uses SrcEditorIntf;
    ...
    Editor:=SourceEditorManagerIntf.SourceEditorIntfWithFilename(aFilename);
    if Editor=nil then exit;
    Filename:=Editor.FileName; // can be different to what you searchedm e.g. due to UTF-8 and case insensitive file system
    

## Edit source
    
    
    // You can set the whole source editor text and wipe out the undo history
    Editor.SourceText:='New content';
    
    // In order to keep the undo history use the Selection:
    Editor.BlockBegin:=Point(1,1);
    Editor.BlockEnd:=Point(1,Editor.LineCount+1);
    Editor.Selection:='New Content';
    

# SynEdit

## Getting the settings for a TSynEdit

When you have a dialog using a TSynEdit and you want to use the same font and settings like the source editor use: 
    
    
    uses SrcEditorIntf;
    ...
    SourceEditorManagerIntf.GetEditorControlSettings(ASynEdit);
    

## Getting the settings for a SynEdit highlighter

When you have a dialog using a TSynEdit with a highlighter and you want to use the same colors like the source editor highlighter for this language use: 
    
    
    uses SrcEditorIntf;
    ...
    SourceEditorManagerIntf.GetHighlighterSettings(ASynHighlighter);
    

See for an example: TSQLStringsPropertyEditorDlg.Create in the unit [SQLStringsPropertyEditorDlg](<http://svn.freepascal.org/svn/lazarus/trunk/components/sqldb/sqlstringspropertyeditordlg.pas>). 

# Help

## Adding help for sources

First create a THelpDatabase: 
    
    
    HelpDB := TFPDocHTMLHelpDatabase(
      HelpDatabases.CreateHelpDatabase('ANameOfYourChoiceForTheDatabase',
      TFPDocHTMLHelpDatabase,true));
    HelpDB.DefaultBaseURL := 'http://your.help.org/';
    
    FPDocNode := THelpNode.CreateURL(HelpDB,
      'Package1 - A new package',
      'file://index.html');
    HelpDB.TOCNode := THelpNode.Create(HelpDB,FPDocNode);// once as TOC
    DirectoryItem := THelpDBISourceDirectory.Create(FPDocNode,'$(PkgDir)/lcl',
      '*.pp;*.pas',false);// and once as normal page
    HelpDB.RegisterItem(DirectoryItem);
    

  


## Adding lines to the messages window
    
    
    unit IDEMsgIntf, IDEExternToolIntf;
    ...
    var Dir: String;
    begin
      Dir:=GetCurrentDir;
      IDEMessagesWindow.AddCustomMessage(mluProgress,'Some important message','file.pas',0,0,'My section');
    end;
    

# IDE options frame

## Open options in a specific frame
    
    
    uses LazIDEIntf;
    ...
    LazarusIDE.DoOpenIDEOptions(TYourIDEOptionsFrame);
    

# Miscellaneous

## Adding a macro

You can add your own IDE macros: 
    
    
    uses MacroIntf, MacroDefIntf;
    
    procedure Register;
    begin
      // registering a Macro with a fixed value:
      IDEMacros.Add(TTransferMacro.Create('MyMacro1','Value','Description',nil,[]));
    end;
    
    // registering a Macro with a dynamic value requires a method of an object
    procedure TForm1.Init;
    begin
      IDEMacros.Add(TTransferMacro.Create('MyMacro','Value','Description',@OnResolveMyMacro,[]));
    end;
    
    function TForm1.OnResolveMyMacro(const s: string; const Data: PtrInt;
      var Abort: boolean): string;
    // s is the parameter. For example $MyMacro(parameter)
    // Data is the querying instance. Can be nil.
    // If there is an error set Abort to true.
    // The result string can contain macros. For example Result:='$(FPCVer)/$(TargetOS)';
    begin
      Result:='MyValue';
    end;
    

## Query an IDE macro
    
    
    uses MacroIntf;
    
    function GetCurrentFPCSourceDirectory: string;
    begin
      Result:='$(FPCSrcDir)';
      if not IDEMacros.SubstituteMacros(Result) then
        raise Exception.Create('unable to retrieve FPCSrcDir');
    end;
    

## Debugging the IDE

  * Open a Lazarus instance (called 1st instance of Lazarus).
  * Compile the IDE as usual e.g. via Tools/Build Lazarus. Make sure you set up debugging flags like -g or -gw2. Best is to use the profile "Debug IDE" which contains many flags useful for debugging.
  * Open the ide\lazarus.lpi project (always with the 1st instance of Lazarus).
  * Run this lazarus project (always with the 1st instance of Lazarus): another instance of Lazarus should appear (called 2nd instance of Lazarus).
  * Set breakpoints as needed etc in the opened source code (in the 1st instance of Lazarus), and play with 2nd run-time instance: for example, install a design-time static package *.lpk, then use, interact with this just installed package inside this 2nd instance.



## Adding a parser for external tool

There is an example parser for the Delphi dcc32.exe compiler in compilers/delphi/lazdelphi.lpk. 

It parses the output of dcc32.exe for filename and line number, etc, so when you click on an error or hint in the Messages window the IDE jumps to the right position. 

After installing this package, you can call dcc32.exe in any project/package instead of fpc: In Compiler Options / Compiler Commands: 

  * **Execute Before** : pathofdcc32.exe params...
  * **Parsers** : check "Delphi Compiler"
  * **Compiler** : Skip calling compiler



## See also

  * [Creating IDE Help](<Creating_IDE_Help.md> "Creating IDE Help")
  * [Help protocol](<Help_protocol.md> "Help protocol")
  * [How To Write Lazarus Component](<How_To_Write_Lazarus_Component.md> "How To Write Lazarus Component")

---

_Source: [https://wiki.freepascal.org/Extending_the_IDE](https://web.archive.org/web/20240920204109/https://wiki.freepascal.org/Extending_the_IDE)_
