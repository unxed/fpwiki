# SizeOf

│ **[English (en)](<../en/SizeOf.md>)** │  **русский (ru)** │

[Функция](<Function.md> "Function/ru") [времени компиляции](<../en/Compile_time.md> "Compile time") [`sizeOf`](<https://www.freepascal.org/docs-html/rtl/system/sizeof.html>) вычисляет размер в байтах данного имени [типа данных](<Data_type.md> "Data type/ru") или [идентификатора](<Identifier.md> "Identifier/ru") [переменной](<Variable.md> "Variable/ru"). 

`sizeOf` также может применяться в выражениях времени компиляции внутри [директив компилятора](<../en/Compiler_directive.md> "Compiler directive"). 

  


## Contents

  * 1 Использование
  * 2 Сравнительные замечания
    * 2.1 Динамические массивы и тому подобное
    * 2.2 Классы



## Использование

`sizeOf` особенно часто встречается в [ ассемблере](<../en/Assembler.md> "Assembler") или при ручном выделении памяти: 
    
    
    program sizeOfDemo(input, output, stderr);
    
    {$typedAddress on}
    
    uses
    	heaptrc;
    
    type
    	s = record
    		c: char;
    		i: longint;
    	end;
    
    var
    	x: ^s;
    
    begin
    	returnNilIfGrowHeapFails := true;
    	
    	getMem(x, sizeOf(x));
    	
    	if not assigned(x) then
    	begin
    		writeLn(stderr, 'malloc for x failed');
    		halt(1);
    	end;
    	
    	x^.c := 'r';
    	x^.i := -42;
    	
    	freeMem(x, sizeOf(x));
    end.
    

Прямая обработка структурированных типов данных на ассемблере также требует знания размеров данных: 
    
    
    program sizeOfDemo(input, output, stderr);
    
    type
    	integerArray = array of integer;
    
    function sum(const f: integerArray): int64;
    {$ifdef CPUx86_64}
    assembler;
    {$asmMode intel}
    asm
    	// убеждаемся, что f находится в конкретном регистре
    	mov rsi, f                       // rsi := f  (указатель на массив)
    	
    	// проверка на пустой (nil) указатель (напр. пустой массив)
    	test rsi, rsi                    // rsi = 0 ?
    	jz @sum_abort                    // если rsi = nil, то переходим к @sum_abort
    	
    	// загружаем последний индекс массива [теоретически это верхний элемент массива F]
    	mov rcx, [rsi] - sizeOf(sizeInt) // rcx := (rsi - sizeOf(sizeInt))^
    	
    	// загружаем первый элемент, поскольку условие цикла не дойдет до него
    	{$if sizeOf(integer) = 4}
    	mov eax, [rsi]                   // eax := rsi^
    	{$elseif sizeOf(integer) = 2}
    	mov ax, [rsi]                    // ax := rsi^
    	{$else} {$error неожиданный целочисленный размер} {$endif}
    	
    	// мы сделаем, если f не содержит больше элементов
    	test rcx, rcx                    // rcx = 0 ?
    	jz @sum_done                     // если high(f) = 0,  то переходим к @sum_done
    	
    @sum_iterate:
    	{$if sizeOf(integer) = 4}
    	mov edx, [rsi + rcx * 4]         // edx := (rsi + 4 * rcx)^
    	{$elseif sizeOf(integer) = 2}
    	mov dx, [rsi + rcx * 2]          // dx := (rsi + 2 * rcx)^
    	{$else} {$error неожиданный масштабный коэффициент} {$endif}
    	
    	add rax, rdx                     // rax := rax + rdx
    	
    	jo @sum_abort                    // если OF, то переходим к @sum_abort
    	
    	loop @sum_iterate                // dec(rcx)
    	                                 // если rcx <> 0,  то переходим к @sum_iterate
    	
    	jmp @sum_done                    // переходим к @sum_done
    	
    @sum_abort:
    	// загружаем нейтральный элемент для прибавления
    	xor rax, rax                     // rax := 0
    	
    @sum_done:
    end;
    {$else}
    unimplemented;
    begin
    	sum := 0;
    end;
    {$endif}
    
    begin
    	writeLn(sum(integerArray.create(2, 5, 11, 17, 23)));
    end.
    

В [FPC](<FPC.md> "FPC/ru") размер [`integer`](<https://www.freepascal.org/docs-html/rtl/system/integer.html>) зависит от используемого [режима компилятора](<../en/Compiler_Mode.md> "Compiler Mode"). Однако `sizeOf(sizeInt)` был вставлен только для демонстрационных целей. В ветке `{$ifdef CPUx86_64}` `sizeOf(sizeInt)` всегда равен `8`. 

## Сравнительные замечания

### Динамические массивы и тому подобное

Поскольку [динамические массивы](<Dynamic_array.md> "Dynamic array/ru") реализованы как указатели на блок в куче, `sizeOf` оценивает размер указателя. Чтобы определить размер массива - его данных - необходимо использовать `sizeOf` вместе с функцией [`length`](<https://www.freepascal.org/docs-html/rtl/system/length.html>)
    
    
    program dynamicArraySizeDemo(input, output, stderr);
    
    uses
    	sysUtils;
    
    resourcestring
    	enteredN = 'Вы ввели %0:d целых чисел';
    	totalData = 'занимающих всего %0:d Bytes.';
    
    var
    	f: array of longint;
    
    begin
    	setLength(f, 0);
    	
    	while not eof() do
    	begin
    		setLength(f, length(f) + 1);
    		readLn(f[length(f)]);
    	end;
    	
    	writeLn(format(enteredN, [length(f)]));
    	writeLn(format(totalData, [length(f) * sizeOf(f[0])]));
    end.
    

Подход аналогичен для [ANSI strings](<../en/Ansistring.md> "Ansistring") (в зависимости от состояния переключателя компилятора `{$longstrings}`, возможно, также обозначаемого [`string`](<String.md> "String/ru")). Не забывайте, что динамические массивы управляют данными перед ссылочным блоком полезной нагрузки. Поэтому, если вы действительно хотите знать, сколько памяти было зарезервировано для одного массива, вам также нужно будет учитывать [индекс массива] high (последний индекс в массиве) и счетчик ссылок. 

### Классы

[Классы](<Class.md> "Class/ru") также являются указателями. Класс [`TObject`](<https://www.freepascal.org/docs-html/rtl/system/tobject.html>) предоставляет функцию [`instanceSize`](<https://www.freepascal.org/docs-html/rtl/system/tobject.instancesize.html>). Он возвращает размер объекта, как это предписывается определением типа класса. Дополнительная память, которая выделяется [конструктором](<Constructor.md> "Constructor/ru") или любым другим [методом](<../en/Method.md> "Method"), не учитывается. Обратите внимание, что классы также могут содержать динамические массивы или строки ANSI.

---

_Source: [https://wiki.freepascal.org/SizeOf/ru](https://web.archive.org/web/20240807022528/https://wiki.freepascal.org/SizeOf/ru)_
