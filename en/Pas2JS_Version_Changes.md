# Pas2JS Version Changes

## Contents

  * 1 Releases
    * 1.1 Next Version
    * 1.2 Version 2.2.0
    * 1.3 All changes of version 2.2.0
      * 1.3.1 2.2.0 Incompatibilities
        * 1.3.1.1 pas2js.cfg
    * 1.4 Version 2.0.6
    * 1.5 Version 2.0.4
    * 1.6 Version 2.0.2
    * 1.7 Version 2.0.0
    * 1.8 Version 2.0.0RC8
    * 1.9 Version 2.0.0RC7
    * 1.10 Version 2.0.0RC6
    * 1.11 Version 2.0.0RC5
    * 1.12 Version 2.0.0RC4
    * 1.13 Version 2.0.0RC3
    * 1.14 Version 2.0.0RC2
    * 1.15 Version 2.0.0RC1
    * 1.16 All changes of version 2.0.0
      * 1.16.1 2.0.0 Incompatibilities
        * 1.16.1.1 Signature for event handlers TEventListenerEvent and TJSEvent
        * 1.16.1.2 TJSEvent type definition
        * 1.16.1.3 modeswitch ignoreattributes was removed
    * 1.17 Version 1.4.34
    * 1.18 Version 1.4.32
    * 1.19 Version 1.4.30
    * 1.20 Version 1.4.28
    * 1.21 Version 1.4.26
    * 1.22 Version 1.4.24
    * 1.23 Version 1.4.22
    * 1.24 Version 1.4.20
    * 1.25 Version 1.4.18
    * 1.26 Version 1.4.16
    * 1.27 Version 1.4.14
    * 1.28 Version 1.4.12
    * 1.29 Version 1.4.10
    * 1.30 Version 1.4.8
    * 1.31 Version 1.4.6
    * 1.32 Version 1.4.4
    * 1.33 Version 1.4.2
    * 1.34 Version 1.4.0
    * 1.35 Version 1.4.0RC7
    * 1.36 Version 1.4.0RC6
    * 1.37 Version 1.4.0RC5
    * 1.38 Version 1.4.0RC4
    * 1.39 Version 1.4.0RC3
    * 1.40 Version 1.4.0RC2
    * 1.41 Version 1.4.0RC1
    * 1.42 All changes of version 1.4.0
    * 1.43 Version 1.2.0
    * 1.44 Version 1.2.0RC1
    * 1.45 Version 1.0.4
    * 1.46 Version 1.0.3
    * 1.47 Version 1.0.2
    * 1.48 Version 1.0.1
    * 1.49 Version 1.0.0
    * 1.50 Version 1.0.0rc1
    * 1.51 Version 0.9.32
    * 1.52 Version 0.9.31
    * 1.53 Version 0.9.30
    * 1.54 Version 0.9.29
    * 1.55 Version 0.9.28
    * 1.56 Version 0.9.27
    * 1.57 Version 0.9.26
    * 1.58 Version 0.9.25
    * 1.59 Version 0.9.24
    * 1.60 Version 0.9.23
    * 1.61 Version 0.9.22
    * 1.62 Version 0.9.21
    * 1.63 Version 0.9.20
    * 1.64 Version 0.9.19
    * 1.65 Version 0.9.18
    * 1.66 Version 0.9.17
    * 1.67 Version 0.9.16
    * 1.68 Version 0.9.15
    * 1.69 Version 0.9.14
    * 1.70 Version 0.9.13
    * 1.71 Version 0.9.12
    * 1.72 Version 0.9.11
    * 1.73 Version 0.9.10
    * 1.74 Version 0.9.9
    * 1.75 Version 0.9.8
    * 1.76 Version 0.9.7
    * 1.77 Version 0.9.6
    * 1.78 Version 0.9.5
    * 1.79 Version 0.9.4
    * 1.80 Version 0.9.3
    * 1.81 Version 0.9.2
    * 1.82 Version 0.9.1
    * 1.83 Version 0.9.0
    * 1.84 Version 0.8.45
    * 1.85 Version 0.8.44
    * 1.86 Version 0.8.43
    * 1.87 Version 0.8.42
    * 1.88 Version 0.8.41
    * 1.89 Version 0.8.40
    * 1.90 Version 0.8.39
    * 1.91 Version 0.8.38
    * 1.92 Version 0.8.37
    * 1.93 Version 0.8.36
    * 1.94 Version 0.8.35
    * 1.95 Version 0.8.34
    * 1.96 Version 0.8.33
    * 1.97 Version 0.8.32
    * 1.98 Version 0.8.31
    * 1.99 Version 0.8.30
    * 1.100 Version 0.8.29
    * 1.101 Version 0.8.28
    * 1.102 Version 0.8.27
    * 1.103 Version 0.8.26
    * 1.104 Version 0.8.25
    * 1.105 Version 0.8.24
    * 1.106 Version 0.8.23
    * 1.107 Version 0.8.22
    * 1.108 Version 0.8.21
    * 1.109 Version 0.8.20
    * 1.110 Version 0.8.19
    * 1.111 Version 0.8.18
    * 1.112 Version 0.8.17
    * 1.113 Version 0.8.16
    * 1.114 Version 0.8.15
    * 1.115 Version 0.8.14
    * 1.116 Version 0.8.13
    * 1.117 Version 0.8.12
    * 1.118 Version 0.8.11
    * 1.119 Version 0.8.10
    * 1.120 Version 0.8.9
    * 1.121 Version 0.8.8
    * 1.122 Version 0.8.7
    * 1.123 Version 0.8.6
    * 1.124 Version 0.8.5
    * 1.125 Version 0.8.4
    * 1.126 Version 0.8.3
  * 2 Trunk
  * 3 Navigation



# Releases

## Next Version

  * fixed marking _library export function result_ sub elements as used
  * fixed parsing _var_ section after _class var_ section
  * fixed names of properties _MultilineStringsTrimLeft_ and _MultilineStringsEOLStyle_
  * fixed multilinestrings: 
    * combos like `abc`#10
    * double backticks become, same as double apostrophs become one in string literals
    * apostroph
  * fixed **anonymous procedure type**
  * fixed **anonymous record type**
  * fixed searching **TJSPromise** in global scopes, while context is a dot scope
  * fixed typecast array literal to _TJSArray_



## Version 2.2.0

22th Feb 2022 

## All changes of version 2.2.0

  * moved from svn to gitlab
  * New command line option: -Ja<x>: Append JS file <x> to main JS file. E.g. -Jamap.js. Can be given multiple times. To remove a file name append a minus, e.g. -Jamap.js-.
  * Added RTTI _TProcedureFlag_ **pfSafeCall** and **pfAsync**
  * pas2js now creates unique method pointers (**@SomeMethod**), e.g. the following code will now remove the _@OnLoad_ :


    
    
     xhr.addEventListener('load', @OnLoad);
     xhr.removeEventListener('load', @OnLoad);
    

  * Pascal **Library**
    * export for global functions and static methods
    * export for global variables
  * **{$linklib}** directive, see [pas2js modules](<pas2js_modules.md> "pas2js modules")
  * new -T platform **module**
  * new -T platform **electron**
  * mark record fields as used when passing record to a jsvalue parameter
  * $mode Delphi support for generic overloads, e.g. _TBird, TBird <T>, TBird<S,T>_. Can be used from a unit with mode ObjFPC too, although cannot yet be declared there.
  * fixed delay init specializations after loading impl sections
  * fixed specialize **try except on** , issue 38795
  * fixed _float / 0.0_ results at compiletime in _infinity_ instead of div by zero, issue 38815
  * fixed _low/high(aString)_
  * fixed typecast _jsvalue_ to _external class_ instance not object checking by default, only when object checks are enabled. For example _TJSString(aJSValue).length_.
  * fixed emulate compile time assign integer constant of different type
  * fixed **typeinfo** _Module, Attributes, ResultType_ and _Params_ now have default value **nil** , not _undefined_.
  * fixed cloning multi dim static array on assign
  * fixed stack overflow on deep nested binary expressions
  * fixed releasing com interface fields on destroy
  * fixed class property getter for array property of static method
  * fixed find generic proc overload without params, issue 38796
  * fixed calling constructor of nested external class, issue 38858
  * fixed await() as aclass, issue 39028
  * fixed consistent error message on custom js file not found, issue 38978
  * fixed writing sourceMappingURL only if map file enabled, issue 39210
  * fixed Setlength(unicodestring) issue 39208
  * fixed call type helper on type helper read from pcu



### 2.2.0 Incompatibilities

#### pas2js.cfg

  * The binary packages' file _pas2js.cfg_ now enables _-Jc_ (concatenate all js) by default. You can disable this with _-Jc-_.



