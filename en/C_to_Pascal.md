# C to Pascal

│ **English (en)** │  **[français (fr)](</C_to_Pascal/fr> "C to Pascal/fr")** │    
****

One of the weaknesses of Pascal compared to the C language, is a lower number of existing libraries. The reason for that is the much greater popularity of the C language compared to Pascal. Rather than rewrite the existing C libraries in Pascal, an easier alternative is to use the C libraries from Pascal. 

While Pascal is a high-level language, it's also "low-level" enough to interact with the very basic API functions used by the C language. Thus, using existing C-based libraries in Pascal programs is a common practice. Also, most of the popular operating systems and low-level APIs are written in and designed to be used by the C language. 

Each C language library comes with header files and binaries (static and/or dynamic library file(s)). In order to use the library, the header files need to be converted to Pascal and the binaries must be linked with the Pascal program. While usage of the library is resolved by the linker at compile time (static linking) or at runtime by the OS (dynamic linking), the task of converting a C header file to Pascal is the job of the programmer. However, there have been many utilities written in order to automate or semi-automate the task. 

Problems and solutions are explained in these good articles: 

  * [Creating bindings for C libraries](<Creating_bindings_for_C_libraries.md> "Creating bindings for C libraries")
  * [Common problems when converting C header files](<Common_problems_when_converting_C_header_files.md> "Common problems when converting C header files")
  * [Pascal for C users](<Pascal_for_C_users.md> "Pascal for C users")
  * [Pitfalls of converting](<http://rvelthuis.de/articles/articles-convert.html>)



  


## C to Pascal conversion tools and libraries

There might be much more found online (please add them to this list).  Name  | Notes  | Headers  | Source  | Precompiler  | C++  | ObjC  | Interface   
---|---|---|---|---|---|---|---  
[H2Pas](<H2Pas.md> "H2Pas") | FPC team official yylex-based tool to convert C-headers to Pascal. Issues could be reported on Mantis.  | Yes  | No  | somewhat  |  |  | command line or [H2Paswizard](<H2Paswizard.md> "H2Paswizard")  
[OpenC2Pas](<http://sourceforge.net/projects/c2pas/>) |  |  |  |  |  |  |   
[C2Pas32](<http://www.astonshell.com/freeware/c2pas32/>) | Freeware, but closed source solution  | Yes  | Yes  |  |  |  | command line   
[C To Pascal Converter](<http://cc.embarcadero.com/item/26951>) |  |  |  |  |  |  |   
[Dr. Bob's HeadConv](<http://www.drbob42.com/delphi/headconv.htm>) |  |  |  |  |  |  |   
[Rudy's Conversion Helper Package](<http://rvelthuis.de/programs/convertpack.html>) | On [the site](<http://rvelthuis.de/articles/>) a lot of useful information can be found.  |  |  |  |  |  |   
[C-To-Delphi](<https://github.com/WouterVanNifterick/C-To-Delphi>) | This tool will convert most of your standard C code. It contains a split view, with C on the left and Delphi on the right. The Delphi code gets updated in real time when you edit the C code. The editors are kept in sync, so you can easily find how a specific piece of code was converted.  | Yes  | Yes  |  | No  | No  | GUI   
[Chet](<https://github.com/neslib/Chet>) | Chet uses the Clang compiler to parse header files, resulting in more accurate translations that require fewer manual adjustments.  | Yes  | Yes  | Yes  | No  | No  | GUI   
[Chelper](<Chelper.md> "Chelper") | The project started as ObjC converter, thus it does understand ObjC notation. Though development is stalled a few years ago  | Yes  | No  | somewhat - no expressions  | No  | Yes  | command line, GUI as Lazarus Plugin   
[ToPas C-to-Pascal](<http://sourceforge.net/p/topas>) | The original author is Hans-Peter Diettrich  |  |  |  |  |  |   
[CToPas](<https://github.com/DrDiettrich/CtoPas/>) | The latest one and new implementation by Dr Diettrich  | Yes  |  |  |  |  | GUI   
[CtoPas](<http://sourceforge.net/projects/ctopas/files/>) |  | Yes  | Yes  |  |  |  | GUI   
[Objective Pascal Framework Parser](<http://www.objectivepascal.com/parser/index.html>) | PHP written tool is used by FPC team for Cocoa and iOS bindings. Was designed to handle ObjC specifically  | Yes  | No  | OSX versions only  | No  | Yes  | command line   
[SWIG](<SWIG.md> "SWIG") | Non pascal based solution; aimed at generating bindings for C++  | Yes  | Yes  |  | Yes  |  |

---

_Source: [https://wiki.freepascal.org/C_to_Pascal](https://web.archive.org/web/20240920204107/https://wiki.freepascal.org/C_to_Pascal)_
