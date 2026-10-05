# OS aware RTL

## The problem

Currently there's a runtime problem with binaries created by [FPC](<FPC.md> "FPC"). The problem lies in [libC](<libc_library.md> "libc library")/syscall numbers but it spans to userland as I'll explain later. 

I'll talk mostly about the Linux [RTL](<RTL.md> "RTL") since it's the most chaotic OS out there but this really applies to any [OS](<Operating_System.md> "Operating System") including Windows. 

  1. Syscall number changes
  2. LibC [functions](<Function.md> "Function") missing



It's a known fact that syscall numbers change. This happens on all unices out there more or less frequently. The problem with FPC RTL is that it's smartlinked into the binary. Although this is also a big advantage, the drawback is that things like syscall numbers are hardcoded. This means one of two things for ABI compatibility: 

a: Old syscalls will be used on new OSes if the binary was compiled on old ones  
b: Non-existing syscalls will be used on old OSes if the binary was compiled on new ones 

There's more: as an example I'll use the newly added epoll* functions in linux.pp. These are used as syscalls if {$FPC_USE_LIBC} is false. This way the binaries will compile even on 2.4 kernel where epoll doesn't exist. The problem comes if someone tries to create a libfprtl.so with {$FPC_USE_LIBC} on these systems (or use one from 2.6) since smartlinking cannot "hide" the missing functions anymore. 

## The proposal

Currently the only thing used to alleviate this is {$ifdef} and good luck. This won't last forever and if fpc wants to both use latest features and be backwards ABI compatible, something which I call "OS aware binaries" must be created. 

Lets say we change the unit which defines syscall numbers on Linux to something like this: (note: I know it's chopped up to include file but let's keep it simple) 
    
    
    unit
      Syscalls;
    
    interface
    
    var
      syscall_nr_epoll_create: Integer; // defined them as variables
      ...
    
    implementation
    
    uses
      Linux, Kernel24, Kernel26;
    
    initialization
      if VersionToInt(LinuxVersion()) > VersionToInt('2.5.66') then begin
        sycall_nr_epoll_create:=...;
      end else begin
        ...
      end;
    
    end.
    

Another solution which applies to the epoll/libC problem is this: 
    
    
    unit Linux;
    
    interface
    ...
    function epoll_create(size: cint): cint;
    ...
    implementation
    
    uses
      Syscalls, Epoll;
    
    var
      epoll_create_func: TEpoll_CreateFunction;
    
    function epoll_create(size: cint): cint;
    begin
      epoll_create_func(size);
    end;
    
    initialization
      if VersionToInt(LinuxVersion()) > VersionToInt('2.5.66') then
        epoll_create_func:=Epoll.epoll_create // assign as it's there
      else
        epoll_create_func:=@Epoll.epoll_create_dummy;
    end.
    

And epoll would look something like: 
    
    
    unit Epoll;
    
    interface
    
    type
      TEpollCreateFunction = function(cint): cint;
      ...
    
    function epoll_create_dummy(size: cint): cint;
    ...
    
    var
      epoll_create: TEpollCreateFuncion;
      ...
    
    implementation
    
    uses
      dl;
    
    {$ifndef FPC_USE_LIBC}
    function epoll_create_syscall(size: cint): cint;
    begin
      // do syscall stuff
    end;
    {$endif}
    
    function epoll_create_dummy(size: cint): cint;
    begin
      raise TOSMismatchException.Create; // we don't have this one here...
    end;
    
    initialization
    {$ifndef FPC_USE_LIBC}
      epoll_create:=dlopen();
    {$else}
      epoll_create:=@epoll_create_syscall;
    {$endif}
    
    end.
    

This should enable libfprtl.so to be compilable and linkable on any Linux version. Of course it should be compiled on latest possible so all "features" are there. 

Of course there are cons and pros, and inherited problems in this solution. 

The pros: 

  * ABI compatible binaries
  * Fixes certain future problems (epoll/FPC_USE_LIBC)
  * It would be just cool since FPC would be the only compiler with proper ABI compat. on linux



The cons: 

  * Overhead both memory and CPU (initialization slows down, smartlinking would be useless on these things)
  * Detection of OS version is more than tricky even with standardized POSIX.1 functions like uname
  * problem still only runtime detectable. (language exception instead of CPU detection)
  * more code, several interfaces (2.4, 2.6, libc) involved, bigger chance that one develops a problem and pulls down the whole binary. More code to maintain, bigger chance that it will get old and not fixed, and we have the same thing over again.



## Areas of use

As was stated before there are basically two major problems which would be solved if this solution was implementable. There are a few additional problems which fall under this category however. 

  1. Changes of Syscalls (changes also means new syscalls in recent versions)
  2. libC linking problems over platforms with missing syscalls/functions
  3. Use of effective syscalls. For example on linux 2.6 "oldselect" is currently used.



The areas where this approach is viable are limited and few luckily. Only new syscalls and changed syscalls should be addressed this way. Same goes for functions/units like winsock1 vs winsock2. It should be clear now that this is not a system-wide solution or abstraction layer, but a solution to specific areas in RTL. 

The question now is, how should FPC RTL address ABI issues? There are three known possible ways known to me: 

  1. Ignore new features/syscalls in OSes for the sake of compatibility.
  2. Ignore backwards compatibility and use only new features/syscalls.
  3. Try to stay compatible while using new features on new versions and vice versa.



The pros and cons of these are: 

  1. This is how it was done mostly in the unix world. Only common stuff was added to the RTL, units like <OSName> with OS/version specific stuff are rather new. This approach leads to use of old/obsolete syscalls which might result in FPC being unusable for highly optimized specialized code.
  2. This is even worse than #1. It brakes ABI compatibility and binaries will become "C-like". Smartlinking can alleviate this problem if RTL is linked in and users don't use special features, but it does not solve the "oldselect" problem.
  3. This is what this solution was meant to be. Problem is implementation. At the moment there's no known way of knowing OS versions for sure in unix.

---

_Source: [https://wiki.freepascal.org/OS_aware_RTL](https://web.archive.org/web/20250122071717/https://wiki.freepascal.org/OS_aware_RTL)_
