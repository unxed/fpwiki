# Lazarus Packages

│ **[Deutsch (de)](</Lazarus_Packages/de> "Lazarus Packages/de")** │  **English (en)** │  **[español (es)](</Lazarus_Packages/es> "Lazarus Packages/es")** │  **[français (fr)](</Lazarus_Packages/fr> "Lazarus Packages/fr")** │  **[日本語 (ja)](</Lazarus_Packages/ja> "Lazarus Packages/ja")** │  **[português (pt)](</Lazarus_Packages/pt> "Lazarus Packages/pt")** │  **[русский (ru)](<../ru/Lazarus_Packages.md> "Lazarus Packages/ru")** │  **[slovenčina (sk)](</Lazarus_Packages/sk> "Lazarus Packages/sk")** │    
****

## Overview of the Lazarus Package System

For other meanings of **Package** in the combined FPC/Lazarus projects see [Packages(disambiguation)](<packages\(disambiguation\).md> "packages\(disambiguation\)"). 

[![packagegraph1.png](https://wiki.freepascal.org/images/7/78/packagegraph1.png)](</File:packagegraph1.png>)

## Contents

  * 1 Overview of the Lazarus Package System
  * 2 What is a Lazarus package?
  * 3 Coming from Delphi
  * 4 FAQ
  * 5 Quick Start
    * 5.1 Creating a new package
    * 5.2 Adding a new component
    * 5.3 Add a component icon
      * 5.3.1 Using Lazarus resources
      * 5.3.2 Using FPC resources
    * 5.4 Creating a package for your common units
    * 5.5 Adding the LCL as dependency
    * 5.6 Use your package in your project
  * 6 The IDE menu items for packages
    * 6.1 Removing Packages
  * 7 The theory
  * 8 Package states
  * 9 Hints and Tips
  * 10 Adding existing components to a Package
  * 11 Create a package with a unit that has the same name as the package
  * 12 File Type
  * 13 The Register procedure
  * 14 Search paths
  * 15 Your package provides a library, how to extend the linker search path in all your projects
  * 16 Design Time vs Run Time package
    * 16.1 Typical scenario for a run time and design time package
    * 16.2 For Delphians: I can not install a run time package, why not?
    * 16.3 Examples
  * 17 Platform specific units
    * 17.1 One unit per platform, several units with same name
    * 17.2 One unit for only one platform / conditional unit
  * 18 Different versions of a package
    * 18.1 Example library with multiple lpk
      * 18.1.1 Using minimum, maximum version
      * 18.1.2 Using preferred file name
  * 19 Build modes
  * 20 Compiling a package via make
  * 21 Creating a closed sourced package
  * 22 Parallel Compilation
  * 23 See also



## What is a Lazarus package?

A Lazarus package is a collection of units and components, containing information how they can be compiled and how they can be used by projects or other packages or the IDE. The IDE automatically compiles packages if some of its files are modified. In contrast to Delphi, packages are not limited to libraries and they can be OS independent. ([Library Packages](<packages.md> "packages") are specially compiled libraries used by applications, the IDE or both. Delphi/Library packages require in-compiler support, which FPC is not capable of at the moment and of course this magic is OS dependent.) 

Currently the [Free Pascal](<Free_Pascal.md> "Free Pascal") compiler only supports static packages, not dynamic packages. Therefore you must compile and restart the IDE, each time a package is installed or uninstalled. 

A lazarus package is identified/distinguished by the name and its version. 

Packages are ideal for sharing code between projects. 

Here is a typical directory layout of a Lazarus package: 
    
    
    /home/user/pascal/pkg1/  - an arbitrary directory, often it is named similar to the package name
      pkg1.lpk               - the package file
      pkg1.pas               - this unit is automatically created by the IDE and is only needed for compiling
      apackageunit.pas       - your shared unit, choose a unique name that do not clash with other sources/packages
      aform.pas              - your shared form
      aform.lfm              - the form file of your shared form
      lib/i386-linux/        - the output directory of the package. The directory and its content is automatically created.
        pkg1.compiled        - the state file containing the parameters how the package was compiled the last time.
        pkg1.ppu
        pkg1.o
        apackageunit.ppu
        apackageunit.o
        aform.ppu
        aform.o
        aform.lfm
    

## Coming from Delphi

If you are familiar with Delphi packages then read the following paragraph, because there are some important differences between a Delphi and a Lazarus package. 

  * Lazarus packages are not only a dynamic library. They are used to share units between projects and modularize big projects.
  * Sometimes the Delphi main file of a package is called "Package Project". This term makes no sense under Lazarus.
  * Many Delphi packages provide one "package project" per compiler version. This is hardly needed under Lazarus. But you can do that for Delphi compatibility. Lazarus packages have version information and projects can define that they require at least or at most a specific version of a package. Lazarus will automatically load the right version.
  * If a unit can not be found, under Delphi you are used to add the directory to the global search path. This has many drawbacks when sharing code. That's why this is not supported by Lazarus. Use packages instead.



## FAQ

**Q:** Do I need to install a package?  
**A:** You **only** need to install a package, if it contains designtime items, like components for the IDE component palette. If you don't use those items, you don't need to install the package. If you only want to use a package in your project, don't install it. Use the project inspector and add a requirement. 

**Q:** I installed a package, but the IDE does not find the units  
**A:** Installing a package means that the package is integrated into the IDE, not in your project. These are separate things. To use a package in your project, use Project -> Project Inspector -> Add -> New Requirement. And uninstall the package, if it does not contain any IDE goodies. 

## Quick Start

To see the packagesystem in action and to get used to it, do the following: 

### Creating a new package

  * Package->New Package... or File->New... -> Package -> Package
  * A package editor opens



[![Package Maker](https://wiki.freepascal.org/images/3/37/How_to_write_lazarus_component_package_maker.png)](</File:How_to_write_lazarus_component_package_maker.png> "Package Maker")

  * A window titled "Save Package NewPackage 0.0(*.lpk)" opens. Choose a directory and a name for your new package. The name must be a valid Pascal identifier with the extension _.lpk_.
  * Use the _Save_ button at the bottom right to save the package in the directory and name you've chosen.
  * Depending on your 'naming' setting in the 'environment options', the IDE will ask you to save the file lowercase. Say yes.



Congratulations: You have just created your first package! 

### Adding a new component

  * Use the Add button -> New component
  * Choose a component in the ancestor type combobox. For instance: TBevel.
  * Click Ok
  * The file will be added to the package and opened in the editor
  * Install the package by clicking the 'install' button in the top of the package editor.
  * Lazarus will save the package and ask you, if the IDE should be rebuilt. Say yes.
  * The packages are statically linked, so a restart of the IDE is needed.
  * Restart Lazarus and see your new component in the component palette (For example: A TBevel1 will be on the 'Additional' page).
  * If you do not see your new component in the component palette, it is most likely that you are not running the re-compiled version of Lazarus. You can set where Lazarus builds to in: Tools -> Options -> Environment options -> Files -> Lazarus directory. Instead of calling lazarus directly, you also can use [startlazarus](<startlazarus.md> "startlazarus"), which starts the newly created lazarus, for example the lazarus executable in the ~/.lazarus directory, if you don't have write access to the directory lazarus was installed into.



Congratulations: You have just installed your first package with your first package component. 

### Add a component icon

Component icons can use Lazarus resources or FPC resources. 

#### Using Lazarus resources

Also page exists here: [How_to_add_icons_my_package_components](<How_to_add_icons_my_package_components.md> "How to add icons my package components"). 

The idea is to create a png, convert that into a lrs include file using the [lazres](<lazres.md> "lazres") tool and include that. Since 0.9.29 there is a button on the new component page to do that automatically. 

Here are the details: 

  * Size of PNG file should be 24x24
  * Name of png must be the _class name_ of the new component, case does not matter (e.g. _tmyComponent.png_ for _TMyComponent_).



To create the lrs file run (where _TMyComponent_ is the class name of your component and _yourunit_ is the unit name): 
    
    
    ~/lazarus/tools/lazres yourunit.lrs TMyComponent.png
    

  * You may need to compile _lazres_ at first use. Simply open the _lazres.lpi_ in the IDE and at the menu click run > build.



Hint: You can add more than one image to the lrs file by appending the image filename at the end. For example 
    
    
    ~/lazarus/tools/lazres yourunit.lrs TMyComponent.png TMyOtherCom.png ...
    

Following is a sample of the resulting _yourunit.lrs_ file. 
    
    
    LazarusResources.Add('TMyComponent','PNG',[
      #137'PNG'#13#10#26#10#0#0#0#13'IHDR'#0#0#0#24#0#0#0#24#8#2#0#0#0'o'#21#170#175
      +#0#0#0#4'gAMA'#0#0#177#143#11#252'a'#5#0#0#0'|IDAT8O'#237#212#209#10#192' '#8
      +#5'P'#247#231#251's'#215#138#133#164#166'\'#220#195'`'#209'c'#157'L'#173#131
      +#153#169'd4'#168'dP'#137'r_'#235'5'#136'@Zmk'#16'd9'#144#176#232#164'1'#247
      +'I'#8#160'IL'#206'C'#179#144#12#199#140'.'#134#244#141'~'#168#247#209'S~;'#29
      +'V+'#196#201'^'#10#15#150'?'#255#18#227#206'NZ>42'#181#159#226#144#15'@'#201
      +#148#168'e'#224'7f<@4'#130'u_YD'#23#213#131#134'Q]'#158#188#135#0#0#0#0'IEND'
      +#174'B`'#130
    ]);
    

  * Click the Add button again, goto the Add File tab and browse to the yourunit.lrs file and click Ok. This is needed, so that the IDE checks this file for changes and will recompile your package.


  * Add an include directive to yourunit:


    
    
    unit YourUnit;
    ...
    uses 
      ..., LResources;
    ...
    implementation
    ...
    initialization
      {$I yourunit.lrs}
    end.
    

Note: You can include more than one lrs file, but each only once. 

  * That's all. Compile or install your package



#### Using FPC resources

Create a 24x24 16 color bitmap for each component. This format won't support PNG files. Then create an rc file as follows: 
    
    
    TMYCOMPONENT1NAME  BITMAP  "pathtobitmap1//bitmap1.bmp"
    TMYCOMPONENT2NAME  BITMAP  "pathtobitmap2//bitmap2.bmp"
    ....
    TMYCOMPONENTnNAME  BITMAP  "pathtobitmapn//bitmapn.bmp"
    

Note that double slashes (Linux) and double backslashes (Windows) are used in the bitmap path. It is advisable for the file's name to be the same as the package name with an extension of rc, ie MyPackageName.rc 

Let windres (Binutils) create the actual resource: 
    
    
    pathToWindres/windres -J rc -O res "MyPackageName.rc" "MyPackageName.dcr"
    

Alternatively, you can use the lazres tool to create a .res file: 
    
    
    pathToLazarus/tools/lazres MyPackageName.res bitmap.bmp bitmap_150.bmp bitmap_200.bmp
    

Add to the registration unit of the package {$R *.dcr} or {$R *.res} right after the implementation keyword, and compile/install the package. 

### Creating a package for your common units

Let's say you have some units, that you want to use in your projects. Put the units in a separate directory, for instance _C:\MySharedUnits_. 

  * Package->New Package or File->New... -> Package -> Standard Package
  * A package editor opens
  * Use the Save button at top left. Choose _C:\MySharedUnits\MySharedUnits.lpk_. The package name must not conflict with any existing unit.
  * Depending on your 'naming' setting in the 'environment options', the IDE will ask you to save the file lowercase. Say _yes_.
  * Now you have a package with the name _MySharedUnits_ stored as _C:\MySharedUnits\mysharedunits.lpk_.
  * Adding the units: _Add- >Add Files->Add directory_.
  * A dialog will appear with the directory _C:\MySharedUnits_ and some include and exclude filters. Click _Ok_ to add all units.
  * The units will now be listed. Click _Add files to package_.
  * The files are now added to the package. Click _Save_.
  * If your units only require the standard FPC units you can now simply press _Compile_. Otherwise see below.



### Adding the LCL as dependency

If your package contains forms or any other GUI LCL unit, then it requires the LCL package. 

  * In the package editor of your package click _Add - > New Requirement_
  * Choose as _package name_ _LCL_
  * Click _Ok_.
  * The LCL is now added under Required Packages. Click _Save_.



You can see the what the LCL does, by clicking in the package editor of your package _Compiler Options / Inherited_. 

If you only use non-GUI LCL components such as [fileutil](<fileutil.md> "fileutil"), you can add a dependency to _LCLBase_

### Use your package in your project

You need to add a dependency from the project to your new package. There are several ways to do this: 

  * Open your package. For example via _Package / Open recent package_. On the package editor of your package click _More ... / Add to project_.
  * Or you can use the project inspector: 
    * _Project / Project Inspector_.
    * Click on the _Add_ button with the plus.
    * Select the page _New Requirement_
    * Choose as package name your package
    * Click Ok



## The IDE menu items for packages

  * Package -> New Package **or** File->New... -> Package -> Standard Package 
    * Creates a new package.
  * Project -> Project Inspector 
    * Here you can see, what packages are required by the currently open project.
    * You can add new dependencies and remove unneeded ones.
  * Run -> Compiler options -> Inherited 
    * Here you can see what compiler options are inherited from which package.
  * Package 
    * **Open package** : A dialog shows all open packages with their state.
    * **Open package file** : Open a .lpk file
    * **Open package of current unit** : Open the .lpk file, that belongs to the file in the source editor
    * **Open recent package** : Open a recently open package file (lpk file)
    * **Add active unit to a package** : Adds the unit in the source editor to a package
    * **Package Graph** : The package graph shows all open packages and their dependencies.
    * **Configure installed packages** : Edit the list of packages installed in the IDE. Install or uninstall several packages at once.



### Removing Packages

  * To remove installed components, on the IDE menu, click Package > Configure installed packages. The following image shows the Installed Packages tool.



[![Installed Components](https://wiki.freepascal.org/images/0/0e/How_to_write_lazarus_component_installed_components.png)](</File:How_to_write_lazarus_component_installed_components.png> "Installed Components")

  * Select the package you want to uninstall and click Uninstall selection.



If something goes wrong with a package (for instance a package directory is deleted without first uninstalling it), Lazarus may not allow you to uninstall packages. To fix the problem, at the IDE menu click Tools > Build Lazarus. Lazarus will rebuild all packages and restart. You should now be able to uninstall problematic packages. 

## The theory

Each Lazarus package has a .lpk file. A package is identified by its name and its version. The name must correspond to the lpk filename. For example: 

Name: Package1, Version: 1.0, Filename: /home/.../package1.lpk. 

  * The IDE automatically creates the main source file (package1.pas). See below. The lpk file contains information about the required packages, the files it uses, how to compile them, and what is needed to use the package by other packages/projects. The directory where the lpk file is, is called the "package directory".
  * The IDE maintains a list of all package files (<config directory>/packagelinks.xml). Every time a package is opened in the IDE it will be added to this list. When a package is opened, the IDE automatically opens all required packages via this list.
  * Normally a package has a source directory with some pascal units. And normally the lpk file will be there too. A package has also an output directory. Default is the subdirectory 'lib/$(TargetCPU)-$(TargetOS)/' in the package directory.
  * Before a package is compiled the IDE checks all required packages and if they need update and have the auto update flag, they are compiled first. Then the IDE creates the package main source file. If the lpk file was package1.lpk, then the main source file is package1.pas. This file contains all units in the uses section plus a ['Register' procedure](<Register_\(package\).md> "Register \(package\)"), which is called in the initialization section.



For example: 
    
    
    { This file was automatically created by Lazarus. Do not edit!
       This source is only used to compile and install
       the package GTKOpenGL 1.0.
    }
    unit GTKOpenGL;
    
    interface
    
    uses GTKGLArea, GTKGLArea_Int, NVGL, NVGLX, LazarusPackageIntf;
    
    implementation
    
    procedure Register;
    begin
      RegisterUnit('GTKGLArea', @GTKGLArea.Register);
    end;
    
    initialization
      RegisterPackage('GTKOpenGL', @Register);
    end.
    

  * Then the compiler is called and the package is compiled to the output directory.
  * The IDE compares the current compiler options with the last one, which are stored in the .compiled file. If they differ the IDE cleans up the output directory and compiles the package with -B, telling the compiler to recompile everything it can. Differences in search paths and verbosity options do not trigger a clean up. Since 0.9.31 the IDE removes all files in the unit output directory on clean up if the output directory does not contain source files (listed in the package editor) and is not shared with required packages.
  * After compilation (successful or not) the state file is created. The state file is put into the output directory. It has the name <packagename>.compiled and contains the information, how the package was compiled and if the compilation succeeded. This state file is used by the IDE to check if update is needed and if a full recompile (-B) is needed.



For example: gtkopengl.compiled: 
    
    
    <?xml version="1.0"?>
    <CONFIG>
      <Compiler Value="/usr/bin/ppc386" Date="781388725"/>
      <Params Value=" -Rintel -S2cgi -CD -Ch8000000 -OG1p1
        -Tlinux -gl -vewnhi -l -Fu../../../lcl/units
        -Fu../../../lcl/units/gtk -Fu../../../packager/units
        -Fu. -FElib/ gtkopengl.pas"/>
    </CONFIG>
    

  * The IDE opens all needed packages automatically. This means, it opens all installed packages, all packages marked for installation (auto install), all packages with an open Editor, all packages required by the project and all packages required by one of the other packages. Unneeded packages are automatically unloaded, when the IDE becomes idle.
  * The IDE never opens two packages with the same name at the same time. When the user opens another package file with the same name the IDE will ask if it should replace the old one.
  * The IDE maintains two extra sets of packages: The 'installed' packages and the 'auto install' packages. The auto install packages will be linked into the IDE on next compile. It creates two new files in the config directory: staticpackages.inc and idemake.cfg. Then it calls 'make ide OPT=@/path/to/your/config/idemake.cfg' to compile itself.



## Package states

A package can have none, one or several of the following states: 

  * **used** \- a package can be **used** by a project or package, i.e. it is required and listed in Project inspector or Package Editor respectively. When you open a project or package all **used** packages are automatically **loaded** by the IDE.
  * **loaded** \- When the IDE opens a .lpk file the package is loaded. The IDE knows its settings and files. The IDE automatically loads all **used** packages of a project and its **installed** packages. This does not load the package code, as the compiler and IDE do not support dynamic packages yet.
  * **marked for installation** \- To run the package code inside the IDE, you need to rebuild the IDE and restart it. That means the first step is to **mark** the package **for installation**. Then rebuild the IDE and finally restart it.
  * **installed** \- The compiled code of the package is loaded and registered in the currently running IDE. If the IDE was able to locate the .lpk file it will be **loaded**.
  * **uninstall on next start** \- The package is **installed** , but will not be compiled into a new IDE. That means, when you rebuild the IDE and restart it, the package will no longer be **installed**. Keep in mind, that a package can be required indirectly by an installed package and thus will be installed as well.



## Hints and Tips

Please add any hints, tips or gotchas here. 

  * To rename a package, use 'save as'.
  * You can add to every dependency the default filename. For instance you provide an example project for your package. When another programmer opens your example for the first time, the IDE does not know where the lpk file is. Simply right click on the dependency in the project inspector and 'Store file name as default for this dependency'. The file name is stored relative to the project. You can do the same for package dependencies.
  * Use package: to use all units of the package in the project, enable the **Add package unit to uses section** on package editor / Options / Usage. When the package is added to a project, the lpr uses section is extended with the package main unit. For example if the package implements a new graphic type, it only has initialization sections, but the no project code use it directly.



## Adding existing components to a Package

If you want to add a file that contains a component to an existing package, you can do the following: 

  * Open the package file
  * Click on the 'Add' file to add a new item to the package.
  * Select the unit tab in the 'Add to package' dialog.
  * Choose the file to add.
  * Check the 'Has register procedure' if the unit contains a Register procedure. If you do not do this, the component(s) will not be shown on the component palette.
  * Click 'Add unit'.
  * Recompile and install the package.



That's it. The component should now show on the component palette. 

## Create a package with a unit that has the same name as the package

Normally the IDE auto creates the registration code into a unit with the same name as the package. If you want to name your package the same as an existing unit, for example because the package has only one unit, then you need at least Lazarus 0.9.29 and use this: 

  * Make a backup of your unit _mypackage.pas_.
  * Create the package _mypackage.lpk_ normally and do not add the unit yet.
  * Add a new unit, for example _mypackageall.pas_.
  * Select the unit _mypackageall.pas_ in the package editor and right click for the popup menu.
  * Change _File type_ to _Main Unit_.
  * Now restore your unit _mypackage.pas_ and add it to the package.



## File Type

Select a file in the package editor, right click for the popup menu. File Type: 

  * Unit - A .pas or .pp file (or .p in mode macpas)
  * Virtual Unit - Placeholder for a unit not in the package folders. For example the package FCL contains some virtual units for some FPC units.
  * Main Unit - this unit contains the autogenerated code to compile the package. There can only be one per package.
  * Issues xml file [see here](<IDE_Window__Restriction_Browser.md> "IDE Window: Restriction Browser")



## The Register procedure

To show a component on the component palette, it must be registered in Lazarus. This is done in the 'Register' procedure. This is a procedure that must appear in the interface section of the unit it is in, and must issue one or more RegisterComponent calls, as well as install property and component editors for the components. 

One or more Register procedures can be present in a package: in the package editor, you must indicate the units that have a 'register' procedure, so Lazarus knows which procedures it must call when the package is installed. 

There are 2 ways to mark a unit as having a register procedure: 

  * You can do this while adding the units to the package (see the 'Adding existing components to a package' section),
  * or you can select the unit in the package dialog, and set the 'Register' checkbox in the details panel.



Do not forget to recompile and install the package after changing the properties. 

## Search paths

All search paths are stored relative to the directory of the .lpk file. If you use an absolute path you have either not understood packages or you don't care. 

Each package has two types of search paths: 

  * Paths in the _Options / Compiler Options_ are used to compile the package itself.
  * Paths in the [Options / Package Options / Usage](<IDE_Window__Package_Options.md> "IDE Window: Package Options") are used by packages and projects that require this package. In other words these paths are added to the search paths of the project (or requiring package). Keep in mind that if project uses package A and that uses package B, the _Usage_ options of B are appended to A **and** the project.



Since 0.9.29 you can specify compiler options depending on the target platform. 

## Your package provides a library, how to extend the linker search path in all your projects

A package can define search paths that are added to packages and projects using it. See [Options / Package Options / Usage](<IDE_Window__Package_Options.md> "IDE Window: Package Options"). 

## Design Time vs Run Time package

There are four different types of packages. 

  1. A **design and runtime** package can be installed in the IDE and can be used normally by projects. This is the default.
  2. A **design time** package can be installed in the IDE and provides IDE extensions like new components, new file or project types, new menu items, new component or property editors, etc. Projects can use _design time_ packages (i.e. they are listed in the project inspector requirements), but by default a design time package is not compiled into the project. That means project units cannot use units of the _design time_ package. When a project uses such a package the IDE warns before opening a form of the project, so the user can install it first. To test a design time package outside the IDE you may want to use it in a test project. For this case you can change this in _Project Options / Miscellaneous / Use design time packages_.
  3. A **run time** package can be used by projects. A user cannot install such a package directly (it is not listed). It cannot register things in the IDE. It can be used by _design time_ packages, so it can be installed indirectly.
  4. A **run time only** package can only be used by projects. It cannot be installed in the IDE, not even indirectly. For example if the package is incompatible with the core IDE packages like the LCL. Since 0.9.31.



  


### Typical scenario for a run time and design time package

You have a component library called _Awesome_ and you want to write some IDE editors for some special properties. For example a property editor for the property Datafile which shows a TOpenDialog to let the user select a file. You can put the components into one run time package named _AwesomeLaz.lpk'_ and create another directory _design_ for the design time package _AwesomeIDELaz.lpk_. Move all units containing the IDE editors and registering stuff into the _design_ directory. Add a dependency in AwesomeIDELaz to AwesomeLaz. Make sure they do not share units or search paths. For an example see _components/turbopower_ipro/_ : The run time package is _turbopoweripro.lpk_ and the design time package is _design/turbopoweriprodsgn.lpk_. 

### For Delphians: I can not install a run time package, why not?

**Installing a package** means compiling the whole code into the IDE. A **run time** package does not contain code for the IDE or maybe even bites it. In order to use a package in your project, simply open the package, right click on the package editor for the popup menu and click **Add to project**. Alternatively you can use _View / Project Inspector_. The project inspector shows what packages are used by your project, you can add dependencies or delete them. 

### Examples

Example for extending the Usage / Units path. 

The tiOPF framework has the following directory layout, due to the fact that it compiles for FPC, Delphi 5-7, D2005 and D2006: 
    
    
    Source                <= full path \Programming\3rdParty\tiOPF\Source
      \Compilers
         \Delphi7          <= Delphi 7 package files live here
         \D2005
         \FPC              <= the tiOPF.lpk lived here
      \Core                <= core unit files
      \Options             <= optional unit file for extra features
      \GUI

Using this example, the following paths are included in the "Options - Usage - Units" editbox: 
    
    
    "$(PkgOutDir);..\..\Core;..\..\Options;..\..\GUI" 
    

This search path will be added to whatever project uses this package. The default is $(PkgOutDir), which would add only the ppu files to the project. Adding more search paths has the effect that the .pas source files are found by the compiler a second time - once when compiling the package and once when compiling your project. Since package and project use different settings the compiler might create a second version of the ppu, which is put into your project's output directory. Then you have two ppu for one unit and then you can get strange errors. To avoid the duplication disable the compilation of the package, by setting it to compile only manually. 

## Platform specific units

There are various ways to deal with separating platform-specific code: 

### One unit per platform, several units with same name

For example the unit _myconnect.pas_ has two versions, one for Windows and one for Unix/Linux/macOS/BSD. The idea is to put the units into different sub directories and use macros in search paths. 

  * Create two sub directories: **win** and **unix**.
  * Add to the package options / Compiler options / Paths / Other unit files the path **$(SrcOS)**. SrcOS is a [macro](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames"), that translates to _win_ under Windows and _unix_ under Linux/macOS/BSD.
  * Create a new package unit (package / Add / New file / Pascal unit) and save it under **win/myconnect.pas**.
  * If the unit is not selected in the package editor, select it.
  * Disable **use unit**.
  * Create a second package unit (package / Add / New file / Pascal unit) and save it under **unix/myconnect.pas**.
  * Keep the **use unit** enabled. It should be used once.



### One unit for only one platform / conditional unit

For example: the unit _mywin.pas_ should only be used under Windows, i.e. it should only be used and compiled if the flag **Windows** is defined. 

The idea is to use the unit indirectly using IFDEFs. The following example shows how the unit **mywin.pas** is only used and compiled when the flag **Windows** is defined. 

  * Create a new package unit (package / Add / New file / Pascal unit) and save it under **mywin.pas**.
  * If the unit is not selected in the package editor, select it.
  * Disable **use unit**.
  * Create a new package unit (package / Add / New file / Pascal unit) and save it under **mypkgcompile.pas**.
  * Add to the uses section of **mypkgcompile.pas** :


    
    
    uses
      Classes
      {$IFDEF Windows}
      ,mywin
      {$ENDIF}
      ;
    

Note: mypkgcompile will be compiled under all platforms. mywin will only be compiled under Windows. You don't need to use the unit mypkgcompile in your project. 

## Different versions of a package

If you work with different versions of a package, you can open at any time the other package file. The old package will be closed and the new one will be opened. Registered items will be transferred to the new package. For example the component palette still shows the components of the old package, with which the IDE was built. But right clicking on a component will show a menu item to open the recently used package. 

You can specify the minimum, maximum version required. When the project is opened and there are multiple .lpk files the IDE will automatically open the one with the right version. 

If you work with different projects that require different versions of a package (for example with different branches of the same package), you can define the preferred one in the dependencies. In the project inspector right click on the dependency and click on _Set file name as preferred for this dependency_. When the project is opened, the preferred package will be opened. If there is already a package with the same name it will be replaced. This feature exists since 0.9.29. 

[![SetDefaultPkDependency1.png](https://wiki.freepascal.org/images/c/c8/SetDefaultPkDependency1.png)](</File:SetDefaultPkDependency1.png>)

[Project Inspector](<IDE_Window__Project_Inspector.md> "IDE Window: Project Inspector")

### Example library with multiple lpk

A library has two lpk files, _main.lpk_ and _sub.lpk_. _main_ depends on _sub_. The library has two version _1.0_ and _2.0_. 
    
    
    trunk/
      main.lpk  version 2.0, requires sub
      sub.lpk   version 2.0
    fixes/
      main.lpk  version 1.0, requires sub
      sub.lpk   version 1.0
    

When you open _main.lpk 1.0_ for the first time, the IDE searches the package _sub_ in its database and then it searches _sub.lpk_ in the current folder. It finds _sub.lpk 1.0_ and loads it. 

When you then open _main.lpk 2.0_ , the IDE knows already a _sub.lpk_ , the version 1.0 and loads it. It does not load the _sub.lpk 2.0_. 

There are two solutions: 

#### Using minimum, maximum version

You can set for each dependency the minimum and maximum version. When you set the minimum and maximum version of the dependency the following will happen: The _main 1.0_ and _sub 1.0_ is open. You open _main 2.0_ which requires _sub >=2.0 and <=2.0_. The IDE unloads _main 1.0_ , loads _main 2.0_. The _sub 1.0_ does not fit, so the IDE searches for _sub 2.0_. Again it first searches in its database and then in the current folder. It finds _sub.lpk_ and loads _sub 2.0_. 

#### Using preferred file name

If the two lpk files are always distributed together you can use instead of minimum and maximum versions the **preferred** option. Open from trunk the _main.lpk_ and _sub.lpk_ to make sure that the right versions are loaded. Then right click on the dependency from _main_ to _sub_ and choose _preferred file name_. The file name will appear to the right of the dependency. Repeat this step with the lpk files from version _2.0_. The IDE will now always open the right _sub.lpk_. 

## Build modes

You can add build modes to your package. See here for the various ways to achieve this: 

[Build modes for packages](<Macros_and_Conditionals.md> "Macros and Conditionals")

## Compiling a package via make

The IDE can create a simple **Makefile** for a package via _Package editor / More ... / Create Makefile_. The IDE creates the file **Makefile.fpc** and calls **fpcmake** , which creates the Makefile. Since 0.9.29 the IDE also creates the file **Makefile.compiled**. If you are using a version control system like svn or git, add all three files to the repository: 
    
    
    Makefile
    Makefile.fpc
    Makefile.compiled
    

You can now run **make** (e.g. open a console, cd to the package directory with the Makefile) to compile the package. This will compile the package and after successful compilation copy the **Makefile.compiled** to the unit output directory. 

**Notes:**

  * The Makefile uses fixed paths, so it will **not** automatically adapt to changes as the IDE or lazbuild.
  * The IDE can recreate the Makefile on every compile. Enable the option _Package editor / Options / Compiler Options / Compilation / Create Makefile_.
  * The Makefile does not contain the logic for conditionals and build macros, it contains the settings at the time of creation.
  * The macros $(TargetOS), $(TargetCPU) and $(LCLWidgetType) are replaced by %TargetOS%, %TargetCPU% and %LCLPlatform%. The Makefile has somelogic to set some default values. You can override them (e.g. make LCLPlatform=qt).
  * The Makefile.compiled file is copied by the Makefile after successful compilation to <PkgOutDir>/<PkgName>.compiled and is checked by the IDE (and lazbuild) if a package needs a recompile. For example the Lazarus binaries (Windows Installer, deb and rpm packages, OS X dmg package) are compiled via **make** and when the IDE is started and compiles a program it will not recompile used packages, unless the user set some extra flags. For example when the user changes the _IDEBuildOptions_ via the _Configure Build Lazarus dialog_ the IDE will detect the difference and recompile the package on next compile.
  * If you forget to add the Makefile.compiled to svn you will get on make the error: /bin/cp: cannot stat `Makefile.compiled': No such file or directory



## Creating a closed sourced package

Goal: you want to publish a package, but you want to keep the source closed. 

At the moment many IDE functions require the source; the IDE warns if the source of a unit is missing. Therefore you must publish the interface parts of the units, and the files the compiler creates (ppu and o files). 

Steps: 

  * Create a copy of the package folder and change this copy as follows.
  * In the package editor / Options / IDE integration, set **Update / Rebuild** to **Manual compilation**.
  * Compile it once.
  * Delete the implementation sections of all units (i.e. everything after the implementation keyword of each unit including the initialization and finalization, only keeping the "end."). You can also remove the private section.
  * Optional: You can use the publish package tool (Package Editor / More... / Publish Package) to create a zip of the package.
  * You can delete the .compiled file, Makefile, rst files, lrt files. All other files in the output directory like .ppu, .o, .lfm files are needed by the compiler. The IDE needs the .lpk and the stripped .pas and .inc files.



There is a command line utility **sourcecloser** which alters the lpk and removes implementation sections. You can find it under _lazarus/components/codetools/examples/sourcecloser.lpi_. 

See also: 

  * this article about FPC closed source units: [Releasing units without source code](<Releasing_units_without_source_code.md> "Releasing units without source code")
  * Prezi: <http://prezi.com/vpuyxca-qboq/how-to-create-a-closed-source-package-with-lazarus/>



## Parallel Compilation

The IDE and lazbuild compile packages in parallel (since 1.3). Each package is still compiled with one call of fpc (single threaded). 

You can set the maximum number of parallel processes in _Tools / Options / Messages Window_. By default it tries to guess the amount of cpu cores with the function _GetSystemThreadCount_ in unit _UTF8Process_. 

## See also

  * [How To Write Lazarus Component](<How_To_Write_Lazarus_Component.md> "How To Write Lazarus Component") \- a tutorial about creating a new component.
  * [Install IDE packages without the IDE](<Install_IDE_packages_without_the_IDE.md> "Install IDE packages without the IDE")

---

_Source: [https://wiki.freepascal.org/Lazarus_Packages](https://web.archive.org/web/20250321020106/https://wiki.freepascal.org/Lazarus_Packages)_
