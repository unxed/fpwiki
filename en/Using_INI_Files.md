# Using INI Files

│ **English (en)** │  **[русский (ru)](<../ru/Using_INI_Files.md>)** │

## Contents

  * 1 INI Files
    * 1.1 Basic Information
    * 1.2 INI Files
    * 1.3 A Simple Example
    * 1.4 Ini file reading example
    * 1.5 Handling of string representations of boolean values
    * 1.6 Objects to know
      * 1.6.1 Reference Documentation
  * 2 See also



# INI Files

## Basic Information

INI files are text files that store key/value pairs that are grouped in sections. 

INI files can be used to save user settings easily. With the [IniFiles unit](<http://www.freepascal.org/docs-html/fcl/inifiles/index.html>) and the [TINIFile class](<http://www.freepascal.org/docs-html/fcl/inifiles/tinifile.html>) you can easily work with them. The IniFiles unit is part of the FCL. 

Currently only INI files compliant with the more restrictive Windows ini-file format are supported. I.e. using [sections](<http://en.wikipedia.org/wiki/INI_file#Sections>) is mandatory and only the semicolon can be used for comments. For more general information about ini-files read [this](<http://en.wikipedia.org/wiki/INI_file>). 

## INI Files

INI Files use brackets to create and mark **Sections** , which contain **keys** and key **values**. A Key and its corresponding Value are separated with an equals sign (Key=Value). 

Section names are put inside square brackets ([Section]). 

Comments are permitted and are marked with a semicolon (;) at the beginning of a line. 

An example ini file: 
    
    
    ; Comment. Beginning of INI file
    
    ; empty lines are ignored
    
    [General]
    ; This starts a General section
    Compiler=FreePascal
    ; Key Compiler and value FreePascal
    

  


## A Simple Example

In practise, you need to be able to create an ini file at run time if it does not exist and set default values for entries. Here is an example, intended to be partially pasted into a new Lazarus Application. We define and declare a record that holds some of our settings, we have a checkbox that represents another setting. To keep it simple, the only setting we can change here is the checkbox, its 'on change' event triggers a write of the ini file, updated data each time. We have also added a memo to the form as an easy way to show us what is happening. Add the new method's prototypes into your Form's interface. Add 'inifiles' to the implementation uses section. 
    
    
    interface
    .......
    
    type SettingRec = record
        X : integer;
        MyName : string;
    end; 
    ....
    
    implementation  
    ....
    uses IniFiles;
    const
      IniFile = 'settings.ini';   
    ....
    procedure TForm1.ReadSettings;
    var
        Sett : TIniFile;
    begin
        Sett := TIniFile.Create(IniFile);
        Settings.X:= Sett.ReadInteger('Main', 'X', 1);    // (Section, Key, Default)
        Settings.MyName := Sett.ReadString('Main', 'MyName', 'Davo');
        CheckBox1.Checked := Sett.ReadBool('Main', 'CheckBox', true);
        Sett.Free;
    end;
    
    procedure TForm1.WriteSettings;
    var
        Sett : TIniFile;
    begin
        Sett := TIniFile.Create(IniFile);
        Sett.WriteBool('Main', 'CheckBox', CheckBox1.Checked);
        Sett.WriteInteger('Main', 'X', Settings.X);
        Sett.WriteString('Main', 'MyName', Settings.MyName);
        Sett.Free;
    end;
    

Note we don't check to see if a ini file is present or not, it does not matter. If a particular key is not present (or the whole file is not present) the default value is used when reading, so use a default that works initially. All our data is being written into and read from the ini file's 'Main' section, it makes sense to group related data into separate sections but the actual section name used does not matter, but it must be consistent. 

Next we need a couple of methods there just to 'exercise' the above ones. Obviously, you use the Lazarus Object Inspector to create FormCreate() and CheckBox1Change() but copy the content from here. 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
        ReadSettings;
        ShowSettings;
    end;
    
    procedure TForm1.CheckBox1Change(Sender: TObject);
    begin
        WriteSettings;
        ShowSettings;
    end;
    
    procedure TForm1.ShowSettings;
    begin
        Memo1.Clear;
        Memo1.Append('X=' + Settings.X.tostring);
        Memo1.Append('MyName=' + Settings.MyName);
        Memo1.Append('CheckBox ' + Booltostr(CheckBox1.Checked, true));
    end;
    

An ini file can (and must be able to) have the order its data is presented in changed, don't rely on particular line numbers. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Things can go wrong. For readability the above code has left out the important **try** statements that can prevent crashes or memory leaks if something unexpected happens. Real code needs such protection.

## Ini file reading example

The console application below shows how to read ini files. To test it, create an ini file with the name "DB.ini" and the contents below, and save it in the same folder as the executable program. 
    
    
    [DB-INFO]
    Author=Adam
    Pass=secret
    MaxAttempts=5
    DBFile=C:\Money.dat
    

The code to read this ini file: 
    
    
    Program IniReadExample;
    
    {$mode objfpc}{$H+}
    
    uses
      classes, sysutils, IniFiles;
    
    const
      C_DB_SECTION = 'DB-INFO';
    
    var
      INI: TINIFile;
      Author, Pass, DBFile: String;
      MaxAttempts: integer;
      PassEnter: String;
      TryCount: integer;
    begin
      // Create the object, specifying the the ini file that contains the settings
      INI := TINIFile.Create('DB.ini');
    
      // Put reading the INI file inside a try/finally block to prevent memory leaks
      try
        // Demonstrates reading values from the INI file.
        Author      := INI.ReadString(C_DB_SECTION,'Author','');
        Pass        := INI.ReadString(C_DB_SECTION,'Pass','');
        DBFile      := INI.ReadString(C_DB_SECTION,'DBFile','');
        MaxAttempts := INI.ReadInteger(C_DB_SECTION,'MaxAttempts',1);
    
        // Do something with the values read; e.g. verify the password
        if Pass <> '' then
        begin
          // Ask for the password, max attempts = MaxAttempts
          TryCount := MaxAttempts;
          repeat
            write('Please enter password; ', TryCount, ' attempt(s) left: ');
            readln(PassEnter);
            dec(TryCount);
            if (PassEnter <> Pass) and (TryCount > 0) then
              writeln('Wrong Password, please try again');
          until(PassEnter = Pass) or (TryCount = 0);
    
          // Correct password given?
          if PassEnter = Pass then
            writeln('Correct Password.')
          else
            writeln('Invalid password, but maxiumm number of password attempts reached.');
        end;
    
        writeln('Author               : ', Author);
        writeln('File                 : ', DBFile);
        writeln('Password             : ', Pass);
        writeln('Max password attempts: ', MaxAttempts);
        writeln;
        write('Press Enter to close...');
        Readln;
    
      finally
        // After the ini file was used it must be freed to prevent memory leaks.
        INI.Free;
      end;
    end.
    

## Handling of string representations of boolean values

The TiniFile.ReadBool and TIniFile.WriteBool functions use values of "0" (false) and "1" to represent boolean values in the ini file. You can change this to any other text you want: 
    
    
    [...]
      iniFile.Options := iniFile.Options + [ifoWriteStringBoolean];
      iniFile.BoolTrueStrings := ['true'];
      iniFile.BoolFalseStrings := ['false'];
    [...]
    

ReadBool is not case-sensitive, the texts "true", "True" and "TRUE" are all interpreted as boolean true. 

TIniFile.BoolTrueStrings and TIniFile.BoolFalseStrings are arrays of string, you can specify multiple values: 
    
    
    [...]
      iniFile.Options := iniFile.Options + [ifoWriteStringBoolean];
      iniFile.BoolTrueStrings := ['true','yes','1'];
      iniFile.BoolFalseStrings := ['false','no','0'];
    [...]
    

When reading from the ini file, any of the array values will make ReadBool return the correct boolean value, when writing the ini file, the first value of the corresponding array is used. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The current implementation if TIniFile.ReadBool has an odd behaviour, when the ifoWriteStringBoolean option is set. TIniFile.ReadBool and TIniFile.WriteBool have, when the BoolTrueStrings and BoolFalseStrings arrays are not specified, an asymmetrical behaviour: TIniFile.ReadBool will expect "0" or "1", and TIniFile.WriteBool will write "true" or "false". This will lead to the surprising behavior that the TIniFile.ReadBool function won't be able to read back the "true" values if they were written by the TIniFile.WriteBool function. TIniFile.ReadBool will always return boolean false instead, even if a value is set to "true" in the ini file. If you use the ifoWriteStringBoolean option, and unless this behavior is the desired behaviour, don't forget to specify the TIniFile.BoolTrueStrings and TIniFile.BoolFalseStrings arrays.

## Objects to know

In the TINIFile class there are many different properties, procedures and functions that can be used. 

**CaseSensitive** \- This property allows you to say if the keys and sections are case sensitive or not. By default, they aren't. 

**ReadString** \- Has 3 constant statements. The first one is the section to search in. The second one is the key to look for. The third one is a default string in case the key and/or section searched for is not found. 

**WriteString** \- has 3 constant statements, too. The first is the section. The second is the key and the last is the value that you want to write. If the key and section exist already, the key will be overwritten with the new value. 

**ReadSections** \- returns the names of all sections in the ini file even if the section contains no keys. 

**DeleteKey** \- Remove an existing key from the named section. 

**EraseSection** \- Remove a named section and all its data. 

There are more procedures and functions, but this is enough to get you started. 

### Reference Documentation

Here: [Free Pascal documentation on INI files](<http://lazarus-ccr.sourceforge.net/docs/fcl/inifiles/index.html>)

# See also

  * [HistoryFiles](<HistoryFiles.md> "HistoryFiles")
  * [XML Tutorial](<XML_Tutorial.md> "XML Tutorial")

---

_Source: [https://wiki.freepascal.org/Using_INI_Files](https://web.archive.org/web/20241213021124/https://wiki.freepascal.org/Using_INI_Files)_
