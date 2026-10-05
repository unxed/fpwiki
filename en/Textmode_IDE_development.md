# Textmode IDE development

See also [Textmode IDE](<Textmode_IDE.md> "Textmode IDE") for more user-oriented hints, and [go32v2 development](<go32v2_development.md> "go32v2 development") for hints about the particular dos problems of the IDE. 

## Contents

  * 1 General problems
  * 2 Compatibility
  * 3 ansistringification
  * 4 Chm Support
    * 4.1 CHM boundaries problem
  * 5 Files of the IDE
    * 5.1 Classchart of objects + FV + IDE
  * 6 Debugging the IDE
    * 6.1 With GDB tty
    * 6.2 With GDB attach
    * 6.3 Writeln like
    * 6.4 Exceptions to stderr



## General problems

While the obvious task for the textmode IDE is of course fixing several or some of the 80 bugs, some other stuff comes to mind as future projects. Nearly all have to do with the fact that the IDE effectively stopped evolving significantly since 1.0 times. A lot of problems and ugliness lies in the TP/DOS legacy from this period: 

  * the fact that it uses Dos, rather than Sysutils for its filename operations. 
    * now that the compiler uses sysutils mostly, the DOS versions are the less tested ones.
    * the fact that shortstrings (or parts of them) are used for filenames.
    * under windows there is no support for unc-paths


  * Even the shortstring filename handling is far from perfect, since the IDE clearly hasn't had the end-user testing for *nix targets that dos/win32 had. Grepping for fixfilename() might yield such problems (like e.g. bug #5614), where filenames are pretty printing, however effectively killing case sensitive filesystems. Bringing this up to ansistring/sysutils level might also help to unify these routines between textmode IDE and Lazarus.


  * Some functionality is deprecated or not active or documented. Examples are browser and code folding (see comments of bug #6696 Some of the functionality mentioned in #11583 might also be "newer"). Note that this kind of functionality is often not finished, e.g. afaik there is no way to save folds. (make them persistent some how). Also the editor components seem to have incremental syntaxhighlighting under ifdef.


  * Another major problem is that the docs have significantly grown. On a PII-233 (128MB and a more recent 20GB disc) it took 20-40 minutes to install the HTML files from the help. Nowadays we have chm support in the FPC tree, maybe it is time to kill all the old helpfile formats and standarize on CHM for a while. (removing the need to have separate files HTML is I think more important than killing e.g. .tph) **Initial CHM support added to trunk** See the separate section about this topic.


  * On *nix, terminals are swiftly adopting UTF-8 as output type. Supporting this, even very minimally (say ISO8859-1+linedrawing) will be a quite complex task. Required FV rewrite is [done](<Free_Vision.md> "Free Vision") as of February, 2022.



## Compatibility

In the past the Textmode IDE was compatible with lots of compilers and platforms that are dead. This compatibility has not been maintained/tested for close to a decade, so it is doubtful it works at all. So IMHO it can be removed, and dependencies on things like sysutils are no longer forbidden. 

## ansistringification

Since FV has reduced in meaning, maybe it can be modernised using the delphi model a bit (streaming and constructors/destructors). I know this is heresy, but a tpobj version could continue to exist. It is mainly for the IDE. I also don't know if it is worthwhile at all (most notably if FV exploits the static nature of objects...). 

Maybe a good first approximation would be to try to introduce ansistring everywhere. (**Florian has said he has tried this in the past, but that he got stuck. I did an attempt and also got stuck.**) 

I did a real minor attempt to kill some redundant string routines, but are stuck with the fact that one can't import sysutils into a major TV or IDE because that will cause incompatibilities due to the pstring type. (which is defined both by TV as by sysutils). This is probably the reason why wutils wraps so many sysutils routines, together with the fact that cross-unit overloading seems a bit ambiguous if the relevant parameter types are convertible to eachother (e.g. shortstring and ansistring, or char) It would be major FV re-engineering, and a significant loss of backwards compatibility to fix this. ) 

It could be easily remedied by reworking TV/IDE to use a "pshortstring" everywhere, which is still compatible with pstring for backwards compatibility as long as sysutils isn't added. 

A second stage would replace all path related strings by ansistrings. 

For this it is also interesting to look at the C++ version, since there some features only more recently introduced to pascal are added (operator overloading, generics). However a new codebase doesn't have to strictly follow C++, since FPC has certain features that are useful (open arrays, arrays of const, dyn arrays) too. 

**note:** it seems that (static) TP objects are not initialized/finalized. Using ansistrings with them is thus hazardous. Heap TP objects seem to be initialized/finalized. **Fixed by Sergei r16632,16635**

## Chm Support

The [chm](<chm.md> "chm") support was mainly meant to remedy the defects of the loose html based help; mostly install time, HD size (fragmentation, uncompressed) the fact that indexing must be done on the fly, and the constant little problems with filenames in generated HTML files. 

Extract the loose HTML help on a Athlon XP2000 takes 8 minutes, and creating the index another 5. CHM reduces this to effectively zero, and adds the full text search capability. On the P-II 233MHz that I occasionally use for DOS testing, times can be over an hour on plain dos. 

The CHM support has been in the IDE since 2.2.4, and most seems to be working. Sometimes the html generation from the latex documents has problems, but strictly speaking that is not a chm problem 

The LaTeX docs used to be a problem, but recently there has been progress in this department, see [ Chm backend for fpdoc, latex part.](<chm_backend_for_fpdoc.md> "chm backend for fpdoc") Other improvements to be done: 

  * Links over CHM file boundaries still have to be fixed, this is needed for an effective main page. I already have created a simple main page that works in Windows. (and can generate it) (**done** an initial implementation)
  * installation into the IDE by the installer. (**done but** not properly installed yet)
  * Index majeur?
  * index for latex pages. (**done** for ref archive from ref.kwd)
  * preparing the main index takes several seconds on a XP 2000+, specially with LCL.chm installed. Might be annoying on slow computers. Cache? It might also be that the index collection, suffers from the inserted sorting dimensioning problem. (tcollections and tlist are both one big array of pointers. (Ordered) Inserting in such an large array requires a lot of fields to move to make room )
  * there are problems with labels. E.g. from the main TOCs. For now they are stripped, but maybe first labels should be tried, and only then revert to main page.
  * The ref/user/prog chms are in UTF-8 which is not nice for the textmode IDE. (fixed in +/- 2.4.0, there might be detail bugs left though)



### CHM boundaries problem

This problem is partial to a deeper problem in the textmode IDE. The way a help page/topic is identified in the textmode IDE is by two (16-bit?) integers. One is the fileid, the other is the topicid in the file. 

HTML and CHM's however work string based. IOW a string for file, and a string for the topic (combination of path/file#label). This is solved by allocating a fileid for each chm dynamically onload, and similarly for the topic files. The html subsystem maintains a links-in-current topic cache, and the indexes into that cache act as topicids. (in other wpords a bit dynamic, not all URLs in the chm have one). 

However since helpfiles are loaded on a need-to basis, and one might encounter a link to a.chm/path/file in another chm before a.chm is loaded, making it hard to resolve the IDs. This is currently solved by having a registry of CHM files as a global. However that totally bypasses the central helpsystem. 

The plain HTML help didn't suffer from this because they cheated: all HTML must be installed in one directory, and links are fixed in the HTML itself. (refer to hardcoded paths ../prog/) So the entire HTML became one big helpfile in practice. Which is fine as a hack, but not really nice or reproducible for other formats. 

A minor rewrite that changes the identification to something else (a record with both string and topic id's?) is a possibility, but for that I must first dig up examples of all helpfile types supported and test how they work. 

## Files of the IDE

Here a short list of the units in the IDE, and their contents. A rough size in kb is added to indicate the relative size of the unit. 

  


Unit | Size | Remark   
---|---|---  
fp | 14k | The main program for the IDE. Parameter processing and a *lot* (400 lines) of initialization.   
fpcalc | 14k | Calculator object for the IDE   
fpcatch | 6k | IDE exception handling.   
fpcodcmp | 20k | Code Complete routines   
fpcodtmp | 18k | Code Template routines   
fpcompil | 38k | Compiler call routines for the IDE   
fpconst | 17k | Constants used by the IDE   
fpcygwin | 2k | Cygwin helpers (versioning, dll name)   
fpdebug | 103k | Debugger call routines for the IDE   
fpdesk | 29k | Desktop loading/saving routines   
fpdpansi | 3k | Unit to export current screen buffer to an ansi file (screendump)   
fpevalw | 2k | Debug expression evaluator dialog ?   
fphelp | 24k | Help routines for the IDE   
fpide | 72k | Main IDEApp object   
fpini | 24k | Write/Read Options to INI File   
fpintf | 8k | Misc routines for the IDE. compiling/running related mostly.   
fpkeys | 8k | Learn keys routines for the IDE   
fpredir | 19k | Unit to redirect output and error to files   
fpregs | 50k | (cpu)Register debug routines for the IDE   
fpswitch | 49k | Compiler switches routines for the IDE   
fpsymbol | 58k | Symbol browse support routines for the IDE   
fptemplt | 7k | Template support routines for the IDE   
fptools | 47k | Tool (menu) support for the IDE   
fpusrscr | 41k | User screen support routines   
fputils | 11k | Utilility routines used by the IDE. Path/file/string related mostly.   
fpvars | 5k | Global variables for the IDE   
fpviews | 131k | Views and view-related functions for the IDE   
pmode | 6k | Support routines for DPMI programs (dos support)   
vesa | 20k | VESA (graphmode switching) support routines   
wansi | 28k | ANSI support   
wcedit | 58k | Code editor template objects   
wchmhwrap | 5k | Chm wrapper. CHM reader parts that deal with classes, to obtain a clean TV - Delphi model separation   
wconsole | 2k | Contains some routines that save/restore console state when executing built programs.   
wconsts | 0k | Strings for common utilities. A handful of numeric consts, and a big includefile with textual constants (localisation?)   
weditor | 208k | Code editor template objects   
whelp | 27k | Help support & Borland OA .HLP reader objects and routines (the latter are moved to woahelp?)   
whlpview | 41k | Help display objects. The base widgets for the help system (?)   
whtml | 30k | base object definitions for html help?   
whtmlhlp | 43k | Main html parser. (for separate html files, and the base of .chm support)   
whtmlscn | 28k | HTML scanner objects   
winclip | 5k | Functions that wrap the windows clipboard for Dos and Windows   
windebug | 4k | Helper functions for use of GDB under Windows. cygdrive + update of running program's windowtitle.   
wini | 12k | Ini file using TP objects   
wnghelp | 13k | Help support for Norton Guide (.NG) files   
woahelp | 15k | Borland OA .HLP reader objects and routines   
wos2help | 19k | Help support for OS/2 (.INF) help files   
wresourc | 22k | (TP?) Resource File support objects and routines   
wtphwrit | 7k | Routines to create .tph files   
[ wutils](<fv_wutils.md> "fv:wutils") | 32k | Utils, mostly collections, string routines and file(name/path) routines.   
wviews | 69k | Enhanced View s(widgets) (special for IDE?)   
wvphelp | 4k | Help support for (.VPH) help files   
wwinhelp | 49k | Help support for Windows Help (.HLP) files   
compunit | 0k | (compiler/ dir) build unit for the compiler.   
gdbcon | 2k | (fakegdb/) fake gdb controller unit to be used when no gdb support is included   
gdbint | 5k | (fakegdb/) fake gdb controller unit to be used when no gdb support is included   
  
Some functionality seems to be split between a w* unit and a fp* unit. (wini,fpini, wviews, fpview). Possibly this is an artifact of an older division between reusable and non reusable units. The fact that there are multiple utility units is also a bit of an indicator for this. 

If you know/find out what the criteria for it is, please add it here. If you find out something about a certain unit, make a link out of it prefixed with fv: (see the wutils example). 

Besides units, there are also a bunch of include files, most of which seem to be part of fpide, and group routines according to menu category (file, edit, compile etc) 

Unit | Size | Unit | Remark   
---|---|---|---  
empty.inc | 0k | test | Part of IDE testproject, not IDE itself.   
fpmansi.inc | 4k | fpide | Ansi dump capability   
fpmcomp.inc | 4k | fpide | Compiler menu entries   
fpmdebug.inc | 8k | fpide | Debug menu entries   
fpmedit.inc | 4k | fpide | Edit menu entries (one proc only)   
fpmfile.inc | 8k | fpide | File menu entries   
fpmhelp.inc | 12k | fpide | Help menu entries   
fpmopts.inc | 48k | fpide | Options menu entries   
fpmrun.inc | 16k | fpide | Run menu entries   
fpmsrch.inc | 8k | fpide | Search menu entries   
fpmtools.inc | 12k | fpide | Tools menu entries   
fpmwnd.inc | 8k | fpide | Window menu entries   
globdir.inc | 4k | MANY | IDE project wide defines, included in 19 files.   
wconstse.inc | 8k | wconsts | English constants.   
wconstsh.inc | 4k | wconsts | Hungarian constants.   
  
### Classchart of objects + FV + IDE

An improvised classchart (objectchart?) of all code containing TP style objects ( unit objects, FV and the IDE source) is available here: [Object Chart (Postscript view with GV)](<http://www.stack.nl/~marcov/objectchart.ps>) or [pdf version, less often updated](<http://www.stack.nl/~marcov/objectchart.pdf>)

## Debugging the IDE

Note that the methods describe here are mostly for *nix. 

### With GDB tty

Normally one would debug the IDE as follows (Unix) 

Terminal Window 1: 
    
    
    tty
    

Example output: 
    
    
    /dev/pts/5
     
    

Terminal Window 2: 
    
    
    gdb ./fp
    tty /dev/pts/5
    r
    

The tty command should then reroute all I/O to tty /dev/pts/5 (Terminal window 1), leaving Terminal Window 2 free to communicate with GDB. 

Unfortunately, due to the special keyboard support of the textmode IDE (to capture all CTRL-ALT variants) input is not properly rerouted. This means you can debug IDE startup (and more if you save state smartly), but not the normal working. This method is mainly useful when debugging the start of the IDE. 

### With GDB attach

This method works better if you want to debug after the IDE has start up. 

Terminal Window 1: 
    
    
    Run the IDE from the command line, and wait till it has start up.  
    

Terminal Window 2: 
    
    
    Find the PID of the running IDE In window 1 using ps aux |grep fp
    Start gdb with the correct "fp" file (the one that is running!)
    Type attach <pid> on the GDB cmdline
    Note that gdb will immediately break upon attach. If you want to carry on, type "c" (continue)
    

### Writeln like

While developing the CHM package I needed to debug browsing also. For this, at first I simply wrote to stderr, and pipe the output to a different terminal (using bash as shell) 

Terminal Window 1 
    
    
    tty
    

Example output: 
    
    
    /dev/pts/5
    

Terminal Window 2 
    
    
    ./fp 2> /dev/pts/5
    

However, while this would work, it seems sometimes buffering is a problem. Adding a flush would help, but then you have to go after all those writelns again. So it is better to extract it to a procedure. Such a procedure already existed in wutils, and I enhanced it a bit. There are two versions now. "DebugMessage" is the old one, and DebugMessageS is the new one. 

To avoid any garbage on the cmdline, all debug messages are always placed under IFDEF. Historically the define "DEBUG" was used for it, but apparantly this is also used in the compiler, and a broken debug feature in the compiler breaks debugging the IDE this way. So I use WDEBUG (mostly in whtmlhlp). The typical debug code fragment then becomes: 
    
    
    {$IFDEF WDEBUG}
      DebugMessageS({$i %file%',' Adding Name "'+Name+'"',%line%,'1');
    {$endif WDEBUG}
    

Note that we use a compiler feature to fill in filename + linenumber for us. 

On Windows: 
    
    
    fp.exe 2> logme.txt
    

### Exceptions to stderr

When debugging the IDE, I often change the RTL to log errors to stderr instead of stdout, since stderr is logged using the above 2> redirects, I use the following patch: 
    
    
     ===================================================================
     --- sysutils/sysutils.inc       (revision 12440)
     +++ sysutils/sysutils.inc       (working copy)
     @@ -234,7 +234,7 @@
        i : longint;
        hstdout : ^text;
      begin
     -  hstdout:=@stdout;
     +  hstdout:=@stderr;
        Writeln(hstdout^,'An unhandled exception occurred at $',HexStr(PtrUInt(Addr),sizeof(PtrUInt)*2),' :');

---

_Source: [https://wiki.freepascal.org/Textmode_IDE_development](https://web.archive.org/web/20250514115010/https://wiki.freepascal.org/Textmode_IDE_development)_
