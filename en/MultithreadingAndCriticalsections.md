# MultithreadingAndCriticalsections

│ **English (en)** │

## Code Snippet demonstrating Thread programming
    
    
    {$mode objfpc}{$H+}
    program Project1;
    
    uses
     {$ifdef unix}cthreads,
     {$endif}Classes,
      SysUtils;
    
    var
      CriticalSection: TRTLCriticalSection;
      Counter: integer;
      UseCriticalSection: boolean;
    
    
      function Supposedly: string;
      begin
        if UseCriticalSection then
          Result:=''
        else Result:='supposedly ';
      end;
    
    type
      { TMyThread }
    
      TMyThread = class(TThread)
      private
        FAFinished: boolean;
        FID: integer;
      public
        constructor Create(CreateSuspended: Boolean; ID: integer; const StackSize: SizeUInt=DefaultStackSize);
        procedure Execute; override;
        property AFinished: boolean read FAFinished write FAFinished;
      end;
    
      constructor TMyThread.Create(CreateSuspended: Boolean; ID: integer; const StackSize: SizeUInt);
      begin
        inherited Create(CreateSuspended, StackSize);
        FID:=ID;
      end;
    
      procedure TMyThread.Execute;
      var
        j: integer;
      begin
        FAFinished := False;
        if UseCriticalSection then
          EnterCriticalSection(CriticalSection);
        try
          for j := 1 to 1000000 do
            Inc(Counter);
          WriteLn(Format('Thread %d has %sfinished making 1000000 increments to Counter',[FID, Supposedly]));
        finally
          if UseCriticalSection then
            LeaveCriticalSection(CriticalSection);
        end;
        FAFinished := True;
      end;
    
      function HasCriticalSection: string;
      begin
        if UseCriticalSection then
          Result:=''
        else Result:='NOT ';
      end;
    
      procedure ExerciseThreads(aUseCriticalSection: boolean);
      var
        Threads: array[1..20] of TMyThread;
        AllFinished: boolean;
        i: integer;
      begin
        Counter := 0;
        UseCriticalSection:=aUseCriticalSection;
        WriteLn(Format('A critical section is %sbeing used; Counter starts at %d',[HasCriticalSection, Counter]));
        // start 20 threads
        for i := Low(Threads) to High(Threads) do
          Threads[i] := TMyThread.Create(False, i);
        // wait till all threads finished
        repeat
          AllFinished := True;
          for i := Low(Threads) to High(Threads) do
            if not Threads[i].AFinished then
              AllFinished := False;
        until AllFinished;
        // free the threads
        for i := Low(Threads) to High(Threads) do
          Threads[i].Free;
        WriteLn('Final value of Counter=' + IntToStr(Counter));
        WriteLn;
      end;
    
    var
      b: boolean;
    begin
        // create the CriticalSection
      InitCriticalSection(CriticalSection);
    
      for b in Boolean do
        ExerciseThreads(b);
    
        // free the CriticalSection
      DoneCriticalSection(CriticalSection);
    
      {$IfDef WINDOWS}
        Write(' Press [Enter] to finish');
        ReadLn;
      {$EndIf}
    end.

---

_Source: [https://wiki.freepascal.org/MultithreadingAndCriticalsections](https://web.archive.org/web/20241212110217/https://wiki.freepascal.org/MultithreadingAndCriticalsections)_
