# OpenGL

│ **[Deutsch (de)](</OpenGL/de> "OpenGL/de")** │  **English (en)** │  **[français (fr)](</OpenGL/fr> "OpenGL/fr")** │  **[日本語 (ja)](</OpenGL/ja> "OpenGL/ja")** │  **[português (pt)](</OpenGL/pt> "OpenGL/pt")** │  **[русский (ru)](<../ru/OpenGL.md> "OpenGL/ru")** │  **[Tiếng Việt (vi)](</OpenGL/vi> "OpenGL/vi")** │  **[中文（中国大陆）‎ (zh_CN)](</OpenGL/zh_CN> "OpenGL/zh CN")** │    
****

OpenGL (**Open** **G** raphics **L** ibrary) is a cross-platform API for producing 3D computer graphics. Most modern video cards provide hardware accelerated OpenGL support, wich makes OpenGL a good solution for writing advanced graphics software. 

## Contents

  * 1 OpenGL units in Free Pascal
  * 2 OpenGL units in Lazarus
  * 3 Third party OpenGL units
  * 4 Tutorials
  * 5 Tutorial OpenGL 3.3 ( modern OpenGL )
  * 6 Q/A using OpenGL binding
  * 7 See also



## OpenGL units in Free Pascal

The Free Pascal OpenGL interface consists of the following units: 

  * **gl** : This unit contains the main functionality such as drawing polygons, applying transformations, setting colors and materials,... Procedures always start with the preposition "gl".
  * **glu** : This unit contains OpenGL utils. Although it has some useful functions, this unit is not necessary as you can implement all glu Procedures with the functionality of the gl unit. Procedures always start with the preposition "glu".
  * **glext** : Vendors can provide additional functionality through extensions. Use this unit to use these extensions. 

The functionality specific to higher OpenGL versions (1.2 and later) is available in this unit as well. Initializing this functionality is similar to initializing normal OpenGL extensions: just call Load_GL_version_X_Y function. If your OpenGL library version is older than X.Y, Load_GL_version_X_Y will return false.

  * **glut** : This unit provides functionality for creating an OpenGL window. Although this is a cross-platform unit, most Windows operating systems don't have a glut dll by default.
  * **glx** : glx provides functionality to set up an OpenGL window in an X Window system. Procedures always start with the preposition "glx". Obviously, you cannot use this unit on non-X Window systems such as Windows.



## OpenGL units in Lazarus

Lazarus includes a TOpenGLControl - a LCL control with an OpenGL context. The Lazarus package LazOpenGLContext can be found in lazarus/components/opengl/lazopenglcontext.lpk. An example can be found in lazarus/examples/openglcontrol/openglcontrol_demo.lpi. 

If you want to modify TOpenGLControl implementation, for example to add new properties like ColorBits and AuxBuffers, see [Extending TOpenGLControl](<Extending_TOpenGLControl.md> "Extending TOpenGLControl"). 

## Third party OpenGL units

  * [GLScene](<GLScene.md> "GLScene") is a Lazarus package with lots of extra features for OpenGL applications.
  * [Castle Game Engine](<Castle_Game_Engine.md> "Castle Game Engine") allows you to navigate and render 3D worlds (in VRML, X3D and other 3D formats).



## Tutorials

  * [OpenGL Tutorial](<OpenGL_Tutorial.md> "OpenGL Tutorial")
  * [OpenGL Core Tutorials](<https://github.com/neurolabusc/OpenGLCoreTutorials>)



## Tutorial OpenGL 3.3 ( modern OpenGL )

  * [Lazarus OpenGL 3.3 - Tutorial](<https://wiki.delphigl.com/index.php/Lazarus_-_OpenGL_3.3_Tutorial>)\- (deutsch) (extern)
  * [Lazarus OpenGL 3.3 - Tutorial](<https://github.com/sechshelme/Lazarus-OpenGL-3.3-Tutorial/blob/master/wiki.md>)\- (deutsch) (extern) (github)



## Q/A using OpenGL binding

**Q: I don't understand why the ARB version does not work. AFAIK it has the same entry points as the core functionality and OpenGL versions are required to support the ARB version of functions even if the non ARB version is in the core now.**

A: No. OpenGL doesn't have to support the extension entry points, even when it supports the same functionality in core already. Vendor is free to remove the extensions, and support only the "core" functions. 

Also, sometimes when functionality is promoted from ARB extension into core it's slightly changed (although in 99% cases it stays the same). Example is the GLSL shaders, that expose (very slightly) different API through ARB extensions and through core 2.0 specification. 

Basically, what is exposed through extensions is completely orthogonal to what is exposed through core functionality, so they should be initialized separately. In particular cases, you can make tricks like 
    
    
    if glIsBufferARB = nil then glIsBufferARB := @glIsBuffer;
    

but this should be left to the final programmer that knows that "ARB_vertex_buffer_object" behavior is identical to how it works in core. 

**Q: When I try to use the gluTessCallback function from the glu unit, either the compiler complains or I get a segmentation fault when I try to run my program. How can I get it to work properly?**

A: The gluTessCallback function is defined in the following way in the **glu** unit: 
    
    
    {$IFDEF Windows}
      {$DEFINE extdecl := stdcall}
    {$ELSE}
      {$DEFINE extdecl := cdecl}
    {$ENDIF}
    
    gluTessCallback: procedure(tess: PGLUtesselator; which: GLenum; fn: TCallBack); extdecl;
    

**Windows note:** A common problem is that the calling convention of the glu functions seems to vary in several different GLUT DLLs builds for windows. In some it is stdcall, in some cdecl. The most used one seems to be stdcall. 

You should define your [callback](<callback.md> "callback") functions like so: 
    
    
    procedure MyTessBegin(which: GLEnum); 
    {$IFDEF Windows} stdcall; {$else} cdecl; {$endif}
    begin
      glBegin(which);
    end;
    
    procedure MyTessEnd; 
    {$IFDEF Windows} stdcall; {$else} cdecl; {$endif}
    begin
      glEnd();   // The () is required
    end;
    
    procedure MyTessVertex(data: Pointer);
    {$IFDEF Windows} stdcall; {$else} cdecl; {$endif}
    begin
      glVertex3dv(PGLDouble(data));
    end;
    

Note the cdecl/stdcall modifiers; they are required. You then call gluTessCallback in the following manner: 
    
    
    gluTessCallback(tess, GLU_TESS_BEGIN, TCallBack(@MyTessBegin));
    gluTessCallback(tess, GLU_TESS_END, TCallBack(@MyTessEnd));
    gluTessCallback(tess, GLU_TESS_VERTEX, TCallBack(@MyTessVertex));
    

Make sure you cast the pointer to your callback function to the TCallBack type, as shown above. 

Alternatively, if you want to refer to the OpenGL functions directly, use the following calling convention: 
    
    
    gluTessCallback(tess, GLU_TESS_BEGIN, TCallBack(glBegin));
    gluTessCallback(tess, GLU_TESS_END, TCallBack(glEnd));
    gluTessCallback(tess, GLU_TESS_VERTEX, TCallBack(glVertex3dv));
    

Finally, [here](<http://www.ludicity.org/files/tess_demo.pp>) is source code for a simple tessellation demo (uses glut). 

## See also

  * [Vulkan](<Vulkan.md> "Vulkan") is a new low-level Graphics API intended to replace OpenGL.

---

_Source: [https://wiki.freepascal.org/OpenGL](https://web.archive.org/web/20240308085007/https://wiki.freepascal.org/OpenGL)_
