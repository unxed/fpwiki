# IDE Window: Add to Package

│ **English (en)** │

## Contents

  * 1 New File
  * 2 Add Unit
    * 2.1 Has Register procedure
    * 2.2 Virtual unit
    * 2.3 Add LFM, LRS files, if they exist
    * 2.4 Scan unit for Unit name and register procedure
    * 2.5 Add Unit
  * 3 New Component
    * 3.1 Ancestor Type
    * 3.2 New class name
    * 3.3 Palette Page
    * 3.4 Unit file name
    * 3.5 Unit name
  * 4 New Requirement
    * 4.1 Package name
    * 4.2 Minimum version (optional)
    * 4.3 Maximum version (optional)
  * 5 Add File
    * 5.1 File name
    * 5.2 File type
  * 6 Add Files



## New File

Select a file type and click on the button 'Create new file'. A new file of this type will be created and added to the package. 

## Add Unit

This page allows you to add a unit on disk to the package. Choose the file with the '...' button. 

The **Unit name** will be fetched from the unit. If the IDE was not able to parse the unit, you can type the unit name yourself. But this is not needed because the IDE fetches the unit name anyway on several occassions. 

### Has Register procedure

This option is only relevant for design time packages installed in the IDE. 

Enable this, if the unit has a global 'procedure Register;' in the interface. The IDE will then add code to automatically call this procedure on initialization of the package in the IDE. Normally this procedure is used to register IDE items like new components in the component palette. All items registered in this procedure are associated with this unit. See for an example the LCL package the unit buttons.pp. 

### Virtual unit

This option defines that the added unit should be treated as virtual. That means the unit is part of the package and will be used, but it does not exists in the package directory itself. This option is used for third party units, that are added to a package. 

For example the interbaselaz package registrates components and icons of the FPC interbase.pp unit. That means the package defines, that a TIBDatabase belongs to the unit 'interbase'. 

### Add LFM, LRS files, if they exist

Add automatically the LFM and LRS files of the unit to the package too. 

### Scan unit for Unit name and register procedure

Parse the unit source and try to guess the above values. 

### Add Unit

Click this button to add the unit to the package. All the above options can be changed later. 

## New Component

Use this page to create a new unit with new component class. 

### Ancestor Type

For example: If you want to create a button like component, then choose TButton as base type. 

### New class name

Choose a new class name for the new component class. For example: TMyMuchBetterButton. 

### Palette Page

Choose the page in the component palette, where the new component class should be put, if the package is installed in the IDE. If the page name does not exist, a new page will be created during loading of the package. For example: Misc. 

### Unit file name

Choose the new unit name. For example: mycomponentunit.pas. 

### Unit name

Choose the new unit name. For example: MyComponentUnit. 

## New Requirement

Add a new dependency to the package. The package can then use all units of the new selected package. 

### Package name

Choose one of the packages, known to the IDE. If the package is not yet in the list, close this dialog and open the package once. Then come back and the package will be in the list. 

### Minimum version (optional)

You can set, what the minimum version the used package must have. 

### Maximum version (optional)

You can set, what the maximum version the used package must have. 

  


## Add File

Add an arbitrary file to the package. The file must exist on the disk. 

### File name

Type the full path or use the browse button '...' to select a file. 

### File type

Choose the file type. 

## Add Files

Add arbitrary files to the package. The files must exist on the disk. The file types are guessed. 

Use **Browse** to open a file dialog, select some files and add to the list. 

Use **Add directory** to open a dialog to add files of a directory and sub directories. See [Add directory to package](<IDE_Window__Add_directory_to_package.md> "IDE Window: Add directory to package"). 

Use **Switch paths** to switch between showing relative and absolute filenames. 

Use **Delete** to delete selected files from the list. 

Use **Add files to package** to confirm, close the dialog and add the whole list to the package.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Add_to_Package](https://web.archive.org/web/20190918075000/https://wiki.freepascal.org/IDE_Window%3A_Add_to_Package)_
