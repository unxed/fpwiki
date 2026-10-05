# iOS Designer

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Known Issues:
  * 8 To do:
  * 9 Installation
  * 10 How to use
    * 10.1 Quick start
    * 10.2 Combination with XCode
    * 10.3 How does it work
    * 10.4 How does the XIB Designer work
    * 10.5 Good to know
  * 11 Examples
  * 12 Also See



### About

This package adds a designer for XIB-resource files to the Lazarus-IDE. XIB resource files are used to store iOS screen layouts and are normally designed using Xcode. The XIB-designer makes it easier to use iOS screen layouts in combination with Objective Pascal code. 

iOS applications created by this package are using the Objective-Pascal language extensions in order to use the native Objective-C Cocoa and iOS SDK objects. Therefor you need a copy of the iOS-SDK Pascal headers, which are generated from the headers in the iOS SDK. Furthermore you need a Free Pascal compiler for ARM if you want to deploy your applications to a real device, or a Free Pascal compiler for the iphonesim target, if you want to test your applications on the iPhone Simulator. More information about this can be found in the [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development") section. 

This package depends on the [iPhone Laz Extension](<iPhone_Laz_Extension.md> "iPhone Laz Extension") package, which is used for the integration with Xcode and some utilities from the iOS SDK. 

The extension does **NOT** provide another LCL WidgetSet (like Carbon or Cocoa) nor requires to use any. 

### Author

[Joost van der Sluis](</User:Loesje> "User:Loesje")

email: joost at cnoc dot nl 

### License

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). 

### Download

The latest stable release can be found at svn: 
    
    
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/iosdesigner
    

### Change Log

  * Version 0.1 Aug 2011
  * Version 0.2 Feb 2013



### Dependencies / System Requirements

  * Lazarus 1.1 (SVN trunk) or higher.
  * Mac OS X 10.6 + iPhoneSDK/Xcode
  * FPC 2.6.0
  * [iPhone Laz Extension](<iPhone_Laz_Extension.md> "iPhone Laz Extension")
  * Since Apple doesn't provide any tools for development on other than OS X system, the package could be installed and used on Windows or Linux, but in that case you can not compile your applications.



Status: _Alpha_

### Known Issues:

### To do:

## Installation

In order to develop for iPhone or iPod, you'll need arm FPC compiler with rtl and packages installed in the system. There're 2 ways to do it: built manually via SVN or installed from a special FPC arm package. Both ways are described [here](<iPhone/iPod_development.md> "iPhone/iPod development"). 

You don't need to install ARM compiler or packages, if you're planning to use iPhone simulator only. 

  * Make sure you have a Free Pascal compiler installed for ARM or the iPhoneSim target
  * Make sure you have the iOS header files installed. (The iPhoneAll unit)
  * Install the [iPhone Laz Extension](<iPhone_Laz_Extension.md> "iPhone Laz Extension") package
  * Get the package source via svn or download from Sourceforge
  * Open and install the package



## How to use

### Quick start

  * Start a new application (using File menu -> new ...) and choose for 'iOS application'.
  * The IDE will create a simple application with an empty XIB-layout and an application-delegate. By default the project settings are set to compile the application for the iPhone simulator.
  * Drop an UIWindow object from the component palette into the empty layout.
  * Maybe add a UIButton too, or else you won't see anything.
  * Save the project. (Or else installation on the Simulator will fail)
  * Build your application. It will be installed onto the iPhoneSimulator automatically.
  * Run the iPhone Simulator (using Project menu -> Run iPhone Simulator)
  * Run your application from within the Simulator.



### Combination with XCode

It is possible to create your XIB-resource with Lazarus, save it, and then open it in the Xcode Designer. The changes that you make in the Xcode designer will be preserved when you open it again in the Lazarus Designer. To open a XIB-layout in Xcode from within the Lazarus-IDE, right-click on the grey area in the Designer-window, and select 'Show in XCode'. Each time you save the XIB-resource in Lazarus, Xcode will detect a change and will reload the file and show your latest changes. This does not work (yet) the other way around, so to view your changes made (and saved) in Xcode, you have to close and reopen the file in Lazarus. 

[![iOSDesigner.png](https://wiki.freepascal.org/images/e/e5/iOSDesigner.png)](</File:iOSDesigner.png>)[![Simulator.png](https://wiki.freepascal.org/images/6/6b/Simulator.png)](</File:Simulator.png>)

[![XCodeDesigner.png](https://wiki.freepascal.org/images/f/fc/XCodeDesigner.png)](</File:XCodeDesigner.png>)

Note that it is almost impossible to add all properties, settings and objects from the Xcode designer to the Lazarus XIB-designer. But the Lazarus XIB Designer could be used to make the first layout and to add the basic objects, and then the Xcode designer could be used to fill in all the specific settings. Also the drawing of the designer is very rough. This could also be improved, but the final result could always be showed in the Xcode designer. 

If you want to deploy your application, it has to be signed. This can all be done from within Xcode. You can create a Xcode project for your Lazarus project using the Project menu -> update XCode project. Then you can open the project in Xcode and compile and run from there. 

### How does it work

By default the IDE creates an application-delegate. For each object that is placed onto the designer an outlet (reference) is created which links to the application-delegate. The same holds for events. It is possible to create multiple windows which are bound to one application-delegate. 

### How does the XIB Designer work

Normally, Lazarus designers do only work with components. In this case the objects in the designer are objective-c classes. To be able to design the XIB-resource file, some fake components are created and added to the IDE. Instead of storing their properties in memory, these fake components do use getters and setters to read their values. A XIB-file is in fact a xml-file. When the resource-file is loaded the xml is parsed and stored into a DOM-tree. All the properties get and set their values from this DOM-tree. This way, all xml-elements that are unknown to the Lazarus iOS designer are preserved. 

### Good to know

  * Each application has it's own Application-ID. It is not possible to install multiple applications onto one device (or simulator) with the same Application-ID. So keep in mind to change the Application-ID to something different than the default (using the Project menu -> Project options, on the iPhone tab)
  * When you need an object that is not yet available in the object designer, take a look at the ios_views.pas unit. It is very easy to add a component by your own. And when you've added such an object, please send me the patch so that others can benefit from your work, and spend their time by adding other objects that you might need in the future.



## Examples

There are two examples, one simple 'Hello world' application, and one applications to show how to use an UITableView with a delegate that implements the UITableViewDelegate and UITableViewDataSource protocols. 

## Also See

  * [iPhone Laz Extension](<iPhone_Laz_Extension.md> "iPhone Laz Extension")

---

_Source: [https://wiki.freepascal.org/iOS_Designer](https://web.archive.org/web/20220816111850/https://wiki.freepascal.org/iOS_Designer)_
