# Typecast

│ **[Deutsch (de)](</Typecast/de> "Typecast/de")** │  **English (en)** │  **[français (fr)](</Typecast/fr> "Typecast/fr")** │  **[русский (ru)](<../ru/Typecast.md> "Typecast/ru")** │    
****Typecasting is the concept, allowing to[assign](<Becomes.md> "Becomes") values of [variables](<Variable.md> "Variable") or expressions, that do not match the variable’s [data type](<Data_type.md> "Data type"), virtually overriding [Pascal](<Pascal.md> "Pascal")’s strong typing system. 

## Contents

  * 1 definition
    * 1.1 implicit typecasting
    * 1.2 explicit typecasting
      * 1.2.1 value typecast
      * 1.2.2 variable typecast
  * 2 conversion versus typecasting
  * 3 caveats
  * 4 see also



## definition

Two flavors of typecasting are recognized: 

### implicit typecasting

If the complete range of values the source can be stored by the destination operand, an automatic – thus “implicit” – typecast occurs. For instance, all the values of a [`byte`](<Byte.md> "Byte") can be stored in an `int64`. Assigning a `byte`’s value to a `int64` works without problems, since the missing 0-bits are filled in automatically, i.e. implicitly. The programmer does not have to insert any additional code. 

### explicit typecasting

If the source’s range of value does not fit into the destination operand’s range, [the compiler](<FPC.md> "FPC") will not compile the program, unless it is instructed to ignore this. There are two different explicit typecasts: 

#### value typecast

A value typecast is done, by prepending a data type identifier and surrounding the expression to typecast with parentheses, like this: `dataType(expression)`. Extraneous bits are just cut off. This approach is usually used, if there is absolute certainty, the actual value of the `expression` will fit into the destination. Occasionally this effect is also used instead of a [modulo operation](<Mod.md> "Mod"). 

#### variable typecast

A variable typecast treats a variable as if it were a different type. Retrieving and storing the variable is done, as if it was the specified data type. Just as a value typecast, the data type identifier is prepended and parentheses surround, in this case, a variable identifier: `dataTypeIdentifier(variableIdentifier)`. Unlike a value typecast, the variable typecast can occur on both sides of an assignment. 

## conversion versus typecasting

Type conversion is the ordered process of mapping the domain’s values to a co-domain, possibly triggering [exceptions](<Exceptions.md> "Exceptions") or [run-time errors](<runtime_error.md> "runtime error"). This is done by properly defined [functions](<Function.md> "Function"). Bare typecasts on the other hand are always with brute force. They cut and push the bits 1:1 to the destination. However, you may define an [operator overload](<Operator_overloading.md> "Operator overloading") redefining this behavior. 

In some instances, you can convert values: 

conversion opportunities  soure data type | target data type | type of type conversion | method   
---|---|---|---  
[`integer`](<Integer.md> "Integer") | [`real`](<Real.md> "Real") | implicit  | assignment statement   
`real` | `integer` | explicit  | 

  * [`trunc`](<Trunc.md> "Trunc") cuts off fractional part
  * [`round`](<Round.md> "Round") rounds fractional part

  
`integer` | [`string`](<String.md> "String") | explicit  | [`sysUtils.intToStr`](<https://www.freepascal.org/docs-html/rtl/sysutils/inttostr.html>)  
`real` | `string` | explicit  | 

  * [`sysUtils.floatToStr`](<https://www.freepascal.org/docs-html/rtl/sysutils/floattostr.html>)
  * [`sysUtils.floatToStrF`](<https://www.freepascal.org/docs-html/rtl/sysutils/floattostrf.html>)

  
`string` | `integer` | explicit  | [`sysUtils.strToInt`](<https://www.freepascal.org/docs-html/rtl/sysutils/strtoint.html>)  
`string` | `real` | explicit  | [`sysUtils.strToFloat`](<https://www.freepascal.org/docs-html/rtl/sysutils/strtofloat.html>)  
`string` | [`char`](<Char.md> "Char") | explicit  | `stringVariable[indexExpression]`  
`char`/`ANSIChar`/`wideChar` | `string` | implicit  | assignment statement   
`char`/`ANSIChar` | `byte` | explicit  | 

  * [`ord`](<Ord.md> "Ord")
  * `byte(characterVariableOrExpression)`

  
`byte` | `char`/`ANSIChar` | explicit  | 

  * [`chr`](<Chr.md> "Chr")
  * `ANSIChar(byteVariableOrExpression)`

  
enumerated type  | `string` | explicit  | 

  * [`system.Str`](<https://www.freepascal.org/docs-html/rtl/system/str.html>)`(enumeratedVariableOrExpression, stringVariable)`
  * [`system.writeStr`](<https://www.freepascal.org/docs-html/rtl/system/writestr.html>)`(stringVariable, enumeratedVariableOrExpression)`

  
  
In other cases you manually have to perform explicit typecasts: 

typecasting  source data type | target data type | type of type conversion | method   
---|---|---|---  
[`qWord`](<QWord.md> "QWord") | `byte` | explicit  | `byte(qWordVariableOrExpression)`  
`qWord` | `word` | explicit  | `word(qWordVariableOrExpression)`  
`qWord` | [`cardinal`](<Cardinal.md> "Cardinal") | explicit  | `cardinal(qWordVariableOrExpression)`  
`qWord` | `longWord` | explicit  | `longWord(qWordVariableOrExpression)`  
`longWord` | `byte` | explicit  | `byte(longWordVariableOrExpression)`  
`longWord` | `word` | explicit  | `word(longWordVariableOrExpression)`  
`longWord` | `cardinal` | implicit  | assignment statement   
`int64` | `byte` | explicit  | `byte(int64variableOrExpression)`  
`int64` | `shortInt` | explicit  | `shortInt(int64variableOrExpression)`  
[`comp`](<Comp.md> "Comp") | `byte` | explicit  | `byte(compVariableOrExpression)`  
`comp` | `shortInt` | explicit  | `shortInt(compVariableOrExpression)`  
`comp` | `real` | explicit  | `real(compVariableOrExpression)`  
  
## caveats

  * Explicit typecasts disable range checks altogether in the complete [line of code](</index.php?title=LOC&action=edit&redlink=1> "LOC \(page does not exist\)").



## see also

  * [“type casting (computer programming)”](<https://en.wikipedia.org/wiki/Type_casting_\(computer_programming\)>) in the English Wikipedia
  * [§ “value typecasts”](<https://www.freepascal.org/docs-html/ref/refse84.html>) in the FreePascal Reference guide
  * [§ “variable typecasts”](<https://www.freepascal.org/docs-html/ref/refse85.html>) in the FreePascal Reference guide
  * [§ “`unaligned` typecasts”](<https://www.freepascal.org/docs-html/ref/refse86.html>) in the FreePascal Reference guide
  * [operator overloading](<Operator_overloading.md> "Operator overloading") (especially the group of assignment operators, and § “routing”)

---

_Source: [https://wiki.freepascal.org/Typecast](https://web.archive.org/web/20241213020718/https://wiki.freepascal.org/Typecast)_
