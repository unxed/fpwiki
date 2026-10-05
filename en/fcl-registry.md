# fcl-registry

│ **English (en)** │  **[español (es)](</fcl-registry/es> "fcl-registry/es")** │  **[русский (ru)](<../ru/fcl-registry.md> "fcl-registry/ru")** │    
****

fcl-registry is an [FCL](<FCL.md> "FCL") unit that provides access to the Windows registry. **It is cross-platform**. How? In non-Windows operating systems it creates a `reg.xml` file. For example, under non-Windows operating systems it creates the per user XML file (HKEY_CURRENT_USER) `reg.xml` in the `/home/[user]/.config/application_name/` directory; the global XML file (HKEY_LOCAL_MACHINE) `reg.xml` is created in the `/etc/application_name/` directory which is invariably a read-only location unless you are the root or Admin user. 

## Contents

  * 1 Registry terms
  * 2 Example
  * 3 Accessing 64 bit and 32 bit registry views
  * 4 Administrative privileges
  * 5 See also



## Registry terms

RootKey: registry hive where you want to start accessing the registry. Examples: HKEY_LOCAL_MACHINE, HKEY_CURRENT_USER 

Key: the path to the "directory" that contains individual data. This is a bit counter-intuitive but a holdover from compatibility with earlier versions of the registry. 

Name/value: the actual name/value pairs in the Key "directory". Each key can have a default value, whose name is _(an empty string)_. 

## Example

Example that tries to get a value: 
    
    
    uses ... registry...
    
    var
      CompileCommand: string='';
      Registry: TRegistry;
    begin
      Registry := TRegistry.Create;
      try
        // Navigate to proper "directory":
        Registry.RootKey := HKEY_LOCAL_MACHINE;
        if Registry.OpenKeyReadOnly('\SOFTWARE\Classes\InnoSetupScriptFile\shell\Compile\Command') then
          CompileCommand:=Registry.ReadString(''); //read the value of the default name
      finally
        Registry.Free;  // In non-Windows operating systems this flushes the reg.xml file to disk
      end;
    end;
    

## Accessing 64 bit and 32 bit registry views

If you have 64 bit Windows, the registry is split up into a 64 bit and 32 bit (compatibility) part. By default, if you run a 32 bit process, you see the 32 bit part; if you run a 64 bit application, you see the 64 bit part. 

You can also access the 32 bit part from 64 bit applications and vice versa. From [MSDN](<http://msdn.microsoft.com/en-us/library/windows/desktop/aa384129%28v=vs.85%29.aspx>)

  * KEY_WOW64_64KEY: Access a 64-bit key from either a 32-bit or 64-bit application.
  * KEY_WOW64_32KEY: Access a 32-bit key from either a 32-bit or 64-bit application.



These keys are defined in the registry unit so you can just use them: eg in the registry object's Access property, like this: 
    
    
    Registry := TRegistry.Create;
    Try
      Registry.Access:=Registry.Access or KEY_WOW64_64KEY;
    

or in the registry.create call, eg: 
    
    
    TRegistry.Create(KEY_READ or KEY_WOW64_64KEY);
    

## Administrative privileges

Depending on what you want to read/write in the Windows registry, you may need administrator rights and elevation (Windows Vista+). Please see [Use manifest file to set execution level](<IDE_Window__Project_Options.md> "IDE Window: Project Options")

## See also

[Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/fcl-registry](https://web.archive.org/web/20250111210641/https://wiki.freepascal.org/fcl-registry)_
