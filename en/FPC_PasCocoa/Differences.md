# FPC PasCocoa/Differences

│ **English (en)** │

FPC's [Objective-Pascal](<../FPC_PasCocoa.md> "FPC PasCocoa") dialect was introduced to enable seamless interfacing with Objective-C code, in particular on macOS. Due to inherent differences between the C and Pascal philosophies, this language mode will have some seemingly strange behaviour regardless of whether your main background is in Objective-C or in Object Pascal. 

This page describes the conceptual differences between Objective-Pascal and on the one hand Objective-C, and on the other hand Object Pascal. 

## Contents

  * 1 Differences with Object Pascal
    * 1.1 No constructors
    * 1.2 No destructors
    * 1.3 No unique root class
  * 2 Differences with Objective-C
    * 2.1 Marking methods as _override_
    * 2.2 Class instances are implicit pointers
    * 2.3 Protocol method signatures in class declarations
    * 2.4 Duplicate identifiers
    * 2.5 Using class types as first class entities
    * 2.6 Sending class messages to instances
    * 2.7 Instances that conform to protocols
    * 2.8 Sending messages to _id_
    * 2.9 Class extensions



## Differences with Object Pascal

### No constructors

  * **Description** : The Objective-C language has no notion of a formal _constructor_. Hence, the _constructor_ keyword is not supported in an _objcclass_.
  * **What to do** : Construction of class instances in Objective-C usually happens explicitly in two stages: first the _alloc_ class message is sent to the class type that you want to instantiate, and next you send an _initXXX_ instance message to its result. This _initXXX_ (with _XXX_ dependent on the class type) message is called an _initializer_ , and will initialise in the various fields, or may in theory even return a completely different object of a different type.
  * **Warning** : By convention, _initXXX_ methods free the allocated memory and return _nil_ if the initialisation failed for some reason. This means that if you allocate and initialise an instance using two different statements, you have to assign the result of the _initXXX_ method to your instance variable. And you always have to check for _nil_ after calling an _initXXX_ method before using the result if you are not certain that initialisation will always succeed!
  * **Example** :


    
    
    {$modeswitch objectivec1}
    var
      obj: NSObject;
    begin
      // 1. Initialisation in two steps. First allocate the memory.
      obj:=NSObject.alloc;
      // Next, initialise. Warning: do not just use "obj.init", since in theory it could
      // return nil, or even a completely different object!
      obj:=obj.init;
      obj.release;
    
      // 2. Alternative one-liner.
      obj:=NSObject.alloc.init;
      obj.release;
    end.
    

  * **More information** : 
    * <https://web.archive.org/web/20101206131250/http://developer.apple.com/library/mac/#documentation/cocoa/Conceptual/ObjectiveC/Articles/ocAllocInit.html>
    * <https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithObjects/WorkingwithObjects.html>
    * <https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html>
    * <https://web.archive.org/web/20121013134748/http://MacDeveloperTips.com/objective-c/objective-c-initializers.html>
    * <https://web.archive.org/web/20160308011452/http://macdevelopertips.com/objective-c/objective-c-memory-management.html>



### No destructors

  * **Description** : The Objective-C language has no notion of a formal _destructor_. Hence, the _destructor_ keyword is not supported in an _objcclass_.
  * **What to do** : By convention, the _dealloc_ instance method is used like a destructor in Objective-C. You can override this method to perform any kind of cleanup.
  * **Warning** : Never call _dealloc_ manually. Instead, call _release_ if you no longer need the instance **and** if you are responsible for releasing the instance (see the links below for more information about memory management basics of Objective-C/Cocoa). Instances under Objective-C are reference counted, and the _release_ method will only call the _dealloc_ method when the reference count reaches 0. Additionally, when using garbage collected Objective-C (not yet supported by FPC), the _finalize_ instance method is called rather than the _dealloc_ method.
  * **More information** : 
    * <http://macdevelopertips.com/objective-c/objective-c-memory-management.html>
    * <http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/GarbageCollection/Articles/gcInapplicablePatterns.html#//apple_ref/doc/uid/TP40006762-SW4>



### No unique root class

  * **Description** : In Object Pascal, if you do not specify a parent class, a newly defined class will automatically inherit from _TObject_. While in NeXTStep/Apple/GNUStep's Objective-C frameworks _NSObject_ fulfills a similar role, there are also other root classes (such as _NSProxy_). Since there is no unique root class in the Objective-C language, this means that if you do not specify a parent class, you will define a new root class.
  * **What to do** : Generally, you will want new classes to inherit from _NSObject_ if they do not have to inherit from any other class.



