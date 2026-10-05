# Step-by-step instructions for creating multi-language applications

│ **English (en)** │

## Contents

  * 1 Introduction
  * 2 Getting started
  * 3 GUI Design
  * 4 Enable translations
  * 5 Use LCLTranslator unit
    * 5.1 Excluding Controls
  * 6 Translating
  * 7 Switching languages
  * 8 Strings defined by the LCL and other packages
  * 9 Format settings
    * 9.1 Windows
    * 9.2 Linux
    * 9.3 macOS
  * 10 Additional information
    * 10.1 Operating system dialogs
    * 10.2 macOS
      * 10.2.1 Language designators
  * 11 Conclusion
  * 12 See also



## Introduction

Preparing applications for several languages has always been a bit of a mystery to me - until I finally tried. Then I found out that is is amazingly simple once I understood the first steps. I'd like to share this information with interested users of Lazarus. 

You may jump right in. But it may be a good idea to learn something about the basic ideas behind that architecture. Therefore I'd urge you to read the wiki articles [Localization](<Localization.md> "Localization") and/or [Translations_/_i18n_/_localizations_for_programs](<Translations_/_i18n_/_localizations_for_programs.md> "Translations / i18n / localizations for programs") which explain the basic fundamentals behind the scene. 

## Getting started

Before starting, I'd like to mention that this tutorial was initially written for Lazarus version 1.2 and now is being updated to version 3.0. There will be some notes here and here to identify the differences between the versions. 

