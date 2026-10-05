# Record

│ **[Deutsch (de)](</Record/de> "Record/de")** │  **[English (en)](<../en/Record.md> "Record")** │  **[español (es)](</Record/es> "Record/es")** │  **[suomi (fi)](</Record/fi> "Record/fi")** │  **[français (fr)](</Record/fr> "Record/fr")** │  **[magyar (hu)](</Record/hu> "Record/hu")** │  **[polski (pl)](</Record/pl> "Record/pl")** │  **[português (pt)](</Record/pt> "Record/pt")** │  **русский (ru)** │    
****

Структурный [тип](<Type.md> "Type/ru") данных в [Pascal](<../en/Pascal.md> "Pascal") . 

В то время как простые структуры данных, такие как [массивы](<Array.md> "Array/ru") или [множества](<Set.md> "Set/ru") состоят из элементов одного типа, запись (**record**) может состоять из нескольких элементов различных типов. Каждая отдельная часть записи называется полем. 

Примеры записей: 
    
    
    type
      ExampleRecord = record
        Values: array[1..200] of Real;
        NumValues: Integer; 
        Average: Real;
      end;
    
      Member = record
        Firstname, Surname: string;
        Address: array[1..3] of string;
        Phone: Integer;
        Birthdate: TDateTime;
        PaidCurrentSubscription: Boolean;
      end;
    

Запись обрабатывается программой как одно целое, и записи могут быть скопированы таким образом: 
    
    
    var 
      a, b: Member;
    begin
      { Заполнение полей записи a }
      ...
      ...
      b := a;
    end;
    

Доступ к полям записи осуществляется через точку после имени переменной. Например: 
    
    
      a.firstname := 'George';
      a.surname := 'Petersen';
      a.phone := 789534;
      a.PaidCurrentSubscription := True;
    

C использованием ключевого слова with, к полям записи можно обращаться без указания переменной каждый раз с названием поля. 
    
    
      with a do
      begin
        firstname := 'George';
        surname := 'Petersen';
        phone := 789534;
        PaidCurrentSubscription := True;
      end;

---

_Source: [https://wiki.freepascal.org/Record/ru](https://web.archive.org/web/20250113131406/https://wiki.freepascal.org/Record/ru)_
