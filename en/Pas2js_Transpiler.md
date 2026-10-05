# Pas2js Transpiler

## Contents

  * 1 About pas2js
  * 2 Command line parameters
  * 3 Delphi and ObjFPC mode
    * 3.1 Delphi mode
    * 3.2 ObjFPC mode
  * 4 Translating modules
  * 5 Translating variables
  * 6 Translating string
  * 7 Translating resourcestrings
  * 8 Translating currency
  * 9 Translating Types
  * 10 Translating pointer
  * 11 Translating record type
  * 12 Translating generics
  * 13 Translating functions
  * 14 Translating passing a parameter by reference
  * 15 Translating nested functions
  * 16 Translating for-loops
  * 17 Translating repeat..until
  * 18 Translating while..do
  * 19 Translating case..do
  * 20 Translating with..do
  * 21 Translating enums
  * 22 Translating sets
  * 23 Translating array type
  * 24 Translating class type
  * 25 Translating class-of type
  * 26 Translating TObject.Free
  * 27 Translating class interfaces
  * 28 Translating helpers
  * 29 Translating attributes
  * 30 Translating try..finally
  * 31 Translating try..except
  * 32 Translating enumerators
  * 33 Translating function types
  * 34 Translating anonymous functions
  * 35 Translating var modifier absolute
  * 36 Translating assert()
  * 37 Dispatch messages
  * 38 Calling JavaScript from Pascal
  * 39 The asm block
  * 40 The procedure modifier assembler
  * 41 The procedure modifier external
  * 42 The procedure modifier varargs
  * 43 The var modifier external
  * 44 The external modifier of class members
  * 45 External classes
  * 46 External class as ancestor
  * 47 The JSValue type
  * 48 Accessing JS object properties with the bracket accessor
  * 49 RTTI - Run Time Type Information
  * 50 Async/AWait
  * 51 Compiler directives
  * 52 Numbers
  * 53 Other supported Pascal elements
  * 54 Not supported elements
  * 55 JavaScript Version
  * 56 Creating source maps
  * 57 See Also



## About pas2js

pas2js is a compiler/transpiler to translate programs written in Pascal (subset of Delphi/ObjFPC syntax) to JavaScript.  
The goal is to use strong typing, while still be able to use low level whenever you choose.  
The compiled Pascal functions can be used in DOM events or called by JavaScript.  
pas2js is written completely in FPC, runs on many platforms like Windows, Mac and Linux and more. It is built modular consisting of the following parts: 

  * file cache - loading, caching files, converting to UTF-8
  * file resolver - handling search paths, finding used units and include files
  * scanner - reading tokens, handling compiler directives like $IfDef and $Include
  * parser - reading the tokens, checking syntax, creating Pascal nodes
  * resolver - resolving references, type checking and checking duplicate identifiers
  * use analyzer - finding unused identifiers, emit hints and warning
  * converter - translating Pascal nodes into JavaScript nodes
  * compiler - handling config files, parameters, compiling recursively all used units, writes js
  * command line interface - a small wrapper to embed the compiler into a console program
  * library and interface - a small wrapper to embed the compiler into a library



Each part is tested separately and is used by other FPC tools as well. For example the scanner and parser are used by fpdoc too. Thus they are tested and extended by other programmers, reducing greatly the work for developing pas2js. Consistency is kept by several test suites, containing thousands of tests. 

Note: The modular structure allows to compile any parts or the whole compiler into an IDE addon (not yet started). 

## Command line parameters

Most parameters work the same as their FPC equivalent. pas2js has some options of its own (see -J options). 
    
    
     
    Options:
    Put + after a boolean switch option to enable it, - to disable it
      @<x>   : Read compiler options from file <x> in addition to the default
                pas2js.cfg
      -B     : Rebuild all
      -d<x>  : Defines the symbol <x>. Optional: -d<x>:=<value>
      -i<x>  : Write information and halt. <x> is a combination of the following:
        -iD  : Write compiler date
        -iSO : Write compiler OS
        -iSP : Write compiler host processor
        -iTO : Write target platform
        -iTP : Write target processor
        -iV  : Write short compiler version
        -iW  : Write full compiler version
        -ic  : Write list of supported JS processors usable by -P<x>
        -im  : Write list of supported modeswitches usable by -M<x>
        -io  : Write list of supported optimizations usable by -Oo<x>
        -it  : Write list of supported targets usable by -T<x>
        -iJ  : Write list of supported JavaScript identifiers -JoRTL-<x>
      -C<x>  : Code generation options. <x> is a combination of the following
                letters:
        o    : Overflow checking of integer operations
        r    : Range checking
        R    : Object checks. Verify method calls and object type casts.
      -F...   Set file names and paths:
       -Fe<x>: Redirect output to file <x>. UTF-8 encoded.
       -FE<x>: Set main output path to <x>
       -Fi<x>: Add <x> to include paths
       -FN<x>: add <x> to namespaces. Namespaces with trailing - are removed.
                Delphi calls this flag "unit scope names".
       -Fu<x>: Add <x> to unit paths
       -FU<x>: Set unit output path to <x>
      -I<x>  : Add <x> to include paths, same as -Fi
      -J...  Extra options of pas2js
       -Ja<x>: Append JS file <x> to main JS file. E.g. -Jamap.js. Can be given
                multiple times. To remove a file name append a minus, e.g.
                -Jamap.js-.
       -Jc   : Write all JavaScript concatenated into the output file
       -Je<x>: Encode messages as <x>.
         -Jeconsole: Console codepage. This is the default.
         -Jesystem : System codepage. On non Windows console and system are the
                same.
         -Jeutf-8  : Unicode UTF-8. Default when using -Fe.
         -JeJSON   : Output compiler messages as JSON. Logo etc are outputted
                as-is.
       -Ji<x>: Insert JS file <x> into main JS file. E.g. -Jirtl.js. Can be given
                multiple times. To remove a file name append a minus, e.g.
                -Jirtl.js-.
       -Jl   : lower case identifiers
       -Jm   : generate source maps
         -Jmsourceroot=<x>: use x as "sourceRoot", prefix URL for source file
                names.
         -Jmbasedir=<x>: write source file names relative to directory x, default
                is map file folder.
         -Jminclude: include Pascal sources in source map.
         -Jmabsolute: store absolute filenames, not relative.
         -Jmxssiheader: start source map with XSSI protection )]}', default.
         -Jm-: disable generating source maps
       -Jo<x>: Enable or disable extra option. The x is case insensitive:
         -JoSearchLikeFPC: search source files like FPC, default: search case
                insensitive.
         -JoUseStrict: add "use strict" to modules, default.
         -JoCheckVersion-: do not add rtl version check, default.
         -JoCheckVersion=main: insert rtl version check into main.
         -JoCheckVersion=system: insert rtl version check into system unit init.
         -JoCheckVersion=unit: insert rtl version check into every unit init.
         -JoRTL-<y>=<z>: set RTL identifier y to value z. See -iJ.
       -Jr<x> Control writing of resource string file
         -Jrnone: Do not write resource string file
         -Jrunit: Write resource string file per unit with all resource strings
         -Jrprogram: Write resource string file per program with all used
                resource strings in program
       -JR<x> Control writing of linked resources
         -JRnone: Skip resource directives
         -JRjs: Write resources in Javascript structure
         -JRhtml[=filename] : Write resources as preload links in HTML file 
                (default is projectfile-res.html)
       -Jpcmd<command>: Run postprocessor. For each generated js execute command
                passing the js as stdin and read the new js from stdout. This
                option can be added multiple times to call several postprocessors
                in succession.
       -Ju<x>: Add <x> to foreign unit paths. Foreign units are not compiled.
       -JU: This pas2js does not support PCU files
      -l     : Write logo
      -M<x>  : Set language mode or enable/disable a modeswitch
        -MDelphi: Delphi 7 compatibility mode
        -MObjFPC: FPC's Object Pascal compatibility mode (default)
        -M<x>  : enable or disable modeswitch, see option -im
        Each mode (as listed above) enables its default set of modeswitches.
        Other modeswitches are disabled and need to be enabled one by another.
      -NS<x> : obsolete: add <x> to namespaces. Same as -FN<x>
      -n     : Do not read the default config files
      -o<x>  : Change main JavaScript file to <x>, "." means stdout
      -O<x>  : Optimizations:
        -O-  : Disable optimizations
        -O1  : Level 1 optimizations (quick and debugger friendly)
        -O2  : Level 2 optimizations (Level 1 + not debugger friendly)
        -Oo<x>: Enable or disable optimization. The x is case insensitive:
          -OoEnumNumbers[-]: write enum value as number instead of name. Default
                in -O1.
          -OoRemoveNotUsedPrivates[-]: Default is enabled
          -OoRemoveNotUsedDeclarations[-]: Default enabled for programs with -Jc
          -OoRemoveNotUsedPublished[-] : Default is disabled
          -OoShortRefGlobals[-]: Insert JS local var for types, modules and
                static functions. Default enabled in -O2
      -P<x>  : Set target processor. Case insensitive:
        -Pecmascript5: default
        -Pecmascript6
      -S<x>  : Syntax options. <x> is a combination of the following letters:
        2    : Same as -Mobjfpc (default)
        a    : Turn on assertions
        c    : Support operators like C (*=,+=,/= and -=)
        d    : Same as -Mdelphi
        j    : Allows typed constants to be writeable (default)
        m    : Enables macro replacements
      -SI<x>  : Set interface style to <x>
        -SIcom  : COM, reference counted interface (default)
        -SIcorba: CORBA interface
      -T<x>  : Set target platform
        -Tbrowser: default
        -Tnodejs : add pas.run(), includes -Jc
        -Telectron: experimental
        -Tmodule : add pas.run(), includes -Jc
      -u<x>  : Undefines the symbol <x>
      -v<x>  : Be verbose. <x> is a combination of the following letters:
        e    : Show errors (default)
        w    : Show warnings
        n    : Show notes
        h    : Show hints
        i    : Show info
        l    : Show line numbers, needs -vi
        a    : Show everything
        0    : Show nothing (except errors)
        b    : Show file names with full path
        c    : Show conditionals
        t    : Show tried/used files
        d    : Show debug notes and info, enables -vni
        q    : Show message numbers
        x    : Show used tools
        v    : Write pas2jsdebug.log with lots of debugging info
        z    : Write messages to stderr, -o. still uses stdout.
      -vm<x>,<y>: Do not show messages numbered <x> and <y>.
      -?     : Show this help
      -h     : Show this help
    
    Environment variable PAS2JS_OPTS is parsed after default config and before
                command line parameters.
    

## Delphi and ObjFPC mode

### Delphi mode

  * Defines macro _DELPHI_
  * Assigning a function to a function type variable does not require the @ operator. For example, you can write either _OnGetThing:=GetValue;_ or _OnGetThing:=@GetValue;_.
  * A function type variable reference without brackets is treated as a call. For example: If _OnGetThing_ is a variable of type _function: integer_ you can write: _If OnGetThing=3 then ;_.
  * You must use the @@ operator to get the procedure address (i.e. JS reference) of a procedure type variable. For example instead of _If OnClick=nil then ;_ you must use _if @@OnClick=nil then ;_.
  * Every procedure/method overload needs the 'overload' modifier.



### ObjFPC mode

