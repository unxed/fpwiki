# Parallel procedures

│ **[Deutsch (de)](</Parallel_procedures/de> "Parallel procedures/de")** │  **English (en)** │  **[日本語 (ja)](</Parallel_procedures/ja> "Parallel procedures/ja")** │  **[русский (ru)](<../ru/Parallel_procedures.md> "Parallel procedures/ru")** │    
****

## Contents

  * 1 Overview
  * 2 Getting MTProcs
  * 3 Simple example
  * 4 Features
  * 5 Overhead, slow down
  * 6 Setting the maximum number of threads for a procedure
  * 7 Wait for index / executing in order
  * 8 Exceptions
  * 9 Synchronize
  * 10 Example: Parallel loop
    * 10.1 The original loop
    * 10.2 Splitting the work
    * 10.3 Local and shared variables
    * 10.4 DoParallel
  * 11 Example: parallel sort
  * 12 Using nested procedure
  * 13 See also



# Overview

This page describes how to run single procedures in parallel using the MTProcs unit, which simplifies running procedures in parallel and simplifies implementing parallel algorithms. 

Parallel procedures and methods are often found in parallel algorithms and some programming languages provide built-in support for them (e.g. OpenMP in gcc). See [OpenMP support](<OpenMP_support.md> "OpenMP support") for the plans adding such language features to FPC. Embedding these things into the language can save some typing and allows the compiler to generate code with less overhead. On the other hand there are many ways to convert a single threaded piece of code into parallel code. In fact simple approaches often slows down the code. To get good results one must specify some parameters, which a compiler can not guess. For examples see the vast amount of settings and discussions of [OpenMP](<http://openmp.org/wp/>) and [OpenCL](<http://www.khronos.org/opencl/>). You need parallel algorithms. MTProcs helps to implement parallel algorithms. 

# Getting MTProcs

The unit **mtprocs.pas** is part of the **multithreadprocslaz.lpk** package, which needs no other packages, only FPC >= 2.6.0. 

You can find its sources either on sourceforge 
    
    
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/multithreadprocs multithreadprocs

Or in Lazarus _components/multithreadprocs_. 

As always: Open the package multithreadprocslaz.lpk in the IDE once, so that it learns the path. To use the package in your project: Either use _IDE Menu / Package / Open recent package / .../multithreadprocslaz.lpk / More / add to project_ to use it in your current project. Or add it via the project inspector. 

# Simple example

Here is a short example, that does not do anything useful, but to demonstrate how a parallel procedure looks like and how it is called: 
    
    
    program Test;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}
      cthreads, cmem,
      {$ENDIF}
      MTProcs;
    
    // a simple parallel procedure
    procedure DoSomethingParallel(Index: PtrInt; Data: Pointer; Item: TMultiThreadProcItem);
    var
      i: Integer;
    begin
      writeln(Index);
      for i:=1 to Index*1000000 do ; // do some work
    end;
    
    begin
      ProcThreadPool.DoParallel(@DoSomethingParallel,1,5,nil); // address, startindex, endindex, optional data
    end.
    

The output will be something like this: 
    
    
    2
    3
    1
    4
    5

