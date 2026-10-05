# Metal Framework

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Metal and Lazarus
    * 2.1 Requirements
    * 2.2 Errata
    * 2.3 Lazarus Example Code
    * 2.4 Metal debugging in Xcode
  * 3 External links



## Overview

Graphics processors are designed to quickly render graphics and perform data-parallel calculations. The Apple Metal framework allows you to communicate directly with the graphics processor available on a device. Applications that render complex scenes or that perform advanced scientific calculations can use this power to achieve maximum performance. Such applications include: 

  * Games that render sophisticated 3D environments;
  * Video processing applications, like Final Cut Pro;
  * Data-crunching applications, such as those used to perform scientific research.



Metal works hand-in-hand with other Apple frameworks that supplement its capability. MetalKit simplifies the task of getting your Metal content onscreen. Metal Performance Shaders implement custom rendering functions or to take advantage of a large library of existing functions. 

Many high level Apple frameworks are built on top of Metal to take advantage of its performance, including Core Image, SpriteKit, and SceneKit. Using one of these high-level frameworks shields you from the details of graphics processor programming, but writing custom Metal code enables you to achieve the highest level of performance. 

## Metal and Lazarus

Metal support in Lazarus uses Ryan Joseph's [lazmetalcontrol](<https://github.com/genericptr/Metal-Framework/tree/master/lazarus/package>). 

### Requirements

  * You will need to have the latest version of Lazarus installed on a macOS computer (or better yet, have the latest Lazarus SVN version).
  * You will need to download the Lazarus Metal Package (lazmetalcontrol) by Ryan Joseph.
  * From Lazarus, choose the Package menu -> Open Package File and select lazmetalcontrol.lpk - install this into Lazarus.
  * To use Metal you will need macOS 10.11 (El Capitan) or later and a Mac computer that supports Metal - typically Macs from 2012 and later.



### Errata

