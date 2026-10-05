# macOS Static Libraries

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
  * 2 Library extensions and prefixes
  * 3 Example FPC static library
  * 4 Example FPC application to use FPC static library
  * 5 Example C static library
  * 6 Example FPC application to use C static library
  * 7 See also



## Overview

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** **Dynamic libraries** are also known as dynamic shared libraries, shared objects, or dynamically linked libraries.  


**Static libraries** are also known as static archive libraries and static linked shared libraries.

Most of an application's functionality is implemented in libraries of executable code. When an application's source code is compiled into object code and linked with a static library, the object code and library code that the application uses is copied into the executable file that is loaded into memory in its entirety at launch time. The kind of library that becomes part of an application's executable is known as a static library. Static libraries are collections or archives of object files. 

There are two important factors which determine the performance of applications: their launch times and their memory footprints. Reducing the size of an executable file and minimizing its memory use once launched make an application launch faster and use less memory. Using dynamic libraries instead of static libraries reduces the executable file size of an application. Dynamic libraries also allow applications to delay loading libraries with special functionality until they’re needed instead of loading them at launch time. This feature contributes further to reduced launch times and efficient memory use. Another reason to use dynamic libraries is so that you can share code among multiple applications thereby saving the memory (and to a lesser extent nowadays, disk space) that would otherwise be used for multiple copies of the library code. 

There are, however, some advantages to statically linking libraries with an executable instead of dynamically linking them. The most significant advantage is that the application can be certain that all its libraries are present and that they are the correct version. Static linking of libraries also allows the application to be contained in a single executable file, simplifying distribution and installation. Also with static linking, only those parts of the library that are directly and indirectly referenced by the target executable are included in the executable. With dynamic libraries, the entire library is loaded, as it is not known in advance which functions will be used by the application. Whether this advantage is significant in practice depends on the structure of the library. 

## Library extensions and prefixes

Operating System | Dynamic library | Static library | Library prefix   
---|---|---|---  
FreeBSD | .so | .a | lib   
macOS | .dylib | .a | lib   
Linux | .so | .a | lib   
Windows | .dll | .lib | n/a   
  
The library prefix column indicates how the names of the libraries are resolved and created. Under macOS, the library name will always have the lib prefix when it is created. So if you create a dynamic library called test, this will result in the file libtest.dylib. When importing routines from shared libraries, it is not necessary to give the library prefix or the filename extension. 

## Example FPC static library

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** If you use "cdecl", FPC ensures that your function completely adheres to all ABI requirements (naming, parameter passing, etc). If you don't, then you are on your own.

test.pas: 
    
    
    {$mode objfpc} {$H+}
    
    
    // Note this is a unit and not a library
    unit test;
    
    interface 
    
    uses
      // needed for UpperCase
      SysUtils;     
    
    implementation
    
    // library subroutine
    function cvtString(strIn : string) : PChar; Cdecl;
    begin
      cvtString := PChar(UpperCase(strIn));
    end;
    
    end.
    

Compile: 
    
    
     fpc test.pas
    

which produces an object file named `test.o` which we will now convert into a static library archive named `libtest.a` using the `libtool` command line utility: 
    
    
     libtool -static -o libtest.a test.o
    

## Example FPC application to use FPC static library

statlibdemo.pas: 
    
    
    {$mode objfpc} {$H+}
    
    // Link in our static library
    // - note the prefix (lib) and extension (.a) are not necessary
    {$LinkLib test}
    
    program statlibdemo;
    
    // Declare our function
    function cvtString(strIn : string) : PChar; Cdecl; External Name 'cvtString';
    
    begin
      WriteLn(cvtString('hello world'));
    end.
    

Compile: 
    
    
     fpc statlibdemo.pas
    

which unexpectedly produces: 
    
    
    Free Pascal Compiler version 3.3.1 [2020/04/20] for x86_64
    Copyright (c) 1993-2020 by Florian Klaempfl and others
    Target OS: Darwin for x86_64
    Compiling staticlibdemo.pas
    Assembling (pipe) staticlibdemo.s
    Linking staticlibdemo
    Undefined symbols for architecture x86_64:
      "_cvtString", referenced from:
          _PASCALMAIN in staticlibdemo.o
    ld: symbol(s) not found for architecture x86_64
    An error occurred while linking 
    staticlibdemo.pas(11,34) Error: Error while linking
    staticlibdemo.pas(11,34) Fatal: There were 1 errors compiling module, stopping
    Fatal: Compilation aborted
    Error: /usr/local/bin/ppcx64 returned an error exitcode
    

