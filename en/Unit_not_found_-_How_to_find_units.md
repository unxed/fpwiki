# Unit not found - How to find units

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Quick list
  * 3 Notations, terms
  * 4 Step 1: Compiler or IDE or both
    * 4.1 Compiler
    * 4.2 IDE
    * 4.3 Search strategies
      * 4.3.1 What both search
      * 4.3.2 Differences between IDE and FPC unit search
  * 5 Case 1: Both can not find a unit
  * 6 Case 2: Compiler finds a unit, but IDE does not
  * 7 Case 3: IDE finds a unit, but compiler does not
    * 7.1 Case 3.1: The compiler finds the .ppu file, but does not like it
    * 7.2 Case 3.2: The compiler cannot find source (.pas, .pp) or .ppu file
    * 7.3 Case 3.3: The wrong compiler is being used
    * 7.4 Case 3.4: _make_ , _apt_ or _fpcUPdeluxe_ installed a 2nd compiler _fpc_ into _$PATH_



## Overview

This page is about the most dreaded, prominent _error_ of Pascal: **unit not found** or **can't find unit**. 

This _error_ has a lot of potential reasons, so this page attempts to gather most of those, plus how to identify which particular problem it is and how to hopefully solve it. Usually, under this condition, Lazarus _does not compile any program_ at all nor does it recompile the IDE any longer, meaning Lazarus is completely "broken" until the fix is applied. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Lazarus provides a tool to test for many common misconfigurations: The button **Project Options / Test**. Click these and see the warnings it produces. Normally it will only warn (safely ignore those) about certain duplicate ppu files in the fpc sources (the actual file dupes depend on the _fpc_ version and distribution).

## Quick list

