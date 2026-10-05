# Mode ObjFPC

│ **English (en)** │  **[español (es)](</Mode_ObjFPC/es> "Mode ObjFPC/es")** │  **[français (fr)](</Mode_ObjFPC/fr> "Mode ObjFPC/fr")** │    
****

The mode **ObjFPC** , switched on with `{$mode objfpc}` in [source code](<Source_code.md> "Source code"), or `-Mobjfpc` on the [command line](<Command-line_interface.md> "Command-line interface"), is the default mode for [Lazarus](<Lazarus.md> "Lazarus") source files (the default [compiler mode](<Compiler_Mode.md> "Compiler Mode") when not using Lazarus is [FPC mode](<Mode_FPC.md> "Mode FPC")). 

Using the ObjFPC mode has the following consequences: 

  1. The [address operator](<@.md> "@") has to be used to assign procedural variables. Use `{$modeswitch classicProcVars+}` to disable this requirement.
  2. A [forward declaration](<Forward.md> "Forward") must be repeated exactly the same by the [implementation](<Implementation.md> "Implementation") of a [`function`](<Function.md> "Function")/[`procedure`](<Procedure.md> "Procedure"). In particular, parameters cannot be omitted when implementing the function or procedure, and the calling convention must be repeated as well.
  3. [Overloading](</index.php?title=Overload&action=edit&redlink=1> "Overload \(page does not exist\)") of functions is allowed.
  4. Nested [comments](<Comments.md> "Comments") are allowed.
  5. The [Objpas](</index.php?title=Objpas&action=edit&redlink=1> "Objpas \(page does not exist\)") unit is loaded right after the system unit. One of the consequences of this is that the [type `integer`](<Integer.md> "Integer") is redefined as [`longint`](<Longint.md> "Longint").
  6. The [`cvar`](</index.php?title=Cvar&action=edit&redlink=1> "Cvar \(page does not exist\)") type may be used.
  7. [`PChar`s](<PChar.md> "PChar") are converted to [`string`s](<String.md> "String") automatically.
  8. Parameters in class methods cannot have the same names as class properties.
  9. Strings are [`shortstring`s](<Shortstring.md> "Shortstring") by default. This may be changed by using the `-Sh` command line switch or the `{$H+}` switch.
  10. [Exceptions](<Exceptions.md> "Exceptions"), [classes](<Class.md> "Class") and [Interfaces](<Interfaces.md> "Interfaces") are enabled.
  11. Inline code is allowed: There is no need to enable it with the `{$INLINE}` directive.

---

_Source: [https://wiki.freepascal.org/Mode_ObjFPC](https://web.archive.org/web/20241117171923/https://wiki.freepascal.org/Mode_ObjFPC)_
