# fpbrowser

│ **English (en)** │

fpbrowser is a web browser written in Lazarus. 

## Contents

  * 1 Screenshots
  * 2 Rendering Engines
  * 3 Modules
    * 3.1 mod_braille
  * 4 Download
  * 5 See Also



## Screenshots

[![fpbrowser mac.png](https://wiki.freepascal.org/images/3/3a/fpbrowser_mac.png)](</File:fpbrowser_mac.png>)

## Rendering Engines

The source code of fpbrowser allows selecting the used rendering engine. Currently there are 2 options available: TurboPower iPro and THTMLPort., Currently TurboPower is utilized by default. 

  * [TurboPower iPro](<Webbrowser.md> "Webbrowser") <\- This is the default one utilized
  * [THtmlPort](<THtmlPort.md> "THtmlPort") <\- This shows the HTML a bit better, but it has some problems while compiling, it seems to hit a FPC bug which prevents building sometimes and is very annoying



Also interesting is this wiki page which lists possible rendering engines: [Webbrowser](<Webbrowser.md> "Webbrowser")

## Modules

FPBrowser support modules, which are plugins which can change the behavior of the browser. 

### mod_braille

This module converts the text of websites in Braille so that people can practice their Braille skills. 

## Download

You can download the source using Subversion: 
    
    
     svn co <https://svn.code.sf.net/p/lazarus-ccr/svn/applications/fpbrowser> fpbrowser
    

And it requires Synapse: 
    
    
    svn co <https://synalist.svn.sourceforge.net/svnroot/synalist/trunk/> synapse
    

## See Also

  * [THtmlPort](<THtmlPort.md> "THtmlPort")
  * [Free Pascal Application Suite](<Free_Pascal_Application_Suite.md> "Free Pascal Application Suite")

---

_Source: [https://wiki.freepascal.org/fpbrowser](https://web.archive.org/web/20250417145904/https://wiki.freepascal.org/fpbrowser)_
