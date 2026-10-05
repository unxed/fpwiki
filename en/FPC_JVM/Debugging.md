# FPC JVM/Debugging

## Contents

  * 1 Debugging help
    * 1.1 Bytecode verification
      * 1.1.1 Download BCEL snapshot
      * 1.1.2 Running the verifier
    * 1.2 Java debuggers
      * 1.2.1 Source code searching logic
      * 1.2.2 jdb
      * 1.2.3 Eclipse



# Debugging help

The compiler does not catch all errors in the source code that may result in invalid JVM bytecode. Examples include uninitialized variables and some invalid typecasts. The consequence is that you will get verification errors from the JVM at run time. 

If the source code is compiled with the -g option, all exception backtraces will contain source line information (except in case of include files, as explained earlier). 

Additionally, even if the bytecode passes the verifier, there can still be logic bugs in your program. 

## Bytecode verification

The Apache Commons [BCEL project](<http://commons.apache.org/bcel/>) includes a very handy JVM byte code verifier. Unfortunately, the current release version (5.2) is quite old and does not support all JDK 1.5 features used by FPC. You have to use the svn version instead. 

### Download BCEL snapshot

A prebuilt version of the patched BCEL 6.0 snapshot is included in the [snapshots](<../FPC_JVM.md> "FPC JVM") and can also be downloaded as part of the [Java-specific utilities](<ftp://ftp.freepascal.org/pub/fpc/contrib/jvm/fpcjvmutilities.zip>). 

Compilation instructions and a required patch can be found at [FPC_JVM/Building](<Building.md> "FPC JVM/Building"). 

### Running the verifier

To run the verifier on, e.g., the system unit class _org.freepascal.rtl.system_
    
    
     java -Xmx1000000000 -cp /full/path/to/bcel/target/bcel-6.0-SNAPSHOT.jar:.:/full/path/to/fpc/rtl/units/jvm-java org.apache.bcel.verifier.Verifier org.freepascal.rtl.System
    

(that is for Unix systems, on Windows replace the forward slashes with backslashes, and separate directories with _;_ instead of with _:_). 

On Unix systems, you can create a shell script called _jverify.sh_ with the following contents: 
    
    
     #!/bin/sh
     java -Xmx1000000000 -cp /full/path/to/bcel/target/bcel-6.0-SNAPSHOT.jar:.:/full/path/to/fpc/rtl/units/jvm-java org.apache.bcel.verifier.Verifier $@
    

This makes it possible to run it as e.g. _jverify.sh org.freepascal.rtl.System_. The reason for the large maximum heap size is because the verifier otherwise can run out of memory while verifying more complex classes. 

The verifier will report byte code offsets rather than source code line numbers when it detects an error. To obtain the corresponding source code line number, run _javap -l_ on the same class. It will list the translation table from byte code offsets to source code line numbers for every function in that class. 

## Java debuggers

### Source code searching logic

All Java debuggers will look for source code in the same way as they look for classes. This means that if your class is in a package called _org.freepascal.rtl_ , then the debugger will look for the source file containing that class in a directory structure _org/freepascal/rtl_ that is located somewhere in the class or source directory path. 

I.e., if you specify _/home/me/fpc/rtl/java_ as a source directory, the debugger will look in _/home/me/fpc/rtl/java/org/freepascal/rtl_ for the sources of any class in the _org.freepascal.rtl_ package rather than in the top level directory you specified. One way to deal with this is to create symbolic links to the source code next to the class files, if your operating system supports that, or to copy the source files next to the class files. 

### jdb

_jdb_ is the standard command line debugger included with the JDK. It is very primitive and requires lots of typing, but other than that works fine. Some useful commands: 

  * **Run debugger** (see the general note above for information about the _sourcebasedirectory_ parameter, and make sure that you do not put the _.class_ extension after the class name) 
    * Unix: `jdb -classpath _/home/me/fpc/rtl/units/jvm-java:/some/other/directory_ -sourcepath _sourcebasedirectory_ _MainProgramClass_`
    * Windows: `jdb -classpath _D:\fpcjvm\units\rtl\jvm-java;C:\some\other\directory_ -sourcepath _sourcebasedirectory_ _MainProgramClass_`
  * **Start program and stop in main function**
    * `stop in _progname_.main`
    * `run`
  * **Continue execution** after it stopped at a breakpoint 
    * `cont`
  * Go to the next source line, **stepping over calls**
    * `next`
  * Go to the next source line, **stepping into calls** (note: stepping into procedure variable calls or virtual class methods/constructors will not work, because they contain compiler generated code without debug information; set break points at the appropriate places to step into them) 
    * `step`
  * **Repeat the last command** you typed 
    * `!!`
  * **Set a breakpoint at the start of a particular function**
    * `stop in packagename.functionname`
    * e.g. `stop in org.freepascal.rtl.randomize`
  * **Set a breakpoint at a particular line** (note: "stop at" instead of "stop in", and specify a class name rather than a source file name) 
    * `stop at packagename.class:linenumber`
    * e.g. `stop at TMyClass:123` or `stop at MyUnit:432` (in case the breakpoint is in a procedure/function that is not a method)
  * Show the **local variables and parameters** in the current stack frame 
    * `locals`
  * **Print the value of an expression** (can include function calls) 
    * `print variablename` (case-sensitive!)
    * `print variablename.field` (case-sensitive!)
  * **List the source code** around the current location in the code 
    * `list`
  * See the current **backtrace**
    * `where`
    * `wherei` (includes byte code offsets)
  * Stop when an **exception is raised** (only works once the program has been started) 
    * `catch all packagename.exceptionname`
    * e.g. `catch all java.lang.NullPointerException`
    * To catch all raised exceptions: `catch all java.lang.Throwable` (note that this will include a java.lang.ClassNotFoundException for every new class that gets loaded)
  * Get a complete **trace of all method calls and exits** , including the values they return (lots of output!) 
    * `trace go methods`
  * When you are stopped at a break point, **go up/down one or more frames in the stack** (note that these are relative to where you are currently in the stack) 
    * `up 1`
    * `down 2`
  * **Watchpoints** (reads and/or writes to a particular field -- note that these watchpoints are per class type, not per instance!) 
    * `watch access package.classname.field` (-> writes)
    * `watch all package.classname.field` (-> reads + writes)
    * `unwatch all package.classname.field` (-> remove watchpoint)
  * **Usage information**
    * `help`



### Eclipse

It is undoubtedly possible to set up Eclipse so that it will debug FPC-generated Java class files. Anyone who figures out the details, feel free to describe them here.

---

_Source: [https://wiki.freepascal.org/FPC_JVM/Debugging](https://web.archive.org/web/20250121205752/https://wiki.freepascal.org/FPC_JVM/Debugging)_
