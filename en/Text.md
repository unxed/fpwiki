# Text

│ **English (en)** │  [**日本語 (ja)**](</Text/ja> "Text/ja") │  [**русский (ru)**](<../ru/Text.md> "Text/ru") │    
****

The type **TextFile** (or, equivalent and older, just **Text**) is used in a Pascal program to read from and write to a text file. 
    
    
    {$mode objfpc}{$H+}
    var 
      MyFile: TextFile;
      s: string;
    begin
      AssignFile(MyFile, 'a.txt');
    
      try
        reset(MyFile);    //Reopen the file for reading
        readln(MyFile, s);
        writeln('Text read from file: ', s) 
       
        {
        or add some text:
        append(MyFile);
        writeln(MyFile, 'some text'); 
        }
    
      finally
        CloseFile(MyFile)
      end
    end.
    

The [variable](<Variable.md> "Variable") representing the text file (_MyFile_ in the example above) may be used to read from, write to, or both to the actual file. It must be tied to the actual file by a [run-time library](<RTL.md> "RTL") routine [AssignFile](</index.php?title=AssignFile&action=edit&redlink=1> "AssignFile \(page does not exist\)"). Then the file must be opened by the [Reset](</index.php?title=Reset&action=edit&redlink=1> "Reset \(page does not exist\)"), [Rewrite](</index.php?title=Rewrite&action=edit&redlink=1> "Rewrite \(page does not exist\)") or [Append](</index.php?title=Append&action=edit&redlink=1> "Append \(page does not exist\)") procedure. You can read and write to file using Read, Readln, Write, Writeln. After you have finished processing the file, you should release the necessary file resources by closing the file calling [CloseFile](</index.php?title=CloseFile&action=edit&redlink=1> "CloseFile \(page does not exist\)"). 

Note that **TextFile** type is very different than the **file of char** type: 

  * **file of char** is just a simple sequence of single byte characters and you can only read or write a single character at a time. That is, you can only call **Read(F, C)** or **Write(F, C)** where C is variable of type **char**.


  * **TextFile** offers much more functions, and represents the usual concept of a text file. You can use Read, Readln, Write, Writeln to read/write from a text file a number of standard types, like strings, integers and floating-point values. [Line endings](<End_of_Line.md> "End of Line") are also automatically handled: when reading, various line endings are recognized; when writing, the current OS line endings are used.



File-related types, procedures and functions: 

    [File](</File> "File") \- Text \- [AssignFile](</index.php?title=AssignFile&action=edit&redlink=1> "AssignFile \(page does not exist\)") \- [CloseFile](</index.php?title=CloseFile&action=edit&redlink=1> "CloseFile \(page does not exist\)") \- [Reset](</index.php?title=Reset&action=edit&redlink=1> "Reset \(page does not exist\)") \- [Rewrite](</index.php?title=Rewrite&action=edit&redlink=1> "Rewrite \(page does not exist\)") \- [Get](<Get.md> "Get") \- [Put](</index.php?title=Put&action=edit&redlink=1> "Put \(page does not exist\)") \- [Read](<Read.md> "Read") \- [Readln](<Read.md> "Read") \- [Write](<Write.md> "Write") \- [Writeln](<Write.md> "Write")

---

_Source: [https://wiki.freepascal.org/Text](https://web.archive.org/web/20210511012009/https://wiki.freepascal.org/Text)_
