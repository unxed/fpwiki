# Apple-specific UI elements

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 Apple main menu item
  * 2 About Box
  * 3 Fixing your application name
  * 4 How to make Apple main menu multilingual
  * 5 Adding an icon to your app bundle
  * 6 Removing the app icon from title bars
  * 7 Removing the title bar and border from a form
  * 8 Add an Apple help book to your app
  * 9 Using preferences in your application
  * 10 Menu bar
  * 11 Mac Buttons
  * 12 The Dock
    * 12.1 Hiding an application from the Dock
    * 12.2 Show Badge on application icon in Dock
    * 12.3 Bouncing an application icon in the Dock
    * 12.4 Adding menu items to an application Dock Menu
  * 13 Dialogs / Windows
    * 13.1 How to use a Drawer
    * 13.2 How to use a Sheet
    * 13.3 How to use Alert Panels
    * 13.4 About windows / About Boxes
  * 14 How to deliver user notifications
  * 15 Detecting Light or Dark mode
  * 16 See also
  * 17 External links



## Apple main menu item

In each Mac application there is a main menu item with the name of the application. Lazarus will automatically add the "Services", "Hide" and "Quit" menu items to this menu. In order to add more items to this menu, create a menu item with the apple character as its Caption "" (without quotes, it is Unicode 0xF8FF). You can use ifdefs {$IFDEF DARWIN} to hide it in non-macOS. 

For example: 
    
    
      {$IFDEF DARWIN}
      AppMenu := TMenuItem.Create(Self);  {Application menu}
      AppMenu.Caption := #$EF#$A3#$BF;  {Unicode Apple logo char}
      MainMenu.Items.Insert(0, AppMenu);
    
      AppAboutCmd := TMenuItem.Create(Self);
      AppAboutCmd.Action:= ActionList1.Actions[1];
      AppMenu.Add(AppAboutCmd);  {Add About as item in application menu}
    
      AppSep1Cmd := TMenuItem.Create(Self);
      AppSep1Cmd.Caption := '-';
      AppMenu.Add(AppSep1Cmd);
    
      AppCheckCmd := TMenuItem.Create(Self);
      AppCheckCmd.Action:= ActionList1.Actions[2];
      AppMenu.Add(AppCheckCmd);  {Add Check as item in application menu}
    
      AppSep2Cmd := TMenuItem.Create(Self);
      AppSep2Cmd.Caption := '-';
      AppMenu.Add(AppSep2Cmd);
      {$ENDIF}
    

## About Box

See the [Simple Way](<Mac_Show_Application_Title,_Version,_and_Company.md> "Mac Show Application Title, Version, and Company") to have the system automatically create a Cocoa Framework About Box for you that is appropriately decorated with just a single line of code, a single unit added to your Uses clause and an HTML (credits) file. 

## Fixing your application name

By default your application's name will be the name of your project. You can rename your project, but you cannot add a space to the project name. So, "My App" will show up in the AppName/Apple Menu as "MyApp". Your application dock icon will also display "MyApp". Fear not, this can be fixed by editing the application Info.plist file and changing the Bundle Name key value from "MyApp" to "My App". For example: 
    
    
      <key>CFBundleName</key>
      <string>My App</string>
    

The two keys to consider when renaming your application: 

Key name | Description   
---|---  
CFBundleName (Bundle name) | The short name for the bundle. The value for this key is usually the name of your application. Xcode sets the value of this key by default when you create a new project.   
CFBundleDisplayName (Bundle display name) | The localized version of your application name. You typically include a localized value for this key in an InfoPlist.strings file in each of your language-specific resource directories.   
  
## How to make Apple main menu multilingual