## Version 2.0.6

15th Apr 2021 

  * fixed **insert(item,array,pos)** when _array=nil_
  * fixed published field with anonymous array
  * fixed -O- and record const, issue 38683
  * fixed multi add **a + ImplicitFuncCall + b**
  * (generics) Add comparer version of TDictionary create.
  * (generics) Fix wrong comparison of objects, adjusted patch by Henrique Werlang (Issue 38748)
  * (db) Patch from Henrique Werlang to implement TDatasetField
  * (rtti) Patch from Henrique Werlang to implement getting method parameters info
  * (db) Fix in TDataSet.DefaultBytesToBlobData, index out of range
  * (sysutils) TStringBuilder implementation
  * (arrayutils) Start of array utils
  * (web) Clipboard support (bug ID 0038726)
  * (sysutils) Use TBoolStrs for Boolean.ToString helper
  * (jsondataset) Publish OnRecordResolved, OnLoadFail
  * (db) Implement TBlobField.DisplayValue
  * (classes) Add ExtractStrings
  * (classes) StringStream.ReadString/WritString. Fix TMemoryStream.LoadFromStream.
  * (classes) Correctly handle CR/LF in GetNextLineBreak in TStrings.
  * demos: Fix demo to work again with latest webwidget
  * demos: Fixed demorouter example
  * (packages) new flatpickr lib
  * (sysutils) provide suitable defaults for Long- and ShortTimeFormat
  * (web) Patch from Henrique Werlang to implement TJSCSSStyleDeclaration
  * (web) Patch from Henrique Werlang to implement TJSElement.Remove
  * (classes) correctly stream TStrings based properties
  * (db) Patch from Henrique Werlang to let TDatalink transmit events ony when active
  * (sysutils) Introduce FormatSettings



## Version 2.0.4

26th Mar 2021 

  * fixed wrong setting _rtl.TObjectDestroy_
  * fixed no hint when published method hides ancestor method
  * fixed compileserver option **\--simpleserver=** and **-d** relative path
  * fixed stack overflow on long string concatenations
  * fixed filer restore global shortrefs



## Version 2.0.2

9th Mar 2021 

  * fixed freeing temporary class interface, if it is nil.
  * removed obsolete _TTypeInfoDynArray.DimCount_
  * fixed _TTypeInfoStaticArray.Dims_
  * fixed _obj as COMIntfType_ , when _obj_ is _nil': for FPC and Delphi compatibility nil returns nil and does not raise an exception._
  * fixed checking statement after _except-on_
  * fixed **if then asm a;b end**
  * fixed **ord(integer)**
  * fixed **Include(SetOfIntegerRange, integer)**
  * fixed omitting hint for not used property
  * fixed **var a: (b,c);**
  * fixed calling **Instance.StaticMethod** not using Instance
  * fixed fixed creating enum shortrefs for precompiled code for optimization _EnumValues_



## Version 2.0.0

11th Jan 2021 

[All changes of version 2.0.0](<Pas2JS_Version_Changes.md> "Pas2JS Version Changes")

  * fixed _TDictionary_ with procedural type
  * fixed _TTypeInfoRecord.RecordType_



## Version 2.0.0RC8

31th Dec 2020 

  * fixed UTF-16 char literal _#128..#255_
  * fixed using invalid UTF-16 but valid string _#$DC00'b'_
  * fixed checking class method modifiers match class interface (as in IUnknown)
  * fixed typecast specialized array to specialized type
  * fixed implicit call of specialized method
  * added separate hints **4501 field "x" not used** and **4502 field "x" assigned but never used**. Delphi/FPC do not have these hints, as they treat records as one memory block and can't omit fields. pas2js can omit fields.
  * The param in **await(param)** must now be an async function, to avoid accidentally calling the wrong function or not a function at all.
  * Allowing **await(T,jsvalue)** , because jsvalue could be a promise.
  * fixed typecast implicit function call passing to arg, e.g. _aFunc(aType(obj.bFunc))_
  * fixed delayed init specialized class interface



## Version 2.0.0RC7

13th Dec 2020 

  * fixed optimization _shortrefglobals_ and _enum value_
  * fixed filer storing unix line ending and writing precompiled code with platform line ending
  * fixed unit without implementatiom
  * fixed record member type



## Version 2.0.0RC6

8th Dec 2020 

  * the binary snapshot now use same directories as "make all". E.g. **bin\i386-win32** for the Windows 32bit executables.
  * the binary snapshot now contains the **makestub** utility
  * fixed filer, call type helper of unit, which unit implementation has not been parsed.
  * fixed catching load file exceptions and turn into regular errors
  * fixed generating typeinfo for classes with published members, but not referenced via typeinfo



## Version 2.0.0RC5

4th Dec 2020 

  * fixed skipping non fully specialized types
  * fixed filer, storing reference to _await_ and _debugger_
  * fixed filer, specialize signature of implementation of methods
  * fixed filer, skipping generic reference to generic type
  * fixed filer, reading inline specialize expression
  * fixed filer, checking signatures needing indirect units of implementation



## Version 2.0.0RC4

30th Nov 2020 

  * fixed **await** on **as** operator
  * fixed **await(ProcArgument)**
  * fixed hint _Await needs a promise_
  * fixed calling **async function** with result type _COM interface_
  * fixed checking '_await(T,callasyncfunc)_ type match
  * fixed **async** procedure modifier not needed in implementation of a procedure
  * added _FormData_ js keyword
  * fixed _shortrefglobals_ optimization of new/free instance fields
  * fixed crash on parser error in inline specialize expression, issue 38111
  * fixed typeinfo path of inline specialize type
  * fixed **-OoShortRefGlobals**
  * fixed error when using generic type without parameters
  * fixed shortrefglobals when unit initialization uses unit of otherwise empty implementation section



## Version 2.0.0RC3

13th Nov 2020 

  * fixed **typeinfo unicodestring** and **widechar**
  * fixed passing _widechar_ to _var argument char_ and _unicodestring_ to _string_ and vice versus.
  * rtl: system.pas: removed obsolete _UnicodeString=string_ and _WideChar=char_ type alias. They are normal compiler types.
  * fixed typecast integer to **widechar**
  * fixed **ord(widechar)**
  * added overload _TryStrToInt64_ with _int64_ , _TryStrToQWord_ with _QWord_ , _TryStrToUInt64_ with _UInt64_



## Version 2.0.0RC2

11th Nov 2020 

  * fixed _shortrefglobals_ for minimal class interface, issue 38042
  * fixed disable optimization using fpc syntax e.g. **-OoNoShortRefGlobals** , issue 3804
  * fixed _shortrefglobals_ for procedure without args, issue 38043
  * **-JRnone** now skips resource directives



## Version 2.0.0RC1

5th Nov 2020 

## All changes of version 2.0.0

  * the binary snapshots now use same directories as "make all". E.g. bin\i386-win32 for the Windows 32bit executables.
  * [Generics](<pas2js_Generics.md> "pas2js Generics")
  * **Attributes** : 
    * **Incompatibility** : **$modeswitch ignoreattributes** was removed
    * base class _System.TCustomAttribute_
    * **$modeswitch prefixedattributes** : enabled by default in _$mode Delphi_
    * declare in front of any type, type member
    * the Delphi compiler attributes like _ref, weak, volatile_ , etc are not supported.
    * query attributes of a type: 
      * use either typinfo function **GetRTTIAttributes(typeinfo(SomeType).Attributes)** :
      * or use RTTI unit _r:=TRTTIContext_ , _r.GetType(typeinfo(SomeType)).GetAttributes_
  * **class constructors** : 
    * for classes, records, class helpers, record helpers and type helpers.
    * Called in initialization section
    * Optimizer removes attributes if type is not used.
  * **VarArg.Free** , e.g. _procedure DoIt(var Arg: TObject); begin Arg.Free end;_ setting Arg to nil
  * range checking for type helpers
  * range checking for var/out arguments
  * fixed **low/high(nativeint)** for 53 significand bits instead of only 52 explicit bits, increasing high to _$1fffffffffffff = 9007199254740991_
  * float literal: removing unneeded 0 in front of E e.g. 1.20E1 as 1.2E1
  * **Overflow checks for integers** -Co _{$overflowchecks on}_ _{$Q+}_ for integer operators +, -, *. Checks if result is outside nativeint and raises EIntOverflow.
  * **class abstract** modifier
  * _type helper for classtype_ , same as FPC
  * _type helper for interfacetype_ , no constructors, same as FPC
  * Dispatch messages: 
    * method modifier **message integer** and **message string**
    * directive **{$DispatchField fieldname}** and **{$DispatchStrField fieldname}**



