# OpenMP support

## Contents

  * 1 What is OpenMP?
  * 2 Pascal syntax for OpenMP
    * 2.1 Proposal 1
      * 2.1.1 Foreword
      * 2.1.2 Syntax vs. Compiler directives
      * 2.1.3 parallel
      * 2.1.4 parallel for
      * 2.1.5 data sharing attributes
        * 2.1.5.1 threadprivate
    * 2.2 Proposal 2: Using local functions
      * 2.2.1 parallel
    * 2.3 Proposal 3
    * 2.4 Proposal 4
    * 2.5 See also



## What is OpenMP?

OpenMP is an API accessed by language directives to do multi threaded programming, see also <http://www.openmp.org>. Currently, there is only OpenMP syntax defined for C and Fortran. This page tries to collect some stuff to settle down pascal syntax for it. 

## Pascal syntax for OpenMP

### Proposal 1

#### Foreword

At first, I must admit that some parts of the OpenMP specification I still don't understand. They did a terrible good job throwing away all common terms ever used in multi threading context, and invented their own ones. 

#### Syntax vs. Compiler directives

OpenMP for C and C++ is implemented by using compiler directives mainly due to the reasons of source code compatibility (or: standards compliance). So a _conforming_ program is intended to behave the same regardless if the actual compiler compiling the program supports those special pragmas or not. 

For FreePascal I don't think this is the way to go, because first it changes comments into code and second, it makes the program far less readable. For C programs this doesn't seem to be an issue, if you get my meaning. But in my opinion, readability is a far more important issue than compatibility to older/different compilers. If all else fails, a preprocessor could be provided to strip out the parallel specific stuff, as has been suggested by Marco. Note that you would need this preprocessor for the directives too because older FPCs and Delphi don't skip unknown directives. 

Well, enough talk, I start with the easier directives which are luckily the more fundamental ones. 

Ok, I got more input than I'd expected and less time than I wished. :) Anyway, against my own objection, the idea of enclosing the parallel code into (local) functions looks very appealing, so I've changed the example accordingly. 

#### parallel

The `**parallel**` construct can only be used for a structured block. That means in Pascal it should be enclosed in some sort of `**begin**`/`**end**` pair anyway - so, as it has been suggested, we could use a (in this particular example non-local) function instead. Though, I don't know yet, if this may bite with other parts of the spec as this is evolving. Let's try: 

(Original example A.4.1.c of the OpenMP V2.5 specification): 
    
    
      **procedure** SubDomain (**var** x       : **array of** Float;
                               istart  : Integer;
                               ipoints : Integer)
      **var**
         i : Integer;
      **begin**
         **for** i := 0 **to** ipoints - 1 **do**
            x[istart + i] := 123.456;
      **end** _{SubDomain}_ ;
      
      **parallel** **procedure** Sub (**var** x : **array of** Float);
      _// Variables declared here have_**private** _context._
      _// So each instance of the parallel function has its own set, as usual._
      **var**
         iam     : Integer;
         nt      : Integer;
         ipoints : Integer;
      _// Any variable access outside of the function's scope accesses the variable in_
      _// a_**shared** _context._
      _// This might prove problematic, especially because it causes special semantics_
      _// on the function's parameters, probably depending on the parameter mode or worse:_
      _// On the calling convention actually used (call-by-value vs. call-by-reference)._
      **begin** _// of (possibly) parallel section_
         iam := OMP.Get_Thread_Num;  _// OMP library calls._
         nt  := OMP.Get_Num_Threads;
            
         ipoints := Length (x) **div** nt; _// size of partition_
         istart  := iam * ipoints;     _// starting array index_
            
         **if** iam = Pred (nt) **then**
            ipoints := Length (x) - istart; _// last thread may do more_
            
         SubDomain (x, istart, ipoints);
      **end** _{Sub}_ ;
      
      **var**
         arr : **array**[0 .. 9999] **of** Float;
      **begin** _// Main program_
         Sub (arr);
      **end**.
    

I don't like the idea of declaring variables inside the actual statments, this looks very unpascalish. Maybe we can find a way around it. --[FPK](</User:FPK> "User:FPK") 10:22, 26 July 2006 (CEST) 

I agree with Florian that this is not the way to go. Why not require all parallelizable code to be in local functions ? After all, that's almost what you are doing: declaring a local function. That would be a simple extension of the current syntax. You have access to all local variables; all you'd need is to add a parallel keyword to the local function declaration. 

* * *

Ok, so what do you think about the changed example above? OpenMP really is about coarse grain parallelism, so I see indeed no strong reason, why parallel blocks shouldn't be enclosed in procedures. Parallel functions obviously do not make sense, as every thread could return its own return value, but the block calling the parallel function can only evaluate one. I would have liked the notion of a local block, though (I'm quite used to it), but as I seem to be the only one... \--[V.hoefler](</User:V.hoefler> "User:V.hoefler") 21:03, 27 July 2006 (CEST) 

How would a try/finally type blocking approach work? (No begin needed) Like, 
    
    
     **Parellel**
       SomeCodeHere;
       SomeCodeHereToo;
     **End** ; _{ Parellel Block }_
    

Very simple, just as easy when we first discovered Exception handling. \--[Raid](</index.php?title=User:Raid&action=edit&redlink=1> "User:Raid \(page does not exist\)") 19:11, 1 September 2009 (CEST) 

#### parallel for

