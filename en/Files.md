# Files

│ **English (en)** │

[ ◄ ](<Formatting_output.md> "Formatting output") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<EOLN_and_EOF.md> "EOLN and EOF")  
---|---|---  
  
2D - Files (author: Tao Yue, state: unchanged) 

Reading from a file instead of the console (keyboard) can be done by: 
    
    
    read (file_variable, argument_list);
    write (file_variable, argument_list);
    

Similarly with `readln` and `writeln`. file_variable is declared as follows: 
    
    
    var
      ...
      filein, fileout : text;
    

The `text` data type indicates that the file is just plain text. 

After declaring a variable for the file, and before reading from or writing to it, we need to associate the variable with the filename on the disk and open the file. This can be done in one of two ways. Typically: 
    
    
    reset (file_variable, 'filename.extension');
    rewrite (file_variable, 'filename.extension');
    

`reset` opens a file for reading, and rewrite opens a file for writing. A file opened with `reset` can only be used with `read` and `readln`. A file opened with `rewrite` can only be used with `write` and `writeln`. 

Turbo Pascal introduced the assign notation. First you assign a filename to a variable, then you call `reset` or `rewrite` using only the variable. 
    
    
    assign (file_variable, 'filename.extension');
    reset (file_variable);
    

The method of representing the path differs depending on your operating system. Windows uses backslashes and drive letters due to its DOS heritage (e.g. `c:\directory\name.pas`), while FreeBSD, macOS and Linux use forward slashes due to their UNIX heritage. 

After you're done with the file, you can close it with: 
    
    
    close (File_Identifier);
    

Here's an example of a program that uses files. This program was written for Turbo Pascal and DOS, and will create file2.txt with the first character from file1.txt: 
    
    
    program CopyOneByteFile;
    
    var
       mychar : char;
       filein, fileout : text;
    
    begin
       assign (filein, 'c:\file1.txt');
       reset (filein);
       assign (fileout, 'c:\file2.txt');
       rewrite (fileout);
       read (filein, mychar);
       write (fileout, mychar);
       close(filein);
       close(fileout)
    end.
    

[ ◄ ](<Formatting_output.md> "Formatting output") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<EOLN_and_EOF.md> "EOLN and EOF")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Files](https://web.archive.org/web/20221007003416/https://wiki.freepascal.org/Files)_
