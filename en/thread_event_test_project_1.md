# thread event test project 1

## Contents

  * 1 TEventThread example: matrix multiplication
    * 1.1 GUI unit
    * 1.2 worker unit
    * 1.3 TEventThread dummy unit



### TEventThread example: matrix multiplication

A thread is created for each line of the Matrix For now only a dummy is used as an implementation of TEventThread. So the result is correct, but no Threads are used. 

#### GUI unit
    
    
    unit eventthreadtest1;
       
    interface
       
    uses
      Windows, Messages, SysUtils, Variants, Classes, Graphics, Controls, Forms,
      Dialogs, StdCtrls,
      eventthreadtest2,
      eventthread_1;
       
    type
      TForm33 = class(TForm)
        Button1: TButton;
        Memo1: TMemo;
        procedure Button1Click(Sender: TObject);
       private
       public
        { Public declarations }
      end;
    
      TMatrixMultMainEventThread = class (TMainEventThread)
      private
        { Private declarations }
        mm: TMatrixMult;
        mr: TMatrix;
        ThreadsRunning : Integer;
        procedure doit;
        procedure ThreadReady(Sender: TObject);
        procedure AllThreadsReady;
      end;
      
    var
      Form33: TForm33;
     
    implementation
    
    {$R *.dfm}
    
    var
      MatrixMultMainEventThread: TMatrixMultMainEventThread;
     
    
    procedure SetupMatrices(var mr, m1, m2: TMatrix);
    var
      i, j: Integer;
    begin
      SetLength(m1, 3);
      SetLength(m2, 4);
      SetLength(mr, 3);
      for i := 0 to length(m1)-1 do begin
        SetLength(m1[i], length(m2));
        SetLength(mr[i], length(m1));
      end;
      for i := 0 to length(m2)-1 do begin
        setlength(m2[i], length(m1));
      end;
      for i := 0 to length(m1)-1 do begin
        for j := 0 to length(m2)-1 do begin
          m1 [i, j] := i+j;
          m2 [j, i] := 1 + 2*i + 3*j;
        end;
      end;
    end;
       
    procedure TMatrixMultMainEventThread.doit;
    type
      trtest = record ptr: pointer; self: TObject; end;
    var
      m1, m2: TMatrix;
      l: Integer;
     
      etest: TMultLineEvent;
      rtest: trtest;
      stest: string;
    
    begin
      Form33.Memo1.Clear;
      SetupMatrices(mr, m1, m2);
      mm := TMatrixMult.Create(length(m1), ThreadReady);
      ThreadsRunning := length(m1);
      for l := 0 to length(m1)-1 do begin
       
        ///////////////////////////////////////////
        // just for demonstartion //////////////////
        //  if (mm.MultLineEvent[l] is TEventThread) then begin
            etest := mm.MultLineEvent[l];
            move(etest, rtest, 8);
            if rtest.self is TEventThread then begin
              stest := 'is TEventThread';
             end else begin
              stest := 'wrong type';
            end;
            Form33.Memo1.Lines.Add(stest);
        ///////////////////////////////////////////
        ///////////////////////////////////////////
       
        mm.MultLineEvent[l](mr, m1, m2, l);
        // doing an indirect call to a "procedure..of object"
        // which _is_ a TThreadEvent schedulres a thread event
        // instead of just calling the precudure
      end;
      // We now need to wait for all threads to be ready
    end;
       
    procedure TMatrixMultMainEventThread.ThreadReady(Sender: TObject);
    begin       // this is a thread event calling the main thread. Tus the code
                // is always running as the Main thread. So no danger that
                // e.g. ThreadsRunning := ThreadsRunning - 1 suffers from
                // multitasking ambiguity
      ThreadsRunning := ThreadsRunning - 1;
      if ThreadsRunning <> 0 then exit;
      mm.Free;
      AllThreadsReady;
    end;
       
    procedure TMatrixMultMainEventThread.AllThreadsReady;
    var
      i, j: Integer;
      s: String;
    begin
      // calculation ready
      for i := 0 to length(mr)-1 do begin
        s := '';
        for j := 0 to length(mr[0])-1 do begin
          s := s + FloatToStr(mr[i,j]) + ' ';
        end;
        Form33.Memo1.Lines.Add(s);
      end;
    end;
       
    procedure TForm33.Button1Click(Sender: TObject);
    begin
      MatrixMultMainEventThread := TMatrixMultMainEventThread.Create;
      MatrixMultMainEventThread.doit;
      MatrixMultMainEventThread.Free;
    end;
       
    end.
    

