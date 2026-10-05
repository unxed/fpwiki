# Codetools

│ **English (en)** │  **[русский (ru)](<../ru/Codetools.md>)** │

## Contents

  * 1 What are the codetools?
  * 2 Using the codetools without the IDE
    * 2.1 Defining search paths and macros
      * 2.1.1 Example: Expand IncPath for a directory and/or its sub directories
      * 2.1.2 Example: Expand global UnitPath
    * 2.2 Define Template Macros
    * 2.3 Define Template Actions
    * 2.4 How to extend the include path of a directory
  * 3 Using the codetools in the IDE with the IDEIntf
  * 4 Codetools rules for FPC sources
    * 4.1 Usage of testfpcsrcunitrules
    * 4.2 Example for testfpcsrcunitrules
    * 4.3 Duplicate source files
  * 5 How the codetools parses sources differently from the compiler
  * 6 Tool, Node, LinkScanner
    * 6.1 Tool
    * 6.2 LinkScanner
    * 6.3 Node
  * 7 CleanPos and CursorPos
  * 8 Inserting, deleting, replacing - the TSourceChangeCache
    * 8.1 Usage
    * 8.2 BeginUpdate/EndUpdate
    * 8.3 Saving changes to disk
  * 9 Hints/Tips/Guide Lines
  * 10 Test suite
  * 11 Links



# What are the codetools?

Codetools is a Lazarus package providing tools to parse, explore, edit and refactor Pascal sources. The codetools have been packaged in their own module and are licensed under GPL. Many examples showing how to use the codetools in your own programs can be found under components/codetools/examples. 

svn: 

  * Lazarus: <http://svn.freepascal.org/svn/lazarus/trunk>
  * Only CodeTools: <http://svn.freepascal.org/svn/lazarus/trunk/components/codetools>



# Using the codetools without the IDE

You can use the codetools without the IDE. This can be used to test a new tool. An easy example is 
    
    
     <lazarusdir>/components/codetools/examples/methodjumping.lpi
    

To test find declaration, the codetools need to parse sources. Especially the RTL and FCL sources. The examples use the following environment variables: 

  * FPCDIR: path to the FPC sources, the default is _~/freepascal/fpc_.
  * PP: path to the compiler executable (/usr/bin/fpc or /usr/bin/ppc386 or C:\lazarus\ppc386.exe). The codetools need to ask the compiler for the settings. The default is to search for 'fpc' via the PATH variable.
  * FPCTARGETOS: tell the codetools to scan for another operating system (cross compiling). For example: linux, freebsd, darwin, win32, win64, wince
  * FPCTARGETCPU: when scanning for another CPU. For example: i386, powerpc, x86_64, arm, sparc
  * LAZARUSDIR: path of the lazarus sources. Only needed if you want to scan them.



FPC is a very complex project with lots of search paths, include files and macros. The codetools need to know all these paths and macros in order to parse this jungle. To setup all this easily the codetools contain predefined templates for FPC, Lazarus, Delphi and Kylix source directories. See for a find declaration example 
    
    
     <lazarusdir>/components/codetools/examples/finddeclaration.lpi
    

Because the FPC sources contain multiple versions of some units, and the FPC sources change often, the codetools do not use a fixed table of paths, but rather scan the whole FPC directory structure on first use, applying a set of rules specifying what is the correct source for the current TargetOS and TargetCPU. This scan may take a while depending on your disk speed. All examples save the result in **codetools.config** , so that the next time Lazarus is started this possibly lengthy scan is skipped. 

Whenever the FPC sources are changed or a unit is renamed, just delete the **codetools.config** file. The Lazarus IDE has its own config file and does a rescan whenever the compiler executable changes (or when the user forces a 'Tools > Rescan FPC source directory'). 

## Defining search paths and macros

Important: Codetools assumes that you have multiple directories with different compiler settings, e.g. the FPC sources have different search paths than your sources. You can have FPC, Delphi and pas2js directories - all at the same time. 

