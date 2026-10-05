# macOS Text-To-Speech

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 Overview
    * 1.1 Using code
    * 1.2 Using utilities
  * 2 Generating speech from code
    * 2.1 Using Speech Synthesis Manager
    * 2.2 Using NSSpeechSynthesizer
    * 2.3 Using AVSpeechSynthesis
  * 3 Generating speech using utilities
    * 3.1 Using /usr/bin/say
    * 3.2 Using osascript
  * 4 See also
  * 5 External links



## Overview

This article covers some ways of generating speech from text strings using the native macOS text-to-speech facilities. This is known as text-to-speech (TTS) or speech synthesis. This synthesized speech is an essential aid to those users with attention or vision disabilities. It is also useful when you want to draw the user’s attention to something important when they might be distracted by something else on the screen. For example, you may want your application to incorporate the capability to speak its dialog box messages to the user. 

There are two distinct methods available to macSO developers: using code to generate speech from text and using utility tools provided with the operating system. 

### Using code

There are three options. In their order of evolution, they are: 

  * The Speech Synthesis Manager, formerly called the Speech Manager, is a standardized method for macOS applications to generate synthesized speech. The Speech Synthesis Manager is available from macOS 10.0+ (Cheetah) although some of its functions have been deprecated and some new ones were introduced with macOS 10.5 (Leopard).


  * Using an NSSpeechSynthesizer object, you can make your application speak a word, phrase, or sentence to the user. NSSpeechSynthesizer is available from macOS 10.3+ (Panther). The Cocoa interface to speech synthesis in macOS.


  * Using the AVSpeechSynthesizer object that produces synthesized speech from text utterances and provides controls for monitoring or controlling the progress of ongoing speech. Available from macOS 10.14+ (Mojave).



### Using utilities

There are two utility tools provided with macOS that enable you to generate speech from text: 

  * The `say` utility;
  * The `osacscript` utility.



## Generating speech from code

### Using Speech Synthesis Manager

The Speech Synthesis Manager, formerly called the Speech Manager, is the part of the macOS that provides a standardized method for applications to generate synthesized speech. While it is the easiest method to use, a number of its functions are now deprecated (see the Apple documentation below for details). 

A simple example using the _SpeakString_ function: 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Dialogs, StdCtrls, MacOSAll;
    
    type
    
      { TForm1 }
    
      TForm1 = Class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    function mySpeechMgrPresent: OSErr;
    var
       myErr    : OSErr;
       myFeature: LongInt = 0;
    begin
    {Feature being tested}
    myerr := Gestalt(gestaltSpeechAttr, myFeature);
    
    {Test Speech Manager present bit}
    if (myErr = noErr) and ((((myFeature) shr (gestaltSpeechMgrPresent)) and 1)=1) then
      begin
        myErr := SpeakString('The Speech Manager is working and is almost done.');
    
        {Wait until synthesizer is done speaking}
        while (SpeechBusy <> 0) do
          begin
            Application.ProcessMessages;
          end;
    
        mySpeechMgrPresent := myErr;
      end
    else
      mySpeechMgrPresent := -1;
    end;
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      if(mySpeechMgrPresent = noErr) then
        ShowMessage('Speech Manager present: No error')
      else
        ShowMessage('Speech Manager not present: Error');
    
    end;
    
    end.
    

Unfortunately the _SpeakString_ function was deprecated in macOS 10.8 (Mountain Lion) - it is also limited to Pascal strings (ie a string length of 255 characters). The replacement function _SpeakCFString_ introduced in macOS 10.5 (Leopard) is a little more complicated, as demonstrated in the reworked example below, and is not limited to 255-character strings: 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Dialogs, StdCtrls, MacOSAll;
    
    type
    
      { TForm1 }
    
      TForm1 = Class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    function MySpeechMgrPresent: OSErr;
    var
       myErr    : OSErr;
       myFeature: LongInt = 0;
       mySpChan : SpeechChannelRecordPtr = Nil;
    begin
    {Feature being tested}
    myerr := Gestalt(gestaltSpeechAttr, myFeature);
    
    {Test Speech Manager present bit}
    if (myErr = noErr) and ((((myFeature) shr (gestaltSpeechMgrPresent)) and 1)=1) then
      begin
        myErr := NewSpeechChannel(Nil, mySpChan);
        myErr := SpeakCFString(mySpChan, CFStr('The Speech Manager is working and is almost done.'), Nil);
    
        {Wait until synthesizer is done speaking}
        while (SpeechBusy <> 0) do
          begin
            Application.ProcessMessages;
          end;
    
        mySpeechMgrPresent := myErr;
      end
    else
      mySpeechMgrPresent := -1;
    end;
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      if(mySpeechMgrPresent = noErr) then
        ShowMessage('Speech Manager present: No error')
      else
        ShowMessage('Speech Manager not present: Error');
    end;
    
    end.
    

