# chm

## Contents

  * 1 Overview
  * 2 Package status
  * 3 Units
  * 4 Files
    * 4.1 chmls
  * 5 Missing features
  * 6 Implemented features
    * 6.1 Disable binary toc/index generation
    * 6.2 How to enable threaded LZX Compression
  * 7 See also



## Overview

The FPC CHM package is a set of units distributed with FPC 2.2.2+ that allow to read/write .chm compressed HTML help files on multiple platforms. 

There are two projects that use the CHM package: 

  1. the textmode IDE (FPC 2.2.4+)
  2. the Lazarus [chmhelp](<chmhelp.md> "chmhelp") package (which features lhelp, a separate help viewer, connected over TCPIP). The original author is Andrew Haines, but some parts are by others (specially Lars/z505).



The write aspect is mostly used in combination with [fpdoc](<FPDoc_Editor.md> "FPDoc Editor"), a doxygen like documentation tool, and [chmcmd](<chmcmd.md> "chmcmd") the commandline compiler. 

It is expected that in the next FPC release, chm will replace the doc-html archive with its thousands of separate htmls. 

The package is pure Pascal, and thus portable in principle: 

  * The package is prepared for different endianness, but has not been extensively tested on Big endian systems
  * No testing has been done on systems that generate an exception on unaligned memory access (like arm-wince). Specially the reader parts should be made `aligned()` clean.



The package also packages two examples: 

  1. chmls
  2. [chmcmd](<chmcmd.md> "chmcmd") (see below)



## Package status

The basic system works since 2.2.4. However keep in mind that the FPC help systems, while massive (30000+ webpages in the various chms) are generated, compiled and read mostly by FPC tools (with an occasional test in Windows/gnochm/kchmviewer to see if it opens properly). 

If you want to use FPC's chm package/compiler for your own projects, take the follow advice under consideration: 

  * Work on data that is backed up. I haven't lost data yet, but better be safe than sorry.
  * Don't expect it to work right away.
  * Keep in mind what is implemented and what is not (see below)
  * report bugs in the FPC tracker with a reproducible description (if you are in an hurry with patch :_)



## Units

  1. chmbase - some structures, constants and helper functions (compare and compression streaming helpers)
  2. chmfiftimain - the unit that is responsible for reading and writing the search index of CHMs (TChmSearchReader, TChmSearchWriter)
  3. chmfilewriter - TChmProject, a class that describes a CHM project in the form of a XML description. Can be used to generate CHMs easily.
  4. chmreader - the base CHM Reader
  5. chmsitemap - Support for sitemaps (TOC - table of contents - and index) The unit helps transforming these XML files to collections.
  6. chmspecialfiles - Streaming helpers for special index files in the chm
  7. chmtypes - CHM helper class types.
  8. chmwriter - the base CHM Writer.
  9. fasthtmlparser - Base skeleton of an HTML parser.
  10. htmlindexer - Contains classes that TChmWriter uses to parse and index the contents of HTML files for searching
  11. htmlutil - helper functions. Mostly getting info out of HTML/XML tags.
  12. lzxcompressthread - wrapper around paslzxcomp that will do compression in multiple threads
  13. paslznonslide - (de)compression routines
  14. paslzxcomp - (de)compression routines
  15. paslzx - (de)compression routines



## Files

This describes the FPC 2.6+ situation. 

Besides the library units, there are also two end user programs: 

  * chmls - listing, extracting and unblocking (needed on Windows XP SP2+ and presumably newer Windows versions) of a chm
  * [chmcmd](<chmcmd.md> "chmcmd") \- creates a CHM using a XML file made with the TCHMProject class. Roughly equivalent to a simple help compiler. Experimental support for .hhp is also available.



chmcmd used to be very rough in pre 3.0.0 times, but hhp usage is on firmer ground now, and with 3.2.0 the binary sitemap support has been rewritten and the (binary) indexes and tocs generated from HHPs are better now. It still does not support goodies like autoindexing, since it is mostly used as a backend for tools that already generate hhc/hhk themselves. (iow I rather like that to be in the workshop part rather than the compiler) 

### chmls

