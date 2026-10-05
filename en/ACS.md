# ACS

│ **[Deutsch (de)](</ACS/de> "ACS/de")** │  **English (en)** │  **[français (fr)](</ACS/fr> "ACS/fr")** │  **[日本語 (ja)](</ACS/ja> "ACS/ja")** │  **[português (pt)](</ACS/pt> "ACS/pt")** │    
****

## Contents

  * 1 About
    * 1.1 Screenshot
    * 1.2 Authors
    * 1.3 License
  * 2 ACS 2.4
  * 3 ACS 3.0
    * 3.1 Help
    * 3.2 Demos and examples



### About

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Because most of the required libraries are 32-bit, the whole ACS package is 32-bit only!

**ACS** (**Audio Components Suite**) is a freeware cross-platform set of components designed to perform different sound-processing tasks. It supports reading the audio data from CD, different file formats, for example wav,mp3,wma,ogg,mac and others, output to soundcard and files is also possible. Between input and output you can work with the data, different converters, mixers and processors are available. 

Its main characteristics are : 

  * Abstract Layer to include different "drivers" 
    * Alsa, /dev/dsp, AOLive, OSS support within Linux
    * DirectX, Wavemapper support within Windows
    * Audio playback and capture
    * Simultaneous operations on the same or different devices are allowed.


  * Abstract Layer to make it easy to add new file formats; already included file formats: 
    * Wave files/streams support, Raw PCM, MS ADPCM, DVI IMA ADPCM support
    * MP3 format support: Encode mp3 files using LAME, mp3 playback with smpeg library, streams conversion using MAD decoder
    * Ogg Vorbis format support: Reading Ogg files/streams (including multi-streamed ones). Storing data in Ogg Vorbis format with wide range of settings for compression/quality tweaks. Ogg comments support
    * FLAC format support: Reading FLAC files/streams, storing data in FLAC format with wide range of settings for compression tweaks.
    * Monkey Audio format support (for Windows only)
    * CD-ROM playback and direct CDDA data capture
    * Append data to existing file/stream capability


  * AudioMixer component for mixing/concatenating audio streams
  * InputList component for dynamically building playback/input lists
  * Set of audio converter components 
    * Sample converter for bits per sample conversion.
    * Sample rate converter (resampler) using sinc filtering
    * Mono/Stereo converter
    * Stereo balance control
    * Sound indicator
    * Windowed sinc and Butterworth filters for changing audio spectrum
    * Convolver component for applying custom sound effects
  * Mixer component to use mixer devices



#### Screenshot

[![Acs demos.jpg](https://wiki.freepascal.org/images/f/f8/Acs_demos.jpg)](</File:Acs_demos.jpg>)

#### Authors

Author: Andrei Borovsky 

Original work may be found [here](<http://www.mtu-net.ru/aborovsky/acs/index.html>) \- it's a part of Andrei Borovsky's personal page, some old versions of ACS (for Delphi/Kylix) can be downloaded, also there are some docs, which can't be found in the Lazarus port. 

    Copyright (c) 2002-2010, Andrei Borovsky, anb@symmetrica.net
    Copyright (c) 2005-2006 Christian Ulrich, mail@z0m3ie.de
    Copyright (c) 2014-2015 Sergey Bodrov, serbod@gmail.com

#### License

License: [MIT](<https://opensource.org/licenses/MIT>)

### ACS 2.4

You can browse the source and download a tar file from 
    
    
    <http://lazarus-ccr.svn.sourceforge.net/viewvc/lazarus-ccr/components/acs/>
    

checkout from svn: 
    
    
    <svn://svn.code.sf.net/p/lazarus-ccr/svn/components/acs>
    

Please add your Bugreports/Feature requests in [Lazarus CCR](<http://bugs.freepascal.org/set_project.php?project_id=9>) project of the Lazarus bug tracker. 

Installation: 

  * Open the package .lpk with Component/Open package file (.lpk)
  * Click on Compile (only necessary, if you don't want to install the component into the IDE)
  * Click on Install if you want to install the component into the IDE



  


### ACS 3.0

Clone or download from: 
    
    
    <https://github.com/serbod/acs>
    

or get from [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager") from "Multimedia" category 

Bug reporting / Feature Request: 
    
    
    <https://github.com/serbod/acs/issues>
    

#### Help

Open docs/help/index.htm file from package directory in your web browser. Some information is outdated, look for comments in sources. 

#### Demos and examples

Tested working demos: 

  * AcsConsolePlayer - Minimal console audio player
  * audiodeck - Any input -> any output test, replace for 'converter', 'linerecord', 'recording' demos
  * player2 - Small audio player

---

_Source: [https://wiki.freepascal.org/ACS](https://web.archive.org/web/20230917160219/https://wiki.freepascal.org/ACS)_
