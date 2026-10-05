# Cocoa Internals

The page is about the 64 bit Cocoa Widgetset internal implementation. The page should be useful for Cocoa widgetset developers (maintainers and/or contributors) as well as any developers who need to use the Cocoa-specific API. 

## Contents

  * 1 Minimum Version
    * 1.1 Compiler Support
  * 2 Recompilation
    * 2.1 Reconfigure LCL package
  * 3 LCL specific ObjC classes
  * 4 Code Style
  * 5 Using for-in instead of for
  * 6 Writing a Cocoa app without the LCL
  * 7 Units
  * 8 Callback Objects
  * 9 Why so Boolean?
  * 10 See also



## Minimum Version

  * Some parts of Cocoa are written using Objective C 2.0 features. Objective C 2.0 features used:



    

    [Enumeration in Objective-C classes](<https://www.freepascal.org/docs-html/ref/refse76.html#x138-16000011.10>)

  * Minimal macOS version is:



    

  * 10.5 - spiritual
  * 10.6 - de facto (a lot of 10.6 only APIs are used without any actual verification of the proper macOS version)



There's a reason to keep the backwards compatibility for 10.6. This is the last system that supports Rosetta applications to allow running [PowerPC](<https://en.wikipedia.org/wiki/PowerPC>) applications on an Intel processor. 

### Compiler Support

APIs that not (yet) provided by a corresponding version of FPC (delivered with Lazarus), should be declared at **cocoa_extra.pas** header 

It's desired that the legacy version (i.e. deprecated APIs) would be wrapped into the [minimum version defines](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2"). 

If an application using LCL to be deployed to App Store, Apple would verify the used APIs and ban, if any older APIs are used. 

## Recompilation

If you tried to modify Cocoa-Widget and the next attempt at the project or its Cocoa compilation throws an unexpected error like "cocoawscommon.pas(y:x) Error: identifier idents no member "lclFrame"", you need to delete all Cocoa compiled units. 
    
    
     ./lazarus/lcl/units/i386-darwin/cocoa
    

or 
    
    
     ./lazarus/lcl/units/x86_64-darwin/cocoa
    

(to be reported - compiler issue with ignoring extensions) 

### Reconfigure LCL package

[![LCL configure cocoa to removeunits.png](https://wiki.freepascal.org/images/5/53/LCL_configure_cocoa_to_removeunits.png)](</File:LCL_configure_cocoa_to_removeunits.png>)

[](</File:LCL_configure_cocoa_to_removeunits.png> "Enlarge")

Another way is to configure the LCL package to always remove .o files before recompilation of the package itself. (The same effect as removing it from command line): 

  * Open LCL package ( Package -> Open Loaded Package ... ->LCL )
  * Select (package) "Options"
  * Select "Compiler Command"
  * specify command for "Execute before"


    
    
    sh -c "rm -f ../units/$(TargetCPU)-$(TargetOS)/$(LCLWidgetType)/*"
    

Note that use of **sh** is necessary, because Lazarus IDE doesn't process * (asterisk) 

## LCL specific ObjC classes

In order to control and handle an NSView's behavior LCL uses descendent classes from standard Cocoa controls. That is, for NSWindow, TCocoaWindow is introduced. The descedants are used for the purpose of "overriding" default class implementation, where it is needed. In some cases using delegate classes is not enough. 

There are two ways, in order to provide a common LCL(friendly) API interface for all Cocoa classes or sub-classes: 

  * use **protocol** (similar to Interfaces in Object Pascal form). Any class, sub-classing from standard Cocoa classes, would have to implement the **protocol**. The downside of this approach is - a lot of code would be duplicated.
  * use **categories** (similar to Javascript prototypes, not available in Object Pascal. Class-helpers looks alike, but are not the same). Categories allow to add methods (only methods! adding variables is not allowed) to any base class of Objective-C. Any descendants of this class automatically acquire the method, and it's default implementation. Also any subclass, can override the default implementation with its own. Apple doesn't recommend the use of categories (due to ambiguity and possible conflicts among different libraries introducing their own extensions). See more at the [Cocoa Internals Extensions](<Cocoa_Internals/Extensions.md> "Cocoa Internals/Extensions") page



## Code Style

The following requirement applies for Cocoa widgetset. Other widgetset may be following some other rules (historically), but in general LCL follows the same requirements. 

  * **Operators** Keep the operator separated by spaces between operands


    
    
     A := B; 
     A := B * C;
    

  * **Blocks** The main rule is to make 2 character spacing from the code block start. Begin / Else should start on a new line.


    
    
     procedure B;
     begin
       if A then // 2 character spacing from begin
       begin
         Start Here // 2 character spacing from begin
         Next Line
       end
       else
       begin
         Another Line
       end;
     end;
    

  * **Standard Function Name** please keep naming in ProperCase. Reserved words should be lower case. Name of (global/local) variables and fields should match declaration. Local variables should start with lower case.


    
    
      if not Assigned(A) then
      begin
        Result := nil;
      end;
    

## Using for-in instead of for

When dealing with arrays, it's common for Pascal to write the code using "Integer" type array index like: 
    
    
    var
      i : integer;
      l : TList;
    ...
     for i:=0 to l.Count-1 do
       ...
    

Cocoa provides it's own implementation of arrays using NSArray class. Writing the the loop in the same manner: 
    
    
    var
      i : integer;
      l : NSArray;
    ...
     for i:=0 to l.count-1 do
       ...
    

is possible, though not desired. 

The reason for that, is that NSArray "count" is an NSUInteger type for Cocoa. This type has a different size on 32 and 64 platforms. (While "Integer" type is always 32 bit in FPC.) 

Thus while the code written above might work just fine on 32-platform, it might cause a range check error (if such check is enabled) on a 64 bit machine. For example, when count is zero. Here's a simplest (no ObjC) example of the problem: 
    
    
    {$R+}
    function Count: qword;
    begin
      Result := 0;
    end;
    var
      i: Integer;
    begin
      for i := 0 to Count-1 do // <- throws a range check error, because QWord result doesn't fit for Integer index
        WriteLn(i);
    end.
    

However, running a loop from elements of an array is quite a common task. For example - changing attributes of all children controls, due to some action. 

Instead of using a higher size Index variable (i.e. PtrInt or PtrUInt), it's handy to use for-in loop. Most common Objective C classes provide a special feature (NSFastEnumerator protocol) to do enumeration through its member. It works for the 32 bit and well as the 64 bit platform, in the same manner, but requires the Objective C 2.0 runtime version (with objectivec2 modeswitch enabled). 

An iteration for-in loop might look like this: 
    
    
    var
      obj : NSObject;
      arr : NSArray;
    ...
     for obj in arr do
       ...
    

## Writing a Cocoa app without the LCL

This is useful for testing hard to debug bugs in the LCL. Here is an example program written in Pascal-Cocoa which creates a window with 2 buttons and a simple set of menus: 
    
    
    program cocoa_without_lcl;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    uses
      CocoaAll, classes, sysutils;
    
    type
      { TMyDelegate }
      TMyDelegate = objcclass(NSObject)
      public
        procedure HandleButtonClick_A(sender: id); message 'HandleButtonClick_A:';
        procedure HandleButtonClick_B(sender: id); message 'HandleButtonClick_B:';
        procedure HandleMenuClick_A(sender: id); message 'HandleMenuClick_A:';
        procedure HandleMenuClick_B(sender: id); message 'HandleMenuClick_B:';
      end;
    
    var
      appName: NSString;
      window: NSWindow;
      pool: NSAutoreleasePool;
      lText: NSTextField;
      lButton: NSButton;
      lDelegate: TMyDelegate;
      //
      MainMenu_A: NSMenu;
      TopItem_A: NSMenuItem;
      TopMenu_A: NSMenu;
      MenuItem_A: NSMenuItem;
      //
      MainMenu_B: NSMenu;
      TopItem_B: NSMenuItem;
      TopMenu_B: NSMenu;
      MenuItem_B: NSMenuItem;
    
    procedure TMyDelegate.HandleButtonClick_A(sender: id);
    begin
      NSApp.setMainMenu(MainMenu_A);
    end;
    
    procedure TMyDelegate.HandleButtonClick_B(sender: id);
    begin
      NSApp.setMainMenu(MainMenu_B);
    end;
    
    procedure TMyDelegate.HandleMenuClick_A(sender: id);
    var
      Str: string;
    begin
      Str := 'A '+inttostr(random(888));
      window.setTitle(NSSTR(@Str[1]));
    end;
    
    procedure TMyDelegate.HandleMenuClick_B(sender: id);
    var
      Str: string;
    begin
      Str := 'B '+inttostr(random(888));
      window.setTitle(NSSTR(@Str[1]));
    end;
    
    procedure CreateMenus;
    var
      nsTitle: NSString;
      nsKey  : NSString;
    
      function CreateMenuItem(AName, AShortcut: string; ASelector: SEL): NSMenuItem;
      begin
        nsKey := NSSTR(PChar(AShortcut));
        nsTitle := NSSTR(PChar(AName));
        Result := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(nsTitle, ASelector, nsKey);
        Result.setKeyEquivalentModifierMask(NSCommandKeyMask);
        nsTitle.release;
        nsKey.release;
        Result.setKeyEquivalentModifierMask(NSCommandKeyMask);
        Result.setTarget(lDelegate);
      end;
    
    begin
      MainMenu_A := NSMenu.alloc.initWithTitle(NSSTR('Menu A'));
      TopItem_A := CreateMenuItem('TopItem A', '', nil);
      MainMenu_A.insertItem_atIndex(TopItem_A, 0);
      TopMenu_A := NSMenu.alloc.initWithTitle(NSSTR('TopMenu A'));
      TopItem_A.setSubmenu(TopMenu_A);
      MenuItem_A := CreateMenuItem('MenuItem A', 'A', objcselector('HandleMenuClick_A:'));
      MenuItem_A.setTarget(lDelegate);
      TopMenu_A.insertItem_atIndex(MenuItem_A, 0);
    
      MainMenu_B := NSMenu.alloc.initWithTitle(NSSTR('Menu B'));
      TopItem_B := CreateMenuItem('TopItem B', '', nil);
      MainMenu_B.insertItem_atIndex(TopItem_B, 0);
      TopMenu_B := NSMenu.alloc.initWithTitle(NSSTR('TopMenu B'));
      TopItem_B.setSubmenu(TopMenu_B);
      MenuItem_B := CreateMenuItem('MenuItem B', 'B', objcselector('HandleMenuClick_B:'));
      MenuItem_B.setTarget(lDelegate);
      TopMenu_B.insertItem_atIndex(MenuItem_B, 0);
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
      lDelegate := TMyDelegate.alloc.init;
    
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
      lButton.setTitle(NSSTR('Button A!'));
      lButton.setButtonType(NSMomentaryLightButton);
      lButton.setBezelStyle(NSRoundedBezelStyle);
      // button event handler setting
      lButton.setTarget(lDelegate);
      lButton.setAction(ObjCSelector(lDelegate.HandleButtonClick_A));
    
      // button B
      lButton := NSButton.alloc.initWithFrame(NSMakeRect(50, 150, 120, 50)).autorelease;
      window.contentView.addSubview(lButton);
      lButton.setTitle(NSSTR('Button B!'));
      lButton.setButtonType(NSMomentaryLightButton);
      lButton.setBezelStyle(NSRoundedBezelStyle);
      // button event handler setting
      lButton.setTarget(lDelegate);
      lButton.setAction(ObjCSelector(lDelegate.HandleButtonClick_B));
    
      // Menus
      CreateMenus();
      NSApp.setMainMenu(MainMenu_A);
    
      // Window showing and app running
      window.center;
      window.setTitle(appName);
      window.makeKeyAndOrderFront(nil);
      NSApp.activateIgnoringOtherApps(true);
      NSApp.run;
    end.
    

## Units

The units should use the ".pas" extension, not ".pp" for consistency. Use of "include" is not welcome and is kept there for legacy reasons only (not to lose the history, if a file is merged). 

Unit  | Description   
---|---  
CocoaPrivate  | The unit contains most of the Cocoa classes overrides. The unit should stay LCL-classes free as much as possible (i.e. should not use Forms, StdCtrls, etc. Though using LCLTypes or LCLProc is ok).   
CocoaUtils  | The set of utility functions mostly to convert types (between LCL and Cocoa types) Input constant conversion utilities are also placed here.   
Cocoa_Extra  | The purpose of the unit is to full-fill missing FPC headers declarations for macOS. (whatever is missing at CocoaAll). Should not contain any additional functions, except for external and constant declarations.   
CocoaButtons  | Controls that are considered buttons (except for NSPopupButton) are declared in this unit.   
CocoaCarent  | The unit provides the code to implement caret for Cocoa. Cocoa itself doesn't have any "caret" specific API.   
CocoaDatepicker  | The unit contains the implementation of cocoa calendar control   
CocoaGDIObjects  | Highlever wrappers over Cocoa object to implement LCL GDI handles.   
CocoaScrollers  | All controls related to scrolling: TScrollBar and variants of ScrollView are implemented here   
CocoaTabControls  | Classes that are used to implement tabcontrol (and tabpage) are declared here   
CocoaTables  | The master table class is declared here. (used for TListView and TListBox). Supplemental classes for NSCell and NSView based tables are also in this unit   
CocoaTextEdits  | Text editting controls are implemented here. Including Combobox and NSpopupBox (readonly combobox)   
CocoaTheme  | Theme drawing class.(todo: get rid of "customdrawing" parts)   
CocoaWindows  | Classes related to the window and its contents are implemented here   
CocoaInt cocoawinapih.inc / cocoawinapi.inc   
cocoalclapih.inc /cocoalclapi.inc  | The Widgetset class API (winApi-style) are implemented here. **TCocoaApplication** class is also declared and implemented in this unit. Both Widgetset object and code of TCocoaApplication class are mutually using each other.   
Widgetset interfacing units   
CocoaWSCommon  | The unit contains common bindings between native Cocoa (obcjclasses implemented in CocoaPrivate) and LCL. The actual callbacks interfaces are implemented here. Most of the "messaging" to LCL is also implemented here   
  
## Callback Objects

  * **ICommonCallback** is the primary object for Cocoa objects to report back actions taken. It prevents "the handlers" from dealing with Objective-C APIs over all.
  * Some controls might introduce an additional interface to process more events. They all should inherit from ICommonCallback
  * For each HANDLE created there should be exactly one CallBack object allocated, because
  * Callback object is released on Destruction of the HANDLE (from LCL perspective). (Even though the actual ObjC object can be released later.)



## Why so Boolean?

Cocoa Widgetset introduces two ObjC-specific boolean types: 

  * LCLObjCBoolean - declared at CocoaPrivate and is commonly used for any Cocoa overridden methods
  * ObjCBool - declared at Cocoa_Extra and is used under BOOL8FIX.



Both types were introduced to handle the issues with the Boolean type parameter. (Resulting Boolean value seems to be working as expected on x86_64). 

However, the intent of each type is different: 

  * LCLObjCBoolean - is to compensate for FPC backwards incompatible change (targeted for 3.2.0 release) that's changing all "Boolean" type parameters to "Boolean8". Thus any overridden methods in LCL would stop compile. (Boolean8 is not an alias for Boolean.) Unfortunatelly the type is expected to stay likely forever.
  * ObjCBool - is a need to resolve Boolean parameter problem with 3.0.4. For any call to boolean parameter method, additional methods were introduced to pass the parameter as byte parameter (**unsigned char**), and pass the actual value property. The type can be dropped as soon as LCL support for 3.0.4 is dropped. The type should not be used in any 3rd party components.



## See also

  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- general information regarding the Cocoa Interface.
  * [Cocoa Internals/Application](<Cocoa_Internals/Application.md> "Cocoa Internals/Application") \- details about use of NSApplication and clipboard.
  * [Cocoa Internals/Buttons](<Cocoa_Internals/Buttons.md> "Cocoa Internals/Buttons") \- provides the information about internal implementation of Button controls.
  * [Cocoa Internals/Dialogs](<Cocoa_Internals/Dialogs.md> "Cocoa Internals/Dialogs") \- implementation of common dialogs
  * [Cocoa Internals/Extensions](<Cocoa_Internals/Extensions.md> "Cocoa Internals/Extensions") \- additional method introduced to ObjC classes, to prevent code duplication and if-conditions.
  * [Cocoa Internals/Forms](<Cocoa_Internals/Forms.md> "Cocoa Internals/Forms") \- provides the information about internal implementation of Forms (Windows)
  * [Cocoa Internals/Graphics](<Cocoa_Internals/Graphics.md> "Cocoa Internals/Graphics") \- graphics, fonts related topics.
  * [Cocoa Internals/Input](<Cocoa_Internals/Input.md> "Cocoa Internals/Input") \- covers topics of Cocoa specific input processing implementation. Cocoa input processing is somewhat foreign to LCL (or Windows style) processing.
  * [Cocoa Internals/Memory Management](<Cocoa_Internals/Memory_Management.md> "Cocoa Internals/Memory Management") \- guidelines of memory management tactics, whenever writing/understanding Cocoa code. (provides a lot of general Cocoa information (also applicable for UIKit))
  * [Cocoa Internals/Menu](<Cocoa_Internals/Menu.md> "Cocoa Internals/Menu") \- macOS menu bar
  * [Cocoa Internals/OS Versions](<Cocoa_Internals/OS_Versions.md> "Cocoa Internals/OS Versions") \- suggestions around macOS API deprecations.
  * [Cocoa Internals/Scroll](<Cocoa_Internals/Scroll.md> "Cocoa Internals/Scroll") \- Scroll controls.
  * [Cocoa Internals/Text Controls](<Cocoa_Internals/Text_Controls.md> "Cocoa Internals/Text Controls") \- Cocoa text controls.
  * [Cocoa Internals/Theme](<Cocoa_Internals/Theme.md> "Cocoa Internals/Theme") \- Cocoa Themes.
  * [Cocoa Internals/Widgetset](<Cocoa_Internals/Widgetset.md> "Cocoa Internals/Widgetset") \- information about Handle implementation and focus change.

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals](https://web.archive.org/web/20250123184459/https://wiki.freepascal.org/Cocoa_Internals)_
