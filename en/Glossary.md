# Glossary

│ **English (en)** │

This page will never be complete. You are encouraged to continue adding things to it that — 

  * Relate to this site
  * Relate to Free Pascal
  * Relate to Lazarus



Note that the [buildfaq](<buildfaq.md> "buildfaq") also has a FPC/Lazarus glossary 

  


Contents   
---  
# A B C D E F G H I J K L M N O P Q R S T U V W X Y Z  
  
## #

## A

### Ada

Computer language derived from Pascal and Modula, and named after Charles Babbage's lady friend. Babbage (1791–1871), an Englishman, was one of the earliest exponents of computer design and published a design for a "difference engine" (which he never actually built). Ada King, Countess of Lovelace and daughter of the poet Lord Byron, was a student/colleague of Babbage and wrote a program to drive the difference engine; she can be regarded as the first computer programmer. (There was no romantic involvement with Babbage.) 

Ada is a high-level procedural language, with very similar structure and syntax to Pascal, but with several extensions. At one time in the 1980s, the U.S. Department of Defense decreed that all programming by its contractors must be done using Ada, but unfortunately there was a dearth of suitable compilers and software development tools, and the language never achieved popularity. Very few applications for the U.S. military were actually written in Ada. 

### Assembler, Assembly Language

An assembler converts human-readable symbols (that represent machine instructions) into the actual binary instructions that the computer executes. For example an assembler would translate 
    
    
    LD a,b # move the contents of location a into location b
    

into a binary sequence such as `01110010` which the machine can execute. Assembly language is the list of codified instructions that a programmer writes, which the assembler translates into machine code. 

An assembler differs from a [Compiler](<Glossary.md> "Glossary") by being processor-specific (_i.e._ , different assemblers for i386, MC68000, ARM processors, etc.) and by being much more low-level: it deals with very specific instructions to move data and perform detailed arithmetic. Many compilers (including [Free Pascal](<Glossary.md> "Glossary")) allow the programmer to embed Assembler sections within the high-level source code; such sections are, of course, only useful using the processor for which they are intended, whereas a [Compiler](<Glossary.md> "Glossary") can write programs which will work on a variety of different processors. 

### API

Application Program Interface: a set of tools such as procedural/functional calls to allow programmers to use a software package. 

