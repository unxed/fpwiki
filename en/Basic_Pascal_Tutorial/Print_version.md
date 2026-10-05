# Basic Pascal Tutorial/Print version

## Contents

  * 1 Introduction
  * 2 History of Pascal
    * 2.1 Origins
    * 2.2 Wirth Invents Pascal
    * 2.3 UCSD Pascal
    * 2.4 Pascal Becomes Standard
    * 2.5 Extensions
    * 2.6 The World Changes
    * 2.7 So what are the advantages of learning Pascal?
  * 3 Pascal Compilers
    * 3.1 About Computer Languages and Compilers
    * 3.2 More About Compilers
    * 3.3 Open Source Compilers
    * 3.4 See also
  * 4 Hello, world
  * 5 Program Structure
  * 6 Identifiers
  * 7 Reserved words
    * 7.1 Turbo Pascal reserved words
    * 7.2 Delphi reserved words
    * 7.3 Free Pascal reserved words
  * 8 Constants
    * 8.1 Scalar constants
    * 8.2 Record constants
    * 8.3 Array constants
      * 8.3.1 One-dimensional arrays
        * 8.3.1.1 One-dimensional array of record
      * 8.3.2 Two-dimensional array constant
      * 8.3.3 Three-dimensional array constant
  * 9 Variables and Data Types
  * 10 Assignment and Operations
  * 11 Standard Functions
  * 12 Punctuation and Indentation
  * 13 Programming Assignment
  * 14 Solution
  * 15 Input
  * 16 Output
  * 17 Formatting output
  * 18 Files
  * 19 EOLN and EOF
  * 20 Programming Assignment
  * 21 Solution
  * 22 Sequential control
  * 23 Boolean Expressions
  * 24 Branching
    * 24.1 Format
    * 24.2 One-way branch
    * 24.3 Two-way branch
    * 24.4 Format
    * 24.5 One-way branch
    * 24.6 Two-way branch
  * 25 Looping
    * 25.1 FOR...DO Loops (author: Tao Yue, state: changed)
      * 25.1.1 Loops
      * 25.1.2 FOR...DO Loop
    * 25.2 See also
    * 25.3 FOR...DO Loops (author: Tao Yue, state: changed)
      * 25.3.1 Loops
      * 25.3.2 FOR...DO Loop
    * 25.4 See also
    * 25.5 While ... DO loops
    * 25.6 See also
  * 26 Programming Assignments: Fibonacci Sequence and Powers of Two
  * 27 Solutions
  * 28 Procedures
  * 29 Parameters
  * 30 Functions
  * 31 Scope
  * 32 Recursion
  * 33 Forward Referencing
  * 34 Programming Assignment: the Towers of Hanoi
  * 35 Solution
  * 36 Enumerated types
  * 37 Subranges
  * 38 1-dimensional arrays
  * 39 Multidimensional arrays
  * 40 Records
  * 41 Pointers
    * 41.1 Memory routines
    * 41.2 Trivial usage example: singly linked lists



# Introduction

│ **English (en)** │

[ ◄ ](</index.php?title=Basic_Pascal_Tutorial/Basic_Pascal_Tutorial&action=edit&redlink=1> "Basic Pascal Tutorial/Basic Pascal Tutorial \(page does not exist\)") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<History.md> "Basic Pascal Tutorial/History")  
---|---|---  
  
Introduction (author: Tao Yue, state: negligible changes) 

Welcome to Learn Pascal! This tutorial is a simple, yet complete, introduction to the Pascal programming language. It covers all of the syntax of standard Pascal, including pointers. If you're in a rush to get started, or if you're searching for information on a specific feature of Pascal, you can go directly to the Table of Contents to select any lesson in the tutorial. 

I have tried to make things as clear as possible. If you don't understand something, try it out in your Pascal compiler and tweak things a bit. Pascal is a syntactically-strict language. This means that if you make a mistake, the compiler will stop and inform you of the error. Except when you're using files, there's practically no way for you to completely screw up your computer. 

If you'd like to go into more depth or learn about topics beyond basic data structures, you can purchase books and magazines about Pascal programming which are listed in the wiki page [Pascal and Lazarus Books and Magazines](<../Pascal_and_Lazarus_Books_and_Magazines.md> "Pascal and Lazarus Books and Magazines")

To program in Pascal (or any high-level language, for that matter), you will need a compiler. You can use any standard Pascal compiler with this tutorial. If you need to download a compiler, or if you're new to programming altogether and don't know what a compiler does, check out the [Compiler](<../Compiler.md> "Compiler") page. 

[ ◄ ](</index.php?title=Basic_Pascal_Tutorial/Basic_Pascal_Tutorial&action=edit&redlink=1> "Basic Pascal Tutorial/Basic Pascal Tutorial \(page does not exist\)") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<History.md> "Basic Pascal Tutorial/History")  
---|---|---  
  
# History of Pascal


[ ◄ ](<Introduction.md> "Basic Pascal Tutorial/Introduction") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Compilers.md> "Basic Pascal Tutorial/Compilers")  
---|---|---  
  
History (original author: Tao Yue, state: changed) 

### Origins

Pascal grew out of ALGOL, a programming language intended for scientific computing. Meeting in Zurich, an international committee designed ALGOL as a platform-independent language. This gave them comparatively free rein in the features they could design into ALGOL, but also made it more difficult to write compilers for it. Those were the days when many computers lacked hardware features that we now take for granted. Because many platforms lacked an ALGOL compiler, and ALGOL itself lacked pointers and many basic data types such as characters, the ALGOL language was never widely accepted. Scientists and engineers flocked to FORTRAN, a programming language which was available on many platforms. ALGOL mostly faded away except as a language for describing algorithms. 

### Wirth Invents Pascal

In the 1960s, several computer scientists worked on extending ALGOL. One of these was [Dr. Niklaus Wirth](<../Niklaus_Wirth.md> "Niklaus Wirth") of the Swiss Federal Institute of Technology (ETH-Zurich), a member of the original group that created ALGOL. In 1971, he published his specification for a highly-structured language which resembled ALGOL in many ways. He named it Pascal after the 17th-century French philosopher and mathematician who built a working mechanical digital computer. 

Pascal is very data-oriented, giving the programmer the ability to define custom data types. With this freedom comes strict type-checking, which prevented data types from being mixed up. Pascal was intended as a teaching language, and was widely adopted as such. Pascal is free-flowing, unlike FORTRAN, and reads very much like a natural language, making it very easy to understand code written in it. 

### UCSD Pascal

One of the things that killed ALGOL was the difficulty of creating a compiler for it. Dr. Wirth avoided this by having his Pascal compiler compile to an intermediate, platform-independent object code stage. Another program turned this intermediate code into executable code. 

Prof. Ken Bowles at the University of California at San Diego (UCSD) seized on the opportunity this offered to adapt the Pascal compiler to the Apple II, the most popular microcomputer of the day. UCSD P-System became a standard, and was widely used at universities. This was aided by the low cost of Apple II's compared to mainframes, which were necessary at the time to run other languages such as FORTRAN. Its impact on computing can be seen in IBM's advertisements for its revolutionary Personal Computer, which boasted that the PC supported three operating systems: Digital Research's CP/M-86, Softech's UCSD P-system, and Microsoft's PC-DOS. 

### Pascal Becomes Standard

By the early 1980's, Pascal had already become widely accepted at universities. Two events conspired to make it even more popular. 

First, the Educational Testing Service, the company which writes and administers the principal college entrance exam in the United States, decided to add a Computer Science exam to its Advanced Placement exams for high school students. For this exam, it chose the Pascal language. Because of this, secondary-school students as well as college students began to learn Pascal. Pascal remained the official language of the AP exams until 1999, when it was replaced by C++, which was quickly replaced by Java. 

Second, a small company named Borland International released the Turbo Pascal compiler for the IBM Personal Computer. The compiler was designed by Anders Hejlsberg, who would later head the group at Microsoft that developed C# and (re)introduced Managed Code back to the world of computing. 

Turbo Pascal was truly revolutionary. It did take some shortcuts and made some modifications to standard Pascal, but these were minor and helped it achieve its greatest advantage: speed. Turbo Pascal compiled at a dizzying rate: several thousand lines a minute. At the time, the available compilers for the PC platform were slow and bloated. When Turbo Pascal came out, it was a breath of fresh air. Soon, Turbo Pascal became the de facto standard for programming on the PC. When PC Magazine published source code for utility programs, it was usually in either assembly or Turbo Pascal. 

At the same time, Apple came out with its Macintosh series of computers. As Pascal was the preeminent structured programming language of the day, Apple chose Pascal as the standard programming language for the Mac. When programmers received the API and example code for Mac programming, it was all in Pascal. 

### Extensions

From version 1.0 to 7.0 of [Turbo Pascal](<../Turbo_Pascal.md> "Turbo Pascal"), Borland continued to expand the language. One of the criticisms of the original version of Pascal was its lack of separate compilation for modules. Dr. Wirth even created a new programming language, Modula-2, to address that problem. Borland added modules to Pascal with its units feature. 

By version 7.0, many advanced features had been added. One of these was DPMI (DOS Protected Mode Interface), a way to run DOS programs in protected mode, gaining extra speed and breaking free of the 640K barrier for accessing memory under DOS. Turbo Vision, a text-based windowing system, allowed programmers to create great-looking interfaces in practically no time at all. Pascal even became object-oriented, as version 5.5 adopted the Apple [Object Pascal](<../Object_Pascal.md> "Object Pascal") extensions. When Windows 3.0 came out, Borland created Turbo Pascal for Windows, bringing the speed and ease of Pascal to the graphical user interface. It seemed that Pascal's future was secure. 

### The World Changes

However, this was not to be. In the 1970s, Dennis Ritchie and Brian Kernighan of AT&T Bell Laboratories created the C Programming Language. Ritchie then collaborated with Ken Thompson to design the UNIX operating system. At the time, AT&T had a government-sanctioned monopoly on telephone service in the United States. In return for the monopoly, its telephone business was regulated and it was prohibited from entering the computer business. AT&T, seeing no market for a research operating system, gave UNIX away to universities for free, complete with source code. Thus, a whole generation of computer science students learned C in their university courses on languages and operating systems. Slowly but surely, C began to filter into the computer programming world. 

Pascal took a heavy hit in the 90s when several large companies focused on other programming languages. Microsoft for example focused on Visual Basic and C, and Apple migrated its APIs from Pascal to C and later to Objective C. Despite the lack of support from operating system producers, Pascal still retained a large following through Delphi and Free Pascal. 

### So what are the advantages of learning Pascal?

Despite having lost its previous position of dominance, Pascal is still quite useful, one of its advantages being that it has a very clear syntax which uses common words, such as begin/end, to express concepts, making its code easier to read and maintain. 

Another reason: speed and size. Pascal compilers are lightning-fast and Delphi and Free Pascal are no exceptions. While C programmers might wait for hours, Pascal programmers have to wait only 1 minute for a program of a similar size. Besides that the Pascal IDEs are still leaders in terms of productivity in the world through the [Delphi](<../Delphi.md> "Delphi") IDE and the [Lazarus IDE](<../Lazarus_Faq.md> "Lazarus Faq"). 

Also, Pascal remains preferred at many universities. In addition, Pascal was well-suited for teaching programming, and remains so. There is less overhead and fewer ways for a student to get a program into trouble. For teaching simple procedural programming, Pascal remains a good choice. Pascal has hung on longer in education outside the United States, and remains an official language of the International Informatics Olympiad. A basic programming background is useful in many technical occupations and Pascal is easier to learn than C/C++. 

Today Pascal retains a niche in the market through Delphi, Free Pascal and Lazarus. Many small-scale freeware, shareware, open-source and commercial programs are written in Pascal/Delphi. So enjoy learning it. It's a great introduction to computer programming. It's not dangerous like C, confusing like C++, or slow like Java. 

[ ◄ ](<Introduction.md> "Basic Pascal Tutorial/Introduction") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Compilers.md> "Basic Pascal Tutorial/Compilers")  
---|---|---  
  
# Pascal Compilers


[ ◄ ](<History.md> "Basic Pascal Tutorial/History") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Hello,_World.md> "Basic Pascal Tutorial/Hello, World")  
---|---|---  
  
Pascal Compilers (author: Tao Yue, state: _changed_) 

This document will explain the basics about compilers as well as provide links to well-known Pascal compilers and explain how to set up Free Pascal. 

### About Computer Languages and Compilers

When talking about computer languages, there are basically three major terms that will be used. 

  1. **Machine language** \-- actual binary code that gives basic instructions to the computer's CPU. These are usually very simple commands like adding two numbers or moving data from one memory location to another. For example, on x86, `add eax, 5` becomes `0x83, 0xC0, 0x05`, while `mov ax,bx` becomes `66 89 d8`.
  2. **Assembly language** \-- a way for humans to program computers directly without memorizing strings of binary numbers. There is a one-to-one correspondence with machine code. For example, in Intel x86 machine language, ADD and MOV are mnemonics for the addition and move operations.
  3. **High-level language** \-- permits humans to write complex programs without going step-by step. High-level languages include Pascal, C, C++, FORTRAN, Java, Visual Basic, C#, Java, and many more. One command in a high-level language, like writing a string to a file, may translate to dozens or even hundreds of machine language instructions.



Microprocessors can only run machine language programs directly. Assembly language programs are assembled, or translated into machine language. Likewise, programs written in high-level languages, like Pascal, must also be translated into machine language before they can be run. To do this translation is to compile a program. 

The program that accomplishes the translation is called a **compiler**. This program is rather complex since it not only creates machine language instructions from lines of code, but often also optimizes the code to run faster, adds error-correction code, and links the code with subroutines stored elsewhere. For example, when you tell the computer to print something to the screen, the compiler translates this as a call to a pre-written module. Your code must then be linked to the code that the compiler manufacturer provides before an executable program results. 

With high-level languages, there are again three basic terms to remember: 

  1. Source code -- the code that you write. This typically has an extension that indicates the language used. For example, Pascal source code usually ends in "`.pas`" and C++ code usually ends in "`.cpp`"
  2. Object code -- the result of compiling. Object code usually includes only one module of a program, and cannot be run yet since it is incomplete. On DOS/Windows systems, this usually has an extension of "`.obj`"
  3. Executable code -- the end result. All the object code modules necessary for a program to function are linked together. On DOS/Windows systems, this usually has an extension of "`.exe`"



### More About Compilers

