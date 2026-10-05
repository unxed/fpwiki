# fpGUI

│ **English (en)** │

fpGUI is a Object Pascal toolkit for cross-platform application development. It provides single-source portability across Linux, MS Windows, *BSD, Solaris/OpenSolaris, [ReactOS](<ReactOS.md> "ReactOS") and embedded devices like Embedded Linux and Windows CE. fpGUI Toolkit can be used for Open Source and Commercial applications. 

fpGUI is a widgetset completely written in Object Pascal. It links directly with the underlying windowing system (Xlib, GDI), and thus avoids the need for many large external libraries (eg: Qt, GTK etc). The main design goal is to get a consistent look and behavior across platforms. 

  
**Latest released version: v1.4.1 (2015-09-02).**

For more information, see the fpGUI Toolkit website at: 

  * <http://fpgui.sourceforge.net>



  
**Complete documentation.**

Detailed documentation is here =>

  * <http://fpgui.sourceforge.net/apidocs>



  
**fpGUI is also on GitHub.**

Last stable version =>

  * <https://github.com/graemeg/fpgui/>



  
Last develop (trunk) version (very stable too) =>

  * <https://github.com/graemeg/fpgui/tree/develop>



  


## Contents

  * 1 Installing fpGUI:
  * 2 Compile fpGUI code from terminal:
  * 3 Compile fpGUI code from Lazarus:
  * 4 License
  * 5 Support
  * 6 Screenshots
  * 7 See also



## Installing fpGUI:

Go to fpGUI site: 

Last stable version is in the 'master' branch =>

  * <https://github.com/graemeg/fpgui/>



  
The 'develop' branch contains the latest changes (and normally very stable too) =>

  * <https://github.com/graemeg/fpgui/tree/develop>



  
-Click on Download ZIP button, on right side... 

-Unzip it. 

  


## Compile fpGUI code from terminal:

You may use a extrafpc.cfg file and @extrafpc.cfg as fpc parameter. 

  
=> Example of extrafpc.cfg file (change fpGUI_dir according of your installation and save extrafpc.cfg in same directory as application-code): 

  

    
    
    -Fi/fpGUI_dir/src/
    -Fi/fpGUI_dir/src/corelib/
    -Fi/fpGUI_dir/src/corelib/x11/
    -Fu/fpGUI_dir/src/
    -Fu/fpGUI_dir/src/corelib/
    -Fu/fpGUI_dir/src/gui/
    -Fu/fpGUI_dir/src/corelib/x11/
    -FUunits/
    -FE./
    

  
And you may compile your fpGUI application like that :=>

> **fpc myfpguiapp.pas @extrafpc.cfg**

  


## Compile fpGUI code from Lazarus:

Configure Lazarus for hosting pure fpGUI applications: =>

    \- in Package, => Open Package (.lpk)

Choose: 

    \- for Windows : <fpgui>/src/corelib/gdi/fpgui_toolkit.lpk

    \- for Linux/FreeBSD/OSX : <fpgui>/src/corelib/x11/fpgui_toolkit.lpk

Compile the package. 

Now you may compile pure fpGUI applications with Lazarus. 

## License

fpGUI uses the LGPL v2 license with a static linking exception - the same as the Free Pascal Compiler's RTL. 

## Support

A dedicated support newsgroup exists for fpGUI Toolkit. Connection details are as follows: 

| Details   
---|---  
**NNTP Server** | geldenhuys.co.uk   
**Port** | 119   
**Group** | fpgui.support   
  
Any News Client (eg: Mozilla Thunderbird, [XanaNews](<https://github.com/graemeg/xananews/releases>), Opera Mail etc) can be used to connect to the news group. This is by far the best option and gives you the freedom to use your preferred news client software. 

In a pinch, there is also a HTML webnews interface. This interface has some limitations (eg: attachments), but is good enough to read and reply to messages when on the go via a web browser (smartphone or desktop). To access the HTML interface, visit the following URL: [<http://geldenhuys.co.uk/webnews/>] 

## Screenshots

This is a small sample of what the fpGUI Toolkit's new 2D graphics engine can do. Full sub-pixel accuracy, anti-aliased line drawing, anti-aliased text, alpha blending, fully customisable gradient and dash-line generator, gamma support etc. The 2D graphics engine is also fully implemented in Pascal, so no external libraries are required, and makes it very portable to other platforms. 

[![](https://wiki.freepascal.org/images/4/43/fpGUI_Agg-powered.png)](</File:fpGUI_Agg-powered.png>)

A demo application showing some AggPas rendering capabilities, now built into fpGUI Toolkit.

[![](https://wiki.freepascal.org/images/6/68/fpgui_plastic_medium_gray_theme.png)](</File:fpgui_plastic_medium_gray_theme.png>)

A sample application showing one of the seven built-in themes.

More of fpGUI's built-in themes can be seen by visiting [this URL](<http://geldenhuys.co.uk/~graemeg/themes/start.html>). 

## See also

  * [fpGUI Toolkit homepage](<http://fpgui.sourceforge.net>)
  * [fpGUI Interface](<fpGUI_Interface.md> "fpGUI Interface") for Lazarus LCL
  * [**Easy fpGUI**](<http://www.turbocontrol.com/easyfpgui.htm>) The easy way to try fpGUI and Free Pascal! Simply unpack the archive and you have a fully working FPC and fpGUI environment.

---

_Source: [https://wiki.freepascal.org/fpGUI](https://web.archive.org/web/20220128045929/https://wiki.freepascal.org/fpGUI)_