Insert these directives in front of your dispatch methods to let the compiler check all methods with message modifiers if they pass a record with the right field. 
    
    
      TMyComponent = class
        {$DispatchField Msg}
        procedure Dispatch(var aMessage); virtual;
        {$DispatchStrField MsgStr}
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
    

  * added rtti utility functions GetInterfaceProp, SetInterfaceProp, GetMethodProp, SetMethodProp
  * TStream with TBytes 
    * TBytesStream
  * convert ord(const) to a const
  * **Resource strings** : 
    * Resource strings can now be written to file. This is controlled by the -Jr option, see the main wiki page for [pas2js](<pas2js.md> "pas2js").
  * **TJSFunction(@obj.methodname)** is now converted to _classtype.methodname_
  * Constructors of external classes are now supported in four ways: 
    * _constructor New_ is translated to _new ExtClass(params)_. Note the missing JS path.
    * _constructor New; external name 'GlobalFunc'_ is translated to _new GlobalFunc()_. Note the missing JS path.
    * _constructor SomeName; external name '{}'_ is translated to _{}_ \- a basic, empty JS object
    * Otherwise it is translated to _new ExtClass.FuncName(params)_
  * You can now specify a type for the procedure modifier _varargs_ : **varargs of _SomeType_ **. For example


    
    
      procedure SumWords(); varargs of Word;
      ...
      SumWords(1,2,3); // this works
      SumWords(1,98765); // this gives a range check error
    

  * Srcmap for pju files
  * allowing _using a unit twice_ , with different names, e.g. _uses ns.unit1, unit1;_ or _uses foo in 'unit1.pas', unit1;_
  * option **-Sj** to allow/disallow typed const to be writable
  * option **-im** to show available modeswitches
  * option **-M <modeswitch>** to enable or disable a modeswitch, see option -im
  * typecasting unrelated classes now gives only a warning "Class types are not related" instead of an error, Reason: FPC/Delphi compatibility
  * mode ObjFPC now checks procedural types in procedure arguments by signature, reason: FPC compatibility. Note: FPC does that too in $mode delphi, pas2js only in mode ObjFPC.
  * type helper for **bytebool, wordbool, longbool**
  * _ArrayOfChar:=String_ and pass _string_ to _ArrayOfChar_
  * **safecall** calling convention and rtl catch uncaught [exceptions](<pas2js_exceptions.md> "pas2js exceptions").
  * [Async procedure modifier](<pas2js_AsyncAWait.md> "pas2js AsyncAWait") and await functions: 
    * _function await(AsyncFunctionOfResultT): T;_ // implicit promise
    * _function await(aType; p: TJSPromise): aType;_ // explicit promise requires the resolved type
    * _function await(aType; v: jsvalue): aType;_ // explicit optional promise requires the resolved type
  * Descending a Pascal class from a JS Function
  * The [makestub](<pas2js_makestub.md> "pas2js makestub") utility converts a Pascal import unit for importing Pascal classes to a unit that is compilable by Delphi
  * _fcl-json_ JSON interface from FPC.
  * New units for Javascript libraries: 
    * _libjitsimeet_ interface to jitsi video conferencing.
    * _libopentok_ interface to opentok video conferencing.
    * _libkurento_ interface to kurento video conferencing.
    * _libfullcalendar(4/5)_ interface to fullcalendar.io JS library
    * _libdatatables_ interface to datatables.net JS library
    * _pushjs_ interface to browser Push Notifications
    * _libbootstrap_ interface to bootstrap classes.
    * _gmaps_ interface to google maps interface.
  * added separate hints **4501 field "x" not used** and **4502 field "x" assigned but never used**. Delphi/FPC do not have these hints, as they treat records as one memory block and can't omit fields. pas2js can omit fields.
  * Optimizations: 
    * **-O2** : Level 2 optimizations (Level 1 + not debugger friendly)
    * **-OoShortRefGlobals[-]** : Insert a JS local var for each module, type and static function. Default enabled in _-O2_.



### 2.0.0 Incompatibilities

#### Signature for event handlers TEventListenerEvent and TJSEvent

The signature for event handlers has been corrected. The class TEventListenerEvent is now an alias for TJSEvent 

    That means that the 2 event handler types
    
    
     TJSEventHandler = reference to function(Event: TEventListenerEvent): boolean;
     TJSRawEventHandler = reference to Procedure(Event: TJSEvent);
    

    are now equivalent.

#### TJSEvent type definition

The TJSEvent type definition has been corrected 

    The currentTarget and Target properties of TJSEvent are now of type TJSEventTarget, as they are in the official specs:
    
    
      property currentTarget : TJSEventTarget Read FCurrentTarget;
      property target : TJSEventTarget Read FTarget;
    

    For convenience, a targetElement and CurrentTargetElement property have been added which are of type TJSElement:
    
    
       property currentTargetElement : TJSElement;
       property targetElement : TJSElement;
    

#### modeswitch ignoreattributes was removed

The workaround _{$modeswitch ignoreattributes}_ was removed. Attributes are now implemented. 

## Version 1.4.34

28th Oct 2020 

  * fixed passing in mode delphi a proc address to a proc type argument.
  * fixed searching pju file, when uses-in-file missing
  * fixed _a div b_ for negative results
  * fixed _aCurrency/bCurrency_ for negative results



## Version 1.4.32

15th Oct 2020 

  * fixed crash on class function
  * fixed array shrink using _SetLength_
  * fixed _try except on ExternalClass do ; end;_
  * fixed dynamic array function **a:=concat(b);** // marking b as referenced
  * fixed dynamic array function **c:=concat(a,b);**
  * fixed _$ancestor_ of all root classes is _null_ instead of _undefined_
  * fixed _rtl.spaceLeft_ return value



## Version 1.4.30

8th July 2020 

  * fixed assign record with field of dynamic array
  * fixed **SysUtils.AnsiSameText** comparing strings with umlauts.



## Version 1.4.28

3rd Jul 2020 

  * fixed **system.inc()**
  * fixed namespace search order 
    * Reason: Delphi/FPC compatibility.
    * Old behaviour: search default namespace (aka prepend program namespace), prepend command line namespaces, no namespace (i.e. as written in uses section)
    * New behaviour: search no namespace (i.e. as written in uses section), prepend command line namespaces, prepend default namespace (aka program namespace)
    * For example program that uses "classes" and units classes.pas and system.classes.pas are in unit search path.
  * fixed _try exit(value) finally read Result end_



## Version 1.4.26

6th Jun 2020 

  * fixed RTTI of record/class field with shared anonymous array, e.g. _type t = record a,b:array of byte; end;_



## Version 1.4.24

11th May 2020 

  * fixed assign array



## Version 1.4.22

10th May 2020 

  * fixed allowing member with same name as an ancestor member in $mode delphi
  * fixed allow **static** directive repetition in method implementation
  * fixed type helper for **NativeInt** and **NativeUInt**
  * fixed type helper **Self** in nested procedure
  * fixed **(i*i).helperfunc**
  * fixed **SetLength(array,..)** using resize instead of clone, when not shared. Assign marks an array with a hidden property _$pas2jsrefcnt_.



## Version 1.4.20

11th April 2020 

  * fixed stack overflow on long procedures
  * fixed class helper in with-do



## Version 1.4.18

15th Dec 2019 

  * access modifier **constref** : works as _const_ , gives a warning for non records, arrays
  * omit not used resourcestrings
  * fixed sourcemap for Firefox



## Version 1.4.16

15th Oct 2019 

  * fixed helper for type alias type
  * fixed selecting last declared helper
  * replaced rtl.setArrayLength with faster non recursive version
  * fixed external static class method
  * fixed check for helper class method for external class must be static
  * fixed marking implicit call in property parameter



## Version 1.4.14

30th Aug 2019 

  * source map: when using -Jmabsolute -Jmsourceroot=file:// prepend the absolute source files with _file://_. Needed by Firefox.
  * fixed longword bitwise operations _not, and, or, xor, shl, shr_ for numbers _> $7fffffff_
  * fixed creating relative paths without shared based directory, e.g. in source maps _C:\foo\project1.js.map C:\bar\project1.pas_



## Version 1.4.12

28th Aug 2019 

  * fixed get method reference of Self inside anonymous method without self
  * fixed typecast nil to class, interface, dynamic array
  * fixed endless loop on _type TArr = array of TArr_
  * no warning "function result not set" for fieldless record
  * fixed check for duplicate unit, when names differ
  * fixed _ComInterfaceInstance is/as InterfaceType_ to use _QueryInterface_
  * fixed passing dynamic array to open array var parameter



