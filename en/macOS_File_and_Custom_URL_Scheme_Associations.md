# macOS File and Custom URL Scheme Associations

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
  * 2 Identifying the type of content in a file
  * 3 Simple MyEditor example application
  * 4 Adding your custom file type extension
  * 5 Adding your custom URL scheme
  * 6 Last words
  * 7 See also
  * 8 External links



## Overview

macOS does not have a central registry like Windows to which applications explicitly write information. Instead, every application with a graphical interface has a [property list file](<macOS_property_list_files.md> "macOS property list files") named `Info.plist`, roughly equivalent to a manifest in the Windows world, stored in its [application bundle](<Application_Bundle.md> "Application Bundle"). The `Info.plist` file may optionally include file type association and custom URL scheme information. 

macOS Launch Services is an API that enables a running application to open other applications or their document files in a way similar to the Finder or the Dock. Using Launch Services, an application can perform such tasks as: 

  * Open (launch or activate) another application
  * Open a document or a URL (uniform resource locator) in another application
  * Identify the preferred application for opening a given document or URL
  * Register information about the kinds of document files and URLs an application is capable of opening
  * Obtain appropriate information for displaying a file or URL on the screen, such as its icon, display name, and kind string
  * Maintain and update the contents of the Recent Items menu



Launch Services automatically registers an application with the database the first time the application becomes known to the system. This can occur when: 

  * The Finder reports an application has been added to the Applications folder.
  * An application installer is run.
  * When a document is opened that has no preferred application, the user is asked to select an application to use, and that application is registered with Launch Services.
  * When the built-in Launch Services tool is run whenever you boot your Mac or login as a user. This tool scans the Applications folder looking for any new applications that have been placed there.



When you open a document or URL, Launch Services is used to determine which application should open the item. Launch Services uses the following specific order: 

  * If the user has manually set a file association, then Launch Services will use that application to open the document or URL.
  * If the document has a file name extension, Launch Services will find all applications that list the extension as compatible.
  * If the document has a four-character file type, Launch Services finds all applications that accept that file type.
  * If more than one application is found, then: 
    * If the document has a four-character creator type that matches an application: 
      * Give preference to applications on the boot volume;
      * Give preference to applications on local volumes rather than remote volumes.
  * If two or more applications still meet the criteria, give preference to the newest version.



## Identifying the type of content in a file

There are two primary techniques for identifying the type of content in a file: 

  * Uniform Type Identifiers (UTIs)
  * Filename extensions



A UTI is a string that uniquely identifies a class of entities considered to have a "type". UTIs provide consistent identifiers for data on which all applications and services can recognise and rely. They are also more flexible than most other techniques because you can use them to represent any type of data, not just files and directories. Examples of UTIs include: 

  * public.text — A public type that identifies text data.
  * public.jpeg — A public type that identifies JPEG image data.
  * com.apple.bundle — An Apple type that identifies a bundle directory.
  * com.apple.application-bundle — An Apple type that identifies a bundled app.



Whenever a UTI-based interface is available for specifying file types, you should prefer that interface over any others. Many macOS interfaces allow you to specify UTIs corresponding to the files or directories you want to work with. 

One way the system determines the UTI for a given file is by looking at its filename extension. A filename extension is a string of characters appended to the end of a file and separated from the main filename with a period. Each unique string of characters identifies a file of a specific type. For example, the .png extension identifies a file with image data in the portable network graphics format. 

Note: Because period characters are valid characters in macOS filenames, only the characters after the last period in a filename are considered part of the filename extension. Everything to the left of the last period is considered part of the filename itself. 

If your application defines custom file formats, you should register those formats and any associated filename extensions in your application's `Info.plist` file. The `CFBundleDocumentTypes` key specifies the file formats that your application recognises and is able to open. Entries for any custom file formats should include both a filename extension and UTI corresponding to the file contents. The system uses that information to direct files with the appropriate type to your application. 

## Simple MyEditor example application

