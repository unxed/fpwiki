# FpDebug

│ **English (en)** │  **[русский (ru)](<../ru/FpDebug.md>)** │

## Contents

  * 1 About
    * 1.1 Different meanings of "FpDebug"
      * 1.1.1 The package "FpDebug"
      * 1.1.2 FpDebug as reference to the IDE backend LazDebuggerFp
      * 1.1.3 Other FpDebug projects
    * 1.2 Installation in the IDE
  * 2 Related topics
  * 3 General Debugging Issues
    * 3.1 Properties
    * 3.2 Strings (vs PChar and Array of Char)
    * 3.3 Modifying managed types
    * 3.4 Scope / Variable search order
      * 3.4.1 Nested Procedures
      * 3.4.2 Global vars from other units
  * 4 FPC Issues
  * 5 Status
    * 5.1 Notes
      * 5.1.1 Stepping out
      * 5.1.2 Hardware Watchpoints (aka Data Breakpoints)
      * 5.1.3 Thread and Stack
      * 5.1.4 Watches / Locals / Inspect
      * 5.1.5 Modify variables (not implemented)
      * 5.1.6 Console Output
      * 5.1.7 Cross bit-ness Debugging (32/64 bit)
        * 5.1.7.1 Windows
        * 5.1.7.2 Linux
  * 6 Not implemented yet



# About

FpDebug is a [debugger](<Debugger.md> "Debugger") written in [Pascal](<Pascal.md> "Pascal") for Pascal. 

In detail: 

  * FpDebug is written in Pascal
  * FpDebug uses the [DWARF](<DWARF.md> "DWARF") debug info format 
    * The current implementation is a subset of DWARF with support for DWARF version 2 and 3. However the "subset" is merely the state of implementation. The aim is to extend this to implement as much of the standard as possible.
    * The current implementation covers the subset that is used by FPC
    * FpDebug has special implementations to deal with FPC specific DWARF usage (bugs, missing info, implementation details)
  * FpDebug "is for" (i.e. has a prioritized focus for) debugging Pascal 
    * It accepts expressions (Watches) in Pascal (e.g. using @foo and foo^.x instead of *foo and foo->x)
    * Yet, it is open for having other interpreters added.



## Different meanings of "FpDebug"

FpDebug can refer to 

### The package "FpDebug"

This is the engine of the debugger. It is not [IDE](<IDE.md> "IDE") specific. The package is used by several IDE backends: 

  * LLDB Debugger (with fpdebug): This debugger uses LLDB as a backend. In order to display Pascal-style results, it uses the "fpdebug" engine to show locals/watches.
  * [GNU Debugger](<GDB.md> "GDB") (with fpdebug): Using GDB for stepping/breakpoints/... and FpDebug for watches.
  * FpDebug internal Dwarf debugger: Pure FpDebug. Stepping, Running, Breakpoint,Watches, Locals, Stack...



### FpDebug as reference to the IDE backend [LazDebuggerFp](<LazDebuggerFp.md> "LazDebuggerFp")

The actual name of the IDE backend is [LazDebuggerFp](<LazDebuggerFp.md> "LazDebuggerFp"). This is the name of the Package that needs to be installed to use the debugger. However as seen above, in none technical context (such as captions in the IDE) this is referred to as "FpDebug ..." 

### Other FpDebug projects

The name is used for another project with the same goal. 

  * <https://github.com/graemeg/fpdebug>
  * <http://sourceforge.net/projects/duby/>



## Installation in the IDE

The FpDebug debugger is pre-installed. If your IDE is still configured to use another debugger backend, then go to:  
\- MainMenu -> Tools -> Options -> Debugger -> Debugger backend  
\- Select the debugger "FPDebug internal Dwarf-debugger" 

# Related topics

  * [Function evaluation in watches (Calling functions in the debugged app)](<FpDebug-Watches-FunctionEval.md> "FpDebug-Watches-FunctionEval") (This is a feature of [LazDebuggerFp](<LazDebuggerFp.md> "LazDebuggerFp"))
  * [Intrinsic (build in) helper functions](<FpDebug-Watches-Intrinsic-Functions.md> "FpDebug-Watches-Intrinsic-Functions")



# General Debugging Issues

These issues can not be fixed by the debugger alone. They need changes in FPC too. Some may also be affected by lack of support in DWARF. 

These issues apply to all debugger backends. 

## Properties

Properties are one of the most wanted features for (any of) the debugger(s). Here is why they can not yet be inspected, and why it will still be a good while before this happens. 

