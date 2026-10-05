# IDE Macros in paths and filenames

│ **English (en)** │  **[русский (ru)](<../ru/IDE_Macros_in_paths_and_filenames.md>)** │

## Contents

  * 1 Types of Macros
  * 2 IDE macro format
  * 3 General Purpose
  * 4 Filename Parts
  * 5 Paths and Parts
  * 6 Environment
  * 7 Macros in IDE addons
    * 7.1 Pas2jsDsgn
    * 7.2 ProjectGroups
    * 7.3 Delphi compiler tool



## Types of Macros

There are various types of macros: 

  * **IDE macros** : they can be used in almost all IDE fields, e.g. search paths, custom options, file names, run parameters. They are replaced with their value before calling external tools like the compiler or the debugger. They are case insensitive.
  * **FPC symbols** : these are either defined (on) or undefined (off). They are passed via the command line option **-d** , which can be set under _Compiler Options / Custom Options_. For example _-dDEBUG -dVerbose_ will define the FPC symbols DEBUG and Verbose, so you can use _{$IFDEF Debug}_. They are case insensitive.
  * **FPC macros** : they are e.g. specified via code like {$define MYFPCMACRO:=42}. An example of a predefined macro is FPC_FULLVERSION.
  * **Build macros** : they are IDE macros with a limited scope. They are defined by projects and packages and are case insensitive.
  * Some IDE plugins have their own macros.



## IDE macro format

IDE macros are used in the following format. Replace the _macro-name_ part with one of the macros listed below. 
    
    
    $(macro-name)
    

For example: This "Unit Output directory" is often used for Lazarus Packages: 
    
    
    lib/$(TargetCPU)-$(TargetOS)
    

  * under a x86 Linux 32-bit system that would equate to: **lib/i386-linux**
  * under a x86 Linux 64-bit system that would equate to: **lib/x86_64-linux**



There are also some **macro functions** that use the following format: 
    
    
    $macro_name(parameters)
    

For example 
    
    
    $Ext(unit1.pas)
    

will give _.pas_. 

  
_**Windows:**_ Compiling to a RamDrive where each project gets its own unit directory (Project> Options> Unit Output Directory) 
    
    
     R:\FPC\$NameOnly($(ProjFile))
    

  