This the default mode of pas2js and is generally more strict than the Delphi mode, and allows some more operations. 

  * Defines macro _OBJFPC_
  * Assigning a function to a function type variable requires the @ operator. For example: _OnGetThing:=@GetValue;_.
  * A function type variable always needs brackets to be called. For example: If _OnGetThing_ is a variable of type _function: integer_ then this is allowed: _If OnGetThing()=3 then ;_. While this gives an error: _If OnGetThing=3 then ;_.
  * You can compare a procedure type with _nil_. For example _If OnClick=nil then ;_.
  * You can compare a procedure type with a procedure address (i.e. JS reference). For example _If OnClick=@OnFormClick then ;_.
  * The procedure modifier 'overload' can be omitted when all overloads are in one scope, e.g. a unit or a class. And if one procedure has such modifier all procedures with same name and in same scope are overloads as well.



## Translating modules

A Pascal Program is translated into the following JavaScript structure: 

Pascal  | JavaScript Structure, not code!   
---|---  
      
    
     Program <unitname>;
     Implementation
       [implementation section]
     Begin
       [main code]
     End.
    

| 
    
    
     pas.<program>={
       [implementation section],
       $main: function() {
         [main code]
       }
     };
       
  
A Pascal Unit is translated into the following JavaScript structure: 

Pascal  | JavaScript Structure, not code!   
---|---  
      
    
    Unit <unitname>;
    Interface
      [interface section]
    Implementation
      [implementation section]
    Initialization
      [initialization section]
    End.
    

| 
    
    
    pas.<unitname>={
      [interface section],
      $impl: {
        [implementation section],
      },
      $init: function() {
        [initialization section]
      }
    };
      
  
Note: The **finalization** section is not supported by pas2js.  


To create and initialize the units in topological order the compiler translates an Unit to the following JavaScript code:  Pascal  | JavaScript   
---|---  
      
    
    Unit <unitname>;
    Interface
      [interface section]
    Implementation
      [implementation section]
    Initialization
      [initialization section]
    End.
    

| 
    
    
    rtl.module('<unitname>',
      ['system',...other used units of the interface section...],
      function(){
        var $mod = this;
        var $impl = $mod.$impl;
        [interface section]
        $mod.$implcode = function(){
          [implementation section]
        }
        $mod.$init = function(){
          [initialization section]
        };
      },
      [...used units of the implementation section]
      };
      
  
Here is a more detailed example to make it more clear: 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Uses Sysutils;
    var
      dIntf: double;
      sIntf: string = 'abc';
    procedure MyIntfProc;
    Implementation
    Uses Classes;
    Var dImpl:double;
    Procedure MyIntfProc;
    Begin
      dImpl:=dIntf;
    End;
    Procedure MyImplProc;
    Begin
      dImpl:=dIntf;
    End;
    Initialization
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System","SysUtils"],
    function(){
      var $mod = this;
      var $impl = $mod.$impl;
      this.dIntf = 0.0;
      this.sIntf = "abc";
      this.MyIntfProc = function(){
        $impl.dImpl = $mod.dIntf;
      };
      $mod.$implcode = function(){
        $impl.dImpl = 0.0;
        $impl.MyImplProc = function() {
          $impl.dImpl = $mod.dIntf;
        };
      }
      $mod.$init = function() {
      };
    },
    ["Classes"]);
      
  
Notes: 

  * Unit _System_ is always loaded implicitely.
  * References to other units are translated to full path. For example _TObject_ is translated to _pas.system.TObject_
  * References to dotted unitnames, aka units with namespaces are translated to _pas["namespace.unitname"]_.



  


## Translating variables

Variables are converted without type, because JavaScript lacks a clear type. They are however always initialized, which helps JavaScript engines to optimize. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Uses Classes,Forms;
    const
      c1:integer=3;
      c2 = 'abc';
      c3 = 234;
      c4 = 12.45;
      c5 = nil;
    var
      v1:string;
      v2,v3:double;
      v4:byte=0;
      v5:TForm;
      v6:TIdentMapEntry;
      v7:string='abc';
      v8:char='c';
      v9:array of byte;
    Implementation
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System","Classes","Forms"],
    function(){
      this.c1 = 3;
      this.c2 = "abc";
      this.c3 = 234;
      this.c4 = 12.45;
      this.c5 = null;
      this.v1 = "";
      this.v2 = 0.0;
      this.v3 = 0.0;
      this.v4 = 0;
      this.v5 = null;
      this.v6 = new pas.Classes.TIdentMapEntry();
      this.v7 = "abc";
      this.v8 = "c";
      this.v9 = [];
    },
    []);
      
  
Notes: 

  * Type casting a _boolean_ to _integer_ , gives _0_ for _false_ and _1_ for _true_.
  * Type casting an _integer_ to _boolean_ , gives _false_ for _0_ and _true_ otherwise.
  * A **char** is translated to a JS string, because JS lacks a native char type.
  * A **char** is a single JS char code. An UTF-16 codepoint can contain one or two **char**.
  * **Integers overflows** at runtime differ from Delphi/FPC, due to the double format. For example adding _var i: byte = 200; ... i:=i+100;_ will result in _i=300_ instead of _i=44_ as in Delphi/FPC. When range checking _{$R+}_ is enabled _i:=300_ will raise an ERangeError.
  * **type cast integer to integer** , e.g. _byte(aLongInt)_
    * with range checking enabled: error if outside range
    * without range checking: emulates the FPC/Delphi behaviour: e.g. _byte(value)_ translates to _value & 0xff_, _shortint(value)_ translates to _value & 0xff <<24 >> 24._
  * The **mod-operator** works 32-bit signed in JS.



## Translating string

Strings are translated to JavaScript strings. They are initialized with "" and are never **null**.  
There are no _ShortString, AnsiString or RawByteString_. _Unicodestring_ and _Widestring_ are alias of _String_.  
JavaScript strings are immutable, which means that changing a single character in a string, creates a new string. So a _s[2]:='c';_ is a slow operation in pas2js compared to Delphi/FPC.  
Although pas2js creates .js files encoded as UTF-8 with BOM, JavaScript strings are UTF-16 at runtime. Keep in mind that one UTF-16 codepoint can need two _char_ , and a visible glyph can need several codepoints. Same as in Delphi. 

## Translating resourcestrings

Resourcestrings are translated to JS objects with original (org) and current value. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    resourcestring
      rsCompiler = 'pas2js';
    var
      s:string;
    Implementation
    initialization
      s:=rsCompiler;
    End.
    

| 
    
    
    rtl.module("test1",["System"],function () {
      var $mod = this;
      this.s = "";
      $mod.$resourcestrings = {rsCompiler: {org: "pas2js"}};
      $mod.$init = function () {
        $mod.s = rtl.getResStr(pas.test1,"rsCompiler");
      };
    });
      
  
  


## Translating currency

_Currency_ in Delphi/FPC is an int64 with a factor of 10000. This is translated to a double with factor 10000 and truncated. 

  * _CurA := 1.12345_ -> _CurA = 11234_
  * _CurA + CurB_ -> _CurA + CurB_
  * _CurA * CurB_ -> _CurA * CurB/10000_
  * _CurA / CurB_ -> _Math.floor(CurA/CurB * 10000)_
  * _CurA ^^ CurB_ -> _Math.floor(Math.pow(CurA/10000,CurB/10000) * 10000)_
  * _Currency + Double_ -> _Currency + (Double*10000)_
  * _Double := Currency_ -> _Double = Currency/10000_
  * _Currency := Double_ -> _Currency = Math.floor(Double*10000)_
  * _JSValue := Currency_ -> _JSValue = Currency/10000_
  * Keep in mind that a double has only 54 bits for the number, so calculating values greater than 900,719,925,474 might give a different result than in Delphi/FPC. See SysUtils.MinCurrency/MaxCurrency



  


## Translating Types

JavaScript type design has no declarative form, except for object types (so-called prototypes). That's why all the derivatives from simple Pascal types can not be translated. The compiler ensures type safety at compile time though, which is a big plus for using Pascal.  
Complex Pascal types (classes, records, or arrays) are translated into JavaScript objects or arrays respectively.  


## Translating pointer

A _pointer_ is translated to a JS reference. It can be assigned a class, a class instance, a class-of, an array, a procedure var, a method var, a @proc address, a @method address, or a pointer of record. There is no pointer arithmetic, i.e. no p+1, and no typed pointers, except for pointer of record. You can find out its type using the functions _isArray_ , _isClass_ , _isClassRef_ , _isCallback_ , etc of unit _JS_. 

## Translating record type

A record is translated to a JavaScript object. 

Pascal  | JS Pas2js 1.3  | JS Pas2js 1.2   
---|---|---  
      
    
    Unit MyModule;
    Interface
    Type
      TMyRecord = Record
        i: integer;
        s: string;
        d: TDateTime;
      End;
    Var
      r, s: TMyRecord;
    Implementation
    Initialization
      r.i := 123;
      r:=s;
      if r=s then ;
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      var $mod = this;
      rtl.recNewT($mod, "TMyRecord", function() {
        this.i = 0;
        this.s = "";
        this.d = 0.0;
        this.$eq = function (b) {
          return (this.i == b.i) &&
             (this.s == b.i) && (this.d == b.d);
        };
        this.$assign = function (s) {
          this.i = s.i;
          this.s = s.s;
          this.d = s.d;
          return this;
        };
      };
      this.r = this.TMyRecord.$new();
      $mod.$init = function() {
        $mod.r.i=123;
        $mod.r.$assign($mod.s);
        if ($mod.r.$eq($mod.s)) ;
      },
    },
    []);
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      var $mod = this;
      this.TMyRecord = function(s) {
        if (s){
          this.i = s.i;
          this.s = s.s;
          this.d = s.d;
        } else {
          this.i = 0;
          this.s = "";
          this.d = 0.0;
        };
        this.$equal = function (b) {
          return (this.i == b.i) &&
            (this.s == b.i) && (this.d == b.d);
        };
      };
      this.r = new this.TMyRecord();
      $mod.$init = function() {
        $mod.r.i=123;
        $mod.r = new $mod.TMyRecord($mod.s);
        if ($mod.r.$equal($mod.s)) ;
      },
    },
    []);
      
  
  * The record variable creates a JavaScript object.
  * Variant records are not supported.
  * Supported: Assign, pass as argument, equal, not equal, array of record, pointer of record, const, default(), RTTI.
  * Advanced record (since pas2js 1.3): 
    * visibility private, strict private, public, default is public
    * methods, class methods (must be static like in Delphi/FPC)
    * class vars
    * const fields
    * property, class property, array property, default array property
    * sub types
    * constructor
    * class constructor
  * Not yet implemented: 
    * operator overloading
    * reference counted interfaces as fields
    * Interfaces as nested types
    * default non array property
  * Until Pas2js 1.2 when assigning a record it is cloned, creating a new JS object. Since Pas2js 1.3 only values are copied, keeping the object, so pointer of record is compatible.
  * Since record types are JS objects it is possible to typecast a record type to the JS Object, e.g. _TJSObject(TPoint)_. Note that you cannot typecast directly to a _TJSObject_ descendant. You can use _TJSWindow(TJSObject(aRecord))_.
  * A pointer of record is simply a reference. 
    * _p:=@r_ translates to _p=r_
    * _p^.x_ becomes _p.x_.
    * _New(PointerOfRecord)_ creates a new record
    * _Dispose(PointerOfRecord)_ Sets the variable to null if possible.
  * Passing a record to an untyped arguments (e.g. ''TObject.Dispatch(var Msg)'') passes the record JS object directly, not creating a temporary reference object.
  * Typecasting RecordType(UntypedArgument) returns the argument, i.e. no conversion.



