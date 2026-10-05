# Systems 2006

## Contents

  * 1 Planning
    * 1.1 Booth service
  * 2 CD
    * 2.1 Content
  * 3 FPC advantages
  * 4 Feature list
    * 4.1 FPC
    * 4.2 Lazarus
    * 4.3 Third party packages
    * 4.4 Missing
  * 5 Booth FAQ



### Planning

#### Booth service

  * Florian: prefered 23th to 24th or 25th, 26th and 27th not possible
  * Mattias: preferred 25th to 27th
  * Swen: 23rd to 27th possible



## CD

### Content

  * tgz source: fpc 2.0.4, lazarus 0.9.18
  * tgz source: fpc 2.0.4, lazarus 0.9.18
  * rpm i386: fpc 2.0.4, fpc-src, fpc-doc, lazarus 0.9.18
  * deb i386: fpc 2.0.4, fpc-src, fpc-doc, lazarus 0.9.18
  * dmg powerpc: fpc 2.0.4, fpc-src, lazarus 0.9.18
  * windows installer: fpc 2.0.4, fpc-src, lazarus 0.9.18
  * FreeBSD binary i386: fpc 2.0.4, fpc-src, lazarus 0.9.18
  * tgz source: fpc 2.1.1
  * Installation instructions
  * tools (e.g. Tortoise SVN)



## FPC advantages

  * Very clean language Pascal is a very nice language, your programs will be more readable and maintainable than for example in C, and let's even forget about C++. And you don't need to give up the power, a modern Pascal language is as powerful as you want it.
  * No Makefiles Unlike most programming languages, Pascal does not need Makefiles. You can save huge amounts of time, the compiler just figures out itself which files need to be recompiled.
  * Pascal compilers are Fast with a big F and Free Pascal is no exception. Yes, you no longer need to grow roots while compiling your programs, just hit the compile key and it's done, even for large programs.
  * Each unit has it's own identifiers In Pascal you never need to worry about polluting the namespace, like in C where an identifier needs to be unique accross the entire program. No, in Pascal each unit gets it's own namespace and that's very relaxed.
  * Integrated development environment Free Pascal comes with an IDE which work on several platforms, in which you can write, compile and debug your programs. You will save huge amounts of time using the IDE, the best programming friend you have.
  * Great integration with assembler Do you think pascal is for wimps who need to learn programming? WRONG! It's excellent for high tech programming and for the supreme nerds among you we have the integrated assemblers. You can easily mix assembler code and Pascal code, in the language you wish? Prefer Intel styled assembler? No problem, if it's needed Free Pascal will convert it to ATT for you. Do you want to convert your program into a source file for Nasm? No problem, and all ATT assembler in your source files is automatically converted.
  * Object oriented programming And if you do the serious programming, you are of course very interested in object oriented programming. Use the Turbo Pascal and Object Pascal ways of OOP according to your taste. The FCL and Free Vision and provide you with the powerful object libraries you need. For your database needs we support PostgreSQL, MySQL, Oracle, SQLite, Interbase and ODBC.
  * Smartlinking Free Pascal's smart linker leaves out any variable or code that you do not use. That makes small programs small with a big S, while they are still statically linked, avoiding DLL hell!
  * Distribution independence (Linux) As a result of this, software compiled by the Linux version of Free Pascal runs on any Linux distribution, making it much, much, easier to make your software support multiple Linux distributions.
  * Available for a lot of platforms on several architectures Free Pascal is available for more platforms than most other Pascal compilers and allows easy cross-compiling, just change the target in the IDE and compile! And there is work going on for even more platforms and processors.
  * Compatible Have existing code? Free Pascal is more compatible with it than any other Pascal compiler. We are almost completely compatible with Turbo Pascal and quite well compatible with Delphi source code. If you have code in another language, like C or assembler, just use favorite compiler for it and call it from Free Pascal.



## Feature list

