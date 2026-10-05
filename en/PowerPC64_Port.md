# PowerPC64 Port

The **PowerPC64 port** supports code generation for 64-bit [PowerPC](<PowerPC.md> "PowerPC") systems. Historically it started as entry for the [Linux On Power](<http://www.linuxonpower.com/>) contest. 

This page contains a rough outline of what is working and especially what not, some notes about the port and several other related information. More, detailed information (about calling conventions and so on) later. 

  


## Contents

  * 1 Status
  * 2 Installing and Building
    * 2.1 Installing the snapshot
    * 2.2 Updating to the latest version
    * 2.3 Compiling a program
  * 3 Register usage
    * 3.1 Notes
  * 4 32 bit compatibility
  * 5 Optimizing for PowerPC64
    * 5.1 Alignment rules
  * 6 Debugging and Profiling
    * 6.1 Additional resources
  * 7 Troubleshooting
  * 8 More information
  * 9 Contact



## Status

  * 2.1.x compiler support
  * Supports POWER4 and derivative (G5) for Linux at the moment.
  * Not all instructions of the instruction set are recognized by the compiler yet, including missing support for all AltiVec (VMX) instructions.
  * The compiler can be built via the standard procedure, e.g. a `make build`
  * Failed test suite program count is around 20. See the [test suite results](<http://www.freepascal.org/cgi-bin/testsuite.cgi>) for yourselves for the latest numbers. Note that the results for the PowerPC64 compiler are not automatically created every day, but manually every now and then.
  * The IDE is okay now, except for the missing debugging support.
  * Many packages and their examples which are 64 bit big-endian safe already do work (OpenGL, threading, GTK, GTK2, ...)
  * Default debugger information type is DWARF. Stabs are not supported and generation has been disabled. Note that DWARF support is not complete, and hence compiling some code with debugging support fails (e.g. Lazarus)



## Installing and Building

Although the current compiler is still beta, and still has a few bugs, it can already be used by interested people. To get a recent working compiler, it is easiest to start on a G5 using a snapshot, and then update it to the latest version using the development sources. Simply download one for the current development version (2.1.1 at the time of writing this) from the [FTP](<ftp://ftp.freepascal.org/pub/fpc/snapshot/v21/>). The snapshot for the compiler is located in the `powerpc64-linux` subdirectory. 

### Installing the snapshot

Installing the snapshot is easy: Extract it somewhere (e.g. in `/usr`, the FreePascal default installation base directory), and if necessary set your `PATH` environment variable to point to the `bin` directory. Additionally, you need to add a symlink called `ppcppc64` pointing to the `ppcppc64` binary in the `lib` directory of the extracted snapshot directory. For example type 
    
    
     ln -s /usr/lib/fpc/2.1.1/ppcppc64 ppcppc64
    

if you are in the `/usr/bin` directory. 

To be able to use the latest features, it is recommened to update to the most current version using the source code, see below for more information on how to do this. 

### Updating to the latest version

Another way, in particular if you do not want to install the compiler before updating to the latest version, get the `ppcppc64` binary from the snapshot file and use it to cycle the sources. Then do the following: Change into the directory where you have stored the current SVN sources from the current development branch, and enter the following command: 
    
    
     make build PP=<path_to_ppcppc64>/ppcppc64
    

Take care that the given path must be an absolute path, otherwise the compilation process immediately aborts. For the people which want a much faster binary, add `OPT=-O2` to the command line. After a while (and lots of compilation) it should leave you with an empty commandline, and you are ready for installing the compiler. Assuming that you compiled as non-root, do the following: 
    
    
     su   (You will be asked for your root password)
     make install
     exit (To give up root access again)
    

It is possible that compiling the compiler with the `-O2` or other additional options you specified in the `OPT` parameter does not successfully cycle. In that case, first compile and install a current compiler without any additional options, and retry with that one. 

### Compiling a program

Compiling a Pascal program should be as easy as typing 
    
    
     fpc <pascal_sourcefile>
    

after installation completed successfully. If the `fpc` complains about not finding the ppcppc binary, you have to set a symbolic link to the actual binary in the installation directory as described above. 

For further information please also read the offical documentation ([PDF](<ftp://ftp.freepascal.org/pub/fpc/docs-pdf>)) ([HTML](<http://community.freepascal.org:10000/docs-html/fpctoc.html>)). 

## Register usage

The table below shows how the compiler uses the registers for code generation and their meaning. The „volatile“ column indicates whether this register (or register set) is volatile or not, i.e. if a register is marked as volatile, the value is not preserved across function calls, otherwise they are automatically saved and restored by the function prolog and epilog. A value of „N/A“ means that the register is handled in a special way, and in general should not be modified by the programmer. This table basically contains a summary of the information in chapter 3.2.1 of the ABI specification. 

Register | Volatile | Description  
---|---|---  
r0 | Y | Volatile register used in function prologs and constant loading (i.e. scratch register)  
r1 | N/A | Stack frame pointer. The stack must always be quadword (16 byte) aligned  
r2 | N/A | TOC pointer. Used for the GOT/TOC when enabling generation of PIC code (with `-Cg`), otherwise unused by Pascal programs.  
r3 | Y | Parameter register and return value register  
r4-r10 | Y | Registers used for function parameters  
r11 | Y | Register used in calls by pointer, otherwise used as scratch register  
r12 | Y | Register used for glink code and scratch register  
r13 | N/A | Reserved for use as system thread ID. Never touched by the FreePascal compiler  
r14-r31 | N | Registers used for local variables  
  
|   
|   
  
f0 | Y | Scratch register  
f1 | Y | Floating point parameter and return value register  
f2-f13 | Y | Floating point parameter registers  
f14-f31 | N | Registers used for local variables  
  
|   
|   
  
LR | Y | Link register  
CTR | Y | Loop counter register. FreePascal uses it for special code patterns, but not in common code  
XER | Y | Fixed point exception register  
FPSCR | Y | Floating point status and control register  
  
|   
|   
  
CR0-CR3 | Y | Condition code register fields  
CR2-CR4 | N | Nonvolatile condition code register fields  
CR5-CR7 | Y | Volatile condition code register fields  
  
### Notes

  * The condition register is never saved to the stack in a function prolog, because FreePascal and its RTL only uses the volatile parts of this register.
  * There is no support for VMX (AltiVec) extensions in FreePascal, so no register usage is given.
  * r0, r2, r11 and r12 may be destroyed during a function call, i.e. you cannot pass a value to the callee using these registers.
  * do not change r2 at any time, it is used to access the GOT/TOC and is managed automatically by the program loader and linker.
  * The stack is always aligned to 16 bytes as per ABI convention.



## 32 bit compatibility

During porting several compatibility problems of 32 bit PowerPC programs with the 32 bit emulation layer of Linux were found. They are: 

  * the default cache line size is 128 byte instead of 32 byte. This affects some RTL routines which assume 32 byte cache line length, i.e. the assembly fillchar() and move() methods. For this reason, most (except very trivial) FPC compiled programs immediately segfault on PowerPC64. Fixing this involves selecting cache line aware fillchar() and move() methods at program startup.



There may (and I think there are) more issues. These are only those which I could reproducably pinpoint at this time. 

## Optimizing for PowerPC64

There are a few rules of thumb to make PowerPC64 programs perform well. This section will present at least some of them. Of course, these are rather low-level, often a better algorithm is more advantageous than to do this sort of bean-counting. 

  * Use at least the `-O2` (level 2 optimizations) compiler switch which enables register variables to minimize stores and loads to memory. As a side effect this also results in smaller executables. PowerPC64 is a RISC platform, and as such, does not have the complex instructions which can take memory operands. For example, using `-O2` more than halves the time required for cycling the compiler.
  * Align your data if possible. Unaligned access (especially for 64 bit loads and stores on addresses not divisble by four) performs poorly. This basically means, do not use the `packed` modifier for records unless required. See also below for more information about misalignment costs.
  * Use the appropriate data type, preferring 32 bit integers over 64 bit integers due to reduced memory bandwidth requirements. Using a data type less than 32 bits does not give any speed advantage (except maybe if you are bandwidth constrained) because there are only 32 and 64 bit variants of the "slow" integer arithmetic instructions (div, mul). (*)
  * The compiler automatically replaces integer divisions and integer modulo operation with a constant by faster multiplication. Additionally it does proper replacement by shifts if possible, so you do not need to care too much about it. This can be activated using the `-O2` compiler switch.
  * If you enable `-Os` (Optimize for size) the compiler does not inline function prolog and epilog, and uses a stub for calling methods by pointer. This decreases the size of these parts of the code at the expense of additional calls, maybe helping in situations where there are problems with instruction fetch bandwidth.
  * Enable PIC code generation (with `-Cg`), resulting in smaller and faster code. The reason is that on the 64 bit PowerPC platform address loading takes five instructions, while when enabling this switch this instruction sequences are replaced by a single memory load.



(*) Although these optimizations are not implemented yet for the PowerPC64 platform, they surely will be =). Maybe these optimizations will require setting some optimization switch though, or will be disabled for some settings. 

### Alignment rules

Some rules and information on alignment penalties on the PPC970 processor, taken from the manuals. These are mainly of interest for assembly programmers though: 

  * A memory reference is misaligned if its address is not divisible by the size of the access.
  * The hardware handles all misaligned memory references, i.e. loads and stores. However, it does so with different penalties depending on the memory boundary crossed and the size of the memory access. The interesting boundary granularities are 32, 64 and 4096 bytes.
  * In general the processor does a reasonable job of handling unaligned loads/stores which do not cross a lower than 32 byte boundary.
  * Doubleword (8 byte) should be at least aligned on a word (4 byte) boundary. If you are sure to do a misaligned doubleword load/store, use two word sized operations.
  * When using the loads and stores to copy data, and both source and destination are misaligned, align the source. Loads get penalties on misaligned 32/64 and 4k boundaries, while stores only on 4k boundaries. One can avoid both penalties in this case by aligning on one address, and constructing an aligned access to the other address by clever shifts.
  * VMX memory loads/stores require 16 byte alignment. If you load/store from an unaligned address, the processor silently aligns the address, i.e. it works as if the lower 4 bits of the effective address are zero.



## Debugging and Profiling

This section contains some information on tools to debug and test applications for the PowerPC64 platform generated by the FreePascal compiler. 

In general, the FreePascal standard debugging facilities, i.e. enabling debug code generation (`-g` switch) in conjunction with line information (`-gl`) and heap tracing (`-gh`) works. See the FreePascal manual for more information about these things. 

Currently FreePascal the integrated debugger in the IDE does not work. You either have to resort to the [GNU GDB](<http://www.gnu.org/software/gdb>) commandline debugger and any of the backends (for example [DDD](<http://www.gnu.org/software/ddd>)). Make sure that debugging for the 64 bit PowerPC platform has been compiled in - some Linux distributions only provide a GDB capable of debugging 32 bit applications. 

Using the latest FreePascal compiler version, basic profiling can be done with [GNU gprof](<http://www.gnu.org/software/binutils/>). See the manual on how to enable generation of required code (Note: try the `-pg` compiler switch). Since gprof seems to be pretty basic and the technique used rather intrusive, requiring some additional code in the executable, make sure to also try the profilers mentioned in the subsection below. 

### Additional resources

This subsection contains some information for additional debugging or profiling resources on the PowerPC64/Linux platform: 

  * The current source tree of [Valgrind](<http://www.valgrind.org>) (3.2.0) supports the PowerPC64 processor on the Linux platform. Valgrind provides a set of tools for memory debugging, memory leak detection and profiling.  
Since PowerPC64 support is only available in the source tree, you have to get the latest version from the SVN tree and compile it yourselves. The Valgrind homepage contains instructions for all these steps (see the [Valgrind Code Repository](<http://valgrind.org/downloads/repository.html>)).
  * Another profiling tool available for profiling PowerPC64 applications is [OProfile](<http://oprofile.sourceforge.net>). Depending on your Linux distribution, it may or may not already be installed. Next to the introduction on the OProfile homepage, IBM provides some examples [here](<http://www-128.ibm.com/developerworks/linux/library/l-oprof.html>) and [here](<http://www-128.ibm.com/developerworks/library/l-pow-oprofile/>).



## Troubleshooting

This section contains some hints for solving mysterious behaviour of compiled code 

**Problem:** When linking to some C library, the code sometimes crashes. 

**Solution:** Some C libraries (for example OpenGL) sometimes give an "Invalid Floating Point Operation" (Underflow, Overflow) or similar when used in Pascal. This is due to bugs in the C library which are not detected there, because C by default disables these exceptions. Try adding the following assembly code to your main program: ` asm mtfsfi 6, 1 end; `. This disables these exceptions globally (also in the Pascal program). To fix this, tell the maintainers of the library about this bug. 

**Problem:** The compiler gives strange errors, especially after updating to a new version 

**Solution:** Make sure that there are not any stale (old) compiled units lying around, and make sure that the compiler actually uses the recent ones (use the `-vt` switch to display tried files) 

**Problem:** Debugging information is displayed incorrectly or not at all. 

**Solution:** Enable debugging support using the `-g` switch to enable generation of debugging information. 

## More information

This section contains links and notes to further information about the PowerPC64 architecture. This includes links to the ABI specification, general processor descriptions, instruction set documentation and general compiler notes. 

  * Ian Lance Taylor, _64-bit PowerPC ELF Application Binary Interface Supplement 1.7_ , Zenbu Labs IBM, 2004 ([HTML](<http://www.linuxbase.org/spec/ELF/ppc64/spec/book1.html>)) ([PDF](<http://www.linuxbase.org/spec/ELF/ppc64/PPC-elf64abi-1.7.pdf>)) - This was the original ABI specification the PowerPC64 compiler was developed with.


  * Ian Lance Taylor, _64-bit PowerPC ELF Application Binary Interface Supplement 1.9_ , Zenbu Labs IBM, 2004 ([HTML](<http://www.linuxbase.org/spec/ELF/ppc64/spec/>)) (No PDF) - An update to the previous ABI specification. However, it only seems to add some clarifications compared to 1.7.


  * [PowerPC 970 and 970FX microprocessor documentation](<http://www-306.ibm.com/chips/techlib/techlib.nsf/products/PowerPC_970_and_970FX_Microprocessors>) \- This website contains lots of information about the 970 and 970FX microprocessors. The (imo) most interesting references from this page are summarized below.


  * [IBM PowerPC 970FX RISC Microprocessor User's Manual](<http://www-306.ibm.com/chips/techlib/techlib.nsf/techdocs/AE818B5D1DBB02EC87256DDE00007821>) \- This website contains a manual which contains the technical description of the 970FX (G5) processor with (mainly) interesting information about the storage and memory subsystem.


  * [Developing Embedded Software For The IBM PowerPC™ 970FX Processor](<http://www-306.ibm.com/chips/techlib/techlib.nsf/techdocs/AB70A3470F9CC0E287256ECC006D6A54>) \- Contains an application note which describes the differences between PowerPC32 and PowerPC64, and the issues which need to be handled when porting from the PowerPC32 to the PowerPC64 architecture


  * [The Programming Environments Manual for 64-bit Microprocessors](<http://www-306.ibm.com/chips/techlib/techlib.nsf/techdocs/F7E732FF811F783187256FDD004D3797>) \- This website contains a manual which summarizes the features and pecularities of the 64 bit processor from a programmer's point of view, including overview of the processor, instruction set and so on.


  * [PowerPC Architecture Book](<http://www-128.ibm.com/developerworks/eserver/articles/archguide.html>) \- This website contains links to the PowerPC architecture manuals, i.e. a complete description of the instruction sets, containing both privileged and unprivileged instructions. The next links are direct references to the three books: Book I ([PDF](<ftp://www6.software.ibm.com/software/developer/library/es-archpub1.zip>)), Book I I ([PDF](<ftp://www6.software.ibm.com/software/developer/library/es-archpub2.zip>)), Book III ([PDF](<ftp://www6.software.ibm.com/software/developer/library/es-archpub3.zip>))


  * [PowerPC Compiler Writer's Guide](<http://www-3.ibm.com/chips/techlib/techlib.nsf/techdocs/852569B20050FF7785256996007558C6>) \- This website contains useful information for writing compilers for the PowerPC architecture, i.e. optimized code examples for common patterns. This manual has been written for the PowerPC32 architecture, but the techniques presented there can be easily ported over to the PowerPC64 platform.


  * [GNU C/C++ toolchain for Linux on POWER](<http://www-128.ibm.com/developerworks/library/l-pow-gnutool/>) page - Introductory text about the GNU C/C++ toolchain. Although this page seems to be totally unrelated to FreePascal, it also contains a short overview of the GOT and TOC sections.



## Contact

Message to the fpc-devel mailing list, or look for "tom_at_work" in the IRC channel (or of course discuss it here).

---

_Source: [https://wiki.freepascal.org/PowerPC64_Port](https://web.archive.org/web/20241212105936/https://wiki.freepascal.org/PowerPC64_Port)_
