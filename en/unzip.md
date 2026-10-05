# unzip

## Unzip and zip

The unzip and zip package contains several routines to unzip or zip files on every Lazarus supported platforms. There are 4 units: 

  * **zip** a unit which implements an zip mechanism with native Pascal links.
  * **unzip** a unit which implements an unzip mechanism with native Pascal links.
  * **ziputils** contains some tools for the unzip unit.
  * **zipbase** a unit which defines the interface for the info-zip unzip library.



The zip library unit is included in Lazarus 2. 

### Example

Zip compression is in widespread use. It is also used to compress EPUB files. 
    
    
    uses
      unzip,ziputils;
    const  cover  =  'cover.jpeg';
    var UnZipper          : unzFile;
        epubfile, UnPackPathFile : String;
        zfinfos  :  unz_file_info_ptr;
        i        :  Integer;
        li_SizeRead,li_SizeWrite,li_TotalW  : Longint;
        lb_FoundFile : Boolean;
        FBuffer  :  TExtFileBuffer;
        li_HandleDest : Integer;    
    begin
       epubfile :='/Your/Path/to/epub';
       UnPackPathFile :='/Your/Path/to/uncompressed/file';
       try
         UnZipper:=unzOpen(@epubfile[1]);
         if unzLocateFile(UnZipper,@cover[1],2)= UNZ_OK then
           try
             unzOpenCurrentFile(UnZipper);
             Getmem(zfinfos,SizeOf(zfinfos));
             unzGetCurrentFileInfo(UnZipper,zfinfos,@cover[1],SizeOf(cover),nil,0,nil,0);
             lb_FoundFile := False;
             if not FileExistsUTF8(UnPackPathFile)
               Then  FileCreateUTF8(UnPackPathFile);
             li_HandleDest := FileOpenUTF8(UnPackPathFile, fmopenwrite );
             while not lb_FoundFile do
               begin
                 li_SizeRead := unzReadCurrentFile(UnZipper,@FBuffer[0],high ( Fbuffer ) + 1);
                 if li_SizeRead < high ( Fbuffer ) + 1 then lb_FoundFile := True;
                 li_SizeWrite := Filewrite(li_HandleDest,Fbuffer,li_SizeRead);
                 inc( li_TotalW, li_SizeWrite );
               end;
             Break;
           finally
             Freemem(zfinfos,SizeOf(zfinfos));
         end;
       finally
         try
           FileClose(li_HandleDest);
         finally
           unzCloseCurrentFile(UnZipper);
           unzClose(UnZipper);
         end;
       end;   
    end.
    

## See also

  * [zip library link](<zip.md> "zip")
  * [paszlib: Object oriented zip file handling which does not require external libraries](<paszlib.md> "paszlib")



Go back to [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/unzip](https://web.archive.org/web/20231202113418/https://wiki.freepascal.org/unzip)_
