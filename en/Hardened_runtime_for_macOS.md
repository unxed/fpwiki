# Hardened runtime for macOS

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
  * 2 Hardening your application
  * 3 Hardened runtime entitlements
  * 4 Verifying hardening
  * 5 Protected resource access
  * 6 Hardened runtime issues and solutions
  * 7 See also
  * 8 External links



## Overview

The hardened runtime was introduced by Apple in macOS 10.14 (Mojave) and while it is optional for applications, it is required in order to [notarize](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+") your application. 

The hardened runtime, along with System Integrity Protection (SIP), protects the runtime integrity of your application software by preventing certain classes of exploits, like code injection, dynamically linked library hijacking, and process memory space tampering. If your application relies on a capability that the hardened runtime restricts, you need to add an entitlement to disable an individual protection. You should only add the entitlements that are absolutely necessary for your applications functionality. 

[Sandboxing](<Sandboxing_for_macOS.md> "Sandboxing for macOS") and the hardened runtime prevent an application from doing things it would ordinarily have had permission to do. It should be noted that these two protections are overlapping. The sandbox and the hardened runtime could prevent the same action, so even if the hardened runtime would allow the action, the sandbox may prevent it, and vice versa. See the external links to Apple's lists of hardened runtime entitlements and sandboxing entitlements below. 

## Hardening your application

The hardening of an application is done during [code signing](<Code_Signing_for_macOS.md> "Code Signing for macOS"). To harden your application, open a Terminal and run this command: 
    
    
    codesign --force --options runtime --timestamp --sign "Developer ID Application: YOUR NAME (TEAM_ID)" /path/to/bundle.app
    

## Hardened runtime entitlements

To allow your application to do something that is now prevented by your hardened runtime, you need to give it that entitlement. You do this by creating an entitlements plist file like this: 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
    	<key>com.apple.security.device.camera</key>
    	<true/>
    </dict>
    </plist>
    

This would allow your application to capture movies and still images using the built-in camera. To verify you have a valid plist file, open a Terminal and run the following command: 
    
    
    plutil entitlements.plist
    

If the file is properly formatted, the output of that command should be: 
    
    
    entitlements.plist: OK
    

To add this entitlement to your application, you need to add it to your code signing command like this: 
    
    
    codesign --force --options runtime --timestamp --entitlements entitlements.plist --sign "Developer ID Application: YOUR NAME (TEAM_ID)" /path/to/bundle.app
    

## Verifying hardening

To verify that you have successfully hardened your application, open a Terminal and run this command: 
    
    
    codesign --display --verbose /path/to/bundle.app
    

The output of that command should be similar to: 
    
    
    Executable=/Users/trev/your.app/Contents/MacOS/yourapp
    Identifier=org.yourdomain.yourapp
    Format=app bundle with Mach-O thin (x86_64)
    CodeDirectory v=20500 size=101470 flags=0x10000(runtime) hashes=3164+3 location=embedded
    Signature size=9063
    Timestamp=7 Dec 2019 at 18:36:08
    Info.plist entries=16
    TeamIdentifier=<10 alpha numerical digits>
    Runtime Version=10.14.0
    Sealed Resources version=2 rules=13 files=45
    Internal requirements count=1 size=180
    

The (runtime) flag in the "CodeDirectory" line indicates that your application is hardened. 

To check whether the required hardened runtime entitlements are available, open a Terminal and run the following command: 
    
    
    codesign -d --entitlements :- /path/to/bundle.app
    

which should result in the following output: 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
     <key>com.apple.security.device.camera</key>
     <true/>
    </dict>
    </plist>
    

## Protected resource access

Your application needs to declare its intent to access protected resources by providing a purpose string that explains to the user why you need access to protected resources. When declaring resource access, only set the entitlements and usage strings on your main bundle. 

Description | Entitlement | Usage string Info.plist key   
---|---|---  
Audio input and microphone | com.apple.security.device.audio-input | NSMicrophoneUsageDescription   
Any camera exposed via AVFoundation | com.apple.security.device.camera | NSCameraUsageDescription   
Location | com.apple.security.personal-information.location | NSLocationUsageDescription   
Contacts | com.apple.security.personal-information.addressbook | NSContactsUsageDescription   
Calendars and Reminders | com.apple.security.personal-information.calendars | NSCalendarUsageDescription   
Apple Photos library | com.apple.security.personal-information.photos-library | NSPhotoLibraryUsageDescription   
Sending Apple Events to other apps | com.apple.security.automation.apple-events | NSAppleEventsUsageDescription   
  
## Hardened runtime issues and solutions

**Issue:** My application runs non-native code, and I want that code to run blazing fast with JIT, but my application crashes when I enable Hardened Runtime. 

  * Recommended solution: Adopt the “com.apple.security.cs.allow-jit” entitlement; use mmap and the MAP_JIT flag to allocate anonymous Read/Write/Execute memory.


  * Fallback solution: Disable Runtime Code Signing Enforcement with the “com.apple.security.cs.allow-unsigned-executable-memory” entitlement; bytes mapped from disk will still be checked against any associated code signature.



**Issue:** My application patches system frameworks it loads into memory to accomplish “...” but now my application crashes when I enable Hardened Runtime. 

  * Recommended Solution: Don’t do this! (Library Validation may meet your use case.)


  * Fallback Solution: Disable Runtime Code Signing Enforcement with the “com.apple.security.cs.allow-unsigned-executable-memory” entitlement.



**Issue:** My application crashes when I adopt the Hardened Runtime and then run my auto-update mechanism. 

  * Explanation: Code signatures are latched to files on first use. Modifying files in place causes a signature mismatch.


  * Recommended Solution: Whenever you update a signed file, create a new file.



**Issue:** My application loads plugins from other developers in-process, but plug-in loading fails when I adopt the Hardened Runtime. 

  * Recommended Solution: Consider moving to an out of process plugin model.


  * Fallback Solution: Use the “com.apple.security.cs.disable-library-validation” entitlement Allows loading unsigned and adhoc signed plug-ins.



**Issue:** I need to use DYLD environment variables while building and debugging my application, but they are being ignored when I enable Hardened Runtime. 

  * Solution: Use the “com.apple.security.get-task-allow” entitlement on your debug build.



**Issue:** My application uses DYLD environment variables when it ships to my customers and now it doesn’t work with Hardened Runtime. 

  * Recommend Solution: Don’t do this!


  * Fallback Solution: Use the “com.apple.security.cs.allow-dyld-environment-variables” entitlement.



**Issue:** Hardened Runtime prevents debugging of hardened processes by default. How can I build and test with the Hardened Runtime if I cannot attach a debugger? 

  * Solution: Use the “com.apple.security.get-task-allow” entitlement on your debug build. Note: Running an application under a debugger will mask Hardened Runtime related issues, so be sure to test a release build.



**Issue:** My application supports an in-process plug-in ecosystem. How can my plug-in developers debug their plug-ins? 

  * Recommended Solution: Move to an out of process plug-in model.


  * Alternative Solution: Ship a debug version to registered plug-in developers.


  * Fallback Solution: Combine “com.apple.security.get-task-allow” with “com.apple.security.cs.disable-library-validation”.



## See also

  * [Code Signing for macOS](<Code_Signing_for_macOS.md> "Code Signing for macOS")
  * [Notarization for macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+")
  * [Sandboxing for macOS](<Sandboxing_for_macOS.md> "Sandboxing for macOS")



## External links

  * [Apple: Hardened runtime entitlements](<https://developer.apple.com/documentation/security/hardened_runtime_entitlements>)
  * [Apple: Sandbox entitlements](<https://developer.apple.com/documentation/security/app_sandbox_entitlements>)

---

_Source: [https://wiki.freepascal.org/Hardened_runtime_for_macOS](https://web.archive.org/web/20250429231631/https://wiki.freepascal.org/Hardened_runtime_for_macOS)_
