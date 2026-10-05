# Generics proposals

## Contents

  * 1 Already implemented!
  * 2 Why
  * 3 Interested Parties
  * 4 Usage example (uncategorized)
  * 5 Usage example
    * 5.1 Using brackets: <>
    * 5.2 Using the "generic" keyword
      * 5.2.1 Suggestion 2
      * 5.2.2 Suggestion 3
  * 6 Terms
  * 7 Changes
    * 7.1 Changes in class/record definition representation
    * 7.2 Changes in 'type user' compilation (for class/record types only)
    * 7.3 type params
  * 8 Other things to note (for *later*)
    * 8.1 What if a method implementation depends on the type used to know how the implementation should look like ?
    * 8.2 How to do internal storage classes
    * 8.3 How to derive from the generic type
    * 8.4 Interfaces should support generics too
    * 8.5 Limitation of possible specialisations
    * 8.6 What to do with class functions
    * 8.7 'class of'
  * 9 Implementation details
    * 9.1 Sanity checks
    * 9.2 Things to note and not forgot
  * 10 Implementation steps
    * 10.1 Extend parser to support '<T>' and '<T: bla or bla or bla>' and '<T,Q>' notation
  * 11 Different ways to do it, one excluding the other
    * 11.1 Way 1
      * 11.1.1 Change class/record/object/pointertype/interface parser to mark if it is a generic, a specialization or normal
      * 11.1.2 insert dummy symbols (typesym + typedef) for the type params in the generic
      * 11.1.3 parse implementation section methods *for the generic*
      * 11.1.4 when parsing generics, use a dummy type in place of the type params
      * 11.1.5 Change derivation handlers to do the sanity checks
      * 11.1.6 Add error messages for it
      * 11.1.7 Change pexpr (tnode creating)
      * 11.1.8 Change ppu streaming routines to save the tree of generics to the ppu
      * 11.1.9 Change type user to instantiate specialized types for generics when used
    * 11.2 Way 2
      * 11.2.1 Notes
  * 12 Already existing stuff
    * 12.1 metaclasses
  * 13 To be incorporated here
    * 13.1 Chat logs
  * 14 See also



## Already implemented!

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Generic classes are already implemented in the 2.3.1 version of FPC. 

