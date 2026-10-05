# Asynchronous Calls

│ **[English (en)](<../en/Asynchronous_Calls.md> "Asynchronous Calls")** │  **[français (fr)](</Asynchronous_Calls/fr> "Asynchronous Calls/fr")** │  **[日本語 (ja)](</Asynchronous_Calls/ja> "Asynchronous Calls/ja")** │  **русский (ru)** │    
****

## Contents

  * 1 Постановка задачи
  * 2 Решение
  * 3 Примеры
    * 3.1 Передача в асинхронную функцию простых данных
    * 3.2 Передача в асинхронную функцию записи
  * 4 См.также



## Постановка задачи

При обработке какого-либо события вам иногда нужно что-то сделать, но вы не можете сделать это сразу. Например, вам нужно освободить объект, но на него ссылаются или будут ссылаться позже где-то в родителе (или в его родителе и т.д.). Или вам нужно обновить элементы графического интерфейса, к которым обращаются несколько потоков, безопасным способом. 

## Решение

Вызовите Application.[QueueAsyncCall](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tapplication.queueasynccall.html> "doc:lcl/forms/tapplication.queueasynccall.html"): 
    
    
    [TDataEvent](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tdataevent.html> "doc:lcl/forms/tdataevent.html") = procedure (Data: PtrInt) of object;
    
    
    
    procedure [QueueAsyncCall](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tapplication.queueasynccall.html> "doc:lcl/forms/tapplication.queueasynccall.html")(AMethod: TDataEvent; Data: PtrInt);
    
    

Это «поставит» данный метод с заданным параметром для выполнения в очередь в главном цикле событий, когда все остальные события будут обработаны. В приведенном выше примере ссылка на объект, который вы хотели освободить, исчезла, поскольку тогдашний родительский объект завершил выполнение, и объект, который вы хотели освободить, можно безопасно освободить. 

Обратите внимание, что это более обобщенная версия [ReleaseComponent](<http://lazarus-ccr.sourceforge.net/docs/lcl_/_forms_/_tapplication.releasecomponent.html> "doc:lcl / forms / tapplication.releasecomponent.html"), и ReleaseComponent вызывает этот метод. 

## Примеры

### Передача в асинхронную функцию простых данных

Следующая программа демонстрирует использование [QueueAsyncCall](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tapplication.queueasynccall.html> "doc:lcl/forms/tapplication.queueasynccall.html"). Если вы нажмете на кнопку CallButton, в LogListBox добавится 'Click 1', 'Click 2' и 'Async 1'. Обратите внимание, что асинхронный вызов выполняется только после завершения события CallButtonClick. 
    
    
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
    

### Передача в асинхронную функцию записи

Вот некоторый код, извлеченный из модуля MemoChannel MultiLog, который используется для потоковой записи сообщений в журнал для управления Memo. Благодаря QueueAsyncCall() все записи в Memo сериализуются. Здесь нет блокировок и конфликтов, даже если сотни потоков вызывают метод Write(). 
    
    
    ...
    type
      TLogMsgData = record // запись может содержать гораздо больше, чем простая строка :-)
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
      Application.QueueAsyncCall(@WriteAsyncQueue, PtrInt(LogMsgToSend)); // помещаем содержимое msg в очередь, которая будет обработана в основном потоке сразу после обработки всех других сообщений
    end;
    ...
    procedure TMemoChannel.WriteAsyncQueue(Data: PtrInt);
    var // вызывается в основном потоке после обработки всех других сообщений, чтобы обеспечить потокобезопасный доступ к TMemo
      ReceivedLogMsg: TLogMsgData;
    begin
      ReceivedLogMsg := PLogMsgData(Data)^;
      try
        if (FMemo <> nil) and (not Application.Terminated) then
        begin
          ...
          FMemo.Append(ReceivedLogMsg.Text) // <<< полностью потокобезопасен
        end;
      finally
        Dispose(PLogMsgData(Data));
      end;
    end;
    

## См.также

  * [Новые фичи 0.9.26. Часть 1. SendMessage и PostMessage](<http://lazarus-dev.blogspot.com/2008/01/new-0926-features-part-1-sendmessage.html>)
  * [Main Loop Hooks](<../en/Main_Loop_Hooks.md> "Main Loop Hooks")
  * [Multithreaded Application Tutorial](<../en/Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial")

---

_Source: [https://wiki.freepascal.org/Asynchronous_Calls/ru](https://web.archive.org/web/20250117033148/https://wiki.freepascal.org/Asynchronous_Calls/ru)_
