# Lazarus Tutorial

│ **[Deutsch (de)](</Lazarus_Tutorial/de> "Lazarus Tutorial/de")** │  **English (en)** │  **[español (es)](</Lazarus_Tutorial/es> "Lazarus Tutorial/es")** │  **[suomi (fi)](</Lazarus_Tutorial/fi> "Lazarus Tutorial/fi")** │  **[français (fr)](</Lazarus_Tutorial/fr> "Lazarus Tutorial/fr")** │  **[magyar (hu)](</Lazarus_Tutorial/hu> "Lazarus Tutorial/hu")** │  **[italiano (it)](</Lazarus_Tutorial/it> "Lazarus Tutorial/it")** │  **[日本語 (ja)](</Lazarus_Tutorial/ja> "Lazarus Tutorial/ja")** │  **[македонски (mk)](</Lazarus_Tutorial/mk> "Lazarus Tutorial/mk")** │  **[Nederlands (nl)](</Lazarus_Tutorial/nl> "Lazarus Tutorial/nl")** │  **[português (pt)](</Lazarus_Tutorial/pt> "Lazarus Tutorial/pt")** │  **[русский (ru)](<../ru/Lazarus_Tutorial.md> "Lazarus Tutorial/ru")** │  **[slovenčina (sk)](</Lazarus_Tutorial/sk> "Lazarus Tutorial/sk")** │  **[shqip (sq)](</Lazarus_Tutorial/sq> "Lazarus Tutorial/sq")** │  **[中文（中国大陆） (zh_CN)](</Lazarus_Tutorial/zh_CN> "Lazarus Tutorial/zh CN")** │  **[中文（臺灣） (zh_TW)](</Lazarus_Tutorial/zh_TW> "Lazarus Tutorial/zh TW")** │    
****

Lazarus is a free and open source development tool for the [Free Pascal compiler](<FPC.md> "FPC"), which is also free and open source. The Lazarus [Integrated Development Environment](<IDE.md> "IDE") (IDE, see [Screenshots](<Screenshots.md> "Screenshots")) is a programming environment to create standalone [graphical](<Graphical_User_Interface.md> "Graphical User Interface") and [console applications](<Command-line_interface.md> "Command-line interface"). 

Lazarus currently runs on [FreeBSD](<Portal_FreeBSD.md> "Portal:FreeBSD"), [Linux](<Portal_Linux.md> "Portal:Linux"), [macOS](<Portal_Mac.md> "Portal:Mac") and [Windows](<Portal_Windows.md> "Portal:Windows") and provides a customizable source editor and visual form creation environment along with a package manager, [debugger](<Debugger.md> "Debugger") and complete GUI integration with the Free Pascal [compiler](<Compiler.md> "Compiler"). 

## Contents

  * 1 Getting Started
    * 1.1 Your first Lazarus Program!
    * 1.2 Modify your Program
  * 2 The Editor Windows
  * 3 The Main Menu
  * 4 The Toolbar
  * 5 The Component Palette
    * 5.1 How To Use the Palette
    * 5.2 Customization
    * 5.3 Examples
  * 6 How to use Common Controls
    * 6.1 Ways to Set Properties
    * 6.2 Common Properties
    * 6.3 Event Actions
    * 6.4 Constructors & Destructors
  * 7 Menu controls
    * 7.1 TMainMenu
    * 7.2 TPopupMenu
    * 7.3 Menu editor
      * 7.3.1 Setting Shortcuts Programmatically
    * 7.4 ActionList use
  * 8 The Debugger
  * 9 The Lazarus files
  * 10 See also



## Getting Started

After you have [downloaded and installed](<Installing_Lazarus.md> "Installing Lazarus") the Lazarus IDE, which will also install the Free Pascal Compiler, you are ready to launch the Lazarus IDE and create your first program. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** On Linux Ubuntu at least, the command to start Lazarus from a console is "startlazarus". Otherwise, if you installed it from a Debian package, you should have a Lazarus menu entry under Application/Programming. (Issue: In Debian and Ubuntu the main binary and the package was renamed to "lazarus-ide" because the "tct" package already comes with a utility called "lazarus"). 

### Your first Lazarus Program!

From the Main Menu, choose Project-New Project-Application (or: File-New-Project-Application). A new GUI application is created, see also [Form Tutorial](<Form_Tutorial.md> "Form Tutorial"). 

Several windows will appear on the desktop: the Main Menu at the top, the 'Object Inspector' on the left, the 'Source Editor' occupying most of the desktop, and a ready-made 'Form1' window ([form1](<TForm.md> "TForm")) overlying the Source Editor. 

