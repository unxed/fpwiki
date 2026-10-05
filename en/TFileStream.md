# TFileStream

│ **English (en)** │

A **TFileStream** is a descendant of [TStream](<TStream.md> "TStream") that gets/stores its data from/to a file on disk. 

Constant | Decimal | Description   
---|---|---  
fmCreate  | 65280  | Creates a new file   
fmOpenRead  | 0  | opens a file for reading   
fmOpenWrite  | 1  | opens a file for writing   
fmOpenReadWrite  | 2  | opens a file for reading and writing   
fmShareDenyWrite  | 32  | prohibit writing if file is already opened   
  
  * read entire file _fnam_ into a string.


    
    
    function readstream(fnam: string): string;
    var
      strm: TFileStream;
      n: longint;
      txt: string;
    begin
      txt := '';
      strm := TFileStream.Create(fnam, fmOpenRead or fmShareDenyWrite);
      try
        n := strm.Size;
        SetLength(txt, n);
        strm.Read(txt[1], n);
      finally
        strm.Free;
      end;
      result := txt;
    end;
    

  * read 8 bytes of _fnam_ into a byte array:


    
    
    ..
    type
        TByte8Array = array [0..7] of byte;
    ..
    function readstream(fnam: string; position= integer): TByte8Array;
    var
      strm : TFileStream;
    begin
      strm := TFileStream.Create(fnam, fmOpenRead or fmShareDenyWrite);
      try
        strm.Position:=position;
        strm.Read (Result[0],8);
      finally
        strm.Free;
      end;
    end;
    

  * Write a string _txt_ to the file _fnam_.


    
    
    procedure writestream(fnam: string; txt: string);
    var
      strm: TFileStream;
      n: longint;
    begin
      strm := TFileStream.Create(fnam, fmCreate);
      n := Length(txt);
      try
        strm.Position := 0;
        strm.Write(txt[1], n);
      finally
        strm.Free;
      end;
    end;
    

## See also

  * [File types](<File_types.md> "File types")
  * [TFileStream doc](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/tfilestream.html> "doc:rtl/classes/tfilestream.html")
  * [TStream doc](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/tstream.html> "doc:rtl/classes/tstream.html")

---

_Source: [https://wiki.freepascal.org/TFileStream](https://web.archive.org/web/20231007201306/https://wiki.freepascal.org/TFileStream)_
