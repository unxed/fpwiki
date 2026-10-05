# macOS Dynamic Libraries

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
  * 2 Library extensions and prefixes
  * 3 Example FPC dynamic library
  * 4 Universal 32 bit and 64 bit dynamic library
  * 5 Example application to load FPC dynamic library
  * 6 Alternative example application to load FPC dynamic library
  * 7 Additional steps using a Lazarus project
  * 8 Example application to load libc dynamic library
  * 9 Observe the dynamic linker in action
  * 10 Standard locations for dynamic libraries
  * 11 Logging dynamic loader events
  * 12 See also
  * 13 External links



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

## Example FPC dynamic library

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** If you use "cdecl", FPC ensures that your function completely adheres to all ABI requirements (naming, parameter passing, etc). If you don't, then you are on your own.

test.pas: 
    
    
    library TestLibrary;
      
    {$mode objfpc} {$H+}
    
    uses
      // needed for UpperCase
      SysUtils;     
    
    // library subroutine
    function cvtString(strIn : string) : PChar; cdecl;
      begin
        cvtString := PChar(UpperCase(strIn));
      end;
    
    // exported subroutine(s)
    exports
      cvtString;
    end.
    

Compile: 
    
    
     fpc test.pas
    

which produces the dynamic library file named libtest.dylib. 

## Universal 32 bit and 64 bit dynamic library

If needed to support both 32 bit and 64 bit architectures, `test.pas` can be compiled as a universal dynamic library that contains both 32 bit and 64 bit libraries: 
    
    
    ppc386 test.pas
    ppcx64 -olibtest64.dylib test.pas
    lipo -create libtest.dylib libtest64.dylib -output libtest.dylib
    

To check that both architectures have been included in the dynamic library: 
    
    
    $ file libtest.dylib
    libtest.dylib: Mach-O universal binary with 2 architectures: [i386:Mach-O dynamically linked shared library i386] [x86_64:Mach-O 64-bit dynamically linked shared library x86_64]
    libtest.dylib (for architecture i386):	Mach-O dynamically linked shared library i386
    libtest.dylib (for architecture x86_64):	Mach-O 64-bit dynamically linked shared library x86_64
    

Too easy! 

## Example application to load FPC dynamic library

This version of the example application loads the dynamic library on demand when necessary (load-time dynamic linking aka dynamic loading) and can also unload it when it is no longer necessary. You can successfully compile the application even if the dynamic library does not exist. 

The alternative example version that follows loads the dynamic library at start-up (run-time dynamic linking aka dynamic linking) and cannot unload it until the application quits. You cannot successfully compile the application unless the dynamic library exists. 

dynlibdemo.pas: 
    
    
    Program dynlibdemo;
      
    {$mode objfpc}{$H+}
    
    uses
       Dynlibs,
       SysUtils;
    
    type
       // definition of the subroutine to be called as defined in the dynamic library to be loaded
       TcvtString = function(strToConvert : string) : PChar;  cdecl;
    
    var
       // create suitable variable for the dynamic library subroutine
       cvtString : TcvtString;
    
       // create a handle for the dynamic library
       LibHandle : TLibHandle;
    
    begin
       // load and get the dynamic library handle
       LibHandle := LoadLibrary(PChar('libtest.dylib'));
    
       // check whether loading was successful
       if LibHandle <> 0 then
         begin
           // assign address of the subroutine call to the variable cvtString
           Pointer(cvtString) := GetProcAddress(LibHandle, 'cvtString');
    
           // check whether a valid address has been returned
           if @cvtString <> nil then
             WriteLn(cvtString('hello world'))
           // error message on no valid address
           else
             WriteLn('GetLastOSError1 = ', SysErrorMessage(GetLastOSError));
         end
       else
         // error message on load failure
         WriteLn('GetLastOSError2 = ', SysErrorMessage(GetLastOSError));
    
       // release memory
       cvtString := nil;
    
       // unload library
       if LibHandle <> NilHandle then
         UnloadLibrary(LibHandle);
    
       LibHandle := NilHandle;
    end.
    

