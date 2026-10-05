# Passing Pascal Types to C Routines

│ **English (en)** │  **[français (fr)](</Passing_Pascal_Types_to_C_Routines/fr> "Passing Pascal Types to C Routines/fr")** │    
****

## Issue

A number of standard Pascal types do not have equivalents in C. Nevertheless, sometimes one may want to pass a variable of such a type to an external C routine. The question is how to pass such variables to such routines, given that it is impossible to write an equivalent C program to check what C compilers do. 

We therefore have to define our own standards, in order to ensure that the same conventions are followed on all supported architectures (which was not the case when this article was started). Note that the table below only applies to functions declared as "cdecl", Pascal functions (may) use other calling conventions. 

## Overview

Type | How passed in 2.0.2 | How passed in 2.1.1+ | How it should be passed | Rationale   
---|---|---|---|---  
Shortstring  | By value on x86, by reference elsewhere | By reference | By reference | Apple's Universal Interfaces have several routines expecting Pascal strings as value parameters. They are declared as arrays of char on the C side, which have to be passed by reference. A downside of passing them by reference is that the call-by-value semantics cannot be guaranteed (i.e., the C-routine can modify the string)   
Small sets (<= 32 elements)  | By value | By value | By value | Since such sets are 4 bytes large, it does not make sense to pass them by reference   
Large sets (32 < x < 256 elements)  | By value on x86, by reference elsewhere | By value on x86, by reference elsewhere | ? | A similar argument as for shortstrings could be made, in that the C equivalent will probably be an array. In either case, it should become the same for x86 and the rest.   
Method pointer  | By value on x86, by reference elsewhere | By value on x86, by reference elsewhere | ? | ? Should become the same on x86 and the rest.

---

_Source: [https://wiki.freepascal.org/Passing_Pascal_Types_to_C_Routines](https://web.archive.org/web/20250115190802/https://wiki.freepascal.org/Passing_Pascal_Types_to_C_Routines)_
