# Legacy Information: Installing Lazarus on Mac

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 Compatibility
  * 2 FPC 2.4.4 bug
  * 3 Mac OS X 10.4 Tiger
  * 4 Installing Lazarus 2.0.8, 2.0.10 with FPC 3.2.0 for macOS 10.10 and earlier
  * 5 Installing Lazarus 2.0.8 with FPC 3.2.0 for macOS 10.11+
  * 6 macOS 10.5 Leopard
  * 7 macOS 10.8 Mountain Lion additional steps
  * 8 Resurrecting Subversion (svn) on Catalina, Big Sur and Monterey
  * 9 Installation - PowerPC-based Macs
    * 9.1 Download stable sources
  * 10 Installing gdb
  * 11 Xcode Compatability
    * 11.1 Compatibility with Xcode 5.0 and newer versions (OS X 10.8, 10.9, 10.10 and 10.11)
    * 11.2 Xcode 4.3+ compatibility (Mac OS X 10.7 / OS X 10.8)
    * 11.3 Xcode 3.2.x compatibility (Mac OS X 10.6)
    * 11.4 Xcode 3.1.x compatibility (Mac OS X 10.5)
    * 11.5 Xcode 2.x compatibility (Mac OS X 10.4)



## Compatibility

Not every combination of Lazarus and Free Pascal is compatible with every installation of the Mac operating system. Please refer to the following table in order to find the correct version for your development environment: 

Lazarus Compatibility Matrix (Legacy version) | Lazarus 1.0.0  | Lazarus 1.0.2  | Lazarus 1.0.4  | Lazarus 1.0.6  | Lazarus 1.0.8  | Lazarus 1.0.10  | Lazarus 1.0.12  | Lazarus 1.0.14  | Lazarus 1.2.0   
---|---|---|---|---|---|---|---|---|---  
| Free Pascal (FPC) 2.6.0  |  FPC 2.6.2   
PPC processors   
Mac OS X 10.4 (Tiger)  | Incompatible | Incompatible | N/A | Compatible | Incompatible | Incompatible | Compatible* | Compatible* | Not tested   
Mac OS X 10.5 (Leopard)  | Compatible | Compatible | N/A | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible**   
Intel processors   
Mac OS X 10.4 (Tiger)  | Incompatible | Incompatible | Compatible | Compatible | Incompatible | Incompatible | With hack only | Not tested | Not tested   
Mac OS X 10.5 (Leopard)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Not tested | Not tested   
Mac OS X 10.6 (Snow Leopard)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible   
Mac OS X 10.7 (Lion)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible   
OS X 10.8 (Mountain Lion)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible   
OS X 10.9 (Mavericks)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible* | Compatible* | Compatible*   
OS X 10.10 (Yosemite)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
OS X 10.11 (El Capitan)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS 10.12 (Sierra)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS 10.13 (High Sierra)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS 10.14 (Mojave)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS 10.15 (Catalina)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS 11.0 (Big Sur)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS 12.0 (Monterey Beta)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
| Lazarus 1.2.2  | Lazarus 1.2.4  | Lazarus 1.2.6  | Lazarus 1.4.0  | Lazarus 1.4.2  | Lazarus 1.4.4  | Lazarus 1.6.0  | Lazarus 1.6.2  | Lazarus 1.6.4   
---|---|---|---|---|---|---|---|---|---  
|  FPC 2.6.4  |  FPC 3.0.0  | FPC 3.0.2   
PPC processors   
Mac OS X 10.4 (Tiger)  | Compatible | N/A | Compatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Mac OS X 10.5 (Leopard)  | Compatible | Compatible** | Compatible | Compatible** | Compatible** | Compatible** | Not tested | Not tested | Not tested   
Intel processors   
Mac OS X 10.4 (Tiger)  | Not tested | Not tested | Not tested | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible | Incompatible   
Mac OS X 10.5 (Leopard)  | Compatible | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
Mac OS X 10.6 (Snow Leopard)  | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible | Compatible   
Mac OS X 10.7 (Lion)  | Compatible | Compatible | Compatible | Not tested | Not tested | Not tested | Compatible | Compatible | Compatible   
OS X 10.8 (Mountain Lion)  | Compatible | Compatible | Compatible | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible*   
OS X 10.9 (Mavericks)  | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible*   
OS X 10.10 (Yosemite)  | Not tested | Not tested | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible* | Compatible*   
OS X 10.11 (El Capitan)  | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible* | Compatible* | Compatible* | Compatible*   
macOS 10.12 (Sierra)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible* | Compatible* | Compatible*   
macOS 10.13 (High Sierra)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Compatible   
macOS 10.14 (Mojave)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS 10.15 (Catalina)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS 11.0 (Big Sur)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
macOS 12.0 (Monterey Beta)  | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested | Not tested   
  
