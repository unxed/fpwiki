# User Changes Trunk

## Contents

  * 1 About this page
  * 2 All systems
    * 2.1 Language Changes
      * 2.1.1 Precedence of the IS operator changed
      * 2.1.2 Visibilities of members of generic specializations
    * 2.2 Implementation Changes
      * 2.2.1 Disabled default support for automatic conversions of regular arrays to dynamic arrays
      * 2.2.2 Directive clause _[…]_ no longer useable with modeswitch _PrefixedAttributes_
      * 2.2.3 Type information contains reference to attribute table
      * 2.2.4 Explicit values for enumeration types are limited to low(longint) ... high(longint)
      * 2.2.5 Comp as a type rename of Int64 instead of an alias
      * 2.2.6 Routines that only differ in result type
      * 2.2.7 Open Strings mode enabled by default in mode Delphi
    * 2.3 Unit changes
      * 2.3.1 System - TVariantManager
      * 2.3.2 System - buffering of output to text files
      * 2.3.3 System - type returned by BasicEventCreate on Windows
      * 2.3.4 System - Ref. count of strings
      * 2.3.5 System.UITypes - function TRectF.Union changed to procedure
      * 2.3.6 64-bit values in OleVariant
      * 2.3.7 Classes TCollection.Move
      * 2.3.8 Math Min/MaxSingle/Double
      * 2.3.9 Random generator
      * 2.3.10 Types.TPointF.operator *
      * 2.3.11 CocoaAll
        * 2.3.11.1 CoreImage Framework Linking
      * 2.3.12 DB
        * 2.3.12.1 TMSSQLConnection uses TDS protocol version 7.3 (MS SQL Server 2008)
        * 2.3.12.2 DB.TFieldType: new types added
      * 2.3.13 DaemonApp
        * 2.3.13.1 TDaemonThread
        * 2.3.13.2 TDaemon
      * 2.3.14 FileInfo
      * 2.3.15 Sha1
      * 2.3.16 Image
        * 2.3.16.1 FreeType: include bearings and invisible characters into text bounds
      * 2.3.17 Generics.Collections & Generics.Defaults
      * 2.3.18 fpviews
  * 3 AArch64/ARM64
    * 3.1 _{_ -style comments no longer supported in assembler blocks
  * 4 Darwin/iOS
  * 5 Previous release notes



## About this page

Listed below are intentional changes made to the FPC compiler (trunk) since the [previous release](<User_Changes_3.2.md> "User Changes 3.2.2") that may break existing code. The list includes reasons why these changes have been implemented, and suggestions for how you might adapt your code if you find that previously working code has been adversely affected by these recent changes. 

The list of new features that do not break existing code can be found [here](<FPC_New_Features_Trunk.md> "FPC New Features Trunk"). 

Please add revision numbers to the entries from now on. This facilitates moving merged items to the user changes of a release. 

## All systems

### Language Changes

#### Precedence of the IS operator changed

  * **Old behaviour** : The IS operator had the same precedence as the multiplication, division etc. operators.
  * **New behaviour** : The IS operator has the same precedence as the comparison operators.
  * **Reason** : Bug, see [[1]](<https://bugs.freepascal.org/view.php?id=35909>).
  * **Remedy** : Add parenthesis where needed.



#### Visibilities of members of generic specializations

  * **Old behaviour** : When a generic is specialized then visibility checks are handled as if the generic was declared in the current unit.
  * **New behaviour** : When a generic is specialized then visibility checks are handled according to where the generic is declared.
  * **Reason** : Delphi-compatibility, but also a bug in how visibilities are supposed to work.
  * **Remedy** : Rework your code to adhere to the new restrictions.



### Implementation Changes

#### Disabled default support for automatic conversions of regular arrays to dynamic arrays

  * **Old behaviour** : In FPC and ObjFPC modes, by default the compiler could automatically convert a regular array to a dynamic array.
  * **New behaviour** : By default, the compiler no longer automatically converts regular arrays to dynamic arrays in any syntax mode.
  * **Reason** : When passing a dynamic array by value, modifications to its contents by the callee are also visible on the caller side. However, if an array is implicitly converted to a dynamic array, the result is a temporary value and hence changes are lost. This issue came up when adding [TStream.Read() overloads](<https://bugs.freepascal.org/view.php?id=35580>).
  * **Remedy** : Either change the code so it no longer assigns regular arrays to dynamic arrays, or add _{$modeswitch arraytodynarray}_
  * **Example** : this program demonstrates the issue that appeared with the TStream.Read() overloads that were added (originally, only the the version with the untyped variable existed)


    
    
    {$mode objfpc}
    type
      tdynarray = array of byte;
    
    procedure test(var arr); overload;
    begin
      pbyte(arr)[0]:=1;
    end;
    
    procedure test(arr: tdynarray); overload;
    begin
      test[0]:=1;
    end;
    
    var
      regulararray: array[1..1] of byte;
    begin
      regulararray[1]:=0;
      test(arr);
      writeln(arr[0]); // writes 0, because it calls test(tdynarr)
    end.
    

  * **svn** : 42118



#### Directive clause _[…]_ no longer useable with modeswitch _PrefixedAttributes_

  * **Old behaviour** : A function/procedure/method or procedure/method variable type could be followed by a directive clause in square brackets (_[…]_) that contains the directives for the routine or type (e.g. calling convention).
  * **New behaviour** : If the modeswitch _PrefixedAttributes_ is enabled (which is the default in modes _Delphi_ and _DelphiUnicode_) the directive clause in square brackets is no longer allowed.
  * **Reason** : As custom attributes are bound to a type/property in a way that looks ambiguous to a directive clause and this ambiguity is not easily solved in the parser it is better to disable this feature.
  * **Remedy** : 
    * don't set (in non-_Delphi_ modes) or disable modeswitch _PrefixedAttributes_ (in _Delphi_ modes) if you don't use attributes (_{$modeswitch PrefixedAttributes-}_)
    * rework your directive clause:


    
    
    // this
    procedure Test; cdecl; [public,alias:'foo']
    begin
    end;
    
    // becomes this
    procedure Test; cdecl; public; alias:'foo';
    begin
    end;
    

  * **svn** : 42402



#### Type information contains reference to attribute table

  * **Old behavior** : The first field of the data represented by _TTypeData_ is whatever the sub branch of the case statement for the type contains.
  * **New behavior** : The first field of the data represented by _TTypeData_ is a reference to the custom attributes that are attributed to the type, only then the type specific fields follow.
  * **Reason** : Any type can have attributes, so it make sense to provide this is a common location instead of having to parse the different types.
  * **Remedy** : 
    * If you use the records provided by the _TypInfo_ unit no changes _should_ be necessary (same for the _Rtti_ unit).
    * If you directly access the binary data you need handle an additional _Pointer_ field at the beginning of the _TTypeData_ area and possibly correct the alignment for platforms that have strict alignment requirements (e.g. ARM or M68k).
  * **svn** : 42375



#### Explicit values for enumeration types are limited to low(longint) ... high(longint)

  * **Old behavior** : The compiler accepted every integer value as explicit enumeration value. The value was silently reduced to the longint range if it fell outside of that range
  * **New behavior** : The compiler throws an error (FPC mode) or a warning (Delphi mode) if an explicit enumeration value lies outside the longint range.
  * **Reason** : _Type TEnum = (a = $ffffffff);_ resulted in an enum with size 1 instead of 4 as would be expected, because $ffffffff was interpreted as "-1".
  * **Remedy** : Add Longint typecasts to values outside the valid range of a Longint.



#### Comp as a type rename of Int64 instead of an alias

  * **Old behavior** : On non-x86 as well as Win64 the Comp type is declared as an alias to Int64 (_Comp = Int64_).
  * **New behavior** : On non-x86 as well as Win64 the Comp type is declared as a type rename of Int64 (_Comp = type Int64_).
  * **Reason** : 
    * This allows overloads of _Comp_ and _Int64_ methods/functions
    * This allows to better detect properties of type _Comp_
    * Compatibility with Delphi for Win64 which applied the same reasoning
  * **Remedy** : If you relied on _Comp_ being able to be passed to _Int64_ variables/parameters either include typecasts or add overloads for _Comp_.
  * **svn** : 43775



#### Routines that only differ in result type

  * **Old behaviour:** It was possible to declare routines (functions/procedures/methods) that only differ in their result type.
  * **New behaviour:** It is no longer possible to declare routines that only differ in their result type.
  * **Reason:** It makes no sense to allow this as there are situations where the compiler will not be able to determine which function to call (e.g. a simple call to a function _Foo_ without using the result).
  * **Remedy:** Correctly declare overloads.
  * **Notes:**
    * As the JVM allows covariant interface implementations such overloads are still allowed inside classes for the JVM target.
    * Operator overloads (especially assignment operators) that only differ in result type are still allowed.
  * **svn:** 45973



#### Open Strings mode enabled by default in mode Delphi

  * **Old behaviour:** The Open Strings feature (directive _$OpenStrings_ or _$P_) was not enabled in mode Delphi.
  * **New behaviour:** The Open Strings feature (directive _$OpenStrings_ or _$P_) is enabled in mode Delphi.
  * **Reason:** Delphi compatibility.
  * **Remedy:** If you have assembly routines with a _var_ parameter of type _ShortString_ then you also need to handle the hidden _High_ parameter that is added for the Open String or you need to disable Open Strings for that routine.
  * **git:** [188cac3b](<https://gitlab.com/freepascal.org/fpc/source/-/commit/188cac3bc6dc666167aacf47fedff1a81d378137>)



### Unit changes

#### System - TVariantManager

  * **Old behaviour:** _TVariantManager.olevarfromint_ has a _source_ parameter of type _LongInt_.
  * **New behaviour:** _TVariantManager.olevarfromint_ has a _source_ parameter of type _Int64_.
  * **Reason for change:** 64-bit values couldn't be correctly converted to an OleVariant.
  * **Remedy:** If you implemented your own variant manager then adjust the method signature and handle the range parameter accordingly.
  * **svn:** 41570



#### System - buffering of output to text files

  * **Old behaviour:** Buffering was disabled only for output to text files associated with character devices (Linux, BSD targets, OS/2), or for output to Input, Output and StdErr regardless of their possible redirection (Win32/Win64, AIX, Haiku, BeOS, Solaris).
  * **New behaviour:** Buffering is disabled for output to text files associated with character devices, pipes and sockets (the latter only if the particular target supports accessing sockets as files - Unix targets).
  * **Reason for change:** The same behaviour should be ensured on all supported targets whenever possible. Output to pipes and sockets should be performed immediately, equally to output to character devices (typically console) - in case of console users may be waiting for the output, in case of pipes and sockets some other program is waiting for this output and buffering is not appropriate. Seeking is not attempted in SeekEof implementation for files associated with character devices, pipes and sockets, because these files are usually not seekable anyway (instead, reading is performed until the end of the input stream is reached).
  * **Remedy:** Buffering of a particular file may be controlled programmatically / changed from the default behaviour if necessary. In particular, perform TextRec(YourTextFile).FlushFunc:=nil immediately after opening the text file (i.e. after calling Rewrite or after starting the program in case of Input, Output and StdErr) and before performing any output to this text to enable buffering using the default buffer, or TextRec(YourTextFile).FlushFunc:=TextRec(YourTextFile).FileWriteFunc to disable buffering.
  * **svn:** 46863



#### System - type returned by BasicEventCreate on Windows

  * **Old behaviour:** BasicEventCreate returns a pointer to a record which contains the Windows Event handle as well as the last error code after a failed wait. This record was however only provided in the implementation section of the _System_ unit.
  * **New behaviour:** BasicEventCreate returns solely the Windows Event handle.
  * **Reason for change:** This way the returned handle can be directly used in the Windows _Wait*_ -functions which is especially apparent in _TEventObject.Handle_.
  * **Remedy:** If you need the last error code after a failed wait, use _GetLastOSError_ instead.
  * **svn:** 49068



#### System - Ref. count of strings

  * **Old behaviour:** Reference counter of strings was a _SizeInt_
  * **New behaviour:** Reference counter of strings is now a _Longint_ on 64 Bit platforms and _SizeInt_ on all other platforms.
  * **Reason for change:** Better alignment of strings
  * **Remedy:** Call _System.StringRefCount_ instead of trying to access the ref. count field by pointer operations or other tricks.
  * **git:** [ee10850a57](<https://gitlab.com/freepascal.org/fpc/source/-/commit/ee10850a5793b69b19dc82b9c28342bdd0018f2e>)



#### System.UITypes - function TRectF.Union changed to procedure

  * **Old behaviour:** _function TRectF.Union(const r: TRectF): TRectF;_
  * **New behaviour:** _procedure TRectF.Union(const r: TRectF);_
  * **Reason for change:** Delphi compatibility and also compatibility with TRect.Union
  * **Remedy:** Call _class function TRectF.Union_ instead.
  * **git:** [5109f0ba](<https://gitlab.com/freepascal.org/fpc/source/-/commit/5109f0ba444c85d2577023ce5fbdc2ddffc267c8>)



#### 64-bit values in OleVariant

  * **Old behaviour:** If a 64-bit value (_Int64_ , _QWord_) is assigned to an OleVariant its type is _varInteger_ and only the lower 32-bit are available.
  * **New behaviour:** If a 64-bit value (_Int64_ , _QWord_) is assigned to an OleVariant its type is either _varInt64_ or _varQWord_ depending on the input type.
  * **Reason for change:** 64-bit values weren't correctly represented. This change is also Delphi compatible.
  * **Remedy:** Ensure that you handle 64-bit values correctly when using OleVariant.
  * **svn:** 41571



#### Classes TCollection.Move

  * **Old behaviour:** If a TCollection.Descendant called Move() this would invoke System.Move.
  * **New behaviour:** If a TCollection.Descendant called Move() this invokes TCollection.Move.
  * **Reason for change:** New feature in TCollection: move, for consistency with other classes.
  * **Remedy:** prepend the Move() call with the system unit name: System.move().
  * **svn:** 41795



#### Math Min/MaxSingle/Double

  * **Old behaviour:** MinSingle/MaxSingle/MinDouble/MaxDouble were set to a small/big value close to the smallest/biggest possible value.
  * **New behaviour:** The constants represent now the smallest/biggest positive normal numbers.
  * **Reason for change:** Consistency (this is also Delphi compatibility), see <https://gitlab.com/freepascal.org/fpc/source/-/issues/36870>.
  * **Remedy:** If the code really depends on the old values, rename them and use them as renamed.
  * **svn:** 44714



#### Random generator

  * **Old behaviour:** FPC uses a Mersenne twister generate random numbers
  * **New behaviour:** Now it uses Xoshiro128**
  * **Reason for change:** Xoshiro128** is faster, has a much smaller memory footprint and generates better random numbers.
  * **Remedy:** When using a certain randseed, another random sequence is generated, but as the PRNG is considered as an implementation detail, this does not hurt.
  * **git:** 91cf1774
  * **If you need the old mersenne twister, for example if you have data that relies on it or from other languages that support mt19937, there is a compatible generator on the wiki:**
  * **[https://wiki.freepascal.org/A_simple_implementation_of_the_Mersenne_twister](<A_simple_implementation_of_the_Mersenne_twister.md>)**



#### Types.TPointF.operator *

  * **Old behaviour:** for `a, b: TPointF`, `a * b` is a synonym for `a.DotProduct(b)`: it returns a `single`, scalar product of two input vectors.
  * **New behaviour:** `a * b` does a component-wise multiplication and returns `TPointF`.
  * **Reason for change** : Virtually all technologies that have a notion of vectors use `*` for component-wise multiplication. Delphi with its `System.Types` is among these technologies.
  * **Remedy:** Use newly-introduced `a ** b`, or `a.DotProduct(b)` if you need Delphi compatibility.
  * **git:** [f1e391fb](<https://gitlab.com/freepascal.org/fpc/source/-/commit/f1e391fb415239d926c4f23babe812e67824ef95>)



#### CocoaAll

##### CoreImage Framework Linking

  * **Old behaviour** : Starting with FPC 3.2.0, the _CocoaAll_ unit linked caused the _CoreImage_ framework to be linked.
  * **New behaviour** : The _CocoaAll_ unit no longer causes the _CoreImage_ framework to be linked.
  * **Reason for change** : The _CoreImage_ framework is not available on OS X 10.10 and earlier (it's part of _QuartzCore_ there, and does not exist at all on some even older versions).
  * **Remedy** : If you use functionality that is only available as part of the separate _CoreImage_ framework, explicitly link it in your program using the _{$linkframework CoreImage}_ directive.
  * **svn** : 45767



#### DB

##### TMSSQLConnection uses TDS protocol version 7.3 (MS SQL Server 2008)

  * **Old behaviour:** TMSSQLConnection used TDS protocol version 7.0 (MS SQL Server 2000).
  * **New behaviour:** TMSSQLConnection uses TDS protocol version 7.3 (MS SQL Server 2008).
  * **Reason for change:** native support for new data types introduced in MS SQL Server 2008 (like DATE, TIME, DATETIME2). FreeTDS client library version 0.95 or higher required.
  * **svn** : 42737



##### DB.TFieldType: new types added

  * **Old behaviour:** .
  * **New behaviour:** Some of new Delphi field types were added (ftOraTimeStamp, ftOraInterval, ftLongWord, ftShortint, ftByte, ftExtended) + corresponding classes TLongWordField, TShortIntField, TByteField; Later were added also ftSingle and TExtendedField, TSingleField
  * **Reason for change:** align with Delphi and open space for support of short integer and unsigned integer data types
  * **svn** : 47217, 47219, 47220, 47221; **git** : c46b45bf



#### DaemonApp

##### TDaemonThread

  * **Old behaviour:** The virtual method _TDaemonThread.HandleControlCode_ takes a single _DWord_ parameter containing the control code.
  * **New behaviour:** The virtual method _TDaemonThread.HandleControlCode_ takes three parameters of which the first is the control code, the other two are an additional event type and event data.
  * **Reason for change:** Allow for additional event data to be passed along which is required for comfortable handling of additional control codes provided on Windows.
  * **svn** : 46327



##### TDaemon

  * **Old behaviour:** If an event handler is assigned to _OnControlCode_ then it will be called if the daemon receives a control code.
  * **New behaviour:** If an event handler is assigned to _OnControlCodeEvent_ and that sets the _AHandled_ parameter to _True_ then _OnControlCode_ won't be called, otherwise it will be called if assigned.
  * **Reason for change:** This was necessary to implement the handling of additional arguments for control codes with as few backwards incompatible changes as possible.
  * **svn** : 46327



#### FileInfo

  * **Old behaviour:** The _FileInfo_ unit is part of the _fcl-base_ package.
  * **New behaviour:** The _FileInfo_ unit is part of the _fcl-extra_ package.
  * **Reason for change:** Breaks up a cycle in build dependencies after introducing a RC file parser into _fcl-res_. This should only affect users that compile trunk due to stale PPU files in the old location or that use a software distribution that splits the FPC packages (like Debian).
  * **svn** : 46392



#### Sha1

  * **Old behaviour:** Sha1file silently did nothing on file not found.
  * **New behaviour:** sha1file now raises sysconst.sfilenotfound exception on fle not found.
  * **Reason for change:** Behaviour was not logical, other units in the same package already used sysutils.
  * **svn** : 49166



#### Image

##### FreeType: include bearings and invisible characters into text bounds

  * **Old behaviour:** When the bounds rect was calculated for a text, invisible characters (spaces) and character bearings (spacing around character) were ignored. The result of Canvas.TextExtent() was too short and did not include all with Canvas.TextOut() painted pixels.
  * **New behaviour:** the bounds rect includes invisible characters and bearings. Canvas.TextExtent() covers the Canvas.TextOut() area.
  * **Reason for change:** The text could not be correctly aligned to center or right, the underline and textrect fill could not be correctly painted.
  * **svn** : 49629



#### Generics.Collections & Generics.Defaults

  * **Old behaviour:** Various methods had **constref** parameters.
  * **New behaviour:** **constref** parameters were changed to **const** parameters.
  * **Reason for change:**
    * Delphi compatibility
    * Better code generation especially for types that are smaller or equal to the _Pointer_ size.
  * **Remedy:** Adjust parameters of method pointers or virtual methods.
  * **git** : [69349104](<https://gitlab.com/freepascal.org/fpc/source/-/commit/693491048bf2c6f9122a0d8b044ad0e55382354d>)



#### fpviews

TListViewer Defaults changed 

Old behaviour: Changing scrollbar value changed focused item. New behaviour: Changing scrollbar value changes top item. Reason: Introduction of scroll by mouse. Remedy: Rework your code if it relay on old behaviour. 

## AArch64/ARM64

### _{_ -style comments no longer supported in assembler blocks

  * **Old behaviour** : The _{_ character started a comment in assembler blocks, like in Pascal
  * **New behaviour** : The _{_ character now has a different meaning in the AArch64 assembler reader, so it can no longer be used to start comments.
  * **Reason for change** : Support has been added for register sets in the AArch64 assembler reader (for the ld1/ld2/../st1/st2/... instructions), which also start with _{_.
  * **Remedy** : Use _(*_ or _//_ to start comments
  * **svn** : 47116



  


## Darwin/iOS

## Previous release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch") | [4.0](<Lazarus_4.md> "Lazarus 4.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | [3.2.0](<User_Changes_3.2.md> "User Changes 3.2.0") | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | trunk (current development)

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/User_Changes_Trunk](https://web.archive.org/web/20250418161403/https://wiki.freepascal.org/User_Changes_Trunk)_
