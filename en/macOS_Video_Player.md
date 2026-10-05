# macOS Video Player

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Supported file types
  * 3 AVPlayer vs AVAudioPlayer
  * 4 Application Transport Security
  * 5 Example code using AVPlayerView
  * 6 Example code using AVPlayerLayer for a custom player
  * 7 Detecting invalid AVAsset URLs
  * 8 Virtual machine issues with remote content
  * 9 See also
  * 10 External links



## Overview

AVPlayer is a controller object used to manage the playback and timing of a media asset. You can use an AVPlayer to play local and remote file-based media, such as QuickTime movies and MP3 audio files, as well as audiovisual media served using HTTPS Live Streaming. AVPlayer is for playing a single media asset at a time. You can reuse the player instance to play additional media assets using its _replaceCurrentItemWithPlayerItem:_ method. The AVFoundation framework also provides a subclass of AVPlayer, called AVQueuePlayer, to create and manage the queuing of media assets to be played sequentially. 

An AVPlayer play media assets, which the AVFoundation framework models using the AVAsset class. AVAsset only models the static aspects of the media, such as its duration or creation date, and on its own, is unsuitable for playback with an AVPlayer. To play an asset, you need to create an instance of its dynamic counterpart found in AVPlayerItem. This object models the timing and presentation state of an asset played by an instance of AVPlayer. 

AVPlayer and AVPlayerItem are nonvisual objects which are unable to present an asset’s video onscreen. There are two main approaches you can take to present your video content onscreen: 

  * AVKit: Apple recommends this as the best way to present your video content with the AVKit framework’s AVPlayerView class which presents the video content, along with playback controls and other media features giving you a full-featured playback experience.


  * AVPlayerLayer: If you want to build a custom interface for your player, use AVPlayerLayer. The player layer can be set as a view’s backing layer or can be added directly to the layer hierarchy. Unlike AVPlayerView, a player layer doesn’t present any playback controls, it just presents the visual content onscreen. You need to build the playback transport controls to play, pause, and seek through the media.



## Supported file types

AVPlayer supports media with the following Uniform Type Identifiers: 

public.mpeg | public.mpeg-2-video   
---|---  
public.avi | public.aifc-audio   
public.aac-audio | public.mpeg-4   
public.au-audio | public.aiff-audio   
public.mp2 | public.3gpp2   
public.ac3-audio | public.mp3   
public.mpeg-2-transport-stream | public.3gpp   
public.mpeg-4-audio   
  
AvPlayer supports media with the following file extensions: 

caf | ttml | au | ts | mqv | pls | flac | dv | amr | mp1   
---|---|---|---|---|---|---|---|---|---  
mp3 | ac3 | loas | 3gp | aifc | m2v | m2t | m4b | m2a | m4r   
aa | webvtt | aiff | m4a | scc | mp4 | m4p | mp2 | eac3 | mpa   
vob | scc | aax | mpg | wav | mov | itt | xhe | m3u | m3u8   
mts | mod | vtt | m4v | 3g2 | sc2 | aac | mp4 | vtt | m1a   
mp2 | avi   
  
## AVPlayer vs AVAudioPlayer

AVPlayer also plays audio only files as does [AVAudioPlayer](<macOS_Audio_Player.md> "macOS Audio Player"), so what are the advantages or disadvantages of using AVPlayer? 

  * The documentation for AVPlayer states that the player works equally well with local and remote media files
  * The documentation for AVAudioPlayer states that Apple recommends that you use this class for audio playback unless you are playing audio captured from a network stream.
  * AVPlayer has more capabilities - see Apple's documentation (links below).



## Application Transport Security

Security. Yes, it sometimes gets in the way. If you try loading your remote content using an HTTP scheme, you will be prevented from doing so by Application Transport Security. To overcome this unwelcome security intervention, you will need to add this to your application's [.plist file](<macOS_property_list_files.md> "macOS property list files"): 
    
    
      <key>NSAppTransportSecurity</key>
        <dict>
            <key>NSAllowsArbitraryLoadsForMedia</key>
                <true/>
        </dict>
    