We shall start by creating a very simple editor application to which we will add our own custom file type extension. After that is working we will then add a custom URL scheme so that other applications (eg Safari) when faced with our custom URL scheme will offer to open files in our MyEditor application. 

  * Start a new Lazarus application
  * Drop a memo on the form
  * Drop three buttons on the form 
    * name one button btnOpen
    * name one button btnSave
    * name one button btnQuit
  * Drop an OpenDialog on the form
  * Drop a SaveDialog on the form
  * Copy the button click handlers from the code below into your unit1.pas file


    
    
    procedure TForm1.btnOpenClick(Sender: TObject);
    begin
      LoadMemo;
    end;
    
    procedure TForm1.btnSaveClick(Sender: TObject);
    begin
      SaveMemo;
    end;
    
    procedure TForm1.btnQuitClick(Sender: TObject);
    begin
      close;
    end;
    

  * Go to the [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"), select each button in turn, select the Events tab, select the OnClick event and double click the empty field. The correct event handler for each button should be chosen automatically.
  * In the private section, before the public section, of TForm1 in the Interface section at the top of the unit add:


    
    
        sTmp : string;
        procedure LoadMemo;
        procedure SaveMemo;
    

  * Add these two procedures after the button click handlers:


    
    
    procedure TForm1.LoadMemo;
    begin
    if OpenDialog1.Execute then
      sTmp:=OpenDialog1.Filename;
    
    if FileExists(sTmp) then
      memo1.Lines.LoadFromFile(sTmp)
     else
       showmessage('File not found: ' + sTmp);
    end;
    
    procedure TForm1.SaveMemo;
    begin
    
    if SaveDialog1.Execute then
      begin
        try
          memo1.Lines.SaveToFile(SaveDialog1.FileName);
        except on EStreamError do
            showmessage('Could not save file: ' + SaveDialog1.FileName);
        end;
      end
    end;
    

  * Compile and run the application. You should be able to Open a file in the editor, Save a file and Quit the application.


  * If you encounter any errors, compare your unit1.pas against the full unit1.pas below and make any necessary corrections:


    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        btnOpen: TButton;
        btnSave: TButton;
        btnQuit: TButton;
        Memo1: TMemo;
        OpenDialog1: TOpenDialog;
        SaveDialog1: TSaveDialog;
        procedure btnOpenClick(Sender: TObject);
        procedure btnQuitClick(Sender: TObject);
        procedure btnSaveClick(Sender: TObject);
      private
        sTmp : string;
        procedure LoadMemo;
        procedure SaveMemo;
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.btnOpenClick(Sender: TObject);
    begin
      Form1.LoadMemo;
    end;
    
    procedure TForm1.btnSaveClick(Sender: TObject);
    begin
      Form1.SaveMemo;
    end;
    
    procedure TForm1.btnQuitClick(Sender: TObject);
    begin
      Form1.Close;
    end;
    
    procedure TForm1.LoadMemo;
    begin
    if OpenDialog1.Execute then
      sTmp:=OpenDialog1.Filename;
    
    if FileExists(sTmp) then
      memo1.Lines.LoadFromFile(sTmp)
     else
       showmessage('File not found: ' + sTmp);
    end;
    
    procedure TForm1.SaveMemo;
    begin
    if SaveDialog1.Execute then
      begin
        try
          memo1.Lines.SaveToFile(SaveDialog1.FileName);
        except on EStreamError do
            showmessage('Could not save file: ' + SaveDialog1.FileName);
        end;
      end
    end;
    
    end.
    

## Adding your custom file type extension

Lazarus will have created an [application bundle](<Application_Bundle.md> "Application Bundle") for our MyEditor application. Use Finder to navigate to the project directory, right-click on the bundle directory MyEditor.app and select "Show package contents". Open the Contents subdirectory and you should see: a MacOS subdirectory, a Resources subdirectory, a Pkginfo file and the application's `Info.plist` [property list file](<macOS_property_list_files.md> "macOS property list files"). Open the file with TextEditor.app and the content should look very similar to this: 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
      <key>CFBundleDevelopmentRegion</key>
      <string>English</string>
      <key>CFBundleExecutable</key>
      <string>MyEditor</string>
      <key>CFBundleName</key>
      <string>MyEditor</string>
      <key>CFBundleIdentifier</key>
      <string>com.company.MyEditor</string>
      <key>CFBundleInfoDictionaryVersion</key>
      <string>6.0</string>
      <key>CFBundlePackageType</key>
      <string>APPL</string>
      <key>CFBundleSignature</key>
      <string>MyEd</string>
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
    

We will now add our custom filename extension to the property list so that MyEditor will be the default application used to open such files. Delete the highlighted lines and insert these replacement lines: 
    
    
      <key>CFBundleTypeName</key>
      <string>My Custom File</string>
      <key>LSHandlerRank</key>
      <string>Owner</string>
      <key>CFBundleTypeRole</key>
      <string>Editor</string>
      <key>LSTypeIsPackage</key>
      <false/>
      <key>CFBundleTypeIconFile</key>
      <string>txt.icns</string>
      <key>CFBundleTypeExtensions</key>
      <array>
            <string>xyy</string>
      </array>
    

Save the file. You will notice that we have changed the "role" from Viewer to Editor and we have added our custom file extension (xyy). If you only need to open files and not edit them, you should leave the "role" as viewer. We have also added an icon file "txt.icns" so that Finder will display your icon for your custom file extension. 

Now we need to add an OnDropFiles event handler to our form. Copy the following code into your unit1.pas file before the last line which contains **end.** : 
    
    
    procedure TForm1.FormDropFiles(Sender: TObject;
      const FileNames: array of string);
    begin
      memo1.Lines.LoadFromFile(FileNames[0]);
    end;
    

Go to the Object Inspector, choose Form1, select the Events tab and double-click the empty OnDropFiles field. FormDropFiles should be automatically added to the field. Now compile the application. 

As you have run MyEditor before, its original `Info.plist` file will have been cached in the Launch Service database and so the changes we made to it will not be noticed. The quickest way to fix this is to copy the MyEditor.app bundle to the Applications directory. As Lazarus only keeps a link to the executable file in the application bundle, before you copy the MyEditor application bundle, you will need to replace the file link with the executable file. Open an Applications > Utilities > Terminal and execute the following commands: 
    
    
     cd <your project directory>
     echo "Test file content" > test.xyy                // Create a test.xyy file while here
     rm MyEditor.app/Contents/MacOS/MyEditor            // remove the link to the executable from the bundle
     cp MyEditor MyEditor.app/Contents/MacOS/MyEditor   // copy the executable to the bundle 
     cp -R MyEditor.app /Applications/                  // copy the application bundle to /Applications
     rm MyEditor.app/Contents/MacOS/MyEditor            // remove the executable from the bundle
     cd MyEditor.app/Contents/MacOS/MyEditor            
     ln -s ../../../MyEditor .                          // restore the link to the executable
    

Now, from Finder, you should be able to double-click on your `test.xyy` file and receive the following dialog: 

[![myeditor dialog.png](https://wiki.freepascal.org/images/1/1d/myeditor_dialog.png)](</File:myeditor_dialog.png>)

Choose Open and MyEditor should open... and display the content of your `test.xyy` file. You can also drop the `test.xyy` file on the application's icon and it will open the file. 

## Adding your custom URL scheme

Apple supports common URL schemes associated with system applications, such as http, https, ftp, and mailto. You can define your own custom scheme and register your application to support it. For this demonstration we are going to define the custom xyy:// scheme. So, when Safari comes across xyy://some_file, it will ask you if you want to use MyEditor to open it. 

For MyEditor to support a custom URL scheme, we need to do the following: 

  1. Define the format for the scheme.
  2. Register the scheme so that macOS directs appropriate URLs to MyEditor.
  3. Handle the URLs that MyEditor receives.


  * Our format is trivial. What follows our custom URL scheme (xyy://) will just be a filename to open.


  * To register the URL scheme we return to MyEditor's `Info.plist` property list file which currently looks like this:


    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
      <key>CFBundleDevelopmentRegion</key>
      <string>English</string>
      <key>CFBundleExecutable</key>
      <string>MyEditor</string>
      <key>CFBundleName</key>
      <string>MyEditor</string>
      <key>CFBundleIdentifier</key>
      <string>com.company.MyEditor</string>
      <key>CFBundleInfoDictionaryVersion</key>
      <string>6.0</string>
      <key>CFBundlePackageType</key>
      <string>APPL</string>
      <key>CFBundleSignature</key>
      <string>MyEd</string>
      <key>CFBundleShortVersionString</key>
      <string>0.1</string>
      <key>CFBundleVersion</key>
      <string>1</string>
      <key>CSResourcesFileMapped</key>
      <true/>
      <key>CFBundleDocumentTypes</key>
      <array>
        <dict>
          <key>CFBundleTypeName</key>
          <string>My Custom File</string>
          <key>LSHandlerRank</key>
          <string>Owner</string>
          <key>CFBundleTypeRole</key>
          <string>Editor</string>
          <key>LSTypeIsPackage</key>
            <false/>
          <key>CFBundleTypeIconFile</key>
          <string>txt.icns</string>
          <key>CFBundleTypeExtensions</key>
          <array>
            <string>xyy</string>
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
    

  * After the highlighted line above, insert the following lines into the property list file:


    
    
      <key>CFBundleURLTypes</key>
      <array>
        <dict>
          <key>CFBundleTypeRole</key>
          <string>Editor</string>
          <key>CFBundleURLName</key>
          <string>com.company.URLScheme</string>
          <key>CFBundleURLSchemes</key>
          <array>
            <string>xyy</string>
          </array>
        </dict>
      </array>
    

  * The final step we need to take care of is to handle the URLs that MyEditor receives. To do this we need to add some more code to unit1.pas. The lines to add are highlighted in the listing below which is now the final version of unit1.pas:


    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec1}                                              
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls,
      Strutils, CocoaAll, MacOSAll; 
                                            
    type
      THandlerProc = procedure(const url : string);
    
      { TAppURLHandler }
    
      TAppURLHandler = objcclass(NSObject)
      public
        procedure  getUrlwithReplyEvent(event : NSAppleEventDescriptor; eventReply: NSAppleEventDescriptor);
             message 'getUrl:withReplyEvent:';
      public
        callBack : THandlerProc;
      end;
    
      { TForm1 }
    
      TForm1 = class(TForm)
        btnOpen: TButton;
        btnSave: TButton;
        btnQuit: TButton;
        Memo1: TMemo;
        OpenDialog1: TOpenDialog;
        SaveDialog1: TSaveDialog;
        procedure btnOpenClick(Sender: TObject);
        procedure btnQuitClick(Sender: TObject);
        procedure btnSaveClick(Sender: TObject);
        procedure FormCreate(Sender: TObject);
        procedure FormDropFiles(Sender: TObject; const FileNames: array of string);
      private
        sTmp : string;
        procedure LoadMemo;
        procedure SaveMemo;
      public
    
      end;
    
    var
      Form1: TForm1;
      handler      : TAppURLHandler;                                          
      eventManager : NSAppleEventManager;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    // Handle my custom URL scheme                                   
    procedure HandleMyCustomURLProtocol(const FileName: string);
    var
      FilePath : String;
    begin
      FilePath := ReplaceStr(FileName, 'xyy://', '');  // ditch protocol scheme
      Form1.memo1.Lines.LoadFromFile(FilePath);        // load file into memo
    end;
    
    procedure TAppURLHandler.getUrlwithReplyEvent(event : NSAppleEventDescriptor; eventReply: NSAppleEventDescriptor);
    var
      url : NSString;
    begin
      url := event.paramDescriptorForKeyword(keyDirectObject).stringValue;
      callBack(url.UTF8String);
    end;
    
    procedure RegisterURLHandler(HandlerProc :  THandlerProc);
    begin
      handler          := TAppURLHandler.alloc.init;
      handler.callBack := HandlerProc;
      eventManager     := NSAppleEventManager.sharedAppleEventManager;
      eventManager.setEventHandler_andSelector_forEventClass_andEventID(handler,ObjCSelector(handler.getUrlwithReplyEvent), kInternetEventClass,kAEGetURL);
    end;
    
    // Form creation handler
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      // Listen for protocol URLs
      RegisterURLHandler(@HandleMyCustomURLProtocol);
    
      // if there is a filename on the command line
      if ParamCount > 0 then
        memo1.Lines.LoadFromFile(ParamStr(1));
    end;
    
    procedure TForm1.btnOpenClick(Sender: TObject);
    begin
      Form1.LoadMemo;
    end;
    
    procedure TForm1.btnSaveClick(Sender: TObject);
    begin
      Form1.SaveMemo;
    end;
    
    procedure TForm1.btnQuitClick(Sender: TObject);
    begin
      Form1.Close;
    end;
    
    procedure TForm1.LoadMemo;
    begin
    if OpenDialog1.Execute then
      sTmp:=OpenDialog1.Filename;
    
    if FileExists(sTmp) then
      memo1.Lines.LoadFromFile(sTmp)
     else
       showmessage('File not found: ' + sTmp);
    end;
    
    procedure TForm1.SaveMemo;
    begin
    if SaveDialog1.Execute then
      begin
        try
          memo1.Lines.SaveToFile(SaveDialog1.FileName);
        except on EStreamError do
            showmessage('Could not save file: ' + SaveDialog1.FileName);
        end;
      end
    end;
    
    procedure TForm1.FormDropFiles(Sender: TObject;
      const FileNames: array of string);
    begin
      memo1.Lines.LoadFromFile(FileNames[0]);
    end;
    
    end.
    

  * When you have added the new lines, go to the Object Inspector, choose Form1, select the Events tab, go to the OnCreate event and double-click the empty field. The TForm1.FormCreate procedure should be automatically filled in for you. Now compile the application for the last time!


  * As you have run MyEditor before, the previous `Info.plist` file will have been cached in the Launch Service database and so the changes we made to it will not be noticed. To fix this, first drag the existing MyEditor.app bundle to the Trash Bin, and then copy the MyEditor.app bundle to the Applications directory again. Open an Applications > Utilities > Terminal and execute the following commands:


    
    
     cd <your project directory>
     rm MyEditor.app/Contents/MacOS/MyEditor            // remove the link to the executable from the bundle
     cp MyEditor MyEditor.app/Contents/MacOS/MyEditor   // copy the executable to the bundle 
     cp -R MyEditor.app /Applications/                  // copy the application bundle to /Applications
     rm MyEditor.app/Contents/MacOS/MyEditor            // remove the executable from the bundle
     cd MyEditor.app/Contents/MacOS/MyEditor            
     ln -s ../../../MyEditor .                          // restore the link to the executable
    

  * Now if you open Safari and type `xyy:///Users/<username>/<path to your project directory>/unit1.pas` into the URL box at the top... the following dialog should appear:



[![myeditor dialog2.png](https://wiki.freepascal.org/images/d/d4/myeditor_dialog2.png)](</File:myeditor_dialog2.png>)

  * Click Allow and... the file should load.



## Last words

As with many things Apple, the CFBundleDocumentTypes entries: 

  * CFBundleTypeExtensions
  * CFBundleTypeMIMETypes
  * CFBUndleTypeOSTypes
  * NSExportableAs



were deemed legacy `Info.plist` keys in macOS 10.5 (Leopard) and were deprecated in favour of Uniform Type Identifiers (UTIs) introduced in macOS 10.5 for document-based applications. 

Instead, you should use the LSItemContentTypes and (new for macOS 10.5) NSExportableTypes keys, and declare the corresponding UTIs as appropriate in the application `Info.plist`. For the declaration of UTIs in `Info.plist` refer to the [External Links](<macOS_File_and_Custom_URL_Scheme_Associations.md> "macOS File and Custom URL Scheme Associations") below. 

It should be noted that the legacy keys are still working in macOS 10.15.4 (Catalina). 

## See also

  * [Application Bundle](<Application_Bundle.md> "Application Bundle")
  * [macOS property list files](<macOS_property_list_files.md> "macOS property list files")



## External links

  * [Apple: Launch Services](<https://developer.apple.com/documentation/coreservices/launch_services>)
  * [Apple:Developing a document-based application](<https://developer.apple.com/documentation/appkit/documents_data_and_pasteboard/developing_a_document-based_app>)
  * [Apple: Uniform Type Identifers](<https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html>)
  * [Apple: System-declared UTIs](<https://developer.apple.com/library/archive/documentation/Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html>)
  * [Info.plist Keys and Values](<https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html>)
  * [Apple: Defining a custom URL scheme](<https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content/defining_a_custom_url_scheme_for_your_app>)

---

_Source: [https://wiki.freepascal.org/macOS_File_and_Custom_URL_Scheme_Associations](https://web.archive.org/web/20240121085309/https://wiki.freepascal.org/macOS_File_and_Custom_URL_Scheme_Associations)_
