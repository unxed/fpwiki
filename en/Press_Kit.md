# Press Kit

_This page is designed to provide background information for people who want to publish Free Pascal and Lazarus related news. It can be used freely for any use; no copyrights will be claimed._

## Contents

  * 1 What is Free Pascal
    * 1.1 What is Lazarus
    * 1.2 Platforms and architectures
    * 1.3 Some numbers
    * 1.4 Possible usages
    * 1.5 Compatibility
  * 2 The Pascal language
    * 2.1 Overview
    * 2.2 Pascal and the programming language universe of today
      * 2.2.1 Scripting languages versus compiled languages
      * 2.2.2 Managed languages versus unmanaged languages
      * 2.2.3 C
  * 3 Architecture
    * 3.1 Scanner and parser
    * 3.2 Tree node system
    * 3.3 Assembler optimizer
  * 4 Optimization power
  * 5 Internal assembler and linker
  * 6 Why Free Pascal is important
    * 6.1 Lazarus
  * 7 What about other Pascal compilers
    * 7.1 Delphi
    * 7.2 GNU-Pascal
    * 7.3 Rem Objects Oxygene (formerly Prism and Chrome)
    * 7.4 Virtual Pascal
  * 8 See also



## What is Free Pascal

Free Pascal is an open source Pascal compiler. Work started in 1993 by Florian Klämpfl. Since 1993 Free Pascal has evolved into an advanced piece of technology. Nowadays it is widely considered one of the best Pascal compilers ever written. 

Free Pascal is not just a compiler. The Free Pascal development team writes and maintains: 

  * The compiler
  * A runtime library for many operating systems
  * Many software libraries
  * Interfaces to many software libraries written in C
  * An integrated development environment
  * A set of manuals
  * A sister project (pas2js) provides a javascript transpiler.



### What is Lazarus

Lazarus is a sister project of Free Pascal. Its aim is to provide a fully graphical integrated development environment (IDE) for rapid application development. It excels in creating modern GUI applications with native look and feel across multiple operating systems. 

The Lazarus team designs and maintains: 

  * The Lazarus development environment
  * The Lazarus component library of GUI widgets (called the LCL)



### Platforms and architectures

Free Pascal is available for the following platforms: 

  * [AIX](<FPC_AIX_Port.md> "FPC AIX Port") (PowerPC, PowerPC64 )
  * [Android](<Android.md> "Android") (ARM, ARM64, MIPS, x86, x86-64)
  * [32-bit DOS](<DOS.md> "DOS") (GO32V2 dos extender)
  * [Windows Win32](<Portal_Windows.md> "Portal:Windows")
  * [Windows Win64](<Portal_Windows.md> "Portal:Windows")
  * [Windows CE](<Portal_WinCE.md> "Portal:WinCE")
  * [FreeBSD i386](<Portal_FreeBSD.md> "Portal:FreeBSD")
  * [FreeBSD x86_64](<Portal_FreeBSD.md> "Portal:FreeBSD")
  * Game Boy Advance
  * [Haiku](<Installing_Lazarus_on_Haiku.md> "Installing Lazarus on Haiku")
  * [Linux/ARM](<Portal_Linux.md> "Portal:Linux")
  * [Linux/i386](<Portal_Linux.md> "Portal:Linux")
  * [Linux/powerpc](<Portal_Linux.md> "Portal:Linux")
  * [Linux/powerpc64](<Portal_Linux.md> "Portal:Linux")
  * [Linux/sparc](<Portal_Linux.md> "Portal:Linux")
  * [Linux/x86_64](<Portal_Linux.md> "Portal:Linux")
  * [MacOS Classic](<Portal_Mac.md> "Portal:Mac")
  * [macOS 32 bit (Intel)](<Portal_Mac.md> "Portal:Mac")
  * [macOS 64 bit (Intel)](<Portal_Mac.md> "Portal:Mac")
  * [macOS 64 bit (Apple Silicon)](<Portal_Mac.md> "Portal:Mac")
  * [MorphOS](<MorphOS.md> "MorphOS")
  * [Nintendo DS](<Nintendo_DS.md> "Nintendo DS")
  * [Novell Netware](<Netware.md> "Netware")
  * [OS/2 / eComStation](<Target_OS2.md> "Target OS2")



