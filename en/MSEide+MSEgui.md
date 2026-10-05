# MSEide+MSEgui

│ **English (en)** │

## Contents

  * 1 Introduction
    * 1.1 But what about Lazarus?
    * 1.2 Why this wiki
  * 2 Download and links
  * 3 Quick start
    * 3.1 Open Project
    * 3.2 First RAD steps
    * 3.3 Adding a Main Menu
      * 3.3.1 Add Main Menu
      * 3.3.2 Add Menu Items
    * 3.4 Create New Project
  * 4 The concept
    * 4.1 Main development goals
    * 4.2 Architecture overview
  * 5 Tips: MSEide
    * 5.1 Larger fonts
    * 5.2 Shortcuts
    * 5.3 Print from IDE
    * 5.4 Recent opened files or projects
    * 5.5 Menu Editing
    * 5.6 New Panels in the IDE
    * 5.7 Design Tab-Order
      * 5.7.1 Creating a new event handler
  * 6 Tips: MSEgui
    * 6.1 Tabbed control
    * 6.2 Splitter
    * 6.3 Switch-off auto-scrolling of the form(widget)
    * 6.4 Read/write data in the twidgetgrid
    * 6.5 First events
    * 6.6 MDI
    * 6.7 Drawing
      * 6.7.1 Redraw form the additional thread
      * 6.7.2 Force repaint
      * 6.7.3 Draw text
      * 6.7.4 Widgets canvas
      * 6.7.5 Theming (Frames & Faces)
      * 6.7.6 Desktop colors in the MSE application
  * 7 Application examples and screenshots
    * 7.1 A Firebird application
    * 7.2 An embedded system
  * 8 Reference: MSEide
    * 8.1 MSEi18n
  * 9 Reference: MSEgui
  * 10 See also



## Introduction

The MSEide is a Cross Platform GUI Development System for Pascal programmers, completely written in Pascal. The MSEgui does not feature VCL compatibility. The graphics library provides an interface to win32 and X11. Despite being a single person (Martin Schreiber) effort for the moment, the IDE and GUI has already an amazing feature list: 

  * Internal character encoding is UCS2.
  * Internationalization tool included.
  * Docking forms.
  * Embedded forms (similar to TFrame).
  * Visual form inheritance.
  * Links to xlib and gdi32, no external widget library needed.
  * Database access components and data edit widgets.
  * Integrated debugging.
  * Integrated report designer.
  * Flexible and handy build system with switchable macros.
  * and more...



### But what about Lazarus?

