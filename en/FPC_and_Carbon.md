# FPC and Carbon

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Apple removed the 32 bit Carbon framework from macOS 10.15 Catalina in 2019 at the same time as removing support for running any 32 bit software. **For macOS 10.15 onwards, you must use the 64 bit Cocoa framework.** See: [Carbon Core Deprecations](<https://developer.apple.com/library/archive/releasenotes/General/CarbonCoreDeprecations/index.html#//apple_ref/doc/uid/TP40012224>) and [Introduction to 64-Bit Guide for Carbon Developers](<https://developer.apple.com/library/archive/documentation/Carbon/Conceptual/Carbon64BitGuide/Introduction/Introduction.html>)

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/60px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

Carbon is a procedural API currently available on macOS and Mac OS Classic 8.1 or later. It can be utilized to create full-fledged native Macintosh applications integrated with its Aqua look. An alternative to the 32 bit Carbon API is Apple's 64 bit [Cocoa API](<Cocoa_Interface.md> "Cocoa Interface"). This document offers specific information about how to use the Carbon API using FPC. 

## Contents

  * 1 Pascal Bindings
  * 2 Carbon Tutorial



## Pascal Bindings

The Carbon bindings are located in the unit MacOSAll, which comes with all standard installations of FPC on the Macintosh. 

## Carbon Tutorial

If you want to create a Carbon application without using LCL components, you'll have to use the Carbon API to achive this goal. This little tutorial shows how to create an application using only CarbonAPI functions, XCode and Interface Builder to create the user interface. 

  * Create a projects folder and create an empty .pas file there, or you can also start a new project using Lazarus (Project->New Project->Custom Application)
  * Save an empty file (or empty project for Lazarus) and leave it for now.



All Mac OS GUI applications must be provided as bundles. Bundle is specially formed set of folders and files, that contain all information necessary to run a application. 

  * Create an application bundle with the same name as empty .pas file. The script to create a bundle can be found here: [XDev Toolkit](<XDev_Toolkit.md> "XDev Toolkit")



(if you're using Lazarus, you can create it easily Project Options -> Create Application Bundle); 

To create a GUI application, will need to use Apple's Interface Builder utility. The utility is provided with the XCode tools and can be found at /Developer/Applications/. It's not necessary to use Interface Builder to create GUI applications, but IB makes it easier. 

  * Open Interface Builder


  * Select template Carbon -> Application. This will create a template for a carbon application containing main menu and window.



The Interface Builder may look familiar in some way. It includes a 'component palette' and Library and an 'object inspector.' To open the inspector, press cmd-shift-I or select it from the main menu Tools->Inspector) 

  * From the created menu, add a Static Text object (Label). To add a control simply drag it to the window. Double click on the newly added Static Text to change its caption for example to the 'Hello World';


  * Save this application template to the projects folder. The name should be set to 'main' and file the type must be nib (3.x or 2.x version for Leopard, .nib extension would be added automatically)


  * Copy the newly created main.nib file to the application bundle. yourBundle.app/Contents/Resources/. The Resources folder is the place where any nib file would be searched.



if you're using the Finder you can access application folder by 'Show Package Content' if you're using the console command line interface, you can copy by the following command cp main.nib -r ./yourBundle.app/Contents/Resources/main.nib (where yourBundle.app is your application bundle name) 

Now, let's return to the empty .pas file (or empty project file), that was perviously created. Set .pas code to the following 
    
    
     uses
       MacOSAll;
            
     var
       err     : OSStatus;
       nibRef  : IBNibRef = nil;
       window  : WindowRef = nil;
         
     begin
       err := noErr;
        
       try
         // Create a Nib reference passing the name of the nib file (without the .nib extension)
         // CreateNibReference only searches into the application bundle.
         err := CreateNibReference(CFSTR('main'), nibRef);
         if err <> noErr then  Exit; 
                  
         // Once the nib reference is created, set the menu bar. "MainMenu" is the name of the menu bar
         // object. This name is set in InterfaceBuilder when the nib is created.
         err := SetMenuBarFromNib(nibRef, CFSTR('MainMenu'));
         if err <> noErr then Exit; 
                
         // Then create a window. "MainWindow" is the name of the window object. This name is set in
         // InterfaceBuilder when the nib is created.
         err := CreateWindowFromNib(nibRef, CFSTR('Window'), window);
         if err <> noErr then Exit; 
            
       finally
         // We don't need the nib reference anymore.
         DisposeNibReference(nibRef);
       end;
         
       // The window was created hidden so show it.
       ShowWindow(window);
         
       // Running main message loop that starts the application.  
       RunApplicationEventLoop;
     end.
    

  * Save the file.
  * Now you'll need to build it.



Remember, Free Pascal must know there the MacOSAll file is located (you should specify the path to it in /etc/fpc.cfg file) The command line is the following. 
    
    
    fpc yourMainFile.pas -k-framework -kCarbon
    

if have not created a hardlink to the file, you'll need to create one or copy the executable (you'll also have to copy executable every time you rebuild the project); 
    
    
    cp youMainFile ./youBundle.app/Contents/MacOS/
    

If you're using Lazarus, you can simply select Run->Build, you can also launch the application.

---

_Source: [https://wiki.freepascal.org/FPC_and_Carbon](https://web.archive.org/web/20250522164135/https://wiki.freepascal.org/FPC_and_Carbon)_
