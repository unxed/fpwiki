# Module handling rewrite

This page describes the planned unit handling rewrite. It is done in a branch in svn, the url of the branch is <http://svn.freepascal.org/svn/fpc/branches/unitrw/compiler>

While implementing this, it should be taken care of running this in multiple threads in the middle/far future. 

## Task list

  * open file
  * Parse header
  * Parse uses
  * Parse interface
  * parse impl. header
  * parse impl. uses


  * load unit 
    * find unit
    * load ppu header
    * load ppu interface
    * load ppu impl.


  * write ppu
  * write object file



## Task queue

  * each task gets a two lists: task which must be processed before itself and tasks which depend on itself
  * each task class gets a counter each time a new task to process is search, that counter is increased when searching a task to process,
  * the queue is walked if a task without empty dependencies is found, it is moved to the end of the queue
  * if a task is found with the same number as the current queue walking, the dependencies can't be resolved => error
  * if a task with an empty dependencies list is found, it is executed after it is finished, it gets removed from the dependency lists of tasks waiting for it



## Misc

Two lesser problems with the module system: 

  * Currently there is no way to have a set of units in one (or two) files. It would clean up tremendously if the all .ppu's and all .o/.a's could be in one file (or one file each), and the need for a dir structure to store all units would go away.
  * Currently there is no scheme to efficiently have a different set of units in the path. E.g. .ppu's suitable for dynamic linking, units that are a result of devirtualization attempts etc, short of have separate hierarchies. I personally didn't like the oder attempts (shared/ and smart/ directory) with separate hierarchies.

---

_Source: [https://wiki.freepascal.org/Module_handling_rewrite](https://web.archive.org/web/20250323220043/https://wiki.freepascal.org/Module_handling_rewrite)_
