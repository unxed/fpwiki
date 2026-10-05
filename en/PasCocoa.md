# PasCocoa

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The information on this page is legacy and only available for historical purposes. Please see [FPC PasCocoa](<FPC_PasCocoa.md> "FPC PasCocoa") for current information.

  


## Contents

  * 1 Objective-C to Pascal Bindings
    * 1.1 Objective-C 1.0 Bindings
    * 1.2 Objective-C 2.0 Bindings
  * 2 Example
  * 3 Subversion
  * 4 Documentation
    * 4.1 Creating a class which inherits from a Cocoa class
    * 4.2 Registering Pascal methods as Objective-c methods
    * 4.3 Getting objects from Cocoa classes
    * 4.4 Using Selectors with the Target/Action Paradigm
  * 5 Roadmap
    * 5.1 General Roadmap
    * 5.2 Foundation Classes Roadmap
    * 5.3 AppKit Classes Roadmap
  * 6 Implementation Details
    * 6.1 Overview
    * 6.2 HEADERS
    * 6.3 CLASSES
    * 6.4 IMPLEMENTATION
    * 6.5 Type Encodings
  * 7 Notes
    * 7.1 Objects allocation
    * 7.2 Categories
    * 7.3 Carbon-Cocoa integration
    * 7.4 Unnamed enums
  * 8 Cocoa related external links
  * 9 Articles about PasCocoa



**PasCocoa** is the project to build object oriented bindings to use Cocoa in Pascal. 

## Objective-C to Pascal Bindings

There are 2 main versions of Objective-C, and thus of Objective-C headers: 1.0 and 2.0 

Version 2.0 is only available on Leopard 10.5 or later, making it too restrictive at the moment, and thus the available headers are a translation of 1.0 headers from a Tiger 10.4.9 machine. 

### Objective-C 1.0 Bindings

  * Reference page: <http://developer.apple.com/documentation/Cocoa/Reference/ObjCRuntimeRef1/Reference/reference.html>


  * Available on: 10.0 and later



### Objective-C 2.0 Bindings

  * Reference Pages: 
    * Main Page: <http://developer.apple.com/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html>
    * Type Encodings: <http://developer.apple.com/documentation/Cocoa/Conceptual/ObjectiveC/Articles/chapter_14_section_9.html>
  * Available on: Leopard 10.5 and later



