# FPC New Features 3.0

## Contents

  * 1 About this page
  * 2 All systems
    * 2.1 Language
      * 2.1.1 Delphi-like namespaces units
      * 2.1.2 Dynamic array constructors
      * 2.1.3 New compiler intrinsic _Default_
      * 2.1.4 Support for type helpers
      * 2.1.5 Support for codepage-aware strings
      * 2.1.6 New _delphiunicode_ syntax mode
    * 2.2 Code generator
      * 2.2.1 Class field reordering
      * 2.2.2 Removing the calculation of dead values
      * 2.2.3 Shortcuts to speed up floating point calculations
      * 2.2.4 Constant propagation
      * 2.2.5 Dead store elimination
      * 2.2.6 Node dfa for liveness analysis
    * 2.3 Units and packages
      * 2.3.1 TDBF support for Visual FoxPro files
      * 2.3.2 Bufdataset supports ftAutoInc fields
      * 2.3.3 TDBF, bufdataset (and descendents such as TSQLQuery) allow escaped delimiters in string expression filter
      * 2.3.4 TODBCConnection (odbcconn) Support for 64 bit ODBC
      * 2.3.5 TZipper support for zip64 format
      * 2.3.6 Multiple codepage and unicode support for most file-related RTL routines
      * 2.3.7 SQL parser/generator improvements
    * 2.4 Tools and miscellaneous
      * 2.4.1 New _Pas2jni_ utility
  * 3 (Mac) OS X/iOS
    * 3.1 New _iosxlocale_ unit
  * 4 New compiler targets
    * 4.1 Support for the Java Virtual Machine and Dalvik targets
    * 4.2 Support for the AIX target
    * 4.3 Support for the 16-bit real mode MS-DOS target
    * 4.4 Support for the Android target
    * 4.5 Support for the armhf EABI
    * 4.6 Support for the AROS target
  * 5 New Features from other versions



## About this page