Compile: 
    
    
     fpc dynlibdemo.pas
    

Run: 
    
    
    $ ./dynlibdemo
    HELLO WORLD
    

## Alternative example application to load FPC dynamic library

This version of the example application loads the dynamic library at startup (run-time dynamic linking aka dynamic linking) and cannot unload it. You cannot successfully compile the application unless the dynamic library exists. 

The previous version of the example application loads the dynamic library when necessary (load-time dynamic linking aka dynamic loading) and can also unload it when it is no longer necessary. You can successfully compile the application even if the dynamic library does not exist. 

alt_dynlibdemo.pas: 
    
    
    {$linklib libtest}    
    
    program alt_dynlibdemo;
    
    {$mode objfpc} {$H+}
    
    function cvtString(const strToConvert: string): PChar; cdecl; external;
    
    begin
      WriteLn(cvtString('hello world'));
    end.
    

Compile: 
    
    
     fpc alt_dynlibdemo.pas
    

Run: 
    
    
    $ ./alt_dynlibdemo
    HELLO WORLD
    

If you think the code looks like it statically links the library, you can verify that it does not by moving the library or renaming it and then re-running the demo application which produces output similar to this: 
    
    
    $ ./alt_dynlibdemo 
    dyld: Library not loaded: /Users/[user]/fpc_dynamic_lib2/libtest.dylib
      Referenced from: /Users/[user]/fpc_dynamic_lib2/./alt_dynlibdemo
      Reason: image not found
    Abort
    

## Additional steps using a Lazarus project

If you turn the pure FPC dynlibdemo.pas example above into a Lazarus project named project1 your unit should resemble: 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Dynlibs,
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls;
    
    type
       // definition of the subroutine to be called as defined in the dynamic library to be loaded
       TcvtString = function(strToConvert : string) : PChar;  cdecl;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
       // create suitable variable for the dynamic library subroutine
       cvtString : TcvtString;
       // create a handle for the dynamic library
       LibHandle : TLibHandle;
    begin
      // load and get the dynamic library handle
      LibHandle := LoadLibrary(PChar('libtestlibrary.dylib'));
    
      // check whether loading was successful
      if LibHandle <> 0 then
        begin
          // assign address of the subroutine call to the variable cvtString
          Pointer(cvtString) := GetProcAddress(LibHandle, 'cvtString');
    
          // check whether a valid address has been returned
          if @cvtString <> nil then
            ShowMessage(cvtString('hello world'))
          // error message on no valid address
          else
            ShowMessage('GetLastOSError1 = ' + SysErrorMessage(GetLastOSError));
        end
      else
        // error message on load failure
        ShowMessage('GetLastOSError2 = ' + SysErrorMessage(GetLastOSError));
    
      // release memory
      cvtString := nil;
    
      // unload library
      if LibHandle <> NilHandle then
        UnloadLibrary(LibHandle);
    
      LibHandle := NilHandle;
    end;
    
    end.
    

After creating an [application bundle](<Application_Bundle.md> "Application Bundle") then you need to: 

  * Open a Terminal (Applications > Utilities > Terminal)
  * Delete the `project1.app/Contents/MacOS/project1` symbolic link to the project1 executable file
  * Copy the project1 executable file into the `project1.app/Contents/MacOS` directory
  * Copy libtest.dylib into the `project1.app/Contents/Frameworks` directory (you need to create the Frameworks directory)
  * Change into the `project1.app/Contents/MacOS` directory
  * Either: 
    * Enter: `install_name_tool -add_rpath "@executable_path/../Frameworks/." project1` **=or=**
    * In the Lazarus IDE go to Project > Project Options > Compiler Options > Compilation and Linking and there you will have to check the option "Pass options to linker with -k, delimiter is space" and enter the following: `-rpath @executable_path/../Frameworks` (Do NOT include -k which Lazarus adds for FPC automatically).