To translate the Apple main menu from the default English to, for example, German, see the procedure below: 
    
    
        unit...
        {$ifdef LCLCocoa}   {$modeswitch objectivec2}   {$endif}
         
        uses ...
        {$ifdef LCLCocoa}   CocoaAll,  {$endif}
         
        procedure TranslateApplicationMenu;
        var mnMain: NSMenu;
            mnApp : NSMenu;
            mnItem: NSMenuItem;
            i     : integer;
        begin
           mnMain := NSApplication (NSApp).mainmenu;
           mnItem := NSMenuItem (mnMain.itemArray.objectAtIndex(0));
    
           if (mnItem.hasSubmenu) then 
             begin
               mnApp := mnItem.submenu;
               
               for i:=0 to mnApp.itemArray.count-1 do 
                 begin
                   mnItem := NSMenuItem(mnApp.itemArray.objectAtIndex(i));
    
                   if (NOT mnItem.isSeparatorItem) then 
                     begin
                       if (mnItem.title.UTF8String = 'Services') then
                         mnItem.setTitle (NSString.stringWithUTF8string('Dienste'));
                       if (mnItem.title.UTF8String = 'Hide Others') then
                         mnItem.setTitle (NSString.stringWithUTF8string('Andere ausblenden'));
                       if (mnItem.title.UTF8String = 'Show All') then
                         mnItem.setTitle (NSString.stringWithUTF8string('Alle einblenden'));
                       if (mnItem.title.UTF8String = 'Hide ' + Application.Title) then
                         mnItem.setTitle (NSString.stringWithUTF8string(PChar(Application.Title + ' ausblenden')));
                       if (mnItem.title.UTF8String = 'Quit ' + Application.Title) then
                         mnItem.setTitle (NSString.stringWithUTF8string(PChar(Application.Title + ' beenden')));
                     end
                 end
             end
        end;
    

## Adding an icon to your app bundle

1\. Use /usr/bin/iconutil or a similar program to create your app's icon file (.icns extension). 

2\. Copy the .icns file to your app bundle's Resources folder. For example, from the command line: 
    
    
    cp -p myapp.icns myapp.app/Contents/Resources
    

3\. Add the following key to your app bundle's Info.plist file: 
    
    
    <key>CFBundleIconFile</key>
    <string>myapp.icns</string>
    

Sometimes Finder doesn't "refresh" an app to use the new icon, continuing instead to show the default icon. You can try these things to force Finder to use the new icon: 

  * In a Terminal, use the UNIX command "touch" to touch the app bundle (eg touch MyApp.app) - this should work instantly.
  * Log out and back in again.
  * Drag (move) your application to a different folder, then back again.
  * Restart Finder (Esc-Option-Apple keys).



You shouldn't have this problem with Finder when you install your app on other computers. The issue is a result of Finder caching the icon. 

## Removing the app icon from title bars

The Apple convention is that a window title bar has an icon only when the window represents a file. However, in the LCL the icon typically defines the application itself. 

The expected Apple behaviour can be achieved by setting the global variable **CocoaIconUse** to true and including the **CocoaInt** unit. See [Cocoa Form Icons](<Cocoa_Internals/Forms.md> "Cocoa Internals/Forms") for the full details. 