## Version 1.4.10

10th Jul 2019 

  * added separate error message duplicate published method
  * fixed allowing reintroduce published method
  * fixed _high(dynarrayvar with expr)_
  * fixed _class var a:t; b:t;_
  * fixed type helper in other unit



## Version 1.4.8

24th June 2019 

  * fixed var a: somearray = nil
  * fixed fixed assignment inside anonymous proc inside for-loop
  * fixed ArcTan definition (bug ID 35655)
  * setlength(arr) now always clone for FPC/Delphi compatibility. Formerly it merely resized the array.



## Version 1.4.6

20th Apr 2019 

  * fixed advanced records
  * fixed linux rtl.js



## Version 1.4.4

19th Apr 2019 

  * fixed duplicate identifier when redefining a procedure.
  * fixed escaping string literals in asm blocks



## Version 1.4.2

11th Apr 2019 

  * fixed optimization of NewInstance function
  * fixed filer class helper
  * fixed passing TExt.new as parameter
  * fixed writing workingdirectory
  * fixed advanced record constructor
  * handling environment options **PAS2JS_OPTS**
  * fixed nodejs _GetEnvironmentVariable_ returning empty string for non existing variables



## Version 1.4.0

24th Mar 2019 

  * fixed supporting older node.js (<7.0.0) using _Math.pow_ instead of newer exponential operator ******.



## Version 1.4.0RC7

16th Mar 2019 

  * fixed accessing Self in anonymous function
  * fixed _UntypedArg:=recordvar_ and _RecordType(UntypedArg)_
  * an external method of a helper is treated like an external method of the helped type
  * updated examples
  * fixed hint method hides identifier with same signature
  * fixed hint argument not used, with array argument and only writing an element
  * fixed hint argument not used, with array argument passed as argument



## Version 1.4.0RC6

8th Mar 2019 

  * fixed passing _class var_ to _var_ argument
  * fixed reading precompiled units by default



## Version 1.4.0RC5

6th Mar 2019 

  * fixed overload var arg and type alias
  * added overload TryStrToFloat with type extended
  * nativeint **shr** int
  * nativeint **shl** int
  * no hint when hiding private method
  * fixed passing multiple -vm parameters
  * fixed include file search in module directoy
  * allow typecast external class to unrelated external class in _$mode delphi_ , e.g. TJSEventTarget(aJSWindow). Normally you need _TJSEventTarget(TJSObject(aJSWindow))_.



## Version 1.4.0RC4

3rd Mar 2019 

  * fixed _{$warn identifier error}_
  * fixed **and/or/xor** with _nativeint_
  * warn on _nativeint shl/shr int_ using only 32bit
  * fixed type helper call as arg
  * fixed _(f*f).helpercall_
  * fixed info message _macro name set to "value"_
  * _make all_ now creates a default bin/targetcpu-targetos/pas2js.cfg, so that pas2js.exe works out-of-the box.



## Version 1.4.0RC3

27th Feb 2019 

  * renamed **$modeswitch multiplescopehelpers** to **multihelpers** , same as FPC
  * fixed _TAliasOfEnumType.EnumValue_
  * fixed emitting hints for not used units
  * fixed SetBufListSize



## Version 1.4.0RC2

18th Feb 2019 

  * fixed typecast jsvalue(anobject/interface), not doing getObject,
  * fixed _obj.Free_ set to _nil_ if already _nil_
  * added **unit websvg.pas** providing API to SVG elements
  * fixed _getdelimitedtext_ , quoting was wrong
  * fixed array-of-const references in precompiled units
  * Added some missing routines in strutils (containstext, containsstr)



## Version 1.4.0RC1

16th Feb 2019 

  * Highlights: 
    * class helpers
    * record helpers
    * type helpers
    * advanced records
    * array of const
    * added locate to JSONdataset, as well as lookup and support for lookup fields.
    * Added Indexes (sorting) to JSONDataset.


  * Incompatibilities: 
    * pas2js now searches units first in the folder of the current module as Delphi does.
    * JSArguments declaration was changed from an array of jsvalue to TJSFunctionArguments.



  


## All changes of version 1.4.0

  * Pas2js supports class helpers, record helpers and type helpers since 1.3. The extend is only virtual, the helped type is kept untouched. 
    * A **class helper** can "extend" Pascal classes and external JS classes.
    * A **record helper** can "extend" a record type. In $mode delphi a record helper can extend other types as well, see _type helper_
    * A **type helper** can extend all base types like integer, string, char, boolean, double, currency, and some user types like enumeration, set, range and array types. It cannot extend interfaces or helpers.
    * Type helpers are available by default in _$mode delphi_ and disabled in _$mode objfpc_. You can enable them with **{$modeswitch typehelpers}**.
    * By default only one helper is active per type, same as in FPC/Delphi. If there are multiple helpers for the same type, the last helper in scope wins. A class with ancestors can have one active helper per ancestor type, so multiple helpers can be active, same as FPC/Delphi. Using **{$modeswitch multihelpers}** you can activate all helpers within scope.
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
    * _inherited_ inside a method of a class/record calls helper of ancestor.
    * _inherited_ inside a helper depends on the $mode: 
      * _$mode objfpc_ : _inherited;_ and _inherited Name(args);_ work the same and searches first in HelperForType, then in ancestor(s).
      * _$mode delphi: inherited;_ : skip ancestors and HelperForType, searches first in helper(s) of ancestor of HelperForType.
      * _$mode delphi: inherited name(args);_ : same as $mode objfpc first searches in HelperForType, then Ancestor(s)
      * In any case if _inherited;_ has no ancestor to call, it is silently ignored, while _inherited Name;_ gives an error.
    * **RTTI** : _typeinfo(somehelper)_ returns a pointer to _TTypeInfoHelper_ with _Kind tkHelper_.
    * There are some special cases when using a **type helper** function/procedure on a value: 
      * _function result_ : using a temporary variable
      * _const, const argument_ : When helper function tries to assign a value, pas2js raises a EPropReadOnly exception. FPC/Delphi use a temporary variable allowing the write.
      * _property_ : uses only the getter, ignoring the setter. This breaks OOP, as it allows to change fields without calling the setter. This is FPC/Delphi compatible.
      * _with value do ;_ : uses a temporary variable. Delphi/FPC do not support it.
  * built-in function **concat** _(string1,string2,...)_
  * local types (declared inside functions) are now created in the global scope
  * added locate to JSONdataset, as well as lookup and support for lookup fields.
  * Added Indexes (sorting) to JSONDataset.
  * Added unit fpexprpars.pas, an expression parser unit (needed for dataset filtering...)
  * omit unnecessary brackets on associative operations (a||b)||(c||d), (a&&b)&&(c&&d), (a|b)|c, (a&b)&c, (a^b)^c, (a+b)+c, (a-b)-c, (a*b)*c
  * **records are now created as _Object_ , instead of JS _function_ **
    * records now have hidden (not enumerable) functions _$new, $assign, $clone, $eq_
    * passing records to _var argument_ now passes the record directly instead of creating a temporary setter
    * **Assigning a record** , e.g. _aRecord:=value_ , now copies the values, while keeping the JS object. This makes _pointer of record_ Delphi/FPC compatible.
  * **Advanced records** : enabled in $mode delphi, disabled im $mode objfpc, enable with **{$modeswitch AdvancedRecords}**
    * visibility private, strict private, public, default is public
    * methods, class methods (must be static like in FPC/Delphi), constructors
    * class vars
    * const
    * property, class property, array property, default array property
    * nested types
    * RTTI
  * Records can now have external fields with '[2]', '["a b"]'
  * In $mode objfpc forward class-of and pointer declarations can now refer to types even if there are other const/var/resourcestring sections in between. Same a FPC. For example:


    
    
    type 
      TClassOfBird = class of TBird;
    const k = 1;
    type 
      TBird = class end;
    

  * **lo(), hi()** in $mode delphi returning the lo/hi byte, in $mode objfpc returning the lo/hi byte|word|longword.
  * char range with non ascii literals: 'Б'..'Я'
  * typecast char to word and other integers
  * added option -Jmabsolute to store absolute filenames in sourcemaps
  * **JSArguments declaration was changed** from an array of jsvalue to TJSFunctionArguments.
  * fixed expr[][] with default properties
  * fixed assigning class vars
  * pas2js now searches units first in the folder of the current module as Delphi does.
  * fixed case-of with non ascii literals
  * implemented **ProcVar:=StaticClassMethod**
  * implemented **ProcVar:=ClassMethod** inside static class method
  * class property getter/setter can now be static or non static.
  * **array of const** : 
    * Works the same: vtInteger, vtBoolean, vtPointer, vtObject, vtClass, vtWideChar, vtInterface, vtUnicodeString
    * longword is converted to vtNativeInt instead of mangling to vtInteger
    * vtExtended is double, Delphi/FPC: PExtended
    * vtCurrency is currency, Delphi/FPC: PCurrency
    * Not supported: vtChar, vtString, vtPChar, vtPWideChar, vtAnsiString, vtVariant, vtWideString, vtInt64, vtQWord
    * only in pas2js: vtNativeInt, vtJSValue
  * fixed _o.ProcVar()_ when ProcVar is typeless property
  * fixed const evaluation float - currency
  * fixed reading #$00xx as widechar, bug 34923
  * fixed relative paths in srcmap in Windows
  * nicer error message on invalid set element type