In the code examples below, loading the Apple URL _https://devstreaming-cdn.apple.com/videos/streaming/examples/img_bipbop_adv_example_ts/master.m3u8_ **may** require this addition to the .plist file: 
    
    
      <key>NSAppTransportSecurity</key>
        <dict>
          <key>NSExceptionDomains</key>
            <dict>
              <key>devstreaming-cdn.apple.com</key>
                <dict>
                  <key>NSExceptionRequiresForwardSecrecy</key>
                    <false/>
                </dict>
            </dict>
        </dict>
    

I say _may_ because I started this code nearly two years ago I had included it, but with the effluxion of time no longer recall why it was needed. When testing today, the applications worked without it - <shrug/>. 

## Example code using AVPlayerView
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework AvKit}
    {$linkframework AVFoundation}
    
    // Enable/Disbale DEBUGGING
    //{$define DEBUG}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls,
      CTypes,
      MacOSAll,
      {$ifdef DEBUG}
      Cocoautils,
      {$endif}
      CocoaAll;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        LoadButton: TButton;
        QuitButton: TButton;
        procedure FormResize(Sender: TObject);
        procedure LoadButtonClick(Sender: TObject);
        procedure QuitButtonClick(Sender: TObject);
      private
    
      public
    
      end;
    
      AVPlayerStatus = NSInteger;
      AVPlayerItemStatus = NSInteger;
      AVKeyValueStatus = NSInteger;
      AVAssetReferenceRestrictions = NSUInteger;
      AVPlayerViewControlsStyle = NSInteger;
    
      CMTimeValue = cint64;
      CMTimeValuePtr = ^CMTimeValue;
      CMTimeScale = cint32;
      CMTimeScalePtr = ^CMTimeScale;
      CMTimeEpoch = cint64;
      CMTimeEpochPtr = ^CMTimeEpoch;
      CMTimeFlags = cuint32;
      CMTimeFlagsPtr = ^CMTimeFlags;
    
      CMTime = record
        value: CMTimeValue;
        timescale: CMTimeScale;
        flags: CMTimeFlags;
        epoch: CMTimeEpoch;
      end;
    
    
      AVAsynchronousKeyValueLoadingProtocol = objcprotocol external name 'AVAsynchronousKeyValueLoading'
      required
        function statusOfValueForKey_error (key: NSString; outError: NSErrorPtr): AVKeyValueStatus;
          message 'statusOfValueForKey:error:';
        procedure loadValuesAsynchronouslyForKeys_completionHandler (keys: NSArray; handler: OpaqueCBlock);
          message 'loadValuesAsynchronouslyForKeys:completionHandler:';
      end;
    
      AVAsset = objcclass external (NSObject, NSCopyingProtocol, AVAsynchronousKeyValueLoadingProtocol)
        private
          _asset: id; //AVAssetInternal;
        public
          class function assetWithURL (URL: NSURL): id; message 'assetWithURL:';
          function duration: CMTime; message 'duration';
          function preferredRate: single; message 'preferredRate';
          function preferredVolume: single; message 'preferredVolume';
          function preferredTransform: CGAffineTransform; message 'preferredTransform';
          function naturalSize: CGSize; message 'naturalSize';
    
          { Adopted protocols }
          function copyWithZone (mzone: NSZonePtr): id; message 'copyWithZone:';
          procedure loadValuesAsynchronouslyForKeys_completionHandler (keys: NSArray; handler: OpaqueCBlock);
            message 'loadValuesAsynchronouslyForKeys:completionHandler:';
          function statusOfValueForKey_error (mkey: NSString; outError: NSErrorPtr): AVKeyValueStatus;
            message 'statusOfValueForKey:error:';
        end;
    
      AVPlayerItem = objcclass external (NSObject, NSCopyingProtocol)
      private
        _playerItem: id; //AVPlayerItemInternal;
      public
        class function playerItemWithURL (URL: NSURL): AVPlayerItem; message 'playerItemWithURL:';
        class function playerItemWithAsset (asset: AVAsset): AVPlayerItem; message 'playerItemWithAsset:';
        class function playerItemWithAsset_automaticallyLoadedAssetKeys (asset: AVAsset; automaticallyLoadedAssetKeys: NSArray): AVPlayerItem;
          message 'playerItemWithAsset:automaticallyLoadedAssetKeys:'; { available in 10_9, 7_0 }
        function initWithURL (URL: NSURL): id; message 'initWithURL:';
        function initWithAsset (asset: AVAsset): id; message 'initWithAsset:';
        function initWithAsset_automaticallyLoadedAssetKeys (asset: AVAsset; automaticallyLoadedAssetKeys: NSArray): id;
          message 'initWithAsset:automaticallyLoadedAssetKeys:'; { available in 10_9, 7_0 }
        function status: AVPlayerItemStatus; message 'status';
        function duration: CMTime; message 'duration';
        function error: NSError; message 'error';
    
        { Adopted protocols }
        function copyWithZone (mzone: NSZonePtr): id; message 'copyWithZone:';
      end;
    
      AVPlayer = objcclass external (NSObject)
        private
          _player: id; //AVPlayerInternal;
        public
          class function playerWithURL (URL: NSURL): id; message 'playerWithURL:';
          class function playerWithPlayerItem (item: AVPlayerItem): id; message 'playerWithPlayerItem:';
          function initWithURL (URL: NSURL): id; message 'initWithURL:';
          function initWithPlayerItem (item: AVPlayerItem): id; message 'initWithPlayerItem:';
          function currentItem: AVPlayerItem; message 'currentItem';
          function status: AVPlayerStatus; message 'status';
          function error: NSError; message 'error';
          procedure setVolume(newValue: Single); message 'setVolume:';
          procedure replaceCurrentItemWithPlayerItem(newItem: AVPlayerItem); message 'replaceCurrentItemWithPlayerItem:';
        end;
    
      AVPlayerPlaybackControl = objccategory external (AVPlayer)
        procedure setRate(newValue: single); message 'setRate:';
        function rate: single; message 'rate';
        procedure play; message 'play';
        procedure pause; message 'pause';
      end;
    
      AVPlayerView = objcclass external (NSView)
      public
        procedure setPlayer(newValue: AVPlayer); message 'setPlayer:';
        function player: AVPlayer; message 'player';
        procedure setControlsStyle(newValue: AVPlayerViewControlsStyle); message 'setControlsStyle:';
        function controlsStyle: AVPlayerViewControlsStyle; message 'controlsStyle';
        procedure setVideoGravity(newValue: NSString); message 'setVideoGravity:';
        function videoGravity: NSString; message 'videoGravity';
        function isReadyForDisplay: ObjCBOOL; message 'isReadyForDisplay';
        procedure setNeedsDisplay(newValue: ObjCBool); message 'setNeedsDisplay:';
        function videoBounds: NSRect; message 'videoBounds';
        function contentOverlayView: NSView; message 'contentOverlayView';
      end;
    
    
    const
      AVPlayerStatusUnknown = 0;
      AVPlayerStatusReadyToPlay = 1;
      AVPlayerStatusFailed = 2;
      AVPlayerItemStatusUnknown = 0;
      AVPlayerItemStatusReadyToPlay = 1;
      AVPlayerItemStatusFailed = 2;
      AVPlayerViewControlsStyleNone = 0;
      AVPlayerViewControlsStyleInline = 1;
      AVPlayerViewControlsStyleFloating = 2;
      AVPlayerViewControlsStyleMinimal = 3;
      AVPlayerViewControlsStyleDefault = AVPlayerViewControlsStyleInline;
      Resize = 2;
      ResizeAspect = 0;
      ResizeAspectFill = 1;
    
    var
      Form1: TForm1;
      myPlayer: AVPlayer = Nil;
      myAsset: AVAsset = Nil;
      myPlayerItem: AVPlayerItem = Nil;
      myPlayerView: AVPLayerView = Nil;
      myURL: NSUrl = Nil;
      myView: NSView = Nil;
      mins: Double = 0;
      secs: Double = 0;
      sixtysecs: Double = 60;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.LoadButtonClick(Sender: TObject);
    begin
      // Prevent multiple instances of myPlayer
      if(myPlayer <> Nil) then
        begin
          myPlayer.pause;
          myPlayer.replaceCurrentItemWithPlayerItem(Nil);
          myPlayer := Nil;
        end;
    
      //
      // 1. Create a URL
      //
      myURL := NSURL.URLWithString(NSSTR(PAnsiChar('https://devstreaming-cdn.apple.com/videos/streaming/examples/img_bipbop_adv_example_ts/master.m3u8')));
      //myURL := NSURL.fileURLWithPath(NSStr(PAnsiChar('/Users/trev/alt.mp4')));
      if(myURL = Nil) then
        ShowMessage('myURL creation failed!');
    
      //
      // 2. Create AVAsset, AVPlayerItem and AVPlayer objects
      //
      myAsset := AVAsset.alloc.assetWithURL(myURL);
      myPlayerItem := AVPlayerItem.alloc.initWithAsset(myAsset);
      myPlayer := AVPlayer.alloc.initWithPlayerItem(myPlayerItem);
    
      // wait while media loads for playback
      while(myPlayerItem.Status = AVPlayerItemStatusUnknown)
        do
          application.processmessages;
    
      {$ifdef DEBUG}
      // Status information for debugging
      if(myPlayer.Status = AVPlayerStatusFailed) then
        ShowMessage('AVplayer status: failed')
      else if(myPlayer.Status = AVPlayerStatusReadyToPlay) then
        ShowMessage('AVplayer status: ready to play');
      if(myPlayerItem.Status = AVPlayerItemStatusFailed) then
        ShowMessage('AVplayerItem status: failed' + '-' + NSStringToString(myPlayerItem.error.description))
      else if(myPlayerItem.Status = AVPlayerItemStatusReadyToPlay) then
        ShowMessage('AVplayerItem status: ready to play');
      {$endif}
    
      //
      // 3. Create playerView object to display video
      //
      myPlayerView := AVPlayerView.alloc.initWithFrame(CGRectMake(0, 0, myView.frame.size.width, myView.frame.size.height));
      myPlayerView.setPlayer(myPlayer);
      myPlayerView.setControlsStyle(AVPlayerViewControlsStyleDefault);
    
      // Enable resizing on the fly
      myPlayerView.setVideoGravity(NSStr('AVLayerVideoGravityResizeAspect'));
      myPlayerView.setAutoresizingMask(kCALayerHeightSizable or kCALayerWidthSizable);
    
      // Make visible by adding PlayerView to Form1 as a subView
      myView := NSView(Form1.Handle);
      myView.addSubview(myPlayerView);
    
      // Setup video frame size and bounds
      myPlayerView.setFrame(CGRectMake(0, 0, myView.frame.size.width, myView.frame.size.height -70));
      myPlayerView.setBounds(CGRectMake(0, 0, myView.frame.size.width, myView.frame.size.height -70));
    
      {$ifdef DEBUG}
      // Status information for debugging
      mins :=  myPlayer.currentItem.duration.value / myPlayer.currentItem.duration.timescale / sixtysecs;
      secs :=  (myPlayer.currentItem.duration.value / myPlayer.currentItem.duration.timescale) - (Trunc(mins) * sixtysecs);
    
      ShowMessage('Frame Height: ' + FloatToStr(myView.frame.size.height) + LineEnding
      + 'Frame Width  : ' + FloatToStr(myView.frame.size.width) + LineEnding
      + 'Video gravity: ' + NSStringToString(myPlayerView.videoGravity) + LineEnding
      + 'Needs display: ' + BoolToStr(myPlayerView.needsDisplay) + LineEnding
      + 'Timescale: ' + FloatToStr(myPlayer.currentItem.duration.timescale) + LineEnding
      + 'Duration: ' + FloatToStr(myPlayer.currentItem.duration.value) + LineEnding
      + 'Flags: ' + FloatToStr(myPlayer.currentItem.duration.flags) + LineEnding
      + 'Epoch: ' + FloatToStr(myPlayer.currentItem.duration.epoch) + LineEnding
      + 'Minutes: ' + FloatToStr(myPlayer.currentItem.duration.value / myPlayer.currentItem.duration.timescale / sixtysecs) + LineEnding
      + 'MM:SS: ' +  FloatToStr(Trunc(mins)) + ':' + FloatToStr(Trunc(Round(secs))) + LineEnding
      + 'Control style: ' + IntToStr(myPlayerView.controlsStyle));
    
      if(myPlayerView.isReadyForDisplay) then
        ShowMessage('Ready for display')
      else
        ShowMessage('Not ready for display');
      {$endif}
    end;
    
    //
    // Resize PlayerView when Form resized
    //
    procedure TForm1.FormResize(Sender: TObject);
    begin
      // Reset video frame size and bounds
      myPlayerView.setFrame(CGRectMake(0, 0, myView.frame.size.width, myView.frame.size.height -70));
      myPlayerView.setBounds(CGRectMake(0, 0, myView.frame.size.width, myView.frame.size.height -70));
    end;
    
    //
    // Quit player
    //
    procedure TForm1.QuitButtonClick(Sender: TObject);
    begin
      Close;
    end;
    
    finalization
    
    // release created objects
    myPlayerView.release;
    myPlayer.release;
    myAsset.release;
    
    end.
    

