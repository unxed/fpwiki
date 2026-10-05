# Feature Ideas

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This page is intended to **collect ideas** for features that **could** be included in FPC/Lazarus. Putting an idea here does **not mean** it **WILL** be put into FPC/Lazarus. It's just a place to gather ideas. Think of this as more of a wishlist.

If it is clear who is going to implement the feature, for example, because you have a patch or somebody on the mailing list volunteered to implement it, then it can be added to the [FPC/Lazarus Bugtracker](<http://bugs.freepascal.org/>), so it can be tracked and scheduled for a release. 

If you really want to see a feature implemented urgently, have a look at [Bounties](<Bounties.md> "Bounties") to pay for development. 

Tags: Wish, Wishes, Wishlist 

## Contents

  * 1 IDE
    * 1.1 An Integrated IDE
    * 1.2 Auto-generated database form
    * 1.3 Easier Dragging & Dropping
      * 1.3.1 Ideas that have been implemented
    * 1.4 Common
      * 1.4.1 Ideas that have been implemented
    * 1.5 Source Editor
      * 1.5.1 Ideas that have been implemented
      * 1.5.2 Improvement of the menu and toolbars
    * 1.6 Integrated and Working Debugger
    * 1.7 Object Inspector
      * 1.7.1 Search field
      * 1.7.2 Recent page
      * 1.7.3 Not registered component classes
    * 1.8 Refactoring
      * 1.8.1 Global Variable/Method renaming
      * 1.8.2 Parameter Renaming in Methods
      * 1.8.3 Parameter reordering
      * 1.8.4 Improvement of the IDE like GExperts does for Delphi
    * 1.9 I have a dream of components : Jedi
    * 1.10 Code completion
      * 1.10.1 Create/Free objects
      * 1.10.2 Add list properties
    * 1.11 Visual Form Inheritance
    * 1.12 Conditional Compiler Options
    * 1.13 Help
      * 1.13.1 A small tutorial for Object Oriented Programming
    * 1.14 Better Macro support by making them easier to find
    * 1.15 Copy and Paste Support for a lazy programmer (in properties-editor)
    * 1.16 Code Library Window or Tab
    * 1.17 Write and run unit tests from subpackage of project being tested
  * 2 LCL
    * 2.1 Components
      * 2.1.1 Visual
      * 2.1.2 NonVisual
    * 2.2 PDA Support
    * 2.3 More Interfaces
      * 2.3.1 improved EventQueue
      * 2.3.2 Lua
  * 3 Visual Improvements
    * 3.1 An idea for a splash screen
      * 3.1.1 critique
  * 4 FPC
    * 4.1 Per-type Byte Alignment
      * 4.1.1 Nuances
    * 4.2 "vectorcall;" modifier for Win32 and Win64
      * 4.2.1 Nuances
    * 4.3 "default"-value in a proc or function call
      * 4.3.1 Critique
    * 4.4 "pure;" function directive
      * 4.4.1 Nuances
    * 4.5 Support for Traits
    * 4.6 Support for Promises
  * 5 Other
    * 5.1 Fixes branch releases
    * 5.2 See Also



# IDE

Features that could be included in the IDE 

## An Integrated IDE

(Partly) done in 0.9.29: see [Anchor Docking](<Anchor_Docking.md> "Anchor Docking")

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** There are known bugs in the docking implementation. The Lazarus developers are aware of these and will be working on them if time permits. If you want to help speed this up by providing code, please get in touch on the Lazarus mailing list

There are too many windows in the IDE. The more features you use the more they pile up on your desktop and become hidden behind other windows. This is confusing for the newbie and otherwise a mess. Docking windows as found in some IDEs might seem to give flexibility however in practice it is a two dimensional solution. You accidentally undock a window and have it hanging on your mouse while you try to put it back where it was. It would be better to use conventional splitters and tab controls to put it all together into a logical layout. 

  * Associated windows should be converted to Frames and then placed on a single form or tab page on the main form
  * 1\. There should be a Welcome Page having new project wizards, a list of recent projects, help and resources
  * 2\. A Development tab containing sub tabs for code and for form design



    

  * a. Code tab including Code Explorer, Source Editor and Project Manager
  * b. Form design tab including Object Inspector, Form designer and Component Palette



  * 3 Debug tab containing the Debug windows as Frames.


  * 4 since an integrated IDE has less screen real estate, swapping in and out windows need to be easier.



    

  * a. Probably a view concept is needed (different layout in debug then in normal (debug)). Maybe differentiate dependning on the F12 state (designer visible) too.
  * b. Big missing feature is the ability to let tabs page in from the side (a tab system to the left and right from the screen, ideal for process manager). When such tab loses focus, it pops back.



## Auto-generated database form

If you have set up a TSQLQuery, it would be nice to be able to let the IDE autogenerate a from with appropriate controls for all fields. --[BigChimp](</User:BigChimp> "User:BigChimp") 16:39, 8 August 2014 (CEST) 

## Easier Dragging & Dropping

### Ideas that have been implemented

Done in 0.9.29: Options / Form editor / Ask name on create ~~pool: after you add a component at Form, what is your next action? Editing Name/Caption in OI. Would be nice having OI focused and a key field like Name/Caption/Text selected. It would be very intuitive just having to type after dropping the component.~~

## Common

  * enable menu access through access keys like under windows (i hate mouse sessions :o)



example: <alt>-<f> for file menu. **Question: what operating system/widget set are you talking about?**

### Ideas that have been implemented

  * (Done in 1.1) ~~Open files in the same folder as the current tab editor, like SCite does, it's a bit annoying to navigate from folder to folder every time you need open a file related to the one we are editing at the moment.~~



## Source Editor

  * Multiple splitting (and close the split) the editor to horizontal and vertical views like with VIM, so I can do split for example Vertically, and inside do a split for Horizontal editor. And we can close each split by right click on the movable splitter and select it from a menu item.


  * VIM keyboard support - require implementation of a command line like that can accept keyboards like with the VIM text editor.



    A good example is Qt Creator: you press Alt+V+V and get into FakeVim mode, where most vim commands work
    Question: what do you mean by keyboard? Another keyboard layout? Different shortcuts keys? Shortcut keys can already be defined via the Tools menu

  * Multi linguistic spell checkers for strings and comments, using aspell, hspell, ispell etc... (aspell and hspell already have binding for FPC)



  


### Ideas that have been implemented

  * (Done in 0.9.27) ~~When focusing/selecting one BEGIN the respective END should be in RED to show where it really ends.~~


  * (Done in 0.9.29) ~~Movable tabs, so we can place tabs as we wish instead of keeping them where they where opened.~~


  * (Done in 0.9.27) ~~Ability to have a way to fold code arbitrary by using comments with special chars such as~~


    
    
    some code {% /* } <-- start folding 
     more code
     ...
     ...
    and more code {% */ } <-- stop folding 
    

  * Done: if you click on an error message, the cursor jumps to the code line in question and highlights it.~~Color code on errors with special underline or some type of color that the user can select/change that will help to locate where the problem is (not instead of the error message that exists today).~~


  * Done: see below ~~Ability to keep the string mark (that added to 0.9.27) of a word/selected string as permanent with a toggle of a menu item/key/toolbar or something~~



    QUESTION: Does that refer to the "highlight all occurrences" of "word under caret"/"current selection"?
    That is possible (try alt-M)

  * (Done: Lazarus 1.0+: context-sensitive help is included with at least the windows installer. See [Installing Help in the IDE](<Installing_Help_in_the_IDE.md> "Installing Help in the IDE") ~~Inline help for a command/reserve word that the mouse cursor/keyboard caret are under that word (not instead of the tooltip that exists today when the mouse is hover the command/variable).~~


  * (Done) ~~When pressing on CTRL on a file/variable/type etc.. that we are trying to load, it will place a tooltip telling what is going to be opened (the file + the line number)~~ (The information is shown when hovering over the variable)


  * Done in 0.9.29 ~~Ability to save and load color scheme for syntax highlight.~~


  * Done (present in at least 1.2): see Edit/Find in Files which has a replace option ~~Within the "Edit" menu, add an option called "Replace in files...", which allow find and replace text in all the units of the application, doing it at once.~~



### Improvement of the menu and toolbars

  * to allow end user customization
  * to obtain more styled menu (similar as Shagrouni xp-menu style)



## Integrated and Working Debugger

Debugging from inside the IDE can be a frustrating process. I believe this is a combination of GDB being more C/C++ centric, or because it is a more generalized standalone debugger. Either way, I think the ultimate solution is to write a custom debugger for use with FPC and Lazarus IDE. This way it will be possible for the debugger to support ALL Object Pascal and Lazarus IDE requirements, or maybe even be compiled into the Lazarus IDE itself. GDB does support Object Pascal, but not nearly at the level it does with C/C++. 

An IDE without sufficient debugging support is a hard sell to would-be contributors or Delphi converts. This might also have contributed to some of the negative comments received in the Delphi non-technical newsgroup. Make no mistake, I love the many unique IDE features Lazarus has to offer - it beats Delphi IDE by far. 

Apart from the standard Lazarus debugging solution using gdb there are multiplle custom written debuggers, partly or completely implemented in Object Pascal: 

  * [FpGdbmiDebugger](<FpGdbmiDebugger.md> "FpGdbmiDebugger"): uses gdb but also adds Pascal code. In development, experimental; included with Lazarus
  * **[FpDebug](<FpDebug.md> "FpDebug")** : Located at components\lazdebuggers\lazdebuggerfp\LazDebuggerFp.lkp Currently support Linux and Windows
  * **[Duby](<http://sourceforge.net/projects/duby/>)** : A research project on SourceForge.net.
  * **[fpdebug (fork of duby)](<https://github.com/graemeg/fpdebug>)** : It's a fork of Duby with some further development work. I didn't know about the name conflict with the Lazarus one though. The joys of the FP* prefix. :)



Please try out these projects and contribute if you can. 

## Object Inspector

### Search field

add a "search" text box at the top or bottom (preferred, maybe user settable) of Object Inspector to be able to search for a name or value (maybe with right click menu checkable options on whether Name or Value or both are searched) in the properties/events list 

Question: What about sub properties? 

### Recent page

add a "Recent" tab to Object Inspector apart from the "Favorites" one, would be more useful than the Favorites one since it's automatic (would keep all properties/events that have been edited [fields that have either been changed or ENTER was pressed to CONFIRM/REset their current value - focus lost events shouldn't be considered editing if value of property/event field didn't change]) 

Question: How are items removed from the list? 

  


### Not registered component classes

Add a field in the object inspector to allow specifying the real class that will be instantiated, with that will allow to uses derived classes without remake/link the IDE, of course the new fields for the real class will not be available to edit in designer. An example: We place a TEdit in a form, but we want to use a derived class we made that add a new field/property/method to TEdit and we will use that new field/property/method programming but in design time it's enough to work with TEdit, actually there is an option to change the class but only to another one that's already linked on the IDE. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The class must be registered. This is needed for streaming and RTTI. If you don't want to register the real class, then register a fake class. It would be possible to create an artificial derived class from a registered component class on the fly. This has nothing to do with the OI. A dialog to manage all artificial classes must be implemented and a menu item must be added somewhere.

## Refactoring

### Global Variable/Method renaming

~~Proper renaming (as in if I rename a field in an object it can change everything that uses it but NOT other stuff that happens to have the same name) -Plugwash~~

    Already implemented: See Source Editor -> popup menu -> refactoring -> rename identifier

### Parameter Renaming in Methods

adding a parameter to a method (and updating everything that uses it) -Plugwash 

    How should that work? What should the IDE/codetools put as parameter at the places? 

    You specify that when you ask for the parameter to be added. [Plugwash](</User:Plugwash> "User:Plugwash") 05:09, 30 September 2006 (CEST)
    For example, you add a parameter "b: boolean". The method is used a 100 times.

  * Possibility 1: You add a default parameter too. No change to the using methods is needed.
  * Possibility 2: You press compile and the compiler will jump to every occurence and you can decide, what to do.
  * Possibility 3: A new dialog is added to the IDE, where you can specify to insert a ",false" at every occurence.
  * Possibility 4: ?



Do you have 3 in mind or 4? 

### Parameter reordering

like Delphi. A dialog to add, remove, reorder parameters of a function, method, property. 

### Improvement of the IDE like GExperts does for Delphi

  * Done: see Project/Publish project ~~Backup tool...~~
  * Tab : provision to set Tab spacing



    Question: please indicate what you specifically miss in Tools/Editor/General/Tab and Indent

  * Rename Components (where it is possible too, to fill some properties in this moment. f.e. Button: Caption, Width, Height, Action)



## I have a dream of components : Jedi

Sure I switch immediately from Delphi to Lazarus. 

  * The Jedi components are a mix of OS dependant, Borland dependant and architecture specific code. I doubt these will ever work on anything but windows/x86. (and maybe win64 with a bit shoehorning). Example: To use a string routine in the strings unit of JCL, you have to fix like 12 units, some of which poke around in the Borland specific debug code. And then you haven't even reached the units of the JVCL, besides this there is a [licensing](<licensing.md> "licensing") problem. [Marcov](</User:Marcov> "User:Marcov") 18:27, 4 October 2008 (CEST)
  * A lot of Jedi code has been implemented in FPC already. Question: do you need more? What components exactly? --[BigChimp](</User:BigChimp> "User:BigChimp") 15:16, 28 March 2014 (CET)



## Code completion

### Create/Free objects

Declare an object myObj:TMyClass into a TForm class an type some Ctrl-Shift-Key, then the IDE adds a line myObj:=TMyClass.Create in FormCreate and myObj.Free in FormDestroy. -Rednaxel 

### Add list properties

Declaring an indexed property like 'property Items[i: integer]: TAType' and using this feature (Ctrl-Shift-Key, Menu), should popup a dialog to setup some parameters and then it should add a private variable fItems: TFPList; (or 'array of' or TList or ^AType), create code in the constructor, destroy code in the destructor and access methods GetItems, SetItems. 

## Visual Form Inheritance

VFI (Visual From Inheritance) works in Lazarus but there are some missing features: 

  * When ancestor and descendant are open in the IDE and a property of the ancestor is changed, then change the descendant too.



## Conditional Compiler Options

See [Conditional Compiler Options](<Conditional_Compiler_Options.md> "Conditional Compiler Options"). 

## Help

### A small tutorial for Object Oriented Programming

I wish to have in Lazarus as in Eclipse in the Help menu a Tutorial for Object Oriented Programming. 

  * This is a good idea, but I think a **offline** help browser is first required. Lucky for you, I already implemented such a beast, called [DocView](<http://sourceforge.net/projects/fpgui/files/fpGUI/1.4/>). :-) I also have a few old programming manuals around that could contribute to the contents. I know Borland released the OOP chapter what was included in the Turbo Pascal v5.5 & v6 manuals for everybody to use (freely downloadable). So maybe that will be the best place to start. A downloadable copy of the OOP Chapter can be found [here](<http://geldenhuys.co.uk/articles/>). I also have freely available INF e-books (viewable with DocView) that cover the introduction to Object Pascal and OOP programming (follow [this](<http://geldenhuys.co.uk/~graemeg/inf_books/>) link) --[Ggeldenhuys](</User:Ggeldenhuys> "User:Ggeldenhuys") 13:44, 29 November 2013 (GMT)
  * Offline help has been included since Lazarus 1.0x with lhelp and the chm format. A help menu item that shows lhelp is included with Laz trunk. Maybe the .inf books could be converted to chm and included with the Lazarus installer --[BigChimp](</User:BigChimp> "User:BigChimp") 14:21, 30 November 2013 (CET)



## Better Macro support by making them easier to find

See the feature request [#14558](<http://bugs.freepascal.org/view.php?id=14558>) in Mantis. In summary, add a button next to EditBox controls which support macro usage. Clicking the button will display a popup menu with available macros at that level and insert the selected macro into the EditBox at the cursor position. 

## Copy and Paste Support for a lazy programmer (in properties-editor)

~~Maybe it's by design (because of the same behaviour under win and mac)... but I think, it's a little bit of annoying. When I try to copy the chosen "name" of a component out of proterties-editor by selecting it and ctrl+c, I will not get the text of the name, but the object/component description itself (which is useless, I guess). There is not even a "copy" item in the context-menu. I use very long descriptive names for components (and variables) and it would be nice, to get these unhandy names somehow into the clipboard.[KaLi](</index.php?title=User:KaLi&action=edit&redlink=1> "User:KaLi \(page does not exist\)") 14:56, 30 June 2010 (CEST)~~

    This seems to work in 0.9.31, both Ctrl+C and via context-menu [Bart](<http://wiki.lazarus.freepascal.org/User:Bart>)

## Code Library Window or Tab

~~It would be great to have a built in location to store all those useful pieces of code. Ones that you have created, or found useful.~~

~~My ideal feature list would be:~~

~~

  * Save and Load my personal code
  * Attach Zip file ( useful for keeping a sample project, showing the code in use).
  * Ability to Upload Code / zip to central online repository.
  * Ability to synchronize or select repository code and download to my own code store.
  * Code could be sectioned into well thought out categories, like Types of controls, Libraries, complete project examples, Tutorials, etc.
  * A place to show the most recent code submitted by Category.

~~~~~~

It would also be a good place to help people who need some assistance. I would have the following options:

~~~~

~~
  * Option to Ask for help on a control or subject matter.
  * Notification when a solution is uploaded to the repository.
~~
  * ~~Maybe a points system rating the assistance given out of 10 ( 1 being not useful, 10 being just what I needed)~~



    The "Code Librarian" third party code library tool is available made by Lazarus forum user taazz. Helping him with incorporating these features (if he wants to) could be the best way of getting this functionality: <http://forum.lazarus.freepascal.org/index.php/topic,22752.0.html> and <https://sourceforge.net/projects/codelibrarian/>

## Write and run unit tests from subpackage of project being tested

The main idea is to have ability to write and run Unit tests from a subpackage of main project. **Please see[Feature Ideas/UnitTests](<Feature_Ideas/UnitTests.md> "Feature Ideas/UnitTests")**

# LCL

## Components

Components that are possible to make that will work across all platforms supported by Lazarus 

  * Do you mean customdrawn components? If so, that is already possible. I think the TStringGrid is a customdrawn component. Alternatively, look at [fpGUI](<fpGUI.md> "fpGUI") for a completely customdrawn GUI toolkit. --[Ggeldenhuys](</User:Ggeldenhuys> "User:Ggeldenhuys") 23:56, 30 September 2009 (CEST)



### Visual

  * The existing **TDBLookupComboBox** , could be greatly enhanced by something like an "**.IsFiltered** property": If true, the control should reduce its list of visible (and hence selectable) values in the dropdown list by filtering the DataField by the current DBLookupComboBox while editing. This means, onChange (resulting in DBLookupComboBox.Text updated to someString) automatically reduces the list to **SELECT lookup_field FROM lookup_table WHERE lookup_field LIKE %'+someString+'%**. A feature like that would greatly enhance the usability of Lazarus made database applications!   
To illustrate this idea, compare other similar projects: [1](<https://www.freelancer.com/projects/Delphi/Custom-Delphi-TDBLookupComboBox/>) [2](<https://www.devexpress.com/Support/Center/Question/Details/B196085/filtering-dataset-of-tcxdblookupcombobox>) or [3](<https://www.drupal.org/sandbox/jauzah/1748434>), or this [screenshot](<https://www.drupal.org/files/project-images/select2element_screen.jpg>) .


  * Already implemented [Add Help to Your Application](<Add_Help_to_Your_Application.md> "Add Help to Your Application"): ~~Help system for apps created by Laz! It is a plugin system. That's why it supports cross-platform help system and using the OS's native help system.~~


  * The current LCL TStringGrid is a nice improvement over Delphi's feeble control with the same name. Would be great if it could be improved even more! 
    * ~~Support for data validation event handler, similar to Orpheus grid's OnUserValidation event.~~ _There are many events, for example OnEditingDone, and OnValidateEntry to do this. See[Grids Reference Page](<Grids_Reference_Page.md> "Grids Reference Page") and [Event order](<Event_order.md> "Event order")_
    * Support for retrieving, displaying, editing and storing data that are stored in an existing memory structure or in a file rather than in grid itself, similar to the Orpheus grid's OnGetCellData and OnGetCellAttributes events. _There is the TTIGrid, that can show TCollection, TList and TFPList of TPersistent. What else do you need? Additionally, you can bind a TBufDataset to a dbgrid..._
    * A TImage component wich support transparent winxp alpha 32 bit icon images
    * ~~Support for RightToLeft BidiMode.~~ Fixed in 0.9.31 according to <http://bugs.freepascal.org/view.php?id=18918>. Bug report tracking all RTL/Bidi: <http://bugs.freepascal.org/view.php?id=19303> If there are still bugs, please report them to the bug tracker.
  * TTrayIcon 
    * Add a TImageList property in TTrayIcon to change TrayIcon icon or animate it



### NonVisual

  * Clipboard and ~~TTimer~~ that don't depend on LCL. If you use these in a library or console app, the compiler drags in a lot of unused and unneeded LCL code. If you have multiple DLL's, this code is duplicated in each. Current implementations appear to follow the Delphi design approach, where Clipboard and TTimer have dependencies on the VCL - this probably wasn't necessary. 
    * TfpTimer is a non-LCL timer already available in FPC in unit **fptimer.pp**. Online help is available [here](<http://lazarus-ccr.sourceforge.net/docs/fcl/fptimer/index.html>). --[Ggeldenhuys](</User:Ggeldenhuys> "User:Ggeldenhuys") 14:38, 2 September 2009 (CEST)
    * This may be true on Windows, but it´s False for Linux. I recently implemented TTimer for qt interface and the implementation calls Qt API. I think the same applies to Clipboard, since the clipboard depends on the widgetset used too (gtk or qt for example), not only on the operating system. --[Sekelsenmat](</User:Sekelsenmat> "User:Sekelsenmat") 20:40, 5 Mar 2006 (CET)
    * The LCL is a visual component library. None visual components are provided by the FCL.
    * Lazarus will only provide components based on the LCL.



## PDA Support

Something that currently does not exist is a complete multiplatform solution for PDAs. If we can implement this, Lazarus will become the first multiplatform RAD for PDAs ever created. Some devices work with Java, some work with c++ and Qtopia, some work with their particular APIs, there is no single solution currently that can embrace a large portion of PDAs. Application have to be redone for every PDA or simply just run on one of them (what most people do). 

The LCL implements many things. It is not necessary to implement them all to have a reasonably working Lazarus PDA widgetset. The most important things to be implementeded for PDAs are: 

  * TApplication
  * TForm
  * TScreen
  * TCanvas and painting
  * Most Events (onClick and OnPaint at least)
  * Basic widgets like: TButton, TLabel, TEdit, TMemo.
  * Only minimum necessary LCLIntf functions to make TCanvas work. (GetDC, ReleaseDC, BeginPaint, EndPaint and some painting functions)



Others can be easily implemented by people who need them after those are ready. 

Here is a list of some PDAs and the status of their support on Free Pascal / Lazarus: 

  * Windows CE - The compiler already works really well cross compiling for Windows CE. The [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") is under implementation.
  * Symbian OS - Possible for their ARM devices. Requires first adding a new Runtime Library for this OS to the Compiler. This OS has it's own API. There are open source projects for python and c to work on Symbian OS, so Free Pascal should work as well when a RTL is written.
  * Qtopia - For Zaurus and other linux-based PDAs. This is much easier now that basic Qt4 widgetset has being implemented, because Qtopia is very similar to Qt4. Some magick on the bindings is expected to make the same widgetset code to run for both Qtopia and Qt4.
  * Java - Some handheld devices, specially phones, only support java bytecode. May seam a little odd, but there is a Pascal Compiler capable of compiling the source into a jar and then into bytecode. It's name is Midlet Pascal <http://www.midletpascal.com/> It may be possible to develop a similar technology that adds a java bytecode target for Free Pascal. Some people already start porting midletpascal to delphi [http://forum.boolean.name/attachment.php?attachmentid=7975&d=1255392966](<http://forum.boolean.name/attachment.php?attachmentid=7975&d=1255392966>), follow instructions on <http://www.pascalgamedevelopment.com/forum/index.php?topic=5466.0> to access it.
  * Palm OS - PalmOS 5 Garnet support requires that first the 68k compiler is working (runs on ARM processor with a kind of emulation) and then a RTL for palmos needs to be written. Only Free Pascal 1.0 supports 68k, this wasn't updated and the new compiler 2.0 doesn't support it yet (work in progress by [fpc:User:Chain-Q](<http://www.freepascal.org/wiki/index.php/User:Chain-Q> "fpc:User:Chain-Q")), so will be a problem.



PalmOS 6 Cobalt is fully arm, so it only requires a rtl, but it has zero devices working on it currently and won't be on the market until 2007 at least. 

Other compilers that support this target: [Pascal & Palm Compiler](<http://www.ppcompiler.org>) and [HsPascal](<http://hspascal.fihl.net/>). Unfortunately those compilers are not open source, so we can't use their code, we need to implement PalmOS RTL from zero. 

## More Interfaces

  * Cocoa - ~~Requires that first OpenSTEP bindings are created for Free Pascal.~~ Cocoa interface support has been started; see [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface")
  * BeAPI - interface for Haiku, ZetaOS and BeOS. There is no powerful RAD on this OS. 
    * Bindings to the BeAPI has been started - though not part of the Lazarus project. Have a look at SourceForge [befpc](<http://befpc.sourceforge.net/>) project.
  * [SDL](<http://www.libsdl.org>) \- something like what was done in [Pixel Image Editor](<http://www.kanzelsberger.com/pixel/>).
  * MacApp 2.0 - For 68k-based Mac computers. MacApp was Apple Computer's primary object oriented application framework for the Mac OS for much of the 1990s. First released in 1985, it is arguably the first such system to be widely used, notably on a microcomputer platform. Microsoft's MFC and Borland's OWL were both based directly on MacApp concepts. This framework was completely written on Object Pascal, so it should integrate really nicely with Free Pascal and Lazarus. When Apple moved to PowerPC it also dropped Object Pascal support, but that's another story. A emulator for 68k-Mac can be easely set up with [BasiliskII](<http://basilisk.cebix.net/>). There is also a project called MacApp2PPC that upgraded MacApp pascal code to run on Power PCs.
  * OS/2 native - Free Pascal works well with OS/2, so this should be quite possible.
  * ~~Solaris - OpenSolaris versions exist and it's quite similar to Linux so why not ?~~ For information on the Solaris port of Lazarus, see: [Lazarus on Solaris](<Lazarus_on_Solaris.md> "Lazarus on Solaris")
  * Web Interface - Application compiles to an embedded HTTP Server who serves forms in ajax pages (or compiles it to an Apache Module), like Intraweb in Delphi. Discussion in [http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&t=2489](<http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&t=2489>)
  * Already implemented: ~~[fpGUI](<http://fpgui.sourceforge.net/>) \- A widget set written in Free Pascal without any large library requirements. fpGUI talks directly to the underlying graphics library and is very easy to port to new platforms. Currently it supports Linux, ARM-Linux, FreeBSD, Windows and WinCE.~~ \- Under development here: [fpGUI Interface](<fpGUI_Interface.md> "fpGUI Interface")



### improved EventQueue

  * [Lazarus improved Event Queue](<Lazarus_improved_Event_Queue.md> "Lazarus improved Event Queue")



### Lua

  * Support for Lua interfacing. (Binary modules?) 
    * What is Lua interfacing? Is it a graphical widgetset? What is the difference between what you want and is supplied by the lua units of fpc: <http://svn.freepascal.org/svn/fpc/trunk/packages/lua/>
    * Perhaps the poster wanted to use the Lua scripting language to access the LCL widgetset. Had a quick look at the FPC lua package mentioned above: no readme, no test, no demo, so not easy to see what that package does/can do. Perhaps it can be used as-is to do what the poster wants; in that case, some additional samples/documentation may be handy. BigChimp, September 2012



# Visual Improvements

## An idea for a splash screen

[![splash logo2 .png](https://wiki.freepascal.org/images/e/ea/splash_logo2_.png)](</File:splash_logo2_.png>)

The font for "write once..." is DejaVue Serif Condensed, for "Lazarus" is Phantom, and for "object pascal" is Tahoma. 

If you want to change the splash logo on Windows, in your main Lazarus installation folder find "images" folder and there find "splash_logo.bat". Then edit and run it or replace "splash_logo.png" and run "splash_logo.bat". 

### critique

  * as if people's desktops are not already cluttered to the max... not desirable



# FPC

## Per-type Byte Alignment

A means to align variables and records in a unit is already possible with compiler directives, but is somewhat messy and might cause conflicts in some situations. What I propose is an addition to the Free Pascal syntax that forces all variables of a defined type to be aligned to a particular boundary. e.g. 
    
    
      type
        AlignedDouble = Double align 16;
    
      type
        TVector4f = packed record
          X, Y, Z, W: Single;
        end align 16;
    
      type
        TVector4fArray = array of TVector4f align 32;
      { tighter restrictions than TVector4f, so it's okay. Note that this should probably align Var[0] to a 32-byte boundary, not necessarily Var as a whole }
    

Such aligned types are useful and even necessary for when working with SSE and AVX, where significant speed gains can be achieved with aligned data (and some operations require it). 

Apparently Delphi supports something like this already, but it isn't well-documented. 

### Nuances

  * The exact syntax.
  * Cross-platform support. Do all platforms support memory alignment?
  * Exact error messages or behaviour if you specify a non-power of 2 alignment.
  * What happens if you try to typecast an unaligned variable to an aligned type? (Aligned to unaligned should be fine)
  * What happens if you try to typecast a pointer to an unaligned type to a pointer to an aligned type?
  * What happens if you, say, declare a type aligned to 16 bytes, and then declare another type that is equal to it but is only aligned to 8 bytes?
  * What happens if you, say, declare a type aligned to 16 bytes, then use it as a field in a record type, aligned or otherwise?



[CuriousKit](</index.php?title=User:CuriousKit&action=edit&redlink=1> "User:CuriousKit \(page does not exist\)") ([talk](</index.php?title=User_talk:CuriousKit&action=edit&redlink=1> "User talk:CuriousKit \(page does not exist\)")) 18:28, 5 December 2017 (CET) 

## "vectorcall;" modifier for Win32 and Win64

This function modifier is the only unique modifier for Win64 (although it can be used in Win32 too), introduced by Microsoft at the behest of game developers who wish to reduce the overhead of passing vectors and the like into subroutines. The basic details are the same as the regular Win64 calling convention, but attempt to vectorise certain record types and pass them in their entirety through a single XMM register (something which is already possible in Linux64 sometimes). Details can be found here: <https://msdn.microsoft.com/en-us/library/dn375768.aspx>

This may require the byte alignment feature described above. 

    vectorcall is implemented in (the upcoming) fpc 3.2 for the win64 platform. [Bart](</User:Bart> "User:Bart")

### Nuances

  * What happens if it is used on a platform other than Win32 or Win64? Does a compiler error occur or is it ignored with a warning?



[CuriousKit](</index.php?title=User:CuriousKit&action=edit&redlink=1> "User:CuriousKit \(page does not exist\)") ([talk](</index.php?title=User_talk:CuriousKit&action=edit&redlink=1> "User talk:CuriousKit \(page does not exist\)")) 23:04, 5 December 2017 (CET) 

  *     * It is ignored with a warning. [Bart](</User:Bart> "User:Bart") ([talk](</User_talk:Bart> "User talk:Bart")) 17:43, 20 December 2022 (CET)


  * As an example of prior art I think Vector Pascal[[1]](<https://sourceforge.net/projects/vectorpascalcom/>) is noteworthy. [MarkMLl](</User:MarkMLl> "User:MarkMLl") ([talk](</User_talk:MarkMLl> "User talk:MarkMLl")) 09:12, 6 December 2017 (CET)



## "default"-value in a proc or function call

  * There is a limitation in the freepascal-Syntax where you can not depend on the default-parameter lefthand from an explicit parameter. I suggest a possibility where you can say "default" in a function-call, and set other parameters at the same time.



  

    
    
    program DefaultParameter;
     
    {$mode objfpc}{$H+}
     
    type
       Pilots = (Charles, Amelia, Belly, Louis);
     
    procedure Fly(Pilot: Pilots = Belly; Copilot: Pilots = Charles);
    begin
      if (Pilot=Copilot) then
       halt(1);
    end;
     
    begin
       Fly; // we fly with Belly+Charles
       Fly(); // we fly with Belly+Charles
       Fly(Amelia); // we fly with Amelia+Charles
       Fly(Louis, Belly); 
     
       // The following "default" is my Feature Idea
       //  │
       //  v
       Fly(default, Amelia); // we fly with Belly+Amelia
    end.
    

### Critique

  * If such a feature should exist, wouldn't it be more straightforward to simply not specify anything before the comma? (i.e. "Fly(, Amelia);") [CuriousKit](</index.php?title=User:CuriousKit&action=edit&redlink=1> "User:CuriousKit \(page does not exist\)") ([talk](</index.php?title=User_talk:CuriousKit&action=edit&redlink=1> "User talk:CuriousKit \(page does not exist\)")) 14:50, 7 December 2017 (CET)



## "pure;" function directive

I propose a new directive keyword that can be specified for functions, named **pure**. Its intention is to act as a hint to the compiler that the function is fully self-contained, has no side-effects (although the compiler will actually check if this is the case) and is guaranteed to return the same output for a given input. An example would be the Int function, which returns the input value with the fractional part stripped off, and nothing else. 

So what would be the advantage of a function being 'pure'? With a pure function, if the input values are constants, then the output(s) will also be constants, hence they can be pre-calculated during compilation and the function call completely replaced with an assignment, offering a massive speed boost and providing a sort of general-purpose means of doing what some internal compiler routines do in regards to replacing calls with static values. 

There are potentially two ways that the compiler can determine if a function is pure: 

  * Perform deep analysis of the pre-compiled assembly language, almost like an emulator (platform-dependent, but will allow pure assembler routines to be considered for function purity, which may be beneficial for operations such as ROL, ROR, BSF and BSR).
  * Run the subroutine's source code through PascalScript (largely platform-independent), and if unresolved reference errors occur, then the function can be considered impure.



In this case, the **pure** keyword gives a hint to the compiler as to which procedures to check for purity, so it doesn't waste a potentially long time on routines that the programmer already knows are impure. 

If a function is marked as **pure** but it turns out not to be, the compiler throws a warning much like what happens if it's marked as **inline** , but something makes it an invalid candidate. The following paradigms would make a function a non-eligible candidate: 

  * Reading from or writing to external memory (including static and global variables)
  * Reading from or writing to I/O
  * Contains **var** parameters (since, by definition, they cannot be a constant)
  * Dereferencing a pointer (which also means object methods cannot be made pure, including **constructor** and **destructor** since, by definition, they allocate and deallocate memory related to object instantiation. I don't think class methods can be pure either) - the only exception to this are **out** parameters, since their type and size are tightly controlled and the dereferencing is effectively hidden from the programmer anyway.
  * Allocating or deallocating memory
  * Obtaining the address of something (using the @ operator)
  * Calling another function (unless that function is also pure, in which case, pre-computing an output becomes a recursive operation. Note that actual recursion, where a function calls itself, is allowed and may be required in places... for example, with an implementation of a factorial function)
  * There are code paths where uninitialized values can be read (this includes reading an **out** parameter prior to writing to it).



There are probably other things that prevent a function from being pure. 

  
**pure** will generally only be used on **function** s; **procedure** is nevertheless valid because the output may be an **out** parameter, so long as the procedure's body follows all of the rules of a pure function. 

### Nuances

  * What happens if the pre-computation of an output value would cause an exception to be raised (e.g. division by zero)?
  * What happens if the pre-computation would cause an infinite loop? How well can this be detected via finite state analysis rather than a timeout? (It's likely very difficult to do it purely via finite states - see [Turing's Halting Problem](<https://en.wikipedia.org/wiki/Halting_problem>)).
  * What happens if a constant is declared to equal a pure function (e.g. "**const** QuarterPi = atan(1);" if atan is declared and proven pure). On the surface this seems perfectly valid, but what happens if optimizations are turned off?



[CuriousKit](</index.php?title=User:CuriousKit&action=edit&redlink=1> "User:CuriousKit \(page does not exist\)") ([talk](</index.php?title=User_talk:CuriousKit&action=edit&redlink=1> "User talk:CuriousKit \(page does not exist\)")) 01:20, 17 June 2018 (CEST) 

  


## Support for Traits

As a user who never got warm with the concept of interfaces, I'd like to suggest support for traits [[2]](<https://en.wikipedia.org/wiki/Trait_\(computer_programming\)>). 

## Support for Promises

I know there are TThread.ExecuteInThread, Synchronize, Queue, etc. But JavaScript's promises [[3]](<https://en.wikipedia.org/wiki/Futures_and_promises>) are so intuitively to use that I'd love to see support in FPC. Could be an enormously handy feature when dealing with tasks for which, on the one hand, TThread would be overdimensioned, but on the other hand, doing the task in the main thread would on the other hand cause lags. I'm especially thinking about web requests, loading data from a file (which could be on a network drive), etc. 

# Other

## Fixes branch releases

Already done: ~~Currently we only have snapshots of the fixes branch, and it would be nice to also have them as releases. To have releases, those builds would have to be tested by someone, so I volunteer for that. What is needed is a testing checklist, and the start of it can be found on my page,[User:Arny](</User:Arny> "User:Arny"). Any further ideas are welcome!~~

  * This is already done and maintained for v0.9.28 onwards of Lazarus. Downloads of _fixes_ releases are available on SourceForge.



## See Also

  * [Bounties](<Bounties.md> "Bounties") \- A way to make sure your feature idea is implemented.
  * [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report") general information on submitting bugs, what should be covered in a bug report, and using the bug tracker.
  * [Tips on writing bug reports](<Tips_on_writing_bug_reports.md> "Tips on writing bug reports") detailed information on what should be covered in a bug report.
  * [Database bug reporting](<Database_bug_reporting.md> "Database bug reporting") Specific info and sample programs for database bugs
  * [Moderating the bug tracker](<Moderating_the_bug_tracker.md> "Moderating the bug tracker")

---

_Source: [https://wiki.freepascal.org/Feature_Ideas](https://web.archive.org/web/20250319103849/https://wiki.freepascal.org/Feature_Ideas)_
