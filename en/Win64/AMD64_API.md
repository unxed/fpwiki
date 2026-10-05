# Win64/AMD64 API

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<../Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Old Information
    * 1.1 Data type sizes
  * 2 Notes on Win64 for AMD calling conventions
    * 2.1 About documentation
    * 2.2 Preliminary notes:
    * 2.3 Some definitions:
    * 2.4 Calling conventions in Win64 for AMD
      * 2.4.1 Parameter passing
      * 2.4.2 Return values
      * 2.4.3 Volatile and nonvolatile registers
        * 2.4.3.1 About x87 and mmx registers
        * 2.4.3.2 x87 and MMX control registers
      * 2.4.4 Stack configuration
        * 2.4.4.1 Shadow space
        * 2.4.4.2 Stack alignment
        * 2.4.4.3 Frame pointer
        * 2.4.4.4 Parts of function: prolog, body and epilog
          * 2.4.4.4.1 Function prolog
          * 2.4.4.4.2 Function epilog
    * 2.5 Data alignment
    * 2.6 Constants
    * 2.7 Exception
    * 2.8 Debugger support
    * 2.9 Differences between PE and PE+ exe format
    * 2.10 Things that must be done on FreePascal
      * 2.10.1 Calling convention
      * 2.10.2 Output format
      * 2.10.3 Adapt rtl



# Old Information

As far as I know, there is no offical document yet decribing the win64/amd64 api so the follow information is collected from several sources. 

Begin in this link <http://msdn.microsoft.com/library/default.asp?url=/library/en-us/win64/win64/getting_ready_for_64_bit_windows.asp>

## Data type sizes

Basic information at <http://msdn.microsoft.com/library/default.asp?url=/library/en-us/win64/win64/the_new_data_types.asp>

# Notes on Win64 for AMD calling conventions

## About documentation

Documentation about Windows x86-64 calling convention does exist. It can be found online on msdn: <http://msdn.microsoft.com/library/default.asp?url=/library/en-us/Kernel_d/hh/Kernel_d/64bitAMD_6ec00b51-bf75-41bf-8635-caa8653c8bd9.xml.asp>

This documentation says that it's preliminary and subject to change, and is dated February 2005. 

However, there is a SWConventions.doc file in Microsoft Platform SDK, dated May 2005. It's a more recent version of what is published in URL above (in fact there are some important differences). In the file the only warning about "preliminary documentation" is found in "hot patchability" chapter. Since win64 has shipped in the USA I think that this documentation can be assumed as definitive. This file is located in the same directory of binaries for amd64 (cl, ml64, link and so on). I think that it cannot be redistributed (license says that one can copy it for its personal reference, but doesn't say anything about redistribution of the file). That file is the reference upon documentation in this wiki is based. 

## Preliminary notes:

rvas are still 32 bit: maximum file size for an exe file is 2 gb, so there is no need to use larger address space. 

## Some definitions:

**leaf function** : a function that doesn't call any function, neither allocates stack space by itself. This function doesn't have a frame pointer. 

**frame function** : a function that has a frame pointer. 

## Calling conventions in Win64 for AMD

There is only one calling convention. Actually Microsoft include files are shared among Windows 32 bit and 64 bit. Functions modifiers like stdcall, cdecl and fastcall are ignored on 64bit windows since they use an unique calling convention. 

### Parameter passing

The first 4 parameters are passed on registers. Integer parameters (from 1 to 8 bytes long) are passed in RCX, RDX, R8 and R9 registers, where the first parameter (the leftmost) is passed in RCX and the fourth in R9. Floating point parameters are passed in XMM0L, XMM1L, XMM2L and XMM3L. Parameters beyond the fourth are passed on stack. **Note** : If there are both integer and floating point parameters, use of n-th register is mutually exclusive; if the second parameter is a floating point number and the third is integer, the second one goes in XMM1L and the third in R8. Example: 
    
    
    function(int1,real1,int2,real2,int3,real3)
    

means: 
    
    
    push real3  // passed on stack
    push int3   // passed on stack
    XMM3L=real2 
    R8=int2
    XMM1L=real1
    RCX=int1
    

Actually cl doesn't use real pushes to put arguments on stack, but we'll talk about it later. 

