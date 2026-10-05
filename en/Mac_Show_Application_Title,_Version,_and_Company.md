# Mac Show Application Title, Version, and Company

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/Mac_Show_Application_Title,_Version,_and_Company.md> "Mac Show Application Title, Version, and Company/ru")** │ 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This only works **if** the software has an application bundle; otherwise please see [Show Application Title, Version, and Company](<Show_Application_Title,_Version,_and_Company.md> "Show Application Title, Version, and Company")

An About window, also called an About Box or About Panel, is an optional window that contains your application’s version and copyright information and may also optionally include credits acknowledging people and code you did not write. Unlike other windows, an About window combines some of the behaviors of panels and windows: Like a panel, an About window is not listed in the application’s Window menu and like a window, it remains visible when the application is inactive. An About window should be modeless so the user can leave it open and perform other tasks in the application. 

Give your macOS application a professional look by using an About window which can be created in one of several different ways: the complex way, the simple way or the overridden simple way. Read on... 

  


## Contents

  * 1 The simple way
  * 2 The overridden simple way
  * 3 The complex way - now outdated
  * 4 See also
  * 5 External links



## The simple way

Thanks to the wonders of the Apple Cocoa Framework, the easy way to create an About Box for an application comprises just one line of code, one unit added to the _Uses_ clause and one HTML file added to your [Application Bundle](<Application_Bundle.md> "Application Bundle") Resources directory. 
    
    
    ...
    
    Uses
      CocoaAll,
      ...;
    
      ...
      NSApp.orderFrontStandardAboutPanel(Nil);
      ...
    
    ...
    

Sample output: 

