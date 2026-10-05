# DefaultFormatSettings

│ **[English (en)](<../en/DefaultFormatSettings.md> "DefaultFormatSettings")** │  **[français (fr)](</DefaultFormatSettings/fr> "DefaultFormatSettings/fr")** │  **русский (ru)** │    
****

**DefaultFormatSettings** является [глобальной переменной](<Global_variables.md> "Global variables/ru"), содержащей настройки [локали](</index.php?title=locale/ru&action=edit&redlink=1> "locale/ru \(page does not exist\)") по умолчанию. В частности, она определяет (ныне устаревшую) переменную [DecimalSeparator](</index.php?title=DecimalSeparator/ru&action=edit&redlink=1> "DecimalSeparator/ru \(page does not exist\)"). 
    
    
      
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

_Source: [https://wiki.freepascal.org/DefaultFormatSettings/ru](https://web.archive.org/web/20250209015341/https://wiki.freepascal.org/DefaultFormatSettings/ru)_