[Lazarus](<http://www.lazarus.freepascal.org>) is an IDE that aims to provide a high degree of compatibility with VCL code, while providing a native look on many platforms. The native look is very important for many developers, but combined with the VCL compatibility necessitates many complex interfaces to the different native widget sets (Win32, Cocoa, GTK, Qt). The continuous evolution of these native widget sets (gtk1->gtk2->gtk3, qt->qt5) provokes endless catching up. 

MSEide+MSEgui neither features (or suffers :-) VCL compatibility, nor provides a real native look on target platforms. The graphics library provides an interface to win32 and X11. The advantage of the X11 layer is the immediate availability of a large and stable (in time) target platform. 

These different goals of both projects make them both fulfill different needs. 

### Why this wiki

The design of the MSE GUI is a fresh approach to GUI design patterns. It features many innovative solutions for typical GUI tasks. Being innovative, the MSE GUI library differs substantially from VCL/LCL/CLX. Switching from VCL to MSE may be more challenging than switching [from VCL to LCL](<Code_Conversion_Guide.md> "Code Conversion Guide"). These wiki pages aim to ease the first steps with this promising alternative IDE and GUI library. 

## Download and links

  * MSE project is now part of **mse-org** : <https://github.com/mse-org>


  * MSEide & MSEgui sources are here: <https://github.com/mse-org/mseide-msegui>


  * Biography of Martin: <https://sites.google.com/view/martin-schreiber-biography/>


  * Beginner's tutorial (in Russian) at: <http://freepascal.ru/article/mse/20060108181639/>


  * See also: "MSEide+MSEgui. Draw lesson." (in Russian): <http://freepascal.ru/article/mse/20060205191314/>


  * MSElang, the future compiler for the MSEide+MSEgui project: <https://gitlab.com/mseide-msegui/mselang/wikis/home>



## Quick start

### Open Project

Launch the IDE and use Project->Open to open the provided demo msegui/apps/demo/demo.prj. 

The title of the IDE reflects the opened project. 

Hit F9 and (if you followed the installation instructions :-), be amazed by the speed of FPC+MSE. 

### First RAD steps

Close the running project. Use Project->Source to open the project source demo.pas. 

Ctrl-click on the parameter mainfo in the line application.createform(tmainfo,mainfo); to navigate to main.pas. 

Click on F12 to toggle between unit and form. 

Not everything will feel that familiar, because of that, these pages are written. 

Click on the button, this will auto raise the property editor (unusual but useful). 

Let us add another button. Make the component palette visible if necessary (View->Toolbar->Component Palette). 

Select the tbutton on the widget page. Click on the form to create the button. 

Give it a caption in the usual way. 

Creating an event handler does not work with the usual double clicking on the onexecute property. 

Instead, you must select the onexecute property in the property editor, type the name of an event handler and press enter. 

The MSE RAD will create the handler and position the pointer on the definition. 

Use ctrl-shift-down arrow (or up) to navigate between implementation and definition. 

Just add some code like writeln('Hello world'); Hit F9. 

The writeln will end up in the target console window of MSE (not the console in which you started MSE). 

### Adding a Main Menu

#### Add Main Menu

Add a tmainmenu component from the Gui component tab to your main form. Click on your mainform (mainfo), so that Object Inspector shows the properties of the mainform. Use the arrow next to the mainmenu property of the mainform, to select tmainmenu1 as the mainmenu of mainfo. 

#### Add Menu Items

In the Object Inspector, expand the mainmenu branch. In this branch, open the menu property of mainmenu. Change the submenu.count of menu to 1 (or higher). Now expand the submenu.count property, which will show "Item 0". Expand "Item 0" and set the caption, for instance to "&File". Now you can repeat the same procedure one level deeper... Change the submenu.count property of "Item 0" to 1. Expand this last submenu.count property and set the caption of the new "Item 0", for instance to "E&xit". Hit F9 and your program should have a menu _F_ ile with the entry E _x_ it in it. 

Alternatively, in Object Inspector you may right click a submenu.count property, or an "Item n" property, which will reveal a context sensitive pop up menu. Use this pop up menu to insert, append or delete menu items. 

Once you have more than one submenu, you can use drag and drop to move the menu items ("Item n"). Start the drag mouse move on the "Item n" line. 

### Create New Project

In order to create a new project based on a template, use Project->New->From Template. For a GUI project select "default.prj", for a console project select "console.prj" 

## The concept

MSEgui is a Pascal program library, which provides the building blocks for developing programs with graphical user interface.1

MSEide is the corresponding integrated development environment for effective software production with Free Pascal and MSEgui. MSEide is also suitable for the development of gcc and microcontroller applications. 

Currently MSEide+MSEgui run under i386-linux, x86-64-linux, arm-linux, x86-64-freebsd, i386-win32 and x86-64-win64 while supported microprocessors are CPU32, ARM and AVR32. MSEgui is predominantly platform independent. Additional platforms may be added by implementing the system-dependent software elements. The system-dependent MSEgui modules are located in subdirectories of lib/common/kernel. 

### Main development goals

  * Relevant increase in productivity, compared with other development systems.
  * Identical appearance and functionality on all platforms, without additional measures in the user code.
  * Dependence on additional libraries kept as little as possible.
  * Orthogonality - there should be as little as possible interference between different program elements and as few as possible exceptions and special cases.
  * Highly parametrised GUI elements.
  * Working with MSEide+MSEgui should be fun.



### Architecture overview

MSEgui requires _no external component libraries_ \- it communicates directly with the graphical interface of the operating system, X11 via Xlib on Linux and gdi32 under Windows. 

For the individual GUI elements _no operating system resources_ are required - only the main windows are known to the operating system. The entire processing of external events (keyboard, mouse, focus control ...) happens within MSEgui on a Pascal level. 

The base class for GUI elements is a _`twidget`_. There is no distinction between simple graphic elements and elements which can receive input focus, as is the case in Delphi. All MSEgui widgets have the full functionality of a `twidget` at their disposal. 

Important features of `twidget` are `twidget.frame` and `twidget.face`. When frame or face are not used, they are represented by NIL pointer, in this way using almost no resources. 

_`twidget.frame`_ is responsible for the frame around the working area of the element. The appearance of the frame is higly adjustable - it can be a simple and quickly drawable 3D frame, as well as a complex and slower composite construct, based on images. There are also other frame elements, that can build scroll bars, buttons and labels. 

_`twidget.face`_ draws the background of the working area of a desktop GUI element - color gradients and images in various forms can be shown, while partial transparency is also possible. 

Setting the properties of frame and face can be centralised by using _`tframecomp`_ and _`tfacecomp`_ , which can be selected into `tframe` and `tface` as templates. 

The next level of centralization is _`tskincontroller`_ , through which program-wide settings can be made. In order to achieve that, `tskinkontroller` assigns to the GUI elements the appropriate `tframcomp` and `tfacecomp` templates. 

_The control of the widget functions_ (focusin, focusout, ...) is done by virtual procedures and functions and not through messages. The MSEgui message function has other tasks. Also a Corba-style interface is used - for the safe connection between the various components and the automatic disconnection through destroy, a `tobjectlinker` is used. 

The type _`msestring`_ is used in MSEgui for storing text. At the moment, msestring equals WideString. However, the reference counted FPC WideString (UnicodeString) is planned to be used under Windows, as soon as it becomes available in one of the future FPC releases. 

In text files the utf-8, ASCII or the local encoding is used - MSEide+MSEgui does not use utf-16 files. 

The _MSEgui database components_ provide the conversion from the local encoding or the utf-8 (adjustable) to WideString for the data buffering. For file operations, a set of MSEgui's own functions with WideString interface exists. An MSEgui application can therefore work consistently with WideStrings, which is a significant relief for some tasks. 

_Specialized data edit widgets_ for the basic data types (integer, real, tdatetime, ...) are available in MSEgui (`tintegeredit`, `trealedit`, `tdatetimeedit`, ...). The main event property of these widgets is `onsetvalue`, which can be used for reacting upon user input. 

The `t*edit` widgets can be inserted into a `twidgetgrid` and in that way form a column of the appropriate data format. 

For _viewing and editing of data fields_ a `tdbwidgetgrid` togeather with a set of `tdb*editwidget`s can be used. Worth mentioning are also different lookup components and a `tlookupbuffer`, a fast associative data memory. 

The basic component of the MSEgui's _database environment_ is `tmsebufdataset`, from which `tmsesqlquery` is derived. MSEgui DB is a fork of the FPC SQLDB system, where TBufDataset and large parts of TSQLQuery and the connection components have been completely rewritten. 

`tmsebufdataset` offers local indices, calculated and internalcalc-fields, a mode with an interrupted database connection, an in-memory mode where no database connectivity is necessary and can keep a local journal, in order to later reconect a previously disconnected database connection. `tmsebufdataset` stores texts as WideStrings of variable length. 

Further, there are _SQL script components_ and `tsqlresult` for quick queries without the TDataset overhead. Persistent TField instances are either kept within `tmsesqlquery`, where they can be edited in the object inspector via `tmsesqlquery.controller.fields`, or they can be inserted into forms or modules as as individual components from the 'DBf' component palette. 

MSEide supports _'visual form inheritance'_ , Submodule (Delphi TFrame equivalent) and allows the assignment of components of other forms or modules to component properties at design time. 

_Internationalization_ is done via resource modules that are loadable at runtime. The MSEi18n, a tool that supports the import and export of texts in spreadsheet programs is available for editing - therefore facilitating a translation by nonprogrammers. 

1) This chapter is based on a translation of a German text provided by Martin Schreiber. 

