# IDE Window: Watch list

│ **English (en)** │  **[русский (ru)](<../ru/IDE_Window__Watch_list.md>)** │

## Contents

  * 1 Important
  * 2 Watch List
    * 2.1 Data displayed
      * 2.1.1 Scope (Stackframe, Thread, History)
      * 2.1.2 Special Values
    * 2.2 Interface
  * 3 Watch Properties
  * 4 See Also



# Important

You must [setup the debugger](<Debugger_Setup.md> "Debugger Setup") and start the project to debug it. Only then this window will be useful. To open Watch list press Ctrl+Alt+W. 

# Watch List

[![Watch List.png](https://wiki.freepascal.org/images/6/63/Watch_List.png)](</File:Watch_List.png>)

The "Watch List" shows the values of variables and expressions ("watches") when the debugged application is paused. (e.g. reached a breakpoint). 

Expressions can be local or global variables, ([certain](<GDB_Debugger_Tips.md> "GDB Debugger Tips")) properties, or pascal expressions (limited support, e.g. "a+1"). [See here for more information](<GDB_Debugger_Tips.md> "GDB Debugger Tips")

## Data displayed

The display has 2 columns: 

  * Expression: The variable or expression to be watched
  * Value: The current value of the expression



Entries can be double-clicked to edit them. 

### Scope (Stackframe, Thread, History)

The values are evaluated according to the scope set in the [Thread](<IDE_Window__Threads.md> "IDE Window: Threads") and [Stack](<IDE_Window__Call_Stack.md> "IDE Window: Call Stack") dialog. Default is the current Thread and top stack frame. Both (Stack and Frame) dialog offer to change the "current" Frame/Thread. The watch window will follow this selection. 

It is also possible to select previously displayed values, using the [History](<IDE_Window__Debug_History.md> "IDE Window: Debug History") dialog. 

### Special Values

<invalid>
    Value currently not available. Can be caused, if the debugger is not active or the debugged app not currently paused.
<evaluating>
    Value is currently retrieved. A result will be show soon
<disabled>
    The expression is excluded from evaluation. See Disable/Enable buttons (light bulbs)
Error...
    The value could not be evaluated. (Error in Expression or Variable not available in selected scope.

## Interface

_Toolbar_

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
[![menu environment options.png](https://wiki.freepascal.org/images/1/1f/menu_environment_options.png)](</File:menu_environment_options.png>) Properties
    Change the expression or properties of the current/selected watch. (Also possible by double clicking the watch)

_Context menu_

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
Copy Value
    Copies the value to the clipboard

  


# Watch Properties

[![Watch Properties.png](https://wiki.freepascal.org/images/2/28/Watch_Properties.png)](</File:Watch_Properties.png>)

Expression
    An expression for which the evaluated value should be shown. Expressions can be local or global variables, ([certain](<GDB_Debugger_Tips.md> "GDB Debugger Tips")) properties, or pascal expressions (limited support, e.g. "a+1").
Repeat Count
    Can be used to get array slices. The watch specifies the first element of the array "A[7]" (must have an index). With a "Repeat count" of 20, this shows A[7] to A[26]. It can also be used with a dynamic array (no index given). Then it specifies haw many elements to show, beginning with Item[0].
Digits
    Not implemented.
Enabled
    See Enable/Disable above.
Allow function calls
    Not yet supported.
Use Instance class type
    Objects are normally shown according to the declaration of the watched expression. Watching "Sender: TObject" will only show you data that is declared on TObject. However object variables can contain objects of inherited classes. Sender may be a TForm. Using this the debugger will find the actual class of the object and display all data.
Style
    How to display the data. If a style can not be applied, default will be used.

# See Also

  * Watch-Points (Data-[Breakpoints](<IDE_Window_Breakpoints.md> "IDE Window:Breakpoints"))
  * [Evaluate Window](<IDE_Window__Evaluate/Modify.md> "IDE Window: Evaluate/Modify")
  * [Debug Inspector](<IDE_Window__Variable_Inspector.md> "IDE Window: Variable Inspector")
  * [Debug History](<IDE_Window__Debug_History.md> "IDE Window: Debug History")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Watch_list](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/IDE_Window%3A_Watch_list)_