For experts here is a list of possible causes for the error. This list is not sorted for probability. 

  * FPC installation 
    * You named one of your units like one of the FPC units.
    * Wrong fpc.cfg. $HOME/.fpc.cfg instead of /etc/fpc.cfg. Find out with -vut.
    * Missing or wrong search path in fpc.cfg. Find out with -vut. 
      * Added a search path to fpc.cfg that contains sources (.pas,.pp)
    * Cross compiling: Forgot to install the cross compiled FPC ppus. Find out with -vut.
    * Broken FPC installation. Remove FPC directory manually and install again.
    * Compiler symlink points to wrong compiler. 
      * subcase: **2 compilers** in searchpath, see: [install on Linux cheatsheet](<install_on_Linux_cheatsheet.md> "install on Linux cheatsheet")
    * FPCDIR environment variable is set
    * You named one of your include files like one of the FPC include files and your FPC installation was not compiled with -Ur.
    * Specific [problem](<https://stackoverflow.com/a/64637024/1020550>) of Ubuntu installations
  * Lazarus 
    * Forgot to add a package dependency. Check project inspector or package editor.
    * A ppu exists twice. Find out with -vut. Remove the wrong ppu. Check if a package usage unit paths is only $(PkgOutDir) and the usage include path is empty. Check if two packages or a package and the project contain the same unit search path in their compiler options.
    * Typo or missing search paths. Check _Project / Project Options / Compiler Options_ , pages _Paths_ and _Inherited_ and check _source editor / popup menu / file settings / unit info / unit paths_.
    * Wrong case. If the unit filename uses upper and lower case letters all uses sections must use the correct case. Before 0.9.29 the IDE did not search 'in' filenames in lower and upper case. Check that the 'in' filenames in uses sections.
    * Wrong unit: If there are two units with the same name in the search path the compiler may use the wrong one without warning. The IDE normally warns unless disabled in _Tools / Options / Environment / Naming / ambiguous file action_.
    * Forgot to install the FPC sources and/or forgot to set the path in _Tools / Options / Environment / Files / FPC source directory_.
    * Using the wrong version of FPC sources
    * Using the wrong compiler. Check _Tools / Options / Environment / Files / compiler path_. Also click "**project options / Test** " to diagnose for this condition
    * Using the wrong lazarus directory. Check _Tools / Options / Environment / Files / Lazarus directory_.
    * Added a search path to your own units to fpc.cfg instead of creating a package.
    * Cross compiling: Forgot to cross compile lcl, synedit for the new platform. These are not compiled automatically (They are since 0.9.31).
    * Wrong package: you have a package (.lpk) twice on your disk and the IDE is still using the old



## Notations, terms

  * **.pas** file: This is a place holder for any pascal unit source. For example unit1.pas or unit1.pp. Both extensions are allowed and there is no difference. There are other compilers, which ignore .pp files, but this page is about the Free Pascal compiler.
  * **.pp** file: Same as .pas. Just another extension for the same thing.
  * **.p** file: Same as .pas. Just another extension for the same thing. Commonly utilized on Macintosh.
  * **.ppu** file: This is place holder for any compiled pascal unit, created by the Free Pascal compiler.



## Step 1: Compiler or IDE or both

The IDE and the compiler do not use the exact same way to locate units. 

### Compiler

If the compiler can not find it, the compilation fails (Build, Ctrl+F9) with the error 'unit not found'. 

### IDE

If the IDE can not find, the codetools will fail. For example _Find Declaration_ (Alt+Up or Ctrl+Left mouse button) or _Identifier completion_ (Ctrl+Space) will fail with the error 'unit not found'. If the error message is about units you don't use directly, then simply add the unit name to a uses section and do a find declaration. After that you can remove it. 

### Search strategies

#### What both search

  * Both search units in the search path.
  * Both search first for pascal sources (.pas, .pp) files. Under MacPAS mode: search for .p files too.
  * Both search for several cases: normal case, lower case, upper case. For example: if the uses section contains 'Unit1', then it searches:


    
    
     1. Unit1.pp
     2. unit1.pp
     3. UNIT1.PP
     4. Unit1.pas
     5. unit1.pas
     6. UNIT1.PAS
    

They are searched in every directory of the search path. 

#### Differences between IDE and FPC unit search

  * FPC uses the fpc.cfg for additional search paths. The IDE uses them only to find files of the FPC sources. So adding a search path to the fpc.cfg is ignored by Lazarus.
  * FPC always starts in one directory and every relative filename is relative to this directory. For the Lazarus IDE each project and each package has its own base directory. So the search path of a package is relative to the package directory. The package directory is the directory of the .lpk file. The base directory of a project is the projects main source file (e.g. the .lpr file). If there is no main source file, the directory of the .lpi file is taken. Normally both are the same and the IDE has no graphical frontend to split. But by editing the .lpi file manually you can split them. At the moment there is only one prominent example for this case: the ppxxx.lpi files of the Free Pascal compiler.
  * FPC and Lazarus have different macros.
  * You can define additional source search paths for the IDE, which are not given to the compiler. (Compiler options -> Other source files)



## Case 1: Both can not find a unit

  * **Check if the unit exists** : If not, then see chapter Searching packages, projects of a unit (ToDo: write me).
  * **Check if the unit is readable** : (You can open it in the IDE). A unit source does not need to be writable. If not, then fix the permissions or get a readable copy.
  * **Check if the unit has a valid pascal name** : (identifier plus extension). Valid examples: unit1.pas, Bogus2.pp, _1.pas, LAZ.PAS. Wrong examples: 1.pas (starts with number), a%.pas (contains special character), unit.PAS (extension uppercase, but filename lowercase).
  * **Check if directory name contains special characters**


    
    
      1. slashes / and backslashes \ are used as directory delimiter and should never be used for directory names
      2. Semicolons are used to separate paths in the IDE.
      3. dollar $ is used for macros in many scripting languages  
    

  * **Check for right case** : If your unit name is all lowercase or all uppercase you don't need to worry. If not, then you must make sure, that **every** uses section in every source code uses exactly this case. For example: if your unit is named Unit1.pas, then every uses section must use Unit1. This is not so important under windows, but under any other platform (Linux, MacOSX). The IDE gives warnings when saving files with mixed case.
  * **Check if the unit is in the search path**. If the compilation stopped while compiling a package, look in the compiler options of that package (open the package, there is a button 'compiler options'). If the compilation stopped while compiling the project, look in the project / compiler options.



## Case 2: Compiler finds a unit, but IDE does not

The next sub cases assumes, that Run -> Build (Ctrl+F9) in the Lazarus IDE works, but Find Declaration fails with the error: unit not found. If unsure, check Case 1 as well. 

  * **Check if the unit source exists** : The IDE searches for pascal source (.pas, .pp) and ignores the .ppu files. The compiler can use a .ppu, but the Lazarus IDE not. This feature is planned, but there is no time frame.
  * **Check if unit directory was added to your fpc.cfg** : The compiler uses all paths defined in the fpc.cfg file. The IDE does not read this file. Add the search path to the compiler options of your projects and/or packages.
  * **Check for macros and special compiler flags** : The compiler understands the asterisk * character in search paths. The IDE treats this character as normal character. You can add the -Fu, -FU, -FE flags manually to the custom compiler flags in the lazarus IDE, but they are not parsed. Always use the right fields for options. The **custom options** are only for flags like the -D flag or uncommon compiler options, like new features of the unstable developer compiler.
  * **Rescan the FPC source directory** : sometimes after a new/fresh install the IDE has problems finding the FPC sources. Selecting this option from the main menu (Environment/Rescan the FPC directory) might fix the problem.



## Case 3: IDE finds a unit, but compiler does not

This is the case, when Find Declaration finds the unit, but building (Ctrl+F9) fails. 

There are two cases: Either the compiler didn't find anything, or it does not like, what it found. In both cases it returns the same error: unit xyz not found. 

You can see where the compiler sarches and what it finds by adding the -vu -vt options (short: -vut). **Beware** : This gives easily thousands of lines of output. The -vut must be applied to the step, that creates the error. If it happens when compiling the project, enable the -vt in the compiler options of the project. If it happens when building a package, use the compiler options of the package. If it happens when building the IDE, use the Tools / Configure Build Lazarus / Advanced Build Options / Options. Then compile again. It will slow down the compilation. Then right click on on messages window and click on 'Copy all and hidden messages to clipboard'. Then paste it i the source editor. Now search for all places where the unit name is mentioned. 

### Case 3.1: The compiler finds the .ppu file, but does not like it

This can happen, when the found .ppu file is older than a required unit (.pas, .pp or .ppu). For example if unit1 uses unit2 and unit2 was changed, then unit1 needs to be recompiled as well. 

It can also happen, if you installed newer files without deleting the old ones. The compiler stops at the first found. 

You can see, what the compiler finds and what is missing by giving the -vt command line switch. But this gives a lot of output and it is hard to read for newbies. 

The easiest way is often to first recompile the packages and project clean (Build all). In case of the Lazarus sources itself: The configure build lazarus dialog has a button to setup all options for a clean build. 

### Case 3.2: The compiler cannot find source (.pas, .pp) or .ppu file

In this case you have to extend the search path. The question is: add the .ppu directory or the source directory? 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** If you add the wrong search path, it can happen, that the compiler will create multiple copies of files and you will later get the 'unit not found' error of Case 3.1.

Many projects and packages have distinct source directories (.pas, .pp) and output directories (.ppu). If a package has an output directory, then only this directory should be added to the search path of any depending project and/or package. The IDE will do this automatically. For example: Adding the LCL as dependency to your project adds two output directories to your unit search path. You can see this in Compiler Options -> Inherited. Never add any other LCL directory to your search paths or you will get Case 3.1 errors. 

  * **Check if the unit is in the compiler search path** : The search path given to the compiler are the -Fu options of the command line parameters. You can see the options given to the compiler in Compiler Options -> Show Options.



### Case 3.3: The wrong compiler is being used

Perhaps when cross compiling the correct cross .ppu is found, but the wrong compiler is used by lazarus (see [install on Linux cheatsheet](<install_on_Linux_cheatsheet.md> "install on Linux cheatsheet")). 

Lazarus uses the compiler specificied in the environment options. It is easy to forget to change the compiler, if you change the target CPU. Therefore it is recommended to set the compiler in the environment options to the fpc executable, which is a front end to the ppcXXX compiler backends. The fpc executable chooses the right ppcXXX compiler based on the -P parameter or the native pccXXX compiler if no such parameter is given. The drawback of using fpc instead of for example ppc386 directly, is that an extra process needs to be started (one for fpc and one for ppcXXX) which slows down compilation slightly, but this is hardly measurable. 

Run lazarus from a text console (with startlazarus) so you an see the debugging output and look at the output generated when you compile. 

It may look like this: 
    
    
    TPkgManager.GetWritablePkgOutputDirectory APackage=DBFLaz 0.1.1 AnOutDirectory="/home/sam/.lazarus/lib/DBFLaz/powerpc-linux"
    TLazPackageGraph.CheckIfPackageNeedsCompilation  No state file for JPEGForLazarus 1.0.1
    [TExternalToolList.Run] CmdLine="/usr/bin/ppc386  -S2cgi -OG1 -Ppowerpc -gl -vewnhi -l -Fu../../lcl/units/powerpc-linux/ -Fu../../lcl/units/powerpc-linux/gtk2/ -Fu../../packager/units/powerpc-linux/ -Fu. -FU/home/sam/.lazarus/lib/JPEGForLazarus/powerpc-linux/ -dLCL -dLCLgtk2 jpegforlazarus.pas"   
    WorkDir="/usr/share/lazarus/components/jpeg/"
    Hint: Start of reading config file /etc/fpc.cfg
    Hint: End of reading config file /etc/fpc.cfg
    Warning: You are using the obsolete switch -OG
    Free Pascal Compiler version 2.3.1 [2007/12/10] for i386
    Copyright (c) 1993-2007 by Florian Klaempfl
    Target OS: Linux for i386
    Compiling jpegforlazarus.pas
    Compiling lazjpeg.pas
    PPU Loading /usr/share/lazarus/lcl/units/powerpc-linux/intfgraphics.ppu
    PPU is compiled for another processor
    Fatal: Can't find unit IntfGraphics used by LazJPEG
    GetMsgLineFilename Filename "lazjpeg.pas" not found.
    TExternalToolList.Run Exception: /usr/share/lazarus/components/jpeg/lazjpeg.pas(1,1) Fatal: Can't find unit IntfGraphics used by LazJPEG 
    

The clues are these lines: 
    
    
    [TExternalToolList.Run] CmdLine="/usr/bin/ppc386  -S2cgi -OG1 -Ppowerpc -gl -vewnhi -l -Fu../../lcl/units/powerpc-linux/ -Fu../../lcl/units/powerpc-linux/gtk2/ -Fu../../packager/units/powerpc-linux/ -Fu. -FU/home/sam/.lazarus/lib/JPEGForLazarus/powerpc-linux/ -dLCL -dLCLgtk2 jpegforlazarus.pas"   
    ...
    Target OS: Linux for i386
    ...
    PPU Loading /usr/share/lazarus/lcl/units/powerpc-linux/intfgraphics.ppu
    PPU is compiled for another processor
    

Which suggest in this case that ppc386 is set in the _Environment_ menu instead of fpc 

  


### Case 3.4: _make_ , _apt_ or _fpcUPdeluxe_ installed a 2nd compiler _fpc_ into _$PATH_

see [install on Linux cheatsheet](<install_on_Linux_cheatsheet.md> "install on Linux cheatsheet") how to move the 2nd **fpc** out of the **$PATH** . Click "**project options / Test** " for diagnosis of this condition.

---

_Source: [https://wiki.freepascal.org/Unit_not_found_-_How_to_find_units](https://web.archive.org/web/20240920204108/https://wiki.freepascal.org/Unit_not_found_-_How_to_find_units)_
