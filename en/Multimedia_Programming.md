# Multimedia Programming

│ **English (en)** │  **[русский (ru)](<../ru/Multimedia_Programming.md>)** │

## Contents

  * 1 Playing Videos
    * 1.1 Native solutions
      * 1.1.1 macOS
  * 2 Playing Sound
    * 2.1 Native solutions
      * 2.1.1 macOS
      * 2.1.2 Windows
        * 2.1.2.1 Windows API
        * 2.1.2.2 Audiere Library
        * 2.1.2.3 Squall sound
    * 2.2 Cross Platform
      * 2.2.1 ACS (Audio Component Suite)
      * 2.2.2 Audorra Library
      * 2.2.3 BASS
      * 2.2.4 FMOD
      * 2.2.5 FPSound
      * 2.2.6 OMEGA Engine
      * 2.2.7 OpenAL Library
      * 2.2.8 Play Sound Package
      * 2.2.9 PortAudio
      * 2.2.10 SDL + SDL_mixer
      * 2.2.11 SFML/CSFML for FPC
      * 2.2.12 uos (United OpenLib of Sound)
  * 3 Recording Sound
    * 3.1 Native solutions
      * 3.1.1 macOS
    * 3.2 Cross platform solutions
      * 3.2.1 uos (United OpenLib of Sound)
  * 4 MIDI
  * 5 Text-To-Speech
  * 6 See also



## Playing Videos

The [Video Playback Libraries](<Video_Playback_Libraries.md> "Video Playback Libraries") page contains an annotated list of libraries for Windows, as well as any operating system which is supported by FFmpeg or by VLC. These libraries can be used to embed video playback in your applications. 

### Native solutions

#### macOS

  * [AVPlayer](<macOS_Video_Player.md> "macOS Video Player") \- Native code for a customised video (and audio, particularly streaming audio) player.



## Playing Sound

### Native solutions

#### macOS

  * [System Sound Services](<macOS_Play_Alert_Sound.md> "macOS Play Alert Sound") For up to 30 second sound clips, alerts etc.
  * [AVAudioPlayer](<macOS_Audio_Player.md> "macOS Audio Player") For longer audio files with volume control, pause, stop, resume, loop control, background music etc. Note: does not work with streaming audio.
  * [AVPlayer](<macOS_Video_Player.md> "macOS Video Player") For streaming audio.
  * [macOS NSSound](<macOS_NSSound.md> "macOS NSSound") A very simple method for playing system sounds, application bundle sound files, other sound files with volume control, pause, stop, resume, loops etc.
  * OpenAL comes pre-installed on macOS (Deprecated at WWDC2019; to be removed in a future macOS release).



#### Windows

##### Windows API

You can use the Windows API to play e.g. wav files: 
    
    
    ...
    uses MMSystem;
    ...
    sndPlaySound('C:\sounds\test.wav', snd_Async or snd_NoDefault);
    

The fail-safe way, which will allow appending paths and nonlatin file names is: 
    
    
    sndPlaySound(pchar(UTF8ToSys('C:\sounds\test.wav')), snd_Async or snd_NoDefault);
    

##### Audiere Library

Has bindings for Delphi, but they do not work with FPC: 

  * <http://code.google.com/p/audiere-bind-delphi/>



##### Squall sound

Squall sound works fine with FPC. It has 3D audio and EAX effects, but just supports MP3, OGG and WAV formats. Open sourced in 2009 and appears dead. Windows only. 

<https://github.com/xtreme3d/squall>

### Cross Platform

#### ACS (Audio Component Suite)

For more information, read the article about the [Audio Component Suite](<ACS.md> "ACS")

#### Audorra Library

This library for Linux and Windows works well with Lazarus: 

<http://sourceforge.net/projects/audorra/>

#### BASS

Closed source. Free for non-commercial. 

The BASS library can be downloaded from: <http://www.un4seen.com/> and <http://www.un4seen.com/forum/?topic=8682.0>

There is a DJ application, for Windows, Linux and macOS, written with Lazarus and using Bass lib: <https://sites.google.com/site/fiensprototyping/>

#### FMOD