[![imgviewer orig.png](https://wiki.freepascal.org/images/3/32/imgviewer_orig.png)](</File:imgviewer_orig.png>)

At first, we need an application that we want to translate. Looking through the example projects that come with Lazarus I found that `examples\imgviewer` may be a decent demo project. In order to keep the original, copy the entire project folder into a separate directory, e.g. `imgviewer_multilanguage`. Open the project file `imgview.lpi`, compile and run it. You'll see a typical application with a menu and some controls to show a file list and the selected image. 

Let's convert this application to support translation into German. 

## GUI Design

During design of the GUI it shall be considered, that a string will have a different length in each language. That is why all components must have the _Autosize_ property set to _True_. It is normal to expect that in some languages strings might be some 50% longer, but even 200% is not much unusual. 

The GUI dimensions must consider the display of the device on which the application will be run. For a PC if a form is not scrollable, it shall not be greater that 1024x600 pixels (10" netbook display). This means that if only single-line string components are used, they shall fit properly in a 700x600 form, so when the form is enlarged to 1024x600 it shall be sufficient for other languages. 

If several vertically positioned buttons are used and they are expected to equal width, besides _Autosize= True_ they shall have _.Constraints.MinWidth_ set to a significantly high value. 

In order to prevent components from overlapping, [anchoring](<Anchor_Sides.md> "Anchor Sides") shall be used. 

Labels are often positioned to the left of the controls they belong to. In this case, precautions have to be taken to avoid that they extend into the control if they become too long. One option would be to allow multi-line labels by setting their WordWrap property to true (and AutoSize to false). Alternatively, single-line labels could be placed above the controls. 

## Enable translations

Only one modification of the project is required to enable translation. It is found in the project options as item "i18n". Strange word, isn't it? It is the abbreviation for "internationalization" and stands for "18 letters between the i and the n". 

Put a checkmark in the checkbox `Enable i18n`. This activates the `i18n Options` underneath. Enter the name for the `PO Output Directory` (use **locale** or **languages** as directory, so later it would be found automatically). This is the folder where the files with translated texts will be stored. As you will see later, the translation files will have the extension .po ("Portable Object"). Let's use the folder `languages`. Note that this folder is relative to the folder containing the exe file. Be sure to keep this structure if you should later copy the exe to somewhere else. 

[![enable i18n.png](https://wiki.freepascal.org/images/6/61/enable_i18n.png)](</File:enable_i18n.png>)

Keep the checkbox `Create/update .po file when saving a lfm file` checked - this updates the translation "master" file whenever you save - see below. 

If you compile the project again, you'll find two changes in the project folder: At first there is a new file with the extension `.lrj` (in older versions than v1.8 the extension had been `.rst`). This file type collects the resource strings declared in a unit. **Resource strings** are the key elements of the translation system: whenever you want a string to be translated declare it as a `resourcestring`, don't hard-code it, and don't declare it as `const`. 

As an example: In order to display an error message "File does not exist." don't call the `ShowMessage` procedure like: 
    
    
    begin
      ShowMessage('File does not exist.');
    end;
    

but declare the text as a `resourcestring` and use it as a parameter for the `ShowMessage`: 
    
    
    resourcestring
      SFileDoesNotExist = 'File does not exist.';
    begin
      ShowMessage(SFileDoesNotExist);
    end;
    

In the demo project there are four resourcestring declarations at the beginning of the implementation section of the main form (`frmmain`). In a larger project, it is convenient to collect all resourcestrings in a separate unit; you can access the strings from other forms and units and avoid cross-referencing of units this way. 

The second modification of the project is a new folder "languages". It contains a file `imgview.pot`. This file was created automatically by Lazarus and contains the resourcestrings in a form ready for translation. The extension `.pot` means "Portable object template" to indicate that it is the "master template" for all languages. To create a German translation from that file create a copy and rename it to `imgview.de.po`. "de" is the language code for "German" in the translation system, accordingly you can use "en" for "English", "ru" for "Russian" etc. See for example [www.science.co.il/Language/Locale-codes.asp](<http://www.science.co.il/Language/Locale-codes.asp>) for a list of all language codes. And don't forget to change the extension which must `.po` now, rather than `.pot`. Note that the translation template had the extension `.po` until about v2.0 where the `.pot` extension was introduced. 

We could open `imgview.de.po` and add translations in a straighforward way. But there is an even simpler way as we will see later. Before doing that let's apply another modification to the demo project. 

## Use LCLTranslator unit

So far, the po files only contain resourcestrings that are explicitly declared as such. There are, however, a lot of other strings in our application which are not yet covered, like the entire menu and submenus, texts in labels or listboxes, etc. 

There is an extremely easy way to include the strings of the user interface into the translation system: Simply add the unit `LCLTranslator` to the main form's uses clause. When the project is compiled after this modification you'll find all strings in the po file. 

[![LCLTranslator.png](https://wiki.freepascal.org/images/1/1e/LCLTranslator.png)](</File:LCLTranslator.png>)

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** In Lazarus versions older than 1.4 this functionality resides in unit `DefaultTranslator.pas`.

### Excluding Controls

To exclude some controls from automatic translation, you can add the full path to the text/caption of this control (e.g. "tmainform.ecit1.text") to the "Exclude" box in project options, _i18n_ page. 

## Translating

For translation we could edit the po files directly by using a standard text editor. But it is more conventient to use a separate program optimized for this purpose. Good candidates are: 

  * [poedit](<https://poedit.net/download>) (old link [poedit](<http://sourceforge.net/projects/poedit/>))
  * [POEditor](<https://poeditor.com/>) or
  * [Better PO Editor](<http://sourceforge.net/projects/betterpoeditor/>).



I'll be using `poedit` here. 

Install this program. If not done before, copy `imgviewer.pot` and rename it to `imgviewer.de.po`. Open `imgviewer.de.po` in `poedit`. 

`poedit` shows a list of all resourcestrings: those explicitly declared, and those extracted from lcl controls by the LCLTranslator. Select a string and type its translation into the lower memo. Repeat with all texts. Save. Before saving open then menu item "Catalogue" / "Properties" and check if "Charset" is UTF-8 - poedit sometimes forgets about this correct setting. 

[![poedit.png](https://wiki.freepascal.org/images/0/04/poedit.png)](</File:poedit.png>)

In the same way, you can add more languages: copy the template file and change the file name of the copy to contain the language code and the .po extension, open this po file in `poedit`, add the translations, and save it with the corresponding language code before the po extension. 

Since the test application is hard-coded in English it is very easy to create an English translation file: after coyping `imgview.pot` to `imgview.en.po` and loading that into `poedit`, select each string item and press `Ctrl`+`B` which copys the raw resourcestring value into the translation memo. Save as `imgview.en.po`. 

In order to force the application to open the German translation at start-up add the line `SetDefaultLang('de')` to the initialization code of the main form unit. If you have several translation and want the application to open in the language of the user's system use an empty string the SetDefaultLang call, or add unit `DefaultTranslator` to the form, in addition to `LCLTranslator`. This way the default language is detected, and the resource strings are replaced by those from the corresponding po file automatically. 

More about `SetDefaultLang` in the next section... 

## Switching languages

But what if your PC is not on German language? We can use the Lazarus translation system to switch languages at run-time. 

First of all, the `LCLTranslator` unit gives access to commandline switches `--lang` or `-l` to override the automatic language detection. For example, 
    
    
        imgview.exe --lang de
    

opens the German translation of the program even on an English-speaking system. 

Of course, changing languages at run-time would be even more favorable. While this was not possible with Lazarus before version 1.2 out of the box, it is no problem with current versions of Lazarus where `LCLTranslator` provides a procedure `SetDefaultLang` for switching between languages upon user request. (In Laz v1.2.x you must "use" unit `DefaultTranslator` rather than `LCLTranslator`.) 

At first, we need some control in the user interface to switch languages. What about a new menu item "Translation" and a submenu containing the available languages? In the menu designer of the main form add a new item "Translations" and an empty submenu. Then, for each language available, add the name of the corresponding language to the submenu. Of course you can also show a flag icon for each language - free flag icons are available from [[1]](<http://www.famfamfam.com/lab/icons/flags/>). In the `OnClick` event handler call `SetDefaultLang` with the language code as a parameter, e.g. 
    
    
    procedure TMainForm.MEnglishlanguageClick(Sender: TObject);
    begin
      SetDefaultLang('en');
    end;  
    
    procedure TMainForm.MGermanLanguageClick(Sender: TObject);
    begin
      SetDefaultLang('de');
    end;
    

Of course, you can use more sophisticated code which determines the language codes from the names of the po files found and creates menu items depending on the available translations. 

Now you can click on a language menu item, and the application language switches to the selected language automatically! 

As you may notice the new menu item "Translation" and its submenu items are not translated. This is because these are new strings missing from the translated po files. Simply open the translated files in `poedit` and add the translations of the new strings. All the previous translations are still there. Save, and you are all set. 

## Strings defined by the LCL and other packages

Here is one idea for refinement: The strings set up up for error messages defined by the LCL or used in standard dialogs or message boxes are not yet translated. Their translation is extremely easy: their translation files can be found in the directory `lcl/languages` of your Lazarus installation; they are named `lclstrconsts.*.po`. Pick those translations used by your application and copy them to the `languages` folder of our project. (And add unit _lclstrconsts_ to the `uses` clause if you refer to one of these strings). 

Do the same when your application requires third-party packages which may have their own set of po files - always copy the po files of all packages needed into the `languages` folder of your project, and mention these resourcestring units in the uses clause. 

This works because resourcestrings are pulled in from all language files for the active language found in the languages folder. 

## Format settings

When creating multi-language applications translation of the strings is not the only task. Another issue is that format settings may change from country to country. Format settings - they define formatting of dates and times, the month and day names, the character to be used as decimal or thousands separator, etc. In Lazarus, the record `TFormatSettings` collects all the possible data. The `DefaultSettings` are used by the standard format conversion routines, such as StrToDate, DateToStr, StrToFloat, or FloatToStr, etc. 

### Windows

For Windows, there exists a procedure `GetLocaleFormatSettings` in the unit `sysutils` which returns the `TFormatSettings` for a given localization. The parameter `LCID` specifies the language code in Windows. Unfortunately I do not know of a convenient conversion between the LCID and the language codes ('de', 'en', etc) used by Lazarus. A conversion table for lookup can be found at [Language Codes](<Language_Codes.md> "Language Codes"). In this table, you see that "German" has the LCID $407, and "English" has the LCID $409. Therefore, we modify the `OnClick` event handler for the language selection menu items as follows: 
    
    
    procedure TMainForm.MEnglishlanguageClick(Sender: TObject);
    begin
      SetDefaultLang('en');
      GetLocaleFormatSettings($409, DefaultFormatSettings);
    end;
    
    procedure TMainForm.MGermanLanguageClick(Sender: TObject);
    begin
      SetDefaultLang('de');
      GetLocaleFormatSettings($407, DefaultFormatSettings);
    end;
    

As a demonstration we add a status bar to our project and display the date when an image was created: 
    
    
    procedure TMainForm.ShowPicDateTime;
    var
      dt: TDateTime;
    begin
      if LBFiles.ItemIndex = -1 then
        Statusbar.SimpleText := ''
      else begin
        dt := FileDateToDateTime(FileAge(LBFiles.Items[LBFiles.ItemIndex]));
        Statusbar.SimpleText := DateToStr(dt) + ' ' + TimeToStr(dt);
      end;
    end;
    

This procedure is called from the `OnSelectionChange` event handler of the files listbox: 
    
    
    procedure TMainForm.LBFilesSelectionChange(Sender: TObject; User: boolean);
    begin
      ShowPicDateTime;
    end;
    

Recompile the program and load some images. You'll see that the date format changes when the language is switched and another image is selected. 

### Linux

I don't know how this issue could be handled in general in Linux. Maybe somebody does out there in the community? 

### macOS

Refer to the [Locale settings for macOS](<Locale_settings_for_macOS.md> "Locale settings for macOS") article. 

## Additional information

### Operating system dialogs

The operating system dialogs (eg [TOpenDialog](<TOpenDialog.md> "TOpenDialog")) are not translatable by Lazarus because they use the operating system strings and not the LCL widgetset strings which can be localised as detailed above. 

### macOS

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** You may need to delete the symbolic link to the executable created by Lazarus in the [application bundle](<Application_Bundle.md> "Application Bundle") and replace it with the actual executable before the localised language will show up in the system dialogs when using the methods described below.

For macOS, one solution is to add the language designators of the supported languages to the application's `Info.plist` file like this: 
    
    
            <key>CFBundleLocalizations</key>
            <array>
                    <string>en</string>
                    <string>fr</string>
                    <string>de</string>
                    <string>it</string>
                    <string>nl</string>
                    <string>ru</string>
                    ... etc ...
            </array>
    

and then the operating system dialogs will present users with their preferred language (**fr** \- French example below). 

[![french dialog localization.png](https://wiki.freepascal.org/images/9/98/french_dialog_localization.png)](</File:french_dialog_localization.png>)

An alternative method for localising a macOS application is to instead create an empty folder for each language in the application bundle's `Resources` directory. For example, for French, add an empty file named `fr.lproj` in `MyApp.app/Contents/Resources`. 

#### Language designators

A language designator is a code that represents a language. Use the two-letter ISO 639-1 standard (preferred) or the three-letter ISO 639-2 standard. If an ISO 639-1 code is not available for a particular language, use the ISO 639-2 code instead. For example, there is no ISO 639-1 code for the Hawaiian language, so use the ISO 639-2 code. For a complete list of ISO 639-1 and ISO 639-2 codes, see [ISO 639.2 Codes for the Representation of Names and Languages](<http://www.loc.gov/standards/iso639-2/php/English_list.php>). 

## Conclusion

Although some issues of localization (e.g. right-to-left mode, sorting order) have not been discussed I hope that this tutorial covered the most important aspects and is at least a good starting point for beginners. 

In the end, here is the final screenshot of this tutorial: it shows the image viewer application translated to German. 

[![imgviewer de.png](https://wiki.freepascal.org/images/f/fa/imgviewer_de.png)](</File:imgviewer_de.png>). 

## See also

  * [Localization](<Localization.md> "Localization")
  * [Getting translation strings right](<Getting_translation_strings_right.md> "Getting translation strings right")
  * [Translations / i18n / localizations for programs](<Translations_/_i18n_/_localizations_for_programs.md> "Translations / i18n / localizations for programs")
  * [Common macOS user interface captions](<macOS_Translation.md> "macOS Translation")

---

_Source: [https://wiki.freepascal.org/Step-by-step_instructions_for_creating_multi-language_applications](https://web.archive.org/web/20221112125225/https://wiki.freepascal.org/Step-by-step_instructions_for_creating_multi-language_applications)_
