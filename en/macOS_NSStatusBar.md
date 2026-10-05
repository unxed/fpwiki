# macOS NSStatusBar

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Creating status items
  * 3 Example code for a text status item menu
  * 4 Example code for an image status item
  * 5 See also
  * 6 External links



## Overview

NSStatusBar is an object that manages a collection of status items that provide interaction with or feedback to the user. A status item can be displayed with text or an icon, can provide a menu or send a target-action message when clicked. Use status items sparingly and only if the alternatives (such as a Dock menu, preference pane, or status window) are not suitable. As there is limited space in which to display status items, status items are not guaranteed to be available at all times. For this reason, Apple recommends that you do not rely on them being available and always provide a user preference for hiding your application’s status items to free up space in the menu bar. 

[![macOS Status Bar.png](https://wiki.freepascal.org/images/1/18/macOS_Status_Bar.png)](</File:macOS_Status_Bar.png>)

Only one status bar, the system status bar, is available in macOS. It resides in the system-wide menu bar as shown above. Status items appear on the right side of the menu bar, just to the left of the menu bar clock and Menu Extras, such as the Displays and Sound menus. The items remain in the menu bar even when your application is not in the foreground. 

## Creating status items

1\. Obtain the system status bar with the **systemStatusBar** class method; you should not allocate an instance of it yourself. ' 2A. If you are displaying an icon, invoke **statusItemWithLength** to create a new status item and allocate space for it in the menu bar. Pass the amount of space in pixels you need to display your status item. You can use the constant **NSSquareStatusItemLength** to make the width the same as the status bar’s thickness. 

2B. If you are displaying text, invoke **statusItemWithLength** to create a new status item and allocate space for it in the menu bar. Pass the amount of space in pixels you need to display your status item. You can use the constant **NSVariableStatusItemLength** to make the width variable based on the contents of the item. 

The system status bar is shared by all applications and therefore cannot retain references to each application’s status item objects. Instead, each application is responsible for retaining its own status items. Each status item then communicates with the status bar as its configuration changes. When deallocated, the status item removes itself from the status bar. Following normal Cocoa memory management rules, you **must retain the object** returned by statusItemWithLength to keep it around. 

Once you have the new status item object, you can assign it a title, a menu, a target-action, a tool tip, and so on. 

## Example code for a text status item menu

[![Ex1 Menu Bar.png](https://wiki.freepascal.org/images/b/bc/Ex1_Menu_Bar.png)](</File:Ex1_Menu_Bar.png>)

This example application places a text status item named "Captioner" on the system-wide menu bar. Clicking on the item reveals a menu with three options, two of which change the caption on the application's title bar while the third quits the application. The toggle button allows you to toggle the status item on/off. By calling the ToggleButtonClick handler from the OnCreate event of the Form, the status item appears automagically when the application is launched. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms, Dialogs, StdCtrls,
      CocoaAll, Classes;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        ToggleButton: TButton;
        procedure ToggleButtonClick(Sender: TObject);
        procedure FormCreate(Sender: TObject);
      private
    
      public
    
      end;
    
      { TMyDelegate }
    
      TMyAppDelegate = objcclass(NSObject)
          private
    
          public
            procedure menuItem1;
              message 'menuItem1';
            procedure menuItem2;
              message 'menuItem2';
            procedure menuItem3;
              message 'menuItem3';
          end;
    var
      Form1: TForm1;
      myAppDelegate: TMyAppDelegate;
      myStatusBarItem: NSStatusItem;
      sbi_created: Boolean = False;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    // AppDelegate methods
    procedure TMyAppDelegate.menuItem1;
    begin
      Form1.Caption := 'Hello menu item 1';
    end;
    
    procedure TMyAppDelegate.menuItem2;
    begin
      Form1.Caption := 'Hello menu item 2';
    end;
    
    procedure TMyAppDelegate.menuItem3;
    begin
      Form1.Close;
    end;
    
    // ToggleButton handler
    procedure TForm1.ToggleButtonClick(Sender: TObject);
    var
      statusBar: NSStatusBar;
      myMenu: NSMenu;
      myMenuItem1: NSMenuItem;
      myMenuItem2: NSMenuItem;
      myMenuItem3: NSMenuItem;
      aSEL1: SEL;
      aSEL2: SEL;
      aSEL3: SEL;
    begin
      // If status bar item exists
      if(sbi_created <> False) then
          begin
            // Remove item
            NSStatusBar.systemStatusBar.removeStatusItem(myStatusBarItem);
            myStatusBarItem := Nil;
            // Allow new one to be created next time
            sbi_created := False;
            exit;
          end
      else
        sbi_created := True;
    
      // Obtain system-wide status bar instance
      statusBar := NSStatusBar.systemStatusBar;
    
      // Setup selectors for menu item actions
      aSel1 := ObjCSelector(TMyAppDelegate.menuItem1);
      aSel2 := ObjCSelector(TMyAppDelegate.menuItem2);
      aSel3 := ObjCSelector(TMyAppDelegate.menuItem3);
    
      // Create menu
      myMenu := NSMenu.alloc.init.autorelease;
      myMenuItem1 := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(NSStr('Menu item 1'),aSEL1, NSStr('A')).autorelease;
      myMenuItem2 := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(NSStr('Menu item 2'),aSEL2, NSStr('B')).autorelease;
      myMenuItem3 := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(NSStr('Quit'),aSEL3, NSStr('Q')).autorelease;
      myMenu.addItem(myMenuItem1);
      myMenu.addItem(myMenuItem2);
      myMenu.addItem(myMenuItem3);
    
      // Setup my status bar item
      myStatusBarItem := statusBar.statusItemWithLength(NSVariableStatusItemLength);
      myStatusBarItem.setTitle(NSStr('Captioner'));
      myStatusBarItem.setHighlightMode(True);
      myStatusBarItem.setMenu(myMenu);
    
      // Important! You must retain the object or it will disappear when the
      // variable goes out of scope
      myStatusBarItem.retain;
    end;
    
    // Form creation
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      // NSApp
      // - The global variable for the shared application instance.
      // NSApplication.sharedApplication
      // - Returns the application instance, creating it if it doesn’t exist yet.
      NSApp := NSApplication.sharedApplication;
    
      // Setup my application delegate
      NSApp.setDelegate(myAppDelegate);
    
      // Show status bar item
      ToggleButtonClick(self);  
    end;
    
    Initialization
      myAppDelegate := TMyAppDelegate.alloc.init;
    
    Finalization
      myAppDelegate.Release;
    end.
    

Full project source code is available on [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_nsstatusbar/>). 

## Example code for an image status item

[![Ex2 Menu Bar.png](https://wiki.freepascal.org/images/4/44/Ex2_Menu_Bar.png)](</File:Ex2_Menu_Bar.png>)

This example application places a status item image on the system-wide menu bar. Clicking on the item reveals a menu with three options, two of which change the caption on the application's title bar while the third quits the application. The toggle button allows you to toggle the status item on/off. By calling the ToggleButtonClick handler from the OnCreate event of the Form, the status item appears automagically when the application is launched. 

This code is pretty much identical to the previous code above - the new and changed lines have been highlighted. The image file, `icon.png` is located in the [application bundle's](<Application_Bundle.md> "Application Bundle") Resources directory. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Forms, StdCtrls,
      CocoaAll, Classes;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        ToggleButton: TButton;
        procedure ToggleButtonClick(Sender: TObject);
        procedure FormCreate(Sender: TObject);
      private
    
      public
    
      end;
    
      { TMyDelegate }
    
      TMyAppDelegate = objcclass(NSObject)
          private
    
          public
            procedure menuItem1;
              message 'menuItem1';
            procedure menuItem2;
              message 'menuItem2';
            procedure menuItem3;
              message 'menuItem3';
          end;
    var
      Form1: TForm1;
      myAppDelegate: TMyAppDelegate;
      myStatusBarItem: NSStatusItem;
      sbi_created: Boolean = False;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    // AppDelegate methods
    procedure TMyAppDelegate.menuItem1;
    begin
      Form1.Caption := 'Hello menu item 1';
    end;
    
    procedure TMyAppDelegate.menuItem2;
    begin
      Form1.Caption := 'Hello menu item 2';
    end;
    
    procedure TMyAppDelegate.menuItem3;
    begin
      Form1.Close;
    end;
    
    // ToggleButton handler
    procedure TForm1.ToggleButtonClick(Sender: TObject);
    var
      statusBar: NSStatusBar;
      myMenu: NSMenu;
      myMenuItem1: NSMenuItem;
      myMenuItem2: NSMenuItem;
      myMenuItem3: NSMenuItem;
      aSEL1: SEL;
      aSEL2: SEL;
      aSEL3: SEL;
      myImage: NSImage;
    begin
      // If status bar item exists
      if(sbi_created <> False) then
          begin
            // Remove item
            NSStatusBar.systemStatusBar.removeStatusItem(myStatusBarItem);
            myStatusBarItem := Nil;
            // Allow new one to be created next time
            sbi_created := False;
            exit;
          end
      else
        sbi_created := True;
    
      // Obtain system-wide status bar instance
      statusBar := NSStatusBar.systemStatusBar;
    
      // Setup selectors for menu item actions
      aSel1 := ObjCSelector(TMyAppDelegate.menuItem1);
      aSel2 := ObjCSelector(TMyAppDelegate.menuItem2);
      aSel3 := ObjCSelector(TMyAppDelegate.menuItem3);
    
      // Create menu
      myMenu := NSMenu.alloc.init.autorelease;
      myMenuItem1 := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(NSStr('Menu item 1'),aSEL1, NSStr('A')).autorelease;
      myMenuItem2 := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(NSStr('Menu item 2'),aSEL2, NSStr('B')).autorelease;
      myMenuItem3 := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(NSStr('Quit'),aSEL3, NSStr('Q')).autorelease;
      myMenu.addItem(myMenuItem1);
      myMenu.addItem(myMenuItem2);
      myMenu.addItem(myMenuItem3);
    
      // Setup my status bar item
      myStatusBarItem := statusBar.statusItemWithLength(NSSquareStatusItemLength);
      myImage := NSImage.alloc.initWithContentsOfFile(NSBundle.mainBundle.pathForImageResource(NSStr('icon.png'))).autorelease;
      myStatusBarItem.button.setImage(myImage);
      myStatusBarItem.setHighlightMode(True);
      myStatusBarItem.setMenu(myMenu);
    
      // Important! You must retain the object or it will disappear when the
      // variable goes out of scope
      myStatusBarItem.retain;
    end;
    
    // Form creation
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      // NSApp
      // - The global variable for the shared application instance.
      // NSApplication.sharedApplication
      // - Returns the application instance, creating it if it doesn’t exist yet.
      NSApp := NSApplication.sharedApplication;
    
      // Setup my application delegate
      NSApp.setDelegate(myAppDelegate);
    
      // Show status bar item
      ToggleButtonClick(self);
    end;
    
    Initialization
      myAppDelegate := TMyAppDelegate.alloc.init;
    
    Finalization
      myAppDelegate.Release;
    end.
    

Full project source code available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_nsstatusbar2/>). 

## See also

  * [macOS Application Dock Menu](<macOS_Application_Dock_Menu.md> "macOS Application Dock Menu")
  * [Apple-specific UI elements](<Apple-specific_UI_elements.md> "Apple-specific UI elements")



## External links

  * [Apple: NSStatusBar](<https://developer.apple.com/documentation/appkit/nsstatusbar>)
  * [Apple: NSStatusItem](<https://developer.apple.com/documentation/appkit/nsstatusitem>)
  * [Apple: NSImage](<https://developer.apple.com/documentation/appkit/nsimage>)

---

_Source: [https://wiki.freepascal.org/macOS_NSStatusBar](https://web.archive.org/web/20250422061850/https://wiki.freepascal.org/macOS_NSStatusBar)_
