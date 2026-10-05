# Dynamically loading headers

## Contents

  * 1 Abstract
  * 2 Pro
  * 3 Contra
  * 4 Suggestion w/o compiler modifications
    * 4.1 Framework
    * 4.2 Naming conventions
    * 4.3 Header implementation
      * 4.3.1 foobar.pas
      * 4.3.2 foobardyn.pas
      * 4.3.3 foobar.inc
    * 4.4 Usage
    * 4.5 Notes
  * 5 Solution of Delphi 2010 (modifying compiler)
  * 6 Others



## Abstract

Currently, most library headers in fpc/packages support only static loading. Some exceptions are mysql, ibase, sqlite (and some other database headers). The idea is now to add dynamic loading support for other headers. This page should provide some place to list pro/contra, implementation details, etc... 

Since dynamic loading is not supported by the compiler directly, it's required to define some framework, so that all headers are "implemented" in an uniform way and can be used easily. 

## Pro

  * loading libraries at runtime for plugin support



## Contra

  * compatibility breaker (using dynamic loading is just an option, that should be choosen carefully)
  * may produce strange errors, if wrong versions of libraries are loaded.



## Suggestion w/o compiler modifications

### Framework

dynlibs.pas 
    
    
    type
      PLibHandler = ^TLibHandler;
       
      TLibEventLoading = function(User: Pointer; Handler: PLibHandler): Boolean;
      TLibEventUnloading = procedure(Handler: PLibHandler);
        
      PPLibSymbol = ^PLibSymbol;
      PLibSymbol = ^TLibSymbol;
      TLibSymbol = record
        pvar: PPointer;  { pointer to Symbol variable }
        name: String;    { name of the Symbol }
        weak: Boolean;   { no error if this symbol don't exist }
      end;
      
      TLibHandler = record
        InterfaceName: String;                { abstract name of the library }
        Defaults     : array of String;       { list of default library filenames }
        Filename     : String;                { handle of the current loaded library }
        Handle       : TLibHandle;            { filename of the current loaded library }
        Loading      : TLibEventLoading;      { loading event, called after the unit is loaded }
        Unloading    : TLibEventUnloading;    { unloading event, called before the unit is unloaded }
        SymCount     : Integer;               { number of symbols }
        Symbols      : PLibSymbol;            { symbol address- and namelist }
        ErrorMsg     : String;                { last error message }
        RefCount     : Integer;               { reference counter }
      end;
        
        
    { handler definition }
    function LibraryHandler(const InterfaceName: String; const DefaultLibraries: array of String;
      const Symbols: PLibSymbol; const SymCount: Integer; const AfterLoading: TLibEventLoading = nil;
      const BeforeUnloading: TLibEventUnloading = nil): TLibHandler;
    
    { initialization/finalization }
    function TryInitializeLibrary(var Handler: TLibHandler; const LibraryNames: array of String;
      const User: Pointer = nil; const NoSymbolErrors: Boolean = False): Integer;
    function TryInitializeLibrary(var Handler: TLibHandler; const LibraryName: String = '';
      const User: Pointer = nil; const NoSymbolErrors: Boolean = False): Integer;
    function InitializeLibrary(var Handler: TLibHandler; const LibraryNames: array of String;
      const User: Pointer = nil; const NoSymbolErrors: Boolean = False): Integer;
    function InitializeLibrary(var Handler: TLibHandler; const LibraryName: String = '';
      const User: Pointer = nil; const NoSymbolErrors: Boolean = False): Integer;
    function ReleaseLibrary(var Handler: TLibHandler): Integer;
    
    { errors }
    procedure AppendLibraryError(var Handler: TLibHandler; const Msg: String);
    function GetLastLibraryError(var Handler: TLibHandler): String;
    procedure RaiseLibraryException(var Handler: TLibHandler);
    
    { symbol load/clear }
    function LoadLibrarySymbols(const Lib: TLibHandle; const Symbols: PLibSymbol; const Count: Integer;
      const ErrorSym: PPLibSymbol = nil): Boolean;
    procedure ClearLibrarySymbols(const Symbols: PLibSymbol; const Count: Integer);
    

### Naming conventions

For the dynamic headers the suffix "dyn" is added 

  * static: foobar.pas
  * dynamic: foobardyn.pas



### Header implementation

#### foobar.pas
    
    
    unit foobar;
      
    {$i foobar.inc}
      
    end.
    

#### foobardyn.pas
    
    
    unit foobardyn;
      
    {$DEFINE LOAD_DYNAMICALLY}
    {$i foobar.inc}
      
    end.
    

#### foobar.inc
    
    
    {$mode objfpc}{$H+}
    {$MACRO ON}
    // other possible compiler flags
      
    interface
      
    uses
      ctypes, dynlibs;
     
    {$IFDEF UNIX}
      {$DEFINE extdecl:=cdecl}
      const
        foobarlib = 'libfoobar.'+sharedsuffix;
        foobarvlib = foobarlib+'.0.9.9';
    {$ENDIF}
    {$IFDEF WINDOWS}
      {$DEFINE extdecl:=stdcall}
      const
        foobarlib = 'libfoobar.dll';
        foobarvlib = foobarlib;
    {$ENDIF}
      
    {$IFDEF LOAD_DYNAMICALLY}
      {$DEFINE D}
    {$ELSE}
      {$DEFINE S}
    {$ENDIF}
      
    // header stuff
      
    {$IFDEF S}function{$ELSE}var{$ENDIF}foobar_dosomething{$IFDEF D}: function{$ENDIF}(a:cint; b:cdouble): cint; extdecl;{$IFDEF S}external foobarlib;{$ENDIF}
    // more external functions
      
      
    {$IFDEF LOAD_DYNAMICALLY}
    function InitializeFoobar(const LibraryName: String = ''): Integer;
    function TryInitializeFoobar(const LibraryName: string = ''): Integer;
    function ReleaseFoobar: Integer;
      
    var
      FoobarLibrary: TLibHandler;
    {$ENDIF LOAD_DYNAMICALLY}
      
    implementation
      
    {$IFDEF LOAD_DYNAMICALLY}
    const
      foobar_symbols: array[0..X] of TLibSymbol = (
        (pvar:@foobar_dosomething; name:'foobar_dosomething'; weak:false),  
        // more external functions
      );
      
    function foobar_init(User: Pointer; Handler: PLibHandler): Boolean;
    var
      args: PMysqlArgs absolute User;
    begin
      // do some initialization calls
    end;
      
    procedure foobar_end(Handler: PLibHandler);
    begin
      // do some calls before library is unloaded
    end;
      
    function TryInitializeFoobar(const LibraryName: string): Integer;
    begin
      Result := TryInitializeLibrary(FoobarLibrary, LibraryName);
    end;
      
    function InitializeFoobar(const LibraryName: String): Integer;
    begin
      Result := InitializeLibrary(FoobarLibrary, LibraryName);
    end;
      
    function ReleaseFoobar: Integer;
    begin
      Result := ReleaseLibrary(FoobarLibrary);
    end;
      
    initialization
      FoobarLibrary := LibraryHandler('foobar', [foobarlib,foobarvlib, {moreversions} ], @foobar_symbols,
        Length(foobar_symbols), @foobar_init, @foobar_end);
    {$ENDIF}
    

### Usage
    
    
    program foobar_user;
      
    uses
      foobardyn;
     
    begin
      InitializeFoobar;  // if no paramter is submitted, the default library is loaded
      foobar_dosomething(2, 1.3);
      ReleaseFoobar;
    end.
    

### Notes

## Solution of Delphi 2010 (modifying compiler)

In Delphi 2010 they introduced "delayed" loading 
    
    
    function GetFoobar: Integer; external 'foobar.dll' delayed;
    

More Info: <http://docwiki.embarcadero.com/RADStudio/en/Libraries_and_Packages#Delayed_Loading>

## Others

Example implementations can be found in 

  * mysql: packages/mysql/src/mysql.inc
  * sqlite: packages/sqlite/src/sqlite3.inc
  * zorba: packages/zorba/src/zorba.inc

---

_Source: [https://wiki.freepascal.org/Dynamically_loading_headers](https://web.archive.org/web/20250416154252/https://wiki.freepascal.org/Dynamically_loading_headers)_
