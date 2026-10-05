# Translations / i18n / localizations for programs

│ **[Deutsch (de)](</Translations_/_i18n_/_localizations_for_programs/de> "Translations / i18n / localizations for programs/de")** │  **English (en)** │  **[español (es)](</Translations_/_i18n_/_localizations_for_programs/es> "Translations / i18n / localizations for programs/es")** │  **[français (fr)](</Translations_/_i18n_/_localizations_for_programs/fr> "Translations / i18n / localizations for programs/fr")** │  **[日本語 (ja)](</Translations_/_i18n_/_localizations_for_programs/ja> "Translations / i18n / localizations for programs/ja")** │  **[한국어 (ko)](</Translations_/_i18n_/_localizations_for_programs/ko> "Translations / i18n / localizations for programs/ko")** │  **[polski (pl)](</Translations_/_i18n_/_localizations_for_programs/pl> "Translations / i18n / localizations for programs/pl")** │  **[português (pt)](</Translations_/_i18n_/_localizations_for_programs/pt> "Translations / i18n / localizations for programs/pt")** │  **[русский (ru)](<../../../ru/Translations_/_i18n_/_localizations_for_programs.md> "Translations / i18n / localizations for programs/ru")** │  **[中文（中国大陆） (zh_CN)](</Translations_/_i18n_/_localizations_for_programs/zh_CN> "Translations / i18n / localizations for programs/zh CN")** │    
****

This is about how a program can use different strings for various languages like English, Chinese, German, Finnish, Italian and any other language. 

  * **i18n** is short for **internationalization** (18 being the number of characters between i and n of the word internationalization)
  * **l10n** is short for **localization** (10 being the number of characters between l and n of the word localization)



  


## Contents

  * 1 Quick i18n
    * 1.1 poedit
    * 1.2 Translating Forms
    * 1.3 Translating Resourcestrings
    * 1.4 Compiling into .po files
    * 1.5 Converting .po files to .mo files
    * 1.6 Automatic translation
    * 1.7 Testing translations
    * 1.8 Final steps
      * 1.8.1 Translate LCL
      * 1.8.2 Format Settings
      * 1.8.3 BiDiMode
    * 1.9 Technical Details
      * 1.9.1 gettext
      * 1.9.2 PO
      * 1.9.3 MO
  * 2 Everything else about translations
  * 3 See also



## Quick i18n

This is intended as be a quick guide to introduce yourself in the world of translations and get things done quickly. Check also [Language Codes](<../../Language_Codes.md> "Language Codes") and [BidiMode](<../../BidiMode.md> "BidiMode"). 

For your information the most used languages in the world are: 

  * 1 - Chinese: With more than 1.2 billion native speakers in the world.
  * 2 - Spanish: Spanish occupies the 2nd place and is spoken in approximately 30 countries.
  * 3 - English: 335 million worldwide, about 5% of the world’s population. (This does not take into account second language spoken).
  * 4 - Hindi: Spoken by over 260 million people.
  * 5 - Arabic: Spoken in almost 60 countries around the world.
  * 6 - Portuguese: Population of Brazil is over 200 million. The population of Portugal is just over 10 million.
  * 7 - Bengali: The main language of Bangladesh (population 155 million) and one of India’s many official languages.
  * 8 - Russian and Japanese.



### poedit

