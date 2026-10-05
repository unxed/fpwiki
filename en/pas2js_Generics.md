# pas2js Generics

## Contents

  * 1 Overview
    * 1.1 Working
  * 2 RTL Units
  * 3 RTL Classes
    * 3.1 ToDos



# Overview

Pas2js 1.5 supports generic types and functions in either the ObjFPC or Delphi like way. 

Next goal is to test and fix bugs. 

## Working

  * generic class 
    * ClassName, TypeInfo Name 
      * Delphi, pas2js: full path with param names, e.g. 'TAnt<System.Word>' and 'TAnt<System.Word>.TLeg'
      * FPC: instead of param names uses crc, e.g. 'TAnt$1$crcHexNumber', full paths for classnames, e.g. 'TAnt$1$crcHexNumber.TLeg', last name for TypeInfo: e.g. 'TLeg'
    * JS specialize code in unit of generic
    * forward generic class, e.g. _type TAnt <T>=class; TBird=class a: TAnt<word>; end; TAnt<T>=class a:T; end;_
      * Delphi/pas2js: supported
      * FPC: 3.3.1 does not support it yet
    * enumtype inside generic is not propagated
    * ObjFPC: members can refer to parent type without type parameters, e.g. _type generic TAnt <T> = class Parent: TAnt; end;_
    * Delphi/FPC: descendants cannot refer to type parameters of ancestors, pas2js allows it within same unit (strict private).
    * Delphi/FPC does not allow accessing local symbols. pas2js allows it.
    * class constructor
  * generic external class
  * generic record
  * generic dynamic array
  * generic static array: Note: delphi wiki says "no static arrays", but 10.3 compiles it, see <http://docwiki.embarcadero.com/RADStudio/Rio/en/Declaring_Generics>
  * generic interface
  * generic procedural type 
    * specialized proc types can assign normal proc
    * Procedure local scope: 
      * pas2js: allowed, except for typeinfo
      * FPC: allowed, including typeinfo
      * Delphi: Error: parameterized type can not be declared in procedure local scope
  * anonymous type in specialize: _TBird <array of word>_
  * Delphi type overloads A, A<T> and A<S,T>
  * specialize: 
    * ObjFPC: _type TFoo = specialize TGen <word>;_
    * Delphi: _type TFoo = TGen <word>;_
    * ObjFPC anonymous: _var r: specialize TBird <word>;_
    * Delphi: anonymous _var r: TBird <word>;_
  * nested specialize: TBird<TWing<byte>>
  * record/class field
  * property
  * method (not confused with generic method) 
    * Delphi: implementation must repeat type params, must omit constraints
    * FPC: implementation must not repeat type params
  * helper for generic type
  * constraints: 
    * keyword _record_ , _class_ , _constructor_ checked at specialization
    * class type checked at specialization
  * constraints: 
    * "class": 
      * Delphi/FPC: T is TObject
      * pas2js: test if T is a class, either TObject or an external class
    * "constructor": test if T is TObject and Create resolves to TObject.create
    * "record": test if T is a record type
    * class type: test if param fits
    * list of interface types: test if param fits
    * forward class must repeat constraints
    * Delphi does not allow _TObject_ as constraint. pas2js allows it, because it has other root classes.
    * constraint can refer prior templates: _type TBird <X, Y: TAnt<X>> = record a: X; b: Y; end;_. This is Delphi compatible. FPC 3.3.1 does not support this (23th Aug 2020).
  * statements: 
    * specialize (cloning) all kinds of statements and expressions
    * for-loop, if-then-else, repeat-until, while-do, try-finally, try-except, case-of
    * assignments
    * primitive expression like identifiers and constants
    * operators: +, -, *, /
    * inline specialize expression, e.g. _TList <word>.Create_
      * Delphi: TFoo<Integer>.Create
      * FPC: specialize TFoo<Integer>.Create
    * "obj is T": allowed with constraint "class" or class type
    * typecast T(): allowed with constraint "class" or class type
    * using implementation function in generic function: 
      * Delphi: Error: Method of parameterized type declared in interface section must not use local symbol '%s'
      * FPC: Error: Global Generic template references static symtable
      * Pas2js: allowed
    * anInt:=GenericVar 
      * Delphi: always Error: Incompatible types: 'Integer' and 'T'
      * FPC/pas2js: allowed for valid combinations -> operators are checked on specialization, Note: because of implementation cross uses, check is delayed until used unit implementation is complete
    * for-in T do -> T must have constraints
    * typeinfo(T)
    * T is TFoo<Integer>
    * T as TFoo<Integer>
    * TFoo<Integer>(expr)
    * call 
      * pas2js: Later checked by specialization.
      * Delphi: must fit the constraint. For example without constraint it only fits untyped args.
      * FPC: if there is only one function in scope select it, overloads are selected by constraints alone. Later checked by specialization.
  * generic function (aka parametrized method): 
    * ObjFPC: _generic procedure Fly <T>(a: T)_
    * Delphi: _procedure Fly <T>(a: T)_ Note: Delphi 10.3 only supports parametrized method, not global procedure, pas2js allows it
    * forward
    * unit interface/implementation
    * overloads
    * generic procedure inside a procedure is forbidden, same as FPC/Delphi
    * local procedures inside a generic function: supported by FPC and pas2js, not supported by Delphi
    * calling self
    * constraints, see above, same as type constraints
    * ObjFPC explicitly spezialize: _specialize Fly <word>(aWord)_
    * Delphi explicitly spezialize: _Fly <word>(aWord)_
    * parameter type inline specialization: _procedure Run <T>(List: TList<T>);_
  * generic method (aka parametrized method): 
    * methods: virtual, message, constructor and destructors cannot have type parameters
    * class interface methods cannot have type parameters
    * proc types
    * overloads
  * automatically inferring types of generic method/function: 
    * _MyProc(1)_ calls _MyProc <T>(a: T)_
    * **$modeswitch implicitfunctionspecialization** : mode Delphi: default enabled, ObjFPC: disabled
    * array of T: _procedure Run <T>(a: array of T)_
    * any order: _procedure Run <S,T>(a:T; b:S)_
    * infer types: widen types, e.g. _Run(1,1000000)_ specializes _Run <T>(a,b:T)_ with _T_ as _longint_. Not supported by Delphi/FPC, supported by pas2js to reduce number of specializations
  * typecast between specializations gives a warning, not an error
  * typecast TGenType<jsvalue> from/to TGenType<SomeType> without warning
  * allowing ATExtClass<jsvalue> := ATExtClass<SomeType>
  * function GetTypeKind(aType): TTypeKind
  * RTTI 
    * RTTI names use FPC/Delphi like _TList <word>_ instead of the JS name _TList$G1_



