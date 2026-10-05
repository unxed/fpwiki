# iPhone/iPod development

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<../Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 Installation
    * 1.1 iPhone SDK 3.0
    * 1.2 Xcode 4, iOS SDK 5.0, Objective-Pascal
  * 2 Apple's Licence limitations
    * 2.1 Signing up to the Apple Developer Program
    * 2.2 Pascal headers required for developing
  * 3 Technical Development Limitations
    * 3.1 iPhone Simulator is not iPhone
  * 4 Compiling and running pure Pascal applications on an iPhone/iPod Touch
    * 4.1 Using only Xcode
    * 4.2 Combining command-line compilation and Xcode
  * 5 Compiling and running pure Pascal applications on iPhone simulator
  * 6 Building an ARM Cross-compiler
  * 7 iOS 8.0 and later
  * 8 More information
  * 9 See Also
  * 10 External Links



## Installation

Requirements: 

  * Intel Mac (iPhone SDK requirement)
  * Leopard 10.5.5 or later (iPhone SDK requirement)
  * iPhone SDK 3.x or later installed (tested with 2.2 - 5.1). **Warning:** there is a bug in the iPhone SDK 3.0 linker. See below for more information.



Some features: 

  * Simulator support
  * the Xcode template transparently supports (requires, actually) integrating Pascal and Objective-C source files into a single application
  * Pascal translation of the OpenGL ES 1.1 C-header from the SDK (this one we can distribute, because it's licensed under an SGI open source license), available in the gles11 unit



Please read the entire ReadMe.rtf on the installer disk image before asking for help. It contains quite a bit of information on things to watch out for (especially when using the Simulator), and how to change the included template when adding extra Pascal source files. 

Note that unlike other FPC releases, this installer will install the FPC sources and a script, and will then launch this script in order to compile FPC (after you tell it where you installed the iPhone SDK, so it can generate a Pascal version of some required C headers). This modus operandi is necessary to comply with the iPhone SDK agreement's stipulation that no derived works of the SDK may be distributed. If the script aborts for some reason, you can find it at `/Developer/FreePascalCompiler/2.6.0/InstallScript/finish_fpc_iphone_install.command` to relaunch it. You can download it from the official [download page](<http://www.freepascal.org/down/i386/macosx.var>). 

If you encounter problems with the script that is launched when the installer finishes the installation, please always provide the entire contents of the Terminal window in which the script was executed when asking for help. 

(Xcode 4 compatibility: the Xcode templates only work in Xcode 3 only, not in Xcode 4.) 

### iPhone SDK 3.0

The linker included with the iPhone SDK 3.0 contains a bug that is triggered by the Xcode template included with FPC. You can work around this bug by adding -Wl,-no_order_inits to the "Other Linker" flags in the Build settings of the project. This is already done for the Xcode templates included in the 2.4.0rc1 download above, but you may have to do this manually if you are using a project based on a template from a previous snapshot. 

The linker bug has been reported to Apple ([rdar://problem/7120987](<http://openradar.appspot.com/7120987>)). This problem has been fixed in the Xcode 4 linker. 

### Xcode 4, iOS SDK 5.0, Objective-Pascal

For Xcode 4 templates and programming in Objective-Pascal on iOS, see <http://dl.dropbox.com/u/28343282/ObjP/index.html>

## Apple's Licence limitations

### Signing up to the Apple Developer Program

You will eventually need to sign up to Apple's iPhone Developer program ($99/year) to distribute your programs through the Apple store. This applies to any iPhone application, compiled with FPC or not. 

You _do not need to pay the fee_ to run applications on your own physical iPhone device. A free developer account will be enough for this, if you use the new XCode versions. 

Without a developer certificate, you can only run programs on the simulator. (Possibly you can also workaround this if you have a jailbroken iPhone/iPod Touch, but in that case you are on your own.) 

Read more about Apple's iPhone development program here: <http://developer.apple.com/iphone/program/>

### Pascal headers required for developing

It is against the iPhone SDK agreement to distribute derivative works of this SDK, and this includes Pascal translations of framework headers. So it would be quite a challenge (if possible at all) to create and distribute the sources of a pure Pascal program with a GUI that does not violate the iPhone SDK agreement terms at this time. See the ReadMe.rtf (included in the .dmg above) file for more information 

## Technical Development Limitations

At this time is not known whether and if so how it is possible to copy an application to an iPhone/iPod Touch and to execute it without using Xcode. It's of course possible to write the application using any IDE available (Lazarus, Lightweight IDE, etc) and it should also be possible to make any of them work with the Simulator. However, any application has to be built by Xcode for it to be uploaded to the Device and to debug it. 

### iPhone Simulator is not iPhone

The iPhone Simulator makes the iPhone OS API in available to regular Mac applications, thereby mimicking the environment that the devices provide. By allowing you to run your applications in macOS, the simulator provides a way to quickly test your application’s functionality without the need for an actual device. However, running applications on the simulator is not the same as running them on devices. 

The simulator uses macOS versions of the low-level iPhone OS frameworks instead of the versions that run on devices. In general, the simulator is a good tool for performing initial testing of your application. Keep however in mind that, because the simulator does not emulate device functionality, you must always perform final testing and performance analysis of your application on actual devices. The main reason is that programs running under the simulator can also transparently use any other frameworks/functionality used by the host macOS operating system, which may not be available on the device. 

## Compiling and running pure Pascal applications on an iPhone/iPod Touch

Plain command line applications work fine on the iPhone/iPod Touch, except that the output will only be visible in the gdb console (if run or debugged from Xcode; press cmd-shift-r, or select Run->Console) or in the device's console log (if started from the iPhone/iPod Touch itself). In case of GUI applications, you will probably require non-distributable header translations due to licensing restrictions as outlined above. You can however still use your own private Pascal header translations (possible generated by an automated tool). 

#### Using only Xcode

  1. Create a new project based on the template
  2. Remove all files except for *Prefix.pch, Info.plist, Frameworks and Products from the project
  3. Add your own Pascal sources (Project->Add to Project...)
  4. Double-click on the project icon at the top left in the Xcode file list, go to the `Build` tab, and change the `FPC_MAIN_FILE` setting to the name of the test program you added (for all configurations).
  5. Build the program



#### Combining command-line compilation and Xcode

If you compile a program on the command line (for Darwin/ARM) and copy it over the built binary in an Xcode project, then if you "run" the program in Xcode, it will code sign and run the binary that you copied over rather than the original program (just make sure that the binary in Xcode has been compiled recently so there are no changes that trigger Xcode to relink it). You can do this using a project based on the default template, without changing anything to the project. 

You can find the Xcode binary's location by opening the `Products` folder in the project overview, right-clicking (ctrl-clicking) on the .app file and selecting "Reveal in Finder". The binary's location is then in program.app/Contents/MacOS (replace `program.app` with the actual name of the folder selected in the finder). Just overwrite whatever file is in this `MacOS` folder with the program you compiled (e.g. using the command line `cp` program). 

To compile for Darwin/ARM on the command line (replace "Developer" with the directory in which you installed the SDK): 

**for FPC 2.6.0:**
    
    
     ppcarm -Cparmv6 -Cfvfpv2 -FD/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin -XR/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS4.2.sdk program.pas
    

for iPhoneSimulator ( OS_TARGET has been introduced) 
    
    
     ppc386 -Tiphonesim  -XR/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator4.2.sdk program.pas
    

**prior to FPC 2.6.0:**
    
    
     ppcarm -FD/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin -XR/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS3.0.sdk program.pas
    

for iPhoneSimulator 
    
    
     ppc386  -XR/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator4.2.sdk program.pas
    

## Compiling and running pure Pascal applications on iPhone simulator

Compiling and linking requires a regular ppc386 compiler. There is little difference between an iPhone Simulator and a regular macOS application, except that iPhoneSim applications are linked to different frameworks compared to regular macOS applications. 

Starting with FPC 2.6.0 **iphonesim** target has been introduced. For SDKs 3.2 and higher fpc packages needs to be compiled for iphonesim target ([see #17470](<http://bugs.freepascal.org/view.php?id=17470>)). Reasoning: while FPC compile is still pretty much the same (i386 target), different Objective-C ABI is used. For darwin-i386 FPC is using ObjC1 (to be compatible with OSX 10.4 and earlier), while iphonesim-i386 required ObjC2 only. (It seems like previous versions of iPhoneSim SDK 2.x and 3.0 allows ObjC1) 

Some frameworks are iPhone-specific (UIKit), other frameworks exist for both macOS and the iOS (CoreFoundation, CoreGraphics, etc). The iPhoneSim SDK provides its own versions of these frameworks. In order to ensure that the SDK's versions of these frameworks are used, you have to to tell the compiler to use appropriate SDK (just like when compiling applications for a real device): 
    
    
     fpc simtest.pas -XR/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator3.0.sdk
    

where _/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator2.0.sdk_ is the root of an iPhoneSim SDK folder. This directory can vary depending on where you have installed the iPhone SDK. 

There are a at least 3 versions of iPhone SDK available: 2.2.1, 3.0, 3.1. Depending on the version you need, the path changes: 

  * SDK version 2.2.1: _/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator2.2.1.sdk_
  * SDK version 3.0: _/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator3.0.sdk_



etc. 

## Building an ARM Cross-compiler

The downloadable FPC/iPhone package contains a _ppcarm_ cross-compiler to build iPhone applications. Should you wish to compile a newer version of the compiler and units from the latest svn sources later on, you can follow these steps: 

**Requirements:** The iOS SDK must be installed via a standard Xcode developer tools installation and the path to the SDK must contain no spaces. 

1) copy _/Developer/FreePascalCompiler/2.6.0/Source/rtl/darwin/arm/sig_cpu.inc_ to the _rtl/darwin/arm_ directory of your svn checkout (that file was generated when you installed the FPC iPhone SDK integration kit) 

2) in the top level fpc directory of your checkout, execute the following commands (in the first line, replace _/Developer_ with the name of the actual directory where you installed the iPhone SDK, and in the second like replace _iPhoneOS4.2.sdk_ with the actual SDK you want to target) 
    
    
     export IPHONEPLATFORMBASEDIR=/Developer/Platforms/iPhoneOS.platform/Developer
     make FPC=/usr/local/lib/fpc/2.6/0/ppc386 CPU_TARGET=arm CROSSOPT="-FD${IPHONEPLATFORMBASEDIR}/usr/bin -XR${IPHONEPLATFORMBASEDIR}/SDKs/iPhoneOS4.2.sdk/ -ap -Cfvfpv2" all
    

3) to install it afterwards (again, replace the SDK path as appropriate): 
    
    
     sudo make FPC=`pwd`/compiler/ppcrossarm OPT="-ap" CPU_TARGET=arm CROSSOPT="-FD${IPHONEPLATFORMBASEDIR}/usr/bin -XR${IPHONEPLATFORMBASEDIR}/SDKs/iPhoneOS4.2.sdk/ -ap" install CROSSINSTALL=1
     sudo ln -sf ../lib/fpc/2.5.1/ppcrossarm /usr/local/bin/ppcarm
    

(possibly with an INSTALL_PREFIX=xyz at the end of the "sudo make" line to install it under "xyz" rather than under /usr/local -- in that case, also adjust the "ln" command afterwards) 

## iOS 8.0 and later

Apple requires everyone to use 64-bit applications for iOS 8.0 (and later). Also iPhone Simulator for iOS 8.0 is 64-bit only. 

Whenever you compile a project you'll need to use the proper SDK and proper bin utilities. Note that for iPhone Simulator, there's also its own set of bin utilities, not compatible with default macOS bin utilities. 

Example 
    
    
    export SDK_PATH=/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk 
    export BIN_PATH=/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/usr/bin/
     
    ppcx64 -Tiphonesim -Px86_64 -Scghi -O1 -va -XR$SDK_PATH -FD$BIN_PATH iphoneproj.pas 
    

Note that using "-va" parameter (show all messages) which helps to know whether you're using the right path for bin utilities and SDK. The desired result is that ctrl.o library and ld tool are found in a proper target platform, similar to shown below. 
    
    
    [0.437] Linking iphoneproj
    [0.481] Searching file /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/usr/lib/crt1.o... found
    [0.481] Searching file /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/usr/bin/ld... found
    

If the path is not found then FPC will fallback to system-wide defaults (macOS target) and the linking is likely to fail. 

## More information

  * [Simple iPhone example using FreePascal and SDL](<http://freeze-dev.de/blog/?p=492>)



## See Also

  * [iPhone headers translator](<../iPhone_headers_translator.md> "iPhone headers translator")



## External Links

  * [Simon Choi's Basic Tutorial for Clean Installation for iOS 8.2](<https://blog.naver.com/simonsayz/220305479793>)
  * [Simon Choi's Basic Tutorial for Clean Installation for iOS 11.2.5](<https://blog.naver.com/simonsayz/221209784815>)
  * [The primary Apple frameworks headers parser (used for FPC package)](<https://github.com/genericptr/Framework-Parser>)
  * [Tips for Castle game engine project compilation for iOS](<https://github.com/castle-engine/castle-engine/wiki/iOS>)

---

_Source: [https://wiki.freepascal.org/iPhone/iPod_development](https://web.archive.org/web/20250114071248/https://wiki.freepascal.org/iPhone/iPod_development)_
