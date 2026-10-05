# PIC information

The linker/loader maintain a global offset table, which consists of an array of pointers to exported variables. All access to those variables is supposed to go through these pointers, if the variable could reside within another dynamic object. 

If 'variable' is an exported symbol, then 'variable@GOT' is replaced by the linker with a constant: the offset of the GOT slot containing the pointer to variable. So, if the %ebx register contains the start address of the GOT, you can retrieve this pointer via 
    
    
    movl variable@GOT(%ebx), %eax
    

If you want the value of the variable, you need another indirection: 
    
    
    movl (%eax), %eax
    

If you know, however, that the variable in question must reside in the same dynamic object as the user, then you can avoid that double indirection, by using the GOTOFF facility. 'variable@GOTOFF' is replaced by the linker with another constant: the difference between the absolute address of the variable and the address of the global offset table (of the current dynamic object). Thus, you can retrieve the value of the variable in one step using: 
    
    
    movl variable@GOTOFF(%ebx), %eax
    

* * *

**source unknown**
    
    
    From: Joerg Pommnitz (pommnitz@heidelbg.ibm.com)
    Subject: Re: ELF--Position-Independent Code: Why? 
    
    View this article only 
    Newsgroups: comp.os.linux.development.system
    Date: 1995/08/01 
    
    
    Just to add a small remark to this thread:
    It is NO requirement that ELF dynamic libraries are position independent. You
    can build dynamic libraries from plain, non-pic object files. The disadvantage
    of this is the fact that they are not shared very much. The ELF loader
    has to do relocations, so lots of pages become dirty and can't be shared
    among processes. That's why non-pic ELF dynamic libraries should not be used
    for code that's normally shared among lots of processes running at the same
    time (i.e. libc, libX11, ...).
    
    Best Regards
           Joerg
    

* * *

However pic code is also bulkier and slower, and specially by C++ code with its massive number of symbols this is a problem. This can be avoided by taking a large heap of libraries and relocate them as a kind of library cache. Most startup accelerators in commercial OSes work that way (OSX/Windows). 

[Marcov](</User:Marcov> "User:Marcov") 13:57, 20 March 2007 (CET) 

### fpcmake

fpcmake automatically adds -Cg/-fPIC for x86-64 platforms: 

  * for freebsd, openbsd, netbsd, linux, solaris
  * for darwin -Cg is enabled by the compiler (see below)



See [this thread](<http://lists.freepascal.org/pipermail/fpc-pascal/2014-November/043159.html>) for more details. 

### fpc compiler

PIC code is on by default for 

  * for darwin, -Cg is always on by default on all (i386, x86-64, arm). Because it's also on by default in the system compilers (apple's gcc). Apple also requires any iOS code to be PIC (starting with iOS 7.0)



For other platforms it's off by default, because system native compiler has it off by default as well.

---

_Source: [https://wiki.freepascal.org/PIC_information](https://web.archive.org/web/20250115181432/https://wiki.freepascal.org/PIC_information)_
