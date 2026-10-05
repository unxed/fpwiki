# Lazarus For Delphi Users

│ **English (en)** │  **[русский (ru)](<../ru/Lazarus_For_Delphi_Users.md>)** │

This writeup is for people who are interested in Lazarus and already know Delphi. It describes the differences between the two. 

## Contents

  * 1 Delphi -> Lazarus
    * 1.1 The biggest differences between Lazarus and Delphi
    * 1.2 The first thing to do when converting a Delphi project to Lazarus
    * 1.3 Unicode support
  * 2 Delphi IDE -> Lazarus IDE
    * 2.1 Projects
    * 2.2 No "empty" IDE with Lazarus
    * 2.3 Source Editor
      * 2.3.1 Example for Local Variable Completion
      * 2.3.2 Example for Event Assignment Completion
      * 2.3.3 Example for Procedure Call Completion
      * 2.3.4 "Word Completion" Ctrl+W
      * 2.3.5 Supports Include files
    * 2.4 Designer
      * 2.4.1 Object Inspector
      * 2.4.2 Packages
  * 3 VCL -> LCL
    * 3.1 TControl.Font/TControl.ParentFont
    * 3.2 Control Dragging/Docking
    * 3.3 TEdit/TCustomEdit
    * 3.4 TDBImage
    * 3.5 (optional) TSplitter -> TPairSplitter
    * 3.6 TCustomTreeView/TTreeView
    * 3.7 Messages / Events
  * 4 See also



## Delphi -> Lazarus

Lazarus is a Rapid Application Development (RAD) tool like Delphi. That means it comes with a visual component library and an Integrated Development Environment (IDE). The Lazarus component library (LCL) is very similar to Delphi's Visual Component Library (VCL). Most Lazarus units, classes and properties have the same name and functionality as their equivalents in Delphi. This makes porting Delphi applications to Lazarus relatively easy. Even though Lazarus is in many respects an open source Delphi clone, the compatibility is not 100%. 

### The biggest differences between Lazarus and Delphi

  * Lazarus is completely open source
  * Lazarus is written in a platform-independent way
  * Lazarus uses the [Free Pascal](<Free_Pascal.md> "Free Pascal") compiler (FPC)



