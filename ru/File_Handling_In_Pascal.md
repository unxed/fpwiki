# File Handling In Pascal

│ **[العربية (ar)](</File_Handling_In_Pascal/ar> "File Handling In Pascal/ar")** │  **[English (en)](<../en/File_Handling_In_Pascal.md> "File Handling In Pascal")** │  **[español (es)](</File_Handling_In_Pascal/es> "File Handling In Pascal/es")** │  **[suomi (fi)](</File_Handling_In_Pascal/fi> "File Handling In Pascal/fi")** │  **[français (fr)](</File_Handling_In_Pascal/fr> "File Handling In Pascal/fr")** │  **[日本語 (ja)](</File_Handling_In_Pascal/ja> "File Handling In Pascal/ja")** │  **русский (ru)** │  **[中文（中国大陆）‎ (zh_CN)](</File_Handling_In_Pascal/zh_CN> "File Handling In Pascal/zh CN")** │  **[中文（台灣）‎ (zh_TW)](</File_Handling_In_Pascal/zh_TW> "File Handling In Pascal/zh TW")** │    
****

## Contents

  * 1 Введение
  * 2 Процедурный стиль
    * 2.1 Обработка ошибок ввода/вывода
    * 2.2 Процедуры работы с файлами
    * 2.3 Пример
  * 3 Объектный стиль
    * 3.1 Двоичные файлы
    * 3.2 Копирование файла
    * 3.3 Обработка текстовых файлов (TStringList)
    * 3.4 Демо: сохранить одну строку в файл
  * 4 Смотрите так же



## Введение

Каждый программист должен знать, как работать с файлами. Файлы используются для сохранения данных, т. е. в качестве хранилища данных, таким образом, что они могут быть получены в любой момент, без их воссоздания. Файлы можно использовать для сохранения пользовательских настроек, журналов ошибок, измерение или вычисление результатов, и многое другое. В данном разделе описываются основы использования файлов. 

## Процедурный стиль

Это довольно старый стиль, использующейся ещё во времена, когда Pascal не был объектно-ориентированным языком. Суть его в том, что задается тип файла, определяющий, какие будут храниться в нем данные. Для этого, используется конструкция вида: **file of** _< тип данных>_, где _< тип данных>_ \- название типа, который хранит в себе файл. Помимо стандартных типов (integer, extended, char и т.д.), существует особый тип - TextFile. Он определят, что каждая строка заканчивается специальным(ми) символом(ами) конца строки (См. [LineEnding](<End_of_Line.md> "End of Line/ru")). Эти файлы могут быть открыты и отредактированы внутри среды Lazarus или в любом другом текстовом редакторе. 

Ниже представлены примеры создания собственных типов файлов: 
    
    
    ...
    type
      TIntegerFile  = file of integer;  // Позволяет писать только целые числа в файл
      TExtendedFile = file of extended; // Позволяет писать только дробные цифры в файл
      TCharFile     = file of char;     // Позволяет писать только одиночные символы в файл
    

### Обработка ошибок ввода/вывода

