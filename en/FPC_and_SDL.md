# FPC and SDL

│ **English (en)** │

**Simple DirectMedia Layer** or **SDL** is a cross-platform development library designed to provide low-level access to audio, keyboard, mouse, joystick, and graphics hardware via OpenGL and Direct3D. It is used by video playback software, emulators and popular games. 

## Contents

  * 1 Introduction
  * 2 Obtaining FPC headers for SDL2
    * 2.1 SDL2-for-Pascal
    * 2.2 SDL headers from Free Pascal SVN (deprecated)
  * 3 Tips and tricks
    * 3.1 SDL_mixer library
    * 3.2 SMPEG Library
  * 4 macOS specific issues
    * 4.1 SDL Framework and SDL libraries
    * 4.2 The framework version
    * 4.3 The Unix library version
  * 5 For FPC 2.2.2 and later
  * 6 For FPC 2.0.2–2.2.0
  * 7 Examples
  * 8 Ubuntu
  * 9 SDL2 Units
    * 9.1 Tutorials



## Introduction

For general information and documentation about SDL, visit the [Official SDL Website](<https://www.libsdl.org/index.php>). This page is going to explain how to use SDL with Free Pascal and where to obtain the Pascal headers, etc. More information is available at the [LibSDL Wiki](<https://wiki.libsdl.org/SDL2/FrontPage>). 

## Obtaining FPC headers for SDL2

After installing the SDL library itself on your system, you'll need the Pascal headers for it to be able to use SDL with FPC. Since version 2.2.2, most SDL units are included with FPC. You can, of course, also download them from SDL2-for-Pascal, in case you need some bug fix or a unit that's not in the version shipped with FPC. 

### SDL2-for-Pascal

**See:** [The SDL2-for-Pascal GitHub Repository](<https://github.com/PascalGameDevelopment/SDL2-for-Pascal>). 

You can download the latest release from the [Releases page](<https://github.com/PascalGameDevelopment/SDL2-for-Pascal/releases>). 

You can also download and browse the current source code using Git, see [GitHub's "About Remote Repositories" page](<https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories>) for instructions. In short, just do: 
    
    
    git clone https://github.com/PascalGameDevelopment/SDL2-for-Pascal.git
    

After you download the SDL2-for-Pascal sources, you want to be able to use these units in your programs. For the simplest scenario, just add the following lines to your `fpc.cfg` file: 
    
    
    -Fi<jedi-sdl-path>/units/sdl2.pas
    -Fu<jedi-sdl-path>/units/sdl2.pas
    -Fu<jedi-sdl-path>/units/sdl2_gfx.pas
    -Fu<jedi-sdl-path>/units/sdl2_image.pas
    -Fu<jedi-sdl-path>/units/sdl2_mixer.pas
    -Fu<jedi-sdl-path>/units/sdl2_net.pas
    -Fu<jedi-sdl-path>/units/sdl2_ttf.pas
    

…where <jedi-sdl-path> is the path where you placed the SDL2-for-Pascal sources. 

Alternatively, you can copy all `*.pas` and `*.inc` files from these directories to one chosen directory, and add only this chosen directory to your `fpc.cfg`. 

### SDL headers from Free Pascal SVN (deprecated)

The code can be downloaded with: 
    
    
    svn co http://svn.freepascal.org/svn/fpcprojects/contrib/sdl/ fpc-sdl
    

## Tips and tricks

### SDL_mixer library

When using SDL_mixer, the call to Mix_OpenAudio MUST happen before the call to SDL_SetVideoMode, otherwise you will get a (sometimes loud) audible pop. 

### SMPEG Library

If you use sdl2_mixer, you'll notice that it's dependent on the [SMPEG Library](<https://github.com/icculus/smpeg>). For modern Linux distributions this is not a problem, because smpeg is distributed with many of them, but it can be more troublesome for FreeBSD and Windows: 

  * You can install smpeg for FreeBSD from ports: `/usr/ports/multimedia/smpeg`.
  * You can get smpeg.dll for Windows from [SMPEG for Visual C sources](<https://github.com/icculus/smpeg/blob/main/VisualC.zip>).



Alternatively, you can simply modify the source of SDL_mixer.pas to remove smpeg from it's uses clause and comment out one line in the sources mentioning mp3s (it's a case near the end). Remember that you will also need to have an `sdl2_mixer.dll`/`sdl2_mixer.so` that is _not_ linked to smpeg—otherwise the sdl2_mixer library will depend on smpeg anyway. 

## macOS specific issues

### SDL Framework and SDL libraries

On macOS, SDL2 uses Cocoa to setup the drawing window and to handle all events (such as keypresses). This is done using a small piece of code that is automatically called when the program starts up, and which subsequently calls a function with the name _SDLmain_. 

There are however two different ways to package/distribute SDL for macOS, and depending on which version you use, the procedure to get this startup code into your program is slightly different. 

### The framework version

This the version that you get when you download the macOS version of SDL from [libsdl.org](<https://libsdl.org/>). In this case, the startup code is only available in source form. It is located on the installation disk image, in the folder `devel-lite`. After copying the `SDL.framework` from the disk image to `/Library/Frameworks` and copying the `devel-lite` folder anywhere on your hard drive (e.g., in your home directory), you can compile it into libSDLMain.a by opening a Terminal session, navigating to the copied `devel-lite` folder, and executing the following commands: 
    
    
    gcc -c -o SDLMain.o SDLMain.m -I/Library/Frameworks/SDL.framework/Headers
    ar r libSDLMain.a SDLMain.o
    ranlib libSDLMain.a
    

You can then move the generated `libSDLMain.a` library anywhere you want. To make sure the linker can find it when you compile SDL applications, pass the `-Fl/full/path/to/folder/containing/libSDLMain.a` command line parameter to the compiler. 

### The Unix library version

There is also a "classic Unix" version of SDL for macOS (no framework, just a bunch of separate libraries), which you get if you compile SDL from sources, or if you install SDL using Fink or MacPorts. In this case, `libSDLmain.a` is already included in a compiled form. You just have to use the compiler's `-Fl` parameter, as explained in the previous section, to make sure that the linker can find this library. 

## For FPC 2.2.2 and later

The SDL unit included with FPC includes all necessary framework, library and main procedure name renaming statements, so you no longer have to do anything special. Just add the sdl unit to your program and everything will be taken care of. However, the remarks about libSDLMain in the above section still hold (libSDLMain.a is required by the SDL unit included with FPC). 

Note that if you use the original units as distributed by the JEDI team, you may still need to follow the instructions in the section below. 

## For FPC 2.0.2–2.2.0

With FPC v2.0.2 to v2.2.0, and possibly also when using the original SDL units as distributed by the JEDI team, you have to use the `-XMSDL_main` command line option and tell the linker to include some frameworks: 

  1. Install sdl, for example using fink.
  2. Add the following to your program: 
     * {$linklib gcc}
           {$linklib SDLmain}
           uses sdl;
           

  3. Then, this command line does it: 
     * fpc -XMSDL_main -k-L/sw/lib -k-lSDL -k-framework -kOpenGL -k-framework -kCocoa -Fu<pathtosdl.pas files> -Fi<pathtosdl.inc files> YourProgram.pas
           




If you installed the SDL libraries using fink, also add `-k-L/sw/lib -k-lSDL` to your fpc command. 

If `sdl.pas` and `jedi-sdl.inc` are in the directory of your program, you can omit `-Fu<pathtosdl.pas files> -Fi<pathtosdl.inc files>`. 

## Examples

Creating an empty SDL window: 
    
    
    program sdltest;
    {$linklib gcc}
    {$linklib SDLmain}
    uses sdl;
    var 
      scr: PSDL_Surface; // Our main screen
    begin
      SDL_Init(SDL_INIT_VIDEO); // Initialize the video SDL subsystem
      scr := SDL_SetVideoMode(640, 480, 8, SDL_SWSURFACE); // Create a software window of 640x480x8 and assign to scr
      SDL_Quit; // close the subsystems and SDL
    end.
    

You can then compile this code with: `fpc -Fu<pathtosdl.pas files> -Fi<pathtosdl.inc files> example.pas`

If you use Lazarus, make sure you fill in the "other unit files" and "include files" properly. 

## Ubuntu

With Ubuntu, you can install the following, which saves you having to manually install the SDK packages… `sudo apt install libsdl1.2-dev` …and possibly also… `sudo apt install libsdl2-dev`

FPC knows SDL from home. 

## SDL2 Units

### Tutorials

See the [Free Pascal meets SDL](<https://www.freepascal-meets-sdl.net/sdl-tutorials/>) website for tutorials on using Free Pascal and SDL.

---

_Source: [https://wiki.freepascal.org/FPC_and_SDL](https://web.archive.org/web/20240308085014/https://wiki.freepascal.org/FPC_and_SDL)_
