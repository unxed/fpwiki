# Helper types

Helper "types" allow you to extend the scope in which the compiler searches for symbols. This page describes helper types in Object Pascal. For the equivalent in Objective Pascal see [here](<FPC_PasCocoa.md> "FPC PasCocoa"). 

## Contents

  * 1 General
  * 2 Declaration
    * 2.1 Syntax
    * 2.2 Implementation
      * 2.2.1 Inherited with function name
      * 2.2.2 Inherited without function name
    * 2.3 Restrictions
    * 2.4 Inheritance
      * 2.4.1 Class Helper
      * 2.4.2 Record Helper
      * 2.4.3 Type Helper
    * 2.5 Example
  * 3 Usage
    * 3.1 Scope
    * 3.2 Method hiding
    * 3.3 Restrictions
  * 4 Differences
    * 4.1 Differences between mode Delphi and ObjFPC
    * 4.2 Differences between Delphi and FPC
      * 4.2.1 Root type
      * 4.2.2 inherited
      * 4.2.3 RTTI
  * 5 Code examples
  * 6 Multi-scope helpers
    * 6.1 Order/Precedence
    * 6.2 Inherited



## General

Class and Record helpers are available since the release of FPC 2.6, Type helpers since the release of FPC 3.0. 

## Declaration

Helper types (which are **not** themselves concrete types of any kind despite appearing as such, and in fact amount to "syntactic sugar") allow you to extend a given class, interface, record or primitive type with additional functionality. So you can add 

  * properties
  * enumerators
  * wrappers for existing functionality
  * class vars and const
  * local types
  * constructors, class constructors, class destructors.



You cannot add new fields. 

### Syntax

Currently helper types can be used to extend classes (_class helper_), records (_record helper_) and (primitive) types (_type helper_) as those are supported by Delphi as well. Additionally, they can extend interfaces, (_type helper_) which is functionality unique to FPC. 

The general syntax for helper types is the following: 
    
    
    HelperName = class|record|type helper[(OptionalBaseHelper)] for TypeName
      [properties, procedures, functions, constructors, consts, vars]
    end [hint modifiers];
    

The sequence _class helper_ declares a helper for a class, _record helper_ declares a helper for a record while _type helper_ declares a helper for a primitive type. The inheritance _(BaseHelper)_ is optional. 

_HelperName_ , _BaseHelper_ and _TypeName_ have to be valid Pascal identifiers. In case of a class helper _TypeName_ must be a class and _BaseHelper_ must be another class helper that extends either the same class as _TypeName_ or a parent class of it. For record helpers _TypeName_ must be a record and _BaseHelper_ must be a record helper that extend the same record type. The declarations inside a helper are very similiar to the declaration of a normal class, but you must not use fields, destructors and class constructors/destructors. 

_Note_ : In mode ObjFPC record helpers need the modeswitch _advancedrecords_ to be declared; in mode Delphi no further modeswitch is necessary. For type helpers the modeswitch _typehelpers_ is required. 

### Implementation

The rules when implementing a helper are a bit differently then when implementing a normal class or record. First of the type of _Self_ is always of the type of the extended type (e.g. _TObject_ in case of a helper declared as _class helper for TObject_). 

If a method is not defined in the helper type itself it is 

  1. first searched in all the ancestor helpers of the current helper
  2. then in the extended type (helper for),
  3. and then (for class helpers) in the ancestors of the extended class. Please note that if there is a helper for an ancestor it is searched right before the ancestor class itself is searched.



#### Inherited with function name

The **inherited FuncName;** skips the ancestors of the helper and starts searching in the extended type, then it proceeds normally, i.e. if the extended type has an ancestor the compiler searches in the helper for the ancestor of the extended type (if available), then in the ancestor of the extended type (if available), and so forth. 

#### Inherited without function name

