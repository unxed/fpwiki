# GLScene

│ **[Deutsch (de)](</GLScene/de> "GLScene/de")** │  **English (en)** │  **[français (fr)](</GLScene/fr> "GLScene/fr")** │  **[Bahasa Indonesia (id)](</GLScene/id> "GLScene/id")** │  **[日本語 (ja)](</GLScene/ja> "GLScene/ja")** │  **[한국어 (ko)](</GLScene/ko> "GLScene/ko")** │  **[português (pt)](</GLScene/pt> "GLScene/pt")** │  **[русский (ru)](<../ru/GLScene.md> "GLScene/ru")** │  **[中文（中国大陆） (zh_CN)](</GLScene/zh_CN> "GLScene/zh CN")** │    
****

## Contents

  * 1 About
  * 2 Screenshots
  * 3 Author
  * 4 Download
    * 4.1 Forked
    * 4.2 Latest (v1.0.0.2)
    * 4.3 Outdated
  * 5 SVN
  * 6 Bug reporting
  * 7 Change Log
  * 8 Installation version 1.0.0.2
  * 9 Installation version 0.9.6.1
  * 10 Help



### About

GLScene is an OpenGL based 3D library. It was initially written for Delphi, now it is running in C++ Builder, Kylix, Delphi and Lazarus. 

Its main caracteristics are : 

Scene description 

  * hierarchical objects structure, with as many objects as memory allows, easily extendable
  * interactive scene management (numerous Delphi experts)
  * easy to use rotation and movement functions for each object
  * predefined objects (all standards from cube to teapot to sprite), easily extendable
  * structural objects (dummy cube, proxy object) for manageing composite objects
  * procedural objects (heightfield, revolution solids, pipes, tesselators...)
  * support for 2D and 1D objects (sprites, lines with spline support) fully integrated
  * HUD objects (sprites rendered in 2D coordinates)
  * Utility objects (grids, skydome, arrows...)
  * support classes for directly accessing OpenGL
  * camera and light objects that can be used anywhere in a scene objects hierarchy
  * behaviour objects can be attached to objects for animation purposes, easily extendable
  * per-object special effects (before/after object rendering and scene after-effects), easily *extendable
  * support for particle systems, any object or object structure can be a particle
  * 3D Studio file import with automatic and precise normal vector calculation (using the given *smoothing groups of the file) and texture coordinate import
  * Other imported formats : OBJ/OBJF, SMD, MD2, STL, TIN, PLY



Materials 

  * easy to use, optimized, material object
  * material library to share and reuse materials
  * ambient, diffuse, emission, specular and shininess support
  * support of blending modes (transparency, additive...)
  * alpha-channel auto-generation
  * support for OpenGL texture formats, including compressed ones (DXT, S3TC etc.)
  * generic plug-in shaders support
  * High-level Cg shaders support classes
  * polymorphic image support for texturing (allows many formats as well as procedural textures), easily extendable
  * more than 150 predefined colors like clrCornflowerBlue or clrCoolCopper (easily extendable) in addition to standard colors and direct RGBA specifications
  * easily to use texture movement and scaling properties, independently from texture coordinates
  * 32 bits Bitmap support class



Rendering 

  * automatically uses the hardware OpenGL driver if available
  * a well working camera model using focal length and targeting
  * multiple viewers for one or more scenes, easy change of view through camera selection
  * fog and depth-of-view support
  * render to a file, a bitmap or to the printer in any color and pixel resolution
  * full screen support with dynamic resolution changes
  * reflections and mirror effects
  * dynamic shadow casting
  * high-performance scene-wide transparent particles system support
  * automatic frustrum culling (hierarchical or per-object)



Animation 

  * propagation of time progression events
  * skeletal animation (multiple bones per vertex)
  * frame interpolation and morph targets
  * easily extendable Behaviours
  * dynamic physics : inertia, acceleration, damping and force application
  * automatic, realtime scene cadenceing



Interface 

  * easy-to-use functions to determine picked objects
  * helper functions for camera movements
  * helper functions for picked object's translation
  * helper functions for converting between screen and world coordinates, raycasting



Utilities 

  * optimized geometry functions and utilities (vector, quaternion and matrix...)
  * cubic spline support functions and class
  * mesh manipulation and optimization (stripification, vertex-cache awareness, etc.)
  * component to make full-featured screen-saver in a few clicks
  * precise frame speed determination
  * asynchronous timer (multi-threaded)
  * joystick support
  * asynchronous keyboard access, keymap support



