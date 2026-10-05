# Function

│ **[English (en)](<../en/Function.md>)** │  **русский (ru)** │

## Contents

  * 1 Возвращаемое значение
  * 2 Замечания
  * 3 См.также



**Функция** — это [подпрограмма](<../en/Routine.md> "Routine"), которая, в отличие от [процедур](<../en/Procedure.md> "Procedure"), возвращает значение. Вызов функции практически заменяется возвращаемым значением. Если состояние переключателя компилятора [`{$extendedSyntax}`](<../en/$extendedSyntax.md> "$extendedSyntax") отключено, вызовы функций не могут отображаться как непродуктивные операторы, но должны быть целиком или частью [выражения](<../en/expression.md> "expression"). 

Слово `function` является [зарезервированным словом](<../en/Reserved_word.md> "Reserved word"). 

## Возвращаемое значение

В дополнение к обычной процедуре формальная сигнатура функции содержит [тип](<Data_type.md> "Data type/ru") возвращаемого значения: список формальных параметров должен сопровождаться [двоеточием](<../en/Colon.md> "Colon") и типом возвращаемого значения. Например, следующая функция возвращает [`Boolean`](<../en/Boolean.md> "Boolean")
    
    
    function myFunction(const firstParameter: real): Boolean;
    

При реализации функций есть несколько способов определить возвращаемое значение функции. 
    
    
    program functionDemo(input, output, stderr);
    
    {$mode objFPC}
    
    // традиционный синтаксис:
    // результат сохраняется в переменной
    // его имя такое же, как у функции
    function myLine(const x: real): real;
    begin
    	myLine := 0.5 * x + 2;
    end;
    

Если `{$modeswitch result+}`, заданный [`{$mode objFPC}`](<../en/Mode_ObjFPC.md> "Mode ObjFPC") и [`{$mode Delphi}`](<../en/Mode_Delphi.md> "Mode Delphi"), внутри блока реализации также доступен результат специального идентификатора: 
    
    
    // используя специальный идентификатор `result`
    function myParabola(const x: real): real;
    begin
    	result := sqr(x) - 1;
    end;
    

Кроме того, в `{$mode objFPC}` подпрограмма [`exit`](<https://www.freepascal.org/docs-html/rtl/system/exit.html>) также установит возвращаемое значение _и_ покинет стековый кадр. В предыдущих двух примерах могли появиться дополнительные операторы, и они были бы выполнены, уже после того, как `exit` подпрограмма _выполнится_. Это поведение оператора `return` в C или других языках программирования. 
    
    
    // использование процедуры выхода exit
    function even(const x: longint): Boolean;
    begin
      exit(not odd(x));
    end;
    

В языке [ассемблера](<../en/Assembly_language.md> "Assembly language") применяются другие правила. Если тип возвращаемого значения является целым числом, используется регистр-накопитель, при условии, что он помещается туда: 
    
    
    // на ассемблере:
    // возвращаемый тип умещается в один регистр => использовать регистр-накопитель
    function zero(const x: int64): Boolean;
    {$ifDef CPUx86_64}
    assembler; register;
    {$asmMode intel}
    asm
    	// xor изменяет флаги => поместите его перед тестом
    	xor rax, rax     // rax := 0    (удалить остаток)
    	
    	test x, x        // x = 0 ?
    	setz al          // rax := ZF
    	
            // Когда вы изучаете вывод ассемблера
            // вы заметите, что компилятор автоматически вставляет код
            // который перемещает содержимое rax в нужное место в стеке,
            // пока не появится подсказка noStackFrame
            // (автоматически устанавливается некоторыми уровнями оптимизации)
            // инструктирует компилятор опустить кадр стека, если это возможно.
    end;
    {$else}
    begin
    	result := x = 0;
    end;
    {$endIf}
    

В противном случае, в зависимости от того, какой режим [`{$asmMode}`](</index.php?title=sAsmmode&action=edit&redlink=1> "sAsmmode \(page does not exist\)") активен, можно использовать макрос `@result` (Intel) или `__result` (AT&T). 
    
    
    type
    	bodyAttributes = record
    		surfaceArea: real;
    		volume: real;
    	end;
    
    // на ассемблере:
    // возвращаемый тип не помещается в аккумулятор => макрос @result дает адрес
    function sphere(const radius: real): bodyAttributes;
    {$ifDef CPUx86_64}
    assembler;
    {$asmMode intel}
    const
    	three: longint = 3;
    	four: longint = 4;
    var
    	r: real;
    asm
    	pextrq r, radius, 0 // r := (@radius+0)^
    	lea rax, @result    // rax := @result
    	fld r               // radius
    	
    	fld st(0)           // radius radius
    	fild four           // 4 radius radius
    	fldpi               // pi 4 radius radius
    	fmul                // 4*pi radius radius
    	fxch                // radius 4*pi radius
    	fld st(0)           // radius radius 4*pi radius
    	fmul                // radius^2 4*pi radius
    	fmul                // 4*pi*radius^2 radius
    	fst [rax].bodyAttributes.surfaceArea
    	
    	fmul                // 4*pi*radius^3
    	fild three          // 3 4*pi*radius^3
    	fdivp               // 4/3*pi*radius^3
    	fst [rax].bodyAttributes.volume
    end;
    {$else}
    begin
    	sphere.surfaceArea := 4 * pi() * sqr(radius);
    	sphere.volume := 4 / 3 * pi() * sqr(radius) * abs(radius);
    end;
    {$endIf}
    

Первоначально Паскаль ожидал _ровно одно_ [присваивание](<../en/Becomes.md> "Becomes") переменной результата (в зависимости от того, что используется). Однако [FPC](<../en/FPC.md> "FPC") не запрещает множественные присваивания. Он выдаст предупреждение, если не был использован ни один из возможных идентификаторов результата или процедура выхода не написана. 

> Warning
>     Function result does not seem to be set (_рус_. Предупреждение: результат функции, похоже, не установлен)
>     You can get this warning if the [compiler](<../en/Compiler.md> "Compiler") thinks that a function return value is not set. This will not be displayed for assembler procedures, or procedures that contain assembler blocks.(_рус_. Вы можете получить это предупреждение, если [компилятор](<../en/Compiler.md> "Compiler") считает, что возвращаемое значение функции не установлено. Это не будет отображаться для ассемблерных процедур или процедур, содержащих ассемблерные блоки.)
    
    
    begin
    	writeLn(sphere(2.0).surfaceArea);
    end.
    

Обратите внимание, что в случае [перегрузок операторов](<../en/Operator_overloading.md> "Operator overloading") в формальной подписи должен быть объявлен специальный вид функции, идентификатор переменной результата. Подробнее см. в статье. 

## Замечания

[Pure functions](<../en/Pure_functions.md> "Pure functions") это функции, не зависящие от внешнего состояния. 

## См.также

  * [Pascal basics](<../en/Pascal_basics.md> "Pascal basics")
  * [§ “Using functions and procedures” in the Reference Guide](<https://www.freepascal.org/docs-html/ref/refch14.html>)
  * [§ “Intel 80x86 Inline assembler” in the Programmer’s Guide](<https://www.freepascal.org/docs-html/prog/progse10.html>)

---

_Source: [https://wiki.freepascal.org/Function/ru](https://web.archive.org/web/20250121222917/https://wiki.freepascal.org/Function/ru)_
