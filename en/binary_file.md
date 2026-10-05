# binary file

│ **English (en)** │

## Contents

  * 1 Binary file
  * 2 Create file
  * 3 Close file
  * 4 Add a character to the end of an existing file
  * 5 Replace a character in an existing file
  * 6 Read file completely
  * 7 See also



## Binary file

A binary file is suitable for processing files of any size. With binary files, the content of a file is read in character by character. Conversely, the file is written character by character. If a file is opened as a binary file, a character can be changed anywhere in the file. Data of the same type can now be written and read into a file defined with FILE OF as follows: 
    
    
     var
       datFile : File of Byte;    // always reads and writes a character
       datFile : File of Char;    // always reads and writes a character
       datFile : File of Integer; // always reads and writes two characters
       ...
    

Any location in the binary file can be accessed with the SEEK command. 

## Create file
    
    
     var
       datFile    : File of Char;
       chrContent : Char;
    
     begin
       Assignfile(datFile, 'example.txt');  // Assigns the name of the file to the variable txtFile and opens the file
       ReWrite(datFile);                    // File will be overwritten if it exists
       chrContent := 'A';
       Write(datFile, chrContent);          // Write the first character to the new file 
       ...
     end;
    

## Close file
    
    
     var
       datFile    : File of Char;
       chrContent : Char;
    
     begin
       ...
       CloseFile(datFile);  // closes the file
     end;
    

## Add a character to the end of an existing file

For binary files, a character can be added to the end of a file using the Seek command. 
    
    
     var
       datFile    : File of char;
       chrContent : Char;
    
     begin
       AssignFile(datFile, 'example.txt');
       Reset(datFile);                    // Sets the file pointer to the beginning of the file
       Seek(datFile, FileSize(datFile));  // Determines the end of the file and sets the file pointer to the end
       chrContent := 'b';
       Write(datFile, chrContent);        // Write another character at the end of the file
       ...
    

## Replace a character in an existing file

For binary files, you can use the Seek command to replace a character anywhere in the file. 
    
    
     var
       datFile    : File of Char;
       chrContent : Char;
    
     begin
       AssignFile(datFile, 'example.txt');
       Reset(datFile);                     // Sets the file pointer to the beginning of the file
       Seek (datFile, 1);                  // Set the file pointer to the _second_ character in the file
       chrContent := 'f';
       Write(datFile, chrContent);         // Overwrite the second character in the file
       ...
    

## Read file completely
    
    
     var
       datFile    : File of Char;
       chrContent : Char;
    
     begin
       AssignFile(datFile, 'example.txt');
       Reset(datFile);
       
       while not eof(datFile)           // keep reading as long as there is data to read
         do begin
           read(datFile, chrContent);   // reads a single character into chrContent variable
           ...
         end;
    
       CloseFile(datFile);
     end;
    

## See also

  * [File types](<File_types.md> "File types")

---

_Source: [https://wiki.freepascal.org/binary_file](https://web.archive.org/web/20250424050448/https://wiki.freepascal.org/binary_file)_
