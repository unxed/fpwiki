# Absolute

│ **[Deutsch (de)](</Absolute/de> "Absolute/de")** │  **[English (en)](<../en/Absolute.md> "Absolute")** │  **[español (es)](</Absolute/es> "Absolute/es")** │  **[suomi (fi)](</Absolute/fi> "Absolute/fi")** │  **[français (fr)](</Absolute/fr> "Absolute/fr")** │  **русский (ru)** │    
****

Модификатор **absolute** предписывает хранить данную переменную в той же области памяти, в которой хранится и другая переменная. 

  

    
    
    // Пример для процессоров x64 с порядком расположения байт от младшего к старшему (little endian)
    Uses SysUtils;
    
    Var
        anInt      : Integer;
        anotherInt : Integer absolute anInt;
        firstByte  : Byte    absolute anInt;
     
    begin
        // обе целочисленные переменные расположены в одной области памяти, при этом изменение одной из них приводит к изменению другой
        anInt := 20;
    
        WriteLn(IntToStr(anInt) + '  ' + IntToStr(anotherInt)); // Выводится: 20  20
    
        // значение 20 соответствует первому байту:
    
        WriteLn('firstByte: ' + IntToStr(firstByte));           // Выводится: firstByte: 20
       
        anotherInt := 333;
    
        WriteLn(IntToStr(anInt) + '  ' + IntToStr(anotherInt)); // Выводится: 333 333
    
        // 333 слишком большое значение, чтобы уместиться в одном байте
        // little-endian x64 - младший байт идет первым в памяти (по меньшему адресу):
        // 333 = 101001101 =  01001101 00000001 в памяти выглядит как = 0x4D 0x01 = десятичное значение: 77 1
    
        WriteLn('firstByte: ' + IntToStr(firstByte));           // Выводится: firstByte: 77
    end.

---

_Source: [https://wiki.freepascal.org/Absolute/ru](https://web.archive.org/web/20250117044742/https://wiki.freepascal.org/Absolute/ru)_
