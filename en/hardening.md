# hardening

**Hardening - PIE or PIC - applies to Linux**. PIE Hardening applies also to Windows, and its probably useful on Windows. The same FPC command line switches apply on Windows. 

## Overview

For our purpose here, "hardening" means making your binary harder to hack while running. Key is [PIE](<https://en.wikipedia.org/wiki/Position-independent_code>), Position Independent Executable. FPC supports PIE with a couple of options on most platforms. Some environments require it and it makes good sense, especially in a server application. Its suggested that PIE Hardening increases the size of an executable and slows it down marginally. 

Most projects only require the following extra FPC command line switches - 
    
    
    -Cg -k-pie -znow
    

However, there are some corner cases - 

  * Lazarus application will probably already have the "-Cg" in its compile line.
  * Small, command line applications that would generate a **statically linked binary** will compile fine but not run ("No such file or directory"). The difficulty is that FPC does not bother to embed instructions about portable linking in such a binary because it appears unnecessary. The solution is to add "{$linklib c}" without the inverted commas to your source. This requests LibC be linked, forcing a dynamically linked binary and FPC provides the necessary information for the linker.
  * At present, late 2021, PIE Hardening does not seem to work on PowerPC64le systems, see [this bug report](<https://gitlab.com/freepascal.org/fpc/source/-/issues/39451>)
  * Some linux operating systems have a _file_ command that mentions the word 'pie' in its output when directed to a PIE Hardened binary, but some don't. But all recent OSs will, with a PIE Hardened binary, list something like _interpreter /lib64/ld-linux-x86-64.so.2_ \- that library must exist, if not, you have the _statically linked binary_ problem mentioned above.



  


## Example

Following is an example test file - 
    
    
    program Test;
    {$linklib c}
    begin
      writeln('test');
    end.
    

Compile with this - 
    
    
    fpc  -Cg  -k-pie  -k-znow  test.pas
    

And examine the result - 
    
    
    $> file test [enter]
    test: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.4.0, stripped
    

  


## See Also

  * [https://wiki.freepascal.org/Hardened_runtime_for_macOS](<Hardened_runtime_for_macOS.md>)
  * <https://wiki.debian.org/Hardening>
  * [https://wiki.freepascal.org/Debian_package_structure](<Debian_package_structure.md>)
  * This [gdb page](<GDB_Debugger_Tips.md>) mentions a problem debugging PIE.

---

_Source: [https://wiki.freepascal.org/hardening](https://web.archive.org/web/20250424004925/https://wiki.freepascal.org/hardening)_
