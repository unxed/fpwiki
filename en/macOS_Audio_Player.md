# macOS Audio Player

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Supported Audio File and Data Formats
  * 3 AVAudioPlayer
  * 4 Example Code
  * 5 See also
  * 6 External links



## Overview

The Apple AVFoundation framework combines four major technology areas: Playback and Editing, Media Capture, Audio and Speech Synthesis. Together these technologies encompass a wide range of tasks for capturing, processing, synthesising, controlling, importing and exporting audiovisual media on Apple platforms. The framework, available from macOS 10.7 (Lion), provides essential services for working with time-based audiovisual media. 

  * To play sound files, you can use AVAudioPlayer. Available from macOS 10.7 (Lion).
  * To play video (or sound) files, you can use [AVPlayer and AVPlayerLayer](<macOS_Video_Player.md> "macOS Video Player"). Available from macOS 10.7 (Lion).
  * To record audio, you can use [AVAudioRecorder](<macOS_Audio_Recorder.md> "macOS Audio Recorder"). Available from macOS 10.7 (Lion).
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
  
## AVAudioPlayer

The AVAudioPlayer class lets you play sound in any audio format available in macOS from a file or memory. The properties of this class are used for managing information about a sound such as the playback point within the sound’s timeline and for accessing playback options such as volume and looping. 

Using an audio player you can: 

  * Play sounds of any duration (unlike [System Sound Services](<macOS_Play_Alert_Sound.md> "macOS Play Alert Sound") that has a 30 second limit);
  * Play sounds from files or memory buffers;
  * Play sounds when your application is in the background;
  * Loop sounds (including indefinitely);
  * Play multiple sounds simultaneously, one sound per audio player, with precise synchronisation;
  * Control relative playback level, stereo positioning, and playback rate for each sound you are playing;
  * Seek to a particular point in a sound file, which supports such application features as fast forward and rewind; and
  * Obtain data you can use for playback-level metering.



## Example Code

