# Rosetta Stone

## Contents

  * 1 Introduction
  * 2 Programming languages
  * 3 Operating systems
  * 4 Emulating uname
  * 5 Emulating which
  * 6 Emulating whoami
  * 7 Finding all occurrences of some bytes in a file
  * 8 Search and replace in a file



## Introduction

Often, people new to a programming language/library have trouble mapping the things they know how to do in other environments to the new environments. 

It can be handy to have a sort of "Rosetta stone" that roughly translates tasks from one environment/language into another. 

## Programming languages

If you have experience in other programming languages (such as C++, Visual Basic), there are some Rosetta stone web pages that might help you. As the Free Pascal/Lazarus syntax and library is fairly consistent with Delphi's, sites that "translate" to Delphi are also a valuable resource. 

Some useful pages: 

  * <http://mc-computing.com/Languages/> This page has some explanation of the concepts involved
  * <http://rosettacode.org/wiki/Category:Pascal> A huge number of languages, even esoteric, and tasks, make this site special
  * [Pascal for C users](<Pascal_for_C_users.md> "Pascal for C users")
  * [Pascal for CSharp users](<Pascal_for_CSharp_users.md> "Pascal for CSharp users")
  * [Pascal for Java users](<Pascal_for_Java_users.md> "Pascal for Java users")



## Operating systems

If you know how to do something in an operating system, chances are you can do the same using FPC/Lazarus. 

The table below gives a rough mapping of some tasks: 

