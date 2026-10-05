# FPC PasCocoa

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Introduction
  * 2 PasCocoa wrappers
  * 3 Objective-Pascal FPC Compiler
  * 4 Syntax (implemented)
    * 4.1 Enabling Objective-C/Objective-Pascal
    * 4.2 Selectors
    * 4.3 Method declaration
    * 4.4 Class declaration
      * 4.4.1 Regular declaration/definition
      * 4.4.2 Formal declaration
      * 4.4.3 Objective-Pascal class references
    * 4.5 Protocol declaration
    * 4.6 Category declaration
    * 4.7 The _id_ type
    * 4.8 Fast enumeration
  * 5 Syntax (proposed)
  * 6 todo:
  * 7 Objective Pascal Examples
    * 7.1 Application with one menu item
    * 7.2 Application with a single window and no menus
    * 7.3 Application with a simple window and menus
    * 7.4 Simple window and a non-editable text field inside it
    * 7.5 Button which handles an event



## Introduction

A large and growing fraction of the macOS system frameworks are written in Objective-C and only offer an Objective-C interface. This makes it unwieldy to use them directly from other programming languages. The Objective-C run time library, which is used for everything from defining classes to sending messages (calling methods), has a public procedural C interface however, which makes it possible to integrate other languages directly with Objective-C code. Two general approaches exist. 

The first approach is to create a _bridge_. In this case calls to the Objective-C run time helpers are wrapped in host language constructs and helpers. The host language itself is generally not modified. A number of examples of Cocoa bridges can be found at <http://www.cocoadev.com/index.pl?CocoaBridges>. 

A second approach is to extend the host language so that it can directly interface with Objective-C code using new language constructs. A list of such language extensions can be found at <http://www.cocoadev.com/index.pl?CocoaLanguages>. As far as Pascal is concerned, a first proposal for Objective-Pascal was made by [MetroWerks in 1997](<http://www.pascal-central.com/cwpascal.html#Extensions%20providing%20compatibili>). Based on this preliminary proposal (it was never finalised nor implemented by MetroWerks) and on evolutions of the Object Pascal language since that time, we have designed our own Objective-Pascal dialect. 

## PasCocoa wrappers

Before the compiler supported Objective-Pascal (as described below), the primary way for Objective-C interfacing was through ObjFPC wrapper classes. This process is described on the [PasCocoa](<PasCocoa.md> "PasCocoa") page, and falls in the _bridge_ approach mentioned above. This approach is no longer recommended nor maintained. 

## Objective-Pascal FPC Compiler

All Objective-Pascal support is available in FPC 2.6.0 and later. 

For an overview of some conceptual differences between Objective-Pascal and Object Pascal on the one hand, and between Objective-Pascal and Objective-C on the other hand, see [FPC PasCocoa differences](<FPC_PasCocoa/Differences.md> "FPC PasCocoa/Differences"). 

## Syntax (implemented)

### Enabling Objective-C/Objective-Pascal

The compiler has mode switches to enable the use of Objective-C-related constructs. To enabled Objective-C 1.0 language features, either add _{$modeswitch objectivec1}_ to a source file, or using the _-Mobjectivec1_ command line switch. Similarly, for Objective-C 2.0 language features use _{$modeswitch objectivec2}_ resp. _-Mobjectivec2_ (this automatically also enables Objective-C 1.0 features). Note that in the latter case, the resulting program may only work on Leopard 10.5 and later. Due to these being mode switches rather than actual syntax modes, they can be used in combination with every general syntax mode (fpc, objfpc, tp, delphi, macpas). 

Note that mode switches are reset when the general syntax mode is changed using, e.g., _{$mode objfpc}_. So if you have such a statement in a unit, compiling it with _-Mobjectivec1_ will not change anything. You will have to add _{$modeswitch objectivec1}_ in the source code after the _{$mode objfpc}_ in that case. 

