# Punctuation and Indentation

│ **[English (en)](<../en/Punctuation_and_Indentation.md>)** │  **русский (ru)** │

[ ◄ ](<Standard_Functions.md> "Standard Functions/ru") | [ ▲ ](<Contents.md> "Contents/ru") | [ ► ](<Programming_Assignment.md> "Programming Assignment/ru")  
---|---|---  
  
Пунктуация и отступы

1G - Punctuation and Indentation (author: Tao Yue, state: unchanged) 

  
Поскольку Pascal игнорирует переводы строк и пробелы, пунктуация должна сообщить компилятору, где заканчивается оператор. 

Места, где вы должны иметь точку с запятой: 

  * Заголовок программы
  * Каждое объявление констант
  * Каждое объявление переменных
  * Каждое объявление типа (будет обсуждаться позже)
  * Почти все операторы



Последний оператор в блоке `BEGIN-END`, непосредственно предшествующий `END`, не требует точки с запятой. Однако, будет гораздо лучше поставить её, поскольку это избавит вас от необходимости добавлять её, если вдруг понадобится переместить оператор выше. 

Отступы не являются обязательными. Однако, они очень полезны для программиста, поскольку помогают сделать программу более понятной. Если вы захотите, вы можете иметь программу, которая выглядит так: 
    
    
    program Stupid; const a=5; b=385.3; var alpha,beta:real; begin 
    alpha := a + b; beta:= b / a end.

Но будет гораздо лучше, если она будет выглядеть так: 
    
    
    program NotAsStupid;
    
    const
      a = 5;
      b = 385.3;
    
    var
      alpha,
      beta : real;
    
    begin (* main *)
      alpha := a + b;
      beta := b / a
    end. (* main *)

В общем, делайте отступ для каждого блока. Пропускайте строку между блоками (например, между блоками **const** и **var**). Современные среды разработки (IDE - Integrated Development Environment) понимают синтаксис Pascal и часто сами будут делать отступы для вас во время набора текста. Вы можете настроить отступы по своему вкусу (количество пробелов на табуляцию). 

Правильные отступы позволяют гораздо легче разобраться, как работает код, чему также очень помогает разумное комментирование. 

[ ◄ ](<Standard_Functions.md> "Standard Functions/ru") | [ ▲ ](<Contents.md> "Contents/ru") | [ ► ](<Programming_Assignment.md> "Programming Assignment/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Punctuation_and_Indentation/ru](https://web.archive.org/web/20191018161642/https://wiki.freepascal.org/Punctuation_and_Indentation/ru)_
