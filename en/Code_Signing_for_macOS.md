# Code Signing for macOS

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/Code_Signing_for_macOS.md>)** │

  


![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For Apple Notarization requirements for kernel extensions and applications from Mojave 10.14.5 onwards (for kernel extensions from 7 April 2019 and for developers whose first use of their code signing certificate occurred from 7 April 2019) and for all software from Catalina 10.15 onwards that is not distributed via the App Store, see [Notarization for macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+")

  


## Contents

  * 1 Introduction
  * 2 The evolution of Gatekeeper
    * 2.1 Big Sur and later on Apple M1 ARM64 processors
  * 3 Gatekeeper Dialogs
  * 4 Overview
  * 5 Gatekeeper requirements
  * 6 Using codesign to sign your application
  * 7 Using productsign to sign your pkg installer
  * 8 Using codesign to sign your disk image
  * 9 Updating a signed file
  * 10 Ad hoc signing
  * 11 Code signing an x86_64 binary on an aarch64 machine
  * 12 How to install certificates on a second computer
    * 12.1 Using the Keychain Access application
    * 12.2 Using Xcode
  * 13 How to check what certificates are installed
  * 14 See also
  * 15 External links



## Introduction

**[Code signing](<Code_signing.md> "Code signing")** ensures both authenticity and integrity of executables that have been downloaded from wide area networks like the Internet. The discussion below applies equally to App Store distribution and distribution outside the App Store. 

Code signing is required in [iOS](<Portal_iOS.md> "Portal:iOS"). On [macOS](<Portal_Mac.md> "Portal:Mac") 10.7 and later, it enables programs downloaded from the Internet to be opened without any warnings (if they are signed with an Apple-issued certificate) and it is required when using certain functionality (eg APIs used by debuggers; note that in this case a self-signed certificate that is marked as "trusted" suffices). This functionality is performed by Apple's Gatekeeper software. 

## The evolution of Gatekeeper

Gatekeeper, first introduced in Mountain Lion (10.8, 2012), is a Mac security feature that was designed to protect Apple computers from malicious software. Gatekeeper checks applications against the list of apps that Apple has approved for its App Store or have been code signed by developers who have Apple-issued certificates where the application is not offered through the app store. It does not perform any safety checks by itself, other than that the application wasn't changed since the developer signed it, nor does it offer any guarantees about the developer other than that they are paying Apple $US 99 per year (aka an "Identified Developer"). 

The original Gatekeeper options introduced in Mountain Lion, accessed from Preferences > Security and Privacy > General, were: 

  * App Store
  * App Store and Identified Developers
  * Anywhere



By choosing the **Anywhere** option, the user was once able to entirely disable Gatekeeper. The default setting only allowed the launching of applications from the App Store or from a developer who had signed their application with an Apple-issued certificate. 

When macOS Sierra was released in 2016, Apple made some important changes to Gatekeeper and limited the the Gatekeeper options to: 

  * App Store
  * App Store and Identified Developers



However, you can restore the missing Gatekeeper _Anywhere'_ option in Preferences by opening a terminal and executing the command: 
    
    
    $ sudo spctl --master-disable
    

which still works up to and including macOS Catalina. The better, as in more secure, alternative was to instead bypass Gatekeeper by opening the application from the right-click context menu or by control clicking on the application. This still triggered the alert dialog but it now contained an **Open** button to successfully launch the application. This method of bypassing Gatekeeper still works in macOS Catalina, except that on Catalina you need to launch the application a second time with the control-click or right-click to successfully bypass Gatekeeper - the first time you launch the application Gatekeeper cannot be bypassed whether you control-click or right-click or not. 

### Big Sur and later on Apple M1 ARM64 processors

And then Apple changed the game. 

When Big Sur is running on an Apple M1 (ARM64) processor, **all native ARM64 code must be signed or the operating system prevents its execution** ; and this new Gatekeeper requirement cannot be bypassed by users. Developers can bypass it by running the following command in an Applications > Utilities > Terminal window: 
    
    
    spctl developer-mode enable-terminal
    

and then going to System Preferences > Security & Privacy > Privacy, scrolling down until you see an entry for "Developer Tools", authenticate and then tick the checkbox next to Terminal. 

Intel executables being run under Big Sur on an Apple M1 processor via Rosetta 2 can still be run even though they are unsigned using the control-click or right-click method to bypass Gatekeeper. 

For more details, see: [Big Sur changes for developers: Code signing - Intel vs M1](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers"). 

## Gatekeeper Dialogs

  * [![Simple warning after opening a downloaded application in Leopard.](https://wiki.freepascal.org/images/9/9c/load_security_warning_dmg_leopard_en.png)](</File:load_security_warning_dmg_leopard_en.png> "Simple warning after opening a downloaded application in Leopard.")

Simple warning after opening a downloaded application in Leopard. 

  * [![Default settings are more strict in Mavericks, where they prevent downloaded unsigned apps from being started via double-clicking.](https://wiki.freepascal.org/images/8/89/load_security_warning_dmg_mavericks_en.png)](</File:load_security_warning_dmg_mavericks_en.png> "Default settings are more strict in Mavericks, where they prevent downloaded unsigned apps from being started via double-clicking.")

Default settings are more strict in Mavericks, where they prevent downloaded unsigned apps from being started via double-clicking. 

  * [![If the application is launched via context menu in Mavericks a simple warning is displayed similar to that of earlier versions.](https://wiki.freepascal.org/images/7/72/load_security_warning_mavericks_context_en.png)](</File:load_security_warning_mavericks_context_en.png> "If the application is launched via context menu in Mavericks a simple warning is displayed similar to that of earlier versions.")

If the application is launched via context menu in Mavericks a simple warning is displayed similar to that of earlier versions. 

  * [![Gatekeeper shows a simplified warning, if a code-signed app has been downloaded and launched for the first time. In subsequent launches the program is starts immediately.](https://wiki.freepascal.org/images/7/7a/simple_load_security_warning_mavericks_en.png)](</File:simple_load_security_warning_mavericks_en.png> "Gatekeeper shows a simplified warning, if a code-signed app has been downloaded and launched for the first time. In subsequent launches the program is starts immediately.")

Gatekeeper shows a simplified warning, if a code-signed app has been downloaded and launched for the first time. In subsequent launches the program is starts immediately. 

  * [![Overview of code-signing a program written with Lazarus and Free Pascal](https://wiki.freepascal.org/images/9/91/codesigning.png)](</File:codesigning.png> "Overview of code-signing a program written with Lazarus and Free Pascal")

Overview of code-signing a program written with Lazarus and Free Pascal 




## Overview

The basic steps to sign an application that has been written with Lazarus and/or Free Pascal are: 

  1. Obtain a Developer ID from Apple and install it in your system's key chain.
  2. Note the alphanumeric key of your Developer ID (aka TeamIdentifier).
  3. Sign your application with the _codesign_ command.
  4. Sign your installer pkg with the _productsign_ command.



It is **not possible** to use certificates from third-party providers like Comodo because they will not pass Gatekeeper which requires an Apple developer issued certificate. Also note that you cannot sign Windows applications with the Apple developer certificate (this time you do need a third-party Comodo etc certificate). 

A macOS application distributed outside the Mac App Store will generally have 3–4 separate layers of signing: 

  * The application itself will be code signed using an application certificate.
  * If the application has an installer, the package file will be signed using an installer certificate.
  * The disk image containing the application or installer will be signed using an application certificate.
  * The disk image will [notarized](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+"), and then the ticket generated by the notary service will be [stapled](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+") to it.



## Gatekeeper requirements

  * Your application should be standalone with no unacceptable external dependencies. The only acceptable external dependencies are system libraries. All other dependencies should be copied to your MyApp.app bundle folder. Gatekeeper rejects any application that has non-system external dependencies.


  * All binary files inside MyApp.app should be code signed.


  * All binary files should be located in standard locations inside the MyApp.app bundle folder. Refer to the table below.

Standard locations for code inside a bundle  Location | Description   
---|---  
Contents | Top content directory of the bundle   
Contents/MacOS | Main executable; helper apps and tools   
Contents/Frameworks | Frameworks, dylibs   
Contents/PlugIns | Plug-ins, both loadable and extensions   
Contents/XPCServices | XPC services   
Contents/Helpers | Helper apps and tools   
Contents/Library/Automator | Automator actions   
Contents/Library/Spotlight | Spotlight importers   
Contents/Library/LoginItems | Installable login items   
Contents/Library/LaunchServices | Privileged helper tools installed by the ServiceManagement framework   
  
Note: No non-binary files should ever be located in the folders specified in the table above. 

## Using codesign to sign your application

**1.** Sign your application with: 
    
    
    codesign -f -o runtime --timestamp -s "Developer ID Application: YOUR NAME (TEAM_ID)" /path/to/bundle.app
    

**2.** Display basic information about the result of the signing process: 
    
    
    codesign -dv -r- /path/to/bundle.app
    

If your application was successfully signed this command will return information similar to this: 
    
    
    Executable=/Users/apple/Desktop/your.app/Contents/MacOS/yourappname
    Identifier=org.yourdomain.yourappname
    Format=app bundle with Mach-O thin (x86_64)
    CodeDirectory v=20500 size=101470 flags=0x10000(runtime) hashes=3164+3 location=embedded
    Signature size=9063
    Timestamp=7 Dec 2019 at 18:36:08
    Info.plist entries=16
    TeamIdentifier=<10 alpha numeric digits>
    Runtime Version=10.14.0
    Sealed Resources version=2 rules=13 files=45
    designated => identifier "org.yourdomain.yourappname" and anchor apple generic and certificate 1[field.1.2.840.113635.100.6.2.6] /* exists */ and certificate leaf[field.1.2.840.113635.100.6.1.13] /* exists */ and certificate leaf[subject.OU] = <10 alpha numeric digits>
    

**3.** Verify your signature: 
    
    
    codesign -vv /path/to/bundle.app
    

If your app was successfully signed this command will return the strings "valid on disk" and "satisfies its Designated Requirement", respectively, after the path to your application. 

## Using productsign to sign your pkg installer

**1.** Sign your installer pkg file with: 
    
    
    productsign --timestamp --sign "Developer ID Installer: YOUR NAME (TEAM_ID)" /path/to/unsigned.pkg /path/to/signed.pkg
    

**2.** Verify your signature with: 
    
    
    spctl -vv --assess --type install /path/to/your.pkg
    

which should yield information similar to this if it was successful: 
    
    
    your.pkg: accepted
    source=Developer ID
    origin=Developer ID Installer: YOUR NAME (<10 alpha numeric digits>)
    

## Using codesign to sign your disk image

Beginning in macOS 10.11.5, you can apply a code signature to read-only, compressed disk images that you use to distribute content. In this case you do not need to separately sign your application. 

**1.** Sign your disk image with: 
    
    
    codesign -f -o runtime --timestamp -s "Developer ID Application: YOUR NAME (TEAM_ID)" /path/to/YourImage.dmg
    

**2.** Verify your signature with: 
    
    
    spctl -a -t open --context context:primary-signature -vv /path/to/YourImage.dmg
    

which should yield information similar to this if it was successful: 
    
    
    YourImage.dmg: accepted
    source=Developer ID
    origin=Developer ID Application: YOUR NAME (<10 alpha numeric digits>)
    

**After 3 February 2020 Apple is fully enforcing notarization** for macOS 10.14.5+ (Mojave) with the consequence that the above verification of your disk image will now return information similar to this: 
    
    
    YourImage.dmg: rejected
    source=Unnotarized Developer ID
    origin=Developer ID Application: YOUR NAME (<10 alpha numeric digits>)
    

See [Notarization for macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+") for more details. 

## Updating a signed file

Whenever you update a signed file, create a new file. Do not copy a new signed file over an old signed file. 

Specifically, the code signing information (code directory hash) is hung off the vnode within the kernel, and modifying the file behind that cache will cause problems. You need a new vnode, which means a new file, that is, a new inode. Documented in WWDC 2019 Session 703 All About Notarization - see [slide 65 (PDF)](<https://devstreaming-cdn.apple.com/videos/wwdc/2019/703yqvyjv9mi5ur6/703/703_all_about_notarization.pdf>). 

## Ad hoc signing

This is especially important on ARM64 M1 Apple processors which require all native code to be validly signed (if only ad hoc) or the operating system will not execute it, instead killing it on launch. To ad hoc sign an application: 
    
    
     codesign --force --deep -s - lazarus.app
    

or to ad hoc sign an executable file: 
    
    
     codesign --force -s - lazarus
    

A single dash/hyphen on its own for the identity makes it an ad hoc signature with no certificate. 

You can check the validity of an ad hoc signed executable with: 
    
    
     codesign -dv -r- lazarus
    

which should produce output similar to the below: 
    
    
    Executable=/Users/trev/lazarus-src/lazarus
    Identifier=lazarus
    Format=Mach-O thin (arm64)
    CodeDirectory v=20400 size=336137 flags=0x2(adhoc) hashes=10498+2 location=embedded
    Signature=adhoc
    Info.plist=not bound
    TeamIdentifier=not set
    Sealed Resources=none
    # designated => cdhash H"2e6f474452344d6fa42feee3db14a05c14d595a3"
    

In cases where you have compiled the executable yourself, the macOS linker will ad hoc sign the executable for you in which case you need do nothing. You can check the validity with the command above in which case the CodeDirectory output line will indicate that the linker signed the executable with an ad hoc signature as follows:: 
    
    
     CodeDirectory v=20400 size=336032 flags=0x20002(adhoc,linker-signed) hashes=10498+0 location=embedded
    

## Code signing an x86_64 binary on an aarch64 machine

macOS’s code signing architecture supports two different hash formats: 

  * sha1, the original hash format, which is now deprecated
  * sha256, the new format, support for which was added in macOS 10.11



`codesign` should choose the signing format based on the deployment target: 

  * If your deployment target is 10.11 or later, you get sha256.
  * If your deployment target is earlier, you get both sha1 and sha256.



A problem crops up because, when building for both Intel and Apple Silicon, your deployment targets are different. You might set the deployment target to 10.9 but, on Apple Silicon, that’s raised to the minimum Apple Silicon system, 11.0. So, which deployment target does it choose? 

Well, the full answer to that is complex but the executive summary is that it chooses the deployment target of the current architecture, that is, Intel if you’re building on Intel and Apple Silicon if you’re building on Apple Silicon. 

The upshot is that you have problems if your deployment target is less than 10.11 and you sign on Apple Silicon. When you run on, say, macOS 10.10, the system looks for a sha1 hash, doesn’t find it, and complains. 

The workaround is to supply the `--digest-algorithm=sha1,sha256` argument to `codesign` which overrides the hash choice logic in `codesign` and causes it to include both hashes. 

## How to install certificates on a second computer

There are two ways to do this: 

  1. using the Keychain Access application; and
  2. using Xcode.



### Using the Keychain Access application

  * On the first computer which has the certificates, open the Keychain Access application and create a new keychain. Now select your existing login keychain, and then My Certificates. Your certificates should be shown on the right. There should be a disclosure arrow on the left side, and selecting that should reveal your private key. Select each certificate, and copy and paste (do not drag or you'll remove it from your login keychain) it into your new keychain.


  * Copy the newly created keychain from your first computer to your second computer, open it with the Keychain Access application and copy/paste/drag the certificate to your second computer's login keychain.



### Using Xcode

  * On the first computer which has the certificates, open Xcode and go to Preferences > Accounts, select your account, select the gear symbol in the bottom left, choose Export Apple ID and Code Signing Assets, and then save the file.


  * Copy the developer profile file you saved on your first computer to the second computer, open Xcode, go to Preferences > Accounts, select your account, select the gear symbol in the bottom left, and then choose Import Apple ID and Code Signing Assets.



## How to check what certificates are installed

Once you have created or imported the certificates on your computer, you can check for their presence by opening an Applications > Utilities > Terminal and executing the following command: 
    
    
    $ security find-identity -p basic -v
    

This command will list all available certificates on this computer. Make sure that you can see the presence of the _Developer ID Application_ and _Developer ID Installer_ certificates. If you are a member of more than one team, you may see multiple certificates for each team. 

You can identify the certificates by the hexadecimal number or by the descriptive name, eg "Developer ID Application: Your Name (AB123456DE)" 

The ten character alpha-numeric code at the end of your name is your Developer Team ID. If you are a member of multiple developer teams, you will have multiple certificates and the team ID will help you distinguish them. 

## See also

  * [Code signing: Intel vs Apple Silicon](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers")
  * [Apple Developer Certificates](<Apple_Developer_Certificates.md> "Apple Developer Certificates")
  * [Notarization for macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+")
  * [Hardened runtime for macOS](<Hardened_runtime_for_macOS.md> "Hardened runtime for macOS")
  * [Sandboxing for macOS](<Sandboxing_for_macOS.md> "Sandboxing for macOS")
  * [Signing and Notarization FAQ](<Signing_and_Notarization_FAQ.md> "Signing and Notarization FAQ")
  * [macOS Gatekeeper Debugging Tips](<macOS_Gatekeeper_Debugging_Tips.md> "macOS Gatekeeper Debugging Tips")



## External links

  * [Apple Developer portal](<https://developer.apple.com/>)
  * [Apple: Code Signing Overview](<https://developer.apple.com/support/technical/code-signing/>)
  * [Apple: Code Signing Guide](<https://developer.apple.com/library/mac/documentation/Security/Conceptual/CodeSigningGuide/>)
  * [Apple: TN2206 - macOS Code Signing In Depth](<https://developer.apple.com/library/archive/technotes/tn2206/_index.html>)
  * [Apple: Code signing x86_64 binaries on aarch64](<https://developer.apple.com/forums/thread/673323>).

---

_Source: [https://wiki.freepascal.org/Code_Signing_for_macOS](https://web.archive.org/web/20250415000106/https://wiki.freepascal.org/Code_Signing_for_macOS)_
