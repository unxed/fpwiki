# Execute action after form is shown

Lazarus doesn't support something like form OnShown/OnShowAfter event where custom code can be executed after form was shown. This is useful mainly to do run custom code after main form is shown. It would serve as kind of one time OnApplicationStart event. In would be useful for example for loading of previous opened project/game file just after application start which can take longer time and main form should be already visible at that moment. 

There are multiple ways how to solve this problem. 

## Contents

  * 1 Methods
    * 1.1 OnActivate event
    * 1.2 Send custom message
    * 1.3 QueueAsyncCall
    * 1.4 TTimer
  * 2 See also



## Methods

### OnActivate event

Form OnActivate event is fired every time if form gets focus. Also this event is fired after OnShow event. Then it is possible to use this event to executed code only once after OnShow event. Boolean variable is needed so after form shown code will be executed only once. 
    
    
    type
      TFormMain = class
        ...
      private
        FormActivated: Boolean;
        ...
      end;
    
    ...
    
    procedure TFormMain.FormActivated(Sender: TObject);
    begin
      if not FormActivated then begin
        FormActivated := True;
        // After show code
      end;
    end;
    

### Send custom message

It is possible to define custom message and send it to application message queue so it will be handled as soon as possible. 
    
    
    uses
      ..., LMessages, LCLIntf;
    
    const
      LM_STARTUP = LM_USER; 
    
    procedure TFormMain.FormShow(Sender: TObject);
    begin
      // On on show code
      SendMessage(Handle, LM_STARTUP, 0, 0);
    end;   
    
    procedure TFormMain.LMStartup(var Msg: TLMessage);
    begin
      // After show code
    end;
    

### QueueAsyncCall

There is a way how to execute asynchronous operations in Lazarus using [Asynchronous Calls](<Asynchronous_Calls.md> "Asynchronous Calls"). This is useful mainly for parallel processing but it can be used also for execution of form after show handler. 
    
    
    {$mode delphi} 
    
    procedure TFormMain.FormShow(Sender: TObject);
    begin
      // On on show code
      Application.QueueAsyncCall(AfterShow, 0);
    end;
    
    procedure TFormMain.AfterShow(Ptr: IntPtr);
    begin
      // After show code
    end;
    

### TTimer

You can use TTimer instance for delayed execution of startup code. Create instance of TTimer component and set its property Enabled to False and Interval to 1. Then from the end of FormShow handler enable timer manually. It will be executed as soon as possible. You need to disable timer at the start of timer handler. 
    
    
    procedure TFormMain.FormShow(Sender: TObject);QueueAsyncCall 
    begin
      // On on show code
      Timer1.Enabled := True;
    end;
    
    procedure TForm1.Timer1Timer(Sender: TObject);
    begin
      Timer1.Enabled := False;
      // After show code
    end;
    

## See also

  * [Event order](<Event_order.md> "Event order")

---

_Source: [https://wiki.freepascal.org/Execute_action_after_form_is_shown](https://web.archive.org/web/20230924212258/https://wiki.freepascal.org/Execute_action_after_form_is_shown)_