## Tips: MSEide

### Larger fonts

Some may find the standard menu font a bit small. The standard font for the main menu of an MSEgui application is the font defined by the stf_menu stock font. You can change a stock font value with a startup parameter, <fontheight> is given in pixels. 
    
    
    --FONTALIAS=<alias>,<fontname>[,<fontheight>[,<fontwidth>[,<options>]]]
    

So if you start the mseide like this, you can get a larger menu font: 
    
    
     mseide --FONTALIAS=stf_menu,sans,16
    

In order to increase the base font size for all stock fonts use 
    
    
     mseide --FONTALIAS=stf_default,,16
    

### Shortcuts

Common 

  * **Ctrl+F4** \- Close
  * **Ctrl+S** \- Save
  * **Ctrl+E** \- Select page
  * **F11** \- Toggle Form/Inspector
  * **F12** \- Toggle Form/Unit
  * **Shift+Ctrl+[0..9]** \- Bookmark 0..9
  * **Ctrl+[0..9]** \- Go to bookmark 0..9
  * **Ctrl+LeftClick** \- Go to var(class, method, unit) declaration
  * **Shift+Ctrl+Up** \- Go to method declaration
  * **Shift+Ctrl+Down** \- Go to method implementation
  * **Shift+Ctrl+Space** \- Show procedure header



