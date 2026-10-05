# Templates

│ **English (en)** │  **[français (fr)](</Templates/fr> "Templates/fr")** │  **[русский (ru)](<../ru/Templates.md> "Templates/ru")** │    
****

## Contents

  * 1 Introduction
  * 2 Usage
    * 2.1 Inheritance
    * 2.2 Constraints
  * 3 Generic classes
  * 4 Existed generic libraries
  * 5 See also
  * 6 External links



## Introduction

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Templates as presented on this page are no longer needed because [Generics](<Generics.md> "Generics") support in FPC is complete and usable enough.

_Templates_ is a simple mechanism which can be used to solve the problem of writing duplicate code to implement a general class for specific types in a strongly typed language. It is used mainly for base (non-object) types; for objects, inheritance from the single base class TObject is usually used to build general classes. In advanced languages, templates are replaced by a native-language implementation of so-called “generics” or ”parametrized types”. Templates are another more complex alternative to implement generic code with FPC. 

**Pros:**

  * Type safe – eliminates the annoyance of typecasting and the errors it often causes
  * Better maintainability – generic class code is written only once and used for all specialized classes
  * Better code reuse – It is possible to create a good set of reusable generic classes
  * Better efficiency, scalability – generic class types can be better adjusted for particular needs. _E.g.,_ on a 16-bit machine, a specialized TList with a 16-bit SmallInt index type could be used.



**Cons:**

  * More complex
  * Bigger code – every specialized class is compiled separately and produces additional code



## Usage

The classic example of a basic generic class is a list of items. Free Pascal currently offers three ways of using lists: 

  * **array of** – classical dynamic array structure which can hold dynamic items of the same type. Items are accessible by direct addressing: SomeArray[Index]. SetLength and Length functions are used to handle size.
  * **TList** – object-oriented way in which the class handles all list operations. Implementation in LCL keeps compatibility with VCL, where TList holds a list of Pointers. If a list of a different item type is desired, a complete new class has to be copied and rewritten or typecasting has to be used in every place where a list item is referenced. Typecasting is not effective— _e.g.,_ Pointer(Byte)—and not type safe: Pointer(Int64). A TList index is of type Integer. If Int64 or SmallInt index is needed, then the class has to be copied and rewritten.
  * **TCollection** – is a more generic and type safe but heavier solution for storing a list of items. In this approach the programmer has to create an item class which inherits from TCollectionItem and set this newly created class type to TCollection constructor parameter.
  * **native generics** – no complete implementation yet. Lacks support of generic type reference, inheritance, constraints.
  * **templates** – use code inclusion to work around lack of native support. Can use inheritance and generic type references. Constraints are given by operators and other functions in the generic class.



Templates tries to solve the problem using the concept of parametrized include files. A generic unit is written only once, as a template with the use of generic (not specified) types in both interface and implementation sections. Then in the process of specialization, a class template is used and general template parameters are replaced by specific types to create a new specialized class. 

_GenericList.inc:_
    
    
    {$IFDEF INTERFACE}     
      // TGList<T> = class
      TGList = class
        Items: array of T;
        procedure Add(Item: T);
      end;
    {$UNDEF INTERFACE}     
    {$ENDIF}     
    
    {$IFDEF IMPLEMENTATION}     
    procedure TGList.Add(Item: T);
    begin
      SetLength(Items, Length(Items) + 1);  
      Items[Length(Items) - 1] := Item;
    end;
    {$UNDEF IMPLEMENTATION}     
    {$ENDIF}
    

In process of specialization early created template files are included to new unit. 

_ListInteger.pas:_
    
    
    unit ListInteger;
    {$mode ObjFPC} 
    interface
    
    uses
      Classes;
    
    type
      T = Integer; // T is specified to some exact type
    {$DEFINE INTERFACE}
    {$INCLUDE 'GenericList.inc'}
    
    type
      // TListInteger<Integer> = class
      TListInteger = class(TGList)
        // Additional fields and methods can be added here
      end;
    
    implementation
    
    {$DEFINE IMPLEMENTATION}
    {$INCLUDE 'GenericList.inc'}
    
    end.
    

Finally we have new specialized unit called ListInteger and we can use our new specialized type in some code. 
    
    
    program GenericTest;
    {$mode ObjFPC}
    uses
      ListInteger;
    var
      List: TListInteger;
    begin
      try
        List := TListInteger.Create;
        List.Add(1);
        List.Add(2);
        List.Add(3);
      finally
        List.Free;
      end;
    end.
    

### Inheritance

Generic classes could inherit from each other in way of non-generic classes do. Implementation is little difficult as only one specialized type of particular class can be defined per unit. To be able to specify generic class which inherits from other generic class some naming scheme should be kept. 

Assume that we want to create advanced generic list TAdvancedList which have Capacity field in addition. 

Base generic class definition: _GenericList.inc:_
    
    
    {$IFDEF INTERFACE}     
      // TGList<TListIndex, TListItem> = class
      TGList = class
        Items: array[TListIndex] of TListItem;
        procedure Add(Item: TListItem);
      end;
    {$UNDEF INTERFACE}
    {$ENDIF}
    
    {$IFDEF IMPLEMENTATION}
    procedure TGList.Add(Item: TListItem);
    begin
      SetLength(Items, Length(Items) + 1);  
      Items[Length(Items) - 1] := Item;
    end;
    {$UNDEF IMPLEMENTATION}
    {$ENDIF}
    

