# pas2js makestub

## Contents

  * 1 The pas2js stub generator
    * 1.1 makestub utility
      * 1.1.1 Synopsis
      * 1.1.2 Command-line
      * 1.1.3 Config file
        * 1.1.3.1 Example
    * 1.2 libstub library
      * 1.2.1 Synopsis
      * 1.2.2 Exported functions
  * 2 Navigation



# The pas2js stub generator

## makestub utility

### Synopsis

The _makestub_ utility converts a Pascal import unit for importing Pascal classes to a unit that is compileable by Delphi 

### Command-line

command-line makestub usage is quite simple: 
    
    
    makestub -i jsunit.pas -o delphiunit.pas
    

you can add defines into the mix 
    
    
    makestub -d DELPHI -d DEBUG -i jsunit.pas -o delphiunit.pas
    

You can also include a header in the generated unit: 
    
    
    makestub -h copyright.pas -i jsunit.pas -o delphiunit.pas
    

It does not put comments around the header, the header file is inserted as-is, at the start of the generated file. 

_makestub_ currently has some problems with forward-defined classes. There is an option to give a list of classes that must be defined forward: 
    
    
    makestub -f TClassA,TClassB -i jsunit.pas -o delphiunit.pas
    

The special word 'all' will define all classes as forward: 
    
    
    makestub -f all -i jsunit.pas -o delphiunit.pas
    

There is the possibility of using a config file (.ini) to set some options: 
    
    
    makestub -c myconfig.ini -i jsunit.pas -o delphiunit.pas
    

Options on the command-line override options set in the config file. 

### Config file

The makestub program supports a config file. This is a .INI file which has 1 section called _config_ , which can contain the following keys : 

  * _input_ : The name of an input file (corresponds to -i).
  * _output_ : The name of an output file (corresponds to -o)
  * _header_ : the name of a header file to insert (corresponds to -h)
  * _defines_ : a list of defines (corresponds to -d)
  * _extra_ : a list of units to insert at the start of the uses clause. Note that the _jsdelphisystem_ unit is always inserted, and that you can use _all_ to indicate that all classes must be eclared forward.
  * _includepaths_ : include paths where to search for include files.
  * _forwardclasses_ : a comma-separated list of classes that must be declared forward.
  * _indentsize_ : number of spaces to use for indent (default 2)
  * _addlinenumber_ : Boolean (0/1) each line is prefixed with a comment containing the line number.
  * _addsourcelinenumber_ : Boolean (0/1) each line is prefixed with a comment containing the line number of the source file that was used to create the current line.
  * _linenumberwidth_ : number of digits to use for line numbers (default 4)



#### Example

Here is an example config file: 
    
    
     [config]
     input=jsunit.pas
     output=delphiunit.pas
     header=copyright.pas
     indentsize=4
     addlinenumber=1
     addsourcelinenumber=1
     linenumberwidth=5
     forwardclasses=TClassA,TClassB
     extra=mysecretunit
    

## libstub library

### Synopsis

The _libstub_ library is the library equivalent of the makestub command-line tool. 

It can be used in Delphi to automate conversion of pas2js units. The stub creator is written in Lazarus/Free Pascal and therefor cannot be compiled in Delphi, since it uses a lot of units not available in pas2js. 

### Exported functions

The following functions are exported by the library creator. Given the above explanations about config files and command-line options, their usage should be clear. 
    
    
    Type
      PStubCreator = Pointer;  
    
    Function GetStubCreator : PStubCreator; stdcall;
    Procedure FreeStubCreator(P : PStubCreator); stdcall;
    Procedure SetStubCreatorInputFileName(P : PStubCreator; AFileName : PAnsiChar); stdcall;
    Procedure SetStubCreatorConfigFileName(P : PStubCreator; AFileName : PAnsiChar); stdcall;
    Procedure SetStubCreatorOutputFileName(P : PStubCreator; AFileName : PAnsiChar); stdcall;
    Procedure SetStubCreatorHeaderFileName(P : PStubCreator; AFileName : PAnsiChar); stdcall;
    Procedure AddStubCreatorDefine(P : PStubCreator; ADefine : PAnsiChar); stdcall;
    Procedure AddStubCreatorForwardClass(P : PStubCreator; AForwardClass : PAnsiChar); stdcall;
    Procedure SetStubCreatorHeaderContent(P : PStubCreator; AContent : PAnsiChar); stdcall;
    Procedure SetStubCreatorOuputCallBack(P : PStubCreator; AData : Pointer; ACallBack : TWriteCallBack); stdcall;
    function ExecuteStubCreator(P : PStubCreator) : Boolean; stdcall;
    Procedure GetStubCreatorLastError(P : PStubCreator; AError : PAnsiChar;
      Var AErrorLength : Longint; AErrorClass : PAnsiChar; Var AErrorClassLength : Longint); stdcall;
    

# Navigation

  * Back to [pas2js](<pas2js.md> "pas2js")
  * Back to [lazarus pas2js integration](<lazarus_pas2js_integration.md> "lazarus pas2js integration")

---

_Source: [https://wiki.freepascal.org/pas2js_makestub](https://web.archive.org/web/20240304082426/https://wiki.freepascal.org/pas2js_makestub)_