_**MacOS:**_ A more complex example but very useful, when you want to compile on its name _**$NameOnly($(ProjFile))**_ on a specific CPU target _**-$(TargetCPU)**_ saving the compiled file on the package path _**.app/Contents/MacOS/**_
    
    
    $NameOnly($(ProjFile))-$(TargetCPU).app/Contents/MacOS/$NameOnly($(ProjFile))-$(TargetCPU)
    

## General Purpose

  * **Col** \- current column in source editor
  * **Row** \- current row in source editor (screen, can differ to byte position due to tabs and UTF-8)
  * **CurToken** \- current token at the cursor in the source editor
  * **EdFile** \- current filename in the source editor
  * **Params** \- run parameters of the current project
  * **Prompt** \- ask the user for a value. This is an interactive macro.
  * **RunCmdLine** \- the command to launch the project
  * **Save** \- save current file in source editor
  * **SaveAll** \- save all
  * **TargetCmdLine** \- the project executable (or host / i.e.: "$TargetFile") plus run parameters



## Filename Parts

  * **Ext(filename)** \- macro function for ExtractFileExt. Example: will return .exe on Windows.
  * **MakeDir(filename)** \- macro function for AppendPathDelim
  * **MakeFile(filename)** \- macro function for ChompPathDelim
  * **MakeExe(filename)** \- change file extension to .exe under Windows, nothing under Linux, BSD, macOS. When cross compiling it will create a file name for the target OS. To get the file name of the host OS use $MakeExe(ide,filename). To get the file name of a specific OS use $MakeExe(os,filename). The two parameter variant exists since 1.1.
  * **MakeLib(filename)** \- change file extension to .dll under Windows, under Linux/BSD change to lowercase libname.so, under macOS to libname.so (since 0.9.29)
  * **Name(filename)** \- macro function for ExtractFileName
  * **NameOnly(filename)** \- macro function for ExtractFileNameOnly
  * **Path(filename)** \- macro function for ExtractFilePath



## Paths and Parts

  * **CompPath** \- compiler path. Before 1.3 it returned the value of the IDE option (_Tools / Options ..._). It cannot use the macros FPCVer, FPC_FULLVERSION, because these macros are retrieved from the compiler. You can use the macros TargetOS, TargetCPU, SrcOS **only** if you set them. You can use LazarusDir and Env macros. Since 1.3 the macro returns the compiler of the project. It returns the default IDE option value: 
    * when using the parameter _IDE_ : $CompPath(IDE)
    * in the project compiler option itself
    * when the project compiler is disabled (_Build_ , _Compile_)
    * when the project compiler is not fpc or ppcsomething
    * when the IDE is built
  * **ConfDir** \- directory where the IDE stores its config files (same as PrimaryConfigPath).
  * **ExeExt** \- executable file extension for the operation system, independent of project's target OS. To get the extension for the current project target OS use $MakeExe().
  * **FallbackOutputRoot** \- the directory where the IDE puts ppu files if the output directory of a package is not writable. Default: $(PrimaryConfigPath)/lib. Since 0.9.31.
  * **FPCSrcDir** \- FPC source directory set in environment options
  * **FPCMsgFile** \- compiler messages file set in the Lazarus environment options (since 0.9.31).
  * **InstantFPCCache** \- path of the instantfpccache. Output of running _instantfpc --get-cache_ (since 0.9.31).
  * **LazarusDir** \- Lazarus source directory set in Lazarus environment options. No macros allowed.
  * **Make** \- path to the make utility (gmake under BSD) (since 0.9.29)
  * **PrimaryConfigPath** \- the directory of the IDE's configuration files (since 0.9.31). See **SecondaryConfigPath**.
  * **ProjFile** \- the full filename of the main source of the current project (.lpr)
  * **ProjIncPath** \- include path of project directory
  * **ProjOutDir** \- path of project output directory (e.g. where the .ppu files are created) (since 0.9.27)
  * **ProjPath** \- the project directory (the directory of the .lpi file)
  * **ProjPublishDir** \- publishing directory of the current project
  * **ProjSrcPath** \- source path of project directory
  * **ProjUnitPath** \- unit path of project directory
  * **Project(param)** \- macro function for various values: 
    * **Project(UnitPath)** \- unit path of project directory
    * **Project(SrcPath)** \- source path of project directory
    * **Project(IncPath)** \- include path of project directory
    * **Project(InfoFile)** \- filename of the project information file (.lpi) (since r15287, 0.9.25)
    * **Project(OutputDir)** \- directory where the project's ppu files are created (since 0.9.27)
  * **Package macros** \- these can be used in the fields of a package. For example in the search paths of a package. To use them elsewhere give a package name as parameter. 
    * **$(PkgName)** \- in a package: gives the package name (since 0.9.31)
    * **$PkgName(id)** \- macro function for the name of a package ID given as parameter (since 0.9.31)
    * **$(PkgDir)** \- macro for the directory (location of the .lpk) of the package
    * **$PkgDir(id)** \- macro function for the directory (location of the .lpk) of a package ID given as parameter
    * **$PkgIncPath(id)** \- macro function for the include path of a package ID given as parameter
    * **$PkgOutDir(id)** \- macro function for the output directory of a package (e.g. where the ppu files are created)
    * **$PkgSrcPath(id)** \- macro function for the source path (unit path + src path) of a package ID given as parameter
    * **$PkgUnitPath(id)** \- macro function for the unit path of a package ID given as parameter
  * **SecondaryConfigPath** \- the directory of the IDE's configuration templates (since 0.9.31). See **PrimaryConfigPath**
  * **OutputFile** \- the output file (executable or library) of the current project build mode. This is the value from the "[Target file name](<IDE_Window__Project_Options.md> "IDE Window: Project Options")" field of the Project Options (macros and relative paths are expanded).
  * **TargetFile** \- executable file (or script) for launching a project. This is the value of the "[Host application](<IDE_Window__Run_parameters.md> "IDE Window: Run parameters")" parameter from the "Run parameters" window (by default it is empty and **OutputFile** is used).
  * **TestDir** \- Test directory set in the Lazarus environment options



## Environment

  * **BuildMode** \- the name of the active build mode (since 1.1)
  * **Env(name)** \- macro function for environment variables given to the IDE (not project nor debugger) (see [GetEnvironmentVariableUTF8](<http://lazarus-ccr.sourceforge.net/docs/lcl/fileutil/getenvironmentvariableutf8.html>)) (since 0.9.27)
  * **IDEBuildOptions** \- the extra options of the 'Configure Build Lazarus' dialog (since 0.9.29). In Makefile: empty.
  * **LanguageID** \- IDE language, for example en for English, de for German
  * **LanguageName** \- IDE language name, the translated name of the current language. For instance: Deutsch for German.
  * **LazVer** \- Version string of the IDE (since 1.3).
  * **FPCVer** \- FPC version (since 0.9.25). For example '2.4.2'. This version is fetched from the compiler $(CompPath), which path is set in the Lazarus environment options. **FPCVer** depends on project's compiler path.
  * **FPC_FULLVERSION** \- FPC version as one number (since 1.3). For example _20701_. This version is fetched from the compiler $(CompPath), which path is set in the Lazarus environment options. **FPC_FullVersion** depends on project's compiler path.
  * **SrcOS** \- 'unix' for linux, darwin, bsd. 'win' for win32, win64, wince. Since 0.9.31: Use **$SrcOS(SomeOS)** to get the SrcOS of a specific OS, for example **$SrcOS($TargetOS(IDE))** to get the SrcOS of the IDE executable. **SrcOS** depends on _TargetOS_.
  * **TargetOS** \- Target OS of the current project. Since 0.9.31: Use **$TargetOS(IDE)** to get the OS of the IDE executable. If the project has not set the **TargetOS** the IDE uses the default platform of the project's compiler (1.3 and below uses as default the IDE's platform). In Makefile it is converted to _%(OS_TARGET)_.
  * **TargetCPU** \- similar to _TargetOS_. In Makefile it is converted to _%(OS_TARGET)_.
  * **Subtarget** \- Subtarget of the current project. Since 3.99.
  * **FPCTarget** \- works similar to the FPC config macro _$fpctarget_. It's a shortform of **$TargetCPU(param)-$TargetOS(param)-$SubTarget(param)**. The -SubTarget is omitted if empty. **$FPCTarget(IDE)** returns the IDE's target, otherwise the project's target platform. Since Lazarus 3.99.
  * **LCLWidgetType** \- LCL widgetset of the current project. In Makefile it is converted to _%(LCL_PLATFORM)_.



  


# Macros in IDE addons

Here is an incomplete list of macros added by IDE packages: 

## Pas2jsDsgn

  * **Pas2JS** \- Pas2JS executable
  * **Pas2JSBrowser** \- Pas2JS selected Browser executable
  * **Pas2JSNodeJS** \- Pas2JS selected NodeJS excutable
  * **Pas2JSProjectURL** \- Pas2JS current project URL



## ProjectGroups

  * **PGSrcPaths** \- source paths of all targets, i.e. projects, packages, etc. of the project group



## Delphi compiler tool

  * **DCC** \- The delphi compiler executable path, as set in the global settings.
  * **DCCCONFIG** If the 'Generate compiler configuration' settting is checked, this contains the generated delphi compiler configuration file, prepended with @. If the setting is not set, the macro will be empty.
  * **DCCARGS** The combined values of the global and project 'Extra compiler arguments' settings.
  * **DCCCOMPILE** This is equivalent to $(DCC) $(DCCCONFIG) $(DCCARGS).

---

_Source: [https://wiki.freepascal.org/IDE_Macros_in_paths_and_filenames](https://web.archive.org/web/20250126164007/https://wiki.freepascal.org/IDE_Macros_in_paths_and_filenames)_
