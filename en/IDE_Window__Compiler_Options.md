# IDE Window: Compiler Options

│ **[Deutsch (de)](</IDE_Window:_Compiler_Options/de> "IDE Window: Compiler Options/de")** │  **English (en)** │  **[español (es)](</IDE_Window:_Compiler_Options/es> "IDE Window: Compiler Options/es")** │  **[français (fr)](</IDE_Window:_Compiler_Options/fr> "IDE Window: Compiler Options/fr")** │  **[日本語 (ja)](</IDE_Window:_Compiler_Options/ja> "IDE Window: Compiler Options/ja")** │  **[русский (ru)](<../ru/IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options/ru")** │    
****  
****

## Contents

  * 1 Navigation
  * 2 Paths
    * 2.1 Other Unit Files
    * 2.2 Include Files
    * 2.3 Other sources
    * 2.4 Libraries
    * 2.5 Unit output directory
    * 2.6 Target file name
      * 2.6.1 Apply Conventions
    * 2.7 Examples
    * 2.8 Debugger path addition
    * 2.9 LCL widget type (pre Lazarus 1.0)
  * 3 Config and Target
  * 4 Build modes
    * 4.1 Adding, deleting, activating build modes
    * 4.2 Adding a release and debug build modes
    * 4.3 Selecting the active build mode
    * 4.4 Project macros
  * 5 Parsing
    * 5.1 Syntax mode
    * 5.2 Syntax Options
    * 5.3 Assembler style
  * 6 Code
  * 7 Compilation and Linking
  * 8 Debugging
  * 9 Verbosity
  * 10 Messages
  * 11 Custom Options
  * 12 Additions and Overrides
    * 12.1 Overview
    * 12.2 Types of build options
    * 12.3 Enabling build options in build modes
    * 12.4 Storage location of build options
    * 12.5 Targets of build options
    * 12.6 Colors in the matrix
    * 12.7 Examples for additions and overrides
      * 12.7.1 Changing the LCLWidgetType in Version 1.1 and above
      * 12.7.2 Add a flag to project and all packages
      * 12.7.3 Add a flag to all projects and packages
      * 12.7.4 Change the output directory of project and all packages
      * 12.7.5 Add a flag to one package without altering the lpk itself
  * 13 Compilation
  * 14 Compiler Commands
    * 14.1 Compiler
    * 14.2 Execute after
  * 15 Inherited
  * 16 Use these settings as default for new projects
  * 17 Buttons
    * 17.1 Test
    * 17.2 Show Options
    * 17.3 Load/Save
    * 17.4 Ok
    * 17.5 Cancel
  * 18 IDE Macros
    * 18.1 For 0.9.29 to 1.0.x
    * 18.2 For 1.1 and above
  * 19 Other



## Navigation

[Main Menu](<Main_menu.md> "Main menu") > [Project](<Main_menu.md> "Main menu") > [Project Options ...](<IDE_Window__Project_Options.md> "IDE Window: Project Options") > Compiler Options 

## Paths

[![Compiler Options - Paths](https://wiki.freepascal.org/images/0/05/CompilerOptions-Paths2.png)](</File:CompilerOptions-Paths2.png> "Compiler Options - Paths")

Here are the general rules about search paths: 

  * Relative paths are expanded with the project or package directory (where the .lpi/.lpk file is located).
  * These paths are added to the search paths. They do not replace existing paths.
  * The IDE has one set of search paths for every package/project. That means each package can have different search paths than the active project.  
"set of search paths" refers to unit search path, include search path, sources search path, ... 
    * Every directory in the unit search path of the project gets all the project search paths.
    * Every directory in the unit search path of the package gets all the package search paths.
    * Other directories get the project search paths. If the project search path contains the '.' the directory will see the project directory too.
  * Using "uses unitname in 'filename'" does not affect any search path.
  * If a package or project uses a package, it will inherit the _usage_ search paths. You can see the inherited search paths in the Inherited page. 
    * If you do not want to use a search path inherited from a used package you must change the compiler options of the used package.
  * Using the Lazarus package system, you hardly ever need to set search paths manually.
  * The Free Pascal Compiler has a configuration file of its own (default /etc/fpc.cfg) which defines a set of search paths to the FPC ppu files. For example to find the FPC units of the RTL or the FCL like 'classes', 'sysutils'. Do not add search paths to source files (.pas, .inc) in there.
  * Search paths are separated by a semicolon ';'.
  * Leading and trailing spaces are ignored and automatically removed by the IDE. The IDE normalizes search paths and appends the path delimiter (Windows: \, all other: /). Search paths are automatically converted to the current operating system when opening an .lpi or .lpk file.
  * You can use macros. For example $NameOnly($(ProjFile))-$(TargetCPU). See [IDE Macros in paths and filenames](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames").



**Wildcards**

  * The last part of a directory can be a wildcard * or **, so e.g. _/path/*_ , but not _/path/*/foo_ , nor _/path/foo*_.
  * You can use a single star * at the end to search in all direct sub directories. For example _C:\project\\*_ will find _unit1.pas_ in _C:\project\foo\unit1.pas_ or _C:\project\bar\unit1.pas_ , but not _C:\project\unit1.pas_ nor _C:\project\foo\bar\unit1.pas_. (Since Lazarus 3.99.)
  * You can use a double star ** at the end to search in the directory and all sub directories. For example _C:\project\\**_ will find _unit1.pas_ in _C:\project\unit1.pas_ , _C:\project\foo\unit1.pas_ , and _C:\project\foo\bar\unit1.pas_. (Since Lazarus 3.99.)
  * You can add any number of wildcard search paths, e.g. 'src/*;tests/**;extra'.
  * Excludes: can be defined in _Tools / Environment / File Filters / Excludes for * and **_. Default is '.*;backup'. Excludes are case insensitive.



### Other Unit Files

This is the search path for the Pascal units (.ppu, .pp, .pas, .p) of the project or package. See the title of the window to know which. This path is given to the Free Pascal Compiler which adds it to its Unit Path. 

  * Adding and removing units to the project/package will automatically ask you to adjust the unit path.
  * This search path contains the directories of your project (or your package) that contain the .pas, .pp or .p files.
  * If you want to share some units between your projects, create a package for them. It's easy.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Do not add directories of used Lazarus packages to this path. Otherwise FPC will rebuild the .ppu files of the package and put them into your project directory. You will end up with multiple .ppu versions and you will get strange 'Can not find unit' errors.   
For example: Do not add any FPC or LCL source directory to this search path.

### Include Files

This is the search path for the include files (eg .inc, .lrs files). This path is given to the Free Pascal Compiler, which adds it to its Include Path, which is used by include directives like {$I filename.inc} or {$INCLUDE filename.inc}. 

### Other sources

This is a search path for Pascal unit sources which is only used by the Lazarus IDE (not by the compiler). Normally you will leave this empty. It is only useful when you build the ppu files **without** Lazarus. 

Example: You have a directory with sources and another directory with the corresponding .ppu files and you can't or don't want to create a Lazarus package. You add the .ppu directory to the 'Other Unit Files' path and the .pas directory to the 'Other sources' path. This way the compiler will use the .ppu files and not try to rebuild them every time. Also, the IDE will find the sources and Find Declaration works. 

### Libraries

This is the search path for libraries (.dll or .so or .a files). These are only used in the compiling/linking phase; e.g. when running your application under the debugger, you need to make sure required libraries are present in the expected place (e.g. - depending on platform - executable directory, PATH, .so/.dylib search path). 

### Unit output directory

This is the directory where the compiler will put all output, like .ppu, .o, .rst files (it passes this to compiler switch **-FU**). If you are using the $R directive for the lfm files, it will also copy the lfm files there. 

A popular usage example is an output directory named _units_ , and then an extra sub-directory for the CPU and OS target. For example: 
    
    
     units/$(TargetCPU)-$(TargetOS)
    

Notes: 

  * If the unit output directory is empty, Lazarus will **not** pass the **-FU** switch to the compiler. The compiler will then use the **-FE** switch. See [Project target file](<IDE_Window__Project_Options.md> "IDE Window: Project Options")
  * Packages normally inherit their output directory to other packages via the 'usage' options. You do not need to add package paths manually to your project.
  * If the output directory of a package is empty, the macro $(PkgOutDir) is too and will not be inherited to descending packages and projects. Use a dot '.' to define the current directory.



### Target file name

Note: Only projects have this. This option is not available for packages. 

Set here the filename of the generated executable. If the file is relative it will be expanded with the project directory (where the .lpi file is). If no file is given the executable is put into the unit output directory and has the name of the main source file name (usually the .lpr file) without the extension. If no extension is given, the default extension for the platform is added (eg _.exe_ for MS Windows, none for others). When a new project has not been saved yet and is built, the IDE saves the files to the tmp directory. The relative files are then relative to this directory. 

Lazarus passes the compiler switch: 

  * **-o** to define the target file name.
  * **-FE** if the target file name is not in the project directory (where the .lpi file is located)
  * **-FU** if the unit output directory is not empty



If the target file name is in another directory (not the directory of the .lpi file), Lazarus will pass the **-FE** switch to the compiler to make sure that the secondary files, like _.o_ and _.rst_ are put into the same directory. If you cleared the _unit output directory_ then the IDE will not pass the **-FU** switch and the compiler will generate the _.ppu/.o_ files of the units in the target directory too. 

If you cleared the _unit output directory_ **and** your project target file is in the project directory, then neither **-FU** nor **-FE** is passed to the compiler, and the compiler will work in Delphi-compatible mode and generate the _.ppu/.o_ file of each unit in the same directory as the unit. 

#### Apply Conventions

Enable this to apply various naming conventions depending on the target platform. 

  * Windows: If it is a program, it appends the '.exe' extension; if it is a library, it appends '.dll'.
  * Unix (eg Linux, FreeBSD, Darwin/macOS): If it is a library the name is lowercased and if the name does not start with 'lib' it prepends 'lib'.
  * Linux, FreeBSD: If it is a library it appends '.so'.
  * Darwin/macOS: If it is a library it appends '.dylib'.



### Examples

Unit output directory | Target file name | Generated options | Note   
---|---|---|---  
lib/$(TargetCPU)-$(TargetOS) | empty | -FElib\x86_64-win64 -olib\x86_64-win64\project1.exe | all output files are put into _lib\x86_64-win64\_  
lib | foo | -FUlib -FE. -ofoo.exe | all unit output files like ppu and o are put into _lib\_ , program output files are put into the base directory (where the lpi is)   
lib | foo/bar | -FUlib -FEfoo -ofoo\bar.exe | all unit output files like ppu and o are put into _lib\_ , program output files are put into _foo_  
empty | empty | -oproject1.exe | ppu files are put to the _.pas_ files, e.g. a _src\unit1.pas_ creates a _src\unit1.ppu_  
empty | foo | -ofoo.exe | ppu files are put to the _.pas_ files   
empty | . (single dot and Apply conventions disabled) |  | ppu files are put to the .pas files, output file is whatever the compiler's default is   
  
### Debugger path addition

These directories are added to the search path of the IDE debugger, when it searches for sources (units and include files). 

### LCL widget type (pre Lazarus 1.0)

In older Lazarus releases: this is the used LCL widget set. Normally the default widget set is used. If you want to try another or you are cross compiling, select another widget set here. 

  * The default widget set of a package is the widget set of the current project.
  * The default widget set of the current project depends on the current operating system. For example: win32 for windows 2000.
  * You should not set the widget set for a package, because then the project cannot override it. Only set it, if the package is part of a set of packages - one for each widget set.


  * In Lazarus 1.0, the "Select another LCL widget set (macro LCLWidgetType)" redirects you to the [Build Modes](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options") page, where you can add the macro **LCLWidgetType**.
  * In Lazarus 1.1, the "Select another LCL widget set (macro LCLWidgetType)" redirects you to the [Additions and Overrides](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options") page, where you can add an **IDE macro** **LCLWidgetType**.



## Config and Target

[![CompilerOptionsConfigAndTarget2023.png](https://wiki.freepascal.org/images/4/4e/CompilerOptionsConfigAndTarget2023.png)](</File:CompilerOptionsConfigAndTarget2023.png>)

  * **Write config instead of command line parameters (@)** : Instead of passing all project/package compiler options as command line parameters, Lazarus can write a config file. You can specify a filename or use the default. All parameters except @, -n, filename and the automatic -B are written to the config and a @filename.cfg is passed to the compiler. This option exists since 3.99.



## Build modes

### Adding, deleting, activating build modes

Only projects have build modes. A package does not have build modes. Note: A project can apply build modes to the packages it depends on using 'Additions and Overrides' - see below. 

Build modes allow to define **sets of compiler options** and to quickly switch between these sets. For example you can define a _mode_ for debugging which compiles your project with range checking, while your default mode does not. 

**Note:** If you want to pass some options depending on the platform, for example passing some extra linker options under OS X, please take a look at Build Macros. 

How to reach this dialog: Project / Project Options / Compiler Options. Make sure that "Build Modes" is enabled. 

[![EditProjectBuildModes.png](https://wiki.freepascal.org/images/2/2c/EditProjectBuildModes.png)](</File:EditProjectBuildModes.png>)

Click on the "..." button right to add/remove/rename build modes: 

[![ListOfBuildModes1.png](https://wiki.freepascal.org/images/7/7b/ListOfBuildModes1.png)](</File:ListOfBuildModes1.png>)

The grid contains the list of build modes with three columns. 

The first column shows which mode is currently active, it is checked. When you activate another mode, all compiler options pages will load the settings of the new mode, including the macro values on the build modes page. There is always only one mode active and you can only edit the properties of one mode at a time. Which mode is active is stored in the session file (lps). The default mode is the first mode. 

If your project stores the session in a separate lps file (see Project options / Session / Save session info in), you can store extra modes in the lps session file, so that each developer can have her own set of modes. In this case the second column shows where the mode is stored, in the lpi or the lps (in session). Keep in mind that the first mode is the default mode for the project, so it must be stored in the project, not in the session file. 

The last column is the name of the mode. It is an arbitrary string, so you can give it a short name or a whole sentence. 

  * The **plus** button adds a new mode, by duplicating the currently active one and activates it.
  * The **minus** button delete the currently selected mode. There must be at least one mode. If you delete the first mode, which is the default mode, the second mode automatically becomes the default mode.
  * The **up** , **down** buttons allows to reorder the modes.
  * The **rightmost** button shows the differences between build modes.



**Hint:** : Once you added another build mode there will be a new button in the IDE main bar to quickly switch the currently active mode. 

**Note:** : When opening a new project with an old IDE (<0.9.31), you will only see the default mode. If you save the project with the old IDE you will loose all other modes, all macros and conditionals. 

_Build modes_ exist since 0.9.31. 

### Adding a release and debug build modes

The most common task for projects will be adding a simple release and a simple debug build modes. Remember to always use the debug build mode, because debugging will not work in the release build mode, and then only use the release build mode in the final release of the application. 

  1. Go to Project / Project Options / Compiler Options
  2. Enable "Build Modes" at the top. A selector for build modes and an edit button will appear.
  3. Click on the button labelled "..." to open the build modes dialog.
  4. Click on Create Debug and Release modes. This adds the two build modes "Debug" and "Release".
  5. Close the dialog.



[![BuildModesAddedDebugAndRelease1.png](https://wiki.freepascal.org/images/d/d8/BuildModesAddedDebugAndRelease1.png)](</File:BuildModesAddedDebugAndRelease1.png>)

Every Build Mode is a complete set of compiler options. You can switch easily between your build modes either via the combobox at the top of the project options, or in the IDE main toolbar, with the arrow button right of the _Compile_ button. 

[![BuildModeSelectorInIDEToolBar1.png](https://wiki.freepascal.org/images/1/16/BuildModeSelectorInIDEToolBar1.png)](</File:BuildModeSelectorInIDEToolBar1.png>)

Once we select the _Debug_ build mode, all configurations from the project options dialog will be specific to this build mode. Here is the default _Debugging_ page in _Debug_ mode: 

[![BuildModeDebugPageDebugging.png](https://wiki.freepascal.org/images/f/ff/BuildModeDebugPageDebugging.png)](</File:BuildModeDebugPageDebugging.png>)

When we select the _Release_ build mode, all configurations from the project options dialog will be specific to this build mode. Here is the default _Debugging_ page in _Release_ mode: 

[![BuildModeReleasePageDebugging.png](https://wiki.freepascal.org/images/2/2d/BuildModeReleasePageDebugging.png)](</File:BuildModeReleasePageDebugging.png>)

### Selecting the active build mode

One can select the currently active build mode either in the "Project Options" dialog or in the main IDE Window, in a special button with a drop down which will appear only if the project has more then 1 build mode, as one can see in this screenshot: 

[![Selecting Build Mode Main IDE Windows.png](https://wiki.freepascal.org/images/6/6f/Selecting_Build_Mode_Main_IDE_Windows.png)](</File:Selecting_Build_Mode_Main_IDE_Windows.png>)

### Project macros

See [here](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options") for adding your own macros or defining the LCLWidgetType macro. 

## Parsing

[![Compiler Options - Parsing](https://wiki.freepascal.org/images/4/47/CompilerOptions-Parsing.png)](</File:CompilerOptions-Parsing.png> "Compiler Options - Parsing")

### Syntax mode

Choose the default [Compiler Mode](<Compiler_Mode.md> "Compiler Mode"). If a unit does not contain a {$mode somemode} directive, this is used as the default. 

### Syntax Options

See [FPC Programmer's guide](<http://www.freepascal.org/docs-html/prog/prog.html>). 

  * C Style Operators (*=, +=, /= and -=)
  * Include Assertion Code
  * Allow LABEL and GOTO
  * C++ Styled [INLINE](<Inline.md> "Inline")
  * C Style Macros (global)
  * Constructor name must be **init** (destructor must be **done**)
  * Static Keyword in Objects
  * Use [AnsiStrings](<String.md> "String")



### Assembler style

Sets the value of -R<x> option: 

  * -Rdefault: use default assembler
  * -Ratt: use AT&T style assembler
  * -Rintel: use Intel style assembler



## Code

## Compilation and Linking

[![CompilerOptions-Compilation and Linking.png](https://wiki.freepascal.org/images/e/e9/CompilerOptions-Compilation_and_Linking.png)](</File:CompilerOptions-Compilation_and_Linking.png>)

For debug settings, see [Debugger_Setup#Project_Options](<Debugger_Setup.md> "Debugger Setup")

## Debugging

[![Compiler Options - Debugging](https://wiki.freepascal.org/images/3/3f/CompilerOptions-Debugging.png)](</File:CompilerOptions-Debugging.png> "Compiler Options - Debugging")

## Verbosity

[![Compiler Options - Verbosity](https://wiki.freepascal.org/images/8/83/CompilerOptions-Verbosity.png)](</File:CompilerOptions-Verbosity.png> "Compiler Options - Verbosity")

See [Free Pascal - Online documentation](<http://www.freepascal.org/docs.html>). Note that adding a lot of verbosity slows down the parsing of the compiler output much, even if most of the messages will be hidden in the messages view. 

## Messages

[![Compiler Options - Messages](https://wiki.freepascal.org/images/7/74/CompilerOptions-Messages.png)](</File:CompilerOptions-Messages.png> "Compiler Options - Messages")

(Introduced in Lazarus 0.9.27 version) The page allows to control what compiler output Notes, Hints and Warnings are shown. The feature requires FP compiler to support -m switch (version 2.2.2 or higher). 

It's also possible to specify message Language file (file should be Utf-8 encoded), to see compiler messages translated, without need to modify fpc.cfg file. 

Samples of the messages translation file can be found at ${LazarusDir}/fpc/${FPCTARGET}/msg, i.e. C:\Lazarus\fpc\2.2.3\msg 

## Custom Options

[![CompilerOptions-Custom Options.png](https://wiki.freepascal.org/images/2/25/CompilerOptions-Custom_Options.png)](</File:CompilerOptions-Custom_Options.png>)

You'll typically define some compiler options here. For example you can define in a build mode something like: 
    
    
     -dRELEASE
    

Then in the build mode, only the code surrounded by the {$IFDEF RELEASE} {$ENDIF} will be compiled. This can be used as an alternative to the Macro system, particularly if you come from Delphi. 

Spaces at start and end are removed. Line breaks are replaced by a space before passing to the compiler. A leading space is added automatically. 

The IDE substitutes IDE macros in custom options and parses the options. Flags like -dRelease are passed to codetools, so the source editor knows them immediately. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Do not add the path options -Fu, -Fi, -Fl, -FU, -o. Use the fields on the page [Paths](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options") instead. The IDE ignores the paths in the custom options. It does not update them when you add or delete files or when you open the project on another host.

## Additions and Overrides

The page "Additions and Overrides" exists since Lazarus v1.1. 

### Overview

The page contains a matrix of build options. 

[![CompilerAdditionsAndOverrides1.png](https://wiki.freepascal.org/images/c/cd/CompilerAdditionsAndOverrides1.png)](</File:CompilerAdditionsAndOverrides1.png>)

The settings "custom options", "output directory", and "IDE Macros" within any target can be switched on and off separately for each build mode. 

Thus, for each target, there is a Matrix of enabling check boxes. 

The vertical words in the header show all the Build Modes that are currently defined, and by this they denote the column in the matrices. 

At the right side of each row of any matrix, the description of the type (such as "Custom", "OutDir", or "IDE Macro" ) and the value of the option to be enabled, is given. 

### Types of build options

A build option can 

  * set an **IDE macro**. The value must be of the form **MacroName:=Value**. For instance **LCLWidgetType:=qt**. You can use IDE macros in the value, but not in the name.
  * append some custom compiler options via **Custom** (e.g. append -O3). You do not need a leading space. That is added automatically. See the notes about [Custom options](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options").
  * override the output directory (-FU) via **OutDir**. Note that when you use a relative directory like _lib/$(TargetOS)_ the working directory of the target is added, not the project directory. For example when the option overrides the output directory of the package _SynEdit_ , then the output directory of synedit is changed to _< lazarusdir>/components/synedit/lib/$(TargetOS)_.



You can create a new option by clicking on the **Add** button above the matrix. You can change the type of an option at any time. 

### Enabling build options in build modes

An option can be enabled with the build modes of the project. You can enable the option only in one build mode or several of them. Each build option has a row in the matrix, each build mode has a column in the matrix. Each combination has a checkbox. Build options are applied from top to bottom. 

Note that enabling options in build modes of the session (stored in the .lps) does **not** alter the _lpi_ file. This information is stored in the _lps_ file. That's why these checkboxes have a yellow background. 

The currently active build mode and options have a green background. 

When you rename a build mode the enabled states of the lpi and lps options are migrated too. The enabled states of IDE options are *not* migrated. For example when an IDE option is enabled for mode _debug_ and you rename the mode to _Test_ , then the IDE option is still enabled for mode _debug_ for other projects. 

### Storage location of build options

An option can be stored in 

  * the project (.lpi)
  * the project session (.lps)
  * or in the IDE configuration (environmentoptions.xml), then it is available to all projects



Build options are applied from top to bottom. That means first the options stored in the IDE, then the options stored in the .lpi and last the options stored in the session. 

You can move build options and whole targets via the Up and Down button above the matrix to other storage groups. 

**Note** : An option in the IDE configuration for mode _debug_ is applied to **all** projects with this mode. For instance when you open a third party project having the build mode _debug_ the option will be applied and there will be no warning. This might break the project. 

### Targets of build options

A build option can be applied to the project and/or to one or more packages. You can limit the scope of the build options to only apply to specific targets (project, packages). Build options are grouped by **Targets**. The **targets** are case insensitive and allow the asterisk "*" for any number of arbitrary characters and the question mark "?" for one arbitrary character. You can exclude targets by prepending a minus "-". To edit a target click behind the 'Targets:'. 

Examples of targets: 

  * ***** : The asterisk "*" means fits all. The option is applied to the project and all packages. This is the default.
  * **LCL,Lazutils** : Apply it only to package "LCL" and "LazUtils"
  * ***dsgn,-syneditdsgn** : Apply it to all packages ending with "dsgn", except "syneditdsgn".
  * **#project** : This fits the project itself. The option is applied only to the project, not to the used packages.
  * **#ide** : This fits the IDE. The option is applied only to the IDE, not to installed packages.



You can have any number of targets. You can create a new target via the the **Add** button above the matrix. You can move targets via the up and down buttons to other storages. All build options of the target group are moved as well. 

### Colors in the matrix

  * **Green** : currently active mode and options
  * **Yellow** : stored in session, not altering the lpi
  * **Red** : syntax error



### Examples for additions and overrides

  * Append compiler options to packages without touching the lpk. For example you can append range checking _-Cr_ via **custom** option under **Targets: ***.
  * Changing the output directory of packages without touching the lpk. For example add a **OutDir** under **Targets: ***.
  * Define IDE macros only for some packages or only for the project. For example compile the LCL with flag **-dQT_NATIVE_DIALOGS** , add a new target **Targets: LCL** , then add a custom option.
  * Append compiler options to all projects with build mode "debug" by adding options to the IDE storage group.
  * Change the package(s) output directory for all projects with build mode "release". Add the build option to the options stored in the IDE.
  * Combine the above with sessions and you can alter third party projects and packages without touching them.



#### Changing the LCLWidgetType in Version 1.1 and above

This setting is only available when the project uses the package LCL. 

Go to Project > Project Options > Comiler Options > Additions and Overrides > Set "LCLWidgetType". 

[![SetLCLWidgetTypeIn1 7.png](https://wiki.freepascal.org/images/9/95/SetLCLWidgetTypeIn1_7.png)](</File:SetLCLWidgetTypeIn1_7.png>)

#### Add a flag to project and all packages

Note: This modifies the project (.lpi). It does not alter the package files (.lpk), but it effects them when you build the project. It also effects building the IDE. 

Go to **Additions and Overrides**. Click on the **Add** button and select **Custom Option** : 

[![addflagtoprojectsandpackages1.png](https://wiki.freepascal.org/images/4/46/addflagtoprojectsandpackages1.png)](</File:addflagtoprojectsandpackages1.png>)

You should now see a "_Targets: *_ " and the new option. It is enabled for the currently active build mode and disabled for all other. The **Targets: *** means it applies to the project and all packages. 

[![addflagtoprojectsandpackages2.png](https://wiki.freepascal.org/images/b/b9/addflagtoprojectsandpackages2.png)](</File:addflagtoprojectsandpackages2.png>)

Click in the value cell behind the option and add your flag. For example: **-dSomeFlag**. 

[![addflagtoprojectsandpackages3.png](https://wiki.freepascal.org/images/b/bd/addflagtoprojectsandpackages3.png)](</File:addflagtoprojectsandpackages3.png>)

That's it. 

#### Add a flag to all projects and packages

The flag from the previous paragraph is only active when the project is loaded. To set the flag for all projects, select the option and click on the **up** button (green arrow up). This moves the option to the "Stored in IDE" group. 

[![addflagtoallprojectsandpackages1.png](https://wiki.freepascal.org/images/8/84/addflagtoallprojectsandpackages1.png)](</File:addflagtoallprojectsandpackages1.png>)

Note: The IDE uses the options of build mode "default". 

#### Change the output directory of project and all packages

Go to **Additions and Overrides**. Click on the **Add** button and select **Output Directory (-FU)**. 

[![AddsAndOverridesOutDir1.png](https://wiki.freepascal.org/images/7/7a/AddsAndOverridesOutDir1.png)](</File:AddsAndOverridesOutDir1.png>)

You should now see a "_Targets: *_ " and the new option "OutDir" with the default value **lib/$(TargetCPU)-$(TargetOS)/$(BuildMode)**. It is enabled for the currently active build mode and disabled for all other. The **Targets: *** means it applies to the project and all packages. 

[![AddsAndOverridesOutDir2.png](https://wiki.freepascal.org/images/6/6a/AddsAndOverridesOutDir2.png)](</File:AddsAndOverridesOutDir2.png>)

For example: Click in the value cell behind the option and change it to **lib/$(FPCVer)/$(TargetCPU)-$(TargetOS)**. This will use different output directories for each Free Pascal compiler version. 

If you want to use the same output directories for all your projects, you can put the option under **Stored in IDE**. Click in the value cell of the OutDir option to select the row. Then use the _Up_ (arrow up) button to move the option. 

If you want to apply the output directory to all packages, but not projects, change the **Targets** value from _*_ to _*,-#project_. 

[![AddsAndOverridesAllPkgOutDir1.png](https://wiki.freepascal.org/images/e/e2/AddsAndOverridesAllPkgOutDir1.png)](</File:AddsAndOverridesAllPkgOutDir1.png>)

**Note:** If a package does not support your output directory, append to _Targets_ value **-packagename**. For example: ***,-chmhelp**. 

If you want to put the package output files into sub directories of the project, change the OutDir to **$(ProjPath)/lib/$(PkgName)/$(TargetCPU)-$(TargetOS)/$(BuildMode)**. Note the use of the macros _$(ProjPath)_ for the project directory and _$(PkgName)_ to give each package its own directory. 

[![OutputDirectoryOFAllPackagesIntoProjectDirectory.png](https://wiki.freepascal.org/images/d/d1/OutputDirectoryOFAllPackagesIntoProjectDirectory.png)](</File:OutputDirectoryOFAllPackagesIntoProjectDirectory.png>)

For example if your project directory is _C:\pascal\myapp_ then the package LCL will be compiled into _C:\pascal\myapp\lib\LCL\i386-win32\default_. 

#### Add a flag to one package without altering the lpk itself

This examples appends the option _-vm4104_ to the package _lazreport_. 

  * Go to **Additions and Overrides**.
  * Click on the **Add** button and select "New Target". A new row is inserted with "Targets: *"
  * Click on the new row right behind the "Targets:". The ***** is now editable.
  * Change it to the package name. In this example: _lazreport_. Press return or click on another row to finish editing.
  * When you move the mouse over the new row you should see the hint _Apply to all packages matching the name "lazreport"_.
  * Click on the **Add** button and select "Custom Option". A new row is inserted below the row _Targets: lazreport_. Note: You can move rows using the green arrows
  * Click in the new empty value and change its value to **-vm4104**.



This option is stored in your current project (lpi) and is only added if you compile this project. If the option should be used with all your projects on your machine, use the green buttons to move the _Targets: lazeport_ up to _Stored in IDE_. 

[![AdditionsAndOverridesOptionForSinglePkg1.png](https://wiki.freepascal.org/images/6/68/AdditionsAndOverridesOptionForSinglePkg1.png)](</File:AdditionsAndOverridesOptionForSinglePkg1.png>)

## Compilation

## Compiler Commands

[![CompilerOptions-Compiler Commands.png](https://wiki.freepascal.org/images/4/48/CompilerOptions-Compiler_Commands.png)](</File:CompilerOptions-Compiler_Commands.png>)

Create Makefile

Enable this, if the IDE should create a Makefile and a Makefile.fpc before each build. This is currently only supported for packages, not for projects. 

Execute before
    Here, you can configure a command to execute before running the compiler.
Call on

  * Compile - execute when normally compiling (F9).
  * Build - execute when rebuilding all. This could for example be a script to clean up.
  * Run - execute when quick compiling. When running a project, the IDE checks if rebuild is needed. If no rebuild is needed the IDE skips the compile step. Set this option to always execute the command, even if the compile step is skipped.



scan for messages
    The IDE can parse and filter the output of the command and stop on errors. Check the boxes for which messages the IDE should watch.

### Compiler

This is compiler path used by the project or package. Default is the macro $(CompPath), which is replaced with the compiler filename in the environment options. 

### Execute after

Provide an (optional) command to execute after running the compiler. See above 'Execute before' for details. 

One handy usage might be to automatically copy your cross-compiled executable from your PC onto your target device, eg a Raspberry Pi: 
    
    
    scp "$TargetFile()" pi@raspberry:/home/pi/bin
    

(And yes, this works even without a password, if you create a key pair with ssh-keygen and then add it to your target's authorized_keys file via "ssh-copy-id [remoteuser@]remotehost"; for details see [here](<https://www.debian.org/devel/passwordlessssh>)) 

## Inherited

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This was moved in Lazarus 1.1 to the [Show Compiler Options](<IDE_Window__Show_Compiler_Options.md> "IDE Window: Show Compiler Options") dialog.

This page shows all the compiler options inherited from packages. Packages inherit options via their **usage** [options](<IDE_Window__Package_Options.md> "IDE Window: Package Options"). 

The topmost node shows all inherited options, that is the sum of all used packages. 

The nodes below show the inherited options of each used package. 

You can see/edit the set the used packages for the project in the project inspector. You can see/edit the set of used packages for a package in the package editor. 

For information about packages in general see [Lazarus Packages](<Lazarus_Packages.md> "Lazarus Packages"). 

## Use these settings as default for new projects

Check this checkbox and click Ok. The settings will be saved to _~/.lazarus/compileroptions.xml_ (or whatever you have as primary config path). When you create a new project this file will be loaded to initialize the compiler options. This feature exists since 0.9.29. 

## Buttons

### Test

This will run various test and detects common configurations mistakes. FPC 3.2.0 will warn about some double units. The warnings are correct, but you can ignore them, if you don't use these units. 

### Show Options

Opens a dialog and shows the current compiler and command line parameters. 

### Load/Save

Opens a dialog to save and/or load the current compiler options from/to a xml file. 

### Ok

This will apply the changes and then exit the dialog. 

### Cancel

This will undo all changes and exit the dialog. 

* * *

* * *

## IDE Macros

[![Compiler Options - IDE Macros](https://wiki.freepascal.org/images/6/6c/CompilerOptions-IDE_Macros.png)](</File:CompilerOptions-IDE_Macros.png> "Compiler Options - IDE Macros")

  


### For 0.9.29 to 1.0.x

This page allows to define your project/package specific macros and conditionals. The IDE already provides a lot of [macros](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames"). You can add your own macros that are valid when the project/package is loaded. 

Conditionals allow to set macro values depending on target platform and other macros. For example you can add a linker option when compiling for Mac OS X. 

Use the left **+** button to add a new macro for the project/package. Select a macro and click on the middle **+** button to add a new possible value. The actual value of a macro is set in the conditionals below, or by the current project on the **build modes** page (IDE menu / Project / Project Options / Compiler options / Build Modes). To delete a value or a macro, select it and click on the **-** button. 

The conditionals use a scripting language similar to pascal and are edited in the text editor at the bottom of the page. Many short cuts work like in the source editor, including word/identifier completion (default: Ctrl+Space). 

For more details about build macros and conditionals see [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals"). 

[![Compileroptions buildmacros1.png](https://wiki.freepascal.org/images/5/52/Compileroptions_buildmacros1.png)](</File:Compileroptions_buildmacros1.png>)

This page exists since version 0.9.29. 

### For 1.1 and above

This page is only available for packages. It lets packages define their own IDE macros. The IDE already provides a lot of [macros](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames") itself. 

Use the left **+** button to add a new macro for the project/package. Select a macro and click on the middle **+** button to add a new possible value. The actual value of a macro is set in the conditionals below, or by the current project on the **build modes** page (IDE menu / Project / Project Options / Compiler options / Build Modes). To delete a value or a macro, select it and click on the **-** button. 

For more details about build macros and conditionals see [Macros and Conditionals](<Macros_and_Conditionals.md> "Macros and Conditionals"). 

## Other

See [Free Pascal - Online documentation](<http://www.freepascal.org/docs.html>)

You'll typically define some compiler options here. For example you can define in a build mode something like: 
    
    
      -dRELEASE
    

Then in the build mode, only the code surrounded by the {$IFDEF RELEASE} {$ENDIF} will be compiled. This can be used alternatively to the Macro system, particularly if you come from Delphi. 

Spaces at start and end are removed. Line breaks are replaced by a space before passing to the compiler. A leading space is added automatically. 

The IDE substitutes IDE macros in custom options and parses the options. Flags like -dRelease are passed to codetools, so the source editor knows them immediately. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Do not add the path options -Fu, -Fi, -Fl, -FU, -o. Use the fields on the page [Paths](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options") instead. The IDE ignores the paths in the custom options. It does not update them, when you add or delete files or when you open the project on another host.

Clicking the **All options...** button allows you to set FPC options easily: [![options.png](https://wiki.freepascal.org/images/c/c5/options.png)](</File:options.png>)

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Compiler_Options](https://web.archive.org/web/20250408162448/https://wiki.freepascal.org/IDE_Window%3A_Compiler_Options)_
