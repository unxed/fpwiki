# Asynchronous Calls

│ **English (en)** │  **[français (fr)](</Asynchronous_Calls/fr> "Asynchronous Calls/fr")** │  **[日本語 (ja)](</Asynchronous_Calls/ja> "Asynchronous Calls/ja")** │  **[русский (ru)](<../ru/Asynchronous_Calls.md> "Asynchronous Calls/ru")** │    
****

## Contents

  * 1 Problem statement
  * 2 Solution
  * 3 Examples
    * 3.1 Simple data passed to async function
    * 3.2 Record passed to async function
  * 4 See also



## Problem statement

When handling some event, you need to do something, but you can't do it right away. For example, you need to free an object, but it is or will be referenced somewhere in the parent (or its parent etc.) later on. Or you need to update GUI elements accessed by several threads in a thread safe manner. 

## Solution

Call Application.[QueueAsyncCall](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tapplication.queueasynccall.html> "doc:lcl/forms/tapplication.queueasynccall.html"): 
    
    
    [TDataEvent](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tdataevent.html> "doc:lcl/forms/tdataevent.html") = procedure (Data: PtrInt) of object;
    
    procedure [QueueAsyncCall](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tapplication.queueasynccall.html> "doc:lcl/forms/tapplication.queueasynccall.html")(AMethod: TDataEvent; Data: PtrInt);
    
    

This will "queue" the given method with the given parameter for execution in the main event loop, when all other events have been processed. In the example above, the reference to the object you wanted to free has gone, since the then-parent has finished execution, and the object you wanted to free can be freed safely. 

Note that this is a more generic version of [ReleaseComponent](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tapplication.releasecomponent.html> "doc:lcl/forms/tapplication.releasecomponent.html"), and ReleaseComponent calls this method. 

## Examples

### Simple data passed to async function

The following program shows the use of [QueueAsyncCall](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tapplication.queueasynccall.html> "doc:lcl/forms/tapplication.queueasynccall.html"). If you press on the CallButton, 'Click 1', 'Click 2' and 'Async 1' is added to the LogListBox. Note that Async call is only executed after the CallButtonClick event has finished. 
    
    
    unit TestQueueAsyncCall;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs, Buttons,
      StdCtrls;
    
    type
    
      { TQueueAsyncCallForm }
    
      TQueueAsyncCallForm = class(TForm)
        CallButton: TButton;
        LogListBox: TListBox;
        procedure CallButtonClick(Sender: TObject);
      private
        { private declarations }
        FCounter: PtrInt;
        procedure Async(Data: PtrInt);
      public
        { public declarations }
      end; 
    
    var
      QueueAsyncCallForm: TQueueAsyncCallForm;
    
    implementation
    
    {$R *.lfm}
    
    { TQueueAsyncCallForm }
    
    procedure TQueueAsyncCallForm.CallButtonClick(Sender: TObject);
    begin
      LogListBox.Items.Add('Click 1');
      FCounter := FCounter+1;
      Application.QueueAsyncCall(@Async,FCounter);
      LogListBox.Items.Add('Click 2');
    end;
    
    procedure TQueueAsyncCallForm.Async(Data: PtrInt);
    begin
       LogListBox.Items.Add('Async '+ IntToStr(Data));
    end;
    
    end.
    

### Record passed to async function

Here is some code extracted from MultiLog's MemoChannel unit used for thread safe logging messages to Memo control. Thanks to QueueAsyncCall() all writings to Memo are serialized. There are no locks and no conflicts, even with hundreds of threads calling Write() method. 
    
    
    ...
    type
      TLogMsgData = record // record can hold much more then a simple string :-)
        Text: string;
      end;
      PLogMsgData = ^TLogMsgData;
    ...
    procedure TMemoChannel.Write(const AMsg: string);
    var
      LogMsgToSend: PLogMsgData;
    begin
      New(LogMsgToSend);
      LogMsgToSend^.Text:= AMsg;
      Application.QueueAsyncCall(@WriteAsyncQueue, PtrInt(LogMsgToSend)); // put log msg into queue that will be processed from the main thread after all other messages
    end;
    ...
    procedure TMemoChannel.WriteAsyncQueue(Data: PtrInt);
    var // called from main thread after all other messages have been processed to allow thread safe TMemo access
      ReceivedLogMsg: TLogMsgData;
    begin
      ReceivedLogMsg := PLogMsgData(Data)^;
      try
        if (FMemo <> nil) and (not Application.Terminated) then
        begin
          ...
          FMemo.Append(ReceivedLogMsg.Text) // <<< fully thread safe
        end;
      finally
        Dispose(PLogMsgData(Data));
      end;
    end;
    

## See also

  * [TThread.Queue, which provides the same functionality in a Delphi compatible way and with anonymous method support in FPC 3.3.x and later](<https://www.freepascal.org/docs-html/rtl/classes/tthread.queue.html>)
  * [New 0.9.26 features. Part 1. SendMessage and PostMessage](<http://lazarus-dev.blogspot.com/2008/01/new-0926-features-part-1-sendmessage.html>)
  * [Main Loop Hooks](<Main_Loop_Hooks.md> "Main Loop Hooks")
  * [Multithreaded Application Tutorial](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial")

---

_Source: [https://wiki.freepascal.org/Asynchronous_Calls](https://web.archive.org/web/20250114033256/https://wiki.freepascal.org/Asynchronous_Calls)_
