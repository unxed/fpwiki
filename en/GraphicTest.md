# GraphicTest

│ **English (en)** │  [**日本語 (ja)**](</GraphicTest/ja> "GraphicTest/ja") │    
****

[![Graphic test.png](https://wiki.freepascal.org/images/5/58/Graphic_test.png)](</File:Graphic_test.png>)

[](</File:Graphic_test.png> "Enlarge")

## Contents

  * 1 Introduction
    * 1.1 Test 1
      * 1.1.1 Test 1.1
      * 1.1.2 Test 1.2
      * 1.1.3 Test 1.3
    * 1.2 Test 3
    * 1.3 Test 4
    * 1.4 Test 5
    * 1.5 Test 6(20170503)
  * 2 Download
  * 3 See also



## Introduction

GraphicTest is simple program which measure FPS of various direct drawing methods. Aim is to determine fastest method how to copy custom off screen bitmap to visible area on screen. 

Listed benchmarks results are dependent on: 

  * computer hardware
  * compiler optimizations
  * operating system
  * used GUI toolkit
  * vertical sync and max FPS limit
  * display color depth (default 32-bit)
  * resolution of image (default 320x240)



In some methods it is necessary to do swapping B a R component of color in some platforms which results to better/worst test score. 

### Test 1

**Hardware:** AMD Turion X2 1,8 GHz, 2 GB RAM, HD2400 

#### Test 1.1

**OS:** Ubuntu 12.10, GNU/Linux kernel 3.5.0 i686, Gtk2 

Method | FPS | Frame duration [ms]   
---|---|---  
TBitmap.Canvas.Pixels | 1 | 1031.7   
TBitmap.Canvas.Pixels Update locking | 3.2 | 308.1   
TLazIntfImage.Colors copy | 54.3 | 18.4   
TLazIntfImage.Colors no copy | 64.4 | 15.5   
TBitmap.RawImage.Data | 102.2 | 9.8   
TBitmap.RawImage.Data PaintBox | 102.5 | 9.8   
TBitmap.RawImage.Data Move | 116 | 8.6   
TBGRABitmap PaintBox | 123.1 | 8.1   
TGR32Image | 68.5 | 14.6   
OpenGL | 56.7 | 17.6   
OpenGL PBO | 152.4 | 6.6   
Dummy | 38166.3 | 0   
  
#### Test 1.2

**OS:** Ubuntu 12.10, GNU/Linux kernel 3.5.0 i686, Qt4 

Method | FPS | Frame duration [ms]   
---|---|---  
TBitmap.Canvas.Pixels | 3.2 | 312.8   
TBitmap.Canvas.Pixels Update locking | 4.3 | 232.3   
TLazIntfImage.Colors copy | 80.1 | 12.5   
TLazIntfImage.Colors no copy | 85.1 | 11.7   
TBitmap.RawImage.Data | 223.4 | 4.5   
TBitmap.RawImage.Data PaintBox | 225.9 | 4.4   
TBitmap.RawImage.Data Move | 276.9 | 3.6   
TBGRABitmap PaintBox | 112.5 | 8.9   
Dummy | 489.6 | 2   
  
#### Test 1.3

**OS:** Windows 8 32-bit 

Method | FPS | Frame duration [ms]   
---|---|---  
TBitmap.Canvas.Pixels | 2,8 | 351   
TBitmap.Canvas.Pixels Update locking | 9,6 | 103,9   
TLazIntfImage.Colors copy | 93,6 | 10,7   
TLazIntfImage.Colors no copy | 97,8 | 10,2   
TBitmap.RawImage.Data | 258,2 | 3,9   
TBitmap.RawImage.Data PaintBox | 256,6 | 3,9   
TBitmap.RawImage.Data Move | 342,5 | 2,9   
TBGRABitmap PaintBox | 428 | 2,3   
TGR32Image | 464,8 | 2,2   
OpenGL | 221,6 | 4,5   
OpenGL PBO | 330,7 | 3   
Dummy | 112824 | 0   
  
### Test 3

**Hardware:** Intel Celeron CPU 2.66 GHz, 1.50 GB RAM, NVIDIA GeForce 6200. 

**OS:** Windows 7 x86. 

Method | FPS | Duration   
---|---|---  
TBitmap.Canvas.Pixels | 1.44 | 694.82   
TBitmap.Canvas.Pixels Update locking | 7.81 | 128.07   
TLazIntfImage.Colors copy | 96.6 | 10.35   
TLazIntfImage.Colors no copy | 96.7 | 10.34   
TBitmap.RawImage.Data | 462 | 2.16   
TBitmap.RawImage.Data PaintBox | 482 | 2.07   
TBGRABitmap PaintBox | 369 | 2.70   
OpenGL | 60.2 | 16.61   
OpenGL PBO | 60.1 | 16.63   
  
**Conclusion** the faster method (less duration in ms) with highest fps: 

1) Bitmap.RawImage.Data PaintBox 

2) TBitmap.RawImage.Data 