## Version 1.2.0

  * 23th Dec 2018
  * SVN release tag is <https://svn.freepascal.org/svn/projects/pas2js/tags/release_1_2_0>
  * added **dataabstract** : support for Remobjects Data Abstract: The TDAConnection and TDADataset classes
  * set rtl.js version to 10200
  * fixed parsing comment in $IFDEF, $IFNDEF, issue 34711
  * fixed searching unit



## Version 1.2.0RC1

  * 16th Dec 2018
  * SVN release tag is <https://svn.freepascal.org/svn/projects/pas2js/tags/release_1_2_0RC1>
  * **Anonymous Functions** :


    
    
    type
      TRefProc = reference to procedure;
      TProc = procedure;
    procedure DoIt(arg: TRefProc);
    var 
      ref: TRefProc;
      proc: TProc;
    begin
      ref:=procedure begin end; // assign to "reference of procedure" type
      ref:=procedure  // note the omitted semicolon
        var i: integer; // var, types, const, local procedures
        begin
        end; 
      DoIt(procedure begin end); // pass as argument
      refproc:=procedure assembler asm // embed JavaScript
          console.log("foo");
        end; 
      // Note that typecasting to non "reference to" does not make a difference 
      // because in JS all functions are closures:
      proc:=TProc(procedure begin end);   
    end;
    

  * added variable _rtl.version_ , which corresponds to the compiler version Major*10000+Minor*100+Release
  * added option **-JoCheckVersion** : 
    * -JoCheckVersion- : do not add rtl version check, default.
    * -JoCheckVersion=main : insert rtl.checkVersion() into main.
    * -JoCheckVersion=system : insert rtl.checkVersion() into system unit.
    * -JoCheckVersion=unit : insert rtl.checkVersion() into every unit.
  * moved classtopas function to a class2pas unit, improved the interface so it uses a stringlist. Adapted the demo.
  * Fix System.Int() so it also works on IE (where Math.trunc is missing).
  * allow typecasting _string(apointer)_ and _pointer(astring)_
  * asm-block now skips Pascal comments //... and Pascal string literals with single quotes. It no longer stops at _end_ in such comments and string literals.
  * Fix FormatFloat() rounding logic (actually FloatToDecimal)
  * Fix stringofchar for count<=0
  * Fix quotestring and add quotedstr
  * implemented special includes like **{$i %date%}:**
    * %date%: current date as string literal, '[yyyy/mm/dd]'
    * %time%: current time as string literal, 'hh:mm:ss' Note that the inclusion of %date% and %time% will not cause the compiler to recompile the unit every time it is used: the date and time will be the date and time when the unit was last compiled.
    * %line%: current source line number as string literal, e.g. '123'
    * %linenum%: current source line number as integer, e.g. _123_
    * %currentroutine%: name of current routine as string literal
    * %pas2jstarget%, %pas2jstargetos%, %fpctarget%, %fpctargetos%: target os as string literal, e.g. 'Browser'
    * %pas2jstargetcpu%, %fpctargetcpu%: target cpu as string literal, e.g. 'ECMAScript5'
    * %pas2jsversion%, %fpcversion%: compiler version as string literal, e.g. '1.0.2'
    * If param is none of the above it will use the environment variable. Keep in mind that depending on the platform the name may be case sensitive. If there is no such variable an empty string is inserted.
  * implemented _pred(char)_ , _succ(char)_
  * allow typecasting _TypedPointer(UntypedPointer)_
  * allow assign _UntypedPointer:=TypedPointer_
  * skip double quotes in asm-blocks, e.g. _s = "end"+"'";_
  * added **{$modeswitch OmitRTTI}** : treat class section 'published' as 'public' and _typeinfo()_ does not work on symbols declared with this switch. This allows to easily disable generating RTTI and allows the optimizer to omit unused published properties.
  * added option **-Jpcmd <command>**: Run postprocessor. For each generated js execute command passing the js as stdin and read the new js from stdout. This option can be added multiple times to call several postprocessors in succession. Quote the <command> to add options for the postprocessor. For an example see [minifier](<pas2js_minifier.md> "pas2js minifier").
  * Added built-in procedure **Debugger;** , which is converted to the JavaScript statement _debugger;_. If a debugger is running it will break on this line just like a break point.
  * Changed operator precedence level of **is** to same as **and** , **or** , **xor**. Same as fpc/delphi.
  * fixed assert to raise on false, bug 34643
  * built-in _procedure**val**(const string; out enum; out int)_
  * added option _-JoRTL- <x>=<y>_ to change the name of a autogenerated identifier. See the list of available identifiers with _-iJ_.



## Version 1.0.4

  * 14th Nov 2018
  * SVN release tag is <https://svn.freepascal.org/svn/projects/pas2js/tags/release_1_0_4>
  * fixed calling _destructor_ after exception in _constructor_
  * fixed initializing static array of record
  * fixed parsing _if expr then raise else_
  * fixed local record and enum types
  * fixed _for e in set do_
  * fixed for-in of shared sets
  * fixed _inc(classvar)_
  * fixed assigning _class var_ of descendant classes
  * fixed error position on include file not found
  * fixed loading include file from cache
  * fixed range check of _o.aString[index]_ and _o.aArray[index]_
  * fixed Result:=inherited;
  * fixed escaping invalid UTF-16 in string literals
  * fixed not generating octal literals in ECMAScript5, it bites strict mode
  * fixed IsNaN on ECMAScript6
  * fixed error on method in record
  * fixed name clash published property and external
  * fixed _str(aCurrency)_
  * catch _ECompilerTerminate_ while parsing params
  * _sLineBreak_ and _LineEnding_ are now **var** under platform NodeJS