[![Easy macOS About Box.jpg](https://wiki.freepascal.org/images/1/11/Easy_macOS_About_Box.jpg)](</File:Easy_macOS_About_Box.jpg>)

Where does this information come from? Your `Info.plist` [property list file](<macOS_property_list_files.md> "macOS property list files") provides the following details: 

  * **Application Icon**



Uses the string value of the `CFBundleIconFile` key. You don't have to include the filename extension in this string value, but you can if you want. If you do not, macOS is smart enough to search for a file of this name that ends with the `.icns` extension. If not available, the generic application icon is used. 

  * **Application Name**



Uses the string value of the `CFBundleName` key which is localisable. If not found, the system uses the `NSProcessInfo.processInfo.processName` (ie the name of your executable). This string is what is displayed in the menu bar when your application is active. 

  * **Application Version String**



Uses the value of the `CFBundleShortVersionString` key. If not found, the build version, if available, is printed alone as "Version x.x.x". This string is useful for actual version identification, but can be thought of as the "marketing" version. As a result, it is typically a more general version number, such as 1.0, and can contain alpha characters with no ill side effects. This value is displayed in the Finder preview too. 

  * **Build Version String**



Uses the value of the `CFBundleVersion` key. Displays the build version number of the application displayed as "(v1.0.0)". If not found, the version area is left blank (the "(v)" is not displayed). 

  * **Copyright**



Uses the string value of the `NSHumanReadableCopyright` key from the Info.plist file. If not found, the copyright area is left blank. 

The **Credits section** comes from a file named `Credits.html` in the Resources directory of your [bundle](<Application_Bundle.md> "Application Bundle"). If this file is not found, the system then looks for a file named `Credits.rtf`. If not found, the system looks for a file named `Credits.rtfd`. If not found, the credits area is left blank. Note that the capitalization of the file name is important. By using the HTML format, you can include links if necessary (eg to your web site). 

The `Info.plist` [property list file](<macOS_property_list_files.md> "macOS property list files") used in the sample above was: 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
    	<key>CFBundleDevelopmentRegion</key>
    	<string>English</string>
    	<key>CFBundleExecutable</key>
    	<string>project1</string>
    	<key>CFBundleName</key>
    	<string>My Amazing Application</string>
    	<key>CFBundleIdentifier</key>
    	<string>com.company.project1</string>
    	<key>CFBundleInfoDictionaryVersion</key>
    	<string>6.0</string>
    	<key>CFBundlePackageType</key>
    	<string>APPL</string>
    	<key>CFBundleSignature</key>
    	<string>proj</string>
    	<key>CFBundleShortVersionString</key>
    	<string>1.0</string>
    	<key>CFBundleVersion</key>
    	<string>11</string>
    	<key>CFBundleIconFile</key>
    	<string>ss4200utility.icns</string>
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
    	<key>NSHumanReadableCopyright</key>
    	<string>Copyright 2021, Your Full Name</string>
    </dict>
    </plist>
    

The HTML file used in the sample above was: 
    
    
    <center>
    <p>
    <h3>Credits</h3>
    <p>
    Free Pascal Team
    <br/>
    Lazarus Team
    <br/>
    Free Pascal and Lazarus Forums
    <br/>
    Free Pascal and Lazarus Wiki
    <br/>
    </center>
    

If the HTML text is short enough, no white scrollbox will appear and the text will appear on the default grey background. 

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_about_boxes/>). 

## The overridden simple way

This is a variation on the simple way described above, but allows us to override some or all of the information which was previously extracted from the `Info.plist` file in the [Application Bundle](<Application_Bundle.md> "Application Bundle"). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** In versions of macOS before 10.13 (High Sierra) only the Copyright information can be overridden - from macOS 10.13 onwards, all of the information extracted from the `Info.plist` file can be overridden.
    
    
    ...
    {$mode objfpc}{$H+} 
    {$modeswitch objectivec1}
    ...
    
    Uses
      CocoaAll,
      ...;
    
    procedure TForm1.MenuItemAboutClick(Sender: TObject);
    var
      dict: NSDictionary;
      val1: NSString;
      val2: NSString;
      val3: NSString;
      val4: NSString;
      val5: NSImage;
      key1: NSString;
      key2: NSString;
      key3: NSString;
      key4: NSString;
      key5: NSString;
    begin
      key1 := NSStr('Copyright');
      val1 := NSStr('Copyright 2021, An Amazing Author');
      Key2 := NSStr('ApplicationName');
      val2 := NSStr('My Overriden Application');
      key3 := NSStr('Version');
      val3 := NSStr('Beta v1.29c');
      key4 := NSStr('ApplicationVersion');
      val4 := NSStr('Version 1.0.0');
    
      key5 := NSStr('ApplicationIcon');
      val5 := NSImage.alloc.init; 
      val5 := val5.imageNamed(NSStr('PICinfo.icns'));
    
      dict := NSDictionary.alloc.initWithObjectsAndKeys(Val1, Key1, Val2, Key2, Val3, Key3, Val4, Key4, Val5, Key5, Nil);
    
      NSApp.orderFrontStandardAboutPanelWithOptions(Dict);
    
      dict.release;
      dict := Nil;
    end;
    

The sample output using the same `Credits.html` and `Info.plist` files from The Simple Way example above, plus the new icon file `PICinfo.icns` located in the Resources directory of the application bundle: 

[![Overidden Easy macOS About Box.jpg](https://wiki.freepascal.org/images/8/8f/Overidden_Easy_macOS_About_Box.jpg)](</File:Overidden_Easy_macOS_About_Box.jpg>)

Full project source code is available from [SourceForge](<https://sourceforge.net/p/lazarus-wiki-projects/code/ci/master/tree/laz_about_boxes2/>). 

## The complex way - now outdated

![<translate> Warning: </translate>](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/OOjs_UI_icon_notice-destructive.svg/18px-OOjs_UI_icon_notice-destructive.svg.png) **Warning**|  This About Box depends on the 32 bit Carbon Interface which Apple removed from macOS 10.14 (Catalina) in 2019 in favour of the 64 bit Cocoa Interface.  
---|---  
  
**Beware** _CFBundleGetMainBundle_ does not really return **nil** if the application has no bundle. Instead it tries to create that handle. See [Apple's documentation](<https://developer.apple.com/documentation/corefoundation/1537085-cfbundlegetmainbundle?language=objc>). So we should check for the existence of _ValueRef_ too. 
    
    
    // CODE FOR SHOWING APPLICATION TITLE, VERSION, AND COMPANY
    uses MacOSAll, CarbonProc, StrUtils;
    
    var
      BundleID: String;
      BundleName: String;
      BundleRef: CFBundleRef;
      BundleVer: String;
      CompanyName: String;
      KeyRef: CFStringRef;
      ValueRef: CFTypeRef;
    
    // Retrieve information from the Info.plist file in the application bundle
    function GetInfoPlistString(const KeyName : string) : string;
    begin
      try
        Result := '';
        BundleRef := CFBundleGetMainBundle;
        if BundleRef = nil then Exit;  {Executable not in an app bundle?}
        KeyRef := CFStringCreateWithPascalString(nil,KeyName,kCFStringEncodingUTF8);
        ValueRef := CFBundleGetValueForInfoDictionaryKey(BundleRef, KeyRef);
        if ValueRef = nil then Exit;  {Executable not in an app bundle!}
        if CFGetTypeID(ValueRef) <> CFStringGetTypeID then Exit;  {Value not a string?}
        Result := CFStringToStr(ValueRef);
      except
      on E : Exception do
        ShowMessage(E.Message);
      end;
      FreeCFString(KeyRef);
    end;
    
    // Display the information in an About Box
    procedure TForm1.FormCreate(Sender: TObject);
    begin
       try
         Form1.Caption := 'About '+Application.Title;
         StaticTextAppTitle.Caption := Application.Title;
         BundleID := GetInfoPlistString('CFBundleIdentifier');
         // CompanyName is presumed to be in the form of: com.Company.AppName
         CompanyName := AnsiMidStr(BundleID,AnsiPos('.',BundleID)+1,Length(BundleID));
         CompanyName := AnsiMidStr(CompanyName,0,AnsiPos('.',CompanyName)-1);
         BundleVer := GetInfoPlistString('CFBundleVersion');
         StaticTextAppVer.Caption := Application.Title+' version '+BundleVer;
         StaticTextCompany.Caption := CompanyName;
       except
       on E : Exception do
              ShowMessage(E.Message);
       end;
    end;
    

Sample output: 

[![About1.png](https://wiki.freepascal.org/images/a/a7/About1.png)](</File:About1.png>)

Note that Apple-compliant About Boxes should not contain a caption in the title bar. Oops. 

## See also

  * [Mac Preferences and About Menu](<Mac_Preferences_and_About_Menu.md> "Mac Preferences and About Menu")
  * [Show Application Title, Version, and Company](<Show_Application_Title,_Version,_and_Company.md> "Show Application Title, Version, and Company").
  * [Locating the macOS application resources directory](<Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory")



## External links

  * [Apple: CFBundle Overview](<https://developer.apple.com/documentation/corefoundation/cfbundle-s0a>).


  * [Apple: orderFrontStandardAboutPanel](<https://developer.apple.com/documentation/appkit/nsapplication/1428724-orderfrontstandardaboutpanel>).


  * [Apple: orderFrontStandardAboutPanelWithOptions](<https://developer.apple.com/documentation/appkit/nsapplication/1428479-orderfrontstandardaboutpanelwith>).


  * [Apple: NSAboutPanelOptionKey](<https://developer.apple.com/documentation/appkit/nsaboutpaneloptionkey>).

---

_Source: [https://wiki.freepascal.org/Mac_Show_Application_Title%2C_Version%2C_and_Company](https://web.archive.org/web/20240907053815/https://wiki.freepascal.org/Mac_Show_Application_Title%2C_Version%2C_and_Company)_
