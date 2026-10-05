# ZSeries/Part 1

## Contents

  * 1 Introduction
  * 2 Background on this project
  * 3 Before Getting Started
  * 4 Issues
    * 4.1 Target Choice
  * 5 To Begin



# Introduction

    _[Go back to zSeries](<../ZSeries.md> "ZSeries")_ — [Onward to Part 2](<Part_2.md> "ZSeries/Part 2")

My name is Paul Robinson, I am the chief programmer at Viridian Development Corporation, which has decided to develop a cross-compiler version of Free Pascal for the IBM 370/390/zSeries mainframe computer. I decided it would be a learning experience, it would allow me to better understand how the Free Pascal compiler works, and because I didn't particularly want to work with a compiler written in C (such as the GCC Pascal Compiler), I wanted to work with one written in Pascal. 

There is an existing more-or-less open source compiler for the 370 architecture which I have a copy of the sources and run-time library, it was modified from an earlier incrementally updated Pascal Compiler called P6 or P7 (depending on which release it was), I think it was P6 when it was on the Decsystem/20 mainframe back in the late 1970s and early 1980s (I have the source to that one too), and may have been P7 by the time it was upgraded for the latest release for Control Data Corporation computers. (When Nicklaus Wirth was creating the Pascal language at ETH in Zurich, that's what they had, so Pascal originally started with Control Data computers. In fact, because of the functionality provided by the existing Control Data libraries, Wirth wrote the first Pascal compiler using Fortran. I am not kidding.) 

This version of P6/P7 Pascal Compiler (the internal comments refer to it as "Stepwise refinement of a Pascal Compiler") was developed by the Australian Atomic Energy Commission. Only problem with it is it's over 25 years old and only supports Standard Pascal. No objects, no strings (you can do "array [1..256] of character" but concatenating two strings? Write your own function!), and doesn't even come up to the level of capability of Turbo Pascal 3 for DOS. It's old, and I wanted to work with something more recent. I can, however, borrow from it to figure out how some Pascal keywords are translated into 370 code. 

I also should have a copy of the [Stanford Pascal Compiler](<../Stanford_Pascal_Compiler.md> "Stanford Pascal Compiler") sources (also over 20 years old), and I believe that the CBT tapes archive (a huge set of over 200 mainframe source code magnetic tapes originally collected by Connecticut Bank and Trust) may contain either a Pascal Compiler or some other compiler source I can use. It also contains lots of IBM 360/370 assembly language sources which will be useful. All these resources and others should help in doing this implementation. 

Also, because I have regularly used the Free Pascal Compiler, I wanted to see it available on another architecture where the full object pascal capability would be available. When I started this project back in January of 2012, I didn't realize how long it would take, because it was the sort of thing that was what is called a "slopsucker" task. On a computer, the "slopsucker" is the task that gets whatever processor time is available after everything else has gotten a chance. Sort of like the amount of attention you give to the sump pump in the basement of your house (if you have one), unless, of course, it fails. So, I'm busy with other things, doing a little here, a little there, and what do you know, 9 months go by and I haven't accomplished a thing. So I decided to "up the priority" of this task and give it more attention. That's when I realized how much of my initial presumptions I made when writing this were wrong. More on that, later. 

# Background on this project

I am aware, first, that this will be a huge undertaking; this is not a weekend project. I'm probably looking at a minimum ~~six months~~ two years work, possibly longer. I have a story. When I wasn't doing programming I did mobile notary. I was, then, a commissioned Notary Public for the Commonwealth of Virginia (I'm now commissioned in Virginia and Maryland), and one of the things that was developed was, when people refinanced their mortgages, the company would send a notary right to the person's home or office. Well, one of the customers happened to discuss how they wanted to upgrade the application that their people ran to handle booking of services. It was a DOS application, they wanted to do more things with it and they wanted it to be a GUI under windows. At that time Free Pascal did not have the equivalent of Lazarus, so I unfortunately had to use Visual Basic to do so. (Please do not send me hate mail, I have to use what I have available.) 

Well, anyway, so I had several meetings with the customer and arranged terms including price. I worked on it on a regular basis, implemented it through "accretion" in which you get part of it working and they can see how it's coming along. We had to change things along the way and various fits and starts, but in the end, they were very pleased with the program that they can use for their employees who book orders on their laptop computers. It wasn't a very big program in terms of what was going on, they have it running on about 60 computers, but from when we first sat down to decide what they wanted, until it was fully running and distributed out to everyone took one solid year. 

So the point of this shaggy dog story is that I'm aware that this is a long-term project and will probably take months. I am also aware that the compiler won't work when I first make changes because I have to learn where the main part of the compiler hands off control to the machine-specific and OS-specific routines so that it can ask that routine, "Hey, the user is implementing a FOR statement, you need to create the code to do this." or "Hey, the guy is declaring the start of a procedure that has an integer argument, here's the information on how it's supposed to be defined." 

So anyway, I know there's a lot involved, there will be fits, and starts, and things won't always go right. But it's a learning experience, and, if you follow this as it goes along over the months as it progresses, maybe you'll learn something too. 

# Before Getting Started

The normal distribution includes most of the sources including the run-time library but **does not** include the sources to the compiler itself because most people do not need it and it's about another 40 meg. You need to obtain the zip/tar archive file for the compiler from the download location you're using for the rest of Free Pascal (probably Sourceforge or a mirror) and extract from that archive the compiler directory, and include it with the 2.6.0 source release. 

There are two ways to go about this. First thing is to start by deciding how you're going to carry the source files on your system. If you're just trying to do a simple change in the compiler to fix something, you can begin by creating a new directory and copying all files and subdirectories from the Compiler subdirectory (and all files in its subdirectories) to a new directory, in order not to contaminate the pristine sources of the current compiler. 

If you're planning to do serious development such as a full-blown port, you're definitely wanting to use full source code control including version management. For that, you need to get an SVN client and use SVN to download everything. (If you're using Mercurial, CVS or Git, quit complaining! Everybody has their own particular style, or toys, or whatever, and you have to go along with that particular crowd to work with them. I mean, there are people who argue over which word processor is best and will almost "go to the mattresses" to defend their decision. (See the movie _Sleepless in Seattle_ about how the movie _The Godfather_ is the answer to everything!) 

Or let's consider formatting style: Which of the following is the correct way to format a block of code: 
    
    
          (1)                              (2)
        BEGIN                           BEGIN
           Instruction1;                        Instruction1;
           IF something THEN                    IF something THEN
              BEGIN                             BEGIN 
                  Instruction2;                         Instruction2;
                  Instruction3;                         Instruction3;
              END                               END
           ELSE                                 ELSE
              BEGIN                             BEGIN
                  Instruction3;                         Instruction3;
                  Instruction2;                         Instruction2;
              END;                              END;
        END;                            END;
    

Or are neither of these right? Well, guess what, whichever one you picked, you're right. You're also wrong. Someone won't like the way someone else does indentations, the number of spaces for each level, whether the BEGIN should be on the same line as the IF or ELSE, or whether the THEN should be moved down and the begin put there, as well as whether they capitalize keywords or use Camel Case, or whatever. It's all a matter of taste, and mostly it's arbitrary. You pick what style works for you to be able to read the code easily, and use that. 

So, anyway, if you don't have SVN, get a Linux client if you're on Linux or [Install Tortoise SVN client for Windows](<http://tortoisesvn.net>), then download the Free Pascal Sources using SVN. If you're going to do an approved project, then you'd get write access to the repository, otherwise all you need is read access. I won't go into how to do this because there's already plenty of information here on how to use SVN to do this, I'll go on to the port I'm working on. 

This compiler will _initially_ be a cross-compiler, it will run on a PC and will generate an assembly language file for the 370 Architecture with the use of the standard High-Level Assembler syntax. That assembly-language source file will be uploaded to the target mainframe (real or simulated) and run through the high-level assembler there. Eventually, as time goes by, it will be ported over and will run natively there. (At least I _hope_ that's how it will work out...) 

Then I discover that if you're porting for Linux, you do not have the High-Level Assembler, you have a bastardized version of Intel and AT&T syntax because the assembler isn't the powerful High-Level Macro Assembler for mainframes, it's the GCC assembler for the GNU C Compiler that uses a form of the syntax for the 386 series microcomputer. So that has to be taken into consideration. 

So presuming you don't use SVN and you need to manage the directories manually, you would also want to copy the **rtl** directory (which is normally outside of the Compiler directory) because you may need to modify some files there. That will also require an i370 subdirectory for its run-time library, which might be different depending on which mainframe OS is targeted. We'll worry about that later. 

There will also be created a new ~~i370~~ **s370** subdirectory within the Compiler directory for all the local files related to that architecture. That's another thing I learned, straight out of the Linux development for the z/System (the extended version of the 370 and 390 series, is that it's called the S370, not the I370. So that gets changed, too. 

Note that the pages here will just basically walk through what was done, if a correction is more than a few lines, the user will be directed to the replacement source file. Once the work is completed a zip file containing all of the new or changed source files will be available. 

# Issues

There are a number of issues when doing this. The S370/390/zSystem has a number of quirks different from the Wintel architecture or Mac hardware 

  * It's big-endian (The number 1 stored as an integer 32-bit word internally as 00000001 while the I386 would store it as 00010000.)
  * It uses non-IEEE floating point so it may have different limit values (the zSystem has IEEE floating point available)
  * While it has more registers (15), about 5 of these are generally not usable due to conventions or hardware requirements (it's worse on Linux for S/390, instead of using one register to point to a list, it uses registers 0 through 6 to hold up to 6 integer arguments.)
  * The maximum amount of memory you can directly address at one time is much smaller (in ESA mode on the 370, you can only address about 4K at a time, either as code or data, the contents of one register with an unsigned offset of 2^12, or 4096). If you're working with two pieces of data, either may be up to 4K in size that you can work with directly. This has been expanded with the new 20-bit signed offset on the S/390 and zSystem, which means you can work with 512K in an area.
  * Depending on whether you target a 370, a 390 or a zSystem you may have access to a 32-bit address space or a 64-bit address space and a much larger area than 4K.
  * There are several different operating systems that could be targeted, such as 
    * The MUSIC/SP emulator I'm using (not going to be very popular as MUSIC was essentially deprecated by its distributor, McGill University in Montreal)
    * a program running on a terminal under VM/370
    * a program running on the TSO timesharing system
    * a program running as a batch job on OS/VS1
    * a program potentially running as a screen application on the CICS terminal monitor (very similar to how Windows programs work, with a few gotchas)
    * a program running on Linux/370, or
    * The z/390 portable mainframe emulator (<http://www.z390.org>), written in Java by Don Higgins, runs on Windows and released as open source by Automated Software Tools Corporation. Allows running of Assembler and Cobol programs, and supports a subset of the VS/1 supervisor call set, which is entirely different from the Linux Supervisor call set and the argument passing rules are different under the old mainframe standard calling conventions. For example, to use dynamic memory in pascal you use the **new** procedure to allocate memory, and **dispose** to release it. At the system level, Linux uses the C-programming language names **malloc()** to allocate memory, and **free()** to release it. In VS/1, you use the operating system macro **GETMAIN** to allocate memory, and **FREEMAIN** to release it. Also, arguments on Linux are passed in registers 1 through 6; arguments on VS/1 are passed as pointers from a list pointed to by register 1.



This issue of where the program will run will be dealt with by using generic I/O instructions (basically private macros) and having an appropriate run-time library for the particular system. 

  * The IBM 370/390/zSystem uses the EBCDIC character set, PCs use ASCII (or Unicode). The zSystem allows use of ASCII, and runs code in ASCII natively, so that's no longer a problem. But you may get some gotchas when transferring files; Windows can release source code using UTF-8, which allows for things like non-roman character sets (for doing other written languages like Japansese, Arabic, Greet or Russian natively in code) but it means you're not using plain 7-bit ASCII, you might even be using double-byte characters, which can be a surprise. I found this out when transferring a C source file so I could test a small program on one of a Linux/z390 machine IBM makes available at no charge for people testing development of applications and porting to the z/System, that text files can have extra characters you don't see because Windows handles Unicode internally, and usually seamlessly, but when those files move to a non-windows environment some of the extra characters show up.



## Target Choice

IBM offers access to an actual z/System running Linux for up to 90 days without charge for porting applications to Linux, so I will take advantage of an actual mainframe to target. I will also use an S/390 emulator program written in Java on my Windows PC that allows programs to be run in an emulated OS/VS1 or VSE environment. That is, however, an EBCDIC environment so I have to be careful. I also got it partially wrong when I was making corrections in that I confused the S/370, S/390 or z/System processor, the CPU, with the operating system, which could be VS/1, the z390 emulator running a version of VS/1, or Linux. 

# To Begin

This compiler is _huge_. It's hundreds of source files, and is going to be an enormous task. (I compiled it once on my computer, the entire compiler, targeting itself, I386 on Windows. It is over 250,000 lines of code and about 208 units. But my computer is fairly fast, the entire compilation took less than 15 seconds. 

Okay, so, given the size of the compiler, where do you start? Well, you start with the main program of the command-line compiler, and you look at it. That file is **pp.pas**. Recursively follow the sources of every unit referenced by it or any unit they reference until every one has been done (I more-or-less explain this in [Part 2](<Part_2.md> "ZSeries/Part 2")) and then you know that you caught all the places you might need to declare, add or change something. (It also gives you at least a fleeting understanding of what each unit does.) 

I originally did this and realized I was looking at too much code. PP, which has conditional compilation directives for various machines, calls the procedure compile, which is located in module compiler. And that module uses conditional compilation to target certain processors or operating systems. And then we can follow that and see where it leads. 

Note that line numbers indicated in any source file are from the version 2.6.0 compiler sources (having just been released, I'll also look at 2.6.2 if I can) and as such, as lines are added, other line numbers where things were found and changed will increase. So line numbers will be referenced in a file from top to bottom so the references should match. Also, so as not to brand this as "Windows centric" since the hope is to build a cross-compiler for I370 that could run on either Windows or Linux, when file names are specified, directory separators will use /. 

Note that from this point on, all editing occurs in our "sandbox" directory separate from the original compiler. So let's get started, with [Part 2](<Part_2.md> "ZSeries/Part 2") of this article. 

    _[Go back to zSeries](<../ZSeries.md> "ZSeries")_ — [Onward to Part 2](<Part_2.md> "ZSeries/Part 2")

---

_Source: [https://wiki.freepascal.org/ZSeries/Part_1](https://web.archive.org/web/20250207041813/https://wiki.freepascal.org/ZSeries/Part_1)_