The Lazarus 2.0.6 release for macOS contained a regression for OpenGL support. See Bugtracker [Issue #35797](<https://bugs.freepascal.org/view.php?id=35797>) GLCocoaNSContext.pas now requires "cocoa_extra" in the _Uses_ section. 

### Lazarus Example Code

[ChrisR](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=57624>) has created a few Lazarus projects that can be compiled for Apple's Metal Framework. The projects can be downloaded from [GitHub](<https://github.com/neurolabusc/Metal-Demos>) and will compile on Lazarus provided you have met the requirements outlined above. 

Unfortunately the documentation is sparse, but both the [main page](<https://github.com/neurolabusc/Metal-Demos>) and the [line project](<https://github.com/neurolabusc/Metal-Demos/tree/master/lines>) include some useful information. 

Most of the projects can also be compiled to support [OpenGL 3.3 Core](<OpenGL.md> "OpenGL") \- providing support for Windows, Linux and older Macs. By keeping a thin layer for either OpenGL or Metal, one can retain most of the Lazarus _write once, compile everywhere_ magic. 

These example projects all use Lazarus. However, [Ryan's repository](<https://github.com/genericptr/Metal-Framework>) also includes some examples of how to use Metal directly from Free Pascal without Lazarus. 

See this [Forum post](<https://forum.lazarus.freepascal.org/index.php/topic,42306.msg366256.html#msg366256>) with demo project for offscreen rendering in Metal. 

### Metal debugging in Xcode

If your Lazarus application uses Metal graphics, Igor Kokarev explains below how you can save a GPU trace to a file and then open it in Xcode to check the performance of Metal, view much useful information about your 3D commands and even see a visualization of your 3D scene. Note that this requires macOS 10.15 Catalina or newer and FPC 3.2.0. 

**Step 1** Download Ryan Joseph's [lazmetalcontrol](<https://github.com/genericptr/Metal-Framework/tree/master/lazarus/package>). 

**Step 2** Replace the file `MTLCaptureManager.inc` with this one: 
    
    
    { Parsed from Metal.framework /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk/System/Library/Frameworks/Metal.framework/Headers/MTLCaptureManager.h }
    
    {$ifdef TYPES}
    type
      MTLCaptureDescriptorPtr = ^MTLCaptureDescriptor;
      MTLCaptureManagerPtr = ^MTLCaptureManager;
    {$endif}
    
    {$ifdef EXTERNAL_SYMBOLS}
    //var
    //  MTLCaptureErrorDomain: NSErrorDomain; cvar; external;
    {$endif}
    
    {$ifdef TYPES}
    type
      MTLCaptureError = NSInteger;
      MTLCaptureErrorPtr = ^MTLCaptureError;
    
    const
      MTLCaptureErrorNotSupported = 1;
      MTLCaptureErrorAlreadyCapturing = 2;
      MTLCaptureErrorInvalidDescriptor = 3;
    
    type
      MTLCaptureDestination = NSInteger;
      MTLCaptureDestinationPtr = ^MTLCaptureDestination;
    
    const
      MTLCaptureDestinationDeveloperTools = 1;
      MTLCaptureDestinationGPUTraceDocument = 2;
    {$endif}
    
    {$ifdef CLASSES}
    
    type
      MTLCaptureDescriptor = objcclass external (NSObject, NSCopyingProtocol)
      private
        _captureObject: id;
        _destination: MTLCaptureDestination;
        _outputURL: NSURL;
      public
        procedure setCaptureObject(newValue: id); message 'setCaptureObject:';
        function captureObject: id; message 'captureObject';
        procedure setDestination(newValue: MTLCaptureDestination); message 'setDestination:';
        function destination: MTLCaptureDestination; message 'destination';
        procedure setOutputURL(newValue: NSURL); message 'setOutputURL:';
        function outputURL: NSURL; message 'outputURL';
    
        { Adopted protocols }
        function copyWithZone (zone: NSZonePtr): id; message 'copyWithZone:';
      end;
    
    type
      MTLCaptureManager = objcclass external (NSObject)
      private
        _isCapturing: objcbool;
        _defaultCaptureScope: MTLCaptureScopeProtocol;
      public
        class function sharedCaptureManager: MTLCaptureManager; message 'sharedCaptureManager';
        function init: id; message 'init'; { unavailable in macos, ios }
        function newCaptureScopeWithDevice (device: id): id; message 'newCaptureScopeWithDevice:';
        function newCaptureScopeWithCommandQueue (commandQueue: id): id; message 'newCaptureScopeWithCommandQueue:';
        function supportsDestination (destination: MTLCaptureDestination): objcbool; message 'supportsDestination:'; { available in macos(10.15), ios(13.0) }
        function startCaptureWithDescriptor_error (descriptor: MTLCaptureDescriptor; error: NSErrorPtr): objcbool; message 'startCaptureWithDescriptor:error:'; { available in macos(10.15), ios(13.0) }
        procedure startCaptureWithDevice (device: id); message 'startCaptureWithDevice:'; deprecated 'Use startCaptureWithDescriptor:error: instead in macos(10.13, 10.15), ios(11.0, 13.0)';
        procedure startCaptureWithCommandQueue (commandQueue: id); message 'startCaptureWithCommandQueue:'; deprecated 'Use startCaptureWithDescriptor:error: instead in macos(10.13, 10.15), ios(11.0, 13.0)';
        procedure startCaptureWithScope (captureScope: id); message 'startCaptureWithScope:'; deprecated 'Use startCaptureWithDescriptor:error: instead in macos(10.13, 10.15), ios(11.0, 13.0)';
        procedure stopCapture; message 'stopCapture';
        procedure setDefaultCaptureScope(newValue: id); message 'setDefaultCaptureScope:';
        function defaultCaptureScope: id; message 'defaultCaptureScope';
        function isCapturing: objcbool; message 'isCapturing';
      end;
    {$endif}
    

This updated header includes a new Cocoa interface - MTLCaptureDescriptor. 

**Step 3** Insert a new text line in the existing file `DefinedClassesMetal.pas`: 
    
    
    MTLCaptureDescriptor = objcclass external;
    

Ryan will probably include both changes (Steps 2 and 3) in a future update to his Metal framework. 

**Step 4** Add this key and value to the `Info.plist` file in your application bundle: 
    
    
    <key>MetalCaptureEnabled</key>
      <true/>
    

Note: remove this key when you do a release build for your application because this key has a small negative effect on Metal performance. 

**Step 5** Add the following code for your Metal application: 
    
    
    captureManager:    MTLCaptureManager;
    captureDescriptor: MTLCaptureDescriptor;
    
    
    
    procedure TMetalContext.StartMetalDebug;
    var
      nserr:   NSError;
      //str:     string;
    begin
      // GPU trace for debugging of Metal
      captureManager := MTLCaptureManager.sharedCaptureManager;
      if captureManager.supportsDestination(MTLCaptureDestinationGPUTraceDocument) then 
        begin
          captureDescriptor := MTLCaptureDescriptor.alloc.init.autorelease;
          captureDescriptor.SetCaptureObject(device);
          captureDescriptor.SetDestination(MTLCaptureDestinationGPUTraceDocument);
          captureDescriptor.setOutputURL(NSURL.URLWithString(StrToNSStr('file:///Users/mac/Development/myapp.gputrace')));
    
          if not captureManager.startCaptureWithDescriptor_error(captureDescriptor, @nserr) then 
            begin
              // Metal captiring failed.
              exit;
            end;
         
          //str := NSStrToStr(nserr.localizedDescription);
        end;
    end;
    
    procedure TMetalContext.StopMetalDebug;
    begin
      captureManager.stopCapture;
    end;
    

A saved file with GPU trace should use the `.gputrace` file extension. 

When you call `stopCapture()` the application will save the *.gputrace file. Double click on this file in the Finder and it will be opened in Xcode for you to explore the Metal performance and 3D data. 

As it is only possible to capture one frame at a time, just call the _start_ and _stop_ commands before and after you render each frame. 

More information is available in the Apple documentation: 

  * [Capturing GPU Command Data Programmatically](<https://developer.apple.com/documentation/metal/frame_capture_debugging_tools/capturing_gpu_command_data_programmatically>).



## External links

  * [Apple: Sample Code](<https://developer.apple.com/metal/sample-code/>)
  * [Apple: Metal Framework Documentation](<https://developer.apple.com/documentation/metal>)
  * [Apple: Metal Programming Guide](<https://developer.apple.com/library/archive/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/>)
  * [Apple: Metal Best Practices Guide](<https://developer.apple.com/library/archive/documentation/3DDrawing/Conceptual/MTLBestPracticesGuide/>)
  * [Apple: Metal Videos](<https://developer.apple.com/videos/graphics-and-games/metal>)
  * [Apple: Metal Forum](<https://developer.apple.com/forums/tags/metal>)


  * **[Apple: Porting Your Metal Code to Apple Silicon](<https://developer.apple.com/documentation/metal/porting_your_metal_code_to_apple_silicon>)**

---

_Source: [https://wiki.freepascal.org/Metal_Framework](https://web.archive.org/web/20231129023918/https://wiki.freepascal.org/Metal_Framework)_
