# Programming Using Objects

## Contents

  * 1 Objects - Basics
  * 2 Objects - Static Inheritance
  * 3 Objects - Virtual Inheritance
    * 3.1 Virtual Keyword
  * 4 Objects - Constructors and Destructors
  * 5 Objects - Dynamic Variables
  * 6 Objects - Continued
  * 7 External links



## Objects - Basics

FPC provides two OOP implementations : [objects](<Object.md> "Object") and [classes](<Class.md> "Class"). The main documentation for Objects is found in Chapter 5 of the FPC language reference, and for Classes in Chapter 6. This tutorial describes the less often used "Objects" implementation. 

An **object** type looks similar to a **record** type with additional **method** fields and optional keywords which indicate the scope of the fields. A object declared without methods is difficult to distinguish from a record. 
    
    
    Type
       MyObject = Object
          f_Integer : integer;
          f_String : ansiString;
          f_Array : array [1.3] of char;
       end;
    

In the above example, the Pascal keyword **record** has been replaced with the keyword **object**. Objects are more useful when **method** fields are added to the object. Object methods are declared in FPC using the keywords **procedure** or **function** and are declared the same way as normal Pascal procedures and functions only that they are declared within the scope of the object declaration itself. A more useful object declaration (as part of an example graphics application) is shown below. 
    
    
    Type
       DrawingObject = Object
          x, y : single;
          height, width : single;
          procedure Draw;
       end;
    
    Var
      Rectangle : DrawingObject;
    

In addition to the single precision floating point data fields, the object shown above declares a method; a procedure called _Draw._ Following the "DrawingObject" type declaration is a variable declaration: _Rectangle_ of the type _DrawingObject_. Next, the _Draw_ procedural code itself needs to be written as well as well as code for accessing and manipulating the data fields. The Draw procedure is written separately outside of the DrawingObject declaration. Where this Procedure declaration is written is not specified and depends on your particular coding standards. 

The following simple program shows how all this works. It should compile and run on any system with FPC 2.2.2 and above. Note: For macOS, the -macpas compiler directive must be turned off. 
    
    
    Program TestObjects;
    
    Type
       DrawingObject = Object
          x, y : single;
          height, width : single;
          procedure Draw;  //  procedure declared in here
       end;
    
      procedure DrawingObject.Draw;
      begin
           writeln('Drawing an Object');
           writeln(' x = ', x, ' y = ', y);  // object fields
           writeln(' width = ', width);
           writeln(' height = ', height);
           writeln;
    //    moveto (x, y);  // probably would need to include a platform dependent drawing unit to do actual drawing
    //    ... more code to actually draw a shape on the screen using the other parameters
      end;
    
    Var
      Rectangle : DrawingObject;
    
    begin
    
      Rectangle.x:= 50;  //  the fields specific to the variable "Rectangle"
      Rectangle.y:= 100;
      Rectangle.width:= 60;
      Rectangle.height:= 40;
    
      writeln('x = ', Rectangle.x);
    
      Rectangle.Draw;  //  Calling the method (procedure)
    
      with Rectangle do   //  With works the same way even with the method (procedure) field
       begin
           x:= 75;
           Draw;
           readln; 
       end;
    
    end.
    

As can be seen in the above program, the body of the _Draw_ method (procedure) is declared after the object type declaration by concatenating the type identifier with the procedure name. In a more realistic situation, the object would likely be declared in the **interface** section of a separate unit while the procedure body would be written in the **implementation** section of the same external unit. In this example, only standard vanilla Pascal is used, but specific graphic primitives would likely be used to actually draw the object. The second thing to notice is that inside the _Draw_ procedure method, the object's data fields are referenced as if they were regular local variables. The only difference to regular local variables is that the values of these fields will persist between calls to the _Draw_ procedure. 

In the main program, the fields are assigned values and accessed just like record fields are assigned. Similarly, the _Draw_ procedure is invoked using the same dot notation as the fields. Also like records, the **with** keyword works the same for accessing object fields and invoking methods. Notice that the second time the _Draw_ method is called, all the fields persisted between calls; the only field different is the x attribute which was explicitly changed. 

