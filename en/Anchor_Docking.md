# Anchor Docking

│ **English (en)** │  **[русский (ru)](<../ru/Anchor_Docking.md>)** │

About docking in general see [Docking](<Docking.md> "Docking"). 

## Contents

  * 1 Overview
    * 1.1 Features
    * 1.2 ToDos
  * 2 Usage I. (Add docking support to the Lazarus IDE)
    * 2.1 Installation
    * 2.2 Options
      * 2.2.1 Scale on resize
      * 2.2.2 Show headers
      * 2.2.3 Flatten headers
      * 2.2.4 Header align top/left
      * 2.2.5 Allow dock sites to be minimized
      * 2.2.6 Multine Tabs
      * 2.2.7 Floating windows on top
  * 3 Usage II. (Add docking support to your own applications)
    * 3.1 Quick start
    * 3.2 Prerequisites
    * 3.3 Making forms dockable
    * 3.4 Example: miniide1
    * 3.5 Making the main form a dock site
      * 3.5.1 Requirements for a custom dock site
    * 3.6 Procedure to create all other forms by name
    * 3.7 Showing forms
    * 3.8 Docking manually, via code
    * 3.9 Undocking manually, via code
    * 3.10 Save layout
    * 3.11 Load layout
    * 3.12 Enlarge/Shrink
    * 3.13 Enlarge one, shrink another
    * 3.14 Enlarge one, shrink many
    * 3.15 General docking options
  * 4 Why use Anchors instead of Align?
    * 4.1 Layouts that can be created with Align
    * 4.2 Layouts that can be created with Align and Panels
    * 4.3 Changing Layouts
    * 4.4 Layouts that are impossible with Align and Panels
    * 4.5 Conclusion
  * 5 Old/deprecated anchor docking
    * 5.1 Create a TLazDockingManager
    * 5.2 Optional: Loading a configuration
    * 5.3 Make a form/control dockable
    * 5.4 Optional: Saving the user configuration to disk
  * 6 See also



## Overview

Docking allows you to combine forms, or to treat controls as forms. You use the mouse to drag and dock forms together or undock (split) them. The basic dragging, docking and undocking functionality is provided by the LCL. But you need a docking manager to add splitters and grab zones, and to save and restore layouts. 

Anchor docking has been implemented since version 0.9.29 in the package _anchordocking.lpk_ for use in your own Lazarus applications, and _AnchorDockingDsgn.lpk_ if you wish to make the Lazarus IDE windows dockable. It is easy to use, and provides many features and options not available in earlier docking packages. 

You will find a code example at: components/anchordocking/miniide/miniide1.lpi 

This means that the earlier TLazDockingManager and unit LDockCtrl are obsolete and now deprecated. 

### Features

  * **Drag and dock** : Headers are added that can be dragged to dock controls with the mouse. Preview rectangles will show where and how a control is docked.
  * **Any layout** : By using the LCL's anchors almost any layout is possible. You can dock to left, right, top, bottom, inside or outside or as page like in a TPageControl. You can enlarge docked controls via the popup menu. No need to undock your whole layout.
  * **What you see is how it is structured** : There are no hidden panels to align controls in rows and columns. Anchor docking makes sure forms do not overlap.
  * **Splitters** are automatically inserted between docked forms to allow resizing.
  * **Page docking**. Forms can be docked not only left/right/above/below, but in pages too. A TPageControl is automatically created for native look and feel. A page can contain arbitrary docked forms too, including paged docked forms, allowing nested pages and layouts. When a page is undocked the pagecontrol is automatically removed. You can drag and drop the tabs or use the popup menu to move pages.
  * **Easy use** : With one line of code you can make a form or control dockable. Just give them unique names.
  * Not only forms, but any **TWinControl** can be made dockable.
  * Can **save/load layouts**. The layout information is stored for each dockable control and old layout files will still work even when the application gets more dockable controls or some are removed. The information is stored in the abstract class LCL TConfigStorage. So you can use for example TXMLConfigStorage to store layouts in xml files or you can write your own storage to store it wherever you want.
  * Each docked form is put into a docksite providing an optional **header**. The header shows the caption of the docked form and contains a close button. You can _drag the header_ to drag the form and undock it or to dock it at another position. The header can be at any of the four sides and moves by default automatically to the smaller side to save space. You can customize this. The header shows a customizable hint allowing to read long captions.
  * When a docksite is resized the _child sites are automatically scaled_. You can turn this off.
  * **Preserve size of forms**. For instance when docking a form to a left side of a site the width of the docked form is kept. The same for the other sides and the same for undocking.
  * **Flickerless** : The restore tries to reuse existing sites and splitters to reduce creation and flickering of forms and to preserve the Z order of forms. This allows to quickly switch between layouts.
  * **Header popup menu** : 
    * lock/unlock (disable dragging)
    * header position (auto, left, top, right, bottom), this is saved/restored with the layout
    * merge (for example after moving a dock page into a layout)
    * undock (needed if no place to undock on screen)
    * enlarge side to left, top, right, bottom
    * close
    * options
  * **Page popup menu** : 
    * lock/unlock (disable dragging)
    * undock (needed if no place to undock on screen)
    * tab position for pagecontrol (top,bottom, left, right), this is saved/restored with the layout
    * move page to the left, right, leftmost, rightmost
    * close
    * options
  * Forms can be made dock sites. Then they can dock at one side. But they can not be docked itself.
  * close button automatically saves a layout
  * Powerful functions to alter the layout. You can use D&D to move a form and you can undock and dock in one move. There are functions to enlarge forms and shrinking others. These functions are available via the header popup menu as well. See below.
  * There is an IDE package which makes the Lazarus IDE dockable. It is called anchordockingdsgn.lpk. Make sure you only install one docking package. Do not install the easydockmgr at the same time!



