# Cocoa Internals/Memory Management

All Objective-C objects are reference counted objects. Once the count reaches zero - the object is freed. 

The modern Objective-C (or Swift) recommends (strongly, fiercely) to use Automatic-Reference-Counting (ARC) support. FPC support for Objective-C was started way before ARC became highly recommended and was part of the language. Right now Objective-C 2.0 syntax provides the language level support for ARC, while FPC doesn't have those (yet?). All Cocoa Widgetset code cannot doesn't rely on ARC and using reference and dereference manually. 

## Contents

  * 1 Memory Management
    * 1.1 Leaking
    * 1.2 Autorelease Pools
    * 1.3 Leaking out of the pool
    * 1.4 Multiple pools
    * 1.5 Leaking with Pools because of too many Retains
    * 1.6 App Event Loop and AutoreleasePools
    * 1.7 Object Ownership
      * 1.7.1 Ownership Violation
      * 1.7.2 Using NSZombies
  * 2 ObjC Object Leaks
    * 2.1 Xcode Instruments
  * 3 LCL Handles
  * 4 See Also



## Memory Management

As mentioned earlier - all Cocoa objects are reference counted objects. They should be allocated via a class constructor (typically alloc()) and be released via release() method (rather than calling a "destructor" dealloc). 

Consider the following code example: 

A new sub-class of NSObject is declared, overriding its alloc (constructor) and dealloc (destructor) methods. Overridden methods are providing a logging output. 
    
    
    program project1;
    
    {$mode delphi}{$H+}
    {$modeswitch objectivec1}
    {$modeswitch objectivec2}
    
    uses CocoaAll;
    
    type
    
      { NSMyObject }
    
      NSMyObject = objcclass(NSObject)
      public
        class function alloc: id; override;
        procedure dealloc; override;
      end;
    
    { NSMyObject }
    
    class function NSMyObject.alloc: id;
    begin
      Result:=inherited alloc;
      writeln('alloc:   ', PtrUInt(Result));
    end;
    
    procedure NSMyObject.dealloc;
    begin
      writeln('dealloc: ', PtrUInt(Self));
      inherited dealloc;
    end;
    
    procedure MakeObject;
    var
      obj : NSMyObject;
    begin
      obj:=NSMyObject.alloc.init;
      obj.release;
    end;
    
    begin
      MakeObject;
      MakeObject;
      MakeObject;
    end.
    

The output of calling such process would be: 
    
    
    alloc:   3206800
    dealloc: 3206800
    alloc:   3206800
    dealloc: 3206800
    alloc:   3206800
    dealloc: 3206800
    

This is happening, because whenever an object is allocated, it's reference count is set to 1. Calling release method reduced the reference counter by 1. If reference count becomes zero, objc runtime - is calling objects destructor - dealloc. 

### Leaking

If MakeObject procedure is modified as following: 
    
    
    procedure MakeObject;
    var
      obj : NSMyObject;
    begin
      obj:=NSMyObject.alloc.init;
      // removed the line with .release call
    end;
    

the output of the application changes to: 
    
    
    alloc:   2146560 
    alloc:   2146288 
    alloc:   2146304
    

That looks like a memory leak via ObjC objects, because allocated objects were not released. 

Preventing a leak requires a proper release of an object. 

### Autorelease Pools

In order to decrease a complexity of tracking of object life-span and releasing, Cocoa provides an autorelease mechanism. Any object registered in with an autorelease pool, would be released at the time autorelease pool is drained and the object's reference count is equal to 1. 

