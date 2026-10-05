# macOS property list files

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
    * 1.1 Data types
    * 1.2 Data formats
    * 1.3 XML, JSON or binary?
  * 2 Info.plist
    * 2.1 Creating an Info.plist
    * 2.2 Editing an Info.plist
    * 2.3 Adding keys to an Info.plist
    * 2.4 Syntax checking
    * 2.5 Recommended Key/Value pairs
      * 2.5.1 iOS
      * 2.5.2 macOS Cocoa
    * 2.6 Localizing an Info.plist
    * 2.7 Protected Resources
  * 3 See also
  * 4 External links



## Overview

A property list is a representation of a hierarchy of objects that can be stored in the file system and reconstituted later. Property lists give applications a lightweight and portable way to store small amounts of data. They are hierarchies of data made from specific types of objects—they are, in effect, an object graph. Property lists are easy to create programmatically and are even easier to serialize into a representation that is persistent. Applications can later read the static representation back into memory and recreate the original hierarchy of objects. Both Cocoa Foundation and Core Foundation have APIs related to property list serialization and deserialization. 

### Data types

Property lists consist only of certain types of data: dictionaries, arrays, strings, numbers (integer and float), dates, binary data, and Boolean values. Dictionaries and arrays are special types because they are collections; they can contain one or multiple data types, including other dictionaries and arrays. This hierarchical nesting of objects creates a graph of objects. The abstract data types have corresponding Foundation classes, Core Foundation types, and XML elements for collection objects and value objects. 

### Data formats

You can write property lists out in XML, JSON and binary formats. The binary format is much more compact than the XML version and thus more efficient. It is recommended for most situations. However, you can manually edit an XML property list if you ever need to. You can also edit a JSON file because it is just concatenated text and much less verbose than pure XML. Property list files have the filename extension of plist. 

You should not use property lists to store large, complex graphs of objects, especially when the objects have variable mutability settings. And you cannot use property lists to store objects that are not supported by the architecture, such as model objects. For these cases, use archiving instead. Although property lists can include NSData objects, it’s best to not use data objects in property lists to hold large amounts of binary data. 

### XML, JSON or binary?

The property list file type can be identified using the **file** command on macOS: 
    
    
     $ file org.sentry.picinfo.plist
     org.sentry.picinfo.plist: Apple binary property list           // binary format
    
    
    
     $ file org.sentry.picinfo.plist
     org.sentry.picinfo.plist: XML 1.0 document text, ASCII text    // XML format
    
    
    
     $ file org.sentry.picinfo.plist
     org.sentry.picinfo.plist: ASCII text, with no line terminators // JSON format
    

Converting between the different plist formats can be performed using the `plutil` command line utility on macOS: 
    
    
     $ plutil -convert binary1 org.sentry.picinfo.plist   // convert to binary format
    
    
    
     $ plutil -convert xml1 org.sentry.picinfo.plist      // convert to XML format
    
    
    
     $ plutil -convert json org.sentry.picinfo.plist      // convert to JSON format
    

## Info.plist

Every macOS and iOS application relies on the presence of special metadata in each application or bundle. This metadata is used in many different ways. Some of it is displayed to the user, some of it is used internally by the system to identify your application location, the icon to display, the document types it supports and many other behaviours that have an impact outside the bundle itself. Some of the metadata is used by the system frameworks to facilitate the launch of applications. The way an application provides its metadata to the system is through the use of a special file called an _information property list file_ which is named `Info.plist`. 

### Creating an Info.plist

The simplest way to create an information property list file for as macOS application is to let Lazarus create it for you. When you use the Lazarus Project > Project Options, and click the _Create application bundle_ option, Lazarus creates a default [application bundle](<Application_Bundle.md> "Application Bundle") and an `Info.plist` file which you can find in the project_name.app/Contents subdirectory. The file created by Lazarus comes preconfigured with basic key value pairs that every information property list should have. 

Here's an example of an `Info.plist` created by Lazarus for a simple macOS application: 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
      <key>CFBundleDevelopmentRegion</key>
      <string>English</string>
      <key>CFBundleExecutable</key>
      <string>MyApp</string>
      <key>CFBundleName</key>
      <string>MyApp</string>
      <key>CFBundleIdentifier</key>
      <string>com.company.MyApp</string>
      <key>CFBundleInfoDictionaryVersion</key>
      <string>6.0</string>
      <key>CFBundlePackageType</key>
      <string>APPL</string>
      <key>CFBundleSignature</key>
      <string>MyAp</string>
      <key>CFBundleShortVersionString</key>
      <string>0.1</string>
      <key>CFBundleVersion</key>
      <string>1</string>
      <key>CSResourcesFileMapped</key>
      <true/>
      <key>CFBundleDocumentTypes</key>
      <array>
        <dict>
          <key>CFBundleTypeRole</key>
          <string>Viewer</string>
          <key>CFBundleTypeExtensions</key>
          <array>
            <string>*</string>
          </array>
          <key>CFBundleTypeOSTypes</key>
          <array>
            <string>fold</string>
            <string>disk</string>
            <string>****</string>
          </array>
        </dict>
      </array>
      <key>NSHighResolutionCapable</key>
      <true/>
    </dict>
    </plist>
    

