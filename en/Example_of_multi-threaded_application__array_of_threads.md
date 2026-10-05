# Example of multi-threaded application: array of threads

│ **English (en)** │  **[español (es)](</Example_of_multi-threaded_application:_array_of_threads/es> "Example of multi-threaded application: array of threads/es")** │  **[日本語 (ja)](</Example_of_multi-threaded_application:_array_of_threads/ja> "Example of multi-threaded application: array of threads/ja")** │  **[polski (pl)](</Example_of_multi-threaded_application:_array_of_threads/pl> "Example of multi-threaded application: array of threads/pl")** │    
****

Here I want to show an example how to create a lot of threads and wait while they will not finish their jobs (I don't need any synchronisation). I'm writing this tutorial because it was not obvious for me to write such a program after reading [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial"). I was writing my application for macOS, but the resulting code should work on any system. 

Let's assume that we have the following loop: 
    
    
    ...
    for i:=1 to n do begin
      power(i,0.5)
    end;
    ...
    

This loop computes the "power" function serially n times one after another. Lets use threads to achieve the same task in parallel. 

## Contents

  * 1 Are multiple threads required?
  * 2 Managing memory
  * 3 1\. Detect number of cores available.
  * 4 2\. Create a custom threads class.
  * 5 3\. Write the main program.
  * 6 Results.



## Are multiple threads required?

For modern multiple CPU core computers, using multiple threads can dramatically increase performance. However, modern computers are very fast and threaded code is often harder to debug and maintain than single-threaded code. One should consider whether the processing time saved justifies the complexity of multi-threaded programming and whether the algorithm is well suited for parallel computing. Finally, note that some algorithms that can benefit from parallel computing may benefit from using CPU threads (as shown here), while others might be more suited for the GPU (where tools like OpenCL may be optimal). 

## Managing memory

Since multiple threads will be working simultaneously, you need to ensure that they do not have memory contention issues. We will have memory contention issues if multiple threads are writing to the same locations in memory. Some algorithms do not lend themselves to multi-threading, because each computation depends on earlier results. On the other hand, multi-threading works very efficiently on problems where the computations can be performed independently and in parallel. In this example we will solve a problem that is completely independent and therefore easy to attack with multiple threads. Advanced algorithms will have to use memory locking features to avoid contention. 

In our example, we will have each thread write to distinct memory locations. Specifically, we will create an array 1..n and compute the value power(i,0.5) where i is in the range 1..n. Each thread will be given an independent portion of the range to compute. Consider n=1000. If we use one thread, it will be tasked with the whole range 1..1000, whereas if we use two threads one will tackle 1..500 and the other 501..1000. This way threads will be working to fill different portions of our memory array. 

## 1\. Detect number of cores available.

A computer with only a single core will not benefit from threading, whereas a computer with four physical cores each with hyperthreading (able to run two tasks simultaneously) will be able to process up to eight tasks at once. The following unit "cpucount" reports the number of cores available. You can use this to determine how many threads your program should run on a given computer. For computers with four or more cores, you many want to run n-1 threads (where n is the core count), reserving one core for the graphical interface and other tasks, as the performance difference between n and n-1 threads will not be great with this many cores. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The function definition used below for FPsysctl() is for FPC v3.0.4 - For FPC v3.2.0 and v3.3.1 (trunk) the first argument is no longer _pchar_ but _pcint_. Adjust the code example accordingly.
    
    
    unit cpucount;
    
    {$mode objfpc}{$H+}
    
    interface
    //returns number of cores: a computer with two hyperthreaded cores will report 4
    function GetLogicalCpuCount: Integer;
    
    implementation
    
    {$IF defined(windows)}
    uses windows;
    {$endif}
    
    {$IF defined(darwin)}
    uses ctypes, sysctl;
    {$endif} 
    
    {$IFDEF Linux}
    uses ctypes;
    
    const _SC_NPROCESSORS_ONLN = 83;
    function sysconf(i: cint): clong; cdecl; external name 'sysconf';
    {$ENDIF}
    
    
    function GetLogicalCpuCount: integer;
    // returns a good default for the number of threads on this system
    {$IF defined(windows)}
    //returns total number of processors available to system including logical hyperthreaded processors
    var
      i: Integer;
      ProcessAffinityMask, SystemAffinityMask: DWORD_PTR;
      Mask: DWORD;
      SystemInfo: SYSTEM_INFO;
    begin
      if GetProcessAffinityMask(GetCurrentProcess, ProcessAffinityMask, SystemAffinityMask)
      then begin
        Result := 0;
        for i := 0 to 31 do begin
          Mask := DWord(1) shl i;
          if (ProcessAffinityMask and Mask)<>0 then
            inc(Result);
        end;
      end else begin
        //can't get the affinity mask so we just report the total number of processors
        GetSystemInfo(SystemInfo);
        Result := SystemInfo.dwNumberOfProcessors;
      end;
    end;
    {$ELSEIF defined(UNTESTEDsolaris)}
      begin
        t = sysconf(_SC_NPROC_ONLN);
      end;
    {$ELSEIF defined(freebsd) or defined(darwin)}
    var
      mib: array[0..1] of cint;
      len: cint;
      status: integer;
    begin
      mib[0] := CTL_HW;
      mib[1] := HW_NCPU;
      len := sizeof(Result);
      status := fpsysctl(pchar(@mib), Length(mib), @Result, @len, Nil, 0);
      if status <> 0 then WriteLn('Error in fpsysctl()');
    end;
    {$ELSEIF defined(linux)}
      begin
        Result:=sysconf(_SC_NPROCESSORS_ONLN);
      end;
    
    {$ELSE}
      begin
        Result:=1;
      end;
    {$ENDIF}
    end.
    

## 2\. Create a custom threads class.

I use a separate unit for defining the behavior of the threads. Note that I am setting the "FreeOnTerminate" to false - so my program will need to dispose of each thread when it is done. This makes it easier to juggle multiple threads (if you set FreeOnTerminate to true and launch multiple very fast jobs it is possible that the thread will be released before your program checks whether the thread is completed - and checking a non-existent thread can cause an exception). By setting FreeOnTerminate to false I can ensure that each thread completed successfully. 

A well behaved thread in a loop should regularly check for termination so that if outside processes (Eg. Destroy) wish to stop execution they do not have to wait forever. The original code did not do this and so the free was not able to be completed until all calculations were completed. 

A check of the code base shows that on exiting "Execute" OnTerminate is called but Terminated is not set. As the example no longer uses waitfor but simply checks Terminated I added Terminate after completion of the loop. 

  

    
    
    unit mythreads;
    {$mode objfpc}{$H+}
    interface
    uses
      Classes, SysUtils, Math;
    type
      TData = array of double;
      PData = ^TData;
     Type
        TMyThread = class(TThread)
        private
        protected
          tPtr: PData;
          tstart,tfinish: integer;
          procedure Execute; override;
        public
          property Terminated;
          Constructor Create(lstart, lfinish: integer; var lPtr: PData);
        end;
    
    implementation
    
      constructor TMyThread.Create(lstart, lfinish: integer; var lPtr: PData);
      begin
        FreeOnTerminate := False;
        tstart := lstart;
        tfinish := lfinish;
        tPtr := lPtr;
        inherited Create(false);
      end;
      procedure TMyThread.Execute;
      var
        i: integer;
      begin
        i:= tstart;
        While not Terminated and (i<= tfinish) do 
        //A well behaved thread in a loop should regularly check for termination 
            begin
        //for i := tstart to tfinish do
            tPtr^[i] := power(i,0.5);
            inc(i);
            end;
        Terminate;
      end;
    
    end.
    

## 3\. Write the main program.

You need to add 'cthreads' to the main unit, not to unit with threads! 

Note that there are two ways to determine whether all the threads have completed. You can use the in-built "waitFor" function - this works very nicely but on my Mac computer I noted that it refreshes only every 100ms. This is perfect for real world programs (we only use threading for computationally slow problems) and reduces thread overhead. However, for quick example benchmarks it can hide the benefits of threading (as operations require a minimum of 100ms regardless of the number of threads). Therefore, in this example I detect the threads terminated status every 2ms. This provides more accurate benchmark timing. 

Remember to free each thread when you are done with it. Since we set "FreeOnTerminate := False" the program needs to do this explicitly. 

Tips: in my Lazarus IDE I was not able to debug multi-threading applications if I don't use 'pthreads'. I have read that if you use 'cmem', the program works faster, but I strongly recommend you to check it for any particular case (my program hangs when I use 'cmem'). 
    
    
    uses //    cmem,pthreads,
      cthreads, Classes, SysUtils, CustApp, MyThreads, Math, cpucount;
    
    procedure DoUnThreaded (nValues: integer);
    var
     dataArray: TData;
     i: integer;
     StartMS: double;
    begin
         if (nValues < 1) then exit;
         StartMS:=timestamptomsecs(datetimetotimestamp(now));
         setlength(dataArray, nValues+1);//+1 since indexed 0..n-1
         for i:=1 to nValues do
             dataArray[i] := power(i,0.5);  ;
         Writeln('Serially processed '+inttostr(nValues)+' values in '+floattostr(timestamptomsecs(datetimetotimestamp(now))-StartMS)+'ms, with '+inttostr(nValues)+'^0.5 = '+floattostr(dataArray[nValues]));
    end;
    
    procedure DoThreading (nThreadsIn, nValues: integer);
    var
     threadArray: array  of TMyThread;
     dataArray: TData;
     lData : PData;
     nThreads, i,lStart,lFinish: integer;
     StartMS: double;
    begin
         if (nThreadsIn < 1) or (nValues < 1) then exit;
         nThreads := nThreadsIn;
         if  nThreads > nValues then nThreads := nValues;
         StartMS:=timestamptomsecs(datetimetotimestamp(now));
         setlength(threadArray,nThreads+1);//+1 since indexed 0..n-1
         setlength(dataArray, nValues+1);//+1 since indexed 0..n-1
         lData := @dataArray;
         lStart := 1;
         for i:=1 to nThreads do begin
             if i < nThreads then
                lFinish:=i*(nValues div nThreads)
             else
                 lFinish:=  nValues;
             threadArray[i]:= TMyThread.Create(lStart, lFinish, lData);
             //Writeln('Thread '+inttostr(i)+' processing '+inttostr(lStart)+'..'+inttostr(lFinish));
             lStart := lFinish+1;
         end;
         //for i:=1 to nThreads do if not ThreadArray[i].Terminated then Sleep(2); Logically Wrong just introduces nx2ms delay
         //for i:=1 to nThreads do threadArray[i].waitFor;  //appears to sleep for 100ms on macOS
    
         for i:=1 to nThreads do 
            While not ThreadArray[i].Terminated do Sleep(2);   //do not progress until all threads have terminated 
     
         for i:=1 to nThreads do threadArray[i].Free; //Free not complete until the Thread Execute process exits.
         Writeln(inttostr(nThreads)+' Threads processed '+inttostr(nValues)+' values in '+floattostr(timestamptomsecs(datetimetotimestamp(now))-StartMS)+'ms, with '+inttostr(nValues)+'^0.5 = '+floattostr(dataArray[nValues]));
    
    end;
    
    begin
      Writeln('Computer reports '+inttostr(GetLogicalCpuCount)+' cores: probably optimal number of threads ');
      DoUnthreaded(10);
      DoThreading(1,10);
      DoThreading(2,10);
      DoThreading(4,10);
      DoThreading(8,10);
      DoUnthreaded(100000000);
      DoThreading(1,100000000);
      DoThreading(2,100000000);
      DoThreading(4,100000000);
      DoThreading(8,100000000);
    end.
    

## Results.

The results show there is a delay in creating threads, but that for large tasks multiple parallel threads outperform serial processing. Note that when the number of threads exceeds the number of cores (4 for this computer) there is little benefit for additional threads. You should not expect speed to scale perfectly with the number of threads: there is some overhead to threading and most moderns CPUs will operate slightly faster when there is only one intensive task than when running running tasks that tax multiple cores ('turboboost'). 

  * Computer reports 4 cores: probably optimal number of threads 
  * Serially processed 10 values in 0ms, with 10^0.5 = 3.16227766016838 
  * 1 Threads processed 10 values in 3ms, with 10^0.5 = 3.16227766016838 
  * 2 Threads processed 10 values in 5ms, with 10^0.5 = 3.16227766016838 
  * 4 Threads processed 10 values in 9ms, with 10^0.5 = 3.16227766016838 
  * 8 Threads processed 10 values in 19ms, with 10^0.5 = 3.16227766016838 
  * Serially processed 100000000 values in 10214ms, with 100000000^0.5 = 10000 
  * 1 Threads processed 100000000 values in 10320ms, with 100000000^0.5 = 10000 
  * 2 Threads processed 100000000 values in 5894ms, with 100000000^0.5 = 10000 
  * 4 Threads processed 100000000 values in 3801ms, with 100000000^0.5 = 10000 
  * 8 Threads processed 100000000 values in 3733ms, with 100000000^0.5 = 10000

---

_Source: [https://wiki.freepascal.org/Example_of_multi-threaded_application%3A_array_of_threads](https://web.archive.org/web/20241207111712/https://wiki.freepascal.org/Example_of_multi-threaded_application%3A_array_of_threads)_