An example is the API for the [MySQL](<http://www.mysql.com>) database system; its authors have published a set of definitions for procedure/function calls, with detailed specifications for each of the parameters or arguments for each function. The original MySQL API was defined for [C](<Glossary.md> "Glossary") programming, but the Free Pascal developers have written a set of corresponding procedure/function call definitions which allow [Free Pascal](<Glossary.md> "Glossary") or [Lazarus](<Glossary.md> "Glossary") programmers to use the library of MySQL function calls. 

A very important part of any API is its documentation, and any conversion of an API library to a different programming environment (_e.g._ , from C to Free Pascal) requires a really excellent set of descriptive documents. 

## B

### BASIC

BASIC is an acronym that stands for **B** eginner's **A** ll-purpose **S** ymbolic **I** nstruction **C** ode. 

A computer language once popular as a training tool, or as an experimental tool for programming, it was developed in 1963–1964 at Dartmouth College in Hanover, New Hampshire, U.S.A. by professors John G. Kemeny and Thomas E. Kurtz. The original version of BASIC was a compiled language and quite efficient on the slow computers of the early 1960s; however, a majority of people were first introduced to pared-down [interpreted](<Glossary.md> "Glossary") versions, particularly those that ran on early microcomputers, and they did not support many of the language features of Dartmouth BASIC. Whether the version in use was compiled or interpreted, it was easy and quick to make changes in one's program and see the effects. The programmer gave a line number for each statement, and the sequence of execution was determined by the order of line numbers. Extra lines could be inserted by giving them intermediate line numbers, _e.g._ , line 15 could be inserted between lines 10 and 20. The original language required the use of line numbers for every line of code. Execution could be transferred by using `GOTO <line number>`, and it was possible to develop very undisciplined habits by the uncontrolled use of GOTOs; the result was sometimes called "spaghetti code". In many ways early BASIC was the exact antithesis of [Pascal](<Glossary.md> "Glossary") with its strong emphasis on structured programming. However, even as inferior BASIC interpreters proliferated on personal computers, BASIC kept evolving at Dartmouth College and elsewhere, and eventually the need for line numbers and GOTO statements was dropped, allowing it to be used for structured programming. One modern commercial variant, for example, is True BASIC, which conforms to ANSI Standard BASIC. It allows programs to be compiled and bound to a runtime module so that they can be run independently of the development environment and compiler. 

BASIC has latterly fallen out of favour, partly because of the introduction of languages like [Pascal](<Glossary.md> "Glossary") and [C](<Glossary.md> "Glossary"), and partly because of the huge improvement in speed of computers and performance of compilers so that proper compiled programs could rapidly be written, executed and debugged. There have been some attempts to improve upon BASIC, such as Microsoft's Visual Basic — which supports some rudimentary structures and doesn't use line numbers, so there is less emphasis on GOTOs — but these latter variants are almost unrecognisable as BASIC, and owe much to C and to several [Command Line](<Glossary.md> "Glossary") or scripting languages. In fact, some Visual Basic programs may look very similar to [Free Pascal](<Glossary.md> "Glossary") or [Lazarus](<Glossary.md> "Glossary") programs. 

For more information on the origins and evolution of BASIC, see _Back to BASIC_ , by John G. Kemeny and Thomas E. Kurtz (Addison-Wesley Publishing Co., 1985 ISBN 0-201-13433-0). 

### BOM

The byte order mark (BOM) is a particular usage of the special Unicode character, U+FEFF, whose appearance as a magic number at the start of a text stream can signal several things to a program reading the text: 

  * The byte order, or endianness, of the text stream in the cases of 16-bit and 32-bit encodings;
  * The fact that the text stream's encoding is Unicode, to a high level of confidence;
  * Which Unicode character encoding is used.



BOM use is optional. Its presence interferes with the use of UTF-8 by software that does not expect non-ASCII bytes at the start of a file but that could otherwise handle the text stream. 

### [Boolean](<Boolean.md> "Boolean")

Logical data type in [Pascal](<Glossary.md> "Glossary") and other languages that can take one of two values, [True](<True.md> "True") or [False](<False.md> "False"). 

Named after George Boole, a pioneer of logical (Boolean) Algebra. 

See [Boolean_Expressions](<Boolean_Expressions.md> "Boolean Expressions") for a full description. 

### Bug

An error in a program which might cause catastrophic machine failure, failure of the program or simply incorrect performance or output. Derived from the days when computers were constructed with thermionic vacuum tubes: insects (bugs) crawling around the interior of the apparatus could cause short-circuits between terminals producing hardware failure or unpredictable performance. 

Most software development teams encourage potential users to report bugs (which are then corrected in subsequent versions) and have a clearly-defined bug-reporting mechanism (for example Lazarus [BugTracker](<http://bugs.freepascal.org/main_page.php>)). 

## C

### C programming language

A widely used computer programming language, particularly useful for system programming. Forms the basis for most of the [Linux](<Glossary.md> "Glossary") operating system, and as far as we can determine, for the [Windows](<Glossary.md> "Glossary") system. Its chief disadvantage is that it is insufficiently rigorous, and allows poor programming practices to creep in. It is also vulnerable to problems such as memory leaks and buffer overflows. By contrast, [Pascal](<Glossary.md> "Glossary") imposes a much more rigorous discipline on the programmer and encourages good programming habits and practices. 

See also [Pascal for C users](<Pascal_for_C_users.md> "Pascal for C users")

### C++

A variant of [C](<Glossary.md> "Glossary") which includes Object-Oriented Programming constructs. 

### C#

Pronounced "C-sharp", a language heavily supported by Microsoft and designed for their .NET platform. It has a syntax based on [C++](<Glossary.md> "Glossary") that includes aspects of other programming languages, most notably Object [Pascal](<Glossary.md> "Glossary") and [Java](<Glossary.md> "Glossary"). 

### CCR

Code and Component Repository 

The [Lazarus Code & Component Repository](<http://lazarus-ccr.sourceforge.net/>) on SourceForge is the host for the Lazarus Documentation files and some component libraries. 

### Class

A special kind of [Object](<Glossary.md> "Glossary") in [Object Oriented](<Glossary.md> "Glossary") programming. 

A Class is a pointer to an Object; its declaration looks very similar to an Object. 

### COBOL

**CO** mmercial and **B** usiness **O** riented **L** anguage 

A language widely used in business in the 1960s and later. Despite its deficiencies, and the introduction of newer languages, it is still in widespread use, mainly as a legacy system. The most notable recent [[1]](<http://www.theregister.co.uk/2008/08/14/cobol_california/>)example is in the State of California, where the government payroll system still uses COBOL, and the governor's recent edict that state employees' salaries should be cut has run into difficulties, because there aren't enough COBOL programmers around! 

### Command Line Interpreter

**CLI**

A program present in most computer operating systems which examines an input stream, either from the keyboard (Console Mode) or from a text file (Batch Mode), [interpreting](<Glossary.md> "Glossary") or parsing the text to find instructions causing execution of operating system commands or running prepared programs or other scripts of instructions. 

The CLI environment, where the operator types commands on a keyboard at a console, exists in contrast to the [ Graphical User Interface](<Glossary.md> "Glossary") (GUI), where the operator makes more use of a mouse, and pictures rather than text convey information to the operator. 

### Compiler

A program that translates Human-readable source code, usually written in a high-level language such as Object [Pascal](<Glossary.md> "Glossary"), [C++](<Glossary.md> "Glossary"), Algol, [FORTRAN](<Glossary.md> "Glossary") or [Ada](<Glossary.md> "Glossary"), into machine instructions for execution by a computer. 

Compilation usually involves several stages: Parsing the source code to check for syntax errors, translation of the source code to machine instructions, and [ linking](<Glossary.md> "Glossary") the resultant code with libraries in order to produce the final machine-executable application. 

See also [Compiler](<Compiler.md> "Compiler") and [Compilers](<Compilers.md> "Compilers") entries in main Wiki. 

### Component

A small piece of code, typically consisting of a data definition or a small number of methods, which defines and describes a particular action or series of actions in a Pascal application. Components are typically grouped together functionally into libraries such as the [Free Pascal Component Library (FCL)](<Glossary.md> "Glossary"), [Lazarus Component Library (LCL)](<Glossary.md> "Glossary") or [Run Time Library (RTL)](<Glossary.md> "Glossary"), so that they can be re-used in many programming applications. 

### Cross Compilation

The act of compiling a program on one type of computer for eventual use on another computer with a different processor a different operating system. For example [Pascal](<Glossary.md> "Glossary") programs can be cross-compiled on a PC running Linux and executed on a PC running [Windows](<Glossary.md> "Glossary"), or programs can be compiled on a PC to run on a [PDA](<Glossary.md> "Glossary") like the Sharp [Zaurus](<Glossary.md> "Glossary") or devices using [ WinCE](<Glossary.md> "Glossary"). 

Another typical example is the Cross-compilation of the [Free Pascal](<Glossary.md> "Glossary") Compiler or the [Lazarus](<Glossary.md> "Glossary") IDE on one platform such as [Linux](<Glossary.md> "Glossary") for use on another platform such as [Windows](<Glossary.md> "Glossary") or on the [Macintosh](<Glossary.md> "Glossary") computer using a totally different processor. 

### Cross-platform

Usually refers to a program that can be run on several different [Operating Systems](<Glossary.md> "Glossary") and Platforms such as [Windows](<Glossary.md> "Glossary"), [Linux](<Glossary.md> "Glossary") and [OSX](<Glossary.md> "Glossary"). Examples are the [Free Pascal](<Glossary.md> "Glossary") compiler and [Lazarus](<Glossary.md> "Glossary"), the [OpenOfficeOrg](<http://www.openoffice.org>) suite of office programs, the [Mozilla](<http://www.mozilla.org>) family of Web browsers and e-mail servers. 

### CVS

[Concurrent Version System](<http://www.cvshome.org/>) (see also [Subversion SVN](<Glossary.md> "Glossary")). A system for producing orderly development of a software suite despite its use by multiple authors. A repository is set up, from which intending developers can check-out documents or files, and to which they can return edited material. Usually the posting of new or updated material is under the control of one or more moderators or administrators, and strict version control is maintained. 

## D

### DataBase

A computer application designed for the structured storage of large amounts of data and for easy access to it. Many of the Web sites on the Internet make extensive use of databases: For example, a vendor may keep his catalogue of items for sale on a database, and may also keep his customer transactions on another related database; a software development team (like the Free Pascal team) might keep their library of programs and subprograms on a database; a medical research team might keep responses to their Case Report forms (CRFs) on a database. 

Some typical examples are [MySQL](<http://www.mysql.com>), [PostgreSQL](<http://www.postgresql.org/>), [Oracle](<http://www.oracle.com/index.html>), [DB2](<http://www-306.ibm.com/software/data/db2/>). Many of these use a common standard [Structured Query Language](<Glossary.md> "Glossary") to place data in the database and retrieve it. Free Pascal/Lazarus has links to several of these databases. 

A database system typically consists of the actual Data files, a Server which reads and writes to the files, and a Client which interprets the users instructions. 

See also [Databases](<Databases.md> "Databases") entry in main Wiki 

### [Delphi](<Delphi.md> "Delphi")

An excellent commercial [Pascal-based](<Glossary.md> "Glossary") [RAD](<Glossary.md> "Glossary") [IDE](<Glossary.md> "Glossary") for Windows made by [Borland](<http://www.borland.com/>). 

[ Free Pascal](<Glossary.md> "Glossary") sets out to be broadly compatible with Delphi, so that applications written by Delphi developers can be readily ported to Free Pascal/Lazarus, and the Free Pascal compiler has a Delphi compatability mode (using the compiler directive {$MODE Delphi} or the command-line option -Sd). 

However, slavish adherence to Delphi constructs and standards is no longer felt to be a requirement, and Free Pascal developers have improved on many of the Delphi constructs, in many cases producing a more flexible and more useful programming environment. All the procedures, functions, classes and other programming elements have been written from scratch by Free Pascal developers, quite independent of the Delphi code. 

### Debugger

A computer program which can be used by developers to control the execution of a program in development, and show where a program is failing. Debuggers typically keep track of the location in the program (for example the line number in the source code), the contents of the variables in the program, and often produce a back-trace showing the instructions that were being executed immediately before the failure occurred in the program. 

The Gnu Debugger [GDB](<http://sourceware.org/gdb/>) is used in Free Pascal/Lazarus systems; GDB must be present in the system. In Linux and other Unix-like systems the native Gnu/Linux version can easily be installed; in Windows systems a special Windows version of GDB has been included in the Lazarus release packages; alternatively GDB can be compiled from source for Windows and used in conjunction with the [CygWin](<http://cygwin.com/>) emulator. 

At present the documentation on debugging for Free Pascal/Lazarus is not very complete, and users are advised to read the documentation on the GDB site carefully. 

### [diff](<http://www.gnu.org/software/diffutils/diffutils.html>)

A GNU program used to show the differences between two files, or even two directories. 

Used by software developers to check the changes in coding before committing an edited source file to a repository such as [SVN](<Glossary.md> "Glossary"). The output from diff can also be used to generate a Patch which can be applied to a source code file without having to re-write or re-import the whole file. 

## E

## [Editor](<Editor.md> "Editor")

Text Editor — one of the essential tools of program development. 

The source code of most computer programs consists of lines of text with the program instructions arranged in a logical sequence. Some form of text editor is necessary for inputting the source code and editing or correcting it. 

All operating systems have available a number of text editors, some very primitive (such as ED or EDLIN in DOS/Windows or ED, vi in Unix/Linux) and some quite sophisticated (such as NotePad and WordPad in Windows, GEdit, KATE, KWrite and EMACS in Unix/Linux). It is even possible (though not recommended) to use Word Processor software for editing computer program source code. 

Many text editors offer Syntax Highlighting (using different colours to show keywords, types of data or levels of nesting in statements) or have automatic indentation to make the structure of a program more apparent. Most have Search and/or Replace facilities, and can often recognise the beginning of Subprograms (Procedures and Functions) or open "include" files when required. 

Both the [Free Pascal](<Glossary.md> "Glossary") and [Lazarus](<Glossary.md> "Glossary") [IDEs](<Glossary.md> "Glossary") have integral text editors which are the ones recommended for developing programs using the Free Pascal Compiler, but programs developed using any text editor can be passed to the compiler, provided they are syntactically and logically correct. 

Each user will find his or her own favourite editor and come to recognise its advantages and limitations. And arguments between proponents of one or other editor are sometimes only slightly less heated than arguments about preferred Operating System!! 

## F

### [FCL](<FCL.md> "FCL")

[Free Component Library](<FCL.md> "FCL")

The main collection of components used by [Free Pascal](<Glossary.md> "Glossary"). See also [RTL (Run Time Library)](<Glossary.md> "Glossary") and [LCL (Lazarus Component Library)](<Glossary.md> "Glossary")

### Firebird

Free relational database system supported by FPC/Lazarus; see [Firebird](<Firebird.md> "Firebird")

### FORTRAN

Procedural language for _FOR_ mula _TRAN_ slation. 

Once very popular for academic, scientific and engineering programs, its emphasis was on getting things done efficiently — very good for calculation-intense applications, but it made few concessions to ease of user interface. 

FORTRAN suffered from a rather rigid syntactic and formatting structure, though later versions have developed softer edges and make a few concessions to a windowed user environment with interactive input-output. 

It is still one of the best languages for doing really hard sums, but is much less popular than languages with a friendlier user interface. 

### FreeBSD

[FreeBSD](<Portal_FreeBSD.md> "Portal:FreeBSD") is an [Operating System](<Glossary.md> "Glossary") for a variety of platforms which focuses on features, speed, and stability. It is derived from BSD, the version of UNIX® developed at the University of California, Berkeley. It is developed and maintained by a large community. 

### [Free Pascal](<http://www.freepascal.org>)

An [OpenSource](<Glossary.md> "Glossary") [CrossPlatform](<Glossary.md> "Glossary") [Pascal](<Glossary.md> "Glossary") [Compiler](<Glossary.md> "Glossary") which supports [TurboPascal](<Glossary.md> "Glossary")/BorlandPascal (TP/BP), [Delphi](<Glossary.md> "Glossary")/[Kylix](<Glossary.md> "Glossary") ([OO](<Glossary.md> "Glossary") or Object Pascal) and Apple syntax, also it has some extra addons like C-style macros and operators/symbols, Operator Overloading, auto function overloading and other nice features. 

Free Pascal initially set out to reproduce the features of Delphi in an open-source environment, and offered the advantage of working on a number of operating system platforms, but it has acquired a character and ethos of its own, and slavish adherance to Delphi compatibility is no longer the fundamental driver. Free Pascal developers write code entirely independently from the Delphi sources. 

Free Pascal consists of the [compiler](<Glossary.md> "Glossary") itself, a number of libraries including the Run-time library [RTL](<Glossary.md> "Glossary"), the Free Pascal Component Library [FCL](<Glossary.md> "Glossary") and a number of optional packages that can be installed by the user. There is also an Integrated Development Environment ([IDE](<Glossary.md> "Glossary")) intended for use in a non-graphic text mode, and very similar to the TurboPascal IDE. Users who prefer to use a Graphical User Interface ([GUI](<Glossary.md> "Glossary")) can use one of a number of products, [Lazarus](<Glossary.md> "Glossary") being the best-known IDE which has been contributed to by over 120 programmers of whom some are closely involved with the Free Pascal project. 

## G

### [GDB](<http://www.gnu.org/software/gdb/gdb.html>)

[GNU](<Glossary.md> "Glossary") [Debugger](<Glossary.md> "Glossary") used for debugging programs developed with the [Free Pascal](<Glossary.md> "Glossary") compiler, as well as programs in [C](<Glossary.md> "Glossary"), [C++](<Glossary.md> "Glossary"), [FORTRAN](<Glossary.md> "Glossary") and other languages. 

### [GIMP](<http://www.gimp.org/>)

[GNU](<Glossary.md> "Glossary") Image Manipulation Program 

One of the largest influences on Linux Graphics — it is a tool that some find hard to use at first, but it so infinitely configurable and intensively useful that most are eventually won over. The GIMP provides most of the [ Widgets](<Glossary.md> "Glossary") (small graphic objects) used in the Gimp ToolKit (GTK) libraries that form one of the major sets of tools in Lazarus. 

### [GNU](<http://www.gnu.org/>)

GNU is a wrapper around the [Linux](<Glossary.md> "Glossary") Kernel and Shell, providing a large number of user programs, user environments (both graphical and text-based), applications and development systems. It also contains a large number of libraries for use by various packages and systems. Its developers suggest that the [Linux](<Glossary.md> "Glossary") operating system should actually be described as the GNU/Linux OS, as the Linux kernel requires the functionality provided by GNU in order to work for the end-user. 

From GNU's Website: GNU is a recursive acronym for “GNU's Not UNIX”; it is pronounced “guh-noo.” GNU is like [UNIX](<Glossary.md> "Glossary"), but is not the same! 

### [GTK](<http://www.gtk.org/>)

[Gimp](<Glossary.md> "Glossary") Tool Kit. 

GTK+ is a multi-platform toolkit for creating graphical user interfaces, and [Free Pascal](<Glossary.md> "Glossary") and the [Lazarus](<Glossary.md> "Glossary") [IDE](<Glossary.md> "Glossary") make extensive use of GTK. It contains a set of [Widgets](<Glossary.md> "Glossary") or small graphical objects which can be incorporated into the forms or panels of applications. 

### GUI

Graphical User Interface 

A working environment in which a computer user is presented with a screen on which there are pictures or Icons representing programs, actions or files, and either uses a mouse cursor (or similar pointing device) to select the appropriate icon or uses a keyboard with directional buttons or keys to move around the screen and select the appropriate icon. There are often drop-down menus available when the mouse is placed over certain parts of the screen. 

A GUI typically makes use of [Object-Oriented](<Glossary.md> "Glossary") or event-driven programming; instead of following a pre-determined sequence of actions, the application waits for an event such as a mouse-click over a particular icon, to determine what action is required and execute the appropriate piece of code; the application then goes back into the "wait" state until another event occurs, such as a mouse click over a different icon. 

## H

## I

### IDE

Integrated Development Environment 

It typically consists of a [text editor](<Glossary.md> "Glossary") for developing the program source, a [Compiler](<Glossary.md> "Glossary"), perhaps an [Assembler](<Glossary.md> "Glossary") and a [Linker](<Glossary.md> "Glossary") to make construction and subsequent execution of the program quick and easy. It may also have access to a [Debugger](<Glossary.md> "Glossary") to help detect and correct logical or operational errors in the code. 

### Interpreter

A software tool that examines an input stream of computer-language instructions (from the console or from a text-file) and converts them into machine-code instructions which are then immediately executed. This differs from a [compiler](<Glossary.md> "Glossary") (which converts a whole file into machine instructions and then stores the code for subsequent execution) in that instructions are interpreted line-by-line, and the conversion process has to happen every time the program is run. 

The [Command-Line interpreters](<Glossary.md> "Glossary") in [Linux](<Glossary.md> "Glossary"), [Windows](<Glossary.md> "Glossary"), IBM systems and DEC PDP and VAX systems are good examples. Several popular programming languages are interpretive rather than compiled. The best examples are [BASIC](<Glossary.md> "Glossary") (in all its variants including Visual BASIC), PERL, Python and [Java](<Glossary.md> "Glossary"). Interpreted-language programs are generally several orders of magnitude slower than compiled-language programs because of the need to re-interpret the commands before execution every time, but they have the great advantage that changes are easily made, new code can be tested quickly, and often speed is not all that important compared with ease of development. Some languages (including various dialects of BASIC) offer the programmer an opportunity to try out a program first in interpreted mode, then when the program works correctly and no more changes are necessary, the whole program can be compiled to produce a much faster product. 

Some variants of Pascal, including [UCSD Pascal](<UCSD_Pascal.md> "UCSD Pascal") from the University of California at San Diego (UCSD), offered a peculiar combination of compiled and interpreted operation: The textual Pascal source code was first converted into an intermediate P-code, which was then passed on to a P-code interpreter that translated and executed it. 

## J

### Java

Computer language developed by Sun Microsystems for writing Web-based applications. It is an [interpreted language](<Glossary.md> "Glossary") (which tends to produce rather slowly-running programs), but applications can be written rapidly and ported to a large number of different processors and machines. It has facilities for [Object-Oriented Programming](<Glossary.md> "Glossary"). 

See also: [Pascal for Java users](<Pascal_for_Java_users.md> "Pascal for Java users")

## K

### Kylix

Delphi-like Rad Tool for Linux made by Borland. It is based on Delphi, and uses the [QT](<Glossary.md> "Glossary") / CLX [widget](<Glossary.md> "Glossary") set. It attempts to port Delphi's IDE to Linux by using [WINE](<http://www.winehq.org/>), a Windows Emulator program, rather than translating the code for the IDE into proper Linux-specific code, and hence operates rather slowly and patchily. Borland seem to have lost interest in developing or supporting Kylix further, and many former users of Kylix have migrated to Free Pascal/Lazarus as the only viable alternative for Pascal development in Linux. 

## L

### [Lazarus](<http://lazarus.freepascal.org>)

A [CrossPlatform](<Glossary.md> "Glossary") [RAD](<Glossary.md> "Glossary") [IDE](<Glossary.md> "Glossary") made with [Free Pascal](<Glossary.md> "Glossary") and designed to use Free Pascal as its programming language and compilation tool. It has many of the features of the IDE found in Borland's [Delphi](<Glossary.md> "Glossary") and [Kylix](<Glossary.md> "Glossary") (although all the code has been written independently), and has some features which are missing from Delphi/Kylix, while lacking a few of the features found in the Borland products. 

Its main user interface includes a Source Code [Editor](<Glossary.md> "Glossary"), a Main Menu bar containing the Component Palette, a Form Design Window and an Object Inspector. Other features include a Documentation editor, considerable Code Help, and access to extensive libraries of components and packages. 

Developers typically use the IDE's Form Designer to sketch out the physical structure of their applications, use the Source Code Editor to add programming instructions to respond to events occurring during program execution, and then use the built-in Free Pascal compiler and linker to produce executable code. During execution of the code, the [debugger](<Glossary.md> "Glossary") is used to trap errors and find logical problems in the program. 

The IDE can even be used to rebuild Lazarus itself, when corrections or updates are received, for example, from the [SVN](<Glossary.md> "Glossary") repositories. 

### LCL

[Lazarus Component Library](<LCL.md> "LCL"), contains a large number of units defining classes, components and methods for creating applications using [Free Pascal](<Glossary.md> "Glossary"). The LCL consists mainly of visual components and objects, often called [Widgets](<Glossary.md> "Glossary"): some examples are Buttons, Labels, Edit Boxes, Drop-down Lists, Popup Menus, Images and Grids. The non-visual elements such as file handling and database management are found in the [Free Pascal Component Library](<Glossary.md> "Glossary") and [Run Time Library](<Glossary.md> "Glossary"). Many of the components defined in the LCL and other libraries are used to build the Lazarus [IDE](<Glossary.md> "Glossary"). 

See [Lazarus Components](<Lazarus_Components.md> "Lazarus Components") for a list of components available, and their equivalent in TurboPascal and Delphi. 

### Linker

A computer program that takes the output from a [Compiler](<Glossary.md> "Glossary") and finds any pre-compiled library routines that are needed, and joins them all together to produce an executable program. 

### [Linux](<http://www.linux.org/>)

[Linux](<Linux.md> "Linux") is an [Open Source](<Glossary.md> "Glossary") [Operating System](<Glossary.md> "Glossary") derived originally from [Unix](<Glossary.md> "Glossary"), but totally re-built using Open Source code, and receiving contributions from literally thousands of developers. Today it is one of the most popular Operating Systems. It is one of the platforms for which [Free Pascal](<Glossary.md> "Glossary") and [Lazarus](<Glossary.md> "Glossary") are built, and one of the platforms extensively used for their development. 

## M

### MAC

  * In computer hardware, Mac refers to Apple Macintosh computers.


  * In networking, MAC means Media Access Control or Medium Access Control, a low level unique identifier for a network interface card (NIC) which it is possible to easily change via software.



### macOS

  * Refers to the Macintosh computer [Operating System](<Glossary.md> "Glossary") from Apple, it is famous for its high quality graphics and stability. Since 2001 macOS has been based on [UNIX®](<Glossary.md> "Glossary") and shares many commonalities with [FreeBSD](<Glossary.md> "Glossary") and some commonalities with [Linux](<Glossary.md> "Glossary"). It is capable of running the [X Window System (X11)](<Glossary.md> "Glossary") windowing system for bitmap displays, common on Unix-like operating systems. macOS was previously marketed as [Mac OS X](<Glossary.md> "Glossary") until 2012 and [OS X](<Glossary.md> "Glossary") until 2016. After Windows, it is the most popular desktop Operating System.



### Mac OS

In 1984, Apple debuted the [Operating System](<Glossary.md> "Glossary") that is now known as the "Classic" Mac OS with its release of the original Macintosh System Software. The system, rebranded "Mac OS" in 1996, was preinstalled on every Apple Macintosh computer until 2002 and offered on Macintosh clones for a short time in the 1990s. Noted for its ease of use, it was also criticized for its lack of modern technologies compared to its competitors. 

### Mac OS X

The first desktop version (version 10.0) of the Apple Macintosh [Operating System](<Glossary.md> "Glossary") known as Mac OS X was released in March 2001. With the release of Mountain Lion (version 10.8) in 2012 Apple shortened the name to [OS X](<Glossary.md> "Glossary") and then changed it to [macOS](<Glossary.md> "Glossary") in 2016. 

### MariaDB

Fork of MySQL; aims to be compatible but more generously licensed for commercial use. 

### Mormot

mORMot : Synopse [mORMot](<mORMot.md> "mORMot") is an Open Source Client-Server ORM SOA MVC framework for FPC, targeting Win/Linux for the server, and any platform for clients (including mobile or AJAX). In other words, nobody knows what it is supposed to be but a heap of gibberish. 

### MySQL

Relational database system; see [MySQL](<MySQL.md> "MySQL"). 

## N

## O

### Object

A special type of [Record](<Glossary.md> "Glossary") used in [Free Pascal](<Glossary.md> "Glossary") and other high-level [Object Oriented](<Glossary.md> "Glossary") languages. As well as containing a group of data Fields (as in a conventional Record), an Object can define Methods (Procedures and Functions, either static, virtual or non-virtual) for processing the data in the fields. For example, an object could contain an array of real values, together with a Method for calculating the average. 
    
    
     Type
       Average = Object
         NumVal: Integer;
         Values: Array [1..200] of Real;
         Function Mean  : Real; { calculates the average value of the array }
       End;
    

Or a Button placed on a Form in a graphical environment might have the following structure (highly simplified): 
    
    
     Type
       Button = Object
         Top, Left: Integer; { screen coordinates to define button's position }
         Height, width: Integer;
         Color: TColor;
         Caption : String;
         Procedure OnClick; { method invoked when the mouse is clicked on the Button's image}
       End;
    

The Procedure or Function in the above definition must, of course, be declared elsewhere in the program, typically in an **Implementation** section. 

See also [Classes](<Glossary.md> "Glossary")

### OO

**Object Oriented**

Extension to a procedural language which enables the creation and manipulation of complicated [Objects](<Glossary.md> "Glossary"). 

While Standard [Pascal](<Glossary.md> "Glossary") maintains a [Record](<Glossary.md> "Glossary") structure, in which various different data types such as [integers](<Integer.md> "Integer"), reals, arrays, [strings](<String.md> "String") and [pointers](<Pointer.md> "Pointer") can co-exist in a predefined way, an object-oriented language like [Free Pascal](<Glossary.md> "Glossary") allows the structure also to contain **Methods** (procedures and functions) for the manipulation of the data elements within the structure. 

See also [Object Oriented Programming with Free Pascal and Lazarus](<Object_Oriented_Programming_with_Free_Pascal_and_Lazarus.md> "Object Oriented Programming with Free Pascal and Lazarus")

### Open Source

A system of software development based on publishing all your source code where it is freely available for others to view, copy, compile and use, and also to correct, modify and improve. Users, developers or modifiers are simply required or expected to acknowledge where the source came from, how they have changed it, and make the source available to anyone to whom they give or sell their program. It exists in contrast to the usual commercial model of software development, where programs are shrouded in secrecy and patent/copyright legality, only a few members of the team have access to the code, and the end user has no idea of how the program works or how to fix it if it breaks. 

From the outset, [Free Pascal](<Glossary.md> "Glossary") and [Lazarus](<Glossary.md> "Glossary") have been developed as open source programs, with all the source code freely available to anybody who wants to use it, and with encouragement to all to try it out, suggest bug-fixes, attempt to improve it or complete the parts that have not been implemented, and generally contribute to its development. This approach has led to an excellent cooperative effort, rapid development of the system and a development environment that is completely transparent for all its users. 

### OS

The **Operating System** is a program that controls the basic performance of a computer. It deals with the input-output functions of the various parts (such as keyboard, mouse, video display, disk storage, memory and peripherals such as serial, parallel and USB interfaces); it controls scheduling of tasks and allocation of processing time between processes and users; it traps system errors and performs a host of other functions. Most of these activities proceed without the user being aware of them. 

An operating system usually has a part called the **kernel** (_i.e._ , the most deeply embedded part of the program) which is started by the **BIOS** (Basic Input-Output System of the computer) and it has various libraries which provide an interface with the computer hardware. The Kernel can also be considered an abstraction layer between the applications and the electronics part. 

There is often another "layer", called the **shell** , which represents an interface between the Kernel and the user who types commands on a keyboard or moves a mouse around the table to select commands displayed on the screen. 

An Operating Systems can operate in Console (Text) or Graphical Mode. Most modern Operating Systems support threading and are multitasking, allowing multiple programs to run at the same time. 

### OS X

The first desktop version (version 10.0) of the Apple Macintosh [Operating System](<Glossary.md> "Glossary") known as [**Mac OS X**](<Glossary.md> "Glossary") was released in March 2001. With the release of Mountain Lion (version 10.8) in 2012 Apple shortened the name to **OS X** and then changed it to [**macOS**](<Glossary.md> "Glossary") in 2016. 

## P

### [Pascal](<http://en.wikipedia.org/wiki/Pascal_programming_language>)

Pascal is a procedural programming language, designed in 1968–1969 and published in 1970 by Niklaus Wirth (see [Pascal History](<Basic_Pascal_Tutorial/History.md> "Basic Pascal Tutorial/History"), [Why_use_Pascal](<Why_use_Pascal.md> "Why use Pascal")). 

There is an international standard definition of the language (ISO 7185, equivalent to ANSI/IEEE770X3.97), but apart from [Gnu Pascal](<http://www.gnu-pascal.de/gpc/h-index.html>) ([Open Source](<Glossary.md> "Glossary")) and Prospero Pascal (a commercial product available only for [Windows](<Glossary.md> "Glossary") systems), not many of the modern implementations conform exactly to the Standard. However, they are all essentially similar in syntax and structure, and programs written for one variant are fairly interchangeable with others. 

[Free Pascal](<Glossary.md> "Glossary"), [Delphi](<Glossary.md> "Glossary") and other variants of Pascal have Object Oriented extensions. 

### PDA

Personal Digital Assistant — a pocket-sized (or Palm-sized) device on which information such as address-books, calendars and other useful information can be stored. These devices (such as the Palm-Pilot and the Compaq Pocket-PC) are becoming increasing sophisticated, and the Sharp [Zaurus](<Glossary.md> "Glossary") which has a [Linux](<Glossary.md> "Glossary") operating system is a popular vehicle for experimenters and developers. 

There is a [Free Pascal](<Glossary.md> "Glossary") [cross-compiler](<Glossary.md> "Glossary") available, which allows Pascal programs compiled on a PC to be run on the Zaurus, and there is also a WinCE [cross-compiler](<Glossary.md> "Glossary") branch of FPC that allows applications to be developed in Windows for operation on the Pocket-PC. 

The PDA functions are now largely incorporated into SmartPhones and portable Tablet computers, most of which run the Android operating system, a Pascal-derived OS developed by Google. Some Lazarus developers are producing applications for the Android platform. 

### PostgreSQL

Free relational database system supported by FPC/Lazarus; see [PostgreSQL](<postgresql.md> "postgresql")

## Q

### [Qt interface](<http://wiki.lazarus.freepascal.org/index.php/Qt_Interface>)

Qt is one of the major [widget](<Glossary.md> "Glossary") sets used in Linux, and forms the basis for the KDE (K-desktop environment). It is one of the libraries of widgets actively supported by Lazarus-FPC, though it is not as well-established as the GTK libraries. 

## R

### RAD

Rapid Application Development (Examples: [Delphi](<Glossary.md> "Glossary"), [Lazarus](<Glossary.md> "Glossary"), Visual Basic) 

A software package for the fast and easy creation of Applications (Programs). Typically includes a [text editor](<Glossary.md> "Glossary"), a [graphic user interface](<Glossary.md> "Glossary"), and easy links to various tools such as [compilers](<Glossary.md> "Glossary"), [linkers](<Glossary.md> "Glossary") and [debuggers](<Glossary.md> "Glossary"). Frequently offers an Integrated Development Environment ([IDE](<Glossary.md> "Glossary")). 

### [Record](<Record.md> "Record")

A highly structured data type in [Pascal](<Glossary.md> "Glossary") and other high-level languages. 

While simple data structures such as arrays or sets consist of elements all of the same type, a record can consist of a number of elements of different types, and can take on a huge complexity. 

Records are widely used in Pascal, to group data items together logically. They also form the basis for even more complicated structured types such as [Objects](<Glossary.md> "Glossary") and [Classes](<Glossary.md> "Glossary"), used in [Object Oriented](<Glossary.md> "Glossary") programming. See also: [Record](<Record.md> "Record")

### [RTL](<RTL.md> "RTL")

[Runtime Library](<RTL.md> "RTL")

The library of components in [Free Pascal](<Glossary.md> "Glossary") which are used at run-time to translate instructions from programs according to the operating system and computer architecture of the current platform. See also [Free Pascal Component Library FCL](<Glossary.md> "Glossary")

### [RTTI](<RTTI.md> "RTTI")

Runtime Type Information 

Keeping the data-types of objects available at runtime. Can be useful for streaming and typecasting objects. 

## S

### Startup

[Startup](<Startup.md> "Startup")

### SQL

Structured Query Language. 

An universal script language used in various types of [DataBases](<Glossary.md> "Glossary") with a defined syntax to be used for execution of queries. 

### SVN

[Subversion](<http://subversion.tigris.org/>)

A document version control system designed to improve upon and ultimately to replace [CVS](<Glossary.md> "Glossary"). 

SVN is the document management system currently used by both the [Free Pascal](<Glossary.md> "Glossary") and the [Lazarus](<Glossary.md> "Glossary") projects. 

## T

### [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal")

A Pascal compiler produced by Borland which revolutionised Pascal development on PCs and microcomputers. 

Previous Pascal compilers were huge and cumbersome, or slow, or expensive, or all three. 

Borland's TurboPascal was very fast, very economic in resources, and very cheap. 

Unfortunately it did not comply in all respects with the ISO standard, notably in its handling of the get() and put() functions with text files. 

However it has been very popular, and with object oriented extensions it formed the basis of Delphi (which, though it has a stripped-down free no-cost version, is rather expensive if you want to do serious computing) and Kylix, the Linux version of Delphi. 

It also forms the foundation for Free Pascal and Lazarus, but the main difference between Borland's compilers and Free Pascal is that the source is not available for Borland, whereas all the sources for Free Pascal are readily available. Borland compilers are commercial (even if offered at no-cost) but Free Pascal and Lazarus are free ([open-source](<Glossary.md> "Glossary")). 

## U

### Unix

One of the first [Operating Systems](<Glossary.md> "Glossary"). It was written in [C](<Glossary.md> "Glossary") and had threading and multi-process capabilities; later Linus Torvalds and other programmers made an Open Source system resembling Unix called [Linux](<Glossary.md> "Glossary") which is one of the most popular Operating Systems today. Others followed the example and other Operating Systems were born: FreeBSD, OpenBSD, NetBSD and many others; also Apple introduced Open Source parts of various Unix clones and made MAC OS X which is famous for its graphical features and stability. 

### [Unit](<Unit.md> "Unit")

A Pascal source file is also called a unit and usually has .pas, .pp or .p extension. 

The Pascal unit has an internal name which is usually the first line of the unit "**unit** MyUnit;", where MyUnit is the internal name. 

The unit also has two main sections **interface** and **implementation** which are similar in C/C++ terminology to the header (.h, .hpp, .hh) file and the body file (.c, .cpp, .cc). 

All the code in the interface section can be accessed by other units or **program** files that "use" this unit: this is done by the **uses** clause. The [uses](<Glossary.md> "Glossary") clause can appear either in the interface or implementation section: the code in the implementation section is "private" and can only be accessed by the same unit unless it is redefined in the interface section which is the case for "global" procedures and functions.  
The Pascal unit can also contain two more sections, **initialization** and **finalization** : these are executed automatically when the main program starts and stops using this unit, as their names suggest. 

### [Uses](<Uses.md> "Uses")

Clause in an Object Pascal program: a line near the top of the file or [Unit](<Glossary.md> "Glossary") (in either the interface or the implementation) which lists other units that need to be accessed to find [components](<Glossary.md> "Glossary") used in the program. 

For Example: 
    
    
    uses
      Classes, SysUtils, MySpecialUnit;
    

## V

### VCL

Visual Component Library — Base of Delphi Components Classes. 

### V4L

Video For Linux 

### VFW

Video For Windows 

## W

### Widget

A small (or even large!) graphical object or image, often part of a larger image, form, screen or picture. [Object Oriented](<Glossary.md> "Glossary") programs often depend heavily on the use of Forms, which comprise a large number of widgets such as buttons, captions, edit boxes, scroll bars, titles, menu bars, panels, grids and pictorial images like photos or drawings. 

[Lazarus](<Glossary.md> "Glossary") has separate [widget sets](<Widgetset.md> "Widgetset") for each Window Manager system, such as Microsoft Windows (widget sets [Win32/64](<Win32/64_Interface.md> "Win32/64 Interface"), or [WinCE](<Windows_CE_Interface.md> "Windows CE Interface") for hand-held devices), Linux ([GTK widgetsets](</Category:GTK_Widgetsets> "Category:GTK Widgetsets") for Gnome-based systems, [Qt](<Qt_Interface.md> "Qt Interface") for KDE systems), Apple (widget sets [Carbon](<Carbon_Interface.md> "Carbon Interface") and [Cocoa](<Cocoa_Interface.md> "Cocoa Interface")) as well as some non-specific sets such as [CustomDrawn](<Custom_Drawn_Interface.md> "Custom Drawn Interface"), [fpGUI](<fpGUI.md> "fpGUI") or [nvidia widgets](<nvidia-widgets.md> "nvidia-widgets"). 

Lazarus developers have had to design custom-built interfaces to each widget set: the aim is to provide an interface of which the user is unaware; he or she simply writes the code or designs the form, and the widgets of the relevant set for his operating system fit into place and work seamlessly. 

### Wiki

A type of Web site that provides documentation for a software package or similar information, and that welcomes editorial activity, additions and comments from the community. Most Wiki sites require potential users to register, and then login in order to edit the site. Documentation can be developed very quickly and flexibly. 

The [Lazarus and Free Pascal Wiki](<Main_Page.md> "Main Page") site that you are currently browsing contains extensive documentation about the [Free Pascal](<Glossary.md> "Glossary") and [Lazarus](<Glossary.md> "Glossary") projects. Users and developers are actively encouraged to register and submit their contributions. 

### WinCE

A Windows-like environment for portable hand-held devices, mobile phones, PDAs, etc. Produced by Microsoft, it has many features that resemble [ Windows](<Glossary.md> "Glossary") for desktops, but represents a "cut-down" version. 

Most hand-helds use the ARM processor, and a [WinCE port](<WinCE_port.md> "WinCE port") of [ Free Pascal](<Glossary.md> "Glossary") and [ Lazarus](<Glossary.md> "Glossary") has been constructed to allow applications for the ARM and WinCE to be [ cross-compiled](<Glossary.md> "Glossary") and loaded for running on the hand-held device. 

For more details see [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") and [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface")

### Windows

An [Operating System](<Glossary.md> "Glossary") from Microsoft, probably the most widely used Operating System today, famous for its user friendly interface but infamous for its vulnerability to malicious attackers. 

"Windows" also refers to rectangular areas on a computer screen which contain the textual and graphical material associated with a particular process or application. A window may form part of a program or system other than Microsoft's offering, for example there can be X-windows (see [X11](<Glossary.md> "Glossary")) or simple text-windows used by inherently text-based systems. 

## X

### [X11](<http://www.x.org/wiki/>)

Also called XFree, Xorg or simply X is a graphical interface originally designed for [Unix](</index.php?title=Glossary_Unix&action=edit&redlink=1> "Glossary Unix \(page does not exist\)") and also widely used in [Linux](<Glossary.md> "Glossary"). 

The X11 concept is quite different from the Windows GDI mainly because XFree is a program, not just a set of libraries. Because X11 is more hardware oriented and doesn't have a full set of [widgets](<Glossary.md> "Glossary") yet, some programmers developed more advanced widgets like [GTK](<Glossary.md> "Glossary") and [QT](<Glossary.md> "Glossary") and various Window Managers and Desktop Environments like GNOME, KDE, XPde, XFCE, IceWM, WindowMaker and many others. 

For more information about X11 please visit the [XFree Web site](<http://www.x.org>). 

## Y

## Z

### Zaurus

[PDA](<Glossary.md> "Glossary") made by Sharp that uses Linux as its operating system. Its processor is the ARM for which a port of the Free Pascal compiler has been made, and it is possible to cross-compile applications in Free Pascal that will run on the Zaurus

---

_Source: [https://wiki.freepascal.org/Glossary](https://web.archive.org/web/20250330000624/https://wiki.freepascal.org/Glossary)_