The above example is meant to show the basic mechanics of a simple **Object**. However, there are a couple of issues. The first issue is that the _Draw_ method only draws one thing: a rectangle (if drawing primitives are available.) Additional methods could be declared in the **DrawingObject** object such as DrawRectangle, DrawCircle, DrawTriangle, etc..., but this would not be much different than declaring separate standard procedures and having to use **case** statements to select the desired procedure. So implementing objects in this manner would require more work than using standard Pascal The concern is that this object's fields can be accessed and modified globally throughout the program circumventing the desire to produce well encapsulated and robust programs. These issues will be addressed in the next section. 

## Objects - Static Inheritance

Next, the code will be expanded to include squares in addition to rectangles. Other shapes could be included in a similar manner. An attempt will be made to leverage the existing code, if possible, by creating two new objects to handle the two specific shapes. In order to make the code and output easier to follow, the main object type, _DrawingObject_ has been renamed _TShape_. Other object types will be named _TRectangle_ , and _TSquare_ with corresponding variables named Shape, Rectangle, Square. The letter "T" is used as a prefix to the object type names since it is a commonly used convention in many object libraries and frameworks including FPC's. 

What follows are type declarations for: the main _TShape_ object type (previously _DrawingObject_) along with the new types _TRectangle_ and _TSquare_. _TShape_ now includes a new method (**procedure** GetParams) to obtain values for its fields. Next, the types _TRectangle_ and _TSquare_ are declared. _TShape_ is usually referred to as what is called an ancestor or parent object, while TRectangle is usually called a child or subobject or subclass of _TShape_ or referred as descending from _TShape_. This parent / child / grandchild hierarchy is identified by the qualifier type name following the Object keyword. Similarly, TSquare is a child of TRectangle. Also, _TShape_ is not really a complete object type in itself but rather a template for other objects to inherit a common structure and behavior(s) from. Such templates are often useful for code clarity and there are language features (explained later) which can be used to enforce certain characteristics of these template objects. For an actual application, the _TShape_ type would more than likely be declared differently. Here "TShape" is used to illustrate some basic concepts. 
    
    
    Type
      TShape = Object
        x, y : single;
        height, width : single;
        procedure GetParams;
        procedure Draw;
      end;
    
      TRectangle = Object(TShape)
        procedure Draw;
      end;
    
      TSquare = Object(TRectangle)
        procedure GetParams;
        procedure Draw;
      end;
    
    Var
      Shape : TShape;
      Rectangle : TRectangle;
      Square : TSquare;
    

Notice that _TRectangle_ lists only the _Draw_ procedure while _TSquare_ lists both the _GetParams_ and _Draw_ procedures. Neither of the subobject types include any fields. The "missing" fields and missing procedure names are said to be inherited from the fields and procedures declared in ancestor objects. In this case, any object variables declared and instantiated of type _TRectangle_ will inherit from the _TShape_ type all four fields, (x, y, height, and width) and the _GetParams_ method . At runtime, a variable of the type _TRectangle_ will look and behave like a _TShape_ variable except that the _Draw_ procedure will use different code than the procedure by the same name in an instantiated variable of the _TShape_ type. Similarly, the _TSquare_ object type will inherit all the "grandparent" fields from _TShape_ and have its own different procedure blocks. If desired, _TRectangle_ and _TSquare_ could have declared additional fields in their type definitions which would show up as additional fields (memory locations) available at runtime which instantiated parent object variables would not have or be able to access. 

