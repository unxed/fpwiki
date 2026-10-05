# Cocoa Internals/OS Versions

│ **English (en)** │    
****

The page cover issues of (backwards) compatibility and API deprecation across different macOS versions. 

## Contents

  * 1 AppStore critical
  * 2 macOS API Deprecation
  * 3 Objective-C methods
  * 4 (Plain) Functions
  * 5 AppKit Version
  * 6 Version Specific
    * 6.1 Mojave (10.14)
      * 6.1.1 Drawing Issues
      * 6.1.2 Dark Mode
      * 6.1.3 Font Quality
  * 7 See Also



## AppStore critical

If you're building an application with LCL Cocoa, and having issues with uploading to AppStore, due to "Found private symbol usage", please bug report the issue if it is caused by a function or a method used in LCL Cocoa. 

The Cocoa Widgetset is expected to be AppStore friendly as AppStore is being made the core environment for the distribution of Apple apps. 

Please note that applications can still be distributed outside of the AppStore (unlike iOS). 

## macOS API Deprecation

It's quite common for Apple to introduce methods in one version of the system and deprecate them in another version. The chances are high that a deprecated method will be removed in a later macOS version completely. Thus, if method is not verified in run-time, the application could stop working (ie the App could fail to load). 

Suggestions: 

  * Use non-deprecated methods (there are plenty of APIs that are in effect since macOS 10.0 and are not planned for deprecation).
  * If the deprecated method is in use and being replaced by some other newly introduced method - check the availability in runtime 
    * if the latest method is available - use it first
    * if the latest method is not available - fallback to the original method.



This would allow your application to execute the same code on earlier and later macOS versions. 

## Objective-C methods

Use ObjCSelector with respondsToSelector() check, prior to the explicit call. If an object gets a selector it's unable to handle, ObjectiveC would throw an exception and the application would crash. 
    
    
      if Assigned(win) then
      begin
        if win.respondsToSelector( ObjCSelector('backingScaleFactor')) then
          Result := win.backingScaleFactor
        else if win.respondsToSelector( ObjCSelector('userSpaceScaleFactor')) then // for older macOS
          Result := win.userSpaceScaleFactor;
      end;
    

## (Plain) Functions

It's possible that a new feature requires a function that has been introduced in a later version of macOS. In this case a straight use of the function would cause a load-time failure on earlier versions of macOS. 

In order to prevent that, a dynamic loading of the function needs to be used. 
    
    
    uses ... 
      dl, dynlibs;
    ...
      p := GetProcedureAddress(TLibHandle(RTLD_DEFAULT), 'CGDisplayCreateImage');
    