Before going into details: For "_Properties without getter function_ " (i.e., "_with direct field access_ ") FPC (as means to a workaround of the issue) currently adds an entry for a variable with the property name. This means such properties can be displayed. They can also be written by the debugger (if the debugger supports changing data), and if they are written this is done bypassing any Setter procedure, since the debugger has no clue there even is a setter. 

There are several missing pieces for allowing the debugger to show (and maybe modify) properties. 

  * The debugger needs support to call functions (as of end 2021: partly supported)
  * FPC needs a way to actually add info about the "properties" to the debug info. (as of end 2021: work has begun...)



As for calling function. This may just be missing implementation in the debugger. This has not yet been investigated in any more depth. It may or may not require additional info in the DWARF generated by FPC 

As for adding "properties" to the debug info: DWARF (at least version 2 and 3 / not checked 4 or 5) does not actually have any way to encode this info. Consequently FPC currently does not add it either. 

In case that DWARF does not add this in time, DWARF allows for vendor specific data (afaik). Up until now, no debugger would have known about any "private fpc data", so there was no point. In future such data could be agreed between FPC and FpDebug. But that is still a good way off. FpDebug does not even do function calling yet. 

## Strings (vs PChar and Array of Char)

Originally DWARF had no concept to encode strings. This has been added in DWARF 4 (need verification), but not yet been investigated for usability with FPC. Also FPC currently only implements DWARF 2 and 3 (with first steps towards version 4). 

This leaves only 2 ways to encode a string in DWARF. 

  * As pointer to char
  * As array of char



Which one is used depends on the version of FPC, and if DWARF 2 or 3 is used. 

If it is "pointer to char", then this makes in indistinguishable from actual PChar. This is the reason all debuggers in Lazarus sometimes for a watch like "SomeString[4]" show the result "PChar: 'a'; String: 'b'". Because PChar is zero-based and string one-based, the index "4" can mean the 4th or the 5th char. 

Recent FPC use the "array of char" when they generate DWARF 3. In that they have (currently) implementation detail differences between how they encode strings and real array of char. (Such as the omission of an optional/redundant bit of information. In this case: DW_AT_byte_stride). Those details are meaningless from the point of any other debugger, but FpDebug can detect them as it knows about those implementation details. Of course this is extremely fragile, the next version of FPC can always change this. 

In a similar way FPC handles ShortStrings internally as record, and encodes them as this. Again distinguishing this is down to implementation details. 

## Modifying managed types

This applies to any debugger that can set new values to a watch (modify variable in the debugged exe). 

Managed types can not be correctly modified. 

To modify managed types the debugger would need to know about: 

  * the ref count
  * copy on write behaviour
  * how to allocate/free memory / or better how to call the inc/dec_ref methods for each types, so they will do the work correctly



None of that info is available. 

If you assign a new value to a string or dynamic array, then the ref count of the old and new value must be adjusted. If needed memory must be allocated or freed. If that is not done correctly it will at least cause a memory leak. But it can also cause crashes or random other buggy behaviour when the debugged exe continues. 

In an array it is possible to change a single element. "SomeArray[5] := 7;". If the array is referenced from several variables, this will affect all of them. The same happens if you execute this statement in your app. 

String (AnsiString) have copy-on-write. If you do "SomeString[4] := 'a'" then the variable from which you accessed the string gets a new copy, and other variables (that share the memory) should not be affected. The debugger does not have that info. Nor does it have access to the refcount and memory alloc/de-alloc (or internal string procs). 

## Scope / Variable search order

There are various issues where FPC does not put enough information into the DWARF info so that a debugger could compute the correct scope for all variables. In same cases this may be due to DWARF not offering this, or not offering it at the time where it was implemented or last updated in FPC. Some of it may be fixable, if FPC would change the way it encodes the info. 

### Nested Procedures
    
    
    var SomeVar: Integer; // global var
    
    procedure Outer(NumText: string);
    var 
      SomeVar: Integer; // local var, declared above "Nested"
    
      procedure Nested;
      var 
        I: Integer;
      begin
        WriteLn(OuterVarInScope);  
      end;
    
    //var 
    //  SomeVar: Integer; // local var, declared below "Nested"
    begin
      Nested;
    end;
    

The local var "SomeVar" hides the global var of the same name, if you are in the procedure "Outer". The local var is also visible (and hides the global) if you are in "Nested". 

But if you comment the local var "declared above Nested" and uncomment the local var "declared below Nested", then the local var is no longer in the scope of "Nested". "Nested" would now see the global var. ("Outer" still sees the local). 