The example code below provides a demo audio player application which implements the basic requirements for such an application. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Not every method in the AVAudioPlayer class has been implemented in the code below. Most methods have been implemented: play audio from a file, pause playback, resume playback, stop playback, set/get volume, set/get number of loops, set/get current time (ie forward, rewind) and set/get rate (ie speed) of playback; etc. Check the Apple documentation for the rest. The implemented methods are described in detail in the code below.
    
    
    //
    // Note: Lazarus IDE 2.2.0 and Free Pascal Compiler v3.2.2 used
    //
    
    unit unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework AVFoundation}
    
    interface
    
    uses
      Forms,     // for Form
      SysUtils,  // for FloatToStr
      StdCtrls,  // for labels, buttons
      CocoaAll,  // for Cocoa types
      ComCtrls,  // for trackbar
      ExtCtrls;  // for timer
    
    type
    
      { TForm1 }
      AVAudioPlayerPtr = ^AVAudioPlayer;
      AVAudioPlayerDelegateProtocolPtr = ^AVAudioPlayerDelegateProtocol;
    
      AVAudioPlayerDelegateProtocol = ObjCProtocol;
    
      AVAudioPlayer = objcclass external(NSObject)
      private
        _internal: id;
      public
        {Initializes and returns an audio player for playing a designated sound file.
         url: a URL identifying the sound file to play. The audio data must be in a
         format supported by Core Audio.
         outError:  If an error occurs, on return the NSError object describes the
         error. Set to NULL to ignore errors.}
         function initWithContentsOfURL_error (url: NSURL; outError: NSErrorPtr): id;
                                message 'initWithContentsOfURL:error:';
        {Plays a sound asynchronously. Calling this method implicitly calls the
         prepareToPlay() method if the audio player is not already prepared to play.}
        function play: Boolean; message 'play';
    
        {A value of 0.0 indicates silence; a value of 1.0 (the default) indicates
         full volume for the audio player instance. Use this property to control an
         audio player’s volume relative to other audio output.}
        function volume: Single; message 'volume';
        procedure setVolume(newValue: Single); message 'setVolume:';
    
        {A value of 0, which is the default, means to play the sound once.
         Set a positive integer value to specify the number of times to return
         to the start and play again. For example, specifying a value of 1
         results in a total of two plays of the sound. Set any negative
         integer value to loop the sound indefinitely until you call the
         stop() method.}
        function  numberOfLoops: Integer; message 'numberOfLoops';
        procedure setNumberOfLoops(newValue: Integer); message 'setNumberOfLoops:';
    
        {A Boolean value that indicates whether the audio player is playing (true)
         or not (false).}
        function isPlaying: Boolean; message 'isPlaying';
    
        {Calling this method preloads buffers and acquires the audio hardware
         needed for playback, which minimises the lag between calling the play()
         method and the start of sound output.}
        function prepareToPlay: Boolean; message 'prepareToPlay';
    
        {Pauses playback; sound remains ready to resume playback from where
        it left off. It does not release the audio hardware that was acquired
        on calling play() or prepareToPlay().}
        procedure pause; message 'pause';
    
        {The stop method does not reset the value of the currentTime property to 0.
         If you call stop during playback and then call play(), playback resumes
         at the point where it left off. Callikng this method undoes the setup
         performed on calling the play() or prepareToPlay() methods.}
        procedure stop; message 'stop';
    
        {The total duration, in seconds, of the sound associated with the audio
        player.}
        function duration: Double; message 'duration';
    
        {If the sound is playing, currentTime is the offset of the current playback
        position, measured in seconds from the start of the sound. If the sound is
        not playing, currentTime is the offset of where playing starts upon calling
        the play() method, measured in seconds from the start of the sound.
        By setting this property you can seek to a specific point in a sound file or
        implement audio fast-forward and rewind functions.}
        function currentTime: Double; message 'currentTime';
        procedure setCurrentTime(newValue: Double); message 'setCurrentTime:';
    
        {To enable adjustable playback rate for an audio player, set this property
         to true after you initialize the player and before you call the
         prepareToPlay() instance method for the player.}
        function enableRate: Boolean; message 'enableRate';
        procedure setEnableRate(newValue:Boolean); message 'setEnableRate:';
    
        {This property’s default value of 1.0 provides normal playback rate. The
         available range is from 0.5 for half-speed playback through 2.0 for
         double-speed playback.
         To set an audio player’s playback rate, you must first enable rate
         adjustment as described in the enableRate property description.}
        function rate: Single; message 'rate';
        procedure setRate(newValue: Single); message 'setRate:';
    
        {The UID of the current audio player.}
        function currentDevice: NSString; message 'currentDevice';
    
        {Number of audio channels in the sound associated with the audio player.}
        function numberOfChannels: Integer; message 'numberOfChannels';
    
        {The default value for the meteringEnabled property is off (Boolean false).
         Before using metering for an audio player, you need to enable it by
         setting this property to true.}
        function isMeteringEnabled: Boolean; message 'isMeteringEnabled';
        procedure setMeteringEnabled(newValue:Boolean); message 'setMeteringEnabled:';
    
        {To obtain current audio power values, you must call this method before
         calling averagePowerForChannel or peakPowerForChannel}
        procedure updateMeters; message 'updateMeters';
    
        {Returns the average power for a given channel, in decibels, for the sound
        being played.
        Channel numbers are zero-indexed. A monaural signal, or the left channel
        of a stereo signal, has channel number 0.
        A return value of 0 dB indicates full scale, or maximum power; a return
        value of –160 dB indicates minimum power (that is, near silence). If the
        signal provided to the audio player exceeds ±full scale, then the return
        value may exceed 0 (that is, it may enter the positive range).}
        function avgPwrForChannel(newValue:LongWord): Double; message 'averagePowerForChannel:';
    
        {Returns the peak power for a given channel, in decibels, for the sound
        being played.
        Channel numbers are zero-indexed. A monaural signal, or the left channel
        of a stereo signal, has channel number 0.
        A return value of 0 dB indicates full scale, or maximum power; a return
        value of –160 dB indicates minimum power (that is, near silence). If the
        signal provided to the audio player exceeds ±full scale, then the return
        value may exceed 0 (that is, it may enter the positive range).}
        function peakPwrForChannel(newValue:LongWord): Double; message 'peakPowerForChannel:';
    
        procedure setDelegate(newValue: AVAudioPlayerDelegateProtocol); message 'setDelegate:';
        function delegate: AVAudioPlayerDelegateProtocol; message 'delegate';
      end;
    
      AVAudioPlayerDelegateProtocol = objcprotocol external name 'AVAudioPlayerDelegate' (NSObjectProtocol)
       optional
       procedure audioPlayerDidFinishPlaying_successfully (player: AVAudioPlayer; flag: ObjCBOOL); message 'audioPlayerDidFinishPlaying:successfully:';
       procedure audioPlayerDecodeErrorDidOccur_error (player: AVAudioPlayer; error: NSError); message 'audioPlayerDecodeErrorDidOccur:error:';
      end;
    
      { TMyDelegate }
      TMyDelegate = objcclass(NSObject, AVAudioPlayerDelegateProtocol)
      public
        procedure audioPlayerDidFinishPlaying_successfully(player: AVAudioPlayer; flag: ObjCBOOL); message 'audioPlayerDidFinishPlaying:successfully:';
        //procedure audioPlayerDecodeErrorDidOccur_error(player: AVAudioPlayer; error: NSError); message 'audioPlayerDecodeErrorDidOccur:error:';
      end;
    
      TForm1 = class(TForm)
        DurationLabel: TLabel;
        Timer1: TTimer;
        TitleLabel: TLabel;
        ResumeButton: TButton;
        StopButton: TButton;
        PauseButton: TButton;
        PlayButton: TButton;
        TrackBar1: TTrackBar;
        procedure PauseButtonClick(Sender: TObject);
        procedure PlayButtonClick(Sender: TObject);
        procedure ResumeButtonClick(Sender: TObject);
        procedure StopButtonClick(Sender: TObject);
        procedure Timer1Update(Sender: TObject);
    
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
      MyAudioPlayer : AVAudioPlayer = Nil;
      fileDuration: NSTimeInterval = 0;
      myDelegate: TMyDelegate = Nil;
    
    implementation
    
    {$R *.lfm}
    
    procedure TMyDelegate.audioPlayerDidFinishPlaying_successfully(player: AVAudioPlayer; flag: ObjCBool);
    begin
      if(flag) then
        begin
          // Set button states
          Form1.PlayButton.Enabled := True;
          Form1.PauseButton.Enabled := False;
          Form1.ResumeButton.Enabled := False;
          Form1.StopButton.Enabled := False;
        end;
    end;
    
    // Play audio procedure
    procedure PlayAudio(audioFileName : NSString; numberOfLoops: Integer);
    var
      path: NSString;
      url : NSURL;
      err : NSError;
    begin
      // Path to your application bundle's resource directory
      // with the filename appended
      path := NSBundle.mainBundle.resourcePath.stringByAppendingPathComponent(audioFileName);
      url  := NSURL.fileURLWithPath(path);
    
      // Setup audio player with file
      MyAudioPlayer := AVAudioPlayer.alloc.initWithContentsOfURL_error(url, @err);
    
      // Setup labels
      Form1.TitleLabel.Caption := 'Audio filename: Minuet in G.mp3';
      Form1.DurationLabel.Caption := 'Duration: ' + FloatToStr(Trunc(MyAudioPlayer.duration)) + ' seconds';
    
      // Setup delegate
      myDelegate := TMyDelegate.alloc.init;
      MyAudioPlayer.setDelegate(myDelegate);
    
      // Setup and play
      if Assigned(MyAudioPlayer) then
        begin
          Form1.TrackBar1.Max := Trunc(MyAudioPlayer.duration);
          MyAudioPlayer.setNumberOfLoops(numberOfLoops);
          Form1.Timer1.Enabled := True;
          MyAudioPlayer.play
        end
      else
        // Use the Applications > Utilities > Console application to find error messages
        NSLog(NSStr('Error in procedure PlayAudio(): %@'), err);
    end;
    
    procedure TForm1.PlayButtonClick(Sender: TObject);
    begin
      // Destroy the existing audio player if any before
      // creating another in PlayAudio procedure
      if Assigned(MyAudioPlayer) then
        begin
          MyAudioPlayer.release;
          MyAudioPlayer := Nil;
        end;
    
      // Set button states
      PlayButton.Enabled := False;
      ResumeButton.Enabled :=  False;
      PauseButton.Enabled := True;
      StopButton.Enabled := True;
    
      // Play audio file located in your application bundle Resources directory
      // This particular demo file can be found on your Mac (up to Mojave) at:
      // /Applications/iPhoto.app/Contents/Resources/Music/Minuet in G.mp3
      // and should be copied to your application bundle Resources directory.
      PlayAudio(NSStr('Minuet in G.mp3'), 0);
    end;
    
    // Pause audio
    procedure TForm1.PauseButtonClick(Sender: TObject);
    begin
      if(MyAudioPlayer.isPlaying) then
        begin
          MyAudioPlayer.Pause;
    
          // Stop timer updating track bar
          Timer1.Enabled := False;
    
          // Set button states
          PlayButton.Enabled := False;
          PauseButton.Enabled := False;
          ResumeButton.Enabled :=  True;
          StopButton.Enabled := False;
        end;
    end;
    
    // Resume audio
    procedure TForm1.ResumeButtonClick(Sender: TObject);
    begin
      // Note: This will restart the audio where it left off after a pause
      //       unless audio has been reset to zero
      // Do not create yet another audio player by calling PlayAudio() again :-)
      if(MyAudioPlayer.currentTime <> 0) then
        begin
          MyAudioPlayer.play;
    
          // Re-start timer updating track bar
          Timer1.Enabled := True;
    
          // Set button states
          PlayButton.Enabled := False;
          ResumeButton.Enabled :=  False;
          PauseButton.Enabled := True;
          StopButton.Enabled := True;
        end;
    end;
    
    // Stop audio
    procedure TForm1.StopButtonClick(Sender: TObject);
    begin
      // if playing then stop, otherwise ignore
      if(MyAudioPlayer.IsPlaying) then
        begin
          MyAudioPlayer.stop;               // audio can still be resumed, so...
          MyAudioPlayer.setCurrentTime(0);  // reset audio to start
    
          // Set button states
          PlayButton.Enabled := True;
          PauseButton.Enabled := False;
          ResumeButton.Enabled := False;
          StopButton.Enabled := False;
    
          // Stop timer, reset trackbar position and duration
          Timer1.Enabled := False;
          TrackBar1.Position := 0;
          DurationLabel.Caption := 'Duration: ' + FloatToStr(Trunc(MyAudioPlayer.duration)) + ' seconds';
        end;
    end;
    
    procedure TForm1.Timer1Update(Sender: TObject);
    begin
      // Update trackbar pos and duration
      TrackBar1.Position := Trunc(MyAudioPlayer.currentTime);
      DurationLabel.Caption := 'Duration: ' + FloatToStr(Trunc(MyAudioPlayer.Duration - MyAudioPlayer.currentTime)) + ' seconds';
    end;
    
    finalization
    
    // Cleanup
    If (Assigned (MyAudioPlayer)) then
      begin
        MyAudioPlayer.Release;
        MyDelegate.Release;
        MyAudioPlayer := Nil;
      end;
    
    end.
    

The full project source code for this application is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_avaudioplayer/>). 

## See also

  * [System Sound Services](<macOS_Play_Alert_Sound.md> "macOS Play Alert Sound")
  * [macOS Audio Recorder](<macOS_Audio_Recorder.md> "macOS Audio Recorder")
  * [macOS Video Player](<macOS_Video_Player.md> "macOS Video Player") \- also plays audio (especially streaming audio).
  * [macOS MIDI Player](<macOS_MIDI_Player.md> "macOS MIDI Player")
  * [macOS NSSound](<macOS_NSSound.md> "macOS NSSound")
  * [macOS Sound Utilities](<macOS_Sound_Utilities.md> "macOS Sound Utilities")
  * [Multimedia Programming](<Multimedia_Programming.md> "Multimedia Programming")



## External links

  * [Apple: Working with Audio](<https://developer.apple.com/audio/>)
  * [Apple: AVFoundation Framework](<https://developer.apple.com/documentation/avfoundation>)
  * [Apple: AVAudioPlayer](<https://developer.apple.com/documentation/avfoundation/avaudioplayer>)

---

_Source: [https://wiki.freepascal.org/macOS_Audio_Player](https://web.archive.org/web/20220702215528/https://wiki.freepascal.org/macOS_Audio_Player)_