# RTL Units

  * generics.defaults.pas
  * generics.strings.pas
  * generics.collections.pas



# RTL Classes

  * TArray<T>
  * IComparer<T>
  * TOnComparison<T>
  * TComparer<T>
  * TDefaultComparer<T>
  * IEnumerator<T>
  * IEnumerable<T>
  * TCollectionNotifyEvent<T>
  * TCustomArrayHelper<T>
  * TArrayHelper<T>
  * TEnumerator<T>
  * TEnumerable<T>
  * TCustomList<T>
  * TCustomListEnumerator<T>
  * TCustomInvertedListEnumerator<T>
  * TList<T>
  * TObjectList<T: class>
  * TThreadList<T>
  * TQueue<T>
  * TObjectQueue<T: class>
  * TStack<T>
  * TObjectStack<T: class>
  * TPair<TKey,TValue>
  * TDictionary<TKey,TValue>



## ToDos

  * anonymous record, e.g. _type TAnt <T> = class a: record end; end;_
  * anonymous set, e.g. _type TAnt <T> = class a: (red, blue); end;_
  * type alias type as parameter: create separate specialization, at the moment _TList <integer>_ and _TList <TColor>_ are the same specialization in pas2js
  * generic class: 
    * nested generic type: i.e. generic inside a generic. Allowed by Delphi, not allowed by FPC
    * cascaded specialize: TBird<word>.TWing<byte>
    * generic ancestor, allowed in FPC, not allowed in Delphi: _TExample <T: class> = class(T) end;_
  * nicer error messages 
    * write a hint where it was specialized
  * Redefinition. Example: _type TBird <T> = class i: integer; end; integer = string; TEagle = TBird<integer>;_ FPC+pas2js: create i as _string_ , Delphi: creates i as _system.integer_
  * optimization: reuse specialized functions
  * constants as templates
  * precompiled js files. At the moment all specializations are added to the unit where the generic is defined. That means using a new specialization require to recompile the js file.

---

_Source: [https://wiki.freepascal.org/pas2js_Generics](https://web.archive.org/web/20240304094128/https://wiki.freepascal.org/pas2js_Generics)_
