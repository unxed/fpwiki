# Break

│ **русский (ru)** │

Зарезервированное слово **break** \- одна из [команд цикла](<../en/Loops.md> "Loops")  
Эта команда используется для выхода из цикла до его запланированного окончания.  
Команда **break** может использоваться только внутри циклов.  
  
Пример:  

    
    
    var
      intI: Integer;
      intA: Integer = 50;
    begin
      for intI := 20 to 200 do
      begin
          ...
          if intI = intA then break; // При выполнении условия цикл завершается
          ...
      end;
    end;
    

## См. также

  * [Reserved words](<../en/Reserved_words.md> "Reserved words")

---

_Source: [https://wiki.freepascal.org/Break/ru](https://web.archive.org/web/20250324062117/https://wiki.freepascal.org/Break/ru)_
