# Mac Preferences Read and Write

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │    
****

## Contents

  * 1 macOS File Storage Overview
  * 2 CFPreferences
  * 3 NSUserDefaults
  * 4 See also
  * 5 External links



## macOS File Storage Overview

Before we go any further, let's recap where the Apple Guidelines indicate your application should store its files: 

  * Use the `/Applications` or `/Applications/Utilities` directory for the [application bundle](<Application_Bundle.md> "Application Bundle"). The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities directory`, it should be able to run. Installing to these folders requires Admin privileges. The data in these folders is backed up by Time Machine.


  * Use the `~/Applications` directory should Admin privileges not be available. This is the standard location for a single user application. This directory should not be expected to exist. The application bundle should contain everything: libraries, dependencies, help, every file that the application needs to run except those created by the application itself. If the application bundle is copied to another machine's `/Applications` or `/Applications/Utilities` directory, it should be able to run. This data is backed up by Time Machine.


  * Use the [Application Support directory](<Locating_macOS_app_support,_preferences_folders.md> "Locating macOS app support, preferences folders") (this data is backed up by Time Machine), appending your <bundle_ID>, for: 
    * Resource and data files that your application creates and manages for the user. You might use this directory to store application state information, computed or downloaded data, or even user created data that you manage on behalf of the user.
    * Autosave files.


  * Use the [Caches directory](<Locating_macOS_significant_directories.md> "Locating macOS significant directories") (this is **not** backed up by Time Machine), appending your <bundle_ID>, for cached data files or any files that your application can recreate easily.


  * Use CFPreferences to read and write your application's preferences. This will automatically write preferences to the appropriate location and read them from the appropriate location. This data is backed up by Time Machine.


  * Use the [application Resources directory](<Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory") (this is backed up by Time Machine) for your application-supplied image files, sound files, icon files and other unchanging data files necessary for your application's operation.


  * Use [NSTemporaryDirectory](<Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory") (this is **not** backed up by Time Machine) to store temporary files that you intend to use immediately for some ongoing operation but then plan to discard later. Delete temporary files as soon as you are done with them.



## CFPreferences

For those who want to read and write preferences for a macOS application, this can be done using the following methods: 

CODE FOR READING PREFERENCES   
---  
      
    
      uses MacOSAll, CFPreferences;
    
      var
        IsValid: Boolean;  // On return indicates if key exists and has valid data
        Pref: Integer;
    
      procedure TForm1.FormCreate(Sender: TObject);
      begin
        try
           Pref := CFPreferencesGetAppIntegerValue(CFStr('Check1'),kCFPreferencesCurrentApplication,IsValid);
           if (Pref = 1) then
              CheckBox1.Checked := true
           else
              CheckBox1.Checked := false;
    
           Pref := CFPreferencesGetAppIntegerValue(CFStr('Check2'),kCFPreferencesCurrentApplication,IsValid);
           if (Pref = 1) then
              CheckBox2.Checked := true
           else
              CheckBox2.Checked := false;
    
           Pref := CFPreferencesGetAppIntegerValue(CFStr('Check3'),kCFPreferencesCurrentApplication,IsValid);
           if (Pref = 1) then
              CheckBox3.Checked := true
           else
              CheckBox3.Checked := false;
    
           Pref := CFPreferencesGetAppIntegerValue(CFStr('Check4'),kCFPreferencesCurrentApplication,IsValid);
           if (Pref = 1) then
              CheckBox4.Checked := true
           else
              CheckBox4.Checked := false;
    
        except
          on E : Exception do
            ShowMessage(E.ClassName+' error raised, with message : '+E.Message);
        end;
      end;
    

  


CODE FOR WRITING PREFERENCES   
---  
      
    
      uses MacOSAll, CFPreferences;
    
      var
        ItemName: CFStringRef;
        ItemVal: CFPropertyListRef;
    
      procedure TForm1.FormClose(Sender: TObject);
      begin
        try
           if (CheckBox1.Checked) then
             begin
               ItemName := CFStr('Check1');
               ItemVal := CFStringCreateWithPascalString(kCFAllocatorDefault,'1',kCFStringEncodingUTF8);
               CFPreferencesSetAppValue(ItemName,ItemVal,kCFPreferencesCurrentApplication);
             end
           else
             begin
               ItemName := CFStr('Check1');
               ItemVal := CFStringCreateWithPascalString(kCFAllocatorDefault,'0',kCFStringEncodingUTF8);
               CFPreferencesSetAppValue(ItemName,ItemVal,kCFPreferencesCurrentApplication);
             end;
    
           if (CheckBox2.Checked) then
             begin
               ItemName := CFStr('Check2');
               ItemVal := CFStringCreateWithPascalString(kCFAllocatorDefault,'1',kCFStringEncodingUTF8);
               CFPreferencesSetAppValue(ItemName,ItemVal,kCFPreferencesCurrentApplication);
             end
           else 
             begin
               ItemName := CFStr('Check2');
               ItemVal := CFStringCreateWithPascalString(kCFAllocatorDefault,'0',kCFStringEncodingUTF8);
               CFPreferencesSetAppValue(ItemName,ItemVal,kCFPreferencesCurrentApplication);
             end;
           
           if (CheckBox3.Checked) then
             begin
               ItemName := CFStr('Check3');
               ItemVal := CFStringCreateWithPascalString(kCFAllocatorDefault,'1',kCFStringEncodingUTF8);
               CFPreferencesSetAppValue(ItemName,ItemVal,kCFPreferencesCurrentApplication);
             end
           else
             begin
               ItemName := CFStr('Check3');
               ItemVal := CFStringCreateWithPascalString(kCFAllocatorDefault,'0',kCFStringEncodingUTF8);
               CFPreferencesSetAppValue(ItemName,ItemVal,kCFPreferencesCurrentApplication);
             end;
    
           if (CheckBox4.Checked) then
             begin
               ItemName := CFStr('Check4');
               ItemVal := CFStringCreateWithPascalString(kCFAllocatorDefault,'1',kCFStringEncodingUTF8);
               CFPreferencesSetAppValue(ItemName,ItemVal,kCFPreferencesCurrentApplication);
             end
           else
             begin
               ItemName := CFStr('Check4');
               ItemVal := CFStringCreateWithPascalString(kCFAllocatorDefault,'0',kCFStringEncodingUTF8);
               CFPreferencesSetAppValue(ItemName,ItemVal,kCFPreferencesCurrentApplication);
             end;
    
           // write out the preference data
           CFPreferencesAppSynchronize(kCFPreferencesCurrentApplication);
    
         except
           on E : Exception do
             ShowMessage(E.ClassName+' error raised, with message : '+E.Message);
         end;
    

[![Prefs1.png](https://wiki.freepascal.org/images/6/61/Prefs1.png)](</File:Prefs1.png>)

## NSUserDefaults

The NSUserDefaults class, available since Mac OS X 10.0 (Cheetah), provides a programmatic interface for interacting with the defaults system which allows an application to customise its behaviour to match a user’s preferences. 

Unfortunately there were serious problems with NSUserdefaults in iOS 9 and macOS 10.12 (Sierra) and 10.13 (High Sierra) which mitigate against its use. Some of the issues are mentioned in this [Apple Developer Forum thread](<https://developer.apple.com/forums/thread/88811>). 

## See also

  * [macOS Form Position Save and Restore](<macOS_Form_Position_Save_and_Restore.md> "macOS Form Position Save and Restore") \- Automatic save and restore using just one line of code!
  * [Introduction to platform-sensitive development](<Introduction_to_platform-sensitive_development.md> "Introduction to platform-sensitive development")
  * [Proper macOS file locations](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips")



## External links

  * [Apple: Managing Preferences Using Core Foundation](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/UserDefaults/AccessingPreferenceValues/AccessingPreferenceValues.html#//apple_ref/doc/uid/10000059i-CH3-108766>)
  * [Apple: NSUserDefaults](<https://developer.apple.com/documentation/foundation/nsuserdefaults>)
  * [Apple: Preferences](<https://developer.apple.com/documentation/foundation/preferences>)
  * [Apple: Preference Utilities](<https://developer.apple.com/documentation/corefoundation/preferences_utilities>)
  * [Apple: Preferences and Settings Programming Guide](<https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html>)
  * [Apple: Human Interface Guidelines - Preferences](<https://developer.apple.com/design/human-interface-guidelines/macos/app-architecture/preferences/>)

---

_Source: [https://wiki.freepascal.org/Mac_Preferences_Read_and_Write](https://web.archive.org/web/20240417153911/https://wiki.freepascal.org/Mac_Preferences_Read_and_Write)_
