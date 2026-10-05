# Getting translation strings right

│ **English (en)** │  **[русский (ru)](<../ru/Getting_translation_strings_right.md>)** │

This page contains some basic notes on getting translation strings right from the start, from the original writers (e.g. most often programmers) angle. 

"To get it right" in this context means that the translation strings have been prepared properly for easier further translation, and the original version adapted to the basic requirements for using them. 

Although it is tried to be as language neutral as possible, there is in fact a slight bias towards English in particular. Feel free to extend or discard the ones which do not apply to your situation (and maybe add them to this page?). 

## Contents

  * 1 General
    * 1.1 Make your original language strings spotless, clear and consistent
  * 2 Technical issues
    * 2.1 Resourcestrings, and GNU gettext
    * 2.2 ResourceStrings in the IDE
    * 2.3 ResourceStrings in your Application
    * 2.4 The format() function
    * 2.5 Converting the translation into the right character set
  * 3 English related
  * 4 See also



### General

To avoid problems, simply make sure that the **original strings are okay** in the first place - there are several reasons to do so: 

  * the original strings are usually used as default translation, making a bad impression to the end user who happens to use the default strings.
  * even worse, this makes the translator's work unnecessarily harder: he will have more trouble conveying the original information to another language. Just remember the "Garbage In, Garbage Out" principle which applies perfectly here.



#### Make your original language strings spotless, clear and consistent

Given the above, please find some tips when writing messages/strings for user consumption. 

Try to make sure that your **spelling is correct** \- use a dictionary if you are unsure. 

Use understandable and well-known phrases for a given situation you want to describe, in a **consistent** manner. If you are unsure whether something is common, try to put other programs in the same situation and examine their responses. Literature and help files are also often a good resource for the exact special terms or phrases, or style issues. Try to be consistent in choosing phrases too. For example the questions 

    _Delete the file?_
    _Erase the file?_
    _Remove the file?_
    _Wipe the file?_
    _Zap the file?_

all have a somewhat similar meaning, but when used interchangably for no apparent reason, readers may start to try and invent a (non-existing) reason for the different terminology. Especially translators are very prone to this error, since they often do not know the exact context of a particular message (e.g. information about the origin of the message) and may interpret simple word variations as indication of important differences, and will likely be tempted to choose uncommon (bad) translations. 

A lot of translation software (e.g. Virtaal) can help you in two ways: 

  * it has a translation memory that allows you to remember already translated strings and help you to translate terms consistently
  * it shows you how comparable strings in other open source software were translated, giving you the forms most often used



Especially for error messages to the user: try to **describe the problem** itself in appropriate words. This is never the state of the program which led to the error message. This is only useful for the person debugging the program, but not for the user. Users will either simply shrug their shoulders and ignore it in the best case, or choose another program in the worst case because without a proper problem description the user will not be able to fix the problem and continue his work - all that only because the program was not able to give a proper problem description. 

Give (easily) **understandable descriptions**. Do not try to impress your audience with foreign or very technical words only for telling that the current work has not yet been saved if not really necessary. 

Especially try to avoid multiple negations within a single sentence in your wordings, they are nearly always harder to read than their non-negated counterparts. An example could be 

    _This component can not be dropped on non-TControls._

which, in its non-negated form 

    _This component can only be dropped on TControls._

is certainly easier to read and understand. 

### Technical issues

In this section a short overview of technical issues, basically an overview of existing possibilites are given. These include the resourcestring construct, GNU gettext() and the format() function. 

#### Resourcestrings, and GNU gettext

