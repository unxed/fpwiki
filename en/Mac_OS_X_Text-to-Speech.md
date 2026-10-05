# Mac OS X Text-to-Speech

[![Logo OSX.png](https://wiki.freepascal.org/images/7/7a/Logo_OSX.png)](</File:Logo_OSX.png>)

This article applies to [Mac OS X](</Category:Mac_OS_X> "Category:Mac OS X") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Speech synthesis from code
  * 3 Using osascript
  * 4 See also



## Overview

This article covers some ways of outputing speech based on text strings on Mac OS X. OS X has a native text to speech engine, but external programs like espeak can also be used. 

## Speech synthesis from code

Source: [forum post by kogs](<http://forum.lazarus.freepascal.org/index.php/topic,23890.msg143222.html#msg143222>) and contributions by skalogryz: 

The code calling speech **synthesis** : speechsynthesizer.pas: 
    
    
    { Parsed from Appkit.framework NSSpeechSynthesizerSpeechSynthesize.h }
    unit speechsynthesizer;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    {$linkframework AppKit}
    
    uses
      Classes, SysUtils, CocoaAll, MacOSAll, CarbonProc;
    
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
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      Menus, speechsynthesizer;
    
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

  


## Using osascript

From a forum post by jwdietrich: 
    
    
    var
      s: longint;
      t: String;
    begin
      t := 'This is a test.';
      s := fpSystem('osascript -e ''say "' + t + '"'''); 
    end;

## See also

  * [espeak](<espeak.md> "espeak") cross platform text to speech
  * [LCL_Accessibility#Mac_OS_X_VoiceOver](<LCL_Accessibility.md> "LCL Accessibility") Using the OS X screenreader
  * [SAPI](<SAPI.md> "SAPI") Text to speech on Windows

---

_Source: [https://wiki.freepascal.org/Mac_OS_X_Text-to-Speech](https://web.archive.org/web/20190821101926/https://wiki.freepascal.org/Mac_OS_X_Text-to-Speech)_
