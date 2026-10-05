# The code generator

│ **English (en)** │

back to contents [FPC internals](<FPC_internals.md> "FPC internals")

With the version 1.9.x of FPC the code generator has been completly rewritten to fit the needs of a compiler with multiple processor target support. 

## MM support

The FPC code generator supports the usage of multimedia registers as they are implemented by modern processors. 

### The shuffle operand

Most multimedia units support scalar as well as vector operations on the multimedia registers. To distinguish between these different operations, the mm methods of tcg take a so-called shuffle parameter. This parameter allows to specify how the data is processed when generating multimedia code. 
    
    
      pmmshuffle = ^tmmshuffle;  
      { this record describes shuffle operations for mm operations; if a pointer a shuffle record
        passed to an mm operation is nil, it means that the whole location is moved }
      tmmshuffle = record
        { describes how many shuffles are actually described, if len=0 then
          moving the scalar with index 0 to the scalar with index 0 is meant }
        len : byte;
        { lower nibble of each entry of this array describes index of the source data index while
          the upper nibble describes the destination index }
        shuffles : array[1..1] of byte;
      end;

---

_Source: [https://wiki.freepascal.org/The_code_generator](https://web.archive.org/web/20240420213629/https://wiki.freepascal.org/The_code_generator)_
