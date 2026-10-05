# User Changes 3.2.0

## Contents

  * 1 About this page
  * 2 All systems
    * 2.1 Usage Changes
      * 2.1.1 Library search directories and custom sysroots
      * 2.1.2 Additional {$include} variables
    * 2.2 Implementation Changes
      * 2.2.1 Dynamic array parameters are passed like pointers
      * 2.2.2 VMT Interface table uses private variable FPC_EMPTYINTF
      * 2.2.3 Modeswitch _TypeHelpers_ in Delphi modes enables _type helper_ -syntax
      * 2.2.4 Class references in a class VMT's field table
      * 2.2.5 Property field access lists no longer allows classes
      * 2.2.6 Range checking for enumeration constants in Delphi mode
    * 2.3 Language Changes
      * 2.3.1 Visibility of generic type parameters
      * 2.3.2 Parsing of _specialize_ has been changed
      * 2.3.3 Operator overload _+_ no longer allowed for dynamic arrays
      * 2.3.4 Generic type parameters need to match
      * 2.3.5 Visibility of methods implementing interface methods
      * 2.3.6 Methods implementing interface methods and overloads
      * 2.3.7 Objective-C "Related Result Type"
    * 2.4 RTTI changes
      * 2.4.1 RTTI for Interfaces (published property count)
      * 2.4.2 RTTI for COM Interfaces (IID String)
      * 2.4.3 RTTI Binary format change
      * 2.4.4 RTTI for Corba Interfaces (IID String)
      * 2.4.5 Reference to Record Init Information
      * 2.4.6 TOrdType extended
      * 2.4.7 New OrdType field for tkInt64 and tkQWord
      * 2.4.8 First field of TProcedureParam changed
      * 2.4.9 TParamFlag extended for constref
      * 2.4.10 Ranges of Boolean types adjusted
      * 2.4.11 OrdType of Boolean64 and QWordBool adjusted
      * 2.4.12 All Boolean types have TypeKind tkBool
      * 2.4.13 TParamFlag extended for hidden parameters
      * 2.4.14 Parameters of method pointer variables contain hidden parameters
      * 2.4.15 Parameters of procedure variables contain hidden parameters
      * 2.4.16 Implicit default values for real, pointer and string properties
    * 2.5 Unit changes
      * 2.5.1 SysUtils
        * 2.5.1.1 faSymlink
        * 2.5.1.2 FileExists on Unix-based systems
      * 2.5.2 Classes.TList auto-growth
      * 2.5.3 Math Min/Max
      * 2.5.4 StrUtils AnsiStartsText/AnsiEndsText
      * 2.5.5 Types IStream interface and Classes TStreamAdapter
      * 2.5.6 Baseunix (Linux only)
      * 2.5.7 Classes TStrings.LoadFromStream/File encoding handling
        * 2.5.7.1 TStringStream now observes system encoding
        * 2.5.7.2 TStringStream derives from TBytesStream
      * 2.5.8 DB
        * 2.5.8.1 TParam.LoadFromFile sets share mode to fmShareDenyWrite
        * 2.5.8.2 CodePage aware TStringField and TMemoField
        * 2.5.8.3 ODBC headers on Unix platforms defaults to 3.52 version
        * 2.5.8.4 TBlobData opaque type reworked to TBytes
      * 2.5.9 FGL
        * 2.5.9.1 Declaration of type TTypeList changed
      * 2.5.10 objcbase
      * 2.5.11 SimpleIPC
      * 2.5.12 Process Unit
        * 2.5.12.1 RunCommand
        * 2.5.12.2 poNoConsole on Windows
      * 2.5.13 fpHTTPClient and fpHTTPServer Units
  * 3 Linux/Android platforms
    * 3.1 GNU Binutils 2.19.1 or later are required by default
    * 3.2 Stack alignment on Linux/i386
  * 4 i386 platforms
    * 4.1 The **Pascal** calling convention now follows Delphi conventions on 32 bit x86 platforms
    * 4.2 -Ooasmcse/{$optimization asmcse} has been removed
  * 5 i8086 platforms
    * 5.1 the codepointer type has been changed in the small and compact memory models
  * 6 Darwin/macOS platforms
    * 6.1 Default Target macOS version
    * 6.2 Darwin/i386 and calling conventions whereby the callee removes stack parameters
  * 7 Windows platforms
    * 7.1 GNU Binutils 2.25 or later are required by default
  * 8 Previous release notes



## About this page

Listed below are intentional changes made to the FPC compiler (3.2) since the [previous release](<User_Changes_3.0.md> "User Changes 3.0.4") that may break existing code. The list includes reasons why these changes have been implemented, and suggestions for how you might adapt your code if you find that previously working code has been adversely affected by these recent changes. 

The list of new features that do not break existing code can be found [here](<FPC_New_Features_3.md> "FPC New Features 3.2"). 

Please add revision numbers to the entries from now on. This facilitates moving merged items to the user changes of a release. 

## All systems

### Usage Changes

#### Library search directories and custom sysroots

  * **Old behaviour** : When specifying a custom sysroot (-XR), all library search directories (both built-in and specified via options) were relative to this sysroot
  * **New behaviour** : Library search directories are only relative to the sysroot if they start with "=".
  * **Reason** : Allow specifying custom search directories for libraries outside a sysroot.
  * **Remedy** : Add "=" at the start of library search paths if you wish them to be relative to any specified sysroot.
  * **Example** : 
    * **-XR/Data/Developer/MacOSX10.4u.sdk/ -Fl=/opt/X11/lib** : will search for libraries in **/Data/Developer/MacOSX10.4u.sdk/opt/X11/lib**
    * **-XR/Data/Developer/MacOSX10.4u.sdk/ -Fl/opt/X11/lib** : will search for libraries in **/opt/X11/lib**
    * **-Fl=/opt/X11/lib** : will search for libraries in **/opt/X11/lib**
    * **-Fl/opt/X11/lib** : will search for libraries in **/opt/X11/lib**
  * **svn** : 43279, 43302, 43306, 43312



