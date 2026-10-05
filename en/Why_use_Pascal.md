# Why use Pascal

“A low level language is one whose programs require attention to the irrelevant.” 

## Contents

  * 1 Introduction
    * 1.1 What is Pascal?
    * 1.2 The Readln and Writeln effect
  * 2 Pros and Cons
    * 2.1 Pros
      * 2.1.1 Strong type safety
      * 2.1.2 Well structured and very strict syntax
      * 2.1.3 Native String support
      * 2.1.4 Parameter passing
      * 2.1.5 Native set operators
      * 2.1.6 Readability
      * 2.1.7 Principle of scope
      * 2.1.8 Case-insensitive
      * 2.1.9 Other
    * 2.2 Cons
      * 2.2.1 Reduced popularity
      * 2.2.2 More usage of reserved words than reserved characters
      * 2.2.3 Language corset
  * 3 See also
  * 4 External links



## Introduction

Pascal often comes under attack as a language which should be dead, or as a language not suitable for very much. 

This document will discuss these claims and add the latest information about Pascal as of 2013 and beyond. 

### What is Pascal?

[Pascal](<Pascal.md> "Pascal") is a very clean programming language, which looks more like real languages in the sense that it uses real English words as keywords rather than random ASCII characters. This is important in understanding existing code as well as debugging because people do not read individual characters but whole words. 

A common misconception is that Pascal started as a teaching language. While this is partially true, the associations are usually wrong; usually that makes people expect systems like Logo, limited playing grounds for children. 

Pascal, like its predecessor ALGOL(‑60), however was primarily designed as a language for formal specification and teaching of algorithms, mostly with future engineers and computer scientists as target. Contrary to ALGOL‑68, emphasis was put on simplicity. This turned out to be beneficial for compiler construction. 

The initial Pascal dialects had a series of serious ugliness (like untyped procedure variables) that were quickly remedied, way before the language's prime time in the eighties (revised J&W and early standardization trajectory). 

Further modernization and facilities for interfacing to lower level systems were added to nearly every dialect. Most of these were not standardized back into the language, but this was normal at the time (way before C and POSIX standards). However the dialects were not entirely random, and could be classified into two major streams, UCSD/Borland like and ISO standards compliant, with Apple creating a hybrid between the two (UCSD in origin, but incorporating most level 1 ISO features). 

Over the years especially the Borland stream language has matured and gained all capabilities necessary for up-to-date large scale software projects (for example the Free Pascal compiler or the Lazarus IDE). 

The particular strength of Pascal is that most development time is spent on the program itself, contrary to C and C++ like languages, where the developer needs to focus on managing the memory of variables or the structure of very basic things like passing parameters and returning them back again. 

As a result, Pascal developers do not have to learn a new sub-language inside the same language, like C++, STL, MFC. 

### The Readln and Writeln effect

Most developers that touched Pascal did not like the language, because they only learned some very basic commands and how to write a more structured code than their mind was thinking at the time. 

That is, why languages such as C and Perl, for example, have tended to win the popularity contests. While Pascal seems very basic and very minimalistic, when you uncover the true language, you find that it is much easier to create a program in Pascal than in C, Java and other popular languages. Even languages such as Python, while popular and still remains structured, have many elements of a disoriented language. That issue arrives first of all from the attempt to create the most “perfect” programming language, that will be easy to use, and have the cleanest way to create things. 

## Pros and Cons

As always: Choosing what programming language to use depends on your program you would like to program. 

### Pros

#### Strong type safety

In C you can add a character to a real and interpret it as a boolean (or whatsoever). Why the heck should I do this? In Pascal that is not possible (only with force). The compiler will raise an error. 

#### Well structured and very strict syntax

For example a function in C looks like this: 

  1. function signature
  2. declaration of variables … and definition of function may be mixed up.



In Pascal it is clear: 

  1. function/procedure signature
  2. declaration of variables …
  3. definition of function/procedure/program



The "For .. to .." statement in Pascal is directly readable. In C it is only readable for someone knowing the C-syntax by head. Many mistakes are caused by this bad syntax of C. 

In C both 

  1. a = b and
  2. a == b



are frequently (and often unintentionally) used valid Boolean expressions. With a total different meaning: the first expression is an assignment too! 

