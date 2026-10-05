# Basic Pascal Tutorial/Chapter 2/Input

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_2/Input/bg> "Basic Pascal Tutorial/Chapter 2/Input/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_2/Input/de> "Basic Pascal Tutorial/Chapter 2/Input/de")** │  **[English (en)](<../../../en/Basic_Pascal_Tutorial/Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input")** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_2/Input/fr> "Basic Pascal Tutorial/Chapter 2/Input/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_2/Input/ja> "Basic Pascal Tutorial/Chapter 2/Input/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Chapter_2/Input/ko> "Basic Pascal Tutorial/Chapter 2/Input/ko")** │  **русский (ru)** │  **[中文（中国大陆）‎ (zh_CN)](</Basic_Pascal_Tutorial/Chapter_2/Input/zh_CN> "Basic Pascal Tutorial/Chapter 2/Input/zh CN")** │    
****

[ ◄ ](<../Chapter_1/Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Output.md> "Basic Pascal Tutorial/Chapter 2/Output/ru")  
---|---|---  
  
Ввод

2A - Input (author: Tao Yue, state: unchanged) 

  
Ввод - это то, что поступает в программу. Ввод может осуществляться с помощью клавиатуры, мыши, файла на диске, сканера, джойстика и т.д. 

Мы не будем рассматривать ввод с помощью мыши, поскольку его синтаксис отличается от машины к машине. Кроме того, сегодняшние событийно-ориентированные, использующие окна операционные системы обычно обрабатывают ввод мышью для вас. 

Основной формат для считывания данных: 
    
    
    read (Variable_List);
    

`Variable_List` \- последовательность идентификаторов переменных, разделяемых запятыми. 

`read` интерпретирует ввод как поток символов, строки которых разделены специальным символом конца строки. С другой стороны, `readln` будет перескакивать на следующую строку после чтения значения, автоматически перемещаясь за символ конца строки. 
    
    
    readln (Variable_List);
    

Предположим, вы имеете следующий ввод от пользователя, а переменные `a, b, c, d` \- целые. 
    
    
    45 97 3
    1 2 3
    

Ниже приведено несколько примеров `read` и `readln` вместе со значениями, считанными в соответствующие переменные. 

Statement(s) | a | b | c | d   
---|---|---|---|---  
read (a); | 45 | 97 |  |   
read (b);   
readln (a); | 45 | 1 |  |   
read (b);   
read (a, b, c, d); | 45 | 97 | 3 | 1   
readln (a, b); | 45 | 97 | 1 | 2   
readln (c, d);   
  
Когда читаются целые, все пробелы пропускаются до тех пор, пока не будет найдена цифра. Тогда считываются все последующие цифры, пока не будет встречен нецифровой символ (включая пробел, но не ограничиваясь им). 
    
    
    8352.38
    

Когда целое читается из указанного выше ввода, его значение становится `8352`. Если сразу после этого вы читаете символ, значение будет '`.`', поскольку читающая головка остановилась на первом алфавитно-цифровом символе. 

Предположим, вы пытались прочитать два целых. Это не будет работать, поскольку, когда компьютер ищет данные для заполнения второй переменной, он видит '`.`' и останавливается, т.к. не может найти какие-либо данные для чтения. 

С вещественными значениями, компьютер также пропускает пробелы, а потом считывает столько, сколько может быть считано. Однако, многие компиляторы Pascal накладывают одно дополнительное ограничение: вещественное, которое не имеет целой части, должно начинаться с `0.` Таким образом, `.678` является недопустимым и компьютер не может прочитать это в вещественное, но `0.678` \- нормально. 

Убедитесь, что все идентификаторы в списке аргументов ссылаются на переменные! Ни константам, ни литералам значения присвоены быть не могут. 

[ ◄ ](<../Chapter_1/Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Output.md> "Basic Pascal Tutorial/Chapter 2/Output/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_2/Input/ru](https://web.archive.org/web/20240526183010/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_2/Input/ru)_
