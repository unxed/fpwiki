# Constructor

│ **[Deutsch (de)](</Constructor/de> "Constructor/de")** │  **[English (en)](<../en/Constructor.md> "Constructor")** │  **[español (es)](</Constructor/es> "Constructor/es")** │  **[suomi (fi)](</Constructor/fi> "Constructor/fi")** │  **русский (ru)** │    
****

[Зарезервированное слово](<Reserved_word.md> "Reserved word/ru") **constructor** относится к [объектно-ориентированному программированию](</index.php?title=object-oriented_programming/ru&action=edit&redlink=1> "object-oriented programming/ru \(page does not exist\)"). Оно является [методом](<Method.md> "Method/ru") для создания [класса](<Class.md> "Class/ru"), который создает объект класса. 

Пример: 
    
    
    // определение класса
    type
      TKlasse = class
      end;
    
    var
      // объявление переменной типа класса
      clsKlasse: TKlasse;
    
    begin
      ...
      // создание класса
      clsKlasse := TKlasse.Create; 
      ...
    end;
    

## См. также

  * [Destructor](</index.php?title=Destructor/ru&action=edit&redlink=1> "Destructor/ru \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Constructor/ru](https://web.archive.org/web/20250117050358/https://wiki.freepascal.org/Constructor/ru)_
