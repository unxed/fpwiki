# runtime error

│ **English (en)** │  **[suomi (fi)](</runtime_error/fi> "runtime error/fi")** │    
****

A run-time error is an irreparable error condition that arises during the [run-time](<runtime.md> "runtime"), i.e. the execution of a [program](<Executable_program.md> "Executable program"). 

## Contents

  * 1 Behavior
  * 2 Comparative remarks
    * 2.1 Compile-time errors
    * 2.2 Exceptions
  * 3 See also



## Behavior

The [FPC](<FPC.md> "FPC") inserts code to detect a vast number of error situations. If such a situation is encountered, the standard [run-time library](<RTL.md> "RTL") will initiate the termination of the program. A run-time error number, and the address the error occurred at is being printed. This is the safest and cheapest error treatment. 

## Comparative remarks

### Compile-time errors

In contrast to [compile-time errors](<compile-time_error.md> "compile-time error"), which the compiler detects during [compilation](<Compile_time.md> "Compile time"), run-time errors depend on the state of the program, thus can not be foreseen in advance. If a compile-time error is encountered, no executable program is generated. 

### Exceptions

Run-time errors are the classical imperative approach in order to avoid inconsistent program states, which may eventually cause faulty program behavior. If FPC's [`sysUtils` unit](<https://www.freepascal.org/docs-html/rtl/sysutils/index.html>) is included, all run-time errors become [exceptions](<Exceptions.md> "Exceptions") (cf. [`system.runTimeErrors`](<https://www.freepascal.org/docs-html/rtl/system/runtimeerrors.html>) for details). Unlike run-time errors those can be caught by [`try`](<Try.md> "Try")...[ `except`](<Except.md> "Except") [`on`](<On.md> "On")...[`do`](<Do.md> "Do") ... [`end`](<End.md> "End") [blocks](<Block.md> "Block"), provided a mode allowing exceptions – such as [`{$mode ObjFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC") or [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi") – is being used. A run-time error causes the program to terminate, while an exception may give the opportunity to “fix” the problem. This standard behavior of [`system.runError`](<https://www.freepascal.org/docs-html/rtl/system/runerror.html>) can be altered by assigning a non-[`nil`](<Nil.md> "Nil") value to [`system.errorProc`](<https://www.freepascal.org/docs-html/rtl/system/errorproc.html>). 

## See also

  * [Appendix D in the Free Pascal User's Guide: “Run-time errors”](<https://www.freepascal.org/docs-html/user/userap4.html>)
  * [procedure `runError`](<RunError.md> "RunError")
  * [“Line numbers in run-time error backtraces”](<https://www.freepascal.org/docs-html/current/user/userse58.html>) in the Free Pascal User's Guide
  * [`system.returnNilIfGrowHeapFails`](<https://www.freepascal.org/docs-html/rtl/system/returnnilifgrowheapfails.html>)

---

_Source: [https://wiki.freepascal.org/runtime_error](https://web.archive.org/web/20250421021019/https://wiki.freepascal.org/runtime_error)_
