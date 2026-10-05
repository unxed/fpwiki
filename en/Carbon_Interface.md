# Carbon Interface

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Introduction
  * 2 See also
  * 3 What you need
  * 4 Getting a "carbonproof" Lazarus
    * 4.1 Compiling Carbon interface via Makefile
    * 4.2 Compiling the Lazarus IDE with the Carbon interface via Makefile
    * 4.3 Error ATSUFindFontFromName
  * 5 Compiling the Carbon interface via Lazarus
  * 6 Your first native Carbon app
    * 6.1 Compiler Options
  * 7 Creating the Apple resource files
    * 7.1 Application Bundle
      * 7.1.1 Hiding a macOS app from the Dock
  * 8 Cross-compiling Intel to PowerPC
    * 8.1 IDE Environment configuration
    * 8.2 Project configuration
    * 8.3 Define binary architecture
    * 8.4 Getting PowerPC units
  * 9 Creating Universal binaries
  * 10 Cocoa controls in Carbon applications
    * 10.1 Lazarus 0.9.29 or later
    * 10.2 Lazarus 0.9.28 or earlier
  * 11 Other Interfaces
    * 11.1 Platform specific Tips
    * 11.2 Interface Development Articles



## Introduction

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** With the release of macOS 10.15 Catalina in October 2019, Apple has removed all support for the 32 bit Carbon framework from the operating system in favour of the [64 bit Cocoa framework](<Cocoa_Interface.md> "Cocoa Interface"). The last versions of macOS and Xcode which had the necessary Apple files to compile 32 bit applications was Mojave 10.14.x and Xcode 11.3.1.   
  
