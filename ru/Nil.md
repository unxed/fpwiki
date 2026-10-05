# Nil

│ **[Deutsch (de)](</Nil/de> "Nil/de")** │  **[English (en)](<../en/Nil.md> "Nil")** │  **[suomi (fi)](</Nil/fi> "Nil/fi")** │  **[français (fr)](</Nil/fr> "Nil/fr")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Nil/zh_CN> "Nil/zh CN")** │    
****

[Зарезервированное слово](<Reserved_word.md> "Reserved word/ru") `nil` представляет собой специальное [значение](<Constant.md> "Constant/ru") [переменной](<../en/Variable.md> "Variable")-указателя, не указывающей ни на что конкретное. В [FPC](<FPC.md> "FPC/ru") это реализовано как `pointer(0)` (числовое значение `0`), однако программист не должен использовать этот факт. На других языках программирования, например на C, кто-то пишет `null`. Термины “null pointer” или “nil pointer” используются взаимозаменяемо, даже среди программистов на Паскале. 

Есть два популярных объяснения этимологии `nil`. Кто-то говорит: `nil` означает латинское слово «nihil», означающее «ничего». Другой предполагает, что `NIL` \- это английская аббревиатура, обозначающая “not in list” (нет в списке). Может быть, поскольку немецкое слово «Null» обозначает цифру «ноль», чтобы избежать путаницы или для различия между понятием и значением, было выбрано слово `nil`. Во всяком случае, это не имеет никакой разницы при программировании. 

## Contents

  * 1 Совместимость присвоения
  * 2 Приложение
  * 3 См.также



## Совместимость присвоения

`nil` can be of course [assigned](<../en/Becomes.md> "Becomes") to a [`pointer`](<../en/Pointer.md> "Pointer") variable, but also to other types, which are in fact pointers, but their usage is more convenient. For instance [dynamic arrays](<../en/Dynamic_array.md> "Dynamic array") or [classes](<../en/Class.md> "Class"): 

Конечно, `nil` может быть [присвоен](<Becomes.md> "Becomes/ru") переменной [`pointer`](<Pointer.md> "Pointer/ru"), но также и другим типам, которые на самом деле являются указателями, но их использование более удобно. Например, [динамические массивы](<Dynamic_array.md> "Dynamic array/ru") или [классы](<Class.md> "Class/ru"): 
    
    
    program nilDemo(input, output, stderr);
    var
    	loc: pointer;
    	chk: array of boolean;
    	msg: PChar;
    	prc: TProcedure;
    	obj: TObject;
    begin
    	// указываем в никуда
    	loc := nil;
    	// очищаем динамический массив
    	chk := nil;
    	// очищаем строку
    	msg := nil;
    	// процедурная переменная не ссылается ни на одну процедуру
    	prc := nil;
    	// теряем ссылку на объект
    	obj := nil;
    end.
    

Обратите внимание, что присвоение `nil` динамическому массиву практически эквивалентно вызову процедуры `setLength(dynamicArrayVariable, 0)`. Значения массива теряются, если счетчик ссылок `dynamicArrayVariable` достигает нуля. Однако не существует сопоставимого механизма для других типов, например, присвоение `nil` переменной `class`'а или `pointer` _не_ освободит (т.е. де-аллокирует) память, которая, возможно, была занята ссылочной структурой. 

## Приложение

В манере Паскаля вы обычно не пишете такие выражения, как `pointerVariable = nil`, а используете более толковые идентификаторы. Шаблон [`system.assigned`](<../en/Assigned.md> "Assigned") заменяется точно таким же выражением, но скрывает тот факт, что переменная (_реализована_ как) указатель. Поэтому его использование не является обязательным. 

Шаблон [`SysUtils.FreeAndNil`](<../en/FreeAndNil.md> "FreeAndNil") будет вызывать шаблон класса `free` и присваивать `nil` передаваемому указателю (переменной типа `class`). Хотя это хорошая идея, очистить указатели, которые больше не указывают на допустимые объекты, это может усложнить отладку, так как нет доступного указателя, указывающего на адрес, которым был определенный объект. 

## См.также

  * [Оператор разыменования указателя `^`](<^.md> "^/ru")
  * Переменная [`system.returnNilIfGrowHeapFails`](<https://www.freepascal.org/docs-html/rtl/system/returnnilifgrowheapfails.html>)
  * `{$objectChecks on}`
  * [Обнуляемые типы](<../en/Nullable_types.md> "Nullable types")

---

_Source: [https://wiki.freepascal.org/Nil/ru](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/Nil/ru)_
