# Command line parameters and environment variables

│ **English (en)** │  **[español (es)](</Command_line_parameters_and_environment_variables/es> "Command line parameters and environment variables/es")** │  **[suomi (fi)](</Command_line_parameters_and_environment_variables/fi> "Command line parameters and environment variables/fi")** │  **[français (fr)](</Command_line_parameters_and_environment_variables/fr> "Command line parameters and environment variables/fr")** │  **[русский (ru)](<../ru/Command_line_parameters_and_environment_variables.md> "Command line parameters and environment variables/ru")** │    
****

On most (interactive) operating systems [programs](<Program.md> "Program") can be started via a [command line interface](<Command-line_interface.md> "Command-line interface") (CLI) that allows supplying the program with additional data. The [system](<System_unit.md> "System unit") (and [`objPas`](<Object_Pascal.md> "Object Pascal")) units provide basic [functions](<Function.md> "Function") in order to access command-line supplied data. 

## Contents

  * 1 Overview
    * 1.1 Terminology
    * 1.2 Basics
  * 2 Command line parameters
    * 2.1 The basics
    * 2.2 User friendly
      * 2.2.1 Check for a parameter
      * 2.2.2 Read the parameter value
      * 2.2.3 Checking parameters for validity
  * 3 Environment variables



## Overview

Although this page speaks of _command-line_ parameters and environment variables, an actual command line interface (CLI) is not a requirement to use such data, nor to use the routines and terminology described here. However, since a CLI is more tangible the following instructions mainly to refer to a shell’s user experience. 

### Terminology

Option
    Options are yes/no flags. They usually assume the form `‑‑dryrun`/`‑‑no‑dryrun` (dryrun enabled or disabled respectively). Options are also called “switches”.
Parameter
    Parameters are key-value-tuples. On the command line they look like: `‑‑processors 4`.
Argument
    Arguments are simple words. On the command line they are usually separated by spaces (confer the IFS variable in some shells). In a narrow sense _arguments_ are all supplied words that are neither options or (part of) parameters. In general, though, arguments are all words on the command line not being interpreted in any way.
Environment variable
    Environment variables refer to a labeled piece of storage in the environment. They are name-value-pairs.

Note, the CLI/OS provides the program only with arguments it is supposed to get: For example, escaped newline characters, file redirection (pipes or to a file), and assignment of environment variables will not be forwarded to the program. 

### Basics

Since the notion of “option” and “parameters” are already kind of high-level, different styles exist (short options vs. long options), and the system unit aims to provide the _basic_ means to work with, all command line arguments are simply enumerated starting from zero. No interpretation is done. 

