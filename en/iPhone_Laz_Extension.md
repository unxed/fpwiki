# iPhone Laz Extension

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Known Issues:
  * 8 To do:
  * 9 Installation
  * 10 How to use
    * 10.1 Common usage
    * 10.2 iPhone Simulator
    * 10.3 Project to Xcode
  * 11 Examples
    * 11.1 Hello World
    * 11.2 iArkanoid



### About

_iPhone Laz extension_ is Lazarus IDE extension that's designed to simplify iPhone applications development. 

Before you use the extension, it's recommended to learn the page about [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development"). 

The extension does **NOT** provide another LCL WidgetSet (like Carbon or Cocoa) nor requires to use any. 

### Author

Dmitry 'skalogryz' Boyarintsev 

email: skalogryz dot lists at gmail dot com 

### License

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). You can contact the author if the modified LGPL doesn't work with your project licensing. 

### Download

The latest stable release can be found at svn: 
    
    
    svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/iphonelazext>
    

### Change Log

  * Version 0.5 05 Jan 2010
  * Version 0.7 Feb 2013
  * Winter-Spring 2016 
    * added support for reading/writing Xcode project
    * reading/repopulating the list of uploaded files
    * initial support for sim debugging



### Dependencies / System Requirements

  * Lazarus 0.9.29 (SVN trunk) or higher. (for Project options integration)
  * Leopard 10.5 (Intel) + iPhoneSDK/Xcode 3.1 or higher
  * Snow Leopard 10.6 + iPhoneSDK/Xcode
  * FPC 2.4.0 (FPC 2.5.1 (or higher) with Objective-P syntax is recommended to use).
  * Since Apple doesn't provide any tools for development on other than macOS system, the package cannot (should not) be installed and used on Windows or Linux.



Status: _Beta_

### Known Issues:

  * Project launched from Lazarus fails with an exception. It should be launched from the iPhone simulator
  * The project, using iPhoneAll unit, compiled for the simulator must use Smart Linking switch (-XX)
  * When application's title is changed (in Project options) a string is added to the main file


    
    
      Application.Title := 'MyApp';
    

It must be deleted manually 

  * XCode sometimes fails to debug the application on iDevice, though it's loaded successfully. Don't worry. Your application is still on the iDevice and ready to be tested.
  * _todo:_



### To do:

  * Improve debugging abilities



## Installation

In order to develop for iPhone or iPod, you'll need arm FPC compiler with rtl and packages installed in the system. There're 2 ways to do it: built manually via SVN or installed from a special FPC arm package. Both ways are described [here](<iPhone/iPod_development.md> "iPhone/iPod development"). 

You don't need to install ARM compiler or packages, if you're planning to use iPhone simulator only. 

  * Get the package source via svn or download from Sourceforge
  * Open and install the package
  * You will find iPhone options group added to both Project and Environment options



