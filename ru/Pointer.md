# Pointer

│ **[English (en)](<../en/Pointer.md>)** │  **русский (ru)** │

Тип **Pointer** в Free Pascal имеет два значения: 

  * термин или понятие для целого класса типов и переменных этих типов
  * просто тип данных, который может хранить адрес переменной любого другого типа



## Contents

  * 1 Общее представление
  * 2 Тип данных
  * 3 Предопределенные типы указателей



## Общее представление

**Pointer** представляет собой переменную, которая содержит адрес памяти локальной или глобальной переменной, а также адрес памяти сложного объекта, который был создан и сохранен в области динамической памяти программы, называемой "куча". Можно сказать, что это [ссылка](</index.php?title=Reference/ru&action=edit&redlink=1> "Reference/ru \(page does not exist\)") на значение/переменную или "указатель на" значение/переменную. Для некоторых типов данных указатель может быть объявлен с помощью оператора **^** перед названием этого типа. С помощью установки рекомендуемой директивы компилятора _{$TYPEDADDRESS ON}_ указатели могут хранить адреса переменных или значений только тех типов данных, на которые они были объявлены. 

Адрес переменной присваивается указателю с помощью оператора **[@](<@.md> "@/ru")**. Выделение памяти и присваивание адреса для значения/объекта, размещенного в куче осуществляется с помощью процедуры **New**. Чтение и запись данных по указателю, "ссылающегося на" переменную, возможно с помощью оператора разыменования **^** , который записывается сразу после указателя. 
    
    
    program test_pointer_wiki2;
    {$TYPEDADDRESS ON}
    var 
      a         : Integer = 100;
      b         : Real = 10.5;
      ptrToInt  : PInteger;  // PInteger - указатель на целое, объявленный в библиотеке RTL,
                             // который может ссылаться только на значение/переменную типа Integer
      ptrToInt2 : ^Integer;  // общий вид объявления указателя
    begin
      //ptrToInt := @b;      // возникнет ошибка компиляции с установленной директивой {$TYPEDADDRESS ON} 
      ptrToInt := @a;        
      //Writeln(ptrToInt);   // компилятор выдаст ошибку: Error: Can't read or write variables of this type
      Writeln(ptrToInt^);    // выведет 100
      New(ptrToInt2);        // выделение памяти в куче для типа Integer
      Writeln(ptrToInt2^);   // выведет случайное целое число
      ptrToInt2^ := 222;     // присваивание значения выделенной памяти целого типа
      Writeln(ptrToInt2^)    // выведет 222 
    end.
    

## Тип данных

Тип данных **Pointer** \- это просто тип, который может содержать адрес переменной или значения **любого** типа данных. В этом заключается противоположность между указателями типа Pointer и типизированными указателями на переменные (например указатели типа PInteger), которые могут ссылаться только на переменные или значения конкретных типов при использовании директивы компилятора {$TYPEDADDRESS ON}. Например: 
    
    
    {$TYPEDADRESS ON} 
    Var 
      a : Integer = 20;
      b : Real = 20.5;
      c : Boolean = true;
      ptr : Pointer;
    begin
      ptr := @a; // корректное хранение адреса переменной типа Integer 
      ptr := @b; // корректное хранение адреса переменной типа Real
      ptr := @c  // корректное хранение адреса переменной типа Boolean
    end.
    

Но, прежде чем использовать переменную типа Pointer она должна быть 

  1. приведена к типу значения или переменной, на которую она ссылается
  2. разыменована с помощью оператора **^**



Например: 
    
    
    Var 
      a      : Integer = 20;
      ptr    : Pointer;
    begin
      ptr := @a;          // корректное хранение адреса переменной типа Integer 
      //Writeln(ptr);     // некорректно - не будет скомпилировано
                          // Error: Can't read or write variables of this type
      //Writeln( ptr^);   // Error: Can't read or write variables of this type
                          // Недостаточно просто разыменовать переменную типа Pointer
      Writeln( PInteger(ptr)^); // После приведения типа и разыменования указателя: выведет 20
    end.
    

## Предопределенные типы указателей

В [модуле System](</index.php?title=System_unit/ru&action=edit&redlink=1> "System unit/ru \(page does not exist\)") стандартной [библиотеки RTL](<RTL.md> "RTL/ru") объявлен ряд указателей для встроенных типов переменных, включающий следующие типы: 
    
    
      PSmallInt           = ^Smallint;
      PShortInt           = ^Shortint;
      PInteger            = ^Integer;
      PByte               = ^Byte;
      PWord               = ^word;
      PDWord              = ^DWord;
      PLongWord           = ^LongWord;
      PLongint            = ^Longint;
      PCardinal           = ^Cardinal;
      PQWord              = ^QWord;
      PBoolean            = ^Boolean;
    

  
См. также: 

  * Использование [указателей](</index.php?title=Pointers/ru&action=edit&redlink=1> "Pointers/ru \(page does not exist\)")
  * <http://www.freepascal.org/docs-html/ref/refse15.html>

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | Pointer | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | [Array](<Array.md> "Array/ru") | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Pointer/ru](https://web.archive.org/web/20250529211914/https://wiki.freepascal.org/Pointer/ru)_