This ensures that the project1 executable in your application bundle will look in your application bundle's Frameworks directory to find your dynamic library. 

For more information on the install_name_tool command line utility, open a Terminal and enter: 
    
    
     man install_name_tool
    

For more information on the linker command line utility, open a Terminal and enter: 
    
    
     man ld
    

## Example application to load libc dynamic library

demo.pas 
    
    
    {$mode objfpc}
    {$linklib C} 
     
    // Declarations for the standard C functions strlen  and toupper  
    function strlen (P : pchar) : longint; cdecl; external;  
    function toupper(P : integer) : integer; cdecl; external;
      
    begin  
      Write('"Programming is fun!" is ');
      Write(strlen('Programming is fun!'));
      WriteLn(' characters long.');
      WriteLn('Before: "c" and after "' + chr(toupper(ord('c'))) + '".');
    end.
    

Compile: 
    
    
     fpc demo.pas
    

Run: 
    
    
     $./demo
     "Programming is fun!" is 19 characters long.
     Before: "c" and after "C".
    

Be aware that, unlike for example FreeBSD and Linux, there is no static libc library on macOS. Instead, there is the dynamic system library (`/usr/lib/libSystem.dylib`) which includes the following libraries: 

  * libc 
    * The standard C library. This library contains the functions used by C programmers on all platforms.
  * libinfo 
    * The NetInfo library.
  * libkvm 
    * The kernel virtual memory library.
  * libm 
    * The math library, which contains arithmetic functions.
  * libpthread 
    * The POSIX threads library, which allows multiple tasks to run concurrently within a single program.
  * libutil 
    * The library that provides functions related to login, logout, terminal assignment, and logging.



There are symbolic links in `/usr/lib` for all of these libraries pointing to `/usr/lib/libSystem.dylib` **except** libutil. 

## Observe the dynamic linker in action

If you want to observe the dynamic linker in action, you can use the command line utility `dtruss`. Open a Terminal and run: 
    
    
    $ sudo dtruss ./dynlibdemo
    

which will produce output similar to: 
    
    
    SYSCALL(args) 		 = return
    HELLO WORLD
    ...
    stat64("libtest.dylib\0", 0x7FFEE96A3A20, 0x0)		 = 0 0
    open("libtest.dylib\0", 0x0, 0x0)		 = 3 0
    ...
    fcntl(0x3, 0x62, 0x7FFEE969B1B0)		 = 0 0
    mmap(0x106726000, 0x84000, 0x5, 0x12, 0x3, 0x0)		 = 0x106726000 0
    mmap(0x1067AA000, 0x1D000, 0x3, 0x12, 0x3, 0x84000)		 = 0x1067AA000 0
    mmap(0x1067C9000, 0x2A28, 0x1, 0x12, 0x3, 0xA1000)		 = 0x1067C9000 0
    fcntl(0x3, 0x32, 0x7FFEE969B440)		 = 0 0
    close(0x3)		 = 0 0
    ...
    

