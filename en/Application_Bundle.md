# Application Bundle

│ **English (en)** │  **[日本語 (ja)](</Application_Bundle/ja> "Application Bundle/ja")** │  **[русский (ru)](<../ru/Application_Bundle.md> "Application Bundle/ru")** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
  * 2 Application bundle layout
  * 3 Creating the Application Bundle
    * 3.1 Using Lazarus
    * 3.2 Using the Lazarus command-line tool
    * 3.3 Using a shell script
  * 4 Executing an application via the Application Bundle
  * 5 See also
  * 6 External links



## Overview

An application bundle is a directory with the extension ".app" on macOS systems. It contains the application executable, resource files, library files (if any), help files and information about the application and is necessary for the correct execution of applications. The Mac Finder treats this .app directory as the application file and by default does not show any of its sub-directories. 

From within Lazarus it is used for the [Carbon](<Carbon_Interface.md> "Carbon Interface") and [Cocoa](<Cocoa_Interface.md> "Cocoa Interface") interfaces, but one can also build applications with other interfaces (eg Gtk or Qt) by using scripts. 

You can learn more about application bundles in the Apple Bundle Programming Guide. 

Application (bundle) settings are located in the property list file: [Info.plist](<macOS_property_list_files.md> "macOS property list files") located in the `bundle.app/Contents/` directory. 

The **PkgInfo** file is an alternative way to specify the type and creator codes of your application or bundle. This file is not required, but can improve performance for code that accesses this information. Regardless of whether you provide this file, you should always include type and creator information in your information property list file using the CFBundlePackageType and CFBundleSignature keys, respectively. The contents of the PkgInfo file are the 4-byte package type followed by the 4-byte signature of your application. Thus, for the TextEdit application, whose type is 'APPL' and whose signature is 'ttxt', the file would contain the ASCII string “APPLttxt”. 

To access the application bundle contents in Finder, you need to right click (ctrl-left click) on a bundle and select _Show Package Contents_