Full source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_avplayer_view/>). 

## Example code using AVPlayerLayer for a custom player
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    {$linkframework AvKit}
    {$linkframework AVFoundation}
    
    // Enable/Disbale DEBUGGING
    //{$define DEBUG}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls,
      CTypes,
      MacOSAll,
      {$ifdef DEBUG}
      CocoaUtils,
      {$endif}
      CocoaAll;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        ResumeButton: TButton;
        PauseButton: TButton;
        PlayButton: TButton;
        QuitButton: TButton;
        procedure PauseButtonClick(Sender: TObject);
        procedure PlayButtonClick(Sender: TObject);
        procedure QuitButtonClick(Sender: TObject);
        procedure ResumeButtonClick(Sender: TObject);
      private
    
      public
    
      end;
    
      AVPlayerStatus = NSInteger;
      AVPlayerItemStatus = NSInteger;
      AVKeyValueStatus = NSInteger;
    
      CMTimeValue = cint64;
      CMTimeValuePtr = ^CMTimeValue;
      CMTimeScale = cint32;
      CMTimeScalePtr = ^CMTimeScale;
      CMTimeEpoch = cint64;
      CMTimeEpochPtr = ^CMTimeEpoch;
      CMTimeFlags = cuint32;
      CMTimeFlagsPtr = ^CMTimeFlags;
    
      CMTime = record
        value: CMTimeValue;
        timescale: CMTimeScale;
        flags: CMTimeFlags;
        epoch: CMTimeEpoch;
      end;
    
      AVAsynchronousKeyValueLoadingProtocol = objcprotocol external name 'AVAsynchronousKeyValueLoading' required
        function statusOfValueForKey_error (key: NSString; outError: NSErrorPtr): AVKeyValueStatus;
          message 'statusOfValueForKey:error:';
        procedure loadValuesAsynchronouslyForKeys_completionHandler (keys: NSArray; handler: OpaqueCBlock);
          message 'loadValuesAsynchronouslyForKeys:completionHandler:';
      end;
    
      AVAsset = objcclass external (NSObject, NSCopyingProtocol, AVAsynchronousKeyValueLoadingProtocol)
        private
          _asset: id; //AVAssetInternal;
        public
          class function assetWithURL (URL: NSURL): id; message 'assetWithURL:';
          function duration: CMTime; message 'duration';
          function preferredRate: single; message 'preferredRate';
          function preferredVolume: single; message 'preferredVolume';
          function preferredTransform: CGAffineTransform; message 'preferredTransform';
          function naturalSize: CGSize; message 'naturalSize';
    
          { Adopted protocols }
          function copyWithZone (mzone: NSZonePtr): id; message 'copyWithZone:';
          procedure loadValuesAsynchronouslyForKeys_completionHandler (keys: NSArray; handler: OpaqueCBlock);
            message 'loadValuesAsynchronouslyForKeys:completionHandler:';
          function statusOfValueForKey_error (mkey: NSString; outError: NSErrorPtr): AVKeyValueStatus;
            message 'statusOfValueForKey:error:';
        end;
    
      AVPlayerItem = objcclass external (NSObject, NSCopyingProtocol)
      private
        _playerItem: id; //AVPlayerItemInternal;
      public
        class function playerItemWithURL (URL: NSURL): AVPlayerItem; message 'playerItemWithURL:';
        class function playerItemWithAsset (asset: AVAsset): AVPlayerItem; message 'playerItemWithAsset:';
        class function playerItemWithAsset_automaticallyLoadedAssetKeys (asset: AVAsset; automaticallyLoadedAssetKeys: NSArray): AVPlayerItem;
          message 'playerItemWithAsset:automaticallyLoadedAssetKeys:'; { available in 10_9, 7_0 }
        function initWithURL (URL: NSURL): id; message 'initWithURL:';
        function initWithAsset (asset: AVAsset): id; message 'initWithAsset:';
        function initWithAsset_automaticallyLoadedAssetKeys (asset: AVAsset; automaticallyLoadedAssetKeys: NSArray): id;
          message 'initWithAsset:automaticallyLoadedAssetKeys:'; { available in 10_9, 7_0 }
        function status: AVPlayerItemStatus; message 'status';
        function error: NSError; message 'error';
    
        { Adopted protocols }
        function copyWithZone (mzone: NSZonePtr): id; message 'copyWithZone:';
      end;
    
      AVPlayer = objcclass external (NSObject)
        private
          _player: id; //AVPlayerInternal;
        public
          class function playerWithURL (URL: NSURL): id; message 'playerWithURL:';
          class function playerWithPlayerItem (item: AVPlayerItem): id; message 'playerWithPlayerItem:';
          function initWithURL (URL: NSURL): id; message 'initWithURL:';
          function initWithPlayerItem (item: AVPlayerItem): id; message 'initWithPlayerItem:';
          function status: AVPlayerStatus; message 'status';
          function error: NSError; message 'error';
          procedure setVolume(newValue: Single); message 'setVolume:';
          procedure replaceCurrentItemWithPlayerItem(newItem: AVPlayerItem); message 'replaceCurrentItemWithPlayerItem:'; 
        end;
    
      AVURLAsset = objcclass external (AVAsset)
      private
        _URLAsset: id; //AVURLAssetInternal;
      public
        class function audiovisualTypes: NSArray; message 'audiovisualTypes'; { available in 10_7, 5_0 }
        class function audiovisualMIMETypes: NSArray; message 'audiovisualMIMETypes'; { available in 10_7, 5_0 }
        class function isPlayableExtendedMIMEType (extendedMIMEType: NSString): ObjCBOOL;
          message 'isPlayableExtendedMIMEType:'; { available in 10_7, 5_0 }
        class function URLAssetWithURL_options (URL: NSURL; options: NSDictionary): AVURLAsset;
          message 'URLAssetWithURL:options:';
        function initWithURL_options (URL: NSURL; options: NSDictionary): id;
          message 'initWithURL:options:';
        function URL: NSURL; message 'URL';
      end;
    
      AVPlayerPlaybackControl = objccategory external (AVPlayer)
        procedure setRate(newValue: single); message 'setRate:';
        function rate: single; message 'rate';
        procedure play; message 'play';
        procedure pause; message 'pause';
      end;
    
      AVPlayerLayer = objcclass external (CALayer)
        private
          _playerLayer: id; //AVPlayerLayerInternal;
        public
          class function playerLayerWithPlayer (player: AVPlayer): AVPlayerLayer;
            message 'playerLayerWithPlayer:';
          procedure setPlayer(newValue: AVPlayer); message 'setPlayer:';
          function player: AVPlayer; message 'player';
          procedure setVideoGravity(newValue: NSString); message 'setVideoGravity:';
          function videoGravity: NSString; message 'videoGravity';
          function isReadyForDisplay: ObjCBOOL; message 'isReadyForDisplay';
          function videoRect: CGRect; message 'videoRect';
      end;
    
    const
      AVPlayerStatusUnknown = 0;
      AVPlayerStatusReadyToPlay = 1;
      AVPlayerStatusFailed = 2;
    
      AVPlayerItemStatusUnknown = 0;
      AVPlayerItemStatusReadyToPlay = 1;
      AVPlayerItemStatusFailed = 2;
    
    var
      Form1: TForm1;
      myPlayer: AVPlayer = Nil;
      myAsset: AVAsset = Nil;
      myPlayerItem: AVPlayerItem = Nil;
      myPlayerLayer: AVPlayerLayer = Nil;
      myURL: NSUrl = Nil;
      myView: NSView = Nil;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    //
    // Play video or audio asset
    //
    procedure TForm1.PlayButtonClick(Sender: TObject);
    begin
      // Prevent multiple instances of myPlayer
      if(myPlayer <> Nil) then
        begin
          myPlayer.pause;
          myPlayer.replaceCurrentItemWithPlayerItem(Nil);
          myPlayer := Nil;
        end;  
    
      //
      // 1. Create a URL
      //
      myURL := NSURL.URLWithString(NSSTR(PAnsiChar('https://devstreaming-cdn.apple.com/videos/streaming/examples/img_bipbop_adv_example_ts/master.m3u8')));
      //myURL := NSURL.fileURLWithPath(NSStr(PAnsiChar('/Users/trev/alt.mp4')));
      if(myURL = Nil) then
        begin
          ShowMessage('myURL creation failed!');
          exit;
        end;
    
      //
      // 2. Create AVAsset, AVPlayerItem and AVPlayer objects
      //
      myAsset := AVAsset.alloc.assetWithURL(myURL);
      myPlayerItem := AVPlayerItem.alloc.initWithAsset(myAsset);
      myPlayer := AVPlayer.alloc.initWithPlayerItem(myPlayerItem);
    
      // Display media loading message 
      loadingLabel.caption := 'Loading ...';  // or set in the Object Inspector 
      loadingLabel.visible := True;           // set to false in the Object Inspector
    
      // Wait until status is known (while loading AVAsset media)
      while(myPlayer.Status = AVPlayerStatusUnknown)
        do
          application.processmessages;
    
      loadingLabel.visible := False;          // hide label now media loaded
    
      {$ifdef DEBUG}
      // Status information for debugging
      if(myPlayer.Status = AVPlayerStatusFailed) then
        ShowMessage('AVplayer status: failed')
      else if(myPlayer.Status = AVPlayerStatusReadyToPlay) then
        ShowMessage('AVplayer status: ready to play');
      {$endif}
    
      //
      // 3. Create PlayerLayer to display video
      //
      myPlayerLayer :=  AVPlayerLayer.playerLayerWithPlayer(myPlayer);
    
      // enable resizing on the fly
      myPlayerLayer.setVideoGravity(NSStr('AVLayerVideoGravityResizeAspect'));
      myPlayerLayer.setAutoresizingMask(kCALayerHeightSizable or kCALayerWidthSizable);
    
      // setup border color and border width
      myPlayerLayer.setBorderWidth(3);
      myPlayerLayer.setBorderColor(NSColor.yellowColor.cgColor);
    
      // Make visible by adding PlayerLayer to Form1 View
      myView := NSView(Form1.Handle);
      myView.layer.addSublayer(myPlayerLayer);
    
      // Setup video frame size and bounds
      myPlayerLayer.setFrame(CGRectMake(0, 0, myView.frame.size.width, myView.frame.size.height - 100));
      myPlayerLayer.setBounds(CGRectMake(0, 0, myView.frame.size.width, myView.frame.size.height - 100));
    
      {$ifdef DEBUG}
      // Status Information for debugging
      ShowMessage('Frame Height: ' + FloatToStr(myView.frame.size.height) + LineEnding
      + 'Frame Width  : ' + FloatToStr(myView.frame.size.width) + LineEnding
      + 'Video gravity: ' + NSStringToString(myPlayerLayer.videoGravity) + LineEnding
      + 'Needs display (layer): ' + BoolToStr(myPlayerLayer.needsDisplay) + LineEnding);
    
      if(myPlayerLayer.isReadyForDisplay) then
        ShowMessage('PlayerLayer: Ready for display')
      else
        ShowMessage('PlayerLayer: Not ready for display');
      {$endif}
    
      //
      // 4. Play
      //
      myPlayer.setRate(1);      // play normal speed
      myPlayer.setVolume(0.5);  // 50% volume
      //myPlayer.play;          // NOT needed - setRate() starts playing at specified rate
    end;
    
    
    //
    // Pause playback
    //
    procedure TForm1.PauseButtonClick(Sender: TObject);
    begin
      myPlayer.pause;
    end;
    
    //
    // Resume playback
    //
    procedure TForm1.ResumeButtonClick(Sender: TObject);
    begin
      myPlayer.play;
    end;
    
    //
    // Quit player
    //
    procedure TForm1.QuitButtonClick(Sender: TObject);
    begin
      Close;
    end;
    
    finalisation
    
    // release created objects 
    myAsset.release;
    myPlayerItem.release;
    myPlayer.replaceCurrentItemWithPlayerItem(Nil);
    myPlayer.release;
       
    end.
    

