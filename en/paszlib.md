# paszlib

│ **English (en)** │  **[русский (ru)](<../ru/paszlib.md>)** │

**paszlib** is a Pascal conversion of the standard zlib library: you don't need any external dependencies. It was implemented by Jacques Nomssi Nzali (his old homepage is dead, see a continuation of the project [here](<http://sageshome.net/oss/paszlib-sg.php>)). It is used in the FCL to implement the TCompressionStream class. 

This class lets you compress and decompress **.zip** files. 

The main unit of this package is **paszlib**. There are other, auxiliary units, but the only unit that needs to be included in a typical program is this one. 

## Contents

  * 1 TZipper
    * 1.1 Documentation
    * 1.2 Examples
      * 1.2.1 Zip files
      * 1.2.2 Unzip files
      * 1.2.3 Zip files with encoding filenames
      * 1.2.4 Unzip files with encoding filenames
      * 1.2.5 Unzip file to a stream
      * 1.2.6 Zipping a whole directory tree
      * 1.2.7 Zipping a whole directory tree storing only a relative path
    * 1.3 See also



# TZipper

TZipper implements support for compressing and decompressing .zip files, but does not support all zip compression methods. 

## Documentation

See [official FPC documentation for Zipper](<http://www.freepascal.org/docs-html/fcl/zipper/index.html>). 

## Examples

### Zip files

Create a zip file named as first parameter. Treats all other parameters as filenames to add, so you can specify e.g. 
    
    
    zipper newzip.zip autoexec.bat config.sys
    
    
    
    uses
      Zipper;
    var
      OurZipper: TZipper;
      I: Integer;
    begin
      OurZipper := TZipper.Create;
      try
        // Define the file name of the zip file to be created
        OurZipper.FileName := ParamStr(1);
        for I := 2 to ParamCount do
          // Specify the names of the files to be included in the zip as first argument
          // The second argument is the name of the file as it appears in the zip and 
          // later in the file system after unzipping
          OurZipper.Entries.AddFileEntry(ParamStr(I), ParamStr(I));
        // Execute the zipping operation and write the zip file.
        OurZipper.ZipAllFiles;
      finally
        OurZipper.Free;
      end;
    end.
    

Please note that the names of the files to be zipped are assumed to contain only characters of the old IBM PC character set (code page 437). For extended characters (UTF-8), see the section below. 

### Unzip files

Unzip all files in archive c:\test.zip into directory c:\windows\temp\ 
    
    
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
    

### Zip files with encoding filenames

As already noted, the zip file format originally was written to support only code page 437 of the IBM PC. In modern operating systems, however, the encoding of file names is much more general. 

If your **FPC is version 3.2 or newer** you can take advantage of the boolean property `UseLanguageEncoding`. When set to `true`, file names are assumed to have the default encoding of FPC, in case of Lazarus programs this is UTF-8. Here any file name can be passed to the zipper (except for restrictions of the OS, of course). 

In the following practical routine for FPC 3.2, a zip file is created from the files in a given directory so that file names are considered correctly: 
    
    
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
           // Set the name of the zip file to be created
           OurZipper.FileName := AZipFileName;
    
           // Read names of the files contained in ADirectory to a stringlist
           list := TStringList.Create;
           try
             // FindAllFiles adds all file names matching the mask (e.g. '*.*')
             // found in the given directory to the provided list.
             // When IncludingSubDirs is true the search continues recursively in
             // the subdirectories.
             FindAllFiles(list, ADirectory, AMask, IncludingSubDirs);
             for i := 0 to list.Count - 1 do
             begin
               // diskfilename is the name of the file to be zipped on the disk
               diskFileName := list[i];
               // archivefilename is the name of the file to be zipped as it appears
               // in the zip. We remove the deirectory from the
               archiveFileName := StringReplace(diskFileName, ADirectory, '', []);
               // Store these filenames for the zipper
               OurZipper.Entries.AddFileEntry(diskFileName, archiveFileName);
             end;
           finally
             list.Free;
           end;
           // By default zipper writes file names in encoding of the IBM PC, CP437.
           // UTF8 encoding is written when UseLanguageEncoding is true.
           OurZipper.UseLanguageEncoding := true;  // Requires FPC 3.2+
           // create and write the zip file
           OurZipper.ZipAllFiles;
           Result := true;
         finally
           OurZipper.Free;
         end;
      end else
        Result := false;
    end;
    

If your **FPC is older than v3.2** you must convert the ArchiveFileName argument to CP437 - of course, this is not possible for all characters, and thus you must be very careful with zipper in this case. Note also that the `DiskFileName` must have the encoding required by the operating system, otherwise the file will not be found for zipping; in case of FPC before 3.0 this may require another code page conversion. 

Here is the adaption of above example for an FPC version before 3.2: 
    
    
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
             // List all files in ADirectory and, if required, its sub-directories.
             FindAllFiles(list, ADirectory, AMask, IncludingSubDirs);
             for i := 0 to list.Count-1 do
             begin
               // Name of the file to be zipped on disk
               diskFileName := list[i];
               {$IF FPC_FullVersion < 30000}
               diskFileName := UTF8ToWinCP(diskFileName);
               {$IFEND}
               // Name of the file to be zipped in the archive: we remove the
               // common path and thus make the files relative to the directory
               // into which they will be unzipped later.
               archiveFileName := StringReplace(list[i], ADirectory, '', []);
               archiveFileName := UTF8ToCP437(archiveFileName);
               OurZipper.Entries.AddFileEntry(diskFileName, archiveFileName);
             end;
           finally
             list.Free;
           end;
           // Execute zipping action and create zip file
           OurZipper.ZipAllFiles;
           Result := true;
         finally
           OurZipper.Free;
         end;
      end;
    end;
    

Another restriction is the path delimiter in the archived file name. Specification of the zip format requires that forward slashes ('`/`') are used even in case of Windows. This is important for the one-parameter overload of `TZipper.Entries.AddFileEntry(DiskfileName)` which simply assume the archived file name to be equal to the `DiskFileName`, without any adaptions. Therefore, such a zip file will contain backslashes on Windows. It will unzip correctly on Windows, but not on Linux where the backslash is considered to be a valid file name character. It is highly recommended to use the two-parameter version of `TZipper.Entries.AFileEntry(DiskFileName, ArchiveFileName)` with `ArchivefileName = DiskFileName` because this procedure automatically replaces the path delimiters as required. 

### Unzip files with encoding filenames
    
    
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
    

More examples can be found in the FPC source directory: 

  * examples: [[1]](<http://svn.freepascal.org/svn/fpc/trunk/packages/paszlib/examples/>)
  * the test program: [[2]](<http://svn.freepascal.org/svn/fpc/trunk/packages/paszlib/tests>)



### Unzip file to a stream

In Lazarus, drop a TMemo, TButton, TEdit and TFileNameEdit on a form. Clicking the button will read the zip file in FileNameEdit, extract the file specified in the Edit box, and display the content in Memo. 
    
    
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
    

### Zipping a whole directory tree

  * This will recursively add the contents of 'C:\MyFolder' to the 'myzipfile.zip' 
    * Note that the absolute path is stored in the zipfile
    * Note that this requires the Lazarus fileutil unit (which you can probably work around)


    
    
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
    

### Zipping a whole directory tree storing only a relative path

This is more complicated, but it can be done. Note that this requires the Lazarus FileUtil unit (which you can probably work around). 
    
    
    uses
      ..., Zipper, FileUtil, StrUtils;
    var
      AZipper: TZipper;
      szPathEntry: String;
      i: Integer;
      ZEntries: TZipFileEntries;
      TheFileList: TStringList;
      RelativeDirectory: String;
    begin
      AZipper := TZipper.Create;
      try
        try
          AZipper.Filename := 'myzipfile.zip';
          RelativeDirectory:='C:\MyFolder\MyFolder\';
          AZipper.Clear;
          ZEntries := TZipFileEntries.Create(TZipFileEntry);
          // Verify valid directory
          If DirPathExists(RelativeDirectory) then
          begin
            // Construct the path to the directory BELOW RelativeDirectory
            // If user specifies 'C:\MyFolder\Subfolder' it returns 'C:\MyFolder\'
            // If user specifies 'C:\MyFolder' it returns 'C:\'
            // If user specifies 'C:\' it returns 'C:\'
            i:=RPos(PathDelim,ChompPathDelim(RelativeDirectory));
            szPathEntry:=LeftStr(RelativeDirectory,i);
    
            // Use the FileUtils.FindAllFiles function to get everything (files and folders) recursively
            TheFileList:=TstringList.Create;
            try
              FindAllFiles(TheFileList, RelativeDirectory);
              for i:=0 to TheFileList.Count -1 do
              begin
                // Make sure the RelativeDirectory files are not in the root of the ZipFile
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
              E.CreateFmt('Zipfile could not be created%sReason: %s', [LineEnding, E.Message])
          end;
        result := True;
      finally
        FreeAndNil(ZEntries);
        AZipper.Free;
      end;  
    end;
    

Note that this example uses an overloaded version of AddFileEntry() (compared to simple examples). This version allows you to specify the directory structure inside the Zip file, and, then, of course, the directory structure when its all unzipped. You can, for example, specify only the file name with no directory structure and have all files returned in one flat output directory. Even if they came for all over the place. 
    
    
    ZEntries.AddFileEntry(FullDiskPathToFile, FileName);
    

## See also

  * [official FPC documentation for Zipper](<http://www.freepascal.org/docs-html/fcl/zipper/index.html>)
  * [Article](<http://www.freepascal.org/~michael/articles/archives/archives.pdf>) demonstrating handling tar, bzip2, gzip, zip files and Blowfish encryption in FreePascal/Lazarus. A good introduction even though it was written some time ago (a lot of functionality has been improved).
  * [unzip](<unzip.md> "unzip")
  * [FreePascalArchivePackage](<FreePascalArchivePackage.md> "FreePascalArchivePackage") Abbrevia archive/zip library
  * [MIT licensed Delphi/Object Pascal library](<http://code.google.com/p/kstools/>) that includes zip file support.



Go back to [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/paszlib](https://web.archive.org/web/20250405055717/https://wiki.freepascal.org/paszlib)_
