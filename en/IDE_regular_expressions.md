# IDE regular expressions

│ **English (en)** │

## Contents

  * 1 Normal regular expression
  * 2 Simple Syntax
    * 2.1 Examples for simple syntax
  * 3 Quoted symbols
  * 4 Search and Replace with regular expressions



## Normal regular expression

Metacharacters: 
    
    
       \   Quote the next metacharacter
       ^   Match the beginning of the line
       .   Match any character (except newline). Example: 'a.c' matches 'abc', 'aBC', 'axc', 'a3c', 'a$c', etc.
       $   Match the end of the line (or before newline at the end)
       |   Alternation
       ()  Grouping. For example: (abc)+ matches 'abc' or 'abcabc' or 'abcabcabc' etc.
       []  Character class. For example [abc], [a-z], [a-zA-Z0-9_]
       [^] Reversed character class. For example [^a-z]
    

Quantifiers: 
    
    
       *      Match 0 or more times
       +      Match 1 or more times
       ?      Match 1 or 0 times
       {n}    Match exactly n times
       {n,}   Match at least n times
       {n,m}  Match at least n but not more than m times
    

Curly brackets in any other context is treated as a regular character. The ``*_is equivalent to {0,}, the ``+_ to {1,} and the ``?_to {0,1}._

By default, a quantifier is _greedy_ , that means, it will match as many times as possible. To match the minimum number of times possible, append a ``?_._
    
    
       *?     Match 0 or more times
       +?     Match 1 or more times
       ??     Match 0 or 1 time
       {n}?   Match exactly n times
       {n,}?  Match at least n times
       {n,m}? Match at least n but not more than m times
    

## Simple Syntax

Some IDE dialogs provide a checkbox to choose between _simple syntax_ and _regular expressions_. The _simple syntax_ is often shorter for common file name filters. 

Technically it does this: 
    
    
      The following characters are replaced with
      . -> \.
      * -> .*
      ? -> .
      , -> |
      ; -> |
      
      Finally enclose by ^( )$
    

### Examples for simple syntax

  * *.pas - matches all file names ending with .pas, like Unit1.pas or Some.txt.pas. Default is to match case sensitive. So it does not fit unit1.PAS.
  * *.pas;*.pp;*p - matches all files like unit1.pas, 678.pp or %.p
  * unit*.pas - matches all files beginning with _unit_ and ending with _.pas_ like unit1.pas and unit.pas.
  * unit?.pas - matches all files beginning with _unit_ , ending with _.pas_ and exactly one like arbitrary character in between like unit1.pas and unit2.pas.



## Quoted symbols

The backslash '\' can be used to treat special characters like '.' as normal character or to get some special ranges. 
    
    
      \. matches a point '.'
      \d matches a number character '0'..'9'
      \D matches a non number character
      \s matches a space character ' ',#9,#10,#12,#13
      \S matches a non space character
      \w matches a word character 'a'..'z','A'..'Z','0'..'9','_'
      \W matches a non word character
      \b word boundary
      \B not word boundary
      \A begin of line
      \Z end of line
    

## Search and Replace with regular expressions

The find dialogs support regular expressions for finding and replacing. Each found pattern grouped in round brackets can be used by a $1, ..., $9 variable 

Example 1: 

  * Find expression: a(.*)c
  * Replace expression: A$1C
  * Text: 'abc aLazc'



The $1 will be replaced with the found text, that matches the first bracket. 

  * Result: 'AbC ALazC'



Example 2: 

  * Find expression: T(prefix)(\w+)
  * Replace expression: T$2Suffix
  * Text: replaced from 'TprefixClass' to 'TClassSuffix'



You can also use the following special chars in the *replace* term 

\n
    Inserts a Linebreak
\l \u
    Lower or Uppercases the *next* Char. Use this in front of $n. \u$1 Will insert the match from the first brackets, but with its first char made uppercase
\L \U
    Lower or Uppercase the *full* text of the next $n. The $n must follow immediately

---

_Source: [https://wiki.freepascal.org/IDE_regular_expressions](https://web.archive.org/web/20250113134904/https://wiki.freepascal.org/IDE_regular_expressions)_
