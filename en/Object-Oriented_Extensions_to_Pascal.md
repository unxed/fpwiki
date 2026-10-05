# Object-Oriented Extensions to Pascal

**Object Oriented Extensions to Pascal** (abbreviated _OOE_) is an ANSI draft to extend [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") to support object-oriented programming. A [Compiler Mode](<Compiler_Mode.md> "Compiler Mode") _ooextpas_ is planned to cover this language mode (after Extended Pascal is implemented). It's similar to but different from the [Object Pascal](<Object_Pascal.md> "Object Pascal") dialect of [Free Pascal](<Free_Pascal.md> "Free Pascal") 3.0 and earlier. 

## Contents

  * 1 Enhancements to Extended Pascal
    * 1.1 Normative enhancements
      * 1.1.1 Already supported features
      * 1.1.2 Root class
      * 1.1.3 Virtual methods
      * 1.1.4 Multiple inheritance
      * 1.1.5 Class types
        * 1.1.5.1 Concrete classes
        * 1.1.5.2 Abstract classes
        * 1.1.5.3 Property classes
          * 1.1.5.3.1 TextWritable
      * 1.1.6 Deferred classes
      * 1.1.7 Class views
      * 1.1.8 Null
      * 1.1.9 Changes to modules
    * 1.2 Non-normative enhancements
      * 1.2.1 Generic types
      * 1.2.2 Exception handling
      * 1.2.3 Procedural variables
      * 1.2.4 Default parameters
      * 1.2.5 Set extensions
      * 1.2.6 Overloading methods
      * 1.2.7 Persistent objects, object evolution, object roles
      * 1.2.8 Access abstraction
      * 1.2.9 Operator overloading and definition



## Enhancements to Extended Pascal

### Normative enhancements

#### Already supported features

These features are already supported in Free Pascal 3.0 and earlier with the modes [OBJFPC](<Mode_ObjFPC.md> "Mode ObjFPC") and [DELPHI](<Mode_Delphi.md> "Mode Delphi"). 

  * The keyword **class**
  * Ancestor class in parentheses
  * **Create** and **Destroy** are the names of the constructor and destructor
  * **inherited** can be use to call a method from an ancestor class
  * **override** overrides virtual methods in ancestor classes
  * **abstract** specifies abstract methods
  * The implicit parameter **Self**
  * Type coercion
  * The membership operator **is**.
  * The **with** statement can be used with classes.



#### Root class

The class from which all other classes derive is called _Root_ , instead of _TObject_. Root has two methods: **Clone** and **Equal**. **Equal** is equivalent to _TObject's_ **Equals**. There's also a **Copy** function that does the same thing as the **Clone** method. 

#### Virtual methods

All methods are virtual in OOE and thus the **override** directive must be used when replacing or augmenting them in descendant classes. 

#### Multiple inheritance

OOE supports multiple inheritance with a defined resolution mechanism. 

#### Class types

##### Concrete classes

These are classes made with the keyword **class** (already supported). 

##### Abstract classes

These are classes made with **abstract class**. They're different from concrete classes in that they can't be constructed; instead, only a descendant class inheriting them can be constructed. 

##### Property classes

These are classes that contain only the interface for its methods. The methods must be implemented in descendant classes. Property classes can only inherit from other property classes. They are declared via **property class**. 

###### TextWritable

OOE specifies a **TextWritable** property class with two methods: **WriteObj** and **ReadObj** , both taking a variable parameter of type **text**. 

#### Deferred classes

Classes can be declared before they're defined via [**abstract** |**property**] **class** **..** **end;**. 

#### Class views

Class views specify which methods can be visible to whom. It's used instead of the **public** , **private** , and **protected** sections of a class. They are declared via **view of** _classname_(_inheritance-list_). 

#### Null

OOE specifies **Null** to be used with classes like **nil** is for pointers. 

#### Changes to modules

Modules can export and rename classes and class members, make certain members **protected** , etc. It's expected that this expanded use of **export** will be generalized in future revisions of Extended Pascal. OOE also suggests that Extended Pascal be changed to allow parameter lists to be repeated in implementation modules so that function overloading will be possible. 

### Non-normative enhancements

#### Generic types

These weren't included in the normative specification because of their complexity, but their implementation is recommended. Generics in OOE are defined as being an application of schemata with the discriminant's type having the syntax **type** (_type-list_). These generic types would include generic classes. 

#### Exception handling

This and the related procedure **Assert** are mentioned, but no mechanism for using them is defined. 

#### Procedural variables

This is discussed and dismissed. Free Pascal of course already supports procedural variables. 

#### Default parameters

The **value** keyword is extended to include default parameters. 

#### Set extensions

**set of** _class_

#### Overloading methods

This would go naturally with the suggested module extensions. 

#### Persistent objects, object evolution, object roles

These aren't included in the specification due to their dubious usefulness. 

#### Access abstraction

A mechanism for this isn't specified. 

#### Operator overloading and definition

Overloading existing operators or creating new ones is deemed useful, but there's no specified mechanism. Free Pascal supports [PXSC](<PXSC.md> "PXSC")-style operator overloading, but not operator definition. [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") permits defining new operators through operator overloading.

---

_Source: [https://wiki.freepascal.org/Object-Oriented_Extensions_to_Pascal](https://web.archive.org/web/20240920204115/https://wiki.freepascal.org/Object-Oriented_Extensions_to_Pascal)_
