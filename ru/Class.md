# Class

│ **[Deutsch (de)](</Class/de> "Class/de")** │  **[English (en)](<../en/Class.md> "Class")** │  **[français (fr)](</Class/fr> "Class/fr")** │  **русский (ru)** │    
****

Класс является хорошо структурированным [типом данных](<Type.md> "Type/ru") в Object [Pascal](<../en/Pascal.md> "Pascal") и его диалектах (таких, как Delphi или [ObjFPC](</index.php?title=Mode_ObjFPC/ru&action=edit&redlink=1> "Mode ObjFPC/ru \(page does not exist\)")). Классы могут содержать [переменные](<Variable.md> "Variable/ru"), конструкторы, деструкторы, [функции](<Function.md> "Function/ru"), [процедуры](<Procedure.md> "Procedure/ru") и [свойства](</Property/ru> "Property/ru"). 

Также классы освобождают программиста от необходимости использовать указатели и ссылки. Они автоматически обрабатываются компилятором во время компиляции. 

Классы могут наследоваться от других классов или быть унаследованными в свою очередь. Любой класс, родительский класс которого не уточнен программистом, автоматически наследуется от TObject, так как он имеет необходимые компоненты для всех классов. Из-за зависимости TObject, в деструкторе любой подкласс должен иметь директиву override. Кроме того, любой из конструкторов вашего класса должен иметь в своем теле оператор inherited. Класс может иметь несколько конструкторов, но только один деструктор. 

Object Pascal не поддерживает множественное наследование: кроме неявного наследования от TObject классы могут иметь только один родительский класс. Полиморфизм реализован с помощью директив методов. Ниже представлен пример простого объявления класса; давайте разберем его. 
    
    
    type
      TMyClass = class
      private
        FSomeVar: integer;
      public
        constructor Create; overload;
        constructor Create(Args: array of integer); overload;
        destructor Destroy; override;
        function GetSomeVar: integer;
        procedure SetSomeVar(newvalue: integer);
      published
        property SomeVar: integer read GetSomeVar write SetSomeVar default 0;
      end;
    

Между [ключевыми словами](<Keyword.md> "Keyword/ru") **class** и **end** мы видим объявления членов - переменных и методов. Некоторым методам (функциям и/или процедурам) предшествуют модификаторы области видимости (**private** , **public** , **published**), за ними следуют директивы (**overload** , **override**), а также _странная штука_ под названием **property**. Давайте разберем их все. 

  


## Contents

  * 1 Пояснения о наследовании
  * 2 Модификаторы области видимости



## Пояснения о наследовании

В Object Pascal производные классы наследуют все члены базового класса, даже те, которые не перегружены с теми же именами. Например: 
    
    
    type
      // базовый класс
      MyClass = class
        procedure Proc1;   
      end;
    
      // производный класс
      YourClass = class(MyClass)  
        procedure Proc1; //такое имя процедуры, как и в классе MyClass
      end;
    
    var
      a: MyClass;
      b: YourClass;
    begin
      a := MyClass.Create;
      b := YourClass.Create;
      a.Proc1;          // использует процедуру класса MyClass 
      b.Proc1;          // использует процедуру класса YourClass
      MyClass(b).Proc1; // использует процедуру класса MyClass
    

  


## Модификаторы области видимости

Модификаторы области видимости сообщают компилятору кто может вызывать методы класса: 

  * **private** : член может быть вызван/доступен только с помощью методов данного класса;
  * **public** : член может быть вызван/доступен из любого другого места программы;
  * **protected** : член может быть вызван/доступен из других классов в том же модуле и из производных классов, но не из внешних классов.
  * **published** : переменная опубликована и будет доступна в Инспекторе объектов IDE.



Модификаторы области видимости не могут быть изменены в производных классах: члены будут сохранять их видимость (или её отсутствие) всегда и везде. 

  


Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | Class | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Class/ru](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/Class/ru)_
