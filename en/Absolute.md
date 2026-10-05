# Absolute

│ **English (en)** │  **[русский (ru)](<../ru/Absolute.md>)** │

The `absolute` [modifier](<modifier.md> "modifier") causes a [variable](<Variable.md> "Variable") to be stored at the same memory location as another variable. 

  

    
    
    // Example on little endian x64 processor
    Uses SysUtils;
    
    Var
        anInt      : Integer;
        anotherInt : Integer absolute anInt;
        firstByte  : Byte    absolute anInt;
     
    begin
        // with both Integer variables at the same memory location, a change to one is reflected
        // in the other
        anInt := 20;
    
        WriteLn(IntToStr(anInt) + '  ' + IntToStr(anotherInt)); // Outputs: 20  20
    
        // a value of 20 fits in the first byte:
    
        WriteLn('firstByte: ' + IntToStr(firstByte));           // Outputs: firstByte: 20
       
        anotherInt := 333;
    
        WriteLn(IntToStr(anInt) + '  ' + IntToStr(anotherInt)); // Outputs: 333 333
    
        // 333 is too large a value to fit in one byte
        // little-endian x64 - least significant byte is first in memory:
        // 333 = 101001101 =  01001101 00000001 in memory = 0x4D 0x01 = decimal: 77 1
    
        WriteLn('firstByte: ' + IntToStr(firstByte));           // Outputs: firstByte: 77
    end.

---

_Source: [https://wiki.freepascal.org/Absolute](https://web.archive.org/web/20250315111058/https://wiki.freepascal.org/Absolute)_
