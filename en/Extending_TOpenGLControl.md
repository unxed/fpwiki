# Extending TOpenGLControl

Some information that may a useful starting point for someone willing to create a patch to add ColorBits and AuxBuffers and other similar properties to TOpenGLControl: 

1\. For GLX (GTK), there are two ways to choose context attributes (both have to be supported for now, to support older GLX versions, and to allow multi-sampling on modern GLX versions): 

  * glXChooseFBConfig. Docs on <http://www.opengl.org/sdk/docs/man/xhtml/glXChooseFBConfig.xml> .
  * glXChooseVisual. Docs on <http://www.glprogramming.com/blue/ch07.html> .



They are both handled by common CreateOpenGLContextAttrList inside glgtkglxcontext.pas, they both support values GLX_RED/GREEN/BLUE_SIZE and GLX_AUX_BUFFERS. 

Current implementation sets minimum red/green/blue bits to 1. 

2\. For WGL (WinAPI), also two ways of choosing context attributes are available, for similar reasons (the modern way has more features, but the older way is 100% universally supported): 

  * wglChoosePixelFormatARB. See values WGL_COLOR_BITS_ARB, WGL_RED/GREEN/BLUE_BITS_ARB, WGL_AUX_BUFFERS_ARB. See docs on <http://www.opengl.org/registry/specs/ARB/wgl_pixel_format.txt> .
  * The older ChoosePixelFormat. See fields cColorBits, cAuxBuffers in PIXELFORMATDESCRIPTOR. There are also cRed/Green/BlueBits, but they are documented as unused. Docs on <http://msdn.microsoft.com/en-us/library/dd318284%28v=vs.85%29.aspx> .



Current implementation sets color bits = 24. 

Note that you cannot request complete colorbits for GLX (only separate r/g/b bits), and you cannot request separate r/g/b bits for WinAPI with ChoosePixelFormat. Probably we should make separate ColorBitsRed, ColorBitsGreen, ColorBitsBlue properties (as these are more flexible), and for ChoosePixelFormat just pass PIXELFORMATDESCRIPTOR.cColorBits := ColorBitsRed + ColorBitsGreen + ColorBitsBlue. 

I hope this information helps anyone willing to create a patch :) The job is fairly easy, you can grep e.g. for AlphaBits inside lazarus/trunk/components/opengl/ to see where you need to insert your code. You basically pass down the information to appropriate WGL/GLX routines.

---

_Source: [https://wiki.freepascal.org/Extending_TOpenGLControl](https://web.archive.org/web/20240308085009/https://wiki.freepascal.org/Extending_TOpenGLControl)_