[![openbundle.png](https://wiki.freepascal.org/images/d/d2/openbundle.png)](</File:openbundle.png>)

[![infoplist.png](https://wiki.freepascal.org/images/8/80/infoplist.png)](</File:infoplist.png>)

## Application bundle layout

The basic structure of a Mac application bundle: 
    
    
    MyApp.app/
       Contents/
          Info.plist
          MacOS/
          PkgInfo
          Resources/
    

Sub-Directory of the Contents directory | Usage description   
---|---  
MacOS | (Required) Contains the application’s standalone executable code. Typically, this directory contains only one binary file with your application’s main entry point and statically linked code. However, you may put other standalone executables (such as command-line tools) in this directory as well.   
Resources | Contains all of the application’s resource files. The contents of this directory are further organized to distinguish between localized and nonlocalized resources. For more information about the structure of this directory, see [Adding a Help Book - Bundle layouts](<Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app").   
Frameworks | Contains any private shared libraries and frameworks used by the executable. The frameworks in this directory are revision-locked to the application and cannot be superseded by any other, even newer, versions that may be available to the operating system. In other words, the frameworks included in this directory take precedence over any other similarly named frameworks found in other parts of the operating system. For information on how to add shared libraries to your application bundle, see [macOS Dynamic Libraries](<macOS_Dynamic_Libraries.md> "macOS Dynamic Libraries").   
PlugIns | Contains loadable bundles that extend the basic features of your application. You use this directory to include code modules that must be loaded into your application’s process space in order to be used. You would not use this directory to store standalone executables.   
SharedSupport | Contains additional non-critical resources that do not impact the ability of the application to run. You might use this directory to include things like document templates, clip art, and tutorials that your application expects to be present but that do not affect the ability of your application to run.   
  
## Creating the Application Bundle

### Using Lazarus

Open a project in the Lazarus IDE and go to Project -> Project Options -> Application tab and push the [Create Application Bundle](<IDE_Window__Project_Options.md> "IDE Window: Project Options") button. The resulting Application Bundle will contain a symbolic link to the real executable. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** You must remove the symbolic link and copy the real executable "project1" into the `project1.app/Contents/MacOS/` directory to distribute the application and use it on another computer.

### Using the Lazarus command-line tool

Open `/Applications/Lazarus/components/macfiles/examples/createmacapplication.lpi` in the Lazarus IDE. Compile. 

Open a Terminal of your choice and type: 
    
    
    cd project1/
    /Applications/Lazarus/components/macfiles/examples/createmacapplication project1
    ln -s ../../../project1 project1.app/Contents/MacOS/project1
    

### Using a shell script

You can adapt the following shell script to create a customized application bundle for your application. It allows the creation of either a debug or a release bundle. 

  * In the debug bundle, a link to the executable is placed, allowing for debugging using the Lazarus IDE,
  * In the release bundle, the executable is copied so the application bundle as a whole can be used stand-alone/copied to other locations.


    
    
    #!/bin/sh
    # Force Bourne shell in case tcsh is default.
    #
    
    #
    # Reads the bundle type
    #
    
    echo "========================================================"
    echo "    Bundle creation script"
    echo "========================================================"
    echo ""
    echo " Please select which kind of bundle you would like to build:"
    echo ""
    echo " 1 > Debug bundle"
    echo " 2 > Release bundle"
    echo " 0 > Exit"
    
    read command
    
    case $command in
    
      1) ;;
    
      2) ;;
      
      0) exit 0;;
    
      *) echo "Invalid command"
         exit 0;;
    
    esac
    
    #
    # Creates the bundle
    #
    
    appname=Magnifier
    appfolder=$appname.app
    macosfolder=$appfolder/Contents/MacOS
    plistfile=$appfolder/Contents/Info.plist
    appfile=magnifier
    
    PkgInfoContents="APPL????"
    
    #
    if ! [ -e $appfile ]
    then
      echo "$appfile does not exist"
    elif [ -e $appfolder ]
    then
      echo "$appfolder already exists"
    else
      echo "Creating $appfolder..."
      mkdir $appfolder
      mkdir $appfolder/Contents
      mkdir $appfolder/Contents/MacOS
      mkdir $appfolder/Contents/Frameworks  # optional, for including libraries or frameworks
      mkdir $appfolder/Contents/Resources
    
    #
    # For a debug bundle,
    # Instead of copying executable into .app folder after each compile,
    # simply create a symbolic link to executable.
    #
    if [ $command = 1 ]; then
      ln -s ../../../$appname $macosfolder/$appname
    else
      cp $appname $macosfolder/$appname
    fi  
    
    # Copy the resource files to the correct place
      cp *.bmp $appfolder/Contents/Resources
      cp icon3.ico $appfolder/Contents/Resources
      cp icon3.png $appfolder/Contents/Resources
      cp macicon.icns $appfolder/Contents/Resources
      cp docs/*.* $appfolder/Contents/Resources
    #
    # Create PkgInfo file.
      echo $PkgInfoContents >$appfolder/Contents/PkgInfo
    #
    # Create information property list file (Info.plist).
      echo '<?xml version="1.0" encoding="UTF-8"?>' >$plistfile
      echo '<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">' >>$plistfile
      echo '<plist version="1.0">' >>$plistfile
      echo '<dict>' >>$plistfile
      echo '  <key>CFBundleDevelopmentRegion</key>' >>$plistfile
      echo '  <string>English</string>' >>$plistfile
      echo '  <key>CFBundleExecutable</key>' >>$plistfile
      echo '  <string>'$appname'</string>' >>$plistfile
      echo '  <key>CFBundleIconFile</key>' >>$plistfile
      echo '  <string>macicon.icns</string>' >>$plistfile
      echo '  <key>CFBundleIdentifier</key>' >>$plistfile
      echo '  <string>org.magnifier.magnifier</string>' >>$plistfile
      echo '  <key>CFBundleInfoDictionaryVersion</key>' >>$plistfile
      echo '  <string>6.0</string>' >>$plistfile
      echo '  <key>CFBundlePackageType</key>' >>$plistfile
      echo '  <string>APPL</string>' >>$plistfile
      echo '  <key>CFBundleSignature</key>' >>$plistfile
      echo '  <string>MAG#</string>' >>$plistfile
      echo '  <key>CFBundleVersion</key>' >>$plistfile
      echo '  <string>1.0</string>' >>$plistfile
      echo '</dict>' >>$plistfile
      echo '</plist>' >>$plistfile
    fi
    

## Executing an application via the Application Bundle

You can start the application from the Lazarus IDE, by clicking on its Finder icon or in the native macOS Terminal.app by typing: 
    
    
    open project1.app
    

or 
    
    
    ./project1.app/Contents/MacOS/project1
    

## See also

  * [Add an Apple Help Book to your macOS app](<Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app")
  * [Deploying your macOS application](<Deploying_Your_Application.md> "Deploying Your Application")
  * [Hiding a macOS app from the Dock](<Hiding_a_macOS_app_from_the_Dock.md> "Hiding a macOS app from the Dock")
  * [macOS property list files](<macOS_property_list_files.md> "macOS property list files")



## External links

  * [Apple: NSBundle class](<https://developer.apple.com/documentation/foundation/nsbundle>)
  * [Apple: Bundle Resources](<https://developer.apple.com/documentation/bundleresources/>)
  * [Apple: CFBundle](<https://developer.apple.com/documentation/corefoundation/cfbundle>)
  * [Apple: Bundle Configuration](<https://developer.apple.com/documentation/bundleresources/information_property_list/bundle_configuration>)
  * [Apple: Resource Programming Guide](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html>)
  * [Apple: Bundle Programming Guide](<https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html>)

---

_Source: [https://wiki.freepascal.org/Application_Bundle](https://web.archive.org/web/20240907074251/https://wiki.freepascal.org/Application_Bundle)_