#### Additional `{$include}` variables

Overview
    The [`{$include}`](<$include.md> "$include") directive now recognizes the additional internal variables 

  * `{$include %currentRoutine%}`
  * `{$include %dateYear%}`, `{$include %dateMonth%}`, `{$include %dateDay%}`
  * `{$include %timeHour%}`, `{$include %timeMinute%}`, `{$include %timeSecond%}`



Remedy
    You cannot name and use environment variable expansion with any of those names.
svn
    30873 (current routine), 38329 (date and time)

### Implementation Changes

#### Dynamic array parameters are passed like pointers

  * **Old behaviour** : When using the default calling convention, dynamic array parameters were passed on the stack.
  * **New behaviour** : When using the default calling convention, dynamic array parameters are now passed like a pointer (which may be in a register).
  * **Reason** : Delphi compatibility, ensuring that SetPointerProp can be used with dynamic arrays.
  * **Remedy** : Adjust pure assembler routines that have dynamic array parameters.
  * **svn** : 30870, 30878, 31622



#### VMT Interface table uses private variable FPC_EMPTYINTF

  * **Old behaviour** : The vIntfTable field has three possible values: 
    * Nil if the class doesn't implement any interface (but an ancestor might)
    * A pointer to an interface table (with count <> 0) if the class implements any interface
    * A pointer to FPC_EMPTYINTF if neither the class itself nor any ancestor implements an interface
  * **New behaviour** : The vIntfTable field has two possible values: 
    * Nil if neither the class nor any ancestor implements an interface
    * A pointer to an interface table in any other case
  * **Reason** : FPC_EMPTYINTF had to be removed due to dynamic packages support on PE-based systems
  * **Remedy** : Adjust code accordingly
  * **svn** : 34087



#### Modeswitch _TypeHelpers_ in Delphi modes enables _type helper_ -syntax

  * **Old behaviour** : The modeswitch _TypeHelpers_ is enabled by default in Delphi modes and allows to extend primitive types with _record helper_ types.
  * **New behaviour** : 
    * The modeswitch is no longer set by default.
    * Primitive types can always be extended by record helpers in Delphi modes.
    * The modeswitch enables the _type helper_ -syntax as known from non-Delphi modes.
  * **Reason** : The previous implementation of the modeswitch was illogical additionally there were user wishes to allow inheritance for record helpers in Delphi modes.
  * **Remedy** : The only problems arise if one disabled the modeswitch on purpose which now no longer disallows the extension of primitive types.
  * **svn** : 37225



#### Class references in a class VMT's field table

  * **Old behaviour** : The class array of the vFieldTable of the VMT contains an array of TClass entries
  * **New behaviour** : The class array of the vFieldTable of the VMT contains an array of PClass entries
  * **Reason** : As for the RTTI the indirect references are necessary for dynamic packages.
  * **Remedy** : Use an additional dereferentiation to access the class type.
  * **svn** : 37485



#### Property field access lists no longer allows classes

  * **Old behaviour** : A field access list for a property is allowed to contain implicit dereferences in the form of fields of class instances.
  * **New behaviour** : A field access list for a property must only contain record or (TP style) object fields.
  * **Reason** : 
    * Delphi compatibility
    * This resulted in an internal error for published properties
  * **Remedy** : 
    * Switch the fields to records or objects
    * Use a method with inline modifier that will result in similar performance
  * **svn** : 40656
  * **Example** : The following code now fails:


    
    
    unit Test;
    {$mode objfpc}
    
    interface
    
    type
      TTest1 = class
        Field: String;
      end;
    
      TTest2 = class
      private
        fTest1: TTest1;
      public
        property Prop: String read fTest1.Field; // Error "Record or object type expected"
      end;
    
    implementation
    
    end.
    

#### Range checking for enumeration constants in Delphi mode

  * **Old behaviour** : Out-of-range enumeration constants never caused an error in Delphi mode, because very early versions of Delphi did not either.
  * **New behaviour** : Out-of-range enumeration constants cause an error in Delphi mode, even if range checking is disabled. Current Delphi versions (and even older ones, such as Delphi 7) behave the same.
  * **Reason** : Delphi-compatibility.
  * **Remedy** : Fix the range errors.
  * **svn** : 42272, 42275



### Language Changes