Free Pascal has some built-in language constructs for providing a way of handling constant strings. There is a special section of a unit called "resourcestring" which was specifically introduced for this reason. See the appropriate section of the FPC manual (prog.pdf, pg. 89ff or [here](<http://www.freepascal.org/docs-html/prog/progch9.html#x184-1870009>)) for the details. 

GNU gettext is a special set of utilities to provide translations for your programs, see the FPC manual once more (prog.pdf, pg. 91ff or [here](<http://www.freepascal.org/docs-html/prog/progse41.html#x210-2130009.4>)). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** GNU gettext has a conceptual flaw which does not allow mapping of a single original string to multiple translated strings, be aware of that.

#### ResourceStrings in the IDE

  * Lazarus has a tool to easily create resourcestrings from string constants. See [Make ResourceString](<IDE_Window__Make_ResourceString.md> "IDE Window: Make ResourceString")
  * For each resourcestring section FPC creates a .rst file, but there are no editor for these files. Lazarus can automatically create .po files of the .rst files. There are a lot of tools to edit .po files (e.g. kbabel).



To enable creating the .po files for a package do the following: 

  * Create a sub directory 'languages' (or 'locale', or whatever)
  * Open the package. Then Options -> IDE Integration -> Directory of .po files set to _languages_



The next time you compile the package, the IDE will create the .po files. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The .rst files must belong to package units. Otherwise the IDE ignores foreign .rst files.

The same works for projects. The directory is set in Project -> Project Options -> IDE Integration -> Directory of .po files. 

  * To create a German translation: copy the unit1.po file to unit1.de.po. Then use a text editor or a .po Editor to translate all strings.
  * The IDE will automatically load .po files for installed packages, if they exists. For example see lazarus/components/projecttemplates/languages/.
  * ToDo: Implement and document updating the translated .po files when new resourcestrings are added.
  * ToDo: Implement and document collecting all .po files of statically linked packages.



#### ResourceStrings in your Application

You can load the .po files at initialization to translate the resourcestrings. Add this to your .lpr file: 
    
    
    ...
    uses
      ...
      Translations, LazUTF8;
    
    procedure TranslateLCL;
    var
      PODirectory, Lang, FallbackLang: String;
    begin
      PODirectory:='/path/to/lazarus/lcl/languages/';
      Lang:='';
      FallbackLang:='';
      LazGetLanguageIDs(Lang,FallbackLang); // in unit LazUTF8
      Translations.TranslateUnitResourceStrings('LCLStrConsts',
                          PODirectory+'lclstrconsts.%s.po',Lang,FallbackLang);
      // ... add here a TranslateUnitResourceStrings call for every po file ...
    end;
    
    begin
      TranslateLCL;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** for macOS: The supported language IDs should be added into the application bundle property list to CFBundleLocalizations key, see lazarus.app/Contents/Info.plist for an example.

#### The format() function

To not only allow completely static strings in translations, you can use the [format()](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/format.html> "doc:rtl/sysutils/format.html") method of the sysutils unit. It is able to replace placeholders within a given text by their actual value given as secondary parameter in a set. Example: 

    `format('Tomorrow on %0:s there will be %1:s.', ['Sunday', 'sunshine']);`

returns 

    `Tomorrow on Sunday there will be sunshine.`

In this case the %0:s is a placeholder for the first (index 0) argument in the set of actual values (Sunshine), and likewise %1:s. For an exact definition of the allowed placeholders and their syntax see the FPC reference manual. 

Some guidelines for the usage of the format() function 

  * Try to use indexed placeholders in the original strings, even if they are optional. When used, they allow the translator to move the arguments easily within the sentence allowing him more natural expressions (and actually sometimes moving sentence parts is required to create proper sentences in that language).
  * Never compose a sentence out of more than one string. Always use the format() method from the sysutils unit to construct the correct string using placeholders during runtime. Translators will usually not be able to reconstruct the whole sentence, therefore not able to give a good translation; only consider that there are often hundreds of such strings within a single translation database...
  * Do not format using whitespaces. Simply move your label to the appropriate position in the first place. There may be problems with font changes, and seemingly superfluous spaces will be in danger of being trimmed by the translator.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** ~~Since format() does not interpret escaped control characters (e.g. like C's "\n", "\t", etc) and GNU gettext for any reason being the translation system of choice (and the tools based on it not being able to interpret non-escaped control characters), it is required to programmatically insert linebreaks.~~ In the current lazarus version, "\n" and "\t" in translated strings are replaced by newline and tab.

#### Converting the translation into the right character set

For example: converting a file from ISO-8859-1 to UTF-8: 

    `iconv --from-code=ISO-8859-1 --to-code=UTF-8 oldfile.po > newfile.po`

Do not forget to change the line 

    "Content-Type: text/plain; charset=ISO-8859-1\n"

to 

    "Content-Type: text/plain; charset=UTF8\n"

### English related

This section contains notes which particularly apply when using English as the base language for translations. 

  * Make sure to reserve enough space where the text is output: English is a language in which texts are almost always shorter (in characters) than their respective translations, so plan ahead by reserving enough space. Experience shows that very short strings of a few characters length often almost double in size; this difference decreases as the strings get longer.
  * Avoid abbreviations in English; in addition to the fact that this shortens the already short strings even more, there are severe problems with e.g. languages that use ideographic characters where these abbreviations simply do not exist at all.
  * Since this is often an issue: In English punctuation marks (full stop, comma, ...) are part of the previous words, or form some sort of words themselves if there is no previous word (in case of an enumeration). Especially after a semicolon there should always be a trailing space. 

    _There was an error ! Please check file settings , compiler settings,... to fix this issue._



    has horrible punctuation and therefore simply looks bad and is harder to read than usual. Consider that common line break algorithms break the line on whitespaces, possibly resulting in a single stop at the beginning of a line... 

    _There was an error! Please check file settings, compiler settings, ... to fix this issue._
    would probably be okay only considering punctuation.

### See also

  * [Translations / i18n / localizations for programs](<Translations_/_i18n_/_localizations_for_programs.md> "Translations / i18n / localizations for programs")
  * [Translating/Internationalization/Localization](<Lazarus_Documentation.md> "Lazarus Documentation")
  * [Step-by-step instructions for creating multi-language applications](<Step-by-step_instructions_for_creating_multi-language_applications.md> "Step-by-step instructions for creating multi-language applications")

---

_Source: [https://wiki.freepascal.org/Getting_translation_strings_right](https://web.archive.org/web/20240308084620/https://wiki.freepascal.org/Getting_translation_strings_right)_