The codetools uses "define templates" to generate search paths and macros. Define templates are trees of rules. 

### Example: Expand IncPath for a directory and/or its sub directories

The following demo demonstrate various ways to expand the IncPath for a directory and/or its sub directories: 
    
    
    program SetIncludePath;
    
    {$mode objfpc}{$H+}
    
    uses
      SysUtils, CodeToolManager, DefineTemplates, LazFileUtils;
      
    var
      Directory: String;
      DirectoryTemplate: TDefineTemplate;
      IncPathTemplate: TDefineTemplate;
      SubDirectory: String;
    begin
      // create a template for the current directory
      // all child nodes of this template are only valid for this directory.
      Directory:=ExpandFileNameUTF8(GetCurrentDirUTF8);
      DirectoryTemplate:=TDefineTemplate.Create('Current working directory',
        'Example template for current working directory','',Directory,da_Directory);
    
      // Example 1:
      //  Add a sub template to extend the include search path #IncPath
      //  only for the current directory, not its sub directories.
      // Note: 'myincludes' is a relative path, so it won't work for sub directories.
      IncPathTemplate:=TDefineTemplate.Create('Add myincludes to the IncPath',
        'Add myincludes to the include search path',
        IncludePathMacroName,  // variable name: #IncPath
        IncludePathMacro+';myincludes' // new value: $(#IncPath);myincludes
        ,da_Define // da_Define extends the IncPath only for this directory
        );
      DirectoryTemplate.AddChild(IncPathTemplate);
    
      // Example 2:
      //  Add a sub template to extend the include search path #IncPath
      //  with an absolute path for the current directory and all its sub directories.
      // Note: '/tmp/myincludes' is an absolute path, so works for sub directories.
      IncPathTemplate:=TDefineTemplate.Create('Add /tmp/myincludes to the IncPath',
        'Add /tmp/myincludes to the include search path',
        IncludePathMacroName,  // variable name: #IncPath
        IncludePathMacro+';'+SetDirSeparators('/tmp/myincludes') // new value: $(#IncPath);/tmp/myincludes
        ,da_DefineRecurse // da_DefineRecuse extends the IncPath for this directory and all sub directories
        );
      DirectoryTemplate.AddChild(IncPathTemplate);
    
      // Example 3:
      //  Using the #DefinePath macro you can use the current directory to create absolute paths.
      IncPathTemplate:=TDefineTemplate.Create('Add ./myincludes to the IncPath',
        'Add ./myincludes to the include search path',
        IncludePathMacroName,  // variable name: #IncPath
        IncludePathMacro+';'+DefinePathMacro+'/myincludes' // new value: $(#IncPath);$(#DefinePath)/myincludes
        ,da_DefineRecurse // da_DefineRecuse extends the IncPath for this directory and all sub directories
        );
      DirectoryTemplate.AddChild(IncPathTemplate);
    
      // add the directory template to the tree
      CodeToolBoss.DefineTree.Add(DirectoryTemplate);
    
      writeln('Directory="',Directory,'"',
        ' IncPath="',CodeToolBoss.GetIncludePathForDirectory(Directory),'"');
      SubDirectory:=AppendPathDelim(Directory)+'sub';
      writeln('SubDirectory="',SubDirectory,'"',
        ' IncPath="',CodeToolBoss.GetIncludePathForDirectory(SubDirectory),'"');
    end.
    

### Example: Expand global UnitPath

