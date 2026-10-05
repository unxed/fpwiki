# macOS Open Sesame

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Opening files
  * 3 Opening directories
  * 4 Opening URLS
  * 5 Opening applications
  * 6 Opening Preference Panes
  * 7 More options
  * 8 See also (cross-platform)



## Overview

The macOS `/usr/bin/open` command line utility is a veritable Aladdin's cave of utility. The command opens a file (or a directory or URL), just as if you had double-clicked the file's icon. If no application name is specified, the default application as determined via LaunchServices is used to open the specified file. If the file is in the form of a URL, the file will be opened as a URL. See [Executing External Programs](<Executing_External_Programs.md> "Executing External Programs") to run the command from within your application. 

## Opening files

Want to open a test.c file? 
    
    
      open test.c
    

Want to open your test.c file with TextEdit and not with Xcode? 
    
    
      open -a TextEdit test.c
    

or 
    
    
      open -e test.c
    

## Opening directories

Want to open Finder at a particular directory, say the user's Documents directory? 
    
    
      open ~/Documents
    

## Opening URLS

Open our favourite website: 
    
    
      open https://wiki.freepascal.org
    

Safari not your favourite? Use FireFox maybe: 
    
    
      open -a FireFox  https://wiki.freepascal.org
    

## Opening applications

Want to open another copy of a running application? Normally macOS would simply bring the already-running application to the foreground, but: 
    
    
      open -n -a TextEdit
    

Want the `open` command to wait until the application opened has exited? 
    
    
      open -W -n -a TextEdit
    

## Opening Preference Panes

Want to open the Security Preferences Pane? 
    
    
      open "x-apple.systempreferences:com.apple.preference.security"
    

How about the Firewall tab of the Security Preferences pane? 
    
    
      open "x-apple.systempreferences:com.apple.preference.security?Firewall"
    

Note that this scheme was new in Yosemite (macOS 10.10) and that Apple subsequently restricted use of the URL scheme in El Capitan (macOS 10.11). 

## More options

For the full gamut of open command options, open a Terminal and call up the Unix man page: 
    
    
     man open
    

## See also (cross-platform)

  * [OpenDocument](<opendocument.md> "opendocument")
  * [OpenURL](<OpenURL.md> "OpenURL")

---

_Source: [https://wiki.freepascal.org/macOS_Open_Sesame](https://web.archive.org/web/20240909222034/https://wiki.freepascal.org/macOS_Open_Sesame)_
