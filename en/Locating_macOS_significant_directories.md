# Locating macOS significant directories

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/Locating_macOS_significant_directories.md>)** │

## Contents

  * 1 Overview
  * 2 Determining where to store files
  * 3 Get significant directories function
  * 4 Table of Directory Locations
  * 5 Table of Path Domains
  * 6 Code Examples
    * 6.1 User caches directory
    * 6.2 User trash directory
    * 6.3 Users directory
    * 6.4 All application directories
    * 6.5 All user application directories
    * 6.6 All system application directories
  * 7 Replacements for FPC's GetAppConfigDir and GetAppConfigFile
  * 8 See also



## Overview

To locate various significant directories in macOS you can use the native macOS Foundation function 
    
    
    NSSearchPathForDirectoriesInDomains(NSSearchPathDirectory directory, NSSearchPathDomainMask domainMask, BOOL expandTilde);
    

which creates a list of path strings for the specified directories in the specified domains. The list is in the order in which you should search the directories. 

Note: The directory returned by this method may not exist. This method simply gives you the appropriate location for the requested directory. Depending on the application’s needs, it may be up to the developer to create the appropriate directory and any in between. 

## Determining where to store files

Before we go any further, let's recap where the Apple Guidelines indicate your application should store its files: 

  * Use the `/Applications` or `/Applications/Utilities` directory for the [application bundle](<Application_Bundle.md> "Application Bundle"). The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities directory`, it should be able to run. Installing to these folders requires Admin privileges. The data in these folders is backed up by Time Machine.


  * Use the `~/Applications` directory should Admin privileges not be available. This is the standard location for a single user application. This directory should not be expected to exist. The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities` directory, it should be able to run. This data is backed up by Time Machine.


  * Use the [Application Support directory](<Locating_macOS_app_support,_preferences_folders.md> "Locating macOS app support, preferences folders") (this data is backed up by Time Machine), appending your <bundle_ID>, for: 
    * Resource and data files that your application creates and manages for the user. You might use this directory to store application state information, computed or downloaded data, or even user created data that you manage on behalf of the user.
    * Autosave files.


  * Use the Caches directory (this is **not** backed up by Time Machine), appending your <bundle_ID>, for cached data files or any files that your application can recreate easily.


  * Use [CFPreferences](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write") to read and write your application's preferences. This will automatically write preferences to the appropriate location and read them from the appropriate location. This data is backed up by Time Machine.


  * Use the [application Resources directory](<Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory") (this is backed up by Time Machine) for your application-supplied image files, sound files, icon files and other unchanging data files necessary for your application's operation.


  * Use [NSTemporaryDirectory](<Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory") (this is **not** backed up by Time Machine) to store temporary files that you intend to use immediately for some ongoing operation but then plan to discard later. Delete temporary files as soon as you are done with them.



## Get significant directories function
    
    
    {$mode objfpc}{H+}
    {$modeswitch objectivec1} 
    
    interface
    
    Uses
      ...
      CocoaUtils,   // for NStringToString
      CocoaAll;     // for NSArray      
    ...
    
    function GetSignificantDir(DirLocation: qword; DomainMask: qword; count: byte): string;
    var
      paths : NSArray;
    begin
      paths := NSSearchPathForDirectoriesInDomains(DirLocation, DomainMask, True);
      if(count < paths.count) then
        Result := NSString(paths.objectAtIndex(0)).UTF8String
      else
        Result := '';  
    end;
    

## Table of Directory Locations

Directory Locations  Directory name | Directory Description   
---|---  
NSApplicationDirectory | Supported applications (/Applications)   
NSDemoApplicationDirectory | Unsupported applications and demonstration versions   
NSDeveloperApplicationDirectory | Developer applications (/Developer/Applications)   
NSAdminApplicationDirectory | System and network administration applications   
NSLibraryDirectory | Various user-visible documentation, support, and configuration files (/Library)   
NSDeveloperDirectory | Developer resources (/Developer)   
NSUserDirectory | User home directories (/Users)   
NSDocumentationDirectory | Documentation   
NSDocumentDirectory | Document directory   
NSCoreServiceDirectory | Core services (System/Library/CoreServices)   
NSAutosavedInformationDirectory | The user’s autosaved documents (Library/Autosave Information)   
NSDesktopDirectory | The user’s desktop directory   
NSCachesDirectory | Discardable cache files (Library/Caches)   
NSApplicationSupportDirectory | Application support files (Library/Application Support)   
NSDownloadsDirectory | The user’s downloads directory   
NSInputMethodsDirectory | Input Methods (Library/Input Methods)   
NSMoviesDirectory | The user’s Movies directory (~/Movies)   
NSMusicDirectory | The user’s Music directory (~/Music)   
NSPicturesDirectory | The user’s Pictures directory (~/Pictures)   
NSPrinterDescriptionDirectory | The system’s PPDs directory (Library/Printers/PPDs)   
NSSharedPublicDirectory | The user’s Public sharing directory (~/Public)   
NSPreferencePanesDirectory | The PreferencePanes directory for use with System Preferences (Library/PreferencePanes)   
NSApplicationScriptsDirectory | The user scripts folder for the calling application (~/Library/Application Scripts/<code-signing-id>  
NSItemReplacementDirectory | The constant used to create a temporary directory   
NSAllApplicationsDirectory | All directories where applications can be stored   
NSAllLibrariesDirectory | All directories where resources can be stored   
NSTrashDirectory | The user's trash directory   
  
## Table of Path Domains

Path Domains  Domain name | Domain Description   
---|---  
NSUserDomainMask | The user’s home directory—the place to install user’s personal items (~).   
NSLocalDomainMask | The place to install items available to everyone on this machine   
NSNetworkDomainMask | The place to install items available on the network (/Network)   
NSSystemDomainMask | A directory for system files provided by Apple (/System)   
NSAllDomainsMask | All domains   
  
  


## Code Examples

You can now locate various significant directories and display them as demonstrated by the code examples below. 

### User caches directory
    
    
    procedure TForm1.MenuItem5Click(Sender: TObject);
    begin
      ShowMessage('User caches dir: ' + GetSignificantDir(NSCachesDirectory,NSUserDomainMask,0));
    end;
    

### User trash directory
    
    
    procedure TForm1.MenuItem16Click(Sender: TObject);
    begin
      ShowMessage('User trash dir: '
              + GetSignificantDir(NSTrashDirectory,NSUserDomainMask,0));
    end;
    

### Users directory

Maybe not what you expect... this is the _/Users_ directory :-) 
    
    
    procedure TForm1.MenuItem10Click(Sender: TObject);
    begin
      ShowMessage('Users dir: ' +  GetSignificantDir(NSUserDirectory,NSLocalDomainMask,0));
    end;
    

To retrieve the current user's home directory, you need to use the _NSHomeDirectory()_ function instead: 
    
    
    procedure TForm1.MenuItem11Click(Sender: TObject);
    begin
      ShowMessage('User''s dir: ' + NSStringToString(NSHomeDirectory));
    end;
    

**BUT** note that if the application is in a sandbox, this will return the application’s [sandbox](<Sandboxing_for_macOS.md> "Sandboxing for macOS") directory and not the current user’s home directory. 

An alternative way to retrieve the current user's hime directory: 
    
    
    function NSUserName: CFStringRef external name '_NSUserName';
    ...
    
    procedure TForm1.MenuItem22Click(Sender: TObject);
    var
      usernameStr: ShortString;
      status: Boolean = false;
    begin
       status := CFStringGetPascalString(CFStringRef(NSusername),@usernameStr,255,CFStringGetSystemEncoding);
      if(status = true) then
         ShowMessage('Current user''s dir: ' + NSStringToString(NShomeDirectoryForUser(NSStr(usernameStr))))
      else
         ShowMessage('Error retrieving username'); 
    end;
    

### All application directories
    
    
    procedure TForm1.MenuItem7Click(Sender: TObject);
    var
      count: byte;
    begin
      for count := 0 to 12 do
       if GetSignificantDir(NSAllApplicationsDirectory, NSAllDomainsMask, count) <> '' then
          ShowMessage('Application dir ' + IntToStr(count) + ': '
               + GetSignificantDir(NSAllApplicationsDirectory,NSAllDomainsMask,count))
       else
          exit; 
    end;
    

### All user application directories
    
    
    procedure TForm1.MenuItem8Click(Sender: TObject);
    var
      count: byte;
    begin
      for count := 0 to 12 do
       if GetSignificantDir(NSAllApplicationsDirectory, NSUserDomainMask, count) <> '' then
          ShowMessage('User application dir ' + IntToStr(count) + ': '
               + GetSignificantDir(NSAllApplicationsDirectory,NSUserDomainMask,count))
       else
          exit; 
    end;
    

### All system application directories
    
    
    procedure TForm1.MenuItem9Click(Sender: TObject);
    var
      count: byte;
    begin
      for count := 0 to 12 do
       if GetSignificantDir(NSAllApplicationsDirectory, NSSystemDomainMask, count) <> '' then
          ShowMessage('System application dir ' + IntToStr(count) + ': '
               + GetSignificantDir(NSAllApplicationsDirectory,NSSystemDomainMask,count))
       else
          exit; 
    end;
    

## Replacements for FPC's GetAppConfigDir and GetAppConfigFile

The existing FPC `GetAppConfigDir` and `GetAppConfigFile` functions are not macOS friendly because they use the UNIX convention and **not** the Apple macOS convention. For example, the output from these functions: 
    
    
    writeLn(GetAppConfigDir(true));
    writeLn(GetAppConfigDir(false));
    
    writeLn(GetAppConfigFile(true));
    writeLn(GetAppConfigFile(false));
    
    writeLn(GetAppConfigFile(true,true));
    writeLn(GetAppConfigFile(true,false));
    
    writeLn(GetAppConfigFile(false,false));
    writeLn(GetAppConfigFile(false,true));
    

is this: 
    
    
    /etc/
    /Users/<username>/.config/
    
    /etc/project1.cfg
    /Users/<username>/.config/project1.cfg
    
    /etc/project1/project1.cfg
    /etc/project1.cfg
    
    /Users/<username>/.config/project1.cfg
    /Users/<username>/.config/project1/project1.cfg
    

whereas the correct output for a macOS command line application (ie without an [Application Bundle](<Application_Bundle.md> "Application Bundle")) is: 
    
    
    /Library/Application Support/project1/
    /Users/<username>/Library/Application Support/project1/
    
    /Library/Application Support/project1/project1.plist
    /Users/<username>/Library/Application Support/project1/project1.plist
    
    /Library/Application Support/project1/project1.plist
    /Library/Application Support/project1/project1.plist
    
    /Users/<username>/Library/Application Support/project1/project1.plist
    /Users/<username>/Library/Application Support/project1/project1.plist
    

and the correct output for a macOS GUI application (ie with an [Application Bundle](<Application_Bundle.md> "Application Bundle")): 
    
    
    /Library/Application Support/com.company.project1/
    /Users/<username>/Library/Application Support/com.company.project1/
    
    /Library/Application Support/com.company.project1/project1.plist
    /Users/<username>/Library/Application Support/com.company.project1/project1.plist
    
    /Library/Application Support/com.company.project1/project1.plist
    /Library/Application Support/com.company.project1/project1.plist
    
    /Users/<username>/Library/Application Support/com.company.project1/project1.plist
    /Users/<username>/Library/Application Support/com.company.project1/project1.plist
    

Here are the replacement functions -- named with the addition of a 2 to the existing function names -- and with ${IFDEF}s so that they can be used in multi-platform code: 
    
    
    {$mode objfpc}{H+}
    {$modeswitch objectivec1}
    
    ...
    
    Uses
      ...
      SysUtils,
      CocoaAll,
      Cocoautils;
    ...
    
    //
    // GetAppConfigDir Replacement
    //
    
    function GetAppConfigDir2(Global: Boolean): String;
    
    {$IFDEF DARWIN}
    var
      bundleIdStr: String;
      pathsArr : NSArray;
    {$ENDIF}
    begin
      {$IFDEF DARWIN}
      bundleIdStr := NSStringToString(NSBundle.mainBundle.bundleIdentifier);
      if(bundleIdStr = '') then
         bundleIdStr := ExtractFileName(paramstr(0)); // Handle non-bundled apps eg command line
    
      if(Global) then
        pathsArr := NSSearchPathForDirectoriesInDomains(NSApplicationSupportDirectory, NSLocalDomainMask, True)
      else
        pathsArr := NSSearchPathForDirectoriesInDomains(NSApplicationSupportDirectory, NSUserDomainMask, True);
    
      Result := NSString(pathsArr.objectAtIndex(0)).Utf8String + '/' + bundleIdStr + '/';
      {$ELSE}
        GetAppConfigDir(global);
      {$ENDIF}
    end;  
    
    //    
    // GetAppConfigFile replacement
    //
    
    Function GetAppConfigFile2(Global : Boolean; SubDir : Boolean) : String;
    
    {$IFDEF DARWIN}
    var
      bundleIdStr: ShortString;
      appNameStr: ShortString;
      pathBufferStr: String;
      isAppBundle : Boolean = True;
    {$ENDIF}
    begin
      {$IFDEF DARWIN}
      bundleIdStr := NSStringToString(NSBundle.mainBundle.bundleIdentifier);
      if(bundleIdStr = '') then
        begin
         bundleIdStr := ExtractFileName(paramstr(0)); // Handle non-bundled apps eg command line
         isAppBundle := False;
        end;
    
      pathBufferStr := GetAppConfigDir2(Global);
    
      if(isAppBundle) then
        appNameStr := StringReplace(ExtractFileExt(bundleIdStr), '.', '', [rfReplaceAll])
      else
        appNameStr := extractFileName(ExcludeTrailingPathDelimiter(pathBufferStr));
    
      if(SubDir) then
        // Ignore because Apple dictates the subdir already as part of GetAppConfigDir()
        ;
    
      Result := pathBufferStr + appNameStr + '.plist';
    
      {$ELSE}
      Result := GetAppConfigFile(Global, SubDir);
      {$ENDIF}
    end;
    

## See also

  * [Locating macOS app support, preferences folders](<Locating_macOS_app_support,_preferences_folders.md> "Locating macOS app support, preferences folders")
  * [Locating the macOS application resources directory](<Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory")
  * [Locating the macOS tmp directory](<Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory")

---

_Source: [https://wiki.freepascal.org/Locating_macOS_significant_directories](https://web.archive.org/web/20240910072729/https://wiki.freepascal.org/Locating_macOS_significant_directories)_
