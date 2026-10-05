# bzip2lib

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Usage
  * 8 Example code for bzip2lib



### About

bzip2lib provides stream classes for bzip2 compression and decompression. It uses external bzip2 libraries. 

Unit bzip2lib contains 2 streams: 

  * TBzip2CompressStream for compressing (packing) data to bzip2 format.
  * TBzip2DecompressStream for decompressing (unpacking) data back to its original form.



Unit bzip2lib is collected and edited from 2 unit files in [Jcl](<https://wiki.delphi-jedi.org/wiki/JEDI_Code_Library>) package: 

  * JclCompression
  * bzip2



The purpose was to make it compile and work with FPC (Free Pascal Compiler). In the process lots of code with conditional compilation directives was removed and also many unit dependencies were removed. 

The download contains the source unit file and a demo program which compresses and then again decompresses a text file given as a command line parameter. 

### Author

Juha Manninen <juha dot manninen (at) phnet dot fi>

\+ the original authors of Jcl code. 

### License

This code is licensed under the same terms as Jcl library. 

### Download

The latest stable release can be found on <http://sourceforge.net/projects/lazarus-ccr/files/bzip2lib/bzip2lib-1.tar.gz/download>. 

### Change Log

  * Version 1.0, Date: 2009.11.06



### Dependencies / System Requirements

The code uses external bzip2 libraries for its job: 

  * 'bzip2.dll' in Windows
  * 'libbz2.so.1' in Linux



Status: Stable 

Issues: The author has tested the code only under Linux. There it works well. 

### Usage

Just include unit `bzip2lib` in the "uses" section. Set path for it in project settings if needed. The external bzip2 library must be installed in your machine (see *Dependencies*). Use `TBzip2CompressStream` and `TBzip2DecompressStream` just like any stream, connecting them to file stream or memory stream if needed. 

Note: there is a bzip2 component supplied with FPC: packages/bzip2. It is a pure Pascal solution (doesn't need external libraries) but it can only decompress. If you also need compression then bzip2lib is for you. 

### Example code for bzip2lib

  * Compressing a string to a file. "S" is the string to compress.


    
    
    OutFileStream := TFileStream.Create(CompressedFileName, fmCreate);
    CompressStream := TBzip2CompressStream.Create(OutFileStream);
    try
      CompressStream.Write(Pointer(S)^, Length(S));
    finally
      CompressStream.Free;
      OutFileStream.Free;
    end;
    

  * Decompressing from a file to a TStringList (SL). TStringList.LoadFromStream can be used here.


    
    
    SL := TStringList.Create;
    InFileStream := TFileStream.Create(CompressedFileName, fmOpenRead);
    DecompressStream := TBzip2DecompressStream.Create(InFileStream);
    try
      SL.LoadFromStream(DecompressStream);
      SL.SaveToFile(OutFileName);
    finally
      DecompressStream.Free;
      InFileStream.Free;
      SL.Free;
    end;

---

_Source: [https://wiki.freepascal.org/bzip2lib](https://web.archive.org/web/20250314125303/https://wiki.freepascal.org/bzip2lib)_
