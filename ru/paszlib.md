# paszlib

│ **[Deutsch (de)](</paszlib/de> "paszlib/de")** │  **[English (en)](<../en/paszlib.md> "paszlib")** │  **[한국어 (ko)](</paszlib/ko> "paszlib/ko")** │  **[polski (pl)](</paszlib/pl> "paszlib/pl")** │  **русский (ru)** │    
****

**paszlib** представляет собой преобразование стандартной библиотеки zlib на Паскаль: вам не нужны никакие внешние зависимости. Его реализовал Jacques Nomssi Nzali (его старая домашняя страница мертва, см. продолжение проекта [здесь](<http://sageshome.net/oss/paszlib-sg.php>)). Он используется в FCL для реализации класса TCompressionStream. 

Этот класс позволяет сжимать и распаковывать файлы **.zip**. 

Основной модуль этого пакета - **paszlib**. Существуют и другие вспомогательные модули, но единственный модуль, который необходимо включить в типичную программу, - этот. 

## Contents

  * 1 TZipper
    * 1.1 Документация
    * 1.2 Примеры
      * 1.2.1 Упаковка файлов
      * 1.2.2 Распаковка файлов
      * 1.2.3 Упаковка файлов с сохранением кодировки имен файлов
      * 1.2.4 Распаковка файлов с сохранением кодировки имен
      * 1.2.5 Распаковка файла в поток (память)
      * 1.2.6 Архивирование всего дерева каталогов
      * 1.2.7 Архивирование всего дерева каталогов с сохранением только относительного пути
    * 1.3 См. также



# TZipper

TZipper поддерживает сжатие и распаковку файлов .zip, но не поддерживает все методы сжатия zip. 

## Документация

См. [официальную документацию FPC для Zipper](<http://www.freepascal.org/docs-html/fcl/zipper/index.html>)

## Примеры

### Упаковка файлов

Создание zip-файла с именем в качестве первого параметра. Все остальные параметры обрабатываются как имена файлов для добавления, поэтому вы можете указать, например, 
    
    
    zipper newzip.zip autoexec.bat config.sys
    
    
    
    uses
      Zipper;
    var
      OurZipper: TZipper;
      I: Integer;
    begin
      OurZipper := TZipper.Create;
      try
        // Опредяем имя создаваемого zip-файла.
        OurZipper.FileName := ParamStr(1);
        for I := 2 to ParamCount do
          // В качестве первого аргумента указываем имена файлов, которые будут включены в zip-архив.
          // Второй аргумент - это имя файла в том виде, в котором оно отображается в zip-архиве, и
          // позже в файловой системе после распаковки
          OurZipper.Entries.AddFileEntry(ParamStr(I), ParamStr(I));
        // Выполняем операцию архивирования и записываем архивный файл.
        OurZipper.ZipAllFiles;
      finally
        OurZipper.Free;
      end;
    end.
    

Обратите внимание, что имена архивируемых файлов должны содержать только символы из старого набора символов IBM PC (кодовая страница 437). Расширенные символы (UTF-8) см. в разделе ниже. 

### Распаковка файлов

Распаковываем все файлы из архива c:\test.zip в папку c:\windows\temp\ 
    
    
    uses
      Zipper;
    var
      UnZipper: TUnZipper;
    begin
      UnZipper := TUnZipper.Create;
      try    
        UnZipper.FileName := 'c:\test.zip';
        UnZipper.OutputPath := 'c:\windows\temp';
        UnZipper.Examine;
        UnZipper.UnZipAllFiles;
      finally
        UnZipper.Free;
      end;
    end.
    

### Упаковка файлов с сохранением кодировки имен файлов

Как уже отмечалось, формат файла zip изначально был написан для поддержки только кодовой страницы 437 IBM PC. Однако в современных операционных системах кодировка имен файлов является гораздо более общей. 

Если у вас версия **FPC 3.2 или новее** , вы можете воспользоваться логическим свойством `UseLanguageEncoding`. Если установлено значение `true`, предполагается, что имена файлов имеют кодировку по умолчанию FPC, в случае программ Lazarus это UTF-8. Здесь в zipper можно передать любое имя файла (за исключением, конечно, ограничений ОС). 

В следующей практической программе для FPC 3.2 из файлов в заданном каталоге создается zip-файл, чтобы имена файлов считались правильными: 
    
    
    uses
      FileUtil, zipper;
    
    function PackFiles(AZipFilename, ADirectory, AMask: String;
      IncludingSubDirs: Boolean): Boolean;
    var
      OurZipper: TZipper;
      list: TStringList;
      i: Integer;
      diskFileName, archiveFileName: String;
    begin
      Result := false;
      ADirectory := IncludeTrailingPathDelimiter(ADirectory);
      if DirectoryExists(ADirectory) then
      begin
         OurZipper := TZipper.Create;
         try
           // Задаем имя создаваемого zip-файла
           OurZipper.FileName := AZipFileName;
    
           // Считываем имена файлов, содержащихся в ADirectory, в stringlist
           list := TStringList.Create;
           try
             // FindAllFiles добавляет все имена файлов, соответствующие маске (например, '*.*'),
             // найденных в указанном каталоге в предоставленный список.
             // Если IncludingSubDirs - true, поиск продолжается рекурсивно и в
             // подкаталогах.
             FindAllFiles(list, ADirectory, AMask, IncludingSubDirs);
             for i := 0 to list.Count - 1 do
             begin
               // diskfilename - это имя файла, который будет заархивирован на диске
               diskFileName := list[i];
               // archivefilename - это имя файла, который нужно заархивировать, как он выглядит
               // в zip. Убираем директорию из
               archiveFileName := StringReplace(diskFileName, ADirectory, '', []);
               // Сохраняем эти файлы для архиватора
               OurZipper.Entries.AddFileEntry(diskFileName, archiveFileName);
             end;
           finally
             list.Free;
           end;
           // По умолчанию архиватор записывает имена файлов в кодировке IBM PC CP437.
           // Кодировка UTF8 записывается, когда UseLanguageEncoding - true.
           OurZipper.UseLanguageEncoding := true;  // Требуется FPC 3.2+
           // создаем и записываем zip-файл
           OurZipper.ZipAllFiles;
           Result := true;
         finally
           OurZipper.Free;
         end;
      end else
        Result := false;
    end;
    

Если у вас версия **FPC меньше v3.2** , вы должны преобразовать аргумент ArchiveFileName в CP437 - конечно, это возможно не для всех символов, и поэтому вы должны быть очень осторожны с архиватором в этом случае. Также обратите внимание, что `DiskFileName` должен иметь кодировку, требуемую операционной системой, иначе файл для архивирования не будет найден; в случае FPC до 3.0 может потребоваться еще одно преобразование кодовой страницы. 

Вот адаптация приведенного выше примера для версии FPC до 3.2: 
    
    
    uses
      FileUtil, LConvEncoding, LazUTF8, Zipper;
    
    function PackFiles(AZipFilename, ADirectory, AMask: String;
      IncludingSubdirs: Boolean): Boolean;
    var
      OurZipper: TZipper;
      list: TStringList;
      i: Integer;
      diskFileName, archiveFileName: String;
    begin
      Result := false;
      if AMask = '' then AMask := '*.*';
      ADirectory := IncludeTrailingPathDelimiter(ADirectory);
      if DirectoryExists(ADirectory) then
      begin
         OurZipper := TZipper.Create;
         try
           OurZipper.FileName := AZipFileName;
           list := TStringList.Create;
           try
             // Перечислияем все файлы в ADirectory и, если необходимо, его подкаталоги.
             FindAllFiles(list, ADirectory, AMask, IncludingSubDirs);
             for i := 0 to list.Count-1 do
             begin
               // Имя файла, который нужно заархивировать на диске
               diskFileName := list[i];
               {$IF FPC_FullVersion < 30000}
               diskFileName := UTF8ToWinCP(diskFileName);
               {$IFEND}
               // Имя файла, который нужно заархивировать в архиве: убираем
               // общий путь и таким образом обозначаем файлы относительно каталога
               // в который они будут распакованы позже.
               archiveFileName := StringReplace(list[i], ADirectory, '', []);
               archiveFileName := UTF8ToCP437(archiveFileName);
               OurZipper.Entries.AddFileEntry(diskFileName, archiveFileName);
             end;
           finally
             list.Free;
           end;
           // Выполняем действие архивирования и создаем архивный файл.
           OurZipper.ZipAllFiles;
           Result := true;
         finally
           OurZipper.Free;
         end;
      end;
    end;
    

Еще одно ограничение - это разделитель пути в имени заархивированного файла. Спецификация формата zip требует, чтобы косая черта (слэш) ('`/`') использовалась даже в случае Windows. Это важно для однопараметрической перегрузки `TZipper.Entries.AddFileEntry(DiskfileName)`, которая просто предполагает, что имя заархивированного файла равно `DiskFileName`, без каких-либо адаптаций. Следовательно, такой zip-файл будет содержать обратный слэш в Windows. Он будет правильно распакован в Windows, но не в Linux, где обратный слэш считается допустимым символом имени файла. Настоятельно рекомендуется использовать двухпараметрическую версию `TZipper.Entries.AFileEntry(DiskFileName, ArchiveFileName)` с `ArchivefileName = DiskFileName`, поскольку эта процедура автоматически заменяет разделители путей по мере необходимости. 

### Распаковка файлов с сохранением кодировки имен
    
    
    uses
      Zipper, LConvEncoding;
    ...
    function EndPathCP866ToUTF8(AText:string):string;
    var
      c,i:integer;
      s,s1,s2,chr:string;
    begin
      s:='';
      c:=UTF8Length(AText);
      for i:=c downto 1 do
      begin
           chr:=UTF8Copy(AText,i,1);
           if ((not(chr='/')) and (not(chr='\')))or(i=c) then
           begin
                s:=UTF8Copy(AText,i,1)+s;
           end
           else begin
                s:=UTF8Copy(AText,i,1)+s;
                break;
           end;
      end;
      dec(i);
      s1:=UTF8Copy(AText,1,i);
      s2:=CP866ToUTF8(s);
      Result:=s1+s2;
    end;
    
    function UnPackFiles(Filename, UnPackPath: String): Integer;
    var
      UnZipper          :TUnZipper; //PasZLib
      UnPackFileDir,
      ADiskFileName,
      ANewDiskFileName,
      AArchiveFileName  :String;
      i                 :integer;
    begin
      Result:=-1;
      if FileExists(Filename)and DirectoryExists(UnPackPath) then
      begin
           UnPackFileDir :=SysUtils.IncludeTrailingPathDelimiter(UnPackPath);
           UnZipper      :=TUnZipper.Create;
           try
              UnZipper.FileName   := Filename;
              UnZipper.OutputPath := UnPackPath;
              UnZipper.Examine;
              UnZipper.UnZipAllFiles;
    
              for i:=UnZipper.Entries.Count-1 downto 0 do
              begin
                  AArchiveFileName:=UnZipper.Entries.Entries[i].ArchiveFileName;
                  AArchiveFileName:=EndPathCP866ToUTF8(AArchiveFileName);
                  AArchiveFileName:=UTF8ToSys(AArchiveFileName);
                  ANewDiskFileName:=UnPackFileDir+AArchiveFileName;
                  ADiskFileName   :=UnPackFileDir+UnZipper.Entries.Entries[i].DiskFileName;
    
                  if FileExists(ADiskFileName) then
                  begin
                     RenameFile(ADiskFileName, ANewDiskFileName);
                  end
                  else if DirectoryExists(ADiskFileName) then
                  begin
                     ADiskFileName    :=SysUtils.IncludeTrailingPathDelimiter(ADiskFileName);
                     ANewDiskFileName :=SysUtils.IncludeTrailingPathDelimiter(ANewDiskFileName);
                     RenameFile(ADiskFileName, ANewDiskFileName);
                  end;
              end;
    
              Result:=1;
           finally
              UnZipper.Free;
           end;
      end;
    end;
    

Дополнительные примеры можно найти в исходном каталоге FPC: 

  * примеры: [[1]](<http://svn.freepascal.org/svn/fpc/trunk/packages/paszlib/examples/>)
  * тестовые программы: [[2]](<http://svn.freepascal.org/svn/fpc/trunk/packages/paszlib/tests>)



### Распаковка файла в поток (память)

В Lazarus поместите в форму TMemo, TButton, TEdit и TFileNameEdit. Нажатие кнопки приведет к чтению zip-файла в FileNameEdit, извлечению файла, указанному в поле Edit, и отображению содержимого в Memo. 
    
    
    uses
      Zipper;
    
    ...
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      ExtractFileFromZip(FileNameEdit1.FileName,Edit1.Text);
    end;
    
    procedure TForm1.DoCreateOutZipStream(Sender: TObject; var AStream: TStream;
      AItem: TFullZipFileEntry);
    begin
      AStream:=TMemorystream.Create;
    end;
    
    procedure TForm1.DoDoneOutZipStream(Sender: TObject; var AStream: TStream;
      AItem: TFullZipFileEntry);
    begin
      AStream.Position:=0;
      Memo1.lines.LoadFromStream(Astream);
      Astream.Free;
    end;
    
    procedure TForm1.ExtractFileFromZip(ZipName, FileName: string);
    var
      ZipFile: TUnZipper;
      sl:TStringList;
    begin
      sl:=TStringList.Create;
      sl.Add(FileName);
      ZipFile := TUnZipper.Create;
      try
        ZipFile.FileName := ZipName;
        ZipFile.OnCreateStream := @DoCreateOutZipStream;
        ZipFile.OnDoneStream:=@DoDoneOutZipStream;
        ZipFile.UnZipFiles(sl);
      finally
        ZipFile.Free;
        sl.Free;
      end;
    end;
    

### Архивирование всего дерева каталогов

  * Этот пример рекурсивно добавит содержимое 'C: MyFolder' в 'myzipfile.zip' 
    * Обратите внимание, что в zip-файле абсолютный путь сохраняется.
    * Обратите внимание, что для этого требуется модуль Lazarus fileutil (который, вероятно, можно обойти)


    
    
    Uses ...Zipper,FileUtil
    var
      AZipper: TZipper;
      TheFileList:TStringList;
    begin
      MyDirectory:='C:\MyFolder';
      AZipper := TZipper.Create;
      AZipper.Filename := 'myzipfile.zip';
      TheFileList:=TStringList.Create;
      try
        FindAllFiles(TheFileList, MyDirectory);
        AZipper.Entries.AddFileEntries(TheFileList);
        AZipper.ZipAllFiles;
      finally
        TheFileList.Free;
        AZipper.Free;
      end;
    end;
    

### Архивирование всего дерева каталогов с сохранением только относительного пути

  * Это сложнее, но это можно сделать 
    * Обратите внимание, что для этого требуется модуль Lazarus fileutil (который, вероятно, можно обойти)


    
    
    Uses ...Zipper,FileUtil,strutils
    var
      AZipper: TZipper;
      szPathEntry:String;
      i:Integer;
      ZEntries : TZipFileEntries;
      TheFileList:TStringList;
      RelativeDirectory:String;
    begin
      AZipper := TZipper.Create;
      try
        try
          AZipper.Filename := 'myzipfile.zip';
          RelativeDirectory:='C:\MyFolder\MyFolder\';
          AZipper.Clear;
          ZEntries := TZipFileEntries.Create(TZipFileEntry);
          // Проверяем существование каталога
          If DirPathExists(RelativeDirectory) then
          begin
            // Создаем путь к каталогу НИЖЕ RelativeDirectory.
            // Если пользователь указал 'C:\MyFolder\Subfolder' это вернет 'C:\MyFolder\'
            // Если пользователь указал 'C:\MyFolder' это вернет 'C:\'
            // Если пользователь указал 'C:\' это вернет 'C:\'
            i:=RPos(PathDelim,ChompPathDelim(RelativeDirectory));
            szPathEntry:=LeftStr(RelativeDirectory,i);
    
            // Используйте функцию FileUtils.FindAllFiles для рекурсивного получения всего (файлов и папок)
            TheFileList:=TstringList.Create;
            try
              FindAllFiles(TheFileList, RelativeDirectory);
              for i:=0 to TheFileList.Count -1 do
              begin
                // Убедитесь, что файлы RelativeDirectory не находятся в корне ZipFile.
                ZEntries.AddFileEntry(TheFileList[i],CreateRelativePath(TheFileList[i],szPathEntry));
              end;
            finally
              TheFileList.Free;
            end;
          end;
          if (ZEntries.Count > 0) then
            AZipper.ZipFiles(ZEntries);
          except
            On E: EZipError do
              E.CreateFmt('Zip-файл %s не может быть создан по причине %s:', [LineEnding, E.Message])
          end;
        result := True;
      finally
        FreeAndNil(ZEntries);
        AZipper.Free;
      end;  
    end;
    

Обратите внимание, что в этом примере используется перегруженная версия addfileentry() (по сравнению с простыми примерами). Эта версия позволяет вам указать структуру каталогов внутри Zip-файла, а затем, конечно же, структуру каталогов после его распаковки. Вы можете, например, указать только имя файла без структуры каталогов, и все файлы будут возвращены в одном плоском выходном каталоге. Даже если они собраны отовсюду! 
    
    
    ZEntries.AddFileEntry(FullDiskPathToFile, FileName);
    

## См. также

  * [официальная документация FPC для Zipper](<http://www.freepascal.org/docs-html/fcl/zipper/index.html>)
  * [Статья](<http://www.freepascal.org/~michael/articles/archives/archives.pdf>) демонстрация обработки файлов tar, bzip2, gzip, zip и шифрования Blowfish в FreePascal / Lazarus. Хорошее введение, хотя оно было написано некоторое время назад (улучшена большая часть функциональности).
  * [unzip](<../en/unzip.md> "unzip")
  * [FreePascalArchivePackage](<../en/FreePascalArchivePackage.md> "FreePascalArchivePackage") архиватор/zip-библиотека Abbrevia
  * [MIT licensed Delphi/Object Pascal library](<http://code.google.com/p/kstools/>) that includes zip file support.



Назад к [Packages List](<../en/Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/paszlib/ru](https://web.archive.org/web/20250403202239/https://wiki.freepascal.org/paszlib/ru)_
