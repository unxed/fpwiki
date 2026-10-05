# Basic Pascal Tutorial/Chapter 1/Identifiers

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_1/Identifiers/bg> "Basic Pascal Tutorial/Chapter 1/Identifiers/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_1/Identifiers/de> "Basic Pascal Tutorial/Chapter 1/Identifiers/de")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_1/Identifiers/fr> "Basic Pascal Tutorial/Chapter 1/Identifiers/fr")** │  **[italiano (it)](</Basic_Pascal_Tutorial/Chapter_1/Identifiers/it> "Basic Pascal Tutorial/Chapter 1/Identifiers/it")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_1/Identifiers/ja> "Basic Pascal Tutorial/Chapter 1/Identifiers/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Chapter_1/Identifiers/ko> "Basic Pascal Tutorial/Chapter 1/Identifiers/ko")** │  **[русский (ru)](<../../../ru/Basic_Pascal_Tutorial/Chapter_1/Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers/ru")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_1/Identifiers/zh_CN> "Basic Pascal Tutorial/Chapter 1/Identifiers/zh CN")** │    
****

[ ◄ ](<Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants")  
---|---|---  
  
1B - Identifiers (author: Tao Yue, state: changed) 

Identifiers are names that allow you to reference stored values, such as variables and constants. Also, every program must be identified (get it?) by an identifier. 

Rules for identifiers: 

  * Must begin with a letter (a..z or A..Z, Pascal is case insensitive) from the English alphabet or an underscore (_).
  * Can be followed by zero or more letters (a..Z), digits (0..9), or underscores (_), in any combination.
  * Cannot be the same as a [keyword](<../../Keyword.md> "Keyword") such as `begin`, `for`, `case`, `absolute` etc.
  * May not contain special characters, such as:


    
    
     ~ ! @ # $ % ^ & * ( ) + ` - = { } [ ] : " ; ' < > ? , . / | \ (or the space character)
    

## Contents

  * 1 Reserved words
    * 1.1 Turbo Pascal reserved words
    * 1.2 Delphi reserved words
    * 1.3 Free Pascal reserved words



# Reserved words

Several identifiers are reserved in Pascal -- you cannot use them as your own identifiers. According to the [FPC Reference](<http://lazarus-ccr.sourceforge.net/fpcdoc/ref/ref.html>) they are grouped in: 

  * Turbo Pascal reserved words
  * Delphi reserved words
  * FPC reserved words



### Turbo Pascal reserved words

absolute | and | array | asm | begin | break | case | const   
---|---|---|---|---|---|---|---  
constructor | continue | destructor | div | do | downto | else | end   
file | for | function | goto | if | implementation | in | inherited   
inline | interface | label | mod | nil | not | object | of   
on | operator | or | packed | procedure | program | record | reintroduce   
repeat | self | set | shl | shr | string | then | to   
type | unit | until | uses | var | while | with | xor   
  
### Delphi reserved words

The Delphi (II) reserved words are the same as the pascal ones, plus the following ones: 

as | class | except | exports | finalization | finally | initialization   
---|---|---|---|---|---|---  
is | library | on | property | raise | threadvar | try   
  
### Free Pascal reserved words

On top of the Turbo Pascal and Delphi reserved words, Free Pascal also considers the following as reserved words: 

dispose | exit | false | new | true | break | continue   
---|---|---|---|---|---|---  
  
Also, Pascal has several pre-defined identifiers. You can replace them with your own definitions, but then you'd be deleting part of the functionality of Pascal. 

abs | arctan | boolean | char | cos | dispose | eof | eoln   
---|---|---|---|---|---|---|---  
exp | false | input | integer | ln | maxint | new | odd   
ord | output | pack | page | pred | read | readln | real   
reset | rewrite | round | sin | sqr | sqrt | succ | text   
true | trunc write | writeln   
  
Pascal is not case sensitive! `MyProgram`, `MYPROGRAM`, and `mYpRoGrAm` are equivalent. But for readability purposes, it is a good idea to use meaningful capitalization! 

There are two possible methods you could choose to apply to your identifiers: CamelCase and underscore as space. CamelCase, as it appears, means that separate words in an identifier are capitalized, so that you have `newPerson` or `NewPerson` instead of `newperson`. Using underscore as space means you separate words in an identifier with underscores, so that you have `new_person` instead of `newperson`. Or you could combine the two, so that you have `new_Person` or `New_Person` instead of `newperson`. 

Identifiers can be any length, but many Pascal compilers will only look at the first 32 characters or so. That is, 
    
    
        ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFAlphaBeta
        ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGammaDelta
    

may be equivalent to some Pascal compilers because the differences begin in the 33rd character. Free Pascal limits [identifiers](<http://www.freepascal.org/docs-html/3.0.0/ref/refse4.html#x16-150001.4>) to 127 characters. 

This is extremely generous. The original Pascal compiler for the CDC 6000 mainframe only noticed the first 10 characters of an identifier. This was because the CDC had a 60 bit word, and by using 6 bit characters (all upper case letters plus digits and some punctuation) an identifier could fit in one word. You could have more than 10 characters in an identifier, but only the first 10 counted, so `ThisIsObviouslyAVeryLongName` and `ThisIsObviouslyAnEvenLongerName` would be considered the same. 

To make your code compilable by all compilers, use a reasonable length for identifiers -- up to 15 characters. That way, you'll also save on typing. 

While it is a good idea to make identifiers to be mnemonic with the use of longer names, there is nothing wrong with using very short identifiers in specific uses. it is extremely common to use `I, J, `and `K` as the control variable in a [for](<../../For.md> "For") loop. 

  


[ ◄ ](<Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Identifiers](https://web.archive.org/web/20250401031625/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Identifiers)_
