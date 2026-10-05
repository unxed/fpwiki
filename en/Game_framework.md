# Game framework

---  
[**Game Development**](<Portal_Game_Development.md> "Portal:Game Development")  
  
  * [Allegro Game Framework](<FPC_and_Allegro.md> "FPC and Allegro") \- cross-platform
  * [Castle Game Engine](<Castle_Game_Engine.md> "Castle Game Engine") \- 2D and 3D cross-platform Pascal game engine
  * [Choosing a Game Engine](<Choosing_a_Game_Engine.md> "Choosing a Game Engine")
  * [Games](<Games.md> "Games")
  * [Game Engines](<Game_Engine.md> "Game Engine")
  * Game Frameworks
  * [Graphics libraries](<Graphics_libraries.md> "Graphics libraries")
  * [Lazarus- Game Developers Edition](<Lazarus-_Game_Developers_Edition.md> "Lazarus- Game Developers Edition") _Proposal_
  * [nxPascal](<nxPascal.md> "nxPascal") \- lightweight 3D game engine
  * [Peg Solitaire](<Peg_Solitaire_tutorial.md> "Peg Solitaire tutorial") \- a Lazarus game tutorial
  * [Projects using Lazarus - Games](<Projects_using_Lazarus_-_Games.md> "Projects using Lazarus - Games")
  * [ZenGL](<ZenGL.md> "ZenGL") \- Pascal cross-platform game development library

  
  
