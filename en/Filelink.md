# Filelink

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[Deutsch (de)](</Filelink/de> "Filelink/de")** │  **English (en)** │    
****

This tutorial is about Windows programming. This tutorial shows how to create a file link (shortcut) under Windows. 

The following units are required: 
    
    
      uses
       Registry, ActiveX, ComObj, ShlObj, SysUtils;
    

An enumeration is used to tell the function where the link should be saved. 

Depending on where the function is called, this list must be declared either in the [interface](<Interface.md> "Interface") or in the [implementation](<Implementation.md> "Implementation") section of the [unit](<Unit.md> "Unit"). 

Definition of the list: 
    
    
      Type
       ShortcutType = (_DESKTOP, _QUICKLAUNCH, _SENDTO, _STARTMENU, _OTHERFOLDER);
    

The function for creating a link (file links) looks like this: 
    
    
    // This function works with the data of the current user
    function funErstelleLink(                    // Function name
      const strSourceFile : string;              // Path and name of the original file
      const enuLocation : ShortcutType;          // Where the shortcut should be saved
      const subdirectory : string = '';          // Optional: Alternative path to
                                                 // save the link
      const strWorking directory : string = '';  // Optional: path or directory in the
                                                 // the program should work
      const strParameter : string = '';          // Optional: transfer parameters to the program
      const strDescription : string = '';        // Optional: Description of the shortcut
      const strIconFile : string = '';           // Optional: file that contains the icon
      const intIconNummer : integer = 0          // Optional: The number of the icon in 
                                                 // the icon file to use for the shortcut icon
                                                 // (the number of icons starts at 1)
       ) : string;                                                                           
    
      const
        SHELL_FOLDERS_ROOT = 'Software\MicroSoft\Windows\CurrentVersion\Explorer';
        QUICK_LAUNCH_ROOT = 'Software\MicroSoft\Windows\CurrentVersion\GrpConv';
    
      var
        MyObject : IUnknown;
        MySLink : IShellLink;
        MyPFile : IPersistFile;
        Reg : TRegIniFile;
        strDirectory : string = '';
        strLinkName : string = '';
        wstrfilename : WideString = '';
    
      begin
    
        Result := '';
    
       // Creates the objects
       MyObject := CreateComObject(CLSID_ShellLink);
       MySLink := MyObject as IShellLink;
       MyPFile := MyObject as IPersistFile;
    
       // Creates the name of the link
       strLinkName := ChangeFileExt(strSourceFile , '.lnk');
       strLinkName := ExtractFileName(strLinkName);
    
       // Reads the location from the registry
       if enuLocation = _QUICKLAUNCH then
       begin
         Reg := TRegIniFile.Create(QUICK_LAUNCH_ROOT);
         try
           strDirectory := Reg.ReadString('MapGroups', 'Quick Launch', '');
         finally
           Reg.Free;
         end;
       end
       else
       begin
         Reg := TRegIniFile.Create(SHELL_FOLDERS_ROOT);
         try
           case enuLocation of
             _OTHERFOLDER : strDirectory := strDirectory;
             _DESKTOP : strDirectory := Reg.ReadString('Shell Folders', 'Desktop', '');
             _STARTMENU : strDirectory := Reg.ReadString('Shell Folders', 'Start Menu', '');
             _SENDTO : strDirectory := Reg.ReadString('Shell Folders', 'SendTo', '');
           end ;
         finally
           Reg.Free;
         end;
       end;
    
       // If something went wrong, the program ends here
       // a link is not created
       if strDirectory = '' then
         exit;
    
       // Passes the appropriate parameters to the objects
       MySLink.SetPath(PChar(strSourceFile)) ;
       MySLink.SetArguments(PChar(strParameter));
       MySLink.SetDescription(PChar(strDescription));
    
       // If the information about the icon is plausible, it will be adopted
       // otherwise the default settings are used
       if (strIconFile <> '') and (intIconNumber > 0) then
         MySLink.SetIconLocation(PChar(strIconFile), intIconNumber);
    
       // Defines in which directory the link is saved
       if (strSubdirectory <> '') and (enuLocation <> _OTHERFOLDER) then
         wstrFilename := strDirectory + '\' + strSubDirectory + '\' + strLinkName
       else
         wstrFilename := strDirectory + '\' + strLinkName;
    
       // Specifies the directory in which the program is executed
       if strWorkingDirectory = '' then
         MySLink.SetWorkingDirectory(PChar(ExtractFilePath(strSourceFile)))
       else
         MySLink.SetWorkingDirectory(PChar(strWorkingDirectory));
    
       // Creates the link
       MyPFile.Save(PWideChar(wstrFilename), False);
    
       // Returns the path of the link as a return value
       Result := string(wstrfilename);
    
     end ;
    

The function can be called as follows. 
    
    
      funErstelleLink (
         'E:\Test\Test.Exe',  // Path and name of the original file
         _DESKTOP,            // The link should be saved here
         '',                  // Optional: An alternative path to save the link
         '',                  // Optional: The working directory of the program
         '',                  // Optional: transfer parameters to the program
         'This is a test',    // Optional: A description of the shortcut
         'E:\Test\Test.Exe',  // Optional: A file that contains the alternative icon
         2                    // Optional: The icon that should be displayed from the icon file
                              // (the counting of the icons starts at 1)
         ) ;

---

_Source: [https://wiki.freepascal.org/Filelink](https://web.archive.org/web/20230130100103/https://wiki.freepascal.org/Filelink)_
