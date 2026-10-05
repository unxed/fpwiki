# RTL

│ **[Deutsch (de)](</RTL/de> "RTL/de")** │  **English (en)** │  **[español (es)](</RTL/es> "RTL/es")** │  **[français (fr)](</RTL/fr> "RTL/fr")** │  **[Bahasa Indonesia (id)](</RTL/id> "RTL/id")** │  **[日本語 (ja)](</RTL/ja> "RTL/ja")** │  **[русский (ru)](<../ru/RTL.md> "RTL/ru")** │    
****

Free Pascal **Runtime Library** (**RTL**) 

A _Run-Time Library_ is the set of [source code](<Source_code.md> "Source code") files that are used to create the portion of the [application](<Application.md> "Application") which is generated or included by the [compiler](<Compiler.md> "Compiler") and used for the following purposes: 

  * [Initialization](<Initialization.md> "Initialization") of the run-time-library itself prior to activation of the user's application
  * Initialization and [startup](</index.php?title=startup&action=edit&redlink=1> "startup \(page does not exist\)") of the application
  * providing standard Pascal services to the application (support for the [Write](<Write.md> "Write") and [Writeln](</index.php?title=Writeln&action=edit&redlink=1> "Writeln \(page does not exist\)") [standard functions](</index.php?title=standard_function&action=edit&redlink=1> "standard function \(page does not exist\)"), for example)
  * providing any [library functions](</index.php?title=library_function&action=edit&redlink=1> "library function \(page does not exist\)") which are not defined [inline](</index.php?title=inline&action=edit&redlink=1> "inline \(page does not exist\)") by the compiler such as mathematical routines
  * providing extended Pascal services to the application (support for the [Assign](<Assign.md> "Assign") [extended function](</index.php?title=extended_function&action=edit&redlink=1> "extended function \(page does not exist\)") to assign a reference to an [external file](</index.php?title=external_file&action=edit&redlink=1> "external file \(page does not exist\)") to a [file variable](</index.php?title=file_variable&action=edit&redlink=1> "file variable \(page does not exist\)")).
  * providing a conversion for local equivalents for a standard or extended function into the local equivalent (for example, changing the Write or writeln statement to write to a window in a windowed environment if the file variable is pointing to a window, to write to the screen in a text environment if the file is pointing to the terminal, or to write to a file if the file variable is pointing to an external file.



## Contents

  * 1 RTL units
  * 2 Using RTL
  * 3 Developing RTL
  * 4 See Also



## RTL units

There are many different units with partly overlapping functionality. This is caused by different reasons, especially: 

  * FPC tries to be compatible to two different compilers ([Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal")/[Borland Pascal](<Borland_Pascal.md> "Borland Pascal") and [Delphi](<Delphi.md> "Delphi")) with slightly different syntax and different sets of supplied units for two different paradigms (procedural and object oriented programming)
  * FPC supports many different platforms requiring support of both platform specific API functions and common routines available across all or at least most supported platforms.



A simplified overview can be found in this [unit categorization](<Unit_categorization.md> "Unit categorization"). A detailed description of individual units and included routines is available in the [RTL unit reference manual](<http://lazarus-ccr.sourceforge.net/docs/rtl/index.html> "doc:rtl/index.html") provided as part of the FPC documentation. 

## Using RTL

Some problems using the [crt](<crt_unit.md> "crt unit") and the [video](<video_unit.md> "video unit") units with unix terminals are described here: [Terminal & Fonts](<Terminal_&_Fonts.md> "Terminal & Fonts")

Read about the API units (Video/Mouse/Keyboard) and the Crt Unix, the bigger picture in [KVM API and Crt future](<KVM_API_and_Crt_future.md> "KVM API and Crt future")

The windows interface units have an own page [here](<Windows_API_units.md> "Windows API units")

## Developing RTL

[RTL development articles](<RTL_development_articles.md> "RTL development articles")

## See Also

  * [Reference for package 'rtl'](<https://www.freepascal.org/docs-html/rtl/>)—official documentation.
  * [Another link for the official documentation](<https://lazarus-ccr.sourceforge.io/docs/rtl/>) (same as above).


  * [“Free Pascal runtime library”](<https://en.wikipedia.org/wiki/Free_Pascal_Runtime_Library>) in the English Wikipedia.

---

_Source: [https://wiki.freepascal.org/RTL](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/RTL)_
