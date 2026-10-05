# On

│ **[Deutsch (de)](</On/de> "On/de")** │  **English (en)** │  **[suomi (fi)](</On/fi> "On/fi")** │    
****

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The `on` [keyword](<Keyword.md> "Keyword") to act on the [exception](<Exceptions.md> "Exceptions") type. The `on` clause checks against one of a number of exception classes. 
    
    
    try
        //guarded block of code
        // ...
    except
       on {exception type} do begin
         //exception block-handles SomeException
       end;
    end;
    

  

    
    
    program range_error;
    {$mode objfpc} {$H+}
    {$R+} // Range check on
    uses sysutils;
    var
      i,j:integer;
      arr:array[0..9] of integer;
    begin
      try
        i := 0;
        j := 10;
        while i < j do
          begin
            inc(i);
            arr[i] := i + j;
            WriteLn( i,'  ', arr[i]);
          end;
      except
        on E:ERangeError do begin
          WriteLn('Error: valid range exceeded detected.');
          WriteLn(E.Message);
        end;
        on E:Exception do  // generic handler
          WriteLn('Caught ' + E.ClassName + ': ' + E.Message);
      end;
      WriteLn('Press any key');
      ReadLn;
    end.
    

## See also

  * [Try](<Try.md> "Try")
  * [Except](<Except.md> "Except")

---

_Source: [https://wiki.freepascal.org/On](https://web.archive.org/web/20240920204107/https://wiki.freepascal.org/On)_
