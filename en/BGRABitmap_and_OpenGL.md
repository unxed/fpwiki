# BGRABitmap and OpenGL

│ **English (en)** │

[BGRABitmap](<BGRABitmap.md> "BGRABitmap") allows to draw with [OpenGL](<OpenGL.md> "OpenGL") and so to benefit from hardware acceleration. 

## Contents

  * 1 OpenGL surface
    * 1.1 TBGLVirtualScreen
    * 1.2 Using only TOpenGLControl
    * 1.3 Note on Linux
  * 2 Textures
  * 3 Creating textures
  * 4 Examples
    * 4.1 Probability



### OpenGL surface

You will need an OpenGL surface. The package LazOpenGLContext in the directory components\opengl of lazarus supplies this surface. It is necessary to add some code to handle it properly. The control TBGLVirtualScreen is here to make it easy for you. Note that it relies on LazOpenGLContext so you will need to install this package anyway. 

The installation of the package on Linux can be problematic. See below the Linux note before installing the package. 

#### TBGLVirtualScreen

The package BGLControls in the archive of [BGRABitmap](<BGRABitmap.md> "BGRABitmap") contains the component TBGLVirtualScreen that supplies an easy-to-use OpenGL surface via [TBGLContext](<https://bgrabitmap.github.io/doc/BGRAOpenGL.TBGLContext.html>) class. It is recommended to install it as well. 

It appears in the component toolbar, in the OpenGL tab. You just need to put it on the form and to redefine the Redraw event. For example to draw a red rectangle: 
    
    
    uses 
      BGRABitmapTypes;
    
    procedure TForm1.BGLVirtualScreen1Redraw(Sender: TObject; BGLContext: TBGLContext);
    begin
      BGLContext.Canvas.FillRect(10, 10, 100, 100, CSSRed);
    end;
    

#### Using only TOpenGLControl

In the tab OpenGL you will find TOpenGLControl. Add it to the form and set the property `AutoResizeViewPort` to `**true**`. 

In the Paint event: 
    
    
    uses BGRAOpenGL, BGRABitmapTypes;
    
    procedure TForm1.OpenGLControl1Paint(Sender: TObject);
    begin
      BGLViewPort(OpenGLControl1.Width, OpenGLControl1.Height, BGRAWhite);
    
      // do you drawing here
      BGLCanvas.FillRect(10, 10, 100, 100, CSSRed);
    
      OpenGLControl1.SwapBuffers;
    end;
    

You will get: 

[![bgrabitmap-openglcontrol-example1.png](https://wiki.freepascal.org/images/e/e6/bgrabitmap-openglcontrol-example1.png)](</File:bgrabitmap-openglcontrol-example1.png>)

#### Note on Linux

On Linux, it is possible that the [OpenGL](<OpenGL.md> "OpenGL") library be missing, which prevents the final compilation linking step. To solve the problem, do the following:Pour résoudre le problème, effectuez: 
    
    
      #install library
      sudo apt-get install libgl-dev 
      #in some cases, add a link to the library (for 64bits processors)
      sudo ln -s /usr/lib/x86_64-linux-gnu/mesa/libGL.so.1 /usr/lib/libGL.so
      sudo ln -s /usr/lib/x86_64-linux-gnu/libglib-2.0.so /usr/lib/libglib-2.0.so
      sudo ln -s /usr/lib/x86_64-linux-gnu/llibgthread-2.0.so /usr/lib/libgthread-2.0.so
      sudo ln -s /usr/lib/x86_64-linux-gnu/libgmodule-2.0.so /usr/lib/libgmodule-2.0.so
      sudo ln -s /usr/lib/x86_64-linux-gnu/libgobject-2.0.so /usr/lib/libgobject-2.0.so
    

After checking the package to install, clicking Yes directly may not work. Instead you can go to Tools and configure Lazarus build. Check the option to do a full clean compilation. From there compile the IDE. 

When recompiling Lazarus, the program may be stuck on the slashscreen. In that case, terminate the process with the system monitor. 

### Textures

[OpenGL](<OpenGL.md> "OpenGL") uses its own memory to store images. Moreover it is necessary to be in the right OpenGL context. So load images within the OnPaint event or use LoadTextures and UnloadTextures events of TBGLVirtualScreen. You can also use the fonction UseContext. 

Textures are stored in a IBGLTexture variable. You can load a texture directly from a file: 
    
    
    uses 
      BGRAOpenGL;
    
    var 
      tex: IBGLTexture;
    
    begin
      tex := BGLTexture(path);
      ...
    

To free it, simply do: 
    
    
    tex := nil;
    

### Creating textures

Instead of using TBGRABitmap class, use [TBGLBitmap class](<https://bgrabitmap.github.io/doc/BGRAOpenGL.TBGLBitmap.html>) of [BGRAOpenGL unit](<https://bgrabitmap.github.io/doc/BGRAOpenGL.html>). It is similar in all respect except that it has a Texture property that can be used with OpenGL functions. 

Finally, if you have finished your drawing and will not modify it anymore, you can free the TBGLBitmap object and retrieve the texture with MakeTextureAndFree function. 
    
    
    uses 
      BGRAOpenGL, BGRABitmapTypes;
    
    var 
      bmp: TBGLBitmap; 
      tex: IBGLTexture;
    
    begin
      bmp := TBGLBitmap.Create(path);
      bmp.Rectangle(0, 0, bmp.Width, bmp.Height, CSSRed, dmSet);
      tex := bmp.MakeTextureAndFree;
      ...
    

### Examples

You will find examples in the directory test/test4lcl_opengl[[1]](<https://github.com/bgrabitmap/bgrabitmap/tree/master/test/test4lcl_opengl>). 

#### Probability

There is also a demo called probability[[2]](<https://github.com/bgrabitmap/bgracontest/tree/master/2015/probability>) in BGRAContext 2015 repository. 

[![probability-screenshot.png](https://wiki.freepascal.org/images/c/cc/probability-screenshot.png)](</File:probability-screenshot.png>)

---

_Source: [https://wiki.freepascal.org/BGRABitmap_and_OpenGL](https://web.archive.org/web/20250417155540/https://wiki.freepascal.org/BGRABitmap_and_OpenGL)_
