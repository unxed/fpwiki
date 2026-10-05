# FileAssociation

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 TFileAssociation
  * 2 Download
  * 3 Usage
  * 4 How to open the associated file
  * 5 Another way of registering file associations
  * 6 See also



## TFileAssociation

Author: Lainz 

Licence: Modified LGPL 

Version: 1.0 

Description: This unit registers file association for Windows. 

## Download

GitHub: <https://github.com/lainz/FileAssociation>

## Usage

First install the package. You can then drop the component TFileAssociation (FileAssoc unit) that gets installed in the [System tab](<System_tab.md> "System tab") of the IDE on your [form](<TForm.md> "TForm"). 

All parameters are mandatory. Especially ActionName which must be 'Open' to work with double click. This is useful as well for default commands like 'Edit' and 'Print'. This must be in English for 'Edit', 'Open' and 'Print' so it can access the right registry entry. You can customize the translation with ActionText. 
    
    
    ...
    uses
      ...
      FileAssoc; //<-- add fileassociation unit here
    
    type
    
    ...
        { private declarations }
        assoc: TFileAssociation;
    ...
    
    procedure TfrmMain.FormCreate(Sender: TObject);
    begin
      assoc := TFileAssociation.Create(Self);//<-- create like a regular component
    end;
    
    procedure TfrmMain.Button1Click(Sender: TObject);
    begin
      assoc.ApplicationName := 'Lazarus IDE';
      assoc.ApplicationDescription := 'RAD for Free Pascal';
    
      // you can change Extension and Action part for each extension you have
    
      assoc.Extension := '.lpr';
      assoc.ExtensionName := 'Lazarus Project';
      assoc.ExtensionIcon := '"C:\lazarus\images\lprfile.ico"';
    
      // full path required, you can use ParamStr(0) to get the path with the .exe name included. The path must be inside quotes if it has whitespace.
      assoc.Action := '"C:\lazarus\lazarus.exe" "%1"'; 
      assoc.ActionName := 'Open';
      assoc.ActionIcon := '"C:\lazarus\images\mainicon.ico"';
    
      // notice that using RegisterForAllUsers as True requires Administrator Privileges
      // if you want to run without privileges set it to false, but it will register for current user only
      assoc.RegisterForAllUsers:=False;
      if assoc.Execute then
      begin
        ShowMessage('OK');
        assoc.ClearIconCache; //<<-- rebuild icons
      end;
    end;
    
    end.
    

## How to open the associated file
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      s: String;
    begin
    // if there are parameters
      if ParamCount > 0 then
      begin
    // load the first parameter
        s := ParamStr(1);
     
    // if is a .txt file
        if ExtractFileExt(s) = '.txt' then
        begin
    // load the .txt file into a memo
          Memo1.Lines.LoadFromFile(s);
        end;
      end;
    end;
    

## Another way of registering file associations

Without using the component is as well possible to register a file association. This however doesn't cover adding the application to Default Programs, but is shorter if you only want to register a file type that's owned by your application. 
    
    
    {$IFDEF WINDOWS}
    uses Registry, ShlObj;
     
    procedure TMainForm.RegisterFileType(ExtName: string; AppName: string);
    var
      reg: TRegistry;
    begin
      reg := TRegistry.Create;
      try
        reg.RootKey := HKEY_CLASSES_ROOT;
        reg.OpenKey('.' + ExtName, True);
        reg.WriteString('', ExtName + 'file');
        reg.CloseKey;
        reg.CreateKey(ExtName + 'file');
        reg.OpenKey(ExtName + 'file\DefaultIcon', True);
        reg.WriteString('', AppName + ',0');
        reg.CloseKey;
        reg.OpenKey(ExtName + 'file\shell\open\command', True);
        reg.WriteString('', AppName + ' "%1"');
        reg.CloseKey;
      finally
        reg.Free;
      end;
      SHChangeNotify(SHCNE_ASSOCCHANGED, SHCNF_IDLIST, nil, nil);
    end;
    {$ENDIF}
    
    
    
    var
       reg: TRegistry;
           
    begin
      {$IFDEF WINDOWS}
      reg := TRegistry.Create;
      try
        reg.RootKey := HKEY_CLASSES_ROOT;
        if not reg.KeyExists('ext' + 'file\shell\open\command') then
          RegisterFileType('ext', ExtractFilePath(Application.ExeName) + 'my_app.exe');
      finally
        reg.Free;
      end;
     
     if ParamCount > 0 then
      begin
        s := ParamStr(1);
        if ExtractFileExt(s) = '.ext' then
          LoadFile(s);
      end;
      {$ENDIF}
    

## See also

If this component for some reason does not work for you, or you need to run it as administrator for all users without elevating your application privileges, an [Inno Setup Script](<Inno_Setup_Usage.md> "Inno Setup Usage") may fit your needs best.

---

_Source: [https://wiki.freepascal.org/FileAssociation](https://web.archive.org/web/20250421233149/https://wiki.freepascal.org/FileAssociation)_