### Screenshots

[![Gl effect.jpg](https://wiki.freepascal.org/images/5/5c/Gl_effect.jpg)](</File:Gl_effect.jpg>) [![Gl cel.jpg](https://wiki.freepascal.org/images/f/f5/Gl_cel.jpg)](</File:Gl_cel.jpg>) [![Gl trinity.jpg](https://wiki.freepascal.org/images/5/50/Gl_trinity.jpg)](</File:Gl_trinity.jpg>)  
[![Gl ode.jpg](https://wiki.freepascal.org/images/4/49/Gl_ode.jpg)](</File:Gl_ode.jpg>)

### Author

Author: initially Mike Lischke  
at time maintained by Eric Grange  
LCL Port: [Christian Ulrich](</User:Christian> "User:Christian")

### Download

#### Forked

A much newer version of GLScene exists [here](<https://sourceforge.net/p/glscene/code/HEAD/tree/branches/GLSceneLCL/>) (as of 2018-03-15 it's v1.6). 

#### Latest (v1.0.0.2)

  * A new snapshot release can be found at [Lazarus CCR Files page](<https://sourceforge.net/projects/lazarus-ccr/files/GLScene/>).



This should be almost identical to the original cvs repository of today in terms of functionality and bugfixes, however, there are still some parts missing. There wont be a "Lazarus-with-GLScene-Installer" with this new release, unless someone else creates it. \--[Crossbuilder](</User:Crossbuilder> "User:Crossbuilder") 20:16, 18 July 2007 (CEST) 

#### Outdated

An older release (version 0.9.1.6) can be found on the [Lazarus CCR Files page](<https://sourceforge.net/project/showfiles.php?group_id=92177&package_id=98986&release_id=364007>) together with a lazarus installer with GLScene already in the Lazarus IDE and GLScene demoes. UPDATE: This is pretty old now. If possible, use the svn checkout (see below). 

### SVN

you can checkout the actual source from  
<https://svn.freepascal.org/svn/fpcprojects/glscene/trunk/>  


The command would be:   

    
    
    svn co https://svn.freepascal.org/svn/fpcprojects/glscene/trunk glscene

This will download the trunk (main) repository to "glscene" dir to whereever you invoked the command from. 

In windows you can use clients like tortoise SVN and others. 

### Bug reporting

[Bug Reporting/Featurerequest](<https://bugs.freepascal.org/>) Please select Project "Lazarus-ccr" and Category "glscene" for your reports. Thanks. 

### Change Log

  * 18.07.2007 Snapshot release version 1.0.0.2 
    * adjusted some things, so it works with latest lazarus svn version
    * added object bitmaps for the SceneEditor
    * fixed another AV in SceneEditor
    * incorporated all patches and bugfixes from original glscene cvs repo like dynamic textures ..
  * 13.06.2007 Snapshot release version 1.0.0.1 
    * incorporated all patches and bugfixes from original glscene cvs repo
    * improved scene editor (crashes less, but still needs some love)  
**Tip:** Save your project frequently ;-)
    * seperated mmsystem dependent stuff from generic sound support so e.g. FMOD is available in linux, too
    * many more ..
  * 01.12.06 Added support for ODE, gtk2 and Cg shaders in Linux (ODE is limited to normal release, unsupported are cone, ccylinder and terrains)
  * 10.08.05 Too many changes to post here see cvs history
  * 16.07.05 GLScene 0.9.X Port
  * look at GLScene Sourceforge Account for complete Changelog



Status: Beta 

### Installation version 1.0.0.2

**Requires fpc 2.1.4 or greater and lazarus 0.9.23 rev. 11348 or greater !!**

  * **extracting the sources**
    * ZIP: 
      * in "lazarus/components/" folder execute "unzip glscene-1.0.0.2.zip"   
This will create the lazarus/components/glscene/ folder
    * SVN: 
      * in "lazarus/components/" folder execute 
            
            svn co https://svn.freepascal.org/svn/fpcprojects/glscene/trunk glscene

This will create the lazarus/componets/glscene/ folder
      * Tortoise SVN: add the folder "glscene" manually so you have lazarus/components/glscene. Then right-click onto new folder and choose "SVN Checkout...". At field "URL of repository" add 
            
            https://svn.freepascal.org/svn/fpcprojects/glscene/trunk

Confirm by "OK".



  


  * **Finish the installation:**
    * copy "lazarus/components/glscene/globallinks/*.lpl" to "lazarus/packager/globallinks/"
    * start lazarus
    * select "Configure installed packages ..." from the "Components" menu
    * select "glscenelazarus" from the "Available packages" list
    * click "Install selection" button
    * click "Save and rebuild IDE" button 
      * ATTENTION: There may occur an error that package "jpegforlazarus" is not found. If so do the following:
      * Components --> Open loaded package...
      * Choose "glscenelazarus" (Version 1.0.0.3) and click "Open..."
      * In the following list go down to "Required Packages" and remove dependency from "jpegfromlazarus".
      * Look for unit "jpeg" in folder /lazarus/components/glscene/Source/base and replace "LazJPEG" (in uses clause) by "LazJPG" (from package ImagesForLazarus).
      * Save and repeat last 4 steps again.
      * HINT: You should have installed package "ImagesForLazarus" (the same way as described for "glscenelazarus" package).
    * repeat the last 4 steps for the other packages you want to use. 
      * Note: glsceneode and glscenecgshader need their respective libs on the system during installation!
    * **Note:** you can also try to select and install all packages in one go. However, I prefer to do things step by step, so I see where it fails, if it fails ;)
    * **Install "glscenelazarus" - mistakes:** (only needed when using glscene-1.0.0.2.zip, in svn these are already fixed) 
      * To correct the unit GLLCLViewer: procedure TGLSceneViewer.CreateParams(var Params: TCreateParams); - at procedure TGLSceneViewerLCL.CreateParams(var Params: TCreateParams);
      * To correct the unit GLLCLViewer: procedure TGLSceneViewer.EraseBackground(DC: HDC); - at procedure TGLSceneViewerLCL.EraseBackground(DC: HDC);
      * To correct the unit GLLCLFullscreenViewer: Change "RegisterClasses([TGLSceneViewer, TGLFullScreenViewer]);" to "RegisterClasses([TGLFullScreenViewer]);"


  * **List of packages:**
    * **glscenelazarus** The main package.
    * **glscenecgshader** Cg Shader components
    * **glsceneode** ODE components
    * **glss_fmodlaz** FMOD component
    * **gls_winonly** Windows only, contains sound support over mmsystem, BASS support and GLScreenSaver component


  * **Сompilation "demo" - mistakes:**
    * If not found class - TGLSceneViewer: To correct the uses (demo): GLLCLViewer - at GLViewer



### Installation version 0.9.6.1

  * Create the directory lazarus\components\GLScene
  * On this directory, unzip the files from the zip file
  * Open lazarus
  * Open the package glscenelazarus.lpk with Component/Open package file (.lpk) from lazarus\components\glscene\Lazarus
  * (Click on Compile is only necessary, if you don't want to install the component into the IDE)
  * Click on Install
  * Repeat these steps (open .lpk, click install) for the other packages you want to use. Note: these additional packages need additional libs (fmod, ode, Cg). 
    * glscenecgshader.lpk : Cg shaders (only with NVidia cards?)
    * glsceneode.lpk : support for ode lib
    * glsss_fmodlaz.lpk : support for fmod lib
    * gls_winonly.lpk : this can be installed on windows only. Contains screensaver stuff as well as support for BASS lib.



Hint: sometimes an Error "GLGraphics not found" occurs. Don't worry, just do Tools->Build Lazarus again and it should be ok. 

A full install of Lazarus with GLScene already in the IDE is available as [binary win32 release](<https://sourceforge.net/project/showfiles.php?group_id=92177&package_id=185641&release_id=405739>). 

Just a warning for (new) Linux users: GLScene in its current state is not working "out of the box" (lots of modifications/additions needed). UPDATE: this is not true anymore. glscene for lazarus is pretty up-to-date and most things work good right out of the box for windows as well as for linux. 

As of latest trunk version (see svn above) the linux version is working well. Some demos (about 20%) are still broken but the majority of glscene is now working. We're also moving towards making it work in other Unix platforms like FreeBSD. 

The missing/nonworking parts on non-windows are: glKeyboard (uses winapi, but is not required mostly), glSound(which uses mm, we need to use openal or somesuch) 

A [fully featured windows installer](<Lazarus-_Game_Developers_Edition.md> "Lazarus- Game Developers Edition") is planned. 

### Help

  * Can be found at [Main GLScene Page](<http://glscene.sourceforge.net/wikka/>)
  * join #glscene channel at freenode.net

---

_Source: [https://wiki.freepascal.org/GLScene](https://web.archive.org/web/20250219124734/https://wiki.freepascal.org/GLScene)_
