# Dynamic array

│ **[English (en)](<../en/Dynamic_array.md> "Dynamic array")** │  **[español (es)](</Dynamic_array/es> "Dynamic array/es")** │  **[suomi (fi)](</Dynamic_array/fi> "Dynamic array/fi")** │  **[français (fr)](</Dynamic_array/fr> "Dynamic array/fr")** │  **[日本語 (ja)](</Dynamic_array/ja> "Dynamic array/ja")** │  **русский (ru)** │    
****

Динамический массив - это [массив](<Array.md> "Array/ru"), размеры которого неизвестны во [время компиляции](<../en/Compile_time.md> "Compile time"). Динамический массив не является единственным типом, предоставляющим массивы переменной длины, но с 2018 года он является единственным, который поддерживает [FPC](<FPC.md> "FPC/ru"). 

## Contents

  * 1 Использование
    * 1.1 Концепция
    * 1.2 Определение
    * 1.3 Задание размерности
    * 1.4 Инициализация
    * 1.5 Обращение
  * 2 Приложение
  * 3 См. также



## Использование

### Концепция

Определение динамического массива будет выделять пространство только для [указателя](<Pointer.md> "Pointer/ru"). Во [время выполнения](<../en/runtime.md> "runtime") различные подпрограммы будут обеспечивать удобное использование, но что более важно, синтаксис доступа к элементам массива путем помещения индексов в квадратные скобки поддерживается [компилятором](<../en/Compiler.md> "Compiler") (реализован как автоматическое разыменование [указателя](<Pointer.md> "Pointer/ru")). 

Индексы динамических массивов всегда являются неотрицательными целыми числами, начинающимися с нуля для первого элемента. Невозможно использовать перечислительный тип, любой другой порядковый тип в качестве индекса или изменить первый элемент, заданный индексом `1`. 

### Определение

Одномерный динамический массив определяется так: 
    
    
    array of char
    

Обратите внимание, что размерность [массива] не указан. 

Для определения многомерного массива в качестве базового типа указывается сам массив. 
    
    
    array of array of longInt
    

### Задание размерности

