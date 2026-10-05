# TBufStream

**TBufStream** is the common ancestor for the **TReadBufStream** and **TWriteBufStream** streams. It completely handles the buffer memory management and position management. An instance of TBufStream should never be created directly. It also keeps the instance of the source stream. 

TReadBufStream and TWriteBufStream can be used to speed up (or reduce memory consumption) of I/O processes where your application is reading or writing small 'lumps' of data and the underlying operating system is far more efficient handling data in sizable 'lumps'. 

And example is presented below where a TStringList contained a large number of strings totaling about 100,000 characters was to be saved to disk. Using TWriteBufStream (instead of StringList.SaveToFile) reduced to time required by a factor of four. In this example, the StringList already contains it's data and for clarity try..finally structures have not been used. Your production code should differ ... 
    
    
    uses bufstream;
    
    var
      WBufStream: TWriteBufStream;
      FileStream: TFileStream; 
    begin
      ....
      FileStream := TFileStream.Create('somefile.text', fmCreate);
      WBufStream := TWriteBufStream.Create(FileStream);
      StringList.SaveToStream(WBufStream);
      WBufStream.Free;
      FileStream.Free;
    

In use, the WBufStream will accept and store strings from the StringList, one by one, until its buffer is full (default 16k), it then uses the FileStream to write that content to disk, emptying its own buffer and starts again. 

In the same unit there is **TBufferedFileStream** that appears to be even more suited to our need here - 
    
    
    uses bufstream;
    var
      BufFileStream: TBufferedFileStream;
    begin
      ....
      BufFileStream := TBufferedFileStream.Create('somefile.text', fmCreate);
      StringList.SaveToStream(BufFileStream);
      BufFileStream.Free;
    

  


### Buffer Size

Note that BufferedFileStream has a fixed 32k buffer when created, you can then change that with a call such as BufFileStream.InitializeCache(2048, 4); TReadBufStream and TWriteBufStream buffer size can be set in the Create call, the documentation says that the default value is '16', in fact, its 16K. The numbers in all the buffer setting fields are in bytes. 

In some cases it may be worth your while establishing an optimum value for the buffer size, especially if a lot of buffers may be created at the same time. I, personally, found that for my particular application BufferedFileStream needed (2048, 4) and TWriteBufStream delivered the same level of performance with only 1024. That is substantially lower than the default values. 

## See Also

  * <https://www.freepascal.org/docs-html/current/fcl/bufstream/twritebufstream.html>
  * <https://www.freepascal.org/docs-html/current/fcl/bufstream/treadbufstream.html>
  * <https://www.freepascal.org/docs-html/current/fcl/bufstream/tbufstream.html>
  * <https://www.freepascal.org/docs-html/current/fcl/bufstream/tbufferedfilestream.html>
  * [TFileStream](<TFileStream.md> "TFileStream")

---

_Source: [https://wiki.freepascal.org/TBufStream](https://web.archive.org/web/20220807213243/https://wiki.freepascal.org/TBufStream)_
