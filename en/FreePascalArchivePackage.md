# FreePascalArchivePackage

## Contents

  * 1 About
  * 2 Download
  * 3 Dependencies
  * 4 Usage
  * 5 See also



## About

This is a conversion of TurboPower Software's Abbrevia library for the Free Pascal Compiler. Abbrevia is a compression toolkit that supports PKZIP 4, Microsoft CAB, TAR, & gzip formats. The package contains visual components that simplify the manipulation of ZIP files. 

Status: Alpha, ie it needs testing. 

This package was created and generously made available to the open source community by [TurboPower Software](<http://sourceforge.net/projects/tpabbrevia/>). 

License: [MPL 1.1](<http://www.mozilla.org/MPL/MPL-1.1.html>). 

## Download

The latest release of FpArchive can be found on the [Lazarus CCR Files page](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=98986&release_id=478029>). 

Outdated: Getting the latest source from CVS. Only svn ("Subversion") is being used by now. CVS is no longer being used. 
    
    
    cvs -d:pserver:anonymous@cvs.sourceforge.net:/cvsroot/lazarus-ccr login
    

(press the Enter key when prompted for a password) 
    
    
    cvs -z3 -d:pserver:anonymous@cvs.sourceforge.net:/cvsroot/lazarus-ccr co fparchive
    

This package is also available from the Free Pascal Compiler CVS in projects/contrib/abbrevia 

## Dependencies

Small amounts of 386 assembler. 

## Usage

1\. For your project open "Project->Compiler Options->Other unit files" and add the path to the FPArchive units 

2\. In the "uses" clause of your unit add AbBase, AbBrowse, AbZBrows, AbUnzper, AbArcTyp 

3\. Create an unzip object: 
    
    
    UnZip := TAbUnZipper.Create(nil);
    

4\. Set the directory where you want to extract the files to: 
    
    
    UnZip.BaseDirectory := '/path/to/extract';
    

5\. To restore directories structure (optional) add this line: 
    
    
    UnZip.ExtractOptions := [eoCreateDirs, eoRestorePath];
    

6\. Try to extract all CSV files (for example) from an archive file: 
    
    
    try
      UnZip.FileName := '/file/to/extract.zip';
      UnZip.ExtractFiles('*.csv');
    except
      on E: Exception do
      begin
        //some error handling
      end;
    end;
    

7\. Free the created object: 
    
    
    UnZip.Free;
    

## See also

  * [paszlib](<paszlib.md> "paszlib") is built into FPC/Lazarus. Supports zipping/unzipping .zip files.

---

_Source: [https://wiki.freepascal.org/FreePascalArchivePackage](https://web.archive.org/web/20250324165907/https://wiki.freepascal.org/FreePascalArchivePackage)_