Next are the specific procedure implementations for these object types. 
    
    
    procedure TShape.GetParams;
    begin
      write('TShape.GetParams : ');
      readln(x, y, width, height);
      writeln;
    end;
    
    procedure TShape.Draw;
    begin
      writeln('TShape.Draw');
      writeln('Position: x = ', x:4:0, ' y = ', y:4:0);
      writeln('    Size: w = ', width:4:0, ' h = ', height:4:0);
      writeln;
    end;
    
    procedure TRectangle.Draw;
    begin
      writeln('TRectangle.Draw');
    end;
    
    procedure TSquare.GetParams;
    begin
      write('TSquare.GetParams : ');
      readln(x, y, width, height);
      height := width;
      writeln('making sure all sides are equal for Square');
      writeln;
    end;
    
    procedure TSquare.Draw;
    begin
      writeln('TSquare.Draw');
    end;
    

To provide clarity, no actual (platform specific) drawing routines are used. Rather, generic Pascal I/O routines are included to illustrate runtime behavior. This particular hierarchy of objects, fields and methods would likely not be the best implementation for an actual shape drawing application which will become more apparent after new concepts are introduced. 

The _GetParams_ method is used to obtain and to perform any needed processing of the field values. Although the object fields could be accessed directly as was done in the previous section, it is considered better programming style to encapsulate the "getting" a "setting" of object fields using object methods. In fact, FPC provides additional language features to help support and enforce narrowing the accessibility and visibility of internal object data which will be covered later. 

Since the different types of objects in this program all use the same fields, using a common _GetParams_ method would seem to make sense to include at the top level ancestor object which all other descendent objects can inherit/use. Thus, this method is implemented in the _TShape_ object type. 

The _TShape_ object type includes a second method, _Draw_ , which is probably not needed in an actual shape drawing program since it is not a defined shape. However, a variable declared of type _TShape_ likely will be assigned to a variable of a descendent object type such as _TRectangle_ , which does need to have a draw method which will draw using the code of the descendent object type. Normally, this "template" method would just be declared as a stub or not implemented at all. The syntax for this type of situation and others will be described later. In this case, we want to provide some feedback to inspect field values and illustrate program flow so for demonstration purposes, it includes some writeln statements. 

The _TRectangle_ object type does not include a _GetParams_ method but does have its own _Draw_ method. The _TSquare_ object type has its own _GetParams_ method which repeats much of the the same code from the _TShape.GetParams_ method but also adds code to ensure that the height and width fields are the same. _TSquare_ defines its own _Draw_ method. In an actual program, the _Draw_ method for squares and rectangles would probably be the same and the _Draw_ method for _TSquare_ could be omitted and just inherited from _TRectangle_. 

Here is a sample program which demonstrates the behavior of the various objects. 
    
    
    Var
      Shape : TShape;
      Rectangle : TRectangle;
      Square : TSquare;
    begin 
      writeln;
      writeln ('Getting parameters for Shape');
      Shape.GetParams;
      
      write ('Calling Shape.Draw : ');
      Shape.Draw;
    
      writeln;
      writeln ('Getting parameters for Rectangle');
      Rectangle.GetParams;
    
      write ('Calling Rectangle.Draw : ');
      Rectangle.Draw;
    	
      writeln;
      writeln ('Getting parameters for Square');
      Square.GetParams;
    
      write ('Calling Square.Draw : ');
      Square.Draw;
      readln ;
    end.
    

  
The above code produces the following output. 
    
    
    Getting parameters for Shape
    TShape.GetParams : 1 2 3 4
    Calling Shape.Draw : TShape.Draw
    Position: x =    1 y =    2
        Size: w =    3 h =    4
    
    
    Getting parameters for Rectangle
    TShape.GetParams : 11 22 33 44
    Calling Rectangle.Draw : TRectangle.Draw
    
    Getting parameters for Square
    TSquare.GetParams : 111 222 333 444
    making sure all sides are equal for Square
    
    Calling Square.Draw : TSquare.Draw

