# Default parameter

│ **English (en)** │  **[русский (ru)](<../ru/Default_parameter.md>)** │

A default parameter also referred to as optional argument (or default argument) is a [function](<Function.md> "Function") or [procedure](<Procedure.md> "Procedure") parameter that has a default value provided to it. If the programmer does not supply a value for this parameter, the default value will be used. If the programmer does supply a value for the default parameter, the programmer-supplied value is used. 

The programmer can extend an existing function or procedure by adding some parameters that have default value instead of writing an identical method with different parameters. 

Beware: Once in a parameter list an optional parameter is defined, all following parameters have to be optional, too. A mandatory parameter must not have a later position than an optional parameter. 

## Example `deadlineDay`
    
    
    {$mode objfpc}
    
    uses SysUtils, DateUtils;
    
    function deadlineDay(day: integer; month: integer = 0): TDateTime;
    var
      n: TDateTime;
      y, m, d: word;
    begin
      n := now();
      DecodeDate(n, y, m, d);
      if month = 0 then
      begin
        if d > day then
        begin
          n := IncMonth(n);
          DecodeDate(n, y, m, d);
        end;
      end
      else
      begin
        if month < m then
        begin
           n := IncYear(n);
        end;
        decodeDate(n, y, m, d);
        m := month;
      end;
      d := day;
      result := EncodeDate(y, m, d);
    end;
    

Default parameter is used in place of the missing trailing parameter in a call 

  * in program call `deadlineDay(5)` effectively calls `deadlineDay(5, 0)`
  * in program call `deadlineDay(5, 1)` does indeed call `deadlineDay(5, 1)`

---

_Source: [https://wiki.freepascal.org/Default_parameter](https://web.archive.org/web/20240920204137/https://wiki.freepascal.org/Default_parameter)_
