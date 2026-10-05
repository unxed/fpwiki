# uos

[![uoslogo.png](https://wiki.freepascal.org/images/7/76/uoslogo.png)](</File:uoslogo.png>)

**uos** (United OpenLib of Sound) is a multi-platform package of audio handling routines that unifies the best open-source audio libraries. 

## Contents

  * 1 Features
  * 2 Download
  * 3 Forum
  * 4 uoslib
  * 5 Install
  * 6 Syntax



# Features

  * Play .mp3, .ogg, .wav, .flac, .m4a, .opus, .mod, .it, .s3m, .xm and cdrom audio files.
  * Deal with 16, 32 integer or float 32 bit resolution.
  * Record all types of input into wav or ogg file with 16 or 32 bit resolution.
  * Add DSP effects and filters, however many you want and record it.
  * Play multiple inputs and outputs simultaneously.
  * Internet audio streaming of mp3, aac and opus files.
  * Produce sound by the build-in synthesizer.



Uus can use the [SoundTouch](</index.php?title=SoundTouch&action=edit&redlink=1> "SoundTouch \(page does not exist\)"), [PortAudio](</index.php?title=PortAudio&action=edit&redlink=1> "PortAudio \(page does not exist\)"), [SndFile](</index.php?title=SndFile&action=edit&redlink=1> "SndFile \(page does not exist\)"), [Mpg123](</index.php?title=Mpg123&action=edit&redlink=1> "Mpg123 \(page does not exist\)"), [Faad](</index.php?title=Faad&action=edit&redlink=1> "Faad \(page does not exist\)"), [OpusFile](</index.php?title=OpusFile&action=edit&redlink=1> "OpusFile \(page does not exist\)"), [Libxmp](</index.php?title=Libxmp&action=edit&redlink=1> "Libxmp \(page does not exist\)") and [Mp4ff](</index.php?title=Mp4ff&action=edit&redlink=1> "Mp4ff \(page does not exist\)") audio libraries. 

Included in the package: Examples and binary libraries for Linux 32/64, arm aarch64 Raspberry Pi, Windows 32/64, macOS 32/64 and FreeBSD 32/64. 

# Download

  * <http://fredvs.github.io/uos/>
  * <https://github.com/fredvs/uos/tree/main>



# Forum

  * <http://uos-forum.108.s1.nabble.com>



# uoslib

Pascal users should use uos instead. 

**uoslib** is the Universal Library version of **uos** unit and is accessible from Pascal and all other languages (C, Java, ...). 

Some methods of uos are not accessible. 

Included in the package: Examples and binary libraries for Linux 32/64, Windows 32/64, macOS 32/64 and FreeBSD 32/64. 

  * <http://fredvs.github.io/uoslib/>
  * <http://github.com/fredvs/uoslib/>



# Install

The best way to show how uos works is to look at the examples in /uos/examples package. All is included, you only have to compile the demos and run it... 

Here some examples how to use uos : 

  * Copy all the files in /uos/src/ into the directory of your project.
  * add _uos_flat_ or _uos_ in your uses section. (for uoslib add _uoslib_h_) 
    * _uos_flat_ => All is done by the unit. The procedures are more "universal", same syntax as uoslib.
    * _uos_ => The application must declare uos class and variables. The procedures can be used as oop way.
  * All the examples in uos packages use _uos_flat_. There is a example using only _uos_ => SimplePlayer_noflat



Those libraries can be used by uos: 

  * PortAudio for dealing with audio devices of sound card,
  * SndFile for decoding wav, flac, org audio files,
  * Mpg123 for decoding mp3 audio files,
  * Faad and Mp4ff for decoding m4a audio files,
  * OpusFile for decoding opus audio files,
  * Libxmp for decoding mod, it, s3m, xm audio files,
  * fdkaac for decoding aac web-stream audio files.
  * SoundTouch plugin for changing tempo of any type of audio files,
  * bs2b plugin to apply stereo to binaural effect.



You may load those libraries with: 
    
    
    uos_LoadLib( pchar(PortAudioFileName), pchar(SndFileFileName), pchar(Mpg123FileName),  pchar(Mp4ffFileName), pchar(FaadFileName), pchar(OpusFileFileName),  pchar(LibxmpFileFileName),pchar(fdkaacFilename) );
    

If some libraries are not needed, just replace the filename with "nil". 

If you replace the filename with pchar('system'), the library of the system will be used (if exists). 

WARNING: Do not forget to call **uos_free()** when application terminates. 

Play a file 'MySong.mp3': 
    
    
    begin
      uos_LoadLib( PortAudioFileName, nil, Mpg123FileName,  nil, nil, nil, nil, nil); // only PortAudio and Mpg123 are needed. 
      uos_CreatePlayer( 0 );              // you may create how many players you want, from 0 to to what you computer can do...
      uos_AddIntoDevOut( 0 );             // Add Output with default parameters     
      uos_AddFromFile( 0, 'MySong.mp3' ); // Input from audio file 
        // And let's play it...
      uos_Play(0);                        // Play the song...
    end;
    ...
    uos_free;                             // Call this at end of application.
    

Play a sound file with some dsp effects (only for uos, in future for uoslib too): 
    
    
    begin
      uos_LoadLib( pchar(PortAudioFileName), nil, pchar(Mpg123FileName), nil, nil, nil, nil, nil); // only PortAudio and Mpg123 are needed.
      uos_CreatePlayer( 0 );              // First create the player:
      uos_AddIntoDevOut( 0 );             // Add Output with default parameters     
      uos_AddFromFile( 0, 'MySong.mp3' ); // Input from audio file 
      uos_InputAddDSP( 0, 1 );               // Add as many dsp effects you want
      uos_InputAddDSP( 0, 2 );
      uos_Play( 0 );                      // Play the song with DSP effect...
    end;
    ...
    uos_free;                             // Call this at end of application.
    

Listen to your mic into your loudspeakers (for uoslib too) : 
    
    
    begin
      uos_LoadLib( pchar(PortAudioFileName), nil, nil, nil, nil, nil, nil, nil); // only PortAudio is needed.
      uos_CreatePlayer( 0 );     
      uos_AddIntoDevOut( 0 );   
      uos_AddFromDevIn( 0 );   // Input from IN Device
      uos_Play( 0 );           // Listen to your mic in your loudspeakers...
    end;
    ...
    uos_free;                             // Call this at end of application.
    

Now, listen to your mic, listen to a mp3, apply some effects and record the mic into wav file at same time: 
    
    
    begin
      uos_LoadLib( pchar(PortAudioFileName), pchar(SndFileFileName), pchar(Mpg123FileName), nil, nil, nil, nil, nil);
      uos_CreatePlayer( 0 );
      uos_AddIntoDevOut( 0 ) ;              // Output into OUT Device
      uos_AddIntoFile( 0, 'Myrecord.wav' ); // Output into wav file
      uos_AddFromDevIn( 0 );                // Input from IN Device
      uos_InputAddDSP( 0, 1 );              // Add as many dsp's you want
      uos_InputAddDSP( 0, 2 );
      uos_CreatePlayer( 1 );
      uos_AddFromFile( 1, 'MySong.mp3' );   // the background audio file
      uos_AddIntoDevOut( 1 ) ;              // Output into OUT1 Device;
    
      uos_Play(0);                          // Listen your mic with DSP effects and record it ...
      uos_Play(1);                          // Listen to mp3 while recording
    end;
    ...
    uos_free;                               // Call this at end of application.
    

# Syntax

Here are the basic uos methods. Each method has its equivalent with arguments for custom parameters. 

Initialization methods: 
    
    
    uos_LoadLib();              // Load dynamic libraries for uos  
    uos_LoadLibs();             // Load dynamic libraries for uoslibs
    uos_UnLoadLib();             // To unload libraries.
    uos_Free;                    // To use when application terminate.
    

Output 
    
    
    uos_AddIntoDevOut();         // Output into OUT Device
    uos_AddIntoFile();           // Output into Wav File
    uos_AddIntoMemoryBuffer()    // Add a Output into memory buffer
    uos_AddIntoMemoryStream()    // Add a Output into memory stream
    uos_AddIntoIceServer()       // Add a Output into a IceCast server
    uos_AddIntoFileFromMem()     // Add a Output into audio wav file from TMemoryStream
    

Input 
    
    
    uos_AddFromDevIn();          // Input from IN device
    uos_AddFromFile();           // Input from Audio File
    uos_AddFromMemoryBuffer();   // Input from decoded pcm buffer
    uos_AddFromMemoryStream();   // Input from a TMemoryStream of Audio File
    uos_AddFromFileIntoMemory(); // Input from audio file and stored into memory buffer
    uos_AddFromURL();            // Input from internet URL mp3/opus/aac files
    uos_AddFromSynth(;           // Input form built-in Synthesizer
    uos_InputLength();           // Length of Input in samples
    uos_InputPosition();         // Get position
    uos_InputSeek();             // Change position in byte
    uos_InputSeekTime();         // Change position in time
    uos_InputSeekSeconds();      // Change position in second
    

Player 
    
    
    uos_Play();                  // Start playing
    uos_PlayPaused();            // Start playing paused.
    uos_PlayNoFree();            // Start playing but do not free the player after stop.
    uos_PlayPausedNoFree();      // Start playing paused but do not free the player after stop.
    uos_RePlay();                // Resume playing after pause
    uos_Stop();                  // Stop playing and free thread
    uos_Pause();                 // Pause playing
    uos_FreePlayer();            // Free the player. Works only when PlayNoFree() was used.
    

DSP 
    
    
    uos_InputAddDSP()            // DSP procedure for input
    uos_OutputAddDSP()           // DSP procedure for output
    uos_InputSetDSP()            // Set DSP In
    uos_OutputSetDSP()           // Set DSP out
    uos_InputSetDSPVolume(...)   // Set Volume
    uos_InputAddFilter(...)      // Add filer
    uos_AddPlugin(...)           // Add pluggin
    

And much more other procedures/functions (see in uos.pas...)

---

_Source: [https://wiki.freepascal.org/uos](https://web.archive.org/web/20250312204309/https://wiki.freepascal.org/uos)_
