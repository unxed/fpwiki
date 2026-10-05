# IDE Window: Watch list

│ **[Deutsch (de)](</IDE_Window:_Watch_list/de> "IDE Window: Watch list/de")** │  **English (en)** │  **[русский (ru)](<../ru/IDE_Window__Watch_list.md> "IDE Window: Watch list/ru")** │    
****  
****

## Contents

  * 1 Introduction
  * 2 Navigation
  * 3 Overview
  * 4 First steps - Add watches
  * 5 Data - Values
    * 5.1 Resolving variable names (multiple var with same name) - Selected Scope
      * 5.1.1 Scoping limitations and exceptions
      * 5.1.2 Scope (Stackframe, Thread, History)
    * 5.2 Values / Display
      * 5.2.1 Expression
      * 5.2.2 Data Address
      * 5.2.3 Value
      * 5.2.4 Detail pane
      * 5.2.5 Viewing/Expanding Structures (classes/records)
      * 5.2.6 Viewing/Expanding Arrays
    * 5.3 Special Values
  * 6 Interface
    * 6.1 Toolbar
    * 6.2 Context menu
    * 6.3 Drag and Drop
  * 7 Watch Properties Dialog
  * 8 See also



# Introduction

The *Watches* window is part of the Lazarus debugger. 

It displays the values of variables and expressions while an application is paused by the debugger (e.g. at a breakpoint or during stepping). 

# Navigation

The watches window can be opened via 

  * [Main Menu](<Main_menu.md> "Main menu") > [View](<Main_menu.md> "Main menu") > Debug Windows > Watches
  * `Ctrl`+`Alt`+`W`
  * Adding a new watch: 
    * `Ctrl`+`F5` or [Main Menu](<Main_menu.md> "Main menu") > [Run](<Main_menu.md> "Main menu") > Add Watch
    * "Add Watch" tool-button in * [Evaluate Window](<IDE_Window__Evaluate/Modify.md> "IDE Window: Evaluate/Modify") and [Debug Inspector](<IDE_Window__Variable_Inspector.md> "IDE Window: Variable Inspector")



# Overview