A **game framework** is a library designed to help game development. It usually defines [APIs](<https://en.wikipedia.org/wiki/Application_programming_interface>) to deal with [graphics](<Graphics_libraries.md> "Graphics libraries"), sound, user input, data files, etc. In most cases it defines a high-level API that allows cross-platform development. 

Main difference between game frameworks and [game engines](<Game_Engine.md> "Game Engine") is that the latter also implements the game loop, as well as complex data structures (maps, actors...), data containers and tools. 

## Contents

  * 1 Game frameworks
  * 2 Other libraries for games
  * 3 Using LCL for game development
  * 4 See also



## Game frameworks

Name  | Site  | Usage  | License  | Notes   
---|---|---|---|---  
[Allegro](<FPC_and_Allegro.md> "FPC and Allegro") | [liballeg.org](<http://liballeg.org/>) | [Bindings Allegro-pas](<http://allegro-pas.sourceforge.net/>) | Allegro 1-4: Beerware, Allegro 5: zlib  | Allegro is a cross-platform library mainly aimed at video game and multimedia programming. It handles common, low-level tasks such as creating windows, accepting user input, loading data, drawing images, playing sounds, etc. and generally abstracting away the underlying platform.   
Phoenix  | [GitHub](<https://github.com/GuvaCode/PGF>) | FPC/Delphi  | Mozilla Public License  | A set of Delphi 7+ and Free Pascal components and classes to help in creating hardware accelerated 2D and 3D games.   
[GLScene](<GLScene.md> "GLScene") | [SourceForge](<http://glscene.sourceforge.net/>) | FPC/Delphi  | Mozilla Public License  | GLScene is a free OpenGL-based library for the Delphi programming language, C++ and Free Pascal. It provides visual components and objects allowing description and rendering of 3D scenes.   
Raylib  | [www.raylib.com](<https://www.raylib.com/>) | FPC/Lazarus [Raylib 4.0 Pascal Bindings](<https://github.com/sysrpl/Raylib.4.0.Pascal>) | Zlib  | Raylib is a popular game development toolkit in the computer programming education space. It provides windowing, input, graphics, text, audio, collision detection, and a basic GUI. The official website contains many examples and an easy to learn and use documentation cheatsheet.   
SFML  | [www.sfml-dev.org](<https://www.sfml-dev.org/>) | FPC/Lazarus [SFML headers](<https://github.com/DJMaster/csfml-fpc>) [PasSFML(OOP)](<https://github.com/CWBudde/PasSFML>) |  | SFML is a simple, fast, cross-platform and object-oriented multimedia API. It provides access to windowing, graphics, audio and network.   
SDL  | [www.libsdl.org](<http://www.libsdl.org>) | [SDL2 for Pascal](<https://github.com/PascalGameDevelopment/SDL2-for-Pascal>) [SDL2 bindings for FPC](<http://sourceforge.net/projects/sdl2fpc/?source=directory>) [Laz2SDL](<https://sourceforge.net/projects/lazsdl2/>) [SDL2 Headers](<https://github.com/ev1313/Pascal-SDL-2-Headers>) | SDL2: [zlib](<http://libsdl.org/license.php>) | Simple DirectMedia Layer is a cross-platform development library designed to provide low level access to audio, keyboard, mouse, joystick, and graphics hardware via OpenGL and Direct3D.   
[ZenGL](<ZenGL.md> "ZenGL") | [SourceForge](<https://sourceforge.net/projects/new-zengl/>) | FPC/Delphi  | [Zlib](<http://www.zengl.org/license.html>) | Cross-platform game development library written in Pascal, designed to provide necessary functionality for rendering 2D-graphics, handling input, sound output, etc.   
Bare Game  | [GitHub](<https://github.com/sysrpl/Bare.Game>) | FPC  |  | An open source modern minimal game cross platform gaming library using SDL. Archive of official web page from 2017 [www.baregame.org](<https://web.archive.org/web/20171005052020/http://www.baregame.org/>).   
  
## Other libraries for games

Libraries that aren't to create games but are useful. 

Name  | Site  | Usage  | License  | Notes   
---|---|---|---|---  
[Steam Wrapper](</index.php?title=Steam_Wrapper&action=edit&redlink=1> "Steam Wrapper \(page does not exist\)") | <https://github.com/thecocce/steamwrapper> | FPC/Delphi  |  | Cross-platform game development wrapper written in Pascal, designed to provide necessary functionality for rendering 2D-graphics, handling input, sound output, etc.   
  
## Using LCL for game development

1) From forum member **furious programming**. 

I suggest not to waste your time on LCL to build games, because it is not suitable for creating games for many reasons, including the most important: 

  * no Direct3D and OpenGL support
  * unable to use GPU for rendering textures
  * no exclusive video mode support and available screen resolutions
  * absolutely no support for sound mixer and playing multiple sounds at the same time
  * lack of support for game controllers and their full functionality
  * lack o V-Sync support



What I mentioned above are the basics, without which creating even a simple game will be inconvenient, the game itself will be very limited, and its performance will be very low (software rendering is horribly slow in compare to GPU rendering). As you can see, it doesn't make much sense. 

Whether you want to make a tiny game or a bigger one, use the library dedicated to their creation. It does not have to be SDL, but it is important that it has all the functionality to create full-fledged video games. However, if you are interested in creating a 2D game, I strongly advise you not to use 3D engines for this purpose, because it will significantly complicate the implementation, and you will get discouraged very quickly. 

Use something small, light, functional and, above all, supported at all times. I suggest SDL because it meets all the requirements and is great, but it can also be Allegro, because it's the same shelf. 

2) From forum member **Seenkao**. 

There are quite a few solutions for using both LCL and OpenGL/DirectX at the same time. They can be seen in neighboring topics and on the Free Pascal wiki. In the German forum, almost all OpenGL tutorials are made under LCL. dglOpenGL.pas - allows you to work with LCL on Windows without much knowledge of creating an OpenGL window/context. 

  * [OpenGL Tutorial](<OpenGL_Tutorial.md> "OpenGL Tutorial")
  * [dglOpenGL tutorial, Deutsch](<https://wiki.delphigl.com/index.php/Lazarus_-_OpenGL_3.3_Tutorial>)
  * [Metal, OpenGL demos](<https://forum.lazarus.freepascal.org/index.php/topic,42306.0.html>)
  * [GLScene](<GLScene.md> "GLScene")
  * [GLEngine2D](<https://github.com/Dev-Demi/GLEngine2D>), based on dglOpenGL. Deprecated. But it works.
  * [Game Engine](<Game_Engine.md> "Game Engine")



I don't even think I can fully cover this topic. And there's a good chance I'll miss something. LCL can also be used with SDL, Castle Game Engine, PGF, Apus Game Engine, ZenGL and many other engines. Connecting controllers does not depend at all on what we use. To do this, you just need to use the necessary libraries. Often that already come with game engines. And it can also be used in LCL. With sound, no one forbids the use of single-channel / multi-channel sound. All this is provided in the respective libraries. And it can also be used in LCL. 

The only reason this is not a suitable option is if you have a highly loaded application. Here I partly agree! In simple games, this has almost no effect. 

## See also

  * [Graphics libraries](<Graphics_libraries.md> "Graphics libraries")
  * [Game engine](<Game_Engine.md> "Game Engine")

---

_Source: [https://wiki.freepascal.org/Game_framework](https://web.archive.org/web/20250424230611/https://wiki.freepascal.org/Game_framework)_
