# Disk in Drive

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

The function checks whether there is a disk medium in the CD or DVD drive. 
    
    
     uses
       SysUtils, ...;
    
     // enumeration for the return values
     grade
       byte = (enmNoDriveLetter, enmMoMedInserted, enmMedInserted, enmError);
    
      ...
    
     function funDiskInDrive (chrDrive : char) : byte;
     begin
    
       Result := enmNoDriveLetter;
       chrDrive := UpCase(chrDrive);
    
       // Checks whether the drive letter is valid.
       if not (chrDrive in ['A' .. 'Z']) then
         exit;
    
       // Checks if the drive contains media.
       try
         if DiskSize ( Ord ( chr drive ) - $ 40 ) <> - 1 then
           Result := enmMedInserted
         else
           Result := enmNoMedInserted;
       except
         Result := enmError;
       end;
    
     end;
    
       ...
    

Example of calling the function: 
    
    
      ...
    
       case funDiskInDrive('F') of
         enmNoDriveLetter : ... ;
         enmNoMedInserted : ... ;
         enmMedInserted : ... ;
         Error : ... ;
       end; 
     
       ...

---

_Source: [https://wiki.freepascal.org/Disk_in_Drive](https://web.archive.org/web/20250421231216/https://wiki.freepascal.org/Disk_in_Drive)_