## Translating generics

Generics are not translated, their specializations are. 

  * Specializations are created with a $ number postfix, for example **TList$G3**.
  * Their RTTI name has the full name, for example **TList <project1.TBird>**
  * Multiple specializations of _TList <word>_ creates only one specialization. That is true for the whole program.
  * Specializations are created in the unit, where the generic is defined, not where they are specialized.
  * Important: This means that a new specialization of _TList_ require a recompile of the js of unit _generics.collections_.



## Translating functions

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Function DoubleIt(n: integer): integer;
    Implementation
    Function DoubleIt(n: integer): integer;
    Begin
      Result:=2*n;
    End;
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      this.DoubleIt = function(n){
        Result = 0;
        Result = 2*n;
        return Result;
      };
    },
    []);
      
  
Notes: 

  * Local variables become local JavaScript variables: _var l = 0;_.
  * Local constants become JavaScript variables in the unit/program implementation section.
  * Local types are elevated to module.
  * Overloaded functions are given an unique name by appending $1, $2, ...
  * Supported: default values, const/var/out/default, FuncName:=



  


## Translating passing a parameter by reference

JavaScript lacks passing by reference. Instead a temporary object is created with a _get_ and _set_ function. That means changes within the procedure are immediately visible outside, compatible with Pascal. 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    Procedure DoubleIt(var n: integer);
    Begin
      n:=2*n;
    End;
    Function Doubling(n: integer): integer;
    Begin
      DoubleIt(n);
      Result:=n;
    End;
    Var
      i: integer = 7;
    Begin
      Doubling(i);
    End.
    

| 
    
    
    rtl.module("program",
    ["System"],
    function(){
      var $mod = this;
      this.i = 7;
      this.DoubleIt = function(n){
        n.set(2*n.get());
      };
      this.Doubling = function(n){
        var Result = 0;
        DoubleIt({
          get:function(){
            return n
          },
          set:function(v){
            n=v;
          }
        });
        Result = n;
        return n;
      };
      $mod.$main = function(){
        Doubling($mod.i);
      }
    },
    []);
      
  
When the passed value is from another context, the context is passed too: 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    Procedure DoubleIt(var n: integer);
    Begin
      n:=2*n;
    End;
    Var
      i: integer = 7;
    Begin
      DoubleIt(i);
    End.
    

| 
    
    
    rtl.module("program",
    ["System"],
    function(){
      var $mod = this;
      this.i = 7;
      this.DoubleIt = function(n){
        n.set(2*n.get());
      };
      $mod.$main = function(){
        DoubleIt({
            p:$mod,
            get:function(){
              return this.p.i
            },
            set:function(v){
              this.p.i=v;
            }
          });
      }
    },
    []);
      
  
Notes: 

  * Contrary to Delphi/FPC it is allowed to pass a property to a var/out parameter.



  


## Translating nested functions

A nested function is translated to a local variable. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Function SumNNumbers(n, Adder: integer): integer;
    Implementation
    Function SumNNumbers(n, Adder: integer): integer;
    
      Function Add(k: integer): integer;
      Begin
        if k=1 then
          Result:=1
        else
          Result:=Add(k-1)+Adder;
      End;
    
    Begin
      Result:=Add(n);
    End;
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      this.DoubleIt = function(n,Adder){
        Result = 0;
        var Add = function(k) {
          Result = 0;
          if (k==1) {
            Result = 1;
          } else {
            Result = Add(k-1)+Adder;
          }
          return Result;
        };
        Result = Add(n);
        return Result;
      };
    },
    []);
      
  
Note: You can assign a nested procedure to a procedure variable. A nested procedure of a method can be assigned to a method variable.  
JavaScript preserves the current local scope, including references to the local variables of parent functions. Local types and constants belong to the unit scope (singleton).  
When a method has nested functions, the compiler adds a local var _Self_. 

## Translating for-loops

The JavaScript for-loop executes the end expression every iteration, while Pascal only executes it once. Therefore a local variable is introduced. If the loop is not entered at all, the variable is not touched. If the loop was entered the variable contanis the last value. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Function SumNNumbers(n: integer): integer;
    Implementation
    Function SumNNumbers(n: integer): integer;
    Var
      i, j: integer;
    Begin
      j:=0;
      For i:=1 To n Do
      Begin
        j:=j+i;
      End;
      if i<1 then j:=1;
      Result:=j;
    End;
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      this.SumNNumbers=function(n){
        Result = 0;
        j = 0;
        for (var $l1 = 1, $le2 = n; $l1 <= $le2; $l1++) {
          i = $l1;
          j = j + i;
        };
        if (i<1) j=1;
        Result = j;
        return Result;
      };
    },
    []);
      
  
Note: The after-loop decrement is only added if _i_ is read after the loop.  


## Translating repeat..until

The _repeat..until_ is translated to a _do{}while()_. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Function SumNNumbers(n: integer): integer;
    Implementation
    Function SumNNumbers(n: integer): integer;
    Var
      i, j: integer;
    Begin
      j:=0;
      i:=0;
      Repeat
        i:=i+1;
        j:=j+i;
      Until i>=n;
      Result:=j;
    End;
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      this.SumNNumbers=function(n){
        Result = 0;
        j = 0;
        i = 0;
        do{
          i = (i + 1);
          j = (j + i);
        } while (!(i>=n));
        Result = j;
        return Result;
      };
    },
    []);
      
  
  


## Translating while..do

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Function SumNNumbers(n: integer): integer;
    Implementation
    Function SumNNumbers(n: integer): integer;
    Var
      i, j: integer;
    Begin
      j:=0;
      i:=0;
      While i<n Do Begin
        i:=i+1;
        j:=j+i;
      End;
      Result:=j;
    End;
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      this.SumNNumbers=function(n){
        var Result = 0;
        var j = 0;
        var i = 0;
        while(i<n){
          i = (i + 1);
          j = (j + i);
        };
        Result = j;
        return Result;
      };
    },
    []);
      
  
  


## Translating case..do

Although JavaScript has something similar in form of the "switch" statement, it lacks ranges and is on current JS engines often slower than "if-else". Therefore a case..of is translated to if..else. 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    Var
      i: integer;
    Begin
      case i of
        1: ;
        2: i:=3;
      else
        i:=4;
      end;
    End.
    

| 
    
    
    rtl.module("program",
    ["System"],
    function(){
      var $mod = this;
      this.i = 0;
      $mod.$main=function(n){
        $tmp1 = $mod.i;
        if ($tmp1 == 1){
        } else if ($tmp1 == 2) {
          i=3;
        } else {
          i=4;
        }
      };
    },
    []);
      
  
  


## Translating with..do

JavaScript has a **with** , but it is slow and deprecated. Instead a temporary variable is used: 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    type
      TClassA = class
        i: integer;
      end;
    
    procedure DoIt;
    
    Implementation
    
    procedure DoIt;
    begin
      with TClassA.Create do
        i:=3;
    end;
    
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      var $mod = this;
      rtl.createClass($mod, "TClassA", pas.System.TObject, function () {
        this.$init = function () {
          this.i = 0;
        };
      });
      this.DoIt = function(){
        var $with1 = $mod.TClassA.$create("Create");
        $with1.i = 3;
      };
    },
    []);
      
  
Note: If the with-expression is already a local variable no new variable is created. This is Delphi/FPC compatible. 

## Translating enums

Enum values are translated to numbers. The enum type is translated to an object containing a mapping from name to number and number to name. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    type
      TMyEnum = (
        Red,
        Green,
        Blue);
    var
      e: TMyEnum = Blue;
    
    procedure DoIt;
    
    Implementation
    
    procedure DoIt;
    begin
      e := Green;
    end;
    
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      var $mod = this;
      this.TMyEnum = {
        "0":"Red",
        Red:0,
        "1":"Green",
        Green:1,
        "2":"Blue",
        Blue:2
        };
      this.e = $mod.TMyEnum.Blue;
      this.DoIt = function(){
        $mod.e = $mod.TMyEnum.Green;
      };
    },
    []);
      
  
  * Supported: ord(), low(), high(), pred(), succ(), type cast number to enum.
  * With optimization level -O1 the compiler uses numbers instead of names.
  * Not yet implemented: custom values for enum values.



  


## Translating sets

A set s is translated to a JavaScript object, where for each included enum holds _s.enumvalue==true_. This allows arbitrary large sets and the _in_ operator is fast. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    type
      TColor = (Red, Green, Blue);
      TColors = set of TColor;
    
    procedure DoIt;
    
    Implementation
    
    procedure DoIt;
    var
      c: TColor;
      S, T: TColors;
      b: boolean;
    begin
      S:=T;
      b:=Red in S;
      Include(S,Blue);
      Exclude(S,Blue);
      S:=S+T;
      S:=S-[Red,c];
      b:=c in [Red..Blue];
    end;
    
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      var $mod = this;
      this.TColor = {
        "0":"Red",
        Red:0,
        "1":"Green",
        Green:1,
        "2":"Blue",
        Blue:2
        };
      $mod.DoIt = function(){
        var c = 0;
        var S = {};
        var T = {};
        var b = false;
        S = rtl.refSet(T);
        b = $mod.TColor.Red in S;
        S = rtl.includeSet(S,$mod.TColor.Blue);
        S = rtl.excludeSet(S,$mod.TColor.Blue);
        S = rtl.unionSet(S,T);
        S = rtl.diffSet(S,rtl.createSet($mod.TColor.Red,c));
        b = c in rtl.createSet(null,$mod.TColor.Red,$mod.TColor.Blue);
      };
    },
    []);
      
  
  * Supported: 
    * Include
    * Exclude
    * literal
    * literal range, e.g. _[EnumA..EnumB], ['a'..'z']_
    * union +
    * difference -
    * intersect *
    * symmetrical difference ><
    * equal =
    * unequal <>
    * subset <=
    * superset >=
    * set of anonymous enum type: _set of (enum1,enum2,...)_
  * Not supported: set of char, set of boolean
  * There is no optimization yet for small sets like in Delphi/FPC.
  * Assigning a set or passing the set as an argument only creates a reference and marks the set as _shared_. When a _shared_ set is altered with Include/Exclude a new set is created (copy on write).
  * Passing a set as an argument might clone the set. Use the _const_ modifier for parameters whenever possible.
  * Constant sets in expressions (e.g. _if c in ['a'..'z'] then_) are not yet optimized and created every time. Create a _const_ to avoid this.



  


## Translating array type

