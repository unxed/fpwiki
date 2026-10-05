# ZSeries

## Contents

  * 1 Overview
  * 2 Architectural details
    * 2.1 CPU capabilities
      * 2.1.1 Episode 1
      * 2.1.2 Episode 2
      * 2.1.3 Episode 3
      * 2.1.4 Episode 4
      * 2.1.5 Episode 5
      * 2.1.6 Digression: example FPC output
    * 2.2 Target operating system
    * 2.3 Character set: ASCII vs EBCDIC
  * 3 Implementation status
  * 4 More Information
  * 5 Resources



## Overview

IBM uses the zSeries designation to indicate an implementation of the system architecture that includes the System/360 (1964), System/370 (1970), System/390 (1990) and others. 

The zSeries CPU uses a proprietary CISC architecture unrelated to other processors such as the PowerPC which is used in IBM's iSeries (AS/400) and pSeries (RS/6000) systems. Note that these designations are basically marketing terms and as such are somewhat fluid. 

The original S/360 architecture had 32-bit integer registers and a 24-bit address space. This has been extended first to support a 31-bit address space and later to support 64-bit registers and address space. In addition many models support paged/expanded memory. 

In the 1990s Linux was ported onto the S/390, almost invariably running as a guest/virtualised operating system in the context of a "traditional" host OS. In this mode, the underlying operating system handles all error conditions, effectively guaranteeing that Linux is running on a 100%-reliable system; Linux itself does not have the same level of error handling but is promoted since it provides a standardised API. 

In addition to Linux GCC was ported, the paper below discussing some of the problems that were encountered. 

