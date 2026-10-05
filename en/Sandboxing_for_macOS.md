# Sandboxing for macOS

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
  * 2 Sandboxing requirements
  * 3 Enabling sandboxing
  * 4 Adding sandbox entitlements
  * 5 File system access
  * 6 See also
  * 7 External links



## Overview

Sandboxing was introduced in 10.7 Lion and is a requirement for all Apple App Store applications. You can also sandbox applications which you distribute outside the App Store, but it is not required. 

Sandboxing is an access control technology provided in macOS and enforced at the kernel level. It is designed to contain damage to the system and the user’s data if an app becomes compromised. Apps distributed through the Apple App Store must be sandboxed. Apps code signed and distributed outside of the App Store with an Apple Developer certificate can be sandboxed as well but it is not required. 

The [Hardened runtime](<Hardened_runtime_for_macOS.md> "Hardened runtime for macOS") (introduced in macOS 10.14 Mojave) and sandboxing prevent an application from doing things it would ordinarily have had permission to do. It should be noted that these two protections are overlapping. The sandbox and the hardened runtime could prevent the same action, so even if the hardened runtime would allow the action, the sandbox may prevent it, and vice versa. See the external links below to Apple's lists of hardened runtime entitlements and sandboxing entitlements. 

## Sandboxing requirements

To sandbox an application the following requirements must be met: 

  1. a 64 bit executable application;
  2. uses the Lazarus 64 bit Cocoa widgetset, not the 32 bit Carbon widgetset;
  3. code signed with an Apple developer certificate.



Additionally, for security reasons, you should not include any sandbox entitlements that your application does not to need to exercise its full functionality. 

## Enabling sandboxing

An application is sandboxed by using a sandbox entitlements file which is in the plist format. The example below simply enables sandboxing without providing any sandbox entitlements: 
    
    
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>com.apple.security.app-sandbox</key>
        <true/>
    </dict>
    </plist>
    

This entitlements file can be called anything and is not included in your application bundle. To check that the format of your plist files is valid, open a terminal and running the following command on your file: 
    
    
    plutil entitlements.plist
    

If the file is valid, the command will return output similar to the following: 
    
    
    entitlements.plist: OK
    

Make sure that the application bundle does not use a symlink to the executable, which is the default for applications created in Lazarus. To code sign the application using this entitlements file, open a terminal and run the following command: 
    
    
    codesign -f --timestamp --entitlements entitlements.plist -s "Developer ID Application: YOUR NAME (TEAM_ID)" /path/to/bundle.app
    

Now run the application as usual. To check that the application has in fact been sandboxed, simply check the /Users/<username>/ Library/Containers/ for the application's CFBundleIdentifier key. For example: 
    
    
    /Users/<username>/Library/Containers/com.example.MyApp
    

If the application's CFBundleIdentifier key is there, you know your application was sandboxed when it was executed. 

Alternatively, you can check that the application is properly code signed and also display its entitlements by opening a terminal and running the following command: 
    
    
    codesign -dv --entitlements :- /path/to/MyApp.app
    

which should produce output similar to the following: 
    
    
    Executable=/path/to/MyApp.app/Contents/MacOS/PICinfo
    Identifier=com.example.MyApp
    Format=app bundle with Mach-O thin (x86_64)
    CodeDirectory v=20200 size=101490 flags=0x0(none) hashes=3164+5 location=embedded
    Signature size=9063
    Timestamp=8 Dec 2019 at 21:21:02
    Info.plist entries=16
    TeamIdentifier=<10 alpha numeric digits>
    Sealed Resources version=2 rules=13 files=45
    Internal requirements count=1 size=180
    <?xml version="1.0" encoding="utf-8"?>
    <plist version="1.0">
      <dict>
        <key>com.apple.security.app-sandbox</key>
        <true/>
      </dict>
    </plist>
    

## Adding sandbox entitlements

Assuming for the moment that your application needs to access the Internet via HTTP to check if it is up to date, your application will currently be denied any network access. To add missing entitlements, edit the entitlements.plist file and modify it to add the two additional entitlements for network access as below: 
    
    
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>com.apple.security.app-sandbox</key>
        <true/>
        <key>com.apple.security.network.client</key>
        <true/>
        <key>com.apple.security.network.server</key>
        <true/>
    </dict>
    </plist>
    

Re-code sign and open your application. It should now be able to perform its HTTP check over the Internet. 

The full list of sandbox entitlements may be found in Apple's documentation in the external links below. 

## File system access

When sandboxed, your application has access to the following locations: 

  * _The app container directory._ On first launch, the operating system creates a special directory for use by your app - and only by your app - called a container. Each user on a system gets an individual container for your app, within their home directory; your app has unfettered read/write access to the container for the user who ran it.


  * _App group container directories._ A sandboxed app can specify an entitlement that gives it access to one or more app group container directories, each of which is shared among all apps with that entitlement.


  * _User-specified files._ A sandboxed app (with an appropriate entitlement) automatically obtains access to files in arbitrary locations when those files are explicitly opened by the user or are dragged and dropped onto the application by the user.


  * _Related items._ With the appropriate entitlement, your app can access a file with the same name as a user-specified file, but a different extension. This can be used for saving modified files in a different format (eg bmp as jpg or txt as rtf).


  * _Temporary directories, command-line tool directories, and specific world-readable locations._ A sandboxed app has varying degrees of access to files in certain other well-defined locations.



See the Apple documentation below for more in-depth explanations. 

## See also

  * [Code Signing for macOS](<Code_Signing_for_macOS.md> "Code Signing for macOS")
  * [Hardened runtime for macOS](<Hardened_runtime_for_macOS.md> "Hardened runtime for macOS")
  * [Notarization for macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+")
  * [macOS Catalina changes for developers](<macOS_Catalina_changes_for_developers.md> "macOS Catalina changes for developers")



## External links

  * [Apple: App Sandbox Design Guide](<https://developer.apple.com/library/archive/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html>)
  * [Apple: App Sandboxing](<https://developer.apple.com/app-sandboxing/>)
  * [Apple: Sandbox entitlements](<https://developer.apple.com/documentation/security/app_sandbox_entitlements>)
  * [Apple: Hardened runtime entitlements](<https://developer.apple.com/documentation/security/hardened_runtime_entitlements>)
  * [Apple: App Store Review Guidelines](<https://developer.apple.com/app-store/review/guidelines/>)
  * [Apple: Avoiding common app rejection reasons](<https://developer.apple.com/app-store/review/#common-app-rejections>)


  * [Sandboxing Apps on the Mac](<https://macpgmr.github.io/ObjP/SandboxingAppsOnMac.html>)

---

_Source: [https://wiki.freepascal.org/Sandboxing_for_macOS](https://web.archive.org/web/20250401072755/https://wiki.freepascal.org/Sandboxing_for_macOS)_
