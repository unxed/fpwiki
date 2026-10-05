# sysutils

│ **English (en)** │

The [unit](<Unit.md> "Unit") **`sysUtils`** shipped with the [FPC’s](<FPC.md> "FPC") default [run-time library](<RTL.md> "RTL") provides many system utilities. It attempts to be as compatible to [Delphi’s](<Delphi.md> "Delphi") `sysUtils` unit as possible. However, the FPC version is available on [all platforms that the FPC supports](<Platform_list.md> "Platform list"). It does not contain any Windows-related routines or other highly platform-specific functionality. 

## Notable functionality

  * [`format`](<Format_function.md> "Format function")
  * [`executeProcess` for executing external programs](<Executing_External_Programs.md> "Executing External Programs")
  * several routines ensuring productive use of [`tDateTime`](<TDateTime.md> "TDateTime")
  * [type helpers for all basic data types](<https://www.freepascal.org/docs-html/rtl/sysutils/typehelpers.html>)
  * [`freeAndNil`](<FreeAndNil.md> "FreeAndNil")
  * routines to access [environment variables](<Command_line_parameters_and_environment_variables.md> "Command line parameters and environment variables")



## Caveats

[If the `sysUtils` unit is included](<https://www.freepascal.org/docs-html/rtl/system/runtimeerrors.html>), all [run-time errors](<runtime_error.md> "runtime error") become [exceptions](<Exceptions.md> "Exceptions"), which virtually forces you to use a [compiler mode](<Compiler_Mode.md> "Compiler Mode") (or [mode switch](<modeswitch.md> "modeswitch")) that allows exception treatment. To catch an exception by its name you will need to include `sysUtils`, even though the module itself does not use any of the included system utilities. 

Changing run-time errors to exceptions has a _global_ effect. The following program will terminate with an uncaught _exception_ , even though it does not list `sysUtils` in its `uses`-clause: The `sysUtils` unit is _implicitly_ included via the [`strUtils` unit](<https://www.freepascal.org/docs-html/rtl/strutils/index.html>): 
    
    
    program implicitSysUtilsCaveat(input, output, stdErr);
    uses
    	strUtils;
    var
    	x: file of char;
    	c: char;
    begin
    	// deliberately cause an error for demonstration purposes
    	read(x, c); 
    end.
    

Also, if [size matters](<Size_Matters.md> "Size Matters"), using `sysUtils` is by design not the smartest choice. 

## See also

  * [`sysUtils` reference](<https://www.freepascal.org/docs-html/rtl/sysutils/index.html>)
  * [Delphi’s `sysUtils` unit](<http://docwiki.embarcadero.com/Libraries/Sydney/en/System.SysUtils>)
  * [`libc` library](<libc_library.md> "libc library")
  * [`system` unit](<System_unit.md> "System unit")

---

_Source: [https://wiki.freepascal.org/sysutils](https://web.archive.org/web/20240920204158/https://wiki.freepascal.org/sysutils)_