Help output of chmls (FPC trunk, September 2014): 
    
    
    chmls, a CHM utility. (c) 2010 Free Pascal core.
    
    Usage: chmls [switches] [command] [command specific parameters]
    
    Switches :
     -h, --help     : this screen
     -p, --no-page  : do not page list output
     -n,--name-only : only show "name" column in list output
    
    Where command is one of the following or if omitted, equal to LIST.
     list       <filename> [section number]
                Shows contents of the archive's directory
     extract    <chm filename> <filename to extract> [saveasname]
                Extracts file "filename to get" from archive "filename",
                and, if specified, saves it to [saveasname]
     extractall <chm filename> [directory]
                Extracts all files from archive "filename" to directory
                "directory"
     unblockchm <filespec1> [filespec2] ..
                Mass unblocks (XPsp2+) the relevant CHMs. Multiple files
                and wildcards allowed
     extractalias <chmfilename> [basefilename] [symbolprefix]
                Extracts context info from file "chmfilename"
                to a "basefilename".h and "basefilename".ali,
                using symbols "symbolprefix"contextnr
     extracttoc <chmfilename> [filename]
                Extracts the toc (mainly to check binary TOC)
     extractindex <chmfilename> [filename]
                Extracts the index (mainly to check binary index)
     printidxhdr <chmfilename>
                prints #IDXHDR in readable format
     printsystem <chmfilename>
                prints #SYSTEM in readable format
     printwindows <chmfilename>
                prints #WINDOWS in readable format
     printtopics <chmfilename>
                prints #TOPICS in readable format
    

## Missing features

(the bulletlist is old) 

  * merged chm support/ #idxhdr 
    * Combining of indexes of multiple chm files. (.CHW/.CHI?)
    * slave file support ? (making slave CHMs, and getting them out, will make for easy master/toc CHMs later)
  * Writing of binary indexes is multiple level, loading not. (???)
  * Expand TCHMProject to support 
    * ~~aliases. 2.4.2+ (via .hhp in 2.4.4+)~~
    * ~~control over textual/binary toc generation, textual/binary index generation etc from the XML/project (2.4.2+)~~
    * ~~also support .ini (MS helpcompiler compat?) (2.4.4+)~~
    * lcid/language (?)
    * ~~Window list (in progress, probably 2.4.4+)~~
  * Alinks
  * auto index/toc(?) generation (maybe better keep this in the GUI tooling?)
  * (popup) comments
  * samples (fpdoc?) Make samples extractable, MSDN style.
  * exclusion lists (stoplists) for wordindexing. 
    * why is a MS generated index larger while we also index words like "the" and "of" ? Possible reason: probably the parser doesn't see all text.



So the major missing feature is master/slave CHMs. I tried to fix this several times, but the Windows reader doesn't seem to accept it. (neither slave nor master, I mostly worked on slave support. 

If somebody has more knowledge about this subject , don't hesitate to contact me (e.g. via the FPC bugtracker or lazarus forum. Add CHM in the subject to make sure I see it ) 

## Implemented features

  * Full text search
  * Binary Table of contents
  * Binary Index (experimental, completely rewritten in 3.2.0+)
  * Experimental threaded LZX Compression
  * context ids.
  * threaded compilation (requires recompile, see below)
  * windows support
  * toc and indexfile are not always "default.hh[c/k]"
  * chmcmd and chmls totally revised, and chmcmd now also compiles .HHPs.



  


### Disable binary toc/index generation

Generating binary versions of the TOC and/or index can be disabled in [fpdoc](<FPDoc_Editor.md> "FPDoc Editor") respectively with `--no-bintoc` and `--no-indextoc`. 

Note that these options were not added to save space, but to be able to temporarily not generate them if some viewer has a problem with them, and refuses to fall back to default. 

One can notice that kchmviewer loads chms with binary index and TOC significantly faster. 

### How to enable threaded LZX Compression

If you want to test multithreaded chm compression add `-dLZX_USETHREADS` when compiling the package (or the whole snapshot). 

The program using the package (fpdoc must have cthreads added in its uses section on *nix. 

On my Core2 1.73GHz laptop this reduced time from 2min 57s to 2m 7s, in other words a reduction by slightly less that a third. 

It is unknown how to configure the number of parallel threads. 

## See also

  * [chm backend for fpdoc](<chm_backend_for_fpdoc.md> "chm backend for fpdoc")
  * [Textmode IDE development#Chm Support](<Textmode_IDE_development.md> "Textmode IDE development")
  * Lazarus [chmhelp](<chmhelp.md> "chmhelp") component
  * [CheckHelpLinks](<http://www.codeproject.com/Articles/7997/Check-Help-Links-Tool>) Tool that displays links etc in CHM files; open CHM (suggest using Fast mode to avoid crashes) and File/Work to check links
  * [Unofficial CHM file specs](<http://www.nongnu.org/chmspec/latest/>)



Go to back [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/chm](https://web.archive.org/web/20250417151044/https://wiki.freepascal.org/chm)_
