# macOS Audio Recorder

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Supported Audio File and Data Formats
  * 3 AVAudioRecorder
  * 4 Example Code
  * 5 See also
  * 6 External links



## Overview

The Apple AVFoundation framework combines four major technology areas: Playback and Editing, Media Capture, Audio and Speech Synthesis. Together these technologies encompass a wide range of tasks for capturing, processing, synthesising, controlling, importing and exporting audiovisual media on Apple platforms. The framework, available from macOS 10.7 (Lion), provides essential services for working with time-based audiovisual media. 

  * To play sound files, you can use [AVAudioPlayer](<macOS_Audio_Player.md> "macOS Audio Player"). Available from macOS 10.7 (Lion).
  * To play video (or sound) files, you can use [AVPlayer and AVPlayerLayer](<macOS_Video_Player.md> "macOS Video Player"). Available from macOS 10.7 (Lion).
  * To record audio, you can use AVAudioRecorder. Available from macOS 10.7 (Lion).
  * To play MIDI or iMelody files, you can use [AVMIDIPlayer](<macOS_MIDI_Player.md> "macOS MIDI Player"). Available from macOS 10.10 (Yosemite).



## Supported Audio File and Data Formats

Sound File Format  | Sound Data Formats   
---|---  
AAC (.aac, .adts)  | 'aac '   
AC3 (.ac3)  | 'ac-3'   
AIFC (.aif, .aiff,.aifc)  | BEI8, BEI16, BEI24, BEI32, BEF32, BEF64, 'ulaw', 'alaw', 'MAC3', 'MAC6', 'ima4' , 'QDMC', 'QDM2', 'Qclp', 'agsm'   
AIFF (.aiff)  | BEI8, BEI16, BEI24, BEI32   
Apple Core Audio Format (.caf)  | '.mp3', 'MAC3', 'MAC6', 'QDM2', 'QDMC', 'Qclp', 'Qclq', 'aac ', 'agsm', 'alac', 'alaw', 'drms', 'dvi ', 'ima4', 'lpc ', BEI8, BEI16, BEI24, BEI32, BEF32, BEF64, LEI16, LEI24, LEI32, LEF32, LEF64, 'ms\x00\x02', 'ms\x00\x11', 'ms\x001', 'ms\x00U', 'ms \x00', 'samr', 'ulaw'   
MPEG Layer 3 (.mp3)  | '.mp3'   
MPEG 4 Audio (.mp4)  | 'aac '   
MPEG 4 Audio (.m4a)  | 'aac ', alac'   
NeXT/Sun Audio (.snd, .au)  | BEI8, BEI16, BEI24, BEI32, BEF32, BEF64, 'ulaw'   
Sound Designer II (.sd2)  | BEI8, BEI16, BEI24, BEI32   
WAVE (.wav)  | LEUI8, LEI16, LEI24, LEI32, LEF32, LEF64, 'ulaw', 'alaw'   
  
## AVAudioRecorder

The AVAudioRecorder class is intended to allow you to make audio recordings straight to a file with very little programming overhead. Using audio recorder, you can: 

  * Record until the user stops the recording;
  * Record for a specified duration;
  * Pause and resume a recording;
  * Obtain input audio-level data that you can use to provide level metering.



In macOS, the audio comes from the system’s default audio input device as set by a user in System Preferences. To configure a recording, including options such as bit depth, bit rate, and sample rate conversion quality, configure the audio recorder’s settings dictionary. 

The format in which the AVAudioRecorder will save your file depends on the URL/string you specify for the file you save. Specifically, it depends on the extension of the file name you are saving. If you don't specify any extension or specify one that the AVAudioRecorder does not understand then it will wrap the file in Core Audio Format container. So, see to it that the extension and the AVFormatIDKey match. 

