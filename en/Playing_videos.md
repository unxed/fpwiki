# Multimedia Programming

From Free Pascal wiki

(Redirected from [Playing videos](</index.php?title=Playing_videos&redirect=no> "Playing videos"))

****English (en)**** | [**한국어 (ko)**](</Multimedia_Programming/ko> "Multimedia Programming/ko")

## Contents

  * 1 Playing Videos
    * 1.1 Starting a player application
    * 1.2 VFW - Video for Windows
    * 1.3 MPlayer for Linux gtk2/X
  * 2 Playing Sounds
    * 2.1 Windows only: using the Windows API
    * 2.2 Using the Audio Component Suite
    * 2.3 Using the OpenAL Library
    * 2.4 BASS
    * 2.5 Audiere Library
    * 2.6 Audorra Library
    * 2.7 OMEGA Engine
    * 2.8 PortAudio
    * 2.9 SDL + SDL_mixer
    * 2.10 FMOD
    * 2.11 Squall sound
    * 2.12 FPSound
    * 2.13 UOS (United OpenLib of Sound)
  * 3 See also

  
---  
  
#  Playing Videos

##  Starting a player application

Once you know the command line and parameters you can use TProcessUTF8 to start the player: 
    
    
    uses 
      Classes, ..., LCLProc, UTF8Process;
     
    ...
     
    implementation
     
    procedure TMainForm.Button1Click(Sender: TObject);
    var
      Player: TProcessUTF8;
    begin
      Player:=TProcessUTF8.Create(nil);
      try
        Player.CommandLine:=PathToPlayer+' '+ParametersAndMediaFile;
        Player.Execute;
      finally
        Player.Free;
      end;
    end;

For example to start under Linux the mplayer to play a video use: 
    
    
    uses 
      Classes, ..., FileUtil, LCLProc, UTF8Process;
     
    ...
     
    implementation
     
    procedure TMainForm.Button1Click(Sender: TObject);
    var
      Player: TProcessUTF8;
      Filename: String;
      PlayerPath: String;
      PlayerParams: String;
    begin
      Filename:='/home/username/video.mpg';
      PlayerPath:=FindDefaultExecutablePath('mplayer');
      PlayerParams:='"'+Filename+'"';
      Player:=TProcessUTF8.Create(nil);
      try
        Player.CommandLine:=PlayerPath+' '+PlayerParams;
        Player.Execute;
      finally
        Player.Free;
      end;
    end;

##  VFW - Video for Windows

Capturing and playing video streams from TV cards and webcams under Windows the [VFW](<Glossary.md> "Glossary") [API](<Glossary.md> "Glossary") can be used with [SysRec](<SysRec.md> "SysRec"). 

##  MPlayer for Linux gtk2/X

**mplayer** is an open source and free movie player. There is a LCL control embedding the mplayer, so you can built your own movie players or just play a video in your application. You can download it here: 
    
    
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/mplayer mplayer
    

#  Playing Sounds

##  Windows only: using the Windows API

You can use the Windows API to play e.g. wav files: 
    
    
    ...
    uses MMSystem;
    ...
    sndPlaySound('C:\sounds\test.wav', snd_Async or snd_NoDefault);

Obviously, this is not cross-platform 

##  Using the Audio Component Suite

Read the page about the [Audio Component Suite](<ACS.md> "ACS")

##  Using the OpenAL Library

A tutorial for Delphi can be found here: [[1]](<http://www.noeska.com/doal/tutorials.aspx>)

Free Pascal comes with a unit for accessing OpenAL located in fpc/packages/openal as well as [usage examples](<http://svn.freepascal.org/svn/fpc/trunk/packages/openal/examples>)

##  BASS

The BASS library can be downloaded from: <http://www.un4seen.com/> and <http://www.un4seen.com/forum/?topic=8682.0>

There is a DJ application, for Windows, Linux and Mac OSX, written with Lazarus and using Bass lib: <https://sites.google.com/site/fiensprototyping/>

##  Audiere Library

Has bindings for Delphi, but they don't work with FPC: 

  * <http://www.casteng.com/whypascal.shtml>
  * <http://www.afterwarp.net/resources/soundlib>
  * <http://code.google.com/p/audiere-bind-delphi/>



##  Audorra Library

Works fine with Lazarus: 

<http://sourceforge.net/projects/audorra/>

##  OMEGA Engine

The OMEGA Engine is a full game engine in single Omega.dll file which is just 50kb. It can successfully play FLAC,OGG,MP3,MP2,AMR and WMA files. It searches and uses installed audio codecs from the system. 

<http://sourceforge.net/projects/omega-engine/files/>

The project is dead, but is very easy to use: 
    
    
    Media_Play('Music.mp3', TRUE);

##  PortAudio

Various bindings for [PortAudio](<http://www.portaudio.com/>), a cross-platform, open source library for audio playback and recording are available. See [[[2]](<http://wiki.lazarus.freepascal.org/Multimedia_Programming#Playing_Sounds>)] for some versions. Forum user FredvS has written a Pascal unit that dynamically loads PortAudio: [[3]](<http://lazarus.freepascal.org/index.php/topic,17521.0.html>)

##  SDL + SDL_mixer

The basic SDL library comes with a very simple sound system. On top of that, SDL mixer adds more sound APIs which build a more flexible solution. 

##  FMOD

Is a closed source solution. It is free for use for non-comercial software, but requires the payment of licenses for commercial software. 

##  Squall sound

Squall sound works fine with FPC. It has 3D audio and EAX effects, but just supports MP3, OGG and WAV formats. 

<http://www.afterwarp.net/resources/soundlib>

##  FPSound

FPSound is a new library being developed on the molds of fpspreadsheet and fpvectorial: it has independent modules to read, write and play sound files and also an intermediary representation for editing. See [FPSound](<FPSound.md> "FPSound")

##  UOS (United OpenLib of Sound)

UOS : United OpenLib of Sound. UOS unifies the best open-source audio libraries. 

With UOS you can: 

. Listen to mp3, ogg, wav, flac,... audio files. 

. With 16, 32 or float 32 bit resolution. 

. Record all types of input into file. 

. Add DSP effects and filters, however many you want and record it. 

. Listen to multiple inputs and outputs. 

UOS uses the PortAudio, SndFile and Mpg123 audio libraries. 

Included in the package: 

  * examples. 
  * binary libraries for Linux 32/64, Windows 32/64, Mac OSX 32. 



<http://sites.google.com/site/fiensprototyping/U_OS_Test.tar.gz>

<http://github.com/fredvs/uos/>

#  See also

  * [Audio libraries](<Audio_libraries.md> "Audio libraries")

---

_Source: [https://wiki.freepascal.org/Playing_videos](https://web.archive.org/web/20161013001411/https://wiki.freepascal.org/Playing_videos)_
