# Everything else about translations

## Contents

  * 1 Introduction
  * 2 .po Files
  * 3 Translating
  * 4 IDE options for automatic updates of .po files
    * 4.1 Removal of Obsolete entries
    * 4.2 Duplicate entries
    * 4.3 Fuzzy entries
  * 5 Translating Forms, Datamodules and Frames
  * 6 Translating at start of program
  * 7 Compiling po files into the executable
    * 7.1 FPC Resources (Recommended)
    * 7.2 Lazarus Resources
  * 8 Where a Lazarus App looks for Language Files
  * 9 Compiling po files into the executable and change language while running
  * 10 Cross-platform method to determine system language
  * 11 Translating the IDE
    * 11.1 Files
    * 11.2 Translators
  * 12 Control design with internationalization capability
  * 13 See also



### Introduction

**Note:** This stuff was before in [Translations_/_i18n_/_localizations_for_programs](<Translations_/_i18n_/_localizations_for_programs.md> "Translations / i18n / localizations for programs"). If you think that something is really important move that stuff back. Please read both articles carefully in order to avoid duplicate stuff. 

### .po Files

There are many free graphical tools to edit .po files, which are simple text like the .rst files, but with some more options, like a header providing fields for author, encoding, language and date. Every FPC installation provides the tool **rstconv** (windows: rstconv.exe). This tool can be used to convert a .rst file into a .po file. The IDE can do this automatically. Some free tools: kbabel, po-auto-translator, poedit, virtaal. 

Virtaal has a translation memory containing source-target language pairs for items that you already translated once, and a translation suggestion function that shows already translated terms in various open source software packages. These function may save you a lot of work and improve consistency. 

Example of using rstconv directly: 
    
    
    rstconv -i unit1.rst -o unit1.po
    

### Translating

For every language the .po file must be copied and translated. The LCL translation unit uses the common language codes (en=english, de=german, it=italian, ...) to search. For example the German translation of unit1.po would be unit1.de.po. To achieve this, copy the unit1.po file to unit1.de.po, unit1.it.po, and whatever language you want to support and then the translators can edit their specific .po file. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For Brazilians/Portuguese: Lazarus IDE and LCL only have a Brazilian Portuguese translation and these files have 'pt_BR.po' extensions

### IDE options for automatic updates of .po files

  * The unit containing the resource strings must be added to the package or project.
  * You must provide a .po path, this means a separate directory. For example: create a sub directory _language_ in the package / project directory. For projects go to the Project > Project Options. For packages go to Options > IDE integration.



When this options are enabled, the IDE generates or updates the base .po file using the information contained in .rst and .lrt files (rstconv tool is then not necesary). The update process begins by collecting all existing entries found in base .po file and in .rst and .lrt files and then applying the following features it finds and brings up to date any translated .xx.po file. 

#### Removal of Obsolete entries

Entries in the base .po file that are not found in .rst and .lrt files are removed. Subsequently, all entries found in translated .xx.po files not found in the base .po file are also removed. This way, .po files are not cluttered with obsolete entries and translators don't have to translate entries that are not used. 

#### Duplicate entries

Duplicate entries occur when for some reason the same text is used for different resource strings, a random example of this is the file lazarus/ide/lazarusidestrconst.pas for the 'Gutter' string: 
    
    
      dlfMouseSimpleGutterSect = 'Gutter';
      dlgMouseOptNodeGutter = 'Gutter';
      dlgGutter = 'Gutter';
      dlgAddHiAttrGroupGutter = 'Gutter';
    

A converted .rst file for this resource strings would look similar to this in a .po file: 
    
    
    #: lazarusidestrconsts.dlfmousesimpleguttersect
    msgid "Gutter"
    msgstr ""
    #: lazarusidestrconsts.dlgaddhiattrgroupgutter
    msgid "Gutter"
    msgstr ""
    etc.
    
    

Where the lines starting with "#: " are considered comments and the tools used to translate this entries see the repeated msgid "Gutter" lines like duplicated entries and produce errors or warnings on loading or saving. Duplicate entries are considered a normal eventuality on .po files and they need to have some context attached to them. The msgctxt keyword is used to add context to duplicated entries and the automatic update tool use the entry ID (the text next to "#: " prefix) as the context, for the previous example it would produce something like this: 
    
    
    #: lazarusidestrconsts.dlfmousesimpleguttersect
    msgctxt "lazarusidestrconsts.dlfmousesimpleguttersect"
    msgid "Gutter"
    msgstr ""
    #: lazarusidestrconsts.dlgaddhiattrgroupgutter
    msgctxt "lazarusidestrconsts.dlgaddhiattrgroupgutter"
    msgid "Gutter"
    msgstr ""
    etc.
    
    