[Porting GCC to the IBM S/390 Platform](<http://www.google.com/url?q=ftp://sourceware.org/pub/gcc/summit/2003/Porting%2520GCC%2520to%2520the%2520IBM%2520S390.pdf&sa=U&ei=ndP0TNGwGMayhAfQk6CBBg&ved=0CBgQFjAE&usg=AFQjCNGLudKLRblcV-OZrggPwvMKUZs68A>)

Notable points from this paper are that older versions of the S/390 and its predecessors had two significant limitations that were tolerable when the systems were programmed in assembler but caused significant problems for automatic code generation: 

  * Literals had to be in tables rather than inline. Tables were limited to 4K.
  * There were no PC-relative jumps.



These limitations were likely to be particularly severe if a compiler was translating machine-generated source, where functions/procedures might be very large. 

More recent versions of the S/390, probably the G3 manufactured after September 1996, enhance the 32-bit instruction set to allow inline literals and PC-relative jumps. These restrictions do not exist on more recent implementations of the architecture, e.g. the 64-bit zSeries systems, note that GCC v4 and Linux 2.6 appear to assume that the hardware is at least G5 i.e. no older than 2000. 

It is possible to simulate a 32- or 64-bit system using the Hercules emulator, and IBM makes machine time available to developers porting code to their systems. 

[Installing Debian under Hercules](<http://www.josefsipek.net/docs/s390-linux/hercules-s390.html>)

[Community development system](<http://www-03.ibm.com/systems/z/os/linux/support/community.html>)

[An Assembler Programmer's view of Linux for S/390 and zSeries](<http://linuxvm.org/present/SHARE99/S8139db.pdf>)

Whether this is relevant to FPC/Lazarus is arguable since the architecture is already supported on Linux by GCC Pascal, Pascal-XSC, and on other operating systems (as a commercial product) by at least IBM Pascal/VS and/or VS Pascal. Paul Robinson (see below) also points out that Lazarus might be irrelevant on this platform since many facilities provided as standard on PCs and workstations have no counterpart on "classic" IBM operating systems. 

## Architectural details

This section is largely an archive of discussion in the fpc-devel mailing list, starting in January 2012. 

### CPU capabilities

At present, this comprises an episodic list of comments by Steve Smithers from the fpc-devel mailing list starting in late January 2012. Also refer to Paul Robinson's wikibook "360 Assembly" [[1]](<http://en.wikibooks.org/wiki/360_Assembly>)

#### Episode 1

I have just found the thread discussing a port of FreePascal to the System/370 (Which was written by Paul Robinson, who adds his comments here as needed.) and I feel I have to correct some misinterpretations, mistakes and other calumnies that have been thrown into the discussion. First, my qualifications. I have been a developer of Assembler systems, both applications and systems software since 1981. I have worked on VS1, MVS (370, XA, ESA), OS/390 and z/OS systems. I have worked for many large blue chip companies and for software houses (small) and computer manufacturer's (large). 

In this first note, I will deal, mostly, with the character set issues; Other notes will follow - be warned. 

Firstly, the easy one, the System/370 and related processors come with a large supply of 8 bit bytes. :) 

There seems to a common perception on the internet that EBCDIC does not have codes for things like square or curly brackets. This is untrue. (**Note from Paul Robinson:** His remark is correct, EBCDIC did have braces. In fact, there is at least particular version of EBCDIC code pages that allows 100% faithful translation between standard ASCII and EBCIDIC. **End Note**). My little magic EBCDIC reference card (Dated February 1975) lists them as; '}' - 0xD0, '{' - 0xC0, '[' - 0xAD and ']' - 0xBD. Square brackets seem to be "new" with System/370 (1970's), but curly brackets are actually built into the naming requirements of many systems modules for some odd reason. As such, they have always been in the character set. I think the confusion may have arisen by their absence on the original card punch keyboards. But that was 50 years ago, let's try and be a little more up to date than that! 

(**Note from Paul Robinson:** Actually the reason may partially be because the CDC Cyber - which is what Nicklaus Wirth designed Pascal for, initially - did not necessarily have brackets or some characters if you used packed ASCII, which was only 6 bits. This is why the Pascal standard supports (. for [ and .) for ], (* and *) are synonyms for { and }, since you have the former available in sixbit ASCII but you might not have the latter. You always have (, ), * and ., the lesser symbols might not be. **End Note**). (From MarkMLl: even if a character is available in the character set, if it doesn't appear on the available keyboards it's of little use. I'd add that I've tinkered with an APL interpreter written in CDC Pascal, it appears that 12-bit characters were commonly used.) 

A slightly later version of this reference card is available online at [[2]](<http://bitsavers.org/pdf/ibm/370/referenceCard/GX20-1850-3_System370_Reference_Summary_Nov76_2up.pdf>)

'^' doesn't seem to be available, but my eyesight isn't what it was! (VS/PASCAL, an MVS version of ISO pascal, uses -> as a digraph for this). I don't think there's anything else. 

It should be noted that using Linux/390 doesn't remove the '^' problem. IBM display unit (3270) keyboards, being EBCDIC devices, won't have the '^' on the keyboard so an alternative must be found. (I'm assuming that Linux uses 3270 devices, maybe I'm wrong) (**Note from Paul Robinson:** Linux either uses ordinary terminals, IBM supported connecting ASCII terminals to its mainframes, or it can use TN3270, which is telnet using the 3270 full-screen protocol. **End Note**). 

Finally, the suggestions about developing FreePascal/370 as an ASCII compiler seem somewhat pointless to me. Why would anyone want to use an ASCII compiler on an EBCDIC system? (**Note from Paul Robinson:** Newer versions of IBM mainframes support ASCII. But the compiler was not intended to be an ASCII compiler, it was intended to work with the EBCDIC character set. **End Note**). I accept fully that producing an EBCDIC version will present problems, but if this compiler is actually going to be used by anyone, these have to be overcome. -- [[3]](<http://lists.freepascal.org/lists/fpc-devel/2012-January/028174.html>)

#### Episode 2

I have just found the thread discussing a port of FreePascal to the System/370 and I feel I have to correct some misinterpretations, mistakes and other calumnies that have been thrown into the discussion. First, my qualifications. I have been a developer of Assembler systems, both applications and systems software since 1981. I have worked on VS1, MVS (370, XA, ESA), OS/390 and Z/OS systems. I have worked for many large blue chip companies and for software house (small) and computer manufacturer's (large). 

Episode 2. Inline constants 

Firstly, let me explain that there are two different points regarding what has been called "literal" values as concerns S/370 architecture and it's Assemblers. The first of these is called "immediate" values where the literal is included in the actual code generated for that instruction. The second are called "Literals" and describe unnamed constants that are defined on the instruction that uses them in the source code, but resolve to storage areas that are built into the object deck later. 

The "Porting GCC to System/390" document in section 3.1 referred to and other posts state "the original S/390 architecture did not provide instructions that could use literal values as immediate operands". This is untrue. (**Note from Paul Robinson:** Not so, when I say "literal values" I specifically meant anything bigger than one byte or any unsigned value or any value higher than 4095. and specifically strings. **End Note**). Since the System/360 was introduced there was a class of instructions called SI (Storage Immediate) that allowed just that. The values were however, limited to 1 byte. (**Note from Paul Robinson:** One byte of literal storage is hardly useful except to initialize a field to blanks, perhaps. Generally when people work with literals, they're working with slightly larger sized arguments. **End Note**). This has applied to it's descendents (370, 370/XA 390, ESA z/OS and z/OS 64) The 390 extensions in the mid 1990's defined new instructions and extensions to increase this limit to 2 bytes and later to 4 bytes, perhaps, beyond. I've never worked on the latest 64bit machines so I can't comment. (**Note from Paul Robinson:** I have. The new instructions at the 64-bit level support a 20-bit signed integer. Anything larger requires access to storage or use of the contents of a register in some manner. **End Note**). 

An example of SI instruction use. 
    
    
    Code            Source                  Comments
    92C1 C024       MVI   FIELD,C'A'        Move character A to field.
    A728 0009       LHI   R2,H'9'           Load 9 into register 2 Note H is a
                                            halfword or 16 bit integer value
    

The code generated is 92C1,C024 where 92 is the opcode, C1 is the character 'A' and C024 is the address in standard base/displacement form. Or A7 is the opcode, 28 specifies a 32bit load into R2 0009 is the value to load into the register. LHI is S/390 and later. (**Note from Paul Robinson:** Again, the best you're getting as far as an immediate value is one or two bytes. Not really useful except for first-time initialization. **End Note**). 

An equivalent example of literal instruction use. 
    
    
    D200 C024 C136  MVC   FIELD,=C'A'       Move character A to field.
    4820 C134       LH    R2,=H'9'          Load 9 into register 2 Note H is a
                                            halfword or 16 bit integer value
    

The code generated is D200,C024,C136 where D2 is the opcode, C024 is the address of FIELD and C136 the address of the literal, both in standard base/displacement form. The 00 is data regarding the length of data to move limited from 1 to 256 bytes in this case. Or 48 is the opcode, 20 specifies the target register (R2) and the optional index register (unused) C134 is the address of the 16 bit value to load in base/displacement form. Where are the constants? Well they are generated automatically at the end of the module, or if you wish to define them elsewhere you can include a "LTORG" statement which tells the assembler to define them. (**Note from Paul Robinson:** All this does is save you from creating a label. In the case of an = constant, the assembler creates the address and the addressing. The value being moved or loaded is not part of the instruction. **End Note**). 

What I would like to know is "Why is this a problem?" So the constants are defined elsewhere, what issues does this raise? -- [[4]](<http://lists.freepascal.org/lists/fpc-devel/2012-January/028184.html>)

(**Note from Paul Robinson:** Some machines would give you the ability to write code in which a constant was part of the instruction and could be accessed directly. This requires you set up a base-displacement register for the purpose. You would normally do so as part of a program, but that's a different matter. Point is, you do not have the capacity to handle immediately, anything larger than a two-byte value at best and a lot of the time, only one byte. **End Note**). 

#### Episode 3

Episode 3. Addressing and it's limits Part One! 

First, let me apologise for this post as it's going to be a large one. Second, I don't talk about 64 bit modes here because I have never used them. But the basics will not have altered. IBM really does put a lot of effort into maintaining backwards compatibility. (**Note from Paul Robinson:** You can say that again. You can take an application written in 1975 for MVS and run the binary on a zSystem on Z/OS, without recompiling and if the JCL points to the correct files, _it will still work_. Unmodified. More than 35 years after it was written. You can't even run a simple 16-bit MD-DOS application on Windows any more; 64-bit Wintel machines can't run 16-bit code. But the 64-bit mainframes can still run 24-bit apps, even if they are older than the people running them! **End Note**). 

Secondly, I don't actually know anything about the internals of FreePascal or any other compiler come to that, some, or all, of the techniques discussed here, and in part 2, may be impractical or even impossible to implement. It should be noted however that it is not an exhaustive list. 

Thirdly, it should be noted here that if the intention is to provide support on Hercules based systems, that Hercules allows us to use the newer instructions introduced by the processor upgrades even though we are using processors that shouldn't, in theory, support them. This doesn't apply to providing 31 or 64 bit addressing however, as considerable operating system support is required to handle these modes. (**Note from Paul Robinson:** Don Higgins has written an Open Source zSystem emulator in Java called Z/390 that runs on Windows, includes its own Assembler and execution emulator, and will do a fairly good emulation of a 64-bit zSystem running a large subset of what was the MVS operating system. Check out <http://www.z390.org> for more details. **End Note**). 

Finally, I may include bits of 370 assembler in this post. I don't see how I can avoid it. (**Note from Paul Robinson:** Nor do I. **End Note**). I will try and keep them as brief and non-technical as possible, but if you feel your eyes glazing over, ask and I will try and explain another way. 

So, does 370 architecture have a 4k limit on code and data? Well, yes... and no... Sort of... maybe... It depends... (**Note from Paul Robinson:** Newer versions of the processor after the 360 added support for a 19-bit signed address, that, when used in those instruction, since instructions must use a multiple of 2 bytes, allow you to branch upward or downward up to 2^15 16-bit words. So if you use the older instructions, they have a 12-bit signed limit and you can branch anywhere up to 4K bytes in the forward direction from the original address. The newer instructions allow you to branch up to 32767 _words_ up or down from the address in a register, which gives the programmer a potential 128K code and data space per segment. 64K-1 bytes up or down. **End Note**). 

Prior to the upgrades of the 390 processor there was only 1 addressing mode, Base / Displacement or effective addressing. The newer processors introduced Program Counter (it's called the PSW on 390 systems) PC relative addressing but it only applies to code and, perhaps constants, and then only to some instructions; It doesn't apply to data, so the limits are still relevant. 

Base / Displacement consists of a 16 bit value, the first 4 bits enumerate a register, and the other 12 bits hold a displacement from 0 to 4095. The actual or Effective address for each storage operand is calculated as the unsigned addition of the value held in the base register to the displacement from the instruction itself. 

The effective address for each storage reference is real or virtual and 24 or 31 bit depending upon the mode the processor is in at the time. In our case it will, probably, always be a virtual address. 

It should be noted that the base register may not be register 0. Register 0 has an implied value of 0 when used for addressing purposes. 

It is plain that each instruction reference of the Base / Displacement form can only reference a range of 4k, hence the urban myth that that this is a limit on the size of a module. This is where USING enters the fray. USING is an instruction to the assembler. It tells it that a particular register holds the address (24 or 31 bit) of the label mentioned. It is still up to the programmer to load that address into the register, the assembler won't (actually can't) do this for us. 

Throughout, I am assuming that we will be using what IBM defines as standard linkage conventions between modules. Let's start with a basic bit of code that represents a function: 
    
    
     PROG     CSECT              defines the name of our function
              #START             set up standard linkage  
              LR    R12,R15      R15 has the address of PROG, copy it to R12
              USING PROG,R12     Tell the assembler to use R12 as base
                <code goes here>
              #END               return to caller
     SAVEAREA DS    18F          save area for standard linkage         
                <working storage goes here>
     LITPOOL  LTORG
                <constants get defined here>
              END
    

Where #START and #END are macros to set up the standard linkage stuff, SAVEAREA is a required area. The details don't really matter. If the total size of the code, working storage and constants grows beyond 4k, we will get assembly errors. 

However, we can use the USING instruction to help us out here. Part of the standard linkage is that R13 has to point to an area of 18 fullwords (32 bits each). By adding a USING SAVEAREA,R13 to our code; 
    
    
     PROG     CSECT              defines the name of our function
              #START             set up standard linkage
              LR    R12,R15      R15 has the address of PROG, copy it to R12
              USING PROG,R12     Tell the assembler to use R12 as base
              USING SAVEAREA,R13 use R13 as base register for working storage
                <code goes here>
              #END               return to caller
     SAVEAREA DS    18F          save area for standard linkage
                <working storage goes here>
     LITPOOL  LTORG
                <constants get defined here>
              END
    

(**Note from Paul Robinson:** His #START macro presumably will save all registers into the area pointed to by register 13, push the address of SAVEAREA into the current save area pointed to by register 13, save the old value of register 13 in SAVEAREA, then change register 13 to point to SAVEAREA. The #END macro would then have to reverse this process and return to the caller. **End Note**) Now we have defined 2 base registers. We are not allocating an extra register, we have to use R13 as a save area pointer anyway. We are using it to address storage after the save area. Now our code can be up to 4k, and our working storage plus constants can be 4k; 8k in total but still limited. 

One final example and we'll call it a day for part 1. This post is long enough as it is. 
    
    
     PROG     CSECT              defines the name of our function
              #START             set up standard linkage
              LR    R12,R15      R15 has the address of PROG, copy it to R12
              LR    R11,R12      we can set up a second base for the code
              AH    R11,=H'4096' by pointing it 4k past the first one
              USING PROG,R12,R11 Tell the assembler to use R12 and R11 as bases
              LA    R10,LITPOOL  and we can address the literal pool separately.
              USING LITPOOL,R10
              USING SAVEAREA,R13 use R13 as base register for working storage
                <code goes here>
              #END               return to caller
     SAVEAREA DS    18F          save area for standard linkage
                <working storage goes here>
     LITPOOL  LTORG
                <constants get defined here>
              END
    

Here we have set up s second register, R11, to point 4k past R12 and we have used this as a base. The code segment can now be 8k. We have also added a separate register R10, to handle the literals. We now have 16k we can address. A further refinement we can pull is to address all the initialisation code with R12. When we enter the main code, we reset R12 to the start of the main code. Similarly with the exit code. This could give us upto 12k with one base register. 

(**Note from Paul Robinson:** Most programmers, if they were going to use two instructions, would replace 
    
    
             LR    R11,R12      we can set up a second base for the code
             AH    R11,=H'4096' by pointing it 4k past the first one
    

with 
    
    
             LA    R11,4095(R12) Set up second base for the code
             LA    R11,1(R11)    at 4096 from the first one
    

Which does the same thing, but has two advantages. (1) You can do this even if you haven't defined a literal pool - because it generates no literals - and (2) even if you've defined no base register and given no USING instruction, because the LA instruction cannot fault for any reason; an AH instruction could have an overflow, the address of the literal could be wrong, or if the address crossed a page boundary, cause a page fault. The LA instruction can never fail and can never cause a fault, and is faster than accessing an address in memory. **End Note**). 

But there is a limit to the registers we toss around like this and anything more complicated than the above would, if were coding by hand, probably get split into two or more modules. -- [[5]](<http://lists.freepascal.org/lists/fpc-devel/2012-February/028268.html>)

#### Episode 4

Episode 4. Addressing and it's limits Part Two 

So we have seen that Base / Displacement will handle addresses up to a reasonable size. This should cover most of the requirements. But there are still issues that have to be resolved, we need the ability to address more. The way we do this is with Index registers. We get an advantage of unlimited addressing using index registers but we pay a price that we need to generate more code. 
    
    
     PROG     CSECT              defines the name of our function
              £START             set up standard linkage
              LR    R12,R15      R15 has the address of PROG, copy it to R12
              USING PROG,R12     Tell the assembler to use R12 as base
              B     LABELB       we want to branch to the exit here 
                <61440 bytes of code goes here>
     LABELB   EQU   *           
              £END               return to caller
     SAVEAREA DS    18F          save area for standard linkage
                <working storage goes here>
     LITPOOL  LTORG
                <constants get defined here>
              END
    

Taking the simple example we had before, I have added a B (branch or jump to a label) instruction at the beginning. It needs to branch over the arbitrarily large amount of code between itself and the label, but it is more than 4k and we don't have a base register. 
    
    
             LA    R1,(LABELB-PROG)/4096
             SLL   R1,12
             B     LABELA-((LABELA-PROG)/4096)*4096(R1)
    

Explanations first. Instruction 1 splits the code into notional 4k chunks and determines which of these chunks holds the label we want. It places that value into register 1. In our case 61440 / 4096 = 15 (/ is integer division, div in Pascal), so the first instruction loads 15 into R1. Instruction 2 multiplies R1 by 4096 (Shift Left Logical - 12 bit positions). R1 now holds the notional address of the 4k page offset. Instruction 3 is just calculating Address mod 4096 in pascal terms, the remainder from division by 4096. Looks ghastly doesn't it. We wouldn't do it like this in real life, it would be encapsulated into a macro. So we would call something like $BLONG LABELB. The macro would generate 
    
    
             LA    R1,15
             SLL   R1,2
             B     PROG+0x022(R1)
    

The R1 specified on the end is the index register. What happens is that the cpu calculates the address as the base register plus the index register plus the offset. We now have the methodology for infinite (well, as infinite as storage allows) branches. 

(**Note from Paul Robinson:** I'm stupid, and this is way too hard. Or rather, waaaaaaaaaaay too harrrrd if I want whine with my cheesy pun. If I have to branch over an arbitrarily large piece of code or data, I can take advantage of the fact that, by convention, register 15 always points to where our program started, and we do not have to restore register 15. So we could use 
    
    
    PROG    CSECT
            USING *,15
            L     15,NEXT
            BR    15
    NEXT    DC    A(TARGET)
    ... Anything of arbitrary size from 0 to just under 16-megabytes of code and/or data ...
    TARGET  EQU   *
    

As long as the distance between NEXT and TARGET is a multiple of 2 and the entire module is less than, in total, 16 megabytes it will work. No calculations, macros or arbitrary arithmetic needed. **End Note**) 

This complication would need to be considered for other instructions and for constants too. And not all instructions handle index registers, so we would have to point temporary registers at these. But all of this is addressable (sorry for the pun). 

Regardless of what you may believe, FreePascal is not the first compiler to be implemented on 370 architecture. (**Note from Paul Robinson:** Sorry to burst your bubble, but it _ain't been implemented_ , not _yet_ , anyway. **End Note**). Should I tell tell their developers that 370 architecture is too much like a dinosaur to write a 32 bit compiler. (**Note from Paul Robinson:** Free Pascal supports both 32- and 64-bit modes on 64-bit Intel/AMD microprocessors. No reason it can't do the same on the 370 or z/System.**End Note**). 

IBM had 32 bit compilers available in the 1960's. Should I tell them that the architecture is "broken". It's been around for 50 years and there are hundreds of compilers available for it. From FORTRAN to GCC, from COBOL to ADA or from PASCAL/VS to APL. (**Note from Paul Robinson:** APL was always an interpreter, even if it was _the_ finest computer language ever written, but that's nit picking. And let's not forget [Stanford University's Pascal Compiler](<Stanford_Pascal_Compiler.md> "Stanford Pascal Compiler"), the Pascal 8000 compiler from the Australian Atomic Energy Commission, the XPL compiler, the University of Waterloo Fortran Compiler... **End Note**). All of these were (for the ones that were available from the late 60's) 32 bit compilers. -- [[6]](<http://lists.freepascal.org/lists/fpc-devel/2012-February/028310.html>)

#### Episode 5

Episode 5 - Addressing data 

This is where I really expose my ignorance of FreePascal internals. I know a teeny little bit about Delphi so I hope it carries through. 

As far as 370 Assembler is concerned, and this probably applies to most assemblers, there is no inherent difference between data and code, (**Note from Paul Robinson:** That's been true for every type of Von Neumann architecture machine since the 1960s; every commercial processor made today makes no distinction. **End Note**) it is just a label defined as an _aide-memoire_. There is no reason, for example, why you can't load an instruction, or part of it, into a register, modify it and store it back into the program (**Note from Paul Robinson:** You've been able to do that in part on this Mainframe computer series since the 360 with the **EX** ecute instruction, and in some cases, some programs do exactly that. It's called self-modifying code, and he says the same thing about it below, it's bad, bad, bad, practice! **End Note**). I have even seen some programs that pull off such tricks intentionally. I, personally, think shooting is too good for programmers who get up to these sorts of stunts, but it happens. Conversely, there is no reason, as far as the processor is concerned, why you cannot branch (370 for Jump) into the middle of a character array for example. All assembler programmers have come across this one too, it's called a bug. (**Note from Paul Robinson:** No it isn't; tell a salesman about it, and he'll sell it to customers as an extra-cost _feature_! Furthermore, tricking a program into branching into the middle of a character array is a standard practice for what is called a 'buffer overflow attack' as a means to break into a computer. It's why Intel and AMD added the ability to mark some memory in page tables with an "NX" bit, for "No Execute" so that you can mark data as non-executable and thwart these types of attacks. **End Note**). 

So ultimately, it doesn't matter whether we are generating code for a branch instruction or for a move instruction or anything else we use the same basic idea using index registers that we have discussed previously. However, it won't surprise you to learn that there are some complications. (**Note from Paul Robinson:** "Complications" are the reason programmers can make a good living at what they do. **End Note**). Not all instructions support index registers. But remember that the address A for an label is given by A := B + D + I where B is the value held in the base register, D is the displacement held in the instruction and I is the value in the Index register. This sum is performed by the processor internally. So, using the example we had previously: 
    
    
             LA    R1,(LABELB-PROG)/4096
             SLL   R1,12
             B     LABELA-((LABELA-PROG)/4096)*4096(R1)
    

Let's assume that the A (Add) instruction doesn't support index registers, we can rewrite the code fragment as: 
    
    
             LA    R1,(LABELB-PROG)/4096
             SLL   R1,12
             LA    R1,LABELA-((LABELA-PROG)/4096)*4096(R1)
             A     R1,0(R1)
    

The final LA puts the address A in register 1 by doing the arithmetic above, and the A (Add) now specifies a displacement of 0 from base register R1. (we wouldn't actually do it this way under these circumstances. This is an artificial example for illustrative purposes only before any assembler programmers write in.) 

Right - FreePascal - I'm out on a limb here but... All variables (or constants or labels for that matter) have to be defined before use to Pascal (See Note 1 at bottom). (**Note from Paul Robinson:** This is correct. Pascal was designed so that the compilers could potentially be single-pass. That means, with the exception of a pointer, _everything_ must be defined before is is used. You can create a reference to a pointer at the time you use it and then define it at any point later in the program within the TYPE declaration. This is the only exception to the "everything must be defined before use" rule of Pascal. **End Note**). At the time they are defined, an entry will be built in a symbol table. Now the symbol table has to point at the storage reserved for the variable; There are only a limited number of ways it could do that. 

1) It could hold the absolute address. That's stupid because it means the program has to be loaded at the same place all the time. 

2) It could hold the address of the variable as a relocatable value whose value is resolved, either at link-edit time or at program-load time. That doesn't really help us when we come to accessing dynamic storage or using shared code. So... 

3) So it's my guess that it holds the address as a pointer to a block of storage allocated on the heap or the stack or wherever else it wants to put it, and an offset into that block. (plus other stuff like length and type) Sound familiar? This is base / displacement addressing! It's just that the displacement isn't limited to 4k and the base isn't in a register. 

Or maybe I'm wrong. But I don't think I am. Now the displacement, sorry offset, won't have the same limitations that 370 code has, but I've already demonstrated a way to handle this. The only realistic example I can think of that runs counter to this, is where values are themselves in registers. Then, it's not a storage reference problem. 

Note 1. This isn't strictly true I suppose. You can do something like 
    
    
    With TObject.Create
     begin
     ....
     end;
    

However, the compiler has to create a temporary variable to store this value in order to access the object, otherwise it's just memory floating around. At least I think it does! -- [[7]](<http://lists.freepascal.org/lists/fpc-devel/2012-February/028362.html>)

#### Digression: example FPC output

For the purpose of comparison, it may be worth examining FPC's output for a different CPU. The example below is generated by David Zhang's MIPS compiler based on FPC 2.0.0, recompiled with EXTDEBUG to enable the -an option (which is **not** available by default, despite appearing in FPC's help output). Here is the source: 
    
    
    program Test2;
    
    begin
      WriteLn('Hello, World!');
      WriteLn(3.0 + 4.0 + Sin(0.0))
    end.
    

The command used for compilation is ./pp -aln test2.pas and the output is as below: 
    
    
    .file "test2.pas"
    
    .section .text
    
    .section .text
            .balign 4
            .balign 4
    # [test2.pas]
    # [3] begin
    .globl  PASCALMAIN
    PASCALMAIN:
    .globl  main
    main:
    # Temps allocated between $fp-4 and $fp+0
            addiu   $23,$23,0
            .set    noreorder
            .set    nomacro
            sw      $fp,-16($sp)
            sw      $31,-12($sp)
            move    $fp,$sp
            addiu   $sp,$sp,-16
            sw      $4,0($23)
            sw      $5,-4($23)
            sw      $6,-8($23)
            sw      $7,-12($23)
            sw      $8,-16($23)
            sw      $9,-20($23)
            sw      $10,-24($23)
            sw      $11,-28($23)
            sw      $12,-32($23)
            sw      $13,-36($23)
            sw      $14,-40($23)
            addiu   $23,$23,-44
    # second blockn (entry)
    # second nothing-nothg (entry)
    # second nothing-nothg (exit)
    # second asm (entry)
    # second asm (exit)
    # second asm (entry)
    # second asm (exit)
    # second asm (entry)
            jal     FPC_INITIALIZEUNITS
            nop
    # second asm (exit)
    # second asm (entry)
    # second asm (exit)
    # second blockn (entry)
    # second nothing-nothg (entry)
    # second nothing-nothg (exit)
    # second blockn (exit)
    # second blockn (entry)
    # second blockn (entry)
    # second nothing-nothg (entry)
    # second nothing-nothg (exit)
    # second tempcreaten (entry)
    # second tempcreaten (exit)
    # second assignment (entry)
    # second typeconv (entry)
    # second calln (entry)
    # [4] WriteLn('Hello, World!');
            jal     fpc_get_output
            nop
    # second calln (exit)
    # second typeconv (exit)
    # second temprefn (entry)
    # second temprefn (exit)
            sw      $2,-4($fp)
    # second assignment (exit)
    # second calln (entry)
    # second stringconst (entry)
    # second stringconst (exit)
            lui     $6,%hi(_$PROGRAM$_L7)
            addiu   $6,$6,%lo(_$PROGRAM$_L7)
    # second typeconv (entry)
    # second deref (entry)
    # second temprefn (entry)
    # second temprefn (exit)
            lw      $4,-4($fp)
    # second deref (exit)
    # second typeconv (exit)
            addiu   $5,$4,0
    # second ordconst (entry)
    # second ordconst (exit)
            move    $4,$0
            jal     fpc_write_text_shortstr
            nop
            sw      $2,-4($sp)
            addiu   $sp,$sp,-4
            jal     FPC_IOCHECK
            nop
            lw      $2,0($sp)
            addiu   $sp,$sp,4
    # second calln (exit)
    # second calln (entry)
    # second typeconv (entry)
    # second deref (entry)
    # second temprefn (entry)
    # second temprefn (exit)
            lw      $4,-4($fp)
    # second deref (exit)
    # second typeconv (exit)
            addiu   $4,$4,0
            jal     fpc_writeln_end
            nop
            sw      $2,-4($sp)
            addiu   $sp,$sp,-4
            jal     FPC_IOCHECK
            nop
            lw      $2,0($sp)
            addiu   $sp,$sp,4
    # second calln (exit)
    # second tempdeleten (entry)
    # second tempdeleten (exit)
    # second blockn (exit)
    # second blockn (entry)
    # second nothing-nothg (entry)
    # second nothing-nothg (exit)
    # second tempcreaten (entry)
    # second tempcreaten (exit)
    # second assignment (entry)
    # second typeconv (entry)
    # second calln (entry)
    # [5] WriteLn(3.0 + 4.0 + Sin(0.0))
            jal     fpc_get_output
            nop
    # second calln (exit)
    # second typeconv (exit)
    # second temprefn (entry)
    # second temprefn (exit)
            sw      $2,-4($fp)
    # second assignment (exit)
    # second calln (entry)
    # second realconst (entry)
    # second realconst (exit)
            lui     $4,%hi(_$PROGRAM$_L18)
            lw      $8,%lo(_$PROGRAM$_L18)($4)
            lui     $4,%hi(_$PROGRAM$_L18+4)
            addiu   $4,$4,%lo(_$PROGRAM$_L18+4)
            lw      $9,($4)
    # second typeconv (entry)
    # second deref (entry)
    # second temprefn (entry)
    # second temprefn (exit)
            lw      $4,-4($fp)
    # second deref (exit)
    # second typeconv (exit)
            addiu   $7,$4,0
    # second ordconst (entry)
    # second ordconst (exit)
            addiu   $6,$0,-32767
    # second ordconst (entry)
    # second ordconst (exit)
            addiu   $5,$0,-1
    # second ordconst (entry)
    # second ordconst (exit)
            addiu   $4,$0,1
            jal     fpc_write_text_float
            nop
            sw      $2,-4($sp)
            addiu   $sp,$sp,-4
            jal     FPC_IOCHECK
            nop
            lw      $2,0($sp)
            addiu   $sp,$sp,4
    # second calln (exit)
    # second calln (entry)
    # second typeconv (entry)
    # second deref (entry)
    # second temprefn (entry)
    # second temprefn (exit)
            lw      $4,-4($fp)
    # second deref (exit)
    # second typeconv (exit)
            addiu   $4,$4,0
            jal     fpc_writeln_end
            nop
            sw      $2,-4($sp)
            addiu   $sp,$sp,-4
            jal     FPC_IOCHECK
            nop
            lw      $2,0($sp)
            addiu   $sp,$sp,4
    # second calln (exit)
    # second tempdeleten (entry)
    # second tempdeleten (exit)
    # second blockn (exit)
    # second blockn (exit)
    # second asm (entry)
    # second asm (exit)
    # second blockn (entry)
    # second nothing-nothg (entry)
    # second nothing-nothg (exit)
    # second blockn (exit)
    # second asm (entry)
    # second asm (exit)
    # second blockn (exit)
    # [6] end.
            jal     FPC_DO_EXIT
            nop
            addiu   $23,$23,44
            lw      $4,0($23)
            lw      $5,-4($23)
            lw      $6,-8($23)
            lw      $7,-12($23)
            lw      $8,-16($23)
            lw      $9,-20($23)
            lw      $10,-24($23)
            lw      $11,-28($23)
            lw      $12,-32($23)
            lw      $13,-36($23)
            lw      $14,-40($23)
            lw      $fp,0($sp)
            lw      $31,4($sp)
            addiu   $sp,$sp,16
            addiu   $23,$23,0
            j       $31
            nop
            .set    macro
            .set    reorder
    .Le0:
            .size   main, .Le0 - main
            .balign 4 
    
    .section .data
    # [8]
            .ascii  "FPC 2.0.0 [2012/02/09] for mipsel32 - Linux"
            .balign 8
            .balign 8
    .globl  THREADVARLIST_P$TEST
    THREADVARLIST_P$TEST:
            .long   0
    .Le1:
            .size   THREADVARLIST_P$TEST, .Le1 - THREADVARLIST_P$TEST
            .balign 4
    .globl  FPC_THREADVARTABLES
    FPC_THREADVARTABLES:
            .long   2
            .long   THREADVARLIST_SYSTEM
            .long   THREADVARLIST_P$TEST
    .Le2:
            .size   FPC_THREADVARTABLES, .Le2 - FPC_THREADVARTABLES
            .balign 4
    .globl  FPC_RESOURCESTRINGTABLES
    FPC_RESOURCESTRINGTABLES:
            .long   0
    .Le3:
            .size   FPC_RESOURCESTRINGTABLES, .Le3 - FPC_RESOURCESTRINGTABLES
            .balign 4
    .globl  INITFINAL
    INITFINAL:
            .long   1,0
            .long   INIT$_SYSTEM
            .long   0
    .Le4:
            .size   INITFINAL, .Le4 - INITFINAL
            .balign 4
    .globl  __stklen
    __stklen:
            .long   8000000
    .globl  __heapsize
    __heapsize:
            .long   0
    
    .section .data
    
    .section .data
            .balign 4
    .globl  _$PROGRAM$_L7
    _$PROGRAM$_L7:
            .ascii  "\015Hello, World!\000"
    
    .section .data
            .balign 8
    .globl  _$PROGRAM$_L18
    _$PROGRAM$_L18:
    # value: 0d+7.00000000000000E+000
            .byte   0,0,0,0,0,0,28,64
    
    .section .data
    
    .section .data
    
    .section .bss
    

The inserted comments can be fairly easily associated with the points they are generated in the compiler source (look at the call to logsecond() in the secondpass() procedure in ./compiler/pass_2.pas). There might be something at [Assembler and ABI Resources](<Assembler_and_ABI_Resources.md> "Assembler and ABI Resources") that helps. 

### Target operating system

There are a number of operating systems freely available for this architecture: 

  * DOS/360 (DOS/VSE for S/370, EBCDIC) (**Note from Paul Robinson:** Probably not worth targeting. **End Note**).
  * OS/360 (now MVS for S/370, EBCDIC (**Note from Paul Robinson:** The other way around, it used to be called MVS. Then they changed it to OS/VS1 and now it's z/OS. And I think the zSeries can support ASCII, possibly even Unicode. **End Note**).
  * MUSIC/SP (for S/370, EBCDIC) (**Note from Paul Robinson:** MUSIC/SP supports MVS emulation. **End Note**).
  * VM/370 (for S/370, EBCDIC) (**Note from Paul Robinson:** VM/CMS supports MVS or zOS code. You run the same COBOL compiler as z/OS. **End Note**).
  * Linux (for zSeries, ASCII)



Any of these should run on the Hercules emulator, except that MUSIC/SP requires its own emulator (SIM/390) in order for TCP/IP to be available. 

The OS/380 project [[8]](<http://mvs380.sourceforge.net/>) is attempting to enhance the Hercules emulator and the "classic" IBM operating systems above (DOS, OS and VM) to address more memory and possibly make additional facilities available. This is very much "work in progress" but appears to be the most organised maintenance attempt, as well as being a useful repository for obsolete binaries. 

### Character set: ASCII vs EBCDIC

The fact that the "classic" operating systems are EBCDIC-based is likely to cause difficulties, and might necessitate both work in the core compiler and a branch of the RTL. See for example [[9]](<http://en.wikipedia.org/wiki/EBCDIC>), noting that the sorting/capitalisation conventions that apply to ASCII are not applicable. See discussion at [[10]](<http://lists.freepascal.org/lists/fpc-devel/2012-January/027992.html>) [[11]](<http://lists.freepascal.org/lists/fpc-devel/2012-January/028050.html>) [[12]](<http://lists.freepascal.org/lists/fpc-devel/2012-January/028174.html>) plus lesser references in other threads from about the same time. 

## Implementation status

Paul Robinson, Lead Programmer and Chief Cook and Bottle Washer for Viridian Development Corporation is creating a cross-compiler for this architecture, and is documenting in real time what's involved. There is a link to his current work at the bottom of this document. (**Note from Paul Robinson:** Well, I'm trying, anyway! **End Note**). 

Refer to discussion threads in the fpc-devel mailing list [[13]](<http://lists.freepascal.org/lists/fpc-devel/2012-January/thread.html>) which includes discussion of the desirability of this port (unanimously agreed to be a good idea), selection of target hardware and operating system, and problems which might be caused by the EBCDIC character set used by older IBM mainframe operating systems. Also refer to [Qemu and other emulators#Debian zSeries Guest using Hercules, without VM](<Qemu_and_other_emulators.md> "Qemu and other emulators") for discussion of running zSeries Linux on a PC using the Hercules emulator. 

Some of the problems with e.g. limitations on inline literals can potentially be gotten around by careful use of registers in the code. (You can get around most problems if you have spare registers). There are trade-offs: basically registers 3-12 are available for any use; each register can address up to 4K of code or data; (**Note from Paul Robinson:** If you're running on at least a 370 because of the 20-bit instructions you now have access to 64K. But I can't fault him on this, I only found out about the 20-bit expansion within the last couple of months **End Note**) you can use multiple registers to index to the particular portion provided you don't need to manipulate any chunk of data exceeding 4K at a time. One possibility is to use some registers for code and some for data, by moving up from 3 for either code or for data, and moving down from 12 for the other. If it's not too big in both directions, you should be okay.(**Note from Paul Robinson:** This is how the Pascal 8000 compiler from the Australian Atomic Energy Commission does it, it creates a list of registers, knocks them off as it uses them, puts them back as it's finished. If it runs out of registers, it "spills" the expression to memory, then starts over. **End Note**). Nevertheless, one possible use of doing a cross-compiler using Free Pascal for the 370/390/zSystem is to provide a reference on how to do so for other potential architectures. As noted above, Paul Robinson is doing a cross-compiler for the zSeries using Free Pascal and is explaining how he's going about it as he does so. An introduction begins with [Part 1](<ZSeries/Part_1.md> "ZSeries/Part 1") and goes from there. 

## More Information

Paul added further information about the compiler architecture in the following pages. 

  * [Part 1](<ZSeries/Part_1.md> "ZSeries/Part 1")
  * [Part 2](<ZSeries/Part_2.md> "ZSeries/Part 2")
  * [Part 3](<ZSeries/Part_3.md> "ZSeries/Part 3")
  * [Part 4](<ZSeries/Part_4.md> "ZSeries/Part 4")



## Resources

This is a useful book on the zSeries hardware and on z/OS. It's fairly accessible to anybody with industry experience since it explains terms and concepts as it goes along rather than assuming that the reader is from the IBM "priesthood". High Availability and Scalability of Mainframe Environments using System z and z/OS as example (Robert Vaupel, KIT Scientific Publishing) [[14]](<http://digbib.ubka.uni-karlsruhe.de/volltexte/documents/2591965>) <http://comet.lehman.cuny.edu/cocchi/CMP464Mainframe/3.0VMLectures/HighAvailabilityAnd%20ScalabilityBookVaupel.pdf>] 

Mainframe assembler programming. Note that this is principally for an S/370 which doesn't have a conventional stack, IEEE-compatible floating point etc.; it mostly deals with using assembler macros in a business-style environment and is very light on register-level operations and conventions, addressing modes, calling conventions and so on. [[15]](<http://www.billqualls.com/assembler/>)

Bare Metal Programming for the IBM 370 Mainframe. [[16]](<http://www.txxos.com/>)

Content of a book reviewing various operating systems and their usage. Out of date but still useful. [[17]](<http://www.snee.com/bob/opsys/>)

There is still a community of die-hards running antique IBM operating systems on Hercules. The most accessible of these from the point of view of somebody with Windows or unix/Linux experience is probably VM/380, since it has a command line, help facility etc. broadly similar to early-1990s PC-DOS + DLS. [[18]](<http://mvs380.sourceforge.net/>) [[19]](<http://gunkies.org/wiki/Installing_VM/370_on_Hercules>)

Partial emulation of an S/360 using Lazarus [[20]](<http://www.illustratingshadows.com/stats-IBM360.html>)

---

_Source: [https://wiki.freepascal.org/ZSeries](https://web.archive.org/web/20241004030740/https://wiki.freepascal.org/ZSeries)_
