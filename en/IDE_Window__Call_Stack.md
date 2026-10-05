# IDE Window: Call Stack

│ **English (en)** │  **[русский (ru)](<../ru/IDE_Window__Call_Stack.md>)** │

## Contents

  * 1 Important
  * 2 Dialog
    * 2.1 What is the call stack?
    * 2.2 Source column
    * 2.3 Line column
    * 2.4 Function column
    * 2.5 Tricks
  * 3 Popup menu
    * 3.1 Show
    * 3.2 Set as current
    * 3.3 Copy all



## Important

You must [setup the debugger](<Debugger_Setup.md> "Debugger Setup") and start the project to debug it. Only then this window will be useful. 

The output shown on this page is based on the GNU debugger (GDB) which is currently the only debugger Lazarus supports. Its output looks at times like C, not Pascal. Other debuggers may show a more Pascal-like style. 

## Dialog

[![Callstack.png](https://wiki.freepascal.org/images/3/32/Callstack.png)](</File:Callstack.png>)

### What is the call stack?

The call stack is the stack of function calls. The top line is the current function, the lowest line is the main program. 

### Source column

This column lists the filename of the source file. This information is retrieved from the debug information contained in the executable (or from an external GDB symbol file if you have selected that option). Only those parts of the program explicitly compiled with debug information contain this information. 

### Line column

If the position contains debug information, the source line number will be shown, otherwise only the address pointer in the executable is shown. This line is where the next function was called. 

**Note** : The line numbering is the numbering at the time the project was last compiled with debug information. If you have subsequently inserted or deleted lines, the numbering will appear to be incorrect. 

### Function column

The mangled name of the procedure or function. The compiler converts Pascal identifiers into names which the gnu tools (designed for C code) can use. For example: 
    
    
     TAPPLICATION__CREATEFORM(0x81fb738, void, (^TAPPLICATION) 0xb7cd0014)
    

This means: 

  * The mangled function name is TAPPLICATION__CREATEFORM, which is the TApplication.CreateForm procedure of the LCL unit forms.pp. Because Pascal is case insensitive and the gnu tools are case sensitive, FPC converts the name to uppercase. Because the gnu tools don't know about classes and objects, the _class.method_ is converted to a global function name.
  * The parameter list depends on the platform and the calling convention. That means the parameter list may be reversed, starting with the rightmost parameter. This is the case in the above example.
  * The 'Self' parameter is implicit, meaning that you don't write it in the Pascal source, because FPC creates it automatically. This is always the first parameter emitted by the compiler (though invisible in the Pascal source code). Because of the reversed parameter order, Self is shown here as the last parameter. It is of type ^TAPPLICATION and its hexadecimal value is 0xb7cd0014.
  * The next parameter in Pascal is 'var Reference', which has no type. Therefore it is 'void'.
  * The last parameter in Pascal is 'InstanceClass: TComponentClass'. Gnu sees this parameter simply as a pointer with the hexadecimal value 0x81fb738.



### Tricks

Double click on an item to jump to the source. 

## Popup menu

[![Callstack popmenu.png](https://wiki.freepascal.org/images/e/e2/Callstack_popmenu.png)](</File:Callstack_popmenu.png>)

### Show

Jump to the current item's source code position. 

### Set as current

Set the selected entry as the current stack frame. Local variables only exist within their own local stack frame. By setting the current frame, you can watch the values of variables in the (local) context of the procedure you have selected. 

### Copy all

Copy the call stack to the clipboard.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Call_Stack](https://web.archive.org/web/20220522121335/https://wiki.freepascal.org/IDE_Window%3A_Call_Stack)_