Below you can find a list of new features introduced since the [previous release](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2"), along with some background information and examples. 

## All systems

### Language

#### Delphi-like namespaces units

  * **Overview** : Support has been added for unit names with dots. 
  * **Notes** : Delphi-compatible. 
  * **More information** : Unit names with dots create namespace symbols which always have a precedence over unit names in an identifier search. 



#### Dynamic array constructors

  * **Overview** : Support has been added for constructing dynamic arrays with class-like constructors. 
  * **Notes** : Delphi-compatible. 
  * **More information** : Only constructor name 'CREATE' is valid for dynamic arrays. 
  * **Examples** : SomeArrayVar := TSomeDynArrayType.Create(value1, value2) 



#### New compiler intrinsic _Default_

  * **Overview** : A new compiler intrinsic _Default_ has been added which allows you get a correctly initialized value of a type which is given as parameter. It can also be used with generic type parameters to get a default value of the type. 
  * **Notes** : Delphi-compatible. 
  * **More information** : In simple terms the value returned by _Default_ will be initialized with zeros. The _Default_ intrinsic is not allowed on file types or records/objects/arrays containing such types (Delphi ignores file types in sub elements). 
  * **Examples** : 


    
    
    type
      TRecord = record
        i: LongInt;
        s: AnsiString;
      end;
     
    var
      i: LongInt;
      o: TObject;
      r: TRecord;
    begin
      i := Default(LongInt); // 0
      o := Default(TObject); // Nil
      r := Default(TRecord); // ( i: 0; s: '')
    end.
    
    
    type
      generic TTest<T> = class
        procedure Test;
      end;
     
    procedure TTest.Test;
    var
      myt: T;
    begin
      myt := Default(T); // will have the correct Default if class is specialized
    end;

#### Support for type helpers

  * **Overview** : Support has been added for type helpers which allow you to add methods and properties to primitive types. They require modeswitch _TypeHelpers_ to be set. 
  * **Notes** : In mode _Delphi_ it's implemented in a Delphi-compatible way using _record helper_ for declaration, while the modes _ObjFPC_ and _MacPas_ use _type helper_. The modeswitch _TypeHelpers_ is enabled by default _only_ in mode _Delphi_ and _DelphiUnicode_. 
  * **More information** : 
    * [This](<http://lists.freepascal.org/fpc-announce/2013-February/000587.html>) announcement e-mail contains a detailed description of the feature 
    * The tests are named _tthlp*.pp_ and are available in <http://svn.freepascal.org/svn/fpc/trunk/tests/test/>



#### Support for codepage-aware strings

  * **Overview** : Ansistrings have been made codepage-aware. This means that every ansistring now has an extra piece of meta-information that indicates the codepage in which the characters of that string are encoded. 
  * **Notes** : Delphi-compatible (2009 and later). 
  * **More Information:**
    * [FPC Unicode Support](<FPC_Unicode_support.md> "FPC Unicode support")
    * [Embarcadero white paper](<http://edn.embarcadero.com/article/images/38980/Delphi_and_Unicode.pdf>), specifically the sections _The Many String Types_ and _Converting Strings_



#### New _delphiunicode_ syntax mode

  * **Overview** : The new syntax mode _{$mode delphiunicode}_ combines the behaviour of _{$mode delphi}_ with functionality that is specific to Delphi versions that switched to UnicodeString by default (Delphi 2009 and later). 
  * **Notes** : This syntax mode is still in its infancy, and is not yet fully functional. In particular, there is no RTL yet that is largely compatible with Delphi 2009 and later. The only features currently provided by this syntax mode are changing the default [source file codepage](<FPC_Unicode_support.md> "FPC Unicode support") to the system codepage, and changing the meaning of String (in {$H+} mode)/char/pchar to UnicodeString/WideChar/PWideChar. The codepage-aware AnsiStrings are always active in all syntax modes. 
  * **More Information** : [FPC Unicode Support](<FPC_Unicode_support.md> "FPC Unicode support")



### Code generator

#### Class field reordering

  * **Overview** : The compiler can now reorder instance fields in classes in order to minimize the amount of memory wasted due to alignment gaps. 
  * **Notes** : Since the internal memory layout of a class is opaque (except by querying the RTTI, which is updated when fields are moved around), this change should not affect any code. It may cause problems when using so-called "class crackers" in order to work around the language's type checking though. 
  * **More information** : This optimization is currently only enabled by default at the new optimization level -O4, which enables optimizations that may have (unforeseen) side effects. The reason for this is fairly widespread use of some existing code that relies on class crackers. In the future, this optimization may be moved to level -O2. You can also enable the optimization individually using the _-Ooorderfields_ command line option, or by adding _{$optimization orderfields}_ to your source file. It is possible to prevent the fields of a particular class from being reordered by adding _{$push} {$optimization noorderfields}_ before the class' declaration and _{$pop}_ after it. 



#### Removing the calculation of dead values

  * **Overview** : The compiler can now in some cases (which may be extended in the future) remove the calculation of dead values, i.e. values that are computed but not used afterwards. 
  * **Notes** : While the compiler will never remove such calculations if they have explicit side effects (e.g. they change the value of a global variable), this optimization can nevertheless result in changes in program behaviour. Examples include removed invalid pointer dereferences and removed calculations that would overflow or cause a range check error. 
  * **More information** : This optimization is only enabled by default at the new optimization level -O4, which enables optimizations that may have (unforeseen) side effects. You can also enable the optimization individually using the _-Oodeadvalues_ command line option, or by adding _{$optimization deadvalues}_ to your source file. 



#### Shortcuts to speed up floating point calculations

  * **Overview** : The compiler can now in some cases (which may be extended in the future) take shortcuts to optimize the evaluation of floating point expressions, at the expense of potentially reducing the precision of the results. 
  * **Notes** : Examples of possible optimizations include turning divisions by a value into multiplications with the reciprocal value (not yet implemented), and reordering the terms in a floating point expression. 
  * **More information** : This optimization is only enabled by default at the new optimization level -O4, which enables optimizations that may have (unforeseen) side effects. You can also enable the optimization individually using the _-Oofastmath_ command line option, or by adding _{$optimization fastmath}_ to your source file. 



#### Constant propagation

  * **Overview** : The compiler can now, to a limited extent, propagate constant values across multiple statements in function and procedure bodies. 
  * **Notes** : Constant propagation can cause range errors that would normally manifest themselves at runtime to be detected at compile time already. 
  * **More information** :This optimization is enabled by default at optimization level -O3 and higher. You can also enable the optimization individually using the _-Ooconstprop_ command line option, or by adding _{$optimization constprop}_ to your source file. 



#### Dead store elimination

  * **Overview** : The compiler can now, to a limited extent, remove stores to local variables and parameters if these values are not used before they are overwritten. 
  * **Notes** : The use of this optimization requires that data flow analysis (-Oodfa) is enabled. It can help in particular with cleaning up instructions that have become useless due to constant propagation. 
  * **More information** : This optimization is currently not enabled by default at any optimization level because -Oodfa is still a work in progress. You can enable the optimization individually using the _-Oodeadstore_ command line option, or by adding _{$optimization deadstore}_ to your source file. 



#### Node dfa for liveness analysis

  * **Overview** : The compiler can now perform static data flow analysis (dfa) to determine data liveness. 
  * **Notes** : This analysis is only enabled when -O3 is used. 
  * **More information** : Warnings about uninitialized variables are more exact when using dfa compared with the previous approach. However, the current dfa approach is static and non-global, so one might get false positives: 


    
    
      var
        b : boolean;
        i : longint;
      begin
        if b then
          i:=1;
        writeln;
        if b then
          writeln(i);
      end.

In this case, the compiler warns about _i_ being uninitialized. While some cases like the case above could be detected and the warning could be prevent, this does no apply if _b_ is a function. To workaround this, add an assignment to _i_ at the entry of the subroutine body. 

The same applies to functions/procedures: 
    
    
      var
        i : longint;
      procedure p;
        begin
          i:=1;
        end;
      begin
        p;
        writeln(i);
      end.

The current dfa approach works only intra-procedurally instead of globally, so the above case cannot yet be handled correctly. The compiler does not see that _i_ is initialized. To work around this, add an assignment to i at the entry of the outer subroutine body. 

### Units and packages

#### TDBF support for Visual FoxPro files

  * **Overview** : TDBf now has explicit support for Visual FoxPro (tablelevel 30) files, including the VarBinary and VarChar datatypes. 
  * **Notes** : TDBF version increased to 7.0.0. 
  * **More information** : The code does not support .dbc containers, only freestanding dbfs. It does not support (and quite likely will never support) .cdx index files. 



Additionally, TDBf is now included in the database test suite and has received several fixes (including better Visual FoxPro codepage support). 

#### Bufdataset supports ftAutoInc fields

  * **Overview** : Bufdataset now has support for automatically increasing ftAutoinc field values. 



#### TDBF, bufdataset (and descendents such as TSQLQuery) allow escaped delimiters in string expression filter

  * **Overview** : filters that contain string expressions should be quoted (using either ' or "). However, having the same quotes within the filter was not parsed as there was no support for escaping quotes in the string 



Support has been added for escaping quotes to allow this. 

  * **Notes** : Double up the delimiter within the string to escape the delimiter. Example: 


    
    
    Filter:='(NAME=''O''''Malley''''s "Magic" Hammer'')';
    //which gives
    //(NAME='O''Malley''s "Magic" Hammer')
    //which will match record
    //O'Malley's "Magic" Hammer

  * **More information** : N/A 



#### TODBCConnection (odbcconn) Support for 64 bit ODBC

  * **Overview** : 64 bit ODBC support has been added. 
  * **Notes** : if you use unixODBC version 2.3 or higher on Linux/Unix, the unit has to be (re)compiled with -dODBCVER352 to enable 64 bit support 
  * **More information** : Only tested on Windows and Linux. 



#### TZipper support for zip64 format

  * **Overview** : TZipper now supports the zip64 extensions to the zip file format: > 65535 files and > 4GB file size (bug #23533). Related fixes also allow path/filenames > 255 characters. 
  * **Notes** : the zip64 format will automatically be used if the number or size of files involved exceed the limits of the old zip format. Note that there still are 2GB limits on streams as used in extraction/compression. Zip64 is unrelated to the processor type/bitness (such as x86, x64, ...). 
  * **More information** : More information on zip64: <http://en.wikipedia.org/wiki/ZIP_%28file_format%29#ZIP64>



#### Multiple codepage and unicode support for most file-related RTL routines

  * **Overview** : Most file-related routines from the _system_ and _sysutils_ units have been made codepage-aware: they now accept ansistrings encoded in arbitrary codepages as well as unicodestrings, and will convert these to the appropriate codepage before calling OS APIs. 
  * **Notes** : / 
  * **More information** : [Detailed list](<FPC_Unicode_support.md> "FPC Unicode support") of all related changes to the RTL. 



#### SQL parser/generator improvements

  * **Overview** : The SQL parser/generator in packages/fcl-db/src/sql has been improved: 
  * **Notes** : N/A 
    * Support for FULL [OUTER] JOIN; optional OUTER in LEFT OUTER and RIGHT OUTER JOIN 
    * support table.column notation for fields like SELECT A.B FROM MYTABLE or SELECT B FROM MYTABLE ORDER BY C.D 
    * Small improvements (e.g. in array datatype access) that allow the parser to parse the Firebird employee sample database DDL. _Note: there is no support for isql SET TERM statements, so isql DDL dumps containing stored procedures/triggers with semicolons can still not be processed properly_
  * **More information** : N/A 



### Tools and miscellaneous

#### New _Pas2jni_ utility

  * **Overview** : The new _pas2jni_ utility generates a JNI (Java Native Interface) bridge for Pascal code. This enables Pascal code (including classes and other advanced features) to be easily used from Java programs. 
  * **Notes** : The following Pascal features are supported by pas2jni: function/procedure, var/out parameters, class, record, property, constant, enum, TGuid type, pointer type, string types, all numeric types 
  * **More information** : [pas2jni](<pas2jni.md> "pas2jni")



## (Mac) OS X/iOS

### New _iosxlocale_ unit

  * **Overview** : The new unit called _iosxlocale_ can be used to initialise the _DefaultFormatSettings_ and other related locale information in the _sysutils_ unit based on the settings in the (Mac) OS X _System Preferences_ or the iOS _Settings_ app. 
  * **Notes** : The _clocale_ unit, which also works on (Mac) OS X and iOS, instead gets its information from the Unix-layer. This information depends on the contents of the _LC_ALL_ , _LC_NUMERIC_ etc environment variables (see _man locale_ for more information). Adding both _clocale_ and _iosxlocale_ to the uses clause will cause the second in line to overwrite the settings set by the first one. 
  * **More information** : Adding this unit to the uses clause is enough to use its functionality. 



## New compiler targets

### Support for the Java Virtual Machine and Dalvik targets

  * **Overview** : Support has been added for generating Java byte code as supported by the Java Virtual Machine and by the Dalvik (Android) virtual machine. 
  * **Notes** : Not all language features are supported for these targets. 
  * **More information** : [The FPC JVM target](<FPC_JVM.md> "FPC JVM")



### Support for the AIX target

  * **Overview** : Support has been added for the AIX operating system. Both PowerPC 32bit and 64bit are supported, except that at this time the resource compiler does not yet work for ppc64. 
  * **Notes** : AIX 5.3 and later are supported. 
  * **More information** : [The FPC AIX port](<FPC_AIX_Port.md> "FPC AIX Port")



### Support for the 16-bit real mode MS-DOS target

  * **Overview** : Support has been added for the 16-bit real mode MS-DOS target. Multiple memory models are supported (including ones, not supported by TP/BP) 
  * **Notes** : Open Watcom binutils (wlib and wlink) are required. Crosscompilation from go32v2 still has issues, due to lack of long file name support in Open Watcom's binutils and incompatibilities between Open Watcom's dos extender and the GO32 dos extender. 
  * **More information** : [DOS](<DOS.md> "DOS")



### Support for the Android target

  * **Overview** : Support has been added for the Android target. Supported CPUs: ARM, x86, MIPS. 
  * **Notes** : You need to build a cross-compiler from FPC sources to be able to compile for the Android target. 
  * **More information** : [The FPC Android target](<Android.md> "Android")



### Support for the armhf EABI

  * **Overview** : Support has been added for the armhf EABI. 
  * **Notes** : This support was already available in some patched FPC distribution by Debian GNU/Linux, but now it is officially supported. 
  * **More information** : To bootstrap a compiler with armhf support, the compiler must be compiled with -dFPC_ARMHF. An armhf compiler compiling itself will automatically create a new armhf compiler. 



### Support for the AROS target

  * **Overview** : Support has been added for the AROS target. Supports: i386-ABIv0. 
  * **Notes** : You need to build a cross-compiler from FPC sources to be able to compile for the AROS target (requires collect-aros to support binutils). A native compiler for AROS is available. 
  * **More information** : [AROS](<AROS.md> "AROS")



## New Features from other versions

Lazarus - Release Notes and Subversion Branch with Release Fixes

Release notes for Version:

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | **3.0** | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/FPC_New_Features_3.0%23Support_for_the_Java_Virtual_Machine_and_Dalvik_targets](https://web.archive.org/web/20190420172450/https://wiki.freepascal.org/FPC_New_Features_3.0%23Support_for_the_Java_Virtual_Machine_and_Dalvik_targets)_
