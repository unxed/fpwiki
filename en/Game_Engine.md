# Game Engine

│ **English (en)** │  **[français (fr)](</Game_Engine/fr> "Game Engine/fr")** │    
****  
  
---  
[**Game Development**](<Portal_Game_Development.md> "Portal:Game Development")  
  
  * [Allegro Game Framework](<FPC_and_Allegro.md> "FPC and Allegro") \- cross-platform
  * [Castle Game Engine](<Castle_Game_Engine.md> "Castle Game Engine") \- 2D and 3D cross-platform Pascal game engine
  * [Choosing a Game Engine](<Choosing_a_Game_Engine.md> "Choosing a Game Engine")
  * [Games](<Games.md> "Games")
  * Game Engines
  * [Game Frameworks](<Game_framework.md> "Game framework")
  * [Graphics libraries](<Graphics_libraries.md> "Graphics libraries")
  * [Lazarus- Game Developers Edition](<Lazarus-_Game_Developers_Edition.md> "Lazarus- Game Developers Edition") _Proposal_
  * [nxPascal](<nxPascal.md> "nxPascal") \- lightweight 3D game engine
  * [Peg Solitaire](<Peg_Solitaire_tutorial.md> "Peg Solitaire tutorial") \- a Lazarus game tutorial
  * [Projects using Lazarus - Games](<Projects_using_Lazarus_-_Games.md> "Projects using Lazarus - Games")
  * [ZenGL](<ZenGL.md> "ZenGL") \- Pascal cross-platform game development library

  
  
A **game engine** is a software development environment designed to create [games](<Games.md> "Games"). It distinguish from [game libraries](<Game_framework.md> "Game framework") in that: 

  * Implements the _game loop_ , resource managers and other complex subsystems as networking communications, user interfaces and configuration systems.
  * Implements complex data structures, such as maps, particle systems and actors.
  * Implements tools as editors and data managers.



All these subsystems would affect in gameplay aspects as movement, scoring or even genre. You can read this [Wikipedia page](<http://en.wikipedia.org/wiki/Game_engine>) for more information. 

## Game Engines

Here's a list of game engines that are Pascal/Delphi based or have Pascal binding libraries. 

Name  | Site  | Usage  | Notes   
---|---|---|---  
[Castle Game Engine](<Castle_Game_Engine.md> "Castle Game Engine") | [castle-engine.io](<https://castle-engine.io/>) | FPC/Delphi  | 2D & 3D, all platforms supported   
Quad-Engine  | [GitHub](<https://github.com/EliiahPro/quad-engine>) | Delphi/FPC/C#/C++  | v0.9.0 (Diamond) 2018-01-15 (available under Releases/Tags on Github). Engine is written in Delphi, compiled as Win32 dll-file, other languages are supported via header files.   
TERRA Game Engine  | [GitHub](<https://github.com/Relfos/TERRA-Engine>) | Delphi/FPC/Oxygene  | 2D & 3D, all platforms supported   
[Tilengine](<http://www.tilengine.org>) | [GitHub](<https://github.com/turric4n/PascalTileEngine>) | FPC/Delphi OOP Pascal Wrapper and bindings  | Cross-platform 2D graphics engine for creating classic/retro games with tilemaps, sprites and palettes. Last update May 2020.   
[nxPascal](<nxPascal.md> "nxPascal") | [GitHub](<https://github.com/Zaflis/nxpascal>) | FPC/Delphi  | For now only OpenGL is supported. No android or other mobile support yet.   
g2mp  | [GitHub](<https://github.com/MrDan2345/g2mp>) | FPC  | Ideologically replaced Dan Jet X, multiplatform, editor- and code-based development   
Andorra 2D  | [SourceForge](<http://andorra.sourceforge.net/>) | Delphi  | Last update 2008   
CAST II Game Engine  | [www.casteng.com](<http://www.casteng.com/>) | Delphi  | Alive? last update 2011   
Delphi X  | [www.micrel.cz/Dx](<http://www.micrel.cz/Dx/>) | Delphi  |   
Afterwarp  | [www.afterwarp.net](<http://www.afterwarp.net/>) | FPC/Delphi  |   
Brtech1  | [PascalGameDevelopment.com](<http://www.pascalgamedevelopment.com/showthread.php?13795-Brtech1>) | FPC  | Not really available as an engine or library. Many videos can be found on youtube though.   
GameMaker: Studio  | [www.yoyogames.com](<http://www.yoyogames.com/>) | N/A  | Yes, it's not really a game engine library. But it's a game engine and studio written originally in Delphi. Special Pascal proud.   
MinGRo  | [SourceForge](<https://www.sf.net/p/mingro>) | FPC  | Designed for old-school style games. Still work in progress. Build with [Allegro.pas](<FPC_and_Allegro.md> "FPC and Allegro").   
ZGameEditor  | [www.zgameeditor.org](<http://www.zgameeditor.org/>) | FPC/Delphi  | use OpenGL for graphics and a real time synthesizer for audio   
SO Engine  | [GitHub](<https://github.com/dimsa/ShadowEngine>) | Delphi/FMX  | Small Crossplatform (Win, Android, iOs) indy engine with formatters, animations, intersections and etc.   
DGLE  | [GitHub](<https://github.com/DGLE-HQ/DGLE>) | Delphi/FPC/C#/C++  | Powerful free open source cross-platform game engine.   
raylib  | [GitHub](<https://github.com/tazdij/raylib-pas>) | FPC  | A simple and easy-to-use raylib library   
PGF  | [GitHub](<https://github.com/GuvaCode/PGF>) | FPC/Lazarus  | The Phoenix Game Framework is a set of classes for helping in the creation of 2D and 3D games in pascal.   
ZenGL  | source code [ZenGL before version 3.12](<https://code.google.com/archive/p/zengl/>)[ZenGL 4.2 and higher](<https://sourceforge.net/projects/new-zengl/>) | FPC/Lazarus/Delphi  | Cross-platform game development library written in Pascal.   
Apus Game Engine  | [GitHub](<https://github.com/Cooler2/ApusGameEngine>) | FPC/Delphi  | 2D (with some 3D effects) engine   
Ray4Laz  | [GitHub](<https://github.com/GuvaCode/Ray4Laz>) | FPC/Lazarus  | is a simple and easy-to-use library to enjoy videogames programming   
  
## Physics Engines

These engines simulate the physical world (collisions, trajectories etc). Not really game engines per se, but could certainly be used in games. 

Name  | Site  | Usage  | Notes   
---|---|---|---  
TundAx  | [GitHub](<https://github.com/JordiCorbilla/thundax-delphi-physics-engine>) | FPC/Delphi  |   
Newton  | [www.saschawillems.de](<http://www.saschawillems.de/>) | [Bindings](<http://www.saschawillems.de/?page_id=76>) |   
Box2D-Delphi  | [SourceForge](<https://sourceforge.net/projects/box2d-delphi/>) | FPC/Delphi  | This is Delphi implementation of [Box2D](<https://github.com/erincatto/Box2D>) library.   
Kraft  | [GitHub](<https://github.com/BeRo1985/kraft>) | FPC/Delphi  | Pascal native physics engine by Benjamin Rosseaux.

---

_Source: [https://wiki.freepascal.org/Game_Engine](https://web.archive.org/web/20240915022804/https://wiki.freepascal.org/Game_Engine)_