Whether or not the compiler has support for Objective-C-related constructs can be checked using _{$ifdef FPC_HAS_FEATURE_OBJECTIVEC1}_. There will be no separate switches when new Objective-C-related features are implemented. Since all this works happens in FPC trunk, simply expect that people are using the latest version available. 

### Selectors

To declare a selector for a method, use the _objcselector()_ statement. It returns a value of the type _objc.SEL_. It accepts one parameter, which can either be a constant string representing an Objective-C method name, or a method of an Objective-C class. 
    
    
    {$modeswitch objectivec1}
    var
      a: SEL;
    begin
      a:=objcselector('initiWithWidth:andHeight:');
      a:=objcselector('myMethod');
    end.
    

  


### Method declaration

In general methods are declared in the same way as in Object Pascal, with the exception that each objcclass method must also have a messaging name specified: 
    
    
    NSSomeObject = objcclass(NSObject)
      procedure method(params: Integer); message 'method:';
      class procedure classmethod(para: char); override; // "message 'classmethod:'" not required, compiler will get this from the parent class
    end;
    

Every objcclass/objcprotocol/objccategory method must be associated with an Objective-C message name, a.k.a. _selector_. This can happen in two different ways: 

  * an inherited method, an implemented protocol method or a method reintroduced by a category will automatically use the message name of the inherited/implemented/reintroduced method. If an explicit message name is specified, it must match the message name specified for the corresponding method.
  * in other cases, the _message_ keyword must be used, followed by a constant string that contains the selector name.



Instance and class methods correspond respectively to methods preceded with "-" and "+" in Objective-C. Furthermore, every method is by definition _virtual_ in Objective-C/Pascal. 

**Note:** it's common for Objective-C to start method names with a lower case character. 

### Class declaration

#### Regular declaration/definition

To declare an Objective-C class, use the keyword _objcclass_. Objective-C classes must be declared like regular Object Pascal types in a _type_ block: 
    
    
    type
      ObjCClassName =  objcclass [external [name 'ExternalClassName']] [(ObjCSuperClassName|ProtocolName [, ProtocolName, ProtocolName])] 
      [private, protected, public]
        [variables declarations]
        [method declarations]
    end;
    

  * _objcclass_ : defines an Objective-C class type declaration
  * _external_ : many Objective-C classes are implemented in external frameworks, such as Cocoa. The _external_ modifiers enables you to declare such external classes for use in Pascal code. It is possible to specify a different external name for the class than the identifier used in Pascal code, because the duplicate identifier rules in Objective-C are laxer than those of Pascal (e.g., there are both a class and a protocol called _NSObject_ in Cocoa, so one of the two has to be renamed in Pascal)
  * ObjCSuperClassName: defines parent (or super) class for the class. If name is not specified, the class defines a new root class. See [no unique root class](<FPC_PasCocoa/Differences.md> "FPC PasCocoa/Differences") for more information.



For example: 
    
    
    // NSView is an external class, declared in some external framework
    // External classes from Apple's framework
    // have all fields in private sections
    NSView = objcclass external (NSResponder)
    private
      _subview  : id; // private field
    public
      function initWithFrame(rect : NSRect): id; message 'initWithFrame:';
      procedure addSubview(aview: NSView); message 'addSubview:';
      procedure setAutoresizingMask(mask: NSUInteger); message 'setAutoresizingMask:';
      procedure setAutoresizesSubviews(flag: LongBool); message 'setAutoresizesSubviews:';
      procedure drawRect(dirtyRect: NSRect); message 'drawRect:';
    end;
    
    // MyView is a locally implemented class
    // that should be declared in the local unit
    // * drawRect_ method is overriden (the 'message' of the inherited method will be reused)
    // * customMessage_ is a newly added method
    // * data is a newly added field
    MyView = objcclass(MSView)
    public
      data : Integer;
      procedure customMessage(dirtyRect: NSRect); message 'customMessage';
      procedure drawRect(dirtyRect: NSRect); override;
    end;
    
    ... 
    procedure MyView.customMessage(dirtyRect: NSRect); 
    begin
    end;
    
    procedure MyView.drawRect(dirtyRect: NSRect); 
    begin
    end;
    

