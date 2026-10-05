# BeOS port

This page describes the current status of the BeOS/Haiku port of Free Pascal. 

## Current

Since FPC 2.4.0 there is official support for Haiku. The FPC release can be downloaded from [here](<http://www.freepascal.org/down/i386/haiku.var>). 

## In the past

Here are unofficial builds needed to compile Free Pascal on those platforms : 
    
    
    - BeOS : [fpc-2.2.1.i386-beos.tar.gz](<http://olivier.coursiere.free.fr/bootstrap/fpc-2.2.1.i386-beos.tar.gz>) 
    - Haiku : [fpc-2.2.1.i386-haiku.tar.gz](<http://olivier.coursiere.free.fr/bootstrap/fpc-2.2.1.i386-haiku.tar.gz>) 
    

Those compilers should compile "branches/fixes_2_2" and trunk. 

What is missing is a specific target for haiku to use the native ld included in the development package instead of a R5 one. This work is in progress. Until then, the native ld could be used by replacing 
    
    
    LinkRes.Add('elf_i386_be');
    

by 
    
    
    LinkRes.Add('elf_i386_haiku');
    

in compiler/systems/t_beos.pas when compiling under Haiku. 

  
The console mode IDE should work but compiling it with gdb support is more complicated (you will need to collect a lot of static libraries from the haiku build : libgdb.a and all its dependencies...). 

Here is a snapshot of fpc 2.3.1 for Haiku (revision 11134) : [fpc-2.3.1.i386-haiku.tar.gz](<http://olivier.coursiere.free.fr/download/fpc-2.3.1.i386-haiku.tar.gz>)

This snapshot contains the text mode IDE compiled with debugging support.

---

_Source: [https://wiki.freepascal.org/BeOS_port](https://web.archive.org/web/20241213021608/https://wiki.freepascal.org/BeOS_port)_