### FPC

  * Free Pascal (aka FPK Pascal) is a 32 and 64 bit professional Pascal compiler. FPC is written completely in FPC and was bootstrapped using TP years ago.
  * It is available for different processors: Intel x86, Amd64/x86 64, PowerPC, Sparc. The discontinued 1.0 version also supports the Motorola 680x0. The following operating systems are supported: Linux, FreeBSD, Mac OS X/Darwin, Mac OS classic, DOS, Win32, OS/2, Netware (libc and classic) and MorphOS.
  * The language syntax has excellent compatibility with TP 7.0 as well as with most versions of Delphi (classes, rtti, exceptions, ansistrings, widestrings, interfaces).
  * A Mac Pascal compatibility mode is also provided to assist Apple users.
  * Furthermore Free Pascal supports function overloading, operator overloading, global properties and other such features.
  * RTL
  * FCL
  * Packages: gtk, gtk2, opengl, netdb, regexpr
  * profiling: heaptrc for memory leaks, gprof or valgrind for speed
  * cross compiling



### Lazarus

  * IDE, RAD: written completely with fpc+lazarus, running on Windows, Linux, MacOSX and FreeBSD. Extensible via IDEIntf package.
  * Translations: german, finnish, dutch, french, catalan, polish, spanish, ukrainian, portuguese, japanese, indonesian, chinese, italian, afrikaans, russian, arabic
  * Documentation: wiki, tutorials, context sensitive IDE help, mailing list
  * Debugger: breakpoints, watches, call stack
  * Source editor: include files, using synedit, syntax highlighting (more than 15), code folding, codetools, class completion, identifier completion (Delphi: code completion), word completion, code refactoring (rename identifier, extract procedure), bookmarks, keeping comments, search/replace with regexpr and multi lines and in directories, find declaration, method jumping, can parse and work with Delphi, Kylix and FPC sources at the same time
  * Form editing: object inspector, component editors, property editors, visual form inheritance, wysiwyg dragging, events creation,
  * Command line compilation: lazbuild
  * LCL: gtk1 and win32 fully working, gtk2: 95%, carbon: only started, qt amd wince: nearly all components started
  * Converter for Delphi units, projects and packages
  * Printing: printer4lazarus: cups (Linux, MacOSX, FreeBSD), winapi (Windows)
  * C header converter: h2pas and h2pas wizard (package h2paswizard)
  * Help: help for sources (F1 on identifier, e.g. TButton), most IDE windows, fpc keywords
  * DB controls (DBEdit and friends): delphi compatible
  * RTTI controls: controls using RTTI to avoid typing boring code
  * CGI: cgiLaz
  * chm help files: ChmHelpPkg
  * dbf: dbflaz
  * Automatic Testing: fpcunit
  * images: .bmp, .png, .xpm built in LCL. jpg, pnm, tga in imagesforlazarus
  * OpenGL control: lazopenglcontext (gtk, carbon, win32) or glscene (linux/gtk, win32)
  * Memory dataset: memDS
  * code beautifying: prettyformat
  * SDF: SDFLaz
  * rx controls: rx
  * tray icon: trayiconlaz
  * VirtualTreeview: virtualtreeslvl
  * zeosdb: zeosdb



### Third party packages

  * Internet/smtp/ftp/http/tcp: Synapse, Curl, Indy, LNet
  * virtualtreeview: extended TTreeView
  * glscene: opengl (Windows, Linux)



### Missing

  * IDE: Version control system: Using svn, but no frontend in IDE. There are plenty good ones.
  * IDE: frames
  * LCL: complete gtk2
  * LCL: complete carbon



## Booth FAQ

  * .NET support: Why supporting a virtual machine, when FPC is already native on more platforms, than .NET will be in years? The job of the VM should be done by the OS and not other layers which opens new security holes.
  * License: commercial closed source is allowed with the exception of the compiler and the IDE itself. The only but: If you modify the fpc/laz code, then you must publish the changes under modified LGPL.
  * Is database X supported: mysql: 4.0-5.0, postgresql: yes, oracle: yes, BDE: no, odbc: yes, Firebird/Interbase: yes, SQLite: yes
  * How much work is it to migrate to FPC/Lazarus: Normally it is easier to port to Lazarus than to Kylix, because the LCL is very compatible to the VCL. The IDE contains converters for Delphi projects, packages and units.
  * Speed: FPC creates often faster, sometimes slower code than Delphi.
  * How is this financed, who is sponsoring all this: It's open source. It's a hobby.

---

_Source: [https://wiki.freepascal.org/Systems_2006](https://web.archive.org/web/20241213020558/https://wiki.freepascal.org/Systems_2006)_
