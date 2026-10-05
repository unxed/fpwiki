# FindAllFiles

│ **English (en)** │  **[español (es)](</FindAllFiles/es> "FindAllFiles/es")** │  **[suomi (fi)](</FindAllFiles/fi> "FindAllFiles/fi")** │  **[français (fr)](</FindAllFiles/fr> "FindAllFiles/fr")** │  **[polski (pl)](</FindAllFiles/pl> "FindAllFiles/pl")** │  **[русский (ru)](<../ru/FindAllFiles.md> "FindAllFiles/ru")** │    
****

[Unit](<Unit.md> "Unit"): Lazarus [fileutil](<fileutil.md> "fileutil"). 

To enable FileUtil in your project, please add LazUtils into required packages. Follow these steps: 

  * Go to _Lazarus IDE Menu_ > _Project_ > _Project Inspector_
  * In the _Project Inspector_ dialog window, click _Add_ > _New Requirement_
  * In the _New Requirement_ dialog window, find _LazUtils_ package then click OK.



See also: 

  * <https://lazarus-ccr.sourceforge.io/docs/lazutils/fileutil/findallfiles.html>
  * <https://lazarus-ccr.sourceforge.io/docs/lazutils/fileutil/tfilesearcher.html>


    
    
    procedure FindAllFiles(AList: TStrings; const SearchPath: String;
      SearchMask: String = ''; SearchSubDirs: Boolean = True; DirAttr: Word = faDirectory); 
    
    function FindAllFiles(const SearchPath: String; SearchMask: String = '';
      SearchSubDirs: Boolean = True): TStringList;
    

**FindAllFiles** looks for files matching the SearchMask in the SearchPath directory and, if specified, its subdirectories, and populates a [stringlist](<TStrings.md> "TStrings") with the resulting filenames. 

The mask can be a single mask like you can use with the FindFirst/FindNext functions, or it can consist of a list of masks, separated by a [semicolon (;)](<Semicolon.md> "Semicolon").  
Spaces in the mask are treated as literals. 

Parameter DirAttr is int file attribute: if file-system item has this attribute(s), it is considered as a directory. It can be faDirectory, faSymLink, (faDirectory+faSymLink) or maybe another bits can be used. 

There are two overloaded versions of this routine. The first one is a **[procedure](<Procedure.md> "Procedure")** and assumes that the receiving stringlist already has been created. The second one is a **[function](<Function.md> "Function")** which creates the stringlist internally and returns it as a function result. In both cases the stringlist must be destroyed by the calling procedure. 

## Example
    
    
    uses 
      ..., FileUtil, ...
    var
      PascalFiles: TStringList;
    begin
      PascalFiles := TStringList.Create;
      try
        FindAllFiles(PascalFiles, LazarusDirectory, '*.pas;*.pp;*.p;*.inc', true); //find e.g. all pascal sourcefiles
        ShowMessage(Format('Found %d Pascal source files', [PascalFiles.Count]));
      finally
        PascalFiles.Free;
      end;
    
    // or
    
    begin
      //No need to create the stringlist; the function does that for you
      PascalFiles := FindAllFiles(LazarusDirectory, '*.pas;*.pp;*.p;*.inc', true); //find e.g. all pascal sourcefiles
      try
        ShowMessage(Format('Found %d Pascal source files', [PascalFiles.Count]));
      finally
        PascalFiles.Free;
      end;
    

## Watch out for memory leaks, esp. with the `FindAllFiles` function

The _function_ `FindAllFiles` creates the `TStringList`. This is convenient, but beware of memory leaks. 
    
    
    // DON'T DO THIS - the line below means that TStringList created by `FindAllFiles` is never freed.
    Listbox1.Items.Assign(FindAllFiles(LazarusDirectory, '*.pas;*.pp;*.p;*.inc', true));
    

Fix it to this: 
    
    
    List := FindAllFiles(LazarusDirectory, '*.pas;*.pp;*.p;*.inc', true);
    try
      Listbox1.Items.Assign(List);
    finally 
      List.Free;
    end;
    

## This unit is part of LCLBase

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you want to use this function in command line programs, add a project requirement for _LCLBase_ , which will not pull in the entire LCL

---

_Source: [https://wiki.freepascal.org/findallfiles](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/findallfiles)_
