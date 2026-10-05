# Basic Pascal Tutorial/Chapter 1/Standard Functions

│ **[English (en)](<../../../en/Basic_Pascal_Tutorial/Chapter_1/Standard_Functions.md>)** │  **русский (ru)** │

[ ◄ ](<Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation/ru")  
---|---|---  
  
Стандартные функции

1F - Standard Functions (author: Tao Yue, state: unchanged) 

  
Pascal имеет несколько стандартных математических функций, которые вы можете использовать. Например, для нахождения синуса угла Пи радиан: 
    
    
    value := sin (3.1415926535897932);
    

Обратите внимание, что функция **sin** работает с угловыми величинами, выраженными в [радианах](<https://ru.wikipedia.org/wiki/%D0%A0%D0%B0%D0%B4%D0%B8%D0%B0%D0%BD>), как это делают все тригонометрические функции.  
Если всё нормально, вы должны получить значение 0.0 

Функции вызываются путём указания имени функции и затем аргументов в скобках.  
Стандарнтые функции Pascal включают: 

Функция | Описание | Тип аргумента | Тип результата   
---|---|---|---  
abs | абсолютное значение | real или integer | как у аргумента   
arctan | арктангенс в радианах | real или integer | real   
cos | косинус угла, заданного в радианах | real или integer | real   
exp | **e** в заданной степени | real или integer | real   
ln | натуральный логарифм | real или integer | real   
round | округление до ближайшего целого | real | integer   
sin | синус угла, заданного в радианах | real или integer | real   
sqr | квадрат (возведение в степень 2) | real или integer | как у аргумента   
sqrt | квадратный корень (возведение в степень 1/2) | real или integer | real   
trunc | усечение (округление вниз) | real или integer | integer   
  
Для порядковых типов данных (integer или char), где разрешённые значения имеют явного предшественника и последователя, вы можете использовать следующие функции: 

Функция | Описание | Тип аргумента | Тип результата   
---|---|---|---  
chr | символ с заданным числовым значением ASCII | integer | char   
ord | порядковое значение | integer или char | integer   
pred | предшественник | integer или char | как у аргумента   
succ | последователь | integer или char | как у аргумента   
  
**Real** не является порядковым типом данных! Это потому, что он не имеет явного предшественника или последователя. Что является последователем для 56.0? Это 56.1, 56.01, 56.001 или 56.0001? 

Однако, для целого 56 имеется явный предшественник: 55 и явный последователь: 57. 

То-же самое справедливо и для символов: 
    
    
     'b'
     Последователь: 'c'
     Предшественник: 'a'
    

Вышеприведённый список не является исчерпывающим, поскольку современные компиляторы Pascal включают тысячи функций самого разного назначения. Для получения большей информации обратитесь к документации по вашему компилятору. 

[ ◄ ](<Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Standard_Functions/ru](https://web.archive.org/web/20250418094304/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Standard_Functions/ru)_