#### Formal declaration

In Objective-C, a class can also be declared formally using the _[@class SomeClass](<http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Chapters/ocDefiningClasses.html%23//apple_ref/doc/uid/TP30001163-CH12-SW1>)_ syntax. This is called a _forward declaration_ in Objective-C, but because this term has a different meaning in Pascal, we have termed this a _formal declaration_ in Objective-Pascal. 

A formal declaration of an _objcclass_ instructs the compiler to assume that somewhere, there is a complete definition of this _objcclass_ (including fields, methods, etc), but that it is not in this unit. The practical upshot is that it is possible to use such a formal _objcclass_ type for parameter types, field types, function result types and variable types, but that it is not possible to inherit from them, to send any messages to such instances, nor to access any fields (since the compiler does not know the full definition). 

The compiler will automatically resolve any uses of formal _objcclass_ es to the real declaration when this real declaration is also in scope. Hence, as soon as the unit containing the real declaration is added to the uses clause, the aforementioned restrictions no longer apply and the compiler will treat the occurrences of the formal _objcclass_ type the same as the actual type. 

To declare a _formal objcclass_ , use the following syntax: 
    
    
    type
      // note:
      //  * no inheritance is specified
      //  * the external name (if any) must match the external name of any real definition
      //    to which this formal definition resolves later on
      MyExternalClass = objcclass external;
    

Example: 
    
    
    unit ContainerClass;
    
    {$mode objfpc}
    {$modeswitch objectivec1}
    
    interface
    
    type
      // Formal declaration of MyItemClass, which is defined in another unit
      MyItemClass = objcclass external;
    
      MyContainerClass = objcclass
        private
         item: MyItemClass;
        public
         function getItem: MyItemClass; message 'getItem';
      end;
    
    implementation
    
      function MyContainerClass.getItem: MyItemClass;
        begin
           // we cannot send any message to item, but
           // we can use it in assignments
           result:=item;
        end;
    
    end.
    
    // ======================
    // ======================
    
    unit ItemClass;
    
    {$mode objfpc}
    {$modeswitch objectivec1}
    
    interface
    
    
    type
      // the actual definition of MyItemClass
      MyItemClass = objcclass(NSObject)
       private
        content : longint;
       public
         function initWithContent(c: longint): MyItemClass; message 'initWithContent:';
         function getContent: longint; message 'getContent';
      end;
    
    implementation
    
      function MyItemClass.initWithContent(c: longint): MyItemClass;
        begin
          content:=c;
          result:=self;
        end;
    
      function MyItemClass.getContent: longint;
        begin
          result:=content;
        end;
    
    end.
    
    // ======================
    // ======================
    
    Program test;
    
    {$mode objfpc}
    {$modeswitch objectivec1}
    
    // regardless of the order of the units in the uses-clause, as soon as
    // ItemClass is used then the full declaration of MyItemClass is in
    // scope and all uses of the formal declaration are replaced by the
    // actual declaration
    uses
      ItemClass, ContainerClass;
    
    var
      c: MyContainerClass;
      l: longint;
    begin
      c:=MyContainerClass.alloc.init;
     ...
      // even though MyContainerClass.getItem returns a "formal" MyItemClass,
      // the fact that the real declaration is in scope means that it can be
      // used as a regular MyItemClass instance
      l:=c.getItem.getContent;
    end.
    

#### Objective-Pascal class references

Just like in Object Pascal, it is possible to declare a class reference type for an Objective-Pascal class. In Objective-C, these are called meta-classes and are represented by the _pobjc_class_ type. If you wish to mix code using the _pobjc_class_ type and the Pascal syntax for class references, you have to use typecasts, as shown below. 
    
    
    {$mode objfpc}
    {$modeswitch objectivec1}
    
    program Test;
    
    type
      TMyClass = objcclass (NSObject)
      end;
      TMyClassMeta = class of TMyClass;  // declare the meta-class type for dynamic allocation
    
    var
      theClass: pobjc_class;
    begin
      theClass := TMyClass.classClass;      // get the meta-class reference for TMyClass; could 
      TMyClassMeta(theClass).alloc.init;    // cast theClass as TMyClassMeta and send messages as normal
    end.
    

### Protocol declaration

An Objective-C Protocol is pretty much the same as an _interface_ in Object Pascal: it lists a number of methods to be implemented by classes that conform to the protocol. The only difference is that protocol methods can be _optional_. Moreover, unlike in Object Pascal, a protocol can inherit from multiple other protocols: 
    
    
    type
      ObjCProtocolName =  objcprotocol [external [name 'ExternalClassName']] [(ObjCParentProtocol1[, ObjCParentProtocol2, ...])] 
       [required, optional]
        [method declarations]
    end;
    

  * _objcprotocol_ : defines an Objective-C protocol type declaration
  * _external_ : same as with objcclass
  * _ObjCParentProtocols_ : the protocols this protocol inherits from (so a class conforming to this protocol, will also have to implement all methods from those other protocols)
  * _required, optional_ : If nothing is specified, the default is _required_. _optional_ methods do not have to be implemented by classes conforming to this protocol.



Example: 
    
    
    type
      MyProtocol = objccprotocol
        // default is required
        procedure aRequiredMethod; message 'aRequiredMethod';
       optional
        procedure anOptionalMethodWithPara(para: longint); message 'anOptionalMethodWithPara:';
        procedure anotherOptionalMethod; message 'anotherOptionalMethod';
       required
        function aSecondRequiredMethod: longint; message 'aSecondRequiredMethod';
      end;
    
      MyClassImplementingProtocol = objcclass(NSObject,MyProtocol)
        // the "message 'xxx'" modifiers can be repeated, but this is not required
        procedure aRequiredMethod;
        procedure anOptionalMethodWithPara(para: longint);
        function aSecondRequiredMethod: longint;
      end;
    

Similar to _objcclass_ es, _objcprotocol_ s can be external in case they are implemented in e.g. the Cocoa framework. 

### Category declaration

An Objective-C category is similar to a _class helper_ in Object Pascal: it allows adding methods to an existing class, without inheriting from that class. In Objective-C, this functionality goes even further: multiple categories can be in scope at the same time for a particular class, and a category can also replace existing methods in another class rather than only add new ones. Since all methods are virtual in Objective-C, this also means that this method changes for all classes that inherit from the class in which the method was replaced (unless they override it). An Objective-C category can also implement protocols. 

Note that a category cannot extend another category. Furthermore, if two categories add/replace a method with the same selector in the same class, it is undefined which method will actually be called at run time. This would depend on the order in which the categories are registered with the Objective-C run time. 
    
    
    type
      ObjCCategoryName =  objccategory [external [name 'ExternalCategoryName']] (ExtendedObjCClass[,ObjCProtocol1, ObjCProtocol2, ...])
        [method declarations]
    end;
    

  * _objccategory_ : defines an Objective-C category type declaration
  * _external_ : same as with objcclass. As of Objective-C 2.0, the name of a category is optional. _Not yet implemented_ : to declare an external category without a name, use an empty string.
  * _method declarations_ : the methods that this category adds or replaces in the extended class. These can be both class and instance methods. Note that when replacing a method in the extended class, you must mark this new method as _reintroduce_. The reason that we do not use _override_ here, is that the method is really replaced in the original class. This means, e.g., that if you call _inherited_ from the new method, that you will call this method in the parent class of the extended class, and not the replaced method. The replaced method is "lost".
  * _ObjCParentProtocols_ : the protocols this category implements. The category will have to implement all required methods from these protocols.



Example: 
    
    
    type
      MyProtocol = objcprotocol
        procedure protocolmethod; message 'protocolmethod';
      end;
    
      MyCategory = objccategory(NSObject,MyProtocol)
       // method replaced in NSObject. This means that every objcclass that does not
       // override the hash function will now use this method instead. 
       function hash: cuint; reintroduce;
    
       // we have to implement this method, since we declare that we implement the
       // MyProtocol protocol. This method is added to NSObject afterwards.
       procedure protocolmethod;
    
       // a random class methods added to NSObject
       class procedure newmethod; message 'newmethod';
      end;
    

Similar to _objcclass_ es, _objccategory_ s can be external in case they are implemented in e.g. the Cocoa framework. 

### The _id_ type

The _id_ type is special in Objective-C/Pascal. It is assignment-compatible with instances of every objcclass and objcprotocol type, in two directions. This means that you can assign variables of any objcclass/objcprotocol type to a variable of the type _id_ , but also that you can assign these _id_ variables to variables of any particular objcclass/objcprotocol type. In neither case an explicit typecast is required. 

Furthermore, it is possible [to call any Objective-C method declared](<FPC_PasCocoa/Differences.md> "FPC PasCocoa/Differences") in an objcclass or objccategory that's in scope using an _id_ -typed variable. If, at run time, the objcclass instance stored in the _id_ -typed variable does not respond to the sent message, the program will terminate with a run time error. 

When there are multiple methods with the same Pascal identifier, the compiler will use the standard overload resolution logic to pick the most appropriate method. In this process, it will behave as if all objcclass/objccategory methods in scope have been declared as global procedures/functions with the _overload_ specifier. If the compiler cannot determine which overloaded method to call, it will print an error. If you compile with -vh, it will print a list of all methods that could be used to implement the call when it cannot determine the appropriate overloaded method. In this case, you will have to use an explicit type cast to clarify the class type from which you wish to call a method. 

### Fast enumeration

_Fast enumeration_ is a convention that enables enumerating the elements in a Cocoa container class in a generic yet fast way. The syntax used for this feature is the _for-in_ construct, both in Objective-C and in Objective-Pascal. This feature behaves identically in both languages. It requires the Objective-C 2.0 mode switch to be activated. 

Example: 
    
    
    {$mode delphi}
    {$modeswitch objectivec2}
    
    uses
      CocoaAll;
    
    var
      arr: NSMutableArray;
      element: NSString;
      pool: NSAutoreleasePool;
      i: longint;
    begin
      pool:=NSAutoreleasePool.alloc.init;
      arr:=NSMutableArray.arrayWithObjects(
        NSSTR('One'),
        NSSTR('Two'),
        NSSTR('Three'),
        NSSTR('Four'),
        NSSTR('Five'),
        NSSTR('Six'),
        NSSTR('Seven'),
        nil);
    
      i:=0;
      for element in arr do
        begin
          inc(i);
          if i=2 then
            continue;
          if i=5 then
            break;
          if i in [2,5..10] then
            halt(1);
          NSLog(NSSTR('element: %@'),element);
        end;
      pool.release;
    end.
    

## Syntax (proposed)

## todo:

a) how to declare a constant NSString/CFString in Pascal (the @"somestring" from Objective-C) 