All arrays are translated into JavaScript arrays.  
Contrary to Delphi/FPC dynamic arrays are not reference counted and do not copy on write. That means if you pass an array to a procedure and change an element, the original array is changed. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Type
      TIntArr = Array of integer;
      TObjArr = Array of TObject;
      TRec = record c: char; end;
      TRecArr = Array of TRec;
    Procedure Test;
    Implementation
    Procedure Test;
    Var
      IntArr: TIntArr = (1,2,3);
      ObjArr: TObjArr;
      RecArr: TRecArr;
    Begin
      IntArr:=nil;
      SetLength(IntArr,4);
      IntArr[2]:=2;
      IntArr[1]:=length(IntArr);
      SetLength(ObjArr,5);
      SetLength(RecArr,2,TRec);
    End;
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      var $mod = this;
      this.Test = function(){
        this.TRec = function(s){
          if (s){
            this.c = s.c;
          } else {
            this.c = "";
          };
          this.$equal = function(b){
            return (this.c == b.c);
          };
        };
        this.IntArr = [1,2,3];
        this.ObjArr = [];
        this.RecArr = [];
        this.Test = function(){
          $mod.IntArr = [];
          rtl.arraySetLength($mod.IntArr,4,0);
          $mod.IntArr[2] = 2;
          $mod.IntArr[1] = $mod.IntArr.length;
          rtl.setArrayLength($mod.ObjArr,5,null);
          rtl.setArrayLength($mod.RecArr,2,$mod.TRec);
        }
      };
    },
    []);
      
  
Notes: 

  * Supported features of dynamic arrays: SetLength(), Length(), equal/notequal nil, low(), high(), assigned(), concat(), copy(), insert(), delete(), multi dimensional, array of record
  * Dynamic array constants. E.g. in mode ObjFPC _const a: array of byte = (1,2)_. In mode Delphi you must use square brackets, _... = [1,2]_
  * Supported features of static arrays: length(), low(), high(), assigned(), concat(), copy(), const, const records
  * Open arrays are implemented as dynamic arrays.
  * Calling _Concat()_ with only one array simply returns the array (no cloning). Calling it with multiple arrays creates a clone. This is Delphi 10.1 compatible.
  * In Delphi/FPC an empty array is _nil_. In JS it can be _null_ or _[]_. For compatibility comparing an array with _nil_ checks for _length(a) >0_.
  * _function Assigned(array): boolean_ results true iff _length(array) >0_.
  * array of const: 
    * Works the same: vtInteger, vtBoolean, vtPointer, vtObject, vtClass, vtWideChar, vtInterface, vtUnicodeString
    * ''longword'' is converted to ''vtNativeInt''. Delphi/FPC converts to ''vtInteger'', changing big numbers to negative numbers.
    * vtExtended is double, Delphi/FPC: PExtended
    * vtCurrency is currency, Delphi/FPC: PCurrency
    * Not supported: vtChar, vtString, vtPChar, vtPWideChar, vtAnsiString, vtVariant, vtWideString, vtInt64, vtQWord
    * only in pas2js: vtNativeInt, vtJSValue
  * Assignation using constant array, e.g. _a:=[1,1,2];_
  * String like operation: + operator concatenates arrays. e.g. _a:=[1]+[2];_. This is controlled by modeswitch arrayoperators, which is enabled in mode delphi.
  * _function copy(array,start=0,count=max): array_
  * _procedure insert(item,var array,const position)_
  * _procedure delete(var array,const start,count)_



  


## Translating class type

Classes are implemented using _Object.create_ and some rtl magic. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Type
      TClassA = Class(TObject)
      Public
        i: integer;
        Procedure Add(a: integer);
      End;
    var
      ObjA: TClassA;
    Implementation
    Procedure TClassA.Add(a: integer);
    Begin
      i:=i+a;
    End;
    Initialization
      ObjA:=TClassA.Create;
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      var $mod = this;
      rtl.createClass($mod,"TClassA",pas.System.TObject,function(){
        this.$init = function () {
          this.i = 0;
        };
        this.Add = function(a){
          this.i = this.i + a;
        };
      });
      this.ObjA = null;
      $mod.$init = function(){
        $mod.ObjA = $mod.TClassA.$create("Create");
      };
    },
    []);
      
  
Notes: 

  * Each class and each instance is an JS object.
  * Each class has a globally unique JS object, created by rtl.createClass.
  * _Self_ is never _nil_.
  * The method _TObject.Free_ is using compiler magic. See [#tobjectfree Translating TObject.Free].
  * _Class.$class_ is a reference to the class itself.
  * _Class.$ancestor_ is a reference to the ancestor class.
  * A class has _c.$ancestor == Object.getPrototypeOf(c)_.
  * A class instance has _o.$class == Object.getPrototypeOf(o)_.
  * _Class.$classname_ is the short name. E.g. _TClassA.$classname == 'TClassA'_.
  * _Class.$name_ is the long name. E.g. _TClassA.$name == 'MyModule.TClassA'_.
  * _Class.$unitname_ is the unit name. E.g. _TClassA.$unitname == 'MyModule'_.
  * The "_is_ "-operator is implemented using "_isPrototypeOf_ ". Note that "_instanceof_ " cannot be used, because classes are JS objects.
  * The "_as_ " operator is implemented as _rtl.as(Object,Class)_.
  * Supported: 
    * constructor, destructor
    * private, protected, public, strict private, strict protected
    * class vars, const, nested types
    * methods, class methods, class constructor, external methods
    * method modifiers overload, reintroduce, virtual, override, abstract, static, external name, message integer, message string
    * call inherited
    * assigned()
    * type cast
    * class sealed, class abstract
  * Not supported: class destructor
  * Property: 
    * References are replaced by getter/setter.
    * Supported: argument lists, default property, class property, stored modifier, index modifier.
    * Not supported: getter/setter to an array element, e.g. _property A: char read FArray[0];_
    * Class property getter/setter can be static or non static. Delphi: must be static.
    * The _Index_ modifier supports any constant, e.g. a string, while Delphi only allows an ordinal (longint). -2147483648 is not a special number in pas2js. Overriding a property with an index property is allowed in Delphi and pas2js.



  


## Translating class-of type

A class-of is a reference to a class. See above about translating class. 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Type
      TBird = Class(TObject)
      Public
        Class var Count: integer;
        Class Procedure Add(a: integer); virtual;
      End;
      TBirds = class of TBird;
    
      TPigeon = Class(TBird)
      Public
        Class Procedure Add(a: integer); override;
      End;
    
    var
      BirdType: TBirds;
    Implementation
    Class Procedure TBird.Add(a: integer);
    Begin
      Count:=Count+a;
    End;
    Class Procedure TPigeon.Add(a: integer);
    Begin
      inherited Add(a+1);
    End;
    Initialization
      BirdType:=TPigeon;
      BirdType.Add(1);
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System"],
    function(){
      var $mod = this;
      rtl.createClass($mod,"TBird",pas.System.TObject,function () {
        this.Count = 0;
        this.Add = function (a) {
          this.Count = this.Count + a;
        };
      });
      rtl.createClass($mod,"TPigeon",$mod.TBird,function () {
        this.Add = function (a) {
          $mod.TBird.Add.call(this,a + 1);
        };
      });
      $mod.$init = function(){
        $mod.BirdType = $mod.TPigeon;
        $mod.BirdType.Add(1);
      };
    },
    []);
      
  
Note that _this_ in a class method is the class itself.  
  
Notes:  


  * Contrary to Delphi/FPC the "is" operator works with class-of.



  


## Translating TObject.Free

In Delphi/FPC AnObject.Free checks if Self is nil, then calls the destructor and frees the memory, without changing the reference. In JavaScript however calling a method with AnObject=nil causes a crash. And memory cannot be freed explicitely. Memory is only freed if all references are gone (e.g. set to _null_).  
Therefore pas2js adds code to call the destructor and sets the variable to _nil_ :  


  * _Obj.Free_ on a local variable or argument is translated to _Obj = rtl.freeLoc(Obj);_.
  * _Obj.Free_ on a non local variable is translated to _rtl.free(this,"Obj");_.
  * Not supported: Freeing a property or function result.  
For example _List[i].Free_ gives a compiler error. The property setter might create side effects, which would be incompatible to Delphi/FPC.



Notes: 

  * If the destructor raises an exception, the variable is not set to _nil_. This is compatible to Delphi/FPC, where the memory is not freed in this case.
  * Alternatively you can use _FreeAndNil_ , which first changes the variable to _nil_ and then calls the destructor.



  


## Translating class interfaces

JavaScript has nothing like it, so they are emulated.  
An interfacetype is a JS-object with some hidden properties, containing the GUID ($guid) and an array with the method names ($names). Here is how IUnknown looks like in JS:  


  

    
    
    {
      $module: [object Object],
      $name: "IUnknown",
      $fullname: "System.IUnknown",
      $guid: "{00000000-0000-0000-C000-000000000046}",
      $names: ["QueryInterface","_AddRef","_Release"],
      $rtti: [object Object],
      $kind: "com",
    }
    

A class implementing interfaces has a variable _$intfmaps_ , which has for each implemented GUID a map or delegator function. A map is a JS instance of the interfacetype plus a for each method name a function to call the class method. Here is an example map of _IUnknown_ of _TInterfacedObject_ :  


  

    
    
    {
      QueryInterface: function (){ return fn.apply(this.$o,arguments); },
      _AddRef: function (){ return fn.apply(this.$o,arguments); },
      _Release: function (){ return fn.apply(this.$o,arguments); },
      ...
    }
    

When an interface is created for an object (here: a Pascal class instance), for example by using the _as_ -operator "_ObjVar as IUnknown_ ", a JS object is created, which is an instance of the map object with its _$o_ set to the _ObjVar_.  
  
Supported: 

  * methods, properties, default property
  * _{$interfaces com|corba|default}_  

    * COM is default, default ancestor is IUnknown (mode delphi: IInterface), managed type, i.e. automatically reference counted via _AddRef, _Release, the checks for support call QueryInterface
    * CORBA: lightweight, no automatic reference counting, no default ancestor, fast support checks.
  * inheriting
  * An interface without a GUID gets one autogenerated from its name and method names.
  * Contrary to Delphi/FPC you can assign an interface type or var to the type TGuidString.
  * a class implementing an interface must not be external
  * a ClassType "supports" an interface, if it itself or one of its ancestors implements the interface. It does not automatically support an ancestor of the interface.
  * method resolution, procedure IUnknown._AddRef = IncRef;
  * delegation: property Name: interface|class read Field|Getter implements AnInterface;
  * is-operator: 
    * IntfVar is IntfType - types must be releated
    * IntfVar is ClassType - types can be unrelated, class must not be external
    * ObjVar is IntfType - can be unrelated
  * as-operator 
    * IntfVar as IntfType - types must be releated
    * IntfVar as ClassType - types can be unrelated, nil returns nil, invalid raises EInvalidCast
    * ObjVar as IntfType - can be unrelated, nil if not found, COM: uses _AddRef
  * typecast: 
    * IntfType(IntfVar) - must be related
    * ClassType(IntfVar) - can be unrelated, nil if invalid
    * IntfType(ObjVar) - nil if not found, COM: if ObjVar has delegate uses _AddRef
    * TJSObject(IntfTypeOrVar). Note that you cannot typecast directly to a _TJSObject_ descendant. You can use _TJSWindow(TJSObject(IntfType))_.
    * jsvalue(intfvar)
  * Assign operator: 
    * IntfVar:=nil;
    * IntfVar:=IntfVar2; - IntfVar2 must be same type or a descendant
    * IntfVar:=ObjVar; - nil if unsupported
    * jsvalue:=IntfVar;
    * TGUIDVar:=IntfType;
    * TGUIDVar:=IntfVar;
    * TGUIDVar:=stringconstant;
    * TGUIDStringVar:=IntfVar;
    * StringVar:=GuidVar;
  * Equal/Inequal operator: 
    * IntfVar=nil;
    * IntfVar=IntfVar2; - must be related
    * jsvalue=IntfVar;
    * TGUIDVar=IntfType;
    * TGUIDVar=IntfVar;
    * TGUIDVar=string;
    * TGUIDStringVar=IntfVar;
  * Passing an COMIntfVar to an untyped parameter or _JSValue_ does not trigger __AddRef, _Release_.
  * Assigned(IntfVar)
  * RTTI, typeinfo(IntfType), typeinfo(IntfVar)



