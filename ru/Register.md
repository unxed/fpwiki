# Register

│ **[Deutsch (de)](</Register/de> "Register/de")** │  **[English (en)](<../en/Register.md> "Register")** │  **русский (ru)** │    
****  
Вернуться к списку [ зарезервированных слов](<Reserved_words.md> "Reserved words/ru")   
  
Модификатор **register** относится к соглашениям о вызове внутренних и внешних подпрограмм.   
Модификатор **register** присутствует для совместимости с Delphi.   
Модификатор **register** поддерживается в компиляторе FPC начиная с версии 1.9.x.   
Модификатор **register** используется для передачи первых трех параметров в вызываемую функцию через регистры процессора.   
  
Пример №1:   

    
    
    function subTest: string; [register];
    begin
       subTest: = 'abc';
    end;
    

  
Пример №2:   

    
    
    function funcTest (strTestdaten: Pchar): LongWord; register; external 'Test.dll';

---

_Source: [https://wiki.freepascal.org/Register/ru](https://web.archive.org/web/20250323041214/https://wiki.freepascal.org/Register/ru)_
