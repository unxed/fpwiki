# Class helpers

### From Lazarus-ccr

(Redirected from [Class helpers](</index.php?title=Class_helpers&redirect=no> "Class helpers"))

Helper types allow you to extend the scope which the compiler uses to search symbols for. This is useful if you can't extend a class because it's sealed or you can't control of which final type the class instance is (e.g. Lines/Items properties in various LCL components are presented as TStrings, but could be every TStrings descendant). This page describes helper types in Object Pascal. For the equivalent in Objective Pascal see [here](<FPC_PasCocoa.md> "FPC PasCocoa"). 

## Contents

  * 1 General
  * 2 Declaration
    * 2.1 Syntax
    * 2.2 Implementation
    * 2.3 Restrictions
    * 2.4 Inheritance
      * 2.4.1 class helper
      * 2.4.2 record helper
    * 2.5 Example
  * 3 Usage
    * 3.1 Scope
    * 3.2 Method hiding
    * 3.3 Restrictions
  * 4 Differences
    * 4.1 Differences between mode Delphi and ObjFPC
    * 4.2 Differences between Delphi and FPC
  * 5 Code examples

  
---  
  
## [[edit](</index.php?title=Helper_types&action=edit&section=1> "Edit section: General")] General

If you want to test this feature you (currently) need to checkout a FPC branch using the following command: 
    
    
    svn checkout <http://svn.freepascal.org/svn/fpc/branches/svenbarth/classhelpers> fpc-classhelpers
    

You can build the compiler like a normal trunk compiler (you should use FPC 2.4.2 or yet to be released 2.4.4 for building of course). 

_Note_ : This feature is complete and awaits the review for the merge and the merge itself. 

## [[edit](</index.php?title=Helper_types&action=edit&section=2> "Edit section: Declaration")] Declaration

Helper types allow you to extend a given class or record with methods that add more functionality to that type. So you can add default properties, enumerators and wrappers for existing functionality. 

### [[edit](</index.php?title=Helper_types&action=edit&section=3> "Edit section: Syntax")] Syntax

Currently helper types can be used to extend classes (_class helper_) and records (_record helper_) as those are supported by Delphi as well. In theory they could be extended to support interfaces, objects or even primitive types as well (although the latter would need bigger adjustments inside the compiler). 

The general syntax for helper types is the following: 
    
    
     
    HelperName = class|record helper[(BaseHelper)] for ExtendedType
      [properties, procedures, functions, constructors]
    end [hint modifiers];
     

The sequence _class helper_ declares a helper for a class while _record helper_ declares a helper for a record. The inheritance _(BaseHelper)_ is optional. 

_HelperName_ , _BaseHelper_ and _ExtendedType_ have to be valid Pascal identifiers. In case of a class helper _ExtendedType_ must be a class and _BaseHelper_ must be another class helper that extends either the same class as _ExtendedType_ or a parent class of it. For record helpers _ExtendedType_ must be a record and _BaseHelper_ must be a record helper that extend the same record type. The declarations inside a helper are very similiar to the declaration of a normal class, but you must not use fields, destructors and class constructors/destructors. 

_Note_ : In mode ObjFPC record helpers need the modeswitch _advancedrecords_ to be declared; in mode Delphi no further modeswitch is necessary. 

### [[edit](</index.php?title=Helper_types&action=edit&section=4> "Edit section: Implementation")] Implementation

The rules when implementing a helper are a bit differently then when implementing a normal class or record. First of the type of _Self_ is always of the type of the extended type (e.g. _TObject_ in case of a helper declared as _class helper for TObject_). If a method is not defined in the helper type itself or it's prepended by "inherited", it is first searched in the extended type, then in all the parent helpers of the current helper and then (for class helpers) in the parent classes of the extended class. Please note that in the last case helpers for the parent classes are available as well. 

### [[edit](</index.php?title=Helper_types&action=edit&section=5> "Edit section: Restrictions")] Restrictions

A helper type may not 

  * contain (class) destructors 
  * contain class constructors 
  * contain (class) fields 
  * contain abstract methods 
  * "override" virtual methods of the extended class (they can be hidden by the helper though) 
  * for record helpers: contain constructors (this is not a restriction of helper types themselves, but of FPC, as constructors are not yet supported inside advanced records) 



Methods of the extended type can be overloaded (thus they are not hidden by the helper) by using the _overload_ keyword. 

Example: 
    
    
     
    TObjectHelper = class helper for TObject
      function ToString(const aFormat: String): String; overload;
    end;
     
    function TObjectHelper.ToString(const aFormat: String): String;
    begin
      Result := Format(aFormat, [ToString]);
    end;
     
    var
      o: TObject;
    begin
      Writeln(o.ToString('The object''s name is %s'));
    end.
     

### [[edit](</index.php?title=Helper_types&action=edit&section=6> "Edit section: Inheritance")] Inheritance

Inheritance for helper types fullfills two purposes: 

  * use the methods of multiple helpers at once 
  * (class helpers only) bring methods that were added to a parent class to the topmost visiblilty (because equally named methods of helpers are hidden by subclasses) 



#### [[edit](</index.php?title=Helper_types&action=edit&section=7> "Edit section: class helper")] class helper

A class helper can inherit from another class helper if it extends a subclass of the class or the same class which is extend by the parent class helper. 

Example: 
    
    
     
    TObjectHelper = class helper for TObject
      procedure SomeMethod;
    end;
     
    TFoo = class(TObject)
    end;
     
    TFooHelper = class helper(TObjectHelper) for TFoo
    end;
     

#### [[edit](</index.php?title=Helper_types&action=edit&section=8> "Edit section: record helper")] record helper

