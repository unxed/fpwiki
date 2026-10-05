# TMPlayerControl

## Contents

  * 1 Introduction
    * 1.1 Authors
    * 1.2 License
    * 1.3 Download
  * 2 Distributing mplayer
  * 3 Usage
    * 3.1 Example
    * 3.2 Methods
    * 3.3 Events
  * 4 See also



## Introduction

TMPlayerControl is a LCL control that embeds "mplayer" - a movie player available on several platforms. TMPlayerControl only works under X/gtk2 and Windows. TMPlayerControl can be used to play any mplayer supported video or audio file. 

### Authors

[Mattias Gaertner](</User:Mattias2> "User:Mattias2") (package maintainer)   
Michael Koecher (contributor)  
[Mike Thompson](</User:Mike.cornflake> "User:Mike.cornflake") (contributor) 

### License

[Modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL.txt>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL.txt>) (same as FPC RTL and Lazarus LCL). 

### Download

The latest stable release can be found at svn: 
    
    
    svn checkout svn://svn.code.sf.net/p/lazarus-ccr/svn/components/mplayer mplayer
    

mplayer, and additional codecs, can be downloaded from <http://www.mplayerhq.hu/>. The additional codecs must be installed separately, but are not necessary for many common formats. 

## Distributing mplayer

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** mplayer can be easily bundled with your application installation. However mplayer uses FFmpeg and the legal licensing warning at <http://www.ffmpeg.org/legal.html> could also apply (see last question under Mini-Patent FAQ).   
It is recommended that the end-user install mplayer on their system instead of it being distributed with your application.

## Usage

The control can be dropped on any form, and incorporates the video playback area.  
Additional controls such as Trackbars for progess/volume and buttons for playback control need to be added manually, with their values being set during the appropriate events. 

mplayer must be available on the end user system. Either installed with the mplayer folder being in the PATH environment variable, or with the MPlayerPath property pointing to the mplayer executable. 

TMPlayerControl only implements a very small set of mplayer features: **Play** , **Stop** , **Pause** , **Loop** , **Volume** , **Duration** , **Position** and **GrabImage** , essentially focusing on anything required for multimedia playback. 

TMPlayerControl sets a minor selection of mplayer properties at run time. Additional parameters can be set via **StartParam**.  
See <http://mplayerhq.hu/DOCS/man/en/mplayer.1.html#GENERAL%20OPTIONS> for a full list of available parameters. 

TMPlayerControl controls mplayer in slave mode. Slave commands can be sent directly to mplayer after multimedia is loaded (i.e. after calling Play) by using **SendMPlayerCommand(Cmd: string);**  
**SendMPlayerCommand** is not needed for simple video playback, and is made available for advanced mplayer features.  
For a full list of available slave comamnds visit <http://www.mplayerhq.hu/DOCS/tech/slave.txt> or enter the following at the command prompt. 
    
    
    mplayer -input cmdlist
    

### Example

In the examples folder, see FullFeatured\mplayerTestHarness.lpr for a full demonstration of TMPlayerControl capabilities, as well as additional functionality for querying mplayer for available commands. 

[![](https://wiki.freepascal.org/images/8/8b/FullFeaturedDemo.jpg)](</File:FullFeaturedDemo.jpg>)

[](</File:FullFeaturedDemo.jpg> "Enlarge")

FullFeatured example program

### Methods

The following Methods and Properties are available: 

Method  | Description   
---|---  
function FindMPlayerPath : Boolean;  | TMPlayerControl searches for mplayer in the PATH or in a subfolder of the application folder called "mplayer". If found, then sets MPlayerPath and returns True   
property MPlayerPath: string;  | R/W. Full path to the mplayer executable. Either populated by FindMPlayerPath, or programmatically   
property StartParam: string;  | R/W. Any additional parameters to be sent to mplayer before the multimedia file is initialised. See <http://mplayerhq.hu/DOCS/man/en/mplayer.1.html#GENERAL%20OPTIONS> for a full set of available parameters   
property Filename: string;  | R/W. The multimedia file to be played   
procedure Play;  | All the above must be configured correct before this command is called. This initialises mplayer, putting it into slave mode with the file loaded. mplayer will automatically start playing the file.   
procedure Stop;  | This stops the video file playing, which in turn stops mplayer. After this is called, many of the properties will no longer be available.   
property Loop: integer;  | R/W. This instructs mplayer to loop the video. -1 no, 0 forever, 1 once, 2 twice, ...   
property Paused: boolean;  | R/W. Either set or retrieve the paused state.   
function Running: boolean;  | R/O. Is mplayer running?   
function Playing: boolean;  | R/O. Is mplaying running with a file loaded? There is only a small window in time between the file stopping and mplayer itself stopping running.   
property ImagePath: string;  | R/W. The folder you want captured image grabs to be saved in.   
procedure GrabImage;  | Instruct mplayer to grab an image. In testing, success of this call is dependent upon a combination of the codec mplayer is using, and the renderer being used. Under windows, optimal results were returned with the direct3d renderer (-vo direct3d)   
property LastImageFilename: String;  | R/O. The filename of the last image grabbed. The filename of the last image grabbed is also passed as a parameter to the OnGrabImage event.   
property Volume: integer;  | R/W. 0 = mute. 100 = max   
property Rate: single;  | R/W. Playback rate. 1 = normal speed. mplayer allows values between 0.1 and 100 (i.e. no reverse)   
property Duration: single;  | R/O. The length of the loaded video file in seconds.   
property Position: single;  | R/W. Used to set or retrieve the current position of the video file in seconds. Current video position is queried every half second, so during playback at normal speed, there is only a half second accuracy.   
property VideoInfo: TVideoInfo;  | R/O. Only populated after OnPlay event. Record containing Codec, bitrate, width, height etc   
property AudioInfo: TAudioInfo;  | R/O. Only populated after OnPlay event. Record containing Codec, SampleRate, Bitrate etc.   
procedure SendMPlayerCommand(Cmd: string);  | Additional commands can be sent directly to mplayer, and the results can be queried with the OnFeedback event. See <http://www.mplayerhq.hu/DOCS/tech/slave.txt> for a full list of available commands   
  
### Events

The following events are available: 

Event  | Description   
---|---  
OnPlay  | This event is broadcast immediately after mplayer finishes initialising the file to be played. At the point this event is launched the VideoInfo and AudioInfo records will be populated   
OnPlaying  | This event is broadcast regularly while the multimedia file is playing. You can use this event to keep a TTrackbar synchronised with the playback   
OnStop  | This event is broadcast when the multimedia file stops. As soon as this event is launched the VideoInfo and AudioInfo records are cleared   
OnFeedback  | mplayer is polled frequently by TMPlayerControl. Most text output on the standard console will be available through the TStringList parameter. Some housekeeping information (such as the frequent requests for video position) are removed from this TStringList parameter.   
OnError  | mplayer is polled frequently by TMPlayerControl. All text output on the error console will be available through the TStringList parameter.   
OnGrabImage  | This event is raised following an GrabImage request. If successfull, then the filename to the image is returned.   
OnClick, OnMouseUp, OnMouseDown, OnMouseWheel  | Standard LCL events. OnMouseWheel can be used to allow the video to scroll through the video. An example of this is provided in the FullFeatured demo.   
  
## See also

[Video Playback Libraries](<Video_Playback_Libraries.md> "Video Playback Libraries")

---

_Source: [https://wiki.freepascal.org/TMPlayerControl](https://web.archive.org/web/20250219061738/https://wiki.freepascal.org/TMPlayerControl)_