Free Pascal has also supported the following platforms in the past: 

  * [AmigaOS/m68000](<AmigaOS.md> "AmigaOS")
  * [macOS 32 bit (PowerPC)](<Portal_Mac.md> "Portal:Mac")
  * NetBSD/i386
  * NetBSD/m68000
  * [Solaris](<Solaris_Port.md> "Solaris Port")
  * [BeOS](<BeOS_port.md> "BeOS port")
  * QNX



Additionally people are constantly doing experiments with new platforms. 

Free Pascal programs can usually be recompiled for different platforms without significant code changes. Portability is heavily dependent on the code used for the architecture; however, the Free Pascal runtime library bridges the gap between Unix (Posix) derived platforms and DOS derived platforms quite well. 

Lazarus is available for FreeBSD, Linux, macOS and Microsoft Windows and Windows CE. The aim is to make programs useable on all platforms without any significant code changes. 

### Some numbers

Compiler source code sizes: 

  * Compiler: 11,3 MB
  * Runtime Library: 16,5 MB
  * Integrated Development Environment: 2,0 MB
  * Free Component Library: 6,0 MB
  * Test suite: 12,8 MB
  * Packages (Pascal libraries and interfaces to C libraries): 34,0 MB
  * Documentation: 13,3 MB



Amount of users: estimated between 20000 and 30000 

Download traffic: 

  * ftp.freepascal.org: 400-500GB/month, +/- 50000 files/month
  * Sourceforge: +/- 300GB/month, +/- 20000 files/month
  * Mirrors unknown



### Possible usages

Free Pascal is flexible, it is useable for a wide range of projects. Examples include: 

  * Development of GUI applications
  * Development of operating systems
  * Development of computer games
  * Development of command line tools
  * Development of database applications



Free Pascal also continues the tradition of being a good tool for newcomers to programming. 

### Compatibility

Since the original Pascal from 1970, many compilers have extended the Pascal language. This evolved into two major groups of compilers: Those that implement the ISO Pascal dialect and those that implement the USCD-Pascal dialect. Free Pascal is a compiler from the UCSD dialect group. It has native modes which are mostly compatible with Turbo Pascal and Delphi, and special compatibility modes that are intended to be 100% compatible with Turbo Pascal/Delphi or Mac Pascal (the Pascal dialects used on Apple platforms). Free Pascal does not implement ISO Extended Pascal, but its ISO 7185 Standard Pascal is improving swiftly. 

## The Pascal language

The Pascal language has a rich history. It was designed in 1970 by Niklaus Wirth and introduced the concept of structural programming. This concept has revolutionized programming, and the ideas behind it in turn inspired many other programming languages. 

Pascal is a language that has evolved a lot over the years. One of the revolutions inspired by Pascal was byte code, first pioneered in the USCD P-system, and today the basis of Java and .NET. 

Turbo Pascal introduced another paradigm shift, the Integrated Development Environment. Programs could now be written quicker and more easily than ever. Turbo Pascal introduced many enhancement to the Pascal language, including object oriented programming and modular concepts backported from Pascal's successor Modula-2. 

While innovations in its successor, Delphi, were not entirely new to the world, Delphi is regarded as one of the best development environments in existence. Delphi enhanced the language too and prepared it for the future decades. 

While Pascal has evolved a lot, it has unmistakenly remained itself and remained close to the original spirit. 

The Free Pascal project is dedicated to promote the use of the Pascal language. We are also dedicated to free software. The Free Pascal development team provides the tools that make Pascal as realistic an option as other mainstream languages and believes the use of the Pascal language leads to better software. 

### Overview

Pascal is a language designed to be easily readable and has succeeded remarkably well. Most programmers unfamiliar with it can read Pascal code. 

The language uses strong typing, which allows increased compile time possibilities for verification of code. 

Low level types are available, such as byte, char, word, integer, as well as structured types like arrays and records. Dynamic structures can be implemented using pointers. 

High level types that are available are strings, sets, objects and classes. Unique object oriented programming features are properties, dynamic constructors and interfaces. 