The following needs to be taken into consideration: 

  * the use of RTLD_DEFAULT - searches for the function name across all loaded libraries. The system library would likely be loaded by that time. However, if not the actual library name needs to be loaded (unlike Linux, macOS doesn't require the full library path to be specified).


  * if the symbol is not present, then the code should handle it gracefully (instead of calling to unspecified function).



## AppKit Version

In many cases you can call a certain functionality based on the AppKit version. **NSAppKitVersionNumber** is a shared global C-variable, thus the check of the value is performance friendly. 

Values to be compared to are constants (declared at cocoa_extra.pas): 
    
    
    NSAppKitVersionNumber10_4 = 824;
    NSAppKitVersionNumber10_5 = 949;
    NSAppKitVersionNumber10_6 = 1038;
    NSAppKitVersionNumber10_7 = 1138;
    NSAppKitVersionNumber10_8 = 1187;
    NSAppKitVersionNumber10_9 = 1265;
    NSAppKitVersionNumber10_10 = 1343;
    NSAppKitVersionNumber10_11 = 1404;
    NSAppKitVersionNumber10_12 = 1504;
    NSAppKitVersionNumber10_13 = 1561;
    NSAppKitVersionNumber10_14 = 1671; 
    

Each version corresponds to a particular macOS release (or even sub-release in some cases). Major versions are denoted by an integer number, minor versions and release by the fractional part. To use macOS 10.13 and 10.14 as examples this is expressed as follows: 

macOS version  | Value of NSAppKitVersionNumber   
---|---  
macOS 10.13  | 1561   
macOS 10.13.1  | 1561.1   
macOS 10.13.2  | 1561.2   
macOS 10.13.3  | 1561.3   
macOS 10.13.4  | 1561.4   
macOS 10.14.1  | 1671.1   
macOS 10.14.2  | 1671.2   
macOS 10.14.3  | 1671.3   
macOS 10.14.4  | 1671.4   
macOS 10.14.5  | 1671.5   
  
The values are taken from macOS header files provided with Xcode (eg NSapplication.h). 

## Version Specific

### Mojave (10.14)

#### Drawing Issues

According to [AppKit release notes](<https://developer.apple.com/documentation/appkit/appkit_release_notes_for_macos_10_14?language=objc>) for 10.14 there are views are drawn Layer-backed. 
    
    
    Windows in apps linked against the macOS 10.14 SDK are displayed using Core Animation when the app is running in macOS 10.14. 
    This doesn’t mean that all views are layer-backed; rather, it means that all views are either layer-backed or draw into 
    a shared layer with other layers.
     
    This change should be mostly invisible to most apps, but you might notice one or more subtle changes as a result.
    
    Views that depend on drawing in the same backing store as their ancestors or lower-ordered siblings may find 
    that they are instead drawing in separate layers. Views shouldn’t rely on being able to draw into the same 
    backing store as their ancestors; instead, those ancestors should change as required. For example, to affect 
    the background of a window, use the NSWindow properties opaque and backgroundColor
    

The change seems to impact CocoaWS a lot, causing problem with area invalidation. A number of Mojave-specific issues ([[1]](<https://bugs.freepascal.org/view.php?id=34335>)[[2]](<https://bugs.freepascal.org/view.php?id=34358>)) were reported. 

    **LCL solution** \- make sure the code does the right thing. or force all LCL driven controsl to return wantsUpdateLayer as true.

#### Dark Mode

Dark mode is supported "out of the box", but only if an application was built using SDK for 10.14. For example, if an application was built with an earlier SDK (i.e. was compiled on macOS 10.13 High Sierra or earlier), then it would not pick up Dark Mode by default. 
    
    
    When an app links on the macOS 10.14 SDK, it’s automatically opted in to supporting the dark appearance, 
    with its NSApp inheriting the NSAppearanceNameDarkAqua appearance from System Preferences.
    

    **Enable dark mode on LCL level.** Cocoa WS needs to be updated, to check if NSApplication object supports "appearance" and if it does, then enable the currently selected system theme. That allows to support "Dark Mode" even for applications built on an earlier version of the system.
    **Enable dark mode in manifest.** Add a switch into Info.plist NSRequiresAquaSystemAppearance and set the value to "false". (The key must be specified explicitly, if an app is build for an earlier SDK.)
    **Disable dark mode in manifest.** In the Info.plist, the key NSRequiresAquaSystemAppearance should be set to "true".

#### Font Quality

Smooth font rendering is somewhat disabled in the OS by default. It is issue in 10.14 and 10.15 (Catalina). 

  * <https://ahmadawais.com/fix-macos-mojave-font-rendering-issue/>
  * <https://discussions.apple.com/thread/250730319>



Solution: open Terminal and enter the command: 
    
    
    defaults write -g CGFontRenderingFontSmoothingDisabled -bool NO
    

then log off or restart. 

## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")
  * [Apple Release Notes for macOS 10.14](<https://developer.apple.com/documentation/appkit/appkit_release_notes_for_macos_10_14?language=objc>)
  * [Apple Release Notes for macOS 10.13](<https://developer.apple.com/library/archive/releasenotes/AppKit/RN-AppKit/index.html>)
  * [Apple Release Notes for macOS 10.12 and earlier](<https://developer.apple.com/library/archive/releasenotes/AppKit/RN-AppKitOlderNotes/>)


  * [Issue #33672](<https://bugs.freepascal.org/view.php?id=33672>) \- the bug covers some problems linking the app

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/OS_Versions](https://web.archive.org/web/20240909170622/https://wiki.freepascal.org/Cocoa_Internals/OS_Versions)_
