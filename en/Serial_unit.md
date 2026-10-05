# Serial unit

## Contents

  * 1 Example of usage
  * 2 Timeout values



Unit **Serial** in FPC supports work with serial port. It provides many Ser* functions. 

## Example of usage

Note that this uses Windows-style device names, unix systems (Linux, SunOS etc.) have their own conventions. 
    
    
    {
      Usage:
      TestSerialPortCom
        Uses default port COM1.
      TestSerialPortCom 8 'Hello'
        Uses COM8 and outputs 'Hello' before waiting for an answer.
      
      The program will open a serial port and output 'Hello', after that the code will wait until
      a CR (#13) is received, or a key is pressed.
    }
    program TestSerialPortCom;
    uses
      serial, crt;
    
    var
      serialhandle : LongInt;
      ComPortName  : String;
      s,tmpstr,txt : String;
      ComIn        : String;
      ComPortNr    : Integer;
      writecount   : Integer;
      status       : LongInt;
      Flags        : TSerialFlags; { set of (RtsCtsFlowControl); }
      ErrorCode    : Integer;
    
    begin
      ComPortNr:= 1;
      tmpstr:= '';
      txt:= '';
    
      writeln('Parameters: ', ParamCount);
      if (ParamCount>0) then
      begin
        tmpstr:= ParamStr(1);
        val(tmpstr, ComPortNr, ErrorCode);
    
        if (ParamCount>1) then
          txt:= ParamStr(2);
      end;
    
      str(ComPortNr, tmpstr);
     
      ComPortName:= 'COM'+tmpstr+':';
      writeln('Using: '+ComPortname);
    
      serialhandle := SerOpen(ComPortName);
      Flags:= []; // none
      SerSetParams(serialhandle, 9600, 8, NoneParity, 1, Flags); 
      
      s:= txt; // use the input text
      writeln('Output: '+s);
      s:= s+#13+#10; // CR + LF
      writecount:= length(s);
    
      status:= SerWrite(serialhandle, s[1], writecount);
    
      // for debugging only
      writeln('Status: ', status, ', WriteCount: ', writecount);
    
      if status > 0 then
      begin
        writeln('Waiting for answer');
        s:= '';
        ComIn:= '';
        while (Length(ComIn)<10) and (status>=0) and not KeyPressed do 
        begin
          status:= SerRead(serialhandle, s[1], 10);
          if (s[1]=#13) then 
            status:= -1; // CR => end serial read
    
          if (status>0) then
          begin 
            ComIn:= ComIn+s[1];
            writeln('Status: ', status, ', Len: ', length(ComIn), ', ASCII: ', ord(s[1]), ', Input: ', ComIn);
          end;  
        end;
      end
      else
        writeln('Error: unable to send');
    
      SerSync(serialhandle); // flush out any remaining before closure
    
      SerFlushOutput(serialhandle); // discard any remaining output
    
      SerClose(serialhandle);
    end.
    

## Timeout values

Q: I'm trying to figure out when the timeout timer in SerRead/SerReadTimeout starts. 

A (by FPC developer Christo Crause): FPC uses the OS provided functionality to interact with the serial port. On Windows the timeout seems to start when the read request is made - [Link](<https://learn.microsoft.com/en-us/previous-versions/ff547486\(v=vs.85\)>). On POSIX (at least Linux) it depends on the specific set of flags specified , scroll down to the discussion on canonical/noncanonical mode for the details - [Link](<https://linux.die.net/man/3/termios>). 

A bit more information after peeking into the source code: on Win32 the timeout information can be read or set using Get/SetCommTimeouts. The [COMMTIMEOUTS structure](<https://learn.microsoft.com/en-us/windows/win32/api/winbase/ns-winbase-commtimeouts>) contains a read interval timeout and total timeouts for read & write operations. SerReadTimeout sets the ReadTotalTimeoutConstant value, so basically the total timeout duration. 

On Linux SerReadTimeout uses a fpSelect on the handle with the specified timeout, so in principle it is the same behaviour as on Windows, i.e. a total timeout. 

While both OSs provide some functionality to specify inter character timeouts, FPC does not directly expose this functionality in the Serial unit. 

Note from last maintainer: SerReadTimeout() explicitly says "<https://github.com/MarkMLl/serialcomms>" i.e. the intention is that timing starts when the function is called. Different lineages of unix handle fpSelect() differently, while not tested it's reasonable to assume that the Mac's OS-X behaves like SunOS/Solaris. See <https://github.com/MarkMLl/serialcomms> for examples, comments on known issues etc.

---

_Source: [https://wiki.freepascal.org/Serial_unit](https://web.archive.org/web/20240619094027/https://wiki.freepascal.org/Serial_unit)_
