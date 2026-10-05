# IDE Window: Package Graph

│ **English (en)** │

The package graph shows the currently loaded packages and which package uses which and is used by which. 

This window is accessible via the IDE menu **Package / Package Graph**. 

  


## Contents

  * 1 Version 1.0 and below
  * 2 Version 1.1 and above
  * 3 Usage
    * 3.1 State



# Version 1.0 and below

In the left treeview all packages are listed. The nodes are the required packages. Click on a package to see what is using the package in the list to the right and some general information in the memo below. 

# Version 1.1 and above

[![packagegraph1.png](https://wiki.freepascal.org/images/7/78/packagegraph1.png)](</File:packagegraph1.png>)

Move the mouse cursor over nodes and edges to highlight dependencies. 

Click on a package to see some general information in the memo below. 

# Usage

Double click on a package to open the [package editor](<IDE_Window__Package_Editor.md> "IDE Window: Package Editor") of this package. 

Double click on a project to open the project inspector. 

Double click on the IDE to configure the installed packages. 

Packages are automatically unloaded, when they are not used anymore. 

## State

A package can have the following states: 

  * Installed - the package is installed. An installed package is compiled into the IDE and its classes are available in the component palette.
  * AutoCreated - the package is part of the IDE, it can not be uninstalled.
  * Install on next start - the package is not yet installed, but will be compiled into the IDE and will be installed after exiting the IDE and starting the new IDE. You compile a new IDE in Tools -> Build Lazarus.
  * Uninstall on next start (unless needed by an installed package) - This package was installed automatically because it was needed by a package that was installed normally. When the normally installed package is uninstalled this package will be uninstalled as well.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Package_Graph](https://web.archive.org/web/20190924085947/https://wiki.freepascal.org/IDE_Window%3A_Package_Graph)_
