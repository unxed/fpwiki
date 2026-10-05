# FpDebug-Watches-Intrinsic-Functions

## Contents

  * 1 About
    * 1.1 Settings
  * 2 Length(String-or-array)
  * 3 CC(instance) {ChildClass}
  * 4 RefCnt(string-or-array)
  * 5 Pos(SubString, MainString [, CaseInsensitive=False])
  * 6 SubStr(Str, Start, Len [, ForcePtr=False])



# About

[LazDebuggerFp](<LazDebuggerFp.md> "LazDebuggerFp") has some build in helper functions. They can be included in any watch using the pascal function calling syntax. The result is calculated internally be the debugger. 

To avoid clashed with identifier names in the debugged target app, such intrinsic functions require a ":" prefix by default. E.g. ":length(s)". 

## Settings

The prefix can be chosen or switched off. In the property grid of the debugger backend setup the option "IntrinsicPrefix" can be found. 

  * ipColon
    Require a ":" as prefix
  * ipExclamation
    Require a "!" as prefix
  * ipNoPrefix
    Don't require a prefix.  
If function calling is enabled for the watch, the intrinsic takes precedence, and may hide a call-able function in the target app. (Use the & to bypass the intrinsic and trigger function eval. E.g.: "&length()")



# Length(String-or-array)

Returns the length of an array or string. 

See [FpDebug#Strings_(vs_PChar_and_Array_of_Char)](<FpDebug.md> "FpDebug") for info about string types in the debugger. 

  


# CC(instance) {ChildClass}

Type-casts an object (class instance) to the instantiated class type. 

  
E.g. "Sender: TObject" is by default evaluated as TObject. ":CC(Sender)" will find the actual class of the instance and the result will be e.g. a TButton or TForm. ":CC(Sender)" is the same as either writing "TForm(Sender)" or "TButton(Sender)", but automatically adapts to the correct type. 

  
Simply watching ":CC(Sender)" returns the same result as checking the "Use instance class" checkbox in the watches properties. 

The difference between ":CC()" and "Use instance class" is 

  * "Use instance class" will not allow you to write "Sender.FOwner"
  * The intrinsic can be used: ":CC(Sender).FOwner"



    The "Use instance class" checkbox can then be used on the result of ":CC(Sender).FOwner"

  * ":CC()" can be nested: ":CC(:CC(Sender).FOwner).FHint"



  
":CC()" takes exactly one argument. If the argument is not an object, or can not be typecasted, then it is passed through. E.g. ":CC(1)" just return "1". 

# RefCnt(string-or-array)

Get FPC's internal ref-count for LongString(AnsiString) or dynamic arrays. 

Strings may work only with DWARF-3. 

# Pos(SubString, MainString [, CaseInsensitive=False])

Works like the "Pos" function. 

Accepts an optional 3rd argument of boolean type. If True, both strings will be run through "LowerCase" to make it case insensitive. (May not work for utf-8) 

# SubStr(Str, Start, Len [, ForcePtr=False])

Similar to "copy()" 

Gets a substring. 

  * Str: A String (Ansi or short) or pointer (pchar or other)



    Note: the handling of String vs Pchar may depend on the Dwarf version and the debuggers ability to distinguish between PChar and String
    Using Dwarf-2 a string may be seen as pchar instead

  * Start: A 1-based index, that must be 1 or greater (if the argument is a string and ForcePtr absent or false)
  * Len: Amount of chars to copy.
  * ForcePtr: Forces String to be handled the same as pointers



  
If acting on a string: 

  * If "Start" is greater than the length of Str, the the result is empty
  * If "Start + Len" exceeds the length or Str, then the result will be cut at the end of Str.



  
If acting on Pointers or ForcePtr 

  * "Start" is effectively 0-based. Start will be added to the address that the pointer points to.



    "Start" can be negative.

  * "Len" will not be limited.
  * This means any memory around the pointers destination can be selected and returned as string.



Pointers to WideChar are currently not detected as such. Therefore the "Start" index will act byte-wise on them. 

  
In "string mode" the entire string will be read first (subject to future optimization). This means it is limited to the configured mem-read limits. 

In "pointer mode" only the selected memory slice is read. (the "Len" is still subject to the configured limits)

---

_Source: [https://wiki.freepascal.org/FpDebug-Watches-Intrinsic-Functions](https://web.archive.org/web/20230207171139/https://wiki.freepascal.org/FpDebug-Watches-Intrinsic-Functions)_
