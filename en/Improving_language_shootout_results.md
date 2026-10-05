# Improving language shootout results

## Contents

  * 1 About
  * 2 Goals
  * 3 Way to go
  * 4 Benchmarks notes
    * 4.1 SSE2 and comparions
    * 4.2 Binary trees
    * 4.3 Mandelbrot
    * 4.4 N-body



## About

The computer language shootout (<https://benchmarksgame-team.pages.debian.net/benchmarksgame/>) is a realistically flawed benchmarking system which compares many languages on a somewhat unequal basis. See the linked page for more info. 

## Goals

Our goals are to be on the highest possible positions. The requirements to reach them can be categorized into two. 

1: Optimizations on the assembler level. (core devel work) 

2: Optimizations of the benchmarks. (junior devel work) 

## Way to go

It was decided that to keep memory requirements low it's best to use val() and str() and never include SysUtils unless really required. 

Use of inline and records instead of classes can also improve performance considerably, but additional testing and comparing with C and other languages should be the main factor. Special note: inlining of recursive functions is possible and sometimes increases speed. 

Another thing to use is {$implicitexceptions off} to keep rtl hindering speed. Dropping down to pchar level in tight loops can be beneficial. Use also only native sized integers and floats if possible. 

## Benchmarks notes

### SSE2 and comparions

Consider the following code compiled with -Cfsse2: 
    
    
    const 
      limit=4.0;
    var 
      x : double;
    begin
      x := 1.73;
      if x < limit then
        x := x + x;
    end.
    

For the condition **x <limit**, no sse2 code will be generated, because the compiler assumes the const is an extended and therefore needs to perform the comparison with the x87 FPU unit. 
    
    
    const 
      limit = double(4.0);
    

... also doesn't work. However 
    
    
    const 
      limit: double = double(4.0);
    

does. 

### Binary trees

~~The[binary trees benchmark](<https://benchmarksgame-team.pages.debian.net/benchmarksgame/performance/binarytrees.html>) can be improved using a TNonFreePooledMemoryManager available in fpc 2.1.1. That way the allocation of nodes occurs in blocks and all de-allocation happens all at once for all nodes of the tree. Look for an example [here](<http://www.hu.freepascal.org/fpcircbot/cgipastebin?msgid=126>). Such an allocator probably won't be allowed though.~~

FPC is currently in first place for this benchmark, and indeed TNonFreePooledMemoryManager (plus PasMP) were what did the trick. 

### Mandelbrot

~~The[mandelbrot benchmark](<https://benchmarksgame-team.pages.debian.net/benchmarksgame/program/mandelbrot-fpascal-8.html>) can be improved with fpc 2.1.1 using the sse2 instructions. Note that sse2 instructions are volatile and the results of a calculation will be put back on the stack, if the same body contains a procedure call. Therefore, a sse2 calculation needs to be moved to a separate (nested) procedure. The mandelbrot benchmark would probably benefit from [SSA](<Single_assignment.md> "Single assignment") in loops, because without it calculations are performed in a scratch register and the result moved to the final register. SSA will prevent this. At the moment fpc 2.1.1 supports SSA only in simple linear flow control without loops and branches. ~~

~~The latest versions of the competition gain a lot (nearly a factor of 2) by using 128 bit xmm registers for cr, ci, tr, and ti and instructions to do 2 calculations at the same time. I do not see a way to do this with fpc. Please prove me wrong :-)~~

FPC's score in this benchmark is currently much better than it was previously (formerly 22.69 seconds, currently 8.52 seconds). Optimization improvements in FPC 3.2.0 versus the current 3.0.4 being used are likely to improve the time even further. 

### N-body

~~The[n-body benchmark](<https://benchmarksgame-team.pages.debian.net/benchmarksgame/performance/nbody.html>) can be improved with fpc 2.1.1 using the sse2 instructions and factoring out common factors. Compiling with -Cfsse2 and fpc 2.0.4 gives an internal error.~~

FPC 2.1.1 is now very outdated and SSE2 works just fine, but this benchmark still needs improvement in general.

---

_Source: [https://wiki.freepascal.org/Improving_language_shootout_results](https://web.archive.org/web/20230530094635/https://wiki.freepascal.org/Improving_language_shootout_results)_