Full source code available at [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_avplayer_layer/>). 

## Detecting invalid AVAsset URLs

If you feed AVAsset an invalid URL, you receive a very misleading error description from AVplayerItem.status. 
    
    
    if(myPlayerItem.Status = AVPlayerItemStatusFailed) then
        ShowMessage('AVplayerItem status: failed: ' + NSStringToString(myPlayerItem.error.description));
    

The above code produces the following error description: 
    
    
     AVplayerItem status: failed:{
     NSLocalizedDescription = "Operation Stopped";
     NSLocalizedFailureReason = "The server is not correctly configured.";
     NSUnderlyingError = "Error Domain=NSOSStatusErrorDomain Code=-12939 \"(null)\"";}
    

In reality, there is nothing wrong with the server at all; it's the invalid URL that causes the error. The only workaround I could find was to test the duration property value of AVAsset after myPlayerItem.Status leaves the AVPlayerItemStatusUnknown state with: 
    
    
    // Check that URL is valid
    if(myAsset.duration.value = 0) then
        begin
          ShowMessage('Error: Invalid URL - correct it and try again.');
          Exit;
        end;
    

## Virtual machine issues with remote content

The AVPlayer-based applications above do not work when retrieving remote content if you are running in a virtual machine. I have tried virtual machines in both Parallels Desktop v16 and VMware Fusion v12.2 and on two different Mac mini computers. 