## Version 1.0.3

  * 28th Oct 2018
  * SVN release tag is <https://svn.freepascal.org/svn/projects/pas2js/tags/release_1_0_3>
  * fixed _char(#10)_
  * fixed: allow array property accessor argument mismatch const/default for simple types, e.g.:


    
    
    function GetItems(const i: integer): byte;
    property Items[i: integer]: byte read GetItems;
    

  * fixed _high(intvar)_
  * fixed WPO when using record constants
  * fixed _include(FuncResultSet,enum)_
  * fixed _if then <empty> else <something>;_
  * fixed _p^.x:=_
  * fixed _acurrency:=aninteger_ to become _acurrency:=aninteger*10000_
  * fixed _integer(acurrency)_ to become _Math.floor(acurrency/10000)_
  * fixed calling _$final_ , clearing references on destroy
  * fixed not calling _BeforeDestruction_ on exception in constructor
  * fixed _$class_ be a property of the class, not the object
  * fixed local var modifier _absolute_ in method
  * fixed using external const in const expression, e.g. _const tau = 2*pi;_
  * fixed selecting procedure overload, preferring lossy int over int to float
  * fixed calling Free inside method
  * fixed System.Int() so it also works on IE (where Math.trunc is missing).
  * fixed FormatFloat() rounding logic (actually FloatToDecimal)
  * fixed stringofchar for count<=0
  * fixed quotestring and add quotedstr



## Version 1.0.2

  * 26 Sep 2018
  * SVN release tag is <https://svn.freepascal.org/svn/projects/pas2js/tags/release_1_0_2>
  * fixed multiple class interface maps



## Version 1.0.1

  * 19 Sep 2018
  * SVN release tag is <https://svn.freepascal.org/svn/projects/pas2js/tags/release_1_0_1>
  * fixed generating srcmap for precompiled javascript



## Version 1.0.0

  * 9 Aug 2018
  * SVN release tag is <https://svn.freepascal.org/svn/projects/pas2js/tags/release_1_0_0>
  * SVN fixes branch is <https://svn.freepascal.org/svn/projects/pas2js/branches/fixes_1_0>
  * fixed override method of class interface
  * fixed crash on checking body element of empty proc
  * use default source filename in pcu/pju files



## Version 1.0.0rc1

  * 24 Jul 2018
  * fixed _TObject.Create()_
  * -vd shows stacktraces



## Version 0.9.32

  * 17 Jul 2018
  * fixed memory leaks and double frees



## Version 0.9.31

  * 10 Jul 2018
  * fixed aIntSet:=[0]
  * fixed crash on **List.Items.Dummy**
  * fixed some mem leaks
  * fixed -MDelphi + $mode objfpc + overloads withouts overload keyword
  * **TGUIDString** is now **type string**
  * RTL: added websockets



## Version 0.9.30

  * 4 Jul 2018
  * fixed calling COM interface _Release function for expressions.
  * fixed catching exception in pju variant of pas2js
  * split reserved words into two categories: the compiler now checks global JS identifiers like "Date" only for identifiers without path. That means a local variable _Date_ will be renamed, a _property Date_ will not, keeping the RTTI name of properties. Identifiers like _apply_ are still renamed.
  * fixed property RTTI for alias type in other unit
  * fixed default value of integer variables, using 0 even if it is outside range. Reason: Delphi/FPC compatibility.



## Version 0.9.29

  * 29 Jun 2018
  * fixed pcu reading alias type
  * fixed analyzer: element needing typeinfo marks indirect elements as used normally
  * hint for text after final "end.", disable with $warn GARBAGE off
  * $warn BOUNDS_ERROR off: disable range check warnings at compile time
  * $warn MESSAGE_DIRECTIVE off: disable $message notes



## Version 0.9.28

  * 27 Jun 2018
  * fixed static array of char = stringlit+stringlit
  * fixed libpas2js disk full error
  * $warn directive: **{$warn identifier on|off|default|error}** , identifier can be a message number as shown with -vq or one of the following. Note, that some hints like "Parameter %s not used" are currently using the enable state at the end of the module, not the state at the hint source position. 
    * CONSTRUCTING_ABSTRACT: Constructing an instance of a class with abstract methods.
    * IMPLICIT_VARIANTS: Implicit use of the variants unit.
    * NO_RETVAL: Function result is not set
    * SYMBOL_DEPRECATED: Deprecated symbol
    * SYMBOL_EXPERIMENTAL: Experimental symbol
    * SYMBOL_LIBRARY
    * SYMBOL_PLATFORM: Platform-dependent symbol
    * SYMBOL_UNIMPLEMENTED: Unimplemented symbol
    * HIDDEN_VIRTUAL: method hides virtual method of ancestor



## Version 0.9.27

  * 26 Jun 2018
  * mode delphi: fixed passing static array to open array
  * allow assigning aTypeInfo:=pointer, needed by units with and without using unit typinfo



## Version 0.9.26

  * 25 Jun 2018
  * **typecast function address to JS function** , e.g. TJSFunction(@IntToStr)
  * **typecast function reference to JS function** , e.g. TJSFunction(OnClick)
  * **typecast method address to JS function** , e.g. TJSFunction(@List.Sort). Note that a method address creates a function wrapper to bind the Self argument.
  * changed some types to type alias: TDateTime, TDate, TTime, Single, Real, Comp, UnicodeString, WideString. This only effects typinfo and some compiler error messages. Reason: Delphi/FPC compatibility.
  * In mode delphi dynamic array initializations must now use square brackets instead of round brackets. For example _var a: array of integer = [1,2];_. Reason: Delphi compatibility.
  * Assignation using constant array. For instance **Arr:=[1,2,3]** , where Arr is a dynamic array.
  * **\+ operator for arrays** : This concatenation operator is available using the new **modeswitch arrayoperators** , which is enabled by default in mode delphi.
  * unit webgl updated
  * unit webaudio added
  * unit webbluetooth added
  * db unit: ftDataset field type added (not supported)
  * JSONDataset unit: TField.OldValue now works for TJSONDataset
  * **webidl2pas** tool added. A command line tool to create Pascal units from idl specs.
  * allow **external record fields**.
  * hint 5024 'Parameter "%s" not used' was split into two: 
    * 4501 for virtual/override methods
    * 5024 for others



## Version 0.9.25

  * 7 Jun 2018
  * **TComponent now supports IInterface**
  * **-o** is now always **relative to working directory** , even if -FU or -FE is given. Reason: FPC compatibility
  * fixed typeinfo(typeintegerrange)
  * fixed using interface ancestor methods
  * fixed 'make' building with fpc 3.0.4



## Version 0.9.24

  * 2 Jun 2018
  * fixed precompiled js formatting to same options as compiled js
  * fixed unit analyzer private method used by protected property
  * fixed class-of RTTI
  * fixed precompiled resolve pending scopes before pending units



## Version 0.9.23

  * 28 May 2018
  * added webgl demos from Ryan Joseph
  * **for value in jsarray do** \- where jsarray is any external class with a matching _length_ and default property. This enumerates similar to other arrays the values, not the index.
  * allow **typecast array to TJSObject**
  * allow **typecast TJSObject to array**
  * Unicode **character constants outside of BMP** , e.g. #$10437
  * allow **{$H+}** , error on {$H-}
  * added intrinsic procedure **WriteStr(out s: string; params...)** , which works similar to _str(param,s)_ , except it can take any amount of parameters, which are concatenated.
  * added option **-Sm** to enable macro replacements



## Version 0.9.22

  * 17 May 2018
  * Fixed _$00ff00_



## Version 0.9.21

  * 16 May 2018
  * added option **-FE** : set the main output path, used for the main .js file, if there is no -o option or the -o option has no folder.
  * fixed WPO typeinfo of inherited property
  * for easier FPC integration the following message IDs were changed: 
    * nVirtualMethodXHasLowerVisibility = 3250; // was 3050
    * nConstructingClassXWithAbstractMethodY = 4046; // was 3080
    * nNoMatchingImplForIntfMethodXFound = 5042; // was 3088
    * nSymbolXIsDeprecated = 5043; // was 3062
    * nSymbolXBelongsToALibrary = 5065; // was 3061
    * nSymbolXIsDeprecatedY = 5066; // 3063
    * nSymbolXIsNotPortable = 5076; // was 3058
    * nSymbolXIsNotImplemented = 5078; // was 3060
    * nSymbolXIsExperimental = 5079; // was 3059



## Version 0.9.20

  * 11 May 2018
  * fixed passing typecasted alias type to var parameter
  * forbid typecast rectordtype to other recordtype
  * remove leading zeroes in number literals
  * added **namespace option -FN <x>**, **marked -NS as obsolete**. Reason: fpc compatibility.
  * added option **-vz** : write messages to stderr, -o. still uses stdout.
  * added option **-ic** : Write list of supported JS processors usable by -P<x>
  * added option **-io** : Write list of supported optimizations usable by -Oo<x>
  * added option **-it** : Write list of supported targets usable by -T<x>
  * added option **-SIcom** , **-SIcorba** interface style
  * added option **-vv** : Write pas2jsdebug.log with lots of debugging info
  * fixed option -va including option -vt
  * fixed combination of -Jc -o.
  * option -vt now writes used unit scopes
  * **external class fields with brackets**. e.g. _X: nativeint external name '[0]'_
  * autogenerated interface GUIDs now consider the unitname



## Version 0.9.19

  * 2 May 2018
  * **type alias type** , e.g. _type TCaption = type string;_
  * **record const** , e.g. _const p: TPoint = (x:1; y:2);_
  * **{$WriteableConst on|off}** : treat typed constants as readonly, e.g. _const i:byte=3;... i:=4;_ creates a compile time error.
  * forbid assignment of for-loop variable
  * **nested classes**
  * **property specifier nodefault**
  * **default(type)** returning the initial value of the type, e.g. default(recordtype).
  * **external typed const** : e.g. _const NaN: Double; external name 'NaN';_
  * **case string of** support for ranges
  * fixed typecast shortint(integer)



## Version 0.9.18

  * 26 Apr 2018
  * Fixed rcArrR in rtl.js



## Version 0.9.17

  * 25 Apr 2018
  * Fix recno calculation for TJSONDataset
  * Fix initializing next buffers in TDataset.
  * Implemented **Currency** as double, values are multiplied by 10000 and truncated, so a 2.7 is stored as 27000.
  * Implemented **pointer of record**. It's simply a reference. 
    * _p:=@r_ translates to _p=r_
    * _p^.x_ becomes _p.x_
    * intrinsics new(PointerOfRecord), dispose(PointerOfRecord). dispose(p) sets p to null if possible.
  * **enumerator for jsvalue** : e.g. _var v: jsvalue; key: string;**for key in jsvalue do** _ translates to _for (key in jsvalue){}_
  * **enumerator for external class** : e.g. _var o: TJSObject; key: string; for key in o do_ translates to _for (key in o){}_
  * **range checking $R+**
    * compile time: warnings become errors
    * run time: int:=, int+=, enum:=, enum+=, intrange:=, intrange+=, enumrange:=, enumrange+=, char:=, charrange:=
    * run time: parameters: int, enum, intrange, enumrange, char, charrange
    * run time: array[index], string[index]
  * **type cast integer to integer** , e.g. _byte(aLongInt)_
    * with range checking enabled: error if outside range
    * without range checking: emulates the FPC/Delphi behaviour: e.g. _byte(value)_ translates to _value & 0xff_, _shortint(value)_ translates to _value & 0xff <<24 >>24_.
  * case-of statement: error on duplicate values
  * fixed error on int:=double



## Version 0.9.16

  * 21 Apr 2018
  * mode delphi: allow "ObjVar is IntfType" and "ObjVar as IntfType" with unrelated types.
  * TVirtualInterface - create an implementation at runtime. rtl unit rtti.pas
  * typecast a class type to JS Object, e.g. TJSObject(TObject)
  * typecast an interface type to JS Object, e.g. TJSObject(IUnknown)
  * typecast a record type to JS Object, e.g. TJSObject(TPoint)
  * _not jsvalue_ is converted to _!jsvalue_
  * "=" operator for records with static array fields
  * changed TGuid to record
  * TGUID record 
    * GuidVar:='{guid}', StringVar:=GuidVar, GuidVar:=IntfTypeOrVar, GuidVar=IntfTypeOrVar, GuidVar=string
    * pass IntfTypeOrVar to GuidVar parameter
  * added new type TGuidString to system unit 
    * GuidString:=IntfTypeOrVar, GuidString=IntfTypeOrVar
    * pass IntfTypeOrVar to GuidString parameter
  * added option -JoUseStrict, to enable or disable adding "use strict"



## Version 0.9.15

  * 8 Apr 2018
  * fixed crash when ancestor implements more interfaces than current class.



## Version 0.9.14

  * 8 Apr 2018
  * fixed duplicates in rtl/strutils.pas
  * fixed reference counts on reading element lists
  * implemented using function result variable in for-loop. e.g. _for Result:=..._ , _for Result in ..._
  * fixed _for string in arrayofstring do_
  * fixed $scopedenums with anonymous enumtype. e.g. _type TSet = set of (A,B);_
  * implemented class interfaces: 
    * methods, properties, default property
    * {$interfaces com|corba|default} 
      * COM is default, default ancestor is IUnknown (mode delphi: IInterface), managed type, i.e. automatically reference counted via _AddRef, _Release, the checks for support call QueryInterface
      * CORBA: lightweight, no automatic reference counting, no default ancestor, fast support checks.
    * inheriting
    * GUIDs are simple string literals, TGUID = string.
    * An interface without a GUID gets one autogenerated from its name and method names.
    * a class implementing an interface must not be external
    * a ClassType "supports" an interface, if it itself or one of its ancestors implements the interface. It does not automatically support an ancestor of the interface.
    * method resolution, _procedure IUnknown._AddRef = IncRef;_
    * delegation: _property Name: interface|class read Field|Getter implements AnInterface;_
    * is-operator: 
      * IntfVar is IntfType - types must be releated
      * IntfVar is ClassType - types can be unrelated, class must not be external
      * ObjVar is IntfType - can be unrelated
    * as-operator 
      * IntfVar as IntfType - types must be releated
      * IntfVar as ClassType - types can be unrelated, nil returns nil, invalid raises EInvalidCast
      * ObjVar as IntfType - mode delphi: types must be related, objfpc: can be unrelated, nil if not found, COM: uses _AddRef
    * typecast: 
      * IntfType(IntfVar) - must be related
      * ClassType(IntfVar) - can be unrelated, nil if invalid
      * IntfType(ObjVar) - mode delphi: must be related, objfpc: can be unrelated, nil if not found, COM: if ObjVar has delegate uses _AddRef
      * TJSObject(intfvar)
    * Assign operator: 
      * IntfVar:=nil
      * IntfVar:=IntfVar2 - IntfVar2 must be same type or a descendant
      * IntfVar:=ObjVar - nil if unsupported
      * jsvalue:=IntfVar
    * Assigned(IntfVar)
    * RTTI
    * $modeswitch ignoreinterfaces was removed
    * Not supported: array of interface, interface as record member



## Version 0.9.13

  * 21 Mar 2018
  * fixed keeping methods AfterConstruction, BeforeDestruction
  * fixed modeswitch ignoreinterfaces



## Version 0.9.12

  * 19 Mar 2018
  * fixed parsing procedure p(var a; b: t)
  * fixed checking duplicate implementation of unit interface procedure



## Version 0.9.11

  * 16 Mar 2018
  * fixed loading files encoded in non UTF-8, e.g. UTF-8 with BOM. This was a regression.



## Version 0.9.10

  * 13 Mar 2018
  * fixed renaming overloads of unit interface at end of interface, not at end of module. Needed for unit cycles.



## Version 0.9.9

  * 13 Mar 2018
  * Fixed libpas2js to use WorkingDir parameter instead of GetCurrentDir
  * Fixed static array clone function.



## Version 0.9.8

  * 9 Mar 2018
  * fixed optimizer keep overrides



## Version 0.9.7

  * 7 Mar 2018
  * fixed pass local variable _v_ as argument to a var parameter.



## Version 0.9.6

  * 6 Mar 2018
  * static arrays are now cloned on assignment or when passed as argument to a function (no const, var, out)
  * Fixed array[enum..enum] of
  * pas2jslib: fixed checks of directoryexists to use cache
  * uses with _in_ -filename: In _$mode delphi_ the in-filenames are only allowed in the program and the unitname must fit the filename, e.g. _uses unit1 in 'sub/Unit1.pas'_. In _$mode objfpc_ units can use in-filenames too and alias are allowed, e.g. _uses foo in 'bar.pas'_.



## Version 0.9.5

  * 12 Feb 2018
  * Error on duplicate forward class
  * Error on method class in other unit
  * Fixed index property override



## Version 0.9.4

  * 8 Feb 2018
  * Removed debug writeln, fixing Disk Full errors in libpas2js.
  * Nicer error messages on illegal qualifier.



## Version 0.9.3

  * 4 Feb 2018
  * Fixed number literals outside int64.
  * Shorten float numbers, e.g. 1.00000E+001 to 10
  * unexpected exception now sets ExitCode to 1



## Version 0.9.2

  * 4 Feb 2018
  * Fixed -constant, when constant is a negative number



## Version 0.9.1

  * 3 Feb 2018
  * Fixed class const evaluating expression.



## Version 0.9.0

  * 31 Jan 2018
  * Fixed srcmap header. It must be )]}' to work in Firefox. Added option -JmXSSIHeader to exclude or include the XSSI protection header.
  * Ignore procedure modifier "inline"
  * Char(int)
  * search units and include files case insensitive by default. Enable FPC like search with parameter -JoSearchLikeFPC.
  * Const in external classes: 
    * _const c: type = value, const c = value_ are translated to the value.
    * _const c: type, class const c: type_ are treated like readonly variables.



