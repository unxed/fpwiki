# File Into Trash

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[Deutsch (de)](</File_Into_Trash/de> "File Into Trash/de")** │  **English (en)** │    
****

This article deals with Windows programming. This function moves a file to the trash (recycle) bin. 
    
    
    uses
      ShellAPI, 
      FileUtil;
      
      ...
      
    function FileIntoTrashBin(strFilename : string) : boolean;
    var
      fileStructure : TSHFileOpStruct;
    begin
      FillChar(fileStructure, SizeOf(fileStructure), 0);
    
      with fileStructure do
        begin
          wFunc := FO_DELETE ;
          // Allows umlauts etc. in the file name
          pFrom := PChar(UTF8ToSys(strFilename));
          fFlags := FOF_ALLOWUNDO + FOF_NOCONFIRMATION + FOF_SILENT;
        end;
    
      Result := ShFileOperation(fileStructure) = 0;
    end;
      
       ...

---

_Source: [https://wiki.freepascal.org/File_Into_Trash](https://web.archive.org/web/20240910173145/https://wiki.freepascal.org/File_Into_Trash)_
