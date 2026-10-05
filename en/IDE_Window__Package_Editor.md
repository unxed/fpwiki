# IDE Window: Package Editor

│ **English (en)** │

## Navigation

The Package Editor dialog is reached from the Lazarus IDE [Main Menu](<Main_menu.md> "Main menu") > [Package](<Main_menu.md> "Main menu") > New Package... -or- Open Loaded Package... -or- Open Package File -or- Open Recent Packages, and then select a package (LazControls in the image shown). 

## Usage

[![](https://wiki.freepascal.org/images/7/7b/Package_Editor_2_0_10.jpg)](</File:Package_Editor_2_0_10.jpg>)

Package Editor

  * **Save** : Save changes to disk. (.lpk file)


  * **Compile** : Compiles the package and all dependencies.


  * **Use** : This opens a drop-down list when clicked. 
    * **Add to Project** \- Add this package as a dependency to the current project.
    * **Install** \- Compiles the package and all dependencies, adds it to the list of install packages of the IDE and rebuilds the IDE. Should be used only when the package registers components or functions to the IDE.
    * **Uninstall** \- Remove this package and all dependent packages from the list of installed IDE packages. You will be asked if you want then to rebuild the IDE. Rebuilding completes the removal.


  * **Add** : This opens a drop-down list when clicked. 
    * **Add Files from File System** \- Shows a standard FileOpen dialog which allows to choose files to add. Multi-selection is allowed.
    * **Add Files in Directory** \- Opens a dialog for selecting a directory and file name filters. The matching files from that directory are then added to the package. ToDo: explain better.
    * **New File** \- A new file of chosen type is created and added to the package.
    * **New Component** \- A new registered component is created. A dialog asks for an ancestor type, class name and other relevant information. ToDo: screenshot of the dialog.
    * **New Requirement** \- Now required packages and their Minimum/Maximum versions can be added one by one using the dialog that shows up. ToDo: this must be implemented better. There should be a way to add many requirements at one go.


  * **Remove** : Remove the currently selected item (e.g. file or dependency) from the package. A dialog will open to confirm.


  * **Options** : Opens the dialog [Package Options](<IDE_Window__Package_Options.md> "IDE Window: Package Options") to setup the usage options, version information, author and other general information of the package. [Compiler Options](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options") for the package can be found in the same options dialog.


  * **Help** : Show this help.


  * **More** : This opens a drop-down list when clicked. 
    * **Find in Files** \- open a search dialog with the package's directory preset.
    * **Sort Files Permanently** \- The package's files are sorted alphabetically and the new order is stored in the package. This is different from the "Sort files alphabetically" ToolButton which only sorts the view temporarily.
    * **Fix Files Case** \- ToDo: Explain.
    * **Show Missing Files** \- Shows a list of package's files which do not exist in file-system and lets you remove them.
    * **Save Package** \- Save changes to the .lpk file.
    * **Save Package As** \- Opens a dialog in which you can choose a new filename for the .lpk file. The package (.lpk) will be renamed. The files referenced by the package are not moved or renamed. If you save the renamed package (.lpk) in a new directory, check the paths in the [compiler options](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options").
    * **Revert Package** \- Discard any changes you have made, and reload the original package (.lpk) from disk.
    * **Publish Package** \- Opens [Publish Project / Package](<IDE_Window__Publish_Project_Package.md> "IDE Window: Publish Project Package") to copy the package and its files to another directory. This is useful for creating a package without compiled files or version files (svn, cvs, ...) as a preliminary to creating a zipped file of the package to send to another computer.
    * **Compile** \- Compile this package. If any required package needs compiling, that package will be rebuilt first.
    * **Recompile clean** \- Normally the compiler checks each unit in case it needs to be rebuilt. This option prevents such compiler checks, causing it to compile every unit in the package. If any required package needs compiling, it will be rebuilt first.
    * **Recompile all required** \- Same as _Recompile Clean_ , but all required packages are also recompiled. Keep in mind that some base packages like the FCL and LCL are set to never compile automatically and thus are never recompiled by this call. ToDo: Check the validity of the statement.
    * **Create fpmake.pp** \- Creates a fpmake.pp for this package, which can be used to compile the package without the IDE. ToDo: Explain more.
    * **Create Makefile** \- Creates a Makefile.fpc and a Makefile for this package, which can be used to compile the package without the IDE. Be aware that the Makefile is simple, and requires that all needed packages are at the same place. This means that if you copy the Makefile to another computer, it will need exactly the same paths for all packages (including Lazarus). So usually you will need to adapt such a copied Makefile yourself to allow it to run successfully on another computer (which is almost certain to have different path configurations).
    * **View Package Source** \- Opens the main source file of the package. This unit is automatically created by the IDE on each compile. So don't waste time editing it manually, since all your changes will be lost.
    * **View ToDo List** \- Opens the ToDo list viewer, restricted just to ToDo items from inside this package. Note, this shows up only if the TodoListLaz IDE package is installed.


  * **File properties** : The lower pane shows these properties when a file is selected. 
    * Register unit: Check this if the selected unit contains a procedure **Register** in the interface which should be called at designtime by the IDE. This is for example needed when the unit registers a new component for the IDE component palette.
    * Use unit: Check this if the selected unit should automatically be compiled when the package is compiled. This is enabled by default for Pascal units and unchecked for all others. Uncheck this if the unit is only usable under some platforms.
    * Registered plugins: For register units a list of components with icon and component page as provided by the register-unit is shown.


  * **Dependency properties** : The lower pane shows these properties when a dependent package is selected. 
    * Minimum Version: Version number of the dependent package must be equal or higher than this value.
    * Maximum Version: Version number of the dependent package must be equal or lower than this value.



## See also

  * [Lazarus Packages](<Lazarus_Packages.md> "Lazarus Packages")
  * [How To Write Lazarus Component](<How_To_Write_Lazarus_Component.md> "How To Write Lazarus Component") \- a tutorial about how to create a new component.
  * [Install IDE packages without the IDE](<Install_IDE_packages_without_the_IDE.md> "Install IDE packages without the IDE")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Package_Editor](https://web.archive.org/web/20250426113536/https://wiki.freepascal.org/IDE_Window%3A_Package_Editor)_