Here are some short notes. The details follow later. 

  * Multithreading needs under unix the unit **cthreads** as explained in [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial").
  * For speed reasons the **cmem** heap manager is recommended, although it does not make any difference in this example.
  * The parallel procedure **DoSomethingParallel** gets some fixed and predefined parameters.
  * **Index** defines what chunk of work should be done by this call.
  * **Data** is the pointer, that was given to _ProcThreadPool.DoParallel_ as fourth parameter. It is optional and you are free to use it for anything.
  * **Item** can be used to access some of the more sophisticated features of the thread pool.
  * **ProcThreadPool.DoParallel** works like a normal procedure call. It returns when it has run completely - that means all threads have finished their work.
  * The output shows the typical multi threading behavior: the order of the calls is not determined. Several runs can result in different orders.



# Features

Running a procedure in parallel means: 

  * a procedure or method is executed with an Index running from an arbitrary StartIndex to an arbitrary EndIndex.
  * One or more threads execute these index' in parallel. For example if the Index runs from 1 to 10 and there are 3 threads available in the pool, then 3 threads will run three different index at the same time. Every time a thread finishes one call (one index) it allocates the next index and runs it. The result may be: Thread 1 executes index 3,5,7, thread 2 executes 1,6,8,9 and thread 3 runs 2,4,10.
  * The number of threads may vary during run and there is no guarantee for a minimum of threads. In the worst case all index will be executed by one thread - the thread itself.
  * The maximum number of threads is initialized with a good guess for the current system. It can be manually set at any time via


    
    
    ProcThreadPool.MaxThreadCount := 8;
    

  * You can set the maximum threads for each procedure.
  * A parallel procedure (or method) can call recursively parallel procedures (or methods).
  * Threads are reused, that means they are not destroyed and created for each index, but there is a global pool of threads. On a dual core processor there will be two threads doing all the work - the main thread and one extra thread in the pool.



# Overhead, slow down

The overhead heavily depends on the system (number and types of cores, type of shared memory, speed of critical sections, cache size). Here are some general hints: 

  * Each chunk of work (index) should take at least some milliseconds.
  * The overhead is independent of recursive levels of parallel procedures.



Multi threading overhead, which is independent of the MTProcs units, but simply results from todays computer architectures: 

  * As soon as one thread is created your program becomes multi threaded and the memory managers must use critical sections, which slows down. So even if you do nothing with the thread your program might become slower.
  * The cmem heap manager is on some systems much faster for multi threading. In my benchmarks especially on intel systems and especially under OS X the speed difference can be more than 10 times.
  * Strings and interfaces are globally reference counted. Each access needs a critical section. Processing strings in multiple threads will therefore hardly give any speed up. Use PChars instead.
  * Each chunk of work (index) should work on a disjunctive part of memory to avoid cross cache updates.
  * Do not work on vast amounts of memory. On some systems one thread alone is fast enough to fill the memory bus speed. When the memory bus maximum speed is reached, any further thread will slow down instead of making it faster.



# Setting the maximum number of threads for a procedure

You can specify the maximum number of threads for a procedure as the fifth parameter. 
    
    
    begin
      ProcThreadPool.DoParallel(@DoSomethingParallel,1,10,nil,2); // address, startindex, endindex, 
          // optional: data (here: nil), optional: maximum number of threads (here: 2)
    end.
    

This can be useful, when the threads work on the same data, and too many threads will create so many cache conflicts, that they slow down each other. Or when the algorithm uses a lot of WaitForIndex, so that only a few threads can actually work. Then the threads can be used for other tasks. 

# Wait for index / executing in order

Sometimes an Index depends on the result of a former Index. For example a common task is to first compute chunk 5 and then combine the result with the result of chunk 3. Use the WaitForIndex method for that: 
    
    
    procedure DoSomethingParallel(Index: PtrInt; Data: Pointer; Item: TMultiThreadProcItem);
    begin
      ... compute chunk number 'Index' ...
      if Index=5 then begin
        if not Item.WaitForIndex(3) then exit;
        ... compute ...
      end;
    end;
    

WaitForIndex takes as an argument an Index that is lower than the current Index or a range. If it returns true, everything worked as expected. If it returns false, then an exception happened in one of the other threads. 

There is an extended function WaitForIndexRange that waits for whole range of Index: 
    
    
    if not Item.WaitForIndexRange(3,5) then exit; // wait for 3,4 and 5
    

# Exceptions

If an exception occur in one of the threads, the other threads will finish normally, but will not start a new Index. The pool waits for all threads to finish and will then raise the exception. That's why you can use _try..except_ like always: 
    
    
    try
      ...
      ProcThreadPool.DoParallel(...);
      ...
    except
      On E: Exception do ...
    end;
    

If there are multiple exceptions, only the first exception will be raised. To handle all exceptions, add a try..except inside your parallel method. 

# Synchronize

When you want to call a function in the main thread, for example to update some gui element, you can use the class method _TThread.Synchronize_. It takes as arguments the current TThread and the address of a method. Since 1.2 mtprocs provides a threadvar **CurrentThread** , which makes synchronizing simple: 
    
    
    TThread.Synchronize(CurrentThread,@YourMethod);
    

This will post an event on the main event queue and wait until the main thread has executed your method. Keep in mind that a bare fpc program does not have an event queue. A LCL or fpgui program has it. 

If you create your own TThread descendants, you should set the variable in your Execute method. For example: 
    
    
    procedure TYourThread.Execute;
    begin
      CurrentThread:=Self;
      ...work...
    end;
    

# Example: Parallel loop

This example explains step by step how to convert a loop into a parallel procedure. The example computes the maximum number of an integer array _BigArray_. 

## The original loop
    
    
    type
      TArrayOfInteger = array of integer;
    
    function FindMaximum(BigArray: TArrayOfInteger): integer;
    var
      i: PtrInt;
    begin
      Result:=BigArray[0];
      for i:=1 to length(BigArray)-1 do begin
        if Result<BigArray[i] then Result:=BigArray[i];
      end;
    end;
    

## Splitting the work

The work should be equally distributed over n threads. For this the BigArray is split into equally sized blocks and an outer loop runs over every block. Typically n is the number of cpus/cores in the system. MTProcs has some utility functions to compute the block size and count: 
    
    
    function FindMaximum(BigArray: TArrayOfInteger): integer;
    var
      BlockCount, BlockSize: PtrInt;
      i: PtrInt;
      Index: PtrInt;
      BlockStart, BlockEnd: PtrInt;
    begin
      Result:=BigArray[0];
      ProcThreadPool.CalcBlockSize(length(BigArray),BlockCount,BlockSize);
      for Index:=0 to BlockCount-1 do begin
        Item.CalcBlock(Index,BlockSize,length(BigArray),BlockStart,BlockEnd);
        for i:=BlockStart to BlockEnd do begin
          if Result<BigArray[i] then Result:=BigArray[i];
        end;
      end;
    end;
    

The added lines can be used for any loop. Eventually a tool can be written to automate this. 

The work is now split into smaller pieces. Now the pieces must become more independent. 

## Local and shared variables

For each used variable in the loop you must decide whether it is shared variable used by all threads or if each thread uses its own local variable. The shared variables BlockCount and BlockSize are only read and do not change, so no work is needed for them. But a shared variable like Result will be changed by all threads. This can either be achieved with synchronization (e.g. critical section), which is slow, or each thread use a local copy and these local variable are later combined. 

Here is a solution replacing the _Result_ variable with an array, which is combined in the end: 
    
    
    function FindMaximum(BigArray: TArrayOfInteger): integer;
    var
      // shared variables
      BlockCount, BlockSize: PtrInt;
      BlockMax: PPtrInt;
      // local variables
      i: PtrInt;
      Index: PtrInt;
      BlockStart, BlockEnd: PtrInt;
    begin
      ProcThreadPool.CalcBlockSize(length(BigArray),BlockCount,BlockSize);
      BlockMax:=AllocMem(BlockCount*SizeOf(PtrInt)); // allocate space for local variables
      // compute maximum for each block
      for Index:=0 to BlockCount-1 do begin
        // compute maximum of block
        Item.CalcBlock(Index,BlockSize,length(BigArray),BlockStart,BlockEnd);
        BlockMax[Index]:=BigArray[BlockStart];
        for i:=BlockStart to BlockEnd do begin
          if BlockMax[Index]<BigArray[i] then BlockMax[Index]:=BigArray[i];
        end;
      end;
      // compute maximum of all blocks
      // (if you have hundreds of threads there are better solutions)
      Result:=BlockMax[0];
      for Index:=1 to BlockCount-1 do
        Result:=Max(Result,BlockMax[Index]);
    
      FreeMem(BlockMax);
    end;
    

This approach is straightforward and could be automated. The process will need some hints from the programmer though. 

## DoParallel

The final step is to move the inner loop into a sub procedure and replace the loop with a call to DoParallelNested. 
    
    
    ...
    {$ModeSwitch nestedprocvars}
    uses mtprocs;
    ...
    function TMainForm.FindMaximum(BigArray: TArrayOfInteger): integer;
    var
      BlockCount, BlockSize: PtrInt;
      BlockMax: PPtrInt;
    
      procedure FindMaximumParallel(Index: PtrInt; Data: Pointer;
                                    Item: TMultiThreadProcItem);
      var
        i: integer;
        BlockStart, BlockEnd: PtrInt;
      begin
        // compute maximum of block
        Item.CalcBlock(Index,BlockSize,length(BigArray),BlockStart,BlockEnd);
        BlockMax[Index]:=BigArray[BlockStart];
        for i:=BlockStart to BlockEnd do
          if BlockMax[Index]<BigArray[i] then BlockMax[Index]:=BigArray[i];
      end;
    var
      Index: PtrInt;
    begin
      // split work into equally sized blocks
      ProcThreadPool.CalcBlockSize(length(BigArray),BlockCount,BlockSize);
      // allocate local/thread variables
      BlockMax:=AllocMem(BlockCount*SizeOf(PtrInt));
      // compute maximum for each block
      ProcThreadPool.DoParallelNested(@FindMaximumParallel,0,BlockCount-1);
      // compute maximum of all blocks
      Result:=BlockMax[0];
      for Index:=1 to BlockCount-1 do
        Result:=Max(Result,BlockMax[Index]);
      FreeMem(BlockMax);
    end;
    

This was mostly copy and paste, so again this could be automated. 

# Example: parallel sort

The unit mtputils contains the function **ParallelSortFPList** which uses mtprocs to sort a TFPList in parallel. A compare function must be given. 
    
    
    procedure ParallelSortFPList(List: TFPList; const Compare: TListSortCompare; MaxThreadCount: integer = 0; const OnSortPart: TSortPartEvent = nil);
    

This function uses the parallel MergeSort algorithm. The parameter MaxThreadCount is passed to DoParallel. A 0 means to use the system default. 

Optionally you can provide your own sort function (OnSortPart) to sort the part of each single thread. For example you can sort the blocks via QuickSort, which are then merged. Then you have a **parallel QuickSort**. See TFPList.Sort for an example implementation of QuickSort. 

# Using nested procedure
    
    
    procedure DoSomething(Value: PtrInt);
    var
      p: array[1..2] of Pointer;
    
      procedure SubProc(Index: PtrInt; Data: Pointer; Item: TMultiThreadProcItem);
      begin
        p[Index]:=Pointer(Value); // accessing local variables and parameters is possible!
      end;
    
    var
      i: Integer;
    begin
      ProcThreadPool.DoParallelNested(@SubProc,1,2);
    end;
    

This can save a lot of refactoring and makes the parallel procedure much more readable. 

# See also

  * [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial")
  * [OpenCL](<OpenCL.md> "OpenCL")

---

_Source: [https://wiki.freepascal.org/Parallel_procedures](https://web.archive.org/web/20240415163401/https://wiki.freepascal.org/Parallel_procedures)_