## Differences with Objective-C

### Marking methods as _override_

  * **Description** : In Object Pascal, if you want to override an inherited method then you have to add the _override_ keyword to the declaration in the child class. Without _override_ , the inherited method will be hidden in the syntactic scope of the child class, but when casting the child class to the parent type and calling that method, the parent's implementation will still be executed.

In Objective-C all dispatching is name-based, and hence a method with the same selector in a child class will always override a parent method with the same name (unlike in Object Pascal, it is not possible to hide an inherited method, nor to start a new inheritance tree).

Objective-Pascal however also requires _override_ to be specified when overriding inherited Objective-C methods for consistency with Object Pascal. If _override_ is absent, the compiler will give an error. This can also help catch bugs, as explained in [this thread](<http://lists.apple.com/archives/Xcode-users/2009/Aug/msg00251.html>) and [this bug report filed with Apple](<http://openradar.appspot.com/7215146>). There is one exception though: for _external_ classes, _override_ is not required because such declarations are often produced using automated parsers, and requiring them to add _override_ in the right places could needlessly complicate them. Not adding _override_ in such situations will however still cause the compiler to show a hint.

  * **What to do** : Always add _override;_ at the end of method declarations that override inherited methods.



### Class instances are implicit pointers

  * **Description** : In (Delphi-style) Object Pascal, all class instance variables are implicit pointers, and they are also implicitly dereferenced. In Objective-C, class instances are explicitly declared as being "a pointer to a class instance", e.g. _NSObject *obj;_ and need to be explicitly dereferenced when accessing instance variables (although this seldom happens, since most of the time accessors or properties are used). We have chosen to follow the Object Pascal way for Objective-Pascal to make it more familiar for people coming from Object Pascal, and because (given the identifier naming constraints outlined below) using a non-dereferenced form of Objective-C class would hardly ever be wanted in Pascal.
  * **What to do** : Do not use any explicit indirection when dealing with Objective-Pascal class instances.



### Protocol method signatures in class declarations

  * **Description** : When an Objective-C class implements (conforms to) a protocol, the protocol method signatures must not be repeated in the class declaration. In Object Pascal, when a class implements an interface (~ protocol), the method signatures of the interface do have to be repeated in the class definition. The primary reason for this duplication is that in Object Pascal, it is possible to specify that an interface method is implemented using a function with a different name (since dispatching is based on VMT address rather than on name, this is no problem). For consistency with Object Pascal, Objective-Pascal also requires repeating the protocol method declarations in _objcclass_ es that conform to these protocols.
  * **What to do** : copy/paste the protocol method declarations to the _objcclass_ declaration. Repeating the _message_ modifiers is optional, as the compiler will take them from the _objcprotocol_ declaration if they are not specified in the _objcclass_ declaration.



### Duplicate identifiers

  * **Description** : Pascal allows an identifier to appear only once per scope, and treats a class declaration as a single scope. This means that all instance variables, class methods, instance methods and properties must have distinct names. In Objective-C, each of those categories has its own namespace and there is no problem with having an instance method, class method and instance variable all named _foo_ in a single class. Similarly, a class and a protocol can also have the same name (such as the _NSObject_ class and protocol). Also keep in mind that identifiers are case-insensitive in Pascal.
  * **What to do** : On the Pascal-side, all such identifiers must be made unique. In case of methods, these can easily be mapped to the real names used in the corresponding in Objective-C code due to the fact that the selector has to be specified explicitly anyway. We will probably add the ability to specify mappings for instance variables. The real name of an Objective-C class or protocol can be specified by specifying a name along with the _external_ modifier at the end of the type declaration.
  * **Example** :


    
    
    type
      NSObjectProtocol = objcprotocol external name 'NSObject' // specify the real name of this protocol
         // "_class" instead of "class" on the Pascal side, because "class" is a language keyword
         function _class: pobjc_class; message name 'class';
      end;
    
      NSObject = objcclass external (NSObjectProtocol) // name specification is optional since NSObject is the real name
        function _class: pobjc_class; // no need to specify the selector again, it's copied by the compiler from the protocol definition
        // "classClass" (or something else) instead of "_class" because otherwise there are
        // two methods with the same name in the NSObject class.
        class function classClass: pobjc_class; message 'class';
      end;
    

### Using class types as first class entities

  * **Description** : In Objective-C, it is possible to use a class name directly when sending messages to it (e.g., _[NSObject alloc]_), but when an argument is of a class type then the _class_ message must be sent to it (e.g., _[annotation isKindOfClass: [Building class]]_ rather than _[annotation isKindOfClass: Building]_). In Object Pascal, class types can be used directly in such situations (e.g., _instance.inheritsfrom(myclasstype)_). We have opted to also make class types directly usable in Objective-Pascal, because the reason that this is not possible in Objective-C seems to be mostly a [parser limitation](<http://lists.apple.com/archives/Objc-language/2009/Sep/msg00016.html>) rather than a conscious design decision.
  * **What to do** : You can use _typename.classClass_ and _typename_ interchangeably in Objective-Pascal. The _classClass_ name is due to _class_ being a reserved word in Pascal, and __class_ being used for the instance method _class_. That can be seen as another argument in favour of relaxing the Objective-C behaviour.



### Sending class messages to instances

  * **Description** : In Objective-C, class messages must be sent to classes and instance messages to instances. In part, this is required due to the fact that instance and class methods can have the same selector (or alternatively, you can view this as the feature responsible for enabling class and instance methods to have same-named selectors). In Pascal, since every identifier that is part of a class definition (field, instance method, class method, property) must have a unique name, it is possible to call class methods directly using an instance. Since the same identifier scoping rules apply to Objective-Pascal, we have chosen to also enable this feature for that dialect.
  * **What to do** : You can use _instance._class.classmethod_ and _instance.classmethod_ interchangeably. See the previous point for the reason why the _class_ method is called __class_ in Objective-Pascal.



### Instances that conform to protocols

  * **Description** : In Objective-C, declaring a variable of a generic class type that conforms to one or more protocols happens using _id <protocol1,protocol2>_. In Object Pascal, a variable can only be declared to implement a single interface (conform to one protocol), even though individual classes can implement many interfaces just like in Objective-C. The Pascal declaration is simply _interface_typename_. For now, we have limited Objective-Pascal to the same behaviour, i.e., to declare a variable of a generic class type that conforms to the _NSCopying_ protocol, use _var obj: NSCopying;_. The main reason is that it's not clear what the Pascal syntax for a variable conforming to multiple protocols should look like.
  * **What to do** : For now, you will need to typecast your variables to different protocol types if you want call messages belonging to different protocols. Alternatively, you can declare a new protocol type that inherits from all of the required protocols, and then use this protocol as the type for your variable. This may be fixed in the future if a suitable syntax is found to specify this in the variable declaration itself.



### Sending messages to _id_

  * **Description** : In Objective-C, it is possible to send any message to [_id_ -typed](<../FPC_PasCocoa.md> "FPC PasCocoa") variables, even messages whose selector is not prototyped in the current scope. One reason is probably that this is similar to how C works (in C, you can also call functions for which no declaration is visible), and another is that the selector can be deduced unambiguously by the compiler from the invocation. In Objective-Pascal, only messages that are declared as part of an objcclass or objccategory in the current scope can be sent to _id_ -typed variables. This is both due to Pascal conventions (declare before use), and because in Objective-Pascal the compiler cannot automatically deduce the selector name from the method name.
  * **What to do** : Declare a (possibly dummy) _external_ category with the methods that you wish to call before using them. External category declarations will never result in any code being generated that requires the actual existence of this category somewhere, it merely tells the compiler that it should presume that those methods exist for the extended class.



### Class extensions

  * **Description** : Class extensions are basically anonymous categories that are defined in .m rather than in .h files, with the difference that the compiler requires the methods of class extensions to be implemented in the current source file. The reason is that in Objective-C, it is not required to define whether a category is external. As a result, if a category is not implemented in the current source file, the compiler assumes that it is external and implemented somewhere else. If that is not the case and the implementation in the current source file was simply forgotten, then this will result in run time errors. In Objective-Pascal, an _objccategory_ that is not declared as being external must be implemented in the current source file, so this problem does not occur here.
  * **What to do** : You can use regular (non-external) categories defined in the implementation of a unit instead of class extensions, as the effect will be the same.
  * **More Information** : 
    * <http://developer.apple.com/iphone/library/documentation/cocoa/Conceptual/ObjectiveC/Articles/ocCategories.html#//apple_ref/doc/uid/TP30001163-CH20-SW2>

---

_Source: [https://wiki.freepascal.org/FPC_PasCocoa/Differences](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/FPC_PasCocoa/Differences)_
