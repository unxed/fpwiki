# Declaration

A **declaration** introduces the compiler to new [identifiers](<Identifier.md> "Identifier"). It is an agreement between the programmer and compiler, that a certain symbol henceforth has a fixed meaning. 

## characteristics

Declarations do not have any (explicit) impact on a compiled program: For example, a declaration of a variable informs the compiler about a new identifier associated with a data type. However, neither the variable’s name, nor the data type are stored in the [compiled binary file](<Binary.md> "Binary"). 

Nevertheless, implicitly the compiler _reserves_ enough _memory_ and ensures only _legal operations_ are performed in compliance with the data type. Thus declarations ensure the compiler can process the source code. 

## types

[Pascal](<Pascal.md> "Pascal") knows following types of declarations: 

  * modules
  * [constants](<Constant.md> "Constant")
  * resource strings (non-standard extension)
  * [data types](<Type.md> "Type")
  * [variables](<Variable.md> "Variable")
  * [labels](<Label.md> "Label") (legacy)
  * [routines](<Routine.md> "Routine")



## difference to definitions

Declarations merely tell the compiler “there is something”. In contrast to that, _definitions_ elaborate what “something” is. All definitions will (eventually) change the program state. Declarations do not change the program state. 

  * In a [`program`](<Program.md> "Program") the program header is the declaration, the subsequent [block](<Block.md> "Block") _defines_ the program.
  * The same applies for routines.
  * Labels are declared in the `label` section
        
        label
        	systemCrash;
        

Their definition, i. e. actually associating this identifier with an address, occurs later in the source code:
        
        begin
        	…
        	if somethingIsWrong then
        	begin
        		goto systemCrash;
        	end;
        	…
        systemCrash:
        	…
        

  * Constants are declared and defined in one brush: In
        
        const
        	answer = 42;
        

the information “`answer` is an integer constant” is the declaration. The information “`answer` equals `42`” is the definition. The same applies to resource strings.
  * Data types are frequently defined implicitly. The following only _declares_ a data type `point`:
        
        type
        	point = record
        			x, y: integer;
        		end;
        

The definition of `point` is invisible: Which operations are allowed on `point` is not written in this piece of code. Nevertheless, the [assignment](<Becomes.md> "Becomes") of a `point` value to a `point` variable works out of the box, and further operations on `point` can be defined using [operator overloading](<Operator_overloading.md> "Operator overloading").
  * The same applies to variable declarations (except there is no possibility to define operators on anonymous data types). The _definition_ of a variable is an [assignment](<Becomes.md> "Becomes").



In summary, breach of agreed _declarations_ cause [compile-time errors](<compile-time_error.md> "compile-time error"), whereas _wrong definitions_ cannot be caught by the compiler.

---

_Source: [https://wiki.freepascal.org/Declaration](https://web.archive.org/web/20250315161456/https://wiki.freepascal.org/Declaration)_