[FMOD Core](<https://www.fmod.com/core>) comes with an elegant API compatible with C, C++, C#, and Javascript, and runs on all major platforms. It is a closed source solution. It is free for use for non-comercial software, but requires the payment of license fees for commercial software. 

#### FPSound

FPSound is a new library being developed in the mold of fpspreadsheet and fpvectorial: it has independent modules to read, write and play sound files and also an intermediary representation for editing. See [FPSound](<FPSound.md> "FPSound"). Uses OpenAL. Appears dead and non-working. 

#### OMEGA Engine

The OMEGA Engine is a full game engine in single Omega.dll file which is just 50kb. It can successfully play FLAC, OGG, MP3, MP2, AMR and WMA files. It searches and uses installed audio codecs from the system. Windows and Linux only. 

<http://sourceforge.net/projects/omega-engine/files/>

The project is dead, but it is very easy to use: 
    
    
    Media_Play('Music.mp3', TRUE);
    

#### OpenAL Library

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The OpenAL Library Framework is pre-installed on all Apple computers running macOS -- see /System/Library/Frameworks/OpenAL.framework. Apple announced at WWD2019 that **OpenAL is now deprecated** and will be removed completely in a future release of macOS.

OpenAL is a cross-platform 3D audio API appropriate for use with gaming applications and many other types of audio applications. 

The library models a collection of audio sources moving in a 3D space that are heard by a single listener somewhere in that space. The basic OpenAL objects are a Listener, a Source, and a Buffer. There can be a large number of Buffers, which contain audio data. Each buffer can be attached to one or more Sources, which represent points in 3D space which are emitting audio. There is always one Listener object (per audio context), which represents the position where the sources are heard -- rendering is done from the perspective of the Listener. 

  * See the [OpenAL website](<https://www.openal.org>) for documentation, downloads etc.


  * A tutorial for Delphi can be found [here](<http://www.noeska.com/doal/tutorials.aspx>).


  * Free Pascal comes with a unit for accessing OpenAL located in fpc/packages/openal as well as [usage examples](<http://svn.freepascal.org/svn/fpc/trunk/packages/openal/examples>).


  * There is an alternative OpenAL sound manager unit to play wav files by [Lulu](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=59515>) [here](<https://github.com/Lulu04/OALSoundManager>). Tested on macOS Mojave 10.14.



#### Play Sound Package

The [Play Sound Package](<Play_Sound_Multiplatform.md> "Play Sound Multiplatform") takes a different approach to playing sound cross-platform. For Windows, it uses the native sound system; for Linux it searches the system for programs which will play sound and executes those asynchronously or synchronously via [TProcess](<TProcess.md> "TProcess") or [TAsyncProcess](<TAsyncProcess.md> "TAsyncProcess"). Note: only works with Windows and Linux. 

#### PortAudio

Various bindings for [PortAudio](<http://www.portaudio.com/>), a cross-platform, open source library for audio playback and recording, are available. 

Forum user FredvS has written a [Pascal unit](<http://lazarus.freepascal.org/index.php/topic,17521.0.html>) that dynamically loads PortAudio. 

Some examples can be found [here](<http://breakoutbox.de/pascal/pascal.html#PortAudio>). 

#### SDL + SDL_mixer

The basic SDL library comes with a very simple sound system. On top of that, SDL mixer adds more sound APIs which build a more flexible solution. 

Units for accessing SDL libraries are located in fpc/packages/sdl. 

#### SFML/CSFML for FPC

[Simple and Fast Multimedia Library](<https://www.sfml-dev.org/>) aka SFML provides a simple interface to the various components of your PC, to ease the development of games and multimedia applications. It is composed of five modules: system, window, graphics, audio and network. With SFML, your application can compile and run out of the box on the most common operating systems: Windows, Linux, macOS and soon Android & iOS. 

Pre-compiled SDKs for your favorite OS are available on the [download page](<https://www.sfml-dev.org/download.php>). 

CSFML headers binding for FPC can be found at <https://github.com/DJMaster/csfml-fpc>

#### uos (United OpenLib of Sound)

**uos** : United OpenLib of Sound. **uos** unifies the best open-source audio libraries. 

With **uos** you can: 

  * Listen to mp3, ogg, wav, flac, m4a, opus, cd audio, ... audio files.
  * With 16, 32 or float 32 bit resolution.
  * Record all types of input into a file.
  * Add DSP effects and filters, however many you want and record it.
  * Do web audio-streaming (listen to web-radio and apply filters).
  * Listen to multiple inputs and outputs.
  * Produce sound with built-in synthesizer.



**uos** can use the PortAudio, SndFile, Mpg123, Faad, Mp4ff, OpusFile audio libraries and SoundTouch, Bs2b plug-in library. 

Included in the package: 

  * examples.
  * binary libraries for Linux 32/64, Windows 32/64, macOS 32/64, FreeBSD 32/64 and ARM Rpi.



It can play mp3, ogg, wav, flac, m4a, opus, cda files. 

<http://github.com/fredvs/uos/>

## Recording Sound

### Native solutions

#### macOS

  * [AVAudioRecorder](<macOS_Audio_Recorder.md> "macOS Audio Recorder") allows you to make audio recordings straight to a file with very little programming overhead.



### Cross platform solutions

#### uos (United OpenLib of Sound)

**uos** : United OpenLib of Sound. **uos** unifies the best open-source audio libraries. 

With **uos** you can: 

  * Record all types of input into a file.
  * Add DSP effects and filters, however many you want and record it.



**uos** can use the PortAudio, SndFile, Mpg123, Faad, Mp4ff, OpusFile audio libraries and SoundTouch, Bs2b plug-in library. 

Included in the package: 

  * examples.
  * binary libraries for Linux 32/64, Windows 32/64, macOS 32/64, FreeBSD 32/64 and ARM Rpi.



<http://github.com/fredvs/uos/>

## MIDI

Musical Instrument Digital Interface (MIDI) is a technical standard that describes a communications protocol, digital interface, and electrical connectors that connect a wide variety of electronic musical instruments, computers, and related audio devices for playing, editing and recording music. 

See the [MIDI](<MIDI.md> "MIDI") article for more details about the MIDI software aspect. 

## Text-To-Speech

See [Speech Synthesis](<Speech_Synthesis.md> "Speech Synthesis") for native and cross-platform solutions. 

## See also

  * [Audio libraries](<Audio_libraries.md> "Audio libraries")
  * [Video Playback Libraries](<Video_Playback_Libraries.md> "Video Playback Libraries")

---

_Source: [https://wiki.freepascal.org/Multimedia_Programming](https://web.archive.org/web/20231007171600/https://wiki.freepascal.org/Multimedia_Programming)_
