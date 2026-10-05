# FPC JVM/Usage

## Compiling source code

The name of the compiler binary is _ppcjvm_ (_ppcjvm.exe_). Compiling source files works the same as on other platforms: _ppcjvm <options> name_of_source_file_. The most important options are _-g_ (generate debug information) and _-O2_ (enable optimizations). 

Notice that a separate class file will be generated for each non-primitive, non-array type that is defined in a unit or program, and another class for the unit/program itself. Additionally, the names of these classes will correspond exactly to the name of the declarations in the source code. Keep in mind that the JVM and the Java language are case-sensitive. 

Finally, if a program does not have a name (because it does not include a _program somename;_ statement), the default name _Program_ will be used for its corresponding class file. 

If a module contains a _[{$namespace xxx}](<Language.md> "FPC JVM/Language")_ directive, the generated class files for that module will be stored in the directory _xxx_. 

## Running compiled programs

The _java_ program is used to run Java class files. There are a number of important command line parameters: 

  * **-cp xxx**. This parameter defines the class path. You can compare it to the unit search path of the compiler, in the sense that it tells the _java_ program in which directories it should search for class files. Note that in case a class is part of a package, _java_ will search for the class file inside a directory (hierarchy) called _packagename_ in the class path. The default class path already includes the standard JDK classes. For most programs, you will have to include at least the directory containing the FPC RTL classes and the directory in which the class for the program is located. Assuming that the class you want to run is in the current directory: 
    * On Unix-like platforms (directories are separated by _:_) 
      * For official releases: _-cp / <prefix>/lib/fpc/<version>/units/jvm-java/rtl:._
      * For snapshots: _-cp /full/path/to/fpcjvm/units/rtl/jvm-java:._
    * On Windows (directories are separated by _;_) 
      * For official releases: _-cp C:\installdir\units\jvm-java\rtl;._
      * For snapshots: _-cp C:\full\path\to\fpcjvm\units\rtl\jvm-java;._
  * **-Dfile.encoding=utf-8**. This command line parameter sets the encoding of ansistrings and shortstring to utf-8, rather than using the default (which is the hardcoded to MacRoman on macOS, so on that platform you should almost always use this parameter). While there are programatic ways to change the default encoding at run time, these do not change everything (e.g., they won't change the encoding of the _java.lang.System.out_ stream). You can of course also specify other encodings using this parameter.



Note that when running a Java program, you have to specify the name of the main _class_ , not of the _class file name_. This means that you must not specify the _.class_ extension. 

## Known bugs

  * The _-B_ option is currently non-functional and cannot be used to force the compiler to rebuild all sources (nor to delete previously generated class files)
  * Local record types (record types defined locally in procedures/functions/methods) do not yet work. They work fine if they are defined as local types in classes though.
  * ~~There are some memory leaks in _compiler/symcreat.pas_. Fixing them currently crashes the compiler, it requires some changes to the scanner file handling so that macro-files used to [inject source code](<Internals.md> "FPC JVM/Internals") are not registered~~ _fixed in svn r19248_
  * There are a number of constructs of which the compiler knows that they are invalid, but doesn't give an error yet at compile time (including certain typecasts and pointer arithmetic)
  * ~~When compiling an i386 or x86-64 compiler from the jvm branch, functions returning a record in SSE registers will trigger an internal error because this is not yet handled by the new high level code generator.~~ _fixed in svn r18806_
  * The compiler does not yet check whether you use reserved Java keywords as global variable/field/method/procedure/function names, even though this is forbidden by the JVM specifications
  * The _-gttt_ command line parameter is not yet supported (although by initializing local variables with data you actually make the code more likely to contain hidden bugs, since the JVM normally checks for uninitialized accesses and such functionality would disable that)
  * ~~Overflow checking is not yet supported (range checking is supported however)~~ _fixed in svn r22808_

---

_Source: [https://wiki.freepascal.org/FPC_JVM/Usage](https://web.archive.org/web/20250516140112/https://wiki.freepascal.org/FPC_JVM/Usage)_