Tthe _Shape_ object is initialized by calling the _GetParams_ method and then the _Draw_ method is called which prints out the field values. Next, the same is done for the _Rectangle_ object. Notice that since there is no _GetParams_ method for Rectangles, the compiler used the parent object's method, _TShape.GetParams_ to carry out this action. Finally, the _Square_ object is initialized by calling the _GetParams_ method which is explicitly defined for _Squares_ and the output shows this method was called and did extra processing. The _Draw_ method is called and the _Draw_ method specifically defined for _Squares_ was executed. Note that if the _Draw_ procedure was left out for the _TSquare_ type (as would be reasonable for an actual program) the last output line would look like this. 
    
    
    Calling Square.Draw : TRectangle.Draw

Now what happens if we assign a sub object variable to a parent variable as follows? 
    
    
    writeln;
    writeln ('Assigning Rectangle to Shape');
    Shape := Rectangle;
    writeln;
    write ('Calling Shape.Draw : ');
    Shape.Draw;
    	
    writeln;
    writeln ('Assigning Square to Shape');
    Shape := Square;
    writeln;
    write ('Calling Shape.Draw : ');
    Shape.Draw;
    
    writeln;
    writeln ('Assigning Square to Rectangle');
    Rectangle := Square;
    writeln;
    write ('Calling Rectangle.Draw : ');
    Rectangle.Draw;
    

The following is output. 
    
    
    Assigning Rectangle to Shape
    
    Calling Shape.Draw : TShape.Draw
    Position: x =   11 y =   22
        Size: w =   33 h =   44
    
    Assigning Square to Shape
    
    Calling Shape.Draw : TShape.Draw
    Position: x =  111 y =  222
        Size: w =  333 h =  333
    
    Assigning Square to Rectangle
    
    Calling Rectangle.Draw : TRectangle.Draw

The first block of code assigns the _Rectangle_ variable to the _Shape_ variable. When the _Shape.Draw_ method is invoked, it executes the _TShape.Draw_ code and not the _TRectangle.Draw_ code. But as can be seen, the _Rectangle_ fields is what are printed out. This behavior is called **Static** method inheritance in FPC. If it is desired to instead invoke the child _TRectangle.Draw_ method in this situation, FPC provides way to do this called **virtual** methods which will be covered in the next section. Continuing on, _Shape_ is assigned to _Square_ and the _TShape.Draw_ method is invoked which prints out the field values of the _Square_ object. Finally, the Rectangle variable is assigned the Square and the _TRectangle.Draw_ method is invoked similarly to the behavior of the _TShape.Draw_ methods. 

Assigning an ancestor object to a descendent object is not allowed. The compiler will flag the following line as an error. 
    
    
    Rectangle := Shape;   // can not assign a parent to a child, does not compile
    

## Objects - Virtual Inheritance

For a drawing application, a common task would be to refresh the display and step through an array or linked list of shapes and call the draw method. Instead of needing a case statement inside the loop which selects one of many possible specific drawing procedures 
    
    
    for k := 1 to NumShapes do
     with ShapeRec[k] do
      case ShapeRec[k].ShapeKind of
        cRectangle: DrawRectangle (x, y, width, height);
        cSquare:    DrawSquare (x, y, width, height);
        cTriangle:  DrawTriangle (x, y, angle1, angle2, base);
     end;  // case
    

the code would look something like this 
    
    
    for k:= 1 to Numshapes do
     Shape[k].draw;
    

Where each Shape object could be one of any sub objects descended from TShape. Code maintenance is made easier since there is one fewer locations in code which needs to be modified. All changes to the behavior of a particular sub object of shape is kept together in one place. 

### Virtual Keyword

However, as seen in the last section, calling the _Draw_ method for the Shape variable this way will not invoke the appropriate draw method of the sub object which is the behavior that is desired in this (and most) cases. To obtain the desired behavior, the **virtual** keyword must be inserted after the method declaration in the type definition as follows: 
    
    
    Type
      TShape = Object
        x, y : single;
        height, width : single;
        procedure GetParams;
        procedure Draw; virtual;
      end;
     
      TRectangle = Object(TShape)
        procedure Draw; virtual;
      end;
    