On translated .xx.po files the automatic tool does one additional check: if the duplicated entry was already translated, the new entry gets the old translation, so it appears like being translated automatically. 

The automatic detection of duplicates is not yet perfect, duplicate detection is made as items are added to the list and it may happen that some untranslated entries are read first. So it may take several passes to get all duplicates automatically translated by the tool. 

#### Fuzzy entries

Changes in resource strings affect translations, for example if initially a resource string was defined like: 
    
    
    dlgEdColor = 'Syntax highlight';
    

this would produce a .po entry similar to this 
    
    
    #: lazarusidestrconsts.dlgedcolor
    msgid "Syntax highlight"
    msgstr ""
    

which if translated to Spanish (this sample was taken from lazarus history), may result in 
    
    
    #: lazarusidestrconsts.dlgedcolor
    msgid "Syntax highlight"
    msgstr "Color"
    

Suppose then that at a later time, the resource string has been changed to 
    
    
      dlgEdColor = 'Colors';
    

the resulting .po entry may become 
    
    
    #: lazarusidestrconsts.dlgedcolor
    msgid "Colors"
    msgstr ""
    

Note that while the ID remained the same lazarusidestrconsts.dlgedcolor the string has changed from 'Syntax highlight' to 'Colors'. As the string was already translated the old translation may not match the new meaning. Indeed, for the new string probably 'Colores' may be a better translation. The automatic update tool notices this situation and produces an entry like this: 
    
    
    #: lazarusidestrconsts.dlgedcolor
    #, fuzzy
    #| msgid "Syntax highlight"
    msgctxt "lazarusidestrconsts.dlgedcolor"
    msgid "Colors"
    msgstr "Color"
    

In terms of .po file format, the "#," prefix means the entry has a flag (fuzzy) and translator programs may present a special GUI to the translator user for this item. In this case, the flag would mean that the translation in its current state is doubtful and needs to be reviewed more carefully by translator. The "#|" prefix indicates what was the previous untranslated string of this entry and gives the translator a hint why the entry was marked as fuzzy. 

### Translating Forms, Datamodules and Frames

When the i18n option is enabled for the project / package then the IDE automatically creates .lrt files for every form. It creates the .lrt file on saving a unit. So, if you enable the option for the first time, you must open every form once, move it a little bit, so that it is modified, and save the form. For example if you save a form _unit1.pas_ the IDE creates a _unit1.lrt_. And on compile the IDE gathers all strings of all .lrt files and all .rst file into a single .po file (projectname.po or packagename.po) in the i18n directory. 

For the forms to be actually translated at runtime, you have to assign a translator to LRSTranslator (defined in LResources) in the initialization section to one of your units 
    
    
    ...
    uses
      ...
      LResources;
    ...
    ...
    initialization
      LRSTranslator := TPoTranslator.Create('/path/to/the/po/file');
    

~~However there's no TPoTranslator class (i.e a class that translates using .po files) available in the LCL. This is a possible implementation (partly lifted from DefaultTranslator.pas in the LCL):~~ The following code isn't needed anymore if you use recent Lazarus 0.9.29 snapshots. Simply include DefaultTranslator in Uses clause. 
    
    
    unit PoTranslator;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, LResources, typinfo, Translations;
    
    type
    
     { TPoTranslator }
    
     TPoTranslator=class(TAbstractTranslator)
     private
      FPOFile:TPOFile;
     public
      constructor Create(POFileName:string);
      destructor Destroy;override;
      procedure TranslateStringProperty(Sender:TObject; 
        const Instance: TPersistent; PropInfo: PPropInfo; var Content:string);override;
     end;
    
    implementation
    
    { TPoTranslator }
    
    constructor TPoTranslator.Create(POFileName: string);
    begin
      inherited Create;
      FPOFile:=TPOFile.Create(POFileName);
    end;
    
    destructor TPoTranslator.Destroy;
    begin
      FPOFile.Free;
      inherited Destroy;
    end;
    
    procedure TPoTranslator.TranslateStringProperty(Sender: TObject;
      const Instance: TPersistent; PropInfo: PPropInfo; var Content: string);
    var
      s: String;
    begin
      if not Assigned(FPOFile) then exit;
      if not Assigned(PropInfo) then exit;
    {DO we really need this?}
      if Instance is TComponent then
       if csDesigning in (Instance as TComponent).ComponentState then exit;
    {End DO :)}
      if (AnsiUpperCase(PropInfo^.PropType^.Name)<>'TTRANSLATESTRING') then exit;
      s:=FPOFile.Translate(Content, Content);
      if s<>'' then Content:=s;
    end;
    
    end.
    

