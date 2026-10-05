# Application Icon

│ **English (en)** │

The application icon is usually displayed on the main window of the application, and it can be changed as per the code in [Changing application Icon](<Changing_application_Icon.md> "Changing application Icon"). 

To change the icon of the executable itself, it is necessary to employ a platform-dependent technique. In Lazarus 0.9.27 support for this was added to the Project Options dialog, but it currently doesn't work for Linux because it requires calling an application to set the icon. 

## Contents

  * 1 IDE support for the Application Icon
  * 2 Platform-specific techniques
    * 2.1 Windows
    * 2.2 Setting the Application Icon on macOS
    * 2.3 Linux
      * 2.3.1 K Desktop Environment (KDE)
      * 2.3.2 GNOME
      * 2.3.3 LXDE



## IDE support for the Application Icon

Just set the icon in the Project Options dialog, accessible in the Project menu. 

Works for Windows and macOS. 

## Platform-specific techniques

### Windows

1\. Create a new file named "project.rc" (for example) containing: 
    
    
      MAINICON ICON "editor.ico" 
    

2\. Include in you project *.lpr file the following instruction: 
    
    
      {$R project.rc} 
    

Work with version 0.9.24 and above. 

3\. In the article [Windows Icon](<Windows_Icon.md> "Windows Icon") you can see the best practices for creating the icon. 

### Setting the Application Icon on macOS

Under macOS it is necessary to set an icon for the Application Bundle. This is done by adding a field to the Info.plist file, like this: 
    
    
       <key>CFBundleIconFile</key>
       <string>iconfile.icns</string>
    

Where iconfile.icns is located inside MyBundle.app/Contents/Resources 

You can find instructions to create an icns file [here](<http://www.macinstruct.com/node/59>)

### Linux

Under Linux application icons are located in special directories which are different on each Window Manager. The structure inside that directory, however, is standardized and described on the [Icon Theme Specification](<http://www.freedesktop.org/Standards/icon-theme-spec>). 

In order to determine how an application is launched the operating system uses a text file with the extension _.application_. This file provides different information including a description of the application, categories and locations of the executable and the icon. The standard is described in the [Desktop Entry Specification](<http://www.freedesktop.org/wiki/Specifications/desktop-entry-spec/>) of freedesktop.org. 

#### K Desktop Environment (KDE)

You can find the directory for application icons for use by all users and for each user using the command: 
    
    
    kde-config --path icon
    

This should print a list of colon-separated paths to stdout. 

#### GNOME

You can find the directory for application icons for use by all users and for each user using the command: 
    
    
    gnome-config --datadir
    

This should print a path to stdout, inside which is found a directory called pixmaps that attends to the Icon Theme Specification. 

#### LXDE

In LXDE, icons are located in the directory 
    
    
    /usr/share/pixmaps
    

.application files are in 
    
    
    /usr/share/applications

---

_Source: [https://wiki.freepascal.org/Application_Icon](https://web.archive.org/web/20240121090443/https://wiki.freepascal.org/Application_Icon)_
