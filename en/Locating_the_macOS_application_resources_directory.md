# Locating the macOS application resources directory

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Resources are data files that live outside of your application’s executable file. The resources directory is where you should store these files (eg your image files, sound files, icon files and other data files necessary for your application's operation). The contents of this directory may be further divided into subdirectories where you can store localized and non-localized versions of your resource files. 

## Contents

  * 1 macOS File Storage Overview
  * 2 Application Resources Directory
  * 3 Using a CFBundle object
  * 4 Using an NSBundle object
  * 5 See also
  * 6 External links



## macOS File Storage Overview

Before we go any further, let's recap where the Apple Guidelines indicate your application should store its files: 

  * Use the `/Applications` or `/Applications/Utilities` directory for the [application bundle](<Application_Bundle.md> "Application Bundle"). The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities directory`, it should be able to run. Installing to these folders requires Admin privileges. The data in these folders is backed up by Time Machine.


  * Use the `~/Applications` directory should Admin privileges not be available. This is the standard location for a single user application. This directory should not be expected to exist. The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities` directory, it should be able to run. This data is backed up by Time Machine.


  * Use the [Application Support directory](<Locating_macOS_app_support,_preferences_folders.md> "Locating macOS app support, preferences folders") (this data is backed up by Time Machine), appending your <bundle_ID>, for: 
    * Resource and data files that your application creates and manages for the user. You might use this directory to store application state information, computed or downloaded data, or even user created data that you manage on behalf of the user.
    * Autosave files.


  * Use the [Caches directory](<Locating_macOS_significant_directories.md> "Locating macOS significant directories") (this is **not** backed up by Time Machine), appending your <bundle_ID>, for cached data files or any files that your application can recreate easily.


  * Use [CFPreferences](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write") to read and write your application's preferences. This will automatically write preferences to the appropriate location and read them from the appropriate location. This data is backed up by Time Machine.


  * Use the application Resources directory (this is backed up by Time Machine) for your application-supplied image files, sound files, icon files and other unchanging data files necessary for your application's operation.


  * Use [NSTemporaryDirectory](<Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory") (this is **not** backed up by Time Machine) to store temporary files that you intend to use immediately for some ongoing operation but then plan to discard later. Delete temporary files as soon as you are done with them.



## Application Resources Directory

The basic structure of the [application bundle](<Application_Bundle.md> "Application Bundle") is: 
    
    
    MyApp.app/
       Contents/
          Info.plist
          MacOS/
          Resources/
    

A more complex application bundle layout can be seen in my [Apple Help Book article](<Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app"). 

## Using a CFBundle object

CFBundle allows you to use a folder hierarchy called a bundle to organize and locate many types of application resources including images, sounds, localized strings, and executable code. 

Set out below are two ways to find the location of the Resources directory using the Core Foundation bundle object. 

The first way is to retrieve the bundle directory (ie MyApp.app/) and concatenate the standard Contents and Resources directories to create the full Resources directory path like so: 
    
    
    ...
    
    {$modeswitch objectivec1} 
    
    interface
    
    Uses
      MacOSAll;
    
    ...
    
    function GetBundlePath(): string;
    var
      pathRef: CFURLRef;
      pathCFStr: CFStringRef;
      pathStr: shortstring;
      status: Boolean = false;
    begin
      pathRef := CFBundleCopyBundleURL(CFBundleGetMainBundle());
      pathCFStr := CFURLCopyFileSystemPath(pathRef, kCFURLPOSIXPathStyle);
    
      status := CFStringGetPascalString(pathCFStr, @pathStr, 255, CFStringGetSystemEncoding());
    
      if(status = true) then
        Result := pathStr
      else
        raise Exception.Create('Error in GetBundlePath()');
    end;
    
    ...
    
    procedure TForm1.MenuItem1Click(Sender: TObject);
    begin
      ShowMessage(GetBundlePath + PathDelim + 'Contents' + PathDelim + 'Resources' + PathDelim);
    end;
    

The second way is to actually interrogate the application bundle for the full path to its Resources directory without the need to concatenate any directories as shown here: 
    
    
    ...
    
    {$modeswitch objectivec1} 
    
    interface
    
    Uses
      ...
      MacOSAll,
      CocoaAll;
    
    ...
    
    function GetResourcesPath(): string;
    var
      pathStr: shortstring;
      status: Boolean = false;
    begin
      status := CFStringGetPascalString(CFStringRef(NSBundle.mainBundle.resourcePath), @pathStr, 255, CFStringGetSystemEncoding());
    
      if(status = true) then
        Result := pathStr + PathDelim
      else
        raise Exception.Create('Error in GetResourcesPath()');
    end;
    
    ...
    
    procedure TForm1.MenuItem1Click(Sender: TObject);
    begin
      ShowMessage(GetResourcesPath);
    end;
    

If you just want to retrieve the directory in which your application bundle is located, it is a simple matter of extracting it from the `GetBundlePath()` function above like this: 
    
    
    ...
    
    Uses
      SysUtils,
      ...
    
    ...
    
    procedure TForm1.MenuItem1Click(Sender: TObject);
    begin
      ShowMessage(ExtractFilePath(GetBundlePath));
    end;
    

## Using an NSBundle object

Apple uses bundles to represent applications, frameworks, plug-ins, and many other specific types of content. Bundles organize their contained resources into well-defined subdirectories, and bundle structures vary depending on the platform and the type of the bundle. By using a bundle object, you can access a bundle's resources without knowing the structure of the bundle. The bundle object provides a single interface for locating items, taking into account the bundle structure, user preferences, available localizations, and other relevant factors. Any executable can use a bundle object to locate resources, either inside an application’s bundle or in a known bundle located elsewhere. You don't use a bundle object to locate files in a container directory or in other parts of the file system. 

The unit below demonstrates how to find the application bundle directory, the bundle resources directory and various other bundle paths using the Foundation NSBundle object. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}
    
    interface
    
    uses
      Classes,
      Forms,
      Dialogs,    // Needed for ShowMessage
      StdCtrls,   // Needed for Buttons
      SysUtils,   // Needed for pathdelimiter
      CocoaAll,   // Needed for NSBundle
      CocoaUtils; // Needed for NSStringToString
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        Button2: TButton;
        Button3: TButton;
        Button4: TButton;
        Button5: TButton;
        Button6: TButton;
        Button7: TButton;
        procedure Button1Click(Sender: TObject);
        procedure Button2Click(Sender: TObject);
        procedure Button3Click(Sender: TObject);
        procedure Button4Click(Sender: TObject);
        procedure Button5Click(Sender: TObject);
        procedure Button6Click(Sender: TObject);
        procedure Button7Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    Function GetMainBundlePath: string;
    begin
      Result := NSStringToString(NSBundle.mainBundle.bundlePath);
    end;
    
    function GetExecutablePath : string;
    begin
      Result := NSStringToString(NSBundle.mainBundle.executablePath);
    end;
    
    function GetResourcesPath : string;
    begin
      Result := NSStringToString(NSBundle.mainBundle.resourcePath) + PathDelim;
    end;
    
    Function GetSoundResourcePath(Sound: NSString): string;
    begin
      Result := NSStringToString(NSBundle.mainBundle.pathForSoundResource(Sound));
    end;
    
    Function GetBundleIdentifier : string;
    begin
      Result := NSStringToString(NSBundle.mainBundle.bundleIdentifier);
    end;
    
    Function GetSharedSupportPath : string;
    begin
      Result := NSStringToString(NSBundle.mainBundle.sharedSupportPath);
    end;
    
    Function GetPathForImageResource(Image: NSString) : string;
    begin
      Result := NSStringToString(NSBundle.mainBundle.pathForImageResource(Image));
    end;
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      ShowMessage(GetExecutablePath);
    end;
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      ShowMessage(GetResourcesPath);
    end;
    
    procedure TForm1.Button3Click(Sender: TObject);
    begin
      ShowMessage(GetMainBundlePath);
    end;
    
    procedure TForm1.Button4Click(Sender: TObject);
    begin
      // Find path to the Glass.aiff sound file in the resources directory
      ShowMessage(GetSoundResourcePath(NSStr('Glass')));
    end;
    
    procedure TForm1.Button5Click(Sender: TObject);
    begin
      ShowMessage(GetBundleIdentifier);
    end;
    
    procedure TForm1.Button6Click(Sender: TObject);
    begin
      ShowMessage(GetSharedSupportPath);
    end;
    
    procedure TForm1.Button7Click(Sender: TObject);
    begin
      // Find path to world1.png in resources directory
      ShowMessage(GetPathForImageResource(NSStr('world1')));
    end;
    
    end.
    

You can also simplify the above by omitting the CocoaUtils unit (losing access to NSStringToString) and use the UTF8String function. For example: 
    
    
    function GetExecutablePath : string;
    begin
      //Result := NSStringToString(NSBundle.mainBundle.executablePath);
      Result := NSBundle.mainBundle.executablePath.UTF8String;
    end;
    

## See also

  * [Locating macOS significant directories](<Locating_macOS_significant_directories.md> "Locating macOS significant directories")
  * [Locating macOS app support, preferences folders](<Locating_macOS_app_support,_preferences_folders.md> "Locating macOS app support, preferences folders")
  * [Locating the macOS tmp directory](<Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory")



## External links

  * [Apple: Bundle Programming Guide](<https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/BundleTypes/BundleTypes.html>).
  * [Apple: Core Foundation CFBundle Overview](<https://developer.apple.com/documentation/corefoundation/cfbundle-s0a>).
  * [Apple: NSBundle class](<https://developer.apple.com/documentation/foundation/nsbundle>).

---

_Source: [https://wiki.freepascal.org/Locating_the_macOS_application_resources_directory](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Locating_the_macOS_application_resources_directory)_
