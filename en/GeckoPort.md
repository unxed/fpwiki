# GeckoPort

│ **English (en)** │

## Contents

  * 1 New version available
  * 2 About
  * 3 Screenshots
  * 4 Download
  * 5 License
  * 6 Installation
  * 7 To Do
    * 7.1 Win32 widgetset
    * 7.2 Carbon widgetset
    * 7.3 Cocoa widgetset
    * 7.4 GTK2 widgetset
  * 8 See also



### New version available

This information is based on the older, version 1 of GeckoPort. For the latest version see [here](<GeckoPort_version2.md> "GeckoPort version2"). 

### About

GeckoPort is a Lazarus/Free Pascal version of Takanori Ito's Gecko SDK for Delphi, including the TGeckoBrowser component. The original unported Delphi source is available from here: 
    
    
     <http://d-gecko.svn.sourceforge.net/viewvc/d-gecko/>
    

### Screenshots

[![](https://wiki.freepascal.org/images/c/cc/GeckoPort_GBrowser_Win32.jpg)](</File:GeckoPort_GBrowser_Win32.jpg>)

[](</File:GeckoPort_GBrowser_Win32.jpg> "Enlarge")

GBrowser sample app using the Win32 widgetset

### Download

You can download the ported source using Subversion: 
    
    
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/geckoport/version1 [local dir]
    

### License

MPL 1.1 

### Installation

To install the GeckoComponents package in Lazarus: 

  * Choose Package | Open package file, then select GeckoComponents.lpk (in the Components subfolder).


  * Click Compile to compile the package.


  * Click Install to rebuild and relaunch Lazarus.



To compile a sample app: 

  * Choose File | Open, then select ChromeWin.lpi or GBrowser.lpi (both in the SampleApps subfolder).


  * Choose Run | Run to compile and run the sample app.



Note that you'll need the Gecko libraries installed before you can run one of the sample apps. If necessary, use an installer from here: 
    
    
     <https://developer.mozilla.org/en/XULRunner>
    

### To Do

#### Win32 widgetset

The ChromeWin and GBrowser sample apps work the same as they do with Delphi. 

Currently the code looks for a xulrunner in the following order: 

  * The ParamStr(1). This means the first parameter in command line. This is **temporary** for testing purposes only and it will be removed soon.
  * The application is located in the same folder as the xulrunner.
  * The xulrunner is located in a folder under application executable. Folder must be named "xulrunner".
  * Scan registry looking for a GRE "_Gecko Runtime Environment_ ".
  * Scan registry looking for a Firefox installation.



~~Xulrunner working up to revision 1.9.1.11 Releases 1.9.2.x and up does not work currently.~~ XULRunner has been tested with 1.9.2.5 since calling mode in fpc 2.5.1 has been fixed and it works. XULRunner has been tested with 2.0.0.8pre and works but as there are some incompatibilities with 1.x.x.x branch in XULRunner the source code must be compiled using "-dXULRUNNER2". 

With Free Pascal 2.4.x and 2.5.1 on Windows, you must disable floating point exceptions as soon as possible in your program. Add the Math unit to your uses clause and add the following statement at the beginning of your program: 
    
    
    SetExceptionMask([exInvalidOp, exDenormalized, exZeroDivide, exOverflow, exUnderflow, exPrecision]);
    

Note that this will disable floating point exceptions in all code, not just in the Gecko libraries. See the sample app .dpr files for examples of how to add this code. 

~~Firefox runner has not been tested as 3.6 does not work at all.~~ Firefox runner is now working, tested with Firefox 3.6.12 but the application exe **MUST** be in the same folder as Firefox or you will get a hangup now of them. 

#### Carbon widgetset

~~Carbon will not work due the lack of safecall calling convention in current fpc (_2.5.1_ ).~~

A native window must be passed in to initialize the browser component. On OS X, this must be a Cocoa window, otherwise the Carbon widgetset will not work. 

#### Cocoa widgetset

~~Cocoa will not work due the lack of safecall calling convention in current fpc (_2.5.1_ ).~~

Although the Cocoa widgetset currently has support only for TForm and one or two other controls, the ChromeWin sample app can be run with the Cocoa widgetset since this app uses only TForm. 

Note that the GeckoPort package now uses the new Objective Pascal-based Cocoa widgetset instead of the old PasCocoa-based widgetset. To compile the ObjC-based Cocoa widgetset, you will need a version of FPC that includes Objective Pascal support (for example, compiled from the FPC SVN trunk, which is currently version 2.5.1). 

You will also need to set the app's widgetset to Cocoa, since the default is Carbon. Or you can compile ChromeWin.lpi with lazbuild and specify the --ws=cocoa switch. See the lb-cw-cocoa.sh script for an example. 

To run the ChromeWin sample app, you need to set DYLD_LIBRARY_PATH so that the XUL library can load the other Gecko libraries it depends on. A simple way to do this is to start ChromeWin.app with the included run-cw-mac.sh script. Note that this script assumes that you have the XUL.framework installed. 

The GBrowser sample app will crash because of the current incomplete state of the Cocoa widgetset. 

#### GTK2 widgetset

~~GTK2 on Linux/Unix will not work due the lack of safecall calling convention in current fpc (_2.5.1_ ).~~ Solved by Joost in fpc 2.5.1 

On Linux, be sure to set the LD_LIBRARY_PATH environment variable to the xulrunner directory before running one of the sample apps. For example: 
    
    
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/xulrunner-1.9.1.4
    

~~For now the code assumes that libxul.so is located in /usr/lib/xulrunner-1.9.1.4. You can edit the code to change this.~~

The Gecko detect code in Linux gets the LD_LIBRARY_PATH environment variable and looks for a suitable XULRunner which path is in it. 

~~Currently the sample apps do not work on Linux, failing in libmozjs.so within the call to libxul.so's XRE_InitEmbedding from nsXRE.pas.~~

Currently sample apps works in Linux/Ubuntu 9.04 using Lazarus 0.9.29 and fpc 2.5.1 with some paint problems that I think will be resolved soon and seems to be related to GTK2 interface. 

Please report testing success or failure to the GeckoPort maintainer: 
    
    
     "José Mejuto" joshyfun (at) gmail (dot) com (_Current maintainer_)
     MacPgmr (at) fastermac (dot) net (_Original maintainer_)
    

## See also

  * [GeckoPort version2](<GeckoPort_version2.md> "GeckoPort version2")
  * [A detailed Tutorial at LazPlanet](<http://lazplanet.blogspot.com/2013/10/browser-in-lazarus-with-gecko-pt1.html>)

---

_Source: [https://wiki.freepascal.org/GeckoPort](https://web.archive.org/web/20250417155657/https://wiki.freepascal.org/GeckoPort)_
