# Infinite loop

│ **English (en)** │  [**suomi (fi)**](</Infinite_loop/fi> "Infinite loop/fi") │  [**français (fr)**](</Infinite_loop/fr> "Infinite loop/fr") │  [**русский (ru)**](<../ru/Infinite_loop.md> "Infinite loop/ru") │    
****

An **infinite loop** (also known as an endless loop or unproductive loop or a continuous loop) is a loop which never ends. Inside a loop, [statements](<statement.md> "statement") are repeated forever. 

There are two implementations of an infinite loop: 
    
    
    while true do
    begin
    	// loop body repeated forever
    end;
    
    
    
    repeat
    begin
    	// loop body repeated forever
    end
    until false;
    

## [`Break`](<Break.md> "Break") statement

“[`While`](<While.md> "While") [`true`](<True.md> "True") [`do`](<Do.md> "Do")” or “[`repeat`](<Repeat.md> "Repeat") [`until`](<Until.md> "Until") [`false`](<False.md> "False")” loops look infinite at first glance, but there is a way to escape the loop through the `break` statement. 
    
    
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
    

## Optimization

If you really need an infinite loop, it is better to use `repeat … until false;`, since it shifts all instructions of the body “less” to the right (at least, if there is more than one statement in the loop). 

## See also

  * [`true`](<True.md> "True")
  * [`false`](<False.md> "False")
  * [`repeat`](<Repeat.md> "Repeat") [`until`](<Until.md> "Until")
  * [`while`](<While.md> "While") [`do`](<Do.md> "Do")
  * [`break`](<Break.md> "Break")

---

_Source: [https://wiki.freepascal.org/Infinite_loop](https://web.archive.org/web/20200919230630/https://wiki.freepascal.org/Infinite_loop)_
