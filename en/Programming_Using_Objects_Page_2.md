# Programming Using Objects Page 2

Previous article: [Programming Using Objects](<Programming_Using_Objects.md> "Programming Using Objects")

## Contents

  * 1 Objects - Self & Inherited
    * 1.1 Self Keyword
    * 1.2 Inherited Keyword
  * 2 Objects - Abstract Methods
  * 3 Objects - Visibility Features
    * 3.1 Static Fields
    * 3.2 Public, Private, Protected
  * 4 Objects - Run Time Library



## Objects - Self & Inherited

### Self Keyword

Objects have an implicit parameter, _self_ , which can be used in method calls which can be used to qualify fields and methods of the object. The explicit use of the keyword is optional but provides clarity if desired. The following implicit and explicit use of the _self_ qualifier for field and method access within a method are identical. To illustrate this, let's say the the following (non useful) code was inserted into the _Rectangle.Draw_ method: 
    
    
    procedure Rectangle.Draw;
    begin
    
      Self.x := 1;
      x := 1;
    
      Self.Getparams;
      GetParams;
    end;
    

### Inherited Keyword

A bit more useful is the **inherited** keyword for use within an object's methods. By using this keyword, one can explicitly call the parent method of the same or different method name. This is useful because it allows a method overriding it's parent's methods the capability of using the parent's code before or after child's code is called. The use of the **inherited** keyword can propagate up and inheritance chain. For our simple example program, the readln input code in the GetParams method is a common task for all objects. Previously, this code had to be duplicated into the _TSquare.GetParams_ method since it added to the simple input task but in doing so overrode the entire behavior of the ancestor methods. By using the **inherited** keyword, the _GetParams_ method for _TSquare_ can reuse the basic readln input without having to duplicate it. Consider the previous program implementing these GetParams methods. 
    
    
    procedure TShape.GetParams;
    begin
      readln(x, y, width, height);
      writeln;
    end;
    
    procedure TSquare.GetParams;
    begin
      write('TSquare^.GetParams : ');
      inherited;
      height := width;
      writeln('made sure all sides are equal');
      writeln;
      write('Inherited Draw : ');
      inherited Draw;
    end;
    
    Var
      Square : PSquare;
    
    begin
      Square^.GetParams;
    end.
    
    
    
    TSquare.GetParams : 1 2 3 4
    
    made sure all sides are equal
    
    Inherited Draw : TRectangle.Draw

Note that _TRectangle_ does not declare a _Draw_ method and implicitly inherits the draw method from _TShape_. The _TSquare.GetParams_ method overrides the _TShape. GetParams_ method but after calling the writeln statement, it invokes the _TShape.GetParams_ method using the **inherited** keyword. Then, after the inherited method returns, _TSquare.GetParams_ does some additional processing afterwards. The **inherited** keyword can be used in another manner which is illustrated by the TSquare.Getparameter method calling the ancestor Draw method. Explicitly calling inherited _Draw_ method method in this case may not make much logical sense but is shown to provide an example of the syntax. Even in situations it does make logical sense, use of the inherited keyword in this manner can result in hard to follow program flow. 

The **inherited** keyword is often useful in constructors for pre processing task like creating and initializing auxiliary objects and additional fields associated with the object. Similarly, the keyword is also useful in destructors for freeing up auxiliary objects and data structures the object previously created. It should be noted that allthough code reuse and encapsulation is improved, program flow can be more difficult to follow than for straight procedural code. 

## Objects - Abstract Methods

**Abstract** methods are **virtual** methods which must be overridden in a child object. An **abstract** method is declared by adding the **abstract** keyword after the **virtual** keyword. There are no methods implemented for Objects with **abstract** methods declared which means no objects with abstract methods can be declared. Child objects must be created to provide overridden methods. The **inherited** keyword can not be used for child objects to call a parent **abstract** (non existent) method and depending on the situation, either the compiler will catch this and if it doesn't, a run time error will occur. An example declaration follows. 
    
    
    Type
      TShape2 = Object
        f : longint;
        procedure GetParams; virtual;
        procedure Draw; virtual; abstract;
      end;
    

Although the example object drawing program might want to declare a shape type with abstract methods as shown above, a variable of _TShape2_ could not be declared which may or may not be a desired program feature. 

## Objects - Visibility Features

### Static Fields

