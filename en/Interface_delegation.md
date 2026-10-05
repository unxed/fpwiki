# Interface delegation

│ **English (en)** │    
****

back to contents [FPC internals](<FPC_internals.md> "FPC internals")

This page describes FPC implementation of interface delegation, and its differences with Delphi. 

## Contents

  * 1 Structure of implemented interfaces
    * 1.1 RTTI members
    * 1.2 Instance changes
  * 2 GetInterface function and the 'as' operator
  * 3 Typecasting class to interface
  * 4 Calls to interface methods



## Structure of implemented interfaces

### RTTI members

Whenever a class is declared as implementing one or more interfaces, its implemented interfaces are stored in a table which is accessible at runtime with the `TObject.GetInterfaceTable` function. This function returns a pointer to a `TInterfaceTable` record, which contains a number of elements followed by one or more `TInterfaceEntry` records: 
    
    
    TInterfaceTable = record
      EntryCount: ptruint;
      Entries   : array [0..0] of TInterfaceEntry;
    end;
    

Each implemented interface is described by a `TInterfaceEntry` (alignment details omitted, see rtl/inc/objpash.inc for full declaration): 
    
    
    TInterfaceEntry = record
      IID     : pguid;        { this is nil for Corba interfaces, non-nil for COM }
      VTable  : Pointer;
      IOffset : ptruint;      { meaning depends on IType field, see below }
      IIDStr  : pshortstring; { never nil. For COM interfaces, this is upper(GuidToString(IID^)) }
      IType   : TInterfaceEntryType;
    end;
    
    InterfaceEntryType = (
      etStandard,             { Implemented directly by the class }
                              {   IOffset is offset from start of object instance to hidden field storing interface VMT }
      etVirtualMethodResult,  { Delegated to property of type interface, which is accessed by virtual method }
                              {   IOffset is offset from class VMT to slot storing the accessor function }
      etStaticMethodResult,   { Delegated to property of type interface, which is accessed by static method }
                              {   IOffset is direct address of the accessor method }
      etFieldValue,           { Delegated to property of type interface, which is accessed by field }
                              {   IOffset is offset from start of object instance to accessor field }
      etVirtualMethodClass,   { Same as etVirtualMethodResult, but accessor property has class type }
      etStaticMethodClass,    { Same as etStaticMethodResult, but accessor property has class type }
      etFieldValueClass       { Same as etFieldValue, but accessor property has class type }
    );
    

Delphi uses a different TInterfaceEntry structure. Since Delphi does not support Corba interfaces, it does not have an IIDStr counterpart. More importantly, interfaces delegated to class-type properties are stored by Delphi as 'directly implemented', and there's no easy way for runtime to determine whether they are accessed by field or by method. 

### Instance changes

For interfaces implemented directly by a class, each instance of the class gets a hidden field containing a pointer to the VMT of the interface. Thus, every directly implemented interface increases the class instance size by sizeof(Pointer) bytes. An offset to this field is stored by compiler in the `IOffset` field of the corresponding `TInterfaceEntry`. At runtime, the `TObject.InitInstance` procedure initializes these hidden fields with proper VMT pointers. 

FPC specifics: we use a special value of FPC_EMPTYINTF as a value for the class interface table whenever there compiler can determine that neither the class itself nor any of its ancestors implement any interfaces. FPC_EMPTYINTF is simply a pointer to an interface table with zero entries. Its difference to a nil pointer is that `TObject.InitInstance` immediately exits when it encounters FPC_EMPTYINTF. This way, we yield higher construction speed for the 'normal' (non-interfaced) objects. 

Delphi specifics: a hidden field is also added for interfaces delegated to class-type properties. 

## GetInterface function and the 'as' operator

The semantics of casting a class to an interface using the 'as' operator should be as follows: 

  * For COM interfaces, the reference counter is increased by one;
  * If the typecast is not possible, an EInvalidTypecast exception is raised.



Using the 'as' operator in the text of the program typically ends up in a call to the `TObject.GetInterface` method, unless the class overrides the standard behavior, e.g. by providing a customized QueryInterface method. For Corba interfaces, the `TObject.GetInterfaceByStr` method is called instead, specifying the string identifier of the interface rather than its GUID. 

## Typecasting class to interface

## Calls to interface methods

From the caller's point of view, the calling method of an interface looks exactly like calling a virtual method of a class/object (_TODO: add more details on this_). However, the VMT slots of an interface point to the special code called "interface wrappers". The compiler generates a wrapper for every procedure of every interface implemented directly by class (Delphi also generates wrappers for interfaces that are delegated to class properties). The purpose of wrappers is to adjust the implicit 'Self' parameter so it points back to the implementing class, and then jump to the actual implementing procedure.

---

_Source: [https://wiki.freepascal.org/Interface_delegation](https://web.archive.org/web/20230326151745/https://wiki.freepascal.org/Interface_delegation)_
