# Stringlist

│ **English (en)** │

## Contents

  * 1 String list
  * 2 Create a file
  * 3 Append a record to an existing file
  * 4 Find a record
    * 4.1 Method 1
    * 4.2 Method 2
  * 5 Sort a file alphabetically
  * 6 See also



## String list

With a string list, a file is read completely into working memory. Stringlist is therefore suitable for files of small and medium size. Stringlist is a way to process files quickly and easily. Stringlist is ideal for processing files that are smaller than 1 megabyte. To process the file, the entire file is loaded into the main memory. Each line of the data record is read into its own array element. 

## Create a file
    
    
    procedure SubStringlistFileCreate ();
    var
       // TStringlist is part of the Classes unit
       strList : TStringList;
    begin
       // Creates the string list
       strList := TStringList.Create;
    
       // Adds a record including a line break to the string list
       strList.Add('xyz');
    
       // Adds a record including a line break to the string list
       strList.Add('abcd');
    
       // Writes the string list to a file
       strList.SaveToFile('example.txt');
    
       // Releases the string list memory
       strList.Free;
    end;
    

## Append a record to an existing file
    
    
    procedureSubStringlistApplyFile ();
    var
       // TStringlist is part of the Classes unit
       strList : TStringList;
    begin
       // Creates the string list
       strList := TStringList.Create;
    
       // Reads the whole file into the string list
       strList.LoadFromFile('example.txt');
    
       // Adds a record including a line break to the string list
       strList.Add('text');
    
       // Writes the string list to a file
       strList.SaveToFile('example.txt');
    
       // Releases the string list memory
       strList.Free;
    end;
    

## Find a record

### Method 1
    
    
    procedure subStringlistFileSearch ();
    var
       // TStringlist is part of the Classes unit
       strList : TStringList;
       intI : Integer;
    begin
       // Creates the string list
       strList := TStringList.Create;
    
       // Reads the whole file into the string list
       strList.LoadFromFile('example.txt');
    
       // Use a counting loop to find the corresponding record
       for intI := 0 to strList.Count -1 do
         begin
           // Search and change the record
           if strList [intI] = 'abcd' then 
             strList [intI] := 'dcba';
         end;
    
       // Writes the string list to a file
       strList.SaveToFile('example.txt');
    
       // Releases the string list memory
       strList.Free;
    end;
    

### Method 2
    
    
    procedure subStringlistInFileSearch2 ();
    var
       // TStringlist is part of the Classes unit
       strList : TStringList;
       intI : Integer;
    begin
       // Creates the string list
       strList := TStringList.Create;
    
       // Reads the whole file into the string list
       strList.LoadFromFile('example.txt');
    
       // Searches for "Text", if not found, the search returns -1
       intI := strList.IndexOf ('Text');
    
       // If found, changes the record
       if intI <> -1 then
         strlist[intI] := 'tXET';
    
       // Writes the string list to a file
       strList.SaveToFile('example.txt');
    
       // Releases the string list memory
       strList.Free;
     end;
    

## Sort a file alphabetically
    
    
    procedure SubStringlistFileSort ();
    var
      // TStringlist is part of the Classes unit
      strList : TStringList;
      intI : Integer;
     begin
       // Creates the string list
       strList := TStringList.Create;
    
       // Reads the whole file into the string list
       strList.LoadFromFile('example.txt');
    
       // Sort the string list alphabetically
       strList.Sort;
    
       // Writes the string list to a file
       strList.SaveToFile('example.txt');
    
       // Releases the string list memory 
       strList.Free;
     end;
    

## See also

  * [File types](<File_types.md> "File types")

---

_Source: [https://wiki.freepascal.org/Stringlist](https://web.archive.org/web/20250418025834/https://wiki.freepascal.org/Stringlist)_