Code is modularized into units, which offer a clear separation between interface and implementation. Units need to be loaded when used, it is not necessary to parse their interface. Dependencies between units can be figured out by the compiler, eliminating the need for the "make" tool. 

Other features are easy integration with assembler code, exception handling, thread variables and nested procedures. 

Pascal programs have direct access to external libraries. No wrappers around external libraries are necessary, an import unit is used to couple the assembler symbols in the external library with Pascal identifiers. Types and constants for use with the library are defined in such units as well. 

### Pascal and the programming language universe of today

The programming language spectrum has changed over the years. There are many languages the have come and gone, while others have remained remarkably durable. 

There are several reasons why Pascal is still around 50 years after its birth. First, Pascal has adapted very well to new developments. Over the years the language has been extended a lot, and in many cases extended well; Pascal does not feel a crippled language because of its age. These extensions were often done so well that Pascal has beaten "successors" to itself, for example Modula-2. The improvements that these languages provided could be implemented in Pascal as well, which had better tools and provided backward compatibility. 

Compatibility is certainly an important reason why Pascal is still around today. A language vendor that destroys backward compatibility effectively kills all previous investments in software written in that language. This makes people wonder if they should invest either in the latest hype or a more stable language. 

#### Scripting languages versus compiled languages

Scripting languages receive a great amount of attention today. Scripting languages are often special purpose languages specially designed for certain tasks and provide unmatched productivity for those tasks. Efficiency is often as low as memory consumption is high. 

Scripting languages have augmented the tools programmers have at their disposal. It is to be expected that scripting languages will remain very popular, while not undermining compiled languages. 

Compiled languages will not undermine scripting languages either. However, Pascal is a traditional compiled language, but is not as bare to the metal as C; it is possible to program at a higher abstraction level. Because of this, it can be useful in situations where scripting languages are tradionally used, but do not perform efficiently enough. A good example is web programming, for which Pascal-based solutions have appeared. 

#### Managed languages versus unmanaged languages

Pascal is a tradional compiled language. A lot of hype has centered around managed languages, especially Java and .NET. This is partially caused by breakthroughs in the efficiency of those languages, and partially because .NET is supposed to be the next big thing in Windows programming. While it is possible to change Pascal to become a managed language, Free Pascal has chosen to remain a traditional compiled language. 

Managed languages have their uses. By means of byte code, programs written with them can be easily run on multiple processor architectures. Automatic memory management should prevent people from ever having to think about memory references again. 

The Free Pascal development team is sceptical regarding this hype. We are certain that there will be a role for those languages, but they will not threaten the existance of traditional languages. It should be noted that although Pascal pioneered byte code, it did not turn out to be a success. 

The limitations of managed languages lie in both portability and efficiency. Byte code languages are supposed to be very portable, but they depend on a runtime environment to provide functionality. This runtime environment provides the least common denominator of functionality and often provides it by means of emulation. This puts constraints on available features, and thus limits progress, while emulation provides non-native behaviour. 

Efficiency limitations can be found in huge, bloated runtime environments along with memory management issues. While automatic memory management removes a burden for the programmer, users are often not pleased to see simple programs consuming hundreds of megabytes of memory and they detest sluggish software. 

It should be pointed out that Free Pascal already offers many of the features promised by managed languages, such as increased security, full portability between FreeBSD, Linux, macOS and Windows, and easier, safer, programming. 

Native applications can provide very tiny tools with very low memory usage. Taking advantage of the platform is easier because the functionality is already provided. 

It has during the last years become clear that native languages will remain the most dominant in programming. Unlike originally promised, the big .NET Windows release, Windows Vista, hardly used any .NET code. The Win64 API will be the next big thing in Windows programming which Free Pascal supports. 

The reason why Free Pascal will not support .NET at all is that it would require invasive changes without backwards compatibility, and a complete new set of libraries. No current code in such an environment exists, and it is highly doubtful if people will be educated to write code for such an environment. 

#### C

Which leaves us with C as Pascal's biggest competitor. 

The C language has been Pascal's biggest competitor ever since the beginning. Both languages were developed at the same time, offer a very similar programming model, but differ greatly in philosophy. 