## Version 0.8.45

  * 23 Jan 2018
  * is-operator: jsvalue is class-type, jsvalue is class-of-type
  * assertions: -Sa, $C+|-, $Assertions on|off, Assert(boolean), Assert(boolean,string)
  * object checks: -CR, $ObjectChecks on|off, check method calls, check object type casts
  * some bugfixes for alias types
  * multi dimensional static array const, e.g. array[1..2,3..4] of byte = ((5,6),(7,8))
  * fixed overloads when skipping class interface
  * fixed s[i]:= when s is a var parameter



## Version 0.8.44

  * 15 Jan 2018
  * State of directives $Hints|Notes|Warnings on|off at end of procedure is used for analyzer hints, e.g. "local variable x not used".
  * Fixed re-reading directories after Reset.



## Version 0.8.43

  * 4 Jan 2018
  * Fixed -Fi include path
  * Read directories instead of checking every single file. Added hooks for ReadDir to libpas2js.



## Version 0.8.42

  * 25 Dec 2017
  * var absolute modifier for local variables
  * $scopedenums
  * $hint, $note, $warn, $error, $fatal, $message text
  * $message hint|note|warn|error|fatal text
  * $hints, $notes, $warnings on|off



## Version 0.8.41

  * 25 Dec 2017
  * Enumerators: 
    * ordinal types: char, boolean, byte, ..., longword, enums, sets, static array, custom range
    * const set
    * variables: set, string, array
    * class GetEnumerator
    * It does not support operator enumerator, IEnumerator, member modifier enumerator.