Not yet supported: array of intferfacetype, interface as record member. 

## Translating helpers

Pas2js supports class helpers, record helpers and type helpers since 1.3. The extend is only virtual, the helped type is kept untouched.   


  * A **class helper** can "extend" Pascal classes and external JS classes.
  * A **record helper** can "extend" a record type. In $mode delphi a record helper can extend other types as well, see _type helper_
  * A **type helper** can extend all base types like integer, string, char, boolean, double, currency, and user types like enumeration, set, range, array, class, record and interface types. It cannot extend helpers and procedural types.  
Type helpers are enabled by default in _$mode delphi_ and disabled in _$mode objfpc_. You can enable them with **{$modeswitch typehelpers}**.
  * By default only one helper is active per type, same as in FPC/Delphi. If there are multiple helpers for the same type, the last helper in scope wins.  
A class with ancestors can have one active helper per ancestor type, so multiple helpers can be active, same as FPC/Delphi.  
Using **{$modeswitch multihelpers}** you can activate all helpers within scope.
  * Nested helpers (e.g. _TDemo.TSub.THelper_) are elevated. Visibility is ignored. Same as FPC/Delphi.
  * Helpers cannot be forward defined (e.g. no _THelper = helper;_).
  * Helpers must not have fields.
  * **Class Var, Const, Type**
  * **Visibility** : _strict private .. published_
  * **Function, procedure** : In class and record helpers _Self_ is the class/record instance. For other types Self is a reference to the passed value.
  * **Class function, class procedure** : Helpers for Pascal classes/records can add _static_ and non static class functions. Helpers for external classes and other types can only add static class functions.
  * **Constructor**. Not for external classes. Works similar to construcors, i.e. _THelpedClass.Create_ creates a new instance, while _AnObj.Create_ calls the constructor function as normal method. Note that Delphi does not allow calling helper construcors as normal method.
  * no destructor
  * **Property** : getters/setters can refer to members of the helper, its ancestors and the helped class/record.
  * **Class property** : getter can be static or non static. Delphi/FPC only allows static.
  * **Ancestors** : Helpers can have an ancestor helper, but they do not have a shared root class, especially not _TObject_.
  * **no virtual, abstract, override**. Delphi allows them, but 10.3 crashes when calling.
  * **inherited** : _inherited_ inside a method of a class/record calls helper of ancestor.  
_inherited_ inside a helper depends on the $mode: 
    * _$mode objfpc_ : _inherited;_ and _inherited Name(args);_ work the same and searches first in HelperForType, then in ancestor(s).
    * _$mode delphi: inherited;_ : skip ancestors and HelperForType, searches first in helper(s) of ancestor of HelperForType.
    * _$mode delphi: inherited name(args);_ : same as $mode objfpc first searches in HelperForType, then Ancestor(s) In any case if _inherited;_ has no ancestor to call, it is silently ignored, while _inherited Name;_ gives an error.
  * **RTTI** : _typeinfo(somehelper)_ returns a pointer to _TTypeInfoHelper_ with _Kind tkHelper_.
  * There are some special cases when using a **type helper** function/procedure on a value: 
    * _function result_ : using a temporary variable
    * _const, const argument_ : When helper function tries to assign a value, pas2js raises a EPropReadOnly exception. FPC/Delphi use a temporary variable allowing the write.
    * _property_ : uses only the getter, ignoring the setter. This breaks OOP, as it allows to change fields without calling the setter. This is FPC/Delphi compatible.
    * _with value do ;_ : uses a temporary variable. Delphi/FPC do not support it.
  * A method with _external name_ modifier is treated as an external method of the helped type.



  


## Translating attributes

Attributes are stored in the TTypeInfo objects as streams stored in an array. See the function _GetRTTIAttributes_ in unit _TypInfo_ for details. 

## Translating try..finally

JavaScript has the same, so it translates straight forward. 

## Translating try..except

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    Uses SysUtils, Math, JS;
    Function DoIt(n: integer): double;
    Implementation
    Function DoIt(n: integer): double;
    var E: Exception;
    Begin
      try
        Result:=double(7.0)/n;
        if not IsFinite(Result) then
          if n=0 then
            raise EZeroDivide.Create
          else
            raise EOverflow.Create;
      except
        on EZeroDivide do Result:=0.0;
        on E2: EOverflow do Result:=0.0;
        else
          raise EAbort.Create('Something other: '+String(JS.JSExceptObject));
      end;
    End;
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System","SysUtils"],
    function(){
      this.DoIt=function(n){
        Result = 0;
        var E = null;
        try{
          Result = 7.0 / n;
          if (!IsFinite(Result)){
            if (n==0){
              throw pas.SysUtils.EZeroDivide.$create("Create");
            } else {
              throw pas.SysUtils.EOverflow.$create("Create");
            };
          };
        }catch($e){
          if (pas.SysUtils.EZeroDivide.isPrototypeOf($e)){
            Result = 0.0;
          } else if (pas.SysUtils.EOverflow.isPrototypeOf($e)){
            var E2 = $e;
            Result = 0.0;
          } else {
            throw pas.SysUtils.EAbort.$create("Create",["Something other: "+(""+$e)]);
          }
        }
        return Result;
      };
    },
    []);
      
  
Notes: 

  * Division by zero does not raise an exception in JavaScript. Instead it results in Infinity, except for 0/0 which results in NaN.
  * There is no ExceptObject in SysUtils.
  * When calling external functions keep in mind that JS allows to throw (raise) any value, often a string.  
You can access the current except value via JSExceptValue in unit JS.  
Note that this is only valid inside the catch-block. The compiler will not warn, if you use it outside.



  


## Translating enumerators

The for..in..do supports enumerating: 

  * ordinal types like char, boolean, byte, ..., longword, enums, custom ranges are translated to a for loop.
  * set types are translated to a for loop, while const sets and set variables are enumerated via a for(...in...) loop.
  * string and array variables are enumerated via for loops.
  * for aString in ArrayOfString do ...
  * _for key in jsvalue do_ translates to _for (key in jsvalue){}_
  * _for key in ExternalClass do_  

    * If the externalclass has a ''length'' and a matching default property it uses the enumeration of an array. For example _for value in TJSArray do_ enumerates the values of the array, not the index. It checks if the array is nil.
    * Otherwise it translates to _for (key in externalclass){}_ , which enumerates the keys (property names) of the JS object.



The class GetEnumerator function is translated like this: 

Pascal  | JavaScript   
---|---  
      
    
    Unit MyModule;
    Interface
    
    uses Classes;
    
    procedure DoIt(List: TList);
    
    Implementation
    
    procedure DoIt(List: TList);
    var
      Item: Pointer;
    begin
      for Item in List do
        if Item<>nil then ;
    end;
    
    End.
    

| 
    
    
    rtl.module("MyModule",
    ["System","Classes"],
    function(){
      this.DoIt=function(List){
        var Item = null;
        var $in1 = List;
        try {
          while ($in1.MoveNext()) {
            Item = $in1.GetCurrent();
            if (Item !== null) ;
          }
        } finally {
          $in1 = rtl.freeLoc($in1)
        };
      };
    },
    []);
      
  
Notes: 

  * Not supported: operator Enumerator, member modifier enumerator (i.e. custom Current and MoveNext)



  


## Translating function types

JavaScript functions work like Delphi's "reference to function", which means like closures, capturing outer variables. Assigning a normal function or nested function to a procedural variable is translated to a simple assignment. A Pascal method needs **this** to be the class or class instance.  
Note that _bind_ cannot be used, because it does not support the _equal_ operator. Instead a wrapper is created: 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    type
      TMyMethod = procedure(n: integer) of object;
      TBird = class
        procedure DoIt(n: integer); virtual; abstract;
      end;
      TMyProc = procedure(n: integer);
    procedure DoSome(n: integer);
    begin
    end;
    var
      m: TMyMethod;
      Bird: TBird;
      p: TMyProc;
    Begin
      m:=@Bird.DoIt;
      m(3);
      p:=@DoSome;
      p(4);
    End.
    

