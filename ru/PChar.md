# PChar

│ **[English (en)](<../en/PChar.md>)** │  **русский (ru)** │

**PChar** является [типом данных](<Data_type.md> "Data type/ru") и [указателем](<Pointer.md> "Pointer/ru") на строку с завершающим нулевым символом. Наиболее важным применением PChar является взаимодействие с системными библиотеками, такими как dll. 

Пример использования в Messagebox: 
    
    
    var 
      s: String;
    begin
      s := 'Test';
      Application.MessageBox( PChar(s)),'Title', MB_OK );
    end;
    

Объявление: 
    
    
    var 
      p: PChar;
    

Правильные присваивания: 
    
    
       p := 'Это строка с нулевым завершающим символом.';
       p := IntToStr(45);
    

Неправильные присваивания: 
    
    
       p := 45;
    

Как и следовало ожидать, значение типа [integer](<Integer.md> "Integer/ru") не может быть преобразовано в тип PChar. 

## См. также

  * [Символьные и строковые типы](<Character_and_string_types.md> "Character and string types/ru")

---

_Source: [https://wiki.freepascal.org/PChar/ru](https://web.archive.org/web/20230701000000/https://wiki.freepascal.org/PChar/ru)_
