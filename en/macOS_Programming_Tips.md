# macOS Programming Tips

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 Choice of Lazarus LCL widgetsets
  * 2 Cross compiling macOS applications
  * 3 Commonly used Unix commands
  * 4 Useful commands and tools included with macOS
  * 5 Cross-platform considerations
  * 6 Apple-specific UI elements
  * 7 Setting environment variables for an application
  * 8 How to obtain the path to the Bundle
  * 9 Determining where to store files
  * 10 Determining if a file is readable
  * 11 Accessing system information
  * 12 Retrieve username/ full username
  * 13 Making a beep
  * 14 Show application in all desktop spaces
  * 15 Launch/Toggle full screen
  * 16 Locale settings (date & time separator, currency symbol, ...)
  * 17 Accessibility for users with special needs
  * 18 LCL FontDialog workaround
  * 19 Coping with missing features across macOS versions
  * 20 Updating a GUI from a secondary thread
  * 21 Using PrintDialog in Cocoa
  * 22 Detecting the Apple Command key
  * 23 Updating the GUI from a non-main thread
  * 24 Libraries
    * 24.1 Using a library in a Mac application
    * 24.2 Using a library installed using fink
    * 24.3 Dynamic Libraries
    * 24.4 Static Libraries
    * 24.5 Which dynamic libraries are used?
    * 24.6 OpenSSL, LibreSSL, Secure Transport, Network Framework
      * 24.6.1 OpenSSL versions
  * 25 Application deployment/distribution
    * 25.1 Creating Universal binaries
      * 25.1.1 aarch64 or Intel x86_64
      * 25.1.2 PowerPC or Intel i386
    * 25.2 Executable size optimisation
    * 25.3 Deploying an application for an older version of the operating system
      * 25.3.1 FPC 2.6.2 and above
      * 25.3.2 FPC 2.6.1 and below
    * 25.4 Creating an app bundle for a GTK application
    * 25.5 Code Signing
    * 25.6 Notarization
    * 25.7 Deploying your application
  * 26 Using stdout and stderr for logging and debugging
    * 26.1 Using a Terminal
    * 26.2 Using the Console
  * 27 Troubleshooting
  * 28 Converting legacy Pascal code
  * 29 Other Interfaces
    * 29.1 Platform specific Tips
    * 29.2 Interface Development Articles
  * 30 See also
  * 31 External Links



## Choice of Lazarus LCL widgetsets

With macOS there are various widgetsets which can be used with Lazarus. Each has its strengths and weaknesses. 

