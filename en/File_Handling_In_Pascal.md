# File Handling In Pascal

│ **English (en)** │  **[русский (ru)](<../ru/File_Handling_In_Pascal.md>)** │

## Contents

  * 1 Overview
  * 2 Old procedural style
    * 2.1 Input/Output error handling
    * 2.2 File procedures
    * 2.3 Example
  * 3 Object style
    * 3.1 Generic files of any type
    * 3.2 FileCopy
    * 3.3 Handling Text files (TStringList)
    * 3.4 Demo: save single string to a file
  * 4 File Handling in ISO Mode
    * 4.1 Command Line Parameters
    * 4.2 Transparent File Names
  * 5 See also



## Overview

Something every programmer needs to know is how to work with files. Files are used to persist data i.e. store data in such a way that it can be retrieved at a later time, without having to recreate it. Files can be used to store user settings, error logs, measurement or calculation results, and more. This page explains the basics of file handling. 

## Old procedural style

When using files in classic (non-object oriented) Pascal, you can use a file of type TextFile (or simply Text) to store text, that is typically structured in lines. Every line ends with an end-of-line marker ([LineEnding](<End_of_Line.md> "End of Line")). You can store strings in this type of file, but also numbers which can be formatted in different ways. These files can be opened and edited inside the Lazarus IDE or any other text editor. 

For specific purposes you can create your own file type that can only store one type of data. For example: 
    
    
    ...
    type
      TIntegerFile  = file of integer;  // Allows you to write only integer numbers to the file
      TExtendedFile = file of extended; // Allows you to write only real numbers to the file
      TCharFile     = file of char;     // Allows you to write only single characters to the file
    

### Input/Output error handling