Definition for enhanced generic class: 

_GenericAdvancedList.inc:_
    
    
    {$IFDEF INTERFACE}     
      TListIndex = TAdvancedListIndex;
      TListItem = TAdvancedListItem;
    {$DEFINE INTERFACE}     
    {$INCLUDE 'GenericList.inc'}
      // TGAdvancedList<TAdvancedListIndex, TAdvancedListItem> = class(TGList)
      TGAdvancedList = class
        Capacity: TAdvancedListIndex;
      end;
    {$UNDEF INTERFACE}
    {$ENDIF}
    
    {$IFDEF IMPLEMENTATION}
    {$INCLUDE 'GenericList.inc'}
    {$UNDEF IMPLEMENTATION}
    {$ENDIF}
    

Now a specialization example: 

_AdvancedListInteger.pas:_
    
    
    unit AdvancedListInteger;
    
    interface
    
    uses
      Classes;
    
    type
      TAdvancedListIndex = Integer; // specified to some exact type
      TAdvancedListItem = Integer; // specified to some exact type
    {$DEFINE INTERFACE}
    {$INCLUDE 'GenericAdvancedList.inc'}
    
    type
      // TAdvancedListInteger<Integer, Integer> = class
      TAdvancedListInteger = class(TGAdvancedList)
        // Additional fields and methods can be added here
      end;
    
    implementation
    
    {$DEFINE IMPLEMENTATION}
    {$INCLUDE 'GenericAdvancedList.inc'}
    
    end.
    

Now we have specialized class from generic class which inherits form other generic class. 

### Constraints

It is good to have class which e.g. works as containers for any other type. Container class doesn't need to know anything about contained class. But some classes could benefits from knowledge of what can be done with contained class. In native generics implementation there is possibility to restrict group of types which can be used as type parameters of generic class. In templates there aren't such feature but constraints are done simply by usage of generic type in generic class. If generic type is used for addition and subtraction than only types which supports that operations can be used. String can be concatenated using plus operator too. But if multiplication is used than only numeric types are allowed. If generic type is used as array index, than only ordinal types can be used. If some e.g. Free method is used over generic type than constraint include all classes. So in templates operations performed with generic type limits set of usable types for specialization. 

**Theoretical constraint groups:**

  * ordinal types
  * float types
  * records
  * arrays
  * sets
  * interface
  * procedure, function, constructor
  * classes with all descendants



## Generic classes

There are many classes which can benefit from being generic: 

  * TList - list of items, Add, Delete, Insert, Move, Exchange, Clear, Sort,
  * TDictionary - list of key and value pairs,
  * TPair - Key and Value for use in TDictionary
  * TStack - LIFO structure, Push and Pop
  * TQueue - FIFO structure, Enqueue and Dequeue
  * TRange or TInterval - two values structure, Distance
  * TSet - set of items, Add, Remove, Intersection, Complement, Union, Product
  * TStream - divided to TInputStream and TOutputStream
  * TTree - hiearchical structure with nodes
  * TMatrix - multidimensional list
  * TBitmap - extended TMatrix with graphic oriented methods
  * TGraph
  * TPoint - could be 1D, 2D, 3D or multidimensional
  * TVector
  * TComplexNumber



There are some interesting specialized types which can replace non-generic types: 

Specialized generic class | Similar data type   
---|---  
TList<Char> | string   
TList<WideChar> | WideString   
TList<Byte> | array of Byte   
TList<string> | Classes.TStrings   
TList<Pointer> | Classes.TList   
TList<Boolean> | Classes.TBits   
TList<TObject> | Contnrs.TObjectList   
TList<TComponent> | Contnrs.TComponentList   
TList<TClass> | Contnrs.TClassList   
TStack<Pointer> | Contnrs.TStack   
TStack<TObject> | Contnrs.TObjectStack   
TQueue<Pointer> | Contnrs.TQueue   
TQueue<TObject> | Contnrs.TObjectQueue   
TStream<Byte> | Classes.TMemoryStream   
TDictionary<TPair<string, string>> | Classes.TStringList   
TList<TMethod> | LCLProc.TMethodList   
TTree<TTreeNode> | ComCtrls.TTreeView   
TPoint<Integer> | Classes.TPoint   
TPoint<SmallInt> | Classes.TSmallPoint   
TRectangle<Integer> | Classes.TRect   
  
## Existed generic libraries

Every programmer can build own library of generic classes. 

**Some ready to use classes:**

  * **TemplateGenerics** \- Lazarus package of some experimental generic classes. In addition to parametrized list value type it also offer parametrized index type. Latest package can be downloaded from svn repository using svn client.


    
    
    svn co http://svn.zdechov.net/PascalClassLibrary/Generics/TemplateGenerics/ TemplateGenerics

  


## See also

  * [Generics](<Generics.md> "Generics")



## External links

  * [Object Pascal (Delphi) Templates](<http://www.dummzeuch.de/delphi/object_pascal_templates/english.html>)
  * [Templates in Object Pascal](<http://edn.embarcadero.com/article/27603>)
  * [Template (programming)](<http://en.wikipedia.org/wiki/Template_%28programming%29>) \- general description and principles
  * [Generics with Delphi 2009 Win32](<http://sjrd.developpez.com/delphi/tutoriel/generics/>)

---

_Source: [https://wiki.freepascal.org/Templates](https://web.archive.org/web/20250512095646/https://wiki.freepascal.org/Templates)_
