# Overview of Free Pascal and Lazarus

│ **English (en)** │  **[русский (ru)](<../ru/Overview_of_Free_Pascal_and_Lazarus.md>)** │

[Free Pascal (FPC)](<FPC.md> "FPC") is an open-source [Pascal](<Pascal.md> "Pascal") [compiler](<Compiler.md> "Compiler") with two notable features: a high degree of [Delphi](<Delphi.md> "Delphi") compatibility and availability on a variety of platforms, including Windows, Mac OS X, and [Linux](<Linux.md> "Linux"). Free Pascal's compatibility with Delphi includes not only support for the same [Object Pascal](<Object_Pascal.md> "Object Pascal") programming language that Delphi uses, but also for many of the same libraries of powerful [routines](<Routine.md> "Routine") and [classes](<Class.md> "Class") that Delphi is justly known for. This includes familiar [units](<Unit.md> "Unit") such as System, SysUtils, StrUtils, DateUtils, Classes, Variants, Math, IniFiles and Registry, which are included with Free Pascal on all supported platforms. Free Pascal also includes units such as Windows, ShellAPI, BaseUnix, Unix and DynLibs for accessing features specific to an [operating system](<operating_system.md> "operating system"). These dozen or so units make up the core of what is usually referred to as the Free Pascal [run-time library (RTL)](<RTL.md> "RTL"). 

Lazarus is an open-source development system that builds on the Free Pascal compiler by adding an [integrated development environment (IDE)](<IDE.md> "IDE") that includes a [syntax-highlighting](<Syntax_highlighting.md> "Syntax highlighting") code editor and visual form designer, as well as a component library that's highly compatible with Delphi's [Visual Component Library (VCL)](<VCL.md> "VCL"). The [Lazarus Component Library (LCL)](<LCL.md> "LCL") includes equivalents for many of the familiar VCL controls such as [forms](<TForm.md> "TForm"), [buttons](<TButton.md> "TButton"), text boxes and so on that are used to create [applications](<Application.md> "Application") that have a graphical user interface (GUI). 

Both Free Pascal and Lazarus are written in Pascal. Complete [source code](<Source_code.md> "Source code") is available not only for the Free Pascal compiler and Lazarus IDE, but also for all units that make up the Free Pascal RTL and Lazarus LCL. 

Like Delphi, Free Pascal and Lazarus are both general-purpose programming tools, meaning you can develop a wide variety of [programs](<Executable_program.md> "Executable program") with them, including the following: 

## Contents

  * 1 Console applications
  * 2 Dynamically loadable libraries
  * 3 GUI apps
  * 4 Lazarus Architecture
  * 5 See also



### Console applications

Console apps don't have a GUI. Instead they are usually launched from the console, [read](<Read.md> "Read") their input from the console, and [write](<Write.md> "Write") their output to the console. In Windows the console is usually referred to as a command prompt window. In OS X and Linux the console is referred to as a terminal window. Console apps include things like small utilities such as the Windows FC (file compare) program or the Unix cd and cp commands. Console apps can also be powerful number crunching, modeling, or data processing programs that don't need a fancy GUI because they're started by other programs or from batch files (or shell scripts on unix/linux). The Free Pascal compiler and the utility programs included with it are all console apps, meaning they can be run from the console, a batch file, or from the Lazarus IDE. 

You can create a console app with no more than a text editor and the Free Pascal compiler. You don't have to use Lazarus to develop console apps. However, if you prefer to work in an integrated environment, you can use Lazarus to create a project for the console app and then edit and compile the code in the Lazarus IDE. 

### Dynamically loadable libraries

A dynamically loadable library is usually a collection of compiled functions that can be called by other programs. As the name suggests, the library is not linked into your executable at [compile-time](<Compile_time.md> "Compile time"), but instead is loaded at [runtime](<runtime.md> "runtime"). In Windows, a library file has a .dll extension (dynamic-link library, or DLL). In OS X, a library file has a .dylib extension (dynamic shared library). In Linux, a library file has an .so extension (shared object library). Dynamically loadable libraries are typically used to develop add-ons for other programs, to develop libraries that can be called by programs written in other languages such as C and C++, or to partition large projects into chunks so that the project's developers don't step on each other's toes. Windows itself is made up of hundreds of DLLs, as are many other large applications such as OpenOffice.org. 

Like console apps, you only need a text editor and the Free Pascal compiler to develop a library, although you can also create a Lazarus project for the library and develop the library in the Lazarus IDE. 

On Windows, DLLs have an undeserved reputation for being overly complex and unstable. This has more to do with the way they've sometimes been installed than it does with DLLs themselves. Actually, since passing information to and from DLLs is normally done with standard, simple data types (rather than with language-specific objects or structures), this requirement forces programmers to pay more attention to what they're doing. If done right, this can result in better, more stable programs. 

### GUI apps

Most of the programs we use daily are GUI apps, including word processors, Web browsers, spreadsheet programs, even many development tools. Lazarus and Delphi are both good examples of full-featured GUI apps. When developing a GUI app with Lazarus, not only do you develop units of Pascal code, you also design forms containing visual controls such as buttons and list boxes. Like Delphi, form design in Lazarus is done visually. Control properties can be set in the IDE or in code. 

Since the LCL controls are available on all supported platforms, a GUI app developed on one platform (for example, Windows) can be compiled on another platform (for example, OS X or Linux) without any changes to the form design or the form code files. 

### Lazarus Architecture

Programs written in Lazarus follow the architecture drawn here: 

[![Lazarus App Architecture.png](https://wiki.freepascal.org/images/f/f1/Lazarus_App_Architecture.png)](</File:Lazarus_App_Architecture.png>)

And the LCL itself follows the following architecture: 

[![LCL Architecture.png](https://wiki.freepascal.org/images/5/50/LCL_Architecture.png)](</File:LCL_Architecture.png>)

The LCL has platform independent units, like controls, forms, buttons and grids. They can not work on their own. They are abstract and need a backend. The LCL provides many different LCL interfaces that communicate with backends or "widgetsets" (win32, gtk, qt, carbon, cocoa, ...). A backend is compiled in by simply using the package LCL and adding the unit "interfaces" as one of the first units to your program. When you create an application with the Lazarus IDE this is done automatically. The backend is chosen at compile time and can not be changed at runtime. 

### See also

  * [Lazarus Tutorial](<Lazarus_Tutorial.md> "Lazarus Tutorial")

---

_Source: [https://wiki.freepascal.org/Overview_of_Free_Pascal_and_Lazarus](https://web.archive.org/web/20250310164741/https://wiki.freepascal.org/Overview_of_Free_Pascal_and_Lazarus)_
