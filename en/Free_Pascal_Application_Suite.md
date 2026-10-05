# Free Pascal Application Suite

The Free Pascal Application Suite plans to bundle many useful applications written in [Free Pascal](<Free_Pascal.md> "Free Pascal") in order to help increase the popularity of applications written in [Pascal](<Pascal.md> "Pascal"), in order to demonstrate the power of Lazarus/[Free Pascal](<Free_Pascal.md> "Free Pascal") and also in order to have a good kit of applications for the [Free Pascal Window Manager](<Free_Pascal_Window_Manager.md> "Free Pascal Window Manager"). 

## Contents

  * 1 What should all applications in this suite have?
  * 2 Releases
    * 2.1 Release 0.1 (targets mostly Windows)
  * 3 List of good Free Pascal applications to be included
  * 4 Other FPC Applications



## What should all applications in this suite have?

  1. Free/Open software license for the application, such as GPL, BSD or something like that
  2. Available in a public subversion, such as provided by source forge, googlecode, etc
  3. The applications should be cross-platform. 
     1. For the Desktop version: 
        1. An [Inno Setup](<http://www.innosetup.com>) installer for Windows. See also [Inno Setup Usage](<Inno_Setup_Usage.md> "Inno Setup Usage"). Best practices recomendations (not required): [High DPI](<High_DPI.md> "High DPI"), [Windows Icon](<Windows_Icon.md> "Windows Icon").
        2. RPM and DEB packages for Linux.
        3. Mac OS X Application bundle.
     2. For the Mobile version: Installers for Windows CE, iPhone and Android
  4. Some kind of stable release every once in a while
  5. A bug tracker (as soon as people use the application, they will start reporting bugs)



## Releases

### Release 0.1 (targets mostly Windows)

  * Lazarus Image Editor 0.9 - A raster image editor 
    * Download for Windows 32-bits: http:/sourceforge.net/projects/p-tools/files/Lazarus%20Image%20Editor/0.9/
    * Download source code: Lazarus-CCR rev 2285. See [Lazarus Image Editor](<Lazarus_Image_Editor.md> "Lazarus Image Editor")


  * LazPaint 4.7 - A raster image editor 
    * Download for Windows and Linux: <http://sourceforge.net/projects/lazpaint/files/bin/>
    * Download source code: <http://sourceforge.net/projects/lazpaint/files/src/> See [LazPaint](<LazPaint.md> "LazPaint")


  * Double Commander - A file manager 
    * Download for Windows and Linux: <http://doublecmd.sourceforge.net/>


  * Virtual Magnifying Glass 3.5 - a screen magnifier 
    * Download for Windows, Linux and Mac OS X: <http://magnifier.sourceforge.net/#download>


  * FPBrowser 0.5 - A raster image editor 
    * Download for Windows 32-bits: <http://sourceforge.net/projects/p-tools/files/FPBrowser/0.5/>
    * Download source code: Lazarus-CCR rev 2286. See [fpbrowser](<fpbrowser.md> "fpbrowser")


  * LazEyes 2.1 - A toy, the two eyes 
    * Download for Windows 32-bits: <http://sourceforge.net/projects/p-tools/files/LazEyes/2.1/>
    * Download source code: Lazarus-CCR rev 2291. See [LazEyes](<LazEyes.md> "LazEyes")


  * LazEdit 1.9 - A general text editor with syntax highlighting and HTML editing tools 
    * Download for Windows 32-bits: <http://sourceforge.net/projects/p-tools/files/LazEdit/1.9/>
    * Download source code: Lazarus-CCR rev 2299. See [LazEdit](<LazEdit.md> "LazEdit")



## List of good Free Pascal applications to be included

Name | Type/Group | State | Targets | Responsible | Comments   
---|---|---|---|---|---  
[LazEdit](<LazEdit.md> "LazEdit") | Text editor | Ready | all desktop | Bart and Felipe | \-   
[miniedit](<miniedit.md> "miniedit") | Text editor | ? | all desktop | Zaher | \-   
[MyNotex](<http://sites.google.com/site/mynotex/>) | Note taking/Text | Ready | GNU/Linux | Massimo Nardello | GPLv3   
[Magnifier](<http://magnifier.sourceforge.net/>) | Screen Magnifier/Accessibility | Ready | all desktop | Felipe Monteiro de Carvalho | ?   
[OvoPlayer](<OvoPlayer.md> "OvoPlayer") | Audio Player | Ready for Windows, is very slow in Linux | all desktop | - | \-   
[PeaZip](<https://peazip.github.io/>) | Compression | Lacks Mac support | windows,linux | ? | ?   
[Lazarus Image Editor](<Lazarus_Image_Editor.md> "Lazarus Image Editor") | Raster image editor | Needs release building | ? | ? | \-   
[LazPaint](<LazPaint.md> "LazPaint") | Raster image editor | An alternative image editor | all desktop | Circular | [BGRABitmap](<BGRABitmap.md> "BGRABitmap")  
Calculator | Calculator | Not even started | ? | ? | \-   
[Double Commander](<http://doublecmd.sourceforge.net/>) | File Manager | Ready | ? | ? | \-   
[fpChess](<fpChess.md> "fpChess") | Game | Under development | all desktop and all mobile | Felipe Monteiro de Carvalho | \-   
[fpbrowser](<fpbrowser.md> "fpbrowser") | Web Browser | Initial release ready | all desktop | ? | ?   
[fpfolders](</index.php?title=fpfolders&action=edit&redlink=1> "fpfolders \(page does not exist\)") | File Manager | Not started | all desktop | ? | ?   
[Turbo Circuit](<Turbo_Circuit.md> "Turbo Circuit") | Engineering | Under rework | all desktop | Felipe Monteiro de Carvalho | \-   
[LazEyes](<LazEyes.md> "LazEyes") | Toy | Ready | all desktop | Felipe Monteiro de Carvalho | \-   
[TappyTux](<TappyTux.md> "TappyTux") | Educational | Ready | all desktop | Dennis Seman e Felipe | \-   
  
## Other FPC Applications

This is a list for extra apps which might be used. 

Name | Type/Group | State | Targets | Responsible | Comments   
---|---|---|---|---|---  
[KSP](<Lazarus_Application_Gallery.md> "Lazarus Application Gallery") | Audio Player | Lacks maintainer to fix | windows,? | - | \-   
[ConTEXT Editor](<http://code.google.com/p/contexteditor/>) | Text Editor | Needs porting to Lazarus | Desktop | ? |   
[TextDIFF](<http://angusj.com/delphi/>) | Diff/Merge utility | Needs porting to Lazarus | Desktop | ? |   
[Martins Editor](<http://www.hypermake.com/english/betatest.html>) | Text Editor | Not free software? | All Desktop | ? |   
[Cactus Jukebox](<Cactus_Jukebox.md> "Cactus Jukebox") | Audio Player | Ready | all desktop | Abandoned | \-

---

_Source: [https://wiki.freepascal.org/Free_Pascal_Application_Suite](https://web.archive.org/web/20250122110956/https://wiki.freepascal.org/Free_Pascal_Application_Suite)_
