# Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_1/Punctuation_and_Indentation/bg> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_1/Punctuation_and_Indentation/de> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation/de")** │  **[English (en)](<../../../en/Basic_Pascal_Tutorial/Chapter_1/Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation")** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_1/Punctuation_and_Indentation/fr> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_1/Punctuation_and_Indentation/ja> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Chapter_1/Punctuation_and_Indentation/ko> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation/ko")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_1/Punctuation_and_Indentation/zh_CN> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation/zh CN")** │    
****

[ ◄ ](<Standard_Functions.md> "Basic Pascal Tutorial/Chapter 1/Standard Functions/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment/ru")  
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
      beta := b / a;
    end. (* main *)
    

В общем, делайте отступ для каждого блока. Пропускайте строку между блоками (например, между блоками **const** и **var**). Современные среды разработки (IDE - Integrated Development Environment) понимают синтаксис Pascal и часто сами будут делать отступы для вас во время набора текста. Вы можете настроить отступы по своему вкусу (количество пробелов на табуляцию). 

Правильные отступы позволяют гораздо легче разобраться, как работает код, чему также очень помогает разумное комментирование. 

[ ◄ ](<Standard_Functions.md> "Basic Pascal Tutorial/Chapter 1/Standard Functions/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Punctuation_and_Indentation/ru](https://web.archive.org/web/20250117032828/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Punctuation_and_Indentation/ru)_
