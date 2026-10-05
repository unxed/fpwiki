# Singleton Pattern

│ **English (en)** │  **[русский (ru)](<../ru/Singleton_Pattern.md>)** │

## Contents

  * 1 Introduction
    * 1.1 What You Will Learn
    * 1.2 Acknowledgement
  * 2 The Pattern
    * 2.1 The diagram
    * 2.2 The Singleton Client
    * 2.3 Nice Pattern, Don't Use It
  * 3 Going Global
    * 3.1 In Your (Inter)Face
    * 3.2 Up Your Implementation
  * 4 Classy Implementation
    * 4.1 Simple Singleton
    * 4.2 Why It Is Different In Pascal
    * 4.3 The 'constructor should be public' Singleton
    * 4.4 Hide That For Me
  * 5 Discussion Of The Options
    * 5.1 Reducing the visibility of the constructor
    * 5.2 Design Time Help vs. Run Time Breaking
    * 5.3 The pattern is not followed
    * 5.4 The implementations have a problem with multi-threading and synchronization
    * 5.5 The singleton class implementation has a problem with platform independence
  * 6 Conclusion



# Introduction

Considered one of the simplest patterns, it probably is the most discussed pattern: the singleton. What is so difficult to this pattern? Well, no matter the language in which you implement it, you have to do some counter intuitive things to reap all the benefits of the pattern. Every programming language has it's own quirks with this pattern, FreePascal is no exception. As a result, any implementation may lead to discussion and the choices you make for the implementation will be against the principles of another programmer. The implementation of this pattern is never ideal, the approaches provided in this article included. Yet, I hope this article is useful for you. 

## What You Will Learn

  * The benefits of the Singleton pattern and why to avoid it
  * Global variables and class variables in FreePascal
  * How to hide the constructor and keep it private



## Acknowledgement

