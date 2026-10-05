# Generics

│ **English (en)** │

## Contents

  * 1 Introduction
  * 2 fgl unit
    * 2.1 Getting Started
  * 3 Custom Generic Classes
  * 4 Other Points
  * 5 Example
  * 6 See also
  * 7 External links



## Introduction

[FPC](<FPC.md> "FPC") has had official support for generics in [`{$mode ObjFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC") since version 2.2. and in [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi") since version 2.6.0. 

Generics are sometimes called parameterized types. 

The reason that FPC supports two different dialects is simply that FPC implemented generics several years before [Delphi](<Delphi.md> "Delphi"). 

It is possible to use [units](<Unit.md> "Unit") written in `{$mode ObjFPC}` syntax in other units that use `{$mode Delphi}`, and vice versa. 

The **Free Generics Library** or **FGL** is an FPC-native collection of generic containers written in `{$mode ObjFPC}`. 

The **rtl-generics** package is a larger, somewhat more featureful collection of generic containers written in `{$mode Delphi}` that tries to be compatible with the Delphi generics library. This package is provided as standard in FPC 3.1.1.+ but there is a version for FPC 3.0.4. available. 

Both **FGL** and **rtl-generics** can be used in both syntax modes. 

## fgl unit

The easiest way to get started with generics is to use the [fgl unit](<http://freepascal.org/docs-html/current/rtl/fgl/index.html>), which is a prototype unit for base system generic [classes](<Class.md> "Class"). So far it contains a few basic classes: 

  * TFPGList
  * TFPGObjectList
  * TFPGInterfacedObjectList
  * TFPGMap



### Getting Started

The following simple example shows how to store multiple instances of a user defined class in a list: 
    
    
    {$mode objfpc}
    uses fgl;
    
    type
       TMyClass = class(TObject)
          fld1 : string;
       end;
    
       TMyList = specialize TFPGObjectList<TMyClass>;
    
    var
       list : TMyList;
       c : TMyClass;
    
    begin
       // create the list and add an element
       list := TMyList.Create;
       c := TMyClass.Create;
       c.fld1 := 'c1';
       list.Add(c);
       // retrieve an element from the list
       c := list[0];
    

## Custom Generic Classes

If the generics defined in the fgl unit do not suit your needs, you may need to define your own generic classes from scratch using the underlying language primitives. 

A generic class is defined using the [keyword](<Keyword.md> "Keyword") **generic** before the class name and use in class declaration: 
    
    
    type
      generic TList<T> = class
        Items: array of T;
        procedure Add(Value: T);
      end;
    

Example of generic class implementation: 
    
    
    implementation
    
    procedure TList.Add(Value: T);
    begin
      SetLength(Items, Length(Items) + 1);
      Items[Length(Items) - 1] := Value;
    end;
    

A generic class, [object](<Object.md> "Object"), [record](<Record.md> "Record"), [interface](<Interface.md> "Interface") or [method](<Method.md> "Method") can be simply specialized for a particular type by using the **specialize** keyword. 
    
    
    Type  
      TIntegerList = specialize TList<Integer>;
      TPointerList = specialize TList<Pointer>;
      TStringList = specialize TList<string>;
    

## Other Points

  1. The [compiler](<Compiler.md> "Compiler") parses a generic, but instead of generating code it stores all tokens in a token buffer inside the PPU file.
  2. The compiler parses a specialization; for this it loads the token buffer from the PPU file and parses that again. It replaces the generic parameters (in most examples "T") by the particular given type (e.g. **[LongInt](<Longint.md> "Longint")** , **[TObject](<TObject.md> "TObject")**).



The code basically appears as if the same class had been written as the generic but with T replaced by the given type. 

Therefore in theory there should be no speed differences between a "normal" class and a generic one. 

## Example

An example of how to use generics to write a `function gmax()` that takes the maximum of two not-yet-typed [variables](<Variable.md> "Variable"). Note that while the functions here are namespaced by the classname, FPC versions from 3.1.1 onwards also support fully free-standing generic methods. 
    
    
    program UseGenerics;
    
    {$mode objfpc}{$H+}
    
    type
      generic TFakeClass<_GT> = class
        class function gmax(a,b: _GT):_GT;
      end;
    
      TFakeClassInt = specialize TFakeClass<integer>;
      TFakeClassDouble = specialize TFakeClass<double>;
    
      class function TFakeClass.gmax(a,b: _GT):_GT;
      begin
        if a > b then 
          result := a
        else 
          result := b;
      end;
    
    begin
        // show max of two integers
      writeln( 'Integer GMax:', TFakeClassInt.gmax( 23, 56 ) );
        // show max of two doubles
      writeln( 'Double GMax:', TFakeClassDouble.gmax( 23.89, 56.5) );
      readln();
    end.
    

  


## See also

  * [Templates](<Templates.md> "Templates")
  * [Generics proposals](<Generics_proposals.md> "Generics proposals")
  * [Data Structures, Containers, Collections](<Data_Structures,_Containers,_Collections.md> "Data Structures, Containers, Collections")



## External links

  * [Free Pascal: Reference guide - Generics](<http://www.freepascal.org/docs-html/ref/refch8.html>)

---

_Source: [https://wiki.freepascal.org/FGL](https://web.archive.org/web/20201204025542/https://wiki.freepascal.org/FGL)_
