# WriteStr

The [procedure](<Procedure.md> "Procedure") [`writeStr`](<https://www.freepascal.org/docs-html/rtl/system/writestr.html>) formats arguments using the same syntax as [`write`/`writeLn`](<Write.md> "Write"), but the destination is a [string](<String.md> "String") variable. It is an [extended Pascal](<Extended_Pascal.md> "Extended Pascal") extension, but the [FPC](<FPC.md> "FPC")’s default [RTL](<RTL.md> "RTL") supports this procedure regardless of the current [compiler compatibility mode](<Compiler_Mode.md> "Compiler Mode"), not just [`{$mode extendedPascal}`](<Mode_extendedpascal.md> "Mode extendedpascal"). 

## Contents

  * 1 signature
  * 2 behavior
  * 3 application
  * 4 see also



## signature

`WriteStr` needs at least two arguments. The first parameter has to be a string-assignment-compatible [variable](<Variable.md> "Variable"), or, if allowed, a proper “writable constant”. The second and following arguments have the same type requirements and format specification syntax as for `write`/`writeLn`. 

## behavior

`WriteStr` behaves like `write`/`writeLn`, but does not need to open an external file for processing. 

If the destination parameter, the first argument, has a smaller capacity than the concatenated formatted strings, the result is clipped to the left, i. e. only the first x characters will be stored. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** As an extension, the FPC allows referencing the destination variable as a source variable. The extended Pascal states “It shall be an error if any of the write-parameters accesses the referenced string-variable.” The FPC ignores this requirement and does not yield an error.

## application

`WriteStr` is the go-to method of standard, yet simple formatting tasks. A few examples are: 

`writeStr(myString, someInteger:1);` is equivalent to `myString := intToStr(someInteger);` (or `myString := someInteger.toString();`), but the latter requires the [`sysUtils` unit](<sysutils.md> "sysutils"). Note, the minimum width specifier `:1` is optional in non-ISO-compliant compiler modes. 

In non-ISO-compliant modes `writeStr(tableCell, finding:24);` is equivalent to `tableCell := padLeft(finding, 24);` (or `tableCell := finding.padLeft(24);` if `finding` is an [ANSI string](<Ansistring.md> "Ansistring")), but [`padLeft`](<https://www.freepascal.org/docs-html/rtl/strutils/padleft.html>) requires the `strUtils` unit.   
For reference, in ISO-compliant modes `writeStr(tableCell, caption:10);` is equivalent to the more complicated `tableCell := caption.subString(0, 10).padLeft(10);`.

## see also

  * [`readStr`](</index.php?title=ReadStr&action=edit&redlink=1> "ReadStr \(page does not exist\)") performs the reverse operation
  * [`str`](<Str.md> "Str"), similar to `writeStr` but less versatile (only _one_ numeric or ordinal type argument permitted)

---

_Source: [https://wiki.freepascal.org/WriteStr](https://web.archive.org/web/20240420230336/https://wiki.freepascal.org/WriteStr)_
