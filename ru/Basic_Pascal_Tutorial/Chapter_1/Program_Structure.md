# Basic Pascal Tutorial/Chapter 1/Program Structure

│ [**български (bg)**](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/bg> "Basic Pascal Tutorial/Chapter 1/Program Structure/bg") │  [**Deutsch (de)**](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/de> "Basic Pascal Tutorial/Chapter 1/Program Structure/de") │  [**English (en)**](<../../../en/Basic_Pascal_Tutorial/Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure") │  [**español (es)**](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/es> "Basic Pascal Tutorial/Chapter 1/Program Structure/es") │  [**français (fr)**](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/fr> "Basic Pascal Tutorial/Chapter 1/Program Structure/fr") │  [**italiano (it)**](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/it> "Basic Pascal Tutorial/Chapter 1/Program Structure/it") │  [**日本語 (ja)**](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/ja> "Basic Pascal Tutorial/Chapter 1/Program Structure/ja") │  [**한국어 (ko)**](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/ko> "Basic Pascal Tutorial/Chapter 1/Program Structure/ko") │  **русский (ru)** │  [**中文（中国大陆）‎ (zh_CN)**](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/zh_CN> "Basic Pascal Tutorial/Chapter 1/Program Structure/zh CN") │    
****

[ ◄ ](<../Hello,_World.md> "Basic Pascal Tutorial/Hello, World/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers/ru")  
---|---|---  
  
Структура программы

Basics 1A - Program Structure (author: Tao Yue, state: changed) 

  
Основная структура программы на Pascal имеет вид: 
    
    
    PROGRAM ProgramName (FileList);
    
    CONST
      (* Объявления констант *)
    
    TYPE
      (* Объявления типов *)
    
    VAR
      (* Объявления перемнных *)
    
    (* Определения подпрограмм *)
    
    BEGIN
      (* Исполняемые операторы *)
    END.
    

Использование зарезервированоого слова **PROGRAM** в Free Pascal не является обязательным. 

Элементы программы должны следовать в правильном порядке, хотя некоторые из них могут быть опущеы, если не нужны. Вот пример программы, которая ничего не делает, но имеет все необходимые элементы: 
    
    
    program DoNothing;
    begin
    end.
    

Комментарии - это части кода, которые не компилируются и не выполняются. Free Pascal поддерживает два типа комментариев: в свободной форме и строчный. Комментарии в свободной форме либо начинаются с **`(*`** и заканчиваются **`*)`** , либо, что используется чаще, начинается с **{** и завершается **}**. Вы не можете вкладывать комментарии: 
    
    
            (* (* *) *)
    

это даст ошибку, т.к. компилятор сопоставит первый **`(*`** с первым **`*)`** , игнорируюя второй **`(*`** , который находится между первым набором маркеров комментария. Второй **`*)`** останется без парного ему **`(*`**. Эта проблема с начальными/конечными маркерами комментария является одной из причин, почему многие языки используют строчную систему комментирования. 

Free Pascal также поддержиает **//** как строчный комментарий. Когда встречаются две наклонные черты вне строки в апострофах или комментария свободной формы, оставшаяся часть строки игнорируется. 

Turbo Pascal и большинство других современных компиляторов поддерживают комментарии свободной формы на основе фигурных скобок, такие, как **`{Комментарий}`**. Открывающая фигурная скобка означает начало блока комментариев, а закрывающая означает окончание блока комментариев. Комментарии на основе фигурных скобок также используются для директив компилятора. 

Комментирование делает ваш код более лёгким для понимания. Если вы пишете свой код без комментариев, то может случиться так, что через несколько недель, месяцев или лет вы вернётесь к программе, не имея никакой информации, почему что-то в программе было сделано именно таким образом. В частности, вы можете захотеть задокументировать основной дизайн вашей программы и вставлять комментарии в коде, когда вы отклоняетесь от этого дизайна по уважительным причинам. 

Кроме того, комментарии часто используются для исключения проблемного кода из работы без его удаления. Помните упоминавшееся ранее ограничение на вложенные комментарии? Просто так случилось, что фигурные скобки **{ }** имеют приоритет над скобками со звёздочками **`(* *)`**. Вы не получите ошибку, если сделаете так: 
    
    
    { (* Комментарий *) }
    

Пробельные символы (пробелы, табуляции, переводы строк) игнорируются компилятором Pascal, если они не находятся внутри символьной строки. Однако, чтобы сделать вашу программу читаемой для человека, вы должны делать отступы для операторов и располагать отдельные операторы на отдельной строке. Отступы часто являются выражением идивидуальности программиста, однако в совместных проектах обычно выбирают один общий стиль, чтобы позволить любому продолжить работу с той-же страницы. 

[ ◄ ](<../Hello,_World.md> "Basic Pascal Tutorial/Hello, World/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Program_Structure/ru](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Program_Structure/ru)_