Parameters greater than 8 bytes are passed by reference (so it's caller duty to allocate space for a copy of the parameter and then pass a pointer to it). 

Some notes for vararg/unprototyped functions: if there are floating point arguments, they must be passed **both** in integer register **and** in floating point register, without conversion, so that the function can retrieve the parameter if it expected a integer parameter (in other words: if an unprototyped function expects an integer parameter as second parameter it would look in RDX. Since it's not prototyped we don't know it, and if we pass a floating point argument as second parameter we end up with the value in XMM1L and garbage in RDX: so we copy the value in both registers to avoid problems.) If the argument is beyond the fourth there is no special-handling, since callee will look at the same stack location if it expects an integer parameter or a floating point parameter. 

### Return values

Integer values which have length between 1 and 8 bytes are returned in RAX. Floating point values, __m128, __m128i or __m128d values are returned in XMM0. 

If return value is too big to fit in one of these registers, it's **caller** duty to allocate stack space before calling the function, and to pass a pointer to this location as first parameter (so there are now only three registers to store other parameters). 

### Volatile and nonvolatile registers

In addition to registers used to pass parameters, RAX, R10, R11, XMM4 and XMM5 aren't preserved by callee and should be saved by the caller if needed (R10 and R11 are used for syscall/sysret, don't know about XMM4 and XMM5). Other registers must be saved by callee if used, even if a special treatment is reserved to x87 and mmx registers. 

#### About x87 and mmx registers

x87 and mmx registers aren't used: all floating points calculations are made in XMM* registers. Although x87 and mmx registers are guaranteed to be preserved across context switching, this is not true for functions calls. Microsoft says that if you use x87 registers you should consider them as volatile registers. There's nothing about mmx registers apart the context-switching thing, so I think that even mmx registers have to be considered volatile. 

#### x87 and MMX control registers

**x87** The x87 control word is considered nonvolatile and must be preserved by the callee. When program is started, FPCSR is set this way: 
    
    
        FPCSR[0:6]         : Exception masks all 1's (all exceptions masked)
        FPCSR[7]           : Reserved - 0
        FPCSR[8:9]         : Precision Control - 10B (double precision)
        FPCSR[10:11]       : Rounding  control - 0 (round to nearest)
        FPCSR[12]          : Infinity control - 0 (not used)
    

If a function modifies this register it should then reset the register to the original value before returning. It should do the same thing when calling another function (however, if there is a function whose purpose is to modify this register this pass can be skipped. It can be skipped even if it's known that called function doesn't rely on that register). **MMX** MMX control register bits are considered volatile from 0 through 5, and nonvolatile from 6 and beyond. Same considerations made for FPCSR are valid for nonvolatile part of MXCSR. This is MCXSR setting when program starts: 
    
    
        MXCSR[6]        : Denormals are zeros - 0
        MXCSR[7:12]     : Exception masks all 1's (all exceptions masked)
        MXCSR[13:14]    : Rounding  control - 0 (round to nearest)
        MXCSR[15]       : Flush to zero for masked underflow - 0 (off)
    

### Stack configuration

#### Shadow space

There is the concept of "shadow space" when calling a function. The caller must always allocate a fixed space of 32 bytes (4*8 bytes) on stack before calling a function: this is called shadow space, and it's used by callee to store arguments that are passed on registers if the callee needs these arguments to be in memory (after storing four register arguments to shadow area, all arguments appear as contiguous array on stack, making it easy to implement functions like printf()). The register parameters are also typically stored into shadow space when generating debugging code, providing the debugger a consistent location where to look for parameter values. Microsoft cl has an option to force storing register parameters to shadow space, which is enabled for debug builds. This stack space must always be allocated, even if function has less than 4 parameters. This space must be adjacent to callee return address: stack pointer must point to the end of this space before calling the function. So if more than 4 parameters are passed, parameters beyond 4 are at higher addresses, then there is shadow space on top of the stack. 

#### Stack alignment

Stack pointer (RSP) must be aligned on multiples of 16 bytes when calling another function. Note: when called functions begins execution, stack isn't aligned in 16-bytes boundary: in fact it is aligned before call instruction, then call instruction places return address on stack (which is 8 bytes long) making stack unaligned. Therefore, in leaf functions the stack stays unaligned (according to the definition of leaf functions, any change to the stack pointer makes the function non-leaf). 

#### Frame pointer