(Autorelease pools are drained whenever they're released). 

Let's modify the example above in the following manner: 
    
    
    procedure MakeObject;
    var
      obj : NSMyObject;
    begin
      obj:=NSMyObject.alloc.init.autorelease; // registering the allocated object with autorelease pool
    end;
    
    var
      pool : NSAutoreleasePool;
    begin
      pool := NSAutoreleasePool.alloc.init;
      MakeObject;
      MakeObject;
      MakeObject;
      writeln('releasing the pool');
      pool.release;
      writeln('done');
    end.
    

The output of the program is as following: 
    
    
    alloc:   3175888
    alloc:   3172352
    alloc:   3172368 
    releasing the pool
    dealloc: 3172368 
    dealloc: 3172352
    dealloc: 3175888
    done
    

All objects were released together with release of the pool. 

### Leaking out of the pool

Note that any time a pool is created it's placed on the "stack" of the pools and become a current pool. Any objects allocated before the pool, would not be released with the pool itself. 
    
    
    var
      pool : NSAutoreleasePool;
    begin
      MakeObject;
      pool := NSAutoreleasePool.alloc.init;
      MakeObject;
      MakeObject;
      writeln('releasing the pool');
      pool.release;
      writeln('done');
    end.
    

output: 
    
    
    alloc:   4199616
    alloc:   4202144 
    alloc:   4202160
    releasing the pool
    dealloc: 4202160 
    dealloc: 4202144
    done
    

As you can see only two later objects were released. The first object remained in the memory. 

### Multiple pools

It's expected that multiple pools would could be used. The typical example of is to have an additional pool for a routine where a great number of temporary objects could be used. 

In the end the memory would be cleaned up, together with the local pool released: 
    
    
    procedure ExtraProc;
    var
      pool : NSAutoreleasePool;
    begin
      pool := NSAutoreleasePool.alloc.init;
      try
        MakeObject;
        MakeObject;
      finally
        pool.release;
      end;
    end;
    
    var
      pool : NSAutoreleasePool;
    begin
      pool := NSAutoreleasePool.alloc.init;
      MakeObject;
      writeln('calling ExtraProc()');
      ExtraProc();
      writeln('ExtraProc() done');
      pool.release;
    end.
    

output: 
    
    
    alloc:   3155200
    calling ExtraProc()
    alloc:   3155024
    alloc:   3155248
    dealloc: 3155248
    dealloc: 3155024
    ExtraProc() done
    dealloc: 3155200
    

Objects allocated within ExtraProc were released within ExtraProc. Object created outside of extra proc was released with its own "global" autorelease pool. 

### Leaking with Pools because of too many Retains

Note, that an object would be released with autorelease pool only, if it's reference count reached 1. (when draining a pool would simply decrease the reference count, and if it drops to 0 the object is released). 

Modifying ExtraProc from the example above as following: 
    
    
    procedure ExtraProc;
    var
      pool : NSAutoreleasePool;
      obj  : NSMyObject;
    begin
      pool := NSAutoreleasePool.alloc.init;
      try
        MakeObject;
        obj:=NSMyObject.alloc.init.autorelease;  // the same allocation code as in MakeObject proc
        writeln('ref count = ', obj.retainCount);
        obj.retain; 
        writeln('ref count = ', obj.retainCount);
      finally
        pool.release;
        writeln('ref count = ', obj.retainCount);
      end;
    end;
    

the output of the program would be: 
    
    
     alloc:   4202000
     calling ExtraProc()
     alloc:   4201824    ; allocating the first object in ExtraProc 
     alloc:   4202048    ; allocating the second object in ExtraProc
     ref count = 1       ; the reference count after allocation
     ref count = 2       ; manually increasing the reference count by calling .retain
     dealloc: 4201824    ; on release of the pool, the first object with reference count was removed. 
     ref count = 1       ; the second object had its reference count decreased to 1 and leaked 
     ExtraProc() done
     dealloc: 4202000
    

**Note** that "global" pool also has no effect on the leaked object. So after the global pool released, the leaked object remained. The leaked object attached to "local" pool that was in current at that time. 

### App Event Loop and AutoreleasePools

Every event (i.e. mouse press, key press, draw, etc) handling is enclosed in it's own AutoreleasePool. The pool is released once the event has been processed. 

Cocoa Widgetset implements its own event queue, but the same rule remains enforced. 

Make sure that an object that should stay alive after the event is not autoreleased. 

### Object Ownership

As [Apple documentation](<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmRules.html#//apple_ref/doc/uid/20000994-SW1>) suggests: 

The memory management model is based on object ownership. Any object may have one or more owners. As long as an object has at least one owner, it continues to exist. If an object has no owners, the runtime system destroys it automatically. To make sure it is clear when you own an object and when you do not, Cocoa sets the following policy: 

  * **You own any object you create**



    You create an object using a method whose name begins with “alloc”, “new”, “copy”, or “mutableCopy” (for example, alloc, newObject, or mutableCopy).

  * **You can take ownership of an object using retain**



    A received object is normally guaranteed to remain valid within the method it was received in, and that method may also safely return the object to its invoker. You use retain in two situations: (1) In the implementation of an accessor method or an init method, to take ownership of an object you want to store as a property value; and (2) To prevent an object from being invalidated as a side-effect of some other operation (as explained in Avoid Causing Deallocation of Objects You’re Using).

  * **When you no longer need it, you must relinquish ownership of an object you own**



    You relinquish ownership of an object by sending it a release message or an autorelease message. In Cocoa terminology, relinquishing ownership of an object is therefore typically referred to as “releasing” an object.

  * **You must not relinquish ownership of an object you do not own**



    This is just corollary of the previous policy rules, stated explicitly.

In practice, not following the serules, might cause unexpected errors in run-time. 

#### Ownership Violation

Consider this following: 
    
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    uses CocoaAll;
    var
      p : NSIndexSet;
    begin
      p:=NSIndexSet.indexSetWithIndex(10);
      writeln('idx=',p.firstIndex,' refCount=',p.retainCount);
      p.release; // a potential error
    end.
    

If you try to execute the code, it would act as expected and produce no problems. 

However, there's an error. The object was created using "indexSetWithindex" method. 

The last line attempts to .release() the object. That violates the ownership rule, where the object is owned only if "alloc"/"new" or "copy" methods were used. 

The run-time error will show up, if you try to put this code into Autorelease pool: 
    
    
    var
      p : NSIndexSet;
      pool : NSAutoreleasePool;
    begin
      pool := NSAutoreleasePool.alloc.init;
      p:=NSIndexSet.indexSetWithIndex(10);
      writeln('idx=',p.firstIndex,' refCount=',p.retainCount);
      p.release;
      pool.release; // error here: *** error for object 0x308a70: pointer being freed was not allocated
    end.
    

The reason behind it, is that "indexSetWithIndex" did allocate an object of NSIndexSet, however it also autorelease-d object. (Notably, why Cocoa internally is using AutoreleasePools often). 

There are two ways to fix the error: 

1) either alloc.initXXX pair should be used, that guaranteers the ownership of the object 
    
    
    var
      p : NSIndexSet;
      pool : NSAutoreleasePool;
    begin
      pool := NSAutoreleasePool.alloc.init;
      p:=NSIndexSet.alloc.initWithIndex(10);
      writeln('idx=',p.firstIndex,' refCount=',p.retainCount);
      p.release;
      pool.release;
    end.
    

2) or the object should not be released explicitly 
    
    
    var
      p : NSIndexSet;
      pool : NSAutoreleasePool;
    begin
      pool := NSAutoreleasePool.alloc.init;
      p:=NSIndexSet.indexSetWithIndex(10);
      writeln('idx=',p.firstIndex,' refCount=',p.retainCount);
      pool.release;
    end.
    

