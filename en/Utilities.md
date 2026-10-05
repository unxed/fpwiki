# Utilities

│ **[Deutsch (de)](</Utilities/de> "Utilities/de")** │  **English (en)** │  **[français (fr)](</Utilities/fr> "Utilities/fr")** │  **[magyar (hu)](</Utilities/hu> "Utilities/hu")** │  **[Bahasa Indonesia (id)](</Utilities/id> "Utilities/id")** │  **[русский (ru)](<../ru/Utilities.md> "Utilities/ru")** │    
****

## Contents

  * 1 h2pas
  * 2 fpcmake
  * 3 fpmake
  * 4 ppdep
  * 5 delp
  * 6 ppudump
  * 7 ppufiles
  * 8 ppumove
  * 9 ptop
  * 10 rstconv
  * 11 tply
  * 12 pas2jni



## h2pas

is a small command-line utility that can be used to translate C header files to pascal units. The Free Pascal team uses it to make import units for important C libraries such as GTK or MySQL. 

## fpcmake

is a tool that allows you to make complex makefiles to compile programs and units with FPC. The Free Pascal team uses it to create all it's makefiles. ( See [Fpcmake](<Fpcmake.md> "Fpcmake") ) 

## fpmake

is a tool that replaces the combination of fpcmake and (GNU) make in the build process. This tool is replacing fpcmake+make in FPC 2.7.x (See [FPMake](<FPMake.md> "FPMake")) 

## ppdep

is a small utility that scans a program or unit and creates a depend file that can be used for inclusion by make. It understands conditional symbols and interdependency of units. 

## delp

is a small utility that scans a directory for files left over by the Free Pascal compiler, and deletes them. 

## ppudump

dumps the contents of a unit in human-readable format. It understands older versions of units and gracefully handles unknown (future) versions. 

## ppufiles

lists the object files that you need to link in when using a unit file. It lists all libraries and units that are needed. 

## ppumove

combines several units into one; as such it can be used to create static and dynamic libraries. 

## ptop

is a configurable source formatter. It pretty-prints your pascal code, much like indent does for C code. See [PTop](<PTop.md> "PTop")

## rstconv

is a small utility that converts `.rsj` files (files that contain [resource strings](<Using_resourcestrings.md> "Using resourcestrings"), as created by the compiler) to some other format. 

## tply

[TP Lex and Yacc](<Plex_and_Pyacc.md> "Plex and Pyacc"), written by Albert Graef. It can be used to create pascal units from a Lex vocabulary and Yacc grammar. 

## pas2jni

The [pas2jni](<pas2jni.md> "pas2jni") utility generates a JNI (Java Native Interface) bridge for a Pascal code. Then the Pascal code (including classes and other advanced features) can be easily used in Java programs.

---

_Source: [https://wiki.freepascal.org/Utilities](https://web.archive.org/web/20240121090706/https://wiki.freepascal.org/Utilities)_
