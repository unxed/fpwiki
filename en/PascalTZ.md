# PascalTZ

│ **English (en)** │  **[русский (ru)](<../ru/PascalTZ.md> "PascalTZ/ru")** │ 

## Contents

  * 1 About
  * 2 Example
  * 3 Authors
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Bug Reports
  * 8 See also



### About

PascalTZ stands for "Pascal Time Zone". It allows you to convert between local times in various [time zones](<http://en.wikipedia.org/wiki/Time_zone>) and [GMT](<http://en.wikipedia.org/wiki/Gmt>)/[UTC](<http://en.wikipedia.org/wiki/Coordinated_Universal_Time>), taking into account historical changes to time zone rules. PascalTZ uses the [Time Zone Database](<https://www.iana.org/time-zones>) (often called `tz` or `zoneinfo`) to determine how to correctly adjust time for various time zones. The correctness of time zone conversions in future relies on using an up to date database. Beware, time zone rules may be changed by governments around the world, sometimes with a very short notice. 

PascalTZ component can be used in pure FPC projects or installed as a design and runtime package in Lazarus IDE. It also comes with a testing framework, a collection of time zone conversion test vectors and test cases for internal functions. 

More information can be found at [GitHub:PascalTZ](<https://github.com/dezlov/PascalTZ>). 

### Example
    
    
    uses
      SysUtils, DateUtils, uPascalTZ;
    
    var
      PascalTZ: TPascalTZ;
      DateTime: TDateTime;
    
    begin
      PascalTZ := TPascalTZ.Create;
    
      // Load time zone database from "tzdata" directory
      // Download from: https://www.iana.org/time-zones
      PascalTZ.DatabasePath := 'tzdata';
    
      // Current local and UTC time
      DateTime := Now;
      WriteLn('Local time: ', DateTimeToStr(DateTime));
      DateTime := LocalTimeToUniversal(DateTime);
      WriteLn('UTC time: ', DateTimeToStr(DateTime));
    
      // Convert current time to Paris time
      DateTime := PascalTZ.GMTToLocalTime(DateTime, 'Europe/Paris');
      WriteLn('Time in Paris: ', DateTimeToStr(DateTime));
    
      // Convert Paris time to Chicago time
      DateTime := PascalTZ.Convert(DateTime, 'Europe/Paris', 'America/Chicago');
      WriteLn('Time in Chicago: ', DateTimeToStr(DateTime));
    
      // Check if a time zone exists
      WriteLn('Africa/Lagos exists? ', PascalTZ.TimeZoneExists('Africa/Lagos'));
      WriteLn('Australia/Darwin exists? ', PascalTZ.TimeZoneExists('Australia/Darwin'));
    
      PascalTZ.Free;
    end.
    

### Authors

This library was originally published by [José Mejuto](</index.php?title=User:Joshy&action=edit&redlink=1> "User:Joshy \(page does not exist\)") in 2009 and is maintained by [Denis Kozlov](</index.php?title=User:Dezlov&action=edit&redlink=1> "User:Dezlov \(page does not exist\)") since 2015. 

### License

[Modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). 

### Download

[GitHub:PascalTZ releases](<https://github.com/dezlov/PascalTZ/releases>)

### Change Log

  * Version 1.0 (2009-11-10) [[1]](<https://github.com/dezlov/PascalTZ/blob/v2.0/CHANGELOG.md>)
  * Version 2.0 (2016-07-19) [[2]](<https://github.com/dezlov/PascalTZ/blob/v2.0/CHANGELOG.md>)



### Bug Reports

Bug reports and suggestions can be logged at [GitHub:PascalTZ issues](<https://github.com/dezlov/PascalTZ/issues>). 

### See also

Since 2.6.2, FPC has the functions `LocalTimeToUniversal` and `UniversalTimeToLocal` in the _dateutils_ unit to convert between local time and UTC time. These functions can be useful and an alternative for PascalTZ if you are not interested in historical/future date/times (i.e. the functions use current daylight saving time etc to convert to/from UTC time). These functions do allow for specifying manual offsets against UTC, but then you would need to keep track of those offsets - which PascalTZ does for you.

---

_Source: [https://wiki.freepascal.org/PascalTZ](https://web.archive.org/web/20250417141954/https://wiki.freepascal.org/PascalTZ)_
