# Mac Preferences and About Menu

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/Mac_Preferences_and_About_Menu.md>)** │

There are certain items that should appear in the default application menu on the macOS menu bar of your application. While Lazarus does automatically add: 

  * Services
  * Hide MyApp
  * Hide Others
  * Show All
  * Quit MyApp



Lazarus does not add an "About MyApp" menu item or a "Preferences..." menu item. To add these two items on the same application menu as Quit, you have to perform a little trick. 

Add a MainMenu component from the Standard tab of the component palette to your main form. Create a menu item and change its caption property in the Object Inspector to the name of your application just for reference. Then, under that menu add three sub-menu items with the following captions: About MyApp, "-" (a separator) and "Preferences..." 

[![MenuItems3new.jpg](https://wiki.freepascal.org/images/3/32/MenuItems3new.jpg)](</File:MenuItems3new.jpg>)

Assign the "Preferences..." Menu Item Shortcut "Command ," using the ShortCut Editor (choose the Menu item ShortCut property in the Object Inspector and click on the ellipsis dots next to it). After checking the Meta key, change the Unknown key to a comma, this is how your shortcut will appear: 

[![MenuItemsGrab.png](https://wiki.freepascal.org/images/5/50/MenuItemsGrab.png)](</File:MenuItemsGrab.png>)

Finally, to get these items on the MyApp application menu, we must change the caption of the first menu item in the main form's OnCreate event handler as follows: 
    
    
         procedure TForm1.FormCreate(Sender: TObject);
         var
              AppMenu : TMenuItem;
         begin
              AppMenu := TMenuItem.Create(Self); // Application menu
              AppMenu.Caption := #$EF#$A3#$BF;   // Unicode Apple logo char
              MainMenu.Items.Insert(0, AppMenu);
         end;
    

After recompiling your application and running it, you will notice that "About MyApp" and "Preferences..." have been added to the MyApp menu, which is where they should be. 

[![MenuItems4new.png](https://wiki.freepascal.org/images/0/05/MenuItems4new.png)](</File:MenuItems4new.png>)

Did you notice the problem? Do you really want to call your application "MyApp" and not "My App"? 

No, you cannot change the project file "program MyApp" to "program My App" or you are told "MyApp.lpr(1,11) Fatal: Syntax error, ";" expected but "identifier APP" found". 

The solution is to change your application Info.plist file by opening your application bundle, navigating to Contents, double-clicking on Info.plist and using the Xcode plist editor to change the value of the "Bundle name" key to the string "My App" and saving it. Now when you run "MyApp" it displays as "My App". 

Of course you would also now need to change the "About MyApp" caption too :-) 

Finished? No. The Lazarus-generated menu items still show "Hide MyApp" and "Quit MyApp". To change these to "My App" you need to modify the main form's OnCreate event handler as below: 
    
    
         procedure TForm1.FormCreate(Sender: TObject);
         var
              AppMenu : TMenuItem;
         begin
              Application.Title := 'My App';
              AppMenu := TMenuItem.Create(Self); // Application menu
              AppMenu.Caption := #$EF#$A3#$BF;   // Unicode Apple logo char
              MainMenu.Items.Insert(0, AppMenu);
         end;
    

And here is the final result: 

[![macOSMenuFinal.png](https://wiki.freepascal.org/images/1/18/macOSMenuFinal.png)](</File:macOSMenuFinal.png>)

To connect the menu options to an action, add a TActionList to the main form, add actions for About and Preferences (edit the properties and then create the appropriate OnExecute event handlers for each action), and then modify the main form's OnCreate event handler as below: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
         var
              AppMenu     : TMenuItem;
              AppAboutCmd : TMenuItem;
              AppSepCmd   : TMenuItem;
              AppPrefCmd  : TMenuItem;
         begin
              Application.Title := 'My App';
    
              AppMenu := TMenuItem.Create(Self); // Application menu
              AppMenu.Caption := #$EF#$A3#$BF;   // Unicode Apple logo char
              MainMenu.Items.Insert(0, AppMenu);
    
              AppAboutCmd := TMenuItem.Create(Self);
              AppAboutCmd.Action:= ActionList1.Actions[1];
              AppMenu.Add(AppAboutCmd);  // Add About as item in application menu
    
              AppSepCmd := TMenuItem.Create(Self);
              AppSepCmd.Caption := '-';
              AppMenu.Add(AppSepCmd);   // Add menu separator
    
              AppPrefCmd := TMenuItem.Create(Self);
              AppPrefCmd.Action:= ActionList1.Actions[2];
              AppMenu.Add(AppPrefCmd);   // Add Preferences as item in application menu
    
              AppSepCmd := TMenuItem.Create(Self);
              AppSepCmd.Caption := '-';
              AppMenu.Add(AppSepCmd);   // Add menu separator
         end;
    

  


## See also

  * [Mac About Box with one line of code](<Mac_Show_Application_Title,_Version,_and_Company.md> "Mac Show Application Title, Version, and Company")
  * [Introduction to platform-sensitive development](<Introduction_to_platform-sensitive_development.md> "Introduction to platform-sensitive development")
  * [Mac Preferences Read and Write](<Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write")
  * [Mac Show Application Title, Version, and Company](<Mac_Show_Application_Title,_Version,_and_Company.md> "Mac Show Application Title, Version, and Company")
  * [Add an Apple Help Book to your macOS app](<Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app")
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips")

---

_Source: [https://wiki.freepascal.org/Mac_Preferences_and_About_Menu](https://web.archive.org/web/20230327041135/https://wiki.freepascal.org/Mac_Preferences_and_About_Menu)_