Debug 

  * **F7** \- Step
  * **F8** \- Next
  * **F9** \- Continue
  * **Shift+F7** \- Finish
  * **Shift+Ctrl+F8** \- Next instruction
  * **Shift+Ctrl+F7** \- Step instruction
  * **F5** \- Toggle breakpoint
  * **Shift+F5** \- Toggle breakpoint enabled/disabled
  * **Ctrl+B** \- Breakpoints on/off
  * **Ctrl+W** \- Watches on/off



Editor 

  * **Ctrl+Z** \- Undo
  * **Ctrl+X** \- Cut
  * **Ctrl+C** \- Copy
  * **Ctrl+V** \- Paste
  * **Ctrl+F** \- Find
  * **F3** \- Search again
  * **Ctrl+L** \- Go to line.
  * **Ctrl+I** \- Indent block
  * **Ctrl+U** \- Unindent



### Print from IDE

You need the [ghostscript](<http://ghostscript.com/awki>)

### Recent opened files or projects

The last opened projects are in the dropdown list of 'Project'-'Open'-'Name'. The last opened files are in the dropdown list of 'File'-'Open'-'Name'. 

### Menu Editing

  * Use submenu.count to set the wanted menu items count.
  * Right click on submenu.count or on a menu item "Item X" to display a context-sensitive menu that allows to insert,append or delete a submenu entry
  * Use drag and drop on a submenu entry to move the the item in the array.



### New Panels in the IDE

Example, you want to have the watch, stack and cpu window in a window: 

  * select the menu item **View** -**Panels** -**New Panel**.
  * select the menu item **View** -**Watches**.
  * drag the Watches window by the grip (not the window title!) in the new panel.
  * same with Stack and CPU window.
  * to split horizontal drag one of the grip of the inserted widgets to the left border of the panel.
  * to split vertical drag one of the grip of the inserted widgets to the bottom border of the panel.
  * to use tabs drag one of the grip of the inserted widgets to the centre of the panel.



### Design Tab-Order

Main way in **MSEide** is set the tab order after all widgets are placed in the form with the popup menu function "Set Tab Order". Select a widget in the container where you wish to set the tab oder, click right, click "Set Tab Order", click row 0, click "Start", click the widget in the container which should get tab order 0, click the widget which should get tab order 1.. 

An alternative is to drag and drop the rows to the desired position. The first row corresponds to the tab order 0 the second to the 1 and so on. 

#### Creating a new event handler

Select the event you want in the **Object Inspector** , then type name of the new event handler. IDE create a new procedure(declaration and implementation) in the source code. If you double-click the name of the event handler in the **Object Inspector** , source editor opens with the cursor at begin of your handler implementation. 

  


## Tips: MSEgui

### Tabbed control

  * Put **ttabwidget** control on the form. Now you have container (_ttabwidget1_) for pages.
  * Put as many **ttabpage** controls on the _ttabwidget1_ as tabs you want.



### Splitter

We have two widgets (widgetLeft, widgetRight) and we need horizontal splitter between them. 

  * Add TSplitter to form and resize it as you want.
  * Change the **linkleft** of _tsplitter1_ to **widgetLeft** and **linkright** to **widgetRight**. At this step our widgets stick to splitter.
  * Change the **spo_hmove** to **True**. That's all.
  * If you want to save a proportion between width of _widgetLeft_ and width of _widgetRight_ when the main form is resized then change the **so_hprop** to **True**.
  * If you need the vertical splitter then use other properties **linktop** , **linkbottom** , **spo_vmove** , **spo_vprop**.



### Switch-off auto-scrolling of the form(widget)

Change the **container - frame - sbhorz - options - sbo_showauto** property of the form to **False**. 

### Read/write data in the twidgetgrid

  * **editwidget.value** -> value of the actual row
  * **editwidget[rowindex]** or **editwidget.gridvalue[rowindex]** -> value of the addressed row
  * **editwidget.gridvalues** the whole column as a dynamic array.



### First events

**OnCreate** is called before loaded procedures of the components of the form are called, **OnLoaded** is called after loaded procedures of the components of the form are called. Many components do initializations in loaded procedures, event properties are inactive before procedure loaded of the component is called. 

### MDI

MDI - application can be created using TDockFormWidget or TDockPanel (as MDI-area) and TDockForm (as ancestor for you MDI-child window). There are examples: 

  * How to use TDockForm with custom MDI-buttons <http://brefi.narod.ru/mdi_example2.tar.gz> or in _$MSESOURCES/contributed/ivankob/examples_
  * Simple MDI-controller (create, switch, ... the child windows) <http://mihafpc.narod.ru/MDISample.tar.gz> or in _$MSESOURCES/contributed/miha_



### Drawing

#### Redraw form the additional thread

Use a **tthreadcomp** (tab **Gui**) to do the long time procedure. If you need to access widgets and other gui variables from the additional thread, call **application.lock** before and **aplication.unlock** after accessing the main event thread elements. 

#### Force repaint

  * **twidget.invalidate** to invalidate an individual widget
  * **twidget.rootwidget.invalidate** to invalidate the window
  * **application.invalidate** to invalidate all windows (=forms) in the application



#### Draw text

There are two methods to draw texts in MSEgui: 

  * Simple text positioning by baseline position of start of the first character with **tcanvas.drawstring**.
  * With the drawtext procedures of **msedrawstring.pas**.



#### Widgets canvas

No twidget has a canvas. The canvas used for painting is normally one of the canvas of twindow, it can also be an canvas of a tbitmap or a tprinter or... MSEgui uses one canvas to paint a whole window with all widgets. _tcanvas.font_ is initialized by **twidget.getfont** , _canvas.color_ by **twidget.actualcolor** before calling **twidget.dopaint** , see **twidget.paint** procedure in msegui.pas. 

#### Theming (Frames & Faces)

Every twidget has a property **frame** and **face**. If they are deactivated (<disabled> in object inspector) they are only an nil pointer and need no more resources. Activate them by clicking on the ellipse button in the right object inspector column in the row face respective frame. 

The **face** property defines the background of the client area of the widget. 

It is a combination of a fade and a bitmap. Both of them can be semitransparent. The bitmap can be stretched and/or tiled. Example: you want to create a button with a fade as face. 

  * place a _tbutton_ on the form.
  * in object inspector activate property face.
  * set _face.fade.color.count_ to 2.
  * click on the **+** at _color.count_.
  * in item 0 select cl_gray.
  * in item 1 select cl_ltgray, now you have a left to write fade from gray to light gray.
  * change _face.fade.direction_ to _gd_up_ or _gd_down_ , you get a convex resp. concave fade.



If you want, you can blend an additional structure over the fade: 

  * select the wanted pattern image in _face.image_.
  * activate _face.image.alignment_ al_tiled.
  * adjust _face.image.transparency_ to your needs, $000000 = opaque, $ffffff = fully transparent. The number defines the transparency of the three color channels ($RRGGBB).



To centralise the face look use an **tfacecomp** (tab _Gui_) and select the facecomp in _template_ of the _face_ properties. 

The **frame** property defines the look of an additional frame around the component and shows a possible caption at the widget. Example: you want an lowered frame around and a caption right of the button. 

  * activate property _frame_.
  * set _frame.levelo_ to -1.
  * set _frame.leveli_ to 1.
  * enter the caption text in _frame.caption_.
  * set _frame.captionpos_ to cp_right.



To centralise the frame look use an **tframecomp** (tab _Gui_) and select the framecomp in _template_ of the _frame_ properties. 

#### Desktop colors in the MSE application

Default values of the mapped colors stored in the array **msegraphics.defaultmapped**. For change it's values you can use function **msegraphics.setcolormapvalue** [Win32 and KDE example](<http://www.freepascal.org/wiki/index.php/User:Miha>) (_$MSESOURCES/contributed/miha/GuiStyle.pas_) 

  


## Application examples and screenshots

### A Firebird application

The following screen shots are taken from an MSEgui Firebird database application. The programmer writes he has customized the look and feel of his application in about 30 minutes with a tfacecomp and two tframecomp. He states that the look and feel is identical on Linux and Windows. 

[![mse dbwidgetgrid1.jpg](https://wiki.freepascal.org/images/e/e6/mse_dbwidgetgrid1.jpg)](</File:mse_dbwidgetgrid1.jpg>)

[![mse dbwidgetgrid2.jpg](https://wiki.freepascal.org/images/1/1b/mse_dbwidgetgrid2.jpg)](</File:mse_dbwidgetgrid2.jpg>)

Linux version: [![mse form1.jpg](https://wiki.freepascal.org/images/8/84/mse_form1.jpg)](</File:mse_form1.jpg>)

Windows version: [![mse form1w.jpg](https://wiki.freepascal.org/images/d/d4/mse_form1w.jpg)](</File:mse_form1w.jpg>)

Linux version: [![mse button1.jpg](https://wiki.freepascal.org/images/3/35/mse_button1.jpg)](</File:mse_button1.jpg>)

Windows version: [![mse button1w.jpg](https://wiki.freepascal.org/images/3/3e/mse_button1w.jpg)](</File:mse_button1w.jpg>)

The report is designed and rendered with the built-in MSEide+MSEgui report designer and report generator. 

[![mse report1.jpg](https://wiki.freepascal.org/images/e/e2/mse_report1.jpg)](</File:mse_report1.jpg>)

Two MDI forms with fade and flat frame. 

[![mse gripfade1.png](https://wiki.freepascal.org/images/0/0f/mse_gripfade1.png)](</File:mse_gripfade1.png>)

### An embedded system

An embedded system for medical applications, in this case steam sterilizers, was realized with MSE-IDE & MSE-GUI for the control unit, nowadays also commonly called the "human machine interface", HMI. 

[![Main Screen](https://wiki.freepascal.org/images/d/d4/MainScreen.png)](</File:MainScreen.png> "Main Screen")

The control unit uses a touch panel a its display, therefore all active controls have to be made rather large to enable the personnel to handle them easily. The main screen consists of a few informational display fields and allows to select the implemented sterilizing processes by means of a menu consisting of several button components showing the process names along with a short description in a separate panel. The visibility of the buttons and their captions are defined during program loading by means of a configuration file. The informational fields display the devices' operating values in real time using a MSE system timer to collect the information from the PLC. 

  
[![Process Display](https://wiki.freepascal.org/images/2/2b/ProcessDisplay.png)](</File:ProcessDisplay.png> "Process Display")

  
After processing has been started, the display is switched to a different format showing the operating values in large digits on some numeric display field components on the left and a graphical representation of the process history on the right, again both as real time "live displays". If the diagram curves would reach the right panel border, the diagram would change to a scrolling mode, always showing the current state at the extreme right. 

  
[![State Display](https://wiki.freepascal.org/images/b/b2/StateDisplay.png)](</File:StateDisplay.png> "State Display")

  
For diagnostic purposes, also a schematic state display is available. This shows a simplified schematic of the apparatus, along with real time animated symbols of the control elements, as there are valves and pumps, each displayed in a derived version of a timage component, created and placed dynamically on program load. Also shown are the most important process values as numerical displays, also dynamically placed. The schematic field really is the output of a separate program, running in parallel to the process display which provides the upper and lower parts of the screen display. 

  
[![Control Screen](https://wiki.freepascal.org/images/e/ec/ControlScreen.png)](</File:ControlScreen.png> "Control Screen")

  
There is also a mode for setting several control and function parameters and to recall previous operational and diagnostic data. It is organised as a tabbed screen containing several run time parametrized menu pages and a couple of display field pages. Some of the pages allow access only after entering a pass code to prevent missuse of critical functions. 

  
[![Protocol Review](https://wiki.freepascal.org/images/a/aa/ProtocolReview.png)](</File:ProtocolReview.png> "Protocol Review")

  
This is a screen displaying a graphical protocol of a previous process. The protocol is usually printed out immediately after termination, as required by the regulations. The printer output is generated as a postscript file, processed by ghostscript both for print out and for the on screen display shown, which uses the png format for loading into the display field. 

  
[![Attribution](https://wiki.freepascal.org/images/e/e0/Attribution.png)](</File:Attribution.png> "Attribution")

  
Of course, there is also an attribution display, showing program version, creator information and the tools used for the creation of the system, Free Pascal and MSE-IDE & MSE-GUI. As background, it uses a photography of one such apparatus as it is used in clinics displayed in an image component carrying several label fields used for the text. Above all of that sits a transparent button to close the picture. 

## Reference: MSEide

  * To move and resize components without gridsnap use shift+cursorkeys and ctrl+cursorkeys.
  * There are many possibilities with properties twidget.frame and twidget.face, for instance tiled semitransparent bitmaps and color fades (all twidget descendants have these properties!).



For client area of forms use tmseform.container.frame and tmseform.container.face, tmseform has additionally onpaint to do custom drawing. 

You can write your own widgets and override procedure dopaint or use teventwidget with onpaint,onresize,onmouseevent,onkeydown,onkeyup... 
    
    
    How to convert filenames to windows format? 
    Unit msefileutils: 
    function tosysfilepath(const path: filenamety): filenamety; 
    procedure tosysfilepath1(var path: filenamety); 
    Converts to filename format of actual OS. 
    function tomsefilepath(const path: filenamety): filenamety; 
    procedure tomsefilepath1(var path: filenamety); 
    Converts from filename format of actual OS.
    
    
    
    Where is the MDI ???   
    In MSEgui we have tdockform and tdockpanel for that purpose. 
    Have a look at property dragdock.
    

### MSEi18n

## [Reference: MSEgui](<Reference__MSEgui.md> "Reference: MSEgui")

[On this page you can find a working in progress for documenting MSEGUI.](<Reference__MSEgui.md> "Reference: MSEgui")

  


## See also

  * [MSEide MSEgui first step](<MSEide_MSEgui_first_step.md> "MSEide MSEgui first step") Tutorial for beginners showing to create first program.
  * [MSEide MSEgui Howto](<MSEide_MSEgui_Howto.md> "MSEide MSEgui Howto") Windows example.



* * *

---

_Source: [https://wiki.freepascal.org/MSEide%2BMSEgui](https://web.archive.org/web/20230924144345/https://wiki.freepascal.org/MSEide%2BMSEgui)_
