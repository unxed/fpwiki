# Lazarus project files

### Project directory structure

Typical directory structure for Lazarus project: 

  * created - auto-created by IDE or created manually
  * VCS - need to be included in Version Control System

directory name | created | VCS | description   
---|---|---|---  
backup | auto | no | Contains backup copies of project source files.   
lib | auto | no | Contains compiled binary files for different CPU and OS.   
<Project name>.app | auto | no | macOS (Darwin) "application bundle" files (macOS treats folders with extension .app specially).   
frames | manual | yes | TFrame descendant files (*.lfm, *.pas), recommended for large projects with many frames.   
include | manual | yes | Platform-dependent includes. Contains sub-directories for different OS (darwin, linux, win32, win64, etc..) Recommended for multiplatform projects with platform-specific units.   
languages | manual | yes | Contains *.po files with resource strings for different languages. Recommended for multi-language projects.   
images | manual | yes | Picture files (*.png, *.jpg, *.ico, *.xpm, etc..), that can be compiled into resource by [lazres](<lazres.md> "lazres") utility. Recommended for projects and packages with many image files.   
logs | manual | no | Debug output files (*.txt, *.log).   
  
### Project files extensions

The [Lazarus Tutorial#The Lazarus files](<Lazarus_Tutorial.md> "Lazarus Tutorial") explains some extensions by an example. 

  * VCS - need to be included in Version Control System

extension | VCS | filetype | description   
---|---|---|---  
`.lpi` | yes | Lazarus Project Information | contains project-specific settings like compiler settings and needed packages. stored in XML   
`.lps` | no | Lazarus Program Session | Personal data like cursor positions, source editor files, personal build modes. stored in XML   
`.lpr` | yes | Lazarus Program | Pascal source of main program.   
`.lfm` | yes | Lazarus Form | Form configuration information for all objects on a form (stored in a Lazarus-specific textual format, similar to Delphi dfm; the actions are described by Pascal source code in a corresponding `*.pas` file)   
`.pas` | yes | Pascal code | Pascal code typically for a form stored in a corresponding `*.lfm` file   
`.pp`   
`.p` | yes | Pascal code | Useful if you want to avoid confusion with Delphi source code files.   
`.inc` | yes | Pascal code include | Included as part of Pascal code file. Typically contain platform-dependent definitions and routines.   
`.lrs` | no | Lazarus Resource | Generated Lazarus Resource file; not to be confused with a Windows resource file. This file can be created with lazres tool (in directory Lazarus/Tools) using commandline: lazres myfile.lrs myfile.lfm   
`.compiled` | no | FPC compilation state | Compiler does not generate any .compiled files. They are created by some build system (make, fpmake, lazbuild).   
`.ppu` | no | Compiled unit | Compiled source code created by the Free Pascal compiler for each unit and program.   
`.o`   
`.or` | no | Object file | Created by the compiler, every ppu file has a corresponding o file, needed by the linker.   
`.lpk` | yes | Lazarus package information | package-specific settings, like compiler settings and needed packages; stored in XML   
`.lrt` | no | Lazarus Resourcestring table | Lazarus Resourcestring table created when saving a lfm file and i18n is enabled. It contains the TTranslateString properties of the lfm. Do not edit them, they are overwritten.   
`.rst` | no | Resourcestring table | Resourcestring table created by the compiler for every unit with a resourcestring section. Do not edit them, they are overwritten.   
`.rc` | yes | Resource definition file | Delphi-compatible resource-definition script (.rc file) that describes the resources used by your application.   
`.po` | yes | GNU gettext messages | When [i18n](</index.php?title=i18n&action=edit&redlink=1> "i18n \(page does not exist\)") is enabled the IDE creates/updated the .po files with the resourcestrings from the rst and lrt files.   
`.ico` | yes | Icon file | Icon for application file   
`.res` | yes/no | Binary resource | There are essentially 3 types in Delphi: 1) auto generated .res files. 2) other .res files for which you have source (.rc) and are declared with the RC file in project or source like `{$R 'logo.res' 'logo.rc'}`. 3) other .res files for which you don't have source. So category one shouldn't be in SVN (so essentially same as other pre-generated files like .dcu/.o or .ppu). Category two essentially also not, since they can be regenerated. Finally category 3, that should be kept. In older Delphi these were often files to e.g. enable themes while the Delphi version didn't know that windows version. This is less of a problem with Lazarus. You can also convert category 2 to 3 to simply including the .res (removing the .rc source), so that you don't have to add e.g. an image to SVN etc).   
  
## An example .gitignore file for Lazarus/Free Pascal

[Lazarus.gitignore](<https://github.com/github/gitignore/blob/master/Global/Lazarus.gitignore>)

---

_Source: [https://wiki.freepascal.org/Lazarus_project_files](https://web.archive.org/web/20241003071031/https://wiki.freepascal.org/Lazarus_project_files)_
