# Debugging - GDB tricks

## Contents

  * 1 General tricks
    * 1.1 Examining Type Info
    * 1.2 Inspecting an ansistring
    * 1.3 Stop debugger on exception
    * 1.4 Work with breakpoints
    * 1.5 Pointer
    * 1.6 Accessing a symbol in a specific unit unitname.identifier



# General tricks

  * Some pascal specific patches were accepted into [GDB](<GDB.md> "GDB"), so if possible set language to pascal "set language pascal"
  * To get help, use "help <command>" or "help" to see a category list.
  * To view datastructures in their raw form, use the "x" command (see "help x")



## Examining Type Info

  * Q: What function should I use for dumping the type info?
  * A: Run x/100c in gdb until it frames. then use 'frame <framenr>' to jump to the correct frame



## Inspecting an ansistring

  * Q: How do I examine an ansistring?
  * A: "x/s <stringname>"



## Stop debugger on exception

  * Q: How to stop gdb on exception?
  * A: b fpc_raiseexception


  * Q: How to stop gdb on assert, and have a backtrace?
  * A: b fpc_assert


  * Q: How to show call stack/backtrace?
  * A: bt



## Work with breakpoints

  * Add: b TObject__Method or b filename.pas:123
  * Show: inf b
  * Disable: dis 1
  * Enble: en 1
  * Delete: d 1
  * Delete all: d b



## Pointer

When you print the value of a pointer gdb will say something like: 
    
    
     $0 = (MYPTR) 0x0202
    

If MYPTR is the correct type, you can just use 
    
    
     p ptrvar^
    

if it's not the correct type, you can typecast it: 
    
    
     p realptrtype(ptrvar)^
    

If you do not have a separate type for the actual pointer type, but only a type for what it points to, then the syntax is somewhat awkward because the gdb parser is still showing its C heritage in that case: 
    
    
     p ^myvartype(ptrvar)^
    

## Accessing a symbol in a specific unit unitname.identifier

If you have several global variables of the same name in different units, GDB will *not* follow pascal scoping rules, but pick whatever it finds first. So you need to specify which one you want. 

GDB does not know the unitname.ident syntax. But it can instead take the filename of a unit. (filenames with a dot must be in single quotes) 
    
    
    'unit1.pas'::form1

---

_Source: [https://wiki.freepascal.org/Debugging_-_GDB_tricks](https://web.archive.org/web/20241212105356/https://wiki.freepascal.org/Debugging_-_GDB_tricks)_
