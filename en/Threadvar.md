# Threadvar

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The reserved word **threadvar** belongs to thread programming. The declared (known) variable is visible to all functions and procedures of this thread. 

A global variable can be declared thread-local by replacing the [Var](<Var.md> "Var") keyword at the start of the variable declaration block with **Threadvar**. 

Example: 
    
    
      ...
      threadvar x : Integer ;
      ...
    

If no threads are used, the variable behaves as an ordinary variable. If threads are used then a copy is made for each thread (including the main thread). Note that the copy is made with the original value of the variable, not with the value of the variable at the time the thread is started. 

Threadvars should be used sparingly: There is an overhead for retrieving or setting the variable’s value. If possible at all, consider using local variables; they are always faster than thread variables. 

Threads are not enabled by default. **{$mode objfpc}** enables threads.

---

_Source: [https://wiki.freepascal.org/Threadvar](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Threadvar)_