#### worker unit
    
    
    unit eventthreadtest2;
       
    interface
       
    uses Classes,
      eventthread_1;   // provides a dummy impementation of TEventTrread class
       
    type
      TMatrixElement = extended;
      TVector = array of TMatrixElement;
      TMatrix = array of TVector;
       
      TMultLineEvent = procedure (const mr, m1, m2: TMatrix; l: Integer) of object;
     
      TMatrixMultThread = class(TEventThread)
      private
        FNotifyEvent: TNotifyEvent;
        FAktLine: Integer;
        procedure NotifyReady;
      public
        procedure MultLine(const mr, m1, m2: TMatrix; l: Integer);
        property NotifyEvent: TNotifyEvent read FNotifyEvent write FNotifyEvent;
        property AktLine: Integer read FAktLine;
      end;
       
      TMatrixMult = Class(TObject)
      private
        FMatrixMultThread : array of TMatrixMultThread;
        FMultLineEvent : array of TMultLineEvent;
        function GetMultLineEvent(i: Integer): TMultLineEvent;
        procedure SetMultLineEvent(i: Integer; const Value: TMultLineEvent);
        procedure SetEventCount(Value: Integer; ThreadReady: TNotifyEvent);
      public
        constructor Create(Count: Integer; ThreadReady: TNotifyEvent);
        destructor Destroy; override;
        property MultLineEvent[i: Integer]: TMultLineEvent
          read GetMultLineEvent write SetMultLineEvent;
      end;
      
    implementation
       
    { TMatrixMultThread }
       
    procedure TMatrixMultThread.MultLine(const mr, m1, m2: TMatrix; l: Integer);
    var
      i, j: Integer;
      sum: TMatrixElement;
    begin
      FAktLine := l;
      for i := 0 to length(m1)-1 do begin
        sum := 0;
        for j := 0 to length(m2)-1 do begin
          sum := sum + m1[l, j] * m2[j, i];
        end;
        mr[l, i] := sum;
      end;
      NotifyReady;
    end;
       
    procedure TMatrixMultThread.NotifyReady;
    begin
      if assigned(FNotifyEvent) then FNotifyEvent(self);
             // doing an indirect call to a "procedure..of object"
             // which _is_ a TThreadEvent schedulres a thread event
             // instead of just calling the procedure
    end;
       
    { TMatrixMult }
      
    constructor TMatrixMult.Create(Count: Integer; ThreadReady: TNotifyEvent);
    begin
      inherited Create;
      SetEventCount(Count, ThreadReady);
    end;
       
    destructor TMatrixMult.Destroy;
    var
      l: Integer;
    begin
      for l := 0 to length(FMatrixMultThread)-1 do begin
        FMatrixMultThread[l].Free;
      end;
      inherited;
    end;
       
    procedure TMatrixMult.SetEventCount(Value: Integer; ThreadReady: TNotifyEvent);
    var
      l: Integer;
    begin
      SetLength(FMatrixMultThread, Value);
      SetLength(FMultLineEvent, Value);
      for l := 0 to Value-1 do begin
        FMatrixMultThread[l] := TMatrixMultThread.Create;
        FMatrixMultThread[l].NotifyEvent := ThreadReady;
        FMultLineEvent[l] := FMatrixMultThread[l].MultLine;
      end;
    end;
       
    function TMatrixMult.GetMultLineEvent(i: Integer): TMultLineEvent;
    begin
      Result := FMultLineEvent[i];
    end;
       
    procedure TMatrixMult.SetMultLineEvent(i: Integer; const Value: TMultLineEvent);
    begin
      FMultLineEvent[i] := Value;
    end;
     
    end.
    

#### TEventThread dummy unit
    
    
    unit eventthread_1;
      
    interface
     
    type
      TEventThread = class(TObject)
      end;
       
      TMainEventThread = class(TObject)
      end;
        
    implementation
    
    end.

---

_Source: [https://wiki.freepascal.org/thread_event_test_project_1](https://web.archive.org/web/20230924192644/https://wiki.freepascal.org/thread_event_test_project_1)_