## Example Code

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Not every method in the AVAudioRecorder class has been implemented in the code below. Most methods have been implemented. Check the Apple documentation for the rest. The implemented methods are described in detail in the code below. This code has been tested working on a 2018 Mac mini running Mojave with USB microphone.
    
    
    //
    // Note: Free Pascal v3.3.1 (trunk), revision r42644, was used 6-Jan-2019
    //
    
    unit unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework AVFoundation}
    
    interface
    
    uses                          
    ...
      MacOSAll,
      CocoaAll,
      LCLType,
    ...
    
    type
    
      { TForm1 }
    
      AVAudioRecorder = objcclass external(NSObject)
      public
        {Initializes and returns an audio recorder.
        url: File system location to record to. File type to record to is inferred
        from the file extension included in this parameter’s value.
        settings: Settings for the recording session.
        outError: Returns, by-reference, a description of the error, if an error
        occurs. Pass nil to ignore the error.}
        function initWithURL_settings_error (url: NSURL; settings: NSDictionary; outError: NSErrorPtr):id;
                                 message 'initWithURL:settings:error:';
    
        {Calling this method implicitly calls prepareToRecord(), which creates (or
        erases) an audio file and prepares the system for recording.}
        function avRecord: Boolean; message 'record';
    
        {Creates an audio file at the location specified by the url parameter in the
        init() method. If a file already exists at that location, this method
        overwrites it. The preparation invoked by this method takes place
        automatically when you call record(). Use prepareToRecord when you want
        recording to start as quickly as possible upon calling record().}
        function prepareToRecord: Boolean; message 'prepareToRecord';
    
        {Pauses a recording. Call record() to resume recording.}
        procedure pause; message 'pause';
    
        {Stops recording and closes the audio file.}
        procedure stop; message 'stop';
    
        {Deletes a recorded audio file. The audio recorder must be stopped before
        you call this method.}
        function deleteRecording: Boolean; message 'deleteRecording';
    
        {A Boolean value that indicates whether the audio recorder is recording.}
        function isRecording: Boolean; message 'isRecording';
    
        {Audio recorder settings are in effect only after you explicitly call the
        prepareToRecord() method, or after you call it implicitly by starting
        recording.}
        function settings: NSDictionary; message 'settings';
    
        {The time, in seconds, since the beginning of the recording.}
        function currentTime: NSTimeInterval; message 'currentTime';
    
        {By default, audio level metering is off for an audio recorder. Because
        metering uses computing resources, turn it on only if you intend to use it.}
        procedure setMeteringEnabled(newValue: ObjCBOOL); message 'setMeteringEnabled:';
    
        {A Boolean value that indicates whether audio-level metering is enabled.}
        function isMeteringEnabled: ObjCBOOL; message 'isMeteringEnabled';
    
        {To obtain current audio power values, you must call this method before you
        call averagePower(forChannel:) or peakPower(forChannel:).}
        procedure updateMeters; message 'updateMeters';
    
        {The current peak power, in decibels, for the sound being recorded. A return
        value of 0 dB indicates full scale, or maximum power; a return value of
        -160 dB indicates minimum power (that is, near silence). If the signal
        provided to the audio recorder exceeds ±full scale, then the return value
        may exceed 0 (ie enter the positive range).}
        function peakPowerForChannel (channelNumber: NSUInteger): single; message 'peakPowerForChannel:';
    
        {The current average power, in decibels, for the sound being recorded. A
        return value of 0 dB indicates full scale, or maximum power; a return value
        of -160 dB indicates minimum power (that is, near silence). If the signal
        provided to the audio recorder exceeds ±full scale, then the return value
        may exceed 0 (ie, it may enter the positive range).}
        function averagePowerForChannel (channelNumber: NSUInteger): single; message 'averagePowerForChannel:';
      end;
    
        TForm1 = class(TForm)
    
      ...
      
      private
    
      public
    
      end; 
    
    var
      Form1: TForm1;
      MyAudioRecorder : AVAudioRecorder = Nil;
    
    
    implementation
    
    {$R *.lfm}    
    
    ...
    
    // Convert NSStrings function 
    function NSStringToString(ns: NSString): String;
    var
      pathStr: shortstring;
    begin
      CFStringGetPascalString(CFStringRef(ns),@pathStr,255,CFStringGetSystemEncoding());
      Result := pathStr;
    end;
    
    // Record audio to file located in the user's ${TMPDIR}
    procedure RecordAudio(audioFileName : NSString);
    var
      path : NSString;
      url : NSURL;
      settings: NSDictionary;
      err : NSError;
    begin
      path := NSTemporaryDirectory.stringByAppendingPathComponent(audioFileName);
      url := NSURL.fileURLWithPath(path);
      settings := Nil;   
    
      MyAudioRecorder := AVAudioRecorder.alloc.initWithURL_settings_error(url, settings, @err);
    
      if Assigned(MyAudioRecorder) then
          MyAudioRecorder.avrecord
      else
        NSLog(NSStr('Error in procedure RecordAudio(): %@'), err);
    end;
    
    ...
    
    // Record audio - format mp4a inferred from filename
    procedure TForm1.MenuItem27Click(Sender: TObject);
    begin
      RecordAudio(NSStr('testaudio.mp4a'));
    end;
    
    // Pause audio recording
    procedure TForm1.MenuItem28Click(Sender: TObject);
    begin
       MyAudioRecorder.Pause;
    end;
    
    // Resume audio recording
    procedure TForm1.MenuItem29Click(Sender: TObject);
    begin
       MyAudioRecorder.avRecord;
    end; 
    
    // Stop audio recording and close file
    procedure TForm1.MenuItem30Click(Sender: TObject);
    begin
      MyAudioRecorder.Stop;
    end;   
    
    ...
    
    finalization
    
    If (Assigned (MyAudioRecorder)) then
      begin
        MyAudioRecorder.Release;
        MyAudioRecorder := Nil;
      end;
    
    end.
    

## See also

  * [macOS Audio Player](<macOS_Audio_Player.md> "macOS Audio Player")
  * [macOS Play Alert Sound](<macOS_Play_Alert_Sound.md> "macOS Play Alert Sound")
  * [macOS NSSound](<macOS_NSSound.md> "macOS NSSound")
  * [macOS MIDI Player](<macOS_MIDI_Player.md> "macOS MIDI Player")
  * [macOS Sound Utilities](<macOS_Sound_Utilities.md> "macOS Sound Utilities")
  * [Multimedia Programming](<Multimedia_Programming.md> "Multimedia Programming")



## External links

  * [Apple: Working with Audio](<https://developer.apple.com/audio/>)
  * [Apple: AVFoundation Framework](<https://developer.apple.com/documentation/avfoundation>)
  * [Apple: AVAudioRecorder](<https://developer.apple.com/documentation/avfoundation/avaudiorecorder>)

---

_Source: [https://wiki.freepascal.org/macOS_Audio_Recorder](https://web.archive.org/web/20231129023815/https://wiki.freepascal.org/macOS_Audio_Recorder)_
