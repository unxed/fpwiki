# macOS Play Alert Sound

│ **English (en)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Alert Sound Identifiers
  * 3 Result codes
  * 4 Example code
  * 5 Playing an alert sound from a file
  * 6 See also
  * 7 External links



## Overview

The Apple macOS operating system has many possible alert sounds which you can play to alert your user to various situation that need their attention. You can use System Sound Services to play short (30 seconds or shorter) sounds. The interface does not provide level, positioning, looping, or timing control, and does not support simultaneous playback: You can play only one sound at a time and the sound only plays once. You can, however, use System Sound Services to provide audible alerts. 

System Sound Services supported audio formats: 

  * It only supports audio data formats linear PCM or IMA4.
  * It only supports audio file formats CAF, AIF, or WAV.



If these limitations on System Sound Services are an issue, check out the AVFoundation [AVAudioPlayer](<macOS_Audio_Player.md> "macOS Audio Player") which does not suffer from the limitations. 

## Alert Sound Identifiers

These are the AudioToolbox identifiers for System Sound Services alert sounds and alternatives to sounds, for use with the AudioServicesPlayAlertSound: 

Identifier | Description   
---|---  
kSystemSoundID_Vibrate | On the iPhone, use this constant with the AudioServicesPlayAlertSound function to invoke a brief vibration. On the iPod touch, does nothing.   
kSystemSoundID_UserPreferredAlert | On the desktop, use this constant with the AudioServicesPlayAlertSound function to play the alert specified in the Sound preference pane.   
kSystemSoundID_FlashScreen | On the desktop, use this constant with the AudioServicesPlayAlertSound function to display a flash of light on the screen.   
kUserPreferredAlert | Deprecated. Use kSystemSoundID_UserPreferredAlert instead.   
  
When a user has configured System Preferences to flash the screen for alerts, or if sound cannot be rendered, calling _AudioServicesPlayAlertSound_ will result in the screen flashing. In macOS, pass the constant _kSystemSoundID_UserPreferredAlert_ to play the alert sound selected by the user in System Preferences. 

## Result codes

This table lists the result codes defined for System Sound Services: 

Constant | Value | Description   
---|---|---  
kAudioServicesNoError | 0 | No error has occurred   
kAudioServicesUnsupportedPropertyError | 'pty?' | The property is not supported   
kAudioServicesBadPropertySizeError | '!siz' | The size of the property data was not correct   
kAudioServicesBadSpecifierSizeError | '!spc' | The size of the specifier data was not correct   
kAudioServicesSystemSoundUnspecifiedError | -1500 | An unspecified error has occurred   
kAudioServicesSystemSoundClientTimedOutError | -1501 | System sound client message timed out   
  
## Example code

The example application code below shows how to play those alert sounds. In this example the alert sound is being played by choosing a menu item which is just for the purposes of the demonstration. 

Note that the choice of **2** for the alert sound to play in this example is purely arbitrary. You should choose the number for the appropriate sound you want to play. Sound number 4096 is particularly interesting for deaf users. Try it :-) 

Alternatively, you can use the sound identifier variables listed in the section above. 
    
    
    unit unit1;
    
    {$mode objfpc}{$H+}
    {$linkframework AudioToolbox}
    
    interface
    
    uses
      Forms,
      Menus;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        MainMenu1: TMainMenu;
        MenuItem1: TMenuItem;
        procedure MenuItem1PlaySoundClick(Sender: TObject);
        MenuItem2: TMenuItem;
        procedure MenuItem2PlaySoundClick(Sender: TObject);
    
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    // Sound procedure declaration 
    Procedure AudioServicesPlayAlertSound (inSystemSoundID: UInt32)
       external name '_AudioServicesPlayAlertSound';
    
    // Menu item1 OnClick Event
    procedure TForm1.MenuItem1PlaySoundClick(Sender: TObject);
    begin
      AudioServicesPlayAlertSound(2);  // Use the appropriate value for the sound you want
    end;
    
    // Menu item2 OnClick Event
    procedure TForm1.MenuItem2PlaySoundClick(Sender: TObject);
    begin
      AudioServicesPlayAlertSound(kSystemSoundID_UserPreferredAlert);  // Use the user's preferred alert sound
    end;
    
    end.
    

  


## Playing an alert sound from a file