The [I/O error handling flag](<http://www.freepascal.org/docs-html/prog/progsu38.html#x45-440001.2.38>) tells the compiler how to deal with error situations: raise an exception or store the I/O result in the IOResult variable. The I/O error handling flag is a compiler directive. To enable or disable it: 
    
    
    {$I+} // Errors will lead to an EInOutError exception (default)
    {$I-} // Suppress I/O errors: check the IOResult variable for the error code
    

By suppressing I/O errors ({$I-}) the file operation results go into the IOResult variable. This is a [cardinal (number) type](<Cardinal.md> "Cardinal"). Different numbers mean different errors. So you may want to check the documentation for the different errors[[1]](<http://www.freepascal.org/docs-html/rtl/system/ioresult.html>). 

### File procedures

These file handling procedures and functions are located in [unit System](<System_unit.md> "System unit"). See the FPC documentation for more details: [Reference for 'System' unit](<http://www.freepascal.org/docs-html/rtl/system/index-5.html>). 

  * **AssignFile** (prevent the use of the older **[Assign](<Assign.md> "Assign")** procedure) - Assign a name to a file
  * **Append** \- Opens an existing file for appending data to end of file and editing it
  * **BlockRead** \- Read data from an untyped file into memory
  * **BlockWrite** \- Write data from memory to an untyped file
  * **CloseFile** (prevent the use of the older **Close** procedure) - Close opened file
  * **EOF** \- Check for end of file
  * **Erase** \- Erase file from disk
  * **FilePos** \- Get position in file
  * **FileSize** \- Get size of file
  * **Flush** \- Write file buffers to disk
  * **IOResult** \- Return result of last file IO operation
  * **Read** \- Read from a text file
  * **ReadLn** \- Read from a text file and go to the next line
  * **Reset** \- Opens a file for reading
  * **Rewrite** \- Create a file for writing
  * **Seek** \- Change position in file
  * **SeekEOF** \- Set file position to end of file
  * **SeekEOLn** \- Set file position to end of line
  * **Truncate** \- Truncate the file at position
  * **[Write](<Write.md> "Write")** \- Write variable to a file
  * **WriteLn** \- Write variable to a text file and go to a new line



### Example

A full example of handling a text file of type TextFile: 
    
    
    program CreateFile;
    
    {$mode objfpc}
    
    uses
     Sysutils;
    
    const
      C_FNAME = 'textfile.txt';
    
    var
      tfOut: TextFile;
    
    begin
      // Set the name of the file that will be created
      AssignFile(tfOut, C_FNAME);
    
      // Use exceptions to catch errors (this is the default so not absolutely requried)
      {$I+}
    
      // Embed the file creation in a try/except block to handle errors gracefully
      try
        // Create the file, write some text and close it.
        rewrite(tfOut);
    
        writeln(tfOut, 'Hello textfile!');
        writeln(tfOut, 'The answer to life, the universe and everything: ', 42);
    
        CloseFile(tfOut);
    
      except
        // If there was an error the reason can be found here
        on E: EInOutError do
          writeln('File handling error occurred. Details: ', E.ClassName, '/', E.Message);
      end;
    
      // Give feedback and wait for key press
      writeln('File ', C_FNAME, ' created if all went ok. Press Enter to stop.');
      readln;
    end.
    

Now open the file in any text editor and you will see the above text written to it! You can test the error handling by running the program once, then set the file to read-only and run the program again. 

Note that exception handling was used as that is an easy way to perfom multiple file operations and handling the errors. You could also use {$I-}, but then you would have to check IOResult after each operation and modify your next operation. 

Here's how appending more text to a textfile works: 
    
    
    program AppendToFile;
    
    {$mode objfpc}
    
    uses
     Sysutils;
    
    const
      C_FNAME = 'textfile.txt';
    
    var
      tfOut: TextFile;
    
    begin
      // Set the name of the file that will receive some more text
      AssignFile(tfOut, C_FNAME);
    
      // Embed the file handling in a try/except block to handle errors gracefully
      try
        // Open the file for appending, write some more text to it and close it.
        append(tfOut);
    
        writeln(tfOut, 'Hello again textfile!');
        writeln(tfOut, 'The result of 6 * 7 = ', 6 * 7);
    
        CloseFile(tfOut);
    
      except
        on E: EInOutError do
         writeln('File handling error occurred. Details: ', E.Message);
      end;
    
      // Give feedback and wait for key press
      writeln('File ', C_FNAME, ' might have more text. Press enter to stop.');
      readln;
    end.
    

Reading a textfile: 
    
    
    program ReadFile;
    
    {$mode objfpc}
    
    uses
     Sysutils;
    
    const
      C_FNAME = 'textfile.txt';
    
    var
      tfIn: TextFile;
      s: string;
    
    begin
      // Give some feedback
      writeln('Reading the contents of file: ', C_FNAME);
      writeln('=========================================');
    
      // Set the name of the file that will be read
      AssignFile(tfIn, C_FNAME);
    
      // Embed the file handling in a try/except block to handle errors gracefully
      try
        // Open the file for reading
        reset(tfIn);
    
        // Keep reading lines until the end of the file is reached
        while not eof(tfIn) do
        begin
          readln(tfIn, s);
          writeln(s);
        end;
    
        // Done so close the file
        CloseFile(tfIn);
    
      except
        on E: EInOutError do
         writeln('File handling error occurred. Details: ', E.Message);
      end;
    
      // Wait for the user to end the program
      writeln('=========================================');
      writeln('File ', C_FNAME, ' was probably read. Press enter to stop.');
      readln;
    end.
    

## Object style

In addition to the old style file handling routines mentioned above, a new system exists that uses the concept of streams (- of data) at a higher abstraction level. This means data can be read from or written to any location (disk, memory, hardware ports etc.) by one uniform interface. 

In addition, most string handling classes have the ability to load and save content from/to a file. These methods are usually named SaveToFile and LoadFromFile. A lot of other objects (such as Lazarus grids) have similar functionality, including Lazarus datasets (DBExport). It pays to look through the documentation or source code before trying to roll your own save/load routines. 

### Generic files of any type

For opening files for direct access TFileStream can be used. This class is an encapsulation of the system procedures FileOpen, FileCreate, FileRead, FileWrite, FileSeek and FileClose which resides in unit SysUtils. 

[IO routines](<http://www.freepascal.org/docs-html/rtl/sysutils/ioroutines.html>)

Here is a very simple example of appending one file to another using streams. It is equivalent to the append example above, but much simpler: 
    
    
    program InOutStream;
    
    {$mode objfpc}
    
    uses
      Classes, SysUtils;
    var
      InStream, OutStream: TFileStream;
    begin
      if (ParamCount = 2) and
        FileExists(ParamStr(1)) and
        FileExists(ParamStr(2)) then
      begin
        InStream := TFileStream.Create(ParamStr(1), fmOpenRead);
        try
          OutStream := TFileStream.Create(Paramstr(2), fmOpenWrite);
          try
            OutStream.Position := OutStream.size;
            OutStream.CopyFrom(InStream, 0); // appends
          finally
            OutStream.Free;
          end;
        finally
          InStream.Free;
        end;
      end else 
        Writeln('Usage: inoutstream <infile> <outfile>');
    end.
    

  


In the example below, note how we encapsulate the file handling action with a try..except block so that errors are handled correctly just as with the classic file handling routines (as not to convolute the example the fsOut.write is not put inside a try...finally block). 
    
    
    program WriteBinaryData;
    
    {$mode objfpc}
    
    uses
      Classes, Sysutils;
    
    const
      C_FNAME = 'binarydata.bin';
    
    var
      fsOut    : TFileStream;
      ChrBuffer: array[0..2] of char;
    
    begin
      // Set up some random data that will get stored
      ChrBuffer[0] := 'A';
      ChrBuffer[1] := 'B';
      ChrBuffer[2] := 'C';
    
      // Catch errors in case the file cannot be created
      try
        // Create the file stream instance, write to it and free it to prevent memory leaks
        fsOut := TFileStream.Create( C_FNAME, fmCreate);
        fsOut.Write(ChrBuffer, sizeof(ChrBuffer));
        fsOut.Free;
    
      // Handle errors
      except
        on E:Exception do
          writeln('File ', C_FNAME, ' could not be created because: ', E.Message);
      end;
    
      // Give feedback and wait for key press
      writeln('File ', C_FNAME, ' created if all went ok. Press Enter to stop.');
      readln;
    end.
    

  
You can load entire files into memory too if its size is comparatively smaller than available system memory. Bigger sizes might work but your operating system will start using the page/swap file, making the exercise useless from a performance standpoint. 
    
    
    program ReadBinaryDataInMemoryForAppend;
    
    {$mode objfpc}
    
    uses
      Classes, Sysutils;
    
    const
      C_FNAME = 'binarydata.bin';
    
    var
      msApp: TMemoryStream;
    
    begin
      // Set up the stream
      msApp := TMemoryStream.Create;
    
      // Catch errors in case the file cannot be read or written
      try
        // Read the data into memory
        msApp.LoadFromFile(C_FNAME);
    
        // Seek the end of the stream so data can be appended
        msApp.Seek(0, soEnd);
    
        // Write some arbitrary data to the memory stream
        msApp.WriteByte(68);
        msApp.WriteAnsiString('Some extra text');
        msApp.WriteDWord(671202);
    
        // Store the data back on disk, overwriting the previous contents
        msApp.SaveToFile(C_FNAME);
    
      // Handle errors
      except
        on E:Exception do
          writeln('File ', C_FNAME, ' could not be read or written because: ', E.Message);
      end;
    
      // Clean up
      msApp.Free;
    
      // Give feedback and wait for key press
      writeln('File ', C_FNAME, ' was extended if all went ok. Press Enter to stop.');
      readln;
    end.
    

With larger files of many Gb, you may want to read in buffers of, say, 4096 bytes (you're advised to use a multiple of the filesytem cluster or block size) and do something with the data of each buffer read. 
    
    
    var
      TotalBytesRead, BytesRead : Int64;
      Buffer : array [0..4095] of byte;  // or, array [0..4095] of char
      FileStream : TFileStream;
    
    try
      FileStream := TFileStream.Create;
      FileStream.Position := 0;  // Ensure you are at the start of the file
      TotalBytesRead := 0;
      while TotalBytesRead <= FileStream.Size do  // While the amount of data read is less than or equal to the size of the stream do
      begin
        BytesRead := FileStream.Read(Buffer, SizeOf(Buffer));  // Read in 4096 of data
        Inc(TotalBytesRead, BytesRead);                       // Increase TotalByteRead by the size of the buffer, i.e. 4096 bytes
        // Do something with Buffer data
      end;
    

### FileCopy

With the above, we can implement a simple FileCopy function (FreePascal has none in its RTL although Lazarus has [CopyFile](<CopyFile.md> "CopyFile")) - adjust as needed for bigger files etc: 
    
    
    program FileCopyDemo;
    // Demonstration of FileCopy function
    
    {$mode objfpc}
    
    uses
      Classes;
    
    const
      fSource = 'test.txt';
      fTarget = 'test.bak';
    
    function FileCopy(Source, Target: string): boolean;
    // Copies source to target; overwrites target.
    // Caches entire file content in memory.
    // Returns true if succeeded; false if failed.
    var
      MemBuffer: TMemoryStream;
    begin
      Result := false;
      MemBuffer := TMemoryStream.Create;
      try
        MemBuffer.LoadFromFile(Source);
        MemBuffer.SaveToFile(Target); 
        Result := true
      except
        //swallow exception; function result is false by default
      end;
      // Clean up
      MemBuffer.Free
    end;
    
    begin
      If FileCopy(fSource, fTarget)
        then Writeln('File ', fSource, ' copied to ', fTarget)
        else Writeln('File ', fSource, ' not copied to ', fTarget);
      Readln()
    end.
    

### Handling Text files (TStringList)

In general, for text files you can use the TStringList class to load the entire file into memory and have easy access to its lines. Of course, you can also write the StringList back to a file: 
    
    
    program StringListDemo;
    
    {$mode objfpc}
    
    uses
      Classes, SysUtils;
    
    const
      C_FNAME = 'textfile.txt';
    
    var
      slInfo: TStringList;
    
    begin
      // Create an instance of the string list to handle the textfile
      slInfo := TStringList.Create;
    
      // Embed the file handling in a try/except block to handle errors gracefully
      try
        // Load the contents of the textfile completely in memory
        slInfo.LoadFromFile(C_FNAME);
    
        // Add some more contents
        slInfo.Add('An extra line appended to the text');
        slInfo.Add('And another one.');
        slInfo.Add('Let''s stop here.');
        slInfo.Add('It is now ' + DateTimeToStr(now));
    
        // And write the contents back to disk, replacing the original contents
        slInfo.SaveToFile(C_FNAME);
    
      except
        // If there was an error the reason can be found here
        on E: EInOutError do
          writeln('File handling error occurred. Reason: ', E.Message);
      end;
    
      // Clean up
      slInfo.Free;
    
      // Give feedback and wait for key press
      writeln('File ', C_FNAME, ' updated if all went ok. Press Enter to stop.');
      readln;
    end.
    

### Demo: save single string to a file

In order to write a single string to a stream you might want to use the procedure defined below. Note that strings in FreePascal can be extremely long, this is also a useful way to write a big block of textdata to a file. 
    
    
    program SaveStringToPathDemo;
    
    {$mode objfpc}
    
    uses
      Classes, sysutils;
    
    const
      C_FNAME = 'textstringtofile.txt';
    
    // SaveStringToFile: function to store a string of text into a diskfile.
    //   If the function result equals true, the string was written ok.
    //   If not then there was some kind of error.
    function SaveStringToFile(theString, filePath: AnsiString): boolean;
    var
      fsOut: TFileStream;
    begin
      // By default assume the writing will fail.
      result := false;
    
      // Write the given string to a file, catching potential errors in the process.
      try
        fsOut := TFileStream.Create(filePath, fmCreate);
        fsOut.Write(theString[1], length(theString));
        fsOut.Free;
    
        // At his point it is known that the writing went ok.
        result := true
    
      except
        on E:Exception do
          writeln('String could not be written. Details: ', E.ClassName, ': ', E.Message);
      end
    end;
    
    //
    // Main program
    //
    begin
      // Try to save a simple textstring in a file and give feedback of sucess.
      if SaveStringToFile('>> this text gets stored <<', C_FNAME) then
        writeln('Text succesfully written to a file')
      else
        writeln('Writing text to a file failed.');
    
      // Wait for the user to press Enter
      readln
    end.
    

## File Handling in ISO Mode

### Command Line Parameters

The first option to assign external to internal files in iso mode is through command line parameters according to their order of appearance. Example: 
    
    
    program isotest1 (file1, file2);
    var
      file1: text;
      file2: file of integer;
    begin
      reset(file1);
      readln(file1);
      rewrite(file2);
      write(file2, 5);
    end.
    

The program is compiled with `fpc -Miso isotest1.pas` and run with `./isotest1 external1.txt external2.dat `. files1 is assigned to external1.txt and file2 to external2.dat. Without command line parameters the files are assigned to stdin and stdout. As long as there is only one file for input and one for output, the same assignment can be achieved with this command: `./isotest1 <external1.txt >external2.dat `. file1 must exist prior to the run, file2 will be created. 

### Transparent File Names

The second option is transparent file names, achieved with the compiler option -Sr. Example: 
    
    
    program isotest2 (file1, file2);
    var
      file1: text;
      file2: text;
    begin
      reset(file1);
      readln(file1);
      rewrite(file2);
      write(file2, 5);
    end.
    

If the program is compiled with `fpc -Miso -Sr isotest2.pas` and run without command line parameters, the external files FILE1.txt and FILE2.txt are read and written. Note the uppercase conversion of the internal filenames. Files with different filenames can still be used through command line parameters. 

## See also

  * [CopyFile](<CopyFile.md> "CopyFile") Lazarus function (also available for command line programs) that copies files for you
  * [File](</File> "File")
  * [Howto Use TOpenDialog](<Howto_Use_TOpenDialog.md> "Howto Use TOpenDialog")
  * [Howto Use TSaveDialog](<Howto_Use_TSaveDialog.md> "Howto Use TSaveDialog")

---

_Source: [https://wiki.freepascal.org/File_Handling_In_Pascal](https://web.archive.org/web/20241212111224/https://wiki.freepascal.org/File_Handling_In_Pascal)_