Параметр компилятора [обработки ошибок ввода/вывода](<http://www.freepascal.org/docs-html/prog/progsu38.html#x45-440001.2.38>) указывает, как должна вести себя программа при возникновении ошибки в процессе работы с файлами: вызвать исключение или хранить результат операции ввода/вывода в специальной переменной IOResult. 

Это задаётся с помощью специальной директивы компилятора: 
    
    
    {$I+} // В случаи ошибки будет вызвано исключение EInOutError (по умолчанию)
    {$I-} // Подавлять ошибки ввода-вывода: проверьте переменную IOResult для получения кода ошибки.
    

В случаи подавления ошибок ввода-вывода ({$I-}) результат операции с файлом будет храниться в переменной IOResult типа [cardinal (числовой тип)](<../en/Cardinal.md> "Cardinal"). Каждое число, хранимое в IOResult определяет тип возникшей ошибки(подробнее: [[1]](<http://www.freepascal.org/docs-html/rtl/system/ioresult.html>)). 

### Процедуры работы с файлами

Эти процедуры и функции находятся в модуле system. Для более подробной информации смотрите документацию FPC: 
    
    
    [ссылка на модуль 'System'](<http://www.freepascal.org/docs-html/rtl/system/index-5.html>).
    

  * **AssignFile** (не допускайте использование процедуры **Assign**) - Связывает переменную с файлом
  * **Append** \- Открывает существующий файл для записи данных в конец и их редактирования
  * **BlockRead** \- Чтение данных из не типизированного файла в память
  * **BlockWrite** \- Запись данных из памяти в не типизированный файл
  * **CloseFile** (не допускайте использование процедуры **Close**) - Закрыть открытый файл
  * **EOF** \- Проверка наличия конца файла
  * **Erase** \- Стереть файл с диска
  * **FilePos** \- Получить позицию в файле
  * **FileSize** \- Получить размер файла
  * **Flush** \- Записать файловый буфер на диск
  * **IOResult** \- Возвращает результат последней операции ввода\вывода
  * **Read** \- Считать из текстового файла
  * **ReadLn** \- Считать из текстового файла и перейти к следующей строке
  * **Reset** \- Открыть файл для чтения
  * **Rewrite** \- Создать и открыть файл для записи
  * **Seek** \- Изменить позицию в файле
  * **SeekEOF** \- Переместить позицию в файле в его конец
  * **SeekEOLn** \- Переместить позицию в файле в конец строки
  * **Truncate** \- Удалить все данные, после текущей позиции
  * **Write** \- Записать переменную в файл
  * **WriteLn** \- Записать переменную в текстовый файл и перейти к новой строке



### Пример

Пример работы с текстовым файлом (тип TextFile): 
    
    
    program CreateFile;
    
    uses
     Sysutils;
    
    const
      C_FNAME = 'textfile.txt';
    
    var
      tfOut: TextFile;
    
    begin
      // Связываем имя файла с переменной
      AssignFile(tfOut, C_FNAME);
    
      // Использовать исключение для перехвата ошибок (это по умолчанию и указывать не обязательно)
      {$I+}
    
      // Для обработки исключений, используем блок try/except
      try
        // Создать файл, записать текст и закрыть его.
        rewrite(tfOut);
    
        writeln(tfOut, 'Пример текстового файла!');
        writeln(tfOut, 'Пример записи числа: ', 42);
    
        CloseFile(tfOut);
    
      except
        // Если ошибка - отобразить её
        on E: EInOutError do
          writeln('Ошибка обработки файла. Детали: ', E.ClassName, '/', E.Message);
      end;
    
      // Выводим результат операции и ожидаем нажатие Enter
      writeln('Файл ', C_FNAME, ' создан. Нажмите ВВОД для выхода.');
      readln;
    end.
    

Теперь откройте файл в любом текстовом редакторе и вы увидите, что пример текста, записан в нем! Вы можете проверить работу обработки ошибок, установив атрибут файла "только для чтения" и снова запустив программу. 

Обратите внимание, что в примере используется блок try/except. Данный способ позволяет выполнять несколько операций с файлами и использовать обработку исключений. Вы также можете использовать режим {$I-}, но тогда вам придется проверять переменную IOResult после каждой операции с файлами для контроля ошибок. 

Ниже приведен пример записи текста в конец файла: 
    
    
    program AppendToFile;
    
    uses
     Sysutils;
    
    const
      C_FNAME = 'textfile.txt';
    
    var
      tfOut: TextFile;
    
    begin
      // Связываем имя файла с переменной
      AssignFile(tfOut, C_FNAME);
    
      // Для обработки исключений, используем блок try/except
      try
        // Открыть файл для записи в конец, записать текст и закрыть его.
        append(tfOut);
    
        writeln(tfOut, 'Ещё пример текстового файла!');
        writeln(tfOut, 'Результат 6 * 7 = ', 6 * 7);
    
        CloseFile(tfOut);
    
      except
        on E: EInOutError do
         writeln('Ошибка обработки файла. Детали: ', E.Message);
      end;
    
      // Выводим результат операции и ожидаем нажатие Enter
      writeln('Файл ', C_FNAME, ' возможно содержит больше текста. Нажмите ВВОД для выхода.');
      readln;
    end.
    

Чтение текстового файла: 
    
    
    program ReadFile;
    
    uses
     Sysutils;
    
    const
      C_FNAME = 'textfile.txt';
    
    var
      tfIn: TextFile;
      s: string;
    
    begin
      // Вывод некой информации
      writeln('Чтение содержимого файла: ', C_FNAME);
      writeln('=========================================');
    
      // Связываем имя файла с переменной
      AssignFile(tfIn, C_FNAME);
    
      // Для обработки исключений, используем блок try/except
      try
        // Открыть файл для чтения
        reset(tfIn);
    
        // Считываем строки, пока не закончится файл
        while not eof(tfIn) do
        begin
          readln(tfIn, s);
          writeln(s);
        end;
    
        // Готово. Закрываем файл.
        CloseFile(tfIn);
    
      except
        on E: EInOutError do
         writeln('Ошибка обработки файла. Детали: ', E.Message);
      end;
    
      // Выводим результат операции и ожидаем нажатие Enter
      writeln('=========================================');
      writeln('Файл ', C_FNAME, ' считан. Нажмите ВВОД для выхода.');
      readln;
    end.
    

## Объектный стиль

В дополнение к старому методу обработки файлов, упомянутому выше процедурному стилю, существует новая система, которая использует концепции потоков (данных) на более высоком уровне абстракции. Это означает, что данные могут считываться или записываться в любом месте (диск, память, аппаратные порты и т. д.) через один универсальный интерфейс. 

Кроме того, большинство классов обработки строк, могут иметь возможность загружать/сохранять содержимое из/в файл. Эти методы обычно называются SaveToFile и LoadFromFile. 

### Двоичные файлы

Для прямого доступа к файлам, так же удобно использовать класс TFileStream. Этот класс представляет собой инкапсуляцию системных процедур FileOpen, FileCreate, FileRead, FileWrite, FileSeek и FileClose, расположенных в модуле SysUtils. 

[Процедуры ввода-вывода](<http://www.freepascal.org/docs-html/rtl/sysutils/ioroutines.html>)

В приведенном ниже примере обратите внимание, что обработка действий с файлом, расположена внутри блока try..except. Поэтому обработка ошибок происходит так же, как в случаи использования процедур работы с файлами в классическом Pascal. 
    
    
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
      // Создать некоторые случайные данные, которые будут храниться в файле
      ChrBuffer[0] := 'A';
      ChrBuffer[1] := 'B';
      ChrBuffer[2] := 'C';
    
      // Перехват ошибок в случае, если файл не может быть создан
      try
        // Создать экземпляр потока файла, записать в него данные и уничтожить его, чтобы предотвратить утечки памяти
        fsOut := TFileStream.Create( C_FNAME, fmCreate);
        fsOut.Write(ChrBuffer, sizeof(ChrBuffer));
        fsOut.Free;
    
    
      // Обработка ошибки
      except
        on E:Exception do
          writeln('Файл ', C_FNAME, ' не создан, так как: ', E.Message);
      end;
    
      // Выводим результат операции
      writeln('Файл ', C_FNAME, ' создан. Нажмите ВВОД для выхода.');
      //Ожидаем нажатие Enter
      readln;
    end.
    

Вы так же можете загрузить весь файл в память, если его размер существенно меньше, чем имеющийся системной памяти. Если файл превышает размер системной памяти, то ваша операционная система начнет использовать файл подкачки, снижая скорость работы с файлом. 
    
    
    program ReadBinaryDataInMemoryForAppend;
    {$mode objfpc}
    
    uses
      Classes, Sysutils;
    
    const
      C_FNAME = 'binarydata.bin';
    
    var
      msApp: TMemoryStream;
    
    begin
      // Создаем поток в памяти
      msApp := TMemoryStream.Create;
    
      // Перехват ошибок в случае их возникновения
      try
        // Считать данные в память
        msApp.LoadFromFile(C_FNAME);
    
        // Переходим в конец данных
        msApp.Seek(0, soEnd);
    
        // Запись неких данных в поток
        msApp.WriteByte(68);
        msApp.WriteAnsiString('Некий текст');
        msApp.WriteDWord(671202);
    
        // Запись данных на диск (файл будет перезаписан)
        msApp.SaveToFile(C_FNAME);
    
      // Обработка ошибки
      except
        on E:Exception do
          writeln('Файл ', C_FNAME, ' не удалось считать или записать, так как: ', E.Message);
      end;
    
      // Освобождаем память и уничтожаем объект
      msApp.Free;
    
      // Выводим результат операции и ожидаем нажатие Enter
      writeln('Файл ', C_FNAME, ' дописан. Нажмите ВВОД для выхода.');
      readln;
    end.
    

Для работы с файлами большого объёма, рекомендуется использовать буфер, например в 4096 байт. 
    
    
    var
      TotalBytesRead, BytesRead : Int64;
      Buffer : array [0..4095] of byte;  // или, array [0..4095] of char
      FileStream : TFileStream;
    
    try
      FileStream := TFileStream.Create;
      FileStream.Position := 0;  // Установим позицию в начало файла
      while TotalBytesRead <= FileStream.Size do  // Пока объем считанных данных меньше размера файла
      begin
        BytesRead := FileStream.Read(Buffer,sizeof(Buffer));  // Считать 4096 байт данных
        inc(TotalBytesRead, BytesRead);                       // Увеличиваем TotalByteRead на размер буфера, т.е. 4096 байт
        // Что-то делаем с буфером данных
      end;
    

### Копирование файла

Теперь,зная методы работы с файлами, мы можем реализовать простую функцию копирования файла, скажем FileCopy.(такой функции нет в FreePascal, хотя Lazarus её имеет [copyfile](<../en/CopyFile.md> "CopyFile")): 
    
    
    program FileCopyDemo;
    // Пример функции FileCopy
    
    {$mode objfpc}
    
    uses
      classes;
    
    const
      fSource = 'test.txt';
      fTarget = 'test.bak';
    
    function FileCopy(Source, Target: string): boolean;
    // Копируем файл с путем в Source в файл с путем Target.
    // Кэшируем весь файл в память.
    // В случаи успеха возвращаем true, в случаи ошибки - false.
    var
      MemBuffer: TMemoryStream;
    begin
      result := false;
      MemBuffer := TMemoryStream.Create;
      try
        MemBuffer.LoadFromFile(Source);
        MemBuffer.SaveToFile(Target); 
        result := true
      except
        //Подавляем исключение; результатом функции является значение false по умолчанию
      end;
      // Очистка
      MemBuffer.Free
    end;
    
    // Пример использования
    begin
      If FileCopy(fSource, fTarget)
        then writeln('Файл ', fSource, ' скопирован в ', ftarget)
        else writeln('Файл ', fSource, ' не скопирован в ', ftarget);
      readln()
    end.
    

### Обработка текстовых файлов (TStringList)

Для текстовых файлов можно использовать класс TStringList, чтобы загрузить весь файл в память и иметь легкий доступ к его строкам. Вы также можете записать StringList обратно в файл: 
    
    
    program StringListDemo;
    {$mode objfpc}
    
    uses
      Classes, SysUtils;
    
    const
      C_FNAME = 'textfile.txt';
    
    var
      slInfo: TStringList;
    
    begin
      // Создаем TStringList для обработки текстового файла
      slInfo := TStringList.Create;
    
      // Для обработки исключений, используем блок try/except
      try
        // Загружаем файл в память
        slInfo.LoadFromFile(C_FNAME);
    
        // Добавляем некие строки
        slInfo.Add('Некая строка');
        slInfo.Add('Ещё одна строка.');
        slInfo.Add('Всё, хватит.');
        slInfo.Add('Сейчас ' + DateTimeToStr(now));
    
        // Записать содержимое на диск, заменив исходное содержимое
        slInfo.SaveToFile(C_FNAME);
    
      except
        // Обработка ошибки
        on E: EInOutError do
          writeln('Произошла ошибка обработки файла. Причина: ', E.Message);
      end;
    
      // Очистка
      slInfo.Free;
    
      // Выводим результат операции и ожидаем нажатие Enter
      writeln('Файл ', C_FNAME, ' обновлен. Нажмите ВВОД для выхода.');
      readln;
    end.
    

### Демо: сохранить одну строку в файл

Для того, чтобы сохранить одну строку в файл, вы можете воспользоваться процедурой, описанной ниже. 
    
    
    program SaveStringToPathDemo;
    {$mode objfpc}
    
    uses
      Classes, sysutils;
    
    const
      C_FNAME = 'textstringtofile.txt';
    
    // SaveStringToFile: функция для хранения строк текста в файле на диске.
    //   Если результат функции равен True, то строка была написана
    //   Иначе произошла ошибка
    function SaveStringToFile(theString, filePath: AnsiString): boolean;
    var
      fsOut: TFileStream;
    begin
      // По умолчанию результат неудачный
      result := false;
    
      // Записать данную строку в файл, перехватывая ошибки в процессе записи.
      try
        fsOut := TFileStream.Create(filePath, fmCreate);
        fsOut.Write(theString[1], length(theString));
        fsOut.Free;
    
        // На данном этапе известно, что запись прошла успешно.
        result := true
    
      except
        on E:Exception do
          writeln('Строка не записана. Детали: ', E.ClassName, ': ', E.Message);
      end
    end;
    
    //
    // Основная программа
    //
    begin
      // Пытаемся сохранить текст в файл и выводим результат операции
      if SaveStringToFile('>> этот текст сохраняется <<', C_FNAME) then
        writeln('Текст успешно записан в файл.')
      else
        writeln('Не удалось сохранить текст в файл.');
    
      // Ждем нажатия Enter
      readln
    end.
    

## Смотрите так же

  * [CopyFile](<../en/CopyFile.md> "CopyFile") \- функция Lazarus, которая копирует файл
  * [File](</File> "File")

---

_Source: [https://wiki.freepascal.org/File_Handling_In_Pascal/ru](https://web.archive.org/web/20230326130655/https://wiki.freepascal.org/File_Handling_In_Pascal/ru)_
