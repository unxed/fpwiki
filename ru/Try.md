# Try

│ **русский (ru)** │

**try** является частью либо блока try..[finally](</index.php?title=Finally/ru&action=edit&redlink=1> "Finally/ru \(page does not exist\)"), либо блока try..[except](</index.php?title=Except/ru&action=edit&redlink=1> "Except/ru \(page does not exist\)"). 

Если [исключение](</index.php?title=exception/ru&action=edit&redlink=1> "exception/ru \(page does not exist\)") происходит во время выполнения кода между **try** и **finally** , выполнение продолжается за **finally**. Если исключение происходит в коде между **finally** и **end** , то выполнение также продолжится до **end**. 
    
    
    try
      // код, который может сгенерировать исключение
    finally 
      // всегда будет выполняться в качестве завершающих операторов
    end;
    

Всякий раз, когда происходит [исключение](</index.php?title=Exception/ru&action=edit&redlink=1> "Exception/ru \(page does not exist\)"), код между **except** и **end** будет выполнен. 
    
    
    try
      // код, который может сгенерировать исключение
    except
      // будет выполнен только в том случае, если произойдет исключение
      on E: EDatabaseError do
        ShowMessage( 'Database error: '+ E.ClassName + #13#10 + E.Message );
      on E: Exception do
        ShowMessage( 'Error: '+ E.ClassName + #13#10 + E.Message );
    end;
    

## См. также

  * [raise](</index.php?title=Raise/ru&action=edit&redlink=1> "Raise/ru \(page does not exist\)")
  * [Обработка исключений](</index.php?title=Exception_handling/ru&action=edit&redlink=1> "Exception handling/ru \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Try/ru](https://web.archive.org/web/20230205051413/https://wiki.freepascal.org/Try/ru)_
