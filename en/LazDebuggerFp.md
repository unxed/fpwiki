# LazDebuggerFp

│ **English (en)** │ 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For more information about the Free Pascal debugger see   
  


  * [Debugger Status](<Debugger_Status.md> "Debugger Status") for supported features
  * [FpDebug](<FpDebug.md> "FpDebug") for info on this debugger

  


[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** **LazDebuggerFp** works best with **DWARF 3**

  * **LazDebuggerFp** is a [debugger](<Debugger.md> "Debugger") integrated into the IDE based on **[FpDebug](<FpDebug.md> "FpDebug")**.
  * It many cases it is simply referred to as **FpDebug** (making that term ambiguous)...
  * **LazDebuggerFp** requires [DWARF](<DWARF.md> "DWARF") debug info to be enabled in the project options.



  


## Using LazDebuggerFp in the IDE

The package is installed in the IDE by default. 

The debugger can be selected in Tools > Options > Debugger > Debugger Backend 

## Related topics

  * [Function evaluation in watches (Calling functions in the debugged app)](<FpDebug-Watches-FunctionEval.md> "FpDebug-Watches-FunctionEval")
  * [Intrinsic (build in) helper functions](<FpDebug-Watches-Intrinsic-Functions.md> "FpDebug-Watches-Intrinsic-Functions")



## Debugger specific options
    
    
    Tools | [Options](<IDE_Window__IDE_Options_Dialog.md> "IDE Window: IDE Options Dialog") | [Debugger](<IDE_Window__Debugger_Options.md> "IDE Window: Debugger Options") | Debugger Backend | FpDebug integrated Dwarf-debugger
    

ConsoleTty
    (Linux only) Redirect StdOut to an existing tty
ForceNewConsole
    (Windows only) If the IDE has a console, debugging an app that also has a console will normally share the existing console. Setting this to "True" will open a new console for the debugged app.

IntrinsicPrefix
    See [FpDebug-Watches-Intrinsic-Functions#Settings](<FpDebug-Watches-Intrinsic-Functions.md> "FpDebug-Watches-Intrinsic-Functions")

NextOnlyStopOnStartLine
    By default "Step out" will return to the caller, and pause execution in the middle of the line from which the call originally came. This allows to "Step in" again, if several calls are made on that line.  
For example; Foo(Func1(), Func2()); contains 3 calls, each of the 3 can be stepped in (if they have debug info)

HandleDebugBreakInstruction
    If the debugger should stop at hardcoded "int3" breakpoints. <https://bugs.freepascal.org/view.php?id=37292>

MaxMemReadSize
    Limits the amount of Memory the debugger will use for a single read. This can prevent errors if incorrect size information proclaims a structure is tens of Gigabyte big.  
This may also protect from dummy declarations like "type TMaxArray = array [0..$7FFFFFFF] of byte; PMaxArray = ^TMaxArray". Such types are never fully allocated. They only act as accessor.  
The limit applies to the structure as a whole. Individual fields/elements can be read if they are smaller than the limit.

MaxStringLen
    Length in chars to cut off strings. Protects against corrupted data.
MaxArrayLen
    Length in elements to cut off array. Protects against corrupted data.
MaxNullStringSearchLen
    PChar (and strings in dwarf2) have no length info (or not determinable to the debugger). The debugger must read memory until it finds a #0. If no #0 is found the debugger will stop searching at this length.

MaxStackStringLen, MaxStackArrayLen, MaxStackNullStringSearchLen
    Same as above, but applied when evaluating stack-frames (content for the Stack window). Due to the limited space in the stack window, one might choose lower limits.

MaxArrayConversionCnt
    If configured, the debugger can run [ converters (See here)](<IDE_Window__Ide_Options_-_Backend_Value_Converter.md> "IDE Window: Ide Options - Backend Value Converter") on data (Currently only passing **variant** to **SysVarToLStr**). In arrays and structures each entry/field will be converted if it is a variant. This limits the amount of entries in an array that will be converted.  
If an array contains structures with several variant fields, then each field is counted as conversion.  
If the debug inspector fetches an array, then this limit does not apply to the array as a whole. It will rather apply to each entry within the array (should those entries be themself arrays). This is so, that the limit applies to each row displayed by the Inspector.
MaxTotalConversionCnt
    Similar to **MaxArrayConversionCnt** , but applies to all conversions. I.e., also outside of arrays, like fields of a class.  
This limit also applies to the whole of an inspected value in the inspector.  
Unlike what happens in the inspector, in the watches window an expanded watch (sub-watches) does not count as a whole. Each sub-watch counts individually.

  
The debugger currently does not give an indication if a value was cut off. This will be implemented in future.

---

_Source: [https://wiki.freepascal.org/LazDebuggerFp](https://web.archive.org/web/20240526185325/https://wiki.freepascal.org/LazDebuggerFp)_
