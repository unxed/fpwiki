# Operators

│ **English (en)** │

## Contents

  * 1 Free Pascal operators
    * 1.1 Address operator
    * 1.2 Assignment operator
    * 1.3 Relational operators
    * 1.4 Binary arithmetic operators
    * 1.5 Unary arithmetic operators
    * 1.6 Logical operators
    * 1.7 Truth operators
    * 1.8 String operators
    * 1.9 Set operators
    * 1.10 Class Operators
    * 1.11 Pointer Operators
    * 1.12 The priority of the operators
    * 1.13 See also



# Free Pascal operators

## Address operator

The address operator @ returns the start address of a variable, a procedure or a function. If the compiler switch is {$ T-}, the return value is an untyped pointer. If the compiler switch is {$ T +}, the return value is a typed pointer. The default setting for the compiler is {$ T-}. 

## Assignment operator

The assignment operator assigns a value to a data field. 
    
    
      a := b;   // b is assigned to a
    

## Relational operators

The following operations are possible: 

  * Integer and floating point data fields can be mixed on both sides of the operator.
  * Short, ANSI and Widestring data fields can be mixed on both sides of the operator.

Operator | Meaning   
---|---  
= | equal   
<> | not equal   
< | less than   
> | greater than   
<= | less than or equal   
>= | greater or equal   
in | checks whether the element appears in a set of the same data type   
  
## Binary arithmetic operators

Operator | Meaning   
---|---  
+ | addition   
- | subtraction   
* | multiplication   
/ | division   
div | integer division (Calculates the integer value of how many times an integer is in another integer.)   
mod | modulo division (Computes the integer remainder of a division of two integers.)   
  
## Unary arithmetic operators

\+ specifies that the value is positive 

\- specifies that the value is negative 

Examples: 
    
    
       a := +7;
       b := -3;
    

## Logical operators

Operator | Operation   
---|---  
not | Bitwise unary negation   
and | Bitwise and   
or | Bitwise or   
xor | Bitwise exclusive or   
shl | Bitwise shift to the left   
shr | Bitwise shift to the right   
<< | Bitwise shift to the left (corresponds with the operator shl)   
>> | Bitwise shift to the right (corresponds with the operator shr)   
  
Examples: 
    
    
      A shr 1 // corresponds to integer DIV 2 (only that it is faster)
              // (only for integer data fields, the result is rounded down to an integer)
      Not 1   // corresponds to -2
      Not 0   // corresponds to -1
      Not -1  // corresponds to 0
      B shl 2 // corresponds to integer * 4 (only that it is faster)
              // (only for integer data fields, the result is an integer)
      1 or 2  // corresponds to 3
      3 xor 1 // corresponds to 2
    

## Truth operators

Operator | Operation   
---|---  
not | logical negation (unary)   
and | logical and   
or | logical or   
xor | logical xor   
  
## String operators

Operator | Operation   
---|---  
+ | String concatenation (joins two strings together)   
  
## Set operators

Operator | Operation   
---|---  
+ | union   
- | difference set   
* | intersection   
>< | symmetrical difference   
<= | contains   
include | add an item to the set   
exclude | delete an item in the set   
in | checks if the item is in the set   
  
## Class Operators

Operator | Operation   
---|---  
is | checks whether the object is of a certain class   
as | performs a conditional type cast (conditional typecasting)   
  
## Pointer Operators

Operator | Operation | Operand type | Result type | Example   
---|---|---|---|---  
+ | pointer addition | character pointer, integer | character pointer | P + I   
- | pointer subtraction | character pointer, integer | character pointer | P - Q   
^ | pointer dereference | pointer | basic type of pointer | P^   
= | equal | pointer | boolean | P = Q   
<> | not equal | pointer | boolean | P <> Q   
  
## The priority of the operators

There is a rigid sequence for processing the operators. 

Operators | Priority | Category   
---|---|---  
Not @ | first (highest) | Unary operators   
* / div mod and shl shr as << >> | second | Multiplication operators   
\+ - or xor | third | Addition operators   
= <> < > <= >= in is | fourth (lowest) | Relational operators   
  
## See also

  * [Pascal basics](<Pascal_basics.md> "Pascal basics")

---

_Source: [https://wiki.freepascal.org/Operators](https://web.archive.org/web/20250419150707/https://wiki.freepascal.org/Operators)_