To place a button on the form: on the top Menu window, underneath the menu line, is a row of tabs. If the [Standard tab](<Standard_tab.md> "Standard tab") is not already selected, select it by clicking with the mouse. Then find the [Button](<TButton.md> "TButton") icon (a rectangle with 'OK' on it) and click on that with the mouse. Then click on the Form1 window, somewhere to the left of the middle. A shadowed rectangle labelled 'Button1' will appear. Click again on the Button icon in the Standard tab, and click on the Form1 somewhere to the right of centre: a second rectangle labelled 'Button2' will appear. 

Now click on Button1 to select it. The 'Object Inspector' window will now display the properties of the object Button1. Near the top is a property named 'Caption', with the displayed value 'Button1'. Click on that box, and change 'Button1' to 'Press'. If you hit `↵ Enter` or click in another box, you will see the label of the first button on Form1 change to 'Press'. Now click on the Events tab on the Object Inspector, to see the various events that can be associated with this button1. These include OnClick, OnEnter, OnExit etc. Select the box to the right of OnClick: a smaller box with three dots (... ellipsis) appears. When you click on this, you are taken automatically into the Source Editor and your cursor will be placed in a piece of code related to button1, starting: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      {now type (or copy&paste):}    Button1.caption := 'Press again';
      {the editor has already completed the procedure with}
    end;
    

Press `F12` to select the Form1 window instead of the Source Editor. 

Now edit the properties of Button2: click on Button2 to display its properties in the Object Inspector. Change its Caption property to 'Exit' instead of 'Button2'. Now select the Events tab, and click on the box for OnClick. Click on the ... ellipsis, and you will be taken into the Source Editor, in the middle of another [procedure](<Procedure.md> "Procedure") for button2: 
    
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      {now type (or copy&paste):}   Close;
      {the editor has already completed the procedure with} 
    end;
    

Now Press `F12` to see the Form1 window again. You should save your work now (and frequently!!) by selecting from the Main Menu Project-Save Project As > your_selected_file_name.pas. Next, you will be asked to save a **L** azarus **P** roject **I** nformation file, with the suffix .lpi. (Note: For Lazarus-0.9.30 you will be asked for project name first and then file name). Choose a different name for this file, if both are identical you will get a "duplicate identifier" [compile error](<compile-time_error.md> "compile-time error") (see chapter "The Lazarus files" at the end of this tutorial). 

You are now ready to try to compile. The simplest way to compile is to select 'Run' from the main menu at the top, and then the 'Run' option on the sub-menu. Alternatively you could simply press `F9`. This will first compile and then (if all is well) link and execute your program. 

Several text windows will appear and all sorts of compiler messages will be typed, but eventually your Form1 window will re-appear, but without the grid of dots; this is the actual main window of your application, and it is waiting for you to push buttons or otherwise interact with it. 

Try clicking on the button labelled 'Press'. You will notice that it changes to 'Press again'. If you press it again, it will still say 'Press again'!! 

Now click on the button marked 'Exit'. The window will close and the program will exit. The original Form1 window with the grid of dots will reappear, ready to accept more editing activity. 

### Modify your Program

Re-open your saved Project and on the Form1 window click on the 'Press' button (Button1) to select it. Select the 'Events' tab on the Object Inspector, click on the right side box next to OnClick, click on the ... ellipsis, to go back to the appropriate point in the Source Editor. 

Edit your code to read as follows: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    { Makes use of the Tag property, setting it to either 0 or 1}
    begin
      if Button1.tag = 0 then
      begin
        Button1.caption := 'Press again';
        Button1.tag := 1;
      end else
      begin
        Button1.caption := 'Press';
        Button1.tag := 0;
      end;
    end;
    

Save your work, re-compile and run. The left button will now toggle between two alternative messages. 

The rest is up to you! 

If you prefer to write Console- or text-based Pascal programs (for example if you are following a basic Pascal programming course, or you need to write programs to use in batch mode or for system programming), you can still use Lazarus to edit, compile and run your programs. See [Console Mode Pascal](<Console_Mode_Pascal.md> "Console Mode Pascal"). 

## The Editor Windows

When you launch Lazarus for the first time, a series of separate disconnected or 'floating' windows will appear on your desk-top. 

The first, running right along the top of the desk-top, is titled **project1 - Lazarus IDE vXXXXXX** (which will subsequently be modified to reflect the name of your currently-open project). This is the main controlling window for your project, and contains the Main Menu and the [Component Palette](<Component_Palette.md> "Component Palette"). 