In the 90's C became more dominant than it ever was. The reason was a lack of good Pascal tools; Turbo Pascal, a 16 bit compiler had been discontinued and Delphi was not a universal replacement tool. Today, in the Windows world, Pascal still has a healthy market share. In the 90's Pascal vanished from game development (because there was no 32 bit DOS compiler), and later, on Linux, it couldn't compete with C due to lack of good tools. Free Pascal, then heavily in construction, wasn't able to compete seriously either. 

Pascal is nowadays more competitive against C than ever. It provides a reasonably safe language at comparable performance, better productivity and is nowadays backed by an advanced open source compiler tool. 

## Architecture

Free Pascal has been written in Pascal itself. It can recompile itself and does so to port itself to new platforms. The lack of dependency on external tools ensures high portability, one only needs to port Free Pascal itself to port it and not rewrite handling of external interfaces or tools. 

It also ensures reliability of the compiler, any bug that will hurt stability will be noticed immedeately by compiler developers. One can write millions of lines of code without fear of being plagued by compiler failures or limitations. 

### Scanner and parser

Free Pascal uses a handcrafted scanner with built-in preprocessor that converts Pascal source code into tokens. While implemented with OOP, it is mostly procedural. The parser is also hand crafted. The parser receives the stream of tokens from the scanner and creates tree nodes, while interacting with the symbol table. The parser implementation is fully procedural. 

### Tree node system

The heart of Free Pascal is an innovative object oriented code generation design. Every feature of the Pascal language, i.e. add, array subscript, pointer dereference, gets converted into a tree node. This is done like in a traditional compiler. 

These tree nodes are, however, classes that can be overridden. The front-end generator overrides all of these nodes and implements code generation procedures by making calls to primitives in the back end code generator while working with the processor independent register allocator. 

It is possible to implement a code generator by implementing the back end code generator. This makes Free Pascal highly portable. One can, however, also override parts of the front-end code generator, to take advantage of information at a higher level to generate optimized code for a certain processor. This functionality is highly useful to take advantage of processor-specific features like i386 addressing modes, MMX, or shifterop on the ARM processor. 

### Assembler optimizer

Generated assembler code is processed by the assembler optimizer. This one performs peephole optimizations, CPU model tuning, common subexpression elimination and data flow analysis at the assembler level. 

## Optimization power

Free Pascal generates very competitive code, and can win in a few cases against the GCC compiler engine. In general we can say though, that GCC still has better optimization. This is for i386 only. For some platforms, Free Pascal generates faster code than GCC. Over the years, Free Pascal has caught up, and continues to do so. 

## Internal assembler and linker

FPC can use the GNU assembler and linker, allowing the compiler to be easily ported to many platforms. It also has a built-in assembler and linker. Neither the GNU-assembler nor linker have been designed for the load FPC puts on them. This means that in some situations, like smartlinking, the compile speeds aren't optimal or memory usage is excessive. The built-in assembler and linker have been designed to address this, they allow faster compile times and better size optimizations. Furthermore, they allow ports to platforms not supported by the GNU assembler and linker. For example, the Win64 port was started long before the GNU binutils supported it. Free Pascal was the first open source compiler to support the Win64 platform. 

## Why Free Pascal is important

Free Pascal is, to our knowledge, the most advanced open source compiler engine besides the GCC compiler. It can generate speed-competitive code, but also implements high level functionality, and extreme compatibility with language dialects. 

The compiler's innovative code generator design makes the compiler itself highly portable. The design of our libraries also promotes portable software. This allows the availability of software for many platforms with little effort. Due to low portability effort, small operating systems can quickly have the entire compiler toolset available. 

Free Pascal is also very developer friendly. It compiles fast, many times faster than GCC, and provides clear, helpful error messages. It should also be noted that many programmers will greatly appreciate the extensive documentation. There are also extensive support options for people who are stuck. 

The real reason why Free Pascal is important though, is that it solves some of the issues with which the free software community is currently coping. There is demand for languages and development environments that make people more productive than in C. Many languages promise this, but at a cost for the user of those programs: performance. 

