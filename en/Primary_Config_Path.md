# pcp

**PCP** , short for **Primary Config Path** , is a Lazarus command-line option which permits specifying the directory where Lazarus configuration files are located. 

For example, a Linux launcher link (.desktop file) may contain a line like: 
    
    
    Exec=/P/s/lazarus/lazarus **--pcp** ="/P/s/config_lazarus"
    

which tells Lazarus to look for its configuration in the folder: 
    
    
     /P/s/config_lazarus
    

This option is most useful when using [ multiple Lazarus](<Multiple_Lazarus.md> "Multiple Lazarus"): it is needed to keep their configurations properly separated. 

  


## Windows

If option --pcp is not specified, the default folder in Windows Vista and up is: 
    
    
    C:\Users\USERNAME\AppData\Local\lazarus
    

## Linux

For Linux (and other Unix-like systems) the default configuration folder is: 
    
    
    ~\.lazarus
    

## Configuration files

file | decription   
---|---  
lazarus.dci | [Code Templates](<IDE_Window__Code_Templates.md> "IDE Window: Code Templates")  
jcfsettings.cfg | [Jedi Code Format settings](<IDE_Window__JCF_Format_Settings_Options.md> "IDE Window: JCF Format Settings Options")  
codeexploreroptions.xml | [Code Explorer Options](<IDE_Window__Code_Explorer_Options.md> "IDE Window: Code Explorer Options")  
codetoolsoptions.xml | [Code Tools Options](<IDE_Window__Codetools_Options.md> "IDE Window: Codetools Options")  
editormacroscript.xml |   
editoroptions.xml | [Editor Options](<IDE_Window__Editor_Options.md> "IDE Window: Editor Options")  
environmentoptions.xml | [Environment Options](<IDE_Window__Environment_Options.md> "IDE Window: Environment Options")  
fpcdefines.xml   
helpoptions.xml   
includelinks.xml   
inputhistory.xml   
packagefiles.xml   
compilertest.pas   
laz_indentation.pas

---

_Source: [https://wiki.freepascal.org/Primary_Config_Path](https://web.archive.org/web/20190916085455/https://wiki.freepascal.org/Primary_Config_Path)_
