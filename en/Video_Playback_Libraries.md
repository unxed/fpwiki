# Video Playback Libraries

## Introduction

Several libraries are available for embedding video playback within Lazarus applications. These range from simple to complex. 

## Libraries

Library  | Source  | Platforms  | Notes   
---|---|---|---  
[DSPack](<http://code.google.com/p/dspack/>) | <http://code.google.com/p/dspack/> <http://github.com/TheBlackSheep/DSPack-Lazarus> | Windows only  | DirectShow library. Although the DSPack can be simple to use, this offers complex low level functionality as well. Video Playback is dependent upon correct Codecs being installed on the end-user system. A tutorial on DSPack use for Delphi, which also applied to fpc, can be found at <http://www.vwlowen.co.uk/directshow/page01.htm>. Full documentation on the DirectShow API is available at <http://msdn.microsoft.com/en-us/library/windows/desktop/dd375454(v=vs.85).aspx> A more up to date version of DSPack, that compiles against FPC 2.7.1, is available with the CodeTyphon installation.   
libvlc  | Included with FPC 2.7.1 as an optional package  | Any platform supported by VLC  | Currently only available in FPC Trunk Video Playback is dependent upon VLC being installed on the end-user system.   
[PasLibVLC](<http://prog.olsztyn.pl/paslibvlc/>) | <http://prog.olsztyn.pl/paslibvlc/> | Any platform supported by VLC  | A mature package that also installs under various Delphi's. Video Playback is dependent upon VLC being installed on the end-user system.   
[FFmpeg](<http://www.ffmpeg.org>) | <https://www.ffmpeg.org> | Any platform supported by FFmpeg  | Old FPC headers are available with the FFmpeg source code. Assorted open source applications have improved these headers over the years, but there appears to be no maintainer for these updated headers. New FPC headers are <https://github.com/DJMaster/ffmpeg-fpc>. Read <http://www.ffmpeg.org/legal.html> before distributing FFmpeg.   
[FFVCL](<http://www.delphiffmpeg.com>) | <http://www.delphiffmpeg.com/downloads/> | Any platform supported by FFmpeg  | FFVCL provides commerical native FFmpeg components, including an encoder and player, for Delphi. The headers are free to download and reportedly work with FPC.   
[SDL](<FPC_and_SDL.md> "FPC and SDL") | <https://www.libsdl.org/> | Officially supports Windows, macOS, Linux, iOS, and Android. Support for other platforms may be found in the source code.  | SDL 2.0 is distributed under the [zlib license](<https://www.libsdl.org/license.php>). This license allows you to use SDL freely in any software.   
[TMPlayerControl](<TMPlayerControl.md> "TMPlayerControl") | Lazarus-CCR  | X/GTK2 & Windows  | Simple to use. mplayer can be easily bundled with your application, however mplayer uses FFmpeg and the legal licensing warning at <http://www.ffmpeg.org/legal.html> could also apply. It is recommended that the end-user install mplayer on their system instead of it being distributed with your application.   
[Video for Windows](<http://msdn.microsoft.com/en-us/library/windows/desktop/dd757708\(v=vs.85\).aspx>) |  | Windows 16bit or 32bit  | The [SysRec](<SysRec.md> "SysRec") application demostrates capturing and playing video streams from TV cards and webcams under Windows the [VFW](<Glossary.md> "Glossary") [API](<Glossary.md> "Glossary"). Video for Windows (VFW) was created for Win 3.1, and though deprecated in favour of DirectShow, is still available under modern 32bit Windows.   
  
## See also

  * Forum Topic <http://forum.lazarus.freepascal.org/index.php?topic=16797.0> for a discussion on the DSPack ported by TheBlackSheep
  * Forum Topic <http://forum.lazarus.freepascal.org/index.php/topic,22038.msg129568.html#msg129568> for a discussion on the current state of the FFmpeg headers.
  * [UltraStar Deluxe](<http://sourceforge.net/projects/ultrastardx/>) for an open source application with updated FFmpeg headers
  * [LazActiveX](<LazActiveX.md> "LazActiveX") for an example of how to embed VLC using ActiveX
  * [TMPlayerControl](<TMPlayerControl.md> "TMPlayerControl") for details on how to use TMplayerControl library to embed mplayer
  * [Multimedia Programming](<Multimedia_Programming.md> "Multimedia Programming")
  * [SysRec](<SysRec.md> "SysRec") for an example of using Video For Windows (deprecated precursor to DirectShow - created for Win 3.1, but is still available under current 32bit Window OS's)
  * [Free Pascal Meets SDL](<https://www.freepascal-meets-sdl.net/>) website
  * [Metal Framework](<Metal_Framework.md> "Metal Framework") \- macOS
  * [macOS Video Player](<macOS_Video_Player.md> "macOS Video Player") \- Using the macOS AVKit Framework

---

_Source: [https://wiki.freepascal.org/Video_Playback_Libraries](https://web.archive.org/web/20221005051758/https://wiki.freepascal.org/Video_Playback_Libraries)_
