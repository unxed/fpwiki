# fb embedded2 Laz

## Contents

  * 1 About
  * 2 Download
  * 3 Introduction
  * 4 Installation and usage
  * 5 Contribution
  * 6 See also



## About

`Fb_embedded2.pas` is a Pascal unit that can be included in a Lazarus project to enable embedding of [Firebird](<Firebird.md> "Firebird") (version 2.0.3) [database](<Databases.md> "Databases") server (a.k.a. [DBMS](<http://en.wikipedia.org/wiki/DBMS>)) into an `.exe` file, without need for distributing any DLL. The original code for Delphi was created by Fikret Hasovic and Samir Ribic, modified by Zlatko Matic ([matalab@gmail.com](<mailto:matalab@gmail.com>)) for FPC/Lazarus. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** You do **not** need this to use the "regular" embedded Firebird database.

## Download

@ [Lazurus CCR SourceForge repository](<http://sourceforge.net/projects/lazarus-ccr/files/Firebird%20Embedded/Firebird%20Embedded%202%20-%2020080124/>) (file: `fb_embedded2_Laz_1_0.zip`). 

## Introduction

Currently, `fb_embedded2.pas` works only on Windows. It reads lib from memory stream. When you unpack the zipped file `fb_embedded2_Laz_1_0.zip`, you will see it contains everything needed for embedding Firebird 2.0.3 into your `.exe`: 

  * Firebird embedded 2.0.3: all required DLLs,
  * `fb_embedded2.pas`: unit to be included into your Lazarus project,
  * `FB_Embedded.rc`:> textual file to be converted to Windows resource file,
  * `FB_Embedded.res`: Windows resource file, used by `fb_embedded2.pas`,
  * windres utility: used to create `FB_Embedded.res` from `FB_Embedded.rc`,
  * `fb_embedded.bat`: creates `FB_Embedded.res`,
  * several other `.pas` and `.pp` files that should be included into your project.



    These are files from `C:\lazarus\debugger\windebug` folder. `winddwarf.pas` needed small modification, regarding overloaded `Min()` function. I included modified `winddwarf.pas` in this distribution, so you can either include modified `winddwarf.pas`, `winddwarfconst.pas` and `windsymbols.pas` into your project or you can modify the original source code. In that case, you need only `fb_embedded2.pas` and resource file to be included in your project.
    The change to be done in `winddwarf.pas` is the following:
    
    the **for** cycle in line 2511
    
    
    
    for n := 1 to Min(80, count) do
    

    must be changed to
    
    
    
    for n := 1 to Min(80, LongInt(count)) do
    

## Installation and usage

No installation is required. Just copy the content of this folder into your project directory and include `fb_embedded2.pas` and, optionally, other `.pas` and `.pp` files from this folder into your application. You can check paths in `FB_Embedded.rc`. If you change paths, recreate `FB_Embedded.res` by double clicking the `fb_embedded.bat`. Then recompile your application. The Firebird embedded server will be included in your `.exe`, without need for external DLLs. The good thing is that your data-access components will work with embedded database, without change. For example, I use [Zeoslib](<ZeosDBO.md> "ZeosDBO"). Compiled `.exe` works without DLLs distributed. 

_Since Wine 0.9.60, you can run Windows`.exe` files compiled with Windows version of Lazarus and `fb_embedded.pas`. I have tested it on OpenSuse 10.3, Wine 0.9.60. This is an interesting possibility for cross-platform programming._

## Contribution

Everybody is welcome to improve this code. Currently it works only in Windows. I hope somebody will rewrite it to be cross-platform. 

## See also

  * [Firebird embedded](<Firebird_embedded.md> "Firebird embedded") "Regular" Firebird embedded (i.e. not bundled as a resource)

---

_Source: [https://wiki.freepascal.org/fb_embedded2_Laz](https://web.archive.org/web/20250430000559/https://wiki.freepascal.org/fb_embedded2_Laz)_