Now if a Rectangle object is assigned to the Shape variable, the Draw method of _TRectangle_ will be used. The term often used to describe this situation is called **overriding** a parent method. Although the body of main program will the same as in the previous section, the execution behavior will be different. The **virtual** keyword tells the compiler to hold off fixing the specific procedure used and instead lets the binding of the method be determined at runtime dynamically. 

By adding the **virtual** keyword to the type declarations of TShape, TRectangle and TSquare in the last section, the latter portion of the output would look as shown below. Note that in order to run the previous program using virtual methods, some other code needs to be added in order for the program to run. This additional code is described in the next section. 
    
    
    Assigning Rectangle to Shape
    
    Calling Shape.Draw : TRectangle.Draw
    
    Assigning Square to Shape
    
    Calling Shape.Draw : TSquare.Draw
    
    Assigning Square to Rectangle
    
    Calling Rectangle.Draw : TSquare.Draw

Although it is allowed, mixing virtual methods and non virtual (static) methods in the inheritance hierarchy may result in behavior which is difficult to manage. 

## Objects - Constructors and Destructors

Compiling the above example program after adding the virtual keywords will result in non fatal compiler warnings about missing **constructors**. Although the warnings can be ignored, a run time error will almost certainly occur when one of the virtual _Draw_ methods is executed. Due to the peculiarities of this particular OOP implementation, when virtual methods are declared, special initialization code must be included for that object. Specifically, two specialized methods must be included in the object type definition called a **constructor** and a **destructor**. The **constructor** must be called at runtime to initialize the object's virtual method before the method is called. In addition, the constructor can be (and should) be used to initialize any fields, dynamically create associated objects and any other initialization tasks needed when introducing an object. The special **destructor** method is used to take care of any internal and program specific housekeeping when an object is no longer needed. The initialization and cleanup tasks are more useful when using dynamically allocated objects which will be covered in this section also. For simple programs with few objects (like the one in this tutorial), calling not using destructors won't cause any problems. However, in large programs and those that use large class libraries which routinely allocate and deallocate objects dynamically, the implementation of constructors and destructors is very useful. 

Here are are the Shape declarations again, this time using virtual methods and including constructors and destructors. 
    
    
    Type
      TShape = Object
        x, y : single;
        height, width : single;
    
        procedure GetParams; virtual;
        procedure Draw; virtual;
    
        Constructor Init(xx, yy, h, w : single);
        Destructor CleanUp;
      end;
    
      TRectangle = Object(TShape)
        procedure Draw; virtual;
      end;
    
      TSquare = Object(TRectangle)
        procedure GetParams; virtual;
     
        Constructor Init(xx, yy, h, w : single);
      end;
    

The TShape object type includes fields, two virtual methods (Draw and GetParams), a constructor called Init and a destructor called CleanUp. 

TRectangle declares its own _Draw_ method which will override the Parent method in TShape but will inherit all other fields, methods, and the constructor and destructor from TShape. 

TSquare inherits everything from its ancestors (TShape mostly) except for the GetPArams method and the Init constructor. 

In declaring a constructor or destructor, the keyword constructor or destructor is used instead of the keyword function or procedure. In all other respects, they look just like object methods and are called just like object methods. The use of the constructor/destructor keyword is to let the compiler know of any behind the scenes actions to take. Also notice, that the Init constructor includes a parameter list. Normal methods can include parameter lists although none have been used in the tutorial up to this point. 

Fields, methods, constructors and destructors can be declared in any order in the type definition. 

Constructors and destructors act like virtual methods without having to add the virtual keyword. 

For the current program, the Draw method for TSquare has been removed. It will be assumed that the (fictional) graphics code for for drawing a square is the same as for a rectangle and the Draw method can be inherited from TRectangle. The difference in the TSquare and TRectangle object is inherent in the Init constructor and GetParams method. Implementation of the new constructors and destructors is shown below. 
    
    
    Constructor TShape.Init(xx, yy, h, w : single);
    begin
      writeln('TShape.Init');
      x := xx;
      y := yy;
      height := h;
      width := w;
    end;
      
    Destructor TShape.CleanUp;
    begin
      writeln('TShape.CleanUp')
    end;
    
    Constructor TSquare.Init(xx, yy, h, w : single);
    begin
      writeln('TSquare.Init');
      x := xx;
      y := yy;
      height := h;
      width := w;
      if height <> width then
        height := width;
    end;
    