Each attempt to retrieve the remote Apple file in the example code above simply results in a crossed out play symbol. The same behaviour is also exhibited by Safari, so it's not confined to these example applications. Local file content plays without issue. 

The error is: 
    
    
     Error Domain=AVFoundationErrorDomain Code=-11800
     "The operation could not be completed"
     UserInfo={NSLocalizedFailureReason=An unknown error occurred (-12151),
     NSLocalizedDescription=The operation could not be completed,
     NSUnderlyingError=0x101285070 {Error Domain=NSOSStatusErrorDomain Code=-12151"(null)"}}
    

## See also

  * [macOS Audio Player](<macOS_Audio_Player.md> "macOS Audio Player")
  * [macOS Audio Recorder](<macOS_Audio_Recorder.md> "macOS Audio Recorder")
  * [macOS MIDI Player](<macOS_MIDI_Player.md> "macOS MIDI Player")
  * [macOS NSSound](<macOS_NSSound.md> "macOS NSSound")
  * [macOS Sound Utilities](<macOS_Sound_Utilities.md> "macOS Sound Utilities")
  * [System Sound Services](<macOS_Play_Alert_Sound.md> "macOS Play Alert Sound")
  * [Multimedia Programming](<Multimedia_Programming.md> "Multimedia Programming")



## External links

  * [Apple: AVFoundation Framework](<https://developer.apple.com/documentation/avfoundation>)
  * [Apple: Media Playback and Selection](<https://developer.apple.com/documentation/avfoundation/media_playback_and_selection>)
  * [Apple: AVPlayer](<https://developer.apple.com/documentation/avfoundation/avplayer>)
  * [Apple: AVAsset](<https://developer.apple.com/documentation/avfoundation/avasset>)
  * [Apple: AVPlayerItem](<https://developer.apple.com/documentation/avfoundation/avplayeritem>)
  * [Apple: AVPlayerView](<https://developer.apple.com/documentation/avkit/avplayerview>)
  * [Apple: AVPlayerLayer](<https://developer.apple.com/documentation/avfoundation/avplayerlayer>)

---

_Source: [https://wiki.freepascal.org/macOS_Video_Player](https://web.archive.org/web/20241212095632/https://wiki.freepascal.org/macOS_Video_Player)_