What happened? Why wasn't our library function `cvtString` found? Let's look for our function in the `test.o` object file by running the `objdump` command line utility: 
    
    
    $ objdump -d test.o
    test.o:	file format Mach-O 64-bit x86-64
    
    Disassembly of section __TEXT,__text:
    _TEST_$$_CVTSTRING$ANSISTRING$$PCHAR:
           0:	55 	pushq	%rbp
           1:	48 89 e5 	movq	%rsp, %rbp
           4:	48 8d 64 24 80 	leaq	-128(%rsp), %rsp
           9:	48 89 7d f8 	movq	%rdi, -8(%rbp)
           d:	48 8b 7d f8 	movq	-8(%rbp), %rdi
          11:	e8 00 00 00 00 	callq	0 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x16>
          16:	48 c7 45 88 00 00 00 00 	movq	$0, -120(%rbp)
          1e:	48 8d 55 d8 	leaq	-40(%rbp), %rdx
          22:	48 8d 75 98 	leaq	-104(%rbp), %rsi
          26:	bf 01 00 00 00 	movl	$1, %edi
          2b:	e8 00 00 00 00 	callq	0 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x30>
          30:	48 89 c7 	movq	%rax, %rdi
          33:	e8 00 00 00 00 	callq	0 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x38>
          38:	48 63 d0 	movslq	%eax, %rdx
          3b:	48 89 55 90 	movq	%rdx, -112(%rbp)
          3f:	83 f8 00 	cmpl	$0, %eax
          42:	75 22 	jne	34 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x66>
          44:	48 8b 75 f8 	movq	-8(%rbp), %rsi
          48:	48 8d 7d 88 	leaq	-120(%rbp), %rdi
          4c:	e8 00 00 00 00 	callq	0 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x51>
          51:	48 8b 45 88 	movq	-120(%rbp), %rax
          55:	48 83 f8 00 	cmpq	$0, %rax
          59:	75 07 	jne	7 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x62>
          5b:	48 8b 05 00 00 00 00 	movq	(%rip), %rax
          62:	48 89 45 f0 	movq	%rax, -16(%rbp)
          66:	e8 00 00 00 00 	callq	0 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x6b>
          6b:	48 8d 7d f8 	leaq	-8(%rbp), %rdi
          6f:	e8 00 00 00 00 	callq	0 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x74>
          74:	48 8d 7d 88 	leaq	-120(%rbp), %rdi
          78:	e8 00 00 00 00 	callq	0 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x7d>
          7d:	48 8b 45 90 	movq	-112(%rbp), %rax
          81:	48 83 f8 00 	cmpq	$0, %rax
          85:	74 0f 	je	15 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x96>
          87:	e8 00 00 00 00 	callq	0 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x8c>
          8c:	48 c7 45 90 00 00 00 00 	movq	$0, -112(%rbp)
          94:	eb d0 	jmp	-48 <_TEST_$$_CVTSTRING$ANSISTRING$$PCHAR+0x66>
          96:	48 8b 45 f0 	movq	-16(%rbp), %rax
          9a:	48 89 ec 	movq	%rbp, %rsp
          9d:	5d 	popq	%rbp
          9e:	c3 	retq
    

Aha! Our function name has been well and truly mangled. It is obvious we need to use the mangled name `TEST_$$_CVTSTRING$ANSISTRING$$PCHAR` (omit the underscore as the linker will automatically add it) rather than the unmangled `cvtString`. Our example application now looks like: 
    
    
    {$mode objfpc} {$H+}
    
    // Link in our static library
    // - note the prefix (lib) and extension (.a) are not necessary
    {$LinkLib test}
    
    program statlibdemo;
    
    // Declare our function
    function cvtString(strIn : string) : PChar; Cdecl; External Name 'TEST_$$_CVTSTRING$ANSISTRING$$PCHAR';
    
    begin
      WriteLn(cvtString('hello world'));
    end.
    

Compile: 
    
    
     fpc statlibdemo.pas
    

which again dashes our hopes and produces: 
    
    
    Free Pascal Compiler version 3.3.1 [2020/04/20] for x86_64
    Copyright (c) 1993-2020 by Florian Klaempfl and others
    Target OS: Darwin for x86_64
    Compiling staticlibdemo.pas
    Assembling (pipe) staticlibdemo.s
    Linking staticlibdemo
    Undefined symbols for architecture x86_64:
      "_SYSUTILS_$$_UPPERCASE$ANSISTRING$$ANSISTRING", referenced from:
          _TEST_$$_CVTSTRING$ANSISTRING$$PCHAR in libtest.a(test.o)
    ld: symbol(s) not found for architecture x86_64
    An error occurred while linking 
    staticlibdemo.pas(11,34) Error: Error while linking
    staticlibdemo.pas(11,34) Fatal: There were 1 errors compiling module, stopping
    Fatal: Compilation aborted
    Error: /usr/local/bin/ppcx64 returned an error exitcode
    