Again, the implentation of constructors look like regular methods except the keywords **constructor** and **destructor** are used instead of the keywords **procedure** and **function**. Consider the following main program. 
    
    
    begin
      writeln;
      
      Shape.Init(1,2,3,4);
      Rectangle.Init(11, 22, 33, 44);
      Square.Init(111, 222, 333, 444);
     
      writeln;
      write ('Calling Shape.Draw : ');
      Shape.Draw;
    
      write ('Calling Rectangle.Draw : ');
      Rectangle.Draw;
    	
      write ('Calling Square.Draw : ');
      Square.Draw;
    	
      writeln;
      writeln ('Assigning Rectangle to Shape');
      Shape := Rectangle;
      writeln;
      write ('Calling Shape.Draw : ');
      Shape.Draw;
    	
      writeln;
      writeln ('Assigning Square to Shape');
      Shape := Square;
      writeln;
      write ('Calling Shape.Draw : ');
      Shape.Draw;
    
      writeln;
      writeln ('Assigning Square to Rectangle');
      Rectangle := Square;
      writeln;
      write ('Calling Rectangle.Draw : ');
      Rectangle.Draw;
    	
      writeln;
    	
      Shape.CleanUp;
      Rectangle.CleanUp;
      Square.CleanUp;
      readln;
    end.
    

which produces the output below. 
    
    
    TShape.Init
    TShape.Init
    TSquare.Init
    
    Calling Shape.Draw : TShape.Draw
    Position: x =    1 y =    2
        Size: w =    4 h =    3
    
    Calling Rectangle.Draw : TRectangle.Draw
    Calling Square.Draw : TSquare.Draw
    
    Assigning Rectangle to Shape
    
    Calling Shape.Draw : TRectangle.Draw
    
    Assigning Square to Shape
    
    Calling Shape.Draw : TSquare.Draw
    
    Assigning Square to Rectangle
    
    Calling Rectangle.Draw : TSquare.Draw
    
    TShape.CleanUp
    TShape.CleanUp
    TShape.CleanUp

Notice that all three calls to the destructor _CleanUp_ result in the using the inherited destructor of TShape since TRectangle and TSquare did not override the _CleanUp_ constructor. The virtual _Draw_ method was overridden for each sub object type and the output reflects this even when the sub objects were assigned to the parent object. Finally, since none of the sub objects implemented destructors, all calls to the CleanUp destructors used the one declared for the parent TShape object. If any of the destructors were declared separately in a sub object, they would have overridden the destructor for TShape since all constructors and destructors are virtual by default. 

FPC allows any procedure identifier to be used for a constructor or destructor name. However, by convention, many OOP languages and object libraries use specific identifiers. One convention is _Create_ for **constructors** and _Destroy_ for **destructors**. 

## Objects - Dynamic Variables

Although static object variables can be created on the stack as has been shown up to this point, it is much more likely that for most programs, objects will be created dynamically. The syntax for declaring dynamic objects and invoking them are similar to that of any other dynamically created variable. The only difference is the addition of an extended syntax for the **New** keyword which incorporates invoking the object constructor. 

Here, pointer types and variables are declared for the various object types defined previously with some sample code snippets showing how the resulting dynamic variables are created, manipulated and disposed. Only the object declaration for _TShape_ is shown and none of the methods, constructors or destructors. Note that in order to inspect the field values more easily, the Draw method was reverted back to static so the _TShape.Draw_ behavior was available for all the different types of objects. 