The best known tool is a program called [poedit](<https://poedit.net/>). poedit is a tool for translators. It produces both PO and MO as output. 

### Translating Forms

When you save a form with the i18n options enabled, it generates a file .lrj, that file is used to create the final .po file. So is very important that you include that file with your source code in the version system you're using, don't add that file to ignored (say .gitignore), else your translations will be broken. 

### Translating Resourcestrings

This is the way to store a resourcestring in a unit: 
    
    
    resourcestring
      Caption1 = 'Some text';
      HelloWorld1 = 'Hello World';
    

Resourcestrings are like normal string constants, that means you can assign them to any string: 
    
    
    Label1.Caption := HelloWorld1;
    

### Compiling into .po files

Resourcestrings are compiled into .po files if you enable i18n in the Lazarus IDE. Go to Project > Project Options > i18n > Enable i18n. When you recompile your application the .po files will be updated. You may also select a directory where .po files will be stored, recommended **po_files**. 

The default translation is recommended to be in English or the default language of your application, since it will be loaded if no other translation file is found. 

When you have your **project1.po** copy and paste it and rename it to **project1.es.po** or one of the [Language Codes](<../../Language_Codes.md> "Language Codes") of your choice. Then you will have 2 languages: English and Spanish. Then you must send to the translator the files for translation. 

Your folder structure will look something like this: 
    
    
    project1\po_files\
    project1\po_files\project1.po
    project1\po_files\project1.es.po
    

### Converting .po files to .mo files

When you have the translation finished in the .po file, you want to compile it to .mo to load it faster since is a binary format. To convert to .mo you can use poedit and go to File > Compile as .mo. 

Note that [Everything else about translations](<../../Everything_else_about_translations.md> "Everything else about translations") has a section about where a Lazarus app looks for its language files. 

### Automatic translation

When you have the .mo files ready, put them in the **locale** or **languages** folder right to your application executable, then include the unit **DefaultTranslator** and that's all. The translation will be done automatically. 
    
    
    uses
      DefaultTranslator;
    

You want to distribute only the .mo files in the **locale** or **languages** directory, since .po files are usefull only for making the translation and compiling them into a .mo file. 

Your folder structure will look something like this: 
    
    
    project1\project1.exe
    project1\locale\
    project1\locale\project1.mo
    project1\locale\project1.es.mo
    

### Testing translations

When you have everything ready. you want to test if the translations looks fine in your application for each language you have. Automatic translation has a feature that you can use in order to test each language quickly. 

You must run your executable with the command line parameter **\--lang** followed by the language code of your choice. 

You will run your executable like this in order to test Spanish translation: 
    
    
    project1.exe --lang es
    

And you will see the translated application. 

You can do this with the IDE. Go to **Run > Run Parameters ...**. In that window in the input **Command line parameters (without application name)** write this: 
    
    
    --lang it
    

Then **Run (F9)** and you will see the translated application. 

### Final steps

These steps are to get a better result for your entire translation. 

#### Translate LCL

To get everything translated you must include the LCL translations into your application **locale** folder. 

Copy everything inside the folder **C:\lazarus\lcl\languages** to your **locale** folder. Then you will have the LCL translated for your application. 

#### Format Settings

For Windows you must read [Step-by-step instructions for creating multi-language applications#Format settings](<../../Step-by-step_instructions_for_creating_multi-language_applications.md> "Step-by-step instructions for creating multi-language applications") to get format settings in the right place. 

Under Linux, BSD and macOS there are several locales defining things like time and date format or the thousands separator. In order to initialize the RTL you need to include the **clocale** unit in the uses section of your program (.lpr file). 

#### BiDiMode

[BidiMode](<../../BidiMode.md> "BidiMode") is for languages like Arabic that read and write right to left. See also [Script direction and languages](<http://www.w3.org/International/questions/qa-scripts.en>) for a list of LTR (Left To Right) and RTL (Right To Left) languages and most used languages in general. 

With this unit you can determine what BiDiMode is right for you. This code is tested in Lazarus Trunk and may or not work in current release (see the changes on this page to get the old source). 

First save this unit as ubidimodetools.pas: 
    
    
    unit ubidimodetools;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils;
    
    function GetLang: string;
    function GetBiDiMode: TBiDiMode;
    
    implementation
    
    uses
      FileUtil, LCLProc, LazUTF8;
    
    function GetLang: string;
    var
      T: string; // unused FallBackLang
      i: integer;
    begin
      Result := '';
      { We use the same method that is used in LCLTranslator unit }
    
      for i := 1 to Paramcount - 1 do
        if (ParamStrUTF8(i) = '--LANG') or (ParamStrUTF8(i) = '-l') or
          (ParamStrUTF8(i) = '--lang') then
          Result := ParamStrUTF8(i + 1);
    
      //Win32 user may decide to override locale with LANG variable.
      if Result = '' then
        Result := GetEnvironmentVariableUTF8('LANG');
    
      if Result = '' then
        LazGetLanguageIDs(Result, {%H-}T);
    end;
    
    function GetBiDiMode: TBiDiMode;
    begin
      case GetLang of
        // Arabic
        'ar': Result := bdRightToLeft;
        else
          Result := bdLeftToRight;
      end;
    end;
    
    end.
    

When you create a form do this: 
    
    
    BiDiMode := GetBiDiMode;
    

**Note:** You must be sure that you have the proper translations in order to get it working. 

**Note 2:** Maybe this is not neccesary at all since Windows can change the BiDiMode automatically. 

### Technical Details

#### gettext

The main technology involved in the process of translations is [GNU gettext](<https://www.gnu.org/software/gettext/manual/gettext.html>). FPC comes with the gettext unit. 
    
    
    uses
      gettext;
    

#### PO

PO – Portable Object. This is the file that you receive back from the translators. It’s a text file that includes the original text and the translations. 

#### MO

MO – Machine Object. The MO file includes the exact same contents as the PO file. The two files differ in their format. While a PO file is a text file and is easy for humans to read, MO files are compiled and are easy for computers to read. The unit gettext implements TMOFile and has several procedures to do the translation from .mo files, if you want to use it. 
    
    
    unit gettext;
    
    ...
    
    TMOFile = class
    
    ...
    
      procedure GetLanguageIDs(var Lang, FallbackLang: string);
      procedure TranslateResourceStrings(AFile: TMOFile);
      procedure TranslateUnitResourceStrings(const AUnitName:string; AFile: TMOFile);
      procedure TranslateResourceStrings(const AFilename: String);
      procedure TranslateUnitResourceStrings(const AUnitName:string; const AFilename: String);
    

## Everything else about translations

Here is all the translation material that was here in the past. There are a lot of articles about translations that can cause trouble to novices. To keep it simple this other material has been moved to: [Everything else about translations](<../../Everything_else_about_translations.md> "Everything else about translations"). Please keep this article as clean as possible. 

## See also

  * [IDE Development: Translations, i18n, lrt, po files](<../../IDE_Development.md> "IDE Development")
  * [Getting translation strings right](<../../Getting_translation_strings_right.md> "Getting translation strings right")
  * [Translating/Internationalization/Localization](<../../Lazarus_Documentation.md> "Lazarus Documentation")
  * [Step-by-step instructions for creating multi-language applications](<../../Step-by-step_instructions_for_creating_multi-language_applications.md> "Step-by-step instructions for creating multi-language applications")
  * [Embedding po files into the executable](<../../Everything_else_about_translations.md> "Everything else about translations")

---

_Source: [https://wiki.freepascal.org/Translations_/_i18n_/_localizations_for_programs](https://web.archive.org/web/20250323133518/https://wiki.freepascal.org/Translations_/_i18n_/_localizations_for_programs)_
