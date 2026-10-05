# Array

│ **[Deutsch (de)](</Array/de> "Array/de")** │  **[English (en)](<../en/Array.md> "Array")** │  **[español (es)](</Array/es> "Array/es")** │  **[suomi (fi)](</Array/fi> "Array/fi")** │  **[français (fr)](</Array/fr> "Array/fr")** │  **[Bahasa Indonesia (id)](</Array/id> "Array/id")** │  **[日本語 (ja)](</Array/ja> "Array/ja")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Array/zh_CN> "Array/zh CN")** │    
****

Тип **array** (массив) представляет собой последовательность однотипных [переменных](<Variable.md> "Variable/ru"). Примерами могут служить массивы [символов](<Char.md> "Char/ru"), [целых](<Integer.md> "Integer/ru") и [вещественных](<Real.md> "Real/ru") чисел. Фактически, в массивах могут использоваться любые типы, включая определенные пользователем. Однако, элементы массива всегда являются однотипными. Элементы разных типов не могут быть сгруппированы в массив. Для этих целей необходимо использовать [записи](<Record.md> "Record/ru"). 

Массивы отражают идею таких математических понятий, как 

  * [векторы](</index.php?title=vector/ru&action=edit&redlink=1> "vector/ru \(page does not exist\)") (одномерные массивы) и
  * матрицы (двумерные массивы)



## Contents

  * 1 Статические массивы
    * 1.1 Одномерный массив
    * 1.2 Многомерный массив
  * 2 Динамические массивы
  * 3 Доступ к элементам массива
  * 4 Массивы литералов



## Статические массивы

Объявление статического массива выполняется аналогично, как для простых типов, но при этом вам необходимо указать размерность массива (количество элементов) в виде диапазона значений, а также тип элементов. 
    
    
    program
    ...
    var 
      variablename: array [startindex..endindex] of type;
    begin
      ...
    

Значение **startindex** должно быть меньше либо равно значению **endindex**. При этом оба значения должны быть целыми константами, либо целочисленными [константными](</index.php?title=Const/ru&action=edit&redlink=1> "Const/ru \(page does not exist\)") значениями. Допускается, если одно или оба значения будут отрицательными либо равными 0. 

### Одномерный массив

Пример одномерного массива: 
    
    
    type
      simple_integer_array = array [1..10] of integer;
     
    var
      Numbers: simple_integer_array;
    

### Многомерный массив

[Многомерные массивы](</index.php?title=Multidimensional_arrays/ru&action=edit&redlink=1> "Multidimensional arrays/ru \(page does not exist\)") объявляются аналогично одномерным, при этом каждая дополнительная размерность перечисляется через запятую (например [x..y,z..t] и т.д.) 

Пример многомерного массива: 
    
    
    type
      more_complex_array = array [0..5,1..3] of extended;
     
    var
      specialmatrix: more_complex_array;
    

  


## Динамические массивы

Если заранее точно не известно необходимое количество элементов массива, то можно воспользоваться [динамическим массивом](<Dynamic_array.md> "Dynamic array/ru"). В процессе выполнения программы размер динамического массива можно увеличивать или уменьшать. 

## Доступ к элементам массива

Для доступа к элементу массива вам необходимо указать индекс элемента в квадратных скобках ([]) после имени массива. После этого элемент массива можно использовать как обычную переменную. 
    
    
    Var
       my_array   : array[1..3] of Integer;
       my_matrix  : array[1..5,1..5] of Integer;
       some_value : Integer;
    ...
    begin
       my_array[2]    := a + 2;
       my_matrix[2,3] := some_value;
       ...
       some_value := my_array[2];
       some_value := my_matrix[4,3];
    end.
    

## Массивы литералов

Существует два способа, использующихся для массивов литералов, в зависимости от места их размещения. Вы можете проинициализировать статический массив в секции объявления переменных (для [динамических массивов](<Dynamic_array.md> "Dynamic array/ru") это сделать невозможно) с помощью последовательности значений, заключенных в _круглые скобки_. В блоке инструкций вы можете создать анонимный массив с помощью ряда значений, заключенных в _квадратные скобки_. Например: 
    
    
    Var
       // инициализация статического массива типа integer с помощью массива литералов
       Numbers : array [1..3] of Integer = (1, 2, 3);
       
    procedure PrintArray(input : array of String);
    var 
       i : integer;
    begin
        for i := 1 to length(input) do
           write(input[i - 1],' ');
        writeln;
    end;
    
    begin
        Writeln( Numbers[2] );
        // создаем три элемента анонимного массива типа string с помощью массива литералов
        PrintArray( ['one', 'two', 'three'] );
    end.
    

На экран будет выведено:  
**2**  
**one two three**

Типы данных   
---  
Простые типы  | [Boolean](<Boolean.md> "Boolean/ru") | [Byte](<Byte.md> "Byte/ru") | [Cardinal](<Cardinal.md> "Cardinal/ru") | [Char](<Char.md> "Char/ru") | [Currency](<Currency.md> "Currency/ru") | [Extended](<Extended.md> "Extended/ru") | [Int64](<Int64.md> "Int64/ru") | [Integer](<Integer.md> "Integer/ru") | [Longint](<Longint.md> "Longint/ru") | [Pointer](<Pointer.md> "Pointer/ru") | [Real](<Real.md> "Real/ru") | [Shortint](<Shortint.md> "Shortint/ru") | [Smallint](<Smallint.md> "Smallint/ru") | [Word](<Word.md> "Word/ru")  
Сложные типы  | Array | [Class](<Class.md> "Class/ru") | [Record](<Record.md> "Record/ru") | [Set](<Set.md> "Set/ru") | [String](<String.md> "String/ru") | [Shortstring](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Array/ru](https://web.archive.org/web/20250428201735/https://wiki.freepascal.org/Array/ru)_
