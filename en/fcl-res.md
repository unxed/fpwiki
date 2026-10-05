# fcl-res

## Contents

  * 1 Overview
  * 2 Documentation
  * 3 Examples
  * 4 Getting file version information
  * 5 See also



## Overview

_fcl-res_ is a Free Component Library component that provides functionality to create and access all kinds of resources, including those on: 

  * Windows PE
  * (Linux) ELF
  * macOS Mach-O
  * .res files



## Documentation

See the XML directory under fcl-res for fpdoc documentation, which includes a nice introduction and tutorial, complete with code examples. 

You can generate e.g. a CHM file from this using this command in Windows (please adjust file locations): 
    
    
    C:\Development\fpctrunk\bin\i386-win32\fpdoc.exe ^
    --package=fcl-res ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\acceleratorsresource.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\bitmapresource.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\coffconsts.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\coffreader.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\cofftypes.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\coffwriter.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\dfmreader.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\elfconsts.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\elfreader.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\elftypes.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\elfwriter.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\externalreader.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\externaltypes.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\externalwriter.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\fpcrestypes.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\groupcursorresource.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\groupiconresource.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\groupresource.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\icocurtypes.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\machoconsts.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\machoreader.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\machotypes.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\machowriter.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\resdatastream.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\resfactory.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\resmerger.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\resource.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\resourcetree.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\resreader.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\reswriter.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\stringtableresource.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\strtable.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\tlbreader.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\versionconsts.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\versionresource.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\versiontypes.pp ^
    --input=C:\Development\fpctrunk\packages\fcl-res\src\winpeimagereader.pp ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\acceleratorsresource.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\bitmapresource.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\coffreader.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\cofftypes.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\coffwriter.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\dfmreader.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\elfconsts.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\elfreader.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\elfwriter.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\externalreader.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\externaltypes.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\externalwriter.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\groupcursorresource.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\groupiconresource.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\groupresource.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\machoreader.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\machotypes.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\machowriter.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\resdatastream.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\resfactory.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\resource.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\resourcetree.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\resreader.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\reswriter.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\stringtableresource.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\versionconsts.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\versionresource.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\versiontypes.xml ^
    --descr=C:\Development\fpctrunk\packages\fcl-res\xml\winpeimagereader.xml ^
    --format=chm ^
    --auto-index ^
    --auto-toc ^
    --make-searchable ^
    --output=fcl-res.chm
    

## Examples

The resource unit is used in fpcres, a resource management tool, in $(fpcdir)\utils\fpcres\sourcehandler.pas 

See also the examples in the help file, e.g. 
    
    
    program converttosparc;
    // slightly modified from fcl-res help file program res3 (mainly comments).
    // This program reads a supported resource file (and autodetects the resource type)
    // then writes to a specific format - in this case SPARC ELF.
    {$mode objfpc}
    
    uses
      Classes, SysUtils, resource,
    // You need to specify which resources you want to be able to read.
    // The units initialize themselves and register themselves as readers....
      resreader, coffreader, elfreader, winpeimagereader,
    //... same goes for writers
      elfwriter, 
    // elfwriter needs its constants unti:
      elfconsts;
    
    var
      resources : TResources;
      writer : TElfResourceWriter;
      reader : TAbstractResourceReader;
      inFile : TFileStream;
    
    begin
      resources:=TResources.Create;
      inFile:=TFileStream.Create(paramstr(1), fmOpenRead or fmShareDenyNone);
      // Use FindReader to let TResources find the appropriate reader for the file
      reader:=TResources.FindReader(inFile);
      writeln('Selected reader: ',reader.Description);
      resources.LoadFromStream(inFile,reader);
      writer:=TElfResourceWriter.Create;
      writer.MachineType:=emtsparc;
      resources.WriteToFile(ChangeFileExt(paramstr(1),'.o'),writer);
    
      // clean up - adding try..except blocks would be nice:
      resources.Free;
      reader.Free;
      writer.Free;
      inFile.Free;
    end.
    

## Getting file version information

See: [Show Application Title, Version, and Company](<Show_Application_Title,_Version,_and_Company.md> "Show Application Title, Version, and Company")

## See also

  * [Description of using FPC and Lazarus resources](<Lazarus_Resources.md> "Lazarus Resources")
  * [Lazarus Resource compiler](<lazres.md> "lazres")
  * [IDE Project Options > Resources](<IDE_Window__Project_Options.md> "IDE Window: Project Options")
  * [Package List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/fcl-res](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/fcl-res)_
