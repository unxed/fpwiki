# OvoPlayer

## Contents

  * 1 Info
    * 1.1 Features
    * 1.2 Supported Engines
  * 2 Download
  * 3 To Do
  * 4 Development
    * 4.1 Git
  * 5 Screen Shot
  * 6 See Also



## Info

OvoPlayer is a multiplatform music player for various audio formats. 

### Features

  * Can play MP3, FLAC, WMA, APE, OGG, MP4, AAC, M4A files (if supported by selected audio engine)
  * Can read and update tags of MP3, FLAC, WMA, APE and OGG files. Can read tags of MP4 and AAC.
  * Cover viewer
  * Cross platform - works on Linux and Windows
  * Multiple audio engines supported
  * 10 bands equalizer on supported engines
  * Can import M3U, ASX, PLS, XSPF, BSPL, WPL playlists



### Supported Engines

Ovoplayer can use various multimedia playback engines: 

  * **[VLC](<http://www.videolan.org/>)** \- preferred choice, tested on Linux and Windows
  * **[MPlayer](<http://www.mplayerhq.hu>)** \- plays almost any format, tested on Linux and Windows
  * **[XINE](<https://www.xine-project.org/>)** \- almost complete (some performance issues), tested on Linux
  * **[BASS](<http://www.un4seen.com>)** \- complete, tested on Windows
  * **[GStreamer](<http://http://gstreamer.freedesktop.org>)** \- complete, tested on Linux
  * **Direct Show** \- complete, Windows only, may need to install codecs for Flac and Ogg
  * **Media Foundation** \- complete, Windows only, may need to install codecs for Flac and Ogg
  * **[UOS (United Openlib of sound)](<http://wiki.lazarus.freepascal.org/uos>)** \- complete (no WMA, APE, MP4), tested on Linux and Windows
  * **[LibMPV](<http://mpv.io>)** \- complete, tested on linux



Experimental or incomplete support 

  * **FFMPEG** \- abandoned, API is very complex and somewhat unstable
  * **libZPlay** \- Windows only, some issues on detecting song end



## Download

Binary releases for Windows and Linux can be found here: 

<http://ovoplayer.altervista.org/downloads.html>

Source code: see below. 

## To Do

  * macOS port
  * More audio engine support (libzplay, FMod, ...)



## Development

OvoPlayer is hosted in SourceForge: <https://sourceforge.net/projects/ovoplayer/>

Source code is mirrored and kept synchronized on: 

  * GitHub - <https://github.com/varianus/ovoplayer>
  * GitLab - <https://gitlab.com/Varianus/ovoplayer>



### Git

Use this command to check out the latest project source code: 
    
    
    git clone https://github.com/varianus/ovoplayer.git
    

A compressed source code archive can be downloaded here: <http://ovoplayer.altervista.org/downloads.html>

## Screen Shot

[![OvoPlayerScreenshot.png](https://wiki.freepascal.org/images/3/39/OvoPlayerScreenshot.png)](</File:OvoPlayerScreenshot.png>)

## See Also

  * [Free Pascal Application Suite](<Free_Pascal_Application_Suite.md> "Free Pascal Application Suite")
  * [FPSound](<FPSound.md> "FPSound")

---

_Source: [https://wiki.freepascal.org/OvoPlayer](https://web.archive.org/web/20240301082904/https://wiki.freepascal.org/OvoPlayer)_