See the Apple documentation below for more details of the Speech Synthesis Manager's functionality. 

### Using NSSpeechSynthesizer

Using an NSSpeechSynthesizer object to “pronounce” text is easy. You initialize the object with a voice and send a startSpeakingString: message to it, passing in an NSString object representing the text to speak. 

Source: [forum post by kogs](<http://forum.lazarus.freepascal.org/index.php/topic,23890.msg143222.html#msg143222>) and contributions by skalogryz (updated to compile with Lazarus 2.1 and FPC 3.3.1 by Trev): 

The code for the speech synthesis object, speechsynthesizer.pas: 
    
    
    { Parsed from Appkit.framework NSSpeechSynthesizerSpeechSynthesize.h }
    unit speechsynthesizer;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    {$linkframework AppKit}
    
    uses
      Classes, MacOSAll, CocoaUtils; //, CarbonProc; Omit for Cocoa compilation
    
    type
    
    { NSSpeechSynthesizer }
    
      NSSpeechSynthesizer = objcclass external (NSObject)
      private
        _privateNSSpeechSynthesizerVars: id;
        // function and procedure
        // init with voice
        function initWithVoice(voice_: NSString): id; message 'initWithVoice:';
        // Speaking
        function startSpeakingString(string_: NSString): Boolean; message 'startSpeakingString:';
    //    function startSpeakingString_toURL(string_: NSString; url: NSURL): Boolean; message 'startSpeakingString:toURL:';
        function isSpeaking: Boolean; message 'isSpeaking';
        procedure stopSpeaking; message 'stopSpeaking';
    //    procedure stopSpeakingAtBoundary(boundary: NSSpeechBoundary); message 'stopSpeakingAtBoundary:';
    //    procedure pauseSpeakingAtBoundary(boundary: NSSpeechBoundary); message 'pauseSpeakingAtBoundary:';
        procedure continueSpeaking; message 'continueSpeaking';
        function delegate: NSObject; message 'delegate';
        procedure setDelegate(anObject: NSObject); message 'setDelegate:';
        // voice
        function voice: NSString; message 'voice';
        function setVoice(voice_: NSString): Boolean; message 'setVoice:';
        // rate
        function rate: single; message 'rate';
        procedure setRate(rate_: single); message 'setRate:';
        // volume
        function volume: single; message 'volume';
        procedure setVolume(volume_: single); message 'setVolume:';
    //    function usesFeedbackWindow: Boolean; message 'usesFeedbackWindow';
    //    procedure setUsesFeedbackWindow(flag: Boolean); message 'setUsesFeedbackWindow:';
    //    procedure addSpeechDictionary(speechDictionary: NSDictionary); message 'addSpeechDictionary:';
        // phonemes from text
        function phonemesFromText(text: NSString): NSString; message 'phonemesFromText:';
    //    function objectForProperty_error(property_: NSString; outError: NSErrorPointer): id; message 'objectForProperty:error:';
    //    function setObject_forProperty_error(object_: id; property_: NSString; outError: NSErrorPointer): Boolean; message 'setObject:forProperty:error:';
    //    class function isAnyApplicationSpeaking: Boolean; message 'isAnyApplicationSpeaking';
    //    class function defaultVoice: NSString; message 'defaultVoice';
    //    class function availableVoices: NSArray; message 'availableVoices';
    //    class function attributesForVoice(voice_: NSString): NSDictionary; message 'attributesForVoice:';
      end;
    
    
      TSpeechDelegate = objcclass;
    
      { TSpeechSynthesize }
    
      TSpeechSynthesizer = class
      private
        fOnFinish : TNotifyEvent;
        // Speech are the Synthesizer and the Delegate
        SS: NSSpeechSynthesizer;
        Del: TSpeechDelegate;
        function NSStr(const stringA: String): NSString;
        // Voice
        function GetVoice: String;
        procedure SetVoice(stringVoice: String);
        // Rate
        function GetRate: Integer;
        procedure SetRate(integerRate: Integer);
        // Volume
        function GetVolume: Integer;
        procedure SetVolume(integerVolume: Integer);
        // AllocDelegate
        procedure AllocDelegate;
      protected
        procedure DoFinishedSpeaking;
      public
        constructor Create;
        constructor Create(stringVoice: String);
        destructor Destroy; override;
        // Speaking
        function StartSpeakingString(stringA: String): Boolean;
        procedure StopSpeaking;
        function IsSpeaking: Boolean;  // speaking is yes/no
        procedure ContinueSpeaking;
        // Phonemes from text
        function PhonemesFromText(stringText: String): String;
        // Voice
        property Voice: String read GetVoice write SetVoice;
        // Rate
        property Rate: Integer read GetRate write SetRate;
        // Volume
        property Volume: Integer read GetVolume write SetVolume;
        // Notification on end of speech
        property OnFinish: TNotifyEvent read fOnFinish write fOnFinish;
      end;
    
      { TSpeechDelegate }
    
      TSpeechDelegate = objcclass(NSObject)
      public
        Obj : TSpeechSynthesizer;
        procedure SpeechSynthesizer_DidFinishSpeaking(sender: NSSpeechSynthesizer;
          finishedSpeakingSuccess: Boolean); message
          'speechSynthesizer:didFinishSpeaking:';
        {procedure speechSynthesizer_willSpeakWord_ofString(sender: NSSpeechSynthesizer; characterRange: NSRange; string_: NSString); message 'speechSynthesizer:willSpeakWord:ofString:';
        procedure speechSynthesizer_willSpeakPhoneme(sender: NSSpeechSynthesizer; phonemeOpcode: cshort); message 'speechSynthesizer:willSpeakPhoneme:';
        procedure speechSynthesizer_didEncounterErrorAtIndex_ofString_message(sender: NSSpeechSynthesizer; characterIndex: NSUInteger; string_: NSString; message: NSString); message 'speechSynthesizer:didEncounterErrorAtIndex:ofString:message:';
        procedure speechSynthesizer_didEncounterSyncMessage(sender: NSSpeechSynthesizer; message: NSString); message 'speechSynthesizer:didEncounterSyncMessage:';}
      end;
    
    function NSStringToString(ns: NSString): String;
    
    implementation
    
    {-------------------------------------------------------------------------------
        function NSStringToString
    -------------------------------------------------------------------------------}
    function NSStringToString(ns: NSString): String;
    begin
      Result := CFStringToStr(CFStringRef(ns));
    end;
    
    //------------------------------------------------------------------------------
    
    { TSpeechDelegate }
    
    {-------------------------------------------------------------------------------
        procedure SpeechSynthesizer_DidFinishSpeaking
    -------------------------------------------------------------------------------}
    procedure TSpeechDelegate.SpeechSynthesizer_DidFinishSpeaking(
      sender: NSSpeechSynthesizer; finishedSpeakingSuccess: Boolean);
    begin
      if Assigned(Obj) then Obj.DoFinishedSpeaking;
    end;
    
    //------------------------------------------------------------------------------
    
    { TSpeechSynthesize }
    
    {-------------------------------------------------------------------------------
        function NSStr
    -------------------------------------------------------------------------------}
    function TSpeechSynthesizer.NSStr(const stringA: String): NSString;
     begin
       // converting string to NSString (CFStringRef and NSString are interchangable)
       Result := NSString(CFStr(PChar(stringA)));
     end;
    
    {-------------------------------------------------------------------------------
        function StartSpeakingString
    -------------------------------------------------------------------------------}
    function TSpeechSynthesizer.StartSpeakingString(stringA: String): Boolean;
    begin
      Result := SS.startSpeakingString(NSStr(stringA));
    end;
    
    {-------------------------------------------------------------------------------
        procedure StopSpeaking
    -------------------------------------------------------------------------------}
    procedure TSpeechSynthesizer.StopSpeaking;
    begin
      SS.stopSpeaking;
    end;
    
    {-------------------------------------------------------------------------------
        function IsSpeaking
    -------------------------------------------------------------------------------}
    function TSpeechSynthesizer.IsSpeaking: Boolean;
    begin
      Result := False;
      Result := SS.isSpeaking;
    end;
    
    {-------------------------------------------------------------------------------
        procedure ContinueSpeakinG
    -------------------------------------------------------------------------------}
    procedure TSpeechSynthesizer.ContinueSpeaking;
    begin
      SS.continueSpeaking;
    end;
    
    {-------------------------------------------------------------------------------
        function GetVoice
    -------------------------------------------------------------------------------}
    function TSpeechSynthesizer.GetVoice: String;
    begin
      Result := NSStringToString(SS.voice);
    end;
    
    {-------------------------------------------------------------------------------
        procedure SetVoice
    -------------------------------------------------------------------------------}
    procedure TSpeechSynthesizer.SetVoice(stringVoice: String);
    begin
      SS.setVoice(NSStr(stringVoice));
    end;
    
    {-------------------------------------------------------------------------------
        function GetRate
    -------------------------------------------------------------------------------}
    function TSpeechSynthesizer.GetRate: Integer;
    begin
      Result := Integer(SS.rate);
    end;
    
    {-------------------------------------------------------------------------------
        procedure SetRate
    -------------------------------------------------------------------------------}
    procedure TSpeechSynthesizer.SetRate(integerRate: Integer);
    begin
      SS.setRate(single(integerRate));
    end;
    
    {-------------------------------------------------------------------------------
        function GetVolume
    -------------------------------------------------------------------------------}
    function TSpeechSynthesizer.GetVolume: Integer;
    begin
      Result := Integer(SS.rate);
    end;
    
    {-------------------------------------------------------------------------------
        procedure SetRate
    -------------------------------------------------------------------------------}
    procedure TSpeechSynthesizer.SetVolume(integerVolume: Integer);
    begin
      SS.setRate(Integer(integerVolume));
    end;
    
    {-------------------------------------------------------------------------------
        function PhonemesFromText
    -------------------------------------------------------------------------------}
    function  TSpeechSynthesizer.PhonemesFromText(stringText: String): String;
    begin
      Result := NSStringToString(SS.phonemesFromText(NSStr(stringText)));
    end;
    
    {-------------------------------------------------------------------------------
        procedure AllocDelegate
    -------------------------------------------------------------------------------}
    procedure TSpeechSynthesizer.AllocDelegate;
    begin
      Del := TSpeechDelegate.alloc.init;
      Del.Obj:=Self;
      SS.setDelegate(Del);
    end;
    
    {-------------------------------------------------------------------------------
        procedure SetRate
    -------------------------------------------------------------------------------}
    procedure TSpeechSynthesizer.DoFinishedSpeaking;
    begin
      if Assigned(fOnFinish) then fOnFinish(Self);
    end;
    
    {-------------------------------------------------------------------------------
        constructor Create
    -------------------------------------------------------------------------------}
    constructor TSpeechSynthesizer.Create;
    begin
      inherited;
      SS := NSSpeechSynthesizer.alloc.init;
      AllocDelegate;
    end;
    
    constructor TSpeechSynthesizer.Create(stringVoice: String);
    begin
      SS := NSSpeechSynthesizer.alloc.initWithVoice(NSStr(stringVoice));
      AllocDelegate;
    end;
    
    destructor TSpeechSynthesizer.Destroy;
    begin
      Del.release;
      SS.release;
      inherited Destroy;
    end;
    
    end.
    

Example code: 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, Forms, StdCtrls, Menus, speechsynthesize; 
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        MainMenu1: TMainMenu;
        Memo1: TMemo;
        MenuItemEdit: TMenuItem;
        MenuItemEditSpeech: TMenuItem;
        MenuItemEditSpeechStart: TMenuItem;
        MenuItemEditSpeechStop: TMenuItem;
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
        procedure MenuItemEditSpeechStartClick(Sender: TObject);
        procedure MenuItemEditSpeechStopClick(Sender: TObject);
      private
        { private declarations } 
        SpeechSynthesizer: TSpeechSynthesizer;
      public
        { public declarations }
        procedure OnFinish(Sender: TObject);
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      SpeechSynthesizer := TSpeechSynthesizer.Create;
    
      // SpeechSynthesizer.OnFinish <-@OnFinish (Self)
      SpeechSynthesizer.OnFinish := @OnFinish;
    
      MenuItemEditSpeechStop.Enabled := False;
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      SpeechSynthesizer.Free;
    end;
    
    procedure TForm1.MenuItemEditSpeechStartClick(Sender: TObject);
    begin
      //  IsSpeaking - yes -> StopSpeaking
      if SpeechSynthesizer.IsSpeaking then
      begin
        SpeechSynthesizer.StopSpeaking;
        MenuItemEditSpeechStop.Enabled := False;
      end;
    
      // StartSpeakingString
      SpeechSynthesizer.StartSpeakingString(Memo1.Text);
    
      MenuItemEditSpeechStop.Enabled := True;
    end;
    
    procedure TForm1.MenuItemEditSpeechStopClick(Sender: TObject);
    begin
      SpeechSynthesizer.StopSpeaking;
    end;
    
    {  OnFinish }
    
    procedure TForm1.OnFinish(Sender: TObject);
    begin
      //  not SpeechSynthesizer.IsSpeaking - yes or no
      if not SpeechSynthesizer.IsSpeaking then
      begin
        MenuItemEditSpeechStart.Enabled := True;
        MenuItemEditSpeechStop.Enabled := False;
      end;
    end;
    
    end.
    

For more details of the NSSpeechSynthesizer object's functionality, see the Apple documentation below. 

### Using AVSpeechSynthesis

The AVSpeechSynthesizer class produces synthesized speech from text and provides methods for controlling or monitoring the progress of ongoing speech. To speak some text, you must first create an AVSpeechUtterance instance containing the text. (Optionally, you may also use the utterance object to control parameters affecting its speech, such as voice, pitch, and rate.) Then, pass it to the speakUtterance method on a speech synthesizer instance to speak that utterance. By default, AVSpeechSynthesizer will speak using a voice based on the user’s current language preferences. 

The speech synthesizer maintains a queue of utterances to be spoken. If the synthesizer is not currently speaking, calling speakUtterance: begins speaking that utterance immediately (or begin waiting through its preUtteranceDelay if one is set). If the synthesizer is speaking, utterances are added to a queue and spoken in the order they are received. 

After speech has begun, you can use the synthesizer object to pause or stop speech. After speech is paused, it may be continued from the point at which it left off; stopping ends speech entirely, removing any utterances yet to be spoken from the synthesizer’s queue. You may monitor the speech synthesizer by examining its speaking and paused properties, or by setting a delegate. Messages in the AVSpeechSynthesizerDelegate protocol are sent as significant events occur during speech synthesis. 

## Generating speech using utilities

### Using /usr/bin/say

This tool uses the Speech Synthesis manager to convert input text to audible speech and either play it through the sound output device chosen in System Preferences or save it to an AIFF file. 
    
    
    ...
    
    Uses
      Unix,
      BaseUnix,
      ...;
    
    ...
    
    procedure TForm1.MenuItem22Click(Sender: TObject);
    var
      status: LongInt;
      tts: String;
    begin
      tts := 'Hello, this is a test.';
      status := fpSystem('/usr/bin/say "' + tts + '"');
      ShowMessage('Exit status: ' + IntToStr(wexitStatus(status)));
    end;
    
    
    
    Program sayhello;
    
    Uses
      Process;
    
    Var
      AProcess:TProcess;
    
    Begin
      AProcess := TProcess.Create(nil);
      Try
        AProcess.Executable := '/usr/bin/say';
        AProcess.Parameters.Add( '-o');
        AProcess.Parameters.Add( '/Users/<you>/Desktop/hello.aiff');  // save speech to file on your desktop
        AProcess.Parameters.Add( 'hello');
        AProcess.Options := AProcess.Options + [poWaitOnExit];
        AProcess.Execute;
      Finally
        AProcess.Free;
      End;
    End.
    

For more details on how to use this command, open a Terminal and type `man say` for the manual page. 

### Using osascript

The `osascript` command line utility executes the given OSA script, which may be plain text or a compiled script (.scpt) created by the Applications > Utilities > Script Editor application or the `osacompile` command line utility. By default, `osascript` treats plain text as AppleScript, but you can change this using the -l option to JavaScript or the Generic Scripting System. To get a list of the OSA languages installed on your system, run the `osalang` command line utility. 

Example: 
    
    
    //
    // From a forum post by jwdietrich:
    //
    
    var
      s: longint;
      t: String;
    begin
      t := 'This is a test.';
      s := fpSystem('osascript -e ''say "' + t + '"'''); 
    end;
    

For more details on how to use this command, open a Terminal and type `man osascript` for the manual page. 

## See also

  * [Using the macOS screenreader](<LCL_Accessibility.md> "LCL Accessibility")
  * [Speech Synthesis](<Speech_Synthesis.md> "Speech Synthesis") Details of cross-platform and native operating system solutions.



## External links

  * [Apple: Speech Synthesis](<https://developer.apple.com/documentation/avfoundation/speech_synthesis>).
  * [Apple: Speech Manager/Speech Synthesis Manager](<https://developer.apple.com/library/archive/documentation/mac/pdf/Sound/Speech_Manager.pdf>) (PDF) Legacy Pascal code; macOS 10.0+ (with deprecations).
  * [Apple: NSSpeechSynthesizer](<https://developer.apple.com/documentation/appkit/nsspeechsynthesizer>) macOS 10.3+.
  * [Apple: AVSpeechSynthesizer](<https://developer.apple.com/documentation/avfoundation/avspeechsynthesizer>) macOS 10.14+.

---

_Source: [https://wiki.freepascal.org/macOS_Text-To-Speech](https://web.archive.org/web/20250114072524/https://wiki.freepascal.org/macOS_Text-To-Speech)_
