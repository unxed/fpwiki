# FpDebug-Watches-FunctionEval

## Contents

  * 1 Warning: Side effects
  * 2 Configuration - Enabling function-eval
    * 2.1 Global
    * 2.2 Watch properties / Expression
  * 3 Properties of classes
  * 4 Optimization - ommited or inaccessable functions
    * 4.1 smart linking - unused functions
    * 4.2 class methods from other units
  * 5 Limitations
    * 5.1 Exceptions
    * 5.2 ~~Recursion - (re-)enter the currently paused function~~
    * 5.3 ~~Breakpoints~~
    * 5.4 Threads
    * 5.5 "var" params, "out" params, "constref" params, "const" params
  * 6 Supported parameter and result types
    * 6.1 Dwarf versions
      * 6.1.1 Ansi/LongString vs PChar vs Array of char
      * 6.1.2 UnicodeString vs WideString vs PWideChar
      * 6.1.3 class vs pointer to record
    * 6.2 Parameters
      * 6.2.1 Lazarus 2.2.2
      * 6.2.2 Lazarus 2.3 and up



[LazDebuggerFp](<LazDebuggerFp.md> "LazDebuggerFp") ([FpDebug](<FpDebug.md> "FpDebug")) supports watches that call a function in the debugged process. However not all functions are supported, and support depends on the versions of Lazarus, FPC and DWARF used, as well as the target OS/Bitness. 

# Warning: Side effects

Calling a function in the debugged target, will execute whatever is in that function (and any code called from within the function). If any of this code makes changes to global variables, or changes the data of a class, or does affect anything other than it's own local variables, then those effects are persistent. 

  * **This can affect the behaviour of the running application.**
  * **It may cause the application to fail.**
  * It may even **hide bugs** in the application, making the app work in the debugger (while watching that function), when the same app does not work otherwise.



  
Further more, a called function can - like any code - crash with an unrecoverable error. If that happens, it has the same effect as if the debugged application had crashed. Debugging can not be continued. This may also happen, if parameters are passed incorrectly to the function. See the section on parameters below. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Those side effects are part of the very core concept of calling a function. They can't be avoided.  
_Using this feature requires the user to know about those possible effects_ , and to ensure they only call functions of which they know the side effects and want them to happen.

  


# Configuration - Enabling function-eval

## Global

To enable the feature go to 

  * the menu Tools > Options
  * the page Debugger > General



and set the checkbox "BETA: Allow function calls in watches" 

## Watch properties / Expression

In the watch properties the checkbox "Allow function calls" must be enabled. 

The debugger will only call functions, if the function name is followed by "()" in the watched expression. Those "()" must be present, even if no arguments are given. 

# Properties of classes

Properties are not yet supported at all. FPC does not currently encode the required info into the debug info. 

This is being worked on, and hopefully will be released with the next major fpc version 3.4.0. Once released, FpDebug will be extended to support it. 

Getters may be called like any other function, if they fulfil the requirements given on this page. Depending on OS you may find that FPC does ommit debug info for some class methods, includding many getter functions. 

# Optimization - ommited or inaccessable functions

## smart linking - unused functions

The compiler may omit the code for functions that aren't called by any of your code. If a function is not found by the debugger, make sure it is included (e.g. add a call to that function in your code) 

This is not an issue of the debugger, but rather of how you compile your app. 

## class methods from other units

The following depends on the OS, and maybe the linker used. The exact conditions need yet to be documented. 

If a class (or base class) is declared outside the current unit (the unit, in which the app is paused, or in which the code of the current selected stackframe resides) then (some) functions of this class may not have full debug info. Attempting to call them will give an error about "not finding the address of the function". 

This error may for example happen when attempting to get entries (or the full text) of a TStringList by calling "MyList.Get(0)" or "MyList.GetTextStr()" 

This is not an issue of the debugger, this is caused by either FPC or the linker and must be fixed in either of them. 

# Limitations

## Exceptions

Functions using exceptions can not be fully evaluated. They will be aborted, if they raise an exception and an error will be shown. This is even, if they would have caught it. 

They will complete, if they do not raise an exception. 

## ~~Recursion - (re-)enter the currently paused function~~

~~Functions that during their evaluation call the code in which the application is paused are aborted with an error.~~

~~That is if you are paused in the function "FooBar" (based on the currently selected stackframe), then the evaluated function will fail, if it makes a call to that FooBar() routine.~~

~~This is a temporary limitation. (This is a bug in FpDebug)~~

## ~~Breakpoints~~

~~If during the evaluation of a function any breakpoint is hit, the function will be aborted.~~

~~This is a temporary limitation. (This is a bug in FpDebug)~~

## Threads

When a function is evaluated all threads of the debugged application will run. The current thread will evaluate the function. Other threads will continue normally. 

The debugger does not know if the called function may relay on thread related functions, such as aquirirng a critical section. If it does, and the critical section is hold by another thread, then that other thread must run to be able to release the critical section. If the other thread would still be paused, the evaluated function would hang forever. 

The debugger does not currently offer any control to the user to refine this behaviour. 

If another thread executes the code/function where the current thread was paused for the watch-eval, then the eval may fail. (This is a bug in FpDebug) 

## "var" params, "out" params, "constref" params, "const" params

Parameters passed as "var", "out", "constref" are not supported. This also applies to "const" param, if they are passed as reference. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The debugger can't currently detect if a parameter is declared as "var", "out", "constref", "const".   
If you call a function from a watch **you must ensure that there are no such parameters**.  
The debugger will pass all parameters as "normal" parameters. If they are not, then the debugged process is likely to crash, or enter an invalid state. 

