# Reserved words

│ **[English (en)](<../en/Reserved_words.md>)** │  **русский (ru)** │


## Contents

  * 1 Зарезервированные слова в Turbo Pascal
  * 2 Зарезервированные слова в Object Pascal
  * 3 Зарезервированные слова в Free Pascal
  * 4 Модификаторы (директивы)
  * 5 Неподдерживаемые модификаторы Turbo Pascal
  * 6 More functionality



  
Зарезервированные слова отдельных [режимов компилятора](</Category:Modes/ru> "Category:Modes/ru") объединены следующим образом: 

  * [Режим Turbo Pascal](<Mode_TP.md> "Mode TP/ru"): для использования доступны зарезервированные слова Turbo Pascal.
  * [Режим Delphi](<Mode_Delphi.md> "Mode Delphi/ru"): для использования доступны зарезервированные слова Turbo Pascal и Object Pascal.
  * [Режим Free Pascal](</index.php?title=Mode_ObjFPC/ru&action=edit&redlink=1> "Mode ObjFPC/ru \(page does not exist\)"): для использования доступны зарезервированные слова Turbo Pascal и Object Pascal.



  


## Зарезервированные слова в Turbo Pascal

Следующие зарезервированные слова встречаются в режиме Turbo Pascal: 

ключевое слово | описание   
---|---  
[and](<And.md> "And/ru") | логический оператор, требующий, чтобы оба операнда были равны true для того. чтобы результат был равен true   
[array](<Array.md> "Array/ru") | множество элементов с одинаковым именем   
[asm](</index.php?title=Asm/ru&action=edit&redlink=1> "Asm/ru \(page does not exist\)") | начало кода, написанного на языке ассемблера   
[begin](<Begin.md> "Begin/ru") | начало [блока](</index.php?title=Block/ru&action=edit&redlink=1> "Block/ru \(page does not exist\)") кода   
[break](<Break.md> "Break/ru") | выход из условия [case](<Case.md> "Case/ru")  
[case](<Case.md> "Case/ru") | выбирает определенный сегмент кода для выполнения в зависимости от значения   
[const](</index.php?title=Const/ru&action=edit&redlink=1> "Const/ru \(page does not exist\)") | объявляет идентификатор с неизменяемым значением или переменную с инициализированным значением   
[constructor](<Constructor.md> "Constructor/ru") | процедура, используемая для создания объекта   
[continue](</index.php?title=Continue/ru&action=edit&redlink=1> "Continue/ru \(page does not exist\)") | пропускает инструкции в цикле **for** и возобновляет выполнение с начала цикла   
[destructor](</index.php?title=Destructor/ru&action=edit&redlink=1> "Destructor/ru \(page does not exist\)") | процедура, используемая для "уничтожения" объекта   
[div](<Div.md> "Div/ru") | оператор целочисленного деления   
[do](<Do.md> "Do/ru") | используется для указания начала цикла   
[downto](<Downto.md> "Downto/ru") | используется в цикле [for](<For.md> "For/ru") для указания декремента (уменьшения) переменной-счетчика   
[else](<Else.md> "Else/ru") | используется в условной конструкции [if](<If.md> "If/ru") для выполнения альтернативной ветви, когда ветвь **if** не выполняется   
[end](<End.md> "End/ru") | конец блока кода, записи или некоторых других конструкций   
[false](<False.md> "False/ru") | логическое значение, означающее что условие не выполняется; противоположно значению [true](<True.md> "True/ru")  
[file](</index.php?title=File/ru&action=edit&redlink=1> "File/ru \(page does not exist\)") | внешняя структура данных, обычно хранящаяся на диске   
[for](<For.md> "For/ru") | цикл с использованием увеличения или уменьшения управляющей переменной   
[function](<Function.md> "Function/ru") | объявляет начало процедуры, которая возвращает значение результата   
[goto](<Goto.md> "Goto/ru") | осуществляет выход из сегмента кода и переходит в другое место   
[if](<If.md> "If/ru") | проверяет условие и по результату сравнения выполняет набор инструкций   
[implementation](</index.php?title=Implementation/ru&action=edit&redlink=1> "Implementation/ru \(page does not exist\)") | определяет внутренние процедуры в [модуле](<Unit.md> "Unit/ru")  
[in](</index.php?title=In/ru&action=edit&redlink=1> "In/ru \(page does not exist\)") | определяет элементы в коллекции   
[inline](</index.php?title=inline/ru&action=edit&redlink=1> "inline/ru \(page does not exist\)") | непосредственно встраивает машинный код в процедуру   
[interface](</index.php?title=Interface/ru&action=edit&redlink=1> "Interface/ru \(page does not exist\)") | глобальные объявления процедур в [unit](<Unit.md> "Unit/ru")  
[label](<Label.md> "Label/ru") | определяет точку перехода для оператора [goto](<Goto.md> "Goto/ru")  
[mod](</index.php?title=Mod/ru&action=edit&redlink=1> "Mod/ru \(page does not exist\)") | оператор, возвращающий остаток целочисленного деления   
[nil](<Nil.md> "Nil/ru") | значение указателя, означающее, что указатель ни на что не ссылается   
[not](<Not.md> "Not/ru") | логический оператор, который инвертирует значение результата проверки   
[object](</index.php?title=Object/ru&action=edit&redlink=1> "Object/ru \(page does not exist\)") | определяет конструкцию типа "объект"   
[of](</index.php?title=Of/ru&action=edit&redlink=1> "Of/ru \(page does not exist\)") | определяет характеристики переменной   
[on](</index.php?title=On/ru&action=edit&redlink=1> "On/ru \(page does not exist\)") |   
[operator](</index.php?title=Operator/ru&action=edit&redlink=1> "Operator/ru \(page does not exist\)") | определяет процедуру, использующуюся для реализации оператора   
[or](<Or.md> "Or/ru") | логический оператор, который позволяет использовать любой из двух вариантов   
[packed](<Packed.md> "Packed/ru") | указывает, что элементы в массиве используют меньше памяти (данное ключевое слово необходимо, прежде всего, для совместимости с устаревшими программами, когда элементы массива упаковывались обычно автоматически)   
[procedure](<Procedure.md> "Procedure/ru") | определяет начало процедуры, которая не возвращает значение результата   
[program](<Program.md> "Program/ru") | определяет начало приложения. Данное ключевое слово обычно является необязательным   
[record](<Record.md> "Record/ru") | набор разнотипных переменных, объединенных под одним именем   
[repeat](<Repeat.md> "Repeat/ru") | цикл, представляющий секцию кода до условной инструкции [until](<Until.md> "Until/ru"), выполняющийся до тех пор, пока результат сравнения равен true   
[set](<Set.md> "Set/ru") | набор значений   
[shl](<Shl.md> "Shl/ru") | оператор сдвига значения влево; эквивалентен умножению на степень 2   
[shr](<Shr.md> "Shr/ru") | оператор сдвига значения вправо; эквивалентен делению на степень 2   
[string](<String.md> "String/ru") | объявляет переменную, содержащую множество символов   
[then](<Then.md> "Then/ru") | указывает начало кода в условии сравнения [if](<If.md> "If/ru")  
[to](<To.md> "To/ru") | используется в цикле [for](<For.md> "For/ru") для указания инкремента (увеличения) переменной-счетчика   
[true](<True.md> "True/ru") | логическое значение, указывающее что сравнение выполняется; противоположно значению [false](<False.md> "False/ru")  
[type](<Type.md> "Type/ru") | объявляет типы записей или новых классов переменных   
[unit](<Unit.md> "Unit/ru") | раздельно компилируемые модули   
[until](<Until.md> "Until/ru") | указывает окончание блока проверки в цикле [repeat](<Repeat.md> "Repeat/ru")  
[uses](</index.php?title=Uses/ru&action=edit&redlink=1> "Uses/ru \(page does not exist\)") | перечисление названий [модулей](<Unit.md> "Unit/ru") в текущей программе или модулей, на которые есть ссылки   
[var](<Var.md> "Var/ru") | объявление переменных   
[while](<While.md> "While/ru") | проверяет значение и, если оно равно [true](<True.md> "True/ru"), выполняет инструкции цикла   
[with](<../en/With.md> "With") | reference the internal variables within a record without having to refer to the record itself   
[xor](<Xor.md> "Xor/ru") | логический оператор, являющийся исключающим [ИЛИ](<Or.md> "Or/ru")  
  
