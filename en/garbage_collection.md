# garbage collection

This page is a placeholder for experiences with trying to shoehorn the commonly used libgc (Boehm-Demers-Weiser conservative garbage collector) for C-like languages into FPC. This can be specially useful for hybrids systems where a scripting language interacts in complex ways with a FPC core. 

Such GC works by examining the datasegment and stack of the system for possible pointer values. Since FPC has the ability to redefine the memory manager, in theory one could slap the GC under the system. 

There have been one or two attempts in the past, but no code was saved from these events as far as known. Some of these were on windows, which makes it harder because the versioning situation of open source libraries on Windows is often more complicated (multiple C runtimes in broad use; several msvcrt, borland, mingw,cygwin). 

One of the possible problems would be that for automated type FPC often keeps references around that do not point to the beginning of the block (e.g. ansistrings), and no reference to the beginning is kept outside the heapmgr. I asked this question to sb more knowledgable in GC matters, and got this answer (verbatim): 
    
    
    If the GC_all_interior_pointers flag is turned off then the GC will probably prematurely free blocks for 
    which there's no pointer to the start, which will cause all manner of problems.  
    This flag is set by  default on my system (Debian GNU/Linux) but you should probably set it  explicitly.
    

So this issue can be worked around, but adds a requirement to the way libgc is initialized or compiled. (depending if the flag is compile or runtime). Since the checking on potential roots is more complicated, it could be that this option is less optimal (and less safe) than when the GC only has to view pointer values that point to the beginning of a block as potential roots. 

Note that storing the size in the "externally" allocated blocks and advancing the pointer might have similar problems. 

# optimization

Afaik libgc initialized the memory it hands out. FPC/Delphi also do this for some types, and some redundant initializations in the RTL could be removed. Maybe if the libgc option has proven itself, these can be put under some define, to allow easy recompilation for libGC. 

# Threading

It seems that there are multiple libgc versions with respect to collection: non-threadsafe, threadsafe but blocking and threadsafe performance. This probably has to be researched too. 

# Links

  * [an old post](<http://www.hu.freepascal.org/lists/fpc-pascal/2002-November/004229.html>)
  * [gc under Delphi](<http://cc.embarcadero.com/Item/21646>)
  * [SO GC and delphi](<http://stackoverflow.com/questions/415958/how-to-automatically-free-classes-objects>)

---

_Source: [https://wiki.freepascal.org/garbage_collection](https://web.archive.org/web/20190519174858/https://wiki.freepascal.org/garbage_collection)_
