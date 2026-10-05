# libc library

Not to be confused with [libc Kylix compatibility unit](<libc_unit.md> "libc unit")

## Linking

<http://forum.lazarus.freepascal.org/index.php/topic,44784.msg315189.html#new>

    **Question:**

According to <https://www.freepascal.org/docs-html/current/prog/progsu46.html>

I have to use {$LINKLIB c} in my Pascal source file, this works as expected, the binary links correcly and the resulting program runs as expected, however the documentation mentions Quote 

The same can be obtained by removing the linklib directive in the above unit, and specify -k-lc on the command line: 
    
    
    ppc386 -k-lc foo.pp 
    

This does not give me the same results. The program appears to link correctly, but the resulting binary refuses to run. 

    **Answer:**

{$linklib c} is indeed the only correct way, and this is an error in the manual. 

-k-lc just passes the "-lc" command line parameter to the linker, without the compiler having any clue about what it does. This means that the C library will be linked, but nothing else gets changed. 

{$linklib c} additionally also makes the compiler replace the initialisation code of the library (or program) with a version that initialises the C library (since by default FPC programs and libraries do not use the C library on most platforms, it cannot contain any code to initialise it either). Without this initialisation, the C library indeed won't work correctly. 

## See Also

  * <https://en.wikipedia.org/wiki/C_standard_library>

---

_Source: [https://wiki.freepascal.org/libc_library](https://web.archive.org/web/20250209005846/https://wiki.freepascal.org/libc_library)_