Here is an example how to extend the global (all directories) unit search path and query the unit path of one directory. This might be useful for single minded command line tools. 
    
    
    uses
      Classes, SysUtils, CodeToolManager, DefineTemplates, FileProcs;
      
    var
      Directory: String;
      UnitPathTemplate: TDefineTemplate;
    begin
      // add a sub template to extend the include search path #UnitPath.
      UnitPathTemplate:=TDefineTemplate.Create(
        'Add myunits to the UnitPath',  // optional: an arbitrary name of the template, useful for finding it later
        'Add /tmp/myunits to the unit search path', // optional: a description
        UnitPathMacroName,  // variable name: #UnitPath
        UnitPathMacro+';/tmp/myunits' // new value: $(#UnitPath);/tmp/myunits
        ,da_DefineRecurse
        );
      // add the unit path template to the tree
      CodeToolBoss.DefineTree.Add(UnitPathTemplate);
    
      Directory:=ExpandFileNameUTF8(GetCurrentDirUTF8);
      writeln('Directory="',Directory,'"',
        ' UnitPath="',CodeToolBoss.GetUnitPathForDirectory(Directory),'"');
    end.
    

## Define Template Macros

Macros defined by define templates are used by the Pascal parser. The Codetools have functions to query the Free Pascal Compiler for its macros and convert them to define templates. This is done by CodeToolBoss.Init(Options); which you can find in many examples. 

