# Typecast

│ **[English (en)](<../en/Typecast.md>)** │  **русский (ru)** │

Typecasting(приведение типов) - это концепция, позволяющая [присваивать](<Becomes.md> "Becomes/ru") значения [переменных](<Variable.md> "Variable/ru") или выражений, которые не соответствуют [типу данных](<Data_type.md> "Data type/ru"), переменной, фактически перекрывая систему строгой типизации [Pascal](<../en/Pascal.md> "Pascal"). 

  


## Contents

  * 1 Определение
    * 1.1 Неявное приведение типов
    * 1.2 Явное приведение типов
      * 1.2.1 Value typecasts (приведение типа для значения)
      * 1.2.2 Variable typecasts (приведение типа для переменной)
  * 2 Преобразование в сравнении с приведением типов
  * 3 Предостережение
  * 4 См.также



## Определение

Различают два варианта приведения типов: 

### Неявное приведение типов

Если полный диапазон значений, которые может иметь источник, может быть сохранен целевым операндом, происходит автоматическая - то есть «неявная» - передача типа. Например, все значения типа [`byte`](<Byte.md> "Byte/ru") могут быть сохранены в `int64`. Присвоение значения типа `byte` для `int64` работает без проблем, поскольку пропущенные [нулевые биты](<https://ru.wikipedia.org/wiki/%D0%91%D0%B8%D1%82%D0%BE%D0%B2%D0%BE%D0%B5_%D0%BF%D0%BE%D0%BB%D0%B5>) заполняются автоматически, то есть неявно. Программист не должен вставлять дополнительный код. 

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): иными словами, преобразование переменных с меньшим диапазоном значений в переменные с большим диапазоном значений происходит автоматически (неявное преобразование). 

* * *

### Явное приведение типов

Если диапазон значений источника не вписывается в диапазон целевого операнда, [компилятор](<FPC.md> "FPC/ru") не скомпилирует программу, если ему не дано [указаний явно] игнорировать это. 

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): иначе говоря, преобразование переменных с большим диапазоном значений в переменные с меньшим диапазоном значений осуществляется с помощью явного преобразования путем вызова соответствующей функции. И если значение переменной-источника не соответствует поддерживаемому диапазону значений переменной-приемника, то произойдет потеря данных. 

* * *

Существует два разных варианта явного приведения типа: 

#### Value typecasts (приведение типа для значения)

Преобразование типов осуществляется путем добавления идентификатора типа данных и заключения выражения для преобразования типов в круглые скобки, например: `dataType(expression)`. Лишние биты просто отбрасываются. Этот подход обычно используется, если есть абсолютная уверенность, что фактическое значение `expression` будет соответствовать ожидаемому. Иногда этот эффект также используется вместо применения оператора [Mod](<../en/Mod.md> "Mod"). 

#### Variable typecasts (приведение типа для переменной)

Такое приведение типа обрабатывает переменную, как если бы это был другой тип. Извлечение и сохранение переменной выполняется, как если бы это был указанный тип данных. Так же, как и в случае приведения типа для значения, перед типом данных добавляется идентификатор типа, а в этом случае круглые скобки окружают идентификатор переменной: `dataTypeIdentifier(variableIdentifier)`. В отличие от приведения типа для значения, приведения типа для переменной может происходить с обеих сторон присваивания. 

## Преобразование в сравнении с приведением типов

Преобразование типов - это упорядоченный процесс отображения значений домена в совместный домен, что может привести к возникновению [исключений](<../en/Exceptions.md> "Exceptions") или [ошибок времени выполнения](<../en/runtime_error.md> "runtime error"). Это делается с помощью правильно определенных [функций](<Function.md> "Function/ru"). Простое приведение типов, с другой стороны, это всегда грубая сила. Оно обрезает и заталкивает биты 1:1 адресату. Однако вы можете задать [перегрузку оператора](<../en/Operator_overloading.md> "Operator overloading"), переопределяя это поведение. 

В некоторых случаях вы можете конвертировать значения: 

возможности конверсии  исходный тип данных | конечный тип данных | вариант преобразования типа | метод   
---|---|---|---  
[`integer`](<../en/Integer.md> "Integer") | [`real`](<../en/Real.md> "Real") | неявный  | оператор присваивания   
`real` | `integer` | явный  | 

  * [`trunc`](<../en/Trunc.md> "Trunc") отсекает дробную часть
  * [`round`](<../en/Round.md> "Round") округляет дробную часть

  