### ToDos

  * Design time package: Add all layout files to menu
  * simple way to make forms dockable at designtime (that means without writing any code)
  * minimize button and Hide
  * on show again: restore a default layout
  * close button for pages
  * Implement automatic menu merging in the LCL (when two forms with main menus are docked).



## Usage I. (Add docking support to the Lazarus IDE)

If you don't like the default "separate windows" look of the Lazarus IDE and prefer a single window (e.g., like RAD Studio™), you can install a package called **[AnchorDockingDsgn](<AnchorDockingDsgn.md> "AnchorDockingDsgn")** (lazarus/components/anchordocking/design/anchordockingdsgn.lpk). This package makes the IDE windows dockable. 

### Installation

Uninstall any other docking managers (e.g., easydockmgrdsgn). From the Lazarus main menu pick "Package" - "Install/Uninstall Packages" and install the "anchordockingdsgn" package. 

After you have successfully installed the package and restarted the IDE, the IDE starts with a default docked layout like shown below. Note that AnchorDocking headers and splitters have been added to all dockable windows of the IDE. In case the headers don't get visible, see the section "Options" below on how to enable/disable them. 

[![2021-12-02 10 21 07-Debian 10.8 Buster - VMware Workstation.png](https://wiki.freepascal.org/images/a/ac/2021-12-02_10_21_07-Debian_10.8_Buster_-_VMware_Workstation.png)](</File:2021-12-02_10_21_07-Debian_10.8_Buster_-_VMware_Workstation.png>)

You can change the layout by grabbing a dockable window by the Anchordocking header and drag and dock it anywhere else, or resize docked windows via splitters added to the window borders. You can right-click on the headers too for other options, for example, to change the look of the headers, as shown farther down below. 

You can save the layout via the menu item **Tools / Save window layout as default**. This saves to ~/.lazarus/anchordocklayout.xml and restores it when the IDE starts up. 

You can also save the layout to a file and load them later. 

Since Lazarus 1.1 you can pass a command line parameter 
    
    
    ./lazarus --anchordocklayout=<filename>
    

The look (like the headers) can be further tweaked by right-clicking on them. 

[![docking headers.gif](https://wiki.freepascal.org/images/6/6f/docking_headers.gif)](</File:docking_headers.gif>)

### Options

After the successful installation of the AnchorDockingDsgn package, an additional configuration menu is added to the Lazarus options dialog ("Tools" - "Options" - "Environment") 

[![anchordocking options.png](https://wiki.freepascal.org/images/5/57/anchordocking_options.png)](</File:anchordocking_options.png>)

#### Scale on resize

When resizing a window, all docked forms inside are scaled by the same percentage. 

#### Show headers

To dock/undock a window, you need to enable this option. 

#### Flatten headers

No 3d frame in headers. 

#### Header align top/left

Headers are automatically put at top or left depending on these two values: 

  * Move header to top, when (width/height)*100<=HeaderAlignTop, default 80
  * Move header to left, when (width/height)*100>=HeaderAlignLeft, default 120
  * Otherwise keep the header at the current position.



Which means the header is by default put at the smaller side of a docked control. When the control is resized, the header switches to the smaller side. 

#### Allow dock sites to be minimized

A extra minimize button is added, so a site can be minimized. Then sites next to it get more space. If needed, the minimized site then can easy be opened. 

#### Multine Tabs

Feature added in Lazarus 2.2. If a OS support multiline tabs (like Windows), if there are no many tabs, they are shown in multiple lines. 

#### Floating windows on top

Feature added in Lazarus 2.2. If there are undocked windows shown, show these on top of main window. In a own application, there can be set DockMaster.MainDockForm, then all floating windows are shown on top of it. If MainDockForm is not set, Application.MainForm is used. 

## Usage II. (Add docking support to your own applications)

### Quick start

Add the package **AnchorDocking** to the required packages in the project inspector. 

Then add the unit **AnchorDocking** to the uses section of your main unit (the unit with your main form). 

Add to the FormCreate of your main form: 
    
    
      DockMaster.MakeDockSite(Self,[akBottom],admrpChild);
    

For the other dockable forms replace all calls of _Show_ and _ShowOnTop_ with 
    
    
      DockMaster.MakeDockable(Form1,true,true);
    

For the other dockable forms replace all calls of _Visible:=true_ with 
    
    
      DockMaster.MakeDockable(Form1,true);
    

Remove or comment all _Hide_ , _Visible:=false_ , _WindowState:=_ calls. 

### Prerequisites

  * Your project must use the package _AnchorDocking_.
  * All dockable forms must be shown with MakeDockable or MakeDockSite. Do not use _Show_ , _BringToFront_ , _ShowOnTop_ , _Visible:=true_. Of course you can have some non dockable forms, like hints or splashscreens.
  * All dockable forms must be free resizable, that means no constraints, no BorderStyle bsDialog.
  * No other docking system. AnchorDocking is incompatible with EasyDockMgr.



### Making forms dockable

There are two ways to make a form dockable. You can use **DockMaster.MakeDockable** to wrap a form into a dock site. Then your form can dock and be docked completely free. You will use that for the biggest part of your forms, except for the MainForm, because this is only supported for the MainForm on gtk2 and qt. The second way is to use **DockMaster.MakeDockSite**. The form can dock other sites, but can not be docked itself. You might want to use _MakeDockSite_ for the _MainForm_. For most other forms you probably want to use this: 
    
    
    // shows YourForm
    // if not already done, wrap the form into a dock site
    DockMaster.MakeDockable(YourForm);
    

Here are some examples how a form looks like that was made dockable with **DockMaster.MakeDockable** : 

[![Docksite1.png](https://wiki.freepascal.org/images/b/bc/Docksite1.png)](</File:Docksite1.png>) [![Docksite2.png](https://wiki.freepascal.org/images/c/c6/Docksite2.png)](</File:Docksite2.png>)

The header position is normally done automatically and the user can with a right click put it to any of the four sides. 

### Example: miniide1

The example can be found in _lazarus/components/anchordocking/miniide/miniide1.lpi_. It has a _mainform_ in unit _unit1.pas_ with a main menu which works as a docksite and several forms that are dockable. 

[![Anchordocking miniide1.png](https://wiki.freepascal.org/images/a/a5/Anchordocking_miniide1.png)](</File:Anchordocking_miniide1.png>)

### Making the main form a dock site

In the TMainIDE.FormCreate the main form **MainIDE** is turned into a docksite: 
    
    
    DockMaster.MakeDockSite(Self,[akBottom],admrpChild);
    

The parameter _[akBottom]_ allows to dock other sites at the bottom of MainIDE. The parameter _admrpChild_ tells the DockMaster that when the MainIDE is enlarged the extra space is distributed to the docked sites. 

When the user docks a site to the bottom of MainIDE the site is put onto MainIDE (Site.Parent:=MainIDE) with Site.Align=alBottom. The other controls on MainIDE must be compatible to this. MainIDE for example has a BtnPanel with Align=alLeft and a TNoteBook with Align=alClient. See [here](<Autosize_/_Layout.md> "Autosize / Layout") how Align works. 

The DockMaster adds automatically a splitter: 

[![Anchordocking miniide dockedse1.png](https://wiki.freepascal.org/images/c/ca/Anchordocking_miniide_dockedse1.png)](</File:Anchordocking_miniide_dockedse1.png>)

As soon as a dockable form is docked its caption is shown in the dock header. 

#### Requirements for a custom dock site

As long as nothing is docked into a custom dock site, your form can do everything as a normal form. But when a site is docked to it, some properties and methods need special care: 

  * AutoSize=true may result in an endless loop. Disable it or override DoAutoSize/CalculatePreferredSize and take special care.
  * Constraints: The DockMaster clears the maximum Constraints and will restore them on undock.
  * Child controls anchored to the aligned site may overlap. See [here](<Autosize_/_Layout.md> "Autosize / Layout") how Align works.
  * ChildSizing: The spacing properties like LeftRightSpacing, TopBottomSpacing may interfere with the docking layout.



### Procedure to create all other forms by name

The MainIDE sets the **DockMaster.OnCreateControl** event, to enable the DockMaster to create forms by name. This is needed to restore layouts. 
    
    
    DockMaster.OnCreateControl:=@DockMasterCreateControl;
    
    
    
    procedure TMainIDE.DockMasterCreateControl(Sender: TObject; aName: string; var
      AControl: TControl; DoDisableAutoSizing: boolean);
    
      procedure CreateForm(Caption: string; NewBounds: TRect);
      begin
        AControl:=CreateSimpleForm(aName,Caption,NewBounds,DoDisableAutoSizing);
      end;
    
    begin
      // first check if the form already exists
      // the LCL Screen has a list of all existing forms.
      // Note: Remember that the LCL allows as form names only standard
      // pascal identifiers and compares them case insensitive
      AControl:=Screen.FindForm(aName);
      if AControl<>nil then begin
        // if it already exists, just disable autosizing if requested
        if DoDisableAutoSizing then
          AControl.DisableAutoSizing;
        exit;
      end;
      // if the form does not yet exist, create it
      if aName='CodeExplorer' then
        CreateForm('Code Explorer',Bounds(700,230,100,250))
      ...
      else if aName='DebugOutput' then
        CreateForm('Debug Output',Bounds(400,400,350,150));
    end;
    

Creating a form can be as simple as this: 
    
    
    MyForm:=TMyForm.Create(Self);
    if DoDisableAutoSizing then
      MyForm.DisableAutoSizing;
    

But of course you can put there all kind of initialization code. 

The _DisableAutoSizing_ reduces flickering. Make sure your forms **Visible** property is set to **false**. 

Remember to call **DisableAutoSizing** for the form if the parameter **DoDisableAutoSizing** is set to _true_ , because _Disable-_ and _EnableAutosizing_ calls must be balanced. If you miss the _DisableAutosizing_ , the LCL will raise an exception later: 
    
    
    TControl.EnableAutoSizing SourceEditor1:TSimpleForm: missing DisableAutoSizing
    

[![MissingDisableAutoSizing.png](https://wiki.freepascal.org/images/8/8e/MissingDisableAutoSizing.png)](</File:MissingDisableAutoSizing.png>)

If you call **DisableAutoSizing** too much, your forms will not appear and/or not resize properly as the LCL is waiting for the _EnableAutoSizing_ call that never comes. 

### Showing forms

When a form should be shown, you have probably used something like '_MyForm.Show_. Instead you should now use 
    
    
    DockMaster.MakeDockable(MyForm);
    

This will wrap MyForm in a dock site and show it. It is clever enough to figure out if the form is already wrapped in a dock site. 

If you set the **DockMaster.OnCreateControl** event you can use this: 
    
    
    DockMaster.ShowControl('MyForm',true);
    

### Docking manually, via code

Use the procedure **DockMaster.ManualDock** to dock a site to or into another. 
    
    
    procedure ManualDock(SrcSite, TargetSite: TAnchorDockHostSite; Align: TAlign; TargetControl: TControl = nil);
    

  * **SrcSite** is the site to be docked. If SrcSite was docked it will be undocked first.
  * **TargetSite** is the site where SrcSite will be docked into or docked as neighbor.
  * **TargetControl** specifies if docking as neighbor (=nil), as inside neighbor (=TargetSide) or in front of a page (=a TAnchorDockPage).



  


### Undocking manually, via code

Use the procedure **DockMaster.ManualFloat** to undock a site from its neighbors and create a flotaing top level form. The embedded control stays embedded into a TAnchorDockHostSite. 
    
    
    procedure ManualFloat(AControl: TControl);
    

You can give as parameter a TAnchorDockHostSite or a control embedded into a TAnchorDockHostSite (for example a form made dockable with MakeDockable). The site will be put at the same screen location. 

### Save layout

The DockMaster allows to save the current layout to a TConfigStorage. The miniide example uses the xml version: 
    
    
    procedure TMainIDE.SaveLayout(Filename: string);
    var
      XMLConfig: TXMLConfigStorage;
    begin
      try
        // create a new xml config file
        XMLConfig:=TXMLConfigStorage.Create(Filename,false);
        try
          // save the current layout of all forms
          DockMaster.SaveLayoutToConfig(XMLConfig);
          XMLConfig.WriteToDisk;
        finally
          XMLConfig.Free;
        end;
      except
        on E: Exception do begin
          MessageDlg('Error',
            'Error saving layout to file '+Filename+':'#13+E.Message,mtError,
            [mbCancel],0);
        end;
      end;
    end;
    

### Load layout
    
    
    procedure TMainIDE.LoadLayout(Filename: string);
    var
      XMLConfig: TXMLConfigStorage;
    begin
      try
        // load the xml config file
        XMLConfig:=TXMLConfigStorage.Create(Filename,True);
        try
          // restore the layout
          // this will close unneeded forms and call OnCreateControl for all needed
          DockMaster.LoadLayoutFromConfig(XMLConfig);
        finally
          XMLConfig.Free;
        end;
      except
        on E: Exception do begin
          MessageDlg('Error',
            'Error loading layout from file '+Filename+':'#13+E.Message,mtError,
            [mbCancel],0);
        end;
      end;
    end;
    

### Enlarge/Shrink

The anchor docking manager can enlarge/shrink docked neighbors. This is done via the popup menu of the dock header or in code with the function **DockMaster.ManualEnlarge** : 
    
    
    function ManualEnlarge(Site: TAnchorDockHostSite; Side: TAnchorKind; OnlyCheckIfPossible: boolean): boolean;
    

Site is a child site, Side the side of Site to expand. If you only want to test if enlarge is possible set OnlyCheckIfPossible to true. 

### Enlarge one, shrink another

Shrink neighbor _Object Inspector_ , enlarge _Messages_ : Right click on the header of _Messages_ and click on **Enlarge right side**. Two splitters are resized. 

[![Anchordocking before enlarge1.png](https://wiki.freepascal.org/images/1/15/Anchordocking_before_enlarge1.png)](</File:Anchordocking_before_enlarge1.png>) enlarging _Messages_ to the right side: [![Anchordocking after enlarge1.png](https://wiki.freepascal.org/images/f/f8/Anchordocking_after_enlarge1.png)](</File:Anchordocking_after_enlarge1.png>)

### Enlarge one, shrink many

Shrink splitter at one side, enlarge both neighbor splitters, rotate the splitter behind, enlarge Control, shrink controls at rotate splitter. Right click on header of _Source Editor 1_ then click on **Enlarge bottom side**. 

[![Anchordocking before enlarge2.png](https://wiki.freepascal.org/images/c/cb/Anchordocking_before_enlarge2.png)](</File:Anchordocking_before_enlarge2.png>) becomes [![Anchordocking after enlarge2.png](https://wiki.freepascal.org/images/4/42/Anchordocking_after_enlarge2.png)](</File:Anchordocking_after_enlarge2.png>)

### General docking options

The AnchorDocking package provides a dialog to setup some of the properties of the DockMaster. The dialog is in the unit AnchorDockOptionsDlg and you can use it simply: 
    
    
    uses ... AnchorDockOptionsDlg;
    ...
    procedure TMainIDE.FormCreate(Sender: TObject);
    ...
    begin
      ...
      DockMaster.OnShowOptions:=@ShowAnchorDockOptions;
      ...
    end;
    

This will add a new menu item to the popup menus of the dock headers and pages called _Docking options_. 

## Why use Anchors instead of Align?

Anchors allow to create any possible rectangular layout. Align is limited. Align with Panels can theoretically create a lot of layouts, but still not all. And some operations like enlarge/shrink are more complicated using Align/Panels than using Anchors. Align works good for a few forms, but the more forms are docked, the more the disadvantages of Align become visible. 

### Layouts that can be created with Align

Align can only create the following layouts: 

[![Anchordocking align layout1.png](https://wiki.freepascal.org/images/1/13/Anchordocking_align_layout1.png)](</File:Anchordocking_align_layout1.png>)

The alTop controls are always at the top, filling the whole horizontal width. That's because the LCL first aligns all controls with alTop, then all alBottom, then alLeft, then alRight and finally alClient. 

### Layouts that can be created with Align and Panels

It is possible to nest Align layouts by using hidden panels. Then any layout that can be recursively splitted in halves can be created. Then you can create for example: 

[![Anchordocking align panels layout1.png](https://wiki.freepascal.org/images/0/0e/Anchordocking_align_panels_layout1.png)](</File:Anchordocking_align_panels_layout1.png>)

This requires only one hidden panel. 

### Changing Layouts

Now the user wants to enlarge the _FPDocEditor_ horizontally (and shrink _CodeExplorer_). With the _AnchorDocking_ you can simply right click on the _FPDocEditor_ header and click **Enlarge right side**. 

[![Anchordocking align panels layout2.png](https://wiki.freepascal.org/images/8/88/Anchordocking_align_panels_layout2.png)](</File:Anchordocking_align_panels_layout2.png>)

Other docking engines need at least 2 panels. One for _SrcEditor_ ,_Messages_ ,_FPDocEdit_ ,_CodeExpl_ and one for _SrcEdit_ and _Messages_. Most docking layouters do not even provide a simple way to change the layout in this way. An algorithm to allow this layout change, must analyze the whole structure as if there were no panels and must reparent a lot of things. Basically the algorithm must do the same as the anchor docking algorithm, but with the extra work of translating the layout into Align plus hidden panels. 

### Layouts that are impossible with Align and Panels

Now the user wants to enlarge _SourceEditor1_ horizontally (and shrink _ObjectInspector_): 

[![Anchordocking impossible with align panels1.png](https://wiki.freepascal.org/images/e/e9/Anchordocking_impossible_with_align_panels1.png)](</File:Anchordocking_impossible_with_align_panels1.png>)

This layout is impossible with Align and panels, because you can not cut it in two halves. The same with 

[![Anchordocking impossible with align panels2.png](https://wiki.freepascal.org/images/1/1e/Anchordocking_impossible_with_align_panels2.png)](</File:Anchordocking_impossible_with_align_panels2.png>)

### Conclusion

Align with hidden panels allows you to easily create a simple docking manager that works well for a few forms, but it will always limit the user. Align is useful for row and column layouts, not for tables. Anchor docking works even for complex forms. 

## Old/deprecated anchor docking

### Create a TLazDockingManager
    
    
      DockingManager:=TLazDockingManager.Create(Self);
    

The Self as parameter is only used as Owner. That means, when the mainform is freed, the DockingManager is freed too. You can use _nil_ and free the DockingManager yourself. 

### Optional: Loading a configuration

You can load the user configuration from disk. 
    
    
      Config:=TXMLConfigStorage.Create('config.xml',true);
      DockingManager.LoadFromConfig(Config);
      Config.Free;
    

This loads the file _config.xml_. The config can be created by the SaveToConfig function. See below. 

### Make a form/control dockable

Create a TLazControlDocker for each form/control that should be dockable 
    
    
      ControlDocker1:=TLazControlDocker.Create(Self);
      ControlDocker1.Name:='DockerForm1';
      ControlDocker1.Manager:=DockingManager;
    

### Optional: Saving the user configuration to disk

When the program is closed you can save the user configuration to disk 
    
    
      Config:=TXMLConfigStorage.Create('config.xml',true);
      DockingManager.SaveToConfig(Config);
      Config.WriteToDisk;
      Config.Free;
    

## See also

  * [AnchorDockingDsgn](<AnchorDockingDsgn.md> "AnchorDockingDsgn") AnchorDocking in the Lazarus IDE
  * [ Anchor Docking: A Step by step example](<Anchor_Docking_Step_by_step_example.md> "Anchor Docking Step by step example")
  * [Docking](<Docking.md> "Docking") in general
  * [EasyDockingManager](<EasyDockingManager.md> "EasyDockingManager") \- an alternative docking implementation with a simpler layout engine and fewer options.

---

_Source: [https://wiki.freepascal.org/Anchor_Docking](https://web.archive.org/web/20230610205421/https://wiki.freepascal.org/Anchor_Docking)_