Pascal programs are fast. They start very quickly (yes, even faster than C programs). They react quickly to user input, and provide good throughput for actual work. Try it: [Pixel32](<http://www.kanzelsberger.com/pixel/?page_id=12>) as good examples, and feel the responsiveness. 

### Lazarus

Lazarus is the most serious free software attempt at providing a fully graphical, rapid application development, integrated development environment (IDE). There are many open source tools and libraries to build GUIs on open source (and proprietary) operating systems, but few do a combination of a full graphical design, full customizability of the actual GUI, full control to the programmer, and all the tools you would expect in an IDE. Add to that the powerful language on which it is based and the result is that quality applications can, and are, efficiently produced. As a bonus, programs written in Lazarus run on FreeBSD, Linux, macOS and Windows with very little effort. 

The result is both very good productivity for the programmer and quality software for the user. 

Furthermore, Lazarus is the only toolkit that can generate native look and feel for all platforms. Competing tools sometimes emulate native look and feel. Lazarus, in contrast, uses the actual native GTK1, GTK2, QT, Win32/WinCE, Carbon/Cocoa GUI elements, which guarantees the look and feel is close to as native as possible. This is done with a single source code. All that is generally needed to switch an application to a different graphical toolkit is just a recompile! 

The combination of a language and a compiler that is fully comparable to traditional C development, with the comfort of a safer, more powerful language, and development tools that are designed and optimized to build desktop applications rapidly can be a very good solution on which to base an open source desktop. 

## What about other Pascal compilers

### Delphi

Delphi is still a superior tool for writing Windows applications. Borland wants to make their flagship product the ideal tool to develop .NET applications. This is done without full backward compatibility. Whether this a good idea is highly doubted by many people in the Pascal world. 

Delphi will be influential for the Pascal language for the forseeable time, though. 

### GNU-Pascal

GNU-Pascal is an attempt to provide a Pascal compiler using the GCC compiler back end. While still active, this compiler has progressed a lot less than Free Pascal. The compiler tried to implement ISO Extended Pascal. While enhancing the original Pascal language considerably, the problem with ISO Extended Pascal is that no code exists in this dialect (and no programmers are being educated to write it), while it is mutually exclusive with the Turbo Pascal and Delphi dialects and their language extensions. This caused an unfortunate effect that no serious code bases existed that this compiler could compile, something which it still is recovering from today. 

GNU-Pascal is officially no longer maintained after 2010. 

### Rem Objects Oxygene (formerly Prism and Chrome)

This compiler does not implement Pascal, but a Pascal-like language for .NET. It is nice for .NET programmers, but it cannot really compile Pascal code, and also Delphi .NET code cannot be compiled with this compiler. 

### Virtual Pascal

Virtual Pascal was a great 32-bit compiler with high Turbo Pascal compatibility, used by many OS/2 programmers to write real world applications. Over time, support for other x86 platforms was added, especially Win32 and DOS (various 32-bit PM extenders supported) and partly Linux, plus the supported dialect was extended to provide compatibility with Delphi 2. Unfortunately the compiler was from the beginning written in Turbo Assembler, which made it impossible, or at least very hard, to port to other platforms, extend with new features and maintain in general. That resulted in official announcement of Virtual Pascal death by its maintainer and copyright owner in 2005. Full source code has never been available, so nobody can really continue officially (except for fixing some RTL issues, etc.). 

## See also

  * [In the News](<In_the_News.md> "In the News") \- News stories about Free Pascal + Lazarus
  * [Case Studies](<Case_Studies.md> "Case Studies") \- Examples where Lazarus and Free Pascal have been used for the successful development of professional applications for business, game development, science, health-care and other fields.
  * [Projects using Lazarus](<Projects_using_Lazarus.md> "Projects using Lazarus") \- Business Software, communications software, databases and tools, developer utilities, editors and IDEs, educational software, games, hobby software, medical and scientific software, multimedia, user utilities and web.
  * [FPC Advantages](<FPC_Advantages.md> "FPC Advantages")
  * [Why use Pascal](<Why_use_Pascal.md> "Why use Pascal")

---

_Source: [https://wiki.freepascal.org/Press_Kit](https://web.archive.org/web/20241104214411/https://wiki.freepascal.org/Press_Kit)_
