# Installing Help in the IDE

**Installing Help in the IDE** for the Lazarus Runtime Library ([RTL](<RTL.md> "RTL")), Free Component Library ([FCL](<FCL.md> "FCL")) and the Lazarus Component Library ([LCL](<LCL.md> "LCL")). 

The installer or package will probably have already set up Help. If this is not the case, or in case of problems, follow these instructions. 

## Contents

  * 1 Changing the shortcut for context sensitive help
  * 2 Installing CHM help (Lazarus 1.0 and later)
  * 3 Installing CHM help (earlier versions than Lazarus 1.0, e.g. Lazarus 0.9.30.4)
  * 4 Installing INF help in the IDE
    * 4.1 Where to find INF help files?
    * 4.2 Why choose INF help?
    * 4.3 Why avoid INF help?
  * 5 Installing Kylix Help in the Lazarus IDE
  * 6 fpdoc entries of RTL and FCL



## Changing the shortcut for context sensitive help

Some platforms do not pass the F1 or Ctrl-F1 keys to applications. The key can be changed in the menu under _Tools / Options / Editor / Key Mappings / Help Menu Commands / Context sensitive help_. 

## Installing CHM help (Lazarus 1.0 and later)

Note: these instructions use Windows path notation, but they also apply to other systems with minor changes. 

  * Go to $(LazarusDir)\components\chmhelp\lhelp and see if lhelp.exe exists. Compile $(LazarusDir)\components\chmhelp\lhelp\lhelp.lpi if it does not. Although newer versions of Lazarus will compile lhelp if needed the first time you press F1 (OS X: Ctrl-F1), compiling the lpi yourself won't hurt.
  * Make sure the package **chmhelppkg** is installed; it should be installed automatically in newer installers. See the menu Package->Install Packages. It should appear in the left column and is called something like **ChmHelpPkg 0.2** (version number may differ). If not, install it and rebuild Lazarus when prompted.
  * Go to $(LazarusDir)\docs\chm and look for lcl.chm and fcl.chm. If they don't exist, go to <http://sourceforge.net/projects/lazarus/files/Lazarus%20Documentation/>, download the newest archive with the CHM files and unpack the files there.
  * Additional [Troubleshooting](<Troubleshooting.md> "Troubleshooting") step (e.g. useful if installing from Subversion): go to Tools -> Options -> "Help Options" and select the "CHM Help Viewer". Check its properties: 
    * "HelpExe" should be empty in order to use the lhelp you just built (i.e. $(LazarusDir)\components\chmhelp\lhelp\lhelp.exe).
    * "HelpFilesPath" should be where you just put the CHM files. Leave it empty (recommended) to use the default, which includes $(LazarusDir)\docs\html and $(LazarusDir)\docs\chm.
  * If you are on **Windows** you can also use the Windows-internal help viewer: 
    * "HelpExe" should be `hh.exe`
    * "HelpExeParams" must be `"%s::%s"` (WITH QUOTES, note the double colon).



Now context-sensitive help using F1 (OS X: Ctrl-F1) should work. 

## Installing CHM help (earlier versions than Lazarus 1.0, e.g. Lazarus 0.9.30.4)

Older versions of Lazarus search for CHM files only in $(LazarusDir)\docs\html, not $(LazarusDir)\docs\chm. Therefore, please follow the instructions [above](<Installing_Help_in_the_IDE.md> "Installing Help in the IDE") with these differences: 

  * When downloading/extracting the CHM files, extract into $(LazarusDir)\docs\html\ instead of $(LazarusDir)\docs\chm\\.
  * Note that an empty value (as recommended) in Environment Options/Help Options/CHM Help Viewer/HelpFilesPath will only check $(LazarusDir)/docs/html, not $(LazarusDir)/docs/chm



# Installing INF help in the IDE

If you have help files in the INF format, please see ["DocView IDE Integration"](<http://fpgui.sourceforge.net/docview_ide_integration.shtml>) for details on how to install such help in the Lazarus IDE. This option will use the fpGUI DocView help viewer to display and search the INF help files. 

## Where to find INF help files?

This [SourceForge](<http://sourceforge.net/projects/fpgui/files/fpGUI/Documentation/>) page contains links to download ZIP archives of the FPC Language Reference, RTL, FCL, LCL and fpGUI help in the INF format. 

## Why choose INF help?

  * INF help files are much smaller than other help formats, yet contain all of the same information. eg: LCL in HTML format is 187MB, LCL.CHM is 11.6MB, and LCL.INF is only 3.9MB. These sizes are all uncompressed sizes on your hard drive. So if you are limited to internet bandwidth, this is a good option to choose.
  * fpGUI DocView in an optimised help viewer with many advanced features. For example: 
    * Loading help files (even multiple help files) are extremely fast
    * You can do full text searches across all help files.
    * Advanced search terms are possible.
    * Search results are based on an advanced algorithm that takes alternative spelling in consideration, and uses a ranking system to give you the best possible results.
    * Help files can be annotated (inline end-user comments can be added)
    * Help topics can be bookmarked, so they can easily be found again.
    * If you are visually impaired (or just prefer a specific font family), you can customise the fonts and font sizes to use when displaying help text.



## Why avoid INF help?

  * The INF files available in the above link are from 2010 - they are very out of date, and the INF format is not being regularly maintained for FPC/Lazarus help.



# Installing Kylix Help in the Lazarus IDE

For users who have a legal copy of Kylix, it is possible to add Kylix 3 context-sensitive help files to Lazarus IDE running on Linux x86. The benefit from those help files is that they are very detailed, and also contain the Object Pascal Language Reference help - most of which is applicable to FPC's Object Pascal syntax or the Lazarus LCL. Note that this uses the proprietary HyperHelp program, so is not portable to other platforms. 

For instructions, please see [instructions in the Kylix article](<Kylix.md> "Kylix")

# fpdoc entries of RTL and FCL

The fpdoc entries for the FPC sources can be downloaded from svn: 
    
    
     cd /home/username/yourchoice/
     svn co http://svn.freepascal.org/svn/fpcdocs/trunk fpcdocs
    

Add the path _/home/username/yourchoice/fpcdocs_ to Tools / Options / Environment / FPDoc Editor 

The source editor hints (e.g. as displayed by hovering your mouse over a function/procedure/property) should now show help for TComponent.Name.

---

_Source: [https://wiki.freepascal.org/Lazarus_Offline_Documentation](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Lazarus_Offline_Documentation)_
