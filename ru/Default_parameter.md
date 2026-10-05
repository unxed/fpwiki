# Default parameter

│ **[English (en)](<../en/Default_parameter.md>)** │  **русский (ru)** │

Параметры по умолчанию (также называемые необязательными аргументами или аргументами по умолчанию) являются параметрами [функции](<Function.md> "Function/ru") или [процедуры](<Procedure.md> "Procedure/ru"), которые содержат значения по умолчанию. Если программист не указал значение для такого параметра, то будет использоваться значение по умолчанию. В случае, если программист указал значение параметра, то использоваться будет указанное программистом значение. 

Программист может расширить существующую функцию или процедуру путем добавления некоторого количества параметров, которые будут иметь значения по умолчанию, вместо написания аналогичной функции или процедуры с различным количеством параметров. 

_Будьте осторожны_ : если хоть один параметр определен как необязательный, то все последующие параметры тоже будут необязательными. Обязательный параметр _не должен_ идти после необязательного. 

## Пример `deadlineDay`
    
    
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
    

Необязательный параметр используется вместо отсутствующего завершающего параметра в вызове 

  * при вызове функции `deadlineDay(5)` фактически вызывается `deadlineDay(5, 0)`
  * при вызове функции `deadlineDay(5, 1)` происходит непосредственный вызов `deadlineDay(5, 1)`

---

_Source: [https://wiki.freepascal.org/Default_parameter/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Default_parameter/ru)_
