# Locale settings for macOS

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

In macOS, like on other Unix platforms, the RTL does not load the locale settings (date & time separator, currency symbol, etc) by default. They can be initialised in three ways. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Adding both _iosxlocale_ and _clocale_ to the Uses clause will cause the second in line to overwrite the settings set by the first one.

## Automatic initialisation based on the System Preferences settings

In FPC 2.7.1 and later, adding the _iosxlocale_ unit to the uses clause will initialise the locale settings using the settings from the System Preferences. This unit is not available in older versions of FPC. 

## Automatic initialisation based on the settings in the Unix layer

Adding the _clocale_ unit to the uses clause will cause the locale settings to be initialised using the configuration set at the Unix layer of macOS (based on the _LANG_ and related environment variables). This unit is also available on other Unix-like platforms. 

The macOS `locale` command line utility available since macOS 10.4 (Tiger) will reveal your locale settings. For example, open a Terminal and type: 
    
    
    $ locale
    LANG="en_AU.UTF-8"
    LC_COLLATE="en_AU.UTF-8"
    LC_CTYPE="en_AU.UTF-8"
    LC_MESSAGES="en_AU.UTF-8"
    LC_MONETARY="en_AU.UTF-8"
    LC_NUMERIC="en_AU.UTF-8"
    LC_TIME="en_AU.UTF-8"
    LC_ALL=
    

## Manual initialisation

  * Hardcoded:


    
    
     
      // use in initialization or in onCreate of the main form
      DateSeparator := '.';
      ShortDateFormat := 'dd.mm.yyyy';
      LongDateFormat := 'd. mmmm yyyy';
    

Source: [Lazarus Form post](<http://forum.lazarus.freepascal.org/index.php?topic=9566.0>)

  * Partially replicating the functionality of the _iosxlocale_ unit:


    
    
     
    
    uses
      MacOSAll, CocoaUtils;
    
    var
      theFormatString: string;
      theFormatter: CFDateFormatterRef;
    
    procedure GetMacDateFormats;
    begin
      theFormatter := CFDateFormatterCreate(kCFAllocatorDefault, CFLocaleCopyCurrent, kCFDateFormatterMediumStyle, kCFDateFormatterNoStyle);
      theFormatString := CFStringToStr(CFDateFormatterGetFormat(theFormatter));
      if pos('.', theFormatString) > 0 then
        DefaultFormatSettings.DateSeparator := '.'
      else if pos('/', theFormatString) > 0 then
        DefaultFormatSettings.DateSeparator := '/'
      else if pos('-', theFormatString) > 0 then
        DefaultFormatSettings.DateSeparator := '-';
      DefaultFormatSettings.ShortDateFormat := theFormatString;
      CFRelease(theFormatter); 
      theFormatter := CFDateFormatterCreate(kCFAllocatorDefault, CFLocaleCopyCurrent, kCFDateFormatterLongStyle, kCFDateFormatterNoStyle);
      theFormatString := CFStringToStr(CFDateFormatterGetFormat(theFormatter));
      DefaultFormatSettings.LongDateFormat := theFormatString;
      CFRelease(theFormatter); 
    end;
    

If the procedure _GetMacDateFormats_ is called in the beginning of the program's main unit the "International" settings of systems preferences are used.

---

_Source: [https://wiki.freepascal.org/Locale_settings_for_macOS](https://web.archive.org/web/20230309162410/https://wiki.freepascal.org/Locale_settings_for_macOS)_
