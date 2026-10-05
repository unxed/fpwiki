# Distributing Lazarus - Installers

This page is for expert users who know how to use/install Lazarus and want to write an installer for a platform. 

If you have trouble installing Lazarus see [Installation](<Lazarus_Documentation.md> "Lazarus Documentation"). 

## Contents

  * 1 Primary and secondary config files
  * 2 Config files
    * 2.1 environmentoptions.xml
    * 2.2 Changing/Creating a config file
  * 3 Include third party packages



# Primary and secondary config files

For a multi user environment you can install default config files in the **secondary config path**. When a user starts the IDE, Lazarus will automatically copy these files to the primary config path. It does not copy all files; it copies each file when needed (i.e. the primary file does not exist). 

# Config files

All config files are created on the fly. They only contain values that differ from defaults. Note: If they contain more, then it means there is no default or you found a bug. 

The easiest way to get the config files is to start an IDE, setup and install packages and then use the created configs. You should remove everything personal. 

## environmentoptions.xml

Here is an example for the environmentoptions.xml: 
    
    
    <?xml version="1.0"?>
    <CONFIG>
      <EnvironmentOptions>
        <Version Value="106"/>
        <LazarusDirectory Value="/usr/share/lazarus"/>
        <CompilerFilename Value="/usr/bin/fpc"/>
        <FPCSourceDirectory Value="/usr/share/fpcsrc/$(FPCVer)/fpc/"/>
        <MakeFilename Value="make"/>
        <TestBuildDirectory Value="/tmp"/>
        <DebuggerFilename Value="/usr/bin/gdb"/>
      </EnvironmentOptions>
    </CONFIG>
    

**Notes:**

  * **LazarusDirectory** can not contain any macro.
  * **CompilerFilename** can use macros, sets the macro **CompPath**. You can not use the **FPCVer** macro, because the IDE fetches it from the compiler using the CompilerFilename.
  * The other values can use macros.
  * See [Macros](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames"), especially the **env** macro.



## Changing/Creating a config file
    
    
    uses laz2_xmlcfg;
    var
      cfg: TXMLConfig;
    begin
      cfg:=TXMLConfig.Create('/home/username/.lazarus/environmentoptions.xml');
      try
        cfg.SetDeleteValue('EnvironmentOptions/LazarusDirectory/Value','/home/username/new/path/lazarus','');
        cfg.Flush;
      finally
        cfg.Free;
      end;
    end;
    

The IDE saves all file names as relative file names(except for history lists and some minor items). So stop the IDE, change the value in the xml and start the IDE and that's it. The IDE will automatically load the new lpk files, the new fpdoc files, etc. 

# Include third party packages

If you want to use third party packages, you can put them somewhere and create a **lpl** file in **packager/globallinks**. You can put the package into the **components** directory or at a fixed location on the users machine.

---

_Source: [https://wiki.freepascal.org/Distributing_Lazarus_-_Installers](https://web.archive.org/web/20250219125525/https://wiki.freepascal.org/Distributing_Lazarus_-_Installers)_
