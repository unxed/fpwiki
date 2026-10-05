# Basic Pascal Tutorial/Chapter 2/Files

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_2/Files/bg> "Basic Pascal Tutorial/Chapter 2/Files/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_2/Files/de> "Basic Pascal Tutorial/Chapter 2/Files/de")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_2/Files/fr> "Basic Pascal Tutorial/Chapter 2/Files/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_2/Files/ja> "Basic Pascal Tutorial/Chapter 2/Files/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_2/Files/zh_CN> "Basic Pascal Tutorial/Chapter 2/Files/zh CN")** │    
****

[ ◄ ](<Formatting_output.md> "Basic Pascal Tutorial/Chapter 2/Formatting output") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<EOLN_and_EOF.md> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF")  
---|---|---  
  
2D - Files (author: Tao Yue, state: changed) 

Reading from a file instead of the console (keyboard) can be done by: 
    
    
    read(File_variable, Argument_list);
    write(File_variable, Argument_list);
    

Similarly with `readln` and `writeln`. The output is stored into the variables named in `Argument_list`. `File_variable` is declared as follows: 
    
    
    var
      ...
      Filein, Fileout : text;
    

The `text` data type indicates that the file is just plain text. 

After declaring a variable for the file, and before reading from or writing to it, we need to associate the variable with the filename on the disk and open the file. This can be done in one of two ways. Typically: 
    
    
    reset(File_variable, 'filename.extension');
    rewrite(File_variable, 'filename.extension');
    

`reset` opens a file for reading, and rewrite opens a file for writing. A file opened with `reset` can only be used with `read` and `readln`. A file opened with `rewrite` can only be used with `write` and `writeln`. 

Turbo Pascal introduced the assign notation. First you assign a filename to a variable, then you call `reset` or `rewrite` using only the variable. 
    
    
    assign(File_variable, 'filename.extension');
    reset(File_variable);
    

The method of representing the path differs depending on your operating system. Windows uses backslashes and drive letters due to its DOS heritage (e.g. `c:\directory\name.pas`), while FreeBSD, macOS and Linux use forward slashes due to their UNIX heritage. 

After you're done with the file, you can close it with: 
    
    
    close (File_variable);
    

Here's an example of a program that uses files. This program was written for Turbo Pascal and DOS, and will create file2.txt with the first character from file1.txt: 
    
    
    program CopyOneByteFile;
    
    var
       Mychar : char;
       Filein, Fileout : text;
    
    begin
       assign(Filein, 'c:\file1.txt');
       reset(Filein);
       assign(Fileout, 'c:\file2.txt');
       rewrite(Fileout);
       read(Filein, Mychar);
       write(Fileout, Mychar);
       close(Filein);
       close(Fileout)
    end.
    

[ ◄ ](<Formatting_output.md> "Basic Pascal Tutorial/Chapter 2/Formatting output") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<EOLN_and_EOF.md> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_2/Files](https://web.archive.org/web/20250401011230/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_2/Files)_
