# OS X Programming Tips

[![Logo OSX.png](https://wiki.freepascal.org/images/7/7a/Logo_OSX.png)](</File:Logo_OSX.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

**English (en)** | [**日本語 (ja)**](</OS_X_Programming_Tips/ja> "OS X Programming Tips/ja") | [**한국어 (ko)**](</OS_X_Programming_Tips/ko> "OS X Programming Tips/ko")   
****

## Contents

  * 1 Other Interfaces
    * 1.1 Platform specific Tips
    * 1.2 Interfaces Development Articles
  * 2 Cross compiling OSX applications on Linux
  * 3 Choice of Lazarus LCL widgetsets
  * 4 Creating an app bundle for a GTK application
  * 5 Using stdout for logging and debugging
  * 6 Deploying an executable, Compiling under 10.6 for older version of Mac OS X
    * 6.1 FPC 2.6.2 and above
      * 6.1.1 Adding the custom options only when compiling for OS X
    * 6.2 FPC 2.6.1 and below
      * 6.2.1 Adding the custom options only when compiling for OS X
  * 7 Useful tools to download and install
  * 8 Useful commands and tools included with OS X
  * 9 Commonly used Unix commands
  * 10 Mac OS X Libraries
    * 10.1 Using a library in a Mac application
    * 10.2 Using a library installed using fink
  * 11 How to obtain the path to the Bundle
  * 12 How to obtain the Application Support and Preferences paths
  * 13 Accessing system information
  * 14 Creating Universal binaries
  * 15 Code Signing
  * 16 Notarization
  * 17 Locale settings (date & time separator, currency symbol, ...)
    * 17.1 Automatic initialisation based on the System Preferences settings
    * 17.2 Automatic initialisation based on the settings in the Unix layer
    * 17.3 Manual initialisation
  * 18 Apple-specific UI elements
    * 18.1 AppName/Apple main menu item
    * 18.2 Adding an icon to your app bundle
    * 18.3 Removing the app icon from title bars
    * 18.4 Add an Apple help book to your app
    * 18.5 Using preferences in your app
  * 19 See also
  * 20 Links



## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The winapi interface for Windows 95/98/Me/2K/XP/Vista, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon interface for Mac OS X
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa interface for Mac OS X
  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, Mac OS X, Windows, and Linux-based PDAs
  * [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, Mac OS X, Windows, and Linux-based PDAs
  * [GTK1 Interface](<GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, Mac OS X (X11), Windows
  * [GTK2 Interface](<GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, Mac OS X (X11), Windows
  * [GTK3 Interface](<GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, Mac OS X (X11), Windows
  * [fpGUI Interface](<fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * macOS Programming Tips \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interfaces Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## Cross compiling OSX applications on Linux

See [Cross compiling OSX on Linux](<Cross_compiling_OSX_on_Linux.md> "Cross compiling OSX on Linux")

## Choice of Lazarus LCL widgetsets

With OS X there are various widgetsets which can be used with Lazarus. Each has its strengths and weaknesses. 

**Carbon widgetset** (see the [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") page) 

Pros | Cons   
---|---  
All standard LCL controls working | The entire Carbon framework was deprecated by Apple. It is not available in 64 bit Mac OS X applications   
No additional libraries or frameworks to install; uses Carbon framework included with OS X | Only relevant to OS X   
Native OS X look and feel |   
  
**Cocoa widgetset** (see the [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") page) 

Pros | Cons   
---|---  
No additional libraries or frameworks to install; uses the Cocoa framework included with OS X | Some LCL controls not yet working   
Native OS X look and feel | Only relevant to OS X   
  
**GTK widgetset**

Pros | Cons   
---|---  
All standard LCL controls working | Ugly; clunky common dialogs; doesn't look like Mac software   
This widgetset is well tested | Requires X11 and bulky GTK to be installed to run Lazarus or any GUI apps created with Lazarus   
| Have to start Lazarus at command line -- can't double-click Lazarus or place on dock (same with GUI apps) -- although see next topic for how you can add this capability yourself   
  
  
**Qt widgetset** (see the [Qt Interface Mac](<Qt_Interface_Mac.md> "Qt Interface Mac") page) 

Pros | Cons   
---|---  
Native OS X look and feel | Requires the Qt interface framework to be installed to run app, which is rather large   
All standard LCL controls working |   
The Qt widgetset also available for other platforms, so any effort put into developing Qt widgetset benefits multiple platforms |   
The Qt interface provides rich functionality |   
The Qt widgetset is easier to develop than other widgetsets |   
  
## Creating an app bundle for a GTK application

With OS X, you need to create an app bundle for an executable file in order to drop it on the dock or launch it by double-clicking. An app bundle is really just a special folder that can have the same name as the executable, but with an .app extension. Finder doesn't display the .app extension, although you'll see it if you use ls in a Terminal window. 

The Carbon and Qt pages both explain how to create an app bundle for apps compiled with those widgetsets. With a GTK app, it's a bit more complicated since X11 always has to be running in order to run the app. Copy and paste the following script to file create_gtk_app.sh, use chmod +x on the file so OS X will let you run it, then run it as follows to create an app bundle (substitute the name of your own GUI app compiled with the GTK widgetset): 
    
    
    ./create_gtk_app.sh execfile
    

You can even use this script to create an app bundle for Lazarus itself, since it's a GTK app. Just change to wherever you want to create the app bundle (for example, the Applications folder) and run the script: 
    
    
    [path_to_script]/create_gtk_app.sh lazarus
    

Now you can drag and drop the resulting Lazarus app bundle onto the dock just like any other application. 

For more information, study the script. 

_**Tip:**_ In the script below, one of the lines is really long and may get chopped off when you print this page _(but why would you print this page only to type it in again when you can just copy and paste?) --[BigChimp](</User:BigChimp> "User:BigChimp") 12:37, 24 November 2013 (CET)_. Here's the line on two lines. Just be sure it's all one line in your script file. 
    
    
     echo '<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" 
      "http://www.apple.com/DTDs/PropertyList-1.0.dtd">' >>$plistfile
    
    
    
    #!/bin/sh
    # Force Bourne shell in case tcsh is default.
    #
    appname=$1
    appfolder=$appname.app
    macosfolder=$appfolder/Contents/MacOS
    plistfile=$appfolder/Contents/Info.plist
    if [ $appname = "lazarus" ]
    then
      appfile=/usr/local/share/lazarus/lazarus
    else
      appfile=$appname
    fi
    #
    if [ "$appname" = "" ]
    then
      echo "Usage: $0 executable_file"
      echo "Creates .app bundle (folder) for executable file that uses GTK widgetset"
    elif ! [ -e $appfile ]
    then
      echo "$appfile does not exist"
    elif [ -e $appfolder ]
    then
      echo "$appfolder already exists"
    else
      echo "Creating $appfolder..."
      mkdir $appfolder
      mkdir $appfolder/Contents
      mkdir $appfolder/Contents/MacOS
      mkdir $appfolder/Contents/Resources
    #
      if [ $appname = "lazarus" ]
      then
    # This is special case for lazarus IDE.
    # Create a script file in .app folder that starts X11, then Lazarus.
        echo '#!/bin/sh' >$macosfolder/$appname.sh
        echo '# This script starts X11, then Lazarus.' >>$macosfolder/$appname.sh
        echo 'open -a x11' >>$macosfolder/$appname.sh
        echo "export DISPLAY=':0.0'" >>$macosfolder/$appname.sh
        echo 'open -a '$appfile >>$macosfolder/$appname.sh                #IMPORTANT! If you're running TCSH, or other,
        chmod +x $macosfolder/$appname.sh                                 # "open -a" doesn't work. Use "open" instead
      else
    # Instead of copying executable into .app folder after each compile,
    # simply create a symbolic link to executable.
        ln -s ../../../$appname $macosfolder/$appname
    # Create a little script file in .app folder that opens executable with X11.
        echo '#!/bin/sh' >$macosfolder/$appname.sh
        echo '# This script opens the executable file with X11.' >>$macosfolder/$appname.sh
        echo 'open ${0/%.sh}' >>$macosfolder/$appname.sh
        chmod +x $macosfolder/$appname.sh
      fi
    #
    # Create PkgInfo file.
      echo "APPL????" >$appfolder/Contents/PkgInfo
    #
    # Create information property list file (Info.plist).
      echo '<?xml version="1.0" encoding="UTF-8"?>' >$plistfile
      echo '<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">' >>$plistfile
      echo '<plist version="1.0">' >>$plistfile
      echo '<dict>' >>$plistfile
      echo '  <key>CFBundleDevelopmentRegion</key>' >>$plistfile
      echo '  <string>English</string>' >>$plistfile
      echo '  <key>CFBundleExecutable</key>' >>$plistfile
      echo '  <string>'$appname'.sh</string>' >>$plistfile
      echo '  <key>CFBundleInfoDictionaryVersion</key>' >>$plistfile
      echo '  <string>6.0</string>' >>$plistfile
      echo '  <key>CFBundlePackageType</key>' >>$plistfile
      echo '  <string>APPL</string>' >>$plistfile
      echo '  <key>CFBundleSignature</key>' >>$plistfile
      echo '  <string>????</string>' >>$plistfile
      echo '  <key>CFBundleVersion</key>' >>$plistfile
      echo '  <string>1.0</string>' >>$plistfile
      echo '  <key>CSResourcesFileMapped</key>' >>$plistfile
      echo '  <true/>' >>$plistfile
      echo '</dict>' >>$plistfile
      echo '</plist>' >>$plistfile
    fi
    

  


## Using stdout for logging and debugging

Unix systems usually allows output to stdout for the GUI applications (while for Windows it requires to create a console object for stdout, use file handle as stdout or link the application as non-GUI). The output is really useful for debugging and logging purposes. 

MacOSX GUI applications (the ones created by Lazarus) are deployed as [bundles](<http://en.wikipedia.org/wiki/Application_bundle>). The bundled application conceals the output. However, it's possible to see the output using [Console](<http://en.wikipedia.org/wiki/Console_\(Mac_OS_X\)>) application. 

It's also possible to see the output, but launching the application from [Terminal](<http://en.wikipedia.org/wiki/Apple_Terminal>). 

  * Launch Terminal
  * enter to the project directory
  * enter the bundle (the directory with the .app extension), and launch the executable


    
    
    cd project.app/Contents/MacOS
    ./project
    

  * once project launched the output will be seen in the Terminal window



[![OSXStdOutExample.png](https://wiki.freepascal.org/images/0/0b/OSXStdOutExample.png)](</File:OSXStdOutExample.png>)

## Deploying an executable, Compiling under 10.6 for older version of Mac OS X

When compiling an application on Mac OS X without any special options, the application is only guaranteed to work on that particular major Mac OS X release and later (e.g., when compiling under Mac OS X 10.6.8, the application is only guaranteed to work on Mac OS X 10.6.8 and later). 

Here is an example of an error that you may encounter when running an application compiled for Mac OS X 10.5 under Mac OS X 10.4 in case you use the widestring manager: 
    
    
    dyld: Library not loaded: /usr/lib/libiconv.2.dylib
      Referenced from: /Volumes/..../yourprogram
      Reason: Incompatible library version: yourprogram requires version 7.0.0 or later, but libiconv.2.dylib provides version 5.0.0
    Trace/BPT trap
    

Here is an example of an error that you may encounter when running an application compiled for Mac OS X 10.6.8 under Mac OS X 10.6.7: 
    
    
    Runtime error 203
    

### FPC 2.6.2 and above

[http://wiki.freepascal.org/FPC_New_Features_2.6.2#Support_for_specifying_and_querying_the_deployment_version](<FPC_New_Features_2.6.md>)

#### Adding the custom options only when compiling for OS X

To add the above custom options only when compiling for OS X add to Project / Compiler options / IDE macros / Conditionals: 
    
    
    if TargetOS = 'darwin' then begin
      UsageCustomOptions += ' -WM10.6';
    end;
    

### FPC 2.6.1 and below

To make sure that it runs under previous Mac OS X releases, use the _-macosx_version_min_ linker parameter and link against the appropriate SDK (e.g. -XR/Developer/SDKs/MacOSX10.5.sdk/ or -XR/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.7.sdk/). For example, to compile an application that should work on Mac OS X 10.5 and higher: 

Add to /etc/fpc.cfg OR to Project / Compiler options / Other / Custom options: 
    
    
    -k-macosx_version_min -k10.5
    -XR/Developer/SDKs/MacOSX10.5.sdk/
    

The 10.4 SDK is the only one with a special name (10.4u instead of 10.4). Other SDK names simply contain the major Mac OS X version number: MacOSX10.5.sdk, MacOSX10.6.sdk, ... 

Note: The path _/Developer_ depends on where you installed the Apple developer tools, and may be different if you chose a different location. Do not assume it will always be /Developer. 

The 10.7 is by default: 
    
    
    -k-macosx_version_min -k10.7
    -XR/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.7.sdk/
    

For targeting Mac OS X 10.4 (Tiger) the following settings are required: 
    
    
    -k-macosx_version_min -k10.4
    -XR/Developer/SDKs/MacOSX10.4u.sdk/
    

#### Adding the custom options only when compiling for OS X

To add the above custom options only when compiling for OS X add to Project / Compiler options / IDE macros / Conditionals: <syntaxhighlight="pascal"> if TargetOS = 'darwin' then begin 
    
    
     UsageCustomOptions += ' -k-macosx_version_min -k10.5';
     UsageCustomOptions += ' -XR/Developer/SDKs/MacOSX10.5.sdk/';
    

end; </syntaxhighlight>

## Useful tools to download and install

[**TextWrangler**](<http://barebones.com/products/textwrangler/index.shtml>)

Every programmer needs a great text editor, but OS X's TextEdit app is not it. Instead, download and install TextWrangler, a freeware text editor that can highlight Pascal syntax, even highlight script file syntax. 

Other editors include AlphaX, BBedit, or you can use OS X's Xcode IDE to edit text files. 

[**NeoOffice**](<http://www.neooffice.org/>)

NeoOffice is an OS X version of OpenOffice.org that actually looks and behaves like an OS X app. Once you've started using Neo you'll never want to use the plain X11 version of OO again. 

[**Unix and Open Source downloads**](<http://www.apple.com/downloads/macosx/unix_open_source/>)

Apple provides downloads of numerous free applications, many of which have been ported from Unix and run under X11, such as the venerable Gimp. 

[**Onyx**](<http://www.titanium.free.fr/pgs/english.html>)

Onyx is a useful maintenance tool for OS X. 

[**Cyberduck**](<http://cyberduck.ch/>)

Need an FTP client app for OS X? Cyberduck is an open source FTP client specifically designed for OS X. 

## Useful commands and tools included with OS X

**open**

Use open to start an application or open a file from the command line. This is useful when you’re working in a terminal window and don’t want to fire up the app and navigate to where you’re working just to open a file there. For example, to open a Pascal file in the current directory in TextWrangler, enter the following: 
    
    
    open –a textwrangler myfile.pas
    

**zip / unzip**

These standard command-line programs are installed with OS X’s Xcode tools (on a separate CD with OS X 10.3). 

**Console**

Drag this app from /Applications/Utilities and drop it on the dock so you always have it handy. Launch it whenever you want to see messages or errors outputted to the console by GUI apps (for example, when they crash). Invaluable for debugging. 

**Activity Monitor**

This app is also in /Applications/Utilities and is useful for monitoring CPU and disk usage. 

**otool / install_name_tool**

Use otool to display information about an executable file or library. For example, enter the following to see information about Lazarus: 
    
    
    cd /usr/local/share/lazarus
    otool –L lazarus
    

This shows that Lazarus is dependent on various libraries in /sw/lib, /usr/X11R6/lib and /usr/lib, as you would expect. 

Use install_name_tool with the –change switch to change where an executable file or library looks for a library that it requires. 

**Grab**

Use Grab (in /Applications/Utilities) to create a screenshot or window shot, then save it to a disk file. Note: replaced in recent versions of macOS by CMD+SHIFT+4 for image capture and CMD+SHIFT+5 for video capture (full or part screen). 

**Icon Composer**

Use Icon Composer (in /Developer/Applications/Utilities) to create icon files (.icns) for use with your .app bundles. 

**pkgbuild, productbuild / Disk Utility**

Use these apps to create disk image (.dmg) files for deploying your apps. See [Deploying Your Application](<Deploying_Your_Application.md> "Deploying Your Application") for more information. 

Command line tools pkgbuild and productbuild are in /usr/bin/. Disk Utility is in /Applications/Utilities. You can drag and drop Disk Utility on the dock. 

A popular third party option is the Packages GUI application (<http://s.sudre.free.fr/Software/Packages/about.html>). 

**Script Editor / osascript**

Use Script Editor to edit, compile and run AppleScript files. It’s located in /Applications/AppleScript. 

Use osascript to execute an AppleScript command or file from the command line or from a script file. For more information, enter man:osascript in Safari once you have Sogudi installed (see above). 

## Commonly used Unix commands

If you’re coming to OS X from Windows, you may find some of its Unix terminal commands confusing. Here are some equivalents. For more information about a command, enter man:_command_ in Safari once you have Sogudi installed (see above). 

Action | Windows command prompt window | OS X Terminal or X11 window   
---|---|---  
Change to a different directory | cd | cd   
Make a new directory | mkdir | mkdir   
Delete a directory | rmdir | rmdir   
List file directory in chronological order with detail | dir /od | ls -ltr   
Copy a file, preserving its date-time stamp | copy | cp -p   
Display contents of a text file | type | cat   
Delete a file | erase | rm   
Move a file | move | mv   
Rename a file | ren | mv   
Find a file | dir /s | find   
Grep a file | findstr | grep   
Display differences between two text files | fc | diff   
Change file attributes | attrib | chmod   
“Super-user” root authorization | N/A | sudo   
Create symbolic link to a file or directory | N/A | ln   
Shrink executable file size | strip (included w/ Free Pascal) | strip   
  
  


## Mac OS X Libraries

### Using a library in a Mac application

Many software projects use external libraries to complement their functionality. For example, OpenAL might be utilized to provide sound support, or FreeType might be utilized to provide font rendering support for FCL-Image. Mac OS X already comes with a number of open source libraries installed, being that OpenAL is one of them. If one of these libraries is utilized, one should simply add the corresponding import unit to the uses clause and start using the library. To use other open source libraries which aren't included with Mac OS X, the easiest way is by grabbing a pre-compiled library from the application bundle of popular open source applications. FreeType for example is included inside the application bundle from Gimp in the path Gimp.app/Resources/lib/libfreetype.6.dylib. Another option is downloading the source code from the library and building it manually, but compiling a C library is usually an unpleasant task for Pascal developers. 

So, again in the previous example, let's say that an application requires the FreeType library. One can grab the libfreetype.6.dylib file from Gimp and then place it in a location of our source code tree, for example ../Mac/libfreetype.dylib relative to the path of the main project file, which is where the executable is placed. To properly link the application the Free Pascal command line option "-Fl../Mac" should be added, so that this path is added to the library search path. The linker will then find the library in this location and read the install path which is written inside it. At runtime the linker will search for the library in this install path. To change the install path to our desired one, the command line application install_name_tool, which comes with the Apple Developer Tools, can be used. It can be utilized with the following command line command: 
    
    
    install_name_tool libfreetype.dylib -id @executable_path/../Resources/lib/libfreetype.dylib
    

One can then verify if this command worked by using the command: 
    
    
    otool -D libfreetype.dylib
    

It should return the written path. When building the application bundle, we should copy the libfreetype.dylib file to the directory Resources/lib. The _install_name_tool_ command uses the _@executable_path_ macro to set a install path _relative to the executable_ , which is inside the MacOS folder in the application bundle. Now the application can be built, the bundle and be built and then the application can be run and it will load the library correctly from the application bundle. 

### Using a library installed using fink

For using libraries, which were installed using fink, you need to add the path to the libraries. The default is /sw/lib. Adding the option -Fl/sw/lib to the command line tells the linker where to find the libraries. The resulting application is fine for distribution with fink, but in order to run without fink, you have to make a "standalone" version. There are several possibilities to achieve this, one of them will be outlined here: First copy the .dylib file into the application bundle, for example to Contents/MacOS/ next to the executable binary. Then, adjust the paths install_name_tool: 

First, change the path of the library in the executable: 
    
    
    install_name_tool  -change /sw/lib/libfreetype.dylib @executable_path/libfreetype.dylib @executable_path/your_executable_binary
    

Second, adjust the path in the library 
    
    
    install_name_tool  -id @executable_path/libfreetype.dylib @executable_path/libfreetype.dylib
    install_name_tool  -change /sw/lib/libfreetype.dylib @executable_path/libfreetype.dylib @executable_path/libfreetype.dylib
    

The complete list of libraries in your executable can be obtained with: 
    
    
    otool -L @executable_path/your_executable_binary | grep version | cut -f 1 -d ' '
    

The list with libraries from fink only without libraries from /usr/lib and /System/Library gives this command: 
    
    
    otool -L @executable_path/your_executable_binary | grep version | cut -f 1 -d ' ' | grep -v \/System\/Library | grep -v \/usr\/lib
    

With this a script can be created, which loops over all libraries from fink in the executable. 

Beware of the fact that the libraries can itself call libraries from fink. You have to adjust them as well. 

Another important point of libraries from fink is, that they have only one architecture, i.e. i386, ppc, x86_64 or ppc64. For a universal application, you have to combine single architecture libraries with lipo. 

## How to obtain the path to the Bundle
    
    
     
    ...
    uses
      MacOSAll;
    ...
     
    function GetBundlePath(): string;
    var
      pathRef: CFURLRef;
      pathCFStr: CFStringRef;
      pathStr: shortstring;
    begin
      pathRef := CFBundleCopyBundleURL(CFBundleGetMainBundle());
      pathCFStr := CFURLCopyFileSystemPath(pathRef, kCFURLPOSIXPathStyle);
      CFStringGetPascalString(pathCFStr, @pathStr, 255, CFStringGetSystemEncoding());
      CFRelease(pathRef);
      CFRelease(pathCFStr);
     
      Result := pathStr;
    end;
    

This returns the full path up to and including the application bundle name (eg /Users/Me/MyApp.app). 

## How to obtain the Application Support and Preferences paths

To obtain the Apple-sanctioned user and system Application Support and Preferences folder paths, see: [Proper macOS file locations](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide"). 

## Accessing system information

Sysctl provides an interface that allows you to retrieve many kernel parameters in macOS (and Linux and the BSDs). This provides a wealth of information detailing system hardware which can be useful when debugging user issues or simply optimising your application (eg to take advantage of threads on multi-core CPU systems). For full details and example code, see the article [Accessing macOS System Information](<Accessing_macOS_System_Information.md> "Accessing macOS System Information"). 

## Creating Universal binaries

Lazarus will create an application tuned for a single CPU. With the carbon version of the lcl, this is either PowerPC or Intel i386 (32bit). You may want to distribute your application as a 'Universal Binary', allowing users that may have either a PowerPC or Intel computer to use the program. To do this, you need to compile two versions of your application: one with PowerPC as the target CPU and one as Intel. Next, you need to stitch these two executables together using the OS X program "lipo" (installed with the other OS X developer tools required by Lazarus). For example, consider two versions of the application 'myprogram'. One in the 'ppc' folder compiled for PowerPC and one in the 'intel' folder compiled for Intel CPUs. You can then combine these two to a univeral application by running this command: 
    
    
     lipo -create ./ppc/myproj ./intel/myproj -output ./myproj
    

Now, copy the newly created application myproj inside your .app folder. 

A note on 64 bit binaries: Depending on the version of OS X and after corresponding (cross-)compilers are installed, programs, which do not use the lcl, can also be built as ppc64 or x86_64 binaries, in particular command line utilities. lipo is used in the same way to put them into universal binaries. 

## Code Signing

Before macOS Sierra users had the option to allow the installation of applications from "anywhere", "the App Store" and "identified developers". From macOS Sierra on, the "anywhere" option was removed. Thereafter, clicking on an application to run it results in a scary dialog being presented to your users stating: "YourApp can't be opened because it is from an unidentified developer. Your security preferences allow installations of only apps from the App Store and identified developers". The only dialog option is to close it. 

Users can still run your application, but they have to know they can right-click or control-click on your application and then they get another scary dialog stating "YourApp is from an unidentified developer. Are you sure you want to open it?" but this time the dialog has an "open" button and a pre-focussed "cancel" button. 

[Code signing](<Code_signing.md> "Code signing") an application that has been written in Lazarus and Free Pascal requires only a few steps that are described in the article [Code Signing for Mac OS X](<Code_Signing_for_Mac_OS_X.md> "Code Signing for Mac OS X"). Note: You will need to pay Apple $US 99 per year to be an "identified developer" and code sign your applications. 

## Notarization

Beginning with macOS Mojave 10.14.5 all new or updated kernel extensions and all software from developers new to distributing with a Developer ID must also be notarized in order to run or users are prevented from running the application with a very scary dialog stating: "YourApp can't be opened because Apple cannot check it for malicious software. This software needs to be updated. Contact the developer for more information." 

Beginning with macOS Catalina 10.15 (currently in Beta for release later in 2019), notarization is required by default for all software. If your application is not notarised, your users get the very scary dialog stating: "YourApp can't be opened because Apple cannot check it for malicious software. This software needs to be updated. Contact the developer for more information." This is in addition to requiring the application to be code signed. 

The steps to notarize an application are described in the article [Notarization for macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+"). 

## Locale settings (date & time separator, currency symbol, ...)

On OS X, like on other Unix platforms, the RTL does not load the locale settings by default. They can be initialised in three ways. 

### Automatic initialisation based on the System Preferences settings

In FPC 2.7.1 and later, adding the _iosxlocale_ unit to the uses clause will initialise the locale settings using the settings from the System Preferences. This unit is not available in older versions of FPC. 

### Automatic initialisation based on the settings in the Unix layer

Adding the _clocale_ unit to the uses clause will cause the locale settings to be initialised using the configuration set at the Unix layer of OS X (based on the _LANG_ and related environment variables). This unit is also available on other Unix-like platforms. 

### Manual initialisation

  * Hardcoded:


    
    
     
      // use in initialization or in onCreate of the main form
      DateSeparator := '.';
      ShortDateFormat := 'dd.mm.yyyy';
      LongDateFormat := 'd. mmmm yyyy';
    

[[1]](<http://forum.lazarus.freepascal.org/index.php?topic=9566.0>)

  * Partially replicating the functionality of the iosxlocale unit:


    
    
     
    
    uses
      {$IFDEF LCLCarbon}
      , MacOSAll, CarbonProc
      {$ENDIF}
    
    var
    {$IFDEF LCLCarbon}
      theFormatString: string;
      theFormatter: CFDateFormatterRef;
    {$ENDIF}
    
    procedure GetMacDateFormats;
    begin
      {$IFDEF LCLCarbon}
      theFormatter := CFDateFormatterCreate(kCFAllocatorDefault, CFLocaleCopyCurrent, kCFDateFormatterMediumStyle, kCFDateFormatterNoStyle);
      theFormatString := CFStringToStr(CFDateFormatterGetFormat(theFormatter));
      if pos('.', theFormatString) > 0 then
        DefaultFormatSettings.DateSeparator := '.'
      else if pos('/', theFormatString) > 0 then
        DefaultFormatSettings.DateSeparator := '/'
      else if pos('-', theFormatString) > 0 then
        DefaultFormatSettings.DateSeparator := '-';
      DefaultFormatSettings.ShortDateFormat := theFormatString;
      CFRelease(theFormatter); 
      theFormatter := CFDateFormatterCreate(kCFAllocatorDefault, CFLocaleCopyCurrent, kCFDateFormatterLongStyle, kCFDateFormatterNoStyle);
      theFormatString := CFStringToStr(CFDateFormatterGetFormat(theFormatter));
      DefaultFormatSettings.LongDateFormat := theFormatString;
      CFRelease(theFormatter); 
      {$ENDIF}
    end;
    

If the procedure _GetMacDateFormats_ is called in the beginning of the program's main unit the "International" settings of systems preferences are used. 

## Apple-specific UI elements

### AppName/Apple main menu item

In each Mac application there is a main menu item with the name of the application. Lazarus will automatically add the "Services", "Hide" and "Quit" menu items to this menu. In order to add more items to this menu, create a menu item with the apple character as its Caption "" (without quotes, it is Unicode 0xF8FF). You can use ifdefs {$IFDEF DARWIN} to hide it in non-macOS. 

For example: 
    
    
      {$IFDEF DARWIN}
      AppMenu := TMenuItem.Create(Self);  {Application menu}
      AppMenu.Caption := #$EF#$A3#$BF;  {Unicode Apple logo char}
      MainMenu.Items.Insert(0, AppMenu);
    
      AppAboutCmd := TMenuItem.Create(Self);
      AppAboutCmd.Action:= ActionList1.Actions[1];
      AppMenu.Add(AppAboutCmd);  {Add About as item in application menu}
    
      AppSep1Cmd := TMenuItem.Create(Self);
      AppSep1Cmd.Caption := '-';
      AppMenu.Add(AppSep1Cmd);
    
      AppCheckCmd := TMenuItem.Create(Self);
      AppCheckCmd.Action:= ActionList1.Actions[2];
      AppMenu.Add(AppCheckCmd);  {Add Check as item in application menu}
    
      AppSep2Cmd := TMenuItem.Create(Self);
      AppSep2Cmd.Caption := '-';
      AppMenu.Add(AppSep2Cmd);
      {$ENDIF}
    

By default your application name will be the name of your project. You can rename your project, but you cannot add a space to the project name. So, "My App" will show up in the AppName/Apple Menu as "MyApp". Your application dock icon will also display "MyApp". Fear not, this can be fixed by editing the application Info.plist file and changing the Bundle Name key value from "MyApp" to "My App". For example: 
    
    
      <key>CFBundleName</key>
      <string>My App</string>
    

### Adding an icon to your app bundle

1\. Use OS X's Icon Composer or a similar program to create your app's icon file (.icns extension). 

2\. Copy the .icns file to your app bundle's Resources folder. For example, from the command line: 
    
    
    cp -p myapp.icns myapp.app/Contents/Resources
    

3\. Add the following key to your app bundle's Info.plist file: 
    
    
    <key>CFBundleIconFile</key>
    <string>myapp.icns</string>
    

Sometimes Finder doesn't "refresh" an app to use the new icon, continuing instead to show the default icon. You can try these things to force Finder to use the new icon: 

  * In a Terminal, use the UNIX command "touch" to touch the app bundle (eg touch MyApp.app) - this should work instantly.
  * Log out and back in again.
  * Drag (move) your application to a different folder, then back again.
  * Restart Finder (Esc-Option-Apple keys).



You shouldn't have this problem with Finder when you install your app on other computers. The issue is a result of Finder caching the icon. 

### Removing the app icon from title bars

The Apple convention is that a window title bar has an icon only when the window represents a file. However, in the LCL the icon typically defines the application itself. 

The expected Apple behaviour can be achieved by setting the global variable **CocoaIconUse** to true and including the **CocoaInt** unit. See [Cocoa Form Icons](<Cocoa_Internals/Forms.md> "Cocoa Internals/Forms") for the full details. 

See also the Apple [_Human Interface Guidelines_](<https://developer.apple.com/design/human-interface-guidelines/macos/windows-and-views/window-anatomy/>) document. 

### Add an Apple help book to your app

No professional macOS application should be without a functioning Apple Help Book. Unfortunately Apple make it harder than it should be for developers to create an Apple Help Book because their documentation is significantly out of date (2013 anyone?) and the Apple Help Viewer has undergone significant change in recent versions of the operating system. For the full details on how to add an Apple Help Book to your application, along with sample code, refer to the article [Add an Apple Help Book to your macOS app](<Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app"). 

### Using preferences in your app

For details on how to use Preferences in your application, refer to the articles: [Reading and Writing Preferences](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write") and [Preferences Menu](<Mac_Preferences_and_About_Menu.md> "Mac Preferences and About Menu"). 

## See also

  * [Installing Lazarus on MacOS X](<Installing_Lazarus_on_MacOS_X.md> "Installing Lazarus on MacOS X")



## Links

  * [Apple developer documents](<http://developer.apple.com/documentation/MacOSX/>)


  * [Making Your Lazarus App Mac-Friendly](<https://macpgmr.github.io/MacXPlatform/MacXPlatform_Part8.html>) (Apple help info outdated)



**Coming to OS X from Unix or Linux?**

  * [What is Mac OS X?](<http://www.kernelthread.com/mac/osx>)


  * [Top Ten Mac OS X Tips for Unix Geeks](<http://www.macdevcenter.com/pub/a/mac/2002/10/22/macforunix.html>)


  * [Setting Up an OS X Mac with The Usual Suspects](<http://users.ece.gatech.edu/~lanterma/mac_setup.html>)



**Coming to OS X from Windows and want to learn Unix scripting?**

  * [Mac OS X Unix Tutorial](<http://www.osxfaq.com/Tutorials/LearningCenter/UnixTutorials/ShellScripting1/index.ws>)


  * [Shell Scripting Primer](<http://developer.apple.com/documentation/OpenSource/Conceptual/ShellScripting/index.html>)



**Coming to Object Pascal from C++, C# or Java?**

  * [Comparing Object-Oriented Features of Delphi, C++, C# and Java](<http://www.derangedcoder.net/programming/general/comparingObjectOrientedFeatures.html>)

---

_Source: [https://wiki.freepascal.org/OS_X_Programming_Tips](https://web.archive.org/web/20190825133953/https://wiki.freepascal.org/OS_X_Programming_Tips)_
