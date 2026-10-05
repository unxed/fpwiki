# TDateTime

│ **English (en)** │

TDateTime is stored as a double, the integer part representing days and the fractional part being fraction of a day. The integer part is the number of days that have passed since December 30, 1899, and can be a negative number. The fractional part reflects the fraction of a 24-hour day without regard to the sign of the TDateTime value, so care must be taken when computing negative TDateTime values with a fractional part.  
Methods exist in Sysutils (datetimeroutines) that allow reading, writing and calculating with TDateTime, converting to and from a number of formats.  
The following table displays examples of TDateTime values and their corresponding dates and times: 

Double Value | 'dd-mm-yyyy hh:nn:ss'   
---|---  
-1.75 | 29-12-1899 18:00:00   
-1.5 | 29-12-1899 12:00:00   
-1.25 | 29-12-1899 06:00:00   
0 | 30-12-1899 00:00:00   
1.25 | 31-12-1899 06:00:00   
2.25 | 01-01-1900 06:00:00   
25569 | 01-01-1970 00:00:00 (Unix Epoch)   
36152.5 | 23-12-1998 12:00:00 (FPC 1.0 Beta2)   
36526 | 01-01-2000 00:00:00   
44927 | 01-01-2023 00:00:00   
  
  


## Contents

  * 1 Get current time
  * 2 Adding and subtracting TDateTime
  * 3 Set Date
  * 4 Compare two TDateTime
  * 5 See also



## Get current time

[Unit](<Unit.md> "Unit") SysUtils [function](<Function.md> "Function") Now retrieves the current system date and time. 
    
    
     function Now : TDateTime;
    

## Adding and subtracting TDateTime

Unit DateUtils function DaysBetween tell number of whole days between two DateTime values. 
    
    
      function DaysBetween ( const ToDate, FromDate : TDateTime ) : Integer;
    

Unit DateUtils function WeeksBetween tell number of whole weeks between two DateTime values. 
    
    
      function WeeksBetween( const ToDate, FromDate : TDateTime ):Integer;
    

  

    
    
    program DateProject1;
    
    uses
      SysUtils,DateUtils;
    const
      DateFormatChars = 'dd"/"mm"/"yyyy';
    var
      DateTime1, DateTime2: TDateTime;
    begin
      DateTime1 := now;
      DateTime2 := DateTime1 + 4*7;       //28 days later
      WriteLn('Current date is '+ FormatDateTime( DateFormatChars, DateTime1 ));
      WriteLn('28 days later date is '+ FormatDateTime( DateFormatChars, DateTime2 ));
      WriteLn('Number of days is '+ ( DaysBetween( DateTime1 ,DateTime2)).ToString );
      WriteLn('Number of weeks is '+ ( WeeksBetween( DateTime1 ,DateTime2)).ToString );
      ReadLn;
    end.
    

## Set Date

Unit SysUtils function EncodeDate to set the date. 
    
    
      function EncodeDate ( const Year, Month, Day : Word ) : TDateTime;
    

## Compare two TDateTime
    
    
    program CompareTwoDateTime;
    
    uses
      SysUtils, DateUtils;
    var
       firstDate, secondDate: TDateTime;
    begin
      firstDate := EncodeDate(2000, 2, 29);
      secondDate := EncodeDate(2018, 8, 11);
      if DaysBetween( firstDate, secondDate) = 0
        then WriteLn('Both dates are same')
        else
          if firstDate < secondDate
            then WriteLn('First date is earlier')
            else WriteLn('First date is later');
      ReadLn;
    end.
    

## See also

  * Date and time format [ISO 8601](<ISO_8601.md> "ISO 8601")
  * [RFC 1123 Time Format](<RFC_1123_Time_Format.md> "RFC 1123 Time Format")
  * [datetimeroutines](<https://www.freepascal.org/docs-html/rtl/sysutils/datetimeroutines.html>) in the [`sysUtils` unit](<sysutils.md> "sysutils")

---

_Source: [https://wiki.freepascal.org/TDateTime](https://web.archive.org/web/20250215233717/https://wiki.freepascal.org/TDateTime)_