Carbon development has been discontinued. The [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") is now the default interface for macOS. Patches however are still accepted to fix compilation when Carbon breaks. There are no core Lazarus developers that still use the Carbon interface. The information below may be outdated. 

Carbon is a macOS native API which descends from the old Mac OS APIs and is available since Puma 10.0. One thing to note, however, is that Apple has restricted Carbon to 32 bit applications only. To write 64 bit applications one should use the Lazarus [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface")

## See also

  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips")
  * [Carbon interface FAQ](<Carbon_interface_FAQ.md> "Carbon interface FAQ") \- A list of frequently asked questions about Carbon interface
  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface



## What you need

The Carbon widgetset is now the default [widgetset](<Widgetset.md> "Widgetset") under macOS. For installation instructions see [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS"). 

  


## Getting a "carbonproof" Lazarus

[![](https://wiki.freepascal.org/images/3/3f/carbon_ide_0_9_25.png)](</File:carbon_ide_0_9_25.png>)

[](</File:carbon_ide_0_9_25.png> "Enlarge")

Carbon IDE version 0.9.25 running on Tiger 10.4

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you installed even a vaguely recent Lazarus version, you can skip this and the next section since Lazarus installers since 0.9.25 include both the Carbon widgetset source and compiled units for the Carbon widgetset.

  * macOS includes a command line svn client if you have installed the [Xcode](<Xcode.md> "Xcode") command line tools. If you want a GUI SVN for macOS: Install the SVN client using [fink](<http://www.finkproject.org/>). SVN clients with a GUI (graphical user interface) are available from [Versiontracker](<http://www.versiontracker.com/macosx/>).


  * Then follow [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS").


  * Start Lazarus. The IDE will start with a new project and an empty form. Save this project under a name of your choice. In the following examples we assume this to be /Users/<yourUsername>/pascal/test/project1.lpi



### Compiling Carbon interface via Makefile

Since 0.9.25 carbon is the default widgetset for macOS, so the below is not needed anymore. 

If your are still using a Lazarus below 0.9.25, you have to type this in Terminal.app: 
    
    
    make lcl LCL_PLATFORM=carbon
    

### Compiling the Lazarus IDE with the Carbon interface via Makefile

This requires Lazarus 0.9.25 or later. 

Type in Terminal.app: 
    
    
    make all LCL_PLATFORM=carbon OPT="-k-framework -kCarbon -k-framework -kOpenGL"
    

On Leopard (Leopard 10.5.x) you should type: 
    
    
    make all LCL_PLATFORM=carbon OPT="-k-framework -kCarbon -k-framework -kOpenGL -k'-dylib_file' \
        -k'/System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGL.dylib:/System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGL.dylib'"
    

As stated on Apple's developer pages: <http://developer.apple.com/qa/qa2007/qa1567.html>

**Important notes about the Carbon IDE**

  * You must run Lazarus via lazarus.app.


  * Make sure your editor is using a mono-font. You can check that on the Editor Options dialog. The "Monaco" font is a good suggestion.



### Error ATSUFindFontFromName

if you're getting an error 
    
    
    carbonproc.pp(563,13) Error: Identifier not found "ATSUFindFontFromName"
    

when compiling a project for macOS using FPC 3.0.4 you need: 

  * either set CPU target explicitly to i386. (FPC 3.0.4 compiles to x86_64 for Darwin target by default. This is done due Apple stopping support for 32-bit target)



    

  * You either set the target in Project options (by switching it from default to i386)
  * or if you compiling from command-line (i.e building an IDE) set up the following parameter to make command


    
    
    CPU_TARGET=i386
    

  * either set LCL target (widgetset) to "[Cocoa](<Cocoa_Interface.md> "Cocoa Interface")"



## Compiling the Carbon interface via Lazarus

We now assume your Lazarus directory is located at /Users/<yourUsername>/pascal/lazarus/ 

  * Start Lazarus.
  * Set Tools -> Options -> Environment -> Files -> Lazarus Directory to /Users/<yourUsername>/pascal/lazarus/
  * Set Tools -> Configure "Build Lazarus"> to



[![BuildLazarusOptionsCarbonIntf.png](https://wiki.freepascal.org/images/7/7c/BuildLazarusOptionsCarbonIntf.png)](</File:BuildLazarusOptionsCarbonIntf.png>)

For 0.9.24 add this to your 'Options': 
    
    
    -k-framework -kCarbon -k-framework -kOpenGL
    

This will prevent unresolved symbols (Carbon-symbols like _ActivateWindow) while linking lazarus. 

  * Tools>Build Lazarus -- This will compile the Carbon Interface and put the .ppu files into /Users/<yourUsername>/pascal/lazarus/lcl/units/powerpc-darwin and /Users/<yourUsername>/pascal/lazarus/lcl/units/powerpc-darwin/carbon



## Your first native Carbon app

### Compiler Options

Set Project > Compiler Options > Paths > LCL Widget Type to _carbon_

You should now be able to compile the project without errors. It will create an executable _project1_ , but you cannot focus it. The reason is that macOS expects some hidden resource files. 

**Note for Leopard 10.5:** As discussed in [this](<http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=viewtopic&t=5174&start=0&postdays=0&postorder=asc&highlight=>) forum topic, on Leopard you have to add the following linker parameters to your project: 
    
    
    -dylib_file '/System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGL.dylib:/System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGL.dylib'
    

(This parameter is located at Project > Compiler Options > Linker > Pass additional options to the linker) 

## Creating the Apple resource files

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Since 0.9.25 Lazarus can create application bundles without compiling this tool on your own. The corresponding button is at Project > Project Options... > Create Application Bundle

The command line tool: 

Open /Users/<yourUserName>/pascal/lazarus/components/macfiles/examples/createmacapplication.lpi in the IDE. Compile. 

Open a Terminal of your choice. Type: 
    
    
    cd /Users/<yourUserName>/pascal/project1/
    /Users/<yourUserName>/pascal/lazarus/components/macfiles/examples/createmacapplication project1
    ln -s ../../../project1 project1.app/Contents/MacOS/project1
    

Now you can start the program from IDE (checked option [Use Application Bundle for running and debugging (darwin only)](<IDE_Window__Project_Options.md> "IDE Window: Project Options")) or via its Finder icon or in the native macOS Terminal via "open project1.app" 

Tip: There is also a script that creates an app bundle for a GTK executable at [macOS Programming_Tips](<macOS_Programming_Tips.md> "macOS Programming Tips"). You can modify it to use with a Carbon executable (take out the 4 instructions that start the executable with X11). A slightly improved version of this script for Carbon apps is also available for downloading from [here](<http://web.fastermac.net/~MacPgmr/Lazarus/>). 

### Application Bundle

See [Application Bundle](<Application_Bundle.md> "Application Bundle"). 

  


#### Hiding a macOS app from the Dock

For information on how to hide a macOS application from the Dock, refer to the article [Hiding a macOS app from the Dock](<Hiding_a_macOS_app_from_the_Dock.md> "Hiding a macOS app from the Dock"). 

## Cross-compiling Intel to PowerPC

With 0.9.26 Lazarus comes with precompiled units for both architectures Intel and PowerPC. Since there are still a lot of Mac PowerPC users, you might want to build an application for both archs, or even create a universal binary. 

### IDE Environment configuration

Before you're able to cross-compile, you must make sure, that the IDE Environment compiler settings are correct. Open Environment->Files, check out "Complier path" setting. 

If you need to cross-compile an application or LCL (using Lazarus), you should define "fpc" binary as a compiler. fpc binary will select the proper compiler (ppc386 or ppcppc), depending on your current target selected. 

[![crossppcidesettings.png](https://wiki.freepascal.org/images/0/01/crossppcidesettings.png)](</File:crossppcidesettings.png>)

Of course, you specify the necessary compiler manually (ppc386 - for Intel, ppcppc - for PowerPC), but using "fpc" is more correct and easier 

### Project configuration

If you are using an Intel Mac and want to compile an application for PowerPC, you need to do the following 

  * Go to projects _Compiler options_ ->_Code_
  * Select _powerpc_ for Target CPU family.



Rebuild the application (if you have compiled an intel application it will be overwritten with powerpc app). 

### Define binary architecture

If you are not sure about the target CPU of the binary, you can use the command-line tool "lipo" to check it. Example: 
    
    
    $ lipo -info project1. 
    Non-fat file: project1 is architecture: ppc
    

### Getting PowerPC units

If you have Lazarus and FPC installed from .dmg file provided at Lazarus sourceforge, then you already should have all necessary units for PowerPC compilation. 

If you don't use the provided .dmg files (i.e. using svn version), you might need to rebuild LCL units for powerpc target as well. 

Open Tools -> Configure "Build Lazarus" -> Advanced Build Options. 

  * Type _powerpc_ into Target CPU field.
  * Select all components, except for IDE and Samples (since there's no need to rebuild them) for building.
  * It's recommended that you check 'Clean all' as well.



[![powerpcbuild.png](https://wiki.freepascal.org/images/4/4c/powerpcbuild.png)](</File:powerpcbuild.png>)

Press Build and wait for building process to complete. After that you need to recompile your project, making sure that the project's compiler project is configured for powerpc. 

LCL building might fail if you don't have RTL/FCL units compiled for the powerpc arch. You'll need RTL/FCL sources to compile them. 

  * Go to the rtl/fcl sources directory (the one where 'rtl' and 'packages' directories are contained')
  * Run the following line from the terminal



(making and installing rtl/fcl files tries to write into /usr/local dir, that might need root access. using sudo might be required) 
    
    
    make clean all install CPU_TARGET=powerpc   
    

  * Wait for the make process to complete, then rebuild the LCL and your project again.



## Creating Universal binaries

Lazarus will create an application tuned for a single CPU, for macOS this is either a PowerPC or Intel CPU. You may want to distribute your application as a 'Universal Binary', allowing users that may have either a PowerPC or Intel computer to use the program. To do this, you need to compile two versions of your application: one with PowerPC as the target CPU and one as Intel. Next, you need to stitch these two executables together using the macOS program "lipo" (installed with the other macOS developer tools required by Lazarus). For example, consider two versions of the application 'myprogram'. One in the 'ppc' folder compiled for PowerPC and one in the 'intel' folder compiled for Intel CPUs. You can then combine these two to a univeral application by running this command: 
    
    
     lipo -create ./ppc/myproj ./intel/myproj -output ./myproj
    

Now, copy the newly created application myproj inside your .app folder. 

## Cocoa controls in Carbon applications

Carbon is considered a legacy API by Apple. This means that newer features and controls in macOS might not be available using Carbon API. Though it doesn't mean, that they cannot be used in Carbon application. It just requires additional work to access them. In C it means using Obj-C, but since Obj-C is not available in Pascal. In Lazarus you have to use the [PasCocoa](<PasCocoa.md> "PasCocoa") library. 

The following capabilities require Cocoa: 

  * [TTrayIcon](<TTrayIcon.md> "TTrayIcon")



To configure Cocoa support in Lazarus, complete the following steps depending on your Lazarus version: 

### Lazarus 0.9.29 or later

In these versions PasCocoa is already installed and configured and LCL-Carbon is compiled with Cocoa support by default, you just need to complete this steps: 

1) If you have previously configured PasCocoa in your /etc/fpc.cfg file, remove this configuration now 

2) Build LCL-Carbon if not already built 

### Lazarus 0.9.28 or earlier

1) Download and configure FPC for the PasCocoa library. 

You'll find the instruction [here](<http://wiki.lazarus.freepascal.org/Cocoa_Interface#Compiling_the_Cocoa_Interfaces>). You only have to to download PasCocoa and configure your fpc.cfg file, as described. (You don't need to compile the PasCocoa samples or the Cocoa widgetset). 

2) Now you need to recompile Carbon LCL with Cocoa support enabled. 

Open Tools -> Configure "Build Lazarus"...->Advanced Build Options 

Select LCL for Build+Clean Add "-dCarbonUseCocoa" to Options. 

[![CarbonUseCocoa.png](https://wiki.freepascal.org/images/6/6d/CarbonUseCocoa.png)](</File:CarbonUseCocoa.png>)

After that click build 

This should rebuild the LCL only. If you finished step 1 (downloading PasCocoa) successfully, then the LCL should compile with no problems. 

After the LCL is rebuilt, you need to rebuild your project. 

Don't forget, if you need Cocoa controls for CPU other than your Mac, you'll need to rebuild the LCL, specifying the proper CPU target. 

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * Carbon Interface \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset

---

_Source: [https://wiki.freepascal.org/Carbon_Interface](https://web.archive.org/web/20241213012557/https://wiki.freepascal.org/Carbon_Interface)_