[![projoptions.png](https://wiki.freepascal.org/images/a/ad/projoptions.png)](</File:projoptions.png>)

## How to use

### Common usage

  * Mark a project as iPhone project (using Project menu -> is iPhone project or Project Options -> iPhone -> is Phone project application)



[![iphoneextmenu.png](https://wiki.freepascal.org/images/7/7e/iphoneextmenu.png)](</File:iphoneextmenu.png>)

  * To change the displayed name of the application you must change the application Title in Project Options.


  * You can also change the application icon in the same way



### iPhone Simulator

  * To install the application to iPhone simulator you need simply to build the project (ctrl+F9/cmd+B).



The installation is always performed then you try to build the project. 

  
iPhone simulator apps require special environment, created by the simulator or they will fail with "trace error" if run in pure OSX. To make the simulator Run everytime you're running the application, select in menu "Run->Run parameters..." check "Use launching application" put there 
    
    
    '/Developer/Platforms/iPhoneSimulator.platform/Developer/Applications/iPhone Simulator.app/Contents/MacOS/iPhone Simulator'
    

Note: the location of iPhone simulator.app may vary, depending on the iPhone SDK installation path. 

It's not required, but you'll need to launch iPhoneSim manually otherwise. 

  


  * After you build run the iPhone Simulator, your application should be on the one of the iPhone simulated pages.



[![Iphonesim.png](https://wiki.freepascal.org/images/c/c6/Iphonesim.png)](</File:Iphonesim.png>)

  
It's possible, that iPhone sim also starts the required application (as Xcode does), but this possibility is undocumented by Apple. You can use it on your own risk. Please refer to <http://github.com/jhaynie/iphonesim>

### Project to Xcode

There's not way to install an application to iPhone, but using Xcode (or purchasing it via iTunes). (Jailbreaks have their own way, not used by the package). The package provides an utility to generate an Xcode project from Lazarus project. 

  * Write the program (and test it on the Simulator, if necessary)
  * Select Project -> Update XCode project. If not exists _xcode_ directory is selected there you'll find xcode project. If project exists, it's configuration will be updated



[![findershot.png](https://wiki.freepascal.org/images/1/13/findershot.png)](</File:findershot.png>)

  * Open the project (project1.xcodeproj on the screenshot) . Select iPhone device for as Active SDK.
  * You might need to specify Code identity in Build configuration (please, refer to iPhone Developer program portal)
  * Press Build and Go. This should build the application and install it to iPhone device.



[![xcodeproj.png](https://wiki.freepascal.org/images/9/91/xcodeproj.png)](</File:xcodeproj.png>)

Note: 

  * You'll need arm-darwin FPC compiler installed and configured. Any units or package used, must be compiled for arm-darwin target as well.
  * Created XCode project, is not flexible enough to build the project for iPhone Simulator, so make sure that iPhone OS is used as active SDK.



## Examples

The extension also includes a few sample projects. (see _examples_ dir at the iphonelazext directory) 

### Hello World

The sample requires a objective-p compiler with any version of iPhoneAll unit. 

The example has been written using iPhoneAll of this version (<https://github.com/genericptr/iOS_8_0>). 

The intent of the sample is to show how to go from Lazarus project to Xcode project. 

0\. Install iPhone Laz Extension. Make sure you've FPC cross-compiler prepared. It's better to have it ready for all iOS targets (i386-iphonesim, x86_64-iphonesim, arm-darwin, aarch64-darwin). The compilation script will build both 32 and 64-bit targets. 

1\. The example project in Lazarus 

2\. Open Project Options 

2.1 Compiler Options -> "Paths": specify where iPhoneAll unit is located at "Other unit files (-Fu)" 

2.2 Make sure -dIPHONEALL is present in "Custom Options" (thought it might not be needed for the version of iPhoneAll you're using) 

2.3 in "iPhone Specific" section check the box "is iPhone application project" 

2.4 Close the dialog by hitting "OK" 

3\. in Project menu press "Update Xcode project" 

4\. At the directory with the lazarus project a new "xcode" directory will created. Within the directory there should be "ihelloworld.xcodeproj" generated. Double-click on the bundle so Xcode will start. 

5\. In Xcode. make sure you've a simulator selected as a target device. Hit "Play" icon (top-left side of the window) ( or through menu Product -> Run ) 

5.1 This should start compilation of the project. If no issues would be detected, the simulator will start. As soon as its booted Xcode will install the application and run it. 

6\. An actual device. Plug your device via USB. 

6.1 In "General" tab you should see a "warning sign" with "No identity or provisioning profile". "Fix issue" button should be availabe. 

6.2 Hit the button. The Xcode should get you a certificate, so you'd be able to run the application on your device. 

6.3 Select the device as run target (top-left side of the Xcode window). 

6.4 Clear the project (Product -> Clear) 

6.5 Run the project (Product -> Run) 

The result would look like: 

[![iphonescreenshot.png](https://wiki.freepascal.org/images/2/27/iphonescreenshot.png)](</File:iphonescreenshot.png>)

### iArkanoid

Written from scratch simple arkanoid game UIKit and GoreGraphics games for iPhone/iPod. 

[![iArkShot.png](https://wiki.freepascal.org/images/b/b1/iArkShot.png)](</File:iArkShot.png>)

---

_Source: [https://wiki.freepascal.org/iPhone_Laz_Extension](https://web.archive.org/web/20220816111739/https://wiki.freepascal.org/iPhone_Laz_Extension)_