In Pascal 

  1. a = b is Boolean and not an assignment
  2. a == b does not exist
  3. a := b is an assignment and not a Boolean.



In C the keyword Break must be inserted at each Case section in a Switch statement. In Pascal the syntax of the "Case ... of" statement is strict and avoids unexpected results. 

In Pascal, there is a strict difference between a pointer and a string. In C, the pointer to a string and a string can be intermixed, leading to unexpected results after compilation. 

This, and more, ambiguity in C leads to very frequent fatal errors. And costly repair during development. 

#### Native String support

Pascal supports definition of and manipulation with strings. C does not support that. You can emulate strings by defining an array [ ] of chr. Manipulations should be done by Library functions. 

#### Parameter passing

Pascal supports variable parameter passing. C supports only constant parameter passing and forces you to use the weak type controlled pointer passing to manipulate a variable through a function/procedure. 

#### Native set operators

Since Mr. Wirth (the father of the Pascal language) has a background in mathematics, where [sets](<Set.md> "Set") (containers of units called “elements”) are a fundamental tool, he included operators for handling with sets, too. We can check, whether an element is in a set via the [`in` operator](<In.md> "In"). We can `include`, `exclude` elements, compare sets (difference, [symmetric difference](<symmetric_difference.md> "symmetric difference")), combine and intersect them. Knowing where to use sets, makes your code incredibly more readable! There is no need to include any extra class or unit for that, it is already part of the language. However, [FPC](<FPC.md> "FPC")’s current implementation of sets allows 256 elements top per set. You cannot do mathematics in a large scale with that, though. 

#### Readability

You can write Pascal as you speak (or at least it is some more natural): 
    
    
    if x = true then
    begin
      writeline('hello world' );
    end
    

Counter example: 
    
    
    if (x == true)
    { 
       printf( 'hello world\n');
    }
    

The counter example shows that you may get lost if you do not know what brackets and braces and '=' versus '==' mean. 

#### Principle of scope

There are many chances to limit the validity of [identifiers](<Identifier.md> "Identifier") – to limit their scope. Encapsulating functions/procedures or limiting a variable’s/constant’s/type’s scope is done implicitly: Depending where you declare such things, they are only known on the same or lower levels. 

#### Case-insensitive

A function defined with the name getLimit will be called though you forgot the upper-case-L in the middle ( (calling getlimit()). It does not raise a compile-time-error. 

#### Other

Pascal has OO implementations, like other languages have. Pascal can also be applied to many platforms, even like embedded systems, Arduino, PI and Android. 

### Cons

#### Reduced popularity

Finding people for software projects who already have experience in Pascal can become difficult. It could be harder to find suitable library solutions for specific problems. 

#### More usage of reserved words than reserved characters

Sometimes writing all the reserved words (like `begin`, `end`, `then`, …) feels uncomfortable. Other languages just use parentheses or curly braces – that is just a single letter. Editor functions like “match-corresponding parenthesis/brace” or in Pascal-context “match corresponding `begin`/`end`” are harder to find. Most Pascal-dialects however support { } in stead of "Begin ... end"...... 

#### Language corset

On multiple occasions programmers may encounter boundaries the language sets (e.g. the strong type and syntax safety mentioned above). These measures were set for good reasons, though: To prevent programmers from doing stupid stuff. 

**That's actually a pro point!**

However, if you do not care about bad practices, you might get annoyed by that fact. 

## See also

  * [FPC Advantages](<FPC_Advantages.md> "FPC Advantages")
  * [/ Real Programmers Don't Use Pascal](<https://www.pbm.com/~lindahl/real.programmers.html>): the ultimate ironical and scientific proof that Pascal is the best of all languages.



## External links

  * [Pascal Central](<https://web.archive.org/web/20211019221607/http://www.pascal-central.com/>), a repository with technical information, tools, references, source code, internet links, and more.
  * [StandardPascal.org](<https://www.StandardPascal.org>), reference information about the ANSI ISO 7185 standard.
  * [ISO 7185:1990](<https://www.ISO.org/standard/13802.html>), official normative version of the Pascal standard.
  * [ISO/IEC 10206:1991](<https://www.ISO.org/standard/18237.html>): [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") standard

---

_Source: [https://wiki.freepascal.org/Why_use_Pascal](https://web.archive.org/web/20241213023227/https://wiki.freepascal.org/Why_use_Pascal)_
