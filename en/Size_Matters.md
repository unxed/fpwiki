# Size Matters

│ **English (en)** │  **[русский (ru)](<../ru/Size_Matters.md>)** │

## Contents

  * 1 Introduction
  * 2 Rule of thumb, what are current realistic sizes for Free Pascal/Lazarus binaries?
  * 3 Why are the binaries so big ?
  * 4 Is it bad when binaries are big ?
    * 4.1 Embedded
    * 4.2 Modem distribution
    * 4.3 Contests
  * 5 Incorrect compiler configuration
    * 5.1 Debug information
    * 5.2 Smartlinking
    * 5.3 Initialization and finalization sections
    * 5.4 Static binaries
    * 5.5 Optimization
    * 5.6 Lazarus lpr files
  * 6 2.2.0 problems
  * 7 UPX
  * 8 Framework costs
  * 9 Constant strings
  * 10 Unrealistic expectations
  * 11 Comparisons with GCC
  * 12 Comparisons with Delphi
  * 13 Comparisons with .NET/Java
  * 14 Analysis of various options
  * 15 See also



## Introduction

This page is about binary sizes. Through the years there has been a lot of confusion about FPC and Lazarus binary sizes. Before you make any remark on the maillist, please read this FAQ. 

The main reason for this faq is that most discussions about this subject tend to get caught up in details too quickly. Also the opinion of people that shout "bloat" at nearly everything clouds the global picture often more than it contributes to clarity. 

## Rule of thumb, what are current realistic sizes for Free Pascal/Lazarus binaries?

  * **anything under 1MB is not considered a problem**. 
    * Make sure they are properly **stripped and smartlinked** before measuring, and that ALL libraries are built using smartlinking.
    * **DO NOT UPX binaries routinely** , unless you have extremely good reasons to do so (see below). The size is less important than the memory load an unpacked binary poses. And memory is still more expensive than disk. Most remote access tools implement compression in their tunnel.
  * With small apps it is a bit harder to estimate. This is because the exact RTL size is OS dependent. However 100k standalone binaries that do something can be done, usually even below 50k. 
    * On windows 20k WinAPI GUI using binaries are no problem.
    * Unit Sysutils contains internationalisation, textual errormessages, and exception handling and some other stuff that is always linked in when this unit is used (think 40-100k total).
  * Lazarus apps on Windows are about 500k, but quickly grow to 1.5 MB as more and more of the Lazarus widgets are used. Lazarus apps binaries can become 100MB+ when debug info is linked in (comparable with TD32 debug info in Delphi) 
    * This is a bit more than when recompiling it with an older Delphi, and a bit less than with a modern Delphi (D2009+ RTL minimal RTL size jumped considerably) which is the price for cross-platform compatibility and project maintainability.
    * When the moment is reached where extra code doesn't add a dependency on more LCL code, this quick growth subsides.
    * The 1.5MB point above is a rule-of-thumb. It depends very much on your GUI creation style and the number of different widgets that you use - and their complexity.
    * For Lazarus applicationss, quite a percentage of the binary is non-code, mostly strings and tables.
  * Linux/FreeBSD simple binaries are bigger than the corresponding GCC ones. This is because they don't use shared libraries (which you can easily see using `ldd ./xx`)
  * 64 bit binaries will always be larger than x86 ones. In general RISC platforms also generate slightly larger code.



## Why are the binaries so big ?

**Answer: They are not supposed to be big.**

If you **perceive** them as big, then 

  * either you didn't configure FPC properly, or
  * have an unrealistic expectation of what the binary size should be
  * You are trying to do something that FPC is not designed to do.



The last one is the least likely of all three. I'm going to treat all cases quickly in the next paragraphs. 

## Is it bad when binaries are big ?

Well, depends on the magnitude of course. But it is safe to say that hardly anybody should be worried of having binaries as big as a few MB or even over ten MB for sizable applications. 

However, there still are a few categories that might want to have some control over keeping binaries small. 

  1. the embedded programming world obviously (and then I mean not the embedded PCs which still have tens of MBs)
  2. people that really distribute daily by modem
  3. Contests, benchmarking (the notorious language shootout)



Note that an often cited misconception is that bigger binaries are slower in operation. In general this is not true, exotic last-cycle stuff as code cachelines aside. 

