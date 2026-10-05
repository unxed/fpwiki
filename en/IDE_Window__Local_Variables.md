# IDE Window: Local Variables

│ **English (en)** │  **[русский (ru)](<../ru/IDE_Window__Local_Variables.md>)** │

## Contents

  * 1 Navigation
  * 2 Important
  * 3 Local variables
  * 4 Data displayed
  * 5 Scope (Stackframe, Thread, History)
  * 6 See also



## Navigation

[Main Menu](<Main_menu.md> "Main menu") > [View](<Main_menu.md> "Main menu") > Debug Windows > Local Variables 

## Important

You must [setup the debugger](<Debugger_Setup.md> "Debugger Setup") and start the project to debug it. Only then this window will be useful. 

## Local variables

[![Local Variables 2 0 10.jpg](https://wiki.freepascal.org/images/4/4f/Local_Variables_2_0_10.jpg)](</File:Local_Variables_2_0_10.jpg>)

This is the list of the local variables and their current values of the current function/procedure. 

## Data displayed

Name
    The mangled name of the variable. Normally the compiler converts the Pascal identifier to uppercase. You will see local variables only if the procedure was compiled with debugging information.
Values
    The current value of the local variable.

Note: The values are shown in a very basic form. e.g Objects are shown as pointer, instead of structure. You may get more information by adding a variable to the [Watches](<IDE_Window__Watch_list.md> "IDE Window: Watch list") window. 

## Scope (Stackframe, Thread, History)

The values are evaluated according to the scope set in the [Thread](<IDE_Window__Threads.md> "IDE Window: Threads") and [Call Stack](<IDE_Window__Call_Stack.md> "IDE Window: Call Stack") dialogs. Default is the current Thread and top stack frame. Both (Stack and Frame) dialogs offer to change the "current" Frame/Thread. The Watches window will follow this selection. 

It is also possible to select previously displayed values, using the [History](<IDE_Window__Debug_History.md> "IDE Window: Debug History") dialog. 

## See also

  * [Watches Window](<IDE_Window__Watch_list.md> "IDE Window: Watch list")
  * [Debug History](<IDE_Window__Debug_History.md> "IDE Window: Debug History")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Local_Variables](https://web.archive.org/web/20250114091308/https://wiki.freepascal.org/IDE_Window%3A_Local_Variables)_
