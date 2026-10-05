# untyped files

│ **[Deutsch (de)](</untyped_files/de> "untyped files/de")** │  **English (en)** │  **[polski (pl)](</untyped_files/pl> "untyped files/pl")** │    
****

## Contents

  * 1 Untyped text file
  * 2 Create file
  * 3 Open file
  * 4 Close file
  * 5 Append a record to an existing file
  * 6 Read file completely
  * 7 See also



## Untyped text file

An untyped text file is suitable for processing files of any size. With untyped files, the content of a file is read line by line. Conversely, the file is written line by line. The data records of an untyped file can be of different lengths. 

## Create file
    
    
     var
       datFile : TextFile;  // or: datFile: text
       strName : String;
       sinContent : Single;
     begin
       Assign(datFile, 'example.txt');  // Assigns the name of the file to the variable datFile and opens the file
       ReWrite(datFile);                // The old file is overwritten
       strName := 'qwert';
       sinContent := 1800.23;
       WriteLn(datFile, strName, sinContent);  // Writes the first record to the new file
       ...
     end;
    

In the OBJPFC or DELPHI compiler mode, the AssignFile procedure is available in addition to Assign to avoid confusion with TPersistent.Assign. 

## Open file
    
    
     var
       datFile : TextFile;
       strName : String;
       sinContent : Single;
     begin
       Assign(datFile, 'example.txt');        // Assigns the name of the file to the datFile variable
       Reset(datFile);                        // Sets the file pointer to the beginning of the file
       ReadLn(datFile, strName, sinContent);  // Reads the first line from the file
       ReadLn(datFile, strName, sinContent);  // Reads the second line from the file
       ...
     end;
    

## Close file
    
    
     var
       datFile : TextFile;
       strName : String;
       sinContent : Single;
     begin
       ...
       Close(datFile);  // closes the file
     end;
    

Similar to AssignFile / Assign, the CloseFile procedure is available in OBJPFC or DELPHI compiler mode. 

## Append a record to an existing file

For text files (untyped files), a data record can be added to the end of the file using the Append command. 
    
    
     var
       datFile : TextFile;
       strName : String;
       sinContent : Single;
     begin
       Assign(datFile, 'example.txt');   // Assigns the name of the file to the datFile variable
       Append(datFile);                  // Opens the file for write access and adds records to the end of the file
       strName := 'sdf' ;
       sinContent : = 2001.21 ;
       WriteLn(datFile, strName, sinContent);  // Writes another record to the new file
       Close(datFile);                         // Closes the file
    

## Read file completely
    
    
     var
       datFile : TextFile;
       strName : String;
       sinContent : Single;
     begin
       Assign(datFile, 'example.txt');
       Reset(datFile);
    
       while not eof(datFile)                  // As long as data can still be read
         do begin
           read(datFile, strName, sinContent); // Read file content
         end;
    
       Close(datFile);
     end;
    

## See also

  * [File types](<File_types.md> "File types")

---

_Source: [https://wiki.freepascal.org/untyped_files](https://web.archive.org/web/20250502004034/https://wiki.freepascal.org/untyped_files)_