The function [`paramStr`](<https://www.freepascal.org/docs-html/rtl/system/paramstr.html>) returns the n-th argument on the command line. `ParamStr(0)` tries returning the name, and possibly full path to the executable program file. 

## Command line parameters

### The basics

A Pascal program can a access command line arguments by using the `paramStr` [function](<Function.md> "Function") in conjunction with [`paramCount`](<https://www.freepascal.org/docs-html/rtl/system/paramcount.html>). `ParamCount` returns the number of supplied arguments. 
    
    
     1program listArguments(input, output, stdErr);
     2{$mode objFPC}
     3var
     4	i: integer;
     5begin
     6	writeLn({$ifDef Darwin}
     7			// on Mac OS X return value depends on invocation method
     8			'This program was invoked via: ',
     9		{$else}
    10			// Turbo Pascal-compliant paramStr(0) returns location
    11			'This program is/was stored at: ',
    12		{$endIf}
    13		paramStr(0));
    14	
    15	for i := 1 to paramCount() do
    16	begin
    17		writeLn(i:2, '. argument: ', paramStr(i));
    18	end;
    19end.
    

Running this program (on a non-Mac OS Ⅹ platform) may look like this: 
    
    
    $ ./listArguments foo bar able
    This program is/was stored at: /tmp/listArguments
     1. argument: foo
     2. argument: bar
     3. argument: able
    

The [RTL](<RTL.md> "RTL")’s system unit provides a `paramStr` version that returns [`shortString`s](<Shortstring.md> "Shortstring"). Due to their implementation `shortString`s are capped at 255 characters. However the user may supply longer command line arguments. To access them the [`objPas` unit](<https://www.freepascal.org/docs-html/rtl/objpas/index.html>) redefines `paramStr` that returns [`ansiString`s](<Ansistring.md> "Ansistring") instead which do not have this length limitation. The `objPas` unit is automatically included in the [`{$mode objFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC") (line 2) and [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi") compiler modes. 

The distributed `paramStr` function tries to be [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal")-compliant. In TP `paramStr(0)` returns the location of the program. However, the operating system needs to support that. Most notably, on Mac OS Ⅹ the value of `paramStr(0)` depends on the invocation method, how the application is being started. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Since `paramStr(0)` relies on OS functionality, it is not suitable for [cross-platform programs](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide").

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** On Unixoid platforms it is impossible to accurately define the location of a file merely by a string. There could be multiple hard-links pointing to the same inode, the file could have already been deleted, parts of the file path might have changed, beside other details. For these reasons, `paramStr(0)` is useless on Unixoid platforms.

The [`getOpts` unit](<https://www.freepascal.org/docs-html/rtl/getopts/index.html>) provides a few other routines for program invocation following a certain style. 

### User friendly

A good program should give a help message when invoked with the wrong parameters and it should follow a common way of giving parameters. The [unit `custApp`](<https://www.freepascal.org/docs-html/fcl/custapp/index.html>) that comes with FPC provides the [`TCustomApplication` class](<https://www.freepascal.org/docs-html/fcl/custapp/tcustomapplication.html>), which provides functions to easily check and read parameters. Of course you can still access the parameters directly via `paramStr` and `paramCount`. 

Every [LCL](<LCL.md> "LCL") application uses this automatically. The Application object is a TCustomApplication. 

If you want to write a non LCL program, then create in [Lazarus](<Lazarus.md> "Lazarus") a new project of type “Console Application”. This will create a project1.lpr with some nice goodies, that almost all programs need. Go to the `doRun` method. 

#### Check for a parameter

With TCustomApplication you can access parameters by name. For example your program should print a help text when the user gave the common help parameter _-h_. The _-h_ is a short option. The long form is the _\--help_. To test whether the user called the program with _-h_ or _\--help_ you can use: 
    
    
    if hasOption('h', 'help') then
    begin
    	writeHelp;
    	halt;
    end;
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** In an LCL form you must prepend `application.` in front of `hasOption`.

For example: 
    
    
    if application.hasOption('h', 'help') then
    begin
    	writeHelp;
    	halt;
    end;
    

If you only want to support the short option, use: 
    
    
    if hasOption('h', '') then
    

If you only want to support the long option, use: 
    
    
    if hasOption('help') then
    

#### Read the parameter value

Each parameter can be given a value. For example: 
    
    
    $ project1 -f filename
    

or in the long form: 
    
    
    $ project1 --file=filename
    

In order to retrieve `filename` use: 
    
    
    writeLn('f=', getOptionValue('f', 'file'));
    

Note: if you get the error message “`Option at position 1 needs an argument : f.`” then you forgot to add the option in the [`checkOptions`](<https://www.freepascal.org/docs-html/fcl/custapp/tcustomapplication.checkoptions.html>) call. 

#### Checking parameters for validity

Command line parameters are free text, so the user can easily type errors. Checking the syntax of the parameters is therefore mandatory. You can use the [CheckOptions](<http://lazarus-ccr.sourceforge.net/docs/fcl/custapp/tcustomapplication.checkoptions.html> "doc:fcl/custapp/tcustomapplication.checkoptions.html") method for this: 

You can define, what parameters are allowed, which ones ones need a parameter and in case of a syntax error you can get an error message plus the options that were wrong to print helpful and detailed errors. 

Examples: 
    
    
    errorMsg := checkOptions('hf:', 'help file:');
    

This allows passing short options `‑f value` and `‑h`. It allows passing long options `‑‑help` or `‑‑file=filename`. It does not allow `‑‑help` with a value, nor `‑‑file` without a value. 

A Parameter Example: 
    
    
    procedure TMainForm.FormShow(Sender: TObject);
    var
        I: Integer;
        Params : TStringList
        LongOpts : array [1..2] of string = ('debug-sync', 'config-dir:');
    begin
        Params := TStringList.Create;
        try
            Application.GetNonOptions('hgo:', LongOpts, Params);
            for I := 0 to Params.Count -1 do
                debugln('Extra Param ' + inttostr(I) + ' is ' + Params[I]);  }
        finally
            FreeAndNil(Params);
        end;
    end;
    

This example finds parameters and does not mix them with command line switches or options. In this example, the app also accepts the switches --debug-sync and --config-dir=somedir but they are not reported here. 

Note that Application.GetNonOptions() takes the long options as an array but Application.CheckOptions takes the same data but as a string. Bit sad! 

## Environment variables

The [`sysUtils` unit](<sysutils.md> "sysutils") defines three basic functions in order to retrieve environment variables. 

  * [`getEnvironmentVariableCount`](<https://www.freepascal.org/docs-html/rtl/sysutils/getenvironmentvariablecount.html>) returns the total number of environment variables
  * [`getEnvironmentString`](<https://www.freepascal.org/docs-html/rtl/sysutils/getenvironmentstring.html>) returns an environment variable by its index
  * [`getEnvironmentVariable`](<https://www.freepascal.org/docs-html/rtl/sysutils/getenvironmentvariable.html>) returns an environment variable by its key



Example: 
    
    
     1program environmentVariablesList(input, output, stdErr);
     2uses
     3	sysUtils;
     4var
     5	i: integer;
     6begin
     7	for i := 0 to getEnvironmentVariableCount() - 1 do
     8	begin
     9		writeLn(getEnvironmentString(i));
    10	end;
    11end.
    

It is also possible to load all environment variables to a [`tStringList` object](<https://www.freepascal.org/docs-html/rtl/classes/tstringlist.html>) and access to name-value pair easily. String lists automatically take care of the separator character which is by defaut is an [equal sign (`'='`)](<=.md> "="). 
    
    
     1program environmentVariablesSorted(input, output, stdErr);
     2{$mode objFPC}
     3uses
     4	classes, sysUtils;
     5var
     6	i: integer;
     7	environmentVariables: tStringList;
     8begin
     9	environmentVariables := tStringList.create();
    10	
    11	try
    12		// load all variables to string list
    13		for i := 0 to getEnvironmentVariableCount() - 1 do
    14		begin
    15			environmentVariables.append(getEnvironmentString(i));
    16		end;
    17		
    18		environmentVariables.sort;
    19		for i := 0 to environmentVariables.count - 1 do
    20		begin
    21			writeLn(environmentVariables.names[i]:20,
    22				' = ',
    23				environmentVariables.valueFromIndex[i]);
    24		end;
    25	finally
    26		environmentVariables.free;
    27	end;
    28end.
    

There is also a `tCustomApplication` [method](<Method.md> "Method") available to get all environment variables as a [`tStringList`](<TStringList.md> "TStringList") at once. Then the pattern may look like this: 
    
    
    var
    	environmentVariables: tStringList;
    begin
    	environmentVariables := tStringList.create();
    	try
    		application.getEnvironmentList(environmentVariables);
    		…
    	finally 
    		environmentVariables.free;
    	end;
    end;
    
  *[IFS]: internal field separator

---

_Source: [https://wiki.freepascal.org/Command_line_parameters_and_environment_variables](https://web.archive.org/web/20240421215115/https://wiki.freepascal.org/Command_line_parameters_and_environment_variables)_