Normally, every instantiated object has its own privately maintained fields (i.e. separate individual memory locations) Child objects inherit the name of the field but the memory location is separate from all the other object fields of the same name qualified by the dot notation or scope. If the **static** keyword is added after the field name in the type definition, it signifies that this particular field is global for all instantiated object variables throughout the program which also propagates down the inheritance chain. The field then acts like a regular global variable with limited accessibility only via the object dot notation or in the local scope of object's methods. In addition, the type name itself can be used to access the global field. In order to use accidental use of the static field feature, the {$static on} compiler directive must be included in the source file. The following program illustrates the behavior of both regular and static fields. 
    
    
    program StaticFieldsForever;
    
    {$static on} 
    
    type 
      Ts1 = object 
        f1 : longint;
        f2 : longint; static; 
      end; 
    
      Ts2 = object (TS1)
      end; 
    
    var 
      A1, B1 : Ts1; 
      A2, B2 : Ts2; 
    
    begin 
      writeln;
    
      A1.f1:=2;
      B1.f1:=3;
      writeln('f1');
      writeln(' A1 :', A1.f1, '  B1 :', B1.f1); 
      writeln(' A2 :', A2.f1, '  B2 :', B2.f1); 
      writeln('-------');
    
      A2.f1:=4; 
      writeln('f1');
      writeln(' A1 :', A1.f1, '  B1 :', B1.f1); 
      writeln(' A2 :', A2.f1, '  B2 :', B2.f1); 
      writeln('-------');
    
      B1.f2:=5; 
      writeln('f1');
      writeln(' A1 :', A1.f1, '  B1 :', B1.f1); 
      writeln(' A2 :', A2.f1, '  B2 :', B2.f1); 
      writeln('f2');
      writeln(' A1 :', A1.f2, '  B1 :', B1.f2, ' T1 :', Ts1.f2); 
      writeln(' A2 :', A2.f2, '  B2 :', B2.f2, ' T2 :', Ts2.f2); 
      writeln('-------');
    
      Ts2.f2:=6; 
      writeln('f2');
      writeln(' A1 :', A1.f2, '  B1 :', B2.f2, ' T1 :', Ts1.f2); 
      writeln(' A2 :', A2.f2, '  B2 :', B2.f2, ' T2 :', Ts2.f2); 
      writeln;
    end.
    

Resulting output 
    
    
    f1
     A1 :2  B1 :3
     A2 :0  B2 :0
    -------
    f1
     A1 :2  B1 :3
     A2 :4  B2 :0
    -------
    f1
     A1 :2  B1 :3
     A2 :4  B2 :0
    f2
     A1 :5  B1 :5 T1 :5
     A2 :5  B2 :5 T2 :5
    -------
    f2
     A1 :6  B1 :6 T1 :6
     A2 :6  B2 :6 T2 :6

### Public, Private, Protected

These specifiers are used for blocks of fields and methods declared in object types. The definitions of these specifiers are detailed in the language reference guide. Basically, **public** (or non specified) fields or methods are visible anywhere in the global scope of the program. **Protected** fields and methods are visible everywhere within the scope of a unit but limited only to objects and descendant objects outside of the unit they are declared in. Private fields and methods are only visible to objects and descended objects of the declared type. An example declaration follows: 
    
    
    type 
      TMyObject =object 
        f0: longint;
        procedure m0;
      public
        f1 : longint;
        procedure m1;
      protected
        f2 : longint; 
        procedure m2;
      private
        f3 : longint; 
        procedure m3;
      public
        f4 : longint;
        procedure m4;
      end;
    

The fields f, f1 and f4 and methods m0, m1 and m4 all have public visibility; f1 and m1 by default (no specifier.) Also, the order or repetition of visibility blocks is fairy flexible as can be seen from the above example. 

  


## Objects - Run Time Library

FPC provides several predefined Object types for this dialect. By convention, all the supplied object types and sub types start with the letter "T". These object types are included in the **OBJECTS** unit. A similar looking set of types is suplied for the Classes object dialect which are declared in the **SYSTEM** unit.. This can get confusing especially as both run time units declare a base object type (or class): _TObject_. 

For this Object dialect, _TObject_ is the implicit root object which all objects descend from. When no sub object is specified in an object type declaration, the type _TObject_ is implicitly declared for you. 

These predefined object types are documented in the FPC Run-Time Library Reference Guide, chapter 21. 

<ftp://ftp.freepascal.org/pub/fpc/docs-pdf/rtl.pdf>

This unit documents basic types and support routines as well as the following object types:

---

_Source: [https://wiki.freepascal.org/Programming_Using_Objects_Page_2](https://web.archive.org/web/20241212120500/https://wiki.freepascal.org/Programming_Using_Objects_Page_2)_
