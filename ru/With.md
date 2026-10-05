# With

│ **[Deutsch (de)](</With/de> "With/de")** │  **[English (en)](<../en/With.md> "With")** │  **[suomi (fi)](</With/fi> "With/fi")** │  **русский (ru)** │    
****

[Зарезервированное слово](<Reserved_word.md> "Reserved word/ru") **with** предназначено для сокращенного написания типа [запись (структура)](<Record.md> "Record/ru"). Оно используется совместно с ключевым словом [do](<Do.md> "Do/ru"). 

Пример: 
    
    
    // Объявление записи (структуры)
    type
      TreRecord = record
        strValue: string;
        intValue: integer;
        dblValue: double;
      end;
    
    var
       reRecord: TreRecord; // Объявляем переменную типа "запись"
    
    begin
      ...
    
      // стандартное обращение к полям записи:
      reRecord.strValue := 'Test';
      reRecord.intValue := 5;
      reRecord.dblValue := 4.2;
    
      // с использованием слова "with"
      with reRecord do
      begin
        strValue := 'Test';
        intValue := 5;
        dblValue := 4.2;
      end;
      ...
    end;

---

_Source: [https://wiki.freepascal.org/With/ru](https://web.archive.org/web/20240308084605/https://wiki.freepascal.org/With/ru)_
