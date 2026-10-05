# callback

**Callback** functions are in widespread usage in Lazarus; in fact any [Event](</index.php?title=Event&action=edit&redlink=1> "Event \(page does not exist\)") is made available using 'callback'. 
    
    
     type
      TForm = class(TForm)
        btn: TButton;
        procedure btnClick(Sender: TObject);
      end;
     ...
      procedure TForm.btnClick(Sender: TObject);
      begin
       // dosomething
      end;
    

The _btnClick_ method is a callback from within the TButton implementation. 

Apart from their use in the userinterface of a form, callbacks are used in many [libraries](<Library.md> "Library") that were linked. 

## Callback from library
    
    
    DWORD LpprogressRoutine(
      LARGE_INTEGER TotalFileSize,
      LARGE_INTEGER TotalBytesTransferred,
      LARGE_INTEGER StreamSize,
      LARGE_INTEGER StreamBytesTransferred,
      DWORD dwStreamNumber,
      DWORD dwCallbackReason,
      HANDLE hSourceFile,
      HANDLE hDestinationFile,
      LPVOID lpData
    )
    
    
    
    TMyForm = class( TForm )
      progressbar: TProgressbar;
      bIsAborted: boolean;
      ...
    end;
    
      // the callback function as needed by CopyFileEx
    function _CopyCallback( 
      TotalFileSize, TotalBytesTransferred, StreamSize, StreamBytesTransferred: Int64; 
      dwStreamNumber, dwCallbackReason: DWORD;
      hSourceFile, hDestinationFile: THandle; 
      f: TMyForm ): DWORD; stdcall;
    var
      newpos: Integer;
    const
      PROCESS_CONTINUE = 0;
    begin
      result := PROCESS_CONTINUE;
      if dwCallbackReason = CALLBACK_CHUNK_FINISHED then begin
        newpos := Round( TotalBytesTransferred / TotalFileSize * 100 );
        with f.Progressbar do
          if newpos <> Position then
            Position := newpos;
        Application.ProcessMessages();
      end;
    end;
    
    ...
    
      // copy 'fromfile' to 'tofile' showing progress in myFor.Progressbar and obey to abort in myForm.bIsAborted
    result := CopyFileEx(  'fromfile', 'tofile', @_CopyCallback, pointer(myForm), @myForm.bIsAborted, 0 );

---

_Source: [https://wiki.freepascal.org/callback](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/callback)_