#### Visibility of generic type parameters

  * **Old behaviour** : Type parameters of generics had _public_ visibility.
  * **New behaviour** : Type parameters of generics now have _strict private_ visibility.
  * **Reason** : With the previous visibility it was possible to create code that leads to infinite loops during compilation or other hard to debug errors. In addition there is no real possibility to work around this issue (for an example see [this](<http://bugs.freepascal.org/view.php?id=25917>) bug report). Also the fix is Delphi compatible.
  * **Remedy** : Declare a type alias for the type parameter with the desired visibility.
  * **Example** : In the following example _T_ is declared as _strict private_ , while _TAlias_ is declared as _public_ and thus can be used as before the change.


    
    
    type
      generic TTest<T> = class
      public type
        TAlias = T;
      end;
    

#### Parsing of _specialize_ has been changed

  * **Old behaviour** : _specialize_ was used to initialize a specialization and was followed by a type name that might contain a unit name and parent types.
  * **New behaviour** : _specialize_ is now considered part of the specialized type, just as _generic_ is. This means that unit names and parent types need to be used before the part containing the _specialize_.
  * **Reason** : This allows for a more logical usage of _specialize_ in context with nested types (especially if multiple specializations are involved) and more importantly generic functions and methods.
  * **Remedy** : Put the _specialize_ directly in front of the type which needs to be specialized.



#### Operator overload _+_ no longer allowed for dynamic arrays

  * **Old behaviour** : The _+_ operator could be overloaded for dynamic array types.
  * **New behaviour** : The _+_ operator for dynamic array types can no longer be overloaded if the modeswitch _ArrayOperators_ is active (default in Delphi modes).
  * **Reason** : When the modeswitch _ArrayOperators_ is active a _+_ operator is now provided by the compiler itself which concatenates two arrays together.
  * **Remedy** : 
    * Disable the modeswitch (for Delphi modes this can be done with _{$modeswitch ArrayOperators-}_).
    * If your operator merely concatenates two arrays then remove the overload.
    * If your operator does something different or more then use a different operator.
  * **Note** : A warning is provided by the compiler if an overload is in scope, but not used due to the internal operator.



#### Generic type parameters need to match

  * **Old behaviour** : When defining the method of a generic class or record it was possible to use different names for the type parameters than those that were used in the declaration.
  * **New behaviour** : The order and name of generic type parameters of the class or record type in the method definition has to match the order and name of generic types parameters of the class or record declaration.
  * **Reason** : To avoid the user making mistakes e.g. when the order of parameters is swapped. Also this is compatible to at least newer Delphi versions.
  * **Remedy** : Use the correct generic type parameters for the definition.
  * **svn** : 39701



#### Visibility of methods implementing interface methods

  * **Old behaviour** : All methods in a class hierarchy were considered when looking for methods to implement interfaces
  * **New behaviour** : Only methods visible in the class that is declared as implementing the method are considered.
  * **Reason** : The symbol visibility rules should be respected in all cases. This fix is Delphi-compatible.
  * **Remedy** : Make (strict) private methods that should implement interface methods in descendant classes protected or public.
  * **svn** : 40645



#### Methods implementing interface methods and overloads

  * **Old behaviour** : All methods in a class hierarchy were considered when looking for methods to implement interfaces.
  * **New behaviour** : In case of overloads, searching stops when a method is found that has the right name but not the _overload_ directive. Note that this directive is automatically inherited when overriding a method.
  * **Reason** : The same happens when calling a method, and the symbol resolution rules should be the same in all cases. This fix is Delphi-compatible.
  * **Remedy** : Add an _overload_ directive to methods of which all overloads should be visible in the entire class hierarchy.
  * **Example** : 
    * [Bug found by this change](<https://gitlab.com/freepascal.org/fpc/source/-/tree/main/tests/webtbs/tw27349.pp>) (previously, on non-Windows platforms the **_Addref** method of _TInterfacedObject_ was selected for implementing by the classes implementing _tmyintf_).
    * [Example that no longer compiles](<https://gitlab.com/freepascal.org/fpc/source/-/tree/main/tests/tbf/tb0267.pp>).
    * [Fixed example](<https://gitlab.com/freepascal.org/fpc/source/-/tree/main/tests/tbs/tb0654.pp>).
  * **svn** : 40683



#### Objective-C "Related Result Type"

  * **Old behaviour** : The type checking system always evaluated calls to Objective-C methods as them returning the declared result type.
  * **New behaviour** : The type checking system now takes into account the ["related result type"](<http://releases.llvm.org/8.0.0/tools/clang/docs/LanguageExtensions.html#objective-c-features>) rule from Objective-C. This means that certain methods are now assumed to always return an instance whose type is compatible with the object to which the message was sent (i.e., it's the same, or inherits from it). This is done for the following methods, provided their declared result type is also compatible with the class type in which the method is declared: 
    * instance methods whose first lower case word in the message name is _init_ or _new_
    * class methods whose first lower case word in the message name is _autorelease_ , _init_ , _retain_ , or _self_
  * **Reason** : Compatibility with translations of more recent versions of Cocoa headers
  * **Remedy** : Normally this should not break existing, correct code. If it does, you can probably fix it by removing superfluous typecasts.
  * **Example** :


    
    
    {$mode objfpc}
    {$modeswitch objectivec1}
    
    type
      FPCMyType = objcclass
        function init: id; message 'init';
        class function alloc: FPCMyType; message 'initWithValue';
        // since ObjCBool is not related to FPCMyType, the "related result type" rule will not apply here
        // even though the message starts with "retain" and it's a class method
        class function retainCounting: ObjCBool; message 'retainCounting';
      end;
    
    ...
    
    var
      a: NSArray;
    begin
      // will give an error even though FPCMyType returns 'id', because 'id' is related to
      // FPCMyType and hence the compiler will now interpret the 'init' message as returning
      // an "FPCMyType" when called. Do note that the actual return type of "init" remains
      // "id" in all other contexts, e.g. when deciding override compatibility. The declaration
      // of that method is not modified, and the only changed behviour is that when calling it
      // the result type is statically assumed to be of FPCMyType.
      a:=FPCMyType.alloc.init;
    end;
    

  * **svn** : r42816



### RTTI changes

#### RTTI for Interfaces (published property count)

  * **Old behavior** : The property RTTI data of an interface (both COM and Corba) immediately followed the TTypeData record without any count.
  * **New behavior** : Before the property RTTI data is now a Word count field that specifies the amount of properties
  * **Reason** : Both user request and usability.
  * **Remedy** : Adjust pointer offsets accessing the property data accordingly.



#### RTTI for COM Interfaces (IID String)

  * **Old behavior** : COM interfaces contain an undocumented IIDStr between the unit name IntfUnit and the property data.
  * **New behavior** : The undocumented field has been removed.
  * **Reason** : The IID of COM interfaces can always be represented as GUID, thus the undocumented IIDStr field is redundant.
  * **Remedy** : Use the GUID field and convert that to a String.



#### RTTI Binary format change

  * **Old behavior** : References to other types are designated by PTypeInfo.
  * **New behavior** : References to other types are designated by PPTypeInfo.
  * **Reason** : While the change in the binary format is Delphi-compatible the reason for this is the introduction of the support for dynamic packages and the rules of the PE file format (Windows) that need to be played by.
  * **Remedy** : If you don't access the binary data directly then there should be no change necessary. Otherwise you need to add in a further derefentiation.
  * **Note** : 
    * The PPTypeInfo value itself will be Nil if there's no reference, not the PTypeInfo reference stored in it.
    * This does not apply to TObject.ClassInfo which still returns a PTypeInfo



#### RTTI for Corba Interfaces (IID String)

  * **Old behavior** : IIDStr is a field of TTypeData.
  * **New behavior** : IIDStr is a property of TTypeData.
  * **Reason** : The compiler assumes the RawIntfUnit field immediately preceding IIDStr is a ShortString of length 255 despite the corresponding binary data only having the length of the string it contains, thus accessing incorrect data.
  * **Remedy** : If you merely accessed the data nothing changes, but the data following IIDStr needs to be accessed differently as RawIntfUnit is the last visible field.
  * **svn** : 35026



#### Reference to Record Init Information

  * **Old behavior** : Field RecSize follows directly after start of TTypeData record.
  * **New behavior** : Field RecSize is preceeded by pointer field RecInitInfo which points to a TTypeInfo for the record init information (the TTypeInfo for that is followed by TRecInitData).
  * **Reason** : With the record init information one can more quickly decide whehther a record needs to be classed as _managed_ or not which is among others useful for the IsManaged() function of the Rtti unit.
  * **Remedy** : If you relied on the relative position of RecSize in TTypeData you'll need to adjust your code.
  * **Note** : The TRecInitData shares its first three fields with a record's TTypeData with the difference that the pointer to the init information is Nil.
  * **svn** : 35125, 35134



#### TOrdType extended

  * **Old behavior** : TOrdType contains entries for 8-, 16- and 32-bit widths.
  * **New behavior** : TOrdType contains entries for 8-, 16-, 32- and 64-bit widths.
  * **Reason** : Without extending TOrdType it wouldn't be possible to correctly represent Boolean type of 64-bit width as they couldn't be differentiated from ordinary Integers with a minimum of 0 and maximum of 1 for the Pascal Boolean type and minium of -1 and maximum of 0 for the Windows Boolean type.
  * **Remedy** : Adjust arrays that have TOrdType as index.
  * **svn** : 35135



#### New OrdType field for tkInt64 and tkQWord

  * **Old behavior** : MinInt64Value and MinQWordValue follow directly after start of TTypeData record.
  * **New behavior** : MinInt64Value and MinQWordValue are preceeded by a OrdType field which explicitely designates them as otSQWord or otUQword respectively.
  * **Reason** : To allow for 64-bit Booleans to be represented correctly the tkInt64 and tkQWord branches of TTypeData had to become part of the tkInteger (aka Ordinal) branch and thus they gained the OrdType field.
  * **Remedy** : If your code relied on the relative position of MinInt64Value and MinQWordValue towards the start of the TTypeData record it needs to be adjusted.
  * **svn** : 35135



#### First field of TProcedureParam changed

  * **Old behavior** : First field of TProcedureParam is called Flags and has type Byte.
  * **New behavior** : First field of TProcedureParam is called ParamFlags and has type TParamFlags and there is a property called Flags of type Byte.
  * **Reason** : Since TParamFlags is a set it might grow beyond the size of a Byte if more values are added to TParamFlag.
  * **Remedy** : Most code should be able to use the backwards and Delphi compatible Flags property (at least if it only wants to check TParamFlag values of which the ordinal value is less than 8) other code should switch to using ParamFlags.
  * **svn** : 35174



#### TParamFlag extended for constref

  * **Old behavior** : TParamFlag has no entry for constref parameters.
  * **New behavior** : TParamFlag has entry pfConstRef for constref parameters.
  * **Reason** : To correctly detect constref parameters they need to be marked accordingly.
  * **Remedy** : Adjust code that relies on the amount of elements contained in TParamFlag (e.g. case-statements or array indices).
  * **svn** : 35175



#### Ranges of Boolean types adjusted

  * **Old behavior** : ByteBool, WordBool and LongBool have a range of 0..-1.
  * **New behavior** : ByteBool, WordBool and LongBool have a range of Low(LongInt) to High(LongInt).
  * **Reason** : The old behavior simply cut the Low(Int64) and High(Int64) values used as ranges for the Boolean types which was wrong. That the ranges are not size dependant is Delphi compatible.
  * **Remedy** : Adjust code that might have relied on the range being _invalid_.
  * **svn** : 35184



#### OrdType of Boolean64 and QWordBool adjusted

  * **Old behavior** : Boolean64 has OrdType otUByte and QWordBool has OrdType otSByte and their ranges are in MinValue and MaxValue.
  * **New behavior** : Boolean64 has OrdType otUQWord with the range being in MinQWordValue and MaxQWordValue and QWordBool has OrdType otSQWord with the range being in MinInt64Value and MaxInt64Value.
  * **Reason** : Both types have a size of 64-bit thus using a smaller size is incorrect.
  * **Remedy** : Use the correct fields to retrieve the range boundaries of the types.
  * **svn** : 35185



#### All Boolean types have TypeKind tkBool

  * **Old behavior** : Boolean has TypeKind tkBool while the other Boolean types (Boolean16, Boolean32, Boolean64, ByteBool, WordBool, LongBool, QWordBool) have TypeKind tkInteger.
  * **New behavior** : Boolean, Boolean16, Boolean32, Boolean64, ByteBool, WordBool, LongBool and QWordBool have TypeKind tkBool.
  * **Reason** : Consistency and possibility to differentiate ordinary subranges from the real Boolean types.
  * **Remedy** : Check for TypeKind tkBool instead of some "magic" to detect Boolean types.
  * **Note** : Since fields can only appear once in a record the tkBool branch of TTypeData was not adjusted. This has no practical consequences however as all fields of a variant record are always accessible.
  * **svn** : 35186, 35187



#### TParamFlag extended for hidden parameters

  * **Old behavior** : TParamFlag has no entries for the hidden parameters (Array High, Self, Vmt, Result) of a function; SizeOf(TParamFlags) = 1
  * **New behavior** : TParamFlag has entry pfHidden for hidden parameters in general and pfHigh for the high parameter of open array, pfSelf for the instance or class type, pfVmt for constructor's VMT parameter and pfResult if the result is passed as a parameter; SizeOf(TParamFlags) = 2
  * **Reason** : As the plan is to use a manager based approach for Invoke() the RTL glue code should need as less knowledge as possible about calling conventions; due to this the locations of the hidden parameters needs to be known.
  * **Remedy** : Adjust code that relies on the amount of elements contained in TParamFlag (e.g. case-statements or array indices) and also code that relies on the size of TParamFlags being 1 (SizeOf(TParamFlags) should be used instead).
  * **svn** : 35267, 35286, 35291



#### Parameters of method pointer variables contain hidden parameters

  * **Old behavior** : The parameters of method pointer variables only listed visible parameters.
  * **New behavior** : The parameters of method pointer variables list all parameters.
  * **Reason** : Makes the implementation of function call managers more stable in case the ABI should change.
  * **Remedy** : Depending on your code either handle the new parameters or check for _pfHidden_ in the parameter flags and ignore them.
  * **svn** : 39885



#### Parameters of procedure variables contain hidden parameters

  * **Old behavior** : The parameters of procedure variables only listed visible parameters.
  * **New behavior** : The parameters of procedure variables list all parameters.
  * **Reason** : Makes the implementation of function call managers more stable in case the ABI should change.
  * **Remedy** : Depending on your code either handle the new parameters or check for _pfHidden_ in the parameter flags and ignore them.
  * **svn** : 39885



#### Implicit default values for real, pointer and string properties

  * **Old behavior** : Real, pointer and string properties had no implicit values. Zero values and empty strings were stored by default.
  * **New behavior** : Real, pointer and string properties have got implicit default values of 0 or `` respectively. As a result these values are not stored by default anymore.
  * **Reason** : 1. Delphi compatibilty. 2. The _default_ modifier cannot be used for these properties - there was no possibility to set 0 or `` default values in FPC 3.0.x.
  * **Remedy** : The _nodefault_ modifier must be added to the property declaration.
  * **More details** : Check the [documentation](<https://www.freepascal.org/docs-html/ref/refsu38.html>) for more information. There were more fixes regarding string storage handling as well.
  * **svn** : 37954



### Unit changes

#### SysUtils

##### faSymlink

  * **Old behaviour:** faSymlink had the numerical value $40. This was wrong on windows, and not compatible with Delphi.
  * **New behaviour:** faSymlink has the numerical value $400. Correct on windows.
  * **Reason for change:** Wrong functionality and Delphi compatibility.
  * **Remedy:** If you are using the old numerical constant $40, simply use faSymlink instead.
  * **svn:** r33340



##### FileExists on Unix-based systems

  * **Old behaviour:** _FileExists_ returns _True_ for existing directories.
  * **New behaviour:** _FileExists_ does not return _True_ for existing directories.
  * **Reason for change:** Directories are not files, thus it was illogical for _FileExists_ to return _True_ for existing directories. Also this brings it in line with Windows and Delphi.
  * **Remedy:** Use _DirectoryExists_ to check for the existance of a directory
  * **svn:** r43111



#### Classes.TList auto-growth

  * **Old behaviour:** Lists with number of elements greater than 127 was expanded by 1/4 of its current capacity
  * **New behaviour:** Adds two new thresholds. If number of elements is greater than 128 MB then list is expanded by constant amount of 16 MB elements (corresponds to 1/8 of 128 MB). If number of elements is greater then 8 MB then list is expanded by 1/8 of its current capacity.
  * **Reason for change:** Avoid out-of-memory when very large lists are expanded
  * **svn:** 34462



#### Math Min/Max

  * **Old behaviour:** Mixing of QWord and Int64 was possible as there was no QWord overload. However the result might have been wrong.
  * **New behaviour:** Overload for QWord exists now but mixed calls with Int64/QWord cause a compiler error.
  * **Reason for change:** Delphi compatibility.
  * **Remedy:** Apply an explicit typecast to one of the parameters so both are of the same type.
  * **svn:** 39995



#### StrUtils AnsiStartsText/AnsiEndsText

  * **Old behaviour:** AnsiStartsText or AnsiEndsText would return false for empty substring. This is not in line with Delphi, which returns true.
  * **New behaviour:** AnsiStartsText or AnsiEndsText now return True for empty substring.
  * **Reason for change:** Delphi compatibility.
  * **Remedy:** Check explicitly for empty string if you counted on the old behaviour.
  * **svn:** 38768



#### Types IStream interface and Classes TStreamAdapter

  * **Old behaviour:** IStream Interface (classes unit and jwa units) used Int64 for various out parameters.
  * **New behaviour:** IStream Interface (classes unit and jwa units) use QWord for various out parameters.
  * **Reason for change:** The MS Headers use largeUint, which translates to QWord. The headers were for an old version of Delphi, which didn't know QWord.
  * **Remedy:** If a class implementing this interface no longer compiles, adapt the signature of the interface's methods so they conform to the new definition.
  * **svn:** 32820 and 35542



#### Baseunix (Linux only)

  * **Old behaviour:** stat was an union, with one side the proper posix fieldnames, and the other deprecated, simplified 1.0.x fieldnames. The 1.0.x were already marked as deprecated for 2 major cycles.
  * **New behaviour:** Deprecated 1.0.x fieldnames removed.
  * **Reason for change:** Left over 1.0.x transition feature. Incompatible with other *nix ports.
  * **Remedy:** Update fieldnames
  * **svn:** 39644 and 39655



#### Classes TStrings.LoadFromStream/File encoding handling

  * **Old behaviour:** The LoadFromStream call totally ignored the encoding of a file, loading it from a stream without regard for encoding, unless an encoding was specified.
  * **New behaviour:** There is an overloaded call with a Boolean parameter 'IgnoreEncoding' which determines whether the encoding should be taken into account or not. By default, this parameter is False, meaning that LoadFromStream with just a stream as argument now calls the encoding-aware version of LoadFromStream, passing it an encoding of Nil, which mean the default encoding will be used.
  * **Reason for change:** Delphi compatibility, and the corresponding changes in TStringStream constructors.
  * **Remedy:** The old behaviour can be restored by setting the IgnoreEncoding parameter to 'True'.
  * **svn:** 37962 and 37965



##### TStringStream now observes system encoding

  * **Old behaviour** : TStringStream copied bytes as-is from the string specified in the constructor.
  * **New behaviour** : Now bytes are fetched from the string using the encoding of the string.
  * **Reason:** Delphi-compatibility
  * **Remedy:** Pass a string with the correct encoding to TStringStream
  * **svn** : 36758



##### TStringStream derives from TBytesStream

  * **Old behaviour** : _TStringStream_ is derived from TStream.
  * **New behaviour** : _TStringStream_ is derived from _TBytesStream_ which in turn derives from _TMemoryStream_.
  * **Reason:**
    * needed for the above mentioned observing of the system encoding
    * Delphi-compatibility
  * **Remedy:** You need to adjust checks for inheritance.
  * **svn** : 36758



#### DB

##### TParam.LoadFromFile sets share mode to fmShareDenyWrite

  * **Old behaviour** : TFileStream.Create(FileName, fmOpenRead) was used, which has blocked subsequent access (also read-only) to same file
  * **New behaviour** : TFileStream.Create(FileName, fmOpenRead+fmShareDenyWrite) is used, which does not block read access to same file
  * **Remedy** : If your application requires exclusive access to file specify fmShareExclusive



##### CodePage aware TStringField and TMemoField

  * **Old behaviour** : These character fields were agnostic to data they presented. IOW when we read content of such field using AsString data was passed from internal character buffer to string as is without any conversion.
  * **New behaviour** : When those fields are created there can be specified CodePage (if none specified CP_ACP is assumed), which defines encoding of character data presented by this field. In case of sqlDB TSQLConnection.CharSet is usualy used. When we read content of such field using AsString character data are translated from fields code page to CP_ACP. Same translation happens when we read content using AsUTF8String (translation to CP_UTF8) or AsUnicodeString (translation to UTF-16). This change reflects CodePage aware strings introduced in FPC 3.



##### ODBC headers on Unix platforms defaults to 3.52 version

  * **Old behaviour** : default was ODBCVER $0351. SQLLEN/SQLULEN was 32 bit on 64 bit Unix platforms.
  * **New behaviour** : default is ODBCVER $0352. SQLLEN/SQLULEN is 64 bit on 64 bit Unix platforms. So it affects some ODBC API functions, which use parameters of this type (it affects also TODBCConnection of course). Also installed unixODBC/iODBC package must match this (in case of unixODBC you can check it using: "odbcinst -j" SQLLEN/SQLULEN must be 8 bytes on 64 bit platform).



##### TBlobData opaque type reworked to TBytes

  * **Old behaviour** : TBlobData was declared as Ansistring
  * **New behaviour** : TBlobData is declared as TBytes
  * **Reason for change** : Delphi compatibility. Also helps to avoid possible code page recodings.
  * **Remedy** : If your application used TBlobData as a string, you can use AsString for parameters, or convert to TBytes.



#### FGL

##### Declaration of type TTypeList changed

  * **Old behavior** : The type _TTypeList_ of the generic classes _TFPGList <>_, _TFPGObjectList <>_ and _TFPGInterfacedObjectList <>_ is declared as _array[0..GMaxListSize] of T_.
  * **New behavior** : The type _TTypeList_ of the generic classes _TFPGList <>_, _TFPGObjectList <>_ and _TFPGInterfacedObjectList <>_ is declared as _PT_.
  * **Reason** : The way _GMaxListSize_ was declared this could lead to too large static array types so that compilation aborted with a _Data segment too large_ error. This also might have lead to incorrect range errors if range checking was turned on. As FPC supports indexed access to pointer types potential type problems when using the _List_ property should be minimal.
  * **Remedy** : If your application relies on _PTypeList_ to be a static array type (instead of merely being indexable like an array) then you'll need to adjust your code.
  * **svn** : 39464



#### objcbase

  * **Old behaviour** : The Objective-C BOOL type was represented by objcbase.BOOL, which was the same as a Pascal boolean on all platforms.
  * **New behaviour** : The Objective-C BOOL type is now represented by objcbase.ObjCBOOL, which maps to a boolean type that always corresponds to the one used in Objective-C (it differs on some platforms)
  * **Reason** : The renaming was to prevent name clashes, and the type definition change was to fix interfacing issues between Objective-Pascal and Objective-C
  * **Remedy** : change all uses of Boolean and BOOL in Objective-Pascal method definitions to ObjCBool.
  * **svn** : 42483



#### SimpleIPC

  * **Old behaviour** : ReadMessage is a procedure.
  * **New behaviour** : ReadMessage is a function, returning a boolean, telling you whether a message was actually read.
  * **Reason** : Provide more information to the user.
  * **Remedy** : Handle the result if necessary. (if you used OnMessage, no change is necessary)
  * **svn** : 36916



  


#### Process Unit

##### RunCommand

  * **Old behaviour:** RunCommand had a few short sleeps to reduce CPU usage when the called process ran long.
  * **New behaviour:** RunCommand defaults to no sleep unless poRunIdle is passed to the default parameter with process options.
  * **Reason for change:** Applications that ran binaries to quickly obtain information were stalled by the pauses for no good reason. Servicing both short and long running binaries adequately without additional info seemed impossible due to slightly differing OS behaviours in pipe reading.
  * **Remedy:** For short living processes that start up quickly: do nothing. For long running processes, pass poRunIdle as process option.
  * **svn:** this changes was merged post RC1



##### poNoConsole on Windows

  * **Old behaviour:** On Windows, poNoConsole mapped to Win API DETACH_PROCESS which still created standard input/output/error handles.
  * **New behaviour:** poNoConsole maps to CREATE_NO_WINDOW, which doesn't create input/output/error handles. A new option poDetached allows for DETACH_PROCESS.
  * **Reason for change:** poNoConsole still created a window in cases. (mantis [bug 32055](<https://bugs.freepascal.org/view.php?id=32055>)).
  * **Remedy:** If you try to hide the console but still need stdin/stdout handles, then poNoConsole no longer works. You could try poDetached. See also [podetached forum thread](<https://forum.lazarus.freepascal.org/index.php/topic,49631.msg360286.html#msg360286>)
  * **svn:** the poDetached option was created post RC1



#### fpHTTPClient and fpHTTPServer Units

  * **Old behaviour:** Old units had SSL support hardcoded built in using OpenSSL.
  * **New behaviour:** A program returns the error "No SSL Socket support compiled in" when doing a HTTPS request.
  * **Reason for change:** pluggable architecture for SSL support.
  * **Remedy:** Add one of the units _opensslsockets_ (OpenSSL) or _gnutlssockets_ (GNU TLS) to the uses clause of your program.



## Linux/Android platforms

### GNU Binutils 2.19.1 or later are required by default

  * **Old behaviour** : The compiler invocation of the linker always resulted in a warning stating "did you forget -T?"
  * **New behaviour** : The compiler now uses a different way to invoke the linker, which prevents this warning, but this requires functionality that is only available in GNU Binutils 2.19 and later.
  * **Reason** : Get rid of the linker warning, which was caused by the fact that we used the linker in an unsupported way (and which hence occasionally caused issues).
  * **Remedy** : If you have a system with an older version of GNU Binutils, you can use the new _-X9_ command line parameter to make the compiler revert to the old behaviour. You will not be able to (easily) bootstrap the new version of FPC on such a system though, so use another system with a more up-to-date version of GNU Binutils for that.



### Stack alignment on Linux/i386

  * **Old behaviour** : The stack alignment for Linux/i386 was 4 bytes.
  * **New behaviour** : The stack alignment for Linux/i386 is 16 bytes.
  * **Reason** : Compatibility with the current Linux/i386 ABI and code generated by other Linux/i386 compilers.
  * **Remedy** : This change will generally only impact inline assembly code that calls subroutines. If you have such code, ensure the stack is aligned to a multiple of 16 bytes before calling any subroutine. You may also have to realign the stack to 16 bytes after routines return in case they use a calling convention whereby the callee removes any stack arguments.



## i386 platforms

### The **Pascal** calling convention now follows Delphi conventions on 32 bit x86 platforms

  * **Old behaviour** : The compiler always saved/restored all registers for a routine with the **Pascal** calling convention, even for pure assembler routines.
  * **New behaviour** : The compiler now only has the old behaviour on the new i8086 (16 bit x86) target. On other targets, it only saves/restores the _ebp_ /_rbp_ register.
  * **Reason** : On 16 bit x86 we follow Turbo Pascal behaviour (which also saved all of those registers), on other x86 platforms we follow Delphi behaviour.
  * **Remedy** : Save/restore additional registers that are non-volatile according to the platform's ABI in case of pure assembler routines that use the _Pascal_ calling convention (generally everything but _EAX_ , _ECX_ and _EDX_).
  * **Remarks**
    * On 64 bit x86-64 platforms, the _Pascal_ calling convention is not supported and maps to the default calling convention defined by the ABI.



### -Ooasmcse/{$optimization asmcse} has been removed

  * **Old behaviour** : The compiler contained an assembler common subexpression elimination pass for the i386 platform.
  * **New behaviour** : This optimisation pass has been removed from the compiler.
  * **Reason** : That pass has been disabled by default since several releases because it hadn't been maintained, and it generated buggy code when combined with newer optimisation passes.
  * **Remedy** : Don't use -Ooasmcse/{$optimization asmcse} anymore.



## i8086 platforms

### the codepointer type has been changed in the small and compact memory models

  * **Old behaviour** : The compiler used the NearPointer type for getting the address of procedures, functions and labels in the small and compact memory models.
  * **New behaviour** : The compiler now uses the NearCsPointer type for getting the address of procedures, functions and labels in the small and compact memory models.
  * **Reason** : Using NearCsPointer more accurately represents the fact that code lives in a separate segment in the small memory model and also allows reading the machine code of a procedure, which might be useful in low level code.
  * **Remedy** : If your program uses the small memory model and needs any kind of fixing, you're very likely to get compile time type checking errors, since the Pointer and CodePointer types are no longer compatible. To fix these, change all your pointers that point to code to CodePointer instead of Pointer. Alternatively, if your program is really small (code+data<=64k), you can switch to the tiny memory model, where the Pointer and CodePointer types are still compatible. If your program uses the compact memory model, it is unlikely to get any sort of breakage, since the Pointer and CodePointer types were already incompatible in this memory model.
  * **svn** : 38691



## Darwin/macOS platforms

### Default Target macOS version

  * **Old behaviour** : The default target version for the i386 (Intel 32 bits) platform was Mac OS X 10.4 (Tiger), and for the x86-64 (Intel 64 bits) was Mac OS X 10.5 (Leopard).
  * **New behaviour** : The default target version for both the i386 (Intel 32 bits) and x86-64 (Intel 64 bits) platforms is now OS X 10.8 (Mountain Lion).
  * **Reason** : On macOS 10.14, installing the command line tools no longer places the crt*.o files required when targeting the older targets in the same location as before, resulting in the need for custom command line options and modifications to fpc.cfg.
  * **Remedy** : If you wish to target those earlier operating system versions, use the _-WM10.4_ resp. _-WM10.5_ command line parameters of the compiler, and if necessary point it to an SDK that still contains support for those operating system versions with the _-XR/path/to/SDK_ parameter (and, if necessary, to an assembler/linker that supports removed architectures with _-FD/path/to/clang_and_ld_).
  * **svn** : 43374



### Darwin/i386 and calling conventions whereby the callee removes stack parameters

  * **Old behaviour** : The Darwin/i386 code generator treated all calling conventions as "caller removes stack parameters", even for calling conventions where this is normally not the case.
  * **New behaviour** : The Darwin/i386 code generator correctly handles calling conventions whereby the callee removes the stack parameters. In particular, this changes the behaviour for the _register_ , _stdcall_ , _safecall_ , and _pascal_ calling conventions.
  * **Reason** : Respect calling conventions, compatibility with other platforms and compilers.
  * **Remedy** : If you have inline assembly code that dealt with this special behaviour of Darwin/i386 under older FPC versions, you can disable it for FPC 3.2 and later. Darwin/i386 now behaves according to the same (new) rules as [Linux/i386](<User_Changes_3.md> "User Changes 3.2").
  * **svn** : r43650



## Windows platforms

### GNU Binutils 2.25 or later are required by default

  * **Old behaviour** : If the binary of a unit contains too many sections ( > $7fff) it fails to compile, due to the binary format not supporting that many sections.
  * **New behaviour** : If the binary of a unit contains many sections ( > $7fff) it will be compiled using the BigObj format of COFF.
  * **Reason** : Some units (e.g. _packages\odata\src\sharepoint.pp_) got so large that they could no longer be compiled using the normal COFF format. The internal assembler and linker can handle this format as can the GNU binutils, but only starting from 2.25 on.
  * **Remedy** : If you compile with the binutils instead of using the internal assembler/linker then either use an up to date version of the binutils (>= 2.25) or if you really need to use an older version, then pass -a5 which will disable the use of BigObj COFF files, though you'll then not be able to build the whole FPC distribution nor will you be able to link to such big units.



## Previous release notes

Lazarus - Release Notes and GIT Branch with Release Fixes

Release notes for Version:

[0.9.24](<Lazarus_0.9.md> "Lazarus 0.9.24 release notes") | [0.9.26](<Lazarus_0.9.md> "Lazarus 0.9.26 release notes") | [0.9.28](<Lazarus_0.9.md> "Lazarus 0.9.28 release notes") | [0.9.28.2](<Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes") | [0.9.30](<Lazarus_0.9.md> "Lazarus 0.9.30 release notes") | [1.0](<Lazarus_1.md> "Lazarus 1.0 release notes") | [1.2](<Lazarus_1.2.md> "Lazarus 1.2.0 release notes") | [1.4](<Lazarus_1.4.md> "Lazarus 1.4.0 release notes") | [1.6](<Lazarus_1.6.md> "Lazarus 1.6.0 release notes") | [1.8](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes") | [2.0](<Lazarus_2.0.md> "Lazarus 2.0.0 release notes") | [2.2](<Lazarus_2.2.md> "Lazarus 2.2.0 release notes") | [3.0](<Lazarus_3.md> "Lazarus 3.0 release notes") | [4.0](<Lazarus_4.md> "Lazarus 4.0 release notes")

Fixes branch (_[How to merge](<Lazarus_1.md> "Lazarus 1.0 fixes branch")_):

[0.9](<Lazarus_0.9.md> "Lazarus 0.9.30 fixes branch") | [1.0](<Lazarus_1.md> "Lazarus 1.0 fixes branch") | [1.2](<Lazarus_1.md> "Lazarus 1.2 fixes branch") | [1.4](<Lazarus_1.md> "Lazarus 1.4 fixes branch") | [1.6](<Lazarus_1.md> "Lazarus 1.6 fixes branch") | [1.8](<Lazarus_1.md> "Lazarus 1.8 fixes branch") | [2.0](<Lazarus_2.md> "Lazarus 2.0 fixes branch") | [2.2](<Lazarus_2.md> "Lazarus 2.2 fixes branch") | [3.0](<Lazarus_3.md> "Lazarus 3.0 fixes branch")

Free Pascal Compiler - User Changes (Release Notes)

User Changes:

[2.2.0](<User_Changes_2.2.md> "User Changes 2.2.0") | [2.2.2](<User_Changes_2.2.md> "User Changes 2.2.2") | [2.2.4](<User_Changes_2.2.md> "User Changes 2.2.4") | [2.4.0](<User_Changes_2.4.md> "User Changes 2.4.0") | [2.4.2](<User_Changes_2.4.md> "User Changes 2.4.2") | [2.4.4](<User_Changes_2.4.md> "User Changes 2.4.4") | [2.6.0](<User_Changes_2.6.md> "User Changes 2.6.0") | [2.6.2](<User_Changes_2.6.md> "User Changes 2.6.2") | [2.6.4](<User_Changes_2.6.md> "User Changes 2.6.4") | [3.0](<User_Changes_3.md> "User Changes 3.0") | [3.0.2](<User_Changes_3.0.md> "User Changes 3.0.2") | [3.0.4](<User_Changes_3.0.md> "User Changes 3.0.4") | 3.2.0 | [3.2.2](<User_Changes_3.2.md> "User Changes 3.2.2") | [trunk (current development)](<User_Changes_Trunk.md> "User Changes Trunk")

New Features:

[2.4.2](<FPC_New_Features_2.4.md> "FPC New Features 2.4.2") | [2.4.4](<FPC_New_Features_2.4.md> "FPC New Features 2.4.4") | [2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0") | [2.6.2](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") | [3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") | [3.2.0](<FPC_New_Features_3.2.md> "FPC New Features 3.2.0") | [3.2.2](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2") | [trunk (current development)](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")

---

_Source: [https://wiki.freepascal.org/User_Changes_3.2](https://web.archive.org/web/20240913192701/https://wiki.freepascal.org/User_Changes_3.2)_
