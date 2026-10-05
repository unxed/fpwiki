# ZenGL

│ **English (en)** │  **[русский (ru)](<../ru/ZenGL.md>)** │

  
ZenGL | [Tutorial 1](<ZenGL_Tutorial.md> "ZenGL Tutorial") | [Tutorial 2](<ZenGL_Tutorial_2.md> "ZenGL Tutorial 2") | [Tutorial 3](</index.php?title=ZenGL_Tutorial_3&action=edit&redlink=1> "ZenGL Tutorial 3 \(page does not exist\)") | [Edit](<ZenGL.md>)

## Contents

  * 1 About
  * 2 Links
  * 3 Tutorial
  * 4 Features
  * 5 In the updated version



## About

**ZenGL** \- a cross-platform game development library, designed to provide necessary functionality for rendering 2D graphics, handling input, sound output, etc. 

  * **Supported OS** : GNU/Linux, Windows, macOS, iOS, Android 2.1+


  * **Supported compilers** : [Free Pascal](<Free_Pascal.md> "Free Pascal"), [Delphi](<Delphi.md> "Delphi")


  * **Graphics API** : [OpenGL](<OpenGL.md> "OpenGL"), OpenGL ES 1.x, Direct3D 8/9


  * **Sound API** : OpenAL, DirectSound


  * **License** : zlib


  * **Authors** : Andrey "Andru" Kemka, Sergey "Seenkao" Shutkin



## Links

  * [Download ZenGL before version 3.12](<https://code.google.com/archive/p/zengl/>)
  * [ZenGL Mirror on GitHub](<https://github.com/skalogryz/zengl>)
  * [Bugtracker](<http://code.google.com/p/zengl/issues/list>)



\--- 

  * [ZenGL up to version 4.2 (version 4.2 has been archived and not tested, see version on SourceForge)](<https://github.com/Seenkao/New-ZenGL>)
  * [SourceForge - ZenGL 4.2 and higher source code](<https://sourceforge.net/projects/new-zengl/>)



## Tutorial

**Attention!** All examples are contained in the downloaded version of the library. At this time, this is relevant for all versions of ZenGL. These tutorials are not fully compatible with the latest version of ZenGL. 

[ZenGL Tutorial](<ZenGL_Tutorial.md> "ZenGL Tutorial"): This is a first tutorial for ZenGL: download, installation, source paths, compilation (statically or with so/dll/dylib) (Windows dll), and the First program 'Initialization' that comes with ZenGL. 

[ZenGL Tutorial 2](<ZenGL_Tutorial_2.md> "ZenGL Tutorial 2"): This is the second tutorial on how to create a font and draw text in the window. 

## Features
    
    
     * **Main**
       o can be used as so/dll/dylib or statically compiled with your application 
       o rendering to own or any other prepared window
       o logging
       o resource loading from files, memory and **zip** archives
       o multithreaded resource loading
       o easy way to add support for new resource format
     * **Configuration of**
       o antialiasing, screen resolution, refresh rate and vertical synchronization
       o aspect correction
       o title, position and size of window
       o cursor visibility in window space
     * **Input**
       o handling of keyboard, mouse and joystick input
       o handling of Unicode text input
       o possibility to restrict the input to the Latin alphabet
     * **Textures**
       o supports **tga** , **png** , **jpg** and **pvr**
       o correct work with NPOT textures
       o control the filter parameters
       o masking
       o _render targets_ for rendering into texture
     * **Text**
       o textured Unicode font
       o rendering UTF-8 text
       o rendering text with alignment and other options like size, color and count of symbols
     * **2D subsystem**
       o _batch render_ for high-speed rendering
       o rendering different primitives
       o sprite engine
       o rendering static and animated sprites and tiles
       o rendering distortion grid
       o rendering sprites with new texture coordinates (with the pixel dimension and the usual 0..1)
       o control the blend mode and color mix mode
       o control the color and alpha of vertices of sprites and primitives
       o additional sprite transformations (flipping, zooming, vertices offset)
       o fast clipping of invisible sprites
       o 2D camera with ability to zoom and rotate the scene
     * **Sound**
       o works through OpenAL or DirectSound; depends on configuration or OS
       o correct work without soundcard
       o supports **wav** and **ogg** as sound sample formats
       o playing audio files in separate thread
       o control volume and playback speed
       o moving sound sources in 3D space
     * **Video**
       o decoding video frames into texture
       o supports **theora** codec in **ogv** container
     * **Math**
       o basic set of additional math functions
       o triangulation functions
       o basic set of collision functions
     * **Additional**
       o reading and writing INI files
       o functions for working with files and memory
    

## In the updated version

**Attention!** Basic information in Russian! Thank you for understanding. 

  * Corrected compilation for android for FPC 3.2.0 and higher.
  * Moved the main code to correct the library
  * Edited work with Windows 64
  * Fixed minor bugs
  * Edited some demo versions (for FPC and iOS demos were not corrected, demo versions for Lazarus and Delphi were revised)
  * Introduced defines 
    * define USE_EXIT_ESCAPE - exit. Ability not to write additional code to exit the program by pressing the Escape key
    * USE_INIT_HANDLE definition - for using ZenGL in an already created window (LCL/VCL)
  * Introduced support for MacOS Cocoa



Documentation is maintained inside ZenGL modules in Russian and English. 

Also see the updates in the file _Update _ZenGL.txt_ (in Russian).

---

_Source: [https://wiki.freepascal.org/ZenGL](https://web.archive.org/web/20250123182508/https://wiki.freepascal.org/ZenGL)_
