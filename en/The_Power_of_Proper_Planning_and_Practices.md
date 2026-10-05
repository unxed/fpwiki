# The Power of Proper Planning and Practices

│ **English (en)** │

## Contents

  * 1 Plan for cross-platform up front
  * 2 Separate code into UI and non-UI units
  * 3 Use batch files and shell files
  * 4 How to avoid using a debugger
  * 5 How to avoid disappointing your users
  * 6 See also



### Plan for cross-platform up front

See also: 

  * [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")
  * [Introduction to platform-sensitive development](<Introduction_to_platform-sensitive_development.md> "Introduction to platform-sensitive development")



There are several advantages to doing your development in a cross-platform way. Probably the most important advantage is that it can result in better, more reliable software. This is because cross-platform development requires that you spend more time planning your software before you start writing code. It also forces you to develop good programming habits, to test your software more, and to develop a more disciplined approach toward what you're doing. These things (planning, testing, discipline) tend to spill over into other aspects of your development, resulting in still more improvements in design and reliability. 

Another advantage to cross-platform development is that it exposes you to other platforms. For example, many Windows developers and probably all Linux developers could benefit from some time spent simply using a Mac as a way of developing an appreciation for software ease of use and aesthetics. And many Windows developers could benefit from seeing how Linux tends to use text files and scripts for many things, without any loss of application performance. 

A third advantage to cross-platform development is that it is by definition _inclusive_ , which forces you to consider your users more than most single-platform programmers do. 

The best way to make an application cross-platform is to design it to be cross-platform. If you plan on adding cross-platform support to your finished application later, you may find that you'll have to rewrite parts of it or, worse, find yourself in a Windows-only cul-de-sac that makes adding cross-platform support difficult or impossible. 

If you've already developed an application for Windows with Delphi, it's possible in some situations to convert it to cross-platform app without starting over. If you want to do a one-way conversion of your Delphi code and forms to Lazarus, you can use some of the conversion commands on the Lazarus Tools menu. You can also convert your Delphi app to Lazarus, but retain Delphi compatibility (see this [[XDev Toolkit|link] for more information). 

### Separate code into UI and non-UI units

See also: 

  * [SQLdb Tutorial4](<SQLdb_Tutorial4.md> "SQLdb Tutorial4") Info about using data modules with SQLDB components. Data modules can also be used for non-database non-GUI controls.



Probably one of the most important programming practices you can employ with any application, whether cross-platform or not, is to divide your code into user interface (UI) units and non-user interface (non-UI) units. In general, this means that your non-UI units should not implement any of the program's user interface and your UI units should not contain any of the computational or database parts of the program. In practice, a good way to enforce this separation is to require that the units you develop only use units that are appropriate to their side of this "divide". For example: 

  * Your non-UI units should not use LCL units such as Forms, Dialogs, StdCtrls, Button, Graphics, Controls, LMessages, and so forth, or any of your own units that use these units.
  * Your UI units should probably not be using units such as Math, Dbf, MySQLDB4, etc.



Follow the example of the Free Pascal and Lazarus units themselves, where general-purpose units like SysUtils, Classes, Variants and [LazUtils](<LazUtils.md> "LazUtils") don't use any LCL units and LCL units like Forms and Controls don't supply any general-purpose routines. 

There are several advantages to dividing up your code like this. For example, it means that you will be able to use your non-UI units in any type of program, whether a GUI app, library, console app, even a Web app, without making any changes to the code. And it also means that if necessary you could develop a completely different user interface for your program or switch to a different UI library or control without changing the underlying non-UI code. It's also critical that this partitioning be done in large projects so that multiple developers can work on different parts of the program without stepping on each other's toes. 

You may also find it helpful to further subdivide your non-UI code. For example, you might want to put all database access code in one set of units and all business logic and number-crunching code in another set of units. In addition, you might want to put all platform-dependent code (shelling, registry, clipboard, etc.) in its own set of units to isolate that code and make it easier to port to other platforms. 

### Use batch files and shell files

Even though Lazarus allows you to edit and compile code and set most compiler switches without ever leaving the IDE, you'll probably find it useful to create several batch files for each project. Batch files are just special text files containing console commands; you create them with a text editor. With Windows, batch files have a .bat or .cmd extension; with OS X and Linux, batch files have an .sh extension. You typically run a batch file by entering its name in a console window, as follows: 

With Windows: 
    
    
     mybatch.bat
    

Note that you can omit the .bat extension if there's no .exe file with the same name. 

With Unix: 
    
    
     ./mybatch.sh
    

There are many way that batch files can be used in development. For example, you might have one set of compiler options that you use during development and another set that you use to create the executable that you distribute. Here's an example of a command you can put in a batch file that runs the Free Pascal compiler. 

On Windows: 
    
    
    c:\lazarus\pp\bin\i386-win32\fpc.exe -Sda -Cirot -gl %1
    

On Unix/Linux: 
    
    
    /usr/local/bin/fpc -Sda -Cirot -gl $@
    

The above command compiles the unit or non-GUI program specified on the command line with Delphi compatibility, assertions, I/O checking, range checking, overflow checking, stack checking, and line number tracing turned on. Note that you can add additional pre- and post-compile commands to a batch file. For example, you could run the strip.exe utility to shrink the size of your executable or copy the executable to a different folder for testing. 

Here's a similar command that you can put in a batch file to compile a GUI program that you specify on the command line: 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The examples below directly call the FPC compiler. It may be easier to use [lazbuild](<lazbuild.md> "lazbuild") which will let you compile Lazarus projects for you without any need to set include paths etc

On Windows (all on one line): 
    
    
    c:\lazarus\pp\bin\i386-win32\fpc.exe -dLCL -WG -Sda -Cirot -gl -Fuc:\lazarus\lcl\units\i386-win32;c:\lazarus\lcl\units\i386-win32\win32 %1
    

On OS X: 
    
    
    /usr/local/bin/fpc -dLCL -WG -Sda -Cirot -gl \
    -Fu/usr/local/share/lazarus/lcl/units/powerpc-darwin \
    -Fu/usr/local/share/lazarus/lcl/units/powerpc-darwin/gtk \
    -Fl/usr/X11R6/lib/ -Fl/sw/lib/ $@
    

  


### How to avoid using a debugger

See also: 

  * [Debugger Setup](<Debugger_Setup.md> "Debugger Setup")
  * [GDB Debugger Tips](<GDB_Debugger_Tips.md> "GDB Debugger Tips")



Using a debugger is rarely much fun. It often means that you've given up trying to figure out what's wrong with your program and have turned to the debugger as a last resort. Here are some simple techniques that you can employ that will help minimize your use of a debugger: 

  * Set up a debug and a release/default build mode. Use the debug build mode to enable run-time checks, asseertions, line tracing. You can use the default/release mode to disable them. 
    * Develop with all run-time checks turned on. You can turn off some or all checks in your release/default build used for distribution to your users, although code size and performance improvements of turning checks off are pretty negligible. Plus, leaving checks turned on can help you locate bugs that turn up in production (e.g. hitting rarely used code paths).
    * Use assertions. Like run-time checks, assertions can be turned off later, although leaving them on can help you diagnose bugs that your users encounter.
    * Set line number tracing on. If your code handles exceptions, you can determine the line number and source file name of where the exception occurred using line number tracing and the FPC BackTraceStrFunc function, as in the example code below.


    
    
    try
      raise Exception.Create('Exception here');  
    except
      // Handle exception here.
      {$IFDEF FPC}
        WriteLn(BackTraceStrFunc(ExceptAddr));
      {$ENDIF}
    end;
    

The {$IFDEF} and {$ENDIF} lines ensure that you can compile this code with Delphi too. 

  * Initialize variables. Although both Delphi and FPC initialize global variables and object fields, variables local to routines are not guaranteed to be initialized. It's a good idea to initialize all variables just to be clear about what their initial values are.
  * Turn on compiler warnings and pay attention to them.
  * Use "with" sparingly. Although the Pascal with statement can save you a lot of typing, it can also hide incorrect field references if you use nested with statements or if a field with the same name as a field in your with's target is added someday to an ancestor class.
  * Run your code through multiple compilers (or compilers on different operating systems, e.g. Windows and Linux). There will always be differences in how different compilers process your code. Sometimes these differences are dictated by the underlying operating system or hardware. Try to compile your program (or at least the non-UI units) with two or three compilers. For example, if your code compiles and runs correctly with Delphi and on at least two of FPC's platforms, you'll be that much closer not only to a stable cross-platform application but also to having a program that you know will work.



### How to avoid disappointing your users

  * Use [unit tests](<fpcunit.md> "fpcunit") to make sure that functionality in your code does what it should. When users report bugs, add them to the test set. This way you can check for regressions in your next release: tests that previously succeeded should not fail. See [Databases#Running_FPC_database_tests](<Databases.md> "Databases") for an example that is used in FPC: the sqldb database test suite.
  * Have your program tested by somebody else. You'd be surprised what bugs people can find even in simple "monkey tests" (just hit the keyboard, move and click the mouse more or less at random).
  * Please keep [Localization](<Localization.md> "Localization")/internationalization in mind when writing your program. Even if you do not intend to ship to international customers, you may in future. Use fixed formats for import and export files, preferably ISO standard (e.g. ISO 8601 date formats). See the mess that is CSV import/export in Microsoft Excel for a way of not doing this.



### See also

  * [The Power of 10](<The_Power_of_10.md> "The Power of 10")

---

_Source: [https://wiki.freepascal.org/The_Power_of_Proper_Planning_and_Practices](https://web.archive.org/web/20240121090513/https://wiki.freepascal.org/The_Power_of_Proper_Planning_and_Practices)_
