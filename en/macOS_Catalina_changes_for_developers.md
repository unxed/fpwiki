# macOS Catalina changes for developers

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 File system changes
  * 2 Signing and Notarization of applications
  * 3 Gatekeeper and user privacy protection
    * 3.1 What does Gatekeeper check?
    * 3.2 When does gatekeeper check?
    * 3.3 Quarantine
    * 3.4 Notarisation
    * 3.5 Non-quarantined software
    * 3.6 User privacy protection
  * 4 Rebuilt applications re-trigger access consent dialogs
  * 5 Features that have been removed
  * 6 Deprecations: features to be removed
  * 7 External links



## File system changes

  * macOS Catalina is installed on a dedicated read-only system volume called "Macintosh HD", while your files and data are stored separately in another volume named "Macintosh HD - Data". The APFS-formatted disk uses a space-sharing "container" (known as a Volume Group) that can house multiple secure "volumes" or file systems. This allows the disk's free space to be shared on demand and allocated to any of the individual volumes in the container as required. In practice, users should not notice any difference after the split, since both volumes appear in Finder as a single unified Macintosh HD volume.


  * The entire root file system is read-only - it is not writable by a normal user or an Admin user. The data volume is mounted with read-write access as /System/Volumes/Data. No user can write to /usr anymore, but an Admin user (eg during installation) can write to /usr/local/ (BSD preferred) and /opt (SysV preferred).



## Signing and Notarization of applications

  * [Signing](<Code_Signing_for_macOS.md> "Code Signing for macOS") and [notarization](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+") is required for any macOS application that is distributed outside of the Mac App Store to avoid triggering Gatekeeper and scary security warnings. The standard double-click will only run code that has been signed and notarized. Signing and notarization requires membership of the [Apple Developer Program](<https://developer.apple.com/programs/>) (fee $US 99).


  * Gatekeeper can still be bypassed by right-clicking (or Control+clicking) on the application, and then selecting Open. This is _currently_ only required the first time an application is run, but may change in the future.



## Gatekeeper and user privacy protection

If something gets past Gatekeeper, user privacy protections may block malicious software from accessing user data. 

### What does Gatekeeper check?

  * Malicious content scan
  * Signature validation - has it been tampered with?
  * Does it meet the Mac's security policy? (Mac App Store only, Mac App Store and Developer ID, None)
  * Does the user want to run it? (Ask the user if they want to run it.)



### When does gatekeeper check?

When does Gatekeeper check on Mojave? 

  * Gatekeeper checks on the first launch of quarantined content.



### Quarantine

  * Marks files that arrive on the system from a variety of external sources.
  * Adds metadata about the external source (eg web address, etc).
  * Applications can opt-in to quarantining files.
  * Default for files written by sandboxed applications.



### Notarisation

  * Local policy check: All new software requires notarization. Also from Mojave 10.14.5.
  * First launch prompt: User must approve software in bundles.



### Non-quarantined software

  * All software (quarantined and non-quarantined) now gets the malicious content scan.



### User privacy protection

On Catalina users have to consent for an application to use: 

  * Camera
  * Microphone
  * Screen recording
  * Keyboard input monitoring (except for the applications own input)
  * Files and folders protection: 
    * Data that requires user consent to access
    * Private data which is managed by the system.



New protected areas in Catalina: 

  * Desktop
  * Documents
  * Downloads
  * iCloud Drive
  * Third-party cloud storage (Dropbox, OneDrive, Box, etc.)
  * Removable volumes
  * Network volumes



User consent is not required to create new files in protected locations. Only reading data from protected locations. Files can be checked to see if they're readable/writable without triggering consent dialogs. 

Private data managed by the system: 

  * Mail
  * Messages
  * Safari browsing history
  * HTTP cookies
  * Call History
  * iTunes backups
  * Time Machine backups
  * Trash



An application does not need Full Disk Access to move a file to the Trash, but needs authorization to the file being moved. The caller retains access to the file, even once it's in the Trash. 

See further: [Property List Files - Protected Resources](<macOS_property_list_files.md> "macOS property list files"). 

## Rebuilt applications re-trigger access consent dialogs

Each new build of an application is recognized as different application, so it again asks for access consents. According to Eskimo (Apple Forum Dev): Most problems like this are caused by the application not being code signed, so the system can’t be sure that the new build of the application is the same code as the previous build that was previously okayed. 

## Features that have been removed

  * macOS frameworks are now thinned for the x86-64 architecture. Applications that execute i386 (ie 32 bit) code now fail with the EBADARCH error code. The remaining stub frameworks are nonfunctional and exist only for compatibility purposes.


  * Most of the Carbon 32 bit framework has been removed. You must now use the Cocoa 64 bit Widget Set.


  * Legacy Core Audio HAL audio hardware plug-ins are no longer supported. Use Audio Server plug-ins for audio drivers.


  * The previously deprecated 32-bit QuickTime framework is no longer available in macOS 10.15.


  * The symbols for QTKit, which relied on the QuickTime framework, are still present but the classes are non-functional.


  * Command line tool support for Subversion — including svn, git-svn, and related commands — is no longer provided by Xcode. If you need Subversion or related command line tools, install the Command Line Tools package by running xcode-select --install.



## Deprecations: features to be removed

  * The OpenAL framework is deprecated and remains present for compatibility purposes. Transition to AVAudioEngine for spatial audio functionality.


  * AUGraph is deprecated in favor of AVAudioEngine.


  * Inter-App audio is deprecated. Use Audio Units for this functionality.


  * Carbon component-based Audio Units are deprecated and support will be removed in a future release.


  * The kauth API has been deprecated.


  * The Quartz Composer framework is deprecated and remains present for compatibility purposes. Transition to frameworks such as Core Image, SceneKit, or Metal.


  * Scripting language runtimes such as Python, Ruby, and Perl are included in macOS for compatibility with legacy software. Future versions of macOS will not include scripting language runtimes by default, and might require you to install additional packages. If your software depends on scripting languages, it is recommended that you bundle the runtime within the app.


  * Use of Python 2.7 is not recommended as this version is included in macOS for compatibility with legacy software. Python's own EOL date for version 2.7 was 1 January 2020. Future versions of macOS will not include Python 2.7. Instead, it is recommended that you run python3 from within Terminal.



## External links

  * [Apple: macOS Catalina 10.15 Release Notes](<https://developer.apple.com/documentation/macos_release_notes/macos_catalina_10_15_release_notes>).


  * [Apple: About the read-only system volume](<https://support.apple.com/en-ca/HT210650>).

---

_Source: [https://wiki.freepascal.org/macOS_Catalina_changes_for_developers](https://web.archive.org/web/20240225154132/https://wiki.freepascal.org/macOS_Catalina_changes_for_developers)_