This is simply a parallel for-loop. There's nothing special to it. Although OMP2.5 states a for-loop iteration variable is `**private**` in that construct, which I consider rather redundant, I hardly can imagine correctly behaving code with a `**shared**` loop iteration variable. It also places some restrictions onto the allowed loop-statements (no change of iteration variable inside the loop, simple iteration constructs, ...), but these are already implemented in the language, so there's no need to elaborate on that much further. 

(Example A.1.1c): 
    
    
      **procedure** a1 (      n : Integer;
                    **const** a : **array** **of** Float;
                    **var**   b : **array** **of** Float);
      **var**
         i : Integer;
      **begin**
         **parallel** **for** i := Succ (Low (a)) **to** High (a) **do**
            b[i] := (a[i] + a[i - 1]) / 2.0;
      **end** _{a1}_ ;
    

That's it. Now probably someone sees the reason why I wouldn't use the `**parallel**` keyword as function modifier like `**inline**` or `**cdecl**` are used, but rather prepend it to the function header itself. I think, it's a more consistent usage of a new keyword. \-- [V.hoefler](</User:V.hoefler> "User:V.hoefler") 21:17, 27 July 2006 (CEST) 

#### data sharing attributes

To me these seem quite complex constructs considering that most of the time you probably won't need it at all, because the default is fine and follows normal programming logic. So if anyone has an idea, if and why we need to support them explicitely, here's the place. 

##### threadprivate

This attribute closely resembles, what FreePascal already knows as `**threadvar**`, so I even think, we can reuse this keyword here. I see some semantic issue though: 

The OMP2.5 specification states: 

> The values of the data in the threadprivate objects of threads other than the initial thread are guaranteed to persist between two consecutive parallel regions only if **all** the conditions hold: 

and then follows a list of condition, which basically state, that the number of threads in both sections must be the same. 

So to write some simple pseudo code to demonstrate: 
    
    
    **procedure** Thread_Vars;
    **threadvar**
       Count : Integer;
    **var**
       i     : Integer;
    **begin**
       Count := 0; _// initial state_
    
       **parallel** **for** i := 1 **to** SOME_VALUE **do**
          Count := Count + 1;
    
       _// Point A_
    
       **parallel** **for** i := 1 **to** SOME_OTHER_VALUE **do**
          Count := Count + 2;
    
       _// Point B_
    
    **end** _{Thread_Vars}_ ;
    

Now, each iteration of the loop is executed in parallel, so each copy also gets its own copy of `Count`. At _Point A_ after the loop, `Count` would equal `1`, because if each loop iteration was executed by a single thread, the incrementing operation would have happened only once (per thread). Let aside the question _which_ copy is seen after the loop, the thing gets more interesting. What value is seen at _Point B_? Well, if I understood the specification correctly, the value would be `3` if and only if the actual values of the place-holding constants `SOME_VALUE` and `SOME_OTHER_VALUE` are equal. In any other case, the value of `Count` at _Point B_ would be undefined. 

### Proposal 2: Using local functions

Instead of using new block types (like parallel), it uses a nested procedure, with the **parallel** modifier. 

I think we could use a **sequential** keyword for the sub() procedure. More on talk page. -- [MarkMLl](</User:MarkMLl> "User:MarkMLl") 12:46, 9 December 2007 (CET) 

#### parallel
    
    
      **procedure** SubDomain (**var** x       : **array of** Float;
                               istart  : Integer;
                               ipoints : Integer); 
      **var**
         i : Integer;
      **begin**
         **for** i := 0 **to** ipoints - 1 **do**
            x[istart + i] := 123.456;
      **end** _{SubDomain}_ ;
      
      **procedure** Sub (**var** x : **array of** Float);
     
        **procedure** ParallelBlock; **parallel** ;
        **var**     
            iam     : Integer;
            nt      : Integer;
            ipoints : Integer;
        **begin**
             iam := OMP.Get_Thread_Num;  _// OMP library calls._
             nt  := OMP.Get_Num_Threads;
             
             ipoints := Length (x) **div** nt; _// size of partition_
             istart  := iam * ipoints;     _// starting array index_
             
             **if** iam = Pred (nt) **then**
                ipoints := Length (x) - istart; _// last thread may do more_
            
             SubDomain (x, istart, ipoints);
        **end** ;
    
      **begin**
           ParallelBlock;
      **end** _{Sub}_ ;
      
      **var**
         arr : **array**[0 .. 9999] **of** Float;
      **begin** _// Main program_
         Sub (arr);
      **end**.
    

### Proposal 3

`**parallel**`, `**future**`, and `**async**` keywords like implemented in the "Oxygen" Pascal dialect. Oxygen uses features of the CIL (aka ".NET") framework to implement this. (IMHO this does not qualify the paradigm of these keywords as bad.) With FP, this can be implemented in native code in the RTL. 

see 
    
    
    - <http://wiki.oxygenelanguage.com/en/Parallel_Loops>
    - <http://wiki.oxygenelanguage.com/en/Futures>
    - <http://wiki.oxygenelanguage.com/en/Asynchronous_Statements>
    

### Proposal 4

Benefit from the efforts of Modula-2+ and Modula-2* and maybe use (or build upon) their ideas. 

  


### See also

  * [ThreadEvent](<ThreadEvent.md> "ThreadEvent")
  * [Proposal of a Widget Set independent Event Queue implementation](<Proposal_of_a_Widget_Set_independent_Event_Queue_implementation.md> "Proposal of a Widget Set independent Event Queue implementation")

---

_Source: [https://wiki.freepascal.org/OpenMP_support](https://web.archive.org/web/20250425034516/https://wiki.freepascal.org/OpenMP_support)_