`integer` | [`string`](<../en/String.md> "String") | явный  | [`sysUtils.intToStr`](<https://www.freepascal.org/docs-html/rtl/sysutils/inttostr.html>)  
`real` | `string` | явный  | 

  * [`sysUtils.floatToStr`](<https://www.freepascal.org/docs-html/rtl/sysutils/floattostr.html>)
  * [`sysUtils.floatToStrF`](<https://www.freepascal.org/docs-html/rtl/sysutils/floattostrf.html>)

  
`string` | `integer` | явный  | [`sysUtils.strToInt`](<https://www.freepascal.org/docs-html/rtl/sysutils/strtoint.html>)  
`string` | `real` | явный  | [`sysUtils.strToFloat`](<https://www.freepascal.org/docs-html/rtl/sysutils/strtofloat.html>)  
`string` | [`char`](<../en/Char.md> "Char") | явный  | `stringVariable[indexExpression]`  
`char`/`ANSIChar`/`wideChar` | `string` | неявный  | оператор присваивания   
`char`/`ANSIChar` | `byte` | явный  | 

  * [`ord`](<../en/Ord.md> "Ord")
  * `byte(characterVariableOrExpression)`

  
`byte` | `char`/`ANSIChar` | явный  | 

  * [`chr`](<../en/Chr.md> "Chr")
  * `ANSIChar(byteVariableOrExpression)`

  
перечислимый тип  | `string` | явный  | [`system.writeStr`](<https://www.freepascal.org/docs-html/rtl/system/writestr.html>)`(stringVariable, enumeratedVariableOrExpression)`  
  
В других случаях вы должны вручную выполнить явное приведение типов: 

приведение типов  исходный тип данных | конечный тип данных | вариант преобразования типов | метод   
---|---|---|---  
[`qWord`](<../en/QWord.md> "QWord") | `byte` | явный  | `byte(qWordVariableOrExpression)`  
`qWord` | `word` | явный  | `word(qWordVariableOrExpression)`  
`qWord` | [`cardinal`](<../en/Cardinal.md> "Cardinal") | явный  | `cardinal(qWordVariableOrExpression)`  
`qWord` | `longWord` | явный  | `longWord(qWordVariableOrExpression)`  
`longWord` | `byte` | явный  | `byte(longWordVariableOrExpression)`  
`longWord` | `word` | явный  | `word(longWordVariableOrExpression)`  
`longWord` | `cardinal` | неявный  | оператор присваивания   
`int64` | `byte` | явный  | `byte(int64variableOrExpression)`  
`int64` | `shortInt` | явный  | `shortInt(int64variableOrExpression)`  
[`comp`](<../en/Comp.md> "Comp") | `byte` | явный  | `byte(compVariableOrExpression)`  
`comp` | `shortInt` | явный  | `shortInt(compVariableOrExpression)`  
`comp` | `real` | явный  | `real(compVariableOrExpression)`  
  
## Предостережение

  * Явное приведение типов отключает проверку диапазона в полной [строке кода](</index.php?title=LOC&action=edit&redlink=1> "LOC \(page does not exist\)").



## См.также

  * [«Кастинг типов (компьютерное программирование)»](<https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B8%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5_%D1%82%D0%B8%D0%BF%D0%B0>) в русской Википедии
  * [§ “value typecasts”](<https://www.freepascal.org/docs-html/ref/refse80.html>) в справочном руководстве FreePascal
  * [§ “variable typecasts”](<https://www.freepascal.org/docs-html/ref/refse81.html>) в справочном руководстве FreePascal
  * [§ “`unaligned` typecasts”](<https://www.freepascal.org/docs-html/ref/refse82.html>) в справочном руководстве FreePascal
  * [Перегрузка операторов](<../en/Operator_overloading.md> "Operator overloading") (особая группа операторов присваивания и § «маршрутизация»)

---

_Source: [https://wiki.freepascal.org/Typecast/ru](https://web.archive.org/web/20241104204409/https://wiki.freepascal.org/Typecast/ru)_
