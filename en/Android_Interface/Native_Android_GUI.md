# Android Interface/Native Android GUI

[![Android robot.svg](https://wiki.freepascal.org/images/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

This article applies to [Android](</Category:Android> "Category:Android") only.

See also: [Multiplatform Programming Guide](<../Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Go back to [Android Interface](<../Android_Interface.md> "Android Interface")

## Contents

  * 1 Android4Pascal
  * 2 Manifest configuration
    * 2.1 Control the program restart
  * 3 Resource files
    * 3.1 Guidelines for the Icons
  * 4 Using other APIs
    * 4.1 Using the Timer
      * 4.1.1 Timer example



## Android4Pascal

See the page about [Android4Pascal](<../Android4Pascal.md> "Android4Pascal"), which is the bindings between Pascal programs and the Android Java APIs. 

  


## Manifest configuration

### Control the program restart

By default the program will restart on orientation change, on keyboard change, and in a lot of other cases. This is usually unwanted. To disable program restart on keyboard showing and orientation changed add this to the manifest file: 

` `
    
    
     <activity
        ...
        android:configChanges="orientation|keyboardHidden"
    

See also: 

  * <http://developer.android.com/guide/topics/manifest/activity-element.html#config>



## Resource files

### Guidelines for the Icons

Read here: <http://developer.android.com/guide/practices/ui_guidelines/icon_design.html>

## Using other APIs

### Using the Timer

The Android API bindings include a handy timer control called TAndroidTimer. It works just like a Runnable, and inside it a Handler class is utilized to run the Runnable in the main GUI thread so that event executed in this timer can call Android APIs. 

#### Timer example
    
    
    program turbochessclock4android;
     
    {$mode objfpc}{$H+}
     
    uses
      Classes, SysUtils, androidpipescomm, androidview, javalang,
      androidapp, androidtimer;//, gles11;
     
    type
      TEventHandler = class
      public
        procedure HandleOnTimer(ASender: TObject);
        procedure buttonClickCallback(v: TView);
      end;
     
    var
      //...
      TimerCount: Integer = 0;
      MyTimer: TAndroidTimer;
      MyEventHandler: TEventHandler;
     
    procedure TEventHandler.buttonClickCallback(v: TView);
    begin
      MyTimer.postDelayed(100);
    end;
     
    procedure TEventHandler.HandleOnTimer(ASender: TObject);
    begin
      Inc(TimerCount);
      tv.setText(Format('Timer event #%d', [TimerCount]));
      MyTimer.postDelayed(1000);
    end;
     
    begin
      MyEventHandler := TEventHandler.Create;
      // ...
    end.

---

_Source: [https://wiki.freepascal.org/Android_Interface/Native_Android_GUI](https://web.archive.org/web/20171019044003/https://wiki.freepascal.org/Android_Interface/Native_Android_GUI)_
