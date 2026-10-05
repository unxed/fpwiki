# Scanner/Tokenizer

│ **English (en)** │

back to contents [FPC internals](<../FPC_internals.md> "FPC internals")

## Contents

  * 1 Scanner/Tokenizer
    * 1.1 Architecture
      * 1.1.1 Input stream
      * 1.1.2 Preprocessor
        * 1.1.2.1 Conditional compilation (scandir.inc, scanner.pas)
        * 1.1.2.2 Compiler switches (scandir.inc, switches.pas)
    * 1.2 Scanner interface
      * 1.2.1 Routines
        * 1.2.1.1 ReadToken
      * 1.2.2 Variables
        * 1.2.2.1 Token
        * 1.2.2.2 Pattern
    * 1.3 Assembler parser interface
      * 1.3.1 Routines
        * 1.3.1.1 AsmGetChar



# Scanner/Tokenizer

The scanner and tokenizer is used to construct an input stream of tokens which will be fed to the parser. It is in this stage that the preprocessing is done, that all read compiler directives change the internal state variables of the compiler, and that all illegal characters found in the input stream cause an error. 

Info about how macros work: [Macro internals](<../Macro_internals.md> "Macro internals")

## Architecture

The general architecture of the scanner is shown in the following figure: 

<http://www.pjh2.de/fpc/CompilerInternalsFigure02.png>

Several types can be read from the input stream, a string, handled by `readstring`, a numeric value, handled by `readnumeric`, comments , compiler and preprocessor directives. 

### Input stream

_(last updated for fpc version 1.0.x)_

The input data is handled via the standard way of handling all the I/O in the compiler. That is to say, that it is a hook which can be overriden in **comphook.pas (do_openinputfile)** , in case where another I/O method wants to be used. 

The default hook uses a non-buffered dos stream contained in **files.pas**

### Preprocessor

_(last updated for fpc version 1.0.x)_

The scanner resolves all preprocessor directives and only gives to the parser the visible parts of the code (such as those which are included in conditional compilation). Compiler switches and directives are also saved in global variables while in the preprocessor, therefore this is part is completely independent of the parser. 

#### Conditional compilation (scandir.inc, scanner.pas)

_(last updated for fpc version 1.0.x)_

The conditional compilation is handled via a preprocessor stack, where each directive is pushed on a stack, and popped when it is resolved. The actual implementation of the stack is a linked list of preprocessor directive items. 

#### Compiler switches (scandir.inc, switches.pas)

_(last updated for fpc version 1.0.x)_

The compiler switches are handled via a lookup table which is linearly searched. Then another lookup table takes care of setting the appropriate bit flags and variables in the switches for this compilation process. 

## Scanner interface

_(last updated for fpc version 1.0.x)_

The parser only receives tokens as its input, where a token is a enumeration which indicates the type of the token, either a reserved word, a special character, an operator, a numeric constant, string, or an identifier. 

Resolution of the string into a token is done via lookup which searches the string table to find the equivalent token. This search is done using a binary search algorithm through the string table. 

In the case of identifiers, constants (including numeric values), the value is returned in the **pattern** string variable , with the appropriate return value of the token (numeric values are also returned as non-converted strings, with any special prefix included). In the case of operators, and reserved words, only the token itself must be assumed to be preserved. The read input string is assmued to be lost. 

Therefore the interface with the parser is with the **readtoken()** routine and the **pattern** variable. 

### Routines

#### ReadToken

Declaration: | `**procedure** ReadToken;`  
---|---  
Description: | Sets the global variable token to the current token read, and sets the pattern variable appropriately (if required).   
  
### Variables

#### Token

Declaration: | `**var** Token: TToken;`  
---|---  
Description: | Contains the contain token which was last read by a call to ReadToken  
See also: | ReadToken  
  
#### Pattern

Declaration: | `**var** Pattern: String;`  
---|---  
Description: | Contains the string of the last pattern read by a call to ReadToken  
See also: | ReadToken  
  
## Assembler parser interface

_(last updated for fpc version 1.0.x)_

The inline assembler parser is completely separate from the pascal parser, therefore its scanning process is also completely independent. The scanner only takes care of the preprocessor part and comments, all the rest is passed character per character to the assembler parser via the **AsmGetChar()** scanner routine. 

### Routines

#### AsmGetChar

Declaration: | `**function** AsmGetChar: Char;`  
---|---  
Description: | Returns the next character in the input stream.   
  
  
Next chapter: [The parse tree](<../The_parse_tree.md> "The parse tree")

---

_Source: [https://wiki.freepascal.org/Scanner/Tokenizer](https://web.archive.org/web/20230530121141/https://wiki.freepascal.org/Scanner/Tokenizer)_
