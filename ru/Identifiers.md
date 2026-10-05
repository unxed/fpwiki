# Basic Pascal Tutorial/Chapter 1/Identifiers

│ **[English (en)](<../en/Identifiers.md>)** │  **русский (ru)** │

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure/ru") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Basic_Pascal_Tutorial/Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants/ru")  
---|---|---  
  
Идентификаторы

1B - Identifiers (author: Tao Yue, state: changed) 

  
Идентификаторы - это имена, которые позволяют вам ссылаться на сохранённые значения, такие, как переменные и константы. Также, каждая программа должна быть идентифицирована идентификатором. 

Правила для идентификаторов: 

  * Должны начинаться с буквы английского алфавита или подчёркивания (_).
  * Могут быть продолжены алфавитно-цифровыми символами (буквами и цифрами) или подчёркиванием.
  * Не могут содержать специальных символов, таких, как:


    
    
     ~ ! @ # $ % ^ & * ( ) + ` - = { } [ ] : " ; ' < > ? , . / | \
    

## Contents

  * 1 Зарезервированные слова
    * 1.1 Зарезервированные слова Turbo Pascal
    * 1.2 Зарезервированные слова Delphi
    * 1.3 Зарезервированные слова Free Pascal



# Зарезервированные слова

Некоторые идентификаторы зарезервированы в Pascal -- вы не можете использовать их как свои собственные идентфикаторы. В соответствии с [FPC Reference](<http://lazarus-ccr.sourceforge.net/fpcdoc/ref/ref.html>), они сгруппированы следующим образом: 

  * Зарезервированные слова Turbo Pascal
  * Зарезервированные слова Delphi
  * Зарезервированные слова FPC



  


### Зарезервированные слова Turbo Pascal

absolute | and | array | asm | begin | break | case | const   
---|---|---|---|---|---|---|---  
constructor | continue | destructor | div | do | downto | else | end   
file | for | function | goto | if | implementation | in | inherited   
inline | interface | label | mod | nil | not | object | of   
on | operator | or | packed | procedure | program | record | reintroduce   
repeat | self | set | shl | shr | string | then | to   
type | unit | until | uses | var | while | with | xor   
  
  


### Зарезервированные слова Delphi

Зарезервированные слова Delphi (II) - те-же, что и в Turbo Pascal, плюс следующие: 

as | class | except | exports | finalization | finally | initialization   
---|---|---|---|---|---|---  
is | library | on | property | raise | threadvar | try   
  
  


### Зарезервированные слова Free Pascal

В дополнение к зарезервированным словам Turbo Pascal и Delphi, Free Pascal также рассматривает следующее как зарезервированные слова: 

dispose | exit | false | new | true   
---|---|---|---|---  
  
  
Кроме того, Pascal имеет несколько предопределённых идентификаторов. Вы можете заменить их своими собственными объявлениями, но тогда вы потеряете часть функциональности Pascal. 

abs | arctan | boolean | char | cos | dispose | eof | eoln   
---|---|---|---|---|---|---|---  
exp | false | input | integer | ln | maxint | new | odd   
ord | output | pack | page | pred | read | readln | real   
reset | rewrite | round | sin | sqr | sqrt | succ | text   
true | trunc write | writeln   
  
  
Pascal нечувствителен к регистру! MyProgram, MYPROGRAM, и mYpRoGrAm - эквивалентны. Но из соображений читабельности имеет смысл использовать отдельные буквы в верхнем регистре для выделения смысла. 

Идентификаторы могут иметь любую длину, но многие компиляторы Pascal будут учитывать только первые 32 символа, или около того. То есть, идентификаторы: 
    
    
        ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFAlphaBeta
        ABCDEFGHIJKLMNOPQRSTUVWXYZABCDEFGammaDelta
    

могут быть эквивалентными в некоторых компиляторах Pascal, поскольку различия начинаются с 33-го символа. 

Чтобы ваша программа компилировалась на всех компиляторах, используйте разумную длину идентификаторов -- до 15 символов. Кроме того, таким образом вы сэкономите на печати. 

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure/ru") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Basic_Pascal_Tutorial/Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Identifiers/ru](https://web.archive.org/web/20230331160557/https://wiki.freepascal.org/Identifiers/ru)_
