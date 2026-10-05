# Bounce Application Icon in Dock

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │    
****

## Contents

  * 1 Overview
  * 2 Example code
  * 3 See also
  * 4 External links



## Overview

Bouncing your application's icon in the dock can be useful to attract a user's attention when, for example, a long-running task has finished. 

To do this you use the _requestUserAttention_ method. There are two types of request: 

  * NSCriticalRequest 
    * The dock icon will bounce until either the application becomes active or the request is canceled.
  * NSInformationalRequest 
    * The dock icon will bounce for one second. The request, though, remains active until either the application becomes active or the request is cancelled.



Activating the application cancels the user attention request. A spoken notification ("Excuse me, MyApp dot app needs your attention") will occur if spoken notifications are enabled. Sending _requestUserAttention_ to an application that is already active has no effect. 

## Example code

This example will bounce the application's dock icon every second (Timer1.Interval default) while the application is not in focus. 

  * Drop a TTimer on a Form;
  * Add the following code to unit1.pas;
  * Add the Timer1Timer procedure to Timer1's _onTimer_ event.


    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch ObjectiveC1}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Dialogs, ExtCtrls, StdCtrls, CocoaAll;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Timer1: TTimer;
        procedure FormCreate(Sender: TObject);
        procedure Timer1Timer(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
      FRequestUserAttentionID : Int64;    // Use an Integer if 32 bit application
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      NsApp := NSApplication.sharedApplication;
    end;
    
    procedure TForm1.Timer1Timer(Sender: TObject);
    begin
      If(Form1.Active) then
        // no point trying to bounce if application has focus
        exit
      else
        // one shot bounce, no voice notification
        FRequestUserAttentionID := NSApp.requestUserAttention(NSInformationalRequest);
    end;
    
    end.
    

While the _FRequestUserAttentionID_ variable is not used in this demonstration, it is useful if you need to cancel the requestUserAttention which you can do with: 
    
    
       NSApp.cancelUserAttentionRequest(FRequestUserAttentionID);
    

If you are never going to use it in your application, you can omit declaring the variable and omit its assignment. 

## See also

  * [Application disable resize over Dock](<Application_disable_resize_over_Dock.md> "Application disable resize over Dock")
  * [Hiding a macOS app from the Dock](<Hiding_a_macOS_app_from_the_Dock.md> "Hiding a macOS app from the Dock")
  * [macOS Application Dock Menu](<macOS_Application_Dock_Menu.md> "macOS Application Dock Menu")
  * [Show Badge on Application Icon in Dock](<Show_Badge_on_Application_Icon_in_Dock.md> "Show Badge on Application Icon in Dock")



## External links

  * [Apple documentation: requestUserAttention](<https://developer.apple.com/documentation/appkit/nsapplication/1428358-requestuserattention?language=objc>)

---

_Source: [https://wiki.freepascal.org/Bounce_Application_Icon_in_Dock](https://web.archive.org/web/20230203013939/https://wiki.freepascal.org/Bounce_Application_Icon_in_Dock)_