The first thing to notice that there are three different ways to create dynamic object variables. All three produce the same results. All use the **new** procedure in different ways. The Shape1 and Shape2 objects are being created using **new** as a function, passing it two parameters: (1) the type name and (2) the name of the _Init_ constructor and returning a pointer to the object. The second way is using new as a procedure with the desired variable (Rectangle in this case) to be to be created and the constructor name as parameters. Finally, a pointer for the Square object is created with the **new** procedure and then the constructor method is called separately. This last manner of dynamic object creation will generate a compiler warning but will compile and run OK. 

Next some assignments with objects are made and the Draw method to see how the objects were affected. As can be seen, the results of the assignment operations differ depending whether or not the assignments were done with the pointer variables themselves or whether the pointer was dereferenced. The results are the same for manipulating and dereferencing pointers to objects just as they are for any other type of data structure. Finally, the **dispose** procedure is called for all the created objects. Just as there are three ways to use the new procedure, there are three ways to use the dispose procedure. All three ways are shown. 
    
    
    Type
      TShape = Object
        x, y : single;
        height, width : single;
        procedure GetParams; virtual;
        procedure Draw;
        Constructor Init(xx, yy, h, w : single);
        Destructor CleanUp;
      end;
      
      PShape = ^TShape;
      PRectangle = ^TRectangle;
      PSquare = ^TSquare;
    
    Var
      Shape1, Shape2 : PShape;
      Rectangle : PRectangle;
      Square : PSquare;
    
    begin
      Shape1 := new (PShape, Init(1, 1, 1, 1) );
      Shape2 := new (PShape, Init(2, 2, 2, 2) );
    
      new (Rectangle, Init(11, 22, 33, 44) ); 
    
      new(Square);
      Square^.Init(111, 222, 333, 444);
    
      writeln;
    
      Write ('1) Shape1 : ');
      Shape1^.Draw;
    
      Shape1^ := Rectangle^;
      Write ('2) Shape1 : ');
      Shape1^.Draw;
    
      Rectangle^.x := 77;
      Write ('3) Shape1 : ');
      Shape1^.Draw;
    
      Write ('4) Shape2 : ');
      Shape2^.Draw;
    
      Shape2 := Square;
      Write ('5) Shape2 : ');
      Shape2^.Draw;
    
      Square^.y := 88;
      Write ('6) Shape2 : ');
      Shape2^.Draw;
    
      writeln;
    
      dispose(Shape1);
      dispose(Shape2, CleanUp);
    
      Rectangle^.CleanUp;
      dispose(Rectangle);
    
      dispose(Square, CleanUp);
      readln;
    end.
    
    
    
    TShape.Init
    TShape.Init
    TShape.Init
    TSquare.Init
    
    1) Shape1 : TShape.Draw
    Position: x =    1 y =    1
        Size: w =    1 h =    1
    
    2) Shape1 : TShape.Draw
    Position: x =   11 y =   22
        Size: w =   44 h =   33
    
    3) Shape1 : TShape.Draw
    Position: x =   11 y =   22
        Size: w =   44 h =   33
    
    4) Shape2 : TShape.Draw
    Position: x =    2 y =    2
        Size: w =    2 h =    2
    
    5) Shape2 : TShape.Draw
    Position: x =  111 y =  222
        Size: w =  444 h =  444
    
    6) Shape2 : TShape.Draw
    Position: x =  111 y =   88
        Size: w =  444 h =  444
    
    
    TShape.CleanUp
    TShape.CleanUp
    TShape.CleanUp

  


## Objects - Continued

[Programming Using Objects Page 2](<Programming_Using_Objects_Page_2.md> "Programming Using Objects Page 2")

## External links

  * [Free Pascal Reference guide: Chapter 5 Objects](<//www.freepascal.org/docs-html/current/ref/refch5.html>)
  * [Free Pascal Reference guide: Chapter 6 Classes](<//www.freepascal.org/docs-html/current/ref/refch6.html>)

---

_Source: [https://wiki.freepascal.org/Programming_Using_Objects](https://web.archive.org/web/20250401003424/https://wiki.freepascal.org/Programming_Using_Objects)_
