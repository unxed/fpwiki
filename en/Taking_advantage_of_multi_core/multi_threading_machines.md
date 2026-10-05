# Taking advantage of multi core/multi threading machines

Please discuss at the discussion page. 

## Contents

  * 1 Introduction
  * 2 Making the language multi core ready
  * 3 Libraries
  * 4 Multi threading in the compiler
  * 5 Build system
  * 6 See also



## Introduction

During the past years, the focus of the development of FPC was on making FPC multi platform. This goal has been successfully reached and FPC is available on all major platforms. So we need new challenges. 

Looking at the roadmaps of AMD and Intel, you'll see that future processors won't get performance gains by higher clock-rates as it was during the last decades. Instead, the CPUs will be multi core. However, multi core processors require new approaches to take advantage of these cores. So, I consider this as one of the main task for the next years: making FPC ready for multi core systems. 

This must be done on several levels: 

  * language
  * libraries
  * compiler
  * build system



## Making the language multi core ready

I am afraid that this requires a lot of discussion: which new language elements can be added to make building multi threaded applications easier. 

e.g. 

  * monitor classes
  * syntax already provided by Delphi Prism (parallel loops, future variables)



## Libraries

This task is almost finished. Most libraries are multi threading ready. Some things like the heap manager need tuning, but that's it. 

## Multi threading in the compiler

This is a long term task, making the compiler multithreaded, i.e. start different threads to compile several units at once. 

## Build system

This should be rather easy with proper package dependencies: the building of different packages being independent can be done simultaneously. (who determines this? Does this mean that the compiler should know about packages and their dependencies? Package groups?) 

All this also requires a common resource management, i.e. how much threads/processes should be spawned to take optimum advantage of multiple cores. It makes no sense to start building 10 packages at once on a dual core system. This only causes cache thrashing etc. and slows down things. 

Furthermore, e.g. for the build system, it would be interesting to estimate the time that is needed to build a certain package. It would allow better planning in how dependent packages should be build. 

## See also

  * [OpenMP support](<../OpenMP_support.md> "OpenMP support")
  * [Proposal of a Widget Set independent Event Queue implementation](<../Proposal_of_a_Widget_Set_independent_Event_Queue_implementation.md> "Proposal of a Widget Set independent Event Queue implementation")

---

_Source: [https://wiki.freepascal.org/Taking_advantage_of_multi_core/multi_threading_machines](https://web.archive.org/web/20250113190426/https://wiki.freepascal.org/Taking_advantage_of_multi_core/multi_threading_machines)_
