# macOS MIDI Player

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 AVMidiPlayer
  * 3 Example code
  * 4 See also
  * 5 External links



## Overview

The Apple AVFoundation framework combines four major technology areas: Playback and Editing, Media Capture, Audio and Speech Synthesis. Together these technologies encompass a wide range of tasks for capturing, processing, synthesising, controlling, importing and exporting audiovisual media on Apple platforms. The framework, available from macOS 10.7 (Lion), provides essential services for working with time-based audiovisual media. 

  * To play sound files, you can use [AVAudioPlayer](<macOS_Audio_Player.md> "macOS Audio Player"). Available from macOS 10.7 (Lion).
  * To play video (or sound) files, you can use [AVPlayer and AVPlayerLayer](<macOS_Video_Player.md> "macOS Video Player"). Available from macOS 10.7 (Lion).
  * To record audio, you can use [AVAudioRecorder](<macOS_Audio_Recorder.md> "macOS Audio Recorder"). Available from macOS 10.7 (Lion).
  * To play MIDI or iMelody files, you can use AVMIDIPlayer. Available from macOS 10.10 (Yosemite).



## AVMidiPlayer

The AVMidiPlayer class lets you play music file formats such as [MIDI](<https://www.midi.org/>) and iMelody (non-polyphonic sound format created for mobile phones). It is available from macOS 10.10 (Yosemite). The properties of this class are used for managing information about a music file such as the playback point within the sound’s timeline, duration and playback rate. 

## Example code

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Requires FPC trunk (3.3.1) and macOS 10.10+ (Yosemite). 

[![avmidiplayer2.png](https://wiki.freepascal.org/images/0/0b/avmidiplayer2.png)](</File:avmidiplayer2.png>)

The example code below creates a basic MIDI file player application which plays the midi file included in the [application bundle](<Application_Bundle.md> "Application Bundle") Resources directory. This is useful in itself since Apple removed the ability to play MIDI files in macOS 10.9 (Mavericks) in 2013. While you could still download QuickTime 7 (a 32 bit application) [from Apple](<https://support.apple.com/kb/dl923>), macOS 10.15 (Catalina) removed all support for 32 bit software, which includes QuickTime 7 and all media formats and codecs relying on it. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework AVFoundation}
    {$modeswitch cblocks}
    
    //{$DEFINE DEBUG}   // Log completion handler calls to console and terminal
    
    { Requires FPC trunk (3.3.1) and macOS 10.10+ (Yosemite) }
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs,
      CocoaAll, LCLType, StdCtrls, ExtCtrls, ComCtrls;
    
    type
       tblock = reference to procedure; cdecl; cblock;
    
    type
      { AVMIDIPlayer}
    
        AVMIDIPlayer = objcclass external(NSObject)
        public
          {Initializes a newly allocated MIDI player with the contents of the file
           specified by the URL, using the specified sound bank.
           inURL    - The file to play.
           bankURL  - The URL of the sound bank. The sound bank must be a SoundFont2 or DLS bank.
                      For macOS the bankURL can be set to nil to use the default sound bank.
           outError - Returns, by-reference, a description of the error, if an error occurs.}
          function initWithContentsOfURL_error (inURL: NSURL; soundBankURL: NSURL; outError: NSErrorPtr): id;
                      message 'initWithContentsOfURL:soundBankURL:error:';
          {Prepares to play the sequence by prerolling all events.
           Happens automatically on play if it has not already been called, but may produce a
           delay in startup.}
          procedure prepareToPlay; message 'prepareToPlay';
          {Plays the sequence.
           completionHandler - A block that is executed when playback is completed or stopped.
           If prepareToPlay has not been invoked, play may be delayed while the events are prerolled.}
          //procedure play (completionHandler: AVMIDIPlayerCompletionHandler); message 'play:';
          procedure play (completionHandler: tblock); message 'play:';
          {Stops playing the sequence.}
          procedure stop; message 'stop';
          {This property is the length of the currently loaded file in seconds.}
          function duration: NSTimeInterval; message 'duration';
          {A Boolean value that indicates whether the sequence is playing.
           Note: The player may have reached the end of all the events in any of its tracks,
           but it will return YES until it is stopped.}
          function isPlaying: ObjCBOOL; message 'isPlaying';
          {This property’s default value of 1.0 provides normal playback rate. Rate must be > 0.0.}
          procedure setRate(newValue: single); message 'setRate:';
          function rate: single; message 'rate';
          {The current playback position in seconds. No range-checking is done to ensure
           currenPosityion is <= Duration.
           You can set the currentPosition of the player while the player is playing,
           in which case playback will resume at the new position.}
          procedure setCurrentPosition(newValue: NSTimeInterval); message 'setCurrentPosition:';
          function currentPosition: NSTimeInterval; message 'currentPosition';
        end;
    
      { TForm1 }
    
        TForm1 = class(TForm)
        DurationLabel: TLabel;
        ProgressBar: TProgressBar;
        TrackBarLabel: TLabel;
        RateLabel: TLabel;
        SecondsLabel: TLabel;
        PauseButton: TButton;
        StopPlayButton: TButton;
        PlayMidiButton: TButton;
        ElapsedTimer: TTimer;
        RateTrackBar: TTrackBar;
        procedure PauseButtonClick(Sender: TObject);
        procedure PlayMidiButtonClick(Sender: TObject);
        procedure RateTrackBarClick(Sender: TObject);
        procedure StopPlayButtonClick(Sender: TObject);
        procedure ElapsedTimerTimer(Sender: TObject);
        procedure RateTrackBarChange(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
      myMidiPlayer : AVMidiPlayer = Nil;
      filePos : NSTimeInterval = 0;
      fileDuration : NSTimeInterval = 0;
      myRate : Single = 1;
    
    implementation
    
    {$R *.lfm}
    
    // Executed when the file finishes playing
    // or player is paused or player is stopped
    procedure myCompletionHandler;
    begin
      myMidiPlayer.Stop;
      myMidiPlayer.setCurrentPosition(0);
    
      // Button states need to be set here when file
      // finishes playing or is paused or is stopped
      Form1.PlayMidiButton.Enabled := True;
      Form1.StopPlayButton.Enabled := False;
      Form1.PauseButton.Enabled := False;
    
      // If file finished playing or has been stopped
      // but not paused
      if(filePos = 0) then
        begin
          Form1.SecondsLabel.Caption := '0 of ' + FormatFloat('#', fileDuration) + ' seconds';
          Form1.ProgressBar.Position := 0;
          myMidiPlayer.release;   // recycle
          myMidiPlayer := Nil;    // memory
        end;
    
      {$IFDEF DEBUG}
      NSLog(NSStr('Completion handler called'));
      {$ENDIF}
    end;
    
    // Play midi procedure
    procedure PlayMidi(midiFileName : NSString);
    var
      path: NSString;
      url : NSURL;
      err : NSError;
    begin
      // Do nothing if already playing a midi file
      if(myMidiPlayer.IsPlaying) then
        exit;
    
      // If player has not been paused
      if(filePOS = 0) then
        begin
          // Path to your application bundle's resource directory
          // with the midi filename appended
          path := NSBundle.mainBundle.resourcePath.stringByAppendingPathComponent(midiFileName);
          url  := NSURL.fileURLWithPath(path);
    
          // Create MidiPlayer and load midi file
          myMidiPlayer := AVMidiPlayer.alloc.initWithContentsOfURL_error(url, Nil, @err);
    
          // Save file duration
          fileDuration := myMidiPlayer.duration;
    
          if Assigned(myMidiPlayer) then
            begin
              myMidiPlayer.setRate(myRate);
              myMidiPlayer.prepareToPlay;
              Form1.SecondsLabel.Caption := '0 of ' + FormatFloat('#', fileDuration)
                 + ' seconds';
              Form1.ProgressBar.Max:= Trunc(fileDuration);
              myMidiPlayer.play(@myCompletionHandler);
            end
          else
            // Use the Applications > Utilities > Console application to find error messages
            NSLog(NSStr('Error in procedure PlayMidi(): %@'), err);
        end
      // Otherwise resume playing existing file
      else
        begin
           myMidiPlayer.setCurrentPosition(filePos);
           Form1.SecondsLabel.Caption := FormatFloat('#', filePos) + ' of '
             + FormatFloat('#', fileDuration) + ' seconds';
           myMidiPlayer.play(@myCompletionHandler);
        end;
    end;
    
    // Play file
    procedure TForm1.PlayMidiButtonClick(Sender: TObject);
    begin
      PlayMidi(NSStr('Elvis-HoundDog.mid'));
      //PlayMidi(NSStr('whitewed.mid'));
    
      // Enable Timer for elapsed time
      ElapsedTimer.Enabled := True;
    
      // Set button states
      PlayMidiButton.Enabled := False;
      PauseButton.Enabled := True;
      StopPlayButton.Enabled := True;
    end;
    
    // Pause playback of file
    procedure TForm1.PauseButtonClick(Sender: TObject);
    begin
      // If file is playing
      if(myMidiPlayer.IsPlaying) then
        begin
          // Stop play (also calls myCompletionHandler)
          myMidiPlayer.Stop;
    
          // Save file position for resumption
          filePos := myMidiPlayer.currentPosition;
    
          // Disable elapsed timer
          ElapsedTimer.Enabled := False;
        end;
    end;
    
    // Stop playback of file
    procedure TForm1.StopPlayButtonClick(Sender: TObject);
    begin
      // If file is playing
      if(myMidiPlayer.IsPlaying) then
        begin
          // Stop play (also calls myCompletionHandler)
          myMidiPlayer.stop;
    
          // Zero file position (indicates stopped, not paused)
          filePos := 0;
        end;
    end;
    
    // Update elaspsed seconds
    procedure TForm1.ElapsedTimerTimer(Sender: TObject);
    begin
       // Stop timer if file not playing
       if(myMidiPlayer.isPlaying = False) then
         begin
           ElapsedTimer.Enabled := False;
           // Workaround to update the button status and labels
           // from myCompletionHandler() - otherwise not updated
           // in real time (up to 30 seconds later!)
           Form1.RateTrackBarClick(RateTrackBar);
         end
       // Otherwise update elapsed seconds
       // and progeess bar position
       else
         begin
           SecondsLabel.Caption := FormatFloat('#', myMidiPlayer.currentPosition)
             + ' of ' + FormatFloat('#', fileDuration)
             + ' seconds';
           ProgressBar.Position := Trunc(myMidiPlayer.currentPosition);
         end;
    end;
    
    // Workaround to update the button status and labels
    // from myCompletionHandler() - otherwise not updated
    // in real time (up to 30 seconds later!)
    procedure TForm1.RateTrackBarClick(Sender: TObject);
    begin
      // See Bug: https://bugs.freepascal.org/view.php?id=37125
      // RateTrackBar.SetFocus; causes 1 unfreed memory block of 32 bytes on exit
      RateTrackBar.Update;
      ProgressBar.Update;
    end;
    
    // Adjust playing rate of file
    procedure TForm1.RateTrackBarChange(Sender: TObject);
    begin
      If(RateTrackBar.Position = 1) then
        myRate := 0.50
      else if (RateTrackBar.Position = 2) then
        myRate := 0.60
      else if (RateTrackBar.Position = 3) then
        myRate := 0.70
      else if (RateTrackBar.Position = 4) then
        myRate := 0.80
      else if (RateTrackBar.Position = 5) then
        myRate := 0.90
      else if (RateTrackBar.Position = 6) then
        myRate := 1.00
      else if (RateTrackBar.Position = 7) then
        myRate := 1.10
      else if (RateTrackBar.Position = 8) then
        myRate := 1.20
      else if (RateTrackBar.Position = 9) then
        myRate := 1.30
      else if (RateTrackBar.Position = 10) then
        myRate := 1.40
      else
        myRate := 1.50;
    
      myMidiPlayer.setRate(myRate);
      RateLabel.Caption := FormatFloat('##.##',myRate) + 'x';
    end;
    
    end.
    

Full source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_avmidiplayer/>). 

## See also

  * [System Sound Services](<macOS_Play_Alert_Sound.md> "macOS Play Alert Sound")
  * [macOS Audio Player](<macOS_Audio_Player.md> "macOS Audio Player")
  * [macOS Audio Recorder](<macOS_Audio_Recorder.md> "macOS Audio Recorder")
  * [macOS NSSound](<macOS_NSSound.md> "macOS NSSound")
  * [macOS Sound Utilities](<macOS_Sound_Utilities.md> "macOS Sound Utilities")
  * [Multimedia Programming](<Multimedia_Programming.md> "Multimedia Programming")



## External links

  * [Apple: AVFoundation Framework](<https://developer.apple.com/documentation/avfoundation>)
  * [Apple: AVMIDIPlayer](<https://developer.apple.com/documentation/avfoundation/avmidiplayer>)

---

_Source: [https://wiki.freepascal.org/macOS_MIDI_Player](https://web.archive.org/web/20250122122352/https://wiki.freepascal.org/macOS_MIDI_Player)_
