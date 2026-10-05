# CopyFile

│ **[English (en)](<../en/CopyFile.md> "CopyFile")** │  **[suomi (fi)](</CopyFile/fi> "CopyFile/fi")** │  **[français (fr)](</CopyFile/fr> "CopyFile/fr")** │  **русский (ru)** │    
****

[Модуль](<../en/Unit.md> "Unit"): Lazarus [fileutil](<fileutil.md> "fileutil/ru") ([UTF-8](<../en/UTF-8.md> "UTF-8") замена для кода FPC [RTL](<../en/RTL.md> "RTL") и дополнительная обработка файлов/каталогов) 
    
    
    // флаги для копирования
    type
     TCopyFileFlag = (
       cffOverwriteFile,
       cffCreateDestDirectory,
       cffPreserveTime
       );
     TCopyFileFlags = set of TCopyFileFlag;
    
    function CopyFile(const SrcFilename, DestFilename: string): boolean;
    function CopyFile(const SrcFilename, DestFilename: string; PreserveTime: boolean): boolean;
    function CopyFile(const SrcFilename, DestFilename: string; Flags: TCopyFileFlags=[cffOverwriteFile]): boolean;
    

**copyfile** копирует файл из места _SrcFilename_ в место _DestFilename_. При желании можно сохранить метку времени файла (флаг _cffPreserveTime_). 

  


## Windows example

Пример: 
    
    
    uses 
    ...
    fileutil
    ...
    CopyFile('c:\autoexec.bat','c:\windows\temp\autoexec.bat.backup');
    

**Результат работы функции** \- вернёт [True](<True.md> "True/ru") при успешном копировании и [False](<False.md> "False/ru") в противном случае. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если вы хотите использовать данную функцию в консольных программах, вам необходимо добавить модуль **LazUtils** , который не будет _тянуть_ за собой весь пакет [LCL](<LCL.md> "LCL/ru")

## Lazarus example

В этом примере используются следующие компоненты или функции: 

  * [TOpenDialog](<../en/TOpenDialog.md> "TOpenDialog") [![topendialog.png](https://wiki.freepascal.org/images/1/1c/topendialog.png)](</File:topendialog.png>)
  * [TSaveDialog](<../en/TSaveDialog.md> "TSaveDialog") [![tsavedialog.png](https://wiki.freepascal.org/images/4/4a/tsavedialog.png)](</File:tsavedialog.png>)
  * [MessageDlg](<../en/Dialog_Examples.md> "Dialog Examples")



  

    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      ok:boolean;
    begin
      ok := false;
      if OpenDialog1.Execute then
        if SaveDialog1.Execute then
          ok := CopyFile(OpenDialog1.FileName, SaveDialog1.FileName);
      if ok then MessageDlg('Файл '+OpenDialog1.FileName+' успешно скопирован в ' +
          SaveDialog1.FileName,mtInformation,[mbOk],0)
      else MessageDlg('Копирование не удалось',mtWarning,[mbOk],0);
    end;

---

_Source: [https://wiki.freepascal.org/CopyFile/ru](https://web.archive.org/web/20250209011403/https://wiki.freepascal.org/CopyFile/ru)_
