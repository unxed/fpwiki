# Why Pascal is Not My Favorite Programming Language

"Why Pascal is Not My Favorite Programming Language" is an article (can be found [here](<http://www.lysator.liu.se/c/bwk-on-pascal.html>)) written about by Brian W. Kernighan, April 2, 1981. The article is often used in language comparison discussions even today, typically by critics of the Pascal-family, despite the fact that the article is talking about a version of Pascal that has been superseded. The description of the language that B Kernighan is referring to in the article can be found [here](<http://web.archive.org/web/20210718153204/http://pascal-central.com/docs/pascal1973.pdf>). 

The purpose of this page, is to show that the article is quite outdated. People who are still trying to refer to the article are unaware of the capabilities of current Pascal (Delphi/FPC) implementations. 

## Contents

  * 1 Genesis
  * 2 Article
    * 2.1 Types and Scopes
    * 2.2 The size of an array is part of its type
    * 2.3 There are no static variables and no initialization
    * 2.4 Related program components must be kept separate
    * 2.5 There is no separate compilation
    * 2.6 Some miscellaneous problems of type and scope
    * 2.7 There is no escape
  * 3 Control Flow
  * 4 The Environment
  * 5 Cosmetic Issues
  * 6 Perspective



## Genesis

## Article

### Types and Scopes
    
    
    Integer variables may be declared to have an associated range of legal values, and the compiler and run-time support 
    ensure that one does not put large integers into variables that only hold small ones.  This too seems like a service,
    although of course run-time checking does exact a penalty. 
    

It needs to be noted here, that compiler allows to remove the range run-time check completely. 

### The size of an array is part of its type

Without quoting of the article - the complain is about an arrays of different size (though still the same type) are considered a different type. Since Pascal is strict type routines cannot be shared among array of different size. The task of sorting is used as an example in the article. 

The 1985 Ansi/ISO standards have an (optional) section about conformant arrays fixing this. Currently neither FPC (ISO mode) nor Borland derivatives implement it. 

[Dynamic](<http://www.freepascal.org/docs-html/ref/refsu18.htm>) and [Open arrays](<http://www.freepascal.org/docs-html/ref/refsu64.html>) were introduced in Delphi that overcomes the limitation at the cost of the lower bound always being 0. The desired result could be achieved even back at the time, using pointers. 

### There are no static variables and no initialization

As well as Static variables are just "implementation"-only variables per unit. Global variable initialization were added in some version of Delphi, as well as type constants (which are variables) requires the initialization to be present. 

Also, each unit provides an initialization section (which is executed in run-time), that provides a very flexible and convenient way for initialization (of dynamic storages). 

### Related program components must be kept separate

No such strict order limitation in Object Pascal. 

### There is no separate compilation

Fixed mid eighties by the module(ISO)/units(UCSD/Borland) system in a way that doesn't need a separate interpreter (make) to manage building and without the infinate header reparsing problem that causes C and C++ compilers to be relatively slow in practice. 

### Some miscellaneous problems of type and scope
    
    
    It is not legal to name a non-basic type as the literal formal parameter of a procedure; 
    

This is actually still true. 
    
    
    It is nice to have the declaration 'var' for formal parameters of functions and procedures; 
    the procedure clearly states that it intends to modify the argument.  But the calling program 
    has no way to declare that a variable is to be modified - the information is only in one place,
    while two places would be better.
    

[const](<http://www.freepascal.org/docs-html/ref/refsu63.html>) is available to identified unmodified parameter and passing a parameter by reference (where applicable) 
    
    
    Pascal's 'set' construct seems like a good idea, providing notational convenience and some free type checking.  
     For example, a set  of tests like
    
        if (c = blank) or (c = tab) or (c = newline) then ...
    
     can be written rather more clearly and perhaps more efficiently as
    
        if c in [blank, tab, newline] then ..
    

And [they're](<http://www.freepascal.org/docs-html/ref/refsu20.html>)! 

### There is no escape

Type casting is available (and causing issues for some developers) 

## Control Flow
    
    
    There is no guaranteed order of evaluation of the logical operators 'and' and 'or' - nothing like && and || in C.  
    This failing, which is shared with most other languages, hurts most often in loop control:
    
        while (i <= XMAX) and (x[i] > 0) do ...
    

Conditions are [short-cut evaluated](<http://www.freepascal.org/docs-html/ref/refsu53.html>). Thus loops can be implemented the way above without a risk breaking out of the range. 
    
    
    With no 'break' statement...
    

There's [break](<http://www.freepascal.org/docs-html/ref/refsu57.htm>)
    
    
    The increment of a 'for' loop can only be +1 or -1, a minor restriction.
    

Still true. 
    
    
    There is no 'return' statement, again for one in-one out reasons. 
    

There's Exit statement (FPC only Exit() ) available. 
    
    
    The 'case' statement is better designed than in C, except that there is no 'default' clause and the behavior 
    is undefined if the input expression does not match any of the cases.  This crucial omission renders the 'case' construct 
    almost worthless.
    

There's [else (or otherwise)](<https://www.freepascal.org/docs-html/current/ref/refsu56.html#x166-19000013.2.2>) in case statement. 

## The Environment

No problems with using System APIs and any I/O routines provided by an underlying environment. 

## Cosmetic Issues
    
    
    But if something must be inserted before b, it no longer needs a semicolon, because it now precedes an 'end': 
    

A statement before "end" might have or not have a semicolon. Not-having a semicolon is not enforced. 
    
    
    C and Ratfor programmers find 'begin' and 'end' bulky compared to { and }.
    

Is still true and there're no plans to change that. 
    
    
    A function name by itself is a call of that function; there is no way to distinguish such a function call from a simple variable 
    except by knowing the names of the functions
    

Is still true, except for FPC/objFPC mode, where a function call must come with () 
    
    
    In particular, there are no bit-manipulation operators (AND, OR, XOR, etc.).  
    I simply gave up trying to write the following trivial encryption program in Pascal:
    
        i := 1;
        while getc(c) <> ENDFILE do begin
                putc(xor(c, key[i]));
                i := i mod keylen + 1
        end
    

Bit-manipulation [operators](<https://www.freepascal.org/docs-html/current/ref/refsu45.html>) are available. 
    
    
    There is no null string, perhaps because Pascal uses the doubled quote notation to indicate a quote embedded in a string
     
       'This is a _character'_
    
    There is no way to put non-graphic symbols into strings.  In fact, non-graphic characters are unpersons in a stronger 
    sense, since   they are not mentioned in any part of the standard language.  Concepts like newlines, tabs, and so on 
    are handled on each system in an  'ad hoc' manner, usually by knowing something about the character set 
    (e.g., ASCII newline has decimal value 10)
    

Escape characters are available. 
    
    
     'new line'#13#10
    

Needless to say, that Unicode escaping is available too. 
    
    
    There is no macro processor.
    

God bless Pascal! 
    
    
    The 'const' mechanism for defining manifest constants takes care of about 95 percent of the uses of simple 
    #define statements in C, but more involved ones are hopeless.  It is certainly possible to put a macro 
    preprocessor on a Pascal compiler.  This allowed me to simulate a sensible 'error' procedure as
    
        #define error(s)begin writeln(s); halt end
    

Obviously the error(s) should implemented as a function. Since nothing is executed after Halt(). Though these days "raising" an exception might be a better approach. 
    
    
    The language prohibits expressions in declarations, so it is not possible to write things like 
      const   SIZE = 10;
      type    arr = array [1..SIZE+1] of integer;
    

[Constant expressions](<http://www.freepascal.org/docs-html/ref/refse9.html>) are allowed as value of an expression. 

## Perspective
    
    
    To close, let me summarize the main points in the case against Pascal. 
    1. 2. 3. 4. 5. 6. 7. 8.  ...
    9. There's no escape 
    
    This last point is perhaps the most important.  The language is inadequate but circumscribed, because there 
    is no way to escape its limitations.  There are no casts to disable the type-checking when necessary.  
    There is no way to replace the defective run-time environment with a sensible one, unless one controls 
    the compiler that defines the ``standard procedures._The language is closed._
    

Modern Pascals are easily extensible, but still, if you don't like the run-time environment. You can introduce your own. After all, FPC is open source.

---

_Source: [https://wiki.freepascal.org/Why_Pascal_is_Not_My_Favorite_Programming_Language](https://web.archive.org/web/20250504211414/https://wiki.freepascal.org/Why_Pascal_is_Not_My_Favorite_Programming_Language)_
