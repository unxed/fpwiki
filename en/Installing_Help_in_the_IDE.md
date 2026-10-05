# Installing Help in the IDE

**Installing Help in the IDE** for the Lazarus Runtime Library ([RTL](<RTL.md> "RTL")), Free Pascal, Free Component Library ([FCL](<FCL.md> "FCL")) and the Lazarus Component Library ([LCL](<LCL.md> "LCL")) and a number of associated things. 

The installer or package may have already set up Help (or lhelp), depending on how you installed Lazarus. If it is not operational or it is out of date, this page will help you. 

## Contents

  * 1 Changing the key for context sensitive help
  * 2 Installing CHM help (Lazarus 1.0 and later)
    * 2.1 Notes
  * 3 Installing CHM help (earlier than Lazarus 1.0)
  * 4 Installing INF help in the IDE
    * 4.1 Where to find INF help files?
    * 4.2 Why choose INF help?
    * 4.3 Why avoid INF help?
  * 5 Installing Kylix Help in the Lazarus IDE
  * 6 fpdoc entries of RTL and FCL



## Changing the key for context sensitive help

Some platforms do not pass the `F1` key to applications. The key can be changed in the menu under _Tools > Options > Editor > Key Mappings > Help Menu Commands > Context sensitive help_. 

## Installing CHM help (Lazarus 1.0 and later)

Note: these instructions use Windows path notation, but they also apply to other systems with minor changes. 

  * (This step is almost certainly unnecessary, jump to the next one) Go to `$(LazarusDir)\components\chmhelp\lhelp` and see if `lhelp` (Linux, FreeBSD, macOS) or `lhelp.exe` (Windows) exists. Compile $(LazarusDir)\components\chmhelp\lhelp\lhelp.lpi if it does not. Although newer versions of Lazarus will compile lhelp if needed the first time you press `F1`, compiling the lpi yourself won't hurt.


  * Make sure the package **chmhelppkg** is installed; it should be installed automatically in newer installers. See the menu Package->Install Packages. It should appear in the left column and is called something like **ChmHelpPkg 0.2** (version number may differ). If not, install it and rebuild Lazarus when prompted.


  * Go to `$(LazarusDir)\docs\chm` and look for `lcl.chm` and `fcl.chm`. If they don't exist, go to <http://sourceforge.net/projects/lazarus/files/Lazarus%20Documentation/> and download the newest archive with the CHM files and unpack the files into that directory.


  * Additional Troubleshooting step (eg useful if installing from Subversion): go to Tools > Options > "Help Options" and select the "CHM Help Viewer". Check its properties: 
    * "HelpExe" should be empty in order to use the lhelp you just built (ie `$(LazarusDir)\components\chmhelp\lhelp\lhelp.exe`).
    * "HelpFilesPath" should be where you just put the CHM files. Leave it empty (recommended) to use the default, which includes `$(LazarusDir)\docs\html and $(LazarusDir)\docs\chm`.
    * If you use several versions of Lazarus, or like to build new ones from time to time, you can put your help files in a different directory, probably above $LazarusDir, and point HelpFilesPath to it.
  * If you are on **Windows** you can also use the Windows-internal help viewer: 
    * "HelpExe" should be `hh.exe`
    * "HelpExeParams" must be `"%s::%s"` (WITH QUOTES, note the double colon).



Now context-sensitive help using `F1` should work. 

### Notes

  * If lhelp does not appear but there are no error messages, check `$XDG_CONFIG_DIR/lhelp/lhelp-lazhelp.conf` and make sure the Width and Height are not set to 0.


  * On some systems, lhelp has problems the second and subsequent time its activated. An easy solution is to close it after each use, it will startup very quickly the next time you need it.


  * macOS: At least in Lazarus 2.0.6, lhelp opens with an error (ignore by selecting OK) and blank contents. Use the Folder Open icon to navigate to the directory where you have put the help files and select the `toc.chm` file. The issue is resolved in Lazarus 2.0.10.



## Installing CHM help (earlier than Lazarus 1.0)

Pre-1.0 versions of Lazarus search for CHM files only in `$(LazarusDir)\docs\html` and not in `$(LazarusDir)\docs\chm`. Therefore, please follow the instructions above with these differences: 

  * When downloading/extracting the CHM files, extract into `$(LazarusDir)\docs\html\` and not `$(LazarusDir)\docs\chm\`.
  * Note that an empty value (as recommended) in Environment Options > Help Options > CHM Help Viewer > HelpFilesPath will only check `$(LazarusDir)/docs/html` and not `$(LazarusDir)/docs/chm`.



## Installing INF help in the IDE

If you have help files in the INF format, please see ["DocView IDE Integration"](<http://fpgui.sourceforge.net/docview_ide_integration.shtml>) for details on how to install such help in the Lazarus IDE. This option will use the fpGUI DocView help viewer to display and search the INF help files. 

### Where to find INF help files?

This [SourceForge](<http://sourceforge.net/projects/fpgui/files/fpGUI/Documentation/>) page contains links to download ZIP archives of the FPC Language Reference, RTL, FCL, LCL and fpGUI help in the INF format. 

### Why choose INF help?

  * INF help files are much smaller than other help formats, yet contain all of the same information. eg: LCL in HTML format is 187MB, LCL.CHM is 11.6MB, and LCL.INF is only 3.9MB. These sizes are all uncompressed sizes on your hard drive. So if you are limited to internet bandwidth, this is a good option to choose.
  * fpGUI DocView in an optimised help viewer with many advanced features. For example: 
    * Loading help files (even multiple help files) are extremely fast
    * You can do full text searches across all help files.
    * Advanced search terms are possible.
    * Search results are based on an advanced algorithm that takes alternative spelling in consideration, and uses a ranking system to give you the best possible results.
    * Help files can be annotated (inline end-user comments can be added)
    * Help topics can be bookmarked, so they can easily be found again.
    * If you are visually impaired (or just prefer a specific font family), you can customise the fonts and font sizes to use when displaying help text.



### Why avoid INF help?

  * The INF files available in the above link are from 2010 - they are very out of date, and the INF format is not being regularly maintained for FPC/Lazarus help.



## Installing Kylix Help in the Lazarus IDE

For users who have a legal copy of Kylix, it is possible to add Kylix 3 context-sensitive help files to Lazarus IDE running on Linux x86. The benefit from those help files is that they are very detailed, and also contain the Object Pascal Language Reference help - most of which is applicable to FPC's Object Pascal syntax or the Lazarus LCL. Note that this uses the proprietary HyperHelp program, so is not portable to other platforms. 

For instructions, please see [instructions in the Kylix article](<Kylix.md> "Kylix")

## fpdoc entries of RTL and FCL

The fpdoc entries for the FPC sources can be downloaded from svn: 
    
    
     cd /home/username/yourchoice/
     svn co https://svn.freepascal.org/svn/fpcdocs/trunk fpcdocs
    

Add the path `/home/username/yourchoice/fpcdocs` to Tools > Options > Environment > FPDoc Editor 

The source editor hints (eg as displayed by hovering your mouse over a function/procedure/property) should now show help for TComponent.Name.

---

_Source: [https://wiki.freepascal.org/Installing_Help_in_the_IDE](https://web.archive.org/web/20250211180904/https://wiki.freepascal.org/Installing_Help_in_the_IDE)_