Not only can you play the existing operating system alert sounds, you can also play your own .wav sound files. The example application below demonstrates this. The Button1 OnClick event handler plays your sound from the specified file and updates the button caption with the SoundID assigned to the sound. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework AudioToolbox}
    
    interface
    
    {$DEFINE DEBUG}  // Comment if console log messages may required
    
    uses
     Classes,
     SysUtils,
     Forms,
     Controls,
     StdCtrls,
     Dialogs,
     {$IFDEF DEBUG}
     CocoaAll,
     {$ENDIF}
     MacOSAll;
    
    type
    
     TSystemSoundID = UInt32;
     pSystemSoundID = ^TSystemSoundID;
    
     { TForm1 }
    
     TForm1 = class(TForm)
       Button1: TButton;
       procedure Button1Click(Sender: TObject);
     private
    
     public
    
     end;
    
    
    var
     Form1: TForm1;
    
    
    implementation
    
    
    {$R *.lfm}
    
    // Completion handler: Executed when the sound file finishes playing
    procedure myCompletionHandler(SoundID: SystemSoundID; inClientData: CFStringRef);
    begin
      AudioServicesDisposeSystemSoundID(SoundID);  // Dispose of a system sound object and associated resources
      {$IFDEF DEBUG}
      NSLog(NSStr('Completion handler called'));
      {$ENDIF}
    end;
    
    // System Sound Services procedure and function declarations
    procedure AudioServicesPlayAlertSound (inSystemSoundID: TSystemSoundID); 
      external name '_AudioServicesPlayAlertSound';
    function AudioServicesCreateSystemSoundID(inFileURL: CFURLRef; outSystemSoundID: pSystemSoundID): OSStatus; 
      external name '_AudioServicesCreateSystemSoundID';
    function AudioServicesAddSystemSoundCompletion (inSystemSoundID: TSystemSoundID; inRunLoop: CFRunLoopRef; inRunLoopMode: CFStringRef; inCompletionRoutine: Pointer; inClientData: CFURLRef): OSStatus; cdecl; 
      external name 'AudioServicesAddSystemSoundCompletion';
    function AudioServicesDisposeSystemSoundID(inSystemSoundID: TSystemSoundID): OSStatus; cdecl; 
      external name '_AudioServicesDisposeSystemSoundID';
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
       newSoundID: TSystemSoundID;
       filePathURL: CFURLRef;
       filePathCFStringRef: CFStringRef;
       filePathStr: String;
       status: Integer = -1;
    begin
     // Sound file to play
     filePathStr := '/usr/local/share/fpcsrc/fpc-3.0.4/packages/libndsfpc/examples/audio/maxmod/basic_sound/audio/Ambulance.wav';
     filePathCFStringRef := CFStringCreateWithPascalString(kCFAllocatorDefault, filePathStr, CFStringGetSystemEncoding);
     filePathURL := CFURLCreateWithFileSystemPath(kCFAllocatorDefault, filePathCFStringRef, kCFURLPOSIXPathStyle, false);
    
     // Create a new system sound object
     status := AudioServicesCreateSystemSoundID(filePathURL, @newSoundID); 
     if(status = kAudioServicesNoError) then
       begin
          Button1.Caption := IntToStr(newSoundID);
    
          // Register a callback function, invoked when specified system sound finishes playing
          status := AudioServicesAddSystemSoundCompletion(newSoundID, Nil, Nil, @myCompletionHandler, Nil);
          if(status <> kAudioServicesNoError) then
            ShowMessage('Error in AudioServicesAddSystemSoundCompletion() : ' + IntToStr(status));
    
          // Play sound
          AudioServicesPlayAlertSound(newSoundID);
       end
     else
       ShowMessage('Error in AudioServicesCreateSystemSoundID() : ' + IntToStr(status));
    end;
    
    end.
    

In the code above we are disposing of the system sound object and its associated resources in the completion handler procedure. You will not want to do this if you wish to replay the sound using it SoundID at a later time in the application (eg in a game). 

## See also

  * [AVAudioPlayer](<macOS_Audio_Player.md> "macOS Audio Player")
  * [macOS NSSound](<macOS_NSSound.md> "macOS NSSound")
  * [NSBeep](<macOS_Programming_Tips.md> "macOS Programming Tips")
  * [macOS Sound Utilities](<macOS_Sound_Utilities.md> "macOS Sound Utilities")
  * [Multimedia Programming](<Multimedia_Programming.md> "Multimedia Programming")



## External links

  * [Apple: Working with Audio](<https://developer.apple.com/audio/>)
  * [Apple: System Sound Services](<https://developer.apple.com/documentation/audiotoolbox/system_sound_services>)

---

_Source: [https://wiki.freepascal.org/macOS_Play_Alert_Sound](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/macOS_Play_Alert_Sound)_