In most cases, Win64 API uses the fixed stack, i.e. value of the stack pointer is changed only in prolog and epilog, not in the function body. This removes the need of having a separate frame pointer. An exclusion to this rule is dynamic stack allocation using alloca() function. Also, a frame pointer can be set pointing into the middle of local variables area, so local variables are addressed at both positive and negative offsets from the FP. Since offsets values below 128 are encoded using shorter instructions, this allows generating smaller code. The frame pointer is not restricted to be RBP, it can be any non-volatile register. 

#### Parts of function: prolog, body and epilog

To make work of exception handler easy, functions are considered "splitted" in three parts: prolog, body of function and epilog. Prolog is described using unwind data, and the epilog has a standard configuration so that exception handler can recognize them properly. 

##### Function prolog

Prolog is the very first part of function. Generally, function prolog does this work: 

  * Copies parameters from registers to shadow space
  * Pushes registers to be preserved on stack
  * Allocates room on stack for local variables
  * Sets a frame pointer (so frame pointer is set AFTER local variables!) if needed
  * Allocates space needed to store volatile registers that must be preserved in function calls
  * Allocates shadow space for called functions.



This is what microsoft cl does, although this should not be seen as a restriction, since you can chose other methods. If in the body of function dynamic allocation of memory on stack is needed, no problem: memory is allocated but block stays between frame pointer and rsp-(size of space needed for volatile registers+size of shadow space for called functions), so that shadow space for called functions it's always on top of the stack. This layout might help: 
    
    
     ---------------     --
    |      R9       |      |
     ---------------       |
    |      R8       |      |
     ---------------       |Shadow space for this function, allocated by caller
    |      RDX      |      |
     ---------------       |
    |      RCX      |      |
     ---------------     --
    | Return address|
     ---------------
    |               |
    |               |        Local variables of this function
    |               |
    |               |
     ---------------
    | Frame Pointer |        Optional
     ---------------
    |               |
    |               |        Memory used for dynamic allocation
    |               |
    |               |
     ---------------
    |               |
    |               |        Memory used to save registers that may be modified by callee
    |               |
    |               |
     ---------------     --
    |      R9       |      |
     ---------------       |
    |      R8       |      |
     ---------------       |Shadow space for callee, allocated by this function
    |      RDX      |      |
     ---------------       |
    |      RCX      |      |
     ---------------     --
    

Code example: 
    
    
    int func2(int i1, int i2, int i3)
    {
      int lets_waste_stack_space = 53;
      return (i1 + i2 + i3);
    }
    
    int func1(int i1, int i2)
    {
      int another_local_variable = 51;
      void * buf = alloca(100);
      return (func2(i1, i2, 3));
    }
    
    int main()
    {
      int a_local_variable = 50;
      func1(1,2);
      return(0);
    }
    

This is memory layout in func1: 
    
    
    xxxxxxxx
    xxxxxxxx
    i2 = rdx
    i1 = rcx
    retaddr
    old rbp                       | local variables
    buf                           |
    another_local_variable <= rbp |
    xxxxxxxx         | shadow for    | room for buf
    xxxxxxxx         | func2         | dinamically
    xxxxxxxx         | before        | allocated
    xxxxxxxx <= rsp1 | alloca        |
    xxxxxxxx                         |
    xxxxxxxx                         |
    xxxxxxxx                         |
    xxxxxxxx                         |
    xxxxxxxx                         |
    xxxxxxxx                         |
    xxxxxxxx                         |
    xxxxxxxx                         |
    xxxxxxxx                         |
    xxxxxxxx <= buf__________________|
    xxxxxxxx                                       | shadow for
    xxxxxxxx                                       | func2
    xxxxxxxx                                       |
    xxxxxxxx <= rsp2______________________________ |
    

Main allocates shadow space of func1: 32 bytes even if only 2 parameters are used. It puts 1 in rcx and 2 in rdx, then call instruction places return address on stack. This is what func1 does in its prolog: 

  * Copies parameters from registers to shadow space: 
    * Copies rdx in rsp+16 and rcx in rsp+8
  * Pushes registers to be preserved on stack: 
    * Pushes ebp.
  * Allocates room on stack for local variables: 
    * Decrements rsp by 16 to reserve room for buf and another_local_variable
  * Sets a frame pointer: 
    * rbp now points to rsp (that is, to another_local_variable address)
  * Allocates space needed to store volatile registers that must be preserved in function calls: 
    * no space needed
  * Allocates shadow space for called functions: 
    * Decrements rsp by 32. rsp position is rsp1 in previous scheme.