| 
    
    
    rtl.module("program",
    ["System","UnitA"],
    function(){
      var $mod = this;
      rtl.createClass($mod,"TBird",pas.System.TObject,function(){
        this.DoIt = function (n) {
        };
      });
      this.DoSome = function (n) {
      };
      this.m = null;
      this.Bird = null;
      this.p = null;
      $mod.$main = function() {
        $mod.m = rtl.createCallback($mod.Bird,"DoIt");
        $mod.m(3);
        $mod.p = $mod.DoSome;
        $mod.p(4);
      };
    },
    []);
    
    rtl = {
      ...
      createCallback: function(scope, fn){
        var cb;
        if (typeof(fn)==='string'){
          cb = function(){
            return scope[fn].apply(scope,arguments);
          };
        } else {
          cb = function(){
            return fn.apply(scope,arguments);
          };
        };
        cb.scope = scope;
        cb.fn = fn;
        return cb;
      },
      ...
      
  
Notes: 

  * You can assign a nested procedure to procedure variable. You don't need and you must not add the FPC "_is nested_ " modifier.
  * In pas2js a procedural typed declared as **reference to** accepts procedures, local procedures and methods. Delphi only supports capturing procedures and methods. FPC 3.0.4 does not support reference-to.
  * In pas2js the calling convention _safecall_ has a special meaning:  
Assigning a procedure/method, uses _rtl.createSafeCallback_ instead of _createCallback_ , enclosing a call in a _try..catch_ block. When an exception is thrown by JS, it is caught and delegated to _rtl.handleUncaughtException(err)_.  
For example:  
_aButtonElement.OnClick:=@DoClick;_ uses _rtl.createSafeCallback_  
 _aButtonElement.OnClick:=SomeElement.OnClick;_ does not.  




  


## Translating anonymous functions

Anonymous functions are supported since pas2js 1.1.  
Note that in pas2js local procedures are closures as well. See below.  
For pas2js 1.0 the next best thing are local procedures. For example: 

Delphi  | Pas2js   
---|---  
      
    
    Program MyModule;
    type
      TAdder = reference to function(n: integer): integer;
    
    function CreateAdder(a: integer): TAdder;
    begin
      Result:=function(b: integer)
        begin
          Result:=a+b;
        end;
    end;
    
    var
      Adder: TAdder;
    Begin
      Adder:=CreateAdder(3);
      writeln(Adder(5)); // gives 8
    End.
    

| 
    
    
    Program MyModule;
    type
      TAdder = reference to function(n: integer): integer;
    
    function CreateAdder(a: integer): TAdder;
      function Add(b: integer): integer;
      begin
        Result:=a+b;
      end;
    begin
      Result:=@Add;
    end;
    
    var
      Adder: TAdder;
    Begin
      Adder:=CreateAdder(3);
      writeln(Adder(5)); // gives 8
    End.
      
  
  


## Translating var modifier absolute

The absolute modifier works as an alias. That means it works FPC/Delphi compatible for related types like Pointer and TObject, and works incompatible for unrelated types like longword and record (e.g. _var r: TPoint absolute MyLongInt_).  
The modifier is currently only supported for local variables. 

## Translating assert()

The Assert(boolean[,string]) function is translated to _if(bool) throw x_. If unit sysutils is used, it creates an EAssertFailed exception.  
Otherwise it throws a string.  


  * Command line enable with -Sa, disable with -Sa-
  * In code enable with _{$C+}_ or _{$Assertions on}_ , disable with _{$C-}_ or _{$Assertions off}_



  


## Dispatch messages

The procedure modifier **message** and the **Dispatch** method works similar to FPC/Delphi, as it expects a record of a specific format and _**TObject.Dispatch**_ calls the corresponding method with that message number or string.  
The procedure modifier _message <integer>_ adds an entry to hidden _YourClass.$msgint_ object, while the modifier _message <string>_ adds an entry to the hidden _YourClass.$msgstr_ object.  
Two new directives _**{$DispatchField fieldname}**_ and _**{$DispatchStrField fieldname}**_ were added. Insert these directives in front of your class declaration to let the compiler check all methods with message modifiers of this class and its descendants whether they pass a record with the required field. For example: 

  

    
    
      {$DispatchField Msg} // enable checking message methods for record field name "Msg"
      {$DispatchStrField MsgStr}
      TObject = class
        procedure Dispatch(var aMessage); virtual;
        procedure DispatchStr(var aMessage); virtual;
      end;
      TMouseDownMsg = record
        Id: integer; // Id instead of Msg, works in FPC, but not in pas2js
        x,y: integer;
      end;
      TMouseUpMsg = record
        MsgStr: string;
        X,Y: integer;
      end;
      TWinControl = class
        procedure MouseDownMsg(var Msg: TMouseDownMsg); message 3; // warning: Dispatch requires record field Msg
        procedure MouseUpMsg(var Msg: TMouseUpMsg); message 'up'; // ok, record with string field name MsgStr
      end;
    

Note that descendant classes can override the _$DispatchField_ or disable the check using _{$DispatchField -}_. 

## Calling JavaScript from Pascal

Pas2js allows to write low level functions and/or access a JavaScript library with the following possibilities: 

## The asm block

The asm block is pure JavaScript, that is copied directly into the generated .js file. 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    var
      s: string;
    Begin
      s = 'Hello World!';
      Asm
        console.log(s);
      End;
    End.
    

| 
    
    
    rtl.module("program",
     ["System"],
     function(){
       var $mod = this;
       this.s = '';
       $mod.$main = function(){
         $mod.s = "Hello World!";
         console.log(s);
       };
     },
     []);
     
      
  
Notes: 

  * The block is indented to produce more readable JS code. All lines are indented or unindented the same amount, i.e. sub indentation is kept.
  * The compiler does neither parse, nor check the syntax of the JS.
  * The compiler does not know what Pascal identifiers are used by the asm-block and might remove them, if no Pascal code is using them. To make sure that an identifier is kept, add some dummy code like _if MyVar=0 then;_
  * Accessing an interface, program or library identifier:  

    * From inside the module you can use _$mod.Identifier_.
    * Otherwise use the fully qualified path _pas.Unitname.Identifier_.
  * Accessing an implementation identifier:  

    * From inside the unit you can use _$impl.Identifier_.
    * Otherwise use the path _pas.Unitname.$impl.Identifier_.
  * Accessing a class instance member (field, procedure, function, constructor, destructor) from a method of the class: use _this.Identifier_. Inside a nested function of a method you use the _Self.Identifier_.
  * Accessing a class member (class var, class procedure, class function) from a method of the class: for writing use _this.$class.Identifier_ , for reading you can omit the _$class_.
  * Accessing a class member (class var, class procedure, class function) from a class method of the class: use _this.Identifier_.
  * Access to Properties must use the getter/setter.
  * When calling a Pascal method, make sure the **this** is correct: 
    * A class method (e.g. _class function_ , _class procedure_) needs the class as _this_.  
**Wrong** : _aCar.DoIt(params,...)_  
**Correct** : _aCar.$class.DoIt(params,...)_  

  * Calling a Pascal function from a HTML/DOM-element: For example to call a function when user clicks a DOM element you can assign a function to the _onclick_ property. This will call the function with _this_ set to the DOM element.  
Pascal methods needs a wrapper to set _this_ to the instance. Examples: 
    * An unit function: _DOMElement.onclick = $mod.DoIt;_
    * An implementation function: _DOMElement.onclick = $impl.DoIt;_.
    * A method: _DOMElement.onclick = this.DoIt.bind(this);_
    * A class function/procedure: _DOMElement.onclick = this.DoIt.bind(this.$class);_
    * A nested function: _DOMElement.onclick = DoIt;_.



  


## The procedure modifier assembler

You can write pure JavaScript functions like this: 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    
    Procedure Log(const s: string); assembler;
    Asm
      console.log(s);
    end;
    
    Begin
      Log('Hello World!');
    End.
    

| 
    
    
    rtl.module("program",
    ["System"],
    function(){
      var $mod = this;
      this.Log = function(s){
        console.log(s);
      };
      $mod.$main = function(){
        $mod.Log("Hello World!");
      };
    },
    []);
      
  
See also [#asm asm]. 

## The procedure modifier external

The procedure modifier _external_ requires a string constant and tells the compiler to replace a reference with this string value. The value is not checked for JS syntax. 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    Procedure ConsoleLog(const s: string); external name 'console.log';
    // Note: an external procedure has no begin..end block
    Begin
      ConsoleLog('Hello World!');
    End.
    

| 
    
    
    rtl.module("program",
    ["System"],
    function(){
      var $mod = this;
      $mod.$main = function(){
        console.log("Hello World!");
      };
    },
    []);
      
  
  


## The procedure modifier varargs

Appending the **varargs** modifier to a procedure allows to pass arbitrary more parameters to a function. By default these parameters are untyped, i.e. any type fits. Alternatively you can use **varargs of aType** to allow only specific types.  
To access these arguments use either _JSArguments_ from unit JS or an _asm..end_ block. 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    uses JS;
    function Sum(b: boolean): longint; varargs;
    var i: longint;
    begin
      if b then
        asm
          for (var i=0; i<arguments.length; i++) Result+=arguments[i];
        end
      else
        for i:=0 to JSArguments.length-1 do
          Result:=Result+longint(JSArguments[i]);
    end;
    var
      i: integer;
    Begin
      i:=Sum(true,2,4,6); // i=12
      i:=Sum(false,2,4,6); // i=12
    End.
    

| 
    
    
    rtl.module("program",
    ["System","JS"],
    function(){
      var $mod = this;
      this.Sum = function(b){
        var Result = 0;
        var i = 0;
        if (b){
          for (var i=0; i<arguments.length; i++) Result+=arguments[i];
        } else {
          for (var $l1 = 1, $le2 = argumens.length; $l1 <= $le2; $l1++){
            $i = $l1;
            Result = Result + arguments[i];
          }
        }
        return Result;
      };
      this.i = 0;
      $mod.$main = function(){
        $mod.i = $mod.Sum(true,2,4,6);
        $mod.i = $mod.Sum(false,2,4,6);
      };
    },
    []);
      
  
The above example defines a function _Sum_ , that requires the first parameter to be a boolean and then an arbitrary number of parameters. The compiler does not type check the other parameters, so you can pass anything readable. 

## The var modifier external

The var modifier _external_ allows to use a JavaScript variable or constant. 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    var
      EulersNumber: Double; external name 'Math.E';
      d: double;
    Begin
      d:=EulersNumber;
    End.
    

| 
    
    
    rtl.module("program",
    ["System"],
    function(){
      var $mod = this;
      this.d = 0.0;
      $mod.$main = function(){
        $mod.d = Math.E;
      };
    },
    []);
      
  
  


## The external modifier of class members

The method modifier _external_ works as the procedure modifier, except it uses the scope of the class or instance.  
The field modifier _external_ works as the var modifier, except it uses the scope of the class or instance.  
Requires the modeswitch **externalclass**. 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    {$modeswitch externalclass}
    type
      TWrapper = class
      private
        // let's assume this object has the properties "$Handle", "$id", and "0"
      public
        Id: NativeInt; external name '$Id';
        x: NativeInt; external name '[0]';
        y: NativeInt; external name '["A B"]';
        function GetState(typ: longint): NativeInt; external name '$Handle.GetState';
        procedure DoIt;
      end;
    procedure TWrapper.DoIt;
    begin
      Id := GetState(4);
    end;
    var
      W: TWrapper;
    Begin
      W.Id := 2;
      W.x := 3;
      W.y := 4;
      W.GetState(5);
    End.
    

| 
    
    
    rtl.module("program",
    ["System"],
    function(){
      var $mod = this;
      rtl.createClass($mod, "TWrapper", pas.System.TObject, function () {
        this.DoIt = function(){
          this.$Id = this.$Handle.GetState(4);
        };
      });
      this.W = null;
      $mod.$main = function(){
        $mod.W.$Id = 2;
        $mod.W[0] = 3;
        $mod.W["A B"] = 4;
        $mod.W.$Handle.GetState(5);
      };
    },
    []);
      
  
  * Non identifiers like "0" or "A B" must be enclosed in brackets.



  


## External classes

pas2js introduces a new class modifier "_external name_ ", which makes the whole class external. External classes allow to easily declare Pascal wrappers for JavaScript objects and function objects.  
They need the modeswitch **externalclass** in front of the class.  
An external class is not a TObject and has none of its methods.  
All members are external. If you omit the _external_ modifier the external name is the member name. Keep in mind that JS is case sensitive.  
Properties work the same as with Pascal classes, i.e. are replaced by Getter/Setter.  
Destructors are not allowed.  
Constructors are supported in four ways: 

  * _constructor New_ is translated to _new ExtClass(params)_ , and for nested external class: _new ExtParentClass.ExtClass(Params)_
  * _constructor New; external name ''GlobalFunc''_ is translated to _new GlobalFunc(params)_.
  * _constructor SomeName; external name '_{}' is translated to _{}_.
  * Otherwise it is translated to _new ExtClass.FuncName(params)_ , and for nested external class: _new ExtParentClass.ExtClass.FuncName(params)_.

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    {$modeswitch externalclass}
    type
      TJSDate = class external name 'Date'
      private
        function getYear: NativeInt;
        procedure setYear(const AValue: NativeInt);
      public
        constructor New;
        constructor New(const MilliSecsSince1970: NativeInt);
        class function now: NativeInt;
        property Year: NativeInt read getYear write setYear;
      end;
    var
      d: TJSDate;
    Begin
      d:=TJSDate.New;
      d.Year:=d.Year+1;
    End.
    

| 
    
    
    rtl.module("program",["System"],function () {
      var $mod = this;
      this.d = null;
      $mod.$main = function () {
        $mod.d = new Date();
        $mod.d.setYear($mod.d.getYear() + 1);
      };
    });
      
  
Notes: 

  * An external class can descend from another external class.
  * Any class instance can be type casted to any root class.
  * A Pascal class can descend from an external class.
  * You can define a class-of external class and the **is** and **as** operators work similar.
  * Class variables work as in JavaScript. That means, each descendant and each instance can have its own value. For example _TExtA.Value_ might be different from _InstanceExtA.Value_. Setting _InstanceExtA.Value_ does not change _TExtA.Value_.
  * Const with an expression are replaced by the expression.
  * Const without an expression are treated as a readonly variable.
  * Class functions and class procedures are allowed, but can only be called via the class, not via an instance.  
For example you can call the class function _TJSString.fromCharCode()_ , but you cannot call _aJSString.fromCharCode()_.
  * Since class types are JS objects it is possible to typecast a class type to the JS Object, e.g. _TJSObject(TObject)_. Note that you cannot typecast directly to a _TJSObject_ descendant in $mode objfpc. You can use _TJSWindow(TJSObject(ExtClassInstance))_.
  * You can typecast function addresses and function references to JS function, e.g. _TJSFunction(@SomeProc)_ , _TJSFunction(OnClick)_. Keep in mind that typecasting a method address creates a function wrapper to bind the Self argument, except when typecasting to _TJSFunction_ (pas2js 1.5+).



## External class as ancestor

A Pascal class can descend from an external class - a JS object or function.  
The methods _AfterConstruction_ and _BeforeDestruction_ are called if they exist.  
New instances of a JS Object descendant are created by default with _Object.create(ancestorclass)_.  
New instances of a JS Function descendant are created by default with _new DescendantFunc()_.  
You can override this, by providing a non private   
**class function NewInstance(fnname: string; const paramsarray): TPasClass; virtual;**. This method is called to create a new instance and before calling the constructor. The name is arbitrary, but the function must be the **first non private** , non external, virtual class function with the class as result type.  


Pascal  | JavaScript   
---|---  
      
    
    // Example for descending a Pascal class from a JS Object
    Program MyModule;
    {$modeswitch externalclass}
    type
      TExtA = class external name 'ExtA'
      end;
      TMyB = class(TExtA)
      protected
        // optional: override default allocation
        class function NewInstance(fnname: string; const paramarray): TMyB; virtual;
      end;
    class function TMyB.NewInstance(fnname: string; const paramarray): TMyB;
    Begin
      asm
      Result = Object.create(ExtA); // that is what the rtl does
      end;
    End;
    
    Begin
    End.
    

| 
    
    
    rtl.module("program",["System"],function () {
      var $mod = this;
      rtl.createClassExt($mod, "TMyB", ExtA, "NewInstance", function () {
        this.$init = function () {
        };
        this.$final = function () {
        };
        this.NewInstance = function (fnname, paramarray) {
          var Result = null;
          Result = Object.create(ExtA);
          return Result;
        };
      });
      $mod.$main = function () {
      };
    });
      
  
Pascal  | JavaScript   
---|---  
      
    
    // Example for descending a Pascal class from a JS Function
    Program MyModule;
    {$modeswitch externalclass}
    uses JS;
    type
      TExternalFunc = class external name 'ExternalFunc'(TJSFunction)
        constructor New(a: word);
      end;
      TMyFunc = class(TExternalFunc)
        constructor Create(b: word);
      end;
    constructor TMyFunc.Create(b: word);
    Begin
      inherited New(b+1); // optional: call inherited constructor function
    End;
    
    var
      f: TMyFunc;
    Begin
      f:=TMyFunc.Create(3);
      writeln(jsInstanceOf(f,TExternalFunc)); // writes true, instanceof operator works as expected
    End.
    

| 
    
    
    rtl.module("program",["System","js"],function () {
      var $mod = this;
      rtl.createClassExt($mod, "TMyFunc", ExternalFunc, "", function () {
        this.$init = function () {
        };
        this.$final = function () {
        };
        this.Create$2 = function (b) {
          this.$ancestorfunc(b+1);
        };
      });
      this.f = null;
      $mod.$main = function () {
        f = $mod.TMyFunc.$create("Create$2",[3]);
        pas.System.Writeln(pas.JS.jsInstanceOf(f,ExternalFunc));
      };
    });
      
  
## The JSValue type

Pas2js introduces a new type **JSValue** , which works similar to a JS variable. You can assign almost any value to it and it can be type casted to many types. JSValue is useful for JS wrappers, when a variable can have multiple types. And it can be used for containers storing arbitrary data, e.g. a list of JSValue.  
Key features:  


  * A JSValue variable initial value is undefined.
  * Operators: =, <>
  * type casting a _JSValue_ to ... 
    * _Integer: Math.floor(aJSValue)_ Note: may return _NaN_
    * _Boolean: !(aJSValue == false)_ Note: works for numbers too, _0==false_
    * _Double: rtl.getNumber(aJSValue)_ Note: _typeof(n)=="number"?n:NaN;_
    * _String: ""+aJSValue_
    * _Char: rtl.getChar(aJSValue)_ Note: _((typeof(c)!="string") && (c.length==1)) ? c : ""_
    * class instance or class-of: _rtl.getObject()_ Note: checks for type _"object"_
    * enum type
    * pointer
  * A JSValue in a conditional expressions _If aJSValue then, while aJSValue do, repeat until aJSValue_ has the same meaning as in JS: the condition is true, if the value is not _undefined, false, null, NaN, 0, ''_. Note that _new Boolean(false)_ is not _null_ and the condition is true.
  * _function Assigned(V: jsvalue): boolean_ returns true if  
 _(V!=undefined) && (V!=null) && (!rtl.isArray(V) || (V.length > 0))_
  * _function StrictEqual(const A: jsvalue; const B): boolean_
  * _function StrictInequal(const A: jsvalue; const B): boolean_
  * Any array can be assigned to an _array of jsvalue_.
  * is-operator: _jsvalue is class-type_ , _jsvalue is class-of-type_  

  * The unit JS provides many utility functions for JSValue, like _hasString, hasValue, isBoolean, isNumber, isInteger, isObject, isClass, isClassInstance, etc.._



  


## Accessing JS object properties with the bracket accessor

Pas2js allows to define index properties that map directly to the JS object properties. For example the default property of TJSObject allows to get and set the properties of an object. For example _TJSObject(AnObject)['Name']:=Value;_  
Another example is the default property of TJSArray, that allows access via integers _aTJSArray[3]:=Value;_  
To define your own bracket accessor define a normal index property and define the getter/setter as _external name '[]'_.  
Here is an example for a read only accessor: 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    {$modeswitch externalclass}
    type
      TExtA = class external name 'ExtA'
      private
        function GetItems(Index: integer): String; external name '[]';
      public
        property Items[Index: integer]: String read GetItems; default;
      end;
    var
      Obj: TExtA;
      s: String;
    Begin
      ... get Obj from somewhere ...
      s:=Obj[2];
    End.
    

| 
    
    
    rtl.module("program",["System"],function () {
      var $mod = this;
      this.Obj = undefined;
      this.s = "";
      $mod.$main = function () {
        $mod.s = Obj[2];
      };
    });
      
  
Notes: 

  * A property can have a mix of normal accessor and bracket accessor. For example a bracket accessor as getter and a normal function as setter.



  


## RTTI - Run Time Type Information

The RTTI provides access to the type data of all published properties, fields and methods. The type data provides similar information as Delphi/FPC, but the internals are very different. Delphi/FPC uses pointers, variant records and fake static arrays, which have no equivalent in JS. Instead pas2js uses external classes. For example: 

  

    
    
        TTypeInfo = class external name 'rtl.tTypeInfo'
        public
          Name: String external name 'name';
          Kind: TTypeKind external name 'kind';
        end;
        TTypeInfoClass = class of TTypeInfo;
    
        TTypeInfoInteger = class external name 'rtl.tTypeInfoInteger'(TTypeInfo)
        public
          MinValue: NativeInt external name 'minvalue';
          MaxValue: NativeInt external name 'maxvalue';
          OrdType : TOrdType external name 'ordtype';
        end;
    
    

The **typeinfo** function works on type, var, const and property identifiers. By default it returns a _pointer_. If the typinfo unit is used it returns the appropiate _TTypeInfo_. For instance _typeinfo(integer)_ returns a _TTypeInfoInteger_.  
_Typeinfo_ of a _var_ or _const_ returns the typeinfo of its type, not of its current runtime value. The exception is a class and class-of instance variable (e.g. _var o: TObject; ... typeinfo(o)_), which returns the typeinfo of the current runtime value. If _o_ is _nil_ it will give a JS error.  
Local types (i.e. inside a procedure) do not have typeinfo.  
Open array parameters are not yet supported.  
Note that FPC _typeinfo(aClassVar)_ returns the compiletime type, so it works on _nil_.  


## Async/AWait

Pas2js supports the JS operators async and await to simplify the use of Promise. The await operator corresponds to three intrinsic Pas2js functions: 

  * _function await(AsyncFunctionWithResultT()): T;_ // implicit promise, the inner () can be omitted
  * _function await(aType; p: TJSPromise): aType;_ // explicit promise requires the resolved type
  * _function await(aType; j: jsvalue): aType;_ // explicit promise requires the resolved type



The await function can only be used inside a procedure with the async modifier.  
Example for the explicit promise: 

Pascal  | JavaScript   
---|---  
      
    
    Program MyModule;
    
    uses JS, Web;
    
    function ResolveAfter2Seconds: TJSPromise;
    begin
      Result:=TJSPromise.new(procedure(resolve, reject : TJSPromiseResolver)
        begin
        window.setTimeout(procedure
          begin
          resolve('resolved');
          end,
          2000); // wait 2 seconds
        end);
    end;
    
    procedure AsyncCall; async;
    var s: string;
    begin
      writeln('calling');
      s := await(string,resolveAfter2Seconds()); // does not check if result is really a string
      writeln(s); // expected output: 'resolved'
    end;
    
    begin
      AsyncCall;
    end.
    

| 
    
    
    rtl.module("program",["System","JS","Web"],function () {
      "use strict";
      var $mod = this;
      this.ResolveAfter2Seconds = function () {
        var Result = null;
        Result = new Promise(function (resolve, reject) {
          window.setTimeout(function () {
            resolve("resolved");
          },2000);
        });
        return Result;
      };
      this.AsyncCall = async function () {
        var s = "";
        pas.System.Writeln("calling");
        s = await $mod.ResolveAfter2Seconds();
        pas.System.Writeln(s);
      };
      $mod.$main = function () {
        $mod.AsyncCall();
      };
    });
      
  
Notes: 

  * The await function does only compile time checks, no runtime checks.
  * Inside an async function/procedure you can pass a _TJSPromise_ to the _exit()_ function. For example:  
_exit(aPromise);_  
_exit(inherited);_



  


## Compiler directives

In config files: 

  * #IFDEF macroname
  * #IFNDEF macroname
  * #IF expression - same as $if, except only defines
  * #ELSEIF
  * #ELSE
  * #ENDIF
  * #ERROR text



In source files: 

  * {$Define _MacroName_}: defines macro _MacroName_ with value '1'.
  * {$Define _MacroName:=value_}: defines macro _MacroName_ with custom value.
  * {$Undef _MacroName_}: undefines macro _MacroName_.
  * {$IfDef _MacroName_}: if _MacroName_ is not defined, skip to next $Else or $EndIf. Can be nested.
  * {$IfNDef _MacroName_}: as $IfDef, except negated.
  * {$If _boolean expression_}: if _expression_ evaluates to true (not '0'), skip to next $Else or $EndIf. Can be nested.  
Supported functions and operators:  

    * macro - replaced by its value, a simple define has value '1'
    * defined(macro) - '1' if defined, '0' otherwise
    * undefined(macro) - as _not defined(macro)_
    * option(letter) - same as _{$IFOpt letter+}_
    * not \- first level of precedence
    * *, /, div, mod, and, shl, shr \- second level of precedence
    * +, -, or, xor \- third level of precedence
    * =, <>, <, >, <=, >= \- fourth level of precedence
    * If the operands can be converted to numbers they are combined as numbers, otherwise as strings. Not supported functions and operators:  

    * defined(Pascal identifier), undefined(Pascal identifier)
    * declared(Pascal identifier)
    * in operator
  * {$IfOpt _Letter+,-_}: if _expression_ evaluates to true (not '0'), skip to next $Else or $EndIf. Can be nested.
  * {$Else}: If previous $IfDef, $If or $IfOpt was skipped, execute next block, otherwise skip.
  * {$ElseIf _boolean expression_} : As $Else, except with an extra expression like $if to test. There can be multiple $elseif.
  * {$EndIf} : ends an $IfDef block
  * {$mode delphi} or {$mode objfpc} : Same as -Mdelphi or -Mobjfpc, but only for this unit. You can use units of both modes in a program. If present must be at the top of the unit, or after the module name.
  * {$modeswitch externalclass} : allow declaring external classes
  * {$modeswitch arrayoperators} : allow + operator to concatenate arrays, default in mode delphi
  * {$modeswitch OmitRTTI} : treat published sections as public
  * {$macro on|off} enables macro replacements. Only macros with a value are replaced. Macros are never replaced inside directives.
  * {$I filename} or {$include filename} \- insert include file
  * {$I %param%} : 
    * %date%: current date as string literal, '[yyyy/mm/dd]'
    * %time%: current time as string literal, 'hh:mm:ss'. Note that the inclusion of %date% and %time% will not cause the compiler to recompile the unit every time it is used: the date and time will be the date and time when the unit was last compiled.
    * %file%: current source filename as string literal, e.g. **unit1.pas**
    * %line%: current source line number as string literal, e.g. **123**
    * %linenum%: current source line number as integer, e.g. _123_
    * %currentroutine%: name of current routine as string literal
    * %pas2jstarget%, %pas2jstargetos%, %fpctarget%, %fpctargetos%: target os as string literal, e.g. 'Browser'
    * %pas2jstargetcpu%, %fpctargetcpu%: target cpu as string literal, e.g. 'ECMAScript5'
    * %pas2jsversion%, %fpcversion%: compiler version as strnig literal, e.g. '1.0.2'
    * If param is none of the above it will use the environment variable. Keep in mind that depending on the platform the name may be case sensitive. If there is no such variable an empty string _''_ is inserted.
  * {$Warnings on|off}
  * {$Notes on|off}
  * {$Hints on|off}
  * {$Error text} : emit an error
  * {$Warning text} : emit a warning
  * {$Note text} : emit a note
  * {$Hint text} : emit a hint
  * {$Message hint-text} : emit a hint
  * {$Message hint|note|warn|error|fatal text} : emit a message
  * {$Warn identifier on|off|default|error} : enable or disable a specific hint.  
Note, that some hints like "Parameter %s not used" are currently using the enable state at the end of the module, not the state at the hint source position.  
Identifier can be a message number as written by -vq or one of the following case insensitive:  

    * CONSTRUCTING_ABSTRACT : Constructing an instance of a class with abstract methods.
    * IMPLICIT_VARIANTS : Implicit use of the variants unit.
    * NO_RETVAL : Function result is not set
    * SYMBOL_DEPRECATED : Deprecated symbol.
    * SYMBOL_EXPERIMENTAL : Experimental symbol
    * SYMBOL_LIBRARY
    * SYMBOL_PLATFORM : Platform-dependent symbol.
    * SYMBOL_UNIMPLEMENTED : Unimplemented symbol.
    * HIDDEN_VIRTUAL : method hides virtual method of ancestor
    * GARBAGE : text after final end.
    * BOUNDS_ERROR : range check errors
    * MESSAGE_DIRECTIVE : user defined $message
  * {$M+}, {$TypeInfo on} : switches default visibility for class members from public to published
  * {$ScopedEnums on|off} disabled(default): propagate enums to global scope, enable: needs fqn e.g. TEnumType.EnumValue.
  * {$C+} generate code for assertions
  * {$H+} , but not {$H-}
  * {$J-}, {$WriteableConst off} : Typed const become readonly. For example _const i:byte=3; ... i:=4_ creates a compile time error.
  * {$M+} : allow published members
  * {$Q+} : not yet supported, ignored
  * {$R+}, {$RangeChecks on} : compile time range check hints become errors and add runtime range checks for assignments.
  * {$ObjectChecks on|off} : 
    * Verify method calls, i.e. check at runtime in every method if _Self_ is a descendant class.
    * Check type casts, e.g. _TBird(AnObject)_ becomes _AnObject as TBird_
  * {$DispatchField Msg} : enable checking _message number_ methods for record field name "Msg"
  * {$DispatchStrField MsgStr} : enable checking _message string_ methods for record field name "Msg"
  * {$Optimization Name} : same as command line option -OoName
  * {$linklib filename [alias]}: 
    * filename filename to import.
    * alias alias for this import. Must be a pascal identifier. If no alias is specified, the last part of the filename is used, all non-alphanumeric characters are converted to underscores.



Defines: 

  * PAS2JS
  * PAS2JS_FULLVERSION - major*1000+minor*100+release, e.g. 1.2.3 = 10203
  * Target platform: Browser, NodeJS, Pas2JSTargetOS=<value>
  * Target processor: ECMAScript5, ECMAScript6, ECMAScript=5, Pas2JSTargetCPU=<value>
  * Mode: DELPHI, OBJFPC



## Numbers
    
    
    JavaScript only supports double. All Pascal number types and enum values are mapped to this. A double supports integers from  
     MinInteger = -$10000000000000;  
     MaxInteger = $fffffffffffff;  
     MinDouble = 5.0e-324;  
     MaxDouble = 1.7e+308;  
      
     Intrinsic integer types:
    

  * Byte - unsigned 8-bit
  * ShortInt - signed 8-bit
  * Word - unsigned 16-bit
  * SmallInt - signed 16-bit
  * LongWord - unsigned 32-bit
  * LongInt - signed 32-bit
  * NativeUInt - unsigned 53-bit
  * NativeInt - signed 54-bit



Notes: 

  * Division by zero does not raise an exception. 0/0 results in NaN, positive/0 is Infinity, negative/0 is -Infinity.
  * NaN<>NaN
  * Overflows work differently. For example in Delphi adding 100 to a byte of 200 gives _300 and $ff = 44_ , while in pas2js it gives 300, which is not a byte anymore.
  * Math.isNan(double) tests for NaN. Otherwise false. isNan(Infinity)=false.
  * Math.isFinite(double) tests if not NaN, positive or negative infinity.
  * Math.isInfinite(double) tests if positive or negative infinity.
  * For more functions see unit Math.
  * To make porting easier Single is defined in the system unit as alias of double, but gives a warning. Since using higher precision might give unexpected results you should check every place.



  


## Other supported Pascal elements

  * **break** , **continue** , **exit** , **exit()**
  * **chr** , **ord**
  * alias type and type alias type
  * _inc()_ and _dec()_ to += -=
  * Converts "a div b" to "Math.floor(a / b)"
  * and, or, xor, not: logical and bitwise
  * Name conflicts with JS identifiers are automatically fixed by changing case. For example a Pascal function "_apply"_ is renamed to "_Apply_ ".
  * uses unitname in 'filename'. In _$mode delphi_ the in-filenames are only allowed in the program and the unitname must fit the filename, e.g. _uses unit1 in 'sub/Unit1.pas_.  
In _$mode objfpc_ units can use in-filenames too and alias are allowed, e.g. _uses foo in 'bar.pas'_.
  * The intrinsic procedure **str** works with boolean, integer, float and enumvalue.  
Additionally there is **str** function, that takes an arbitrary number of arguments and returns a concatenated string. It supports string as parameter too. For example s:=str(i,' ',d:1:5).  
Width and precision is supported. str(i:10) will add spaces to the left to fill up to 10 characters. str(aDouble:1:5) returns a string in decimal format with 5 digits for the fraction.
  * Intrinsic procedure WriteStr(out s: string; params...)
  * _Debugger;_ converts to _debugger;_. If a debugger is running it will break on this line just like a break point.
  * _function concat(string1,string2,...): string_ since 1.3
  * _$mode delphi: function lo|hi(integer): byte_ since 1.3
  * _$mode objfpc: function lo|hi(integer): byte|word|longword_ since 1.3
  * _function GetTypeKind(Type or Var): TTypeKind;_ since 1.5



## Not supported elements

  * Class destructor
  * Enums with custom values
  * Global properties
  * Futures
  * Inline
  * Objects
  * Operator overloading
  * Pointer arithmetic
  * Package
  * Resources
  * RTTI extended, $RTTI
  * Variant records
  * Variants



## JavaScript Version

Code generation depending on -P option: 

  * ECMAScript5
  * ECMAScript6: using 0b for binary literals, and 0o for octal literals



  


## Creating source maps

Source maps are files telling the browser what JavaScript comes from which original source (e.g. Pascal file), similar to debug information in FPC/Delphi.  
In 2017 FireFox and Chrome supports source maps.  
You can enable generating source map files by using the _-Jm_ option.  
The compiler generates one module.js.map file for every generated module.js file. The last line of the .js file contains the line  
 _//# sourceMappingURL=module.js.map_  
telling the browser where to find the source map.  
The source map contains references to the Pascal files and included .js files (e.g. -Jirtl.js) relative to the location of the source map. Note that if the Pascal file lies in a parent directory, the relativ path contains '../'. You can change the base directory of the relative paths by using the option _-Jmbasedir= <x>_. For example _-JmC:\www\pas_ creates paths relative to C:\www\pas.  
You can set the base URL, where the browser finds the Pascal sources, by passing the _-Jmsourceroot= <x>_ option. For example _-Jmsourceroot=<http://www.yoursite.com/pas/>_. The browser prepends this to the source map filenames when downloading the original source files (e.g. the .pas files).  
You can include the whole Pascal sources in the source map using the option _-Jminclude_.  
  
To show the generated mapping for each line you can use the tool fpc/packages/fcl-js/examples/srcmapdump.  


  * Option -JmXSSIHeader: According to the specifications sourcemap should start with the XSSI (cross site script inclusion) protection header _)]}'_. If your browser does not support that, disable it with _-JmXSSIHeader-_. See here the specs: <https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k/edit#heading=h.h7yy76c5il9v>



## See Also

  * [pas2js](<pas2js.md> "pas2js")

---

_Source: [https://wiki.freepascal.org/Pas2js_Transpiler](https://web.archive.org/web/20250120195222/https://wiki.freepascal.org/Pas2js_Transpiler)_