The **inherited;** without an explicit name can be a bit confusing: 

  * FPC 3.3.1 and below searches first in the extended type then proceeds normally. That means it skips the helper and its ancestors. $mode objfpc and delphi!
  * Delphi even skips the extended type and searches in the helper for the ancestor of the extended type (if available), then in the ancestor of the extended type (if available). That means it skips the helper, its ancestors and the extended type.



### Restrictions

A helper type may not 

  * contain (class) destructors (except in trunk FPC 3.3.1)
  * contain class constructors (except in trunk FPC 3.3.1)
  * contain fields
  * contain abstract methods
  * "override" virtual methods of the extended class (they can be hidden by the helper though)



Methods of the extended type can be overloaded (thus they are not hidden by the helper) by using the _overload_ keyword. 

Example: 
    
    
    {$mode objfpc}
    {$MODESWITCH ADVANCEDRECORDS}
    
    TObjectHelper = class helper for TObject
      function ToString(const aFormat: String): String; overload;
    end;
    
    function TObjectHelper.ToString(const aFormat: String): String;
    begin
      Result := Format(aFormat, [ToString]);
    end;
    
    var
      o: TObject;
    begin
      Writeln(o.ToString('The object''s name is %s'));
    end.
    

### Inheritance

So-called "inheritance" for helper types fulfills two purposes: 

  * use the methods of multiple helpers at once
  * (class helpers only) bring methods that were added to a parent class to the topmost visibility (because equally named methods of helpers are hidden by subclasses)



#### Class Helper

A class helper can inherit from another class helper if it extends a subclass of the class or the same class which is extend by the parent class helper. 

Example: 
    
    
    TObjectHelper = class helper for TObject
      procedure SomeMethod;
    end;
    
    TFoo = class(TObject)
    end;
    
    TFooHelper = class helper(TObjectHelper) for TFoo
    end;
    

#### Record Helper

A record helper can inherit from another record helper if it extends the same record which is extend by the parent record helper. **Important** : Inheritance for record helpers is not enabled in mode Delphi, because there this is not allowed. 

Example: 
    
    
    TTest = record
    end;
    
    TTestHelper = record helper for TTest
      procedure SomeMethod;
    end;
    
    TTestHelperSub = record helper(TTestHelper) for TTest
    end;
    

#### Type Helper

A type helper can inherit from another type helper if it extends the same type which is extended by the parent type helper. 
    
    
    {$modeswitch TypeHelpers}
    
    TTypeHelper = type helper for Integer
      procedure SetZero;
    end;
     
    TTypeHelperSub = type helper(TTypeHelper) for integer
    end;
    
    implementation
    
    procedure TTypeHelper.SetZero;
    begin
      self:= 0;
    end;
    

