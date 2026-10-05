# Locating the macOS tmp directory

│ **English (en)** │    
****

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Determining where to store files
  * 3 Storing temporary files
  * 4 Why not use GetTempDir?
  * 5 See also



## Overview

To locate the current user's temporary directory in macOS you should use the native macOS Foundation function _NSTemporaryDirectory()_. This directory is unique for each user, the user is guaranteed to have write permissions to it, and it is in a hashed location which is not predictable in advance so that it is safe from security issues associated with predictable locations. It is also guaranteed to work when your application has been [sandboxed](<Sandboxing_for_macOS.md> "Sandboxing for macOS"). 

You should be aware that this directory is cleaned out automatically every 3 days but otherwise persists between application launches and reboots. 

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


  * Use NSTemporaryDirectory (this is **not** backed up by Time Machine) to store temporary files that you intend to use immediately for some ongoing operation but then plan to discard later. Delete temporary files as soon as you are done with them.



## Storing temporary files

How to locate the user's unique temporary directory, display it and write a temporary file to it is demonstrated by the code below: 
    
    
    ...
    {$modeswitch objectivec1} 
    
    interface
    
    Uses
      ...
      CocoaAll;
    ...
    
    procedure TmpFile;
    var
      tmpFile: TextFile;
      filePath: String;
    begin
      ShowMessage(NSTemporaryDirectory.UTF8String);
    
      filePath := NSTemporaryDirectory.Utf8String + 'test.tmp';
    
      AssignFile(TmpFile, filePath);
      Try
        Rewrite(TmpFile);
        Writeln(TmpFile, 'This is my test tmp file.');
      Finally
        CloseFile(TmpFile);
      End;
    end;
    

## Why not use GetTempDir?

It should be noted that Free Pascal's _GetTempDir_ function (from SysUtils) uses the value of the macOS TMPDIR environment variable to determine the path to where the current user's temporary files should be stored. This is a possible security issue because the TMPDIR environment variable is easily compromised. 

## See also

  * [Locating macOS significant directories](<Locating_macOS_significant_directories.md> "Locating macOS significant directories")
  * [Locating macOS app support, preferences folders](<Locating_macOS_app_support,_preferences_folders.md> "Locating macOS app support, preferences folders")
  * [Locating the macOS application resources directory](<Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory")

---

_Source: [https://wiki.freepascal.org/Locating_the_macOS_tmp_directory](https://web.archive.org/web/20240910051147/https://wiki.freepascal.org/Locating_the_macOS_tmp_directory)_
