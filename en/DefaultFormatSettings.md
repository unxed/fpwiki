# DefaultFormatSettings

│ **English (en)** │  **[русский (ru)](<../ru/DefaultFormatSettings.md>)** │

**DefaultFormatSettings** is a [global variable](<Global_variables.md> "Global variables") containing settings for the default [locale](</index.php?title=locale&action=edit&redlink=1> "locale \(page does not exist\)"). Among others it defines the decimal separator, thereby replacing the (now deprecated) global variable [DecimalSeparator](<DecimalSeparator.md> "DecimalSeparator"). 
    
    
      
    DefaultFormatSettings : TFormatSettings = (
        CurrencyFormat: 1;
        NegCurrFormat: 5;
        ThousandSeparator: ',';
        DecimalSeparator: '.';
        CurrencyDecimals: 2;
        DateSeparator: '-';
        TimeSeparator: ':';
        ListSeparator: ',';
        CurrencyString: '$';
        ShortDateFormat: 'd/m/y';
        LongDateFormat: 'dd" "mmmm" "yyyy';
        TimeAMString: 'AM';
        TimePMString: 'PM';
        ShortTimeFormat: 'hh:nn';
        LongTimeFormat: 'hh:nn:ss';
        ShortMonthNames: ('Jan','Feb','Mar','Apr','May','Jun', 
                          'Jul','Aug','Sep','Oct','Nov','Dec');
        LongMonthNames: ('January','February','March','April','May','June',
                         'July','August','September','October','November','December');
        ShortDayNames: ('Sun','Mon','Tue','Wed','Thu','Fri','Sat');
        LongDayNames:  ('Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday');
        TwoDigitYearCenturyWindow: 50;
      );
    
      FormatSettings : TFormatSettings absolute DefaultFormatSettings;

---

_Source: [https://wiki.freepascal.org/DefaultFormatSettings](https://web.archive.org/web/20250114094810/https://wiki.freepascal.org/DefaultFormatSettings)_
