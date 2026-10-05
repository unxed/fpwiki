# Text

│ [**English (en)**](<../en/Text.md> "Text") │  [**日本語 (ja)**](</Text/ja> "Text/ja") │  **русский (ru)** │    
Тип **TextFile** (или эквивалентно более старой записи, просто **Text**) используется в программах на Pascal для чтения из текстового файла либо для записи в текстовый файл. 
    
    
    {$mode objfpc}{$H+}
    var 
      MyFile: TextFile;
      s: string;
    begin
      AssignFile(MyFile, 'a.txt');
     
      try
        reset(MyFile);    //Отрыть файл для чтения
        readln(MyFile, s);
        writeln('Текст прочитан из файла: ', s) 
     
        {
        или добавить некоторый текст:
        append(MyFile);
        writeln(MyFile, 'некоторый текст'); 
        }
     
      finally
        CloseFile(MyFile)
      end
    end.

[Переменная](<Variable.md> "Variable/ru"), представляющая текстовый файл (_MyFile_ в примере выше), может быть использована для чтения и/или записи в текущий файл. Она должна быть связана с текущим файлом посредством процедуры [AssignFile](</index.php?title=AssignFile/ru&action=edit&redlink=1> "AssignFile/ru \(page does not exist\)") из [библиотеки времени выполнения](<RTL.md> "RTL/ru"). после этого файл должен быть открыт с помощью процедуры [Reset](</index.php?title=Reset/ru&action=edit&redlink=1> "Reset/ru \(page does not exist\)"), [Rewrite](</index.php?title=Rewrite/ru&action=edit&redlink=1> "Rewrite/ru \(page does not exist\)") или [Append](</index.php?title=Append/ru&action=edit&redlink=1> "Append/ru \(page does not exist\)"). Вы можете читать или писать в файл, используя процедуры **Read** , **Readln** , **Write** , **Writeln**. После окончания обработки файла, вам необходимо закрыть его, используя процедуру [CloseFile](</index.php?title=CloseFile/ru&action=edit&redlink=1> "CloseFile/ru \(page does not exist\)"). 

Обратите внимание, что тип **TextFile** сильно отличается от типа **file of char** : 

  * **file of char** \- простая последовательность однобайтовых символов и вы можете читать или писать только один символ за раз. Т.е. вы можете вызвать только **Read(F, C)** или **Write(F, C)** , где C - переменная типа **char**. 


  * **TextFile** предлагает намного больше функций и представляет обычную концепцию текстовых файлов. Вы можете использовать **Read** , **Readln** , **Write** , **Writeln** для чтения/записи из текстового файла значений стандартных типов, таких как строковые, целые или вещественные числа. Символ [конца строки](<End_of_Line.md> "End of Line/ru") обрабатывается автоматически: когда происходит чтение, распознаются различные символы конца строк; когда происходит запись, то используется символ конца строки, принятый для текущей операционной системы. 



File-related types, procedures and functions: 

    [File](</File> "File") \- [Text](<../en/Text.md> "Text") \- [AssignFile](</index.php?title=AssignFile&action=edit&redlink=1> "AssignFile \(page does not exist\)") \- [CloseFile](</index.php?title=CloseFile&action=edit&redlink=1> "CloseFile \(page does not exist\)") \- [Reset](</index.php?title=Reset&action=edit&redlink=1> "Reset \(page does not exist\)") \- [Rewrite](</index.php?title=Rewrite&action=edit&redlink=1> "Rewrite \(page does not exist\)") \- [Get](</index.php?title=Get&action=edit&redlink=1> "Get \(page does not exist\)") \- [Put](</index.php?title=Put&action=edit&redlink=1> "Put \(page does not exist\)") \- [Read](<../en/Read.md> "Read") \- [Readln](<../en/Read.md> "Read") \- [Write](<../en/Write.md> "Write") \- [Writeln](<../en/Write.md> "Write")

---

_Source: [https://wiki.freepascal.org/Text/ru](https://web.archive.org/web/20230127070910/https://wiki.freepascal.org/Text/ru)_
