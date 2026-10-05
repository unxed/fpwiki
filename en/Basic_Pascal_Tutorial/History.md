# Basic Pascal Tutorial/History

│ **English (en)** │  **[русский (ru)](<../../ru/Basic_Pascal_Tutorial/History.md>)** │

[ ◄ ](<Introduction.md> "Basic Pascal Tutorial/Introduction") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Compilers.md> "Basic Pascal Tutorial/Compilers")  
---|---|---  
  
History (original author: Tao Yue, state: changed) 

## Contents

  * 1 Origins
  * 2 Wirth Invents Pascal
  * 3 UCSD Pascal
  * 4 Pascal Becomes Standard
  * 5 Extensions
  * 6 The World Changes
  * 7 So what are the advantages of learning Pascal?



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

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/History](https://web.archive.org/web/20250314123957/https://wiki.freepascal.org/Basic_Pascal_Tutorial/History)_