### Editing an Info.plist

To edit the contents of your `Info.plist` file, you can use any text editor that uses UTF-8 though it may be safer to use Xcode which understands the XML formatting of information property list files. Double-click the `Info.plist` filename in Finder which will automatically open the Xcode property list editor. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The property list editor in Xcode displays human-readable strings (instead of the actual key name) for many keys by default. To display the actual key names as they appear in the `Info.plist file`, Control-click any of the keys in the editor window and enable the Show Raw Keys/Values item in the contextual menu.

To edit the value for a specify key, double-click the value in the Xcode property list editor to select it, then type a new value. Most values are specified as strings but Xcode also supports several other scalar types. You can also specify complex types such as an array or dictionary. The property list editor displays an appropriate interface for editing each type. To change the type of a given value, make sure the value is not selected and Control-click it to display its contextual menu. From the Value Type submenu, select the type you want to use for the value. 

### Adding keys to an Info.plist

Although the `Info.plist` file generated by Lazarus contains the most critical keys required by the system, most applications should typically specify several additional keys. Many subsystems and system applicationss use the `Info.plist` file to gather information about your application. For example, when the user chooses File > Get Info for your application, the Finder displays information from many of these keys in the resulting information window. 

To add a key/value pair: 

  * Click the Add button (+) beside a key in the property list editor or select an existing property and press Return.
  * Choose a key from the pop-up menu (press the Down Arrow key to display it if it’s not visible) or type a new key name in the Key column.
  * Choose a type from the pop-up menu in the Type column.
  * Enter a value in the Value column.



To add a value to an array or dictionary, expand the disclosure triangle beside the array or dictionary. Next, click the Add button (+) or press Return to add a child property. 

To delete a key/value pair: 

  * Click the Remove button (—) beside a key in the property list editor or select a property and press Delete.



### Syntax checking

In cases where you have edited a property list file by hand rather than by using Xcode's property list editor, it is prudent to syntax check the file using the `plutil` command line utility on macOS. This is very handy because `plutil` will tell you the number of the line on which it finds an error. 

A successful syntax check: 
    
    
     $ plutil Info.plist
     Info.plist: OK
     
    

An syntax check where a problem has been found: 
    
    
     $ plutil Info.plist
     Info.plist: Found non-key inside <dict> at line 40
    

Of course `plutil` will syntax check _any_ property list file, not just `Info.plist`. 

### Recommended Key/Value pairs

#### iOS

An iOS application should include the following keys in its information property list file: 

  * CFBundleDevelopmentRegion
  * CFBundleDisplayName
  * CFBundleExecutable
  * CFBundleIconFiles
  * CFBundleIdentifier
  * CFBundleInfoDictionaryVersion
  * CFBundlePackageType
  * CFBundleVersion
  * LSRequiresIPhoneOS



In addition to these keys, there are several that are commonly included: 

  * UIRequiredDeviceCapabilities (required)
  * UIStatusBarStyle
  * UIInterfaceOrientation
  * UIRequiresPersistentWiFi



#### macOS Cocoa

A macOS Cocoa application should include the following keys in its information property list file. Most are set by Lazarus automatically when you create your application bundle, but some will need to be edited and some may need to be added. 

  * CFBundleDevelopmentRegion
  * CFBundleDisplayName
  * CFBundleExecutable
  * CFBundleIconFile
  * CFBundleIdentifier
  * CFBundleInfoDictionaryVersion
  * CFBundleName
  * CFBundlePackageType
  * CFBundleShortVersionString
  * CFBundleVersion
  * NSHumanReadableCopyright



### Localizing an Info.plist

The values for many keys in an information property list file are human-readable strings that are displayed to the user by the Finder or your own app. When you localize your app, you should be sure to localize the values for these strings in addition to the rest of your app’s content. 

Localized values are not stored in the `Info.plist` file itself. Instead, you store the values for a particular localization in a strings file with the name InfoPlist.strings. You place this file in the same language-specific project directory that you use to store other resources for the same localization. The contents of the InfoPlist.strings file are the individual keys you want localized and the appropriately translated value. The routines that look up key values in the `Info.plist` file take the user’s language preferences into account and return the localized version of the key (from the appropriate InfoPlist.strings file) when one exists. If a localized version of a key does not exist, the routines return the value stored in the `Info.plist` file. 

In addition to the recommended keys, there are several keys that should be localized and placed in your language-specific `InfoPlist.strings` files: 

  * CFBundleDisplayName
  * CFBundleName
  * CFBundleShortVersionString
  * NSHumanReadableCopyright



For example, TextEdit has several keys that are displayed in the Finder and thus should be localized. Suppose your information property list file defines the following keys: 
    
    
    <key>CFBundleDisplayName</key>
    <string>TextEdit</string>
    <key>NSHumanReadableCopyright</key>
    <string>Copyright © 1995-2009, Apple Inc.,All Rights Reserved.
    </string>
    

