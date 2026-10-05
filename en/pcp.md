# pcp

**PCP** , short for **Primary Config Path** , is a Lazarus command-line option which permits specifying the directory where Lazarus configuration files are located. 

For example, a Linux launcher link (.desktop file) may contain a line like: 
    
    
    Exec=/P/s/lazarus/lazarus **--pcp** ="/P/s/config_lazarus"
    

which tells Lazarus to look for its configuration in the folder: 
    
    
     /P/s/config_lazarus
    

This option is most useful when using [ multiple Lazarus](<Multiple_Lazarus.md> "Multiple Lazarus"): it is needed to keep their configurations properly separated. Note that while most versions of Lazarus you will come across are case insensitive to --pcp, lazbuild before (probably) 2.4.0 is not when it reads the lazarus.cfg file. 

Options to Lazarus, including this one, can also appear in the lazarus.cfg file, in the top lazarus directory. 

## Windows

If option --pcp is not specified, the default folder in Windows Vista and up is: 
    
    
    C:\Users\USERNAME\AppData\Local\lazarus
    

## Linux

For Linux (and other Unix-like systems) the default configuration folder is: 
    
    
    ~/.lazarus
    

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
idemake.cfg | A list of the compiled unit files for installed packages. Used to rebuild the IDE   
includelinks.xml   
inputhistory.xml   
packagefiles.xml | A list of the package files (.lpk) of packages installed in the IDE   
compilertest.pas   
laz_indentation.pas

---

_Source: [https://wiki.freepascal.org/pcp](https://web.archive.org/web/20221202073538/https://wiki.freepascal.org/pcp)_