FPC runs on more than 15 platforms (see [Platform list](<Platform_list.md> "Platform list"). But not all FPC packages and libraries have been ported to all 15 platforms, so Lazarus currently runs on: 

  * Linux (i386, x86_64)
  * FreeBSD (i386, x86_64)
  * macOS (PowerPC, i386, x86_64, ARM64)
  * Windows (i386, x86_64)



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Lazarus is not complete and nor, for that matter, is this text. We are always searching for new developers, packagers, porters, documentation writers...

### The first thing to do when converting a Delphi project to Lazarus

Having opened Lazarus, you should go to to **Tools** and then **Convert Delphi Project to Lazarus Project** (Since Lazarus 1.2.6 the menu structure is **Tools** -> **Delphi Conversion** -> **Convert Delphi Project to Lazarus Project**). This won't do everything for you, but nonetheless will take you a good deal of the way. Note that the Lazarus IDE's conversion tools are generally one-way conversions. If you need to retain Delphi compatibility (so you can compile your project with both Delphi and Lazarus) consider converting your files with the [XDev Toolkit](<http://wiki.lazarus.freepascal.org/XDev_Toolkit>) instead. 

### Unicode support

Versions of Delphi up to Delphi 2007 didn't support Unicode, but used Windows ANSI encoding. Delphi supports Unicode by using UTF-16 encoded strings from the 2009 version. 

Lazarus on the other hand started its Unicode support earlier and uses UTF-8 encoded strings. For more information see [LCL Unicode Support](<LCL_Unicode_Support.md> "LCL Unicode Support"). 

## Delphi IDE -> Lazarus IDE

### Projects

The main file for a Delphi application is its .dpr file. A Delphi .dpr file is both the program main source, and also the file where the Delphi IDE stores information about relevant compiler switches and unit locations.  
A Lazarus application has a corresponding .lpr file, which is also the principal **Pascal source** file for the project. However, the main file of a Lazarus project is the .lpi file (Lazarus Project Information), which is created separately together with the .lpr file. All project-specific information (such as compiler switches used, paths to units used etc.) is stored in the .lpi file. So, the important file is the .lpi file. On most platforms, double-clicking on an .lpi file in the system's file explorer will start the Lazarus IDE with that project open. 

For example: 

Delphi stores the paths of the project's units in the .dpr file. For instance: 
    
    
     unit1 in 'path/Unit1.pas';
    

These 'in' paths are Delphi-specific and are not read by the Lazarus IDE. Don't use them. Instead, in the Project Options dialog (**Project** ->**Project Options...**) use the **Paths** page under **Compiler Options** to set unit paths where they differ from the project directory path. Note that most unit paths you will need are set automatically by the IDE when any package dependency is added. For instance, all standard Lazarus LCL projects have the LCL added as a dependency by default, and so any new Lazarus LCL project (**Project** ->**New Project** ->**Application**) knows all paths to all LCL compiled units without further work on your part. 

Delphi stores compiler options in the .dpr file. For instance {$APPTYPE CONSOLE}. These are ignored by the Lazarus IDE. Don't use them. Use the Compiler Options page of the Project Options dialog instead. 

### No "empty" IDE with Lazarus

One important rule: There is _always_ a project. The only way to "close" a project is to exit Lazarus, or open another project. This is because a Lazarus project is also a "session". So session information (e. g. the current editor settings) is also stored in the .lpi file and the Lazarus editor is restored to its last saved state when the project is later reopened. For example: You are debugging an application, and set a lot of breakpoints and bookmarks. You can save the project at any time, close Lazarus or open another project. When you reopen the project (even on another computer) all your breakpoints, bookmarks, open files, cursor positions, jump histories, etc. are restored. 

### Source Editor

Nearly all keys and short cuts can be defined in Tools -> Options -> Editor -> Key Mappings 

The Lazarus IDE has a lot of tools for sources. Many of them look and work very similar to Delphi. But there is one important difference: Lazarus does not use the compiler to get code information. It parses the sources directly. This has a lot of important advantages: 

The source editor works with "comments". For Delphi the comments in the source are just space between code. No code feature works there and when new code is auto inserted, your comments will travel. Under Lazarus you can do a find declaration even on code in comments. Although this is not completely reliable, it often works. And when new code is inserted, the IDE uses some heuristics to keep comment and code together. For example: It will not split the line "c: char; // comment". 

Delphi's "Code Completion" (Ctrl+Space) is called "Identifier Completion" under Lazarus. The Lazarus term "Code Completion" is a feature, combining "Automatic Class Completion" (same as under Delphi), "Local Variable Completion" and "Event Assignment Completion". All of them are invoked by Ctrl+Shift+C and the IDE determines by the cursor position, what is meant. 

#### Example for Local Variable Completion

Assume you just created a new method and wrote the statement "i:=3;" 
    
    
    procedure TForm1.DoSomething;
    begin
      i := 3;
    end;
    

Position the cursor over the identifier "i" and press `Ctrl`+`⇧ Shift`+`C` to get: 
    
    
    procedure TForm1.DoSomething;
    var i: Integer;
    begin
      i := 3;
    end;
    

#### Example for Event Assignment Completion

A nice feature of the object inspector is to auto create methods. You can get the source editor to create the events too.  
For example: 
    
    
    Button1.OnClick:=
    

Position the cursor behind the assign operator ":=" and press `Ctrl`+`⇧ Shift`+`C` to get: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Button1.OnClick:=@Button1Click;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
    
    end;
    

#### Example for Procedure Call Completion

Assume you just wrote the statement "DoSomething(Width);" 
    
    
    procedure SomeProcedure;
    var
      Width: integer;
    begin
      Width:=3;
      DoSomething(Width);
    end;
    

Position the cursor over the identifier "DoSomething" and press `Ctrl`+`⇧ Shift`+`C` to get: 
    
    
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
    

#### "Word Completion" Ctrl+W

It works similar to the "Identifier Completion", but it does not work on pascal identifiers, but on all words. It lets you choose of all words in all open files beginning with the same letters. 

#### Supports Include files

Delphi didn't support them, and so you probably haven't created many include files yet. But include files have a big advantage: They make it possible writing platform (in)dependent code without messing your code with IFDEFs. For example: Method jumping, Class Completion, find declaration, .. all work with include files. 

There are many options for the code features. 

### Designer

\- Guidelines 

#### Object Inspector

In the Delphi and Lazarus IDE's the Object Inspector is used to edit component properties and assign events etc. The following are a few minor differences to note in use : 

  1. Starting in Delphi 5 there is an Object Treeview which can be used to navigate and select objects according to hierarchy in addition to the traditional drop down list in the Object Inspector. In Lazarus this is part of the Object Inspector and is used in place of the default drop-down, you can select to use/not use it from the right click menu with "Show Component Tree"
  2. In Delphi double clicking on a blank event will auto create one and open the Source Editor to that position, in Lazarus there is a button to the right of the selected drop-down which performs this action instead.
  3. In Delphi you must manually delete the name of an event in the edit to remove the attatchement, in Lazarus you can drop down and select "(None)".
  4. Similarly to Events, double clicking regular properties such as boolean will not change the value, you must select it from a drop down. And to open those with an assigned editor form, you must click the '...' button to the right of the edit/drop-down



#### Packages

Can Lazarus install and use Delphi Packages? 

    No, because they require Delphi compiler magic.

Do we need ones specially made for lazarus? 

    Yes.
    Create a new package, save it in the package source directory (normally same directory of the .dpk file), add the LCL as required package and finally add the .pas files. You can install it, or use it in your projects now. There are some [differences between Lazarus and Delphi packages](<packages.md> "packages")

## VCL -> LCL

While the VCL and the LCL both serve much of the same purpose - of an Object Oriented Component Hierarchy especially geared toward rapid application development, they are not identical. For instance while the VCL provides many non-visual components, the LCL tries to only provide visual, while most non-visual components (such as db access) are provided by the FCL, included with [Free Pascal](<Free_Pascal.md> "Free Pascal") . 

Additionally many controls may not exist in the LCL that are in the VCL, or vice versa, or even when controls do exist in both, they are not clones, and changes must be made in applications, components and controls if porting. 

The following is an attempt to provide fairly complete descriptions of major differences or incompatiblities between the two for the Delphi user. It covers differences primarily with the VCL of D4 especially, though at times D5, D6, or D7 as well; and with the current LCL, as is in CVS. As such it may not always be accurate to the version of Delphi you are used to, or completely match the current LCL you have. If you see inacuracies between the following and the LCL as in CVS, or your Delphi feel free to append and modify so as to keep this as comprehensive as possible for all people. 

### TControl.Font/TControl.ParentFont

In the VCL it is quite common and normal to use a specific font name and font properties such as bold and italics for controls, and expect this value to always be followed. Further is provided the TControl.ParentFont property which ensures that a control will always follow its parent's font. Again the implicit assumption being that these values will always be followed, even regardless of Windows Apearance Settings. 

This is not always true in the LCL, nor can it be. The LCL being cross-platform/cross-interface in nature prefers to take a balanced aproach, and instead will always try to use native Desktop/Toolkit Apearance or Theme settings on any widgets. For example if using a GTK interface, and the gtk theme supplies a specific font for buttons, then LCL buttons will always try to use this font. 

This means that most LCL controls do not have the same level of design control that is often expected in the VCL, rather only those custom controls which are Canvas drawn instead of interface allocated can consistantly be modified in this manner regardless of the Interface used. 

### Control Dragging/Docking

In the VCL most (Win)Controls implement methods and [callback](<callback.md> "callback") functions for handling dragging and docking of controls, eg. dragging a control from one panel, and docking it onto another panel at run time. 

This functionality is currently unimplemented/unfinished in the LCL, though it is currently in the initial stages of planning, and should eventually support some level of compatibility for this type of behavior, if not in the exact same manner. 

This currently means that no Control will inherit/use the following TControl functions, procedures, properties, or events - 
    
    
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
    

that the following classes do not exist/are unusable - 
    
    
    TDragImageList = class(TCustomImageList)
    TDockZone = class
    TDockTree = class(TInterfacedObject, IDockManager)
    TDragObject = class(TObject)
    TBaseDragControlObject = class(TDragObject)
    TDragControlObject = class(TBaseDragControlObject)
    TDragDockObject = class(TBaseDragControlObject)
    

and that the following functions are also unusable/incompatible - 
    
    
    function FindDragTarget(const Pos: TPoint;
                             AllowDisabled: Boolean) : TControl;
    procedure CancelDrag;
    function IsDragObject(sender: TObject): Boolean;
    

The start of docking manager is described here: [Anchor Docking](<Anchor_Docking.md> "Anchor Docking")

### TEdit/TCustomEdit

The Edit controls, while functioning essentialy the same in the LCL as the VCL, do have some issues to be aware of in converting - 

  1. Due to restrictions in the Interfaces, TEdit.PasswordChar does not work in all interfaces yet(though in time it may), instead TCustomEdit.EchoMode emPassword should be used in the event text needs to be hidden.
  2. On Drag/Dock Events are not yet implemented. For more information please see earlier section on  Control Dragging/Docking.
  3. Font Properties are usually ignored for interface consistancy, for detailed explanation as too why please see  TControl.Font/TControl.ParentFont



### TDBImage

Delphi and Lazarus both have a [TDBImage](<TDBImage.md> "TDBImage") control that shows images stored in a database field. In current stable versions (1.2.4), Lazarus stores information about the image type in the database field before the actual image data. See procedure _TDBImage.UpdateData_ This means that Delphi and older Lazarus implementations are not compatible. 

Current stable Lazarus (1.2.0+) has implemented changes that allow Delphi compatible behaviour. Please see [Lazarus_1.2.0_release_notes#TDBImage](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") for details on how to activate this. 

Current Lazarus trunk falls back to Delphi-compatible behaviour on reading Delphi-formatted database fields into TDBImage. 

### (optional) TSplitter -> TPairSplitter

**Please Improve Me**

There is now a [TSplitter](<TSplitter.md> "TSplitter") control in the LCL, so no need to convert it. 

Nevertheless, if you want, here it is explained: 

The following is loosely based on questions by [ Vincent Snijders](</User:Vincent> "User:Vincent") on the mailing list, and responses by [Andrew Johnson](<http://lazarus-ccr.sourceforge.net/index.php?wiki=AndrewJohnson>): 

In the VCL, "Splitting" controls, that is a handle which can be dragged between two components to give one more or less space then the other, is accomplished by a TSplitter. This is often seen, for instance in the Delphi IDE between the docked Code Explorer and Source Viewer. 

The LCL provides its own Control called a TPairSplitter, which serves the same type of purpose, however it is not compatible, so "repairing" broken VCL code or Delphi DFM's will be necessary in the event of porting code, even though much is shared in common between the two. 

So what exactly are the differences?

Well the biggest differences are a VCL TSplitter has no children, instead it is placed between two controls aligned properly, and allows resizing between them at runtime, regardless its own size. It must have two controls aligned on each side to do anything. A simple example would be form with a Left Aligned Panel, a left aligned Splitter, and a second client aligned panel. On run time you could then realign the size given each panel by dragging on the handle provided by this Splitter control. 

On the LCL hand however, a TPairSplitter is a special kind of control, with two panels, and it can only be usefull if the controls to split are on these panels, but it will still perform a split between those panel whether or not anything is on them. So following the prior example, you would have a form with a TPairSplitter aligned client, and a panel aligned client on its left side, and a panel aligned client on its right side. 

The other important difference is that in the VCl, since the TSplitter is its own TControl, then the position is kept relative to the other controls on resize, so for instance a client panel will grow while the other panels will not, thus the split position is relative to the alignment of the split controls, 

In the LCL since the side panels are separate then the TPairSplitter has a Position property which is absolute relative to top or left. so on resize the actual position does not change according to contents, so a callback must be set to ensure the ratio is kept on resize if this is important. 

For example if the Right side of a vertical split needs to have alClient like behaviour, you need to add a form resize callback which does something like : 
    
    
    PairSplitter.Position := PairSplitter.Width - PairSplitter.Position; 
    

So how can I convert existing code using TSplitter to the TPairSplitter?

If the splitter and controls are created within an actual function(like form oncreate), conversion shouldn't be too difficult, primarily reorganize the code to create the controls in order of new hierarchy and set the parents of the child controls to split to the left/top and right/bottom portions of the PairSplitter. An example of the changes being - 

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
        Caption:= 'Hello';
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
        Caption:= 'Hello';
      end;
    
      BottomPanel:= TPanel.Create(Self);
      with (BottomPanel) do
      begin
        Parent:= VerticalSplitter.Sides[1];
        Align:= alClient;
      end;
    end;
      
  
So as you can see, farely consistant with most control hierarchy. And if you are familiar with DFM's, the changes needed for DFM->LFM conversion should be farely obvious from the above, as they are the same sort of changes in Parent/Owner etc. 

So the above example would be something like - 

**Delphi DFM** **(extraneous values removed)** | **Lazarus LFM** **(most width, height, etc. removed)**  
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

Both VCL and the LCL provide a TCustomTreeView/TTreeView component, used for tree structured lists of data with multiple nodes and advanced selection and Image lists, and while actual features are comparable, not all properties are entirely compatible. Primary differences are as follows - 

**Incomplete list, also update to include TCustomTreeView Mark functions and protected methods**

  1. The LCL provides a TCustomTreeView.Options, a set of options which can be set on the control to change its behaviour and apearance. These options are : 
     * tvoAllowMultiselect - enables multi node select mode, equivalent to enabling TCustomTreeView.MultiSelect in the D6 VCL
     * tvoAutoExpand - Auto Expand nodes, equivalent to enabling TCustomTreeView.AutoExpand
     * tvoAutoInsertMark - Update the Drag preview on mouse move.
     * tvoAutoItemHeight - Adjust the item heights automatically.
     * tvoHideSelection - Do not mark the selected item.
     * tvoHotTrack - use Hot Tracking, equivalent to enabling TCustomTreeview.HotTrack
     * tvoKeepCollapsedNodes - When shrinking/folding nodes, keep the child nodes
     * tvoReadOnly - make Treeview read only, equivalent to enabling TCustomTreeview.ReadOnly
     * tvoRightClickSelect - allow using Mouse Right Clicks to select nodes, equivalent to enabling TCustomTreeView.RightClickSelect
     * tvoRowSelect - allow selecting rows, equivalent to enabling TCustomTreeView.RowSelect
     * tvoShowButtons - show buttons, equivalent to enabling TCustomTreeView.ShowButtons
     * tvoShowLines - show node lines, equivalent to enabling TCustomTreeView.ShowLines
     * tvoShowRoot - show root note, equivalent to enabling TCustomTreeView.ShowRoot
     * tvoShowSeparators - show seperators
     * tvoToolTips - show tooltips for individual nodes
  2. The LCL provides additional properties: 
     * TCustomTreeView.OnSelectionChange event
     * TCustomTreeView.DefaultItems, for the default number of Items
     * TCustomTreeView.ExpandSignType to determine sign used on expandable/collapsible nodes
  3. While most On Drag/Dock Events are available in the LCL they do not work. For more information please see earlier section on Control Dragging/Docking.



### Messages / Events

The order and frequency of messages and events (OnShow, OnActivate, OnEnter, ...) differ from the VCL and depend on the [widgetset](<Widgetset.md> "Widgetset"). The LCL provides a subset of WinAPI like messages to make porting of Delphi components easier, but almost all LCL messages work a little bit different than the VCL/WinAPI counterpart. The biggest part of Delphi code using WinAPI messages uses them, because the VCL lacks a feature or for speed reasons. Such things will seldom work the same under the LCL, so they must be checked manually. That's why LCL messages are called for example LM_SIZE instead of WM_SIZE (unit lmessages). 

**Note on handling of custom messages!** As of version 0.9.26 (December 2008), way of handling custom WinApi messages (e.g. WM_HOTKEY, WM_SYSCOMMAND) differs from the way of handling these messages in Delphi. At the moment you cannot handle them via **message** directive or via overriding of _WndProc_ method of the form. The only way to handle them in the form is to hook Windows _windowproc_ by yourself. Read more here: [ Processing non-user messages in your window](<Win32/64_Interface.md> "Win32/64 Interface")

## See also

  * [Delphi language features missing from the Free Pascal Compiler](<Delphi_language_features_missing_from_the_Free_Pascal_Compiler.md> "Delphi language features missing from the Free Pascal Compiler")
  * [ Code Conversion Guide (from Delphi & Kylix)](<Code_Conversion_Guide.md> "Code Conversion Guide")
  * [Compile With Delphi](<Compile_With_Delphi.md> "Compile With Delphi")

---

_Source: [https://wiki.freepascal.org/Lazarus_For_Delphi_Users](https://web.archive.org/web/20250319231832/https://wiki.freepascal.org/Lazarus_For_Delphi_Users)_