[![WatchesWindow 3 0 FpDbg minimum.png](https://wiki.freepascal.org/images/7/7a/WatchesWindow_3_0_FpDbg_minimum.png)](</File:WatchesWindow_3_0_FpDbg_minimum.png>)

By default there are 2 columns in the watches Window. 

  * On the left side the "Expression" shows the name of the variable for which data should be shown.  
This can also be an expression containing multiple variables, operators, index access and more.
  * On the right site the current value of the variable or expression is displayed.  
The display format depends on the value and can be refined in the "#Watch Properties" dialog.



Additional details can be enabled using the #Toolbar.  
The images below show: 

  * The "Data-Address" column
  * The "Inspect-Pane" (on the right side, showing the current watch in more detail)  
(Example: Hex dump of a string)
  * Expanded structured values (objects, classes, records)
  * Expanded arrays



  


More detailed view (FpDebug)[![WatchesWindow 3 0 FpDbg expanded.png](https://wiki.freepascal.org/images/1/1a/WatchesWindow_3_0_FpDbg_expanded.png)](</File:WatchesWindow_3_0_FpDbg_expanded.png>) | More detailed view (Gdb)[![WatchesWindow 3 0 Gdb expanded.png](https://wiki.freepascal.org/images/8/81/WatchesWindow_3_0_Gdb_expanded.png)](</File:WatchesWindow_3_0_Gdb_expanded.png>)  
---|---  
  
# First steps - Add watches

When the debugger paused your app, you can add new values to the Watches window. Either 

  * Select a variable in the source and press `Ctrl`+`F5`
  * Add a new watch from the menu [Main Menu](<Main_menu.md> "Main menu") > [Run](<Main_menu.md> "Main menu") > Add Watch
  * Open the `Ctrl`+`Alt`+`W` and press [![laz add.png](https://wiki.freepascal.org/images/0/07/laz_add.png)](</File:laz_add.png>) from the toolbar.



The new "watch" will be evaluated and the result shown.  
You can also add watches before you start the debugger, or while the app is running. In this case the values will be retrieved as soon as the app gets paused. 

If you step through your code or run the app to the next breakpoint all your watches will be updated each time the app pauses at the next location. 

Expressions can be local or global variables, ([certain](<GDB_Debugger_Tips.md> "GDB Debugger Tips")) properties, or pascal expressions (limited support, e.g. "a+1"). [See here for more information](<GDB_Debugger_Tips.md> "GDB Debugger Tips")

# Data - Values

The data is retrieved from the memory of the debugged app. The debugger then formats it according to the type or the variable. 

## Resolving variable names (multiple var with same name) - Selected Scope

Variable/Identifier names are looked up according to Pascal scoping rules from the current **line of code**. 

"Current line"
    _is the line at which the app is paused (or the line selected via thread and stack)._

The "current line" scoping also applies, if a new watch is added. You may select a variable/identifier anywhere in the code and add it with `Ctrl`+`F5`. It will be resolved according to the current line, even if you selected it in another function (or even if it was a global variable). 

### Scoping limitations and exceptions

The debug info does not always provide all scoping info. 

  * For global variables from other units the correct "lookup order" is not known. 
    * Global Identifiers in all other units will be visible, and can be found in a non-specified order.   
So if two units (other than the current unit) both have a variable "MyValue" (and there is no such variable in the current unit), then the debugger will display one of them, but it may not be the one the compiler would have picked. (see <https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/40173>)
  * Local variables, fields of the object (including if inherited from a base class in another unit), and global variables in the current unit are not affected, but can be if type-casted using a global type declaration.
  * Nested procedures: Local variables from the outer procedure(s) are shown, however this includes variables declared below the nested procedure, which should not be visible. Such an incorrectly show outer local variable may hide another var/field that should be shown instead.
  * This may also depend on the debugger backend, for more information see notes on each debugger backend.



This also allows for more flexible type casting. In case of "Sender: TObject", the instance my be of class "TFooUnit.TMyFoo". Yet the current unit may not be using "TFooUnit". The debugger will still find TMyFoo, if you want to add a watch "TMyFoo(Sender)". The caveat is if you have several classes "TMyFoo" in different units, then (again) the debugger can't tell which one to use. 

### Scope (Stackframe, Thread, History)

The values are evaluated according to the scope set in the [Thread](<IDE_Window__Threads.md> "IDE Window: Threads") and [Stack](<IDE_Window__Call_Stack.md> "IDE Window: Call Stack") dialog. Default is the current Thread and top stack frame. Both (Stack and Frame) dialog offer to change the "current" Frame/Thread. The watch window will follow this selection. 

It is also possible to select previously displayed values, using the [History](<IDE_Window__Debug_History.md> "IDE Window: Debug History") dialog. 

  


## Values / Display

##### Expression

The Pascal expression that is evaluated. Usually the name of a variable. 

Expressions are expected to have Pascal Syntax. However the extend to which this is understood depends on the backend (see each backend from the [Debugger_Status](<Debugger_Status.md> "Debugger Status") page). 

  * Note that some backends (like GdbMi) may accept some pascal operators, but have different operator precedence.
  * FpDebug based backends also offer 
    * [FpDebug-Watches-Intrinsic-Functions](<FpDebug-Watches-Intrinsic-Functions.md> "FpDebug-Watches-Intrinsic-Functions")
    * [FpDebug-Watches-FunctionEval](<FpDebug-Watches-FunctionEval.md> "FpDebug-Watches-FunctionEval")



When expanding structures/arrays, this is a short alias for the element within the expanded data. 

##### Data Address

Some Pascal data types have an internal pointer. For example: Strings, dynamic arrays, instances (of classes). For those the data is not stored in the variable (The address of the variable "@MyVar" will point to the internal pointer, but not the the data in the type / not to the field of the object, or entries in the array). 

Pascal code can access the internal pointer by type casting the variable "PtrUInt(MyVar)". In a way the internal pointer is part of the data, it is stored in the variable. This address can be used for testing equality. That is, if 2 variables refer to the same data (not just identical data). 

The debugger shows this "data address" in the equally name column. 

##### Value

The value of the variable (or result of the expression). 

The format can be influenced by the Watch properties. 

##### Detail pane

More detailed view of one watch. This field will show the selected/focused watch. It provides extra space so more data can be viewed. 

It also supports line breaks in the data, so if the value is formatted with line breaks (structures/arrays, but not strings - they are escaped) then it will be shown accordingly. It may be worth changing the width of the pane so it does no add wrapping, if there are linebreaks already) 

##### Viewing/Expanding Structures (classes/records)

Depends on the backend
    

  * Structures can be expanded, so each field is shown on a single line.
  * This can be repeated for nested structures (and array / see section on arrays)
  * pointers to structures will automatically be dereferenced to allow expanding



The Expression column will only show the name of the field, rather than the full "variable.fieldname". If dereferenced, for each level of dereferencing a "^" will be added. 

Individual expanded entries can be dragged and dropped to the main list (anywhere outside an expanded area). This will create a new (copy) watch for "variable.fieldname". So the particular field can be watched, even if the structure is collapsed again. 

##### Viewing/Expanding Arrays

Depends on the backend
    

  * Arrays can be expanded, so each field is shown on a single line.
  * This can be repeated for nested arrays (and structures / see section on structures(
  * pointers to arrays will automatically be dereferenced to allow expanding



The Expression column will only show the index of the entry, rather than the full "variable[123]". If dereferenced, for each level of dereferencing a "^" will be added. 

Expanded Arrays are shown paginated. The embedded toolbar lets you change between pages. It also allows to increase the page size. 

Individual expanded entries can be dragged and dropped to the main list (anywhere outside an expanded area). This will create a new (copy) watch for "variable[123]". So the particular field can be watched, even if the structure is collapsed again. 

## Special Values

<invalid>
    Value currently not available. Can be caused, if the debugger is not active or the debugged app not currently paused.
<evaluating>
    Value is currently retrieved. A result will be show soon
<disabled>
    The expression is excluded from evaluation. See Disable/Enable buttons (light bulbs)
Error...
    The value could not be evaluated. (Error in Expression or Variable not available in selected scope.

# Interface

## Toolbar

[![debugger power.png](https://wiki.freepascal.org/images/b/ba/debugger_power.png)](</File:debugger_power.png>) Power
    Enables/Disables all updates. This does not affect the enabled/disabled state of individual watches. This will freeze the current display.
[![laz add.png](https://wiki.freepascal.org/images/0/07/laz_add.png)](</File:laz_add.png>) Add
    Add a new expression. This will open the Watch property dialog. (It is also possible to double click an empty line in the list)
[![debugger enable.png](https://wiki.freepascal.org/images/7/76/debugger_enable.png)](</File:debugger_enable.png>) Enable/[![debugger disable.png](https://wiki.freepascal.org/images/c/c0/debugger_disable.png)](</File:debugger_disable.png>) Disable
    Enables/Disables individual watches from evaluation. This can be used to prevent spending time on evaluation, if a watch is not available in the current scope.
[![laz delete.png](https://wiki.freepascal.org/images/6/63/laz_delete.png)](</File:laz_delete.png>) Remove
    Deletes the selected Watch(es)
[![debugger enable all.png](https://wiki.freepascal.org/images/6/61/debugger_enable_all.png)](</File:debugger_enable_all.png>) Enable all/[![debugger disable all.png](https://wiki.freepascal.org/images/8/8b/debugger_disable_all.png)](</File:debugger_disable_all.png>) Disable all
    Enables/Disables all watches from evaluation.
[![menu clean.png](https://wiki.freepascal.org/images/7/74/menu_clean.png)](</File:menu_clean.png>) Delete all
    Cleans the list
[![debugger inspect.png](https://wiki.freepascal.org/images/9/99/debugger_inspect.png)](</File:debugger_inspect.png>) Detail-Pane
    Open the detail-pane.
[![menu environment options.png](https://wiki.freepascal.org/images/1/1f/menu_environment_options.png)](</File:menu_environment_options.png>) Properties
    Change the expression or properties of the current/selected watch. (Also possible by double clicking the watch)
[![ce implementation.png](https://wiki.freepascal.org/images/b/b2/ce_implementation.png)](</File:ce_implementation.png>) Data-Address
    Show hide the data-address column

## Context menu

[![Watch List popup.png](https://wiki.freepascal.org/images/4/4b/Watch_List_popup.png)](</File:Watch_List_popup.png>)

Additional to the above functionality the context menu allows to: 

Inspect
    Opens the current watch in the Debug-Inspector
Evaluate/Modify
    Opens the current watch in the Evaluate/Modify window
Create Data/Watch Breakpoint
    Opens the dialog to create a new watchpoint based on the current watch (stop if wachted value is changed or accessed)
Copy Name
    Copies the expression to the clipboard
Copy Value (quoted)
    Copies the value to the clipboard (strings will be quoted, so they can be pasted into pascal code)
Copy RAW Value
    Copies the value to the clipboard (strings will not be quoted, so they have actual tabs,cr,lf when being pasted)
Copy Data Address
    Copy the data address, if available

## Drag and Drop

Watches can be dragged and dropped to change their order in the list. 

If a structure or array is expanded, individual entries can be copied to the main list of watches. 

# Watch Properties Dialog

[![Watch Properties.png](https://wiki.freepascal.org/images/2/28/Watch_Properties.png)](</File:Watch_Properties.png>)

Entries can be double-clicked to edit them. 

Expression
    An expression for which the evaluated value should be shown. Expressions can be local or global variables, ([certain](<GDB_Debugger_Tips.md> "GDB Debugger Tips")) properties, or pascal expressions (limited support, e.g. "a+1").
Repeat Count
    Can be used to get array slices. The watch specifies the first element of the array "A[7]" (must have an index). With a "Repeat count" of 20, this shows A[7] to A[26]. It can also be used with a dynamic array (no index given). Then it specifies haw many elements to show, beginning with Item[0].
Digits
    Not implemented.
Enabled
    See Enable/Disable above.
Allow function calls
    Must also be enabled in the global options and supported by the backend. [FpDebug-Watches-FunctionEval](<FpDebug-Watches-FunctionEval.md> "FpDebug-Watches-FunctionEval")
Run all threads while evaluating
    Normally only the function will run and all threads (current and others) keep being paused. If however the function may wait for a lock hold by another thread, then it would block. This will run the function, and let the other threads run freely during that time. The current thread will be kept paused (it runs the function).
Use Instance class type
    Objects are normally shown according to the declaration of the watched expression. Watching "Sender: TObject" will only show you data that is declared on TObject. However object variables can contain objects of inherited classes. Sender may be a TForm. Using this the debugger will find the actual class of the object and display all data.
Converter
    [IDE_Window:_Ide_Options_-_Backend_Value_Converter](<IDE_Window__Ide_Options_-_Backend_Value_Converter.md> "IDE Window: Ide Options - Backend Value Converter")
Style
    How to display the data. If a style can not be applied, default style will be used.

# See also

  * [Setup the debugger](<Debugger_Setup.md> "Debugger Setup")
  * Watch-Points (Data-[Breakpoints](<IDE_Window_Breakpoints.md> "IDE Window:Breakpoints"))
  * [Evaluate Window](<IDE_Window__Evaluate/Modify.md> "IDE Window: Evaluate/Modify")
  * [Debug Inspector](<IDE_Window__Variable_Inspector.md> "IDE Window: Variable Inspector")
  * [Debug History](<IDE_Window__Debug_History.md> "IDE Window: Debug History")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Watch_Properties](https://web.archive.org/web/20231002172508/https://wiki.freepascal.org/IDE_Window%3A_Watch_Properties)_
