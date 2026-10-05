# macOS Application Dock Menu

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Example code
  * 3 See also
  * 4 External links



## Overview

Mac OS X v10.1 (Puma) added the ability to add custom menu items to an application's dock icon. Dock items like Options, Show All Windows, Hide ands Quit are provided automatically by the dock, so all we need to do is add our custom items. By adding menu items to your application's dock icon, you can provide the user with quick access to commonly used features. 

Another possibility is modifying the application's dock menu while the application is running to produce a dynamic menu with, for example, a status update. This is easily done by not assigning an action selector when creating the menu item. This also greys out the menu item so that it cannot be selected. There is an example included in the example code below. 

## Example code
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls, ExtCtrls,
      CocoaAll;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
        procedure FormClose(Sender: TObject; var CloseAction: TCloseAction);
        procedure FormCreate(Sender: TObject);
      private
    
      public
    
      end;
    
      TMyAppDelegate = objcclass(NSObject)
        private
    
        public
          function applicationDockMenu (sender: NSApplication): NSMenu;
            message 'applicationDockMenu:';
          procedure menuItemHello1;
            message 'menuItemHello1';
          procedure menuItemHello2;
            message 'menuItemHello2';
        end;
    
    var
      Form1: TForm1;
      myAppDelegate: TMyAppDelegate;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TMyAppDelegate.menuItemHello1;
    begin
      NSLog(NSStr('hello menu item 1'));
    end;
    
    procedure TMyAppDelegate.menuItemHello2;
    begin
      NSLog(NSStr('hello menu item 2'));
    end;
    
    function TMyAppDelegate.applicationDockMenu(sender: NSApplication): NSMenu;
    var
      aSel, bSel: SEL;
      myMenu: NSMenu;
      myMenuItem0: NSMenuItem;
      myMenuItem1: NSMenuItem;
      myMenuItem2: NSMenuItem;
    begin
      NSLog(NSStr('dock menu'));
    
      aSel := ObjCSelector(TMyAppDelegate.menuItemHello1);
      bSel := ObjCSelector(TMyAppDelegate.menuItemHello2);
    
      myMenu := NSMenu.alloc.init.autorelease;
      myMenuItem0 := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(NSStr('My Dock Menu'), Nil, NSStr('')).autorelease;
      myMenuItem1 := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(NSStr('Hello Item 1'), aSel, NSStr('')).autorelease;
      myMenuItem2 := NSMenuItem.alloc.initWithTitle_action_keyEquivalent(NSStr('Hello Item 2'), bSel, NSStr('')).autorelease;
      myMenu.addItem(myMenuItem0);
      myMenu.addItem(myMenuItem1);
      myMenu.addItem(myMenuItem2);
      Result := myMenu;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      NSLog(NSStr('Closing'));
      Close;
    end;
    
    procedure TForm1.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    begin
      NSLog(NSStr('Leaving...'));
    
      if(NSApp.isActive) then
        begin
          NSLog(NSStr('exiting'));
          exit;
        end
      else
        // needed if any part of form is occluded or does not quit
        // until dock icon clicked when quitting via dock menu
        begin
          NSLog(NSStr('activating'));
          Application.BringToFront;
          CloseAction := TCloseAction.caFree;
        end;
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      // NSApp
      // - The global variable for the shared application instance.
      // NSApplication.sharedApplication
      // - Returns the application instance, creating it if it doesn’t exist yet.
      NSApp := NSApplication.sharedApplication;
      NSApp.setDelegate(myAppDelegate);
    end;
    
    Initialization
      myAppDelegate := TMyAppDelegate.alloc.init;
    
    Finalization
      myAppDelegate.Release;
    end.
    

Typical log output when the executable is started from an Applications > Utilities > Terminal and the two added menu items are clicked before using the dock menu to quit: 
    
    
    trev@macmini7 [/Users/trev/Programming/LAZARUS/laz_dockmenu] $ ./project1.app/Contents/MacOS/project1
    2021-03-02 19:35:58.366 project1[1483:110782] dock menu
    2021-03-02 19:36:05.700 project1[1483:110782] dock menu
    2021-03-02 19:36:10.862 project1[1483:110782] hello menu item 1
    2021-03-02 19:36:12.644 project1[1483:110782] dock menu
    2021-03-02 19:36:14.360 project1[1483:110782] hello menu item 2
    2021-03-02 19:36:15.915 project1[1483:110782] dock menu
    2021-03-02 19:36:17.061 project1[1483:110782] Leaving...
    2021-03-02 19:36:17.061 project1[1483:110782] activating
    

Alternatively, you can view the output using the Applications > Utilities > Console. 

## See also

  * [Apple-specific UI elements](<Apple-specific_UI_elements.md> "Apple-specific UI elements")
  * [Application disable resize over Dock](<Application_disable_resize_over_Dock.md> "Application disable resize over Dock")
  * [Bounce Application Icon in Dock](<Bounce_Application_Icon_in_Dock.md> "Bounce Application Icon in Dock")
  * [Hiding a macOS app from the Dock](<Hiding_a_macOS_app_from_the_Dock.md> "Hiding a macOS app from the Dock")
  * [macOS NSStatusBar](<macOS_NSStatusBar.md> "macOS NSStatusBar")
  * [Show Badge on Application Icon in Dock](<Show_Badge_on_Application_Icon_in_Dock.md> "Show Badge on Application Icon in Dock")



## External links

  * [Apple: NSApplication](<https://developer.apple.com/documentation/appkit/nsapplication>).
  * [Apple: NSMenu](<https://developer.apple.com/documentation/appkit/nsmenu>).
  * [Apple: NSMenuItem](<https://developer.apple.com/documentation/appkit/nsmenuitem>).
  * [Apple: applicationDockMenu:](<https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428564-applicationdockmenu>).

---

_Source: [https://wiki.freepascal.org/macOS_Application_Dock_Menu](https://web.archive.org/web/20241213015431/https://wiki.freepascal.org/macOS_Application_Dock_Menu)_
