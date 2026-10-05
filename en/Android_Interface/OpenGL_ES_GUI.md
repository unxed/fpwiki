# Android Interface/OpenGL ES GUI

[![Android robot.svg](https://wiki.freepascal.org/images/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

This article applies to [Android](</Category:Android> "Category:Android") only.

See also: [Multiplatform Programming Guide](<../Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Go back to [Android Interface](<../Android_Interface.md> "Android Interface")

In this solution nothing of the drawing will pass through the Java helper applications, instead everything will be based on OpenGL ES and on the Lazarus Custom Drawn Controls, as explained in the image below. A generic Java application will be created to launch the LCL application and to expose indispensable parts of the Java Android API to the LCL and both will communicate using pipes. The Lazarus user application will be built as a linux-arm executable and be attached to the generic Java encapsulating application. The encapsulating Java program shouldn't need to be changed by Lazarus users, and no knowledge of Java will be necessary to develop Lazarus applications for Android. Of course Java knowledge will be necessary to implement the auxiliary Java application. 

[![Android LCL Architecture.png](https://wiki.freepascal.org/images/f/f8/Android_LCL_Architecture.png)](</File:Android_LCL_Architecture.png>)

---

_Source: [https://wiki.freepascal.org/Android_Interface/OpenGL_ES_GUI](https://web.archive.org/web/20170322080335/https://wiki.freepascal.org/Android_Interface/OpenGL_ES_GUI)_
