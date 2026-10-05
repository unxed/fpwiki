# Mac Installation FAQ

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │ 

This page lists solutions to the most frequent problems that may arise during (and after) installation of Lazarus and Free Pascal on macOS. Please read [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") first and pay special attention to the sections that apply to your versions of macOS, Xcode, Lazarus and Free Pascal. 

## Contents

  * 1 Lazarus doesn't run project
  * 2 Compilation aborts with weird messages
  * 3 ld: symbol(s) not found for architecture i386
  * 4 Debugger not found
  * 5 Form not shown after moving to Mac
  * 6 Form not responding to mouse clicks
  * 7 fpcsrc not present
  * 8 Multiple error messages after upgrading Lazarus and/or FPC
  * 9 Unit XY not found
  * 10 Fatal: Cannot find [...] used by [...], incompatible ppu=[filename], package [package name]
  * 11 Error: User defined: only cpu i386 is supported
  * 12 Error: ld: framework not found Cocoa
  * 13 Error ppc1 not found
  * 14 Error: library (X11 or Truetype) not found
  * 15 Application does not accept keyboard input
  * 16 Warning arm64 function not 4-byte aligned
  * 17 Resource compiler "fpcres" not found
  * 18 ld: file not found: /usr/lib/crt1.10.5.o
  * 19 ld: library not found for -lc
  * 20 codesign error when compiling FPC from source
  * 21 Error: Internal error 200609171
  * 22 Lazarus reports success, but there are errors!
  * 23 GUI application menu inaccessible
  * 24 Other questions
  * 25 See also



## Lazarus doesn't run project

_After moving to macOS or after upgrading to a new version of macOS Lazarus is unable to compile and run a project._

**Solution:** Most likely, this behaviour results from missing command line tools. In newer versions of Xcode they are no longer installed by default. You can install them by running 
    
    
     xcode-select --install
    

from the terminal. See also [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") for more details. 

## Compilation aborts with weird messages

_I have correctly installed Lazarus and FPC, but compiling a simple demo program stops with the Free Pascal exit code 256 and the message "Lazarus Panic /bin/sh: clang: command not found"._

**Solution:** This behaviour may arise from a too old Xcode version installed. Generally, you should use the newest Xcode version that is available for your operating system. Lazarus 1.6 requires, e.g., Xcode 3.2.6, or newer. With Xcode 4.3 or newer, you should also install the Xcode command line tools as described above or in the article [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS"). 

## ld: symbol(s) not found for architecture i386

_I am trying to compile a blank project on Catalina immediately after installing FPC and Lazarus but I get this error "ld: symbol(s) not found for architecture i386"._

**Solution:** Go to Lazarus > Tools > Options, if your compiler is set to ppc386 (32 bit), then change it to ppcx64 (64 bit) or fpc (a wrapper that should choose the correct compiler). There is no 32 bit support in Catalina. In later versions of Lazarus, the Lazarus > Tools > Options menu has been moved to the Lazarus > Preferences menu 

## Debugger not found

_I have installed the latest version of Lazarus on a new Mac. My program is built fine, but when I try to run it I get a message that the debugger` /usr/bin/gdb` doesn't exist or isn't runnable._

**Solution:** Since version 5, Xcode does not include the GDB debugger but the LLDB debugger. For recent versions of macOS, go to the Tools > Options | Debugger menu and choose "LLDB debugger (with fpDebug) (Beta)" and set the executable to `/usr/bin/lldb`. In later versions of Lazarus, the Tools > Options menu has been moved to the Lazarus > Preferences menu 

**Solution (legacy):** Xcode v5 no longer includes the GDB debugger. See [GDB on OS X Mavericks or newer and Xcode 5 or newer](<GDB_on_OS_X_Mavericks_or_newer_and_Xcode_5_or_newer.md> "GDB on OS X Mavericks or newer and Xcode 5 or newer") for possible solutions unless you are running a recent version of macOS (in which case, see above). 

## Form not shown after moving to Mac

_My application works well on Windows and Linux, but after changing to Mac my forms are invisible. I tried to re-scan the FPC source directory, and I clicked "Create Bundle", but this didn't solve the problem. Unlike my app, a very simple demo application works well._

**Solution:** This may result from a wrong position setting of your form(s). Your Windows machine may have a multi-monitor setup, so that the forms are outside the visible area of a single-monitor environment, i.e. if your Mac has only one monitor. You should check and correct the top and left properties of your forms in the object inspector. (Of course, this also applies in the other direction, i.e. if you have a multi-monitor Mac and a single-monitor Windows or Linux machine). 

## Form not responding to mouse clicks

_After starting the program the form is visible, but not responding to interaction via the user interface._

**Solution:** Programs for macOS-based systems are more complex than programs for Windows or Linux. They require the existence of an [application bundle](<Application_Bundle.md> "Application Bundle"), a special opaque directory structure, which determines the modalities of interaction with the operating system and the GUI. You may create an application bundle in the [project settings](<IDE_Window__Project_Options.md> "IDE Window: Project Options") or alternatively via shell commands. Make sure that the option [Use Application Bundle for running and debugging (for Darwin)](<IDE_Window__Project_Options.md> "IDE Window: Project Options") is checked. 

## fpcsrc not present

_After installing and first running Lazarus, the welcome window complains that the directory "/usr/local/share/fpcsrc" is not found._

**Solution:** You have to install fpcsrc. This is a Lazarus-specific installer package that comes with your distribution of Lazarus. If you have downloaded Lazarus from SourceForge you find fpcsrc in the same server directory as the Lazarus package. 

## Multiple error messages after upgrading Lazarus and/or FPC

_After upgrading Lazarus and/or Free Pascal to a new major version, trying to compile my code results in multiple error messages._

**Solution:** This behaviour may have multiple reasons. After every major upgrade you should rescan the FPC source directory. If this doesn't help it may be useful to delete the file `fpcdefines.xml` (it is within the ~/.lazarus folder). Additionally, you should check your code for incompatibilities that may result from changes in the compiler (although this is quite rare with code that is not too unusual). See [Category:FPC User Changes by release](</Category:FPC_User_Changes_by_release> "Category:FPC User Changes by release") for intentional changes to the compiler that may break existing code. 

## Unit XY not found

_I have freshly installed a new version of Lazarus on my machine. Trying to compile an application results in the error message "Error: unit not found: XY"._ (XY is a place holder for any unit name.) 

**Solution:** Try to re-scan your FPC source directory via the Lazarus IDE _Tools..._ menu. 

## Fatal: Cannot find [...] used by [...], incompatible ppu=[filename], package [package name]

_When the compiler gives the message "Cannot find A used by B", Lazarus checks what pas/pp/ppu files are in the search path, and if there is a ppu file it inserts "incompatible ppu=filename, package"._

**Explanation:** FPC cannot use this ppu for one of these reasons: 

  * it was compiled with another FPC version;
  * it was compiled with (depends on) some other ppu files that are not in the search path;
  * you misconfigured some search path(s).



**Solutions:** (1) Check that you have the current [Xcode](<Xcode.md> "Xcode") (optional - see [here](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS")) and Xcode command line tools package (mandatory) installed; (2) Lazarus > Menu > Tools > Configure Build Lazarus - Find and check "Clean all" and then Build. If that doesn't work, I'd be tempted to delete both FPC + Lazarus and start again. See [Uninstalling Lazarus and Free Pascal](<Uninstalling_Lazarus_on_macOS.md> "Uninstalling Lazarus on macOS"). 

## Error: User defined: only cpu i386 is supported

_When compiling Lazarus on macOS I receive this error:` carbonbars.pp(16,2) Error: User defined: only cpu i386 is supported `_

**Solution:**
    
    
    make bigide LCL_PLATFORM=cocoa
    

## Error: ld: framework not found Cocoa

_When compiling Lazarus on macOS I receive this error:` Error: ld: framework not found Cocoa`_

**Solution:** You forgot to install the Xcode command line tools or they've since been deleted. See [Installing Xcode Command Line Tools](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") to resolve. You may also need to either: 

(1) re-install FPC **after** installing the Xcode command line tools; or 

(2) tweak the `/private/etc/fpc.cfg` file, after installing the Xcode command line tools, by adding: 
    
    
     -Fl/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/lib
     -XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk
    

so that the compiler can find the macOS frameworks. 

## Error ppc1 not found

_When compiling the Free Pascal Compiler on macOS I receive this error:_
    
    
    /usr/local/bin/ppcx64 -Ur -Xs -O2 -n -Fux86_64 -Fusystems -Fu../rtl/units/x86_64-darwin -Fix86_64 -FEx86_64/bin/x86_64-darwin -FUx86_64/units/x86_64-darwin -dRELEASE    -dx86_64 -dGDB -dBROWSERLOG -Fux86 -Fix86 pp.pas
    /bin/mv -f x86_64/bin/x86_64-darwin/pp ppcx64
    /Applications/Xcode.app/Contents/Developer/usr/bin/make echotime
    Start now 08:06:02
    /bin/rm -f ppc1
    /bin/mv -f ppcx64 ppc1
    /Applications/Xcode.app/Contents/Developer/usr/bin/make 'FPC=/Users/Developer/fpc trunk/compiler/ppc1' 'OLDFPC=' next CYCLELEVEL=2
    Makefile:135: *** Compiler /Users/Developer/fpc trunk/compiler/ppc1 not found.  Stop.
    make[3]: *** [ppc2] Error 2
    make[2]: *** [cycle] Error 2
    make[1]: *** [compiler_cycle] Error 2
    make: *** [build-stamp.x86_64-darwin] Error 2
    

**Solution:** GNU Make does not handle directory names with spaces in them. Rename "fpc trunk" to "fpc_trunk" or similar and it should work without this error. 

## Error: library (X11 or Truetype) not found

_If you are compiling X11 applications (typically using fpGui) FPC needs to know where to find the libX11.dylib and libfreetype.dylib libraries. If FPC cannot find these libraries, you will receive a library not found error during linking._

**Solution:** Depending on your version of macOS or XQuartz these may be found in `/usr/X11/lib` or `/opt/X11/lib` with a symlink from `/usr`. The best solution to this is probably to edit `/etc/fpc.cfg` and add the line ``-Fu/usr/X11/lib``

## Application does not accept keyboard input

_After compiling my application, I cannot enter anything in the edit boxes and the application will not accept any keyboard input._

**Solution:** You forgot to create an [application bundle](<Application_Bundle.md> "Application Bundle"). Lazarus > Project > Project Options > Application - Create Application Bundle. 

## Warning arm64 function not 4-byte aligned

**When compiling on an Apple Silicon M1 processor, I get a "warning arm64 function not 4-byte aligned" for every function.**

**Solution:** That's probably a consequence of using -Os. That setting unconditionally sets procalign (jump align) to 1. That's definitely wrong for AArch64 and a bunch of other architectures. There is no "minimum required alignment" for these settings defined yet anywhere in the compiler, so that will need to be added. In the meantime, do not use -Os to optimize size. 

## Resource compiler "fpcres" not found

**When compiling an application after installing or compiling a new Lazarus version, it fails with the error "Error (9021) Resource compiler "fpcres" not found, switching to external mode** " 

**Solution:** Create a file called `.fpc.cfg` (note the leading dot in the filename) in your home directory and add the lines: 
    
    
    #include /etc/fpc.cfg
    -FD/usr/local/bin
    

If the file already exists it should contain the first of the lines above, so just add the second line. 

## ld: file not found: /usr/lib/crt1.10.5.o

**When compiling FPC from source, linking fails with "ld: file not found: /usr/lib/crt1.10.5.o"**

**Solution:** Add `"OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk"` to your `make` command line - the reason is that when compiling the FPC source, the `fpc.cfg` file, which contains this option, is not used. 

## ld: library not found for -lc

_When compiling FPC from source, I get this "ld: library not found for -lc, An error occurred while linking "_

**Explanation:** The compiler cannot find the operating system SDK frameworks. 

**Solutions:**

  * If you have not installed the command line tools, you need to do so. Note that when you upgrade the operating system, the command line tools are deleted during the upgrade process.


  * If you have installed the command line tools, then you need to tell the compiler where to find the SDK frameworks by adding:



    `OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"`
    to your `make` command line. Note that older versions of the command line tools (eg those for Mountain Lion 10.8.5) did not also install the SDK frameworks in which case you will need to install the latest version of the full Xcode package for your operating system and, for example, adding:
    `OPT="-XR/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.8.sdk/"`
    to your `make` command line.

## codesign error when compiling FPC from source

_When compiling FPC on older versions of macOS, I receive this error:_
    
    
     /usr/bin/codesign --remove-signature ppc3.tmp  
     ppc3.tmp: unsupported type or version of signature
     make[2]: *** [cycle] Error 1
     make[1]: *** [compiler_cycle] Error 2
     make: *** [build-stamp.x86_64-darwin] Error 2
    

**Explanation:** Older versions of the macOS `codesign` command line utility do not have a `--remove-signature` option. 

**Solution:** You can provide a custom codesign command via the CODESIGN=xxx make parameter to your make command line. You could use CODESIGN=/usr/bin/true to work around it. 

## Error: Internal error 200609171

_When compiling my Objective-Pascal delegate function(s)/procdeure(s) the compiler stops with this error: "Error: Internal error 200609171"._

**Explanation:** The internal error comes from using the DWARFv3 (-gw3) information format for debugging. There is a bug in the Free Pascal Compiler up to and including 3.2.2 related to generating debug information for Objective-Pascal with that format. 

**Solution:** Switch to using the DWARFv2 (-gw) debugging information format **OR** use FPC main (development version 3.3.1). 

## Lazarus reports success, but there are errors!

_I compiled my application and although Lazarus says my application compiled successfully, there are a bunch of errors shown. For example -_
    
    
     Compile Project, Target: project1: Success, Errors: 8 
     Error: /Users/trev/prg/LAZARUS/laz_avaudioplayer/lib/x86_64-darwin/unit1.s:107840:10: warning: section "__datacoal_nt" is deprecated
     Error: .section __DATA, __datacoal_nt, coalesced
     Error: /Users/trev/prg/LAZARUS/laz_avaudioplayer/lib/x86_64-darwin/unit1.s:107840:10: note: change section name to "__data"
     Error: .section __DATA, __datacoal_nt, coalesced
     Error: /Users/trev/prg/LAZARUS/laz_avaudioplayer/lib/x86_64-darwin/unit1.s:107883:10: warning: section "__datacoal_nt" is deprecated
     Error: .section __DATA, __datacoal_nt, coalesced
     Error: /Users/trev/prg/LAZARUS/laz_avaudioplayer/lib/x86_64-darwin/unit1.s:107883:10: note: change section name to "__data"
     Error: .section __DATA, __datacoal_nt, coalesced
    

**Explanation:** These are actually harmless warnings from the assembler, but for some reason Lazarus shows them as errors. Just ignore them. 

## GUI application menu inaccessible

_My GUI application menu is not accessible_

**Solution:** All macOS GUI applications must be wrapped in an [Application Bundle](<Application_Bundle.md> "Application Bundle"). 

## Other questions

_I didn't find my issue here._

**Solution:** You might find a possible solution at [the Lazarus and Free Pascal Forum](<http://forum.lazarus.freepascal.org/index.php/board,15.0.html>). Otherwise, you might ask your question there. 

## See also

  * [Lazarus FAQ](<Lazarus_Faq.md> "Lazarus Faq")

---

_Source: [https://wiki.freepascal.org/Mac_Installation_FAQ](https://web.archive.org/web/20240308084949/https://wiki.freepascal.org/Mac_Installation_FAQ)_