A record helper can inherit from another record helper if it extends the same record which is extend by the parent record helper. **Important** : Inheritance for record helpers is not enabled in mode Delphi, because there this is not allowed. 

Example: 
    
    
     
    TTest = record
    end;
     
    TTestHelper = record helper for TTest
      procedure SomeMethod;
    end;
     
    TTestHelperSub = record helper(TTestHelper) for TTest
    end;
     

### [[edit](</index.php?title=Helper_types&action=edit&section=9> "Edit section: Example")] Example

A simple example for extending existing functionality is the following: The LCL's _TCheckListBox_ , which is a list with checkoxes, does not provide a property to get the count of all checked items. Using a class helper this can be added rather easily. 
    
    
     
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
      Result := 0;
      for i := 0 to Items.Count - 1 do
        if Checked[i] then
          Inc(Result);
    end;
     
    // somewhere else (TCheckListBoxHelper needs to be in scope)
    if MyCheckList.CheckedCount > 0 then ...
     

Please note that it's not trivial to implement a cached variation, as helper types can't introduce fields. 

## [[edit](</index.php?title=Helper_types&action=edit&section=10> "Edit section: Usage")] Usage

### [[edit](</index.php?title=Helper_types&action=edit&section=11> "Edit section: Scope")] Scope

The methods declared inside the helper are always available once it is in scope and can be called as if they'd belong to the extended type. 

Example: 
    
    
     
    type
      TObjectHelper = class helper for TObject
        function TheAnswer: Integer;
      end;
     
    function TObjectHelper.TheAnswer: Integer;
    begin
      Result := 42;
    end;
     
    begin
      o := TObject.Create;
      o.TheAnswer;
    end.
     

Because of Delphi compatibility only one helper type can be active for a type at the same time. This helper is always the last one that is in scope (which follows the usual rules of Pascal). 

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
     

### [[edit](</index.php?title=Helper_types&action=edit&section=12> "Edit section: Method hiding")] Method hiding

The methods of helper types hide the methods declared in the extended type as long as they are not declared with "overload". 

If a class helper is in scope for a parent class then only he methods of the parent class are hidden while the methods of the subclass will stay visible. 

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
     

### [[edit](</index.php?title=Helper_types&action=edit&section=13> "Edit section: Restrictions")] Restrictions

You can not reference helper types anywhere in the code except for the following cases: 

  * inheriting from one 
  * using one in (Bit)SizeOf 
  * using one in TypeInfo 



## [[edit](</index.php?title=Helper_types&action=edit&section=14> "Edit section: Differences")] Differences

### [[edit](</index.php?title=Helper_types&action=edit&section=15> "Edit section: Differences between mode Delphi and ObjFPC")] Differences between mode Delphi and ObjFPC

In mode Delphi you can use _virtual_ , _dynamic_ , _override_ and _message_ identifiers. As the concepts behind those identifiers (virtual methods, message dispatching) isn't applicable for helpers, those keywords are ignored in mode Delphi and not allowed in mode ObjFPC. 

In mode Delphi you can't inherit a record helper from another one and you can not use the "inherited" keyword (not even to call the method of the extended record). 

### [[edit](</index.php?title=Helper_types&action=edit&section=16> "Edit section: Differences between Delphi and FPC")] Differences between Delphi and FPC

While the helper feature was implemented as Delphi compatible as possible there is one difference between the two:  
In Delphi a helper is basically a class which inherits from a class _TClassHelperBase_ (both class and record helpers) which in turn inherits from _TInterfacedObject_. In FPC helpers are a type for themself and don't have a basetype.  
As this difference is only visible in the RTTI (which is seldomly used for helpers) this difference was considered neglectable. 

Because helper are there own type in FPC they also have a custom type kind (_tkHelper_) and their own fields in TTypeData: 

  * HelperParent: a PTypeInfo field that points to the type info of the parent helper (can be **nil**) 
  * ExtendedInfo: a PTypeInfo field that points to the type info of the extended type 
  * HelperProps: contains the count of the (published) properties the helper contains 
  * HelperUnit: contains the name of the unit the helper is defined in  




The usual RTTI methods can be used to query for properties of the helper. 

_Note_ : _ExtendedInfo_ and _HelperUnit_ don't have a Delphi equivalent. 

Example: 
    
    
     
    type
      TObjectHelper = class helper for TObject
      end;
     
    var
      ti: PTypeInfo;
      td: PTypeInfo;
    begin
      ti := TypeInfo(TObjectHelper);
      td := GetTypeData(ti);
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
     

## [[edit](</index.php?title=Helper_types&action=edit&section=17> "Edit section: Code examples")] Code examples

The following table lists some examples for class helpers found on the web and whether they work with the current implementation. 

URL |  State   
---|---  
[Class helper to add for ... in support for TComponent.Components / ComponentCount](<http://wiert.wordpress.com/2009/05/07/delphi-class-helper-to-add-for-%E2%80%A6-in-support-for-tcomponentcomponentscomponentcount/> "http://wiert.wordpress.com/2009/05/07/delphi-class-helper-to-add-for-%E2%80%A6-in-support-for-tcomponentcomponentscomponentcount/") |  ok   
[Class helper for Delphi's TStrings: Implemented Add(Variant)](<http://delphi.about.com/od/delphitips2008/qt/tstrings-helper.htm> "http://delphi.about.com/od/delphitips2008/qt/tstrings-helper.htm") |  ok

---

_Source: [https://wiki.freepascal.org/Class_helpers](https://web.archive.org/web/20110829085502/https://wiki.freepascal.org/Class_helpers)_
