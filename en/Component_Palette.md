# Component Palette

│ **English (en)** │  **[suomi (fi)](</Component_Palette/fi> "Component Palette/fi")** │  **[français (fr)](</Component_Palette/fr> "Component Palette/fr")** │  **[日本語 (ja)](</Component_Palette/ja> "Component Palette/ja")** │  **[русский (ru)](<../ru/Component_Palette.md> "Component Palette/ru")** │    
****

The **Component Palette** of the [IDE](<IDE.md> "IDE") is a tabbed toolbar which displays a large number of icons representing commonly used components for building forms. 

[![size="100%](https://wiki.freepascal.org/images/b/ba/Component_Palette_Standard.png)](</File:Component_Palette_Standard.png> "size="100%")

Component Palette  
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

---

_Source: [https://wiki.freepascal.org/Component_Palette](https://web.archive.org/web/20250406075244/https://wiki.freepascal.org/Component_Palette)_