Alternatively you can transform the .po file into .mo using msgfmt (isn't needed anymore if you use recent 0.9.29 snapshot) and simply use the DefaultTranslator unit 
    
    
    ...
    uses
       ...
       DefaultTranslator;
    

which will automatically look in several standard places for a .po file (higher precedence) or .mo file ~~(the disadvantage is that you'll have to keep around both the .mo files for the DefaultTranslator unit and the .po files for TranslateUnitResourceStrings)~~. If you use DefaultTranslator, it will try to automatically detect the language based on the LANG environment variable (overridable using the --lang command line switch), then look in these places for the translation (LANG stands for the desired language, ext can be either po or mo): 

  * <Application Directory>/<LANG>/<Application Filename>.<ext>
  * <Application Directory>/languages/<LANG>/<Application Filename>.<ext>
  * <Application Directory>/locale/<LANG>/<Application Filename>.<ext>
  * <Application Directory>/locale/LC_MESSAGES/<LANG/><Application Filename>.<ext>



under unix-like systems it will also look in 

  * /usr/share/locale/<LANG>/LC_MESSAGES/<Application Filename>.<ext>



as well as using the short part of the language (e.g. if it is "es_ES" or "es_ES.UTF-8" and it doesn't exist it will also try "es") 

### Translating at start of program

For every .po file, you must call TranslateUnitResourceStrings. The LCL po file is lclstrconsts. For example you do this in FormCreate of your MainForm: 
    
    
    uses
     ..., gettext, translations;
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      PODirectory, Lang, FallbackLang: String;
    begin
      PODirectory := '/path/to/lazarus/lcl/languages/';
      GetLanguageIDs(Lang, FallbackLang);
      Translations.TranslateUnitResourceStrings('LCLStrConsts', PODirectory + 'lclstrconsts.%s.po', Lang, FallbackLang);
    
      // the following dialog now shows translated buttons:
      MessageDlg('Title', 'Text', mtInformation, [mbOk, mbCancel, mbYes], 0);
    end;
    

### Compiling po files into the executable

If you don't want to install the .po files, but put all files of the application into the executable, use one the following methods. 

#### FPC Resources (Recommended)

Normal resources are now recommended for current FPC (including all recent Lazarus versions) [Lazarus_Resources](<Lazarus_Resources.md> "Lazarus Resources")

  * Add the resources (.po files) to executable with the Lazarus IDE (Project Options > Resources) as RCDATA.


    
    
    uses
      Classes, Translation, LCLType,
    
    function Translate(const Language: string): boolean;
    var
      Res: TResourceStream;
      PoStringStream: TStringStream;
      PoFile: TPOFile;
    begin
      Res := TResourceStream.Create(HInstance, 'project1.' + Language, RT_RCDATA);
      PoStringStream := TStringStream.Create('');
      Res.SaveToStream(PoStringStream);
      Res.Free;
    
      PoFile := TPOFile.Create(False);
      PoFile.ReadPOText(PoStringStream.DataString);
      PoStringStream.Free;
    
      Result := TranslateResourceStrings(PoFile);
      PoFile.Free;
    end;
    

#### Lazarus Resources

  * Create a new unit (not a form!).
  * Convert the .po file(s) to .lrs using tools/lazres:


    
    
    ./lazres unit1.lrs unit1.de.po
    

This will create an include file unit1.lrs beginning with 
    
    
    LazarusResources.Add('unit1.de','PO',[
      ...
    

  * Add the code:


    
    
    uses LResources, Translations;
    
    resourcestring
      MyCaption = 'Caption';
    
    function TranslateUnitResourceStrings: boolean;
    var
      r: TLResource;
      POFile: TPOFile;
    begin
      r:=LazarusResources.Find('unit1.de','PO');
      POFile:=TPOFile.Create(False);  //if Full=True then you can get a crash (Issue #0026021)
      try
        POFile.ReadPOText(r.Value);
        Result:=Translations.TranslateUnitResourceStrings('unit1',POFile);
      finally
        POFile.Free;
      end;
    end;
    
    initialization
      {$I unit1.lrs}
    

  * Call TranslateUnitResourceStrings at the beginning of the program. You can do that in the initialization section if you like.



Unfortunately this code will not compile with Lazarus 1.2.2 and earlier. 

For these Lazarus versions you can use something like this: 
    
    
    type
      TTranslateFromResourceResult = (trSuccess, trResourceNotFound, trTranslationError);
    
    function TranslateFromResource(AResourceName, ALanguage : String): TTranslateFromResourceResult;
    var
      LRes : TLResource;
      POFile : TPOFile = nil;
      SStream : TStringStream = nil;
    begin
      Result := trResourceNotFound;
      LRes := LazarusResources.Find(AResourceName + '.' + ALanguage, 'PO');
      if LRes <> nil then
      try
        SStream := TStringStream.Create(LRes.Value); 
        POFile := TPoFile.Create(SStream, False);    // WARNING: This example relies on the bug in TStringStream.Create(...) that the
                                                     //          position in the stream is incorrectly set to 0, even though the stream
                                                     //          is initialized with LRes.Value i.e. assumed to be ready for *appending*!!!
        try
          if TranslateUnitResourceStrings(AResourceName, POFile) then Result := trSuccess
          else Result := trTranslationError;
        except
          Result := trTranslationError;
        end;
      finally
        if Assigned(SStream) then SStream.Free;
        if Assigned(POFile) then POFile.Free;
      end;
    end;
    

Usage example: 
    
    
    initialization
      {$I lclstrconsts.de.lrs}
      TranslateFromResource('lclstrconsts', 'de');
    end.
    

  


### Where a Lazarus App looks for Language Files

As a multi-language Lazarus app starts, it looks for a nominated language file in a number of places and looks for a number of different file names. The following Lists are for Linux, I believe pretty much the same for Windows but no '/usr/local' of course. The order is important, it stops looking as soon as it finds a suitable candidate. In all cases, the $LA refers to the language code, es, fr etc. The $APP is the application's name. 

**.po files** \- these contain raw, editable text and are probably what arrives on your desk from your translators. Looked for first, perhaps as an aid to development ? 
    
    
    ./$LA/$APP.po
    ./languages/$LA/$APP.po
    ./locale/$LA/$APP.po
    ./locale/$LA/LC_MESSAGES/$APP.po
    /usr/share/locale/$LA/LC_MESSAGES/$APP.po
    ./$APP.$LA.po
    ./locale/$APP.$LA.po
    ./languages/$APP.$LA.po
    /usr/share/locale/$LA/LC_MESSAGES/$APP.po
    

**.pot files** \- these are template files. Note that they don't identify their language, I wonder why they are even looked for ? 
    
    
    ./$APP.pot
    ./locale/$APP.pot
    ./languages/$APP.pot
    

**.mo files** \- these are the machine readable ones, faster and smaller than text, these are the only files you should ship with a finished product. 
    
    
    ./$LA/$APP.mo
    ./languages/$LA/$APP.mo
    ./locale/$LA/$APP.mo
    ./locale/$LA/LC_MESSAGES/$APP.mo
    /usr/share/locale/$LA/LC_MESSAGES/$APP.mo
    ./$APP.$LA.mo
    ./locale/$APP.$LA.mo
    ./languages/$APP.$LA.mo
    /usr/share/locale/$LA/LC_MESSAGES/$APP.mo
    ./$APP.mo
    ./locale/$APP.mo
    ./languages/$APP.mo
    

**Note 1 :** Firstly, note that Lazarus looks first for .po and then .pot files. So, be very careful about leaving such files lying around in the 'wrong' place. If your app finds an old .po file of the right language, perhaps left there while doing some test, it won't look further for the lovely new .mo file you just made. 

**Notes 2 :** A linux system should always put its language files in /usr/local/somewhere. And really, you should only let your end users get a .mo file, even your beta testers. 

**Note 3 :** Above, where we say ./ we mean the directory where the binary is, not the current working directory. 

### Compiling po files into the executable and change language while running

(State: Lazarus 1.6; last tested with Lazarus 2.0.10) 

With this method a standalone program is possible which can change language without restarting the program. Captions of LCL-components are translated and updated automatically, ressourcestrings change their contents according to the chosen language and can be used to reassign strings manually. 

Convert the .po file(s) to .lrs using tools/lazres: 
    
    
    ./lazres language.lrs testprog1.de.po testprog1.en.po
    

Create a new unit (not a form) and insert the below function: 
    
    
    uses
      Forms, LResources, LCLTranslator, Translations;
    
    function TranslateTo(POFileName: string): boolean;
    var
      res: TLResource;
      ii: Integer;
      POFile: TPOFile;
      LocalTranslator: TUpdateTranslator;
    begin
      Result:= false;
      res:=LazarusResources.Find(POFileName,'PO');
      if res = nil then EXIT;
      POFile:=TPOFile.Create(False);  //if Full=True then you can get a crash (Issue #0026021)
      try
        POFile.ReadPOText(res.Value);
        Result:= Translations.TranslateResourceStrings(POFile);
        if not Result then EXIT;
        
        LocalTranslator := TPOTranslator.Create(POFile);
    
        LRSTranslator := LocalTranslator;
        for ii := 0 to Screen.CustomFormCount-1
          do LocalTranslator.UpdateTranslation(Screen.CustomForms[ii]);
        for ii := 0 to Screen.DataModuleCount-1
          do LocalTranslator.UpdateTranslation(Screen.DataModules[ii]);
        LRSTranslator:= nil;
        LocalTranslator.Destroy;
        //POFile.Destroy; //DONT! Already done in LocalTranslator.Destroy
      finally end;
    end;
    

Also insert the include statement (here the above mentioned lrs-file is in subfolder "locale"): 
    
    
    initialization
      {$I locale/language.lrs}
    

Then the language can be changed (examples German and English) while runtime with 
    
    
    TranslateTo('testprog1.de');
    

and 
    
    
    TranslateTo('testprog1.en');
    

### Cross-platform method to determine system language

The following function delivers a string that represents the language of the user interface. It supports Linux, Mac OS X and Windows. 
    
    
    uses
      Classes, SysUtils {add additional units that may be needed by your code here}
      {$IFDEF win32}
      , Windows
      {$ELSE}
      , Unix
        {$IFDEF LCLCarbon}
      , MacOSAll
        {$ENDIF}
      {$ENDIF}
      ;
    
    
    
    function GetOSLanguage: string;
    {platform-independent method to read the language of the user interface}
    var
      l, fbl: string;
      {$IFDEF LCLCarbon}
      theLocaleRef: CFLocaleRef;
      locale: CFStringRef;
      buffer: StringPtr;
      bufferSize: CFIndex;
      encoding: CFStringEncoding;
      success: boolean;
      {$ENDIF}
    begin
      {$IFDEF LCLCarbon}
      theLocaleRef := CFLocaleCopyCurrent;
      locale := CFLocaleGetIdentifier(theLocaleRef);
      encoding := 0;
      bufferSize := 256;
      buffer := new(StringPtr);
      success := CFStringGetPascalString(locale, buffer, bufferSize, encoding);
      if success then
        l := string(buffer^)
      else
        l := '';
      fbl := Copy(l, 1, 2);
      dispose(buffer);
      {$ELSE}
      {$IFDEF LINUX}
      fbl := Copy(GetEnvironmentVariable('LC_CTYPE'), 1, 2);
        {$ELSE}
      GetLanguageIDs(l, fbl);
        {$ENDIF}
      {$ENDIF}
      Result := fbl;
    end;
    

### Translating the IDE

#### Files

The .po files of the IDE are in the lazarus source directory: 

  * lazarus/languages strings for the IDE
  * lazarus/lcl/languages/ strings for the LCL
  * lazarus/components/ideintf/languages/ strings for the IDE interface



#### Translators

  * The German translation is maintained by Swen Heinig.
  * The Finnish translation is maintained by Seppo Suurtarla
  * The Russian translation is maintained by Maxim Ganetsky
  * The French translation is maintained by Gilles Vasseur



When you want to start a new translation, ask on the mailing if someone is already working on that. 

Please read carefully: [Translating/Internationalization/Localization](<Lazarus_Documentation.md> "Lazarus Documentation")

### Control design with internationalization capability

Controls can easily be designed with i18n-support, e.g. a button with translatable caption. Therefore the control should be packed in a package and the published property of the regarding string has to be of type TTranslateString. After the package is installed in Lazarus, the control has the same translate capabilities as the known LCL controls. 
    
    
    uses LCLType;
    
    type TFancyButton = class(TCustomControl)
      ...
      protected
        fButtonCaption: TTranslateString;
        procedure SetButtonCaption(ACaption: TTranslateString);
      published
        property Caption : TTranslateString read fButtonCaption write SetButtonCaption
    

### See also

[Translating/Internationalization/Localization](<Lazarus_Documentation.md> "Lazarus Documentation") [Translations_/_i18n_/_localizations_for_programs](<Translations_/_i18n_/_localizations_for_programs.md> "Translations / i18n / localizations for programs")

---

_Source: [https://wiki.freepascal.org/Everything_else_about_translations](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/Everything_else_about_translations)_