\-- the dollar sign could work... ex. $theString 

## Objective Pascal Examples

### Application with one menu item

Please note, that for the creation of the menu, the application must be located in the application bundle. Lazarus may create the bundle for you. 
    
    
     program project1;
     
     {$mode objfpc}{$H+}
     {$modeswitch objectivec1}
      
     uses
       CocoaAll;
       
     type
       { MyCallback }
       MyCallback = objcclass(NSObject)
       public
         procedure haltMenuEvent(sender: id); message 'haltMenuEvent:';
       end;
     
     var
       pool     : NSAutoreleasePool;
       filemenu : NSMenu;
       subitem  : NSMenuItem;
       app      : NSApplication;
       callback : MyCallback;
      
     function NSStr(const s: AnsiString): NSString;
     begin
       if s='' then Result:=NSString.alloc.init
       else Result:=NSString.alloc.initWithCString(@s[1]);
     end;
     
     { MyCallback }
     
     procedure MyCallback.haltMenuEvent(sender:id);
     begin
       WriteLn('HALT!');
       app.stop(self);
     end;
     
     procedure initMenus;
     var
       root      : NSMenu;
       rootitem  : NSMenuItem;
     begin
       root:=NSMenu.alloc.init;
       rootitem:=root.addItemWithTitle_action_keyEquivalent(NSStr(''), nil, NSStr(''));
       filemenu:=NSMenu.alloc.initWithTitle(NSStr('File'));
       rootitem.setSubmenu(filemenu);
       rootitem.setEnabled(true);
       subitem:=filemenu.addItemWithTitle_action_keyEquivalent(NSStr('Halt'), nil, NSStr(''));
       subitem.setEnabled(true);
       app.setMainMenu(root);
     end;
     
     procedure InitCallback;
     begin
       callback:=MyCallback.alloc;
       subitem.setAction(ObjCSelector(callback.haltMenuEvent));
       subitem.setTarget(callback);
     end;
     
     begin
       pool:=NSAutoreleasePool.alloc.init;
       app:=NSApplication.sharedApplication;
       initMenus;
       initCallback;
       app.run;
       pool.release;
     end.
    

