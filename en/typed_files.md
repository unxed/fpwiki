# typed files

│ **English (en)** │

## Contents

  * 1 Typed file
  * 2 Set the file and record structure
  * 3 Create the file
  * 4 Close the file
  * 5 Write data record
  * 6 Read data record
  * 7 Read file completely
  * 8 Position pointer on a data record
  * 9 AssignFile, CloseFile vs Assign and Close
  * 10 See also



## Typed file

A typed text file is suitable for processing files of any size. A typed text file consists of individual data records. All records in the file have the same structure. This means that each data record is of the same length. 

## Set the file and record structure

The structure applies to the file type and the record type. The file and data record must have the same structure. 
    
    
     type
       TMemployee = Record
       strName : String[20];
       sinContent : Single;
     end;
    

## Create the file
    
    
    var
      datFile : File of TMemployee;
      recData : TMemployee;
    begin
      Assign(datFile, 'example.txt');
    
      // An existing file is deleted and re-created
      ReWrite(datFile);
    
      ...
    end;
    

Or: 
    
    
    {$mode objfpc}
    var
      datFile : File of TMemployee;
      recData : TMemployee;
    begin
      AssignFile(datFile, 'example.txt'); // calls the above, only mode ObjFpc and mode Delphi
    
      // An existing file is deleted and re-created
      ReWrite(datFile);
    
      ...
    end;
    

## Close the file
    
    
    begin
      ...
      Close(datFile);  // closes the file
      ...
    end;
    

Or 
    
    
    {$mode objfpc}
    begin
      ...
      CloseFile(datFile);  // calls the above, only modes that use objpas.
      ...
    end;
    

## Write data record
    
    
    var
      datFile : File of TMemployee;
      recData : TMemployee;
    begin
      ...
    
      // fill the record
      recData.strName := 'abcdefghij';
      recData.sinContent := 1700.21;
    
      // write the record to the file
      Write(datFile, recData); 
    
      ...
    end;
    

## Read data record
    
    
    var
      datFile : File of TMemployee;
      recData : TMemployee;
    begin
      Assign(datFile, 'example.txt');
    
      Reset(datFile);          // Set the file pointer to the beginning of the file
    
      Read(datFile, recData);  // Reads a record from the file
      ...
    end;
    

## Read file completely
    
    
    var
      datFile : File of TMemployee;
      recData : TMemployee;
    begin
      Assign(datFile, 'example.txt');
      Reset (datFile);
    
      while not eof(datFile)      // as long as data can still be read
        do begin
          read(datFile, recData); // read record from file
          ...
        end;
    
      Close(datFile);
    end;
    

## Position pointer on a data record
    
    
    begin
      Assign(datFile, 'example.txt');
      Reset(datFile);
      ...
    
      Seek(datFile, 0);        // position file pointer at start of first record
      Read(datFile, recData);  // read first record into recData variable
    
      recData.sinContent := 1602.22; // update record 
    
      Seek(datFile, 0);              // position file pointer at start of first record
      Write(datFile, recData);       // write first record back
    
      ...
    end;
    

## AssignFile, CloseFile vs Assign and Close

Proper standard Pascal syntax, independent from compiler mode, for file handling is to use simply Assign and Close, which are defined in the system unit. AssignFile and CloseFile were introduced to mitigate clashes with other types where the syntax Assign or Close are used, like in, but not limited to, GUI apps. The latter two only work in modes that use the objpas unit since they are defined in objpas and are non-standard, although many accept them as de-facto standard. They are of great help to avoid such ambiguities, but what these functions do is simply call System.Assign and System.Close, nothing more. They are not inlined, so if you are a purist or work in non-object oriented modes, you may want to prefix Assign and Close with the system unit name: System.Assign and System.Close. That solves the same ambiguity problem but for all modes and is also slightly faster. This remark is also applicable to untyped files. It is entirely up to you what you prefer as long as you are working in one of the modes that use objpas. This simply explains the background. 

## See also

  * [File types](<File_types.md> "File types")

---

_Source: [https://wiki.freepascal.org/typed_files](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/typed_files)_