See also the Apple [_Human Interface Guidelines_](<https://developer.apple.com/design/human-interface-guidelines/macos/windows-and-views/window-anatomy/>) document on window anatomy. 

## Removing the title bar and border from a form

To remove the title bar and border from a form, use the following code in your Form OnCreate method: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      BorderIcons := [];
      BorderStyle := bsNone;
      FormStyle := fsStayOnTop;
      ShowInTaskBar := stNever;
      ControlStyle := ControlStyle - [csSetCaption];
    end;
    

## Add an Apple help book to your app

No professional macOS application should be without a functioning Apple Help Book. Unfortunately Apple make it harder than it should be for developers to create an Apple Help Book because their documentation is significantly out of date (2013 anyone?) and the Apple Help Viewer has undergone significant change in recent versions of the operating system. For the full details on how to add an Apple Help Book to your application, along with sample code, refer to the article [Add an Apple Help Book to your macOS app](<Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app"). 

## Using preferences in your application

For details on how to use Preferences in your application, refer to the articles: [Reading and Writing Preferences](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write") and [Preferences Menu](<Mac_Preferences_and_About_Menu.md> "Mac Preferences and About Menu"). 

## Menu bar

Only one status bar, the system status bar, is available in macOS. It resides in the system-wide menu bar at the top of the screen. Status items appear on the right side of the menu bar, just to the left of the menu bar clock and Menu Extras, such as the Displays and Sound menus. The items remain in the menu bar even when your application is not in the foreground. 

For details of adding your own status items to the menu bar, refer to the [macOS NSStatusBar](<macOS_NSStatusBar.md> "macOS NSStatusBar") article. 

## Mac Buttons

By default, the button placed onto a form by Lazarus is 75 (width) by 25 (height), which is not the standard for Mac Applications. See [Mac Buttons](<Mac_Buttons.md> "Mac Buttons") for how to create the expected Mac button appearance. 

## The Dock

The Dock is a prominent feature of the macOS GUI which is used to launch applications and to switch between running applications. A user's frequently applications, or indeed any file or folder, may also be placed in the Dock for quick access. Applications that do not normally keep icons in the Dock will still appear there when running and remain until they are quit. By default, the Dock appears at the bottom of the screen, but the user can optionally locate it on the left or right side of the screen. 

Apple applied for a US patent for the Dock design in 1999 and the patent was ultimately granted in 2008. 

### Hiding an application from the Dock

For information on how to hide your application from the Dock, refer to the article [Hiding a macOS app from the Dock](<Hiding_a_macOS_app_from_the_Dock.md> "Hiding a macOS app from the Dock"). 

### Show Badge on application icon in Dock

Badges are used to notify a user of something new in an application. The badge is a counter that appears on the application's dock tile (icon) as a white number on a red circular or oval, depending on the number of digits, background. For how to implement this, see the article [Show Badge on Application Icon in Dock](<Show_Badge_on_Application_Icon_in_Dock.md> "Show Badge on Application Icon in Dock"). 

### Bouncing an application icon in the Dock

Bouncing your application's icon in the dock can be useful to attract a user's attention when, for example, a long-running task has finished. For how to do this, refer to the [Bounce Application Icon in Dock](<Bounce_Application_Icon_in_Dock.md> "Bounce Application Icon in Dock") article. 

### Adding menu items to an application Dock Menu

By adding menu items to your application's dock icon, you can provide the user with quick access to commonly used features. For how to do this, referent to the [macOS Application Dock Menu](<macOS_Application_Dock_Menu.md> "macOS Application Dock Menu") article. 

## Dialogs / Windows

### How to use a Drawer

A drawer is a child window that slides out from a parent window and that the user can open or close (show or hide) while the parent window is open. A drawer should contain frequently accessed controls that do not need to be visible at all times. For how to do this, refer to the [macOS Drawers](<macOS_Drawers.md> "macOS Drawers") article. 

### How to use a Sheet

A sheet is the Apple recommended way to request user input before proceeding with a document-specific action or to present information to the user before the next step. For information on how to create and use a sheet, see: [Using macOS Sheets](<Using_macOS_Sheets.md> "Using macOS Sheets"). 

### How to use Alert Panels

A panel is a special kind of window, typically serving an auxiliary function in an application. An alert panel is a type of built-in dialog for alerting the user to something and retrieving any choice the user made (eg clicking an OK or Cancel button). For information on how to create and use an alert panel in your application, see the article [Using macOS Alert Panels](<Using_macOS_Alert_Panels.md> "Using macOS Alert Panels"). 

### About windows / About Boxes

An About window, also called an About Box, is an optional window that contains your application’s version and copyright information and may also optionally include credits acknowledging people and code you did not write. For several different ways to implement About windows. see the [About windows](<Mac_Show_Application_Title,_Version,_and_Company.md> "Mac Show Application Title, Version, and Company") article. 

## How to deliver user notifications

Notifications are a great way for your applications to keep your users informed. They appear in the upper-right corner of the screen and/or in the Notification Center. To use this feature in your application, see the article [macOS User Notifications](<macOS_User_Notifications.md> "macOS User Notifications"). 

## Detecting Light or Dark mode

Dark mode was introduced with macOS Mojave (10.14). Macs using older versions of macOS like High Sierra and Sierra are unfortunately not able to use dark mode but can enable a dark menu bar and dock. Users can enable or disable dark mode via System Preferences. An Auto mode was introduced with macOS Catalina (10.15). This mode switches between light (during the day) and dark (during the night) modes automatically. 

For details of how to detect the mode, see [Dark mode](<Dark_theme.md> "Dark theme"). 

## See also

  * [Lazarus/FPC macOS Portal](<Portal_Mac.md> "Portal:Mac").
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips").
  * [macOS Big Sur changes for developers](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers").
  * [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide").
  * [Introduction to platform-sensitive development](<Introduction_to_platform-sensitive_development.md> "Introduction to platform-sensitive development").



## External links

  * Apple [_Human Interface Guidelines_](<https://developer.apple.com/design/human-interface-guidelines/macos/>)

---

_Source: [https://wiki.freepascal.org/Apple-specific_UI_elements](https://web.archive.org/web/20240907073522/https://wiki.freepascal.org/Apple-specific_UI_elements)_