End of prolog. 

In function body, alloca is called: rsp is decremented by 112 (alloca allocates memory aligned on 16-bytes boundary). rsp position is rsp2 in schema now. Buf doesn't point to same location of rsp since shadow space for callee must stay on top of the stack: instead it points 32 bytes before rsp, so that buf stays between rbp+8 and rsp-32. When func2 is called, it finds its shadow space where it should be (adjacent to return address). 

Note that this is modus operandi of microsoft cl, but it's perfectly legal to use other strategies (this is even remarked by microsoft). It is perfectly legal to push registers to be preserved and to allocate shadow space before a function call, and then release memory after the call. 

##### Function epilog

Epilog is the last part of function. Function epilog does this work: 

  * Release stack space
  * Pops registers that were saved
  * returns



It should not do other things: function result should be set before epilog. Microsoft suggests that for functions without frame pointer stack is released adding a constant value to rsp. For function with frame pointer this can be accomplished in two ways: adding a costant to rsp or loading rsp with rbp address. This restrictions are needed so that exception handler can easily detect function epilog. 

This is ML64 assembly listing for func1 in previous example; Comments and exception-related stuff has been removed. You can see that cl uses a more optimized approach: instead of reserve space for local variables, set frame pointer and reserve space for shadow it reserves space in an unique operation and then sets frame pointer to the right value. 
    
    
    PUBLIC	func1
    _TEXT	SEGMENT
    func1	PROC NEAR
           mov	DWORD PTR [rsp+16], edx           ; save second parameter
           mov	DWORD PTR [rsp+8], ecx            ; save first parameter
           push	rbp                               ; save old rbp value
           sub	rsp, 48                           ; room for local vars + shadow space for called function
           lea	rbp, QWORD PTR [rsp+32]           ; sets frame pointer to the end of local variables block.
                                                     ; end of prolog
           
           mov	DWORD PTR [rbp], 51               ; another_local_variable = 51;
           
           mov	eax, 112                          ; make room for buf
           sub	rsp, rax
           lea	rax, QWORD PTR [rsp+32]           ;
           mov	ecx, DWORD PTR [rax]              ; buf now points to rsp+32. Shadow space lies between
           mov	QWORD PTR 8[rbp], rax             ; rsp+32 and rsp.
           
           mov	r8d, 3                            ; third parameter of func2: 3
           mov	edx, DWORD PTR 40[rbp]            ; second parameter: i2
           mov	ecx, DWORD PTR 32[rbp]            ; first parameter: i1
           call	func2                             ; call to func2
           
                                                     ; here starts epilog
           lea	rsp, QWORD PTR [rbp+16]           ; frame function, use lea to release stack
           pop	rbp                               ; pop ebp register that was saved
           ret	0                                 ; return
    func1	ENDP
    _TEXT	ENDS
    

## Data alignment

For performance reasons, data should be aligned to its natural alignment. Arrays should be aligned on their elements' natural alignment. Fields inside structures should be aligned on their natural alignment, and structure itself should be aligned according to alignment of its wider field (so if a record is made of a byte and a qword, record is aligned on 8-byte boundary, byte starts at record+0 and qword at record+8). 

Functions must be aligned on 4 byte multiples, but 16-byte alignment is encouraged for performance reasons. 

## Constants

Enums and integer constants are treated as 32-bit integers. Bitfields can't be larger than 64 bits. 

  


## Exception

Win64 doesn't support the old i386 way of doing exception handling using fs:(0) as a linked list of exception handlers. Instead, it uses table based exception handling. However, FPC uses it's own code to unwind exceptions so it needs a way to intercept exceptions before the table based exception handling jumps in. This can be done using vectored exception handling which is new in Windows XP: <http://windowssdk.msdn.microsoft.com/library/default.asp?url=/library/en-us/Debug/base/using_a_vectored_exception_handler.asp>

Don't get fooled by the comment at the top of the article, it only means that the code below uses fields in the context record which aren't available in win64 (eip vs. rip) so adapting this, the code works. 

## Debugger support

## Differences between PE and PE+ exe format

PE32+ Magic number (in optional header) is 0x20b while PE32 is 0x10b 

There is no BaseOfData field in Standard Fields of Optional Header 

Windows Nt Specific Fields (Optional Header): ImageBase, SizeOfStackReserve, SizeOfStackCommit, SizeOfHeapReserve, SizeOfHeapCommit are 8 bytes long instead of 4 

