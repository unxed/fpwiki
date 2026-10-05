# Object Oriented Programming with Free Pascal and Lazarus

│ **English (en)** │  **[русский (ru)](<../ru/Object_Oriented_Programming_with_FreePascal_and_Lazarus.md>)** │

## Contents

  * 1 Introduction
  * 2 Objects - a real-world analogy
    * 2.1 Blood Sample
    * 2.2 Another Example - Automobile
  * 3 Programming Example
  * 4 Object-Oriented Extensions to standard Pascal
    * 4.1 Object
    * 4.2 Class
    * 4.3 Methods
    * 4.4 Properties
      * 4.4.1 Examples
  * 5 Free Pascal Language Extensions
    * 5.1 General Concepts of Object Oriented Pascal
  * 6 Further information
  * 7 See also
  * 8 External Links



## Introduction

There are many excellent tutorials about Pascal, but this tutorial attempts to take the beginner further, into [ Object-Oriented Programming](<Glossary.md> "Glossary") with [Object Pascal](<Object_Pascal.md> "Object Pascal"), which is an extension to standard [Pascal](<Pascal.md> "Pascal"), offered by [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal"), [Delphi](<Delphi.md> "Delphi") and [Free Pascal](<Free_Pascal.md> "Free Pascal") / [Lazarus](<Lazarus.md> "Lazarus"). 

An [ Object](<Glossary.md> "Glossary") is an extension of the standard Pascal [record](<http://wiki.lazarus.freepascal.org/Record>) structure 

Standard text-based Pascal programming is good for creating applications which, like traditional Unix applications, do one thing and do it very well. The 'one thing' that a program does might be a very complicated action, and might offer the user several menu-driven options, but it is essentially restricted to obeying commands that the user types at the keyboard and listing its responses on the terminal or a printer. 

In order to provide a [ Graphical User Interface (GUI)](<Glossary.md> "Glossary") it is usual to invoke some sort of Object-Oriented Programming method (often making use of [ C](<Glossary.md> "Glossary") or one of its variants, or Visual Basic, or one of the [ OO](<Glossary.md> "Glossary") variants of Pascal such as [ Free Pascal](<Glossary.md> "Glossary") with or without [ Lazarus](<Glossary.md> "Glossary")). 

In a GUI the user is presented with a screen with a large number of images arranged in a structured way, consisting of a set of tools or [ Widgets](<Glossary.md> "Glossary") that are associated with various actions such as 

  * selecting from a menu,
  * opening or saving files,
  * connecting to the Internet,
  * performing numerical computations, etc.



The user is expected to move a mouse or other pointer/selection tool over the screen, to select actions which are to be performed in response to a mouse button click or a key-press. 

While systems to deal with a complex graphical user interface could be written in standard Pascal or almost any other programming language, it is much easier to use an object-oriented system, in which each graphical object on the screen can have all its properties and the procedures and functions associated with its use kept together in a common structure. 

## Objects - a real-world analogy

Consider the analogy of a blood sample collected in a hospital or a doctor's office. 

### Blood Sample

The physical sample is certainly an **object** ; it has, associated with it, a lot of information, documents and other physical objects. 

  * **Sample tube** , of a type determined by the test that the physician wants performed.


  * **Local rule** (or _method_ , _standard operating procedure_) to direct the nurse or technician collecting the sample 
    * which type of tube to use,
    * how to process the sample
    * how to store it before transfer to the lab.


  * **Label** on the tube with details of the 
    * sample identifier
    * patient's name and birthdate
    * date and time of collection
    * tests required.


  * **Request form** that accompanies the sample to the lab, directing 
    * sample ID
    * ID of requesting physician
    * what test the physician requests and
    * giving fuller details of the patient
    * the possible diagnosis for which confirmation is sought.



A copy of the request form is placed in the patient's notes, to remind the physician to expect results within an appropriate time. 

  * In the lab - **local methods** to determine 
    * how the sample is to be analysed,
    * what machines to use,
    * how the machines are to be calibrated and operated,
    * how the results are to be stored, formatted and
    * reported back to the physician.



The actual **results** are a record that the physician uses to assist the diagnosis, and a copy of the results is filed in the patient notes. 

The physical sample might be retained for reference, confirmation or further tests, or might be disposed of by pouring down the sink or by incineration; there will be a method to describe this. 

There is no need for a physician to spell out all the details and instructions every time a sample is collected; indeed, he may have very little knowledge of how the sample is processed in the lab. The details of the various processes are **inherited** from previous sample collections and analyses - there will be a generic plan for the whole sequence, and together we could think of the blood sample, all its documents and data, and the underlying methods, as a complex **object**. 

In the physician's mind, the blood sample is seen as almost the same entity as its results, and to the nurses and technicians the sample, the tube, the label and the storage conditions again form a single entity. 

### Another Example - Automobile

If you don't like blood, the same sort of reasoning could be applied to a car taken to a garage for repair. It might consist of: 

  * the physical vehicle
  * documents held by the owner: registration or license (including license plates), insurance, receipts for purchase, parts, repairs etc
  * the fuel consumption history
  * the drivers allowed to use the vehicle, with their license particulars
  * service records held by the garage
  * methods or procedures to be followed for routine checking and maintenance
  * methods to be followed for non-routine repairs etc
  * billing information for the customer



## Programming Example

Enough of this pre-occupation with Real-World examples! Let us proceed to the main purpose: programming in [Object Pascal](<Object_Pascal.md> "Object Pascal"). 

Let us consider the creation of a simple Form with a few controls for an application in Free Pascal/Lazarus. 

[![ObjectInspector-TForm.png](https://wiki.freepascal.org/images/2/25/ObjectInspector-TForm.png)](</File:ObjectInspector-TForm.png>)

[![BlankForm.png](https://wiki.freepascal.org/images/e/e2/BlankForm.png)](</File:BlankForm.png>)

On invoking the Lazarus IDE, the programmer is presented with a blank template Form design, on which he is encouraged to place various controls or objects. 

Note that the pre-made blank Form is already an Object, with its own properties such as position (Top and Left), size (Height and Width), colour, default font for adding text etc. 

  


* * *

If a Button control is placed on the Form (type TButton), it will have its own series of properties, which can be examined in the Object Inspector window. 

Several of the properties have names similar to those for the Form; this is because many properties are **Inherited** from some common Ancestor class, which lays out how properties are to be defined and handled by the descendant classes. 

As well as properties, the Object Inspector offers a tab called **Events** , which gives access to Event Handlers which are methods instructing the application how to deal with things such as a mouse click on a button (OnClick) or some change in the position, size or other properties (OnChange). 

The physical image of the Button on the Form, together with all its properties and Event Handler methods, should be regarded as a single entity or **Object** in Pascal. 

[![ObjectInspector-TButton.png](https://wiki.freepascal.org/images/f/f5/ObjectInspector-TButton.png)](</File:ObjectInspector-TButton.png>)

[![FormWithButton.png](https://wiki.freepascal.org/images/e/e8/FormWithButton.png)](</File:FormWithButton.png>)

[![Source FormWithButton1.png](https://wiki.freepascal.org/images/f/f5/Source_FormWithButton1.png)](</File:Source_FormWithButton1.png>)

## Object-Oriented Extensions to standard Pascal

The Pascal record structure is extended by defining an 

### Object

An Object is a special kind of record. The record contains all the ﬁelds that are declared in the object's deﬁnition (just like a conventional record), but now procedures and functions can be declared as if they were part of the record and are held as pointers to the methods associated with the object's type. 

For example, an object could contain an array of real values, together with a Method for calculating the average. 
    
    
    Type
      Average = Object
        NumVal: Integer;
        Values: Array [1..200] of Real;
        Function Mean: Real; { calculates the average value of the array }
      End;
    

Objects can ”inherit” ﬁelds and methods from ”parent” objects. This means that these ﬁelds and methods can be used as if they were included in the objects declared as a ”child” object. 

Furthermore, a concept of visibility is introduced: ﬁelds, procedures and functions can be declared as public, protected or private. By default, ﬁelds and methods are public, and can be exported outside the current unit. Protected fields or methods are available only to objects descended from the current ancestor object. Fields or methods that are declared private are only accessible in the current unit: their scope is limited to the implementation of the current unit. 

### Class

Objects are not used very often by themselves in Free Pascal and Lazarus; instead, Classes are used very widely. A Class is defined in almost the same way as an Object, but is a pointer to an Object rather than the Object itself. Technically, this means that the Class is allocated on the Heap of a program, whereas the Object is allocated on the Stack. 

Here is a simple example of a typical Class declaration: 
    
    
    {-------------------------------------------}
    { Example of Class declaration from the LCL }
    {-------------------------------------------}
      TPen = class(TFPCustomPen)
      private
        FColor: TColor;
        FPenHandleCached: boolean;
        FReference: TWSPenReference;
        procedure FreeReference;
        function GetHandle: HPEN;
        function GetReference: TWSPenReference;
        procedure ReferenceNeeded;
        procedure SetHandle(const Value: HPEN);
      protected
        procedure DoAllocateResources; override;
        procedure DoDeAllocateResources; override;
        procedure DoCopyProps(From: TFPCanvasHelper); override;
        procedure SetColor
             (const NewColor: TColor; const NewFPColor: TFPColor); virtual;
        procedure SetFPColor(const AValue: TFPColor); override;
        procedure SetColor(Value: TColor);
        procedure SetMode(Value: TPenMode); override;
        procedure SetStyle(Value: TPenStyle); override;
        procedure SetWidth(value: Integer); override;
      public
        constructor Create; override;
        destructor Destroy; override;
        procedure Assign(Source: TPersistent); override;
        property Handle: HPEN read GetHandle write SetHandle; deprecated;
        property Reference: TWSPenReference read GetReference;
      published
        property Color: TColor read FColor write SetColor default clBlack;
        property Mode default pmCopy;
        property Style default psSolid;
        property Width default 1;
      end;
    

  
Note that this class is defined as an instance of another **parent** or **ancestor** class (_TFPCustomPen_) from which it **inherits** all its properties and methods. It has some fields of its own, grouped under 

  * **private** \- this means that items defined here are only available or visible to other classes or procedures/function defined within the same program unit (this example is from _Graphics_ , so any of the other classes such as _TBitMap_ , _TPicture_ etc in the same unit can use them). They are essentially local variables (eg _FColor_ , _FPenHandleCached_) or locally used methods (_GetHandle_ , _SetHandle_) but can be used or referred to in items declared in the **protected** or **public** sections.
  * **protected** \- this means that items defined here are only available or visible to classes that are descended from this **ancestor** class, and inherit its properties or methods
  * **public** \- this means that items defined here are available to any programming unit that includes the current unit in its [ Uses](<Glossary.md> "Glossary") clause
  * **published** \- is the same as a **public** section, but the compiler also generates type information that is needed for automatic streaming of these classes. Often the list of published items appear in the Object Inspector of Lazarus; if there is no **published** list, all the **public** fields usually appear in the Object Inspector.



### Methods

A method is just like a standard procedure or function, but can have some additional **directives**. 

Some of the methods defined above are labelled with the directive **virtual** ; others are labelled with the **override** directive. 

  * **virtual** means that the type or actual instance of a method is not known at compile-time, but is selected at run-time depending on what sub-program actually calls the method. It could be considered a place-holder in the definition of the class.
  * **override** means that at run-time the locally given definition can take the place of a definition inherited from an ancestor class, particularly if it was **virtual**. If you particularly want to use the method defined in the ancestor class, it is sometimes necessary to call it specifically with the **inherited** clause.



Methods with no virtual or override directive are **static** methods (the usual kind in Pascal). Those with a virtual or override directive are **dynamic**. 

Special instances of methods are: 

  * **create** \- a **constructor** for a class, which takes care of allocating memory, collecting together all the information needed and configuring/initializing the various properties.
  * **destroy** \- a **destructor** for a class, which removes all the parts of the class from the system in an orderly and logical way, and returns all its resources for the system to re-use.



### Properties

Properties are just like ordinary fields in a conventional Pascal record, but they can have **read** and/or **write** specifiers. 

  * **read** specifier is a field, or a function that returns a result of the correct type for the property. In the example above, the property _Color_ has a read specifier _FColor_ , which is a local variable containing the value to be used. If a property has a **read** but no **write** specifier, it is read-only.
  * **write** specifier is a field, or a procedure that will store the value of the property in a specific location. In the example above, _Color_ has a write specifier _SetColor_ that is a procedure (defined in the **protected** section) for writing the color value to some specified location. If a property has a **write** but no **read** specifier, it is write-only.
  * **default** \- note that it is possible to set a **default** value for a property. For example, _Color_ here is given the default value _clBlack_ , or black, at the time of creation. It could subsequently be given a different value, by a programming assignment statement, or in the Object Inspector.
  * **index** specifier is an integer that the **read** or **write** methods, shared between properties, can use to identify which property is desired. Note that if **index** is used the **read** or **write** specifiers must be a function or a procedure respectively and cannot be a normal field/variable.



#### Examples
    
    
      TFooClass = class
      private
        FIntProp: Integer;
      public
        property IntProp: Integer read FIntProp write FIntProp;
      end;
    
    
    
      TFooClass = class  
      private
        function GetListProp(AIndex: Integer): String;
        procedure SetListProp(AIndex: Integer; AValue: String);
      public
        // this type of property may not be in the published section of a class
        property ListProp[AIndex: Integer]: String read GetListProp write SetListProp; 
      end;
    
    
    
      TFooClass = class
      private
       function GetValue(const AIndex: Integer): Integer;
       procedure SetValue(const AIndex: Integer; AValue: Integer);
      public
        // note that the read and write methods are shared
        property Value1: Integer index 1 read GetValue write SetValue;
        property Value2: Integer index 2 read GetValue write SetValue;
        property Value3: Integer index 3 read GetValue write SetValue;
        property Value4: Integer index 4 read GetValue write SetValue;
        // index may be a const or a number or you can even use an enumerated type
        // for example:
        // property Value: Integer index ord(seSomeEnum) read SomeFunction write SomeProcedure;
      end;
    

## Free Pascal Language Extensions

FPC includes several language extensions to its "standard" Pascal syntax to support object oriented programming. 

  * Objects (chapter 5) [Programming Using Objects](<Programming_Using_Objects.md> "Programming Using Objects")
  * Classes (chapter 6) [Programming Using Classes](<Programming_Using_Classes.md> "Programming Using Classes")
  * Interfaces (chapter 7) [Programming Using Interfaces](</index.php?title=Programming_Using_Interfaces&action=edit&redlink=1> "Programming Using Interfaces \(page does not exist\)")
  * Generics (chapter 8) [Programming Using Generics](</index.php?title=Programming_Using_Generics&action=edit&redlink=1> "Programming Using Generics \(page does not exist\)")



These extensions are described in the indicated chapters of the FPC Language Reference Guide: <http://www.freepascal.org/docs.var>. Links to tutorial pages for each concept are included above as well. The Language Reference Guide includes syntax diagrams and further details not contained in this introductory tutorial. Of the four language features listed above, **Objects** and **Classes** form the basis of object oriented programming (OOP) in FPC and Lazarus. For those new to OOP, the **Objects** section includes more introductory concepts and the **Classes** section minimizes repetition by emphasizing the similarities and differences to the **Objects** syntax. In general, the **Classes** implementation seems to be more widely in use including Delphi Lazarus developers. Often the word "objects" is used to refer to what is actually a "class" in the **Classes** dialect of OOP in FPC. These documents will be worded to minimize any terminology confusion, but outside of this document, the term "object" oftentimes refers to objects created from a **Class**. In fact, the FPC run time library (RTL) includes a class library with a base **class** called _TObject_. 

Users familiar with the older Turbo Pascal OOP implementation may initially want to skip the section on Classes since the Objects implementation is based on the older Turbo Pascal dialect. The section on **Classes** should be familiar to Delphi users since it is based on Delphi syntax. Be aware that some of the writeup in the **Classes** section may refer to concepts from the **Objects** section. For Macintosh developers familiar with the various Apple, THINK and MPW Object Pascal dialects, neither the FPC Objects or Classes dialects provide a direct migration path. As of March 2009, there are discussions on the Mac Pascal Mailing list about potentially providing some compiler support (new syntax) for accessing Apple's Objective C / Cocoa framework. 

### General Concepts of Object Oriented Pascal

OOP provides different ways to manage and encapsulate data and to manage program flow compared with other available programming language features and constructs. OOP often lends itself to modeling certain applications such as Graphic User Interfaces (GUI's) and physical systems in a more natural feeling manner. However OOP is not appropriate for all applications. Program control is not as explicit as the more basic Pascal procedural constructs. To obtain the most benefit from OOP, understanding of large class libraries is often required which can entail a steep learning curve. Maintaining large OOP application code has its advantages and disadvantages compared to maintaining strictly procedural code. There are many sources for learning OO analysis, design and programming techniques which are beyond the scope of this guide. 

There are numerous programming languages which incorporate OOP features as extensions or the basis of their language. As such, there are many different terms for describing OO concepts. Even within FPC, some of the terminology overlaps. In general, OOP usually consists of the concept of a programming object (or information unit) which explicitly combines and encapsulates a related set of data and procedures which act on the data. This data is usually persistent during program execution but with mechanisms to reduce some of the problems inherent in declaring global variables. In addition, OOP languages enable objects to be incrementally modified and/or extended based on previously defined objects. This feature is usually referred to by the terms _inheritance_ and _polymorphism_. Many OOP languages use the terms _method_ or _message_ referring to procedures which belong to an object. Much of the power of OOP is realized by late (run time) dynamic binding of methods rather than compile binding. This dynamic binding of methods is similar to using procedural variables and procedural parameters but with greater syntactic cohesion, encapsulation with the data it is related to and also inheritance of behavior of previously defined methods. The following wiki pages provide a starting point for discovering more about analysis, design and programming in an object orient manner. 

## Further information

This has only scratched the surface of the topic. For more details, readers are strongly recommended to read the [Free Pascal](<http://lazarus-ccr.sourceforge.net/fpcdoc/ref/ref.html>) manuals, especially Chapters 5 (**Objects**) and 6 (**Classes**) 

Borland was also kind enough to release the Object Oriented Programming chapter from the original Turbo Pascal manuals in PDF form. This chapter is from when Borland introduced this new language feature into Turbo Pascal. The chapter might be old, but the OOP theory is still very relevant today. You can download a copy from the following [link](<http://geldenhuys.co.uk/articles/>) under the "Other" category. 

## See also

  * [Programming Using Classes](<Programming_Using_Classes.md> "Programming Using Classes")
  * [Free Pascal video tutorials](<Free_Pascal_videos.md> "Free Pascal videos")
  * [Lazarus video tutorials](<Lazarus_videos.md> "Lazarus videos")
  * [Pascal and Lazarus Books and Magazines](<Pascal_and_Lazarus_Books_and_Magazines.md> "Pascal and Lazarus Books and Magazines")



## External Links

  * <https://castle-engine.io/modern_pascal_introduction.html> \- FPC mode ObjFPC
  * <http://newpascal.org/assets/modern_pascal_introduction.html> \- FPC mode Delphi


  * <http://en.wikipedia.org/wiki/Object-oriented_programming>
  * <http://en.wikipedia.org/wiki/OOAD>
  * <http://en.wikipedia.org/wiki/Object_oriented_design>

---

_Source: [https://wiki.freepascal.org/Object_Oriented_Programming_with_FreePascal_and_Lazarus](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Object_Oriented_Programming_with_FreePascal_and_Lazarus)_
