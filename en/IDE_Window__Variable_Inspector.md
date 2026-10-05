# IDE Window: Variable Inspector

│ **English (en)** │

## Contents

  * 1 Important
  * 2 Variable Inspector
  * 3 Limitations
  * 4 Scope (Stackframe, Thread)
  * 5 Interface
  * 6 See Also



# Important

You must [setup the debugger](<Debugger_Setup.md> "Debugger Setup") and start the project to debug it. Only then will this window be useful. 

# Variable Inspector

Also known as "Debug Inspector" 

[![Debug Inspector.png](https://wiki.freepascal.org/images/2/26/Debug_Inspector.png)](</File:Debug_Inspector.png>)

This window allows you to watch an expression. If the expression is a structure, then each field is displayed as a separate entry. 

Expressions can be local or global variables, ([certain](<GDB_Debugger_Tips.md> "GDB Debugger Tips")) properties, or Pascal expressions (limited support, e.g. "a+1"). [See here for more information](<GDB_Debugger_Tips.md> "GDB Debugger Tips")

# Limitations

  * The Variable Inspector _does not automatically follow_ the changes of current Thread or Stack. You can toggle the "use instance class" setting, to refresh the result.
  * This dialog is _not_ affected by the [History](<IDE_Window__Debug_History.md> "IDE Window: Debug History") dialog.
  * If the result is a structure that has properties, they may not be included.



# Scope (Stackframe, Thread)

The values are evaluated according to the scope set in the [Thread](<IDE_Window__Threads.md> "IDE Window: Threads") and [Stack](<IDE_Window__Call_Stack.md> "IDE Window: Call Stack") dialog at the time you set the expression. The default scope is the current Thread and top stack frame. Both dialogs (Stack and Frame) offer to change the "current" Frame/Thread. 

# Interface

Setting the value
    
    The interface does not currently provide any method to change the expression from within the dialog. (This was added to Lazarus past version 1.0).
    The value can be set from the [Watch list](<IDE_Window__Watch_list.md> "IDE Window: Watch list") (via context menu of an existing watch) or the [Evaluate Window](<IDE_Window__Evaluate/Modify.md> "IDE Window: Evaluate/Modify").

  


Data/Method-Tabs
    If the data is a structure with methods, they are displayed separately.

_Context menu_

[![Debug Inspector context.png](https://wiki.freepascal.org/images/1/10/Debug_Inspector_context.png)](</File:Debug_Inspector_context.png>)

Use Instance class type
    Objects are normally shown according to the declaration of the watched expression. Showing "Sender: TObject" will only show you data, that is declared in TObject. However object variables can contain objects of inherited classes. Sender may be a TForm. Using this the debugger will find the actual class of the object and display all data.

# See Also

  * [Watch list](<IDE_Window__Watch_list.md> "IDE Window: Watch list")
  * [Evaluate Window](<IDE_Window__Evaluate/Modify.md> "IDE Window: Evaluate/Modify")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Variable_Inspector](https://web.archive.org/web/20221004111958/https://wiki.freepascal.org/IDE_Window%3A_Variable_Inspector)_