### Application with a single window and no menus
    
    
    program simplewindow;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    uses
      CocoaAll;
    
    var
      appName: NSString;
      window: NSWindow;
      pool: NSAutoreleasePool;
    begin
      pool := NSAutoreleasePool.new;
      NSApp := NSApplication.sharedApplication;
      NSApp.setActivationPolicy(NSApplicationActivationPolicyRegular);
      appName := NSProcessInfo.processInfo.processName;
      window := NSWindow.alloc.initWithContentRect_styleMask_backing_defer(NSMakeRect(0, 0, 200, 200),
        NSTitledWindowMask, NSBackingStoreBuffered, False).autorelease;
      window.center;
      window.setTitle(appName);
      window.makeKeyAndOrderFront(nil);
      NSApp.activateIgnoringOtherApps(true);
      NSApp.run;
    end.
    

### Application with a simple window and menus
    
    
    program simplewindow;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    uses
      CocoaAll;
    
    var
      menubar: NSMenu;
      appMenu: NSMenu;
      appName: NSString;
      quitMenuItem: NSMenuItem;
      appMenuItem: NSMenuItem;
      quitTitle: NSString;
      window: NSWindow;
      pool: NSAutoreleasePool;
    begin
      pool := NSAutoreleasePool.new;
      NSApp := NSApplication.sharedApplication;
      NSApp.setActivationPolicy(NSApplicationActivationPolicyRegular);
      menubar := NSMenu.alloc.autorelease;
      appMenuItem := NSMenuItem.alloc.autorelease;
      menubar.addItem(appMenuItem);
      NSApp.setMainMenu(Menubar);
      appMenu := NSMenu.new.autorelease;
      appName := NSProcessInfo.processInfo.processName;
      quitTitle := NSSTR('Quit ').stringByAppendingString(appName);
      quitMenuItem := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(quitTitle,
      sel_registerName(PChar('terminate:')),
      NSSTR('q'));
      appMenu.addItem(quitMenuItem);
      appMenuitem.setSubmenu(appMenu);
      window := NSWindow.alloc.initWithContentRect_styleMask_backing_defer(NSMakeRect(0, 0, 200, 200),
        NSTitledWindowMask, NSBackingStoreBuffered, False).autorelease;
      window.center;
      window.setTitle(appName);
      window.makeKeyAndOrderFront(nil);
      NSApp.activateIgnoringOtherApps(true);
      NSApp.run;
    end.
    

