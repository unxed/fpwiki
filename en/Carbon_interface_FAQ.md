# Carbon interface FAQ

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[日本語 (ja)](</Carbon_interface_FAQ/ja> "Carbon interface FAQ/ja")** │    
****

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The 32 bit Carbon interface is no longer under development since Apple removed all support for it and the ability to run 32 bit software in the release of macOS 10.15 Catalina in 2019. You should now use the 64 bit [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface").

This is a list of frequently asked questions and problems about Carbon interface. 

## Contents

  * 1 What's the state of Carbon interface?
  * 2 What's the state of Lazarus IDE under Carbon interface?
  * 3 When I execute application compiled for Carbon interface, it can't be focused
  * 4 I got a linking error when building Carbon application
  * 5 How can I rebuild LCL or Lazarus IDE for Carbon interface via terminal?
  * 6 I get error "make: -iVSPTPSOTO: Command not found" when I try to rebuild LCL or Lazarus IDE for Carbon interface via terminal.
  * 7 Unknown Stabs
    * 7.1 Dwarf issues
  * 8 Why my project doesn't work on earlier operating system versions?
    * 8.1 User friendly OS version requirement
    * 8.2 Which is the minimum macOS version required by LCL-Carbon?



## What's the state of Carbon interface?

You can find out current status at [Roadmap#Status of features on each widgetset](<Roadmap.md> "Roadmap") and [Roadmap#Status of components on each widgetset](<Roadmap.md> "Roadmap"). You should also look at [Carbon interface internals#Compatibility issues](<Carbon_interface_internals.md> "Carbon interface internals"). 

## What's the state of Lazarus IDE under Carbon interface?

The Lazarus IDE with a Carbon interface is as good or better than others. You can still help by reporting bugs at [Carbon interface internals#Carbon IDE Bugs](<Carbon_interface_internals.md> "Carbon interface internals"). 

## When I execute application compiled for Carbon interface, it can't be focused

You have to execute Carbon application via Application Bundle. See [Carbon Interface#Creating the Apple resource files](<Carbon_Interface.md> "Carbon Interface") how to create one. 

## I got a linking error when building Carbon application

You have forgotten to pass options to the linker to use Carbon Framework. See [Carbon Interface#Compiler Options](<Carbon_Interface.md> "Carbon Interface"). 

## How can I rebuild LCL or Lazarus IDE for Carbon interface via terminal?

You can use the following command in Lazarus directory: 

  * for LCL:


    
    
    make lcl LCL_PLATFORM=carbon
    

  * for Lazarus IDE:


    
    
    make all LCL_PLATFORM=carbon OPT="-k-framework -kCarbon -k-framework -kOpenGL"
    

under Leopard (10.5.x): 
    
    
    make all LCL_PLATFORM=carbon OPT="-k-framework -kCarbon -k-framework -kOpenGL -k'-dylib_file' \
        -k'/System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGL.dylib:/System/Library/Frameworks/OpenGL.framework/Versions/A/Libraries/libGL.dylib'"
    

## I get error "make: -iVSPTPSOTO: Command not found" when I try to rebuild LCL or Lazarus IDE for Carbon interface via terminal.

The reason of this error is, that Free Pascal Compiler executables are not found in search path. You must add their path to the terminal profile, which is located at your home directory: 

  * for Bash terminal in ".profile" file (e.g. use command "open -e ~/.profile" in terminal). At the end of file add this line:


    
    
    export PATH=$PATH:/usr/local/bin
    

  * for tcsh terminal in ".tcshrc" or ".cshrc" or ".login" file (e.g. use command "open -e ~/.cshrc" in terminal). At the end of file add this line:


    
    
    append_path PATH /usr/local/bin
    

, where "/usr/local/bin" is path to FPC executables. Save the result. It should work after terminal restart. 

## Unknown Stabs

The Unknown Stabs warnings are caused by a bug in the Leopard 10.5+ Linker and most Lazarus users can simply ignore them and they don't cause any harm in the software execution. 

<http://www.mail-archive.com/fpc-pascal@lists.freepascal.org/msg13235.html>

That's because it also has to be partly fixed in the linker (which I didn't realise when I posted that message). The unknown stabs warnings (they're not errors afaik) are caused by stabs for local constants in functions/procedures (the linker interprets them as function stabs, because they have a very similar format and gcc does not generate stabs for constants). 

If you want, you can download a patched linker from <http://trappist.elis.ugent.be/~jmaebe/cctools/> (extract it to some directory, add a symbolic link to /usr/bin/as in the same directory, and point FPC to it using the -FD command line switch). That linker corresponds to the one shipped with Xcode 3.0 (+ the patches in that directory). 

But since end users most likely won't have that patched linker (and because stabs has been deprecated by Apple), you may want to switch to dwarf instead on Leopard 10.5+ 

### Dwarf issues

Compiling a project with -gw3 might crash the linker. 

-gw3 crashes the linker because of a bug in the linker. It can be worked around by setting the version number of the DWARF debug info to 2 even though version 3 is generated, but that's a hack. 

-gw2 does add debug information, just try debugging. On macOS, DWARF debug information however remains in the object files and is not added to the main binary by the linker (to speed up linking). You can compile with -Xg to collect all DWARF debug information for an application into a .dSYM bundle using dsymutil. 

Note that Apple's gdb crashes if you try to debug Lazarus after compiling it with -Xg, so that's not a good idea there. 

## Why my project doesn't work on earlier operating system versions?

It's common situation when a project you've compiled works on your operating system version, but not on earlier ones. For example, you're developing using version 10.6. The project runs fine for you, but not for your user using version 10.5. If you look to the console output you'd probably see the error like (or similar): 
    
    
    /Applications/ProjApp.app/Contents/MacOS/projapp ; exit; dyld: unknown
    required load command 0x80000022
    Trace/BPT trap
    logout
    

It's caused, because you have compiled the project **without earlier OS version compatibility**. To add support for it, you need to specify the linker option (Project->Project Options->Linking->Options: 
    
    
    -macosx_version_min 10.5
    

"Pass Options to the Linker (Delimiter is space)" must be checked. 

If you compile for 10.4 target, you should specify the 10.4 minimal version, and so on. 

Remember, if your installed Xcode doesn't contain the backward compatibility SDK, the project linking would probably fail. You can always obtain the required SDKs from the free Apple Xcode downloads. 

You can find more linkers options at [ld man page](<http://developer.apple.com/mac/library/documentation/Darwin/Reference/ManPages/man1/ld.1.html>)

### User friendly OS version requirement

If your application requires a specific OS version and you don't want your app just to crash, you should provide the OS requirement version in bundle's Info.plist. 

### Which is the minimum macOS version required by LCL-Carbon?

This information is documented here: [Minimum Toolkit Versions](<LCL_Internals.md> "LCL Internals")

---

_Source: [https://wiki.freepascal.org/Carbon_interface_FAQ](https://web.archive.org/web/20241206065230/https://wiki.freepascal.org/Carbon_interface_FAQ)_
