# FPC New Features Trunk

## Contents

  * 1 About this page
  * 2 All systems
    * 2.1 Compiler
      * 2.1.1 fpcres can compile RC files
    * 2.2 Language
      * 2.2.1 Support for "volatile" intrinsic
      * 2.2.2 Support for "noinline" modifier
      * 2.2.3 Support for multiple active helpers per type
      * 2.2.4 Support for custom attributes
      * 2.2.5 Support for constant parameters in generics
      * 2.2.6 Support for "IsConstValue" intrinsic
      * 2.2.7 Copy supports Open Array parameters
      * 2.2.8 Array constructors for static arrays
      * 2.2.9 "Align" modifier support for record definitions
      * 2.2.10 Support for binary literals in Delphi mode
      * 2.2.11 Support for Digit Separator
      * 2.2.12 Support for forward declarations of generic types
      * 2.2.13 Support for Function References and Anonymous Functions
      * 2.2.14 Descendant type helpers can extend type aliases
      * 2.2.15 Support for Unicode RTL
      * 2.2.16 Support for Extended RTTI
    * 2.3 Units
      * 2.3.1 DaemonApp
        * 2.3.1.1 Additional control codes on Windows
      * 2.3.2 Classes
        * 2.3.2.1 Naming of Threads
      * 2.3.3 Objects
        * 2.3.3.1 TRawByteStringCollection
        * 2.3.3.2 TUnicodeStringCollection
        * 2.3.3.3 TStream methods for reading and writing RawByteString and UnicodeString
      * 2.3.4 Free Vision
        * 2.3.4.1 Unicode support
      * 2.3.5 Video
        * 2.3.5.1 Unicode output support
      * 2.3.6 Keyboard
        * 2.3.6.1 Unicode keyboard input support
      * 2.3.7 fcl-fpterm package
      * 2.3.8 libjack package
  * 3 Darwin/macOS platforms
    * 3.1 Support for symbolicating Dwarf backtraces
  * 4 New compiler targets
    * 4.1 Support for code generation through LLVM
    * 4.2 Support for address sanitizer (asan) through LLVM
    * 4.3 Support for the Z80
    * 4.4 Support for the WebAssembly target
    * 4.5 Support for the PlayStation 1



## About this page