The de facto standard in DOS and Windows-based Pascal compilers is Borland Pascal. Before it came out, most Pascal compilers were clumsy and slow, strayed from the Pascal standard, and cost several hundred dollars. In 1984, Borland introduced Turbo Pascal, which sold for less than $100, compiled an order of magnitude faster than existing compilers, and came with an abundance of source code and utility programs. 

This product was a great success and was prominent for almost a decade. But in the 1990s, the world was moving to Windows. In 1993, the last version of Turbo Pascal, version 7 for DOS, came out. After that, the demand for DOS programs plummeted and Borland (briefly known as Inprise) focused on producing Windows IDE/compilers (e.g. Delphi). Later, Borland sold its compilers to Embarcadero, who still regularly update Delphi. 

This tutorial will only deal with console-based programming, where the computer prints lines of data to the screen and the user interacts with the program using a keyboard. The goal of the tutorial is to teach how to program in Pascal. Once you've learned that, you can easily look at a reference book or another web page and pick up graphics and windowing systems on your own. 

Although old commercial Pascal compilers are often available for download (e.g. Turbo Pascal 5.5 from the [Borland Museum](<https://web.archive.org/web/20031206003232/http://bdn.borland.com/museum>) and Symantec Think Pascal (Macintosh), see The Free Country's [Free Pascal Compiler List](<http://www.thefreecountry.com/compilers/pascal.shtml>)), computers have progressed much since the 1980s and early 1990s. We are no longer stuck with 8.3 filenames on DOS or non-preemptive multitasking on Mac OS. Using an old compiler is fun in the same sense as playing an old game on an emulator is fun, but the open source movement has produced good compilers for modern operating systems, and a beginner will find it much easier to use those. 

### Open Source Compilers

The two main open-source compiler projects are: 

  * [GNU Pascal](<http://www.gnu-pascal.de>)
  * [Free Pascal](<https://www.freepascal.org>)



Free Pascal is generally considered friendlier for novices, and strives to emulate Borland Pascal in many ways, though both will serve fine for learning Pascal. 

As most users of this tutorial will be running Windows, here's how to set up Free Pascal and get to the point where you're compiling a program on a modern Windows operating system: 

  1. Download the Win32 installer for Free Pascal from the [Free Pascal download page](<https://www.freepascal.org/download.html>).
  2. Run the file you just downloaded and go through the wizard to setup Free Pascal.
  3. Open Free Pascal using the shortcut (by default it is located in Start -> Free Pascal.
  4. Type in a program (flip to the [next lesson](<Hello,_World.md> "Basic Pascal Tutorial/Hello, World") to get a "Hello, world." program).
  5. Save the file with File-Save As ...
  6. Run the program from the Run menu. This will automatically compile the program if you've made any changes, then run the program. It will also run the program without compiling if you've not made any changes since the last time you compiled.



With programs that don't expect user input, you'll see the program flash on a black screen. But the program completes in the blink of an eye and you are returned to the IDE without seeing the results of your work. There are two ways around this: 

  * Select User screen from the Debug menu to see the results of the program.
  * Add a readln statement at the end of every program. This will make the program wait for the user to press the Enter key before the program ends and returns to the IDE.



[![Userscreen.png](https://wiki.freepascal.org/images/1/19/Userscreen.png)](</File:Userscreen.png>)

Note that an .exe file was created in the directory where you saved your program. This is the executable. You can go to the Command Prompt, change to the directory, and run this executable straight. You can also double-click on it in Windows Explorer (and it will still flash by quickly if it ends without requiring user input). 

### See also

  * [Control Pascal Tutorial](<../Control_Pascal_Tutorial.md> "Control Pascal Tutorial") \- a simple way to start with no installation needed
  * [How to start](<../How_to_start.md> "How to start") \- another way to start with an installation

[ ◄ ](<History.md> "Basic Pascal Tutorial/History") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Hello,_World.md> "Basic Pascal Tutorial/Hello, World")  
---|---|---  
  
# Hello, world


[ ◄ ](<Compilers.md> "Basic Pascal Tutorial/Compilers") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure")  
---|---|---  
  
Hello, World (author: Tao Yue, state: unchanged) 

In the short history of computer programming, one enduring tradition is that the first program in a new language is a "Hello, world" to the screen. So let's do that. Copy and paste the program below into your IDE or text editor, then compile and run it. 

If you have no idea how to do this, return to the Table of Contents. Earlier lessons explain what a compiler is, give links to downloadable compilers, and walk you through the installation of an open-source Pascal compiler on Windows. 
    
    
    program Hello;
    begin
      writeln ('Hello, world.');
    end.
    

The output on your screen should look like: 
    
    
    Hello, world.
    

If you're running the program in an IDE, you may see the program run in a flash, then return to the IDE before you can see what happened. See the bottom of the previous lesson for the reason why. One suggested solution, adding a readln to wait for you to press Enter before ending the program, would alter the "Hello, world" program to become: 
    
    
    program Hello;
    begin
      writeln ('Hello, world.');
      readln;
    end.
    

[ ◄ ](<Compilers.md> "Basic Pascal Tutorial/Compilers") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure")  
---|---|---  
  
  


**1\. Chapter  
  
Basics**

# Program Structure


[ ◄ ](<Hello,_World.md> "Basic Pascal Tutorial/Hello, World") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers")  
---|---|---  
  
Basics 1A - Program Structure (author: Tao Yue, state: changed) 

The basic structure of a Pascal program is: 
    
    
    PROGRAM ProgramName (FileList);
    
    CONST
      (* Constant declarations *)
    
    TYPE
      (* Type declarations *)
    
    VAR
      (* Variable declarations *)
    
    (* Subprogram definitions *)
    
    BEGIN
      (* Executable statements *)
    END.
    

The **PROGRAM** statement is optional in Free Pascal. 

The elements of a program must be in the correct order, though some may be omitted if not needed. Here's a program that does nothing, but has all the required elements: 
    
    
    program DoNothing;
    begin
    end.
    

Comments are portions of the code which do not compile or execute. Free Pascal supports two types of comments, free-form and line-based. Free-form comments either start with a (* and end with a *), or more commonly, start with { and end with }. You should be careful if you nest free form comments as they may yield an error, e.g.: 
    
    
            (* (* *) *)
    

will, depending on the compiler, either be accepted, or yield an error because, 

  * if the compiler does not allow nested comments, it matches the first '`(*`' with the first '`*)`', ignoring the second '`(*`' which is between the first set of comment markers, which means everything after the first '`*)`' is treated as code. If there was nothing but blanks there, the second '`*)`' is not recognized and throws an error. OR
  * The second '`(*`' starts a nested comment, everything up to the next '`*)`' is in the nested comment, then the last '`*)`' closes the comment.



The same thing applies to '`{`' comments. Each '`{`' must be closed by '`}`' and if the compiler supports nested comments, all comments must be closed with the same marker as the one that opened it. So if you have code consisting of: 
    
    
            (* {  *)
    

OR 
    
    
            { (*  }
    

Will cause and "unexpected end of file" error, because the inner comment was not closed. 

It's usually best to use one type of comment exclusively, and save the other for when you want to mark off a piece of code you (temporarily) don't want to use, like: 
    
    
    (*
        bad code that does't work {comment}
        more bad code // and comments
    *)
    

This problem with comment markers is one reason why many languages offer line-based commenting systems. 

Free Pascal also supports // as a line-based comment. When two slashes appear, other than in a quoted string or a free-form comment, the rest of the line is ignored. 

Turbo Pascal and most other modern compilers support free-form brace comments, such as `{Comment}`. The opening brace signifies the beginning of a block of comments, and the ending brace signifies the end of a block of comments. Brace comments are also used for compiler directives. 

Commenting makes your code easier to understand. If you write your code without comments, you may come back to it weeks, months, or years later without a guide to why you coded the program that way. In particular, you may want to document the major design of your program and insert comments in your code when you deviate from that design for a good reason. 

In addition, comments are often used to take problematic code out of action without deleting it. Remember the earlier restriction on nesting comments? It just so happens that braces `{}` take precedence over parentheses-stars `(* *)`. You will not get an error if you do this: 
    
    
    { (* Comment *) }
    

Whitespace (spaces, tabs, and end-of-lines) are ignored by the Pascal compiler unless they are inside a literal string. However, to make your program readable by human beings, you should indent your statements and put separate statements on separate lines. Indentation is often an expression of individuality by programmers, but collaborative projects usually select one common style to allow everyone to work from the same page. 

[ ◄ ](<Hello,_World.md> "Basic Pascal Tutorial/Hello, World") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers")  
---|---|---  
  
# Identifiers


[ ◄ ](<Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants")  
---|---|---  
  
1B - Identifiers (author: Tao Yue, state: changed) 

Identifiers are names that allow you to reference stored values, such as variables and constants. Also, every program must be identified (get it?) by an identifier. 

Rules for identifiers: 

  * Must begin with a letter (a..z or A..Z, Pascal is case insensitive) from the English alphabet or an underscore (_).
  * Can be followed by zero or more letters (a..Z), digits (0..9), or underscores (_), in any combination.
  * Cannot be the same as a [keyword](<../Keyword.md> "Keyword") such as `begin`, `for`, `case`, `absolute` etc.
  * May not contain special characters, such as:


    
    
     ~ ! @ # $ % ^ & * ( ) + ` - = { } [ ] : " ; ' < > ? , . / | \ (or the space character)
    

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

While it is a good idea to make identifiers to be mnemonic with the use of longer names, there is nothing wrong with using very short identifiers in specific uses. it is extremely common to use `I, J, `and `K` as the control variable in a [for](<../For.md> "For") loop. 

  


[ ◄ ](<Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants")  
---|---|---  
  
# Constants


[ ◄ ](<Chapter_1/Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Variables_and_Data_Types.md> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types")  
---|---|---  
  
1C - Constants (author: Tao Yue, state: changed) 

Constants are referenced by identifiers, and can be assigned one value at the beginning of the program. The value stored in a constant cannot be changed. 

Constants come in three flavors: Scalars, records, and arrays. A scalar constant is a single identifier which is assigned a single value. A record constant is a single identifier holding one or more separate values in a structured form. An array constant holds multiple values. They will be explained in greater detail in separate sections below. 

A constant, whether a scalar, a record field, or an array subscript, may be used in place of a literal of the same type, or may be used anywhere a variable may be used with two exceptions. They cannot be used as the target of an assignment statement, and they cannot be passed as a VAR parameter in a call to a procedure, function, method or property. 

## Scalar constants

Scalar constants are defined in the Const (constant) section of the program: 
    
    
    const
      Identifier1 = value;
      Identifier2 = value;
      Identifier3 = value;
    

For example, let's define implicitly some constants of various data types: strings, characters, integers, reals, and booleans. These data types will be further explained in the next section. 
    
    
    const
      Name = 'Tao Yue';
      FirstLetter = 'a';
      Year = 1997;
      pi = 3.1415926535897932;
      UsingNCSAMosaic = TRUE;
    

Note that in Pascal, single characters are enclosed in single quotes, or apostrophes (')! This contrasts with newer languages which often use or allow double quotes or Heredoc notation. Standard Pascal does not use or allow double quotes to mark characters or strings. 

Constants are useful for defining a value which is used throughout your program but may change in the future. Instead of changing every instance of the value, you can change just the constant definition. 

**Typed constants** force a constant to be of a particular data type, by defining it explicitly. 

For example, 
    
    
    const
      a : real = 12;
    

would yield an identifier **a** which contains a **real** value **12.0** instead of the integer value **12**. 

## Record constants

Record constants are created by creating a record type with one or more fields, then creating a constant that references that record type, filling the field(s) with values. 

For record constants, the general format is 

    const 

    `identifier`: `record_type` = ( `field_values` );
    `identifier` …

Where 

  * `identifier` is the name of the record constant;
  * `record_type` is the name of the record type used to describe this record constant, and
  * `field_values` is a list consisting of a field name from the `record_type` referenced earlier, a colon, and the value to be assigned to that field, separated from the next field:value pair by a semicolon.



Lets try a simple example, a complex number constant. 
    
    
    type
        complex = record
                    R,I: real;
                  end;
    const
         Pi = 3.14159267;
         C1: Complex = (R:3; I:1783.5);
         C2: Complex = (R:9.6; I:1.62);
         C3: Complex = (R:17; I:115);
         C4: Complex = (R:1.9e56; I:72.43);
         C5: Complex = (R:22.1; I:Pi);
    

Note that fields must be initialized in the same order that they are declared (or the compiler will flag an error). If a record value is missing the compiler will warn of an uninitialized value. The values assigned can be literals or may be a scalar constant. 

For how to describe a constant array of record, see the section below. 

## Array constants

Array constants work much the same as scalar constants, except multiple values can be specified. All of the values must be the same type, whether it is a number (byte, word, integer. real, etc.) or character based (char, string, ansistring, etc.) 

### One-dimensional arrays

For one-dimensional array constants the general format is: 

    const 

    `identifier`: array[`low_value` .. `high_value`] of `type` = ( `values` );
    `identifier` …

Where 

  * `identifier` is the name of the array;
  * `low_value` is the lower bound of the array;
  * `high_value` is the upper bound of the array;
  * `type` is the type of value stored in the elements of the array (char, integer, real, string, etc.) and
  * `values` is a list of values with each item in the list separated from the next item by a comma.



Here's a simple one, the alphabet: 
    
    
    const
      Alphabet: array [1..26] of char =
           ('A','B','C','D','E','F','G','H','I',
            'J','K','L','M','N','O','P','Q','R',
            'S','T','U','V','W','X','Y','Z'   );
    

There are two rules to remember. As was said before, all the values given have to be of the same type, and you have to specify as many values as there are elements in the array. While the example given above is okay, there will be other parts of the program that reference this array, such as `for` loops. etc. Instead of using the literal 26 for the size of the array, let's use a scalar constant instead: 
    
    
    const
       LetterCount = 26;
       Alphabet: array [1 .. LetterCount] of char =
           ('A','B','C','D','E','F','G','H','I',
            'J','K','L','M','N','O','P','Q','R',
            'S','T','U','V','W','X','Y','Z');
    

Now, any place you need to reference the size of the array, you can just use `LetterCount`. 

Here's a more interesting example in which several data types are used. This example, which is probably from a calendar program, has all sorts of types: char, string, and integer. 
    
    
    const
      MonthStart = 0; // could be 1 and
      MonthEnd   = 11; // 12 if desired
      DayStart   = 0; // same,could be 1 and
      DayEnd     = 6;  // 7
      DayNameCh: array [DayStart .. DayEnd] of char =
          ('S','M','T','W','H','F','A');
      DayNameShort: array [DayStart .. DayEnd] of string =
          ('Sun','Mon','Tue','Wed','Thu', 'Fri','Sat');
      DayNameLong: array [DayStart .. DayEnd] of string =
          ('Sunday', 'Monday', 'Tuesday', 'Wednesday',
           'Thursday', 'Friday', 'Saturday');
      MonthNameLong: array [MonthStart .. MonthEnd] of string =
          ('January', 'February', 'March', 'April', 'May', 'June', 'July',
           'August', 'September', 'October', 'November', 'December');
      MonthDays: array [MonthStart .. MonthEnd] of integer =
          (31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31);
    

Notice that for the items indicated as type `string`, while every value had to be a string, they were not required to all be the same size. 

#### One-dimensional array of record

An array constant may consist of records. To describe them, we use the parentheses ( ) around each record. Using the `complex` type defined in the section on record constants, we can create a constant array of `complex` records, as follows: 
    
    
    type
        complex = record
            R,I: real;
        end;
    const
         Pi = 3.14159267;
         C2: complex = ( R: 96; I:1.62);
         C_Low = 1; C_High = 5;
         C: array [C_low .. C_high] of complex = (
             (R:3; I:1783.5),
             (R:96; I:1.62),
             (R:17; I:115),
             (R:1.9e56; I:72.43),
             (R:102.1; I:Pi)
         );
    

As before, values need not be literals; they can be other scalar constants. Also, values must be given in the same order as defined in the record. 

Now for a more (pun unintentional) complex example, we use an enumerated type and a set to define a record constant array: 
    
    
    type
        Month = (January, February, March, April, May,
                 June, July, August, September,
                 October, November, December);
        MonthSet = set of Month;
        EachMonth = record
             Name, Short: string;
             Count: integer;
        end;
    
    const
         Months: array [January .. December] of EachMonth = (
           (Name: 'January';   Short: 'Jan'; Count: 31),
           (Name: 'February';  Short: 'Feb'; Count: 28),
           (Name: 'March';     Short: 'Mar'; Count: 31),
           (Name: 'April';     Short: 'Apr'; Count: 30),
           (Name: 'May';       Short: 'May'; Count: 31),
           (Name: 'June';      Short: 'Jun'; Count: 30),
           (Name: 'July';      Short: 'Jul'; Count: 31),
           (Name: 'August';    Short: 'Aug'; Count: 31),
           (Name: 'September'; Short: 'Sep'; Count: 30),
           (Name: 'October';   Short: 'Oct'; Count: 31),
           (Name: 'November';  Short: 'Nov'; Count: 30),
           (Name: 'December';  Short: 'Dec'; Count: 31)
         );
    

Take notice of how the items are delimited (separated). Elements in the record are delimited by semi-colons, while commas delimit elements of the array. 

### Two-dimensional array constant

As used in the example of ` array of record`, two-dimensional arrays are described by encapsulating each of the second dimension in parentheses. To make it easier to understand, the entries in the array have been identified by number, e.g. 13 is the first row, third column, while 21 is the second row, first column. Here is the format: 
    
    
    const
       DemoArray: array [1..2, 1..3] of integer = (
               (11,12,13),
               (21,22,23)
       );
    

### Three-dimensional array constant

Three-dimensional arrays are rare, and you might never use one, but you should know how to construct a three-dimensional constant array if you need it. Again, each level of array is encapsulated by parentheses. Here is a three-dimensional constant array, and the values represent their position in the array. 
    
    
    const
       Demo3: array [1..2, 1..3, 1..4] of integer =
       (
          (
           (111,112,113,114),
           (121,122,123,124),
           (131,132,133,134)
          ),
          (
           (211,212,213,214),
           (221,222,223,224),
           (231,232,233,234)
          )
       );
    

  


[ ◄ ](<Chapter_1/Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Variables_and_Data_Types.md> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types")  
---|---|---  
  
# Variables and Data Types


[ ◄ ](<Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations")  
---|---|---  
  
1D - Variables and Data Types (author: Tao Yue, state: _changed_) 

Variables are similar to constants, but their values can be changed as the program runs. Variables must first be declared in Pascal before they can be used: 
    
    
    var
      IdentifierList1 : DataType1;
      IdentifierList2 : DataType2;
      IdentifierList3 : DataType3;
      ...
    

`IdentifierList` is a series of identifiers, separated by commas (`,`). All identifiers in the list are declared as being of the same data type. 

The basic [data field](<../Data_field.md> "Data field") [data types](<../Data_type.md> "Data type") in Pascal include: 

  * [Integer](<../Integer.md> "Integer")
  * [Word](<../Word.md> "Word")
  * [LongInt](<../Longint.md> "Longint")
  * [Real](<../Real.md> "Real")
  * [Char](<../Char.md> "Char")
  * [Boolean](<../Boolean.md> "Boolean")



Standard Pascal does not make provision for the string data type, but most modern compilers do. Experienced Pascal programmers also use pointers for dynamic memory allocation, objects for object-oriented programming, and many others, but this gets you started. 

More information on Pascal data types: 

  * The [Integer](<../Integer.md> "Integer") data type can contain whole numbers. the size of an integer depends on the compiler and the processor. On PCs before the 80386, "integer" meant 16-bit whole numbers in the range from `-32768` to `32767`. This is the signed range that can be stored in a 16-bit word, and is a legacy of the era when 16-bit CPUs were common. For backward compatibility purposes, a 32-bit signed integer is a longint and can hold a much greater range of values, `2147483647` to `-2147483648`.
  * The [Word](<../Word.md> "Word") data type is a 16-bit unsigned integer, which has a range of `0` to `65535`.
  * The [Real](<../Real.md> "Real") data type has a range from `3.4x10-38` to `3.4x1038`, in addition to the same range on the negative side. Real values are stored inside the computer similarly to scientific notation, with a mantissa and exponent, with some complications. In Pascal, you can express real values in your code in either fixed-point notation or in scientific notation, with the character `E` separating the mantissa from the exponent. Thus, `452.13` is the same as `4.5213e2`
  * The [Char](<../Char.md> "Char") data type holds characters. Be sure to enclose them in single quotes, like so: `'a' 'B' '+'` Standard Pascal uses 8-bit characters, not 16-bits, so Unicode, which is used to represent all the world's language sets in one UNIfied CODE system, is not supported.
  * The [WideChar](<../WideChar.md> "WideChar") is a two-byte character (an element of a DBCS: Double Byte Character Set) and can hold a Unicode character. Note: some Unicode characters require two WideChars. See _UTF-16_.
  * Free Pascal supports the Delphi implementation of the [PChar](<../PChar.md> "PChar") data type. PChar is defined as a pointer to a Char type, but allows additional operations. The PChar type can be understood best as the Pascal equivalent of a C-style null-terminated string, i.e. a variable of type PChar is a pointer that points to an array of type Char, which is ended by a null-character (#0). Free Pascal supports initializing of PChar typed constants, or a direct assignment. For example, the following pieces of code are equivalent:


    
    
    program one;  
    var P : PChar;  
    begin  
      P := 'This is a null-terminated string.';  
      WriteLn (P);  
    end.
    
    
    
    program two;  
    const P : PChar = 'This is a null-terminated string.';  
    begin  
      WriteLn (P);  
    end.
    

  * Free Pascal supports the [String](<../String.md> "String") type as it is defined in Turbo Pascal: a sequence of characters with an optional size specification. It also supports [AnsiStrings](<../Ansistring.md> "Ansistring") (with unlimited length) as in Delphi. And can be declared as:


    
    
    variable_name : string;                    // if no length is given, it defaults to 255
    variable_name : string[length];            // where:  1 < length <= 255
    

  * The predefined type [ShortString](<../Shortstring.md> "Shortstring") is defined as a string of size 255.
  * [AnsiStrings](<../Ansistring.md> "Ansistring") are strings that have no length limit. They are reference counted and are guaranteed to be null terminated. Internally, an ansistring is treated as a pointer: the actual content of the string is stored on the heap, as much memory as needed to store the string content is allocated.
  * [WideStrings](<../Widestrings.md> "Widestrings") (used to represent unicode character strings) are implemented in much the same way as ansistrings: reference counted, null-terminated arrays, only they are implemented as arrays of `WideChars` instead of regular `Chars`.
  * The [Boolean](<../Boolean.md> "Boolean") data type can have only two values: **TRUE** and **FALSE**



An example of declaring several variables is: 
    
    
    var
      Age, Year, Grade : integer;
      Circumference : real;
      LetterGrade : char;
      DidYouFail : boolean;
    

**From the FPC manual**

**integer types** Type | Range | Bytes   
---|---|---  
Byte | 0 .. 255 | 1   
Shortint | -128 .. 127 | 1   
Smallint | -32768 .. 32767 | 2   
Word | 0 .. 65535 | 2   
Integer | smallint or longint | 2 or 4   
Cardinal | longword | 4   
Longint | -2147483648 .. 2147483647 | 4   
Longword | 0..4294967295 | 4   
Int64 | -9223372036854775808 .. 9223372036854775807 | 8   
QWord | 0 .. 18446744073709551615 | 8   
  
Free Pascal does automatic type conversion in expressions where different kinds of integer types are used. 

**real types** Type | Range | Significant digits | Bytes   
---|---|---|---  
Real | platform dependent | ??? | 4 or 8   
Single | 1.5E-45 .. 3.4E38 | 7-8 | 4   
Double | 5.0E-324 .. 1.7E308 | 15-16 | 8   
Extended* | 1.9E-4932 .. 1.1E4932 | 19-20 | 10   
Comp | -2E64+1 .. 2E63-1 | 19-20 | 8   
Currency | -922337203685477.5808 | 922337203685477.5807 | 8   
  
  * Note that for Windows 64 bits and non-Intel targets Extended is an alias for Double.



  


**boolean types** Type | Bytes | Ord(True)   
---|---|---  
Boolean | 1 | 1   
ByteBool | 1 | Any nonzero value   
WordBool | 2 | Any nonzero value   
LongBool | 4 | Any nonzero value   
[ ◄ ](<Chapter_1/Constants.md> "Basic Pascal Tutorial/Chapter 1/Constants") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations")  
---|---|---  
  
# Assignment and Operations


[ ◄ ](<Chapter_1/Variables_and_Data_Types.md> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Standard_Functions.md> "Basic Pascal Tutorial/Chapter 1/Standard Functions")  
---|---|---  
  
1E - Assignment and Operations (author: Tao Yue, state: unchanged) 

Once you have declared a variable, you can store values in it. This is called assignment. 

To assign a value to a variable, follow this syntax: 
    
    
    variable_name := expression;
    

Note that unlike other languages, whose assignment operator is just an equals sign, Pascal uses a colon followed by an equals sign, similarly to how it's done in most computer algebra systems. 

The expression can either be a single value: 
    
    
    some_real := 385.385837;
    

or it can be an arithmetic sequence: 
    
    
    some_real := 37573.5 * 37593 + 385.8 / 367.1;
    

The arithmetic operators in Pascal are: 

Operator | Operation | Operands | Result   
---|---|---|---  
+ | Addition or unary positive | real or integer | real or integer   
- | Subtraction or unary negative | real or integer | real or integer   
* | Multiplication | real or integer | real or integer   
/ | Real division | real or integer | real   
div | Integer division | integer | integer   
mod | Modulus (remainder division) | integer | integer   
  
**`div`** and **`mod`** only work on _integers_. **`/`** works on both _reals_ and _integers_ but will always yield a _real_ answer. The other operations work on both _reals_ and _integers_. When mixing _integers_ and _reals_ , the result will always be a _real_ since data loss would result otherwise. This is why Pascal uses two different operations for division and integer division. `7 / 2 = 3.5` (real), but `7 div 2 = 3` (and `7 mod 2 = 1` since that's the remainder). 

Each variable can only be assigned a value that is of the same data type. Thus, you cannot assign a real value to an integer variable. However, certain data types will convert to a higher data type. This is most often done when assigning integer values to real variables. Suppose you had this variable declaration section: 
    
    
    var
      some_int : integer;
      some_real : real;
    

When the following block of statements executes: 
    
    
    some_int := 375;
    some_real := some_int;
    

`some_real` will have a value of `375.0`. 

Changing one data type to another is referred to as typecasting. Modern Pascal compilers support explicit typecasting in the manner of C, with a slightly different syntax. However, typecasting is usually used in low-level situations and in connection with object-oriented programming, and a beginning programming student will not need to use it. Here is [information on typecasting from the GNU Pascal manual](<http://www.gnu-pascal.de/gpc-es/Type-Casts.html>). 

In Pascal, the minus sign can be used to make a value negative. The plus sign can also be used to make a value positive, but is typically left out since values default to positive. 

Do not attempt to use two operators side by side, like in: 
    
    
    some_real := 37.5 * -2;
    

This may make perfect sense to you, since you're trying to multiply by negative-2. However, Pascal will be confused — it won't know whether to multiply or subtract. You can avoid this by using parentheses to clarify: 
    
    
    some_real := 37.5 * (-2);
    

The computer follows an order of operations similar to the one that you follow when you do arithmetic. Multiplication and division (`* / div mod`) come before addition and subtraction (`+ -`), and parentheses always take precedence. So, for example, the value of: `3.5*(2+3)` will be `17.5.`

Pascal cannot perform standard arithmetic operations on Booleans. There is a special set of Boolean operations. Also, you should not perform arithmetic operations on characters. 

[ ◄ ](<Chapter_1/Variables_and_Data_Types.md> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Standard_Functions.md> "Basic Pascal Tutorial/Chapter 1/Standard Functions")  
---|---|---  
  
# Standard Functions


[ ◄ ](<Chapter_1/Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation")  
---|---|---  
  
1F - Standard Functions (author: Tao Yue, state: unchanged) 

Pascal has several standard mathematical functions that you can utilize. For example, to find the value of sin of pi radians: 
    
    
    value := sin (3.141592653589793238462643383);
    

Note that the sin function operates on angular measure stated in radians, as do all the trigonometric functions. If everything goes well, value should become 0. 

Functions are called by using the function name followed by the argument(s) in parentheses. Standard Pascal functions include: 

Function | Description | Argument type | Return type   
---|---|---|---  
abs | absolute value | real or integer | same as argument   
arctan | arctan in radians | real or integer | real   
cos | cosine of a radian measure | real or integer | real   
exp | e to the given power | real or integer | real   
ln | natural logarithm | real or integer | real   
round | round to nearest integer | real | integer   
sin | sin of a radian measure | real or integer | real   
sqr | square (power 2) | real or integer | same as argument   
sqrt | square root (power 1/2) | real or integer | real   
trunc | truncate (round down) | real or integer | integer   
  
For ordinal data types (integer or char), where the allowable values have a distinct predecessor and successor, you can use these functions: 

Function | Description | Argument type | Return type   
---|---|---|---  
chr | character with given ASCII value | integer | char   
ord | ordinal value | integer or char | integer   
pred | predecessor | integer or char | same as argument type   
succ | successor | integer or char | same as argument type   
  
Real is not an ordinal data type! That's because it has no distinct successor or predecessor. What is the successor of 56.0? Is it 56.1, 56.01, 56.001, 56.0001? 

However, for an integer 56, there is a distinct predecessor — 55 — and a distinct successor — 57. 

The same is true of characters: 
    
    
     'b'
     Successor: 'c'
     Predecessor: 'a'
    

The above is not an exhaustive list, as modern Pascal compilers include thousands of functions for all sorts of purposes. Check your compiler documentation for more. 

[ ◄ ](<Chapter_1/Assignment_and_Operations.md> "Basic Pascal Tutorial/Chapter 1/Assignment and Operations") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation")  
---|---|---  
  
# Punctuation and Indentation


[ ◄ ](<Chapter_1/Standard_Functions.md> "Basic Pascal Tutorial/Chapter 1/Standard Functions") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment")  
---|---|---  
  
1G - Punctuation and Indentation (author: Tao Yue, state: unchanged) 

Since Pascal ignores end-of-lines and spaces, punctuation is needed to tell the compiler when a statement ends. 

You _must_ have a semicolon following: 

  * the program heading
  * each constant definition
  * each variable declaration
  * each type definition (to be discussed later)
  * almost all statements



The last statement in a `BEGIN-END` block, the one immediately preceding the END, does not require a semicolon. However, it's harmless to add one, and it saves you from having to add a semicolon if suddenly you had to move the statement higher up. 

Indenting is not required. However, it is of great use for the programmer, since it helps to make the program clearer. If you wanted to, you could have a program look like this: 
    
    
    program Stupid; const a=5; b=385.3; var alpha,beta:real; begin 
    alpha := a + b; beta:= b / a end.
    

But it's much better for it to look like this: 
    
    
    program NotAsStupid;
    
    const
      a = 5;
      b = 385.3;
    
    var
      alpha,
      beta : real;
    
    begin (* main *)
      alpha := a + b;
      beta := b / a;
    end. (* main *)
    

In general, indent each block. Skip a line between blocks (such as between the const and var blocks). Modern programming environments (IDE, or Integrated Development Environment) understand Pascal syntax and will often indent for you as you type. You can customize the indentation to your liking (display a tab as three spaces or four?). 

Proper indentation makes it much easier to determine how code works, but is vastly aided by judicious commenting. 

[ ◄ ](<Chapter_1/Standard_Functions.md> "Basic Pascal Tutorial/Chapter 1/Standard Functions") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment")  
---|---|---  
  
# Programming Assignment


[ ◄ ](<Chapter_1/Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution")  
---|---|---  
  
1H - Programming Assignment (author: Tao Yue, state: changed) 

Now you know how to use variables and change their value. Ready for your first programming assignment? 

But there's one small problem: you haven't yet learned how to display data to the screen! How are you going to know whether or not the program works if all that information is still stored in memory and not displayed on the screen? 

So, to get you started, here's a snippet from the next few lessons. To display data, use: 
    
    
    writeln (argument_list);
    

The argument list is composed of either strings or variable names separated by commas. An example is: 
    
    
    writeln ('Sum = ', sum);
    

Here's the programming assignment for Chapter 1: 

Find the sum and average of five integers. The sum should be an integer, and the average should be real. The five numbers are: 45, 7, 68, 2, and 34. 

Use a constant to signify the number of integers handled by the program, i.e. define a constant as having the value 5. 

Then print it all out! The output should look something like this: 
    
    
    Number of integers = 5
    Number 1 = 45
    Number 2 = 7
    Number 3 = 68
    Number 4 = 2
    Number 5 = 34
    Sum = 156
    Average = 3.1200000000E+01
    

As you can see, the default output method for real numbers is scientific notation. Chapter 2 will explain you how to format it to fixed-point decimal. 

To see one possible solution of the assignment, go to the next page. 

[ ◄ ](<Chapter_1/Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution")  
---|---|---  
  
# Solution


[ ◄ ](<Chapter_1/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input")  
---|---|---  
  
1Ha - Solution (author: Tao Yue, state: changed) 

Here's one way to solve the programming assignment in the previous section. 
    
    
    (* Author:    Tao Yue
       Date:      19 June 1997
       Description:
          Find the sum and average of five predefined numbers
       Version:
          1.0 - original version
    *)
    
    program SumAverage;
    
    const
       NumberOfIntegers = 5;
    
    var
       A, B, C, D, E : integer;
       Sum : integer;
       Average : real;
    
    begin    (* Main *)
       A := 45;
       B := 7;
       C := 68;
       D := 2;
       E := 34;
       Sum := A + B + C + D + E;
       Average := Sum / NumberOfIntegers;
       writeln ('Number of integers = ', NumberOfIntegers);
       writeln ('Number 1 = ', A);
       writeln ('Number 2 = ', B);
       writeln ('Number 3 = ', C);
       writeln ('Number 4 = ', D);
       writeln ('Number 5 = ', E);
       writeln ('Sum = ', Sum);
       writeln ('Average = ', Average)
    end.     (* Main *)
    

[ ◄ ](<Chapter_1/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 1/Programming Assignment") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input")  
---|---|---  
  
  


**2\. Chapter  
  
Input/Output**

# Input


[ ◄ ](<Chapter_1/Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Output.md> "Basic Pascal Tutorial/Chapter 2/Output")  
---|---|---  
  
2A - Input (author: Tao Yue, state: changed) 

Input is what comes into the program. It can be from the keyboard, the mouse, a file on disk, a scanner, a joystick, etc. 

We will not get into mouse input in detail, because that syntax differs from machine to machine. In addition, today's event-driven windowing operating systems usually handle mouse input for you. 

The basic format for reading in data is: 
    
    
    read (Variable_List);
    

`Variable_List` is a series of variable identifiers separated by commas. 

`read` treats input as a stream of characters, with lines separated by a special end-of-line character. `readln`, on the other hand, will skip to the next line after reading a value, by automatically moving past the next end-of-line character: 
    
    
    readln (Variable_List);
    

Suppose you had this input from the user, and `a, b, c,` and `d` were all integers. 
    
    
    45 64 97
    1 2 3
    

Here are some sample `read` and `readln` statements, along with the values read into the appropriate variables. 

Statement(s) | a | b | c | d   
---|---|---|---|---  
read (a); read (b); | 45 | 64 |  |   
readln (a); read (b); | 45 | 1 |  |   
read (a, b, c, d); | 45 | 64 | 97 | 1   
readln (a, b); readln (c, d); | 45 | 64 | 1 | 2   
  
When reading in integers, all spaces are skipped until a numeral is found. Then all subsequent numberals are read, until a non-numeric character is reached (including, but not limited to, a space). 
    
    
    8352.38
    

When an integer is read from the above input, its value becomes `8352`. If, immediately afterwards, you read in a character, the value would be '`.`' since the read head stopped at the first alphanumeric character. 

Suppose you tried to read in two integers. That would not work, because when the computer looks for data to fill the second variable, it sees the '`.`' and stops since it couldn't find any data to read. 

With real values, the computer also skips spaces and then reads as much as can be read. However, many Pascal compilers place one additional restriction: a real that has no whole part must begin with `0.` So `.678` is invalid, and the computer can't read in a real, but `0.678` is fine. 

Make sure that all identifiers in the argument list refer to variables! Constants cannot be assigned a value, and neither can literal values. 

[ ◄ ](<Chapter_1/Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Output.md> "Basic Pascal Tutorial/Chapter 2/Output")  
---|---|---  
  
# Output


[ ◄ ](<Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Formatting_output.md> "Basic Pascal Tutorial/Chapter 2/Formatting output")  
---|---|---  
  
2B - Output (author: Tao Yue, state: unchanged) 

For writing data to the screen, there are also two statements, one of which you've seen already in last chapter's programming assignment: 
    
    
    write (Argument_List);
    writeln (Argument_List);
    

The `writeln` statement skips to the next line when done. 

You can use strings in the argument list, either constants or literal values. If you want to display an apostrophe within a string, use two consecutive apostrophes. Displaying two consecutive apostrophes would then requires you to use four. This use of a special sequence to refer to a special character is called escaping, and allows you to refer to any character even if there is no key for it on the keyboard. 

[ ◄ ](<Chapter_2/Input.md> "Basic Pascal Tutorial/Chapter 2/Input") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Formatting_output.md> "Basic Pascal Tutorial/Chapter 2/Formatting output")  
---|---|---  
  
# Formatting output


[ ◄ ](<Chapter_2/Output.md> "Basic Pascal Tutorial/Chapter 2/Output") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Files.md> "Basic Pascal Tutorial/Chapter 2/Files")  
---|---|---  
  
2C - Formatting Output (author: Tao Yue, state: unchanged) 

Formatting output is quite easy. For each identifier or literal value on the argument list, use: 
    
    
    Value : field_width
    

The output is right-justified in a field of the specified integer width. If the width is not long enough for the data, the width specification will be ignored and the data will be displayed in its entirety (except for real values — see below). 

Suppose we had: 
    
    
    write ('Hi':10, 5:4, 5673:2);
    

The output would be (that's eight spaces before the Hi and three spaces after): 
    
    
            Hi   55673
    

For real values, you can use the aforementioned syntax to display scientific notation in a specified field width, or you can convert to fixed decimal-point notation with: 
    
    
    Value : field_width : decimal_field_width
    

The field width is the total field width, including the decimal part. The whole number part is always displayed fully, so if you have not allocated enough space, it will be displayed anyway. However, if the number of decimal digits exceeds the specified decimal field width, the output will be displayed rounded to the specified number of places (though the variable itself is not changed). 
    
    
    write (573549.56792:20:2);
    

would look like (with 11 spaces in front): 
    
    
               573549.57
    

[ ◄ ](<Chapter_2/Output.md> "Basic Pascal Tutorial/Chapter 2/Output") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Files.md> "Basic Pascal Tutorial/Chapter 2/Files")  
---|---|---  
  
# Files


[ ◄ ](<Chapter_2/Formatting_output.md> "Basic Pascal Tutorial/Chapter 2/Formatting output") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/EOLN_and_EOF.md> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF")  
---|---|---  
  
2D - Files (author: Tao Yue, state: changed) 

Reading from a file instead of the console (keyboard) can be done by: 
    
    
    read(File_variable, Argument_list);
    write(File_variable, Argument_list);
    

Similarly with `readln` and `writeln`. The output is stored into the variables named in `Argument_list`. `File_variable` is declared as follows: 
    
    
    var
      ...
      Filein, Fileout : text;
    

The `text` data type indicates that the file is just plain text. 

After declaring a variable for the file, and before reading from or writing to it, we need to associate the variable with the filename on the disk and open the file. This can be done in one of two ways. Typically: 
    
    
    reset(File_variable, 'filename.extension');
    rewrite(File_variable, 'filename.extension');
    

`reset` opens a file for reading, and rewrite opens a file for writing. A file opened with `reset` can only be used with `read` and `readln`. A file opened with `rewrite` can only be used with `write` and `writeln`. 

Turbo Pascal introduced the assign notation. First you assign a filename to a variable, then you call `reset` or `rewrite` using only the variable. 
    
    
    assign(File_variable, 'filename.extension');
    reset(File_variable);
    

The method of representing the path differs depending on your operating system. Windows uses backslashes and drive letters due to its DOS heritage (e.g. `c:\directory\name.pas`), while FreeBSD, macOS and Linux use forward slashes due to their UNIX heritage. 

After you're done with the file, you can close it with: 
    
    
    close (File_variable);
    

Here's an example of a program that uses files. This program was written for Turbo Pascal and DOS, and will create file2.txt with the first character from file1.txt: 
    
    
    program CopyOneByteFile;
    
    var
       Mychar : char;
       Filein, Fileout : text;
    
    begin
       assign(Filein, 'c:\file1.txt');
       reset(Filein);
       assign(Fileout, 'c:\file2.txt');
       rewrite(Fileout);
       read(Filein, Mychar);
       write(Fileout, Mychar);
       close(Filein);
       close(Fileout)
    end.
    

[ ◄ ](<Chapter_2/Formatting_output.md> "Basic Pascal Tutorial/Chapter 2/Formatting output") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/EOLN_and_EOF.md> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF")  
---|---|---  
  
# EOLN and EOF


[ ◄ ](<Chapter_2/Files.md> "Basic Pascal Tutorial/Chapter 2/Files") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 2/Programming Assignment")  
---|---|---  
  
2E - EOLN and EOF (author: Tao Yue, state: unchanged) 

`EOLN` is a Boolean function that is `TRUE` when you have reached the end of a line in an open input file. 
    
    
    eoln (file_variable)
    

If you want to test to see if the standard input (the keyboard) is at an end-of-line, simply issue `eoln` without any parameters. This is similar to the way in which `read` and `write` use the console (keyboard and screen) if called without a file parameter. 
    
    
    eoln
    

`EOF` is a Boolean function that is `TRUE` when you have reached the end of the file. 
    
    
    eof (file_variable)
    

Usually, you don't type the `end-of-file` character from the keyboard. On DOS/Windows machines, the character is `Control-Z`. On UNIX/Linux machines, the character is `Control-D`. 

[ ◄ ](<Chapter_2/Files.md> "Basic Pascal Tutorial/Chapter 2/Files") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 2/Programming Assignment")  
---|---|---  
  
# Programming Assignment


[ ◄ ](<Chapter_2/EOLN_and_EOF.md> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Solution.md> "Basic Pascal Tutorial/Chapter 2/Solution")  
---|---|---  
  
2F - Programming Assignment (author: Tao Yue, state: unchanged) 

Again find the sum and average of five numbers, but this time read in five integers and display the output in neat columns. 

Refer to the original problem specification if needed. You should type in the numbers separated by spaces from the keyboard: `45 7 68 2 34`. 

The output should now look like this: 
    
    
    Number of integers = 5
    
    Number1:      45
    Number2:       7
    Number3:      68
    Number4:       2
    Number5:      34
    ================
    Sum:         156
    Average:      31.2
    

As an added exercise, you can try to write the output to a file. However, I won't use files in the problem solution. 

[ ◄ ](<Chapter_2/EOLN_and_EOF.md> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_2/Solution.md> "Basic Pascal Tutorial/Chapter 2/Solution")  
---|---|---  
  
# Solution


[ ◄ ](<Chapter_2/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 2/Programming Assignment") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/Sequential_control.md> "Basic Pascal Tutorial/Chapter 3/Sequential control")  
---|---|---  
  
2Fa - Solution (author: Tao Yue, state: unchanged) 
    
    
    (* Author:    Tao Yue
       Date:      19 June 1997
       Description:
          Find the sum and average of five predefined numbers
       Version:
          1.0 - original version
          2.0 - read in data from keyboard
    *)
    
    program SumAverage;
    
    const
       NumberOfIntegers = 5;
    
    var
       A, B, C, D, E : integer;
       Sum : integer;
       Average : real;
    
    begin    (* Main *)
       write ('Enter the first number: ');
       readln (A);
       write ('Enter the second number: ');
       readln (B);
       write ('Enter the third number: ');
       readln (C);
       write ('Enter the fourth number: ');
       readln (D);
       write ('Enter the fifth number: ');
       readln (E);
       Sum := A + B + C + D + E;
       Average := Sum / 5;
       writeln ('Number of integers = ', NumberOfIntegers);
       writeln;
       writeln ('Number1:', A:8);
       writeln ('Number2:', B:8);
       writeln ('Number3:', C:8);
       writeln ('Number4:', D:8);
       writeln ('Number5:', E:8);
       writeln ('================');
       writeln ('Sum:', Sum:12);
       writeln ('Average:', Average:10:1);
    end.
    

[ ◄ ](<Chapter_2/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 2/Programming Assignment") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/Sequential_control.md> "Basic Pascal Tutorial/Chapter 3/Sequential control")  
---|---|---  
  
  


**3\. Chapter  
  
Program Flow**

# Sequential control


[ ◄ ](<Chapter_2/Solution.md> "Basic Pascal Tutorial/Chapter 2/Solution") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/Boolean_Expressions.md> "Basic Pascal Tutorial/Chapter 3/Boolean Expressions")  
---|---|---  
  
3A - Sequential Control (author: Tao Yue, state: unchanged) 

Sequential control is simple. The computer executes each statement and goes on to the next statement until it sees an end. 

[ ◄ ](<Chapter_2/Solution.md> "Basic Pascal Tutorial/Chapter 2/Solution") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/Boolean_Expressions.md> "Basic Pascal Tutorial/Chapter 3/Boolean Expressions")  
---|---|---  
  
# Boolean Expressions


[ ◄ ](<Chapter_3/Sequential_control.md> "Basic Pascal Tutorial/Chapter 3/Sequential control") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/IF.md> "Basic Pascal Tutorial/Chapter 3/IF")  
---|---|---  
  
3B - Boolean Expressions (author: Tao Yue, state: unchanged) 

[Boolean](<../Boolean.md> "Boolean") expressions are used to compare two values and get a `true-or-false` answer: 
    
    
    value1 **relational_operator** value2
    

The following relational operators are used: 
    
    
    <	less than
    >	greater than
    =	equal to
    <=	less than or equal to
    >=	greater than or equal to
    <>	not equal to
    

You can assign Boolean expressions to Boolean variables. Here we assign a `true` expression to some_bool: 
    
    
     some_bool := 3 < 5;
    

Complex Boolean expressions are formed by using the Boolean operators: 
    
    
    not	negation (~)
    and	conjunction (^)
    or	disjunction (v)
    xor	exclusive-or
    

`NOT` is a unary operator — it is applied to only one value and inverts it: 

  * `not true = false`
` `
  * `not false = true`



`AND` yields `TRUE` only if both values are `TRUE`: 

  * `TRUE and FALSE = FALSE`
` `
  * `TRUE and TRUE = TRUE`



`OR` yields `TRUE` if at least one value is `TRUE`: 

  * `TRUE or TRUE = TRUE`
`
  * TRUE or FALSE = TRUE
  * FALSE or TRUE = TRUE
`
  * `FALSE or FALSE = FALSE`



`XOR` yields `TRUE` if one expression is `TRUE` and the other is `FALSE`. Thus: 

  * `TRUE xor TRUE = FALSE`
`
  * TRUE xor FALSE = TRUE
  * FALSE xor TRUE = TRUE
`
  * `FALSE xor FALSE = FALSE`



When combining two Boolean expressions using relational and Boolean operators, be careful to use parentheses. 
    
    
    (3>5) or (650<1)
    

This is because the Boolean operators are higher on the order of operations than the relational operators: 

  1. `not`
`
  2. * / div mod and
  3. + - or
`
  4. `< > <= >= = <>`



So `3 > 5 or 650 < 1` becomes evaluated as `3 > (5 or 650) < 1`, which makes no sense, because the Boolean operator or only works on Boolean values, not on integers. 

The Boolean operators (`AND, OR, NOT, XOR`) can be used on Boolean variables just as easily as they are used on Boolean expressions. 

Whenever possible, don't compare two real values with the equals sign. Small round-off errors may cause two equivalent expressions to differ. 

[ ◄ ](<Chapter_3/Sequential_control.md> "Basic Pascal Tutorial/Chapter 3/Sequential control") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/IF.md> "Basic Pascal Tutorial/Chapter 3/IF")  
---|---|---  
  
# Branching


[ ◄ ](<Chapter_3/Boolean_Expressions.md> "Basic Pascal Tutorial/Chapter 3/Boolean Expressions") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE")  
---|---|---  
  
  
Back to [Reserved words](<../Reserved_words.md> "Reserved words"). 

  
3Ca - IF (author: Tao Yue, state: changed) 

The `IF` statement allows you to branch based on the result of a Boolean operation. 

## Format

    **IF** _expression_ THEN
    _statement1_
    [ ELSE
    _statement2_]

Where 

    _expression_ is any comparison, constant or function which returns a [boolean](</index.php?title=boolean&action=edit&redlink=1> "boolean \(page does not exist\)") value, and
    _statement1_ and _statement2_ are either a single [statement](<../statement.md> "statement"), a [Begin](</index.php?title=begin&action=edit&redlink=1> "begin \(page does not exist\)")-[end](<../End.md> "End") [block](<../Block.md> "Block"), a [repeat](<../Repeat.md> "Repeat")-until block, or the [null statement](</;#empty_statement> ";").

The **ELSE** clause is optional. 

There are two ways to use an `IF` statement, a one-way branch or a two-way branch/ 

## One-way branch

The one-way branch format is: 
    
    
    if BooleanExpression then
      StatementIfTrue;
    

If the Boolean expression evaluates to `true`, the statement executes. Otherwise, it is skipped. 

The `IF` statement accepts only one statement. If you would like to use a [compound statement](</index.php?title=compound_statement&action=edit&redlink=1> "compound statement \(page does not exist\)"), you must use a `begin-end` [frame](<../Frame.md> "Frame") to enclose the statements: 
    
    
    if BooleanExpression then
    begin
      Statement1;
      Statement2;
    end;
    

## Two-way branch

There is also a two-way selection: 
    
    
    if BooleanExpression then
      StatementIfTrue
    else
      StatementIfFalse;
    

Note there is never a semicolon `;` immediately before the `else`. 
    
    
    if BooleanExpression then
    begin
      Statement1;  // semicolon here is mandatory
      Statement2;  // semicolon here is optional
    end    // semicolon here is forbidden
    else
    begin
      Statement3;
      Statement4;
    end;
    

If the Boolean expression evaluates to `FALSE`, the statement following the `else` will be performed. Note that you may _never_ use a semicolon after the statement preceding the `else`. That causes the computer to treat it as a one-way selection, leaving it to wonder where the else came from. And when a compiler wonders, it usually gets mad and throws a tantrum, or rather, it throws an error 

If you need multi-way selection, simply nest `if` statements: 
    
    
    if Condition1 then
      Statement1
    else
      if Condition2 then
        Statement2
      else
        Statement3;
    

Be careful with nesting. Sometimes the computer won't do what you want it to do: 
    
    
    if Condition1 then
      if Condition2 then
        Statement2
    else
      Statement1;
    

The `else` is always matched with the most recent `if`, so the computer interprets the preceding block of code as: 
    
    
    if Condition1 then
      if Condition2 then
        Statement2
      else
        Statement1;
    

You can get by with a null statement: 
    
    
    if Condition1 then
      if Condition2 then
        Statement2
      else
    else
      Statement1;
    

Or you could use a `begin-end` block. 

The following proves a semicolon is _absolutely forbidden_ before an else: 
    
    
    // Paul Robinson 2020-12-16
    
    // Compiler test program  Err03.pas
    // tests the proposition that ; is
    // never legal before ELSE
    
    
    program Err03;
    Var
        Test,Test2: Boolean;
    
    
    Begin
    
        Test := True;
        Test2 := True;
    
        if Test then
           if Test2 then
               Writeln('Reached Part 1');  // semi-colon here should be illegal
         else
            Writeln('Reached Part 2');
    
    end.
    

But the best way to clean up the code would be to rewrite the condition. 
    
    
    if not Condition1 then
      Statement1
    else
      if Condition2 then
        Statement2;
    

This example illustrates where the not operator comes in very handy. If Condition1 had been a Boolean like: `(not(a < b) or (c + 3 > 6)) and g`, reversing the expression would be more difficult than NOTting it. 

Also notice how important indentation is to convey the logic of program code to a human, but the compiler ignores the indentation. 

[ ◄ ](<Chapter_3/Boolean_Expressions.md> "Basic Pascal Tutorial/Chapter 3/Boolean Expressions") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE")  
---|---|---  
  

[ ◄ ](<Chapter_3/Boolean_Expressions.md> "Basic Pascal Tutorial/Chapter 3/Boolean Expressions") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE")  
---|---|---  
  
  
Back to [Reserved words](<../Reserved_words.md> "Reserved words"). 

  
3Ca - IF (author: Tao Yue, state: changed) 

The `IF` statement allows you to branch based on the result of a Boolean operation. 

## Format

    **IF** _expression_ THEN
    _statement1_
    [ ELSE
    _statement2_]

Where 

    _expression_ is any comparison, constant or function which returns a [boolean](</index.php?title=boolean&action=edit&redlink=1> "boolean \(page does not exist\)") value, and
    _statement1_ and _statement2_ are either a single [statement](<../statement.md> "statement"), a [Begin](</index.php?title=begin&action=edit&redlink=1> "begin \(page does not exist\)")-[end](<../End.md> "End") [block](<../Block.md> "Block"), a [repeat](<../Repeat.md> "Repeat")-until block, or the [null statement](</;#empty_statement> ";").

The **ELSE** clause is optional. 

There are two ways to use an `IF` statement, a one-way branch or a two-way branch/ 

## One-way branch

The one-way branch format is: 
    
    
    if BooleanExpression then
      StatementIfTrue;
    

If the Boolean expression evaluates to `true`, the statement executes. Otherwise, it is skipped. 

The `IF` statement accepts only one statement. If you would like to use a [compound statement](</index.php?title=compound_statement&action=edit&redlink=1> "compound statement \(page does not exist\)"), you must use a `begin-end` [frame](<../Frame.md> "Frame") to enclose the statements: 
    
    
    if BooleanExpression then
    begin
      Statement1;
      Statement2;
    end;
    

## Two-way branch

There is also a two-way selection: 
    
    
    if BooleanExpression then
      StatementIfTrue
    else
      StatementIfFalse;
    

Note there is never a semicolon `;` immediately before the `else`. 
    
    
    if BooleanExpression then
    begin
      Statement1;  // semicolon here is mandatory
      Statement2;  // semicolon here is optional
    end    // semicolon here is forbidden
    else
    begin
      Statement3;
      Statement4;
    end;
    

If the Boolean expression evaluates to `FALSE`, the statement following the `else` will be performed. Note that you may _never_ use a semicolon after the statement preceding the `else`. That causes the computer to treat it as a one-way selection, leaving it to wonder where the else came from. And when a compiler wonders, it usually gets mad and throws a tantrum, or rather, it throws an error 

If you need multi-way selection, simply nest `if` statements: 
    
    
    if Condition1 then
      Statement1
    else
      if Condition2 then
        Statement2
      else
        Statement3;
    

Be careful with nesting. Sometimes the computer won't do what you want it to do: 
    
    
    if Condition1 then
      if Condition2 then
        Statement2
    else
      Statement1;
    

The `else` is always matched with the most recent `if`, so the computer interprets the preceding block of code as: 
    
    
    if Condition1 then
      if Condition2 then
        Statement2
      else
        Statement1;
    

You can get by with a null statement: 
    
    
    if Condition1 then
      if Condition2 then
        Statement2
      else
    else
      Statement1;
    

Or you could use a `begin-end` block. 

The following proves a semicolon is _absolutely forbidden_ before an else: 
    
    
    // Paul Robinson 2020-12-16
    
    // Compiler test program  Err03.pas
    // tests the proposition that ; is
    // never legal before ELSE
    
    
    program Err03;
    Var
        Test,Test2: Boolean;
    
    
    Begin
    
        Test := True;
        Test2 := True;
    
        if Test then
           if Test2 then
               Writeln('Reached Part 1');  // semi-colon here should be illegal
         else
            Writeln('Reached Part 2');
    
    end.
    

But the best way to clean up the code would be to rewrite the condition. 
    
    
    if not Condition1 then
      Statement1
    else
      if Condition2 then
        Statement2;
    

This example illustrates where the not operator comes in very handy. If Condition1 had been a Boolean like: `(not(a < b) or (c + 3 > 6)) and g`, reversing the expression would be more difficult than NOTting it. 

Also notice how important indentation is to convey the logic of program code to a human, but the compiler ignores the indentation. 

[ ◄ ](<Chapter_3/Boolean_Expressions.md> "Basic Pascal Tutorial/Chapter 3/Boolean Expressions") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE")  
---|---|---  
  

[ ◄ ](<Chapter_3/IF.md> "Basic Pascal Tutorial/Chapter 3/IF") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO")  
---|---|---  
  
3Cb - CASE (author: Tao Yue, state: changed) 

Case opens a case statement. The case statement compares the value of an ordinal expression to each selector, which can be a [constant](<../Const.md> "Const"), a subrange, or a list of selectors separated by [commas](<../Comma.md> "Comma"). Selector field separated to action field by [Colon](<../Colon.md> "Colon"). 

Suppose you wanted to branch one way if `b` is `1, 7, 2037,` or `5`; and another way if otherwise. You could do it by: 
    
    
    if (b = 1) or (b = 7) or (b = 2037) or (b = 5) then
      Statement1
    else
      Statement2;
    

But in this case, it would be simpler to list the numbers for which you want Statement1 to execute. You would do this with a `case` statement: 
    
    
    case b of
      1,7,2037,5: Statement1;
      otherwise   Statement2
    end;
    

The general form of the `case` statement is: 
    
    
    case selector of
      List1:    Statement1;
      List2:    Statement2;
      ...
      Listn:    Statementn;
      otherwise Statement
    end;
    

The `otherwise` part is optional. When available, it differs from compiler to compiler. Many compilers use the word `else` instead of `otherwise`. 

selector is any variable of an ordinal data type. You may not use Reals! 

Note that the lists must consist of literal values. That is, you must use constants or hard-coded values -- you cannot use variables. 

[ ◄ ](<Chapter_3/IF.md> "Basic Pascal Tutorial/Chapter 3/IF") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO")  
---|---|---  
  
# Looping


[ ◄ ](<Chapter_3/CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO")  
---|---|---  
  
## FOR...DO Loops (author: Tao Yue, state: changed)

FOR...DO is a loop construct in Pascal. Of course, that may raise the question "What is a loop?". 

### Loops

Looping means repeating a statement or compound statement over and over until some condition is met. 

There are three types of loops: 

  * fixed repetition - only repeats a fixed number of times
  * pretest - tests a Boolean expression, then goes into the loop if TRUE
  * posttest - executes the loop, then tests the Boolean expression



### FOR...DO Loop

In Pascal, the fixed repetition loop is the for loop. The general form is: 
    
    
    for index := StartingLow to EndingHigh do
      statement;
    

The index variable must be of an **ordinal** data. The index can be used in calculations within the body of the loop, but its value cannot be changed (i.e. `count:=5` will cause a program exception.) 

In Pascal, the `for` loop can only count in increments (steps) of 1. A loop can be interrupted using the _break_ statement. An example of using the index is: 
    
    
    sum := 0;
    for count := 1 to 100 do
    begin
      sum := sum + count;
      if sum = 38 then break;
    end;
    

The computer would do the sum the long way and still finish it in far less time than it took the mathematician Gauss to do the sum the short way (`1+100 = 101. 2+99 = 101`. See a pattern? There are 100 numbers, so the pattern repeats 50 times. `101*50 = 5050`. This isn't advanced mathematics, its attribution to Gauss is probably apocryphal.). 

In the `for-to-do` loop, the starting value MUST be lower than the ending value, or the loop will never execute! If you want to count down, you should use the `for-downto-do` loop: 
    
    
    for index := StartingHigh downto EndingLow do
      statement;
    

## See also

[While ...Do loops](<Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO")

[Repeat... Until loops](<../Until.md> "Until")

[For... in loops](<../for-in_loop.md> "for-in loop")

[ ◄ ](<Chapter_3/CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO")  
---|---|---  
  

[ ◄ ](<Chapter_3/CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO")  
---|---|---  
  
## FOR...DO Loops (author: Tao Yue, state: changed)

FOR...DO is a loop construct in Pascal. Of course, that may raise the question "What is a loop?". 

### Loops

Looping means repeating a statement or compound statement over and over until some condition is met. 

There are three types of loops: 

  * fixed repetition - only repeats a fixed number of times
  * pretest - tests a Boolean expression, then goes into the loop if TRUE
  * posttest - executes the loop, then tests the Boolean expression



### FOR...DO Loop

In Pascal, the fixed repetition loop is the for loop. The general form is: 
    
    
    for index := StartingLow to EndingHigh do
      statement;
    

The index variable must be of an **ordinal** data. The index can be used in calculations within the body of the loop, but its value cannot be changed (i.e. `count:=5` will cause a program exception.) 

In Pascal, the `for` loop can only count in increments (steps) of 1. A loop can be interrupted using the _break_ statement. An example of using the index is: 
    
    
    sum := 0;
    for count := 1 to 100 do
    begin
      sum := sum + count;
      if sum = 38 then break;
    end;
    

The computer would do the sum the long way and still finish it in far less time than it took the mathematician Gauss to do the sum the short way (`1+100 = 101. 2+99 = 101`. See a pattern? There are 100 numbers, so the pattern repeats 50 times. `101*50 = 5050`. This isn't advanced mathematics, its attribution to Gauss is probably apocryphal.). 

In the `for-to-do` loop, the starting value MUST be lower than the ending value, or the loop will never execute! If you want to count down, you should use the `for-downto-do` loop: 
    
    
    for index := StartingHigh downto EndingLow do
      statement;
    

## See also

[While ...Do loops](<Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO")

[Repeat... Until loops](<../Until.md> "Until")

[For... in loops](<../for-in_loop.md> "for-in loop")

[ ◄ ](<Chapter_3/CASE.md> "Basic Pascal Tutorial/Chapter 3/CASE") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO")  
---|---|---  
  

[ ◄ ](<Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/REPEAT..md> "Basic Pascal Tutorial/Chapter 3/REPEAT..UNTIL")  
---|---|---  
  
## While ... DO loops

3Db - WHILE..DO (author: Tao Yue, state: unchanged) 

The pretest loop has the following format: 
    
    
    while BooleanExpression do
      statement;
    

The loop continues to execute until the Boolean expression becomes `FALSE`. In the body of the loop, you must somehow affect the Boolean expression by changing one of the variables used in it. Otherwise, an infinite loop will result: 
    
    
    a := 5;
    while a < 6 do
      writeln (a);
    

Remedy this situation by changing the variable's value: 
    
    
    a := 5;
    while a < 6 do
    begin
      writeln (a);
      a := a + 1
    end;
    

The `WHILE ... DO` loop is called a pretest loop because the condition is tested before the body of the loop executes. So if the condition starts out as `FALSE`, the body of the `while` loop never executes. 

## See also

  * [FOR ...DO loops](<Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO")
  * [Repeat... Until loops](<../Until.md> "Until")
  * [For... in loops](<../for-in_loop.md> "for-in loop")

[ ◄ ](<Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..DO") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/REPEAT..md> "Basic Pascal Tutorial/Chapter 3/REPEAT..UNTIL")  
---|---|---  
  

[ ◄ ](<Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..IN")  
---|---|---  
  
REPEAT...UNTIL 

The repeat .. until construct is termed a _post_ -test loop, because the controlling condition is tested _after_ each iteration of the loop. 

It has the following syntax: 
    
    
    repeat
      statement1;
      // statement2;
      // further statements...
    until BooleanExpression;
    

A `repeat` loop encloses its executed statements, which means they do not need to be further enclosed in a `begin ... end` block. Note that a repeat loop continues until its controlling Boolean expression is `True`; whereas the `while` loop continues until its Boolean expression is `False`. 

For instance, the following `repeat` loop executes at least once: 
    
    
    repeat
      WriteLn(Node.Text);
      Node := GetNextNode;
    until not Assigned(Node);
    

It assumes that Node is not Nil at the outset. If this assumption is incorrect, the code will fail, and the program may crash. 

A `while` loop is more defensive, since the needed check is performed before any loop statements are executed: 
    
    
    while Assigned(Node) do
      begin
        WriteLn(Node.Text);
        Node := GetNextNode;
      end;
    

Use a `repeat` loop when the looping statement(s) must execute at least once, whatever the initial value of the controlling Boolean condition. 

It is not difficult to inadvertently write a Boolean expression controlling a repeat loop that never becomes True. This gives rise to an infinite or endless loop, and causes the program to hang. 

One programming style deliberately sets up an infinite loop, and inserts a `Break` or `Exit` instruction controlled by some condition evaluated in the middle of the loop to break out of the otherwise infinite loop: 
    
    
    repeat
      statement1;
      if Condition then
        Break;
      statement_that_might_affect_the_Condition;
    until False;
    

Successful use of this style depends on Condition dependably becoming True, without exception, at some point. Otherwise you have built in an inescapable infinite loop. 

[ ◄ ](<Chapter_3/WHILE..md> "Basic Pascal Tutorial/Chapter 3/WHILE..DO") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..IN")  
---|---|---  
  

[ ◄ ](<Chapter_3/REPEAT..md> "Basic Pascal Tutorial/Chapter 3/REPEAT..UNTIL") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 3/Programming Assignment")  
---|---|---  
  
FOR..IN 

This loop was added in Delphi 2005, and was introduced in Free Pascal 2.4.2. 

See the following page: [for..in-loops](<../for-in_loop.md> "for-in loop"), which is not part of this tutorial (don't forget to return to the tutorial!). 

[ ◄ ](<Chapter_3/REPEAT..md> "Basic Pascal Tutorial/Chapter 3/REPEAT..UNTIL") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 3/Programming Assignment")  
---|---|---  
  
# Programming Assignments: Fibonacci Sequence and Powers of Two


[ ◄ ](<Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..IN") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/Solution.md> "Basic Pascal Tutorial/Chapter 3/Solution")  
---|---|---  
  
3E - Programming Assignments (author: Tao Yue, state: unchanged) 

**Problem 1**

Find the first 10 numbers in the Fibonacci sequence. The Fibonacci sequence starts with two numbers: 
    
    
    1 1
    

Each subsequent number is formed by adding the two numbers before it. 1+1=2, 1+2=3, 2+3=5, etc. This forms the following sequence: 
    
    
    1 1 2 3 5 8 13 21 34 55 89 144 ...
    

**Problem 2**

Display all powers of 2 that are less than 20000. Display the list in a properly formatted manner, with commas between the numbers. Display five numbers per line. The output should look like: 
    
    
        1, 2, 4, 8, 16,
    32, 64, 128, 256, 512,
    1024, 2048, 4096, 8192, 16384
    

[ ◄ ](<Chapter_3/FOR..md> "Basic Pascal Tutorial/Chapter 3/FOR..IN") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_3/Solution.md> "Basic Pascal Tutorial/Chapter 3/Solution")  
---|---|---  
  
# Solutions


[ ◄ ](<Chapter_3/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 3/Programming Assignment") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Procedures.md> "Basic Pascal Tutorial/Chapter 4/Procedures")  
---|---|---  
  
3Ea - Solutions (author: Tao Yue, state: unchanged) 

**Solution to Fibonacci Sequence Problem**
    
    
    (* Author:    Tao Yue
       Date:      19 July 1997
       Description:
          Find the first 10 Fibonacci numbers
       Version:
          1.0 - original version
    *)
    
    program Fibonacci;
    
    var
       Fibonacci1, Fibonacci2 : integer;
       temp : integer;
       count : integer;
    
    begin    (* Main *)
       writeln ('First ten Fibonacci numbers are:');
       count := 0;
       Fibonacci1 := 0;
       Fibonacci2 := 1;
       repeat
          write (Fibonacci2:7);
          temp := Fibonacci2;
          Fibonacci2 := Fibonacci1 + Fibonacci2;
          Fibonacci1 := Temp;
          count := count + 1
       until count = 10;
       writeln;
    
       (* Of course, you could use a FOR loop or a WHILE loop
          to solve this problem. *)
    
    end.     (* Main *)
    

**Solution to Powers of Two Problem**
    
    
    (* Author:    Tao Yue
       Date:      13 July 2000
       Description:
          Display all powers of two up to 20000, five per line
       Version:
          1.0 - original version
    *)
    
    program PowersofTwo;
    
    const
       numperline = 5;
       maxnum = 20000;
       base = 2;
    
    var
       number : longint;
       linecount : integer;
    
    begin    (* Main *)
       writeln ('Powers of ', base, ', 1 <= x <= ', maxnum, ':');
       (* Set up for loop *)
       number := 1;
       linecount := 0;
       (* Loop *)
       while number <= maxnum do
          begin
             linecount := linecount + 1;
             (* Print a comma and space unless this is the first
                number on the line *)
             if linecount > 1 then
                write (', ');
             (* Display the number *)
             write (number);
             (* Print a comma and go to the next line if this is
                the last number on the line UNLESS it is the
                last number of the series *)
             if (linecount = numperline) and not (number * 2 > maxnum) then
                begin
                   writeln (',');
                   linecount := 0
                end;
             (* Increment number *)
             number := number * base;
          end;  (* while *)
       writeln;
    
       (* This program can also be written using a
          REPEAT..UNTIL loop. *)
    
    end.     (* Main *)
    

Note that I used three constants: the base, the number of powers to display on each line, and the maximum number. This ensures that the program can be easily adaptable in the future. 

Using constants rather than literals is a good programming habit to form. When you write really long programs, you may refer to certain numbers thousands of times. If you hardcoded them into your code, you'd have to search them out. Also, you might use the same value in a different context, so you can't simply do a global Search-and-Replace. Using a constant makes it simpler to expand the program. 

Also note that I used the `longint` type for the number variable. This is because to fail the test `number <= 20000`, `number` would have to reach `32768`, the next power of two after `16384`. This exceeds the range of the integer type: `-32768` to `32767`. (try it without `longint` and see what happens) 

[ ◄ ](<Chapter_3/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 3/Programming Assignment") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Procedures.md> "Basic Pascal Tutorial/Chapter 4/Procedures")  
---|---|---  
  
  


**4\. Chapter  
  
Subprograms**

# Procedures


[ ◄ ](<Chapter_3/Solution.md> "Basic Pascal Tutorial/Chapter 3/Solution") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Parameters.md> "Basic Pascal Tutorial/Chapter 4/Parameters")  
---|---|---  
  
4A - Procedures (author: Tao Yue, state: unchanged) 

A procedure is a subprogram. Subprograms help reduce the amount of redundancy in a program. Statements that are executed over and over again but not contained in a loop are often put into subprograms. 

Subprograms also facilitate top-down design. Top-down design is the tackling of a program from the most general to the most specific. For example, top down design for going from one room to another starts out as: 

  * Get out of first room
  * Go to second room
  * Go into second room



Then it is refined to 

  * Get out of first room 
    * Go to door
    * Open the door
    * Get out of door
    * Close door
  * ...



Just going to the door can be refined further: 

  * Get out of first room 
    * Go to door 
      * Get out of seat
      * Turn towards door
      * Walk until you almost bump into it



This, of course, can be further refined to say how much exercise should be given to your cardiac myofibrils, and how much adenosine diphosphate should be converted to adenosine triphosphate by fermentation or aerobic respiration. This may seem to be too detailed, but for computer programming, this is, in effect what you have to do. The computer can't understand general statements -- you must be specific. 

Main tasks should be contained in procedures, so in the main program, you don't have to worry about the details. This also makes for reusable code. You can just keep your procedures in one file and link that into your program. 

A procedure has the same basic format as a program: 
    
    
    procedure Name;
    
    const
      (* Constants *)
    
    var
      (* Variables *)
    
    begin
      (* Statements *)
    end;
    

There is a semicolon (not a period) at the end. 

To call the procedure from the main program, just use the name, like you would `writeln`. 
    
    
     Name;
    

Procedures are very often used to output data. It's that simple (until the next lesson, of course). 

[ ◄ ](<Chapter_3/Solution.md> "Basic Pascal Tutorial/Chapter 3/Solution") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Parameters.md> "Basic Pascal Tutorial/Chapter 4/Parameters")  
---|---|---  
  
# Parameters


[ ◄ ](<Chapter_4/Procedures.md> "Basic Pascal Tutorial/Chapter 4/Procedures") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Functions.md> "Basic Pascal Tutorial/Chapter 4/Functions")  
---|---|---  
  
4B - Parameters (author: Tao Yue, state: unchanged) 

A parameter list can be included as part of the procedure heading. The parameter list allows variable values to be transferred from the main program to the procedure. The new procedure heading is: 
    
    
    procedure Name (formal_parameter_list);
    

The parameter list consists of several parameter groups, separated by semicolons: 
    
    
    param_group_1; param_group_2; ... ; param_group_n
    

Each parameter group has the form: 
    
    
    identifier_1, identifier_2, ... , identifier_n : data_type
    

The procedure is called by passing arguments (called the actual parameter list) of the same number and type as the formal parameter list. 
    
    
    procedure Name (a, b : integer; c, d : real);
    begin
      a := 10;
      b := 2;
      writeln (a, b, c, d)
    end;
    

Suppose you called the above procedure from the main program as follows: 
    
    
    alpha := 30;
    Name (alpha, 3, 4, 5);
    

When you return to the main program, what is the value of `alpha`? 30. Yet, `alpha` was passed to `a`, which was assigned a value of 10. What actually happened was that `a` and `alpha` are totally distinct. The value in the main program was not affected by what happened in the procedure. 

This is called **call-by-value**. This passes the **value** of a variable to a procedure. 

Another way of passing parameters is **call-by-reference**. This creates a **link** between the formal parameter and the actual parameter. When the formal parameter is modified in the procedure, the actual parameter is likewise modified. _Call-by-reference_ is activated by preceding the parameter group with a `VAR`: 
    
    
    VAR identifier_1, identifier_2, ..., identifier_n : datatype;
    

In this case, constants and literals are not allowed to be used as actual parameters because they might be changed in the procedure. 

Here's an example which mixes _call-by-value_ and _call-by-reference_ : 
    
    
    procedure Name (a, b : integer; VAR c, d : integer);
    begin
      c := 3;
      a := 5
    end;
    
    begin
      alpha := 1;
      gamma := 50;
      delta := 30;
      Name (alpha, 2, gamma, delta);
    end.
    

Immediately after the procedure has been run, `gamma` has the value 3 because `c` was a reference parameter, but `alpha` still is 1 because `a` was a value parameter. 

This is a bit confusing. Think of call-by-value as copying a variable, then giving the copy to the procedure. The procedure works on the copy and discards it when it is done. The original variable is unchanged. 

Call-by-reference is giving the actual variable to the procedure. The procedure works directly on the variable and returns it to the main program. 

In other words, call-by-value is a one-way data transfer: main program to procedure. Call-by-reference goes both ways. 

[ ◄ ](<Chapter_4/Procedures.md> "Basic Pascal Tutorial/Chapter 4/Procedures") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Functions.md> "Basic Pascal Tutorial/Chapter 4/Functions")  
---|---|---  
  
# Functions


[ ◄ ](<Chapter_4/Parameters.md> "Basic Pascal Tutorial/Chapter 4/Parameters") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Scope.md> "Basic Pascal Tutorial/Chapter 4/Scope")  
---|---|---  
  
4C - Functions (author: Tao Yue, state: unchanged) 

Functions work the same way as procedures, but they always _return a single value_ to the main program through its _own name_ : 
    
    
    function Name (parameter_list) : return_type;
    

Functions are called in the main program by using them in expressions: 
    
    
    a := Name (5) + 3;
    

If your function has no argument, be careful not to use the name of the function on the right side of any equation inside the function. That is: 
    
    
    function Name : integer;
    begin
      Name := 2;
      Name := Name + 1
    end.
    

is a no-no. Instead of returning the value 3, as might be expected, this sets up an infinite recursive loop in certain language modes (e.g. {$MODE DELPHI} or {$MODE TP}; other modes require brackets for function calls even if the brackets are empty due to no parameters being required for the particular function). Name will call Name, which will call Name, which will call Name, etc. 

The return value is set by assigning a value to the function identifier. 
    
    
    Name := 5;
    

It is generally bad programming form to make use of VAR parameters in functions -- functions should return only one value. You certainly don't want the sin function to change your pi radians to 0 radians because they're equivalent -- you just want the answer 0. 

[ ◄ ](<Chapter_4/Parameters.md> "Basic Pascal Tutorial/Chapter 4/Parameters") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Scope.md> "Basic Pascal Tutorial/Chapter 4/Scope")  
---|---|---  
  
# Scope


[ ◄ ](<Chapter_4/Functions.md> "Basic Pascal Tutorial/Chapter 4/Functions") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Recursion.md> "Basic Pascal Tutorial/Chapter 4/Recursion")  
---|---|---  
  
4D - Scope (author: Tao Yue, state: unchanged) 

_Scope_ refers to where certain variables are visible. You have procedures inside procedures, variables inside procedures, and your job is to try to figure out when each variable can be seen by a procedure. 

A global variable is a variable defined in the main program. Any subprogram can see it, use it, and modify it. All subprograms can call themselves, and can call all other subprograms defined before it. 

The main point here is: within any block of code (procedure, function, whatever), the only identifiers that are visible are those defined before that block and either in or outside of that block. 
    
    
    program ScopeDemo;
    var A : integer;
    
      procedure ScopeInner;
      var A : integer;
      begin
        A := 10;
        writeln (A)
      end;
    
    begin (* Main *)
      A := 20;
      writeln (A);
      ScopeInner;
      writeln (A);
    end. (* Main *)
    

The output of the above program is: 
    
    
    20
    10
    20
    

The reason is: if two variables with the same identifiers are declared in a subprogram and the main program, the main program sees its own, and the subprogram sees its own (not the main's). The most local definition is used when one identifier is defined twice in different places. 

Here's a scope chart which basically amounts to an indented copy of a program with just the variables minus the logic. By stripping away the application logic and enclosing blocks in boxes, it is sometimes easier to see where certain variables are available. 

[![Scope.gif](https://wiki.freepascal.org/images/4/47/Scope.gif)](</File:Scope.gif>)

  * Everybody can see global variables `A, B`, and `C`.
  * However, in procedure `Alpha` the global definition of `A` is replaced by the local definition.
  * `Beta1` and `Beta2` can see variables `VCR, Betamax`, and `cassette`.
  * `Beta1` cannot see variable `FailureToo`, and `Beta2` cannot see `Failure`.
  * No subprogram except `Alpha` can access `F` and `G`.
  * Procedure `Beta` can call `Alpha` and `Beta`.
  * Function `Beta2` can call any subprogram, including itself (the main program is not a subprogram).

[ ◄ ](<Chapter_4/Functions.md> "Basic Pascal Tutorial/Chapter 4/Functions") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Recursion.md> "Basic Pascal Tutorial/Chapter 4/Recursion")  
---|---|---  
  
# Recursion


[ ◄ ](<Chapter_4/Scope.md> "Basic Pascal Tutorial/Chapter 4/Scope") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Forward_Referencing.md> "Basic Pascal Tutorial/Chapter 4/Forward Referencing")  
---|---|---  
  
4E - Recursion (author: Tao Yue, state: unchanged) 

**Recursion** means allowing a function or procedure to call itself until some limit is reached. 

The summation function, designated by an uppercase letter _sigma_ (Σ) in mathematics, can be written recursively: 
    
    
    function Summation (num : integer) : integer;
    begin
      if num = 1 
      then Summation := 1
      else Summation := Summation(num-1) + num
    end;
    

Suppose you call `Summation` for `3`. 
    
    
    a := Summation(3);
    

  * `Summation(3)` becomes `Summation(2) + 3`.
  * `Summation(2)` becomes `Summation(1) + 2`.
  * At `1`, the recursion stops and becomes `1`.
  * `Summation(2)` becomes `1 + 2 = 3`.
  * `Summation(3)` becomes `3 + 3 = 6`.
  * `a` becomes `6`.



Recursion works backward until a given point is reached at which an answer is defined, and then works forward with that definition, solving the other definitions which rely upon that one. 

All recursive procedures/functions should have a test to stop the recursion, the base condition. Under all other conditions, the recursion should go deeper. If there is no base condition, the recursion will either not take place at all, or become infinite. 

In the example above, the base condition was `if num = 1`. 

[ ◄ ](<Chapter_4/Scope.md> "Basic Pascal Tutorial/Chapter 4/Scope") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Forward_Referencing.md> "Basic Pascal Tutorial/Chapter 4/Forward Referencing")  
---|---|---  
  
# Forward Referencing


[ ◄ ](<Chapter_4/Recursion.md> "Basic Pascal Tutorial/Chapter 4/Recursion") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 4/Programming Assignment")  
---|---|---  
  
4F - Forward Referencing (author: Tao Yue, state: unchanged) 

After all these confusing topics, here's something easy. 

Remember that procedures/functions can only see variables and other subprograms that have already been defined. Well, there is an exception. 

If you have two subprograms, each of which calls the other, you have a dilemma that no matter which you put first, the other still can't be called from the first. 

To resolve this chicken-and-the-egg problem, use _forward referencing_. 
    
    
    procedure Later (parameter list); forward;
    
    procedure Sooner (parameter list);
    begin
      ...
      Later (parameter list);
    end;
    ...
    procedure Later;
    begin
      ...
      Sooner (parameter list);
    end;
    

The same goes for functions. Just stick a forward; at the end of the heading. 

[ ◄ ](<Chapter_4/Recursion.md> "Basic Pascal Tutorial/Chapter 4/Recursion") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 4/Programming Assignment")  
---|---|---  
  
# Programming Assignment: the Towers of Hanoi


[ ◄ ](<Chapter_4/Forward_Referencing.md> "Basic Pascal Tutorial/Chapter 4/Forward Referencing") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Solution.md> "Basic Pascal Tutorial/Chapter 4/Solution")  
---|---|---  
  
4G - Programming Assignment (author: Tao Yue, state: unchanged) 

A classic recursion problem, taught in all introductory Computer Science courses, is the Towers of Hanoi. In this problem, you have three vertical pegs. There is a cone-shaped tower on the leftmost peg, consisting of a series of donut-shaped discs. For example, this is what a four-story tower looks like: 
    
    
       |       |       |
       |       |       |
       *       |       |
      ***      |       |
     *****     |       |
    *******    |       |
    

The pegs are designated 1, 2, and 3 from left to right. The challenge is to move a tower (any height) from peg 1 to peg 3. In the process, no large disc may be placed on top of a smaller disc, and only one disc (the topmost disc on a peg) may be moved at any one time. 

The problem seems trivial, and it is for one or two discs. For one disc, you simply move it from peg 1 to peg 3. For two discs, move the topmost disc from peg 1 to peg 2, then 1 to 3, and finally move the smaller disc from 2 to 3. 

The problem gets harder for three or more discs. For three discs, you'd move 1 to 3, then 1 to 2, then 3 to 2. This effectively creates a two-story tower on peg 2. Then move the largest disc: 1 to 3. Now move the two-story tower on top of the large disc: 2 to 1, 2 to 3, 1 to 3. 

Your mission, should you choose to accept it -- write a program using a recursive procedure to solve the Towers of Hanoi for any number of discs. First ask the user for the height of the original tower. Then, print out step-by-step instructions for moving individual discs from one peg to another. For example, a three-disc problem should produce the following output: 
    
    
    1 to 3
    1 to 2
    3 to 2
    1 to 3
    2 to 1
    2 to 3
    1 to 3
    

As stated in the section on [recursion](<Chapter_4/Recursion.md> "Basic Pascal Tutorial/Chapter 4/Recursion"), recursion is one of the more difficult topics to grasp. Some people will look at this problem and find it extremely easy. Others will have a difficult time with it. However, once you get past the hurdle of understanding recursion, the actual coding of the program is relatively simple. 

So, if you'd like to challenge yourself, stop reading right here. If you have a little trouble, keep reading for a small _hint_. 

  
  
  


* * *

_Hint_ : the problem, like all recursive problems, reduces itself, becoming simpler with each step. Remember the three-disc problem? You first create a two-disc tower on peg 2, which allows you to move the bottommost disc on peg 1 to peg 3. Then you move the two-disc tower on top of peg 3. 

It's the same with four discs. First create a three-disc tower on peg 2, then move the biggest disc over to peg 3 and move the three-disc tower to peg 3. How do you create the three-disc tower? Simple. We already know how to move a three-disc tower from peg 1 to peg 3. This time, you're just moving from peg 1 to peg 2, then when the biggest peg is in place, you're moving the tower from peg 2 to peg 3. In this whole procedure, we can act as though the big disc doesn't exist, since it's guaranteed to be bigger than the others and thus poses no problem. Just utilize the three-disc solution, switching the numbers around. 

Good luck! 

[ ◄ ](<Chapter_4/Forward_Referencing.md> "Basic Pascal Tutorial/Chapter 4/Forward Referencing") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_4/Solution.md> "Basic Pascal Tutorial/Chapter 4/Solution")  
---|---|---  
  
# Solution


[ ◄ ](<Chapter_4/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 4/Programming Assignment") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Enumerated_types.md> "Basic Pascal Tutorial/Chapter 5/Enumerated types")  
---|---|---  
  
4Ga - Solution to Towers of Hanoi (author: Tao Yue, state: unchanged) 
    
    
    (* Author:    Tao Yue
       Date:      13 July 2000
       Description:
          Solves the Towers of Hanoi
       Version:
          1.0 - original version
    *)
    
    program TowersofHanoi;
    
    var
       numdiscs : integer;
    
    (********************************************************)
    
    procedure DoTowers (NumDiscs, OrigPeg, NewPeg, TempPeg : integer);
    (* Explanation of variables:
          Number of discs -- number of discs on OrigPeg
          OrigPeg -- peg number of the tower
          NewPeg -- peg number to move the tower to
          TempPeg -- peg to use for temporary storage
    *)
    
    begin
       (* Take care of the base case -- one disc *)
       if NumDiscs = 1 then
          writeln (OrigPeg, ' ---> ', NewPeg)
       (* Take care of all other cases *)
       else
          begin
             (* First, move all discs except the bottom disc
                to TempPeg, using NewPeg as the temporary peg
                for this transfer *)
             DoTowers (NumDiscs-1, OrigPeg, TempPeg, NewPeg);
             (* Now, move the bottommost disc from OrigPeg
                to NewPeg *)
             writeln (OrigPeg, ' ---> ', NewPeg);
             (* Finally, move the discs which are currently on
                TempPeg to NewPeg, using OrigPeg as the temporary
                peg for this transfer *)
             DoTowers (NumDiscs-1, TempPeg, NewPeg, OrigPeg)
          end
    end;
    
    (********************************************************)
    
    
    begin    (* Main *)
       write ('Please enter the number of discs in the tower ===> ');
       readln (numdiscs);
       writeln;
       DoTowers (numdiscs, 1, 3, 2)
    end.     (* Main *)
    

[ ◄ ](<Chapter_4/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 4/Programming Assignment") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Enumerated_types.md> "Basic Pascal Tutorial/Chapter 5/Enumerated types")  
---|---|---  
  
  


**5\. Chapter  
  
Data types**

# Enumerated types


[ ◄ ](<Chapter_4/Solution.md> "Basic Pascal Tutorial/Chapter 4/Solution") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Subranges.md> "Basic Pascal Tutorial/Chapter 5/Subranges")  
---|---|---  
  
5A - Enumerated Types (author: Tao Yue, state: unchanged) 

You can declare your own ordinal data types. You do this in the type section of your program: 
    
    
    type
     datatypeidentifier = typespecification;
    

One way to do it is by creating an enumerated type. An enumerated type specification has the syntax: 
    
    
    (identifier1, identifier2, ... identifiern)
    

For example, if you wanted to declare the months of the year, you would do a type: 
    
    
    type
      TMonthType = (January, February, March, April,May, June, July, August, September, October, November, December);
    

To make months starting from 1: 
    
    
    type
      TMonthType = (January=1, February, March, April,May, June, July, August, September, October, November, December);
    

You can assign more than one enumeration per value: 
    
    
    type
      TMonthType = (January=1, FirstMonth=1,February=2, SecondMonth=2...);
    

Or the example below will result in January=1, February=2, May=5,June=6, July=7: 
    
    
    type
      TMonthType = (January=1, February, May=5,June, July);
    

You can then declare a variable: 
    
    
    var
      Month : TMonthType;
    

You can assign any enumerated value to the variable: 
    
    
    Month := January; or Month := TMonthType(0);
    

All the ordinal functions are valid on the enumerated type. `ord(January) = 0`, and `ord(December) = 11`. Or if the Month variable should go to next calendaristic month a call to Succ(Month) can be made. Consider verifying with Month < High(TMonthType) that there is a next month. Get number of elements in enumeration with ord(High(TMonthType)) + 1. 

A few restrictions apply, though: enumerated types are internal to a program -- they can neither be read from nor written to a text file. You must read data in and convert it to an enumerated type. Also, the identifier used in the type (such as `January`) cannot be used in another type. 

One purpose of an enumerated type is to allow you, the programmer, to refer to meaningful names for data. In addition, enumerated types allow functions and procedures to be assured of a valid parameter, since only variables of the enumerated type can be passed in and the variable can only have one of the several enumerated values. 

[Write](<../Write.md> "Write") and Writeln can be used to print a string representing the current value in an enumerated type variable. [WriteStr](<../WriteStr.md> "WriteStr")(string-var,enum-var) can be used to place a string representation of the enum value into a string, or in one shot GetEnumName(TypeInfo(TMonthType), Ord(Month)) if typinfo unit is added in uses clause. 

To walk over the enumerated definition: 
    
    
    for i := Ord(Low(TMonthType)) to Ord(High(TMonthType)) do
      begin
         x:= GetEnumName(TypeInfo(TMonthType), Ord(i));
      end;
    

[ ◄ ](<Chapter_4/Solution.md> "Basic Pascal Tutorial/Chapter 4/Solution") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Subranges.md> "Basic Pascal Tutorial/Chapter 5/Subranges")  
---|---|---  
  
# Subranges


[ ◄ ](<Chapter_5/Enumerated_types.md> "Basic Pascal Tutorial/Chapter 5/Enumerated types") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays")  
---|---|---  
  
5B - Subranges (author: Tao Yue, state: unchanged) 

A _subrange_ type is defined in terms of another ordinal data type. The type specification is: 
    
    
    lowest_value .. highest_value
    

where `lowest_value < highest_value` and the two values are both in the range of another ordinal data type. 

For example, you may want to declare the days of the week as well as the work week: 
    
    
    type
      DaysOfWeek = (Sunday, Monday, Tuesday, Wednesday,
                    Thursday, Friday, Saturday);
      DaysOfWorkWeek = Monday..Friday;
    

You can also use subranges for built-in ordinal types such as `char` and `integer`. 

[ ◄ ](<Chapter_5/Enumerated_types.md> "Basic Pascal Tutorial/Chapter 5/Enumerated types") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays")  
---|---|---  
  
# 1-dimensional arrays


[ ◄ ](<Chapter_5/Subranges.md> "Basic Pascal Tutorial/Chapter 5/Subranges") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Multidimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays")  
---|---|---  
  
5C - One-dimensional Arrays (author: Tao Yue, state: unchanged) 

Suppose you wanted to read in 5000 integers and do something with them. How would you store the integers? 

You could use 5000 variables, lapsing into: 
    
    
    aa, ab, ac, ad, ... aaa, aab, ... aba, ...
    

But this would grow tedious (after declaring those variables, you have to read values into each of those variables). 

An array contains several storage spaces, all the same type. You refer to each storage space with the array name and with a subscript. The type definition is: 
    
    
    type
      typename = array [enumerated_type] of another_data_type;
    

The data type can be anything, even another array. Any enumerated type will do. You can specify the enumerated type inside the brackets, or use a predefined enumerated type. In other words, 
    
    
    type
      enum_type = 1..50;
      arraytype = array [enum_type] of integer;
    

is equivalent to 
    
    
    type
      arraytype = array [1..50] of integer;
    

Aside: This is how strings are actually managed internally — as arrays. Back before modern Pascal compilers added native support for strings, programmer had to handle it themselves, by declaring: 
    
    
    type
      String = packed array [0..255] of char;
    

and using some kind of terminating character to signify the end of the string. Most of the time it's the null-character (ordinal number 0, or ord(0)). The packed specifier means that the array will be squeezed to take up the smallest amount of memory. 

Arrays of characters representing strings are often referred to as buffers, and errors in handling them in the C or C++ programming languages may lead to buffer overruns. A buffer overrun occurs when you try to put, say, a 200-character string into a 150-length array. If memory beyond the buffer is overwritten, and if that memory originally contained executable code, then the attacker has just managed to inject arbitrary code into your system. This is what caused the famous Slammer worm that ran rampant on the Internet for several days. Try it in Pascal and see what happens. 

Arrays are useful if you want to store large quantities of data for later use in the program. They work especially well with for loops, because the index can be used as the subscript. To read in 50 numbers, assuming the following definitions: 
    
    
    type
      arraytype = array[1..50] of integer;
      
    var
      myarray : arraytype;
    

use: 
    
    
    for count := 1 to 50 do
      read (myarray[count]);
    

Brackets `[ ]` enclose the subscript when referring to arrays. 
    
    
    myarray[5] := 6;
    

[ ◄ ](<Chapter_5/Subranges.md> "Basic Pascal Tutorial/Chapter 5/Subranges") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Multidimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays")  
---|---|---  
  
# Multidimensional arrays


[ ◄ ](<Chapter_5/1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Records.md> "Basic Pascal Tutorial/Chapter 5/Records")  
---|---|---  
  
5D - Multidimensional Arrays (author: Tao Yue, state: unchanged) 

You can have arrays in multiple dimensions: 
    
    
    type
      datatype = array [enum_type1, enum_type2] of datatype;
    

The comma separates the dimensions, and referring to the array would be done with: 
    
    
    a [5, 3]
    

Two-dimensional arrays are useful for programming board games. A tic tac toe board could have these type and variable declarations: 
    
    
    type
      StatusType = (X, O, Blank);
      BoardType = array[1..3,1..3] of StatusType;
    var
      Board : BoardType;
    

You could initialize the board with: 
    
    
    for count1 := 1 to 3 do
      for count2 := 1 to 3 do
        Board[count1, count2] := Blank;
    

You can, of course, use three- or higher-dimensional arrays. 

[ ◄ ](<Chapter_5/1-dimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/1-dimensional arrays") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Records.md> "Basic Pascal Tutorial/Chapter 5/Records")  
---|---|---  
  
# Records


[ ◄ ](<Chapter_5/Multidimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Pointers.md> "Basic Pascal Tutorial/Chapter 5/Pointers")  
---|---|---  
  
5E - Records (author: Tao Yue, state: unchanged) 

A record allows you to keep related data items in one structure. If you want information about a person, you may want to know name, age, city, state, and zip. 

To declare a record, you'd use: 
    
    
    TYPE
      TypeName = record
        identifierlist1 : datatype1;
        ...
        identifierlistn : datatypen;
      end;
    

For example: 
    
    
    type
      InfoType = record
        Name : string;
        Age : integer;
        City, State : String;
        Zip : integer;
      end;
    

Each of the identifiers `Name, Age, City, State`, and `Zip` are referred to as fields. You access a field within a variable by: 
    
    
     VariableIdentifier.FieldIdentifier
    

A period separates the variable and the field name. 

There's a very useful statement for dealing with records. If you are going to be using one record variable for a long time and don't feel like typing the variable name over and over, you can strip off the variable name and use only field identifiers. You do this by: 
    
    
    WITH RecordVariable DO
    BEGIN
      ...
    END;
    

Example: 
    
    
    with Info do
    begin
      Age := 18;
      ZIP := 90210;
    end;
    

[ ◄ ](<Chapter_5/Multidimensional_arrays.md> "Basic Pascal Tutorial/Chapter 5/Multidimensional arrays") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_5/Pointers.md> "Basic Pascal Tutorial/Chapter 5/Pointers")  
---|---|---  
  
# Pointers


[ ◄ ](<Chapter_5/Records.md> "Basic Pascal Tutorial/Chapter 5/Records") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_6/Final_words.md> "Basic Pascal Tutorial/Chapter 6/Final words")  
---|---|---  
  
5F - Pointers (author: Tao Yue, state: unchanged) 

A [pointer](<../Pointer.md> "Pointer") is a data type which holds a memory address. A pointer can be thought of as a reference to that memory address, while a variable accesses that memory address directly. If a variable is someone's phone num‍ber, then a pointer is the page and line number where it's listed in the phone book. To access the data stored at that memory address, you dereference the pointer. 

## Memory routines

To declare a pointer data type, you must specify what it will point to. That data type is preceded with a caret (^). For example, if you are creating a pointer to an integer, you would use this code: 
    
    
    type
    	PointerType = ^integer;
    

You can then, of course, declare variables to be of type PointerType. 

Before accessing a pointer, you block off an area in memory for that pointer to access. This is done with: 
    
    
    New(PointerVariable);
    

To access the data at the pointer's memory location, you add a caret after the pointer name. For example, if PointerVariable was declared as type PointerType (from above), you can assign the memory location a value by using: 
    
    
    PointerVariable^ := 5;
    

After you are done with the pointer, you must deallocate the memory space. Otherwise, each time the program is run, it will allocate more and more memory until your computer has no more. To deallocate the memory, you use the `Dispose` command: 
    
    
    Dispose(PointerVariable);
    

A pointer can be assigned to another pointer. However, note that since only the **address** , not the value, is being copied, once you modify the data located at one pointer, the other pointer, when dereferenced, also yields modified data. Also, if you free (or deallocate) a pointer, the copied pointer now points to meaningless data. 

## Trivial usage example: singly linked lists

What is a pointer good for? Why can't you just use an integer in the examples above instead of a pointer to an integer? Well, the above is clearly a contrived example. The real power of pointers is that, in conjunction with records, it makes dynamically-sized data structures possible. If you need to store many items of one data type in order, you can use an array. However, your array has a predefined size. If you don't have a large enough size, you may not be able to accomodate all the data. If you have a huge array, you take up a lot of memory when sometimes that memory is not being used. 

A dynamic data structure, on the other hand, takes up only as much memory as is being used. What you do is to create a data type that points to a record. Then, the record has that pointer type as one of its fields. E. g. stacks and queues can all be implemented using this data structure: 
    
    
    type
    	PointerType = ^RecordType;
    	RecordType = record
    		data : integer;
    		next : PointerType;
    	end;
    

Each element points to the next. The last record in the chain indicates that there is no next record by setting its next field to a value of `nil`. 

[ ◄ ](<Chapter_5/Records.md> "Basic Pascal Tutorial/Chapter 5/Records") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_6/Final_words.md> "Basic Pascal Tutorial/Chapter 6/Final words")  
---|---|---  
  
  


**6\. Chapter  
  
Final words**


[ ◄ ](<Chapter_5/Pointers.md> "Basic Pascal Tutorial/Chapter 5/Pointers") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") |  ►   
---|---|---  
  
Final Words (author: Tao Yue, state: unchanged) 

This concludes my informal course in Pascal. You should have a reasonable understanding of the basics of Pascal by now, though this tutorial is by no means comprehensive. 

Compiler manuals contain a wealth of information about additional syntactical elements and predefined routines for use in your programs. Here are the [Free Pascal Compiler manuals](<http://www.freepascal.org/docs.var>). The Reference guide contains information about the system unit which is used automatically with the program, while the units reference manual gives an idea of how many routines there are for accomplishing all sorts of tasks. 

Good luck in your future Pascal endeavors! And when you move on to other languages and find yourself using PascalCasing for variable names, remember how Pascal has left an indelible mark on computer programming even though other languages have risen to prominence. 

[ ◄ ](<Chapter_5/Pointers.md> "Basic Pascal Tutorial/Chapter 5/Pointers") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") |  ►   
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Print_version](https://web.archive.org/web/20250516134033/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Print_version)_