## Example
    
    
    {
     simpleform.pas
    
     This example shows how to use the PasCocoa bindings to create a
     NSAutoreleasePool, initialize the application global variable, create
     a simple window without contents and attach a close handler to it that
     exits the application.
    
     Compilation of this example requires the following options:
     -k-framework -kcocoa -k-lobjc
    
     This example project is released under public domain
    
     AUTHORS: Felipe Monteiro de Carvalho
    }
    program simplewindow;
    
    {$ifdef fpc}{$mode delphi}{$endif}
    
    uses
      objc, ctypes, FPCMacOSAll, AppKit, Foundation;
    
    const
      Str_Window_Title = 'This is the title';
      Str_Window_Message = 'This is the message';
    var
      { classes }
      pool: NSAutoreleasePool;
      MainWindow: NSWindow;
      MainWindowView: NSView;
      TextField: NSTextField;
      { strings }
      CFTitle, CFMessage: CFStringRef;
      { sizes }
      MainWindowRect, TextFieldRect: NSRect;
    begin
      {  Creates a AutoreleasePool for this thread. Every thread must have one }
      pool := NSAutoreleasePool.Create;
    
      { Creates the application NSApp object }
      NSApp := NSApplication.sharedApplication;
    
      { Creates a simple window }
      MainWindowRect.origin.x := 300.0;
      MainWindowRect.origin.y := 300.0;
      MainWindowRect.size.width := 300.0;
      MainWindowRect.size.height := 500.0;
    
      MainWindow := NSWindow.initWithContentRect(MainWindowRect,
        NSTitledWindowMask or NSClosableWindowMask or NSMiniaturizableWindowMask or NSResizableWindowMask,
        NSBackingStoreBuffered, NO);
    
      { Initializes the title of the window }
    
      CFTitle := CFStringCreateWithPascalString(nil, Str_Window_Title, kCFStringEncodingUTF8);
      MainWindow.setTitle(CFTitle);
    
      { Adds a NSTextField with a string }
      
      CFMessage := CFStringCreateWithPascalString(nil, Str_Window_Message, kCFStringEncodingUTF8);
      TextFieldRect.origin.x := 0.0;
      TextFieldRect.origin.y := 200.0;
      TextFieldRect.size.width := 300.0;
      TextFieldRect.size.height := 100.0;
      TextField := NSTextField.initWithFrame(TextFieldRect);
      TextField.setStringValue(CFMessage);
      MainWindowView := NSView.CreateWithHandle(MainWindow.contentView);
      MainWindowView.addSubview(TextField);
    
      { Put's the window on the front z-order }
    
      MainWindow.orderFrontRegardless;
    
      { Enters main message loop }
    
      NSApp.run;
    
      { Releases the AutoreleasePool for this thread }
      pool.Free;
    end.
    

## Subversion

New repository: 
    
    
    svn co http://svn.freepascal.org/svn/lazarus/trunk/lcl/interfaces/carbon/objc objc
    svn co http://svn.freepascal.org/svn/lazarus/trunk/lcl/interfaces/carbon/pascocoa pascocoa
    

Old repository: 
    
    
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/bindings/objc objc
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/bindings/pascocoa pascocoa
    

## Documentation

### Creating a class which inherits from a Cocoa class

To create a class which inherits from a Cocoa class, one needs to create a normal class which inherits from any of the classes on the PasCocoa bindings and use the NSObject.CreateClassDefinition method to register that class on the Objective-C runtime before calling the NSObject constructor. 

Example: 
    
    
    uses foundation;
    
    type
    
      { TMyController }
    
      TMyController = class(NSObject)
      public
        { Extra binding functions }
        constructor Create; override;
        function getClass: objc.id; override;
        ...
      end;
    
    const
      Str_TMyController = 'TMyController';
      ...
    
    implementation
    
    constructor TMyController.Create;
    begin
      { The class is registered on the Objective-C runtime before the NSObject constructor is called }
      if not CreateClassDefinition(Str_TMyController, Str_NSObject) then WriteLn('Failed to create objc class');
    
      inherited Create;
    
      { Insert other desired initialization here }
    end;
    
    { Called by the NSObject constructor to get the definition of our class.
    
      At this point the class is simply identifyed by its name and must be already registered
      on the Objective-C runtime. }
    function TMyController.getClass: objc.id;
    begin
      Result := objc_getClass(Str_TMyController);
    end;
    

### Registering Pascal methods as Objective-c methods

Pascal methods aren't automatically available to Objective-C. They need to be first registered on the Objective-C runtime by overriding the AddMethods method and calling NSObject.AddMethod method for every method to be registered. This is very common practice for assigning delegate and controller methods in Cocoa. 

When calling NSObject.AddMethod you must convert the parameter types to single characters, each representing a specific type (table can be found below). The process contains 3 steps: 

  * Locate the Objective-C method you wish to override in the Cocoa headers (it may not exist in the Pascal interfaces).
  * Make the Pascal wrapper method which conforms to the Objective-C syntax. Note the first 2 paratemters are always "_self: objc.id; _cmd: SEL".
  * Add method to Objective-C runtime using NSObject.AddMethod and convert the parameter types to single character representation.



Example for outlineView:isItemExpandable: method of NSOutlineView: 

  * \- (BOOL)outlineView:(NSOutlineView *)outlineView isItemExpandable:(id)item;
  * function isItemExpandable (_self: objc.id; _cmd: SEL; item: objc.id): BOOL; cdecl;
  * AddMethod('outlineView:isItemExpandable:', 'B@:@', Pointer(isItemExpandable));



Note the string "B@:@" which is the parameters of the method converted to single characters as explained in the table below. 

Full Example: 
    
    
    type
      TMyController = class(NSObject)
      public
        { Extra binding functions }
        constructor Create; override;
        procedure AddMethods; override;
        { Objective-c Methods }
        class procedure doClose(_self: objc.id; _cmd: SEL; sender: objc.id); cdecl; {$ifndef VER2_2_2}static;{$endif}
        class function  applicationShouldTerminateAfterLastWindowClosed(_self: objc.id;
         _cmd: SEL; theApplication: objc.id): cbool; cdecl; {$ifndef VER2_2_2}static;{$endif}
      end;
    
    implementation
    
    procedure TMyController.AddMethods;
    begin
      AddMethod('doClose:', 'v@:@', Pointer(doClose));
      AddMethod('applicationShouldTerminateAfterLastWindowClosed:', 'B@:@',
       Pointer(applicationShouldTerminateAfterLastWindowClosed));
    end;
    
    constructor TMyController.Create;
    begin
      { The class is registered on the Objective-C runtime before the NSObject constructor is called }
      if not CreateClassDefinition(ClassName(), Str_NSObject) then WriteLn('Failed to create objc class ' + ClassName());
    
      inherited Create;
    end;
    
    { Objective-c Methods }
    
    class procedure TMyController.doClose(_self: objc.id; _cmd: SEL; sender: objc.id); cdecl;
    begin
     {close the window}
    end;
    
    class function TMyController.applicationShouldTerminateAfterLastWindowClosed(_self: objc.id;
     _cmd: SEL; theApplication: objc.id): cbool; cdecl;
    begin
      Result := objc.YES;
    end;
    

  


### Getting objects from Cocoa classes

Cocoa classes often contain methods which return an object which was allocated internally for use by that class. For example NSTextView.textStorage will return the NSTextStorage object which the NSTextView uses internally to manage the text. However the object returned belongs to Objective-C and therefore you must explicitly create a wrapper (which you must eventually release). 

Example: 
    
    
    var
      textView: NSTextView;
      textStorage: NSTextStorage;
      bounds: NSRect;
    
    textView := NSTextView.initWithFrame(bounds);
    textView.insertText(CFSTR('Foo'));
    
    textStorage := NSTextStorage.CreateWithHandle(textView.textStorage);
    writeln(textStorage.length);
    textStorage.Free;
    

### Using Selectors with the Target/Action Paradigm

Selectors are used to represent a message in Objective-C and can be used in conjunction with a class (the target) to produce an "action". However, with the PasCocoa you must first create a NSObject class to handle the selector and register a method to accept it. 

For example we create the controller class TMyController and register the handleButtonAction method along with our Pascal implementation. 
    
    
    type
     TMyController = class (NSObject)
     
      class procedure handleButtonAction (_self: objc.id; _cmd: objc.SEL; sender: objc.id); cdecl;
      
      procedure AddMethods; override;
      constructor Create; override;
     end;
    
    class procedure TMyController.handleButtonAction (_self: objc.id; _cmd: objc.SEL; sender: objc.id); cdecl;
    begin
     writeln('Hello World');
    end;
    
    procedure TMyController.AddMethods;
    begin
     AddMethod('myController:handleButtonAction:', 'v@:@', Pointer(handleButtonAction));
    end;
    
    constructor TMyController.Create;
    begin
     if not CreateClassDefinition(ClassName, 'NSObject') then
      writeln('Failed to create class ' + ClassName);
    
     inherited Create;
    end;
    

Next we need to create the class and a test subject: NSButton. 
    
    
    var
     Controller: TMyController;
     Button: NSButton;
     
     {create the NSObject controller class}
     Controller := TMyController.Create;
     
     {create a new NSButton}
     CFButtonText := CFStringCreateWithPascalString(nil, 'Hello World', kCFStringEncodingUTF8);
     ButtonRect.origin.x := 30;
     ButtonRect.origin.y := 30;
     ButtonRect.size.width := 128;
     ButtonRect.size.height := 25;
     Button := NSButton.initWithFrame(ButtonRect);
     Button.setTitle(CFButtonText);
     Button.setBezelStyle(NSRoundedBezelStyle);
    

Finally we can set the buttons target (which is our controller class) with setTarget. Remember to use the classes actual Objective-C reference using Controller.Handle (not the wrapper Controller). Then call the Objective-C runtime function sel_registerName to register the selector, which is the name of our method that we registered before. If the selector has already been registered it will just return the handle, not leak memory. Then, set the action with the selector using setAction. 
    
    
     Button.setAction(sel_registerName(PChar('myController:handleButtonAction:')));
     Button.setTarget(Controller.Handle);
    

The methods setAction and setTarget happen to belong to NSControl but this paradigm of action/target is used commonly throughout the API, where the action is a selector (message string which is the name of a method) and the target which is an object. 

  


## Roadmap

### General Roadmap

Component | Status | Details   
---|---|---  
Objective-C 1.0 Runtime Headers | Working | Cheetah 10.0 or superior   
Objective-C 2.0 Runtime Headers | Not Implemented | Leopard 10.5 or superior only   
Automatic headers Parser | Partially Implemented   
Foundation | Partially Implemented   
AppKit | Partially Implemented   
  
### Foundation Classes Roadmap

Component | Status   
---|---  
NSArray | Working   
NSAutoreleasePool | Working   
NSBundle | Working   
NSDate | Working   
NSGeometry | Partially Implemented   
NSObjcRuntime | Working   
NSObject | Partially Implemented   
NSRange | Partially Implemented   
NSString | Working   
NSValue | Working   
NSZone | Partially Implemented   
  
### AppKit Classes Roadmap

Component | Status   
---|---  
NSActionCell | Partially Implemented   
NSAlert | Partially Implemented   
NSApplication | Working   
NSBitmapImageRep | Working   
NSBox | Working   
NSButton | Working   
NSButtonCell | Partially Implemented   
NSCell | Partially Implemented   
NSControl | Working   
NSGraphics | Partially Implemented   
NSGraphicsContext | Working   
NSImage | Working   
NSImageRep | Working   
NSMenu | Working   
NSMenuItem | Partially Implemented   
NSNibDeclarations | Partially Implemented   
NSNibLoading | Partially Implemented   
NSOpenPanel | Working   
NSPanel | Partially Implemented   
NSResponder | Partially Implemented   
NSSavePanel | Working   
NSStatusBar | Working   
NSStatusItem | Working   
NSText | Working   
NSTextField | Working   
NSTextFieldCell | Working   
NSToolbar | Working   
NSToolbarItem | Working   
NSView | Working   
NSWindow | Working   
  
## Implementation Details

### Overview

The Cocoa headers contain several peculiarities that lead us to need a special structure for the bindings. First, several forward declarations of classes are done in a very unordently way. Pascal only accepts forward declarations of classes if they are all declared on the same **type** clause, and therefore a special session **FORWARD** is created to have a single forward declaration for each file. 

The include file has a total of 4 separate sessions: HEADER, FORWARD, CLASSES and IMPLEMENTATION 

A tipical include file should have this structure: 
    
    
    {%mainunit appkit.pas}
    {
            NSStatusBar.h
            Application Kit
            Copyright (c) 1997-2005, Apple Computer, Inc.
            All rights reserved.
    }
    
    {$ifdef HEADER}
    {$ifndef NSSTATUSBAR_PAS_H}
    {$define NSSTATUSBAR_PAS_H}
    
    //insert constants, records and other interface declarations with the exception of classes
    
    {$endif}
    {$endif}
    
    {$ifdef FORWARD}
    
      TNSClass = class;
    
    {$endif}
    
    {$ifdef CLASSES}
    {$ifndef NSSTATUSBAR_PAS_H}
    {$define NSSTATUSBAR_PAS_H}
    
    //declaration of classes 
    
      TNSClass = class(NSObject)
        ...
      end;
    
    {$endif}
    {$endif}
    {$ifdef IMPLEMENTATION}
    
    //insert implementation declarations here. The order is not important on the implementation, so don't use extra ifdefs.
    
    {$endif}
    

### HEADERS

**Guidelines:**

  * Inclusion of headers in other frameworks should be removed, because the each framework is mapped to a pascal unit, and therefore all include files of a used framework are already accessible. Inclusion of headers of the same file framework should become include clauses which will used a C-styled ifdef mechanism to resolve in which order the include files will be added. This mechanism works fairly well and keeps the structure of the include files similar to the original one.



### CLASSES

**Guidelines:**

  * Should include the same include files as the HEADERS section


  * All private members of classes should be removed



### IMPLEMENTATION

  * Method should send message using one of the functions:



objc_msgSend 

objc_msgSend_frep (float and double types for i386, not for PPC) 

objc_msgSend_stret (records) 

Reference Page: <http://developer.apple.com/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html>

  * Class methods must be called for ClassID
  * Instance methods must be called for Handle



### Type Encodings

  * Reference page: <http://developer.apple.com/documentation/Cocoa/Conceptual/ObjectiveC/Articles/chapter_14_section_9.html#//apple_ref/doc/uid/TP30001163-CH9-TPXREF165>



Table of codes used for types when registering methods in the Objective-C Run-time Library: 

c  | Char  | A character   
---|---|---  
i  | cint (ctypes unit)  | A C compatible integer   
s  | cshort (ctypes unit)  |   
l  | clong (ctypes unit)  |   
q  | clonglong (ctypes unit)  | A long long   
C  | cuchar (ctypes unit)  |   
I  | cuint (ctypes unit)  |   
S  | cushort  |   
L  | culong  |   
Q  | culonglong  | An unsigned long long   
f  | Float  |   
d  | Double  |   
B  | ???  | A C++ bool or a C99 _Bool   
v  | None  | Indicates the absense of the parameter   
*  | PChar  | A C-style string   
@  | id (objc unit)  | An Objective-C class instance   
#  | Class (objc unit)  |   
:  | Sel (objc unit)  | An Objective-C selector   
[array type]  |  | An array   
^type  | P<type> | A pointer to type   
?  | Pointer  | A generic pointer   
  
## Notes

### Objects allocation

Objective-C language does not provide any special syntax to define, if class method is construtor method or not. However, Apple provides some kind of methods naming convention, you can learn more about it here: <http://developer.apple.com/documentation/Cocoa/Conceptual/ObjectiveC/Articles/chapter_13_section_3.html>

Following Objective-C methods, should be constructors for Pascal objects: 

  * class methods named +(id) alloc... (Obj-C construct methods)
  * class methods +(NSClassName *) anyMethodName... (Obj-C convenience constructors)
  * class methods +(id) anyMethodName (Obj-C convenience constructors)
  * instance methods name - init... (Obj-C initialization methods).



Though object allocation and initialization are separated in time for Objective-C. Pascal rarely uses the same style, providing larger number of constructors, so - init... methods should become constructors, though it's questionable. 

### Categories

Objective-C provides mechanism to extend class methods dynamicly without need of class iheritance. These are known as categories and extensions. Reference page: <http://developer.apple.com/documentation/Cocoa/Conceptual/ObjectiveC/Articles/chapter_4_section_1.html> This feature is not supported by Pascal, there's no way to extend methods of the class without inheritance. 

Categories are used very often in Cocoa headers. They are used to group methods logically. It's common, that Base class is extended with several categories within a single header file. The good example is NSString.h, there base NSString class has only 2 method, while it's categories declares dozens of new methods. 

In this case (if base class and it's categories are declared in the single header) all methods of the categories must be declared the base class. So convertede Pascal class NSString, would contain all methods that were declared in NSString.h categories. 

The same way, might be used to generate Pascal classes, if base Obj-C class and it's categories are declared in different headers, but discouraged. 

### Carbon-Cocoa integration

Some Obj-C classes can be cast to Carbon types, such types are also known as toll-free bridged. It's recommended to use Carbon types (all of them declared in the MacOSALL unit) for parameters and method result types. 
    
    
    function GetName: NSString;
    procedure SetStrParam(value: NSString);
    

should be 
    
    
    function GetName: CGStringRef;
    procedure SetStrParam(value: CGStringRef);
    

List of toll-free bridged types, add additional information on Carbon-Cocoa integration can be found at this page: <http://developer.apple.com/documentation/Cocoa/Conceptual/CarbonCocoaDoc/Articles/InterchangeableDataTypes.html>

### Unnamed enums

There're great number of unnamed enums declared in Cocoa headers. These enums are used to describe the constant values for methods parameters. They must be converted as Pascal constants, rather than enumeration, to avoid parameter size conflicts. 

## Cocoa related external links

  * NSToolbar tutorial: <http://www.dikant.de/2007/09/05/nstoolbar-basics/>
  * Working without a nib file


  1. <http://lapcatsoftware.com/blog/2007/05/16/working-without-a-nib-part-1/>
  2. <http://lapcatsoftware.com/blog/2007/06/04/working-without-a-nib-part-2-also-also-wik/>
  3. <http://lapcatsoftware.com/blog/2007/06/10/working-without-a-nib-part-5-no-3/>
  4. <http://lapcatsoftware.com/blog/2007/06/17/working-without-a-nib-part-4-setapplemenu/>
  5. <http://lapcatsoftware.com/blog/2007/07/10/working-without-a-nib-part-5-open-recent-menu/>
  6. <http://lapcatsoftware.com/blog/2007/11/25/working-without-a-nib-part-6-working-without-a-xib/>


  * Code to create a menu without a nib file: <http://trixul.cvs.sourceforge.net/viewvc/*checkout*/trixul/trixul/layout/cocoa/cocoamenubarimpl.mm?revision=1.6>



## Articles about PasCocoa

  * Toolbox Magazine 2008, editions 5 and 6 (german language): <http://www.toolbox-mag.de/archiv2008.html>

---

_Source: [https://wiki.freepascal.org/PasCocoa](https://web.archive.org/web/20250324072607/https://wiki.freepascal.org/PasCocoa)_