## Зарезервированные слова в Object Pascal

Object Pascal extends the (Turbo) Pascal language with both support for dealing more easily with objects (object orientation) as well as other newer/more advanced concepts (threads, etc).  
In addition to the reserved words in Turbo Pascal, the following reserved words are available in Delphi mode as well:  
[as](<../en/As.md> "As")  
[class](<../en/Class.md> "Class")  
[dispose](<../en/Dispose.md> "Dispose")  
[except](<../en/Except.md> "Except")  
[exit](<../en/Exit.md> "Exit")  
[exports](<../en/Exports.md> "Exports")  
[finalization](<../en/Finalization.md> "Finalization")  
[finally](<../en/Finally.md> "Finally")  
[inherited](<../en/Inherited.md> "Inherited")  
[initialization](<../en/Initialization.md> "Initialization")  
[is](<../en/Is.md> "Is")  
[library](<../en/Library.md> "Library")  
[new](<../en/New.md> "New")  
[on](<../en/On.md> "On")  
[out](</index.php?title=Out&action=edit&redlink=1> "Out \(page does not exist\)")  
[property](</Property> "Property")  
[raise](<../en/Raise.md> "Raise")  
[self](<../en/Self.md> "Self")  
[threadvar](<../en/Threadvar.md> "Threadvar")  
[try](<../en/Try.md> "Try")  
  


