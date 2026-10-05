# Program

│ **[Deutsch (de)](</Program/de> "Program/de")** │  **[English (en)](<../en/Program.md> "Program")** │  **[suomi (fi)](</Program/fi> "Program/fi")** │  **[français (fr)](</Program/fr> "Program/fr")** │  **[Bahasa Indonesia (id)](</Program/id> "Program/id")** │  **[italiano (it)](</Program/it> "Program/it")** │  **[português (pt)](</Program/pt> "Program/pt")** │  **русский (ru)** │    
****

Понятие **программа** означает либо [исполняемая программа](<Executable_program.md> "Executable program/ru"), т.е. самодостаточное и запускаемое [приложение](<../en/Application.md> "Application"), либо часть [файла](</File> "File") (файлов) с [исходным кодом](<../en/Source_code.md> "Source code") на языке [Pascal](<../en/Pascal.md> "Pascal"), который может быть скомпилирован и не объявлен в виде [модуля](<../en/Unit.md> "Unit") или [библиотеки](<../en/Library.md> "Library"). Иногда оно называется главной программой. 

## Contents

  * 1 Главная программа
  * 2 Структура программы
  * 3 См.также



## Главная программа

`program` — это [зарезервированное слово](<../en/Reserved_word.md> "Reserved word"), которое представляет файл с исходным кодом классической программы: 
    
    
    program hiWorld(input, output, stdErr);
    
    begin
    	writeLn('Hi!');
    end.
    

