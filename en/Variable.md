# Variable

│ **English (en)** │  **[suomi (fi)](</Variable/fi> "Variable/fi")** │  **[français (fr)](</Variable/fr> "Variable/fr")** │  **[русский (ru)](<../ru/Variable.md> "Variable/ru")** │    
****

A **variable** is an [identifier](<Identifier.md> "Identifier") associated with a chunk of memory that can be inspected and manipulated during [run-time](<runtime.md> "runtime") in accordance with an associated [data type](<Data_type.md> "Data type"). 

## Contents

  * 1 declaration
  * 2 manipulation
  * 3 definition
  * 4 access
  * 5 memory alias
  * 6 see also



## declaration

Variables are [declared](<Declaration.md> "Declaration") in a [`var` section](<Var.md> "Var"). In [Pascal](<Pascal.md> "Pascal") every variable has a data type already known at [compile-time](<Compile_time.md> "Compile time") (and let it be the [data type `variant`](<Variant.md> "Variant")). A variable is declared by a [math]\displaystyle{ (\text{identifier}, \text{data type identifier}) }[/math] tuple, separated by a [colon](<Colon.md> "Colon"): 
    
    
    var
    	foo: char;
    

According to the data type’s space requirements, the appropriate amount of memory is reserved on the stack, as soon as the corresponding [scope](<Scope.md> "Scope") is entered. Depending on where the `var`-section is placed, you can speak of either [global](<Global_variables.md> "Global variables") or [local](<Local_variables.md> "Local variables") variables. 

## manipulation

Variables are manipulated by the [assignment operator `:=`](<Becomes.md> "Becomes"). Furthermore a series of built-in procedures implicitly assign values to a variable: 

  * input/output routines like, [`get`](<Get.md> "Get") and [`put`](</index.php?title=Put&action=edit&redlink=1> "Put \(page does not exist\)"), [`read`](<Read.md> "Read") and `readLn`, [`assign`](<Assign.md> "Assign") and `close`
  * `new` and `dispose` when handling [pointers](<Pointer.md> "Pointer") to [objects](<Object.md> "Object")
  * `setLength` when handling [dynamic arrays](<Dynamic_array.md> "Dynamic array")



## definition

In [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") a variable can be defined, that means declared and initialized, in one term by doing the following. 
    
    
    var
    	x: integer value 42;
    

The [FPC](<FPC.md> "FPC") as of version 3.2.0 does only support Borland Delphi’s `=` notation: 
    
    
    var
    	x: integer = 42;
    

The VAX Pascal notation using `:=` is not supported at all. 

## access

A variable is accessed, that means the value at the referenced memory position is read, by simply specifying its identifier (wherever an [expression](<expression.md> "expression") is expected). 

Note, there are a couple data types which are in fact pointers, but are automatically de-referenced, including but not limited to [classes](<Class.md> "Class"), dynamic arrays and [ANSI strings](<Ansistring.md> "Ansistring"). With `{$modeSwitch autoDeref+}` (not recommended) also typed pointers are silently de-referenced without the [`^` (hat symbol)](<^.md> "^") being present. This means, you do not necessarily operate on the actual memory block the variable is genuinely associated with, but somewhere else. 

Usually the variable’s memory chunk is interpreted according to its data type as it was declared as. With [typecasts](<Typecast.md> "Typecast") the interpretation of a given variable’s memory block can be altered (per expression). 

## memory alias

In conjunction with the [keyword](<Keyword.md> "Keyword") [`absolute`](<Absolute.md> "Absolute") an identifier can be associated with a previously reserved blob of memory. While a plain [math]\displaystyle{ (\text{identifier}, \text{data type}) }[/math] tuple actually sets a certain amount of memory aside, the following declaration of `c` does not occupy any additional space, but links the identifier `c` with the memory block that has been reserved for `x`: 
    
    
    var
    	x: byte;
    	c: char absolute x;
    

Here, the memory alias was used as a, one of many, strategies to convince the [compiler](<Compiler.md> "Compiler") to allow operations valid for the [`char` type](<Char.md> "Char") while the underlying memory was originally reserved for a [`byte`](<Byte.md> "Byte"). This feature has to be chosen wisely. It necessarily requires knowledge of data type’s memory structure, if nothing is supposed to trigger any sort of access violations. 

Most importantly, the additionally referenced memory will be treated as if it was declared regularly. No questions asked. 

## see also

  * [constant](<Constant.md> "Constant")

---

_Source: [https://wiki.freepascal.org/Variable](https://web.archive.org/web/20250407193548/https://wiki.freepascal.org/Variable)_
