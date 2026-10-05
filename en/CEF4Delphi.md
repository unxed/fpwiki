# CEF4Delphi

│ **English (en)** │  **[español (es)](</CEF4Delphi/es> "CEF4Delphi/es")** │    
****

## Contents

  * 1 About
  * 2 Links
  * 3 Installation
  * 4 Usage
    * 4.1 Quick Starters
    * 4.2 Event properties
    * 4.3 Linux
    * 4.4 macOS (Cocoa) - NSPrincipalClass
      * 4.4.1 Cocoa on Lazarus 2.0.12



## About

CEF4Delphi is a Pascal wrapper for the "Chromium Embedded Framework". It enables Lazarus application to embed a Web-browser based on the Chromium engine. 

It currently is supported for Lazarus applications under Windows and Linux(GTK-2) with a x86_64 target (i386 pending tests). 

Starting with Lazarus 2.1 (trunk), there is also support for Cocoa x86_64. (Cocoa does not include all features of the OSR mode, but in normal Window mode all features should work). 

CEF4Delphi is based on [fpCEF3](<fpCEF3.md> "fpCEF3")

## Links

The CEF4Delphi documentation can be found at [https://www.briskbard.com/index.php?lang=en&pageid=cef](<https://www.briskbard.com/index.php?lang=en&pageid=cef>)

Forum: <https://www.briskbard.com/forum/>

The CEF documentation is at <https://magpcss.org/ceforum/apidocs3/>

Forum: <https://magpcss.org/ceforum/>

## Installation

CEF4Delphi is available in the Online Package Manager. 

In order to get the full feature set, you may need to install from the "external repository". 

The git repository can be found here: <https://github.com/salvadordf/CEF4Delphi>

The framework binaries can be found here: <https://cef-builds.spotifycdn.com/index.html>

There version need to match the version expected by the CEF4Delphi package. See git commit log <https://github.com/salvadordf/CEF4Delphi/commits/master>

The version check can be disabled for basic testing: "GlobalCEFApp.CheckCEFFiles := false;". However this should not be done for releases or more complex projects. 

## Usage

The usage is described in the documentation at [https://www.briskbard.com/index.php?lang=en&pageid=cef](<https://www.briskbard.com/index.php?lang=en&pageid=cef>)

### Quick Starters

The simplest way is to look at the following 2 demos: 

demos/Lazarus_any_OS/BrowserWindow
    A demo to show how to use a browser that will exist for the life-time of the application. (I.e. the browser is only closed, when the application exits)
demos/Lazarus_any_OS/BrowserWindowEx
    A demo that shows how to dynamically create and close browsers in your application

Make sure to read: 

  * the part of the documentation (link) on downloading and installing the "CEF binaries".
  * The README.txt in each demo-app's folder



Before writing your own app, read the comments on top of uCEFBrowesWindow.pas 

Optionally explore the settings in each demo-app's GlobalCefApplication.pas 

### Event properties

Many events (OnSomeEvent), including many of those assignable from the Object-Inspector will be executed in threads created by CEF. Events in threads are not allowed to access any components (Forms, Button, entire LCL...), and also restricted in what calls they may make to the OS. For other data they may need CriticalSections or similar. See the linked documentation. 

Some events are executed in a different Process. They can not even share Global variables. See the DOM demos on how communication is done. 

You can check the CEF documentation for each event. 

### Linux

On Linux CEF must be setup before the application creates any threads. Since gtk may create threads, CEF must be initialized before the LCL widgetset initializes gtk2. 

This requires modifications to the project.lpr file. 

In the demos, this is done by including a unit in the "uses" clause, before the "unit interfaces". In that unit is a "initialization" section, and that runs before the widgetset (if units are used in the correct order) 

### macOS (Cocoa) - NSPrincipalClass

On macOS the application [Info.plist](<macOS_property_list_files.md> "macOS property list files") file must be modified to set NSPrincipalClass to TCrCocoaApplication. 

There are scripts provided to do that. The scripts also put the "Helper apps" in place. (Assuming you compile them first / See the README.txt files). 

Under Lazarus 2.1 (trunk) the NSPrincipalClass can be set in the "Project options". 

#### Cocoa on Lazarus 2.0.12

In order to use CEF4Delphi with Cocoa x86_64 you must patch your Lazarus 2.0.12 version: [Patch](<https://github.com/User4martin/lazarus/commit/cccd9460506e42d97fc726faba730ad1a65aa035.diff>). 

[Human readable Patch](<https://github.com/User4martin/lazarus/commit/cccd9460506e42d97fc726faba730ad1a65aa035>).

---

_Source: [https://wiki.freepascal.org/CEF4Delphi](https://web.archive.org/web/20250417153344/https://wiki.freepascal.org/CEF4Delphi)_