### Embedded

While Free Pascal is reasonably usable for embedded or system purposes, the final release engineering and tradeoff decisions are based on the build requirements of more general applications, though some of the more embedded targets (like 16-bit dos or embedded ARM/AVR/MIPS(=PIC32) stretch this to the limit. 

If you have such specialistic needs for more regular targets though, you could set up a shadow project (something like the specialised versions of certain Linux distros that are available). Worrying the already overburdened FPC team with such specialised needs is not an option, especially since half of the serious embedded users roll their own anyway. 

### Modem distribution

The modem case is not just about "downloading from the Net" or "my shareware must be as small as possible", but e.g. in my last job we did a lot of deployment to our customers and our own external sites via remote desktop over ISDN. But even with a 56k modem you can squeeze a MB through in under 5 minutes. 

Be careful to not abuse this argument to try to provide a misplaced rational fundament for an emotional opinion about binary size. If you make this point, it is useless without a thorough statistical analysis of what percentage of actual modem users you have for your application (most modem users don't download software from the net, but use e.g. magazine shareware CDs), and with 70%+ broadband penetration, this is a bit outdated in the West. 

### Contests

Another reason to keep binaries small is language comparison contents (like the Language Shootout). However this is more like solving a puzzle, and not really related to responsible software engineering. 

## Incorrect compiler configuration

I'm not going to go explain every aspect of the compiler configuration at great length, since this is a FAQ, not a manual. This is meant as an overview only. Read [manuals](<http://www.freepascal.org/docs.html>), and [buildfaq](<buildfaq.md> "buildfaq") thoroughly for more background info. 

Generally, there are several reasons why a binary might be bigger than expected. This FAQ covers the most common reasons, in descending order of likelihood: 

  1. The binary still contains debug information.
  2. The binary was not (fully) smartlinked
  3. The binary includes units containing initialization sections that execute a lot of code.
  4. You link in complete (external) libraries statically, rather than using shared linking.
  5. Optimization is not (entirely) turned on.
  6. The Lazarus project file (lpr) has package units in its uses section (this is done automagically by Lazarus)



In the future, shared linking to a FPC and/or Lazarus runtime library might significantly alter this picture. Of course then you will have to distribute a big DLL with lots of other stuff in it which will give you versioning issues. This is all still some time in the future, so it is hard to quantify what the impact on binary sizes would be. Especially because dynamic linking also has size overhead (on top of unused code in the shared library). 

### Debug information

Free Pascal uses GDB as debugger and LD as linker. These work with a system of in-binary debuginfo, in the older _stabs_ or newer _dwarf_ format. People often see e.g. Lazarus binaries that are 40MB. The correct size should be about 6MB, the rest is debug info (and maybe 6 MB from not smartlinking properly). 

Stabs debuginfo is quite bulky, but has the advantage that it is relatively independent of the binary format. It has been replaced by DWARF except on some legacy platforms. 

There is often confusion with respect to the debug info, which is caused by the internal strip in a lot of win32 versions of the binutils. Also some versions of the win32 strip binary don't fully strip the debug info generated by FPC. So people toggle a (Lazarus/IDE or FPC commandline) flag such as -Xs and assume it worked, while it didn't. FPC has been adapted to remedy this. 

So, when in doubt, **always** try to strip manually, and, on Windows, preferably with several different STRIP binaries. 

This kind of problem probably got rarer especially on Windows, since the internal linker provides a more consistent treatment of these problems. However they may apply to people using more exotic targets for quite some time to come. 

You can use the whole strip system to ship the same build as the (stripped) user version while retaining the debug version (unstripped) for e.g. interpreting traceback addresses. So if you do formal releases, retain a copy of the unstripped binary that you ship, and always do a release build with debug info. 

The design of GDB itself lets you keep and use debug information out of the binary file (external debug information), in a separate .dbg file. The size of resulting binary is not increased due to debug information, and you can still successfully debug the binary. You don't need the .dbg file to run and use the application, it is used only by the debugger. Since all debug information has been removed from the binary file, you will not get much effect if you try to strip it. 

To compile your application in this way, you should use the -Xg switch or corresponding the Lazarus GUI option: Project|Compiler Options|Linking|Debugging|Leave generating debugging info enabled and enable use External gdb debug symbols. 

[![BigExe.PNG](https://wiki.freepascal.org/images/e/e6/BigExe.PNG)](</File:BigExe.PNG>)

A blank form application for Win32, compiled with external debug information would occupy about 1 Mb, and .dbg file would be 10 Mb. 

### Smartlinking

_(main article:[File size and smartlinking](<File_size_and_smartlinking.md> "File size and smartlinking"))_

The fundamental smartlinking principle is simple and well known: don't link in what is not used. This of course has a good effect on binary size. 

However the compiler is merely a program, and doesn't have a magic crystal ball to see what is used, so the base implementation is more like this 

  * The compiler divides up the code into so-called "sections" which are fairly small.
  * Then the linker determines what sections are used using the rule "if no label in that section is referenced, it can be removed".



There are some problems with this simplistic view: 

  * virtual methods may be implicitly called via their VMTs. The GNU linker can't trace call sequences through these VMTs, so they must all be linked in;
  * tables for resource strings reference every string constant, and thus all string constants are linked in (one reason for sysutils being big).
  * symbols that can be called from the outside of the binary (this is possible for non-library ELF binaries too) must be kept. This last limitation is necessary to avoid stripping exported functions from shared libraries.
  * Another such pain point are published functions and properties, which have to be kept. References to published functions/properties can be constructed on the fly using string operations, and the compiler can't trace them. This is one of the downsides of reflection.
  * Published properties and methods can be resolved by creating the symbol names using string manipulation, and must therefore be linked in if the class is referenced anywhere. Published code might in turn call private/protected/public code and thus a fairly large inclusion.



Another important side effect that is logical (but often forgotten) is that this algorithm will link in everything referenced in the initialization and finalization parts of units, even if no functionality from those units are used. So be careful what you USE. 

Anyway, most problems using smartlinking stem from the fact that for the smallest result FPC generally requires "compile with smartlinking" to be on **WHEN COMPILING EACH AND EVERY UNIT, EVEN THE RTL**

The reason for this is simple. LD only could "smart" link units that were the size of an entire .o file until fairly recently. This means that for each symbol a separate .o file must be crafted. (and then these tens of thousands of .o files are archived in .a files). This is a time (and linker memory) consuming task, thus it is optional, and is only turned on for release versions, not for snapshots. **Often people having problems with smartlinking use a snapshot that contains RTL/FCL etc that aren't compiled with smartlinking on.** The only solution is to recompile the source with smartlinking (-CX) on. See [buildfaq](<buildfaq.md> "buildfaq") for more info. 

In the future this will be improved when the compiler emits smartlinked code by default, at least for the main targets. This will be made possible by two separate developments. First, the GNU linker LD now can smartlink more finely grained (at least on Unix) using --gc-sections; secondly the arrival of the FPC internal linker (in the 2.1.1 branch) for all working Windows platforms (wince/win32/win64). The smartlinking using LD --gc-sections still has a lot of problems because the exact assembler layout and numerous details with respect to tables must be researched, we often run into the typical problem with GNU development software here, the tools are barely tested (or sometimes not even implemented, see the DWARF standard) outside what GCC uses/stresses. Moreover, versions for non *nix targets are often based on older versions (think dos, go32v2, amiga here). 

The internal linker can now smartlink Lazarus (17 seconds for a full smartlink on my Athlon64 3700+ using about 250MB memory) which is quite good, but is Windows only and 2.1.1 for now. The internal linker also opens the door to more advanced smartlinking that requires Pascal specific knowledge, like leaving out unused virtual methods (20% code size on Lazarus examples, 5% on the Lazarus IDE as a rough first estimate), and being smarter about unused resource strings. This is all still in alpha, and the statistics above are probably too optimistic, since Lazarus is not working with these optimizations yet. 

During a discussion on stack overflow, some [extra details](<https://stackoverflow.com/questions/4519726/delphi-which-are-the-downsides-of-having-unused-units-listed-in-the-uses-clause/4519894#4519894>) came forward (are mostly the same for FPC as for Delphi, at least on Windows): 

### Initialization and finalization sections

If you include a unit in _USES_ section, even when USES'd indirectly via a different unit, then IF the unit contains initialization or finalization sections, that code and its dependencies is always linked in. 

A unit for which this is important is sysutils. As per Delphi compatibility, sysutils converts runtime errors to exceptions with a textual message. All the strings in sysutils together are a bit bulky. There is nothing that can be done about this, except removing a lot of initialisation from sysutils that would make it Delphi incompatible. So this is more something for an embedded release, if such a team would ever volunteer. 

### Static binaries

_(main article:[Lazarus/FPC Libraries](<Lazarus/FPC_Libraries.md> "Lazarus/FPC Libraries"))_

One can also make fully static binaries on most OSes (*), incorporating all libraries into the binary. This is usually done to ease deployment, but produces huge binaries as tradeoff consequence. Since this is wizard territory I only mention this for the sake of completeness. People who do this hopefully know what they are doing. 

Instead of making static binaries, many programmers do dynamic linking / shared linking. This _CAN_ generate a much, much smaller binary executable. However there are also cases where the binary gets bigger, specially on architectures like x86_64 where PIC is on by default. Dynamic linking (win32) and shared linking (*nix) are the same concept, but their internal workings differ, as can be easily seen by the fact that *nix systems need the shared libraries on the host to (cross-)link, and when linking a Windows binary you don't need the relevant .dlls on the system. 

(*) Apple does not support static linking on macOS, because that would make it harder to support backward compatibility. 

### Optimization

_(main article:[Optimization](<Optimization.md> "Optimization"))_

Optimization can also shave off a bit of code size. Optimized code is usually tighter. (but only tenths of a percent) Make sure you use -O3. See also [Whole Program Optimization](<Whole_Program_Optimization.md> "Whole Program Optimization") for further code size reduction. 

### Lazarus lpr files

In Lazarus, if you add a package to your project/form you get its registration unit added to the lpr file. The lpr file is not normally opened. If you want to edit it, first open it (via project -> view source). Then remove all the unnecessary units (Interfaces, Forms, and YOUR FORM units are the only required ones, anything else is useless there, but make sure you don't delete units that register things such as image readers (jpeg) or testcases). 

You can save up to megabytes AND some linking dependencies too if you use big packages (such as glscene). 

This kind of behaviour is typical for libraries that do a lot in the initialization sections of units. Note that it doesn't matter where they are used (.lpr or a normal unit). Of course smartlinking tries to minimize this effect. 

## 2.2.0 problems

There appear to be some size problems in FPC 2.2.0 _is this still relevant for 2.6.x/2.7.x?_ Note that these remarks hold for the default setup with internal linker enabled. 

  * It seems that FPC 2.2.0 doesn't strip if any -g option is used to compile the main program. This contrary to earlier versions where -Xs had priority over -g
  * It seems that FPC 2.2.0 doesn't always smartlink when crosscompiling. This can be problematic when compiling for windows, not only because of size, but also because dependencies are created to functions that might not exist.



## UPX

**Note: UPX support in makefiles, and the distribution of upx by FPC ceased after 2.6.0. New releases of FPC won't package upx any more**

The whole strange UPX cult originates mostly from a mindless pursuit of minimal binary sizes. In reality UPX is a tool with advantages and disadvantages. 

The advantages are: 

  1. The decompression is easy for the user because it is self-contained
  2. Some size savings are made if (and only if) the size criterion is based on the binary size itself (as happens in demo contests). However, especially in the lowest classes it might be worthwhile to minimize the RTL manually and to code your compression yourself, because you can probably get the decompression code much tighter for binaries that don't stress all aspects of the binary format.
  3. For rarely used applications or applications run from removable media the disk space saving may outweigh the performance/memory penalties.



The disadvantages are: 

  1. **UPXed binaries are increasingly being fingered by** the malware heuristics of popular **antivirus and mail-filtering apps.**
  2. worse compression (and also the decompression engine must be factored into _EACH_ binary) by archivers (like ZIP) and setup creation tools
  3. decompression occurs on every execution, which introduces a startup delay.
  4. Since Windows XP and later now feature a built-in decompressor for ZIP, the whole point of SFX goes away a bit. Newer versions of browsers (on windows) made this even more pronounced since directly downloaded EXE's now need an "unblock" operation to run.
  5. An internally compressed binary can't be memory mapped by the OS, and must be loaded in its entirety. This means that the entire binary size is loaded into VM space (memory+swap), including resources.
  6. You introduce another component (UPX, decompression stub) that can cause incompatibilities and problems.



The memory mapping point needs some explanation: With normal binaries under Windows, all unused code remains in the .EXE, which is why Windows binaries are locked while running. Code is paged in 4k (8k on 64-bit) at a time as needed, and under low memory conditions is simply discarded (because it can be reloaded from the binary at any time). This also applies to graphic and string resources. (is this still the case with ASLR?) 

A compressed binary must usually be decompressed in its entirety, to avoid badly affecting the compression ratio. So Windows has to decompress the whole binary on startup, and page the unused pages to the system swap, where they rot unused, and also take up extra swap space. 

## Framework costs

A framework greatly decreases the amount of work to develop an application. 

This comes however at a cost, because a framework is not a mere library, but more a whole subsystem that deals with interfacing to the outside world. A framework is designed for a set of applications that can access a lot of functionality, (even if a single application might not). 

However the more functionality a framework can access, the bigger a certain minimal subset becomes. Think of internationalization, resource support, translation environments (translation without recompilation), meaning error messages for basic exceptions etc. This is the so called framework overhead. 

This size of empty applications is not caused by compiler inefficiencies, but by framework overhead. The compiler will remove unused code automatically, but not all code can be removed automatically. The design of the framework determines what code the compiler will be able to remove at compile time. 

Some frameworks cause very little overhead, some cause a lot of overhead. Expected binary sizes for empty applications on well known frameworks: 

  * No framework (RTL only): +/- 25kb
  * No framework (RTL+sysutils only): +/- 100-125kb
  * [MSEGUI](<MSEide_&_MSEgui.md> "MSEide & MSEgui"): +/- 600kb
  * Lazarus LCL: +/- 1000kb
  * Free Vision: +/- 100kb
  * [Key Objects Library](<KOL-CE.md> "KOL-CE"): +/- 50kb



In short, choose your framework well. A powerful framework can save you lots of time, but, if space is tight, a smaller framework might be a better choice. But be sure you really need that smaller size. A lot of amateurs routinely select the smallest framework, and end up with unmaintainable applications and quit. It is also no fun having to maintain applications in multiple frameworks for a few kb. 

Note that e.g. the Lazarus framework is relatively heavy due to use of RTTI/introspection for its streaming mechanisms, not (only) due to source-size . RTTI makes more code reachable, degrading smartlinking performance. 

  


## Constant strings

If you use many thousands of hardcoded strings, their overhead may become significant. Being aware of how FPC stores constant strings (as of version 3.2.2) may be helpful for choosing more compact data structures. 

Normally, simple string constants in plain ascii will be stored as a natively aligned shortstring. This is the most efficient storage method, and generally preferable for everything except strings longer than 255 characters or that contain non-ascii. 

Ansistrings (or UTF8strings or RawByteStrings etc) are stored natively aligned with 3 pointerfuls of overhead. For 64-bit programs, this means every such string has 24 bytes of unavoidable overhead, which rapidly adds up if you have tons of such strings. Make sure strings are not type-declared as ansistrings unless they have to be. 

If a constant string is used as a direct parameter of a function that expects an ansistring, then the string is stored as an ansistring even if it otherwise could be a simple shortstring. For example, consider a widely-called log writing function that takes an ansistring - then every little debug printout going through the log function gets stored as an ansistring and generates 3 pointerfuls of overhead. A function that takes a shortstring will store constant parameter strings more efficiently, even if the shortstring variant of the function only forwards the parameter to the ansistring function (however, that will incur a runtime string conversion cost). To minimise binary size, take shortstrings as function parameters, unless they have to be ansistrings. 

Constant arrays of strings are the worst. Because constant arrays have to have a predictable, fixed size, such strings cannot be stored as simple shortstrings. Instead, an array of shortstring will store the full 256 bytes for every entry, regardless of how many characters are actually used. An array of string[x] can be space-efficient, but only if all strings in the array are of similar lengths, since every stored item will take as much space as the longest allowed string. An array of ansistrings uses pointers instead, so in this case you only get the 3 pointerfuls of overhead for each item, which is still a bit much. To minimise binary size, avoid defining strings in constant arrays. 

FPC does not compress constant strings, presumably so they can be directly paged from the executable binary on disk, which gives the best startup time and lowest runtime memory use. So all strings appear fully human-readable, if you hex-edit the built binary. FPC does deduplicate constant strings, so if the same constant string appears repeatedly, usually only one reference to it is stored in the binary. You could conceivably save space by custom packing strings in an extra file and reading them into memory when needed, but the space savings probably wouldn't be worth the effort and runtime slowdown, unless you have over a million strings. 

  


## Unrealistic expectations

A lot of people simply look at the size of a binary and scream **bloat!**. When you try to argue with them, they hide behind comparisons (but TP only produces...), they never really say 'why' they need the binary to be smaller at all costs. Some of them don't even realise that 32-bit code is ALWAYS bigger than 16-bit code, or that OS independence comes at a price, or ...,or ..., or... 

As said earlier, with the current HD sizes, there is not that much reason to keep binaries extremely small. FPC binaries being 10, 50 or even 100% larger than compilers of the previous millenium shouldn't matter much. A good indicator that these views are pretty emotional and unfounded is the overuse of UPX (see above), which is a typical sign of binary-size madness, since technically it doesn't make much sense. 

So where is this emotion coming from then? Is it just resisting change, or being control-freaks? I never saw much justified cause, except that sometimes some of them were pushing their own inferior libraries, and tried to gain ground against well established libs based on size arguments. But this doesn't explain all cases, so I think the binary size thing is really the last "640k should be enough for anybody" artefact. Even though not real, but just mental. 

A dead giveaway for that is that the number of realistic patches in this field is near zero, if not zero. It's all maillist discussion only, and trivial RTL mods that hardly gain anything, and seriously hamper making real applications and compatibility (and I'm not a compatibility freak to begin with). Nobody sits down for a few days and makes a thorough investigation and comes up with patches. There are no cut down RTLs externally maintained, no patch sets etc, while it would be extremely easy. **Somehow people are only after the last byte if it is easy to achieve, or if they have something "less bloated" to promote.**

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The above paragraph is still true, nearly five years after writing it.

Anyway, the few embedded people I know that use FPC intensively all have their own customized cut back libraries. For one person internationalization matters even when embedded (because he talks a language with accents), and exceptions do not, for somebody else requirements are different again. Each one has its own tradeoffs and choices, and if space is 'really' tight, you don't compromise to use the general release distro. 

And yes, FPC could use some improvements here and there. But those shouldn't hurt the "general programming", the multiplatform nature of FPC, the ease of use and be realistic in manpower requirements. Complex things take time. Global optimizers don't fall from the sky readily made. 

Some of the work has been done as part of the **embedded RTL** project, to prepare for low memory targets. This could make maintaining a minimal RTL easier, since many conditionals are already in place. Still for major targets the movement on this is still negligible. 

## Comparisons with GCC

Somewhat less unrealistic are comparisons with GCC. Even the developers mirror themselves (and FPC) routinely against GCC. Of course GCC is a corporate sponsored behemoth, that is also the Open Source's world favorite. Not all comparisons are reasonable or fair. Even compilers that base themselves on GCC don't support all heavily sponsored "c" gcc's functionality. 

Nevertheless, considering the differences in project size, FPC does a surprisingly good job. Speed is OK, except maybe for some cases of heavily scientific calculating, binary sizes and memory use are sufficient or even better in general, the number of platforms doesn't disappoint (though it is a pity that 'real' embedded targets are missing). 

Another issue here is that FreePascal generally statically links (because it is not ABI stable and would be unlikely to be on the target system already even if it was) its own RTL. GCC dynamically links against system libraries. This makes very small (in terms of source size) programs made with fpc have significantly larger binaries than those made with GCC. It's worth mentioning here, that the binary size has nothing to do with the memory footprint of the program. FPC is usually much better in this regard than GCC. 

Still, I think that considering the resources, FPC is doing extraordinarily well. 

## Comparisons with Delphi

In comparisons with Delphi one should keep in mind that 32-bit Delphi's design originates in the period that a lot of people DIDN'T even have Pentium-I's, and the developer that had 32MB RAM was a lucky one. Moreover Delphi was not designed to be portable. 

Considering this, Delphi scaled pretty well, though there is always room for improvement, and readjustments that correct historical problems and tradeoffs. (It is a pretty well known fact that a lot of assembler routines in newer Delphi's were slower than their Pascal equivalents, because they were never updated for newer processors. It is said that has only been corrected since Delphi 2006.) 

Still, slowly on the compiler front, FPC isn't Delphi's poor cousin anymore. The comparisons are head-on, and FPC 2.1.1 winning over Delphi is slowly getting the rule, and not the exception anymore. 

Of course that is only the base compiler. In other fields there is still enough work to do, though the internal linker helps a lot. The debugger won't be fun though :-) Also in the language interoperability (C++, Obj C, JNI) and shared libraries is lots of work to do, even within the base system. 

## Comparisons with .NET/Java

Be very careful with comparisons to these JIT compiled systems: JITed programs have different benchmark characteristics, and also extrapolating results from benchmarks to full programs is different. 

While a JIT can do a great job sometimes (specially in small programs that mostly consist out of a single tight loop), but this good result often doesn't scale. Overall my experience is that statically compiled code is usually faster in most code that is not mainly bound by some highly optimizable tight loop, despite the numerous claims otherwise on the net. 

A fairly interesting quantitative source for this is this [Shootout faq](<http://shootout.alioth.debian.org/gp4sandbox/faq.php#fullcpu>) entry. Another interesting one is [memory allocation in JVM/.NET](<http://www.folding-hyperspace.com/program_tip_11.htm>). 

Note that since 2007, Java 6 suddenly caused a significant jump in the Java shootout-ratings, and starts touching the bottom of normal native compilers. This shows that one must be very careful echoing sentiments on the web (both positive and negative) and stick to own measuring, with the border conditions trimmed to the application domain that you are in. 

## Analysis of various options

Tests on Lazarus 0.9.29 with FPC 2.4 (FPC 2.2.4 with Windows). 

_Optimized compiler_ means: 

  * 1\. Project|Compiler Options|Code|Smart Linkable (-CX) -> Checked
  * 2\. Project|Compiler Options|Linking|Debugging| Uncheck all except



Strip Symbols From Executable (-Xs) -> Checked 

  * 3\. Project|Compiler Options|Linking|Link Style|Link Smart (-XX) -> Checked



The most important items seem to be 2. For a simple application the executable size should now be 1-3 MB instead of 15-20 MB. 

  * 4\. (Optional) Project|Compiler Options|Code|Optimizations|smaller rather than faster -> Checked (Warning: this might decrease performance)



_Default Lazarus_ means as installed from package/setup. 

_LCL without debug information_ mean after rebuilding Lazarus IDE and LCL without debug information (-g-). 

|  | Default Lazarus |  | LCL without debug information  
---|---|---|---|---  
_Ubuntu 64 bits / Lazarus 64 bits_ |   
Default application |  | 13,4 Mb |  | 7,5 Mb / 8  
Optimized compiler |  | 4,4 Mb |  | 2,70 Mb (0.29svn FPC2.4 2,5)  
|   
_Ubuntu 32 bits / Lazarus 32 bits_ |   
Default application |  | 19,6 Mb |  | 5,7 Mb  
Optimized compiler |  | 2,9 Mb |  | 1,6 Mb  
|   
_Windows XP 32 bits / Lazarus 32 bits_ |   
Default application |  | 11,8 Mb |  | 2,14 Mb  
Optimized compiler |  | 1,62 Mb |  | 1,50 Mb  
|   
_Windows Seven 64 bits / Lazarus 64 bits_ |   
Default application |  | 12,3 Mb |  | 3,20 Mb  
Optimized compiler |  | 2,14 Mb |  | 2,16 Mb  
  
  


## See also

  * [File size and smartlinking](<File_size_and_smartlinking.md> "File size and smartlinking")
  * [Lazarus FAQ - Why are the generated binaries so big](<Lazarus_Faq.md> "Lazarus Faq")
  * [macOS executable size optimisation](<macOS_Programming_Tips.md> "macOS Programming Tips")

---

_Source: [https://wiki.freepascal.org/Size_Matters](https://web.archive.org/web/20241213173225/https://wiki.freepascal.org/Size_Matters)_