Additionally there some predefined macros, which start with a hash (#). See unit definetemplates for the full list. Here are some important ones: 
    
    
      ExternalMacroStart       = '#';
      // Standard macros
      DefinePathMacroName      = ExternalMacroStart+'DefinePath'; // the current directory
      UnitPathMacroName        = ExternalMacroStart+'UnitPath'; // unit search path separated by semicolon (same as given to FPC)
      IncludePathMacroName     = ExternalMacroStart+'IncPath'; // include file search path separated by semicolon (same as given to FPC)
      SrcPathMacroName         = ExternalMacroStart+'SrcPath'; // unit source search path separated by semicolon (not given to FPC)
    

For example the **IncludePathMacroName** is **#IncPath** and is used to define the include file search path. Keep in mind that macro values depend on the directory. 

Define templates values can contain macros. The macro has to be enclosed in dollar and brackets: **$(macro)**. 

In the above example the value was _'$('+IncludePathMacroName+');/tmp/myincludes'_ , which is _'$(#IncPath);/tmp/myincludes'_ , which executes to the old include search path plus _';/tmp/myincludes'_ , which means a path is appended. For readability you can use the constant IncludePathMacro instead of _'$('+IncludePathMacroName+')'_ : 
    
    
      DefinePathMacro          = '$('+DefinePathMacroName+')'; // the path of the define template
      UnitPathMacro            = '$('+UnitPathMacroName+')';
      IncludePathMacro         = '$('+IncludePathMacroName+')';
      SrcPathMacro             = '$('+SrcPathMacroName+')';
    

## Define Template Actions

A define template has a name, an optional description, a variable, a value and an action. The name and description are optional. The meaning of variable and value depends on the action. You can add define templates as children of define templates - creating a tree of define templates. You can see many examples for define templates in the Lazarus' dialog _Tools / CodeTools Defines Editor_. 

  * da_Block - Used for grouping templates. When the block is executed all children are executed.
  * da_Directory - Use this to define all rules of a directory. If this is a root directory set Value to the full expanded directory path (use function CleanAndExpandDirectory). If this is a sub directory (parent template is a directory) Value is the sub path. Can contain macros. Children are only executed if directory fits.
  * da_Define - sets a macro (Variable) value (Value) of the current directory. The value can contain macros. Note that when a macro value is set to empty string _it is still defined, that means_{$IFDEF variable}_will still result in_ true _. Children are not executed._
  * da_Undefine - clears a macro (Variable) of the current directory. _{$IFDEF macro}_ results in _false_. Children are not executed.
  * da_DefineRecurse - as da_Define, but for current directory and sub directories.
  * da_UndefineRecurse - as da_Undefine, but for current directory and sub directories.
  * da_UndefineAll - clear all macro values.
  * da_IfDef - if macro _Variable_ is defined then execute children and skip following da_Else and da_ElseIf.
  * da_IfNDef - if macro _Variable_ is not defined then execute children and skip following da_Else and da_ElseIf.
  * da_If,daElseIf - if the boolean expression _Value_ executes to true then execute children and skip following da_Else and da_ElseIf. Value can contain macros.
  * da_Else - When this template is executed then execute all children.



## How to extend the include path of a directory

See the example lazarus/components/codetools/examples/setincludepath.lpr. It demonstrates the use of relative paths, absolute paths, how to use the DefinePathMacro and the difference between da_Define and da_DefineRecurse. 

# Using the codetools in the IDE with the IDEIntf

See <lazarusdir>/examples/idequickfix/quickfixexample.lpk package. It demonstrates: 

  * How to write an IDE package. When you install this package it will register a Quick Fix item in the IDE.
  * How to write a Quick Fix item for the compiler message: 'Parameter "Sender" not used'
  * How to use the codetools to: 
    * parse a unit
    * convert Filename, Line, and Column data into a codetools source position
    * find a codetools node at a cursor position
    * find a procedure node, and the begin..end node
    * create a nice insertion position for a statement at the beginning of the begin..end block
    * obtain line indentation information so that a new line will work in a sub-procedure as well
    * insert code using the codetools



# Codetools rules for FPC sources

When the codetools searches the source of a fpc ppu it uses a set of rules. You can write your own rules, but normally you will use the standard rules, which are defined in the include file components/codetools/fpcsrcrules.inc. You can test the rules with the command line utility: components/codetools/examples/testfpcsrcunitrules.lpi. 

## Usage of testfpcsrcunitrules
    
    
    Usage: lazarus/components/codetools/examples/testfpcsrcunitrules -h
    
      -c <compiler file name>, --compiler=<compiler file name>
             Default is to use environment variable PP.
             If this is not set, search for fpc
    
      -T <target OS>, --targetos=<target OS>
             Default is to use environment variable FPCTARGET.
             If this is not set, use the default of the compiler.
    
      -P <target CPU>, --targetcpu=<target CPU>
             Default is to use environment variable FPCTARGETCPU.
             If this is not set, use the default of the compiler.
    
      -F <FPC source directory>, --fpcsrcdir=<FPC source directory>
             Default is to use environment variable FPCDIR.
             There is no default.
    
      -u <unit name>, --checkunit=<unit name>
             Write a detailed report about this unit.
    

## Example for testfpcsrcunitrules

Open the testfpcsrcunitrules.lpi in the IDE and compile it. Then run the utility in a terminal/console: 
    
    
    ./testfpcsrcunitrules -F ~/fpc/sources/2.5.1/fpc/
    

This will tell you which compiler is used, which compiler executes, which config files were tested and parsed, and it warns you about duplicate units in the FPC search path and about duplicated unit source files. **Note:** This example caches results in the file _codetools.config_. You should delete _codetools.config_ when you update the compiler or your fpc.cfg file. 

## Duplicate source files

You find out that the codetools opens for target wince/arm the wrong source of the unit _mmsystem_. Run the tool with the -u parameter: 

./testfpcsrcunitrules -F ~/fpc/2.5.1/fpc/ -T wince -P arm -u mmsystem 

This will give you a detailed report where this unit was found and what score each source file got. For example: 
    
    
    Unit report for mmsystem
      WARNING: mmsystem is not in PPU search path
    GatherUnitsInFPCSources UnitName=mmsystem File=packages/winunits-base/src/mmsystem.pp Score=11
    GatherUnitsInFPCSources UnitName=mmsystem File=packages/winceunits/src/mmsystem.pp Score=11 => duplicate
    

This means there are two source files with the same score, so the codetools took the first. The last one in winceunits is for target wince and the first one is for win32 and win64. 

Now open the rules file fpcsrcrules.inc. 

Rules work like this: 
    
    
    Score:=10;
    Targets:='wince';
    Add('packages/winceunits');
    

The **Add** adds a rule for all files beginning with 'packages/winceunits' that adds a score of 10 to all these files. The **Targets** is a comma separated list of target operating systems and/or target processors. For example Targets='wince,linux,i386' means: apply this rules to TargetOS wince or linux and to all TargetCPU i386. 

# How the codetools parses sources differently from the compiler

A compiler is optimized to parse code linearly and to load needed units and include files as soon as it parses a uses section or a directive. The codetools are optimized to parse only certain code sections. For example jumping from the method declaration to the method body only needs the unit and its include files. When a codetool searches a declaration it searches backwards. That means it starts searching in the local variables, then upwards from the implementation. When it finds a uses section it searches the identifiers in the interface section of the units. When the identifier is found it stops. The result and some intervening steps are cached. Because it often only needs to parse a few interface sections it finds an individual identifier very quickly. 

The codetools do not parse a source in a single step (as the compiler does) but in several steps, which depend on what the current codetool needs: 

  * First a source file is loaded in a TCodeBuffer. The IDE uses this step to change the encoding to UTF8. The files are kept in memory and only reloaded if the modification date changes or if a file is manually reverted. There are several tools and functions which work directly on the buffer.
  * The next step is to parse the unit (or include file). A unit must be parsed from the beginning, so the codetools tries to find the main file, the first file of a unit. It does that by looking for a directive in the first line like {%MainUnit ../lclintf.pp}. If that does not exist, it searches in the includelink cache. The IDE saves this cache to disk, so the IDE learns over time.
  * After finding the main file a TCodeTool is created and a TLinkScanner parses the source. It handles compiler directives, such as include directives and conditional directives. The scanner can be given a range, so it might, for instance, parse only a unit's interface. The scanner creates the **clean source**. The clean source is put together from all include files, having been stripped of code in the _else_ part of conditional directives, which is skipped. It also creates a list of **links** which map the clean source to the real source files. The clean source is now Pascal containing no _else_ code. Note: there are also tools designed to scan a single source for all directives. These tools create a tree of directives.
  * After creating the clean source a TCodeTool parses it and creates a tree of TCodeTreeNode. It can also be given a range. This parser skips a few parts, for example class members, begin..end blocks and parameter lists. Many tools don't need them. These sub nodes are created on demand. A TCodeTreeNode has a range StartPos..EndPos which are clean positions, that means positions in the clean source. There are only nodes for the important parts. Creating nodes for every detail would need more memory than the source itself and is seldom needed. There are plenty of functions to find out the details. For example if a function has the 'cdecl' calling convention.
  * When searching for an identifier the search stores the base types it finds and creates caches for all identifiers in the interface section.



Every level has its own caches, which need to be checked and updated before calling a function. Many high level functions accessible via the CodeToolBoss do that automatically. For others it is the responsibility of the caller. 

Example for: 

unit1.pas: 
    
    
    unit Unit1;
    {$I settings.inc}
    interface
    uses
      {$IFDEF Flag}
      unix,
      {$ELSE}
      windows,
      {$ENDIF}
      Classes;
    

settings.inc: 
    
    
    {%MainUnit unit1.pas}
    {$DEFINE Flag}
    

clean source: 
    
    
    unit Unit1;
    {$I settings.inc}{%MainUnit unit1.pas}
    {$DEFINE Flag}
    interface
    uses
      {$IFDEF Flag}
      unix,
      {$ELSE}{$ENDIF}
      Classes;
    

  
Hint: To easily parse a unit and build the nodes, use **CodeToolBoss.Explore**. 

# Tool, Node, LinkScanner

## Tool

A tool is a TCodeTool. For Pascal files every unit, program and package gets its own TCodeTool. These are automatically created by the CodetoolBoss when parsing sources. A tool has a Scanner (TLinkScanner) and a Root (TCodeTreeNode), which is the root node of the tree of Pascal nodes of the module (e.g. unit). 

## LinkScanner

Every TCodeTool creates a TLinkScanner to scan the files and creates the CleanedSrc, which is the parsed unit source, include files included and code in skipped $IF directives removed. When only the unit interface was parsed, the CleanedSrc may not be complete. TLinkScanner is not shared between TCodeTools. There is a 1:1 mapping between TLinkScanner and TCodeTool. 

## Node

  * A TCodeTreeNode is part of a tree of such nodes. The root node is stored in a TCodeTool.Root. Nodes are not shared between tool. Every node is associated with exactly one TCodeTool.
  * When a function returns a Node, it always returns the associated Tool as well.
  * Node.StartPos/EndPos are positions 1-based in the Scanner.CleanedSrc.
  * To get from the cleaned postion to the file, line, column use Tool.CleanPosToCaret(Node.StartPos,xyp).



# CleanPos and CursorPos

There are several methods to define a position in the codetools. 

The codetools Absolute position is related to the source as a continuous string starting at character 1. For example a TCodeBuffer holds the file contents as a single string in its Source property. Caret or cursor positions are given as X,Y where X is the column number and Y the line number. Each value (X and Y) starts at 1. A TCodeBuffer provides the member functions **LineColToPosition'_and_** _AbsoluteToLineCol_ to convert between (X,Y) values and the Absolute codetools position. When working with multiple source files (such as a unit which may contain several include files), the clean position relates to the absolute position in the stripped code **Src**. Here Src is a string whose clean positions start at 1. Cursor positions are specified as TCodeXYPosition (Code,X,Y). A TCodeTool provides the functions **CaretToCleanPos** and **CleanPosToCaret** to convert between them. 

# Inserting, deleting, replacing - the TSourceChangeCache

When making changes to the source code of a unit (or its include files) you should use the CodetoolBoss.SourceChangeCache instead of altering the source directly. 

  * Simple usage: Connect, Replace, Replace, ... Apply. See below.
  * You can use cleanpos as given by the node tree OR you can use direct position in a file.
  * You can use Replace to insert and delete, which automatically calls events, so connected editors are notified of changes.
  * It can automatically insert needed spaces, line breaks or empty lines in front or behind each Replace. For example you define that there should be an empty line in front. The SourceChangeCache checks what is inserted and how much space there is already and will insert needed space.
  * It checks if the replaced/deleted span is writable.
  * You can do multiple Replaces and you control when they are applied. Keep in mind that inserting code means that the parsed tree becomes invalid and needs rebuilding.
  * Multiple replaces are checked for intersection. For example an insert in the middle of deleted code gives an error.
  * Mutiple insertions at the same place are added FIFO - first at the top.
  * You can combine several functions altering code to one bigger function. See below.



## Usage

The SourceChangeCache works on a unit, so you need to get a TCodeTool and scan a unit/include file. For example: 
    
    
      // Step 1: load the file and parse it
      Code:=CodeToolBoss.LoadFile(Filename,false,false);
      if Code=nil then
        raise Exception.Create('loading failed '+Filename);
      if not CodeToolBoss.Explore(Code,Tool,false) then
        ...;// parse error ...
    
      // Step 2: connect the SourceChangeCache
      CodeToolBoss.SourceChangeCache.MainScanner:=Tool.Scanner;
    
      // Step 3: use Replace to insert and/or delete code
      // The first two parameters are the needed spaces in front and behind the insertion
      // The FromPos,ToPos defines the deleted/replaced range in CleanPos positions.
      // The NewCode is the string of new code. Use '' for a delete.
      if not CodeToolBoss.SourceChangeCache.Replace(gtNone,gtNone,FromPos,ToPos,NewCode) then
        exit; // e.g. source read only or a former Replace has deleted the place
      ...do some more Replace...
    
      // Step 4: Apply the changes
      if not CodeToolBoss.SourceChangeCache.Apply then
        exit; // apply was aborted
    

## BeginUpdate/EndUpdate

BeginUpdate/EndUpdate delays the Apply. This is useful when combining several code changing functions. For example: 

You want to scan the unit, add a unit to the interface uses section, and remove the unit from the implementation uses section. The two functions AddUnitToMainUsesSection and RemoveUnitFromUsesSection use Apply, altering the source, so the second function would rescan the unit a second time. But since the two functions are independent of each other (they change different parts of the source) you can combine them and do it with one scan: 
    
    
      // Step 1: parse unit and connect SourceChangeCache
      if not CodeToolBoss.Explore(Code,Tool,false) then
        ...;// parse error ...
      CodeToolBoss.SourceChangeCache.MainScanner:=Tool.Scanner;
    
      // Step 2: delay Apply
      CodeToolBoss.SourceChangeCache.BeginUpdate;
    
      // Step 3: add unit to interface section
      // AddUnitToMainUsesSection would apply and change the code
      // Because of the BeginUpdate the change is not yet done, but stored in the SourceChangeCache
      if not Tool.AddUnitToMainUsesSection('Classes','',CodeToolBoss.SourceChangeCache) then exit;
    
      // Step 4: remove unit from implementation section
      // Without the BeginUpdate the RemoveUnitFromUsesSection would rescan the unit
      if Tool.FindImplementationUsesSection<>nil then
        if not Tool.RemoveUnitFromUsesSection(Tool.FindImplementationUsesSection,'Classes',CodeToolBoss.SourceChangeCache) then exit;
    
      // Step 5: apply all changes
      if not CodeToolBoss.SourceChangeCache.EndUpdate then
        exit; // apply was aborted
    

BeginUpdate/EndUpdate work with a counter, so if you call BeginUpdate twice you need to call EndUpdate twice. This means you can put the above example in a function and combine that with another function. 

## Saving changes to disk

The above changes are made to the code buffers and the buffers are marked modified. To save the changes to disk, you have to call Save for each modified buffer. 

  * The buffers that will be modified in the next Apply/EndUpdate are in SourceChangeCache.BuffersToModify and BuffersToModifyCount.
  * The events SourceChangeCache.OnBeforeApplyChanges/OnAfterApplyChanges are used by the CodeToolBoss, which connects it to its own OnBeforeApplyChanges/OnAfterApplyChanges. The Lazarus IDE sets these events and automatically opens modified files in the source editor, so all changes go into the undo list of synedit.



# Hints/Tips/Guide Lines

  * BuildTree checks the current state and will only parse if needed. If a former call has parsed the interface and you need the interface again, BuildTree will not do anything. If you need the full unit, only the implementation will be parsed. Nodes are only freed if some files have changed on disk or some settings have changed (initial macros). BuildTree does not check all files on every call. Instead it uses the directory cache of the codetools. So, if nothing has changed it will return very quickly. You should call it before any search operation.
  * Do **not** call BuildTree **after** an operation. That is a waste of CPU.
  * BuildTree raises an exception when there are missing include files or syntax errors. You should enclose your code into


    
    
    try
      Tool.BuildTree(lsrEnd);
      ... search ... replace  
    except
      on E: Exception do
        CodeToolBoss.HandleException(E);
    end;
    

  * After calling SourceChangeCache.Replace (multiple times) the sources (TCodeBuffers) have not changed immediately. You must call SourceChangeCache.Apply to change the sources. This will not save the changes to file.



# Test suite

The codetools test suite can be found here: 
    
    
    lazarus/components/codetools/tests/runtestscodetools
    

Compile it and run it: 
    
    
    cd lazarus/components/codetools/tests
    ./runtestscodetools
    

# Links

  * [Lazarus IDE Tools](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools") \- A tutorial about the built-in tools of the standard IDE
  * [Cody](<Cody.md> "Cody") \- IDE package adding advanced code tools to the IDE
  * [Extending the IDE](<Extending_the_IDE.md> "Extending the IDE") \- How to write your own codetools plugins for the IDE.

---

_Source: [https://wiki.freepascal.org/Codetools](https://web.archive.org/web/20241202161452/https://wiki.freepascal.org/Codetools)_