### Simple window and a non-editable text field inside it

This shows how to create and position a sub-control in a window: 
    
    
    program simplewindow;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    uses
      CocoaAll;
    
    var
      appName: NSString;
      window: NSWindow;
      pool: NSAutoreleasePool;
      lText: NSTextField;
    begin
      // Autorelease pool, app and window creation
      pool := NSAutoreleasePool.new;
      NSApp := NSApplication.sharedApplication;
      NSApp.setActivationPolicy(NSApplicationActivationPolicyRegular);
      appName := NSProcessInfo.processInfo.processName;
      window := NSWindow.alloc.initWithContentRect_styleMask_backing_defer(NSMakeRect(0, 0, 200, 200),
        NSTitledWindowMask or NSClosableWindowMask or NSMiniaturizableWindowMask,
        NSBackingStoreBuffered, False).autorelease;
    
      // text label
      lText := NSTextField.alloc.initWithFrame(NSMakeRect(50, 50, 200, 50)).autorelease;
      lText.setBezeled(False);
      lText.setDrawsBackground(False);
      lText.setEditable(False);
      lText.setSelectable(False);
      lText.setStringValue(NSSTR('NSTextField'));
      window.contentView.addSubview(lText);
    
      // Window showing and app running
      window.center;
      window.setTitle(appName);
      window.makeKeyAndOrderFront(nil);
      NSApp.activateIgnoringOtherApps(true);
      NSApp.run;
    end.
    