Since the introduction, type helpers can for example be found in section [type helpers](<//www.freepascal.org/docs-html/rtl/sysutils/typehelpers.html>) of unit sysutils. 

### Example

A simple example for extending existing functionality is the following: The LCL's _TCheckListBox_ , which is a list with checkboxes, does not provide a property to get the count of all checked items. Using a class helper this can be added rather easily. 
    
    
    type
      TCheckListBoxHelper = class helper for TCheckListBox
      private
        function GetCheckedCount: Integer;
      public
        property CheckedCount: Integer read GetCheckedCount;
      end;
    
    function TCheckListBoxHelper.GetCheckedCount: Integer;
    var
      i: Integer;
    begin
      Result := 0;
      for i := 0 to Items.Count - 1 do
        if Checked[i] then
          Inc(Result);
    end;
    
    // somewhere else (TCheckListBoxHelper needs to be in scope)
    if MyCheckList.CheckedCount > 0 then ...
    

Please note that it's not trivial to implement a cached variation, as helper types can't introduce fields. 

  
Here's another example. This converts array of inter to string with commas, and vice versa. 
    
    
    type
       TIntArray = array of integer;
    
       { HIntArray }
    
       HIntArray = type helper for TIntArray
       private
          function getCommaText: string;
          procedure setCommaText (AValue: string);
       public
          property CommaText: string read getCommaText write setCommaText;
       end;
    
    implementation
    
    { HIntArray }
    
    function HIntArray.getCommaText: string;
    var
       ti: integer;
    begin
       if Length(Self) >= 1 then Result:= IntToStr(Self[0]) else Exit('');
    
       for ti := 1 to High(Self) do Result += Format(',%d', [Self[ti]]);
    end;
    
    procedure HIntArray.setCommaText(AValue: string);
    var
       arr: TStringArray;
       i: integer;
    begin
       Self:= [];
       arr:= AValue.Split([',']);
       for i:= Low(arr) to High(arr) do
         Concat(Self, [StrToIntDef(arr[i], -1)]);
    end;
    
    //----------------------------------------------------
    
    procedure TForm1.Button4Click(Sender: TObject);
    var
       arr: TIntArray = (1,2,3);
    begin
       showmessage(arr.CommaText);  // this will display "1,2,3"
    end;
    

## Usage

### Scope

The methods declared inside the helper are always available once it is in scope and can be called as if they'd belong to the extended type. 

Example: 
    
    
    type
      TObjectHelper = class helper for TObject
        function TheAnswer: Integer;
      end;
    
    function TObjectHelper.TheAnswer: Integer;
    begin
      Result := 42;
    end;
    
    begin
      o := TObject.Create;
      o.TheAnswer;
    end.
    

By default, only one helper type can be active for a type at the same time. This helper is always the last one that is in scope (which follows the usual rules of Pascal). 

Example: 
    
    
    type
      TTestRecord = record
      end;
      TTestClass = class
      end;
    
      TTestRecordHelper = record helper for TTestRecord
        procedure Test;
      end;
    
      TTestClassHelper1 = class helper for TTestClass
        procedure Test1;
      end;
    
      TTestClassHelper2 = class helper for TTestClass
        procedure Test2;
      end;
    
    var
      tr: TTestRecord;
      tc: TTestClass;
    begin
      tr.Test; // this compiles
      tc.Test1; // this won't compile
      tc.Test2; // this compiles
    end.
    

### Method hiding

The methods of helper types hide the methods declared in the extended type as long as they are not declared with "overload". 

If a class helper is in scope for a parent class then only the methods of the parent class are hidden while the methods of the subclass will stay visible. 

Example: 
    
    
    type
      TTest = class
        procedure Test;
      end;
      TTestSub = class(TTest)
        procedure Test;
      end;
    
      TTestHelper = class helper for TTest
        procedure Test;
      end;
    
    var
      t: TTest;
      ts: TTestSub;
    begin
      t.Test; // this calls "Test" of the helper
      ts.Test; // this calls "Test" of TTestSub
    end.
    

Here is another example to explain the search order: 
    
    
    type
      TAnimal = class ... end;
      TBird = class ... end;
      TAnimalHelper = class helper for TAnimal ... end;
      TBirdHelper = class helper for TBird ... end;
      TEagleHelper = class helper(TBirdHelper) for TBird ...end;
    var b: TBird;
    begin
      b.Name;
    end;
    

The compiler searches _Name_ in this order: TEagleHelper, TBirdHelper, TBird, TAnimalHelper, TAnimal. 

### Restrictions

You can not reference helper types anywhere in the code except for the following cases: 

  * inheriting from one
  * using one in (Bit)SizeOf
  * using one in TypeInfo



## Differences

### Differences between mode Delphi and ObjFPC

In mode Delphi you can use _virtual_ , _dynamic_ , _override_ and _message_ identifiers. As the concepts behind those identifiers (virtual methods, message dispatching) isn't applicable for helpers, those keywords are ignored in mode Delphi and not allowed in mode ObjFPC. 

In mode Delphi you can't inherit a record helper from another one and you can not use the "inherited" keyword (not even to call the method of the extended record). 

### Differences between Delphi and FPC

While the helper feature was implemented as Delphi compatible as possible there are some differences between the two:  


#### Root type

In Delphi a helper is basically a class which inherits from a class _TClassHelperBase_ (both class and record helpers) which in turn inherits from _TInterfacedObject_. In FPC helpers are a type for themselves and don't have a basetype.  
As this difference is only visible in the RTTI (which is seldom used for helpers) this difference was considered negligible. 

#### inherited

See [Inherited](<Helper_types.md> "Helper types")

#### RTTI

Because helpers are their own type in FPC they also have a custom type kind (_tkHelper_) and their own fields in TTypeData: 

  * HelperParent: a PTypeInfo field that points to the type info of the parent helper (can be **nil**)
  * ExtendedInfo: a PTypeInfo field that points to the type info of the extended type
  * HelperProps: contains the count of the (published) properties the helper contains
  * HelperUnit: contains the name of the unit the helper is defined in  




The usual RTTI methods can be used to query for properties of the helper. 

_Note_ : _ExtendedInfo_ and _HelperUnit_ don't have a Delphi equivalent. 

Example: 
    
    
    {$mode objfpc}
    uses typinfo;
    type
      TObjectHelper = class helper for TObject
      end;
    
    var
      ti: PTypeInfo;
      td: PTypeData;
    begin
      ti := TypeInfo(TObjectHelper);
      td := GetTypeData(ti);
    {$ifdef fpc}
    // you must use this in FPC
    if ti^.Kind = tkHelper then begin
      if Assigned(td^.HelperParent) then
        Writeln(td^.HelperParent^.Name)
      else
        Writeln('no helper parent');
      Writeln(td^.ExtendedInfo^.Name);
      Writeln(td^.HelperProps);
      Writeln(td^.HelperUnit);
    end;
    {$else}
    // you must use this in Delphi
    if ti^.Kind = tkHelper then begin
      if td^.ParentInfo <> TypeInfo(TClassHelperBase) then
        Writeln(td^.HelperParent^.Name)
      else
        Writeln('no helper parent');
      Writeln(td^.PropCount);
    end;
    {$endif}  
    end.
    

## Code examples

The following table lists some examples for class helpers found on the web and whether they work with the current implementation. 

URL | State   
---|---  
[Class helper to add for ... in support for TComponent.Components / ComponentCount](<http://wiert.wordpress.com/2009/05/07/delphi-class-helper-to-add-for-%E2%80%A6-in-support-for-tcomponentcomponentscomponentcount/>) | ok   
[Class helper for Delphi's TStrings: Implemented Add(Variant)](<http://delphi.about.com/od/delphitips2008/qt/tstrings-helper.htm>) | ok   
  
## Multi-scope helpers

In trunk FPC, it is possible to allow all helpers of a type to be in scope at the same time instead of only the last one found, which can be enabled with {$modeswitch multiscopehelpers} 

### Order/Precedence

With multihelpers disabled (the default) the compiler searches in the last matching helper and its ancestors and then in the extended type. 

With multihelpers enabled the compiler searches in the last matching helper and its ancestors, then in the earlier matching helper(s) and its ancestors and then in the extended type. 
    
    
    {$modeswitch multihelpers}
    type
      TAnimal = class ... end;
      TBird = class ... end;
      TAnimalHelper = class helper for TAnimal ... end;
      TBirdHelper = class helper for TBird ... end;
      TExtBirdHelper = class helper for TBird ... end;
      TEagleHelper = class helper(TBirdHelper) for TBird ...end;
    var b: TBird;
    begin
      b.Name;
    end;
    

The compiler searches _Name_ in this order: TEagleHelper, TBirdHelper, TExtBirdHelper, TBird, TAnimalHelper, TAnimal. 

### Inherited

**inherited** keyword works the same with multihelpers.

---

_Source: [https://wiki.freepascal.org/Helper_types](https://web.archive.org/web/20241213075129/https://wiki.freepascal.org/Helper_types)_