The French localization for TextEdit then includes the following strings in the InfoPlist.strings file of its `Contents/Resources/French.lproj` directory: 
    
    
    CFBundleDisplayName = "TextEdit";
    NSHumanReadableCopyright = "Copyright © 1995-2009 Apple Inc.\nTous droits réservés.";
    

### Protected Resources

With the introduction of macOS 10.15 (Catalina), users have to consent for an application to use: 

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

[![mac consent dialog.png](https://wiki.freepascal.org/images/d/df/mac_consent_dialog.png)](</File:mac_consent_dialog.png>)

When an application attempts to access, for example, a user's Documents folder, a default dialog will be displayed asking the user for permission. 

A sample of the generic dialog from macOS 11.2.3 (Big Sur) which asks for user consent is on the left. 

The user is shown with this dialog only the first time that the application is run. Note that if a developer rebuilds the application, and it is not signed, then the dialog will be shown again. 

To enable a developer to show a more helpful consent dialog which explains why access is required to the protected resource, a number of new property list keys are available: 

  


Raw Key | Key Name | Value | Available from macOS version   
---|---|---|---  
NSCalendarsUsageDescription | Privacy - Calendars Usage Description | String explaining why the application is requesting access to the user’s calendar data. | 10.14   
NSRemindersUsageDescription | Privacy - Reminders Usage Description | String explaining why the application is requesting access to the user’s reminders. | 10.14   
NSCameraUsageDescription | Privacy - Camera Usage Description | String explaining why the application is requesting access to the user's camera. | 10.14   
NSMicrophoneUsageDescription | Privacy - Microphone Usage Description | String explaining why the application is requesting access to the user's microphone. | 10.14   
NSContactsUsageDescription | Privacy - Contacts Usage Description | String explaining why the application is requesting access to the user’s contacts. | 10.8   
NSDesktopFolderUsageDescription | Privacy - Desktop Folder Usage Description | String explaining why the application needs access to the user’s Desktop folder. | 10.15   
NSDocumentsFolderUsageDescription | Privacy - Documents Folder Usage Description | String explaining why the application needs access to the user’s Documents folder. | 10.15   
NSDownloadsFolderUsageDescription | Privacy - Downloads Folder Usage Description | String explaining why the application needs access to the user’s Downloads folder. | 10.15   
NSNetworkVolumesUsageDescription | Privacy - Network Volumes Usage Description | String explaining why the application needs access to files on a network volume. | 10.15   
NSRemovableVolumesUsageDescription | Privacy - Removable Volumes Usage Description | String explaining why the application needs access to files on a removable volume. | 10.15   
NSFileProviderPresenceUsageDescription | Privacy - File Provider Presence Usage Description | String explaining why the application needs to be informed when other applications access files that it manages. | 10.15   
NSFileProviderDomainUsageDescription | Privacy - Access to a File Provider Domain Usage Description | String explaining why the application needs access to files managed by a file provider. | 10.15   
NSLocationUsageDescription | Privacy - Location Usage Description | String explaining why the application is requesting access to the user’s location information. | 10.14   
NSLocalNetworkUsageDescription | Privacy - Local Network Usage Description | String explaining why the application is requesting access to the local network. | 11.0   
NSPhotoLibraryUsageDescription | Privacy - Photo Library Usage Description | String explaining why the application is requesting access to the user’s photo library. | 10.14   
NSAppleEventsUsageDescription | Privacy - AppleEvents Sending Usage Description | String explaining why the application is requesting the ability to send Apple events. | 10.14   
NSSystemAdministrationUsageDescription | Privacy - System Administration Usage Description | String explaining why the application is requesting to manipulate the system configuration. | 10.14   
NSSpeechRecognitionUsageDescription | Privacy - Speech Recognition Usage Description | String explaining why the application is requesting to send user data to Apple’s speech recognition servers. | 10.15   
  
## See also

  * [Add an Apple Help Book to your macOS app](<Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app") \- `Info.plist` file changes needed to support an Apple Help Book.
  * [Fixing your application name](<Apple-specific_UI_elements.md> "Apple-specific UI elements") \- `Info.plist` file changes to turn "MyApp" into "My App".



## External links

  * [Apple: Information Property List](<https://developer.apple.com/documentation/bundleresources/information_property_list>)


  * [Apple: Property List Programming Guide](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html>)


  * [Apple: Information Property List Files](<https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AboutInformationPropertyListFiles.html>)


  * [Apple: macOS Info.plist Keys and Values](<https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html>)


  * [Apple: iOS Info.plist Keys and Values](<https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html>)


  * [Apple: Protected Resouces](<https://developer.apple.com/documentation/bundleresources/information_property_list/protected_resources>)

---

_Source: [https://wiki.freepascal.org/macOS_property_list_files](https://web.archive.org/web/20240909203323/https://wiki.freepascal.org/macOS_property_list_files)_
