# Choosing a Game Engine

## Contents

  * 1 ZGameEditor
  * 2 Castle Game Engine
  * 3 GLScene
  * 4 Allegro.pas
  * 5 Simple Directmedia Library
  * 6 nxPascal
  * 7 BGRABitmap or TCanvas
  * 8 LAMW + jCanvasES2
  * 9 Acknowledgement



## ZGameEditor

**Website:** <http://www.zgameeditor.org/>

This game builder is created using Delphi and its game scripting language is very similar to Pascal (a simplified version of Pascal). Pascal lovers should check this one first. I managed to create a simple vertical scrolling spaceship game in the first few hours of my trying it. 

Advantages  | Disadvantages   
---|---  
Multi platform support: Windows, Linux, macOS, Android  | The editor runs on Windows only   
Can create both 2D and 3D games  | Requires some programming skills   
Can create Windows screensavers   
Easy to use built-in audio synthesizer   
The generated stand-alone executable is very small   
Its scripting language is very similar to Pascal   
  
## Castle Game Engine

**Website:** <https://castle-engine.io/>

A powerful Pascal game engine for FPC/Lazarus and Delphi. 

  * For 3D and 2D games.


  * Supports a lot of model formats, like glTF, X3D, Spine, sprite sheets etc.


  * The engine has a [visual editor for games](<https://castle-engine.io/manual_editor.php>) and command-line build tool.


  * Editor allows to start making games from easy templates (click "New Project" in the editor and you will have a working 3D or 2D game, that instantly builds for all platforms).


  * A lot of [examples](<https://github.com/castle-engine/castle-engine/tree/master/examples>), [documentation](<https://castle-engine.io/manual_intro.php>), [videos](<https://www.youtube.com/c/CastleGameEngineX3d>).

Advantages  | Disadvantages   
---|---  
Multi platform support: Windows, Linux, FreeBSD, macOS, Android, iOS, Nintendo Switch  | Requires good programming skills in Pascal   
Import models form many formats, graphics effects, physics, AI, etc   
Can create both 2D and 3D games   
Good demos and documentation   
  
## GLScene

**Website:** <http://glscene.sourceforge.net/wikka/>

This 3D graphics library is very powerful. I thought I'd build some cool 3D games and applications using it. Unfortunately their development focus is mainly on Windows. I monitored it for years but there was not much progress for Linux. It didn't run well on my Linux computer so I abandoned it. 

Advantages  | Disadvantages   
---|---  
Multi platform support: Windows, Linux, macOS  | Focus is mainly on Windows   
Full featured 3D graphics engine  | Does not run well on Linux   
Good demos  | Where is the documentation?   
Zero deployment setup  | Requires good programming skills in Pascal   
  
## Allegro.pas

**Website:** [FPC and Allegro](<FPC_and_Allegro.md> "FPC and Allegro")

Allegro.pas is a wrapper for the Allegro game framework. Learning a game framework is hard, luckily the official website of Allegro provides detailed documentation so learning it is relatively easy. After only a few days learning it, I managed to create my demo game Furious Paladin. 

Furious Paladin's binary: <http://graphicslearning.com/furious-paladin-tutorials/>

And this is the source code: <https://sourceforge.net/p/allegro-pas/code/HEAD/tree/TRUNK/demos/>

Advantages  | Disadvantages   
---|---  
Multi platform support: Windows, Linux, macOS, iPhone, Android  | Relies on OpenGL or Direct3D to create full 3D games   
Audio, font, video playback, some 3D basic drawing, joystick, keyboard, physics  | How do you use Allegro.pas to create Android games?   
Detailed documentation  | Requires good programming skills in Pascal   
  
## Simple Directmedia Library

**Website:** [FPC and SDL](<FPC_and_SDL.md> "FPC and SDL")

I never really tried it. But I know a kid (who was a member here) created a simple 2D horizontal scrolling game using FPC + SDL only weeks of learning it. Read more about [SDL here](<FPC_and_SDL.md> "FPC and SDL")

## nxPascal

**Website:** [nxPascal](<nxPascal.md> "nxPascal")

Not as famous as the others and it lacks of documentation. But it has potential. Read more in the Forums: <https://forum.lazarus.freepascal.org/index.php/topic,35313.msg253463.html#msg253463>

## BGRABitmap or TCanvas

If you want to create simple 2D games like Tetris, Snake, Pong, Arkanoid, TCanvas and BGRABitmap is totally capable of doing it. I wrote a 2D scrolling spaceship game using only TCanvas line drawing and it's working well. I then tried to switch the graphics library to use BGRABitmap, it works. Any TCanvas program can easily switch to using BGRABitmap, which offers more graphics features. Read more: 

  * <https://lazarus-ccr.sourceforge.io/docs/lcl/graphics/tcanvas.html>
  * [TCanvas](<TCanvas.md> "TCanvas")
  * [BGRABitmap](<BGRABitmap.md> "BGRABitmap")
  * [BGRABitmap_tutorial](<BGRABitmap_tutorial.md> "BGRABitmap tutorial")

Advantages  | Disadvantages   
---|---  
Multi platform support: Windows, Linux. macOS   
Relatively simple and easy to learn than the others  | They handle graphics only   
BGRABitmap can draw simple 3D objects  | Not suitable for graphics intensive games   
Zero deployment setup  | Requires good programming skills in Pascal   
  
## LAMW + jCanvasES2

I am now exploring the possibility of creating Android games using LAMW + jCanvasES2. I already managed to create my Windows/Linux 2D graphics engine using OpenGL. Maybe I'm too ambitious, I want it to also run on Android. LAMW + jCanvasES2 is the combination to make Lazarus produce Android binaries that allow us to use OpenGL ES2. Unfortunately, it crashes too often. I'm examining it, hope I can fix the crashes. 

## Acknowledgement

This content was extracted from [Handoko's](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=39078>) post in the [Games sub-forum](<https://forum.lazarus.freepascal.org/index.php/topic,44855.msg315698.html#msg315698>). Some minor grammar edits have been made and formatting added. 

Note: _Castle Game Engine_ section have been edited by [Michalis](</User:Michalis> "User:Michalis") ([talk](</index.php?title=User_talk:Michalis&action=edit&redlink=1> "User talk:Michalis \(page does not exist\)")), because the original content was outdated IMHO. It was praising the engine but also saying CGE is only a _game framework_. Since the original post was written, CGE got visual editor, new templates that build out-of-the-box for all platforms, lots of videos and introductory documentation... I feel CGE is now a great game engine, suitable also for beginners, too. You're welcome to challenge me on this, just try CGE :)

---

_Source: [https://wiki.freepascal.org/Choosing_a_Game_Engine](https://web.archive.org/web/20241226085302/https://wiki.freepascal.org/Choosing_a_Game_Engine)_