The DWARF info currently only tells the debugger that "SomeVar" is declared as local var in "Outer". And that "Nested" is inside "Outer". But the DWARF info does not include if "SomeVar" is declared above or below "Nested". So the debugger can not tell the difference. The debugger will in both cases show the local "SomeVar" when paused in "Nested" 

  


### Global vars from other units

The DWARF info contains a section for each unit. All Symbols of a unit are listed in the correct section. However there is no info, which unit is in the "uses" clause of which other unit. And evidently even less, in which order they are used. 

If there are several global variables of the same name, one in each of several units, then FPC uses the order of the uses clause (right to left) to find the variable that will be seen from code in the current unit. 

If there is code in a unit FooBar that access a variable named NotInFooBar (that is not declared in unit FooBar), then this variable must be from one of the other units. If there are several variables named NotInFooBar in different units, then the debugger has no information which one FPC did use. The debugger is currently showing the first it finds. (Which is kind of random) 

# FPC Issues

This affects all debuggers 

  * resourcestring



Currently not implemented <https://bugs.freepascal.org/view.php?id=35571>

  * const Foo = unicodestring('abc')



No debug info implemented <https://bugs.freepascal.org/view.php?id=35569>

  * type Foo = object (Dwarf 3) - Fixed in 3.3.3



Incorrectly encoded as class-instance <https://bugs.freepascal.org/view.php?id=36017>   
The debugger can not display the data. 

  * Dynamic array when cross compiling



<https://bugs.freepascal.org/view.php?id=35567>   
The debugger may calculate the length wrong. 

  * Wrong stride for bitpacked array - Fixed in 3.3.3



<https://bugs.freepascal.org/view.php?id=36144>   
FpDebug currently has a workaround 

# Status

FpDebug supports Linux, Windows and MacOs.  
(LazDebuggerFp supports Linux and Windows only. Limited support for MacOS is available via LazDebuggerFpLldb) 

Any functionality regarding execution control (stepping/pausing...) is only tested for Linux and Windows. Support for this on Mac is incomplete/fragile. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For Windows/Linux please see **[Debugger Status](<Debugger_Status.md> "Debugger Status")**

For Mac support is currently considered "Alpha" Data evaluation can be used on Mac via the "LLDB+FpDebug" backend 

The information is indicative and subject to updates / subject to further testing 

## Notes

### Stepping out

"Stepping out" relies on finding the caller address. The location of which depends on whether a function has a "stackframe" (use the BasePointer register) and has the frame already/still set-up. Detection of this is limited to a few standard situations. (Especially in none FPC code, or optimized code this will not be detectable). 

Within un-optimized FPC code this feature works. 

### Hardware Watchpoints (aka Data Breakpoints)

Partly implemented for most data types (subset of the types supported by the hardware), including Conditions and other properties. 

All watchpoints will have global scope. Watching a local var will alert you the next time the variable's memory is overwritten (e.g. if another variable takes the memory). 

"Write only" watchpoints act as "read/write" 

Watchpoints also trigger if they are overwritten with the value they already have. 

Watchpoints do not show the before/after dialog. 

Watchpoints that are not aligned to an address that is a multiply of their size, may be triggered by neighbouring values. E.g., a Word starting at an odd address, can be triggered by the byte before or after it. A 64 bit pointer must be aligned to an 8 byte boundary, or can be triggered by neighbours. 

### Thread and Stack

See "Stepping out". Relies on the same frame detection. 

### Watches / Locals / Inspect

Implemented 

  * Any missing types need to be discovered (i.e. not aware of any type that does not display, if not listed under "fpc issues"
  * currency types may not display the decimal dot (they will show value * 1000)
  * Expressions: The debugger accepts expressions that it will calculate. It expects Pascal syntax. 
    * Expressions are calculated without range checks
  * [ function evaluation](<FpDebug-Watches-FunctionEval.md> "FpDebug-Watches-FunctionEval")
  * But not yet: Properties



### Modify variables (not implemented)

Done for simple types 

### Console Output

On Windows console is opened by OS. On Linux the output goes to the debugger window "Console output" 

### Cross bit-ness Debugging (32/64 bit)

FpDebug has no cross debugging between different platforms. 

However, if your IDE has a different bitness that your project FpDebug may be able to debug it. 

#### Windows

A 64 bit IDE
    can debug both 64bit and 32bit applications
A 32 bit IDE
    can only debug 32bit applications

#### Linux

Cross debugging has not yet been tested, but may be possible. 

# Not implemented yet

  * Changing Location of execution (while paused) / Resume execution from different line
  * Properties
  * ...

---

_Source: [https://wiki.freepascal.org/FpDebug](https://web.archive.org/web/20250411135251/https://wiki.freepascal.org/FpDebug)_