3) TBGRABitmap PaintBox 

### Test 4

**Hardware:** AMD Sempron 1,6 GHz, 1 GB DDR, NVIDIA GeForce FX 5200 

**OS:** Windows XP SP3 

Method | FPS | Frame duration [ms]   
---|---|---  
TBitmap.Canvas.Pixels | 3,7 | 267,6   
TBitmap.Canvas.Pixels Update locking | 12,7 | 78,6   
TLazIntfImage.Colors copy | 76,2 | 13,1   
TLazIntfImage.Colors no copy | 85,9 | 11,6   
TBitmap.RawImage.Data | 205 | 4,9   
TBitmap.RawImage.Data PaintBox | 295,3 | 3,4   
TBitmap.RawImage.Data Move | 241,3 | 4,1   
TBGRABitmap PaintBox | 343,7 | 2,9   
TGR32Image | 320,8 | 3,1   
OpenGL | 7,1 | 141,5   
OpenGL PBO | 6,5 | 154,6   
Dummy | 88110 | 0   
  
### Test 5

**Conditions:** Intel Core 2 Duo 2,66 GHz; 2 GB DDR2, NVIDIA GeForce 8600 GT, Windows XP SP3 32-bit 

Method | FPS | Frame duration [ms]   
---|---|---  
TBitmap.Canvas.Pixels | 7,2 | 138,9   
TBitmap.Canvas.Pixels Update locking | 28,4 | 35,2   
TLazIntfImage.Colors copy | 183,4 | 5,5   
TLazIntfImage.Colors no copy | 207,8 | 4,8   
TBitmap.RawImage.Data | 680,8 | 1,5   
TBitmap.RawImage.Data PaintBox | 1124,5 | 0,9   
TBitmap.RawImage.Data Move | 781,6 | 1,3   
TBGRABitmap PaintBox | 1093,2 | 0,9   
TGR32Image | 1954,1 | 0,5   
OpenGL | 963,3 | 1   
OpenGL PBO | 1757,4 | 0,6   
Dummy | 401069,4 | 0   
  
### Test 6(20170503)

**Conditions:** Intel Core i7-3630QM 2,40 GHz; 8 GB, Windows 8.1 64-bit 

Method | FPS | Frame duration [ms]   
---|---|---  
TBitmap.Canvas.Pixels | 1,8 | 544,8   
TBitmap.Canvas.Pixels Update locking | 10,0 | 100,1   
TLazIntfImage.Colors copy | 113,2 | 8,8   
TLazIntfImage.Colors no copy | 131,8 | 7,6   
TBitmap.RawImage.Data | 662,9 | 1,5   
TBitmap.RawImage.Data PaintBox | 650,1 | 1,5   
TBitmap.RawImage.Data Move | 1047,3 | 1,0   
TBGRABitmap PaintBox | 640,9 | 1,6   
OpenGL | 316,0 | 3.2   
OpenGL PBO | 728,7 | 1,4   
Dummy | 219514,5 | 0.005   
  
## Download

Application can be downloaded from svn repository: 
    
    
    svn checkout <http://svn.zdechov.net/PascalClassLibrary/GraphicTest/>
    

## See also

  * [Fast direct pixel access](<Fast_direct_pixel_access.md> "Fast direct pixel access")

---

_Source: [https://wiki.freepascal.org/GraphicTest](https://web.archive.org/web/20210618214854/https://wiki.freepascal.org/GraphicTest)_