Тем временем [FPC](<../en/FPC.md> "FPC") _отбрасывает_ заголовок программы, т.е. первую строку. Имя выходного файла определяется именем файла исходного кода. Однако имя программы _становится_ зарезервированным [идентификатором](<../en/Identifier.md> "Identifier") (за исключением режимов ISO [начиная с [FPC 3.3.1/trunk revision #45757; cf. [Issue #37322](<https://bugs.freepascal.org/view.php?id=37322>)]). В приведенном выше примере, например. попытка определить константу с именем `hiWorld` вызовет ошибку [времени компиляции](<../en/Compile_time.md> "Compile time") повторяющегося идентификатора. Имя программы идентифицирует глобальную [область](<../en/Scope.md> "Scope"), поэтому его можно использовать для записи полных идентификаторов. 

Список файловых дескрипторов полностью игнорируется, за исключением [`{$mode ISO}`](<../en/Mode_iso.md> "Mode iso"). [`Текстовые`](<Text.md> "Text/ru") [переменные](<Variable.md> "Variable/ru") [`input`](<https://www.freepascal.org/docs-html/rtl/system/input.html>), [`output`](<https://www.freepascal.org/docs-html/rtl/system/output.html>) и [`stderr`](<https://www.freepascal.org/docs-html/rtl/system/stderr.html>) всегда открыты и их имена нельзя изменить в других режимах. (ср.: [`SysInitStdIO`](<https://www.freepascal.org/docs-html/rtl/system/sysinitstdio.html>) всегда вызывается в [`rtl/linux/system.pp`](<https://gitlab.com/freepascal.org/fpc/source/-/tree/release_3_0_4/rtl/linux/system.pp#L367-L368>)) 

Поэтому с помощью FPC следующий полный пример исходного кода компилируется так же, как и предыдущий пример. 
    
    
    begin
    	writeLn('Hi!');
    end.
    

Если программа синтаксически верна, FPC игнорирует все, что идет после финального `end.`. Следующее будет скомпилировано без проблем: 
    
    
    program awesomeProgram(input, output, stdErr);
    begin
    	writeLn('Awesome!');
    end. Я благодарю маму, папу и всех, кто поддерживал меня в создании этой программы.
    

Эта «функция» в основном используется для предоставления журнала изменений в файле или уведомления об авторских правах. 

FPC не поддерживает несколько модулей в одном файле исходного кода, как это делали или делают некоторые другие компиляторы. Исходный код каждого модуля должен находиться в отдельном файле. Однако ограничение, согласно которому имена модулей должны совпадать с именами файлов, не применяется к программам. Это связано с тем, что программы не могут быть включены другими модулями, поэтому их поиск (по имени файла) не требуется. 

## Структура программы

Файл `program` должен иметь определенную [структуру](<Basic_Pascal_Tutorial/Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure/ru"). 

  1. Заголовок программы (в зависимости от используемого компилятора, возможно, необязательный).
  2. Может быть не более одного раздела [`uses`-clause](<../en/Uses.md> "Uses"), и оно должно быть в верхней части программы сразу после заголовка программы.
  3. Ровно один [блок](<../en/Block.md> "Block"), заканчивающийся `end`(обратите внимание на [period](<../en/period.md> "period")). Этот блок может содержать — в отличие от обычных блоков — раздел(ы) `resourcestring`.



Точный порядок и количество различных разделов после (необязательного) предложения `uses` до окончательного составного оператора [`begin`](<../en/Begin.md> "Begin")…[`end`](<../en/End.md> "End") строго не определен. 

Тем не менее, есть некоторые правдоподобные соображения. 

  * Раздел [`type`-section](<../en/Type.md> "Type") предшествует любому разделу, который может использовать типы, например,[`var`](<../en/Var.md> "Var")-разделы или объявления [подпрограмм](<../en/Routine.md> "Routine").
  * Поскольку [`goto`](<../en/Goto.md> "Goto") известен как "инструмент дьявола", раздел [`label`](<../en/Label.md> "Label"), если он есть, максимально близок к фрейму оператора, для которого он должен объявлять метки.
  * Как правило, вы переходите от общего к частному: например, `var`-раздел идет перед разделом [`threadVar`](<../en/Threadvar.md> "Threadvar"). Раздел [`const`](<../en/Const.md> "Const") предшествует разделу [`resourceString`](</index.php?title=Resourcestring&action=edit&redlink=1> "Resourcestring \(page does not exist\)").
  * Разделы `resourceString` могут быть как статическими, так и глобальными, что означает, что они должны появиться относительно скоро после предложения `uses`.
  * Прямое использование [глобальных переменных](<../en/Global_variables.md> "Global variables") в подпрограммах (или даже простое их использование) считается дурным тоном. Вместо этого объявляйте/определяйте свои подпрограммы до любого `var`-(подобного)-раздела. (осторожно: не рискуйте и задайте `{$writeableConst off}`)
  * [Глобальные директивы компилятора](<../en/global_compiler_directives.md> "global compiler directives"), особенно те, которые разрешают или ограничивают то, что может быть написано (например, `{$goto on}` позволяет использовать `goto`) или неявно добавляют зависимости модулей, такие как [`{$mode objFPC}`](<../en/Mode_ObjFPC.md> "Mode ObjFPC"), должны появляться вскоре после заголовка программы.



Принимая во внимание все соображения, примерная структура программы должна выглядеть так (за исключением `label` и [`{$goto on}`](<../en/sGoto.md> "sGoto"), которые упоминаются только для полноты картины): 
    
    
    program sectionDemo(input, output, stdErr);
    
    // Глобальные директивы компилятора ----------------------------
    {$mode objFPC}
    {$goto on}
    
    uses
    	sysUtils;
    
    const
    	answer = 42;
    
    resourceString
    	helloWorld = 'Hello world!';
    
    type
    	primaryColor = (red, green, blue);
    
    procedure doSomething(const color: primaryColor);
    begin
    end;
    
    // M A I N -----------------------------------------------
    var
    	i: longint;
    
    threadVar
    	z: longbool;
    
    label
    	42;
    begin
    end.
    

[Пример сознательно игнорирует возможность «типизированных констант», придерживаясь скорее традиционных концепций, чем невольно сбивает с толку новичков.]

## См.также

  * [unit](<../en/Unit.md> "Unit")
  * [library](<../en/Library.md> "Library")
  * [program structure](<../en/Program_Structure.md> "Program Structure") in the Object Pascal Introduction series
  * [§ “Beginning” in the _Pascal Programming_ book on Wikibooks.org](<https://en.wikibooks.org/wiki/Pascal_Programming/Beginning>)


  *[ISO]: International Organization for Standardization

---

_Source: [https://wiki.freepascal.org/Program/ru](https://web.archive.org/web/20240920204055/https://wiki.freepascal.org/Program/ru)_
