# CD open close

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

These procedures open and close a CD / DVD drive: 
    
    
    uses MMSystem, ...;
    
    ...
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
    // opens the CD-ROM drive
    mciSendstring('SET CDAUDIO DOOR OPEN', nil , 0, Form1.Handle);
    mciSendString ('close cdlw', nil, 0, 0)
    end;
    
    procedure TForm1.Button2Click (Sender: TObject);
    begin
    // closes the CD-ROM drive
    mciSendstring('SET CDAUDIO DOOR CLOSED', nil , 0, Form1.Handle);
    mciSendString ('close cdlw', nil, 0, 0)
    end;
    
    ...
    

If you want to wait until the last operation is finished then this procedure will open/close a CD-ROM/DVD depending on the value of the **blnOpen** boolean variable: 
    
    
    uses
      MMSystem, ...;
      
      ...
      
    procedure cdOpenClose(chrDriveChar: char; blnOpen: boolean);
    begin
      if mciSendString(PChar('open ' + chrDriveChar + ': type cdaudio alias cdlw'), nil, 0, 0) = 0 then
        begin
          // open CD-ROM drive
          if blnOpen = True then
            mciSendString('set cdlw door open wait', nil, 0, 0)
          // close CD-ROM drive
          else
            mciSendString('set cdlw door closed wait', nil, 0, 0);
    
          mciSendString('close cdlw', nil, 0, 0);
        end;
    end;
      
      ...

---

_Source: [https://wiki.freepascal.org/CD_open_close](https://web.archive.org/web/20250215133609/https://wiki.freepascal.org/CD_open_close)_
