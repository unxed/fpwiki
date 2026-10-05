# THtmlPort

│ **English (en)** │

## Contents

  * 1 About
  * 2 Screenshots
  * 3 Download
  * 4 License
  * 5 Installation
  * 6 Status
  * 7 Contact
  * 8 Known Issues
    * 8.1 Carbon widgetset
    * 8.2 Win32 widgetset
    * 8.3 GTK2 widgetset
    * 8.4 All widgetsets
  * 9 See Also



### About

THtmlPort is a Lazarus/Free Pascal version of Dave Baldwin's HTML Components, including THtmlViewer, TFrameViewer and TFrameBrowser. The ported code retains full compatibility with Delphi. 

The original unported source (version 9.45) is available from <http://www.pbear.com/>

For a browser implemented using this component one can see [fpbrowser](<fpbrowser.md> "fpbrowser"). 

### Screenshots

[![](https://wiki.freepascal.org/images/5/59/THtmlPort_Framedem_Carbon.jpg)](</File:THtmlPort_Framedem_Carbon.jpg>)

[](</File:THtmlPort_Framedem_Carbon.jpg> "Enlarge")

FrameDem sample app using the Carbon widgetset

[![](https://wiki.freepascal.org/images/e/eb/THtmlPort_Framedem_Win32.jpg)](</File:THtmlPort_Framedem_Win32.jpg>)

[](</File:THtmlPort_Framedem_Win32.jpg> "Enlarge")

FrameDem sample app using the Win32 widgetset

[![](https://wiki.freepascal.org/images/8/86/THtmlPort_Framedem_GTK2.jpg)](</File:THtmlPort_Framedem_GTK2.jpg>)

[](</File:THtmlPort_Framedem_GTK2.jpg> "Enlarge")

FrameDem sample app using the GTK2 widgetset

### Download

You can download the ported source using Subversion: 
    
    
     svn co <https://svn.code.sf.net/p/lazarus-ccr/svn/components/thtmlport> [local dir]
    

New download: 

<https://github.com/BerndGabriel/HtmlViewer>

### License

The original Delphi code on which this port is based has been released by Dave Baldwin into the public domain. Additional code supplied with this port is released under the MPL 1.1 license. 

### Installation

To install the HtmlComp package in Lazarus: 

  * Choose Package | Open package file, then select htmlcomp.lpk (in the package subfolder).
  * Click Compile to compile the package.
  * Click Compile a second time. A quirk in FPC < 2.7.1 apparently make this extra compile necessary. (Q solution to this problem is to move htmlun2.pas before htmlgif1.pas in the package file list)
  * Click Install to rebuild and relaunch Lazarus.



To compile a sample application that uses the HtmlComp package: 

  * Choose File | Open, then select framedem.lpi or htmldemo.lpi (both in the demo_src subfolder).
  * Choose Run | Run to compile and run the sample app.



### Status

Win32 widgetset, GTK2 widgetset, Carbon widgetset: both FrameDem and HtmlDemo sample apps are working. Works good with Lazarus 1.0+. 

### Contact

If you have fixes for any problems in the package, please contact Phil: 
    
    
     MacPgmr (at) fastermac (dot) net
    

### Known Issues

#### Carbon widgetset

  * ~~In FrameDem app, title and subtitle at top (readme2.htm) display in sans serif font even though style in demo.css specifies Times New Roman, which is a Mac font.~~


  * ~~In FrameDem app, Wingding characters (sample3.htm) are not correct.~~


  * ~~In FrameDem app, image files that are inlined in text (sample3.htm) or tables (properties.htm) do not display. This is accompanied by many errors outputted to the Console app window by the Carbon widgetset code.~~ ==> Note that raster operations are not implemented yet in Carbon widgetset, so some images display very rough looking.


  * Vertical scroll bar thumb does not move all the way to bottom of scroll bar when window is scrolled all the way down.


  * ~~In FrameDem app, shelling to whatsnew.htm does not work since can't start second copy of app and app is not MDI.~~


  * In FrameDem app, clicking Preview in Print dialog ~~gives error that Copies is set to 0. Changing Copies to 1 allows Preview to proceed, but not much is shown. In HtmlDemo app, previewing works fairly well, although doesn't display quite as good as in app itself. With both apps, the second time Preview button is clicked, processing hangs~~ displays preview of all pages, but pages 2+ are blank.



#### Win32 widgetset

  * ~~HtmlDemo sample app throws exception at startup / loads but does not display HTML files.~~


  * ~~FrameDem sample app sometimes fails to load samples3.htm when HTML Samples link is clicked; other times it loads fine.~~


  * ~~In FrameDem app, shelling to whatsnew.htm starts another copy of app okay, but nothing displayed. Same issue with starting FrameDem with a file other than demo.htm specified on command line.~~


  * In Lazarus form designer, dropping THtmlViewer on form displays a scroll bar in the upper left corner of the control. Changing THtmlViewer's BorderStyle, for example, hides the scroll bar. Looks like a form designer bug.



#### GTK2 widgetset

  * ~~HtmlDemo sample app won't load anything / loads but does not display HTML files.~~


  * ~~FrameDem sample app throws exception in several places (for example, clicking Stylesheet Support link). In debugger, enter "break gdk_x_error" and run backtrace following exception to see where error is occurring.~~


  * Font issues in FrameDem app. This may be due to inability to match font against Windows font that is specified in .css files. For example, ~~no underlining or strikethrough,~~ text in <PRE> tag is not indented properly, ~~text looks too big in About box,~~ etc.


  * In FrameDem app, first two Wingding characters (sample3.htm) are not correct (probably a font matching issue).


  * ~~In FrameDem app, File | Printer Setup throws exception in /printers/unix/cupslcl.pas if selected before clicking in one of the frames. Clicking in a frame enables File | Print and once Printing dialog has been displayed, File | Printer Setup is okay.~~


  * ~~In both apps' Fonts dialog (Options | Default Font/Colors), the Font Name list will be empty if HasX was not defined when GTK2 was compiled (default on OS X; without HasX, EnumFontFamiliesEx is not implemented, meaning Screen.Fonts is empty list). But even if the list does contain fonts (HasX defined), clicking one blanks the example HTML (LoadAgain failing?).~~


  * ~~In FrameDem app, shelling to whatsnew.htm starts another copy of app okay and loads file, but no vertical scroll bar is displayed, although clicking on invisible scroll bar does scroll the window.~~



#### All widgetsets

  * In the sample apps' Fonts dialog (Options | Default Font/Colors), the Font Color and Link Color areas are blank since a TColorGrid was used in the original Delphi apps. TColorGrid is not an LCL control and has been removed from the .lfm files.


  * Preview has been disabled since this depends on TMetaFile, which is not part of LCL.


  * Copying to clipboard ~~does not yet work~~ is basically functional but only on first copy (note also that GTK2 apparently does not support text/html clipboard format).


  * ~~Does not yet support 64-bit.~~ ==> Package should now compile, install, and compile and run sample apps okay with 64-bit compilers on Win64 and Linux64 (CPU64).


  * No support for PNG image files (except on Windows - current support is still written for a Windows DLL).



### See Also

  * [Webbrowser](<Webbrowser.md> "Webbrowser")

---

_Source: [https://wiki.freepascal.org/THtmlPort](https://web.archive.org/web/20250326014341/https://wiki.freepascal.org/THtmlPort)_