**Windows/DOS** | **Unix/Linux** | **Description** | **FreePascal/Lazarus**  
---|---|---|---  
assoc |  | Show/edit file associations | See [FileAssociation](<FileAssociation.md> "FileAssociation")  
copy | cp | Copy file | Lazarus [fileutil.copyfile](<CopyFile.md> "CopyFile"); roll your own with filestreams, see e.g. [File_Handling_In_Pascal#FileCopy](<File_Handling_In_Pascal.md> "File Handling In Pascal")  
date /t | date | Show current date | sysutils.now (date/time); sysutils.date (date); sysutils.time (time)   
del, erase | rm, rm -f | Delete file | sysutils.deletefile, erase   
deltree, rmdir /s | rm -r,rm -rf | Remove directories with subdirectories | Lazarus fileutil.deletedirectory   
dir | ls | Find files in a directory | Lazarus [FindAllFiles](<FindAllFiles.md> "FindAllFiles")  
diskpart list volume | mount | Show partitions/volumes/drives on a computer | Windows: see [Windows_Programming_Tips#Listing_all_available_drives](<Windows_Programming_Tips.md> "Windows Programming Tips") Linux: see [Linux_Programming_Tips#Get_the_list_of_mounted_partitions](<Linux_Programming_Tips.md> "Linux Programming Tips") Unix: you'll have to parse some files (e.g. /etc/mtab) yourself, depending on Unix flavour; see [Forum post](<http://lazarus.freepascal.org/index.php/topic,15943.msg86569.html#msg86569>)  
find | grep | Search for a text/string | Several options, including the pos, ansipos functions. Regular expression support is provided by the [RegEx packages](<RegEx_packages.md> "RegEx packages").   
ldifde | ldapsearch | Search through Active Directory/LDAP | Use [Synapse](<Synapse.md> "Synapse") ldapsend unit to work with LDAP queries; see e.g. [example code on Leonardo Ramé's blog](<http://leonardorame.blogspot.com/2007_02_01_archive.html>)  
md5sum | md5sum | Calculate hash of file contents | You can use the MDFile function in the md5 FPC unit.   
mkdir | mkdir | Create directory | mkdir; forcedirectories (creates entire path, if necessary)   
move | mv | Move file to another directory | Within partition/disk: sysutils.renamefile; otherwise copy and delete original   
rename, ren | mv | Rename file | sysutils.renamefile (can also "move" a file between directories, but only on same partition/disk   
sc query <servicename> | service --status-all | Show status for service | See [ServiceManager](<ServiceManager.md> "ServiceManager")  
? | sed -i 's|Delphi|FreePascal|g' readme.txt | Search and replace text in file. | See example code below.   
shutdown | shutdown | Shut down computer | Windows: see [forum post](<http://forum.lazarus.freepascal.org/index.php/topic,16258.msg87992.html#msg87992>)  
tasklist | ps | List or find processes | Windows: [Windows_Programming_Tips#Showing.2Ffinding_processes](<Windows_Programming_Tips.md> "Windows Programming Tips")  
? | touch | Create a new empty file | sysutils.filecreate(filename)   
? | touch | Modify file time | See Create a new file date/de  
? | uname | Get kernel/system info | See uname function below.   
wget, curl | wget, curl | Download file from HTTP/FTP | Multiple options, e.g. use synapse httpsend or ftpsend units: [Synapse#Downloading_files](<Synapse.md> "Synapse")  
? | which | Search for executable in path | Lazarus fileutil.FindDefaultExecutablePath; see also the example below.   
? | whoami | Find out username | See UserName function below. On Windows: you can check if you're Administrator: winutils.iswindowsadmin(). On *nix: fpgeteuid shows you the effective userid (0 for root).   
  
## Emulating uname

Works on Linux, probably FreeBSD: 
    
    
    program uname;
    
    uses  baseunix;
    
    var KernelName: UtsName;
    
    begin
     fpuname(KernelName);
     writeln(kernelname.sysname);
     writeln(kernelname.nodename);
     writeln(kernelname.release);
     writeln(kernelname.version);
     writeln(kernelname.machine);
     writeln(kernelname.domain);
    end.
    

Sample output: 
    
    
    Linux
    mydebianmachine
    3.12-1-amd64
    #1 SMP Debian 3.12.6-2 (2013-12-29)
    x86_64
    (none)
    

## Emulating which

Although Lazarus has FindDefaultExecutablePath, it has some flaws. Use e.g. this code: 
    
    
    function Which(Executable: string): string;
    var
      Output: string;
    begin
      {$IFDEF UNIX}
      // Note: we're using external which because
      // FindDefaultExecutablePath
      // doesn't check if the user has execute permission
      // on the found file.
      ExecuteCommand('which '+Executable,Output,false);
      // Remove trailing LF(s) and other control codes:
      while (length(output)>0) and (ord(output[length(output)])<$20) do
        delete(output,length(output),1);
      {$ELSE}
      Output:=FindDefaultExecutablePath(Executable);
      {$ENDIF UNIX}
      // Extra check:
      if (Output<>'') and fileexists(Output) then
      begin
        result:=Output;
      end
      else
      begin
        result:=''; //command failed
      end;
    end;
    

## Emulating whoami

... or getting the current user's operating system username. Returns a UTF8 string. 
    
    
    uses
      {$IFDEF UNIX}
      {$IF (DEFINED(LINUX)) OR (DEFINED(FREEBSD))}
      ,users //not available on OSX
      {$ENDIF}
      ,baseunix //for fpgetuid
      .... //other unix stuff
      {$ENDIF}
      {$IFDEF MSWINDOWS}
      ,windows
      {$ENDIF}
      ,lazutf8
    
    function UserName: string;
    // Get Operating System user name
    {$IFDEF WINDOWS}
    const
      MaxLen = 256;
    var
      Len: DWORD;
      WS: WideString;
      Res: windows.BOOL;
    {$ENDIF}
    begin
      Result := '';
      {$IFDEF WINDOWS}
        Len := MaxLen;
        {$IFNDEF WINCE}
        // Check for Win98..WinME:
        if Win32MajorVersion <= 4 then
        begin
          SetLength(Result,MaxLen);
          Res := Windows.GetUserName(@Result[1], Len);
          if Res then
          begin
            SetLength(Result,Len-1);
            Result := SysToUtf8(Result);
          end
          else
            SetLength(Result,0);
        end
        else
        {$ENDIF NOT WINCE}
        begin
          SetLength(WS, MaxLen-1);
          Res := Windows.GetUserNameW(@WS[1], Len);
          if Res then
          begin
            SetLength(WS, Len - 1);
            Result := Utf16ToUtf8(WS);
          end
          else
            SetLength(Result,0);
        end;
        {$ENDIF WINDOWS}
        {$IFDEF UNIX}
        {$IF (DEFINED(LINUX)) OR (DEFINED(FREEBSD))}
        Result := SysToUtf8(GetUserName(fpgetuid));
        {$ENDIF}
        if Result = '' then
          Result := GetEnvironmentVariableUTF8('USER'); //fallback or other unixes which export $USER and don't support GetUserName
      {$ENDIF}
    end;
    

## Finding all occurrences of some bytes in a file

A sort of grep. From forum post by KpjComp: [[1]](<http://lazarus.freepascal.org/index.php/topic,16574.msg90287.html#msg90287>)

The block search class: 
    
    
    unit block_search;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, fgl;
    
    type
    
      { TBlockSearch }
      TBlockSearchResults = specialize TFPGList<int64>;
    
      TBlockSearch = class
      private
        src:TStream;
        fresults:TBlockSearchResults;
        block:array of byte;
      public
        procedure SearchFor(a:array of byte);
        constructor Create(_Src:TStream; blocksize:integer = 1024*1024);
        destructor Destroy; override;
        property Results:TBlockSearchResults read fResults;
      end;
    
    implementation
    
    { TBlockSearch }
    
    procedure TBlockSearch.SearchFor(a: array of byte);
    var
      readsize:integer;
      fPos:Int64;
      fifoBuff:array of byte;
      fifoSt,fifoEn,searchLen,lpbyte:integer;
    
      //
      procedure CheckPos;
      var
        l,p:integer;
      begin
        p := fifoST;
        for l := 0 to pred(SearchLen) do
        begin
          if a[l] <> fifoBuff[p] then exit;
          //p := (p+1) mod SearchLen,   the if seems quicker
          inc(p); if p >= SearchLen then p := 0;
        end;
        fresults.Add(fpos-searchLen);
      end;
      //
    begin
      fresults.clear;
      src.Position:=0;
      readsize := src.Read(block[0],Length(block));
      searchLen := length(a);
      if searchLen > length(block) then
        raise Exception.Create('Search term larger than blocksize');
      if readsize < searchLen then exit;
      setlength(fifoBuff,searchLen);
      move(block[0],fifoBuff[0],searchLen);
      fPos:=0;
      fifoSt:=0;
      fifoEn:=SearchLen-1;
      CheckPos;
      while readsize > 0 do
      begin
        for lpByte := 0 to pred(readsize) do
        begin
          inc(fifoSt); if fifoSt>=SearchLen then fifoST := 0;
          inc(fifoEn); if fifoEn>=SearchLen then fifoEn := 0;
          fifoBuff[fifoEn] := block[lpByte];
          inc(fPos);
          CheckPos;
        end;
        readsize := src.Read(block[0],Length(block));
      end;
    end;
    
    constructor TBlockSearch.Create(_Src: TStream; blocksize: integer);
    begin
      inherited Create;
      setlength(block,blocksize);
      src := _src;
      fresults := TBlockSearchResults.Create;
    end;
    
    destructor TBlockSearch.Destroy;
    begin
      freeAndNil(fresults);
      inherited Destroy;
    end;
    
    end.
    

Example of usage: 
    
    
    var
      bs:TBlockSearch;
    ..
    ..
      bs := TBlockSearch.Create(SourceFile);
      try
        bs.SearchFor(WhatIAmSearchingFor);
        for i := 0 to bs.results.count-1 do
        begin
          // Example: we need to do something at an offset after the result
          SourceFile.Position := bs.Results.Items[i] + 5;
          SourceFile.ReadBuffer(RecordToParse, SizeOf(RecordToParse)); 
          // And now I work on the data in the second packed record of arrays and output to memo or whatever
        end;
      finally
        bs.free;
      end;
    

## Search and replace in a file

This will search and replace by loading the entire file into the stringlist in memory and replacing there, then saving the stringlist again. While fine for small files, performance will likely suffer for large files. 
    
    
    procedure FindAndReplace(SearchFor, ReplaceWith, FileName: string);
    var
      FileContents: TStringList;
    begin
      FileContents:=TStringList.Create;
      try
        FileContents.LoadFromFile(FileName);
        FileContents.Text:=StringReplace(FileContents.Text, SearchFor, ReplaceWith, [rfReplaceAll,rfIgnoreCase]);
        FileContents.SaveToFile(FileName);
      finally
        FileContents.Free;
      end;
    end;

---

_Source: [https://wiki.freepascal.org/Rosetta_Stone](https://web.archive.org/web/20250401005734/https://wiki.freepascal.org/Rosetta_Stone)_