## Version 0.8.40

  * File read callback for pas2jslib



## Version 0.8.39

  * 14 Dec 2017
  * fixed circular unit dependencies



## Version 0.8.38

  * 12 Dec 2017
  * support for * and ? in search paths
  * fixed converting a typecast to an alias proc type
  * fixed inherited-identifier-as-expr
  * emit warning method-hides-method-in-base-type only for virtual methods
  * reduced function hides identifier from level hint to info
  * fixed unit contnrs to always use mode objfpc.



## Version 0.8.37

  * 5 Dec 2017
  * Bugfixed a combination of overload/override



## Version 0.8.36

  * 5 Dec 2017
  * fixed missing brackets in binary expression and left side has a call (a-f(b)) / (c-d)



## Version 0.8.35

  * 20 Nov 2017
  * fixed a bug in the overload code



## Version 0.8.34

  * 19 Nov 2017
  * fixed skipping attributes behind procedure declarations.
  * Procedures/methods now properly hides procs with same name.
  * In mode delphi overloads now always require the 'overload' modifier.
  * In mode objfpc the modifier is required when using different scopes.
  * hints for hiding identifiers of other units.
  * implemented system.built-in-identifier.



## Version 0.8.33

  * 14 Nov 2017
  * srcmaps with included sources now ignores untranslatable local paths and simply uses the full local path.
  * custom enum ranges, e.g. TBlobType = ftBlob..ftBla
  * custom integer ranges, e.g. TSome = 1..5
  * custom char ranges
  * set of custom enum/integer/char ranges
  * the conversion of the for-to-do loop has changed. If the loop is never executed, the loop variable is not touched. And the start expression is now executed before the end expression.



## Version 0.8.32

  * 8 Nov 2017
  * some bug fixes for warnings



## Version 0.8.31

  * 29 Oct 2017
  * bugfix for implicit function calls of parameters of some built in functions.



## Version 0.8.30

  * 19 Oct 2017
  * nicer "can't find unit" position
  * fixed a crash parsing uses clause



## Version 0.8.29

  * 16 Oct 2017
  * bugfixes
  * it now supports directive $M alias $TypeInfo



## Version 0.8.28

  * fixed passing static array



## Version 0.8.27

  * 4 Oct 2017
  * implemented resourcestrings
  * implemented logical xor
  * fixed class-of-typealias
  * fixed property index modifier expression



## Version 0.8.26

  * 3 Oct 2017
  * fixed RTTI for static arrays
  * implemented property modifier index
  * implemented FuncName:=



## Version 0.8.25

  * 1 Oct 2017
  * bugfixes
  * a new modeswitch ignoreattributes to ignore attributes.



## Version 0.8.24

  * 28 Sep 2017
  * implemented multi dimensional SetLength
  * fixed keeping old values when using SetLength
  * fixed method override of override



## Version 0.8.23

  * 27 Sep 2017
  * property default value for sets
  * custom integer ranges, like TValueRelationship
  * typecast enums to integer type (same as ord function)
  * new modeswitch ignoreinterfaces to parse class interfaces, but neither resolve nor convert them. Using them will cause an error.



## Version 0.8.22

  * 24 Sep 2017
  * fixed loading dotted units
  * implemented property stored and default modifiers



## Version 0.8.21

  * 21 Sep 2017
  * fixed aString[index]:=
  * fixed analyzer to mark default values of arguments
  * many improvements for sourcemaps making step-over/into nicer in Chrome.
  * new tool fpc/packages/fcl-js/examples/srcmapdump to dump the produced sourcemap.
  * unicodestring and widechar are now declared by the compiler instead of system.pas



## Version 0.8.20

  * 14 Sep 2017
  * Static array const are now implemented. For example:
  * array['a'..'d'] of integer = (1,2,3,4);
  * array[1..3] of char = 'pas';



## Version 0.8.19

  * 12 Sep 2017
  * several bug fixes
  * started static arrays: 
    * array[2..6] of char
    * array['a'..'z'] of char
    * array[boolean] of longint
    * array[byte] of string
    * array[enum] of longint
    * array[char] of boolean // Note that char is widechar!
    * low(), high()



## Version 0.8.18

  * 6 Sep 2017
  * Anonymous arrays in record members are now supported:


    
    
      TFloatRec = Record
         ...
         Digits: Array Of Char;
      End;
    

## Version 0.8.17

  * 3 Sep 2017
  * checks for semicolons between statements
  * fixed proc type of procedure in Delphi mode
  * implemented @@ operator for proc types in Delphi mode.
  * compile time evaluation, range and overflow checking for boolean, base integer types, enums, sets, custom integer ranges, char, widechar, string, single and double.



## Version 0.8.16

  * 28 Jul 2017
  * bugfix release.



## Version 0.8.15

  * 8 Jul 2017
  * compiler can now generate source maps when passing option -Jm



## Version 0.8.14

  * 17 May 2017
  * now supports TObject.Free.



In Delphi/FPC obj.Free works even if obj is nil. In JavaScript this would crash. And to free memory JS requires to clear all references, which is not required in Delphi/FPC. Therefore the compiler adds code to check for null, call the destructor and sets the variable to null. 

It does not support freeing properties and function results. For example: List[i].Free; will give a compiler error. The property setter might create side effects, which would be incompatible to Delphi/FPC. 

## Version 0.8.13

  * 11 May 2017
  * $IF
  * $ELSEIF
  * $IFOPT
  * $Error
  * $Warning,
  * $Note
  * $Hint
  * And in the config files it supports #IF, #IFDEF, #IFNDEF, #ELSEIF, #ELSE, #ENDIF.



## Version 0.8.12

  * 5 May 2017
  * dotted unit names and namespaces.



## Version 0.8.11

  * 23 Apr 2017
  * dynamic arrays can now be initialized with a constant. Same syntax as static arrays: const a: array of string = ('one', 'two');
  * Classes can now be declared in the unit implementation.
  * Unit nodejs now has a console class TNJSConsole.
  * I added the fpcunit package. Successful tests already work. A fail is not yet caught.



## Version 0.8.10

  * 22 Apr 2017
  * you can now use "if aJSValue then", which works just like the JS "if(v)".
  * added ParamCount, ParamStr, GetEnvironment* functions. The default is not doing much.
  * new package fcl_base_pas2js, containing custapp and nodejsapp: - TCustomApplication is the known class from the FCL, but with abstract methods.
  * TNodeJSApplication is a TCustomApplication using nodejs to read command line params and environment variables.



## Version 0.8.9

  * 21 Apr 2017
  * The compiler now has the types 53bit NativeInt and 52bit NativeUInt. Keep in mind that there is still no range checking.



Current aliases: 
    
    
      JSInteger = NativeInt;
    
      Integer = LongInt;
      Cardinal = LongWord;
      SizeInt = NativeInt;
      SizeUInt = NativeUInt;
      PtrInt = NativeInt;
      PtrUInt = NativeUInt;
      ValSInt = NativeInt;
      ValUInt = NativeUInt;
      ValReal = Double;
      Real = Double;
      Extended = Double;
    
      Int64 = NativeInt unimplemented;
      QWord = NativeUInt unimplemented;
      Single = Double unimplemented;
      Comp = Int64 unimplemented;
    
      UnicodeString = String;
      WideString = String;
      WideChar = char;
    

## Version 0.8.8

  * 20 Apr 2017
  * bugfix release.



## Version 0.8.7

  * 18 Apr 2017
  * $mod: the current module
  * Self: in a method with nested functions this holds the class or class instance.
  * overloads are now chosen like FPC/Delphi not only by type, but also by precision. This means if there are several compatible overloads, and none fits exactly it will choose the next higher. Formerly it gave an error.
  * cardinal is no longer a base type, but longword is. Same as FPC.



## Version 0.8.6

  * 17 Apr 2017
  * bugfix release.



## Version 0.8.5

  * 15 Apr 2017
  * bugfix for the analyzer.
  * supports published array properties and published records.
  * array properties for external classes.



## Version 0.8.4

  * 14 Apr 2017
  * bugfix release.



## Version 0.8.3

  * 4 Apr 2017
  * pas2js now has code navigation in Lazarus



# Trunk

# Navigation

  * Back to [pas2js](<pas2js.md> "pas2js")
  * Back to [lazarus pas2js integration](<lazarus_pas2js_integration.md> "lazarus pas2js integration")

---

_Source: [https://wiki.freepascal.org/Pas2JS_Version_Changes](https://web.archive.org/web/20230204000429/https://wiki.freepascal.org/Pas2JS_Version_Changes)_