[Процедура](<Procedure.md> "Procedure/ru") компилятора [`setLength`](<https://www.freepascal.org/docs-html/rtl/system/setlength.html>) изменит длину динамического массива при условии, что памяти достаточно. 
    
    
    program setLengthDemo(input, output, stdErr);
    var
    	sieve: array of longWord;
    begin
    	setLength(sieve, 1337);
    end.
    

Процедура выделяет память для стольких записей базового типа, сколько указано, плюс некоторые данные для управления. Затем она [_copies_](<https://www.freepascal.org/docs-html/rtl/system/copyarray.html>) (копирует) все элементы старого массива в новый. Новые поля, которых раньше не было, инициализируются по умолчанию с помощью встроенного механизма. 

Размер многомерных массивов также можно изменить с помощью `setLength`. 
    
    
    program multidimensionalSetLengthDemo(input, output, stdErr);
    var
    	samples: array of array of smallInt;
    begin
    	setLength(samples, 12, 64);
    end.
    

Допустимые индексы для первого измерения выборки находятся в диапазоне `0..11`, в то время как действительные индексы для его второго измерения находятся в диапазоне `0..63`. 

Один весьма полезный факт заключается в том, что ограничение в неизменности всех размеров [статического массива] не распространяется на динамические массивы. 
    
    
    program binomialPotence(input, output, stdErr);
    var
    	pascalsTriangle: array of array of longWord;
    	exponent: longInt;
    	factor: longInt;
    begin
    	setLength(pascalsTriangle, 20);
    	
    	setLength(pascalsTriangle[0], 1);
    	pascalsTriangle[0][0] := 1;
    	
    	setLength(pascalsTriangle[1], 2);
    	pascalsTriangle[1][0] := 1;
    	pascalsTriangle[1][1] := 1;
    	
    	// строим значения простым сложением
    	for exponent := 2 to high(pascalsTriangle) do
    	begin
    		setLength(pascalsTriangle[exponent], exponent + 1);
    		pascalsTriangle[exponent][0] := 1;
    		pascalsTriangle[exponent][exponent] := 1;
    		for factor := 1 to exponent - 1 do
    		begin
    			pascalsTriangle[exponent][factor] :=
    				pascalsTriangle[exponent - 1][factor - 1] +
    				pascalsTriangle[exponent - 1][factor];
    		end;
    	end;
    	
    	// ...
    

### Инициализация

Начиная с FPC 3.0.0 типы динамических массивов, которые не являются анонимными, автоматически снабжаются «конструктором», как это возможно уже знакомо по объектно-ориентированному программированию. Это позволяет объединить вызовы `setLength` и серию присвоения значений в одном операторе: 
    
    
    program dynamicArrayCreateDemo(input, output, stdErr);
    type
    	truths = array of boolean;
    var
    	l: truths;
    begin
    	l := truths.create(false, true, true, false, true, false, false);
    end.
    

Конечно, вы также можете вкладывать массивы: 
    
    
    program nestedDynamicArrayCreateDemo(input, output, stdErr);
    type
    	truths = array of boolean;
    	pattern = array of truths;
    var
    	p: pattern;
    begin
    	p := pattern.create(
    			truths.create(false, false),
    			truths.create(true, false),
    			truths.create(true, false, false),
    			truths.create(true, true, false)
    		);
    end.
    

### Обращение

Имейте в виду, что динамические массивы являются указателями. Присвоение переменных динамического массива друг другу копирует не какую-либо полезную нагрузку, а только адрес. Это отличается от поведения статических массивов. 

Если вы хотите дублировать данные, вы должны использовать [`system.copy`](<https://www.freepascal.org/docs-html/rtl/system/copy.html>). 
    
    
    program dynamicArrayCopyDemo(input, output, stdErr);
    
    var
    	foo, bar: array of char;
    
    procedure printArrays;
    begin
    	writeLn('foo[0] = ', foo[0], '; bar[0] = ', bar[0]);
    end;
    
    begin
    	setLength(foo, 1);
    	foo[0] := 'X';
    	// копируем _адрес_
    	bar := foo;
    	write('     начальное значение: ');
    	printArrays;
    	
    	// изменяем содержимое _посредством_ _второго_ адреса 
    	bar[0] := 'O';
    	write('изменено посредством 2-го адр: ');
    	printArrays;
    	
    	// копируем содержимое
    	bar := copy(foo, 0, length(foo));
    	bar[0] := 'X';
    	write(' скопировано и изменено: ');
    	printArrays;
    end.
    

Только с помощью `copy` оба массива могут быть изменены независимо. 

Как указано выше, `setLength` копирует данные. Выделенная строка в примере выше (семантически) эквивалентна `setLength(bar, length(bar))`. 

Динамические массивы считаются ссылками. Вызов `setLength(myDynamicArrayVariable, 0)` фактически делает `myDynamicArrayVariable:=` [nil](<../en/Nil.md> "Nil") и уменьшает количество ссылок. Только когда счетчик ссылок достигает нуля, блок памяти освобождается. 
    
    
    program dynamicArrayNilDemo(input, output, stdErr);
    var
    	foo, bar: array of char;
    begin
    	setLength(foo, 1);
    	foo[0] := 'X';
    	// копируем ссылку, увеличиваем счетчик ссылок 
    	bar := foo;
    	// foo становится nil, счетчик ссылок уменьшается 
    	setLength(foo, 0);
    	writeLn('length(foo) = ', length(foo),
    		'; length(bar) = ', length(bar));
    	
    	// уменьшаем количество ссылок в очередной раз
    	bar := nil;
    	writeLn('length(foo) = ', length(foo),
    		'; length(bar) = ', length(bar));
    end.
    

Тем не менее, динамические массивы финализируются автоматически. Нет необходимости вручную устанавливать `setLength(…, 0)` для всех ваших ссылок, когда программа заканчивается или когда выходит из области видимости в целом. 

Без `{$rangeChecks on}` возможен выход за пределы массива. Это означает, что при итерации по динамическим массивам невозможно работать без [`low`](<https://www.freepascal.org/docs-html/rtl/system/low.html>) и [`high`](<https://www.freepascal.org/docs-html/rtl/system/high.html>) значений, чтобы определить допустимые индексы (первый является необязательным, поскольку динамические массивы всегда начинаются с нуля). В качестве альтернативы можно использовать [циклы](<for-in_loop.md> "for-in loop/ru") `for… in`, если индекс не требуется. 

Помните, что [`sizeOf`](<SizeOf.md> "SizeOf/ru") динамического массива соответствует размеру указателя. 

## Приложение

  * есть [`system.tBoundArray`](<https://www.freepascal.org/docs-html/rtl/system/tboundarray.html>) и [`objPas.tBoundArray`](<https://www.freepascal.org/docs-html/rtl/objpas/tboundarray.html>)



## См. также

  * [`string`](<String.md> "String/ru")
  * пример: [multidimensional dynamic array](<../en/Example__Multidimensional_dynamic_array.md> "Example: Multidimensional dynamic array")
  * [Free Pascal Reference guide: “Dynamic arrays”](<https://www.freepascal.org/docs-html/ref/refsu14.html#x38-510003.3.1>)

---

_Source: [https://wiki.freepascal.org/Dynamic_array/ru](https://web.archive.org/web/20250113123934/https://wiki.freepascal.org/Dynamic_array/ru)_