See the [FPC reference](<http://www.freepascal.org/docs-html/ref/ref.html>).

This page contains proposals and ideas, which have lead to the current implementation and for further features. 

**Note, April 8th 2019: almost everything on this page is highly outdated and does not reflect the current state of FPC features.**

## Why

  * Typesafety !
  * Speed
  * Readability



See "Already existing stuff" for possible but lacking ways right now. 

## Interested Parties

\- Almindor \- dannym \- fpk \- giantm \- oliebol \- plugwash \- Synopsis \- tom \- PublicJoke \- neli \- dr_evil 

## Usage example (uncategorized)

TODO: combine the below stuff into one example using the <> syntax 

(tom proposal) 
    
    
      type
      // explicit addition of "generic" keyword. Maybe something like "uses generic <...>" 
      // better use the <>-brackets because otherwise it may cause confusion with arrays 
      // in the actual instantiation
      abc = class(..) generic <a, b>
      end;
      var
            //  because they're still unused ;)
            xyz : abc<String, Integer>;
            xyz2 : abc<AObj1, AObj2>;
      begin
        // don't repeat generics in instance creation (if not necessary)
        xyz := abc.Create;
        xyz2 := abc<descendant_of_Aobj1, descendant_of_Aobj2>.Create;
      end.
    

(dannym proposal new) 
    
    
      type
      GList = generic class of T
      public
        procedure Add(const stuff: T);
      end;
    
      procedure GList.Add(const stuff: T); // note that here no 'of T'
      begin
        ...
      end;
    
      var
        intlist: GList of Integer;
      begin
        intlist := GList.Create; // note that here the detail type is missing and 
                // needs to be deducted from the var declaration
        //or intlist := typeof(intlist).Create;
        //intlist := GList of Integer.Create; <-- this is unclear if that works
      end;
    

(dannym proposal - old) 
    
    
      type
      GList<T> = class
      public
        procedure Add(const stuff: T);
      end;
    
      procedure GList<T>.Add(const stuff: T);
      begin
        ...
      end;
    
      var
        intlist: GList<Integer>;
      begin
        intlist := GList<Integer>.Create;
      end;
    

plugwash: 
    
    
      tmycollection=template(tobject)[x]
      tintegercollection=tmycollection[integer]
    

oliebol: 
    
    
      template blaat<t:integer>(a:t);
      var x : t;
      begin
        do something with a, and use t with it
      end;
    

note that we should have a generics section (like interface and implementation sections) containing the generics: 
    
    
      unit xyz;
      generics
      type
        GList......
      procedure GList.Add(const value: T);
      ....
      end.
    

(eiffel) 

<http://paste.lisp.org/display/6236>

(PublicJoke proposal) 
    
    
      unit Unit1(TMathType);
      
      interface
      
      type
        TMathClass = class
        public
          class function Add(a1, a2: TMathType): TMathType;
        end;
      
      implementation
      
      class function TMathClass.Add(a1, a2: TMathType): TMathType;
      begin
        Result:=a1+a2;
      end;
      
      end.
    
    
    
      unit Unit2;
      
      type
        TMathType = Longword;
      
      interface
      
      implementation
      
      end.
    
    
    
      program prog;
      
      uses
        Unit2, 
        Unit1(Byte) as ByteMath in 'Unit1.pas',
        Unit1(TMathType) as PolyMath in 'Unit1.pas';
      
      type
        TByteMathClass = ByteMath.TMathClass;
        TPolyMathClass = PolyMath.TMathClass;
      
      var
        b: Byte;
        p: TMathType;
      
      begin
        b:=TByteMathClass.Add(1, 2);
        p:=TPolyMathClass.Add(3, 4);
      end.
    

(Gadnio proposal) I personally think the C++ way of doing this is clear enough AND will be the easiest to parse/understand: 
    
    
      template <T>
        TGenericObject< T > = record
          Data : T;
          OtherStuff : Pointer;
        end;{TGenericObject}
      template <TKey, TData = TGenericObject< boolean > >
        TGenericList = class( TWhee< TKey > )
        public
          function Lookup( const Key : TKey ): TData;
          template <THihi>
            class function DataToHihi( Data : TData ): THihi;
        end;{TGenericList}
    

Also, note that template specialization is a very, very important thing (for me especially). In C++ they've got vector< bool > that behaves different than the standard vector< T >. An example: 
    
    
      template <T, TT>
        TSame = class
          class function Same: Boolean;
        end;{TSame}
      template <T>
        TSame = class
          class function Same: Boolean;
        end;{TSame}
    
    
    
      template <T, TT>
      class function TSame.Same: Boolean;
      begin
        Result := false;
      end;{TSame<T, TT>.Same}
      template <T>
      class function TSame.Same: Boolean;
      begin
        Result := true;
      end;{TSame<T>.Same}
    

This allows to make a compile-time check for same types that are not TObject, e.g. 
    
    
      TFoo = class;
      TBar = class( TFoo );
      var
        S : Boolean;
      begin
        S := TSame< String, Integer >.Same; //s = false
        S := TSame< Int64, Integer >.Same; //s = false
        S := TSame< String, String >.Same; //s = true
        S := TSame< TBar, TFoo >.Same;//s = false (not the same as "var x: TFoo; begin x := TBar.Create; end;" )
      end;
    

Another thing to note: It would be very good if something like this (c++ syntax) would be implemented: 
    
    
      template <bool b>
      class h
      {
        public:
        static const int i = 0;
      };//h
    
    
    
      template <>
      class h< true >
      {
        public:
        static const int i = 1;
      };
    
    
    
      template <>
      class h< false >
      {
        public:
        static const int i = 2;
      };
    

Here 
    
    
      h<false>.i = 2
      h<true>.i = 1
    

ALWAYS. 

## Usage example

If you add more syntax examples, please add equivalent to both. Two main syntaxes seem to be popular, "<>" which looks more or less like C++, c#, etc.. and adding a "generic" keyword which is more along the Pascal style of code. Go to [Generics Vote](<Generics_Vote.md> "Generics Vote") to vote for one or the other syntax. 

### Using brackets: <>
    
    
      type
        TGenericCollection<T: TCollectionItem> = class(TComponent)
        ...implement TCollection and use T
        end;
    
        TCollection = TGenericCollection<TCollectionItem>;
        TFieldDefs = TGenericCollection<TFieldDef>; 
    
        TGenericList<T: PtrInt> = class(TObject)
        ...implement TList and use PtrInt size for code generation
        end;
    
        TList = TGenericList<Pointer>;
    

Implementation of TGenericCollection can be compiled as if (T: TCollectionItem) were used. 

Would this solve the circular dependency ? It seems so to me, because one knows at least as much as in current implementation of these classes, but I'm no compiler engineer. 

For procedures: 
    
    
      function MyFunc<T: TTreeItem>(A: T): T;
      begin
       // do something with the tree item
      end;  
    
      function Max<T>(A, B: T): T;
      begin
        if A < B then
          Result := B
        else
          Result := A;
      end;
    

My restrictions won't allow implementing generic Max and Min, I guess. That really needs macro-alike stuff (for the compiler implementation). 

Another example for linked lists: (ripped from mail to fpc-devel of dannym, but modified a bit) 
    
    
      type
        TListItem<T> = record
          Data: T;
          Next: TListItem;
        end;
        PListItem = ^TListItem;
     
        TList<T> = class
        private
          fHead: PListItem<T>;
          fTail: PListItem<T>;
        published
          procedure Add(Item: T);
        end;
     
      procedure TList<T>.Add(Item: T);
      var
        node: PListItem<T>;
      begin
        New(node);
        node^.Data := Item;
        node^.Next := nil;
      
        if Assigned(fTail) then begin
          fTail^.Next := node;
        end else begin
          fHead := node;
        end;
      
        fTail := node;
      end;
      
      type
        TApple = class
        end;
        TOrange = class
        end;
      
        TAppleList = TList<TApple>;
      
      var
        apples: TAppleList;
        apple: TApple;
      begin
        apples.Add(TApple.Create); // works
        apples.Add(TOrange.Create); // compile error
      
        apple := apples[0]; // works
        apple := apples[1]; // not applicable
        apple := apples[0] as TApple; // works, but unneccessary
        apple := apples[1] as TApple; // not applicable
      end;
    

### Using the "generic" keyword
    
    
      type
        TGenericCollection = generic(T: TCollectionItem) class(TComponent)
        ...implement TCollection and use T
        end;
    
        TCollection = specialize TGenericCollection(TCollectionItem);
        TFieldDefs = specialize TGenericCollection(TFieldDef); 
    
        TGenericList = generic(T: PtrInt) class(TObject)
        ...implement TList and use PtrInt size for code generation
        end;
    
        TList = specialize TGenericList(Pointer);
    

For procedures: 
    
    
      function generic(T: TTreeItem) MyFunc(A: T): T;
      begin
       // do something with the tree item
      end;  
    
      function generic(T) Max(A, B: T): T;
      begin
        if A < B then
          Result := B
        else
          Result := A;
      end;
    

Another example for linked lists: (ripped from mail to fpc-devel of dannym, but modified a bit) 
    
    
      type
        TListItem = generic(T) record
          Data: T;
          Next: TListItem;
        end;
        PListItem = ^TListItem;
     
        TList = generic(T) class
        private
          fHead: specialize PListItem(T);
          fTail: specialize PListItem(T);
        published
          procedure Add(Item: T);
        end;
     
      procedure generic(T) TList.Add(Item: T);
      var
        node: specialize PListItem(T);
      begin
        New(node);
        node^.Data := Item;
        node^.Next := nil;
      
        if Assigned(fTail) then begin
          fTail^.Next := node;
        end else begin
          fHead := node;
        end;
      
        fTail := node;
      end;
      
      type
        TApple = class
        end;
        TOrange = class
        end;
      
        TAppleList = specialize TList(TApple);
      
      var
        apples: TAppleList;
        apple: TApple;
      begin
        apples.Add(TApple.Create); // works
        apples.Add(TOrange.Create); // compile error
      
        apple := apples[0]; // works
        apple := apples[1]; // not applicable
        apple := apples[0] as TApple; // works, but unneccessary
        apple := apples[1] as TApple; // not applicable
      end;
    

Ugly things in need to be resolved: 
    
    
      { a generic type "TPointer" which is a pointer to type instantiated with }
      type TPointer = generic(Type) ^Type;
      { a generic type that instantiates all the enums for Type }
      type TAllEnums = generic(Type)(low(Type),high(Type));
    

So, before a keyword can be real "nice" the following things need addressing: 

  1. Double brackets ()(), looks C-ish again
  2. In math, one usually defines functions with the parameters on the left of the equal sign, not on the right



#### Suggestion 2

Inspired by idea from oliebol in combination with ugly examples, in list of const, var, type sections, add "template" section: 
    
    
      const
        ThisIsAConst = 1;
      
      type
        TMyEnum = (ab, ac);
      
      template(T: TCollectionItem)
        TGenericCollection = class(TComponent)
        ...implement TCollection and use T
        end;
      
      type
        TCollection = specialize TGenericCollection(TCollectionItem);
        TFieldDefs = specialize TGenericCollection(TFieldDef); 
      
      template(T: PtrInt)
        TGenericList = class(TObject)
        ...implement TList and use PtrInt size for code generation
        end;
      
      type
        TList = specialize TGenericList(Pointer);
    

For procedures: 
    
    
      function generic(T: TTreeItem) MyFunc(A: T): T;
      begin
       // do something with the tree item
      end;  
    
      function generic(T) Max(A, B: T): T;
      begin
        if A < B then
          Result := B
        else
          Result := A;
      end;
    

Another example for linked lists: (ripped from mail to fpc-devel of dannym, but modified a bit) 
    
    
      template(T)
        TListItem = record
          Data: T;
          Next: TListItem;
        end;
        PListItem = ^specialize TListItem(T);
     
        TList = class
        type
          PItem = specialize PListItem(T);
        private
          fHead: PItem;
          fTail: PItem;
        published
          procedure Add(Item: T);
        end;
     
      ...implementation...
      
      procedure generic(T) TList.Add(Item: T);
      var
        node: PItem;
      begin
        New(node);
        node^.Data := Item;
        node^.Next := nil;
      
        if Assigned(fTail) then begin
          fTail^.Next := node;
        end else begin
          fHead := node;
        end;
      
        fTail := node;
      end;
      
      type
        TApple = class
        end;
        TOrange = class
        end;
      
        TAppleList = specialize TList(TApple);
      
      var
        apples: TAppleList;
        apple: TApple;
      begin
        apples.Add(TApple.Create); // works
        apples.Add(TOrange.Create); // compile error
      
        apple := apples[0]; // works
        apple := apples[1]; // not applicable
        apple := apples[0] as TApple; // works, but unneccessary
        apple := apples[1] as TApple; // not applicable
      end;
    

#### Suggestion 3
    
    
      { Chu Jetcheng, 2008-07-24 }
    
      program Generics;
    
      type
        { The compiler should maintain a global table storing each specialized type for each generic.  The 
          `of' operator is used to create a specialization for a specified generic type.  Specialized types 
          of the same `type layout' (no other words) point to the same entry in the table of specializations
          of the specialized generic type, and thus are compatible.  Generic objects and records should also 
          be allowed. }
    
        TGeneric = generic class(TAncestorClass) of T1, T2
        private
          FData1: T1;
          FData2: T2;
        public
          property Data1: T1 read FData1 write FData1;
          property Data2: T2 read FData2 write FData2;
          constructor Create(const A: T1; const B: T2);
          procedure WriteData; virtual;
        end;
    
        TGenericDescendent = generic class(TGeneric of Integer, T3) of T3
        private
          FData3: T3;
          FObjectOfSameType: TGenericDescendent of T3;
        public
          property Data3: T3 read FData3 write FData3;
          property ObjectOfSameType: TGenericDescendent of T3 read FObjectOfSameType write FObjectOfSameType;
          constructor Create(const C: T3);
          procedure WriteData; override;
        end;
    
        TGenericIR = TGeneric of Integer, Real;
        TGenericIRClass = class of TGenericIR;
    
        TGenericISClass = class of (TGeneric of Integer, string);
    
        PGenericObj = generic ^TGenericObj; 
        TGenericObj = generic object of T 
          MyField: T;
          procedure WriteData;
        end;
    
    
      { TGeneric }
    
      { Use the same syntax for method implementations as with immediate classes. }
    
      constructor TGeneric.Create(const A: T1; const B: T2);
      begin
        FData1 := A;
        FData2 := B;
      end;
    
      procedure TGeneric.WriteData;
      begin
        Writeln(FData1);
        Writeln(FData2);
      end;
    
      { TGenericDescendent }
    
      constructor TGenericDescendent.Create(const C: T3);
      begin
        inherited Create(16, C);
        FData3 := C;
      end;
    
      procedure TGenericDescendent.WriteData;
      begin
        inherited;
        Writeln(FData3);
        if Assigned(FObjectOfSameType) then FObjectOfSameType.WriteData;
      end;
    
      { TGenericObj }
    
      procedure TGenericObj.WriteData;
      begin
        Writeln(MyField);
      end;
    
    
      var 
        Obj1: TGeneric of Integer, Real;
        Obj2, Obj3: TGenericDescendent of string;
        OldStyleObj: TGenericObj of Integer;
        OldStyleObjPtr: PGenericObj of Integer;
    
      begin
        Obj1 := TGeneric.Create(32, 3.14);
        Obj1.WriteData;
        Writeln(#9, Obj1 is TGenericIR);
        Writeln(#9, Obj1 is TGenericIS);
    
        Obj2 := TGenericDescendent.Create('Generic test.');
        Obj2.WriteData;
        Writeln(#9, Obj2 is TGenericIR);
        Writeln(#9, Obj2 is TGenericIS);
    
        Obj3 := TGenericDescendent.Create('Another generic test.');
        Obj3.ObjectOfSameType := Obj2;
        Obj3.WriteData;
    
        Obj3.Free;
        Obj2.Free;
        Obj1.Free;
    
        OldStyleObj.MyField := 100;
        OldStyleObj.WriteData;
    
        OldStyleObjPtr := New(PGenericObj of Integer);
        with OldStyleObjPtr^ do begin
          MyField := 202;
          WriteData;
        end;
        Dispose(ObjStyleObjPtr);
      end.
    

## Terms

type user: 

  * the function in the compiler that defines a variable or return type and thus 'uses a (already defined) type'.



immediate type: 

  * not generic and not specialized, i.e. 'normal type'



generic type: 

  * template for a class with some unspecified types, never to be filled in into this class type. Only by specialisation.



specialization: 

  * copy generic type class to new specialized type class



type parameters: 

  * T and Q, the unknown types for the generic TSomeGeneric<T,Q>



specialized type: 

  * a generic type with all type parameters know, written into a real class type (tree) and reused if possible



## Changes

### Changes in class/record definition representation

Each class definition representation has added fields for: 
    
    
     - class instantiation mode
        0  immediate type
        1  generic type, no instantiation, just generate specialized type
        2  specialized type
    
    
    
     - when mode 2:
         generic type uplink (XXXX, what, unitname, typename?)
       when mode 1:
         list of specialized types known so far (and where),
         XXXX list items
       when mode 0: 
         nothing
    

### Changes in 'type user' compilation (for class/record types only)

Each class type user will have to check mode of the class type. 

For the used class type, If mode is immediate 
    
    
     - proceed as always till now.
    

If mode is specialized 
    
    
     - proceed as always till now 
     - keep in mind the generic type for some checks later (and debugging).
    

If mode is generic, this: 
    
    
     - check 'list of specialized types' for the type parameter to use.
       If there is already a specialized type, use that.
       (given that it is compatible with 'compile parameters' XXXX)
    
    
    
     - If not available, clone the generic type in the tree, and fill in the
       type params with the actual types to use.
       Generate machine code as normal.
       Remember the new specialized type for later in the list by the 
       generic type and type params used.
    

(the generic type is best written to some ppu as parse tree so that it can be refetched for cloning somewhen. To keep it easy, initially it should be limited to having its .pas file around when wanting to use a generic) 

### type params

Generics store a list of the type params (names). 
    
    
     ('T', 'Q')
    

And Specializations store a list of type mappings from the real types to the type params, in the way of 
    
    
      type
        T = Integer;
        Q = Boolean;
    

of course these are local to this class/record specialization. 

## Other things to note (for *later*)

### What if a method implementation depends on the type used to know how the implementation should look like ?
    
    
      procedure GList<T is TObject>.Add(const value: T);
      procedure GList<T is Integer>.Add(const value: T);
    

? 

or more cryptic like 
    
    
      procedure GList<TObject>.Add(const value: T);
      procedure GList<Integer>.Add(const value: T);
    

? 

or like 
    
    
      procedure GList<T>.Add(const value: Integer);
      procedure GList<T>.Add(const value: TObject);
    

the latter would need extra compiler support but would be nicer, overall. Potentially clashing with normal overloads though. 

  
or as an alternative like: 
    
    
      {$IF T is TObject}
        procedure GList.Add(const value:TObject);
      {$ELSE}
        procedure GList.Add(const value:Integer);
      {$ENDIF}
    

This would also allow to define constants or records in dependency of the generic type. This could be useful for example if you can specialize a class with static or dynamic array types (like array of double) and you need a static or dynamic array of integer having the same length as the generic. Currently we can play a bit with sizeOf(T), but this has its limitations (see [http://www.lazarusforum.de/viewtopic.php?f=10&t=11915#p106921](<http://www.lazarusforum.de/viewtopic.php?f=10&t=11915#p106921>))... 

### How to do internal storage classes
    
    
      type
      PNode<T> = ^TNode<T>;
      TNode<T> = record
        prev,next: PNode<T>;
        data: T;
      end;
    
      type
      GList<T> = class
      private
        head, tail: PNode<T>;
        cnt: Integer;
      public
        procedure Add(const stuff: T);
      end;
    

? 

This implies that generics are best implemented both for records and for classes alike! Also implies that pointer types need to support generic too. 

Also one specialization should chain other specializations (probably also when deriving from a generic). 

### How to derive from the generic type

It would be good if generic types could be derived from. 
    
    
      type
      TGenericEventyList<T> = class(TGenericList<T>)
      public
        procedure Add(value: T); override;
      end;
      ...
    

### Interfaces should support generics too
    
    
      type
      IBa<T> = interface
        procedure Add(value: T);
      end;
      TBa<T> = class(...,IBla<T>,IInterface)
      end;
    

(what to do about the guids of the specializations? probably just generate whatever, they dont have any real meaning after all) 

Of course this has limitations to check, like 

  * the generic interface cannot be used, just its specializations
  * the specialized interface cannot be used in a generic class interface list
  * (the generic interface can be used in a generic class interface list)
  * the generic interface itself is invalid to use anywhere (just as generic classes are) except for specializing and deriving from
  * normal interfaces cannot derive from generic interfaces
  * (generic interfaces can derive from generic interfaces) - given the type params are the same
  * specialized interfaces cannot be derived from



### Limitation of possible specialisations
    
    
      type
      TGenericList<T: TObject or Integer> = ...
      end;
    

Probably useful... 

### What to do with class functions

I dont know a whole lot of how class functions are stored internally. What happens to class functions in generics? 

### 'class of'

<Almindor> don't forget class of, we need class of compatibility for dynamic class factories 

<Almindor> There are possible problems with 'class of' 

## Implementation details

### Sanity checks

  * Generic types cannot be instantiated from
  * (Generic types can be derived from)
  * Specialized types cannot be derived from
  * Normal types cannot use generic types in its definition
  * (Normal types can use specialized types in its definition)
  * There are no half specialized types (one type param specified, the other not)
  * prevent loops while 'unpacking' specializations (loop counter field in every specialization)



### Things to note and not forgot

  * A class and record generic can use itself as type within its definition.



example: 
    
    
      type
      TBla<T> = class
        parent: TBla<T>;
      end;
    

  * A class and record generic can use other generics as types within its definition.


    
    
      type
      TBla<T> = class
        parent: TBlo<T>;
      end;
    

## Implementation steps

### Extend parser to support '<T>' and '<T: bla or bla or bla>' and '<T,Q>' notation

Easy to do. Done. 

## Different ways to do it, one excluding the other

### Way 1

#### Change class/record/object/pointertype/interface parser to mark if it is a generic, a specialization or normal

Mediate to do. Have done it for class/object and pointertype. Needs all kinds of clone functions for the defs and syms. fpk says he has patches. waiting. 

#### insert dummy symbols (typesym + typedef) for the type params in the generic

Cannot be inserted into the object because parsing of types within objects has been disabled to make that possible: 
    
    
      type
      Row = Cardinal;
      TBla = class 
        Row: Integer;
      end;
    

weird.. (function searchsym_type, is not possible to have type definitions in records, objects, parameters) 

#### parse implementation section methods *for the generic*

Is harder than it sounds, because the class/object the method is member of has to be searched for the type symbol for the type params (and these *not* derefed but just used as is). 

#### when parsing generics, use a dummy type in place of the type params

For that, defer the type checking of the compiler in that case to 'as late as possible' or 'never for the generic' <\--- TODO... hard... 

#### Change derivation handlers to do the sanity checks

Should pose no problems other than finding all the places. 

#### Add error messages for it

#### Change pexpr (tnode creating)

<Synopsis+> see the do_resulttypepass() calls in pexpr.pas But the compiler needs to known for example if it allows a . after the type 

  * add a new tnode type for postfix operators (instead of resolving the operators into the fields/methods they reference) and defer type checking until later (just before binary generation - which is not done for the generic type).



Examples for postfix operators are: 

  * '[]' array member access (normal array, class default array property?)


  * '.' record/class member method/variable access


  * '^' pointer dereference


  * (automatic pointer dereference)



<Synopsis> grep for m_auto and you'll find the places where it is used <Synopsis> only in pexpr.pas 

#### Change ppu streaming routines to save the tree of generics to the ppu

#### Change type user to instantiate specialized types for generics when used

Somewhat hard to do. (main work here :)) 

For the instantiation 

  * there is a dummy type sym needed (ok),
  * a new objectdef
  * clone of all the syms and defs (at least proc and property) of the original class/object
  * and replacing all the type params by the correct types.
  * Store that into the module as def (not on disk).
  * (<Synopsis+> an additional symtable shall be added to tmodule: tempsymtable; all temporary tdef's shall be stored in that symtable)
  * Then resume compiling for the class/object.



### Way 2

just keep the source code of the generic around and do blind replacing of the type params and then compile a specialization from that as 'source' 

#### Notes

<Synopsis+> but the code to generate the specialization at runtime can be generated at the time the specialization is created. It doesn't need to be stored in ppu. all information is available to generate the tree at that time. 

<Synopsis+> In the 2.1.x development series a rewrite of the ppu loading is planned to make it more clean and more maintainable. Currently it is risky to fix bugs without introducing new bugs 

<plugwash-> It needs to be stored in some form so that the specialisations can be generated from it. <Synopsis+> but you can't store them in the ppu. Or with the same limitations as inlining. No references to the implementation symtable. 

fpk has patches to make cloning defs possible. 

01:28:24 <Synopsis+> users will complain; things like: 
    
    
      interface
        generictype declar
      implementation
        procedure helper;
        begin
        end
        constructor generictype.create
        begin
          helper
        end
      end.
    

Will not be allowed. 

Another problem: 
    
    
      unit a;
      interface
        generictypeA
      implementation
      uses B
      end.
    
      unit b;
      interface
      uses a
      implemenation
      begin
        generictypeA<integer>.craete
      end.
    

At the time that the implementation of B is parsed it needs already the code of generictypeA. but that is not yet parsed! The compiler doesn't know anything yet. It only knows that generictypeA exists. 

<fpk-> dannym, Synopsis: we can simply forbid such code in generics which cause the problem 

<fpk-> i.e. everything used by generics must be already included in the interface uses clause ... or forbid references to units used only in the implementation section :) 

<Synopsis+> fpk had a good idea that can solve some of the issues: unit bla; interface ... generics ... implementation ... initialisation ... finalizatrion end. 

<Synopsis+> The generics delcarations shall be put in a separate section before implementation is parsed 

<dannym> at 'procedure TTest.Add(x: T);', T needs to be known. but it doesnt seem that the object symtable is searched for the class method parameters. correct ? 

<Synopsis+> dannym: correct, object symtables can not contain types, because that created problems. there is special code for that: remove the objectsymtable before parsing type declarations. 
    
    
      { for records, don't search the recordsymtable for the symbols of the types }
      oldsymtablestack:=symtablestack;
      symtablestack:=symtablestack.next;
    

Because you can have a field with the same name as a type 
    
    
      type
      Wrd = Word;
      R = Record
        Wrd : Wrd;
      end
    

this is allowed. To support this the record or object symtable is remvoed from the symtablestack when the type is parsed. 

## Already existing stuff

### metaclasses
    
    
      type
      TObjectClass = class of TObject;
      TTypedList = class
      private
        Fwanttype: TObjectClass;
        ...
      public
        constructor Create(itemtype: TObjectClass);
        procedure Add(item: TObject);
        ...
      end;
      constructor TTypedList.Create(itemtype: TObjectClass);
      begin
        Fwanttype := itemtype;
        ...
      end;
      procedure TTypedList.Add(item: TObject);
      begin
        assert(item.Class = Fwanttype);
        ...
      end;
    
      ...
      TSomeStuff = class
      end;
      ...
      Flist := TTypedList.Create(TSomeStuff);
    

Advantages 

  * already works



Disadvantages 

  * slow at runtime
  * cannot do simple types
  * need to cast all the time even for the supported types (i.e. when reading). eeek.



## To be incorporated here

### Chat logs

<Synopsis> [http://www.eleforum.com/cgi-bin/eleweb_lift?action=3&script=wall&num=18&reversesort=true&type=today&starttime=00:00&endtime=23:59&startdate=01-02-05&enddate=01-02-05](<http://www.eleforum.com/cgi-bin/eleweb_lift?action=3&script=wall&num=18&reversesort=true&type=today&starttime=00:00&endtime=23:59&startdate=01-02-05&enddate=01-02-05>) <Synopsis> or <Synopsis> <http://neli.hopto.org:3980/~micha/irc-fpc/2005/01/%23fpc.20050102.log>

## See also

  * [Generics](<Generics.md> "Generics")

---

_Source: [https://wiki.freepascal.org/Generics_proposals](https://web.archive.org/web/20240524195944/https://wiki.freepascal.org/Generics_proposals)_