It is even more instructive to temporarily rename your dynamic library so it will not be found. This will produce output similar to: 
    
    
    ...
    stat64("libtest.dylib\0", 0x7FFEE00E95E0, 0x0)		 = -1 Err#2
    stat64("libtest.dylib\0", 0x7FFEE00E9A20, 0x0)		 = -1 Err#2
    stat64("/Users/[user]/lib/libtest.dylib\0", 0x7FFEE00E9990, 0x0)		 = -1 Err#2
    stat64("/Users/[user]/lib/libtest.dylib\0", 0x7FFEE00E9DD0, 0x0)		 = -1 Err#2
    stat64("/\0", 0x7FFEE00E7D78, 0x0)		 = 0 0
    getattrlist("/Users\0", 0x117102048, 0x7FFEE00E96D0)		 = 0 0
    getattrlist("/Users/[user]\0", 0x117102048, 0x7FFEE00E96D0)		 = 0 0
    getattrlist("/Users/[user]/lib\0", 0x117102048, 0x7FFEE00E96D0)		 = -1 Err#2
    stat64("/Users/[user]/lib\0", 0x7FFEE00E9560, 0x0)		 = -1 Err#2
    stat64("/Users/[user]/lib\0", 0x7FFEE00E99A0, 0x0)		 = -1 Err#2
    stat64("/usr/local/lib/libtest.dylib\0", 0x7FFEE00E9990, 0x0)		 = -1 Err#2
    stat64("/usr/local/lib/libtest.dylib\0", 0x7FFEE00E9DD0, 0x0)		 = -1 Err#2
    getattrlist("/usr\0", 0x117102048, 0x7FFEE00E96D0)		 = 0 0
    getattrlist("/usr/local\0", 0x117102048, 0x7FFEE00E96D0)		 = 0 0
    getattrlist("/usr/local/lib\0", 0x117102048, 0x7FFEE00E96D0)		 = 0 0
    getattrlist("/usr/local/lib/libtest.dylib\0", 0x117102048, 0x7FFEE00E96D0)		 = -1 Err#2
    stat64("/usr/local/lib/libtest.dylib\0", 0x7FFEE00E9560, 0x0)		 = -1 Err#2
    stat64("/usr/local/lib/libtest.dylib\0", 0x7FFEE00E99A0, 0x0)		 = -1 Err#2
    stat64("/usr/lib/libtest.dylib\0", 0x7FFEE00E99A0, 0x0)		 = -1 Err#2
    stat64("/usr/lib/libtest.dylib\0", 0x7FFEE00E9DE0, 0x0)		 = -1 Err#2
    getattrlist("/usr\0", 0x117102048, 0x7FFEE00E96E0)		 = 0 0
    getattrlist("/usr/lib\0", 0x117102048, 0x7FFEE00E96E0)		 = 0 0
    getattrlist("/usr/lib/libtest.dylib\0", 0x117102048, 0x7FFEE00E96E0)		 = -1 Err#2
    stat64("/usr/lib/libtest.dylib\0", 0x7FFEE00E9570, 0x0)		 = -1 Err#2
    stat64("/usr/lib/libtest.dylib\0", 0x7FFEE00E99B0, 0x0)		 = -1 Err#2
    open(".\0", 0x0, 0x1)		 = 3 0
    fstat64(0x3, 0x7FFEE00E7CF0, 0x0)		 = 0 0
    fcntl(0x3, 0x32, 0x7FFEE00E9CB0)		 = 0 0
    close(0x3)		 = 0 0
    stat64("/Users/[user]/LAZPROJECTS/fpc_dynamic_lib\0", 0x7FFEE00E7C60, 0x0)		 = 0 0
    stat64("/Users/[user]/LAZPROJECTS/fpc_dynamic_lib\0", 0x7FFEE00E7EF8, 0x0)		 = 0 0
    getattrlist("/Users/[user]/LAZPROJECTS/fpc_dynamic_lib/libtest.dylib\0", 0x117102048, 0x7FFEE00E9850)		 = -1 Err#2
    stat64("/Users/[user]/LAZPROJECTS/fpc_dynamic_lib/libtest.dylib\0", 0x7FFEE00E96E0, 0x0)		 = -1 Err#2
    stat64("/Users/[user]/LAZPROJECTS/fpc_dynamic_lib/libtest.dylib\0", 0x7FFEE00E9B20, 0x0)		 = -1 Err#2
    ...
    

The highlighted lines above show all the places the the system looked for the missing dynamic library. 

## Standard locations for dynamic libraries

While in the above example we stored the dynamic library in the application bundle, the standard locations for dynamic libraries are `~/lib` (for single user) and `/usr/local/lib` (for multiple users). There used to also be `/usr/lib`, but that was locked-down in macOS 10.15 (Catalina) and later on the system read-only volume. You need to use the standard locations if you want to share the dynamic library among multiple applications. 

