# CopyFile

│ **English (en)** │  **[suomi (fi)](</CopyFile/fi> "CopyFile/fi")** │  **[français (fr)](</CopyFile/fr> "CopyFile/fr")** │  **[русский (ru)](<../ru/CopyFile.md> "CopyFile/ru")** │    
****

[Unit](<Unit.md> "Unit"): Lazarus [fileutil](<fileutil.md> "fileutil") ([UTF-8](<UTF-8.md> "UTF-8") replacements for FPC [RTL](<RTL.md> "RTL") code and additional file/directory handling) 
    
    
    // flags for copy
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
    

[Function](<Function.md> "Function") **copyfile** copies a source file to a destination file location. Optionally it preserves the file's timestamp. 

**Function result** Returns [boolean](<Boolean.md> "Boolean") value [True](<True.md> "True") if successful, [False](<False.md> "False") if there was an error. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** If you want to use this function in [command line](<Command-line_interface.md> "Command-line interface") programs, add a project requirement for [LazUtils](<LazUtils.md> "LazUtils"), which will not pull in the entire [LCL](<LCL.md> "LCL")

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** This function can**not** be used with wildcards (*.*, etc)

## Windows example
    
    
    uses 
    ...
    fileutil
    ...
    CopyFile('c:\autoexec.bat','c:\windows\temp\autoexec.bat.backup');
    

  


## Lazarus example

The following components or functions are used in this example:: 

  * [TOpenDialog](<TOpenDialog.md> "TOpenDialog") [![topendialog.png](https://wiki.freepascal.org/images/1/1c/topendialog.png)](</File:topendialog.png>)
  * [TSaveDialog](<TSaveDialog.md> "TSaveDialog") [![tsavedialog.png](https://wiki.freepascal.org/images/4/4a/tsavedialog.png)](</File:tsavedialog.png>)
  * [MessageDlg](<Dialog_Examples.md> "Dialog Examples")



  

    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      ok:boolean;
    begin
      ok := false;
      if OpenDialog1.Execute then
        if SaveDialog1.Execute then
          ok := CopyFile(OpenDialog1.FileName, SaveDialog1.FileName);
      if ok then MessageDlg('File '+OpenDialog1.FileName+' successfully copied to '+
          SaveDialog1.FileName,mtInformation,[mbOk],0)
      else MessageDlg('Copying failed',mtWarning,[mbOk],0);
    end;

---

_Source: [https://wiki.freepascal.org/CopyFile](https://web.archive.org/web/20230203230047/https://wiki.freepascal.org/CopyFile)_
