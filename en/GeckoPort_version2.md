# GeckoPort version2

│ **English (en)** │

## Contents

  * 1 About
  * 2 Screenshots
  * 3 License
  * 4 Installation
  * 5 Development
  * 6 To Do
    * 6.1 Win32 widgetset
    * 6.2 Carbon widgetset
    * 6.3 Cocoa widgetset
    * 6.4 GTK2 widgetset
  * 7 See also



### About

GeckoPort is a Lazarus/Free Pascal version of Takanori Ito's Gecko SDK for Delphi, including the TGeckoBrowser component. Version 2 is based on automatically generated include-files from the idl files from the Gecko SDK. For this purpose the idltopas utility is used. This way it's earier to follow the fast Gecko-release cycle now gecko-developers do not maintain backwards compatibility anymore. You can find more information about the old version of GeckoPort [here](<GeckoPort.md> "GeckoPort"). 

Current sources are based on Gecko 9, as Gecko 10 has a bug preventing it from being embedded. 

### Screenshots

[![](https://wiki.freepascal.org/images/c/cc/GeckoPort_GBrowser_Win32.jpg)](</File:GeckoPort_GBrowser_Win32.jpg>)

[](</File:GeckoPort_GBrowser_Win32.jpg> "Enlarge")

GBrowser sample app using the Win32 widgetset

### License

MPL 1.1 

### Installation

There is a fpmake-package for GeckoPort in the Free Pascal repository. To install the geckoport fpmake-package on your system: 

  * Make sure that you have [fppkg](<fppkg.md> "fppkg") correctly configured. Also look if the [path of your Lazarus installation](<fppkg.md> "fppkg") is correctly set.
  * Install the fpmake gecko-package for Free Pascal. Give the following command on the command line:


    
    
    fppkg install gecko
    

  * When the installation is complete (re)start Lazarus
  * In Lazarus, choose Package | Install/Uninstall Packages
  * Select GeckoComponents in the list of 'Do not install' packages and click on 'Install selection'.
  * Click 'Save and rebuild IDE' to rebuild and relaunch Lazarus.



To compile a sample app: 

  * Choose File | Open, then select ChromeWin.lpi or GBrowser.lpi (both in the SampleApps subfolder).
  * Choose Run | Run to compile and run the sample app.



Note that you'll need the Gecko libraries installed before you can run one of the sample apps. If necessary, use an installer from here: 
    
    
     <https://developer.mozilla.org/en/XULRunner>
    

### Development

You can download the sources from the GeckoPort from the Lazarus-ccr using Subversion: 
    
    
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/geckoport/version2 [local dir]
    

To install this version, go to the (local) source-directory and use 
    
    
    fppkg install -o -gl
    

The '-o -gl' parameters are to include debug-info. 

### To Do

#### Win32 widgetset

The ChromeWin and GBrowser sample apps work the same as they do with Delphi. 

Currently the code looks for a xulrunner in the following order: 

  * The ParamStr(1). This means the first parameter in command line. This is **temporary** for testing purposes only and it will be removed soon.
  * The application is located in the same folder as the xulrunner.
  * The xulrunner is located in a folder under application executable. Folder must be named "xulrunner".
  * Scan registry looking for a GRE "_Gecko Runtime Environment_ ".
  * Scan registry looking for a Firefox installation.



With Free Pascal 2.4.x and 2.5.1 on Windows, you must disable floating point exceptions as soon as possible in your program. Add the Math unit to your uses clause and add the following statement at the beginning of your program: 
    
    
    SetExceptionMask([exInvalidOp, exDenormalized, exZeroDivide, exOverflow, exUnderflow, exPrecision]);
    

Note that this will disable floating point exceptions in all code, not just in the Gecko libraries. See the sample app .dpr files for examples of how to add this code. 

Firefox runner is now working, tested with Firefox 3.6.12 but the application exe **MUST** be in the same folder as Firefox or you will get a hangup now of them. 

#### Carbon widgetset

A native window must be passed in to initialize the browser component. On OS X, this must be a Cocoa window, otherwise the Carbon widgetset will not work. 

#### Cocoa widgetset

Although the Cocoa widgetset currently has support only for TForm and one or two other controls, the ChromeWin sample app can be run with the Cocoa widgetset since this app uses only TForm. 

Note that the GeckoPort package now uses the new Objective Pascal-based Cocoa widgetset instead of the old PasCocoa-based widgetset. To compile the ObjC-based Cocoa widgetset, you will need a version of FPC that includes Objective Pascal support (for example, version 2.6 or higher, or compiled from FPC SVN trunk). 

You will also need to set the app's widgetset to Cocoa, since the default is Carbon. Or you can compile ChromeWin.lpi with lazbuild and specify the --ws=cocoa switch. See the lb-cw-cocoa.sh script for an example. 

To run the ChromeWin sample app, you need to set DYLD_LIBRARY_PATH so that the XUL library can load the other Gecko libraries it depends on. A simple way to do this is to start ChromeWin.app with the included run-cw-mac.sh script. Note that this script assumes that you have the XUL.framework installed. 

The GBrowser sample app will crash because of the current incomplete state of the Cocoa widgetset. 

#### GTK2 widgetset

On Linux, be sure to set the LD_LIBRARY_PATH environment variable to the xulrunner directory before running one of the sample apps. For example: 
    
    
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/xulrunner-1.9.1.4
    

The Gecko detect code in Linux gets the LD_LIBRARY_PATH environment variable and looks for a suitable XULRunner which path is in it. 

Currently sample apps works in Linux/Ubuntu 9.04 using Lazarus 0.9.29 and fpc 2.5.1 with some paint problems that I think will be resolved soon and seems to be related to GTK2 interface. 

Please report testing success or failure to the GeckoPort maintainer: 
    
    
     "José Mejuto" joshyfun (at) gmail (dot) com (_Current maintainer_)
     MacPgmr (at) fastermac (dot) net (_Original maintainer_)
    

## See also

  * [GeckoPort](<GeckoPort.md> "GeckoPort")

---

_Source: [https://wiki.freepascal.org/GeckoPort_version2](https://web.archive.org/web/20250417142902/https://wiki.freepascal.org/GeckoPort_version2)_