If you must place the dynamic library file in a non-standard location in the file system, you then must also add that location to one of these environment variables: 

  * LD_LIBRARY_PATH
  * DYLD_LIBRARY_PATH
  * DYLD_FALLBACK_LIBRARY_PATH



Note: You cannot distribute an application through the Mac App Store that depends on a custom library being installed first by the user into `/usr/local/lib` so it is best to store your dynamic library or libraries in the application bundle. 

For applications with a graphical user interface it not only makes sense to include any needed libraries in your application's Frameworks folder, Apple encourages it. 

## Logging dynamic loader events

As you develop and use dynamic libraries, you may want to know when certain events occur. For example, you want to know when the dynamic loader binds a particular undefined external symbol or how long it took for an application to launch. Environment variables to the rescue. The environment variables that affect dynamic loader logging are: 

Environment variable | Description   
---|---  
DYLD_PRINT_LIBRARIES | Logs when images are loaded.   
DYLD_PRINT_LIBRARIES_POST_LAUNCH | Logs when images are loaded as a result of a dlopen call. Includes a dynamic libraries’ dependent libraries.   
DYLD_PRINT_APIS | Logs the invocation that causes the dynamic loader to return the address of a symbol.   
DYLD_PRINT_STATISTICS | Logs statistical information on an application’s launch process, such as how many images were loaded, when the application finishes launching.   
DYLD_PRINT_INITIALIZERS | Logs when the dynamic loader calls initializer and finalizer functions.   
DYLD_PRINT_SEGMENTS | Logs when the dynamic loader maps a segment of a dynamic library to the current process’s address space.   
DYLD_PRINT_BINDINGS | Logs when the dynamic loader binds an undefined external symbol with its definition.   
  
For example, Open an Applications > Utilities > Terminal - note the shell being used below is `tcsh`: 
    
    
    $ setenv DYLD_PRINT_STATISTICS
    $ ./my_app
    Total pre-main time:  13.41 milliseconds (100.0%)
            dylib loading time:   6.39 milliseconds (47.6%)
           rebase/binding time:   0.97 milliseconds (7.2%)
               ObjC setup time:   0.46 milliseconds (3.4%)
              initializer time:   5.40 milliseconds (40.2%)
              slowest intializers :
                libSystem.B.dylib :   1.19 milliseconds (8.9%)
                  libobjc.A.dylib :   0.47 milliseconds (3.5%)
                   CoreFoundation :   2.35 milliseconds (17.5%)
                           AppKit :   0.61 milliseconds (4.6%)
    $ unsetenv DYLD_PRINT_STATISTICS
    

For the default macOS shell `zsh`, use `export DYLD_PRINT_STATISTICS=1` and `unset DYLD_PRINT_STATISTICS` to set and unset an environment variable. 

## See also

  * [Application Bundle](<Application_Bundle.md> "Application Bundle") \- structure of a macOS application


  * [macOS Static Libraries](<macOS_Static_Libraries.md> "macOS Static Libraries") for the same FPC library being used as a static library.


  * [macOS Frameworks](<macOS_Frameworks.md> "macOS Frameworks") \- structured directories for dynamic libraries and associated resources.


  * [macOS Libraries](<macOS_Programming_Tips.md> "macOS Programming Tips").


  * [FPC Shared Libraries](<shared_library.md> "shared library").


  * [Big Sur's built-in dynamic linker cache](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers").



## External links

  * [Apple Documentation: Dynamic Library Programming Topics](<https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/DynamicLibraries/000-Introduction/Introduction.html#//apple_ref/doc/uid/TP40001908-SW1>).

---

_Source: [https://wiki.freepascal.org/macOS_Dynamic_Libraries](https://web.archive.org/web/20250119103455/https://wiki.freepascal.org/macOS_Dynamic_Libraries)_