**Carbon widgetset** (see the [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") page) 

Pros | Cons   
---|---  
All standard LCL controls working | The entire Carbon framework has been deprecated by Apple. It is not available in 64 bit macOS applications. It was completely removed from macOS 10.15 Catalina (October 2019). The Carbon widgetset is no longer being developed.   
No additional libraries or frameworks to install; uses Carbon framework included with macOS   
Native macOS look and feel | Only relevant to macOS   
  
**Cocoa widgetset** (see the [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") page) 

Pros | Cons   
---|---  
All standard LCL controls working | There may still be some minor bugs lurking; please report them!   
No additional libraries or frameworks to install;  
uses the Cocoa framework included with macOS   
The only graphical framework for macOS 10.15 Catalina and later   
Necessary for graphical 64 bit applications   
Native macOS look and feel | Only relevant to macOS   
  
**GTK widgetset**

Pros | Cons   
---|---  
All standard LCL controls working | Ugly; clunky common dialogs; doesn't look like Mac software   
This widgetset is well tested | Requires X11 and bulky GTK to be installed to run Lazarus or any GUI apps created with Lazarus   
| Have to start Lazarus at command line -- can't double-click Lazarus or place on dock (same with GUI apps) -- although see Creating an app bundle for a GTK application for how you can add this capability yourself.   
  
**Qt widgetset** (see the [Qt Interface Mac](<Qt_Interface_Mac.md> "Qt Interface Mac") page) 

Pros | Cons   
---|---  
Native macOS look and feel | Requires the Qt interface framework to be installed to run app, which is rather large   
All standard LCL controls working |   
The Qt widgetset also available for other platforms, so any effort put into developing Qt widgetset benefits multiple platforms |   
The Qt interface provides rich functionality |   
The Qt widgetset is easier to develop than other widgetsets |   
  
## Cross compiling macOS applications

See the [Cross compiling](<Cross_compiling.md> "Cross compiling") article. 

## Commonly used Unix commands

If you’re coming to macOS from Windows, you may find some of its Unix terminal commands confusing. Here are some equivalents. For more information about a command, enter `man command` in an Applications > Utilities > Terminal. 

Action | Windows command prompt window | macOS Terminal or X11 window   
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
Create symbolic link to a file or directory | mklink | ln   
Shrink executable file size | strip (included w/ Free Pascal) | strip   
  
## Useful commands and tools included with macOS

**open**

See the [macOS Open Sesame](<macOS_Open_Sesame.md> "macOS Open Sesame") article for the many and varied uses of the macOS `open` command. 

**zip / unzip**

These are standard command-line zip archive compression/uncompression utilities. 

**Console**

Drag the _Console_ application from `/Applications/Utilities` and drop it on the dock so you always have it handy. Launch it whenever you want to see messages or errors output to the console by GUI apps (for example, when they crash). Invaluable for debugging. 

**Activity Monitor**

The _Activity Monitor_ application is also in `/Applications/Utilities` and is useful for monitoring CPU, memory, energy, network and disk usage. 

**otool**

Use the _otool_ command line utility to display information about an executable file or library. For example, enter the following to see information about Lazarus: 
    
    
    cd /usr/local/share/lazarus
    otool –L lazarus
    

This shows that Lazarus is dependent on various libraries in `/usr/lib` and `/System/Library/Frameworks/`, as you would expect. Open a Terminal and type `man otool` to view its manual page. 

**install_name_tool**

Use _install_name_tool_ with the _–change_ switch to change where an executable file or library looks for a library that it requires. Open a Terminal and type `man install_name_tool` to view its manual page. 

**iconutil**

Use the _iconutil_ command line utility in `/usr/bin/` to create icon files (.icns) for use with your .app bundles. Open a Terminal and type `man iconutil` for its manual page. 

**pkgbuild, productbuild / Disk Utility**

Use these utilities to create disk image (.dmg) files for deploying your applications. See [Deploying Your Application](<Deploying_Your_Application.md> "Deploying Your Application") for more information. 

Command line tools `pkgbuild` and `productbuild` are in `/usr/bin/`. Disk Utility is in `/Applications/Utilities`. You can drag and drop Disk Utility on the dock. 

A popular third party option is the GUI application [Packages](<http://s.sudre.free.fr/Software/Packages/about.html>) which creates .pkg installer archives. 

**PlistBuddy**

The command line utility `PlistBuddy`, which is found in `/usr/libexec/`, may be used to read and modify the values inside of [property list files](<macOS_property_list_files.md> "macOS property list files"). 

**Script Editor / osascript**

Use the _Script Editor_ application to edit, compile and run AppleScript files. It’s located in `/Applications/Utilities/`. 

Use the command line utility _osascript_ from `/usr/bin/` to execute an AppleScript command or file from the command line or from a script file. For more information, enter `man osascript` in a Terminal to view its manual page. 

**latency**

The _latency_ diagnostic command line utility found in `/usr/bin/` monitors scheduling and interrupt latency measuring the number of context switches and interrupts, and reporting on the resulting delays. Open a Terminal and type `man latency` to view its manual page. 

**fs_usage**

The _fs_usage_ diagnostic command line utility reports system calls and page faults related to filesystem activity in real-time. The output displays the timestamp, system call, filename, elapsed time and name of the process. Open a Terminal and type `man fs_usage` to view its manual page. 

**Safari**

Yes, I know, it is a web browser - but it is more. You can read manual pages with it too. So, rather than opening a Terminal and then typing `man 3 pause`, you can simply type `x-man-page://3/pause` into Safari's URL field and access the man page that way. Unfortunately recent version of Safari now ask you if you want to allow Safari to open the Terminal and this new security behaviour cannot be turned off and your choice is not remembered. 

**sc_usage**

The _sc_usage_ command line utility samples system calls and page faults, displaying them onscreen. Open a Terminal and type `man sc_usage` to view its manual page. 

**vm_stat**

The _vm_stat_ command line utility displays virtual memory statistics. Open a Terminal and type `man vm_stat` to view its manual page. 

**DevToolsSecurity**

The _DevToolsSecurity_ command line utility changes the security authorisation policies for use of the Apple-code-signed debugger and performance analysis tools on development systems. See further [DevToolsSecurity](<lldb.md> "lldb"). 

## Cross-platform considerations

When you plan to compile the same application for different widget sets (eg Windows, GTK and Cocoa) or plan to use multiple languages, you should layout the controls on your form(s) using the [anchor editor](<Anchor_Sides.md> "Anchor Sides") and set the [AutoSize](<Autosize_/_Layout.md> "Autosize / Layout") property of your controls to _True_. Doing this from the beginning will save you a lot of frustrating layout problems later. 

## Apple-specific UI elements

The Apple user interface in macOS has a number of unique features (eg the Dock, Apple main menu bar, sheets, drawers, user notifications, etc). For details on how to provide these non-cross-platform features in your applications, see the article [Apple-specific UI elements](<Apple-specific_UI_elements.md> "Apple-specific UI elements"). 

## Setting environment variables for an application

On macOS, the normal way to set any environment variables needed by an application is to set them in the LSEnvironment property of your application's [`Info.plist`](<macOS_property_list_files.md> "macOS property list files") file. 

## How to obtain the path to the Bundle
    
    
     
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1} 
    
    ...
    
    uses
      CocoaAll; // for NSBundle
    
    ...
     
    function GetBundlePath: string;
    begin
      Result := NSBundle.mainBundle.bundlePath.UTF8String;
    end;
    

This returns the full path up to and including the application bundle name (eg `/Users/Me/MyApp.app`). 

Related: [Locating the macOS application resources directory](<Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory"). 

## Determining where to store files

Before we go any further, let's recap where the Apple Guidelines indicate your application should store its files: 

  * Use the `/Applications` or `/Applications/Utilities` directory for the [application bundle](<Application_Bundle.md> "Application Bundle"). The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities directory`, it should be able to run. Installing to these folders requires Admin privileges. The data in these folders is backed up by Time Machine.


  * Use the `~/Applications` directory should Admin privileges not be available. This is the standard location for a single user application. This directory should not be expected to exist. The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities` directory, it should be able to run. This data is backed up by Time Machine.


  * Use the [Application Support directory](<Locating_macOS_app_support,_preferences_folders.md> "Locating macOS app support, preferences folders") (this data is backed up by Time Machine), appending your <bundle_ID>, for: 
    * Resource and data files that your application creates and manages for the user. You might use this directory to store application state information, computed or downloaded data, or even user created data that you manage on behalf of the user.
    * Autosave files.


  * Use the [Caches directory](<Locating_macOS_significant_directories.md> "Locating macOS significant directories") (this is **not** backed up by Time Machine), appending your <bundle_ID>, for cached data files or any files that your application can recreate easily.


  * Use [CFPreferences](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write") to read and write your application's preferences. This will automatically write preferences to the appropriate location and read them from the appropriate location. This data is backed up by Time Machine.


  * Use the [application Resources directory](<Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory") (this is backed up by Time Machine) for your application-supplied image files, sound files, icon files and other unchanging data files necessary for your application's operation.


  * Use [NSTemporaryDirectory](<Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory") (this is **not** backed up by Time Machine) to store temporary files that you intend to use immediately for some ongoing operation but then plan to discard later. Delete temporary files as soon as you are done with them.



## Determining if a file is readable

The Lazarus function [fileisreadable](<https://lazarus-ccr.sourceforge.io/docs/lazutils/lazfileutils/fileisreadable.html>) reports if the user has read permissions for the specified file. However, modern versions of macOS will restrict [access to the following locations](<https://developer.apple.com/library/archive/documentation/Security/Conceptual/AppSandboxDesignGuide/AppSandboxInDepth/AppSandboxInDepth.html>): 

  * The app container directory. Upon first launch, the operating system creates a special directory for use by your app—and only by your app—called a container. Each user on a system gets an individual container for your app, within their home directory; your app has unfettered read/write access to the container for the user who ran it.
  * App group container directories. A sandboxed app can specify an entitlement that gives it access to one or more app group container directories, each of which is shared among all apps with that entitlement.
  * User-specified files. A sandboxed app (with an appropriate entitlement) automatically obtains access to files in arbitrary locations when those files are explicitly opened by the user or are dragged and dropped onto the application by the user.
  * Related items. With the appropriate entitlement, your app can access a file with the same name as a user-specified file, but a different extension. This can be used for accessing files that are functionally related (such as a subtitle file associated with a movie) or for saving modified files in a different format (such as re-saving an RTF flat file as an RTFD container after the user added a picture).
  * Temporary directories, command-line tool directories, and specific world-readable locations. A sandboxed app has varying degrees of access to files in certain other well-defined locations.



At the moment, there appears to be no reliable API to determine if a file is readable in all situations. Apple's own _isReadableFileAtPath_ method is essentially a wrapper around the BSD _access_ system call and does not take into account the various security and privacy features that Apple has added since macOS 10.0. Below, is a brute force method that determines if a file is readable by physically reading the first byte of data from the file. This is necessarily slower than simply reading the file permissions, but does provide a robust solution: 
    
    
    function FileIsReadableByThisExecutable(const AFilename: string): boolean;
    // macOS will block applications from reading files outside their sandbox
    // therefore, simply knowing the user has read acces is not sufficient
    // requires "uses LazFileUtils"
    var
      f: file;
      b: byte;
    begin
      Result := lazfileutils.FileIsReadable(AFilename);
      {$IFDEF Darwin}
      if not Result then exit; //globally not readable
      if FileSizeUtf8(AFilename) < 1 then exit(false);
      AssignFile(f, AFilename);
      {$I+}
      try
        FileMode := fmOpenRead;  //Set file access to read only
        Reset(f, 1);
        if ioresult <> 0 then
           exit;
        b := 0;
        BlockRead(f, b, sizeof(b));
        CloseFile(f);
        result := true;
      except
        result := false;
      end;
      {$ENDIF}
    end;
    

Many system frameworks that provide access to [protected resources](<https://developer.apple.com/documentation/bundleresources/information_property_list/protected_resources>) have dedicated APIs for checking and requesting authorization to use those resources. This allows you to adjust your application’s behaviour depending on the current access it has. For example, if the user denies your application permission to do something, you can remove related elements from your user interface. Because a user can change authorization at any time using Settings, always check the authorization status of a feature before accessing it. In cases without a dedicated API, be prepared to gracefully handle access failures as above. 

For an example of an authorization API, see [AVCaptureDevice API](<https://developer.apple.com/documentation/avfoundation/avcapturedevice/1624613-authorizationstatus>). 

## Accessing system information

Sysctl provides an interface that allows you to retrieve many kernel parameters in macOS (and Linux and the BSDs). This provides a wealth of information detailing system hardware which can be useful when debugging user issues or simply optimising your application (eg to take advantage of threads on multi-core CPU systems). The NSProcessInfo class provides access to a collection of information about the current process. The IOKit framework implements non-kernel access to I/O Kit objects (eg to retrieve the Mac platform serial number and UUID). For full details and example code, see the article [Accessing macOS System Information](<Accessing_macOS_System_Information.md> "Accessing macOS System Information"). 

## Retrieve username/ full username

The code to retrieve the current user's username (eg joe) or full username (eg Joe Bloggs) can be found in the article [macOS Retrieve (full)username](<macOS_Retrieve_\(full\)username.md> "macOS Retrieve \(full\)username"). 

## Making a beep

Procedure declaration: 
    
    
    procedure NSBeep; cdecl;
      external '/System/Library/Frameworks/AppKit.framework/AppKit' name 'NSBeep';
    

To use it: 
    
    
    procedure MakeBeep;
      begin
        {$IFDEF DARWIN}
        NSbeep;
        {$ENDIF}
    
        [...]
      end;
    

You can also assign NSBeep to the [OnBeep](<https://www.freepascal.org/docs-html/rtl/sysutils/onbeep.html>) (TBeepHandler in the SysUtils unit), by setting it on Form creation. 

This way you can call the regular [Beep](<https://www.freepascal.org/docs-html/rtl/sysutils/beep.html>) function (by default, Beep contains no implementation to actually produce a beep). 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
      begin
        {$IFDEF DARWIN}
        OnBeep := @NSbeep;
        {$ENDIF}
    
        [...]
      end;
    

## Show application in all desktop spaces

An application defaults to showing only in the desktop space in which it was opened. To show your application in all desktop spaces for macOS 10.5+ (Leopard): 
    
    
    {$modeswitch objectivec1}
    
    Uses CocoaAll ...
    
    procedure TForm1.MenuItem35Click(Sender: TObject);
    var
      theWindow: NSWindow;
    begin
      theWindow := NSView(Form1.Handle).window;       // Form1 is the name of the main form - adjust as necessary
      theWindow.setCollectionBehavior(theWindow.collectionBehavior or NSWindowCollectionBehaviorCanJoinAllSpaces);
    end;
    

## Launch/Toggle full screen

To toggle an application window full screen for macOS 10.7+ (Lion): 
    
    
    {$modeswitch objectivec1}
    
    Uses CocoaAll ...
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      Window: NSWindow;
    begin
      Window := NSView(Form1.Handle).window;
      Window.toggleFullScreen(Nil);
    end;
    

To launch your application full screen you can call the button event handler (as below) or move the button event handler code (above) to the form's on activate event handler: 
    
    
    procedure TForm1.FormActivate(Sender: TObject);
    begin
      Form1.Button1Click(Form1);
    end;
    

## Locale settings (date & time separator, currency symbol, ...)

On macOS, like on other Unix platforms, the RTL does not load the locale settings by default. They can be initialised in three ways. See the article [Locale settings for macOS](<Locale_settings_for_macOS.md> "Locale settings for macOS") for details. 

## Accessibility for users with special needs

macOS includes a wide variety of features and assistive technologies, such as screen and cursor magnification, a full-featured screen reader, visual flash alerts, closed captioning support, and much more. Take advantage of these features to make your apps accessible to users with special needs. Please refer to the [Accessibility](<LCL_Accessibility.md> "LCL Accessibility") article for more details. 

## LCL FontDialog workaround

The FontDialog.OnApplyClicked event handler is not supported in macOS. So this code will have no effect (the message will not be shown): 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      FontDialog1.Execute;
    end;
     
    procedure TForm1.FontDialog1ApplyClicked(Sender: TObject);
    begin
      ShowMessage('Apply Clicked');
    end;
    

The workaround is to restructure the code as follows: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      if FontDialog1.Execute then
        begin
          ShowMessage('Apply Clicked');
        end;
    end;
    

## Coping with missing features across macOS versions

For an example of how to cope elegantly with new features in macOS which are missing from older macOS versions, see [this example](<Accessing_macOS_System_Information.md> "Accessing macOS System Information"). 

## Updating a GUI from a secondary thread

Apple’s UI frameworks, and Cocoa in particular, can only be called from the main thread. This means that if you have a background threading process, it cannot interact with the user interface because all user interface objects are marked as not safe for threading. Touching the user interface from a background thread can cause all sorts of issues, including corruption of internal state, crashes, or inexplicable incorrect behaviour. 

For example code implementing a solution to this issue, refer to the [Downloading files with graphical user interface feedback](<macOS_NSURLSession.md> "macOS NSURLSession") article. 

## Using PrintDialog in Cocoa

PrintDialog is only partially implemented (July 2020) in Cocoa. As a result, this code does not display the dialog: 
    
    
    procedure TForm1.btnPrintDialogClick(Sender: TObject);
    begin
      if PrintDialog1.Execute then
        Panel1.Invalidate;
    end;
    

This code will display the dialog: 
    
    
    procedure TForm1.btnPrintDialogClick(Sender: TObject);
    begin
      PrintDialog1.Options:= PrintDialog1.Options + [poBeforeBeginDoc];
      if PrintDialog1.Execute then
        Panel1.Invalidate;
    end;
    

## Detecting the Apple Command key

The Apple Command key is _ssMeta_. Heres' how to test for it being depressed: 
    
    
    procedure TForm1.FormKeyDown(Sender: TObject; var Key: Word; Shift: TShiftState);
    var
      ssCmd: TShiftStateEnum;
    begin
      // Define standard menu command key as ssCmd
      {$IFDEF DARWIN}
        ssCmd := ssMeta; // Apple
      {$ELSE}
        ssCmd := ssCtrl  // FreeBSD, Linux, Windows
      {$ENDIF}
    
      if (ssCmd in Shift) then
        Caption:='Command key pressed'
      else
        Caption := IntToStr(Key) + ' not the command key';
    end;
    

## Updating the GUI from a non-main thread

Any attempt to provide user feedback to the graphical user interface from within a delegate method running on a secondary background thread will cause the application to die with a log entry similar to this: 
    
    
     *** Terminating app due to uncaught exception 'NSInternalInconsistencyException',
     reason: 'NSWindow drag regions should only be invalidated on the Main Thread!'
    

The cause? Apple’s UI frameworks, and Cocoa in particular, can only be called from the main thread. Touching the user interface from a background thread can cause all sorts of issues, including corruption of internal state, crashes, or inexplicable incorrect behaviour. 

One solution may be found in the [macOS NSURLSession](<macOS_NSURLSession.md> "macOS NSURLSession") article which also has a code example. 

## Libraries

### Using a library in a Mac application

Many software projects use external libraries to complement their functionality. For example, OpenAL might be utilized to provide sound support, or FreeType might be utilized to provide font rendering support for FCL-Image. macOS already comes with a number of open source libraries installed, being that OpenAL is one of them. If one of these libraries is utilized, one should simply add the corresponding import unit to the uses clause and start using the library. To use other open source libraries which aren't included with macOS, the easiest way is by grabbing a pre-compiled library from the application bundle of popular open source applications. FreeType for example is included inside the application bundle from Gimp in the path `Gimp.app/Resources/lib/libfreetype.6.dylib`. Another option is downloading the source code from the library and building it manually, but compiling a C library is usually an unpleasant task for pure Pascal developers. 

So, again in the previous example, let's say that an application requires the FreeType library. One can grab the libfreetype.6.dylib file from Gimp and then place it in a location of our source code tree, for example `../Mac/libfreetype.dylib` relative to the path of the main project file, which is where the executable is placed. To properly link the application the Free Pascal command line option `-Fl../Mac` should be added, so that this path is added to the library search path. The linker will then find the library in this location and read the install path which is written inside it. At runtime the linker will search for the library in this install path. To change the install path to our desired one, the command line application install_name_tool, which comes with the Apple Developer Tools, can be used. It can be utilized with the following command line command: 
    
    
    install_name_tool libfreetype.dylib -id @executable_path/../Frameworks/libfreetype.dylib
    

One can then verify if this command worked by using the command: 
    
    
    otool -D libfreetype.dylib
    

It should return the written path. When building the application bundle, we should copy the libfreetype.dylib file to the Frameworks sub-directory. The _install_name_tool_ command uses the _@executable_path_ macro to set an install path _relative to the executable_ , which is inside the MacOS folder in the application bundle. Now the application can be built, the bundle and then the application can be run and it will load the library correctly from the application bundle. 

### Using a library installed using fink

For using libraries, which were installed using fink, you need to add the path to the libraries. The default is `/sw/lib`. Adding the option `-Fl/sw/lib` to the command line tells the linker where to find the libraries. The resulting application is fine for distribution with fink, but in order to run without fink, you have to make a "standalone" version. There are several possibilities to achieve this, one of them will be outlined here: 

  * Copy the .dylib file into the application bundle, for example to the `Contents/MacOS/` next to the executable binary. Then, adjust the paths install_name_tool:


  * Change the path of the library in the executable:


    
    
    install_name_tool  -change /sw/lib/libfreetype.dylib @executable_path/libfreetype.dylib @executable_path/your_executable_binary
    

  * Adjust the path in the library:


    
    
    install_name_tool  -id @executable_path/libfreetype.dylib @executable_path/libfreetype.dylib
    install_name_tool  -change /sw/lib/libfreetype.dylib @executable_path/libfreetype.dylib @executable_path/libfreetype.dylib
    

The complete list of libraries in your executable can be obtained with: 
    
    
    otool -L @executable_path/your_executable_binary | grep version | cut -f 1 -d ' '
    

For the list of libraries from fink only, without libraries from `/usr/lib` and `/System/Library`, use this command: 
    
    
    otool -L @executable_path/your_executable_binary | grep version | cut -f 1 -d ' ' | grep -v \/System\/Library | grep -v \/usr\/lib
    

With this a script can be created which loops over all libraries from fink in the executable. Be aware of the fact that the libraries can call libraries from fink. You have to adjust them as well. 

Another important point of libraries from fink is, that they have only one architecture, ie i386, ppc, aarch64, x86_64 or ppc64. For a universal application, you have to combine single architecture libraries with `lipo`. 

### Dynamic Libraries

For the details of creating and using dynamic libraries, see [macOS Dynamic Libraries](<macOS_Dynamic_Libraries.md> "macOS Dynamic Libraries") and [macOS Frameworks](<macOS_Frameworks.md> "macOS Frameworks"). 

### Static Libraries

For the details of creating and using static libraries, see [macOS Static Libraries](<macOS_Static_Libraries.md> "macOS Static Libraries"). 

### Which dynamic libraries are used?

A little known method for checking which dynamic libraries an executable uses is as simple as setting an environment variable. The macOS dynamic linker checks the following environment variables during the launch of each process. 

Environment Variable | Description   
---|---  
DYLD_PRINT_TO_FILE | This is a path to a (writable) file. Normally, the dynamic linker writes all logging output (triggered by DYLD_PRINT_* settings) to file descriptor 2 (usually stderr). This setting causes the dynamic linker to write logging output to the specified file.   
DYLD_PRINT_LIBRARIES | When this is set, the dynamic linker writes to file descriptor 2 (normally stderr) the filenames of the libraries the program is using. This is useful to make sure that your program is loading the dynamic libraries you want.   
DYLD_PRINT_RPATHS | Causes dyld to print a line each time it expands an @rpath variable and whether that expansion was successful or not.   
  
For full details of many other environment variables and their effect, see the manual page for dyld (type `man dyld` in a Terminal). 

### OpenSSL, LibreSSL, Secure Transport, Network Framework

Apple deprecated the use of the included OpenSSL libraries in Mac OS X 10.7 (Lion) in July 2011 and suggested that developers should either include the needed libraries in their application bundle or use the [Secure Transport API](<https://developer.apple.com/documentation/security/secure_transport>) which itself has now been deprecated in favour of the [Network Framework](<https://developer.apple.com/documentation/network>) introduced in macOS 10.14 (Mojave). Use of the Apple-provided OpenSSL and LibreSSL libraries by applications is strongly discouraged by Apple (see [this statement](<https://developer.apple.com/library/archive/documentation/Security/Conceptual/cryptoservices/SecureNetworkCommunicationAPIs/SecureNetworkCommunicationAPIs.html#//apple_ref/doc/uid/TP40011172-CH13-SW3>)). 

In the FPC 3.0.4 release, when trying to load the unversioned `libcrypto.dylib` or `libssl.dylib`, macOS terminates the application with the error: "Invalid dylib load. Clients should not load the unversioned libssl dylib as it does not have a stable ABI.". The fix for that bug (see [Bug #36484](<https://bugs.freepascal.org/view.php?id=36484>)) was included in FPC 3.2.0 which caused FPC to fall back to using LibreSSL v2.2 (`libssl.35.dylib`) from June 2015. 

As of the FPC 3.2.0 release, FPC breaks the ability of macOS to connect to web sites using HTTPS with TLSv1.2 or TLSv1.3 protocols only, because it fails to load the most recent Apple included LibreSSL libraries. It uses the very old LibreSSL v2.2 because the two libraries (`libssl.35.dylib` and `libcrypto.35.dylib`) for that version have matching version numbers (like OpenSSL) whereas the more recent LibreSSL library versions have mismatching version numbers. 

The issue has been fixed in FPC trunk (see [Bug #37977](<https://bugs.freepascal.org/view.php?id=37977>)) and is included in FPC 3.2.2. However, it still uses the outdated Apple-included system libraries listed in the table below. 

macOS version | OpenSSL libraries | LibreSSL libraries   
---|---|---  
10.8, 10.9, 10.10 | libssl.0.9.7.dylib + libcrypto.0.9.7.dylib  
libssl.0.9.8.dylib + libcrypto.0.9.8.dylib | \- -   
10.11 | libssl.0.9.7.dylib + libcrypto.0.9.7.dylib  
libssl.0.9.8.dylib + libcrypto.0.9.8.dylib | libssl.35.dylib + libcrypto.35.dylib   
10.12 | libssl.0.9.7.dylib + libcrypto.0.9.7.dylib  
libssl.0.9.8.dylib + libcrypto.0.9.8.dylib | libssl.35.dylib + libcrypto.35.dylib  
libssl.39.dylib + libcrypto.38.dylib   
10.13 | libssl.0.9.7.dylib + libcrypto.0.9.7.dylib  
libssl.0.9.8.dylib + libcrypto.0.9.8.dylib | libssl.35.dylib + libcrypto.35.dylib  
libssl.39.dylib + libcrypto.38.dylib  
libssl.43.dylib + libcrypto.41.dylib   
10.14 | libssl.0.9.7.dylib + libcrypto.0.9.7.dylib  
libssl.0.9.8.dylib + libcrypto.0.9.8.dylib | libssl.35.dylib + libcrypto.35.dylib  
libssl.43.dylib + libcrypto.41.dylib  
libssl.44.dylib + libcrypto.42.dylib   
10.15 | libssl.0.9.7.dylib + libcrypto.0.9.7.dylib  
libssl.0.9.8.dylib + libcrypto.0.9.8.dylib | libssl.35.dylib + libcrypto.35.dylib  
libssl.43.dylib + libcrypto.41.dylib  
libssl.44.dylib + libcrypto.42.dylib  
libssl.46.dylib + libcrypto.44.dylib   
11 | libssl.0.9.7.dylib + libcrypto.0.9.7.dylib  
libssl.0.9.8.dylib + libcrypto.0.9.8.dylib | libssl.35.dylib + libcrypto.35.dylib  
libssl.43.dylib + libcrypto.41.dylib  
libssl.44.dylib + libcrypto.42.dylib  
libssl.46.dylib + libcrypto.44.dylib   
12 | libssl.0.9.7.dylib + libcrypto.0.9.7.dylib  
libssl.0.9.8.dylib + libcrypto.0.9.8.dylib | libssl.35.dylib + libcrypto.35.dylib  
libssl.43.dylib + libcrypto.41.dylib  
libssl.44.dylib + libcrypto.42.dylib  
libssl.46.dylib + libcrypto.44.dylib  
libssl.48.dylib + libcrypto.46.dylib   
![<translate> Warning: </translate>](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/OOjs_UI_icon_notice-destructive.svg/18px-OOjs_UI_icon_notice-destructive.svg.png) **Warning**|  It should be noted that the most recent LibreSSL is v3.4.3 (March 15, 2022) and that the most recent Apple-included version in macOS is (in March 2022) a much older v2.8.3 from December 2018 and that a number of security vulnerabilities have emerged between December 2018 and March 2022. It is also worth mentioning that LibreSSL is not 100% ABI (application binary interface) compatible with OpenSSL.  
---|---  
  
All the more reason to **not** use the macOS supplied library versions and either include your own up-to-date library versions or use the Apple Network Framework when you need direct access to protocols like TLS, TCP, and UDP for your custom application protocols. For details of including your own dynamic libraries in FPC or Lazarus applications, refer to [macOS Dynamic Libraries](<macOS_Dynamic_Libraries.md> "macOS Dynamic Libraries"). 

You can continue to use the [NSURLSession API](<macOS_NSURLSession.md> "macOS NSURLSession"), which is built on the Apple Network Framework, for loading HTTPS-based and URL-based resources. This does not use the OpenSSL or LibreSSL system-included libraries. 

#### OpenSSL versions

As at September 2023, the latest OpenSSL stable version is the 3.0 series which is supported until 7th September 2026. This is also a Long Term Support (LTS) version. Note that OpenSSL 3.0+ has significant ABI changes. 

The previous LTS version OpenSSL 1.1.1 was on life support until 11th September 2023 at which time all support ceased (see [OpenSSL 1.1.1 End of Life](<https://www.openssl.org/blog/blog/2023/09/11/eol-111/>)), so no more bug fixes for security problems. 

All older OpenSSL versions (including 1.1.0, 1.0.2, 1.0.0, 0.9.8 and 0.9.7) have been out of support for much longer, contain multiple security vulnerabilities and should not be used if you value security and your reputation. 

## Application deployment/distribution

### Creating Universal binaries

#### aarch64 or Intel x86_64

Lazarus will create an application tuned for a single CPU. With the [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") version of the [LCL](<LCL.md> "LCL"), this is either Apple Silicon aarch64 or Intel x86_64. You may want to distribute your application as a 'Universal Binary', allowing users that may have either an Apple Silicon ARM64 processor or Intel processor to use the application. 

For the fun details details of creating universal binaries for these two processor architectures, see [Creating a universal binary for aarch64 and x86_64](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers"). 

#### PowerPC or Intel i386

Lazarus will create an application tuned for a single CPU. With the [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") version of the [LCL](<LCL.md> "LCL"), this is either [PowerPC](<PowerPC.md> "PowerPC") or Intel i386 (32 bit). You may want to distribute your application as a 'Universal Binary', allowing users that may have either a PowerPC or Intel computer to use the application. To do this, you need to compile two versions of your application: one with PowerPC as the target CPU and one as Intel. Next, you need to stitch these two executables together using the macOS program "lipo" (installed with the other Xcode developer tools required by Lazarus). For example, consider two versions of the application 'myprogram'. One in the 'ppc' folder compiled for PowerPC and one in the 'intel' folder compiled for Intel CPUs. You can then combine these two into a universal application by running this command: 
    
    
     lipo -create ./ppc/myproj ./intel/myproj -output ./myproj
    

Now, copy the newly created application myproj to the MacOS folder inside your [Application Bundle](<Application_Bundle.md> "Application Bundle"). 

A note on 64 bit binaries: Depending on the version of macOS and after corresponding (cross-)compilers are installed, programs which do not use the LCL can also be built as ppc64 or x86_64 binaries, in particular command line utilities. `lipo` is used in the same way to put them into universal binaries. 

### Executable size optimisation

The normal way to optimise the size of your executable with Lazarus would be: 

1\. Project > Project Options > Compilation and Linking 

  * Check Optimisation level 3 (-O3)
  * Check Smaller rather than faster (-Os)
  * Check Smart Linkable (-CX)
  * Check Link smart (-XX)



2\. Project > Project Options > Debugging 

  * Uncheck all except Strip Symbols From Executable (-Xs)



An example (Xcode 11.3; macOS Mojave 10.14.6): 

  * MyApp compiled with Lazarus defaults (-O1) - executable size 12,011,600 bytes.
  * MyApp compiled with (-O3) - executable size - executable size 12,011,600 bytes.
  * MyApp compiled with (-O3) and (-Os) - executable size 12,011,592 bytes.
  * MyApp compiled with (-O3), (-Os) and (-CX) - executable size 12,011,592 bytes.
  * MyApp compiled with (-O3), (-Os), (-CX) and (-XX) - executable size 4,624,076 bytes!
  * MyApp compiled with (-O3), (-Os), (-CX), (-XX) and (-Xs) - executable size 4,624,076 bytes.



**Problem:** Whoa! Stripping symbols from the executable _**should have made a difference**_. The reason it did not make any difference is that Apple removed the ability to strip the symbol table from an executable in Xcode 3.1 (2007 Leopard 10.5). Previously, in Xcode 2.5 (2005 Tiger 10.4), the linker could strip the symbol table from 32 bit executables. So, providing the (-Xs) option to the linker has no effect at all. 

**Solution:** The strip command line utility will strip the symbol table. Manually running `strip MyApp` in a Terminal resulted in the executable size further reducing from 4,624,076 bytes to 2,922,396 bytes. 

### Deploying an application for an older version of the operating system

When compiling an application on a specific macOS release without any special options, the application is only guaranteed to work on that particular major operating system release and later (eg when compiling under Snow Leopard 10.6.8, the application is only guaranteed to work on Snow Leopard 10.6.8 and later). 

Here is an example of an error that you may encounter when running an application compiled for Leopard 10.5 under Tiger 10.4 in case you use the widestring manager: 
    
    
    dyld: Library not loaded: /usr/lib/libiconv.2.dylib
      Referenced from: /Volumes/..../yourprogram
      Reason: Incompatible library version: yourprogram requires version 7.0.0 or later, but libiconv.2.dylib provides version 5.0.0
    Trace/BPT trap
    

Here is an example of an error that you may encounter when running an application compiled for Snow Leopard 10.6.8 under Snow Leopard 10.6.7: 
    
    
    Runtime error 203
    

#### FPC 2.6.2 and above

See the article [Support for specifying and querying the deployment version](<FPC_New_Features_2.6.md> "FPC New Features 2.6.2") which is current up to and including FPC 3.04. 

For FPC 3.2 (aka fixes branch) see the article [Default target macOS version](<User_Changes_3.md> "User Changes 3.2"). 

**Adding custom options only when compiling for macOS**

To add the above custom options only when compiling for macOS add to Project / Compiler options / IDE macros / Conditionals: 
    
    
    if TargetOS = 'darwin' then begin
      UsageCustomOptions += ' -WM10.6';
    end;
    

#### FPC 2.6.1 and below

See the [FPC 2.6.1 macOS Targets](<FPC_2.6.md> "FPC 2.6.1 macOS Targets") article. 

### Creating an app bundle for a GTK application

macOS needs an app bundle for an executable file in order to drop it on the dock or launch it by double-clicking. An app bundle is really just a special folder that can have the same name as the executable, but with an .app extension. Finder doesn't display the .app extension, although you'll see it if you use the UNIX `ls` command in a Terminal window to list the directory contents. For details of how to create an app bundle for a GTK application, see [Creating an app bundle for GTK applications](<Creating_an_app_bundle_for_GTK_applications.md> "Creating an app bundle for GTK applications"). 

### Code Signing

Before macOS Sierra users had the option to allow the installation of applications from "anywhere", "the App Store" and "identified developers". From macOS Sierra on, the "anywhere" option was removed. Thereafter, clicking on an application to run it results in a scary dialog being presented to your users stating: "YourApp can't be opened because it is from an unidentified developer. Your security preferences allow installations of only apps from the App Store and identified developers". The only dialog option is to close it. 

Users can still run your application, but they have to know they can right-click or control-click on your application and then they get another scary dialog stating "YourApp is from an unidentified developer. Are you sure you want to open it?" but this time the dialog has an "open" button and a pre-focussed "cancel" button. 

[Code signing](<Code_signing.md> "Code signing") an application that has been written in Lazarus and Free Pascal requires only a few steps that are described in the article [Code Signing for macOS](<Code_Signing_for_macOS.md> "Code Signing for macOS"). Note: You will need to pay Apple $US 99 per year to be an "identified developer" and code sign your applications. 

### Notarization

Beginning with macOS Mojave 10.14.5 all new or updated kernel extensions and all software from developers new to distributing with a Developer ID must also be notarized in order to run or users are prevented from running the application with a very scary dialog stating: "YourApp can't be opened because Apple cannot check it for malicious software. This software needs to be updated. Contact the developer for more information." 

Beginning with macOS 10.15 Catalina (released in October 2019), notarization is required by default for all software. If your application is not notarised, your users get the very scary dialog stating: "YourApp can't be opened because Apple cannot check it for malicious software. This software needs to be updated. Contact the developer for more information." This is in addition to requiring the application to be code signed. 

The steps to notarize an application are described in the article [Notarization for macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+"). 

### Deploying your application

See [macOS Deployment](<Deploying_Your_Application.md> "Deploying Your Application"). 

## Using stdout and stderr for logging and debugging

### Using a Terminal

Unix systems usually allow output to `stdout` and `stderr` for GUI applications (while for Windows it requires creating a console object, using the file handle as `stdout/stderr` or linking the application as a non-GUI). The output is really useful for debugging and logging purposes. 

macOS GUI applications (the ones created by Lazarus) are deployed as [bundles](<http://en.wikipedia.org/wiki/Application_bundle>). The bundled application conceals the output. However, it is possible to see the output generated by a simple `WriteLn('Debug: Test')` or `WriteLn(stderr, 'Debug: error alert!');`, by launching the application bundle's executable from a Terminal. Here are the steps: 

  * Launch Terminal (found in Applications > Utilities)
  * Change to the project directory
  * Launch the executable:


    
    
     $ ./MyApp.app/Contents/MacOS/MyApp
    

  * once MyApp is running, the output will be seen in the Terminal window.



[![OSXStdOutExample.png](https://wiki.freepascal.org/images/0/0b/OSXStdOutExample.png)](</File:OSXStdOutExample.png>)

This is especially useful when using Project Options > Compiler Options > Debugging when you have ticked the Other debugging info - Use Heaprtc unit (check for mem-leaks). The Terminal will show the report: 
    
    
      Heap dump by heaptrc unit of ./MyApp
      1441 memory blocks allocated : 99080/102040
      1441 memory blocks freed     : 99080/102040   
      0 unfreed memory blocks : 0
      True heap size : 458752 (32 used in System startup)
      True free heap : 458720
    

### Using the Console

It is also possible to see the output by using the native macOS `NSLog()` procedure which logs your error message to the Apple System Log facility. You can read the System Log in realtime using the Console utility (located in Applications > Utilities). As there will be many message entries scrolling by, add MyApp as a process to the search window to filter the entries being shown. For example: 
    
    
      MyAudioPlayer := AVAudioPlayer.alloc.initWithContentsOfURL_error(url, @err);
    
      if Assigned(MyAudioPlayer) then
          MyAudioPlayer.play
      else
        NSLog(NSStr('Error in procedure PlayAudio(): %@'), err);
    

When the audio file is missing from the application's Resources directory, the following appears in the Console system log: 

[![NSLog Console.png](https://wiki.freepascal.org/images/1/15/NSLog_Console.png)](</File:NSLog_Console.png>)

Note that the NSlog information also appears in the Terminal if you have opened the application as explained above. 

## Troubleshooting

The following articles are relevant for troubleshooting application issues: 

  * [Cocoa debugging](<Cocoa_Debugging.md> "Cocoa Debugging")
  * [Debugging with the macOS lldb debugger](<lldb.md> "lldb")
  * [macOS Gatekeeper Debugging Tips](<macOS_Gatekeeper_Debugging_Tips.md> "macOS Gatekeeper Debugging Tips")
  * [LazLogger (part of Lazarus) will log to a file](<LazLogger.md> "LazLogger")
  * [Logging exceptions](<Logging_exceptions.md> "Logging exceptions")
  * [Memory leak checking](<heaptrc.md> "heaptrc")



Seek help in the forums: 

  * [How to use the Forums effectively](<Forum.md> "Forum")
  * [FPC and Lazarus Forums](<https://forum.lazarus.freepascal.org>)



Obscure native macOS error codes? 

  * [Look up Apple API errors quickly!](<https://OSStatus.com>)



Finally, if the issue is in Free Pascal or Lazarus and not your own code: 

  * [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report")
  * [Tips on writing bug reports](<Tips_on_writing_bug_reports.md> "Tips on writing bug reports")



## Converting legacy Pascal code

Bxxx code macro replacements: 

  * BAnd(i,j) = ((i) and (j))
  * BOr(i,j) = ((i) or (j))
  * BXOr(i,j) = ((i) xor (j))
  * Bsr(i,n) = ((i) shr (n))
  * Bsl(i,n) = ((i) shl (n))
  * BTST(i,n) = ((((i) shr (n)) and 1)=1)
  * BSet(i,n) = i:=(i) or (1 shl (n))
  * BClr(i,n) = i:=(i) and not (1 shl (n))



See also: [Porting from Mac Pascal](<Porting_from_Mac_Pascal.md> "Porting from Mac Pascal"). 

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
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
  * macOS Programming Tips \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## See also

  * [Lazarus/FPC macOS Portal](<Portal_Mac.md> "Portal:Mac") \- more macOS tips and code snippets


  * [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS")



## External Links

  * [Apple developer documents](<https://developer.apple.com/documentation/>) \- current and up to date


  * [Apple archived developer documents](<https://developer.apple.com/library/archive/navigation/>) \- dated, but often more helpful


  * [Making Your Lazarus App Mac-Friendly](<https://macpgmr.github.io/MacXPlatform/MacXPlatform_Part8.html>) (Apple help info outdated)


  * Using Xcode for Pascal projects, refer to [this page](<https://macpgmr.github.io/ObjP/ProjectXC.html>).

---

_Source: [https://wiki.freepascal.org/macOS_Programming_Tips](https://web.archive.org/web/20250328094840/https://wiki.freepascal.org/macOS_Programming_Tips)_
