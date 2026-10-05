# Zlibar

│ **[Deutsch (de)](</Zlibar/de> "Zlibar/de")** │  **English (en)** │    
****

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
    * 4.1 Releases
    * 4.2 Subversion
  * 5 Dependencies / System Requirements
  * 6 Usage
    * 6.1 TZLibWriteArchive
    * 6.2 TZlibReadArchive



# About

**zlibar** is a unit that contains components to create a single compressed file archive that contains many files. It uses paszlib that is included with FreePascal so there are no external library requirements 

The two main components are : 

  * TZlibWriteArchive - Used to create an archive
  * TZlibReadArchive - Used to read the archive and extract it's files



Features of TZlibWriteArchive : 

  * Create the Archive file in a stream, allowing you to save it to a file or do something else creative with it
  * Each file in the archive is stored with a MD5 checksum
  * Each file can be stored with a relative path
  * Contains progress [callbacks](<callback.md> "callback") to inform the program of the current file and all files progress



Features of TZlibReadArchive : 

  * Loads the archive from a TStream.
  * Contains a progress callback to inform the program of the extraction progress
  * Files in the archive are extractable by an index number and are extracted to a TStream



The download contains a demo program, and the unit. Also a package file. 

Although not tested, it should work on any platform. 

**Changes in versions** : 

0.2.3 (19/02/2012) 

  * Fixed compilation with newer versions of fpc



0.2.2 

  * LCL requirement removed
  * FPC version 2.0.4 (or a recent development version) required.
  * Compatible with FPC 2.0.4 ( go figure.... ;)



0.2.1 

  * archives should work across different endian machines now.
  * fixed bad memory corruption bug
  * now requires LCL for endian safeness



# Author

[Andrew Haines](</User:AndrewH> "User:AndrewH")

# License

zlibar.pas is distributed with the same modified LGPL as the LCL, so it is possible to use it in proprietary programs without disclosing your source. 

# Download

## Releases

The latest stable release can be found on the [Lazarus CCR Files page](<http://sourceforge.net/projects/lazarus-ccr/files/ZlibAr/>). 

## Subversion

zlibar is available in the lazarus code and component repository since version 0.2.3: 
    
    
     svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/zlibar> zlibar
    

# Dependencies / System Requirements

  * Free Pascal
  * Demo Program requires Lazarus



Issues: 

  * Tested on Linux.
  * Needs testing on Windows. _Confirmed working on Windows_  
\--[Eyecreate](</index.php?title=User:Eyecreate&action=edit&redlink=1> "User:Eyecreate \(page does not exist\)") 03:22, 23 February 2007 (CET)



Version 0.2.2 will not compile on newer versions of fpc (>2.4.2) Use a version newer than 0.2.2 or subversion. 

# Usage

Add the unit "zlibar" to your uses section. 

## TZLibWriteArchive

Example: 
    
    
    var
      Zar: TZlibWriteArchive;
      Stream: TMemoryStream;
      X: Integer;
    begin    
      Stream := TMemoryStream.Create;
      Zar := TZlibWriteArchive.Create;    
      Zar.OutStream := Stream;
      
      Zar.InputFiles.Add('/path/to/MyFileName1');
      Zar.InputFiles.Add('/path/to/MyFileName2');
    
      Zar.CreateArchive;
      
      Stream.SaveToFile('MySavedFileName');
      Zar.Free;
      Stream.Free;
    end;
    

## TZlibReadArchive

Example: 
    
    
    var
      ArchStream: TMemoryStream;
      FileStream: TMemoryStream;
      ZReadArc: TZlibReadArchive;
      X: Integer;
      DestPath: String;
    begin
      ArchStream := TMemoryStream.Create;
      FileStream := TmemoryStream.Create;
       
      ArchStream.LoadFromFile('ArchiveFileName');
      ZReadArc:= TZlibReadArchive.Create(ArchStream);
      DestPath := '/Some/Path';
      for X := 0 to ZReadArc.Count -1 do begin    
        ZReadArc.ExtractFileToStream(X, FileStream);
        FileStream.SaveToFile(DestPath+ZReadArc.FilesInArchive[X].FilePath+'/'+ZReadArc.FilesInArchive[X].FIleName);
        FileStream.Position := 0;
        FileStream.Size := 0;
      end;
      ZReadArc.Free;
      ArchStream.Free;
      FileStream.Free;  
    end;

---

_Source: [https://wiki.freepascal.org/Zlibar](https://web.archive.org/web/20241209230853/https://wiki.freepascal.org/Zlibar)_