### Button which handles an event

This event shows how to make a button that responds to clicking on it. 
    
    
    program simplewindow;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    uses
      CocoaAll;
    
    type
      { TMyDelegate }
      TMyDelegate = objcclass(NSObject)
      public
        procedure HandleButtonClick(sender: id); message 'HandleButtonClick:';
      end;
    
    var
      appName: NSString;
      window: NSWindow;
      pool: NSAutoreleasePool;
      lText: NSTextField;
      lButton: NSButton;
      lDelegate: TMyDelegate;
    
    procedure TMyDelegate.HandleButtonClick(sender: id);
    begin
      lText.setStringValue(NSSTR('New text!'));
    end;
    
    begin
      // Autorelease pool, app and window creation
      pool := NSAutoreleasePool.new;
      NSApp := NSApplication.sharedApplication;
      NSApp.setActivationPolicy(NSApplicationActivationPolicyRegular);
      appName := NSProcessInfo.processInfo.processName;
      window := NSWindow.alloc.initWithContentRect_styleMask_backing_defer(NSMakeRect(0, 0, 200, 200),
        NSTitledWindowMask or NSClosableWindowMask or NSMiniaturizableWindowMask,
        NSBackingStoreBuffered, False).autorelease;
    
      // text label
      lText := NSTextField.alloc.initWithFrame(NSMakeRect(50, 50, 120, 50)).autorelease;
      lText.setBezeled(False);
      lText.setDrawsBackground(False);
      lText.setEditable(False);
      lText.setSelectable(False);
      lText.setStringValue(NSSTR('NSTextField'));
      window.contentView.addSubview(lText);
    
      // button
      lButton := NSButton.alloc.initWithFrame(NSMakeRect(50, 100, 120, 50)).autorelease;
      window.contentView.addSubview(lButton);
      lButton.setTitle(NSSTR('Button title!'));
      lButton.setButtonType(NSMomentaryLightButton);
      lButton.setBezelStyle(NSRoundedBezelStyle);
    
      // button event handler setting
      lDelegate := TMyDelegate.alloc.init;
      lButton.setTarget(lDelegate);
      lButton.setAction(ObjCSelector(lDelegate.HandleButtonClick));
    
      // Window showing and app running
      window.center;
      window.setTitle(appName);
      window.makeKeyAndOrderFront(nil);
      NSApp.activateIgnoringOtherApps(true);
      NSApp.run;
    end.

---

_Source: [https://wiki.freepascal.org/FPC_PasCocoa](https://web.archive.org/web/20250326043631/https://wiki.freepascal.org/FPC_PasCocoa)_