Unfortunately we are missing the object file for the `UpperCase` function which was pulled in from the FPC SysUtils unit. What to do? Simply add the SysUtils unit to the the Uses clause of the application. Our example application now looks like: 
    
    
    {$mode objfpc} {$H+}
    
    // Link in our static library
    // - note the prefix (lib) and extension (.a) are not necessary
    {$LinkLib test}
    
    program statlibdemo;
    
    // Needed for linking in the UpperCase function
    Uses
      SysUtils;
    
    // Declare our function
    function cvtString(strIn : string) : PChar; Cdecl; External Name 'TEST_$$_CVTSTRING$ANSISTRING$$PCHAR';
    
    begin
      WriteLn(cvtString('hello world'));
    end.
    

Third time lucky, compile: 
    
    
     fpc statlibdemo.pas
    

Run: 
    
    
    $ ./statlibdemo
    HELLO WORLD
    

## Example C static library

myfunc1.c 
    
    
    #include <stdio.h>
    
    void myfunc1() 
      {
      printf("This is my func1!\n");
      }
    

myfunc2.c 
    
    
    #include <ctype.h>
    
    int myfunc2(int c) 
      {
      c = toupper(c);
      return(c);
      }
    

Compile: 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you do not specify the minimum macOS version, the C compiler will compile for your current operating system version, but your Free Pascal Compiler will compile for 10.5 (FPC 3.0.4) or 10.8 (FPC 3.2.0 and later) by default.
    
    
    cc -c -mmacosx-version-min=10.8 myfunc1.c myfunc2.c
    

which produces object files named myfunc1.o and myfunc2.o which we will now convert into a static library archive named `libmyfuncs.a` using the `libtool` command line utility: 
    
    
    libtool -static -o libmyfuncs.a myfunc1.o myfunc2.o
    

## Example FPC application to use C static library

statlibdemo.pas: 
    
    
    {$mode objfpc}
    {$linklib myfuncs}
    
    procedure myfunc1; cdecl; external;
    function myfunc2(c : integer) : integer; cdecl; external;
    
    begin
      myfunc1;
    
      write('This is my func2 - Before: "b"  ');
      writeLn('After : ' + '"' + chr(myfunc2(ord('b'))) + '"');
    end.
    

Compile: 
    
    
    fpc statlibdemo.pas
    

Run: 
    
    
    $ ./statlibdemo
    This is my func1!
    This is my func2 - Before: "b"  After : "B"
    

If you need to check whether your external routines have been linked into the final application, you can use the `objdump` command line utility: 
    
    
    $ objdump -d staticlibdemo | grep -A13 ^_myfunc
    _myfunc1:
    100000830:	55 	pushq	%rbp
    100000831:	48 89 e5 	movq	%rsp, %rbp
    100000834:	48 83 ec 10 	subq	$16, %rsp
    100000838:	48 8d 3d 7f 44 02 00 	leaq	148607(%rip), %rdi
    10000083f:	b0 00 	movb	$0, %al
    100000841:	e8 e6 3d 02 00 	callq	146918 <dyld_stub_binder+0x10002462c>
    100000846:	89 45 fc 	movl	%eax, -4(%rbp)
    100000849:	48 83 c4 10 	addq	$16, %rsp
    10000084d:	5d 	popq	%rbp
    10000084e:	c3 	retq
    10000084f:	90 	nop
    
    _myfunc2:
    100000850:	55 	pushq	%rbp
    100000851:	48 89 e5 	movq	%rsp, %rbp
    100000854:	48 83 ec 10 	subq	$16, %rsp
    100000858:	89 7d fc 	movl	%edi, -4(%rbp)
    10000085b:	8b 7d fc 	movl	-4(%rbp), %edi
    10000085e:	e8 29 3e 02 00 	callq	146985 <dyld_stub_binder+0x10002468c>
    100000863:	89 45 fc 	movl	%eax, -4(%rbp)
    100000866:	8b 45 fc 	movl	-4(%rbp), %eax
    100000869:	48 83 c4 10 	addq	$16, %rsp
    10000086d:	5d 	popq	%rbp
    10000086e:	c3 	retq
    10000086f:	90 	nop
    

If you don't care for the assembly language, you can also use the `nm` command line utility: 
    
    
    nm staticlibdemo | grep myfunc
    0000000100000830 T _myfunc1
    0000000100000850 T _myfunc2
    

## See also

  * [macOS Dynamic Libraries](<macOS_Dynamic_Libraries.md> "macOS Dynamic Libraries") for the same FPC library being used as a dynamic library.
  * [macOS Frameworks](<macOS_Frameworks.md> "macOS Frameworks") \- structured directories for dynamic libraries and associated resources.
  * [macOS Libraries](<macOS_Programming_Tips.md> "macOS Programming Tips").

---

_Source: [https://wiki.freepascal.org/macOS_Static_Libraries](https://web.archive.org/web/20250401031759/https://wiki.freepascal.org/macOS_Static_Libraries)_