Below you can find a list of new features introduced since the [previous release](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2"), along with some background information and examples. Note that since svn trunk is by definition still under development, some of the features here may still change before they end up in a release version. 

A list of changes that may break existing code can be found [here](<User_Changes_Trunk.md> "User Changes Trunk"). 

## All systems

### Compiler

#### fpcres can compile RC files

  * **Overview** : The _fpcres_ utility gained support for compiling RC files to RES files if the output format (parameter _-of_) is set to _rc_. The Free Pascal compiler itself can use _fpcres_ instead of _windres_ or _gorc_ as well if the option _-FF_ is supplied.
  * **Notes** : Using _fpcres_ instead of _windres_ or _gorc_ will become default once a release with the new _fpcres_ is released.
  * **svn** : 46398 (and others before and after that)



### Language

#### Support for "volatile" intrinsic

  * **Overview** : A **volatile** intrinsic has been added to indicate to the code generator that a particular load from or store to a memory location must not be removed.
  * **Notes** : 
    * Delphi uses an attribute rather than an intrinsic. Such support will be added once support for attributes is available in FPC. An intrinsic that applies only to a specific memory access also has the advantages outlined in <https://lwn.net/Articles/233482/>
    * Accesses to fixed absolute addresses (as common on DOS and embedded platforms) are automatically marked as volatile by the compiler.
  * **Example** : <https://gitlab.com/freepascal.org/fpc/source/-/blob/main/tests/test/tmt1.pp>
  * **svn** : 40465



#### Support for "noinline" modifier

  * **Overview** : A **noinline** modifier has been added that can be used to prevent a routine from ever being inlined (even by automatic inlining).
  * **Notes** : Mainly added for internal compiler usage related to LLVM support.
  * **svn** : 41198



#### Support for multiple active helpers per type

  * **Overview** : With the modeswitch _multihelpers_ multiple helpers for a single type can be active at once. If a member of the type is accessed it's first checked in all helpers that are in scope in reverse order before the extended type itself is checked.
  * **Examples** : All tests with the name _tmshlp*.pp_ in <https://gitlab.com/freepascal.org/fpc/source/-/tree/main/tests/test>
  * **svn** : 42026



#### Support for custom attributes

  * **Overview** : Custom attributes allow to decorate types and published properties of classes to be decorated with additional metadata. The metadata are by itself descendants of _TCustomAttribute_ and can take additional parameters if the classes have a suitable constructor to take these parameters. This feature requires the new modeswitch _PrefixedAttributes_. This modeswitch is active by default in modes _Delphi_ and _DelphiUnicode_. Attributes can be queried using the _TypInfo_ or _Rtti_ units.
  * **Notes** : More information can be seen in the [announcement mail](<https://lists.freepascal.org/pipermail/fpc-announce/2019-July/000612.html>) and [Custom Attributes](<Custom_Attributes.md> "Custom Attributes")
  * **svn** : 42356 - 42411
  * **Example** :


    
    
    program tcustomattr;
    
    {$mode objfpc}{$H+}
    {$modeswitch prefixedattributes}
    
    type
      TMyAttribute = class(TCustomAttribute)
        constructor Create;
        constructor Create(aArg: String);
        constructor Create(aArg: TGUID);
        constructor Create(aArg: LongInt);
      end;
    
      {$M+}
      [TMyAttribute]
      TTestClass = class
      private
        fTest: LongInt;
      published
        [TMyAttribute('Test')]
        property Test: LongInt read fTest;
      end;
      {$M-}
    
      [TMyAttribute(1234)]
      [TMy('Hello World')]
      TTestEnum = (
        teOne,
        teTwo
      );
    
      [TMyAttribute(IInterface), TMy(42)]
      TLongInt = type LongInt;
    
    constructor TMyAttribute.Create;
    begin
    end;
    
    constructor TMyAttribute.Create(aArg: String);
    begin
    end;
    
    constructor TMyAttribute.Create(aArg: LongInt);
    begin
    end;
    
    constructor TMyAttribute.Create(aArg: TGUID);
    begin
    end;
    
    begin
    
    end.
    

#### Support for constant parameters in generics

  * **Overview** : Generic types and routines can now be declared with constants as parameters which function as untyped constants inside the generic. However these generic parameters have a type which allows the author of the generic to restrict the possible values for the constant. Only constant types that can also be used for untyped constants can be used.
  * **Notes** : 
    * This feature is not Delphi compatible, but can be used in mode _Delphi_ as well
    * More information is available in the [announcement mail](<https://lists.freepascal.org/pipermail/fpc-devel/2020-April/042708.html>).
  * **Examples** : All tests with the name _tgenconst*.pp_ in <https://gitlab.com/freepascal.org/fpc/source/-/tree/main/tests/test>
  * **svn** : 45080



#### Support for "IsConstValue" intrinsic

  * **Overview** : An _IsConstValue_ intrinsic has been added to check whether a provided value is considered a constant value. This is mainly useful inside inlined functions to manually improve the generated code if a constant is encountered.
  * **Notes** : 
    * This function returns a constant Boolean value and is Delphi compatible.
    * Typed constants are _not_ considered constants (Delphi compatible and also compatible with the usual modus operandi regarding typed constants).
  * **Example** : <https://gitlab.com/freepascal.org/fpc/source/-/tree/main/tests/test/tisconstvalue2.pp>
  * **svn** : 45695



#### Copy supports Open Array parameters

  * **Overview** : The _Copy_ intrinsic can now be used to copy (a part of) the contents of an open array parameter to a dynamic array.
  * **Notes** : 
    * The result of the _Copy_ function will have the type of a dynamic array with the same element type as the parameter that is copied from.
    * If the _Start_ parameter is out of range the resulting dynamic array will be empty.
    * If the _Count_ parameter is too large then the resulting dynamic array will only contain the elements that exist.
  * **svn** : 46890
  * **Example** :


    
    
    procedure Test(aArg: array of LongInt);
    var
      arr: array of LongInt;
    begin
      arr := Copy(aArg, 3, 5);
    end;
    

#### Array constructors for static arrays

  * **Overview** : Array constructors can be used to assign values to static arrays.
  * **Notes** : 
    * The array constructor needs to have the same amount of elements as the static array.
    * The first element of the array constructor will be placed at the location of the first element of the static array (e.g. if the array starts at -1 the first element will be at that location).
    * Arrays with enumeration index are supported as well.
  * **svn** : 46891, 46901
  * **Example** : <https://gitlab.com/freepascal.org/fpc/source/-/tree/main/tests/test/tarrconstr16.pp>



#### "Align" modifier support for record definitions

  * **Overview** : It is now possible to add an **Align X** modifier at the end of a record definition to indicate that the record as a whole should be aligned to a particular boundary.
  * **Notes** : 
    * Should be Delphi compatible, although documentation is not available (["semi-official" reference](<https://web.archive.org/web/20171221044023/http://qc.embarcadero.com/wc/qcmain.aspx?d=87283>); the mentioned issue does not exist in the FPC implementation).
    * This does not influence the alignment of the individual fields, which are still aligned according to the current _{$packrecords y}_ /_{$align y}_ settings.
    * X can be 1, 2, 4, 8, 16, 32 or 64.
  * **svn** : 47892
  * **Example** : <https://gitlab.com/freepascal.org/fpc/source/-/blob/main/tests/webtbs/tw28927.pp>



#### Support for binary literals in Delphi mode

  * **Overview** : It is now possible to use binary literals ('%' as prefix) in Delphi mode.
  * **Notes** : [Delphi compatibility](<https://docwiki.embarcadero.com/RADStudio/Alexandria/en/What%27s_New#Binary_Literals_and_Digit_Separator>).
  * **GitLab issue** : [#39503](<https://gitlab.com/freepascal.org/fpc/source/-/issues/39503>)
  * **Example** :


    
    
    {$mode Delphi}
    var
      b: byte;
    begin
      b:=%11001001;
    end.
    

#### Support for Digit Separator

  * **Overview** : It is now possible to use the Digit Separator ('_') with the _underscoreisseparator_ modeswitch. It is also enabled by default in Delphi mode.
  * **Notes** : [Delphi compatibility](<https://docwiki.embarcadero.com/RADStudio/Alexandria/en/What%27s_New#Binary_Literals_and_Digit_Separator>).
  * **GitLab issue** : [#39504](<https://gitlab.com/freepascal.org/fpc/source/-/issues/39504>)
  * **Example** :


    
    
    {$mode Delphi}
    var
      i: Integer;
      r: Real;
    begin
      i := %1001_1001;
      i := &121_102;
      i := 1_123_123;
      i := $1_123_123;  
      r := 1_123_123.000_000;
      r := 1_123_123.000_000e1_2;
    end.
    

#### Support for forward declarations of generic types

  * **Overview** : It is now possible to use forward declarations for generic classes and interfaces like is possible for non-generic classes and interfaces.
  * **Notes** : 
    * Generic type constraints and const declarations need to be used for both the forward and the final implementation like is necessary for global generic routines.
    * This is Delphi-compatible.
  * **Commit** : [2a502350](<https://gitlab.com/freepascal.org/fpc/source/-/commit/2a5023508a2bc4ff3ba4f3a0ca16366d3df86db8>)
  * **Example** :


    
    
    {$mode objfpc}
    type
      generic TTest<T> = class;
      generic TFoo<T: class> = class;
      generic TBar<const N: LongInt> = class;
    
      TSomething = class
        fTest: specialize TTest<LongInt>;
        fFoo: specialize TFoo<TObject>;
        fBar: specialize TBar<42>;
      end;
    
      generic TTest<T> = class end;
      generic TFoo<T: class> = class end;
      generic TBar<const N: LongInt> = class end;
    
    begin
    end.
    

#### Support for Function References and Anonymous Functions

  * **Function References** : Function References (also applicable names are Procedure References and Routine References, in the following only Function References will be used) are types that can take a function (or procedure or routine), method, function variable (or procedure variable or routine variable), method variable, nested function (or nested procedure or nested routine) or an anonymous function (or anonymous procedure or anonymous routine) as a value. The function reference can then be used to call the provided function just like other similar routine pointer types. In contrast to these other types nearly all function-like constructs can be assigned to it (the only exception are nested function variables (or nested procedure variables or nested routine variables), more about that later on) and then used or stored.
  * **Anonymous Functions** : Anonymous Functions (or Anonymous Procedures or Anonymous Routines, in the following simply Anonymous Functions) are routines that have no name associated with them and are declared in the middle of a code block (for example on the right side of an expression or as a parameter for a function call). However they can just as well be called directly like a nested function (or nested procedure or nested routine) would.
  * **More Information and Examples** : [Feature announcement: Function References and Anonymous Functions](<https://forum.lazarus.freepascal.org/index.php/topic,59468.0.html>)
  * **GitLab Issue** : [#24481](<https://gitlab.com/freepascal.org/fpc/source/-/issues/24481>)



#### Descendant type helpers can extend type aliases

  * **Overview** : A type helper that descends from another type helper can now extend a unique type alias of the type the inherited type helper extends. This way a type helper for the original type can be made available for the type alias as well.
  * **Notes** : 
    * The type alias the descendant extends does not need to be a _direct_ type alias of the parent's extended type.
    * The other way around is not allowed: a type helper that inherits from another helper which extends a unique type alias can not extend the base type.
  * **Example** : See the [test](<https://gitlab.com/freepascal.org/fpc/source/-/blob/main/tests/test/tthlp30.pp>) for the feature.
  * **Commit** : [7133ad7e](<https://gitlab.com/freepascal.org/fpc/source/-/commit/7133ad7ecc46700618193adff85cef84682355b0>)



#### Support for Unicode RTL

  * **Overview** : The Unicode RTL is required for Delphi 2009+ compatibility. This also includes support for dotted filenames like _System.SysUtils_
  * **Notes** : [FPC Unicode RTL](<FPC_Unicode_RTL.md>).
  * **fpc-devel Mailinglist announcement** : [Unicode RTL](<https://lists.freepascal.org/pipermail/fpc-devel/2023-July/045194.html>)



#### Support for Extended RTTI

  * **Overview** : Extended RTTI includes the for _$RTTI_ directive, generating RTTI metadata for all fields, methods and properties and allowing the access of the RTTI metadata via TypInfo and RTTI unit.
  * **Notes** : [Delphi 2010+ compatibility](<https://docwiki.embarcadero.com/RADStudio/Alexandria/en/RTTI_directive_\(Delphi\)>).
  * **GitLab issues/merge-requests** : [#38964](<https://gitlab.com/freepascal.org/fpc/source/-/issues/38964>), [!888](<https://gitlab.com/freepascal.org/fpc/source/-/merge_requests/888#note_2267133513>)
  * **Commits** : [7eea8507..a9846283](<https://gitlab.com/freepascal.org/fpc/source/-/compare/c74441323ac9712f0a1f08349debcffe580734d1...a98462835ed6848b62ef95188627e11c4ba52df0>), [bb2d1245..cb072b6b](<https://gitlab.com/freepascal.org/fpc/source/-/compare/bb2d12457cc0d860ddbbb857d9b867c3eb37fa40..cb072b6b8c4a228000f98307e63bb7744bf7287e>)



### Units

#### DaemonApp

##### Additional control codes on Windows

  * **Overview** : Windows allows a service to request additional control codes to be received. For example if the session of the user changed. These might also carry additional parameters that need to be passed along to the _TDaemon_ instance. For this the _WinBindings_ class of the _TDaemonDef_ now has an additional _AcceptedCodes_ field (which is a set) that allows to define which additional codes should be requested. Then the daemon should handle the _OnControlCodeEvent_ event handler which in contrast to the existing _OnControlCode_ handler takes two additional parameters that carry the parameters that the function described for MSDNs _[LPHANDLER_FUNCTION_EX](<https://docs.microsoft.com/en-us/windows/win32/api/winsvc/nc-winsvc-lphandler_function_ex>)_ takes as well.
  * **Notes** : This lead to slight incompatibilities which are mentioned in [User Changes Trunk](<User_Changes_Trunk.md> "User Changes Trunk")
  * **svn** : 46326, 46327



#### Classes

##### Naming of Threads

  * **Overview** : _TThread.NameThreadForDebugging_ has been implemented for macOS.
  * **Notes** : Delphi compatible, was already implemented for Windows, Linux and Android and finally for macOS now. Read documentation as every platform has its own restrictions.
  * **svn:** 49323



#### Objects

##### TRawByteStringCollection

  * **Overview** : A new object type TRawByteStringCollection, similar to TStringCollection, but works with RawByteString/AnsiString
  * **git:** 0b8a0fb4



##### TUnicodeStringCollection

  * **Overview** : A new object type TUnicodeStringCollection, similar to TStringCollection, but works with UnicodeString
  * **git:** 0b8a0fb4



##### TStream methods for reading and writing RawByteString and UnicodeString

  * **Overview** : New methods have been added to TStream for reading and writing RawByteString/AnsiString and UnicodeString types.
  * **Notes** : The following methods have been added:


    
    
    FUNCTION TStream.ReadRawByteString: RawByteString;
    FUNCTION TStream.ReadUnicodeString: UnicodeString;
    PROCEDURE TStream.WriteRawByteString (Const S: RawByteString);
    PROCEDURE TStream.WriteUnicodeString (Const S: UnicodeString);
    

RawByteStrings are written to the stream as a 16-bit code page, followed by 32-bit length, followed by the string contents. UnicodeStrings are written as a 32-bit length (the number of 16-bit UTF-16 code units needed to encode the string), followed by the string itself in UTF-16. 

  * **git:** 0b8a0fb4



#### Free Vision

##### Unicode support

  * **Overview** : Unicode versions of the Free Vision units have been added.
  * **Notes** : The Unicode versions of the units have the same name as their non-Unicode counterparts, but with an added 'U' prefix in the beginning of their name. For example, the Unicode version of the 'app' unit is 'uapp', the Unicode version of 'views' is 'uviews', etc. The Unicode versions of the units use the UnicodeString type, instead of shortstrings and pchars. Mixing Unicode and non-Unicode Free Vision units in the same program is not supported and will not work.
  * **More information** : [Free_Vision#Unicode_version](<Free_Vision.md> "Free Vision")
  * **git:** 0b8a0fb4



#### Video

##### Unicode output support

  * **Overview** : Unicode video buffer support has been added to the Video unit.
  * **Notes** : To use the Unicode video buffer, you must call InitEnhancedVideo instead of InitVideo and finalize the unit with DoneEnhancedVideo instead of DoneVideo. After initializing with InitEnhancedVideo, you must use the EnhancedVideoBuf array instead of VideoBuf. On non-Unicode operating systems, the Video unit will automatically translate Unicode characters to the console code page. This is done transparently to the user application, so new programs can always use EnhancedVideoBuf, without worrying about compatibility.
  * **git:** 0b8a0fb4



#### Keyboard

##### Unicode keyboard input support

  * **Overview** : Unicode keyboard input support has been added to the Keyboard unit.
  * **Notes** : After initializing the unit normally via InitKeyboard, you can obtain enhanced key events, which include Unicode character information, as well as an enhanced shift state. To get enhanced key events, use GetEnhancedKeyEvent or PollEnhancedKeyEvent. They return a TEnhancedKeyEvent, which is a record with these fields:


    
    
     TEnhancedKeyEvent = record
       VirtualKeyCode: Word;    { device-independent identifier of the key }
       VirtualScanCode: Word;   { device-dependent value, generated by the keyboard }
       UnicodeChar: WideChar;   { the translated Unicode character }
       AsciiChar: Char;         { the translated ASCII character }
       ShiftState: TEnhancedShiftState;
       Flags: Byte;
     end;
    

TEnhancedShiftState is a set of TEnhancedShiftStateElement, which is defined as: 
    
    
     TEnhancedShiftStateElement = (
       essShift,             { either Left or Right Shift is pressed }
       essLeftShift,
       essRightShift,
       essCtrl,              { either Left or Right Ctrl is pressed }
       essLeftCtrl,
       essRightCtrl,
       essAlt,               { either Left or Right Alt is pressed, but *not* AltGr }
       essLeftAlt,
       essRightAlt,          { only on keyboard layouts, without AltGr }
       essAltGr,             { only on keyboard layouts, with AltGr instead of Right Alt }
       essCapsLockPressed,
       essCapsLockOn,
       essNumLockPressed,
       essNumLockOn,
       essScrollLockPressed,
       essScrollLockOn
     );
    

A special value NilEnhancedKeyEvent is used to indicate no key event available in the result of PollEnhancedKeyEvent: 
    
    
     { The Nil value for the enhanced key event }
     NilEnhancedKeyEvent: TEnhancedKeyEvent = (
       VirtualKeyCode: 0;
       VirtualScanCode: 0;
       UnicodeChar: #0;
       AsciiChar: #0;
       ShiftState: [];
       Flags: 0;
     );
    

  * **git:** 0b8a0fb4



#### fcl-fpterm package

  * **Overview** : Terminal emulator library, written in Free Pascal. Provides a reasonably accurate implementation of an xterm-compatible terminal.
  * **git:** b4164181



#### libjack package

  * **Overview** : Header translation for the [JACK Audio Connection Kit](<https://jackaudio.org/>) library.
  * **git:** 904c2574



## Darwin/macOS platforms

### Support for symbolicating Dwarf backtraces

  * **Overview** : The -gl option now works with DWARF debug information on Darwin. This is the default for non-PowerPC Darwin targets, and the only support format for ARM and 64 bit Darwin targets.
  * **Notes** : You have to also use the -Xg command line option when compiling the main program or library, to generate a .dSYM bundle that contains all debug information. You can also do this manually by calling _dsymutil_
  * **svn** : 49140



## New compiler targets

### Support for code generation through LLVM

  * **Overview** : The compiler now has a code generator that generates LLVM bitcode.
  * **Notes** : LLVM still requires target-specific support and modifications in the compiler. Initially, the LLVM code generator only works when targeting Darwin/x86-64, Darwin/AArch64 (only macOS; iOS has not been tested), Linux/x86-64, Linux/ARMHF and Linux/AArch64.
  * **More information** : [LLVM](<LLVM.md> "LLVM")
  * **svn** : 42260



### Support for address sanitizer (asan) through LLVM

  * **Overview** : The compiler allows to check code with the LLVM address sanitizer.
  * **Notes** : The LLVM address sanitizer is supported for all 64 bit targets supported by the LLVM code generator.
  * **More information** : 
    * [LLVM](<LLVM.md> "LLVM")
    * [Address Sanitizer on Wikipedia](<https://en.wikipedia.org/wiki/AddressSanitizer>)
  * **git** : [403292a1](<https://gitlab.com/freepascal.org/fpc/source/-/commit/403292a13151dbc265748d2119f9d1bd52fb9d54>)



### Support for the Z80

  * **Overview** : Support has been added for generating Z80 code.
  * **More information** : [Z80](<Z80.md> "Z80")



### Support for the WebAssembly target

  * **Overview** : Support has been added for generating WebAssembly code.
  * **More information** : [WebAssembly/Compiler](<WebAssembly/Compiler.md> "WebAssembly/Compiler")



### Support for the PlayStation 1

  * **Overview** : Support has been added for generating programs for the Sony PlayStation 1 video game console.
  * **More information** : [PlayStation_1](<PlayStation_1.md> "PlayStation 1")



Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch") | [4.0](<Lazarus_4.md> "Lazarus 4.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | trunk (current development)

---

_Source: [https://wiki.freepascal.org/FPC_New_Features_Trunk](https://web.archive.org/web/20250513142505/https://wiki.freepascal.org/FPC_New_Features_Trunk)_
