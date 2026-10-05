# Show Badge on Application Icon in Dock

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

![<translate> Warning: </translate>](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/OOjs_UI_icon_notice-destructive.svg/18px-OOjs_UI_icon_notice-destructive.svg.png) **Warning**|  For unknown reasons this does not always work in macOS 12 (Monterey) and does not log any error in the system console log - it has been tested working in macOS 10.6 (Snow Leopard) through 11 (Big Sur). It has also been tested working on one 2018 Intel Mac mini macOS 12 system, but not working on one 2018 Intel Mac mini macOS 12 system and one 2020 M1 Mac mini macOS 12 system.  
---|---  
  
Badges are used to notify a user of something new in an application. The badge is a counter that appears on the application's dock tile (icon) as a white number on a red circular or oval, depending on the number of digits, background. What the number represents depends on the application - it might be the number of new emails in an email application, new iMessages, unfinished Reminders, missed FaceTime calls, the number of outstanding application updates, etc. 

## Example code

This example shows you how to implement notification badges in an application. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Dialogs, StdCtrls,
      CocoaAll;
    
    type
    
      { TForm1 }
    
      TForm1 = Class(TForm)
        Button1: TButton;
        Button2: TButton;
        procedure Button1Click(Sender: TObject);
        procedure Button2Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      // Display badge text on the dock tile (icon)
      NSApp.dockTile.setBadgeLabel(NSStr('1'));
    end;
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      // Remove badge text from the dock tile (icon)
      NSApp.dockTile.setBadgeLabel(Nil);
    end;
    
    end.
    

## See also

  * [Application disable resize over Dock](<Application_disable_resize_over_Dock.md> "Application disable resize over Dock")
  * [Bounce Application Icon in Dock](<Bounce_Application_Icon_in_Dock.md> "Bounce Application Icon in Dock")
  * [Hiding a macOS app from the Dock](<Hiding_a_macOS_app_from_the_Dock.md> "Hiding a macOS app from the Dock")
  * [macOS Application Dock Menu](<macOS_Application_Dock_Menu.md> "macOS Application Dock Menu")



## External links

  * [Apple: NSApplication](<https://developer.apple.com/documentation/appkit/nsapplication>).
  * [Apple: NSDockTile](<https://developer.apple.com/documentation/appkit/nsdocktile>).

---

_Source: [https://wiki.freepascal.org/Show_Badge_on_Application_Icon_in_Dock](https://web.archive.org/web/20240229213207/https://wiki.freepascal.org/Show_Badge_on_Application_Icon_in_Dock)_