* [Restrictions](<GDB_on_OS_X_Mavericks_and_Xcode_5.md> "GDB on OS X Mavericks and Xcode 5") apply to debugging with [GDB](<GDB.md> "GDB"). 

** Not available as pre-built installer. [Compiling from source](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") required. 

## FPC 2.4.4 bug

  * FPC 2.4.4 has a bug. You can not compile the IDE with the range check flag **-Cr**.



## Mac OS X 10.4 Tiger

  * On Mac OS X 10.4 Tiger you have to manually uninstall any previous version before installing a new dmg. Delete the following files and folders: 
    * /Developer/lazarus
    * /Library/Receipts/lazarus.pkg
    * /etc/lazarus
    * /usr/local/bin/lazbuild



## Installing Lazarus 2.0.8, 2.0.10 with FPC 3.2.0 for macOS 10.10 and earlier

  * [Lazarus 2.0.8 only] There is a patch required for Lazarus 2.0.8 so that the Cocoa widgetset can be compiled. See above.


  * [Lazarus 2.0.8 + 2.0.10] There is also a patch needed for FPC 3.2.0. Edit the FPC 3.2.0 source in `../packages/cocoaint/src/CocoaAll.pas` and comment out or remove the CoreImage linking line:


    
    
     unit CocoaAll;
     interface
     
     {$linkframework Cocoa}
     {$linkframework Foundation}
     //{$linkframework CoreImage} **Comment out this line**
     {$linkframework QuartzCore}
     {$linkframework CoreData}
     {$linkframework AppKit}
    

  * To rebuild FPC 3.2.0 with the patch, you need an FPC 3.0.4 binary installation. My `build_320.sh` shell script to rebuild and reinstall FPC 3.2.0 (adjust path for FPC source as required):


    
    
    #!/bin/sh
    cd /usr/local/share/src/fpc-3.2.0/
    svn cleanup --remove-unversioned
    make clean all FPC=/usr/local/lib/fpc/3.0.4/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
    make install FPC=/usr/local/lib/fpc/3.0.4/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

Note that: 

  * if more than one line above is highlighted, all the highlighted content should be on one line;
  * you may need to change the second line depending on where your FPC Source code is installed;
  * you will almost certainly need to run the script by using `sudo`.



Now rebuild Lazarus with this `build_laz.sh` shell script (adjust path for Lazarus source as required): 
    
    
    #!/bin/sh
    cd ~/Documents/Lazarus/
    svn cleanup --remove-unversioned
    make distclean all LCL_PLATFORM=cocoa CPU_TARGET=x86_64 bigide
    

## Installing Lazarus 2.0.8 with FPC 3.2.0 for macOS 10.11+

  * There is a patch needed for Lazarus 2.0.8 so that the Cocoa widgetset can be compiled:


    
    
    Index: lcl/interfaces/cocoa/cocoascrollers.pas
    ===================================================================
    --- lcl/interfaces/cocoa/cocoascrollers.pas     (revision 63408)
    +++ lcl/interfaces/cocoa/cocoascrollers.pas     (revision 63409)
    @@ -31,8 +31,6 @@
       cocoa_extra, CocoaPrivate;
    
     type
    -  TSetDocumentViewType = {$if FPC_FULLVERSION = 30200}id{$else}NSView{$endif};
    -
       { TCocoaScrollView }
    
       TCocoaScrollView = objcclass(NSScrollView)
    @@ -51,7 +49,7 @@
         procedure resetCursorRects; override;
         function lclClientFrame: TRect; override;
         function lclContentView: NSView; override;
    -    procedure setDocumentView(aView:  TSetDocumentViewType); override;
    +    procedure setDocumentView(aView: NSView); override;
         procedure scrollContentViewBoundsChanged(notify: NSNotification); message 'scrollContentViewBoundsChanged:';
         procedure resetScrollRect; message 'resetScrollRect';
    
    @@ -647,7 +645,7 @@
       Result:=documentView;
     end;
    
    -procedure TCocoaScrollView.setDocumentView(aView: TSetDocumentViewType);
    +procedure TCocoaScrollView.setDocumentView(aView: NSView);
     begin
       inherited setDocumentView(aView);
       resetScrollRect;
    

  * After making this patch, recompile Lazarus 2.0.8 from within itself or with this `build_laz.sh` shell script (adjust path for Lazarus source as required):


    
    
    #!/bin/sh
    cd ~/Documents/Lazarus/
    svn cleanup --remove-unversioned
    make distclean all LCL_PLATFORM=cocoa CPU_TARGET=x86_64 bigide
    

## macOS 10.5 Leopard

  * After installing Xcode 3.1 (includes the command line tools) for Leopard, the first issue is that Xcode 3.1 does not come with the Clang compiler.


  *     * The solution is to create/add this to the .fpc.cfg file in your home directory:


    
    
     #include /etc/fpc.cfg ## only add this line if the .fpc.cfg file does not already exist
     -WM10.5
     -Aas-darwin
    

  * The second issue is that the official [Lazarus Mac OS X i386 download](<https://sourceforge.net/projects/lazarus/files/Lazarus%20Mac%20OS%20X%20i386/Lazarus%202.0.10/>) of FPC is FPC 3.0.4 and not 3.2.0. 
    * The solution is to download the source for FPC 3.2.0 to your home directory. Open an Applications > Utilities > Terminal and:


    
    
       svn co <https://svn.freepascal.org/svn/fpc/tags/release_3_2_0> 
    

  * The third issue is that the official [Lazarus Mac OS X i386 download](<https://sourceforge.net/projects/lazarus/files/Lazarus%20Mac%20OS%20X%20i386/Lazarus%202.0.10/>) for Lazarus 2.0.10 was compiled with FPC 3.0.4 and not FPC 3.2.0. 
    * The solution is to download the Lazarus release sources to your home directory. Open an Applications > Utilities > Terminal and:


    
    
       svn co <https://svn.freepascal.org/svn/lazarus/tags/lazarus_2_0_10> 
    

  * Compiling FPC 3.2.0 on Leopard is tricky (thanks to Jonas for supplying the solution). Before compiling FPC 3.2.0, make this change to the source so that it will not attempt to link the missing CoreImage framework. Then compile using the following `build_320.sh` script (enter your password at the prompt for the install to proceed):


    
    
       #!/bin/sh
       cd /Users/$USER/release_3_2_0/
       svn cleanup
       make clean all FPC="/usr/local/lib/fpc/3.0.4/ppc386" OS_TARGET=darwin CPU_TARGET=i386 CPU_SOURCE=i386 OPT="-WM10.5 -Aas-darwin -Xs-" 
       sudo make install OS_TARGET=darwin CPU_TARGET=i386 CPU_SOURCE=i386
    

Note: The highlighted line above should all be on one line. 

  * Compiling Lazarus 2.0.10 is less tricky. Before compiling Lazarus 2.0.10, make  this change to the source so that the "run without debugging" option will work. I use the following `build_laz.sh` script:


    
    
      #!/bin/sh
      cd /Users/$USER/lazarus_2_0_10
      svn cleanup
      make distclean all LCL_PLATFORM=carbon CPU_SOURCE=i386 CPU_TARGET=i386 bigide OPT="-WM10.5 -Aas-darwin -Xs- -dCarbonDontUseCocoa"
    

Note: The highlighted line above should all be on one line. 

  * Finally, launch the newly compiled lazarus.app and adjust these Lazarus settings: 
    * Tools > Options > Environment > Files > Compiler executable: this should be set to **`/usr/local/bin/ppc386`**.
    * Project > Project Options > Custom Options: add **-WM10.5** to the Custom options.
    * Project > Project Options > Config and Target > Target CPU family: set to **i386**.
    * Project > Project Options > Config and Target > Current LCL widgetset: set to **Carbon**.
    * Project > Project Options: in the left pane, at the bottom, **check** "Set compiler options as default".



You should now be able to successfully compile and run the Lazarus default blank form project. 

## macOS 10.8 Mountain Lion additional steps

  * The first issue is that the official Lazarus pkg file downloads will not install on Mountain Lion, giving the error "Lazarus IDE cannot be installed on this disk" being the only disk in the system!


  *     * The solution is to download the Lazarus release sources to your home directory. Open an Applications > Utilities > Terminal and:


    
    
       svn co <https://svn.freepascal.org/svn/lazarus/tags/lazarus_2_0_8>
       =or=
       svn co <https://svn.freepascal.org/svn/lazarus/tags/lazarus_2_0_10>
    

  * Unfortunately the compiler and assembler in the last version of Xcode and its command line tools available for Mountain Lion is too old to compile Lazarus. 
    * The solution is to download the macOS Clang7 binary package from <https://releases.llvm.org/7.0.0/clang+llvm-7.0.0-x86_64-apple-darwin.tar.xz>, uncompress it and install in your home directory.
    * Open an Applications > Utilities > Terminal and:


    
    
       cd /usr/bin
       sudo mv clang clang.OLD
       sudo ln -s /Users/$USER/clang/bin/clang clang
       sudo mv as as.OLD
       sudo ln -s /Users/$USER/clang/bin/as as
    

Now follow the steps above. 

## Resurrecting Subversion (svn) on Catalina, Big Sur and Monterey

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** In August 2021, the Free Pascal Compiler and Lazarus IDE Projects moved their Subversion (svn) repositories to git and now host them on GitLab.com. The Lazarus IDE Project also mirrors their repository on GitHub.com which also allows access to the git repository using Subversion (svn).

Xcode version 11.3.1 (Mojave) was the last version to include the Subversion (svn) utilities. You can however copy the utilities and their associated libraries from a Mojave Mac to Catalina, Big Sur and Monterey on both Intel and ARM64 processor Macs. There are two methods to do this - one uses Subversion from Xcode and the other uses Subversion from the command line tools as follows: 
    
    
    cd /Applications/Xcode.app/Contents/Developer/usr/bin
    scp svn* <remote_destination>
    cd /Applications/Xcode.app/Contents/Developer/usr/lib
    scp libsvn_*.dylib <remote_destination>
    scp libserf*.dylib <remote_destination>
    

OR 
    
    
    cd /Library/Developer/CommandLineTools/usr/bin
    scp svn* <remote_destination>
    cd /Library/Developer/CommandLineTools/usr/lib
    scp libsvn_*.dylib <remote_destination>
    scp libserf*.dylib <remote_destination>
    

## Installation - PowerPC-based Macs

Download the three disk images (.dmg files) for fpc, fpcsrc and lazarus from the following link: 

  * [PowerPC-based Macs](<https://sourceforge.net/projects/lazarus/files/Lazarus%20Mac%20OS%20X%20powerpc/>)



Open up each disk image and install in this order: 

  1. fpc
  2. fpcsrc
  3. Lazarus



After installation the Lazarus application can be found in /Developer/lazarus/, the FPC source files in /usr/local/share/fpcsrc. 

If you receive a "Can't find unit Interfaces used by Project1" error on trying to compile a blank form, check the following settings in Lazarus (should be set by default): 

Environment Options 
    
    
      Lazarus directory: /Developer/lazarus
      Compiler path: /usr/local/bin/ppc386 (PowerPC Macs: /usr/local/bin/ppcppc)
      FPC Source: /usr/local/share/fpcsrc
    

Project | Options 
    
    
      All paths blank
      LCL Widget Type: default (Carbon beta)
    

Project | Inspector 
    
    
      Required Packages
         LCL
    
    

Note - different versions of Lazarus depend on particular versions of the Free Pascal compiler and will not work with other versions. 

Another common problem is that the versions of fpc and fpcsrc are different. 

### Download stable sources

The source code of stable versions is available in the [Lazarus Zip - GZip](<http://sourceforge.net/projects/lazarus/files/Lazarus%20Zip%20_%20GZip/>) archives. This is an option for users, who want to run the latest official Lazarus versions on "unsupported" machines, e.g. PPC-based Macs. Provided that you have installed the latest stable FPC version together with sources, it is sufficient to download and unpack the zip archive. Move it to the desired location (e.g. your /Developer folder), open a terminal window and change the shell's path to this folder (the easiest way to do this is to type "cd " and then to drag the icon of the freshly unpacked Lazarus folder to the position after this command in the Terminal window). If necessary enter <return> and then 
    
    
    []$ make clean bigide
    

and <return> again. Subsequently, FreePascal will use the makefile in this folder to build the Lazarus IDE. This may take a few minutes, but the process is fully automated. Therefore, you might want to look for a cup of coffee, while FPC is working. If the compiler is ready it prints the number of processed lines and the required time to the shell's output. 

The last step is to right-klick on the Lazarus bundle icon and to select "Show Package Contents" from the context menu. Then drag the executable "lazarus" to the folder "Contents/MacOS" of the bundle. That's it. 

## Installing gdb

As of the Lazarus 2.0.0 release this is entirely optional. The IDE now supports LLDB directly. See <http://forum.lazarus-ide.org/index.php/topic,42869.0.html> and <http://wiki.lazarus.freepascal.org/Lazarus_2.0.0_release_notes#LLDB_based_Debugger_.28New.29>

Getting gdb to run can be a bit tedious but is worthwhile in the end if using Lazarus < V2.0 or have some specific requirement for gdb. You probably don't ! 

This step should be done after installing XCode and before installing FPC/Lazarus. 

  1. Install fink or brew and use them to install gdb- [http://wiki.freepascal.org/GDB_on_OS_X_Mavericks_and_Xcode_5](<GDB_on_OS_X_Mavericks_and_Xcode_5.md>)
  2. Self sign the gdb binary using tools already on your Mac - <https://sourceware.org/gdb/wiki/BuildingOnDarwin> This is the tedious part. The instructions are clear except, perhaps they could have mentioned that after you have set the Keychain to ’System’ you need click ‘Create’ and ‘Done’ on the next screen. Then go and find the cert in your overall list of certs to do the next instruction “using the contextual menu…”. Don’t worry about step “2.1.4 Disable starting the debuggee…”, we’ll address that once Lazarus itself is installed and working.
  3. Once Lazarus is installed and fired up, tell it how it should start gdb, that is step 5.



**Note** that as of April 2018, there may be issues with newest gdb and High Sierra. <https://forum.lazarus.freepascal.org/index.php/topic,40874.0.html> _According to<https://stackoverflow.com/questions/49001329/gdb-doesnt-work-on-macos-high-sierra-10-13-3>, gdb 8.1 does not work on High Sierra, but 8.0.1 does._

**Part 2 of gdb install, Tell Lazarus about starting gdb**

Again, this step is not necessary with Lazarus V2.0 and later. See <http://wiki.lazarus.freepascal.org/Lazarus_2.0.0_release_notes#LLDB_based_Debugger_.28New.29> If you do install (and use) gdb, you must tell Lazarus about how to call gdb. Obviously, this step happens after Lazarus is installed. 

OK, now you should be able to start Lazarus, it should display the various parts and paths it needs (including gdb) in the little opening screen. You will get an alert there if you chose to not use a debugger. Proceed to the IDE and we now need to tell Lazarus to tell gdb to not open a shell, as noted by a number of Forum users (<http://forum.lazarus.freepascal.org/index.php/topic,37310.0.html>). Within Lazarus, click Tools > Options > Debugger > General In the “Debugger specific Options” there is a field labeled Debugger_Startup_Options. Type in "--eval-command=set startup-with-shell off" (including the inverted commas) AND press the return key, click OK. Note you cannot copy and paste into this box (##$!). See screen shot. 

  * [![Debug Startup Options 01.png](https://wiki.freepascal.org/images/6/6e/Debug_Startup_Options_01.png)](</File:Debug_Startup_Options_01.png>)




## Xcode Compatability

Information relating to older versions of FPC, Lazarus and Apple Developer Tools 

### Compatibility with Xcode 5.0 and newer versions (OS X 10.8, 10.9, 10.10 and 10.11)

Like Xcode 4.3+, Xcode 5 and subsequent versions no longer install the command line tools by default. These are required by FPC/Lazarus, however. Unlike in Xcode 4, the installation option is no longer accessible in Xcode 5 via the Downloads tab. Additionally, the "Locations" tab of the preferences dialog may be confusing, as it mentions that the command line tools are installed: 

  * [![](https://wiki.freepascal.org/images/c/cf/xcode5_misleading_screenshot.png)](</File:xcode5_misleading_screenshot.png>)

Default location of command line tools in Xcode 5. 

  * [![](https://wiki.freepascal.org/images/7/75/xcode_select.png)](</File:xcode_select.png>)

Installation of command line tools with the xcode-select command part 1... 

  * [![](https://wiki.freepascal.org/images/1/1e/command_line_tools_installation_on_mavericks.png)](</File:command_line_tools_installation_on_mavericks.png>)

...and part 2. 




The first dialog box shows that by default the command line tools are only installed inside the Xcode.app bundle itself. To install them globally, as required by FPC/Lazarus, execute the following terminal command: 
    
    
    xcode-select --install
    

Depending from your Xcode version alternative steps to install the command line developer tools may be to download them directly in the Xcode preferences window (see screenshots above) or to download them from [developer.apple.com](<http://developer.apple.com>) (please take care to install the command line tools that are correct exactly for your Xcode version). 

  * You can compile and install the normal gdb. See [GDB on OS X Mavericks and Xcode 5](<GDB_on_OS_X_Mavericks_and_Xcode_5.md> "GDB on OS X Mavericks and Xcode 5").



### Xcode 4.3+ compatibility (Mac OS X 10.7 / OS X 10.8)

Xcode 4.3 and later no longer install the command line tools by default. These are required by FPC/Lazarus. 

To install them manually, open Xcode, go to Preferences, select "Downloads;" and install the "Command Line Tools". Afterwards, FPC will install and function correctly. 

  * [![install command line tools 1.png](https://wiki.freepascal.org/images/a/a5/install_command_line_tools_1.png)](</File:install_command_line_tools_1.png>)

  * [![install command line tools 2.png](https://wiki.freepascal.org/images/d/dd/install_command_line_tools_2.png)](</File:install_command_line_tools_2.png>)

  * [![install command line tools 3.png](https://wiki.freepascal.org/images/d/d0/install_command_line_tools_3.png)](</File:install_command_line_tools_3.png>)




It will be necessary to reinstall the command line tools after each upgrade of Xcode (especially when it's a new major version, the installer may delete the old copies of the command line tools), and also after switching to a new major version of the system software (idem). You may also download the command line tools from [Apple Developer](<https://developer.apple.com/download/more/?=command%20line%20tools>). 

### Xcode 3.2.x compatibility (Mac OS X 10.6)

FPC 2.6.0: there is an issue when compiling dynamic libraries with FPC under Mac OS X 10.6 due to a bug in the Xcode 3.2.x linker. This bug has been fixed in Xcode 4. You can work around the bug in Xcode 3.2.x by using the -k-no_order_inits command line parameter when compiling a dynamic library. 

### Xcode 3.1.x compatibility (Mac OS X 10.5)

Most versions of Lazarus and FPC are compatible with Xcode 3.1 (see the compatibility matrix for detailed reference). The Xcode integration kit is provided as an optional install to allow for developing with Free Pascal directly from Xcode. It is not required for development with Lazarus, however. 

### Xcode 2.x compatibility (Mac OS X 10.4)

Most versions of Lazarus and FPC are compatible with Xcode 2.3 or newer (see the compatibility matrix for detailed reference). The Xcode integration kit is provided as an optional install to allow for developing with Free Pascal directly from Xcode. It is not required for development with Lazarus, however. Xcode 2.0 was compatible with Lazarus 0.9.x, but it is no longer supported by newer versions of the Lazarus IDE.

---

_Source: [https://wiki.freepascal.org/Legacy_Information%3A_Installing_Lazarus_on_Mac](https://web.archive.org/web/20230130023340/https://wiki.freepascal.org/Legacy_Information%3A_Installing_Lazarus_on_Mac)_
