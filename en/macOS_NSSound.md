# macOS NSSound

│ **English (en)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 NSSound Encoding and file format support
  * 2 Playing system sounds
  * 3 Playing sounds from your application bundle
  * 4 Playing sounds from elsewhere
  * 5 Managing sound playback
    * 5.1 Pause
    * 5.2 Resume
    * 5.3 Stop
    * 5.4 Volume control
    * 5.5 Loop control
    * 5.6 Sound duration
    * 5.7 Sound progress
    * 5.8 Determine if sound is playing
    * 5.9 Determine when sound finishes playing
  * 6 See also
  * 7 External links



## NSSound Encoding and file format support

NSSound supports any audio encodings and file formats that QuickTime does (as it still uses QuickTime for certain things) and, as of macOS 10.3 (Panther), it also uses CoreAudio for playback. The NSSound initializer first tries to use CoreAudio with the file it is initialized with. If that fails, NSSound tries to use QuickTime. If that fails, NSSound initialization will fail. 

NSSound will use CoreAudio for any files supported by the CoreAudio AudioFile APIs as long as the file type is supported in <AudioToolbox/AudioFile.h> and the encoding format has codec support (with the exception of Linear PCM which is always supported). The table below shows CoreAudio support. 

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
  
![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Apple ceased support for QuickTime on macOS in 2018 at which time the underlying media framework for QuickTime and QTKit was deprecated in favour of a newer graphics framework, AVFoundation. QuickTime's underlying media framework was completely removed from macOS 10.15 (Catalina).

QuickTime supports the following audio file and data formats: 

  * A-law
  * Advanced Audio Coding (AAC)
  * AMR Narrowband
  * Apple Lossless
  * Au file format
  * Audio Interchange File Format (AIFF)
  * Apple Core Audio Format (CAF)
  * FLAC (since macOS 10.13)
  * MACE
  * Microsoft Adaptive DPCM (MS ADPCM)
  * MIDI
  * MPEG-1 Audio Layer 3 (MP3)
  * Pulse-code modulation (PCM)
  * QCELP (Qualcomm PureVoice)
  * QDesign
  * Waveform Audio File Format (WAV)
  * μ-law



## Playing system sounds

Here is the list of macOS system sounds (found in `/System/Library/Sounds`): Basso, Blow, Bottle, Frog, Funk, Glass, Hero, Morse, Ping, Pop, Purr, Sosumi, Submarine, and Tink. 

A system sound can be played very easily using the NSSound class. The process is very simple: 

  1. Create an object based on the NSSound class;
  2. Load an audio file into the NSSound object using the _SoundNamed_ class method;
  3. Play the audio file using the _Play_ class method.



Here is the code to do that: 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls, CocoaAll, CocoaUtils;   
    
    ...
    
    Var
      Form1: TForm1;
      mySound: NSSound := Nil;                              // Create an object based on the NSSound class
    
    ...
    
    procedure TForm1.PlayButtonClick(Sender: TObject);
    var
      status: boolean = false;
    begin
      mySound := NSSound.soundNamed(NSStringUTF8('Frog'));  // Load an audio file into the NSSound object 
      mySound.play;                                         // Play the audio file
    
      if(not status) then                                   // Play returns true/false for success/failure
        ShowMessage('Error!'); 
    end;
    
    ...
    
    end.
    

While the Frog system sound file is named Frog.aiff, you can drop the file extension (.aiff) from the filename as we did in the above example code. 

You can also combine the loading of a sound and playing it and omit the creation of a sound object: 
    
    
    NSSound.soundNamed(NSStringUTF8('Frog')).play;          // Load and play audio file
    

## Playing sounds from your application bundle

The _soundNamed_ class method searches your application’s main [bundle](<Application_Bundle.md> "Application Bundle") and if no sound file can be located in the application main bundle, the following directories are searched in order: 

  * `~/Library/Sounds`
  * `/Library/Sounds`
  * `/Network/Library/Sounds`
  * `/System/Library/Sounds`



If no data file can be found for the sound name, no object is created and nil is returned. 

Copy the sound file from `/Applications/iPhoto.app/Contents/Resources/Music/Minuet in G.mp3` to the Resources sub-directory of your [application bundle](<Application_Bundle.md> "Application Bundle"). You can now play that sound file using the code below: 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls, CocoaAll, CocoaUtils;   
    
    ...
    
    Var
      Form1: TForm1;
      mySound: NSSound := Nil;                                     // Create an object based on the NSSound class
    
    ...
    
    procedure TForm1.PlayButtonClick(Sender: TObject);
    var
      status: boolean = false;
    begin
      mySound := NSSound.soundNamed(NSStringUTF8('Minuet in G'));  // Load an audio file into the NSSound object 
      mySound.play;                                                // Play the audio file 
    
      if(not status) then                                          // Play returns true/false for success/failure
        ShowMessage('Error!'); 
    end;
    
    ...
    
    end.
    

Note that once again the file extension is not used when loading sound files using the _soundNamed_ class method. Also note that AIFF files must use the .aiff file extension (not .aif) for it to be located by the _soundNamed_ class method. 

## Playing sounds from elsewhere

If you want to load audio files from somewhere else in the file system other than from the system sounds directory or your application bundle, you can use the NSSound class method _initWithContentsOfFile_byReference_. As you would expect, this method attempts to initialize a newly allocated NSSound instance with the audio data in the specified file. The following code example shows you how to create an NSSound instance and initialize it with a sound file using a pathname to the file. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls, CocoaAll, CocoaUtils;   
    
    ...
    
    Var
      Form1: TForm1;
      mySound: NSSound = Nil;                                                           // Create an object based on the NSSound class
    
    ...
    
    procedure TForm1.PlayButtonClick(Sender: TObject);
    var
      audioFilePath: NSString;
      status: boolean = false;
    begin
      audioFilePath := NSStringUTF8('/Applications/iPhoto.app/Contents/Resources/Music/Barbeque Blues.m4a');
      // mySound := NSSound.alloc;
      // mySound := mySound.initWithContentsOfFile_byReference(audioFilePath, True);
      // The two lines above can be combined into the single line below
      mySound := NSSound.alloc.initWithContentsOfFile_byReference(audioFilePath, True);  // Load an audio file into the NSSound object
      mySound.Play;                                                                      // Play the audio file
    
      if(not status) then                                                                // Play returns true/false for success/failure
        ShowMessage('Error!'); 
    end;                        
    
    ...
    
    end.
    

As we used _NSSound.alloc..._ in the code above, do not forget to use _mySound.Release; mySound :- Nil; mySound.dealloc_ somewhere appropriate or you'll create a memory leak every time you play the file. This is not necessary when using _NSSound.soundNamed(soundName)_ class method in the previous two examples because that method creates an NSSound object for you using a helper function or, as the Apple documentation states more opaquely the "NSSound instance is initialized with the sound data identified by soundName". 

## Managing sound playback

This section describes how to manage the playback of a sound using the NSSound class. 

### Pause
    
    
    procedure TForm1.PauseButtonClick(Sender: TObject);
    begin
      mySound.pause;
    end;
    

### Resume
    
    
    procedure TForm1.ResumeButtonClick(Sender: TObject);
    begin
      mySound.resume;
    end;
    

### Stop
    
    
    procedure TForm1.StopButtonClick(Sender: TObject);
    begin
      mySound.Stop;
    end;
    

### Volume control
    
    
    mySound.SetVolume(0.5);
    

The valid range is between 0.0 and 1.0. The value of this property does not affect the systemwide volume. Make sure you set it after loading and before playing the sound file. macOS 10.5+ required. 

### Loop control

When the value of the _loops_ instance property is True, the sound restarts playback when it finishes and does not send didFinishPlaying message to its delegate when it reaches the end of its content and restarts playback. The default value of this property is False. macOS 10.5+ required. 
    
    
    mySound.setLoops(True);   // Set true and the sound will continue looping
    

### Sound duration

The duration instance property provides the duration of the sound in seconds. macOS 10.5+ required. 
    
    
    status := mySound.play;
    
    if(status) then
      ShowMessage('Duration: ' + FloatToStrF(mySound.duration, ffFixed, 0, 2) + ' seconds')
    else
      ShowMessage('Not OK');
    

### Sound progress

The currentTime instance property returns the sound’s playback progress in seconds. A sounds starts with currentTime equal to zero and ends with currentTime equal to sound duration minus one. macOS 10.5+ required. 
    
    
    procedure TForm1.PauseButtonClick(Sender: TObject);
    begin
      mySound.pause;
    
      ShowMessage('Progress: ' + FloatToStrF(mySound.currentTime, ffFixed, 0, 2)
        + ' of ' + FloatToStrF(mySound.duration, ffFixed, 0, 2) + ' seconds');
    end;
    

### Determine if sound is playing
    
    
    if(mySound.IsPlaying) then
      ShowMessage('Sound is playing')
    else   
      ShowMessage('Sound is not playing');
    

### Determine when sound finishes playing

This is somewhat more complicated. First, create a delegate object: 
    
    
    Uses 
      ...
    
    type
    
      { NSSoundFinishedDelegate }
      SoundFinishedDelegate = objcclass(NSObject, NSSoundDelegateProtocol)
      public
        procedure sound_didFinishPlaying(Sound: NSSound; FinishedPlaying: Boolean);
           message 'sound:didFinishPlaying:';
      end;
    

Add two new variables: _SoundDelegate_ for the SoundFinishedDelegate and a _Playing_ boolean variable to indicate whether a sound is playing. 
    
    
    Var
      Form1: TForm1;
      mySound: NSSound = Nil;                              // Create an object based on the NSSound class
      SoundDelegate : SoundFinishedDelegate;
      SoundPlaying : Boolean = False;
    

Now create the new delegate procedure: 
    
    
    { NSSoundFinishedDelegate }
    procedure SoundFinishedDelegate.sound_didFinishPlaying(Sound: NSSound; FinishedPlaying: Boolean);
    begin
      if(FinishedPlaying) then
         begin
           SoundPlaying := False;
    
           Form1.PlayButton.Enabled   := True;
           Form1.PauseButton.Enabled  := False;
           Form1.ResumeButton.Enabled := False;
           Form1.StopButton.Enabled   := False;
           
           Sound.Release;  // avoid memory
           Sound := Nil;   // leak when using
           Sound.dealloc;  // NSSound.alloc...
         end;
    end;
    

The updated play procedure: 
    
    
     
    procedure TForm1.PlayButtonClick(Sender: TObject);
    var
      status: Boolean;
    begin
      Sound := NSSound.alloc.initWithContentsOfFile_byReference(NSStringUTF8
        ('/Applications/iPhoto.app/Contents/Resources/Music/Barbeque Blues.m4a'), True); // File system sound file
      //Sound := NSSound.SoundNamed(NSStringUTF8('Minuet in G'));                        // Bundle sound file
      //Sound := NSSound.soundNamed(NSStringUTF8('Frog'));                               // System sound file
    
      Sound.setDelegate(SoundDelegate);
    
      status := Sound.play;
      if(status) then
        SoundPlaying := True
      else
        ShowMessage('Playing failed!');
    
      PlayButton.Enabled  := False;
      PauseButton.Enabled := True;
      StopButton.Enabled  := True;
    end;
    

Finally, the new Initialisation clause for the delegate object: 
    
    
      
    Initialization
      SoundDelegate := SoundFinishedDelegate.alloc.init;
    End.
    

Here is the whole unit for the application which now takes care of the enabled-state of the Play/Pause/Resume/Stop buttons: 
    
    
      
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls, CocoaAll,
      CocoaUtils;
    
    type
    
      { NSSoundFinishedDelegate }
      SoundFinishedDelegate = objcclass(NSObject, NSSoundDelegateProtocol)
      public
        procedure sound_didFinishPlaying(Sound: NSSound; FinishedPlaying: Boolean);
           message 'sound:didFinishPlaying:';
      end;
    
      { TForm1 }
      TForm1 = class(TForm)
        StopButton: TButton;
        PlayButton: TButton;
        PauseButton: TButton;
        ResumeButton: TButton;
        procedure StopButtonClick(Sender: TObject);
        procedure FormShow(Sender: TObject);
        procedure PlayButtonClick(Sender: TObject);
        procedure PauseButtonClick(Sender: TObject);
        procedure ResumeButtonClick(Sender: TObject);
      private
      public
    end;
    
    var
      Form1: TForm1;
      Sound: NSSound = Nil;
      SoundDelegate : SoundFinishedDelegate;
      SoundPlaying : Boolean = False;
    
    implementation
    
    {$R *.lfm}
    
    { NSSoundFinishedDelegate }
    
    procedure SoundFinishedDelegate.sound_didFinishPlaying(Sound: NSSound; FinishedPlaying: Boolean);
    begin
      if(FinishedPlaying) then
         begin
           SoundPlaying := False;
    
           Form1.PlayButton.Enabled   := True;
           Form1.PauseButton.Enabled  := False;
           Form1.ResumeButton.Enabled := False;
           Form1.StopButton.Enabled   := False;
    
           Sound.Release;  // avoid memory
           Sound := Nil;   // leak when using
           Sound.dealloc;  // NSSound.alloc...
         end;
    end;
    
    { TForm1 }
    
    // Setup button state
    procedure TForm1.FormShow(Sender: TObject);
    begin
      PauseButton.Enabled  := False;
      ResumeButton.Enabled := False;
      StopButton.Enabled   := False;
    end;
    
    // Play
    procedure TForm1.PlayButtonClick(Sender: TObject);
    var
      status: Boolean;
    begin
      Sound := NSSound.alloc.initWithContentsOfFile_byReference(NSStringUTF8
        ('/Applications/iPhoto.app/Contents/Resources/Music/Barbeque Blues.m4a'), True); // File system sound example
      //Sound := NSSound.SoundNamed(NSStringUTF8('Minuet in G'));                        // Bundle sound file
      //Sound := NSSound.soundNamed(NSStringUTF8('Frog'));                               // System file
    
      Sound.setDelegate(SoundDelegate);
    
      status := Sound.play;
      if(status) then
        SoundPlaying := True
      else
        ShowMessage('Playing failed!');
    
      PlayButton.Enabled  := False;
      PauseButton.Enabled := True;
      StopButton.Enabled  := True;
    end;
    
    // Pause
    procedure TForm1.PauseButtonClick(Sender: TObject);
    begin
      if(SoundPlaying) then
        begin
          Sound.pause;
          PauseButton.Enabled  := False;
          StopButton.Enabled   := False;
          ResumeButton.Enabled := True;
        end;
    end;
    
    // Resume
    procedure TForm1.ResumeButtonClick(Sender: TObject);
    begin
      if(SoundPlaying) then
        begin
          Sound.resume;
          ResumeButton.Enabled := False;
          PauseButton.Enabled  := True;
          StopButton.Enabled   := True;
        end;
    end;
    
    // Stop
    procedure TForm1.StopButtonClick(Sender: TObject);
    begin
      if(SoundPlaying) then
         begin
          SoundPlaying := False;
          Sound.stop;
    
          Sound.Release;  // avoid memory
          Sound := Nil;   // leak when using
          Sound.dealloc;  // NSSound.alloc...
    
          PlayButton.Enabled   := True;
          PauseButton.Enabled  := False;
          ResumeButton.Enabled := False;
          StopButton.Enabled   := False;
         end;
    end;
    
    Initialization
      SoundDelegate := SoundFinishedDelegate.alloc.init;
    end.
    

## See also

  * [System Sound Services](<macOS_Play_Alert_Sound.md> "macOS Play Alert Sound")
  * [macOS Audio Player](<macOS_Audio_Player.md> "macOS Audio Player")
  * [macOS Audio Recorder](<macOS_Audio_Recorder.md> "macOS Audio Recorder")
  * [macOS Play Alert Sound](<macOS_Play_Alert_Sound.md> "macOS Play Alert Sound")
  * [NSBeep](<macOS_Programming_Tips.md> "macOS Programming Tips")
  * [macOS Sound Utilities](<macOS_Sound_Utilities.md> "macOS Sound Utilities")
  * [Multimedia Programming](<Multimedia_Programming.md> "Multimedia Programming")



## External links

  * [Apple: NSSound Class](<https://developer.apple.com/documentation/appkit/nssound>)
  * [Apple: Introduction to Sound Programming Topics for Cocoa](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Sound/Sound.html#//apple_ref/doc/uid/10000104-SW1>)
  * [Apple: System Sound Services](<https://developer.apple.com/documentation/audiotoolbox/system_sound_services>)

---

_Source: [https://wiki.freepascal.org/macOS_NSSound](https://web.archive.org/web/20240909150116/https://wiki.freepascal.org/macOS_NSSound)_
