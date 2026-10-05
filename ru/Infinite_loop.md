# Infinite loop

│ **[English (en)](<../en/Infinite_loop.md> "Infinite loop")** │  **[suomi (fi)](</Infinite_loop/fi> "Infinite loop/fi")** │  **[français (fr)](</Infinite_loop/fr> "Infinite loop/fr")** │  **русский (ru)** │    
****

Бесконечный цикл (также известный как непродуктивный или непрерывный цикл) - это цикл, который никогда не заканчивается. Операторы внутри цикла всегда повторяются. 

  

    
    
     while true do
       begin
       end;
    

  

    
    
     repeat
     until false;
    

  


## Оператор [Break](<Break.md> "Break/ru")

Циклы "[While](<While.md> "While/ru") [True](<True.md> "True/ru") [Do](<Do.md> "Do/ru")" или "[Repeat](<Repeat.md> "Repeat/ru") [Until](<Until.md> "Until/ru") [False](<False.md> "False/ru")" на первый взгляд кажутся бесконечными, но при этом возможен выход из цикла с помощью оператора [Break](<Break.md> "Break/ru"). 

  

    
    
    var
      i:integer;
    begin
      i := 0;
      while true do
        begin
          i := i + 1;
          if i = 100 then break;
        end;
    end;
    
    
    
    var
      i:integer;
    begin
      i := 0;
      repeat
        i := i + 1;
        if i = 100 then break;
      until false;
    end;
    

## См. также

  * [True](<True.md> "True/ru")
  * [False](<False.md> "False/ru")
  * [Repeat](<Repeat.md> "Repeat/ru") [Until](<Until.md> "Until/ru")
  * [While](<While.md> "While/ru") [Do](<Do.md> "Do/ru")
  * [Break](<Break.md> "Break/ru")

---

_Source: [https://wiki.freepascal.org/Infinite_loop/ru](https://web.archive.org/web/20250515032728/https://wiki.freepascal.org/Infinite_loop/ru)_
