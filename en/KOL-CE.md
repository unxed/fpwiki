# KOL-CE

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

This article applies to [Windows CE](</Category:WinCE> "Category:WinCE") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[français (fr)](</KOL-CE/fr> "KOL-CE/fr")** │  **[한국어 (ko)](</KOL-CE/ko> "KOL-CE/ko")** │  **[русский (ru)](<../ru/KOL-CE.md> "KOL-CE/ru")** │  **[中文（中国大陆） (zh_CN)](</KOL-CE/zh_CN> "KOL-CE/zh CN")** │  **[中文（臺灣） (zh_TW)](</KOL-CE/zh_TW> "KOL-CE/zh TW")** │    
****

## Contents

  * 1 Introduction
    * 1.1 Requirements
    * 1.2 Supported targets
  * 2 Download
  * 3 Installation
  * 4 Using MCK
    * 4.1 Creating MCK project
    * 4.2 Adding a form
    * 4.3 Writing code
    * 4.4 IMPORTANT
    * 4.5 Run-time form creation
  * 5 WinCE
    * 5.1 Setup
    * 5.2 Hints and notes
    * 5.3 Known issues
  * 6 Documentation
  * 7 KOL-CE in real-world applications
  * 8 See also
  * 9 Contacts



## Introduction

KOL-CE is Free Pascal/Lazarus port of KOL&MCK devloped by Vladimir Kladov ([website](<http://f0460945.xsph.ru/rindex.htm>)). KOL-CE is developed by Yury Sidorov and distributed under [wxWindows Library Licence](<http://www.opensource.org/licenses/wxwindows.php>). 

KOL-CE allows to create very compact Win32/WinCE GUI applications (starting from ~40KB executable for project with empty form). 

MCK is Lazarus package wich allows VISUAL development of KOL-CE projects in Lazarus IDE. 

Initially KOL-CE was planned as KOL port for [WinCE](<WinCE_port.md> "WinCE port") only. But later it was decided to keep Win32 functionality and made it work with FPC smoothly. The original MCK can not be used with Lazarus at all. The more current versions of [KOL](<KOL.md> "KOL") also work very well with FPC and targets both 32 and 64 bit applications, but do not include an MCK for Lazarus yet: you can use MCK from KOL-CE and recompile against the newer kol.pas version with some thought, but requires patching some of the Lazarus MCK generated sourcecode to reflect kol's current status. 

### Requirements

  * Free Pascal compiler 2.2.0 or later for Win32.
  * arm-wince cross compiler 2.2.0 or later for Win32 (for [WinCE](<WinCE_port.md> "WinCE port") development).
  * Lazarus 0.9.26 or later for Win32.



### Supported targets

  * All 32-bit Windows: from Windows 95 to Windows 8.1.
  * [Windows CE](<WinCE_port.md> "WinCE port") based PocketPC and Smartphones.



## Download

Download the latest release of KOL-CE [here](<http://sourceforge.net/project/showfiles.php?group_id=188451>). 

Also you can check out the freshest KOL-CE sources from svn using this link: <https://kol-ce.svn.sourceforge.net/svnroot/kol-ce/trunk>

## Installation

* * *

**Important:** Since KOL-CE 2.80.2 `DisableFakeMethods` define is not needed anymore. 

If you previously used KOL-CE 2.80.1 or older, you need to rebuild Lazarus **without** `DisableFakeMethods` define before installing MCK package. Otherwise **event handlers will not work**! 

To do that: 

  1. Run Lazarus.
  2. Choose **Tools > Configure "Build Lazarus"...** menu item.
  3. Choose **Clean Up + Build all** on **Quick Build Options** page.
  4. Open **Advanced Build Options** page and **remove** `-dDisableFakeMethods` from **Options** input field.
  5. Click **Build** button to rebuild Lazarus.



* * *

[![MCK package](https://wiki.freepascal.org/images/2/2e/MirrorKOLPackage.png)](</File:MirrorKOLPackage.png> "MCK package")

  1. Download KOL-CE sources and put them on some folder on your filesystem.
  2. Run Lazarus and choose **Components > Load package file** menu item. Then navigate to **MCK** folder and choose `**MirrorKOLPackage.lpk**` file.
  3. Package window will appear. Press **Install** button.
  4. Lazarus will compile MCK package and IDE will be restarted.
  5. After restart KOL tab will appear on components palette.



[![KOL components palette](https://wiki.freepascal.org/images/d/d1/KOLComponents.png)](</File:KOLComponents.png> "KOL components palette")

NOTE: If you can't see all KOL components on the palette, resize window with components palette vertically. You will see the second row of components on KOL tab (as on screenshot above). 

MCK package upgrade is very simple as well. Just overwrite KOL-CE sources with new version, open MCK package and press **Install** button to recompile the package. 

## Using MCK

### Creating MCK project

[![MCK form](https://wiki.freepascal.org/images/0/04/MCKForm.png)](</File:MCKForm.png> "MCK form")

  1. Start Lazarus and select **File > New...** menu item.
  2. Choose **KOL Application** under **Project** section and press **OK** button.
  3. New project for KOL application will be created.
  4. Save this project with desired name.
  5. Play with your new KOL/MCK Project (adjust parameters, drop TKOL... components, compile, run, debug, etc.) Enjoy!



### Adding a form

  1. Select **File > New...** menu item.
  2. Choose **KOL Form** under **File** section and press **OK** button.
  3. Save new form with desired name.



### Writing code

Do not use names from RTL/FCL/LCL, especially from SysUtils, Classes, Forms, etc. All what you need, you should find in KOL, Windows, Messages units. And may be, write by yourself (or copy from another sources). When you write code in mirror project - usually place it in event handlers. You also can add any code where you wish but avoid changing first section of your mirror LCL form class declaration. And do not change auto-generated inc-files. Always remember, that code, that you write in mirror project, must be accepted both by LCL and KOL. By LCL - at the stage of compiling mirror project (and this is necessary, because otherwise converting mirror project to reflected KOL project will not be possible). And by KOL - at the stage of compiling written code in KOL namespace. 

### IMPORTANT

To resolve conflict between words `LCL.Self` and `KOL.@Self`, which are interpreted differently in KOL and LCL, special field is introduced - `Form`. In LCL, `Form` property of `TKOLForm` component "returns" `Self`, i.e. form object itself. And in KOL, `Form: PControl` is a field of object, containing resulting form object. Since this, it is correctly to change form's properties in following way: 
    
    
    Form.Caption := 'Hello!'; 
    

(Though old-style operator Caption := 'Hello!'; is compiled normally while converting mirror project to KOL, it will be wrong in KOL environment). But discussed above word `Form` is only to access form's properties - not its child controls. You access child controls and form event handlers by usual way. e.g.: 
    
    
    Button1.Caption := 'OK';
    Button1Click(Form);
    

### Run-time form creation

It is possible to create several instances of the same form at run-time. And at least, it is possible to make form not AutoCreate, and create it programmatically when needed. Use global function `NewForm1` (replacing Form1 with your mirror form name), for instance: 
    
    
    NewForm1( TempForm1, Applet );
    

To make this possible, NEVER access global variable created in the unit during conversation, unless You know why You are doing so. Refer to Form variable instead. 

## WinCE

#### Setup

[![arm-wince target](https://wiki.freepascal.org/images/7/7a/arm-wince-target.png)](</File:arm-wince-target.png> "arm-wince target")

You need to install arm-wince cross compiler for Win32 to compile WinCE executables. Get it [here](<http://www.freepascal.org/download.var>). 

To compile for arm-wince target open compiler options of your project using **Project > Compiler options...** menu item. Open **Code** tab and change target platform to arm-wince. 

**NOTE:** You can receive the following error while compiling your KOL-CE project for WinCE: 
    
    
    Compiling resource KOL-CE.rc
    arm-wince-windres.exe: no resources
    KOL.PAS(57901) Error: Error while linking
    KOL.PAS(57901) Fatal: There were 1 errors compiling module, stopping
    

In such case you need to edit Windows **PATH** environment variable and add path to folder where win32 fpc binaries are located.  
To find out which path to add, go to **Environment options** in Lazarus and see compiler path.  
**Quit Lazarus before editing PATH**.  
To edit **PATH** variable right click on **My Computer** icon go to **Advanced** tab and click **Environment Variables** button. 

#### Hints and notes

  * To make form fullscreen as most Pocket PC applications have, don't change the form position and size. If form size and/or position was changed the form will look like dialog with caption and close button. In MCK set **defaultSize** and **defaultPosition** of TKOLForm to True to make form fullscreen.



#### Known issues

  * The following components are not supported: RichEdit.
  * Transparency and double buffering are not supported.
  * Only gsVertical, gsHorizontal gradient panel styles are supported.
  * Horizontal text alignment does not work in single line edit control. Use memo if you need text alignment.
  * Vertical text alignment does not work for panel and label.



## Documentation

  * Visit official KOL&MCK website for documentation and information: <http://kolmck.ru>
  * Read MCK documentation in **KOLmirrorReadme.txt** file inside MCK folder.



## KOL-CE in real-world applications

  * [Password Manager XP Mobile](<http://www.cp-lab.com/windows-mobile.html>)
  * [ChARMeD disassembler](<http://blog.carolos.za.net/2007/05/charmed-for-pocket-pc-beta-030.html>)
  * [WMWifiRouter](<http://www.jongma.org/WMWifiRouter/>)



## See also

  * [ The wiki page for KOL](<KOL.md> "KOL")
  * [Official KOL&MCK website](<http://f0460945.xsph.ru/rindex.htm>)
  * KOL-CE page at SourceForge <http://sourceforge.net/projects/kol-ce/>
  * [WinCE port of Free Pascal](<WinCE_port.md> "WinCE port")
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface")
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes")



## Contacts

Report bugs, submit patches and ask questions at project's page at SourceForge: <http://sourceforge.net/projects/kol-ce/>

---

_Source: [https://wiki.freepascal.org/KOL-CE](https://web.archive.org/web/20240920213339/https://wiki.freepascal.org/KOL-CE)_
