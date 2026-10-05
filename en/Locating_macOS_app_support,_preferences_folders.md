# Locating macOS app support, preferences folders

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Application Support and Preferences Folders
  * 3 Actual preference folder locations
  * 4 See also



## Overview

Before we go any further, let's recap where the Apple Guidelines indicate your application should store its files: 

  * Use the `/Applications` or `/Applications/Utilities` directory for the [application bundle](<Application_Bundle.md> "Application Bundle"). The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities directory`, it should be able to run. Installing to these folders requires Admin privileges. The data in these folders is backed up by Time Machine.


  * Use the `~/Applications` directory should Admin privileges not be available. This is the standard location for a single user application. This directory should not be expected to exist. The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities` directory, it should be able to run. This data is backed up by Time Machine.


  * Use the Application Support directory (this data is backed up by Time Machine), appending your <bundle_ID>, for: 
    * Resource and data files that your application creates and manages for the user. You might use this directory to store application state information, computed or downloaded data, or even user created data that you manage on behalf of the user.
    * Autosave files.


  * Use the [Caches directory](<Locating_macOS_significant_directories.md> "Locating macOS significant directories") (this is **not** backed up by Time Machine), appending your <bundle_ID>, for cached data files or any files that your application can recreate easily.


  * Use [CFPreferences](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write") to read and write your application's preferences. This will automatically write preferences to the appropriate location and read them from the appropriate location. This data is backed up by Time Machine.


  * Use the [application Resources directory](<Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory") (this is backed up by Time Machine) for your application-supplied image files, sound files, icon files and other unchanging data files necessary for your application's operation.


  * Use [NSTemporaryDirectory](<Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory") (this is **not** backed up by Time Machine) to store temporary files that you intend to use immediately for some ongoing operation but then plan to discard later. Delete temporary files as soon as you are done with them.



## Application Support and Preferences Folders

The function below will return the location the macOS user/global application support directories and the user/global application preferences directories. You would not normally need to manually locate the macOS user preferences directory because the [CFPreferences read/write methods](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write") will do this for you automatically. 

As a bonus, the function below will also return the user/global application support directories for other operating systems. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** While the **FSFindFolder()** function was deprecated by Apple after macOS 10.8 (Mountain Lion), it is still available in macOS 10.15 (Catalina), In the interests of longevity, you may prefer to use the **NSSearchPathForDirectoriesInDomains()** function instead which is available in macOS 10.0+. See [Locating macOS significant directories](<Locating_macOS_significant_directories.md> "Locating macOS significant directories") for details.
    
    
    ...
    
    Uses
      {$IFDEF DARWIN}
      MacOSAll, Files
      {$ENDIF}
    
    ...
    
      {platform-independent method to retrieve application support/preferences directories
      
       Global: true (global location) _or_ false (user location)
       FolderType: kApplicationSupportFolderType _or_ kPreferencesFolderType
      
       User preferences live in the location returned by FSFindFolder(kUserDomain,kPreferencesFolderType).
       User application support items live in the location returned by FSFindFolder(kUserDomain, kApplicationSupportFolderType).
       Global preferences live in the location returned by FSFindFolder(kLocalDomain,kPreferencesFolderType).
       Global application support items live in the location returned by FSFindFolder(kLocalDomain, kApplicationSupportFolderType).
      }
    
    function GetSupportDir(Global:boolean; FolderType:LongWord): String;
    
    {$IFDEF DARWIN}
    const
      kMaxPath = 1024;
    var
      theError: OSErr;
      theRef: FSRef;
      pathBuffer: PChar;
    {$ENDIF}
    begin
      {$IFDEF DARWIN}
        theRef := Default(FSRef);   // init 
    
        try
          pathBuffer := Allocmem(kMaxPath);
        except on exception 
          do exit;
        end;
    
        try
          Fillchar(pathBuffer^, kMaxPath, #0);  // actually already done by allocmem
          Fillchar(theRef, Sizeof(theRef), #0); // actually already done by Default();
          if Global then   // kLocalDomain
            theError := FSFindFolder(kLocalDomain, FolderType, kDontCreateFolder, theRef)
          else             // kUserDomain
            theError := FSFindFolder(kUserDomain , FolderType, kDontCreateFolder, theRef);
          if (pathBuffer <> nil) and (theError = noErr) then
            begin
              theError := FSRefMakePath(theRef, pathBuffer, kMaxPath);
              if theError = noErr then 
                GetSupportDir := UTF8ToAnsi(StrPas(pathBuffer)) + '/' + ApplicationName + '/';
            end;
        finally
          Freemem(pathBuffer);
        end
      {$ELSE}
        //
        // Other operating systems
        //
        GetSupportDir := GetAppConfigDirUTF8(Global);
      {$ENDIF}
    end;
    

## Actual preference folder locations

  * Preferences for [sandboxed](<Sandboxing_for_macOS.md> "Sandboxing for macOS") applications with the _App Groups_ entitlement may be found in `~/Library/Group Containers/[app identifier]/Library/Preferences`, where [app identifier] is something like com.companyname.appname or org.developername.appname but may be prefaced by alphanumeric characters.


  * Preferences for other [sandboxed](<Sandboxing_for_macOS.md> "Sandboxing for macOS") and all non-sandboxed applications may be found in `~/Library/Containers/[app identifier]/Data/Library/Preferences`, where [app identifier] is something like com.companyname.appname or org.developername.appname.


  * Preferences which apply to all users, particularly if they apply before a user logs in, may normally be found in `/Library/Preferences`.


  * Preferences which apply to an individual user, only after they have logged in, may normally be found in `~/Library/Preferences`.



Preference files should be [property list files](<macOS_property_list_files.md> "macOS property list files") and named something like com.companyname.appname.plist or org.developername.appname.plist. Using the [CFPreferences read/write methods](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write") will do this for you automatically and will also use the correct folder locations. 

## See also

  * [Locating the macOS application resources directory](<Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory")
  * [Locating the macOS tmp directory](<Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory")
  * [Locating macOS significant directories](<Locating_macOS_significant_directories.md> "Locating macOS significant directories").
  * [Reading and Writing macOS Preferences](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write").

---

_Source: [https://wiki.freepascal.org/Locating_macOS_app_support%2C_preferences_folders](https://web.archive.org/web/20241213014931/https://wiki.freepascal.org/Locating_macOS_app_support%2C_preferences_folders)_