Import Lookup Table entries are 64 bits long instead of 32. Bit 63 is Ordinal/Name flag, bits 62 through 0 are Ordinal Number or Hint/Name Table RVA when Ordinal/Name flag is 1 or 0 respectively 

Import Address Table has same structure as Import Lookup Table 

TLS Directory Raw Data Start VA, Raw Data End VA, Address of Index, Address of Callbacks are 8 bytes long instead of 4. 

## Things that must be done on FreePascal

First of all: I'm not a fpc developer nor I know fpc internals (I only have some ideas of what files do what thing) so I might be wrong :P It has been said that it's useless to make a win64 port before gcc has been ported, since fpc relies on gas, ld and gdb. But maybe something can start already, so that fpc is already "one step forward" when gcc arrives on win64. 

### Calling convention

First of all, a new calling convention is required. Here there are two choices: 

  * Make a brand new calling convention (win64amd?)
  * Make cdecl, stdcall and safecall as "aliases" to this new calling convention on x86_64-win64



Second choice is microsoft choice: since microsoft c compiler considers these conventions all the same thing, logical consequence is that every other c compiler will use this convention in the future. Old calling conventions have no meaning in win64. 

First choice can be used if there is the possibility that some c compiler uses these conventions even on win64. This means that freepascal headers for windows apis should be rearranged: on i386 there should be a 
    
    
    {$calling stdcall}
    

while on win64 for amd there should be a 
    
    
    {$calling win64amd}
    

and parts where calling convention is directly specified as function modifiers should be carefully modified/moved to inc files. 

If second option is chosed it could be useful to temporary set this name to "win64amd" so that it can be tested and debugged on linux-x86_64, where we have gdb (making functions with win64amd modifier and calling them in pascal source files, and test and debug the calling convention). When everything is ok it will be an alias to cdecl, stdcall and safecall on win64 for amd and name will be removed. 

A function that is declared to use this convention should do these things: 

  * if RCX, RDX, R8, R9, XMM0, XMM1, XMM2, XMM3 are used, their value should be saved in shadow space
  * other nonvolatile registers, if used, should be pushed on stack. These registers are R12 through R15, RDI, RSI, RBX, XMM6 through XMM15
  * MXCSR and FPCSR, if used, should be pushed on stack.



A function that call a function that uses this convention should do these things: 

  * Save volatile registers, if used. These are RCX, RDX, R8, R9, XMM0, XMM1, XMM2, XMM3, RAX, R10, R11, XMM4, XMM5.
  * Save x87 and MMX registers, if used.
  * If MXCSR and FPCSR have been modified, their original value should be restored.
  * Push parameters beyond the fourth on stack and align stack on 16-byte boundary if needed. Stack must be padded before pushing parameters according to current stack alignment and number of parameter beyond fourth, so that when last parameter that must be on stack is pushed, stack is 16 byte aligned.
  * Put parameters 1..4 on registers.
  * Allocate 32 bytes on stack for shadow space of callee.



### Output format

Okay, there is no gas for win64. Since rtl and windows api should be adjusted, a temporary way to start working on it could be to use masm output and compile with ml64. There is a masm writer, and it looks like it has been upgraded (if not upgraded, something has started to move) to handle ml64 syntax too. While this is a good thing, since it's good to have masm output option even if gas would be available, it can be seen as a useless effort. So, binary writer could be improved to handle x86_64 as well. x86_64 is i386 with larger pointers, a couple of instruction were added and some removed: it should be easy to adapt i386 binary writer so that it can write x86_64 code too. Another good thing is that this can be tested and debugged under linux for x86_64, and this platform would benefit from binary writer too. There is the PE+ coff format to implement, but it's not very different to PE coff: maybe differences will be written in this wiki too. 

### Adapt rtl

Having binary writer and calling convention, link can be performed with microsoft link until binutils are ported to win64: we could start adapting windows rtl. 

Of course, debugging support will still be missed, and I think that adding support for another debugger like windbg should be a big and useless effort, but while gcc isn't available for win64 freepascal could be almost-ready and win64 could be considered an experimental platform, not ready yet to be included to the official ones.

---

_Source: [https://wiki.freepascal.org/Win64/AMD64_API](https://web.archive.org/web/20220705061421/https://wiki.freepascal.org/Win64/AMD64_API)_
