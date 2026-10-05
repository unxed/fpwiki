# How to use nullable types

│ **English (en)** │

  
Here is a small example of how to use the [nullable types](<Nullable_types.md> "Nullable types") in [Delphi mode ( `{$mode Delphi}` )](<Mode_Delphi.md> "Mode Delphi"). The [unit's](<Unit.md> "Unit") nullable [source code](<Source_code.md> "Source code") is referenced on the [nullable types](<Nullable_types.md> "Nullable types") wiki page. 
    
    
    program temperatureproject;
    {$mode delphi}{$H+}
    uses Nullable, SysUtils;
    
    type
    
    NullableFloat = TNullable<single>;
    
    var
      weekDayTemperature : array[1..7] of  NullableFloat;
      max, min :NullableFloat;
      i, values,code:integer;
      sum, temp:single;
      s:string;
    begin
      for i:= 1 to 7 do  weekDayTemperature[i].Clear;
      min.Clear;
      max.Clear;
      // ...
      for i:= 1 to 7 do
        begin
          writeln('Enter the temperature on '+ DefaultFormatSettings.LongDayNames[i] );
          readln(s);
          Val (s,temp,code);
          if code = 0 then weekDayTemperature[i].SetValue(temp);
        end;
      // ...
      sum := 0;
      values := 0;
      for i:= 1 to 7 do
         begin
           if weekDayTemperature[i].HasValue then
              begin
                if min.HasValue then
                  begin
                    if min.GetValue > weekDayTemperature[i].GetValue
                      then min.SetValue( weekDayTemperature[i].GetValue);
                  end
                    else  min.SetValue( weekDayTemperature[i].GetValue);
                if max.HasValue then
                  begin
                    if max.GetValue < weekDayTemperature[i].GetValue
                      then max.SetValue( weekDayTemperature[i].GetValue);
                  end
                  else max.SetValue( weekDayTemperature[i].GetValue);
                inc(values);
                sum := sum + weekDayTemperature[i].GetValue;
              end;
         end;
      if min.HasValue then
        begin
          writeln('The average week temperature was ',sum/values:0:1,' degrees.');
          writeln('The minimum temperature was ',min.GetValue:0:1,' degrees.');
          writeln('The maximum temperature was ',max.GetValue:0:1,' degrees.');
        end;
      //...
      writeln('Press any key to continue!');
      readln;
    end.
    

## See also

  * [Val](<Val.md> "Val")
  * [For](<For.md> "For")

---

_Source: [https://wiki.freepascal.org/How_to_use_nullable_types](https://web.archive.org/web/20250401011212/https://wiki.freepascal.org/How_to_use_nullable_types)_