The actual approach depends on how NSIndexSet needs to be used in the code. 

#### Using NSZombies

In the example below, the error typed into stderr is not very informative. 
    
    
    error here: *** error for object 0xNNNNNNNN: pointer being freed was not allocated
    

When there's potentially a great number of objects/code that could cause a double-release, it might be handy to use NSZombies. 

NSZombies are the special debugging mode in Cocoa library. Whenever an object is released, it's not actually being released, but instead is released by NSZombie object. If NSZombie object is released again, then it would produce an error message to stderr, to warn about the double release. 

In order to enable NSZombies an environmental variable NSZombiesEnable must be set to "YES" (no quotes) declared at the time application starts. 

Here's an example: 
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    uses
      CocoaAll,SysUtils, Classes;
    
    type
      NSMyObject = objcclass(NSObject);
    
    var
      m: NSMyObject;
    begin
      m:=NSMyObject.alloc.init;
      m.release;
      m.release;
    end.
    

In order to have the zombies enabled you might want to run the application from Terminal using the following commands: 
    
    
    export NSZombieEnabled="YES"
    ./project1
    

(export command could be ran once per Terminal tab. It doesn't affect other opened terminal tabs. Any new Terminal tab or window would require to run the command again, if you need to enable zombies) 

when application fails, it would produce the following error message: 
    
    
    *** -[NSMyObject release]: message sent to deallocated instance 0x606d20
    

If you try to run the faulty code from ownership violation the error you'd see would be like this: 
    
    
    *** -[NSIndexSet release]: message sent to deallocated instance 0x605140
    

indicating that NSIndexSet was improperly released. 

While NSZombies is very handy way of resolving problems, such as incorrect and double releases, they should be treated carefully. NSZombies in their nature are memory leaks, because the objects are never released, but rather replaced by the zombie's code. Thus NSZombies should never be used with memory leaks checks, as they would produce false positive results. If there are many objects allocation and releases, the application memory consumption will grow, that might also impact performance. 

## ObjC Object Leaks

Free Pascal debugging unit [heaptrc](<../heaptrc.md> "heaptrc") is only able to track leaking of memory allocated via Pascal Run-time memory management. It's not able to track allocations made via C-runtime/Obj-C memory manager. 

Apple provides external tools in its Xcode Instruments to keep track of ObjC allocations. 

### Xcode Instruments

The Xcode Instruments Allocations profiling template uses the Allocations and VM Tracker instruments to measure general and virtual memory usage in your app. However, to track down abandoned memory that’s been allocated but isn’t needed again, focus strictly on the Allocations instrument. This instrument measures heap memory usage and tracks allocations, including specific object allocations by class. 

  * See further [Find abandoned memory](<https://help.apple.com/instruments/mac/current/#/dev7b8f6eb6>).



The Xcode Instruments Leaks profiling template uses the Allocations and Leaks instruments to measure general memory usage in your app and check for leaks — memory that has been allocated to objects that are no longer referenced and reachable. 

  * See further [Find memory leaks](<https://help.apple.com/instruments/mac/current/#/dev022f987b>).



## LCL Handles

Whenever CreatingHandle in Cocoa widgetset at least 1 NSView (or NSControl) is allocated. (Allocation makes a control with 1 reference count) 

The view is added to parent view by calling addSubView. By Cocoa standards, adding a view to subview adds another reference count, and object can be released right after the call. LCL Widgetset code SHOULD NOT release immediately, instead the explicit .release call would be called during DestroyHandle. 

Any additional controls should also be released during DestroyHandle. It's not predictable when the actual "dealloc" would be called. It might be called right away, or at the end of the event processing, with NSAutorelease pool, or whenever the hosting NSWindow decides to cleanup. It's safer to release as much memory as possible at DestroyHandle. 

## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")
  * [Advanced Memory Management](<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html>) \- at Apple Developer

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Memory_Management](https://web.archive.org/web/20240909154648/https://wiki.freepascal.org/Cocoa_Internals/Memory_Management)_