# Supported parameter and result types

## Dwarf versions

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Strings of any type are not supported in Lazarus 2.2.x. The described support is only present in Lazarus 2.3 and up.

Some function calling depends on the version of DWARF used. This may include DWARF versions used for other units, such as units from the RTL (if compiled with debug info). 

In order to call a function the debugger must be able to detect 

  * the type expected by each parameter of the function
  * the type of the data passed for each parameter.


    
    
    var
      s: AnsiString;
      p: pchar;
    
    function Foo(p1: String; p2: PChar; p3: pointer)integer;
    

In order to evaluate the watch "Foo(s, p, nil)" the debugger must know that 

  * p1 and s are both strings, and s can therefore be used for p1.
  * p2 and p are both pchar, and p can therefore be used for p2.
  * The debugger also must know how a parameter of each type is passed.



_For certain groups of types the compiler does not generate a detailed enough description. The debugger can not tell the exact type of such data._

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** In some cases the debugger may be able to reject unsafe attempts.  
_However the debugger may not always catch attempts to pass data of incorrect/unsupported types._ Attempting to pass such data in a function call may therefore lead to errors/crashes in the called function and _**abort the entire debug session**_.

### Ansi/LongString vs PChar vs Array of char

LongString do not currently have their own encoding in DWARF info. They are either encoded as "PChar" or "Array of char". 

However LongString are handled different if 

  * used as function result
  * in some cases if passed as parameter



If the debugger can not properly detect them, they may either be rejected by the debugger, or crash the target app. 

  
Currently the debugger relies on certain implementation details in FPC. So the described behaviour works only for the **exact fpc version**. It may break with new releases. 

To detect LongStrings the application must be compiled with 

  * DWARF-3 or DWARF-4
  * FPC 3.2.2 or 3.2.0   
FPC 3.0.4 or 3.0.2 or 3.0.0 may also work, but have only been tested on Windows (64/32)



On Windows
    it is sufficient if the current unit (the unit where the debugger is paused, or where the code of the selected stackframe is) is compiled with DWARF-3/4.  
For global variables, this may (sometimes) also require the unit where this variable is declared to have DWARF-3/4

On Linux
    in addition to the Windows requirements, there are conditions on how the RTL was compiled (as the type "AnsiString" is part of the RTL).  
The RTL must either be without any DWARF info. Or it must be compiled using DWARF-3/4.  
It will not work if the RTL is compiled using DWARF-2.  
If the RTL is without debug info, but there are other units using AnsiStrig, and any of those units use DWARF-2 this may also break the functionality.

### UnicodeString vs WideString vs PWideChar

  * UnicodeString has not yet been tested at all.
  * WideString currently only works on Windows with **Lazarus 2.3** and FPC 3.2.0 or 3.2.2 and all the conditions given on LongString above



### class vs pointer to record

Pointer to record can in rare cases be mistaken as class. 

This has not yet been tested on how it affects function calling. 

## Parameters

### Lazarus 2.2.2

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Lazarus 2.2.2 has a known issue (race condition) if watches with function eval are either   
  


  * evaluated to soon after the debugger paused the app. (Usually within the top 5 entries of the watch window.   
This is about the time passed between the app being paused and the eval being started, no matter how the required delay is added. Therefore such watches can be disabled, and then be only be enabled when they need to be evaluated, and disabled again before the next step/run
  * interrupted by F8/F9 to continue stepping/running. Make sure to wait until those watches display a result.



  
**Function calling works on**

  * Windows 64 and 32 bits
  * Linux 64 bits



  
**Function are only supported if they return any of the following types**

  * Integer/Cardinal values of 1,2,4 or 8 bytes size. (Including currency which is handled as an integer type)(**8 bytes may be limited to 64bit targets**)
  * pointer
  * enum
  * boolean
  * char
  * class (TObject ...)



  
**Function are only supported if all parameters are of the following types**

  * Integer/Cardinal values of 1,2,4 or 8 bytes size. (**8 bytes may be limited to 64bit targets**)
  * pointer
  * enum
  * boolean
  * char
  * class (TObject ...)



  
**Some data types as function parameters only work when the data is a variable in the target process.**

  * Int/Cardinal can be specified as constant/literal
  * Int/Cardinal can be the result of a nested function call



Other types may support this. This needs to be tested and documented 

### Lazarus 2.3 and up

**Function calling works on**

  * Windows 64 and 32 bits
  * Linux 64 and 32 bits



  
**Function are only supported if they return any of the following types**

  * any of the types supported by Lazarus 2.2.2
  * Ansi/LongString (**requires DWARF-3/4**)
  * WideString (**requires DWARF-3/4** / **Only Windows 64/32**)



  
**Function are only supported if all parameters are of the following types**

  * any of the types supported by Lazarus 2.2.2
  * Ansi/LongString (**requires DWARF-3/4**) 
    * Support constant/literal and result from nested function call
  * WideString (**requires DWARF-3/4** / **Only Windows 64/32**)
  * record (only Win 64/32 and Linux 64 -- **not on 32bit Linux**)

---

_Source: [https://wiki.freepascal.org/FpDebug-Watches-FunctionEval](https://web.archive.org/web/20221230220039/https://wiki.freepascal.org/FpDebug-Watches-FunctionEval)_
