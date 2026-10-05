# Internal Linker

Good information about linkers and loaders can be found in the book Linkers and Loaders at <http://www.iecc.com/linker/>

Steps 

  * Get familiar with the fpc sources
  * Analyse what is available in the fpc sources. A couple of years ago a basic linker for go32v2 was already working.
  * Start coding, the first thing to add is a scriptable memory/file layout, like the ld-scripts.
  * Add PE coff support
  * Add static ELF support
  * Add shared ELF support
  * Add optimizations for smartlinking
  * Add internal object format
  * Keep an eye on memory usage and overall speed.

---

_Source: [https://wiki.freepascal.org/Internal_Linker](https://web.archive.org/web/20250115202111/https://wiki.freepascal.org/Internal_Linker)_