I didn't think up the examples and the arguments. I found the examples in forum discussions and their archives. This article is a overview of the discussion about singletons and a collection of the usable examples I found. 

  * [Making a Singleton class](<http://community.freepascal.org:10000/bboards/message?message_id=234708&forum_id=24097>)
  * [Implementing a true Singleton - Can we decrease the visibility of a method?](<http://lists.freepascal.org/lists/fpc-pascal/2006-December/012826.html>)
  * [Warning: Constructor should be public](<http://www.mail-archive.com/fpc-devel@lists.freepascal.org/msg13833.html>)
  * [Compiler Warning: Constructor should be public](<http://www.mail-archive.com/fpc-pascal@lists.freepascal.org/msg18257.html>)
  * TDockManager
  * Printers4Lazarus package



# The Pattern

## The diagram

[![The Singleton pattern](https://wiki.freepascal.org/images/5/56/250pxSingleton.png)](</File:250pxSingleton.png> "The Singleton pattern")  


The Singleton pattern is described in the above image. The Singleton class defines a private class variable that holds the Singleton instance. The class method _GetInstance_ creates that instance once and only once and returns it to the client. The constructor method 'Singleton()' is private and thus not visible to the client. The Design Patterns book lists the following benefits: 

  * Controlled access to sole instance
  * Reduced namespace
  * Permits refinement of operations and representation
  * Permits variable number of instances (extend the class to allow a controlled number of instances if designer wants to allow such a situation)
  * More flexible than class operations



## The Singleton Client

The following program would be typical use of a singleton. 
    
    
    program TrueSingleton;
     
     {$mode objfpc}{$H+}
     
     uses
       {$IFDEF UNIX}{$IFDEF UseCThreads}
       cthreads,
       {$ENDIF}{$ENDIF}
       Classes, Singleton2
       { you can add units after this };
     
     {$R TrueSingleton.res}
     var
       s1, s2: TSingleton;
     
     begin
       s1:= TSingleton.GetInstance;
       s1.name := 'one';
       writeln('name of s1: '+s1.name);
     
       s2:= TSingleton.GetInstance;
       s2.name := 'two';
     
       writeln('name of s1: '+s1.name);
       writeln('name of s2: '+s2.name);
       //writeln('name of singleton: ' + Singleton.name);
       readln;
     end.
    

That is, if the singleton would work according to the pattern. The _GetInstance_ method is a class method that returns the only available instance. We will help our user, the programmer, if the constructor _Create_ would not show up in the code completion at all. This helps our users to remember that they should use _GetInstance_. A problem here however is that FreePascal, in contrast to other languages, doesn't require a specific name as constructor. _Init_ is just as good a name for a constructor as _Create_ is. 

The correct output will look like this: 
    
    
    name of s1: one
    name of s1: two
    name of s2: two
    

When the correct output is produced, `Singleton.GetInstance` will return the singleton instance and the name of both `s1` and `s2` will always be the same. The wrong output will look like this: 
    
    
    name of s1: one
    name of s1: one
    name of s2: two
    

When the wrong output is produced, a second instance is created that has its own name. 

## Nice Pattern, Don't Use It

Or at least think it over before you use it. There are many reasons to not use a singleton. Really, there are more reasons not to use a singleton than there are reasons to use a singleton. See the [Drawbacks and Reference sections](<https://en.wikipedia.org/w/index.php?title=Singleton_pattern&oldid=397293289#Drawbacks>) of an old version of the WikiPedia article discussing singletons. The main reasons: 

  * Difficult to test. Because of its nature, singletons are hard to replace for testing.
  * Singletons make dependencies unclear. Although the singleton comes from another unit, you cannot see in the interface of your object that the singleton is used. Well, the **uses** section gives a clue, but that doesn't say _where_ exactly it is used.



Singletons are commonly used for shared resources or 'engines': 

  * Printers
  * Databases
  * Loggers



To avoid the use of too many singletons, try to avoid them and use only one in your whole application. For example, you may want to create a toolbox (aka [[Registry](<http://martinfowler.com/eaaCatalog/registry.html>)]) for your application. Now the toolbox knows the printer, database or logger and your code queries the toolbox for instances. Your code depends on the toolbox and the toolbox depends on the resources that can be queried. This reduces the dependencies from a lot of singleton to only one singleton. 

Another idea is to use [[Dependency Injection](<http://www.theserverside.com/news/1321158/A-beginners-guide-to-Dependency-Injection>)]. If an object 'B' needs another object 'A' to function properly, then hand the object 'A' to the constructor of the class 'B'. This at least makes clear that the class B depends on A in the interface. 

To improve testing of your code, you may want to have your singletons implement an interface. The _GetInstance_ method or function may then return an instance that conforms to the interface. Depending on whatever (compilation directives etc.) the _GetInstance_ method can return the 'real' object or a mock-up. 

In the rest of this article I discuss how to create a singleton. 

# Going Global

## In Your (Inter)Face

In Free Pascal it is quite common to define a global variable for the Singleton pattern. All through the source code of FreePascal and Lazarus you will find examples of this. The idea is to create an instance at initialization of the unit file and then use it when needed. The client programmer should just know that there is a global instance and that that global instance should be used. Every programmer writes and reads full documentation, so usually this is no problem. 
    
    
    unit GlobalVariableSingleton;
     
     {$mode objfpc}{$H+}
     
     interface
     
     type
       TSingleton = class
         name: String;
         constructor create;
       end;
     
     var
       Singleton: TSingleton = nil;
     
     implementation
     {* Something happens out here *}
     
     initialization
       Singleton := TSingleton.Create;
     
     end.
    

When this unit is loaded, the singleton is instantiated and can be used. The initialization takes place when the program is started, so the singleton is always present. This violates the reduction of namespace, the namespace is polluted by a global variable that is waiting for use. Even if the programmer is not interested in the instance, but is only interested in other functionality in your unit, the instance is created and will use memory. On the other hand, we wanted to have a singleton so that we would use it right? There is no probability involved, we will use the instance. So, why not create the instance when we startup the application? With the above definition of a global variable the instantiation is not controlled. Any programmer can still instantiate the TSingleton. This can be improved with documentation and a constructor that warns against instantiation after the global variable is created. Something like the following: 
    
    
    constructor TSingleton.Create;
       begin
         If Assigned(Singleton) then
           Raise Exception.Create('Please use the global variable Singleton');
         inherited Create;
       end;
    

However, such a constructor only warns at run time and doesn't help at design time. An alternative to the if statement could be an assert. With an assert the detection would be moved to test time, which – as you know - we all do and happens far before run time. However, an assert doesn't even help at run time since the asserts are not linked in for the run time application. I agree, it does seem to happen that not all code is always tested. In the given constructor the program at least breaks due to an unhandled exception. The programmer of your client will probably work around that annoyance with a `try ... except` statement. Even though programmers can work around the issue, their code doesn't work so why should they? The global variable is not a singleton at design time, the constructor will still show up. However, it will certainly be a singleton at run time. The typical client code is: 
    
    
    program TrueSingleton;
     
     {$mode objfpc}{$H+}
     
     uses
       {$IFDEF UNIX}{$IFDEF UseCThreads}
       cthreads,
       {$ENDIF}{$ENDIF}
       Classes, GlobalVariableSingleton
       { you can add units after this };
     
     {$R TrueSingleton.res}
     var
       s1, s2: TSingleton;
     
     begin
       s1:= Singleton;
       s1.name := 'one';
       writeln('name of s1: '+s1.name);
     
       s2:= Singleton;
       s2.name := 'two';
     
       writeln('name of s1: '+s1.name);
       writeln('name of s2: '+s2.name);
       //writeln('name of singleton: ' + Singleton.name);
       readln;
     end.
    

Of course, instead of assigning the global variable to a local variable you can also use the global variable directly. 

## Up Your Implementation

Notice how in the _GlobalVariableSingleton_ unit the global variable is defined in the interface section. This is what makes the variable _Singleton_ a global variable. If you put the variable in the implementation section, the variable will only be available for code in the implementation section. As such, a variable defined in the implementation section acts similar to a class or package variable. The implementation variable can be used by all the code in the unit but not outside that unit. The interface variable is public, can be used inside and outside the unit. This is used in the following code. 
    
    
    unit GlobalFunctionSingleton;
     
     {$mode objfpc}{$H+}
     
     interface
     
     uses
       Classes, SysUtils; 
     type
       TSingleton = class
       public
         name : String;
         constructor Create;
       end;
     
     function GetSingleton : TSingleton;
     
     implementation
     var
       Singleton : TSingleton = nil;
     
       constructor TSingleton.Create;
       begin
         assert(Singleton=nil, 'illegal recreation of Singleton.');
         inherited Create;
         Singleton := self;
       end;
     
     function GetSingleton : TSingleton;
     Begin
       If(Singleton = nil) then
         raise Exception.Create('Singleton not created during initialization.');
       Result := Singleton;
     end;
     
     initialization
       Singleton := TSingleton.Create;
     end.
    

In this case, the singleton instance is not a global variable (it is defined in the implementation section) and cannot be used outside the implementation section. The _GetSingleton_ function however can be used outside the unit. But in contrast to the pattern diagram, it is not a class method but a good old global function. The advantage over the previous implementation is that the _GetSingleton_ function can contain more functionality than just returning the one instance. For example, the function could be used to hand out instances from a pool of singletons. The client code is as follows: 
    
    
    program TrueSingleton;
     
     {$mode objfpc}{$H+}
     
     uses
       {$IFDEF UNIX}{$IFDEF UseCThreads}
       cthreads,
       {$ENDIF}{$ENDIF}
       Classes, GlobalFunctionSingleton
       { you can add units after this };
     
     {$R TrueSingleton.res}
     var
       s1, s2: TSingleton;
     
     begin
       s1:= GetSingleton;
       s1.name := 'one';
       writeln('name of s1: '+s1.name);
     
       s2:= GetSingleton;
       s2.name := 'two';
     
       writeln('name of s1: '+s1.name);
       writeln('name of s2: '+s2.name);
       //writeln('name of singleton: ' + Singleton.name);
       readln;
     end.
    

It works, but I have not seen this applied in any source code. 

# Classy Implementation

## Simple Singleton

The following code presents a very simple singleton. 
    
    
    unit Singleton1;
     
     {$mode objfpc}{$H+}
     
     interface
     
     type
       TSingleton = class
         name: String;
         constructor create;
       end;
     
     implementation
     
     var
       Singleton: TSingleton = nil;
     
     constructor TSingleton.Create;
     begin
       if not(assigned(Singleton)) then begin
         inherited;
         (*... do initializations ...*)
         Singleton := self;
       end else begin
         self := Singleton;
       end;
     end;
     
     end.
    

In the _Singleton1_ unit, notice the following: 

  * The Singleton variable is defined in the **implementation** section. This makes the variable invisible outside the unit.
  * A public constructor is used.



Free Pascal doesn't allow for class variables and class properties. Only procedures and functions can be modified with the class keyword. Defining the 'static' variable in the implementation section is the closest we can get to a static variable. At initialization this variable is set to **nil** , so no memory is used yet. Although this implementation works, the `self := Singleton;` bugs me. I didn't try it, but I assume this allows for memory leaks. Even if it doesn't allow for memory leaks, it just doesn't look right. 

## Why It Is Different In Pascal

Then why not implement the singleton just like it is shown in the Pattern diagram? Well, here we go: 
    
    
    unit Singleton2;
     
     {$mode objfpc}{$H+}
     
     interface
     
     uses
       Classes, SysUtils;
     
     type
       TSingleton = class
         private
           constructor Create;
         public
           name: String;
           class function GetInstance : TSingleton;
       end;
     
     implementation
     var
       Singleton : TSingleton = nil; 
     
     constructor TSingleton.Create;
     begin
       inherited Create;
     end;
     
     class function TSingleton.GetInstance : TSingleton;
     begin
       if Singleton = nil then
         Singleton := TSingleton.Create;
       Result := Singleton;
     end;
     
     end.
    

The problem with the _Singleton2_ unit is that the program will work with the following client code: 
    
    
    program TrueSingleton;
     
     {$mode objfpc}{$H+}
     
     uses
       {$IFDEF UNIX}{$IFDEF UseCThreads}
       cthreads,
       {$ENDIF}{$ENDIF}
       Classes, Singleton2
       { you can add units after this };
     
     {$R TrueSingleton.res}
     var
       s1, s2: TSingleton;
     
     begin
       s1:= TSingleton.Create;
       s1.name := 'one';
       writeln('name of s1: '+s1.name);
     
       s2:= TSingleton.Create;
       s2.name := 'two';
     
       writeln('name of s1: '+s1.name);
       writeln('name of s2: '+s2.name);
       //writeln('name of singleton: ' + Singleton.name);
       readln;
     end.
    

It is difficult to see the problem. In the _Singleton2_ unit we made the constructor private. Run the client code, and the compiler complains about the constructor not being public, but after that the program works and shows that the code doesn't work as a singleton. Two different instances will be created. We learn from this that although we reduced the visibility of the constructor, the _TObject.Create_ constructor is still visible. 

## The 'constructor should be public' Singleton

A _small_ change makes a big difference: 
    
    
    unit Singleton3;
     
     {$mode objfpc}{$H+}
     
     interface
     
     uses
       Classes, SysUtils;
     
     type
       TSingleton = class
         private
           constructor Init;
         public
           name: String;
           class function Create: TSingleton;
       end;
     
     implementation
     var
       Singleton : TSingleton = nil;
     
     constructor TSingleton.Init;
     begin
       inherited Create;
     end;
     
     class function TSingleton.Create: TSingleton;
     begin
       if Singleton = nil then
         Singleton := TSingleton.Init;
       Result := Singleton;
     end;
     
     end.
    

The _Init_ constructor provides us with the constructor we need to avoid illegally hiding the TObject constructor. The _Init_ constructor is also private. The _Create_ **function** hides the _TObject.Create_ constructor. This code works. Using the above we have controlled access to the singleton. We also do not pollute the namespace with global variables. We can create additional methods as much as we want. Also, if we declare an array of singletons in the implementation section we can provide additional, but limited instances of the singleton. And, we do not need additional static methods other than the _Create_ function. All benefits of the singleton pattern are thus achieved. Typical client code now needs to look like the following: 
    
    
    program TrueSingleton;
     
     {$mode objfpc}{$H+}
     
     uses
       {$IFDEF UNIX}{$IFDEF UseCThreads}
       cthreads,
       {$ENDIF}{$ENDIF}
       Classes, Singleton3
       { you can add units after this };
     
     {$R TrueSingleton.res}
     var
       s1, s2: TSingleton;
     
     begin
       s1:= TSingleton.Create;
       s1.name := 'one';
       writeln('name of s1: '+s1.name);
     
       s2:= TSingleton.Create;
       s2.name := 'two';
     
       writeln('name of s1: '+s1.name);
       writeln('name of s2: '+s2.name);
       //writeln('name of singleton: ' + Singleton.name);
       readln;
     end.
    

Notice how this program uses the _Create_ method, not the _GetInstance_ method. But, look at the class definition again. _Create_ is not a constructor it is a class method that returns the singleton instance. _Init_ is the constructor. 

## Hide That For Me

When compiling, the compiler warns that the constructor is not public. So, now you are warned. You can choose to ignore the warning, because the code is really what you want. You can also suppress the warning. The message code for the 'Constructor should be public' warning is 3018. To suppress the warning add `-vm3018` to the command line of the compiler. In Lazarus you can deselect the message {From the **Project** menu, choose **Project Options**. Then, in the **Options for Project:** dialog, open the **Compiler Options** branch in the navigation pane and click **Messages**. Now deselect '(W)Constructor should be public' in the compiler messages list.} 

# Discussion Of The Options

In this part we will pay attention to some of the discussions. 

## Reducing the visibility of the constructor

Of course there is a reason why a constructor should be public. The Free Pascal compiler thus gives a warning when the constructor is made private. However, why does FreePascal give the warning? Other programming languages allow private constructors without a problem. Apart from that it is bad Pascal programming behavior, I don't know the answer. Maybe, since it is a warning, the reasoning may be that if the constructor is not public, then there is no way to create the object and what is the use of a class if you cannot make an object out of it? To sum things up, the argument is as follows: 
    
    
    The reason you can't lower the visibility of the constructor or any
    method for that matter... A class interface (implementation section) ,
    even for TObject as in your example, is a contract of how you can use
    that class and what is available to you.  By lowering the visibility
    of a method or constructor in a descendant class, you are breaking the
    contract previously defined. This is a BIG no-no, hence the reason it
    is not allowed.
    

## Design Time Help vs. Run Time Breaking

The following arguments may lead you to choose the Singleton3 unit as example implementation: 

  * The Singleton3 provides design time guidance. Possible alternatives that are more in the spirit of Pascal give run time break downs, or at best testing time messages. Design time guidance is preferred in my opinion.
  * If you want to avoid multiple instantiation of the class, then why not provide design time help to the client programmer.
  * In other programming languages it is not bad programming behavior, so FreePascal must have a very good reason to avoid it.
  * Reducing the visibility of any method of a class should not be allowed except for the constructor.



## The pattern is not followed

Yes, the pattern is not closely followed. The constructor is called _Init_ while in FreePascal there is the convention to call the constructor _Create_. Then on top of that name _Create_ is used for the static function instead of the name _GetInstance_. The following arguments are in favor of the most likable implementation: 

  * The intent of the pattern is followed and the benefits achieved.
  * The code is quite nice and well behaved. There are no run-time breaks built in and there are no doubtful constructs in the code.
  * Some compromises had to be made to make the singleton work in FreePascal.



## The implementations have a problem with multi-threading and synchronization

I never saw a good discussion about this, but in discussions about singletons in various programming languages programmers go far to make their singleton multi-threaded and synchronized and a number of other things. The singleton implementations given in this article are not optimized for that. 

## The singleton class implementation has a problem with platform independence

A singleton can never have descendants. What should happen when a descendant class calls **inherited Create**? Also it would be quite wrong to make the singleton descend from anything else but TObject. Now consider platform independence. Look at FreePascal sources where a global variable is used for a printer or whatever central resource. These global variables are by no exception made in the platform specific units, the shared functionality is written in a platform independent unit. Consider how this should be done with a singleton class. (I'll wait here until you find out that a hierarchy of singletons will inevitably cause a mess and will not allow for platform independent code.) Now that you are convinced we can easily say that the singleton class cannot be used with shared functionality, repeating singleton functionality for all platform implementations will cause a maintenance mess. 

# Conclusion

Yes, singletons are possible! Even in FreePascal. But, singletons are not a very good idea in platform independent FreePascal code. Whether you use a class or a global variable, there is no _perfect_ implementation of a singleton.

---

_Source: [https://wiki.freepascal.org/Singleton_Pattern](https://web.archive.org/web/20250122035531/https://wiki.freepascal.org/Singleton_Pattern)_
