# Basic Pascal Tutorial/Compilers

│ **English (en)** │  **[русский (ru)](<../ru/Compilers.md>)** │

[ ◄ ](<Basic_Pascal_Tutorial/History.md> "Basic Pascal Tutorial/History") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Hello,_World.md> "Basic Pascal Tutorial/Hello, World")  
---|---|---  
  
Pascal Compilers (author: Tao Yue, state: _changed_) 

This document will explain the basics about compilers as well as provide links to well-known Pascal compilers and explain how to set up Free Pascal. 

## Contents

  * 1 About Computer Languages and Compilers
  * 2 More About Compilers
  * 3 Open Source Compilers
  * 4 See also



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
  4. Type in a program (flip to the [next lesson](<Basic_Pascal_Tutorial/Hello,_World.md> "Basic Pascal Tutorial/Hello, World") to get a "Hello, world." program).
  5. Save the file with File-Save As ...
  6. Run the program from the Run menu. This will automatically compile the program if you've made any changes, then run the program. It will also run the program without compiling if you've not made any changes since the last time you compiled.



With programs that don't expect user input, you'll see the program flash on a black screen. But the program completes in the blink of an eye and you are returned to the IDE without seeing the results of your work. There are two ways around this: 

  * Select User screen from the Debug menu to see the results of the program.
  * Add a readln statement at the end of every program. This will make the program wait for the user to press the Enter key before the program ends and returns to the IDE.



[![Userscreen.png](https://wiki.freepascal.org/images/1/19/Userscreen.png)](</File:Userscreen.png>)

Note that an .exe file was created in the directory where you saved your program. This is the executable. You can go to the Command Prompt, change to the directory, and run this executable straight. You can also double-click on it in Windows Explorer (and it will still flash by quickly if it ends without requiring user input). 

### See also

  * [Control Pascal Tutorial](<Control_Pascal_Tutorial.md> "Control Pascal Tutorial") \- a simple way to start with no installation needed
  * [How to start](<How_to_start.md> "How to start") \- another way to start with an installation

[ ◄ ](<Basic_Pascal_Tutorial/History.md> "Basic Pascal Tutorial/History") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Hello,_World.md> "Basic Pascal Tutorial/Hello, World")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Compilers](https://web.archive.org/web/20241202163640/https://wiki.freepascal.org/Compilers)_