[![Lazarus-IDE.jpg](https://wiki.freepascal.org/images/d/d0/Lazarus-IDE.jpg)](</File:Lazarus-IDE.jpg>)

On the line below the title bar is the _Main Menu_ with the usual entries for File, Edit, Search, View categories and so on, with a few selections that are specific to Lazarus. Below this on the left is a set of symbols (icons which take you rapidly to particular menu commands); and on the right is the Component Palette. 

Under the Lazarus Editor window will appear the 'Object Inspector' window on the left, and the Lazarus 'Source Editor' on the right. There may be another smaller window, labelled 'Form1', overlying the Lazarus Source Editor window. If this is not visible immediately, it can be made to appear by pressing the `F12` key, which toggles between the Source Editor view and the Form view. The Form window is the one on which you will construct the graphical interface for your application, while the Source Editor is the window which displays the Pascal code associated with the application which you are developing. The operation of the Object Inspector is discussed in more detail below while the Component Palette is described. Finally, there may also be a Message window in the lower of the screen: this one is used by Lazarus to show feedback to the programmer, for instance when compiling a program. 

When you start a new project (or when you first launch Lazarus) a default Form will be constructed, which consists of a box in which there is a grid of dots to help you to position the various components of the form, and a bar along the top which contains the usual **Minimise** , **Maximise** and **Close buttons**. If you click with your mouse cursor anywhere in this box, you will see the properties of this form displayed in the Object Inspector on the left side of the desk-top. 

Other windows that may become visible during your work: the 'Project Inspector', which contains details of the files included in your project, and allows you to add files to or delete files from your project; the **Messages** window, which displays compiler messages, errors or progress reports on your project; if Lazarus was launched from a terminal window, the original terminal remains visible and detailed compiler messages are also printed there. 

## The Main Menu

For a detailed description of every menu item which is accessible from the Lazarus IDE Main Menu, please refer to the [Main menu](<Main_menu.md> "Main menu") page. 

## The Toolbar

[![ide coolbar.png](https://wiki.freepascal.org/images/e/eb/ide_coolbar.png)](</File:ide_coolbar.png>)

The small toolbar area, referred to as a Coolbar in the IDE options, to the left of the Component Palette, contains a set of buttons in two lines which replicate these frequently used Main Menu entries: 

    **New unit** , **New form** , **Open** (with a down-arrow to display a drop-down list of recently used files), **Save** , **Save all** , **Toggle Form/Unit** (ie show either form or source code of Unit)

    **View Units** , **View Forms** , **Change Build Modes** , **Run** (ie compile and Run under the debugger), **Pause** , **Stop** , **Step Into** , **Step over** , **Step out** (the last three are debugger functions).

The image above also shows an added button (the same as the Run button image but with a red X) which compiles and runs without using the debugger. You can add and remove buttons by right-clicking on the toolbar and choosing Options. See [IDE Coolbar](<IDE_Window__Environment_Options.md> "IDE Window: Environment Options") for more details. 

## The Component Palette

The **Component Palette** of the [IDE](<IDE.md> "IDE") is a tabbed toolbar which displays a large number of icons representing commonly used components for building forms. 

[![size="100%](https://wiki.freepascal.org/images/b/ba/Component_Palette_Standard.png)](</File:Component_Palette_Standard.png> "size="100%")

[Component Palette](<Component_Palette.md> "Component Palette")  
---  
[Standard/ja](</Standard_tab/ja> "Standard tab/ja") \- [Additional/ja](</Additional_tab/ja> "Additional tab/ja") \- [Common Controls/ja](</index.php?title=Common_Controls_tab/ja&action=edit&redlink=1> "Common Controls tab/ja \(page does not exist\)") \- [Dialogs/ja](</index.php?title=Dialogs_tab/ja&action=edit&redlink=1> "Dialogs tab/ja \(page does not exist\)") \- [Data Controls/ja](</Data_Controls_tab/ja> "Data Controls tab/ja") \- [Data Access/ja](</Data_Access_tab/ja> "Data Access tab/ja") \- [System](<System_tab.md> "System tab") \- [Misc](<Misc_tab.md> "Misc tab") \- [LazControls](<LazControls_tab.md> "LazControls tab") \- [RTTI](<RTTI_tab.md> "RTTI tab") \- [SQLdb](<SQLdb_tab.md> "SQLdb tab") \- [Pascal Script](<Pascal_Script_tab.md> "Pascal Script tab") \- [SynEdit](<SynEdit_tab.md> "SynEdit tab") \- [Chart](<Chart_tab.md> "Chart tab") \- [IPro](<IPro_tab.md> "IPro tab")  
  
Each tab causes the display of a different set of icons, representing a functional group of components. The left-most icon in each tabbed group is an obliquely leftward-facing arrow, called the Selection Tool. 

If you allow the mouse cursor to hover over any of the icons on the Component Palette, without clicking on the icon, the title of that component will pop-up. Note that each title begins with a 'T' - this signifies 'Type' or more accurately 'Class' of the component. When you select a component for inclusion in a form, the [Class](<Class.md> "Class") is added to the **[type](<Type.md> "Type") ** section of the **[interface](<Interface.md> "Interface") ** part of the [Unit](<Unit.md> "Unit") (usually as part of the overall TForm1), and an **instance** of that class is added to the **[var](<Var.md> "Var") ** section (usually as the variable Form1). Any **[Methods](<Method.md> "Method") ** that you design to be used by the Form or its Components (ie [Procedures](<Procedure.md> "Procedure") or [Functions](<Function.md> "Function")) will be placed in the **[implementation](<Implementation.md> "Implementation") ** part of the Unit. 

#### How To Use the Palette

To use the palette, there must be an open form on view in the editor (if there isn't one, select File -> New Form). Click on the icon in the appropriate tab of the palette for the component you want to use, then click on the form, near where you want the component to appear. When the desired component appears, you can select it by clicking with the mouse. Once selected on the form, the object is also selected in the Object Inspector window, where you can edit its properties and events. 

Adjustments to the visual appearance of an object can be made either by altering the picture itself on the Form using the mouse, or by changing the relevant Property in the Object Editor for that component. 

If you install additional components, either those you have written yourself, or some coming as a package from some other source, then extra tabs with the relevant icons will appear in your Component Palette. These new components can be selected and used on your forms in the same way as those supplied by default. 

In the following list of the Components, you will find links to files that contain descriptions of the Units in which they are found. If you want to find out about the properties of a particular component, it is often worth looking at the Inheritance of that component and then inspecting the properties of the base type from which it is derived. For example, to understand [TMaskEdit](<TMaskEdit.md> "TMaskEdit") it is also useful to examine [TCustomMaskEdit](</index.php?title=TCustomMaskEdit&action=edit&redlink=1> "TCustomMaskEdit \(page does not exist\)"). 

### Customization

Using the menu [Tools|Options] or `Crtl-Shift-O` it is possible to change layout and visibility of the component palette items. 

[![IDE Options - Environment - Component Palette.png](https://wiki.freepascal.org/images/a/ab/IDE_Options_-_Environment_-_Component_Palette.png)](</File:IDE_Options_-_Environment_-_Component_Palette.png>)

### Examples

Several useful [dialog procedures or functions](<Dialog_Examples.md> "Dialog Examples") don't appear on the palette, but are easily used as direct calls from your [source](<Source_code.md> "Source code") [program](<Program.md> "Program"). 

For several good examples of the use of Components see the $LazarusPath/lazarus/examples subdirectory of your source installation. Many of the programs show how to use dialogs and other components directly without using the IDE and component palette or having a separate form definition file: all the components are fully and explicitly defined in the main Pascal program. Other example programs make full use of the IDE. 

Remember to open these examples as projects, with the .lpi file. Opening the .pas source code file and hitting RUN will just append this source file to whatever project you opened last. 

Some examples don't work straight away: you may need to play about with paths and permissions of files or directories. If you want to compile any of the examples, make sure that you have read/write/execute permissions for the files and directories, or copy the files to a directory where you do have the appropriate permissions. 

Try running the 'testall' program to see a menu of the available components together with small example test forms for most of them; then inspect the code to find out how they work! 

## How to use Common Controls

The Units **StdCtrls** , **ComCtrls** and **ExtCtrls** contain definitions and descriptions of many of the most commonly used controls for constructing Forms and other Objects in Lazarus Applications. 

Many of the final target controls that the application developer wants to use, such as **TButton** , **[TMemo](<TMemo.md> "TMemo")** , **[TScrollBar](<TScrollBar.md> "TScrollBar")** etc, have a corresponding ancestor class such as **TCustomButton** , **TCustomMemo** or **TCustomScrollBar**. Several of the properties and methods relevant to the final target control are defined (and explained) more fully in the **TCustomXXX** class, and are **inherited** by the final target control. 

If you drop a component on the form editor you don't need to add code explicitly to create it. The component is automatically created by the IDE together with the form, and destroyed when the form is destroyed. However, if you create the component yourself by code don't forget to free it when it is no longer needed. 

#### Ways to Set Properties

If you place a component on the Form Designer and look at the Object Inspector, you can observe the properties change as you move the component around. 

For example, if you place a button (TButton) on the form, click on it to select it, then move it around the form with the mouse, you can watch the values of Top and Left change in the Object Inspector to reflect the new position. If you use the object's re-sizing bars to adjust its size, you can watch the Height and Width properties change as well. 

On the other hand, by using the Object Inspector, you can select the value associated with a property such as height, and type in a new value; you can watch the size of the object on the form change to reflect the new value. 

You can also explicitly change the properties of the object in code by typing (in the appropriate Implementation section of the Source editor), for example 
    
    
    Form1.Button1.Height := 48;
    

If you type this new value into the Source Editor and then look back at the Form Designer, you will see that the button on the Form has taken the new size. The new value will also be shown in the Object Inspector. 

In summary, there are usually about three different ways to determine each property of an object: 

  * by using the mouse on the form,
  * by setting the values in the Object Inspector,
  * or explicitly by writing code in the editor.



#### Common Properties

The components defined in these Units have several properties that are common to most of them, and other properties that are specific to the individual components. We shall describe the most common ones here. Unusual or control-specific properties will be described for the individual controls. 

Additional Help can always be obtained by selecting a property or keyword, in either the Object Inspector or the Source Editor, and pressing `F1`. You will be taken by your Help browser to the appropriate page in the documentation. 

If the description of a property on that page is insufficient, you can navigate to the corresponding description in the ancestor classes, by selecting the links in the Inheritance listing or by selecting the ancestor Type in the declaration of the object. 

  


**Some commonly listed properties** **Property**| **Meaning**  
---|---  
Action | The main action or event associated with the object. For example selecting an 'Exit' Button might cause the 'Close' action  
Align | Defines the way in which an object is to be lined up with the parent object. Possible values are alTop (placed at the top and using the full available width), alBottom, alLeft (placed at the left and using the full available height), alRight. alNone (place anywhere on parent control) or alClient (takes all available space next to controls aligned to top, bottom, left or right)  
Anchor | Used to keep a control a certain distance from the defined edges of a parent control, when the parent is resized. For example **[akBottom, akRight]** will keep the control a fixed distance from the bottom right corner.  
AutoSelect | When True, an editing control will select all its text when it receives focus or when the Enter key is pressed.  
AutoSelected | True indicate that the edit or combobox control has just performed an AutoSelect operation so that subsequent mouse-clicks and keystrokes proceed normally without selecting the text.  
BorderSpacing | The space around the edge between an **Anchored** control and its parent.  
Caption | The text that is displayed on or near the control; it should preferably give some clue as to the function of the control, or an instruction such as 'Close' or 'Execute'. By default Caption is set to be the same as the 'Name' property, and the application programmer should substitute meaningful text instead of the default values.  
CharCase | Indicates how text is displayed in a text editing control: Normal (retaining the case of the letters typed by the user), converted to uppercase, or converted to lowercase  
Constraints | Sets the minimum and maximum sizes for a control. If a control is resized the new dimensions are always within the ranges given here. You should take care when setting these options that they do not conflict with the Anchors and Align settings.  
Color | The Colour to be used to draw the control or to write the text it contains.  
Enabled | A Boolean property to determine whether or not a control is capable of being selected and performing an action. If it is not **Enabled** , it is often **Grayed** out on the Form.  
Font | The Font to be used for writing the text associated with the control - either the caption or label, or the text-strings contained within the control. The entry on the Object Inspector usually has a (+) box on the left, and selecting this box reveals further options such as character set, colour and size.  
Hint | A short piece of informative pop-up text that appears if the mouse-cursor hovers over the control. See the ShowHint property.  
Items | The list of 'Things' that the object contains, such as a group of images, a series of lines of text, a number of actions in an actionlist, etc  
Lines | An array of strings, containing the textual data in controls with more than a single line of data, such as an Edit-Box or a Combo-Box. The array is zero-indexed, ie the lines are numbered [0..numLines-1]  
Name | The identifier by which the control is known in the program. The IDE gives it a default name based on the underlying type, for example successive instances of TBitButton would be named Form1.BitBitton1 and Form1.BitButton2; it is up to the application programmer to give them more meaningful names such as ExitButton or OKButton. By default the Name of the control is applied to the Caption for the control, but the text of the Caption may be changed separately.  
PopUpMenu | A window containing context-sensitive menu information that pops up when the right mouse button is clicked on the object.  
Position (or Top, Left) | Determines where the control is located on the parent form or window  
ReadOnly | Boolean property which, if True, signifies that the contents of the control can be read by the user or the calling routine, but cannot be written or changed.  
ShowHint | Allows a small window containing a context-sensitive Help or other description to be displayed when the mouse cursor 'hovers' over the control. See the Hint property.  
Size (or Height and Width) | The dimensions of the control  
Style | The options available for Style depend upon the sort of Control being considered: for instance the Style may be defined by TFormStyle, TBorderStyle, TButtonStyle etc.  
TabOrder | Integer defining where in the sequence of tabs on the Form this control is to lie  
TabStop | Boolean property which if True places this control in the sequence of objects that the user can reach by successively pressing the Tab key  
Text | The String of Text that represents the actual data that this control contains. Applies particularly to Text, Memo and StringList types of object. Most of the editing operations (such as **Select** , **Clear** , **Cut** , **Copy**) are performed in this part of the object, which holds the actual string being edited. If the control contains more than a single line of text, for example **TMemo** or **TComboBox** , then the textual elements are arranged as an array of strings (zero-indexed, ie numbered from [0..numLines-1]) in **Lines**.  
Visible | If true, the object can be seen on the Form; if False, object is hidden  
WordWrap | Logical flag to show whether or not word-wrap is enabled, ie if a word comes close to the end of a line and is going to be too long for the line, it is wrapped down to the next line.  
  
#### Event Actions

Many actions are commonly listed in the 'Events' tab of the Object Inspector. If you select an entry in the list, a ComboBox appears with a DropDown list showing any actions that have aleady been defined, and allowing you to choose one to be associated with this event. Alternatively you can select the ellipsis (three dots ...) and you will be taken to an area of the Source Editor where you can begin typing your own action instructions for the selected event. 

While a large number of events is available for any given control, in practice it is only necessary to populate a few of them. For most controls, it is sufficient to provide coding for 'OnClick'; for more complex controls it may be necessary also to provide for 'OnEntry' (when the mouse cursor enters the Control and gives it focus) and 'OnExit' (when the mouse cursor leaves the Control; or you may need to write an event handler for 'OnChange' or 'OnScroll', depending on the nature of the particular control with which you are dealing. 

The pop-up menu that appears when you right-click an object in the Form Designer has, as its first item: 'Create default event' and selecting this option will have the same effect as selecting the ellipsis in the Object Inspector for the default event, usually OnClick: you are taken to the Implementation area of the Source Editor where you can type the code for the event handler. 

A common strategy in Object-Oriented programming is to provide an **[ActionList](<TActionList.md> "TActionList")** with the facility for entering, removing or editing a number of pre-defined actions from which the most appropriate can be selected to use in any particular instance. 

**Some commonly listed Actions** **Action** | **Meaning**  
---|---  
OnChange | Action to be taken if any change is detected (eg mouse move, mouse click, key press, edit text, alter picture, etc)  
OnClick | Action to be taken when the (first, usually left) mouse button is clicked. This is commonly the main or default action of the control; for example clicking on a button or checkbox initiates the action associated with the checkbox. It may alternatively initate a process of selection, for instance in a TextBox or Memo, or signal the beginning of painting with a Pen or Brush.  
Click | A method to emulate in code the effect of clicking on a control. This method is most often found in Button-type controls (TButton, [TBitBtn](<TBitBtn.md> "TBitBtn"), [TSpeedButton](<TSpeedButton.md> "TSpeedButton") etc). A procedure can be written that calls the same code as the OnClick action. This facility can be particularly useful if the activation of one control by clicking causes a cascade of other controls to be activated, and the Click method can be used to initiate the action rather than having the user explicitly click on a lot of controls.  
OnDragDrop | Action to be taken during Drag-Drop manoeuvres, ie when the mouse is used to 'capture' an item or some text etc and move it around the screen to a new location.  
OnEditingDone | Action to be taken when the user has finished all edits/modifications to the control. This is often used to validate the control content (e.g. check if an entered text is a valid IP address)  
OnEntry | Action to be taken when the mouse cursor enters the area occupied by the object, usually transfering focus to that object. This might include changes in the appearance of the object such as highlighting or raising the border.  
OnExit | Action to be taken when the mouse moves out of the area of the object, usually transferring focus out of the object.  
OnKeyPress | Action to be taken for any key-press. Subtly different from OnKeyDown, which simply responds to a key being down, whether or not it was already down when focus was given to this control. OnKeyPress requires that a key becomes pressed while focus is in this control.  
OnKeyDown | Action to be taken if a key is down while focus is in this control. Subtly different from OnKeyPress - for example the key might already have been down when focus entered this control, whereas OnKeyPress requires the key to become pressed while focus is in the control.  
OnKeyUp | Action to be taken if a key is up (ie not pressed) while focus is in this control.  
OnMouseMove | Action to be taken if the mouse cursor moves while focus is in this control.  
OnMouseDown | Action to be taken if the mouse button is down while focus is in this control.  
OnMouseUp | Action to be taken if the mouse button is up while the cursor is over this control. Implies that the mouse button was previously down and has been released. The case where the cursor enters the control but the mouse button has not yet been pressed is covered by OnEntry or OnMouseEnter.  
OnResize | Action to be taken when the control is resized. Might include re-alignment of text or selection of a different font size etc.  
  
#### Constructors & Destructors

These are two special methods associated with each control: 

  * **[Constructors](<Constructor.md> "Constructor"):** such as **Create** allocate memory and system resources needed by the object. They also call the constructor of any sub-objects present in the class.
  * **[Destructors](<Destructor.md> "Destructor"):** remove the object and de-allocate memory and other resources. If you call **Destroy** for an object which hasn't being initialized yet it will generate an error. Always use the **Free** method to deallocate objects, because it checks whether an object's value is **[nil](<Nil.md> "Nil")** before invoking **Destroy**.



Take the following precautions when creating your own **Destroy** method: 

  * Declare **Destroy** with the **override** directive, because it is a **virtual** method.
  * Always call '**inherited Destroy;'** as the last thing on the destructor code
  * Be aware that an **[exception](<Exceptions.md> "Exceptions")** may be raised on the **constructor** in case there is not enough memory to create an object, or something else goes wrong. If the **exception** is not handled inside the constructor, the object will be only partially built. In this case **Destroy** will be called when you weren't expecting it, so your destructor must check if the resources were really allocated before disposing of them.
  * Remember to call **Free** for all objects created on the constructor.



## Menu controls

### TMainMenu

[TMainMenu](<TMainMenu.md> "TMainMenu") is the Main Menu that appears at the top of most forms; form designers can customise by choosing various menu items. 

[TMainMenu](<TMainMenu.md> "TMainMenu") is a non-visual component: that is, if the icon is selected from the [Component Palette](<Component_Palette.md> "Component Palette") and placed on the form, it will not appear at [run time](<runtime.md> "runtime"). Instead, a menu bar with a structure defined by the [Menu Editor](<IDE_Window__Menu_Editor.md> "IDE Window: Menu Editor") will appear. 

### TPopupMenu

[TPopupMenu](<TPopupMenu.md> "TPopupMenu") is a menu window that pops up with pertinent, usually context-sensitive, details and choices when the right mouse button is clicked on a control if the popupmenu is linked to the PopupMenu [property](</Property> "Property") of that component, thus providing a context sensitive menu for that component. 

### Menu editor

To see the [Menu Editor](<IDE_Window__Menu_Editor.md> "IDE Window: Menu Editor"), right-click on the Main Menu or Popup Menu icon on your Form. A pop-up box appears that invites you to enter items into the Menu bar. 

An edit box is displayed, containing a button labeled "New Item1". If you right-click on that box, a pop-up menu is displayed that allows you to add a new item before or after (along the same level) or create a sub-menu with the opportunity to add further items below (or above) the new item in a downward column. 

Any of the [TMenuItems](</index.php?title=TMenuItem&action=edit&redlink=1> "TMenuItem \(page does not exist\)") that you add can be configured using the [ Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"). 

At the least you should give each item a _Caption_ which will appear on the Menu Bar. The caption should indicate the activity to be selected, such as "File Open" or "Close", "Run" or "Quit". You may also wish to give it a more meaningful _Name_. 

If you want a particular letter in the Caption to be associated with a shortcut key, that letter should be preceded by an [ ampersand (&)](<&.md> "&"). The Menu item at run-time will appear with the shortcut letter underlined, and hitting that letter key will have the same effect as selecting the menu item. Alternatively you can choose a shortcut key sequence (such as `Ctrl`+`C` for Copy or `Ctrl`+`V` for Paste - the standard keyboard shortcuts) with the _ShortCut_ property of the [TMenuItem](</index.php?title=TMenuItem&action=edit&redlink=1> "TMenuItem \(page does not exist\)"). 

#### Setting Shortcuts Programmatically

Two functions are provided that convert virtual key to shortcuts and visa versa, KeyToShortCut() and ShortCutToKey(). eg 
    
    
    MenuBold.ShortCut:= KeyToShortCut(VK_B, [ssMeta]);
    

### ActionList use

It is often helpful to use the Menu controls in conjunction with a [TActionList](<TActionList.md> "TActionList") which contains a series of standard or customised [TActions](<TAction.md> "TAction"). Menu items can be linked in the Object Inspector to _actions_ on the list, and the same actions can be linked to [TButtons](<TButton.md> "TButton"), [TToolButtons](<TToolButton.md> "TToolButton"), [TSpeedButtons](<TSpeedButton.md> "TSpeedButton") etc. It is obviously more efficient to re-use the same code to respond to the various events, rather than writing separate _OnClick_ event handlers for each individual control. 

By default, a number of standard actions are pre-loaded from _StdActns_ or, if DataAware controls are used, from _DBActns_. These actions can be chosen using the [ActionList editor](</index.php?title=ActionList_editor&action=edit&redlink=1> "ActionList editor \(page does not exist\)") which appears when you right-click on the [TActionList](<TActionList.md> "TActionList") icon on the Form Designer. 

## The Debugger

    _Still to be written_.

1) Make sure you read the setup: [Debugger_Setup](<Debugger_Setup.md> "Debugger Setup")

2) See also [category: IDE Window - Debug](</Category:IDE_Window_-_Debug> "Category:IDE Window - Debug")

3) Read limitations: [GDB_Debugger_Tips#Inspecting_data_types_.28Watch.2FHint.29](<GDB_Debugger_Tips.md> "GDB Debugger Tips") This page also helps with some platform/version specific issues 

## The Lazarus files
    
    
       (Thanks to Kevin Whitefoot.)
       (Additions by Giuseppe Ridinò, [User:Kirkpatc](</User:Kirkpatc> "User:Kirkpatc") and Tom Lisjac)
    

When you save you will actually be saving two files: 
    
    
      xxx.pas and yyy.lpr 
    

(You save more than that but those are the ones you get to name). The project file (lpr) and the unit file (pas) must not have the same name because Lazarus will helpfully rename the unit (inside the source code) to the same as the unit file name and the program to the name of the project file (it needs to do this or the compiler will probably not be able to find the unit later when referred to in the project file). Of course to be consistent it changes all the occurrences of unit1 to xxx. 

So if you are saving a project called **again** , trying to save again.pas and again.lpr fails because unit names and program names are in the same name space resulting in a duplicate name error. 

So here is what I ended up with: 

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
    

Note that there are many more files than the two that I thought I was saving. 

Here is a brief note about each file: 

**again.exe:** The main program binary executable. Win32 adds an "exe" extension. Linux has none (just the name of the program). This file will be huge in Linux due to the inclusion of debugging symbols. Run the "strip" utility to remove them and substantially shrink the executable size. 

**again.lpi:** (Lazarus Project Information). This is the main information file of a Lazarus project; the equivalent Delphi main file of an application will be the .dpr file. It is stored in an XML format and contains instructions about all the libraries and units required to build the executable file. 

**again.lpr:** The main program source file or master file. Despite its Lazarus specific extension it is in fact a perfectly normal Pascal source file. It has a uses clause that lets the compiler find all the units it needs. Note that the program statement does not have to name the program the same as the file name. This file is usually fairly small, with just a few statements to initialise, build the forms, run and close the application. Most of the work is done in the unit source files, with suffix '.pas' 

**againu.lfm:** This is where Lazarus stores the layout of the form unit, in human readable form. It reflects the properties of the various components, as set in the Object Inspector. Each object description starts with a line: 
    
    
    object xxxx 
      then there follows a list of properties 
      (including embedded or nested objects) then an 
    end 
    

line. Lazarus uses this file to generate a resource file (.lrs) that is included in the initialisation section of the againu.pas unit. Delphi dfm files can be converted to lfm format in the Lazarus IDE using the Tools->Convert DFM file to LFM utility. 

**againu.lrs:** This is the generated resource file which contains the instructions to the program for building the form (if you look in the main Unit file, you will see in the initialization section the line 
    
    
    {$i againu.lrs}
    

which instructs the program to load the resource file). Note that it is not a Windows resource file. 

**againu.pas:** The unit that contains the code for the form; this is usually the only file that the application programmer needs to edit or inspect, and contains any code specifically supplied by the programmer (especially event handlers). 

**againu.ppu:** This is the compiled unit which gets linked into the executable file together with any other units named in the Uses section. 

**ppas.bat:** This is a simple script that links the program to produce the executable. If compilation is successful, it is deleted by the compiler. 

## See also

  * [Lazarus Tutorial Part 2](<Lazarus_Tutorial_Part_2.md> "Lazarus Tutorial Part 2")
  * [Lazarus Form Tutorial](<Form_Tutorial.md> "Form Tutorial")
  * [Howdy World (Hello World on steroids)](<Howdy_World_\(Hello_World_on_steroids\).md> "Howdy World \(Hello World on steroids\)")
  * [Peg Solitaire tutorial](<Peg_Solitaire_tutorial.md> "Peg Solitaire tutorial")
  * [Lazarus IDE Tools](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools")
  * [Lazarus project files](<Lazarus_project_files.md> "Lazarus project files")

---

_Source: [https://wiki.freepascal.org/Lazarus_Tutorial](https://web.archive.org/web/20250314134731/https://wiki.freepascal.org/Lazarus_Tutorial)_
