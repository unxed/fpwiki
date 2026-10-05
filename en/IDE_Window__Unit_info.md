# IDE Window: Unit info

│ **English (en)** │

The **Unit information dialog** as available from the menu [Source|Unit information] shows various information of the current file in the source editor. 

## Contents

  * 1 General
  * 2 Unit paths
  * 3 Include paths
  * 4 Source paths
  * 5 Show CodeTools Values
  * 6 OK



## General

[![Unit Info General 2 0 10.jpg](https://wiki.freepascal.org/images/c/c4/Unit_Info_General_2_0_10.jpg)](</File:Unit_Info_General_2_0_10.jpg>)

  * Name: the unit name as written in the source code. Otherwise it is the filename without the extension and path.
  * Type: The highlighter type. This can be changed via the popup context menu in the source editor.
  * In Project: Displays if the file is part of the current project. This can be changed in the [IDE Window: Project Inspector](<IDE_Window__Project_Inspector.md> "IDE Window: Project Inspector") or the [Main Menu](<Main_menu.md> "Main menu") > [Project](<Main_menu.md> "Main menu") > Add to Editor File to Project or Remove from Project menu items.
  * Size: The size of the file in bytes
  * Lines: The number of source code lines.
  * Path: Full file pathname.
  * Included by: This file was included last time by this source file. For example via an {$I filename.inc} include directive. This is automatically set each time the IDE parses the unit containing the include directive. You can clear this with the Clear button. You can use Search -> Goto include directive to jump to the include directive of the parent file.
  * Clear include cache: This button clears the 'Included by'.



  


  


## Unit paths

[![Unit Info Unit Paths 2 0 10.jpg](https://wiki.freepascal.org/images/5/5c/Unit_Info_Unit_Paths_2_0_10.jpg)](</File:Unit_Info_Unit_Paths_2_0_10.jpg>)

These are the unit paths used by the IDE for this file. The unit paths are used to search for .ppu, .pas, .pp, .p files. Normally these paths are given to the compiler, but there are cases where this is not the case and it does not show the unit paths defined in the compiler configuration file (`fpc.cfg`). 

These paths are created by the rules in the Tools -> Options -> CodeTools defines editor. All files in a directory share the same search paths (unit paths, include paths, ...). If this a project directory, all project unit paths are added. If the directory belongs to a package its unit paths will be added. If you put a project and a package into one directory, both paths will be added. That it why it is recommended to give packages and projects directories of their own. 

Difference between compiler search paths and IDE search paths: 

Normally the difference does not matter. The compiler is invoked in one directory with a set of search paths. For example: There are 4 packages: 
    
    
     /home/user/package1/package1.lpk
     /home/user/package2/package2.lpk
     /home/user/package3/package3.lpk using package1 and package2
     /home/user/package4/package4.lpk using package1 and package2
    

When package3 is compiled in /home/user/package3 with unit path /home/user/package1/;/home/user/package2/. When the compiler finds a unit in /home/user/package2/ and compiles it it applies all unit paths, including path to /home/user/package3/. This is normally unwanted and therefore a standard package defines an output directory. But for this example we assume the package compiles it .ppu files into the source directory. 

When the compiler is invoked for package4 it will be started in a different directory with different search paths. Here it is invoked in /home/user/package4 with unit path /home/user/package1/;/home/user/package2/. When the compiler finds a unit in /home/user/package2/ and compiles it it applies all unit paths, including the path to package4. This means the compiler compiles the units in package2 with two different unit paths. This is normally a bad idea as the result depends on the order - if you compile first package3 then package4 or the other way round. But in real world (e.g. the Delphi world) this is more common than one would expect. It is clear, that if packages contain the units with the same name the order of the file paths in the search paths becomes important. 

The IDE solves this by collecting search paths for every directory. That means, for example,the search paths for the Lazarus sources have their own unit paths and do not include the unit paths to your project. A directory, that is not part of your project, does not include the unit paths of your project. This way you can browse Delphi VCL sources and Lazarus LCL sources both containing units with same names. 

## Include paths

[![Unit Info Include Paths 2 0 10.jpg](https://wiki.freepascal.org/images/d/d0/Unit_Info_Include_Paths_2_0_10.jpg)](</File:Unit_Info_Include_Paths_2_0_10.jpg>)

Same as unit paths, but for the include paths. Search paths for include directives  
`{$I filename.inc}` or `{$INCLUDE filename.inc}`. 

  


  


  


  


  


  


  


  


  


  


  


## Source paths

[![Unit Info Source Paths 2 0 10.jpg](https://wiki.freepascal.org/images/0/0b/Unit_Info_Source_Paths_2_0_10.jpg)](</File:Unit_Info_Source_Paths_2_0_10.jpg>)

This is the unit path plus the compiled source paths. 

For example: 
    
    
     /home/user/project
     /home/user/package/lib
    

The project uses the package. The package source files are in /home/user/package and the output files (.ppu) of the package are put in /home/user/package/lib. 

The unit path of the project will contain the path to the output directory of the package - /home/user/package/lib. So when the compiler is invoked to compile the project it finds the .ppu files of the package, but not the source (.pas) of the package. This way it can not recompile the package, which is good, because the compiler options of the project are probably different than for the package. The IDE should find the source files (.pas). It knows, that /home/user/package/lib is the output directory of the package, so it adds the unit path of the package to the source path of the project. This addition is called the compiled source path. 

  


  


## Show CodeTools Values

See [Codetools Directory Values](<IDE_Window__Codetools_Directory_Values.md> "IDE Window: Codetools Directory Values")

## OK

The `OK` button dismisses the dialog.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Unit_info](https://web.archive.org/web/20230530055244/https://wiki.freepascal.org/IDE_Window%3A_Unit_info)_
