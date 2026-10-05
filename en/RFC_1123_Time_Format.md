# RFC 1123 Time Format

│ **English (en)** │

[Function](<Function.md> "Function") RFC1123TimeFormat(aDateTime:[TDateTime](<TDateTime.md> "TDateTime"); isLocalTime:boolean):string; creates and displays the time in the RFC 1123 International format: 
    
    
     DAY, DD MON YYYY hh:mm:ss GMT
    

e.g: 
    
    
     Sun, 21 Oct 2018 12:16:24 GMT
    

Day in the form of DAY, three letter abbreviation followed by a [comma](<Comma.md> "Comma"). Date in the form DD-MON-YYYY (day, month, and year), where Month in the form three letter abbreviation. 

The date and time are displayed on the same line, date first followed by the time, and the time is in this format, with a space between the date's year and the hours, Time in the form hh:mm:ss (hours, minutes, and seconds) 

The time is Coordinated Universal Time (UTC), though the indication can optionally be Greenwich Mean Time (GMT) 

## program RFC1123_TimeFormat
    
    
    program RFC1123_TimeFormat;
    
    uses
      SysUtils, DateUtils;
    
    function RFC1123TimeFormat(aDateTime:TDateTime; isLocalTime:boolean):string;
    const
      ShortDayNamesEnglish :array[1..7] of string =
        ('Sun', 'Mon','Tue', 'Wed', 'Thu', 'Fri', 'Sat');
      ShortMonthNamesEnglish :array[1..12] of string = ('Jan', 'Feb','Mar', 'Apr',
         'May','Jun','Jul','Aug', 'Sep','Oct','Nov','Dec');
    var
       day,month : string;
       someDateTime:TDateTime;
    begin
      if isLocalTime then someDateTime := LocalTimeToUniversal(aDateTime)
        else  someDateTime := aDateTime;
      day := ShortDayNamesEnglish[DayOfWeek(someDateTime)];
      month := ShortMonthNamesEnglish[MonthOf(someDateTime)] ;
      result := day+', '+FormatDateTime('dd', someDateTime)+' '+
        month+' '+FormatDateTime('yyyy hh:nn:ss', someDateTime)+' GMT';
    end;
    
    
    var
      someDate: TDateTime;
    begin
      someDate := EncodeDate(2018, 10, 23);
      WriteLn(RFC1123TimeFormat(now,true));
      WriteLn(RFC1123TimeFormat(someDate,false));
      ReadLn;
    end.
    

[Unit](<Unit.md> "Unit") DateUtils function LocalTimeToUniversal convert local time to UTC time Declaration 
    
    
      function LocalTimeToUniversal( LT: TDateTime ):TDateTime;
    

Unit SysUtils function FormatDateTime provides rich formatting of a TDateTime value into a string. Formatting is defined by the Formatting string. 
    
    
      function FormatDateTime ( const Formatting : string; DateTime : TDateTime ) : string;
    

Unit SysUtils function EncodeDate build a TDateTime value from year, month and day values 
    
    
      function EncodeDate(Year, Month, Day :word): TDateTime;
    

## See also

  * [TDateTime](<TDateTime.md> "TDateTime")
  * Date and time format [ISO 8601](<ISO_8601.md> "ISO 8601")

---

_Source: [https://wiki.freepascal.org/RFC_1123_Time_Format](https://web.archive.org/web/20231002114103/https://wiki.freepascal.org/RFC_1123_Time_Format)_