## Зарезервированные слова в Free Pascal

Зарезервированные слова в режиме Free Pascal включают: 

  * зарезервированные слова [режима Turbo Pascal](<Mode_TP.md> "Mode TP/ru")
  * зарезервированные слова [режима Object Pascal](<Mode_Delphi.md> "Mode Delphi/ru")  




  


## Модификаторы (директивы)

Ниже представлен список модификаторов. Модификаторы не являются строго зарезервированными словами, однако они используются так же, как зарезервированные слова.  
Подробнее см. в руководстве по Free Pascal.  
[absolute](<Absolute.md> "Absolute/ru")  
[abstract](</index.php?title=abstract&action=edit&redlink=1> "abstract \(page does not exist\)")  
[alias](<../en/alias.md> "alias")  
[assembler](</index.php?title=assembler&action=edit&redlink=1> "assembler \(page does not exist\)")  
[cdecl](</index.php?title=cdecl&action=edit&redlink=1> "cdecl \(page does not exist\)")  
[cppdecl](<../en/Cppdecl.md> "Cppdecl")  
[default](</index.php?title=default&action=edit&redlink=1> "default \(page does not exist\)")  
[export](</index.php?title=export&action=edit&redlink=1> "export \(page does not exist\)")  
[external](</index.php?title=external&action=edit&redlink=1> "external \(page does not exist\)")  
[forward](</index.php?title=forward&action=edit&redlink=1> "forward \(page does not exist\)")  
[index](</index.php?title=index&action=edit&redlink=1> "index \(page does not exist\)")  
[local](</index.php?title=local&action=edit&redlink=1> "local \(page does not exist\)")  
[name](</index.php?title=name&action=edit&redlink=1> "name \(page does not exist\)")  
[nostackframe](</index.php?title=nostackframe&action=edit&redlink=1> "nostackframe \(page does not exist\)")  
[oldfpccall](<../en/oldfpccall.md> "oldfpccall")  
[override](</index.php?title=override&action=edit&redlink=1> "override \(page does not exist\)")  
[pascal](<../en/pascal.md> "pascal")  
[private](</index.php?title=private&action=edit&redlink=1> "private \(page does not exist\)")  
[protected](</index.php?title=protected&action=edit&redlink=1> "protected \(page does not exist\)")  
[public](</index.php?title=public&action=edit&redlink=1> "public \(page does not exist\)")  
[published](</index.php?title=published&action=edit&redlink=1> "published \(page does not exist\)")  
[read](</index.php?title=read&action=edit&redlink=1> "read \(page does not exist\)")  
[register](<Register.md> "Register/ru")  
[reintroduce](<../en/Reintroduce.md> "Reintroduce")  
[safecall](</index.php?title=safecall&action=edit&redlink=1> "safecall \(page does not exist\)")  
[softfloat](</index.php?title=softfloat&action=edit&redlink=1> "softfloat \(page does not exist\)")  
[stdcall](</index.php?title=stdcall&action=edit&redlink=1> "stdcall \(page does not exist\)")  
[virtual](</index.php?title=virtual&action=edit&redlink=1> "virtual \(page does not exist\)")  
[write](</index.php?title=write&action=edit&redlink=1> "write \(page does not exist\)")  
  


## Неподдерживаемые модификаторы Turbo Pascal

The reason why these modifiers are not supported is that these modifiers deal with 16 bit code for DOS. In other words, these modifiers have special meaning for 16 bit programming under DOS and Windows 3.x. 

As Free Pascal does not support 16 bit code (only 32 and 64 bit), these modifiers are irrelevant in Free Pascal code. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** However, these modifiers are supported in the [DOS](<../en/DOS.md> "DOS") crosscompiler present in the FPC development version

[far](<../en/Far.md> "Far")  
[near](<../en/Near.md> "Near")  


## More functionality

Apart from the language features provided by the reserved words/keywords mentioned above, there is a lot of functionality available for the programmer in the various libraries: 

  * RTL: Run-Time Library, available for all FPC and Lazarus programs
  * FCL: Free Component Library: a core set of libraries available for Lazarus programs and usually for FPC (FPC can be compiled without it, but that only happens on purpose for low-memory embedded systems etc)
  * FPC Packages: other packages provided by FPC
  * Lazarus components: these are Lazarus components that can be dropped on a form and often based on FCL or FPC packages
  * Lazarus utility functions: e.g. the [fileutil](<http://wiki.lazarus.freepascal.org/Category:fileutil>) unit.



Apart from the libraries provided by FPC and Lazarus, there are more libraries/components available: 

  * FPC user-supplied units: see the FPC wiki
  * Lazarus CCR: components
  * User-supplied code on the internet: see

---

_Source: [https://wiki.freepascal.org/Reserved_words/ru](https://web.archive.org/web/20250219053235/https://wiki.freepascal.org/Reserved_words/ru)_
