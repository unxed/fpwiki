# IDE Window: Installed Packages

│ **[Deutsch (de)](</IDE_Window:_Installed_Packages/de> "IDE Window: Installed Packages/de")** │  **English (en)** │  **[français (fr)](</IDE_Window:_Installed_Packages/fr> "IDE Window: Installed Packages/fr")** │    
****

## Contents

  * 1 Left List: Installed
  * 2 Right List: Available for installation
  * 3 Memo: Package info
  * 4 Buttons



[![IDE Window- Install-Uninstall Packages.png](https://wiki.freepascal.org/images/4/45/IDE_Window-_Install-Uninstall_Packages.png)](</File:IDE_Window-_Install-Uninstall_Packages.png>)

## Left List: Installed

This is the list of packages that will be compiled into the IDE when it is rebuilt. At the moment only static packages are available, so you have to rebuild the IDE to install/uninstall a package. 

Buttons: 

  * **Uninstall selection** : Removes the selected packages from the above list.
  * **Import list** : Load the list from a file.
  * **Export list** : Save the above list to a file (XML format).



## Right List: Available for installation

This is the list of all packages the IDE has seen on your system. If you are missing a package in this list, close the dialog and open the package (.lpk) file once. Then open the dialog again. 

Button: 

  * **Install selection** : Add the selected packages from the above list to the left list of **packages to install**.



## Memo: Package info

Show some information about the last clicked package. For example Author and Description, if available, and whether installed or not. 

## Buttons

  * **Help** : Show this Wiki Help page.
  * **Cancel** : Discard changes and close the dialog.
  * **Save and rebuild IDE** : Save changes and rebuild the IDE to compile the packages into the IDE. The packages will be available, when you restart the IDE. Depending on your installation, the IDE executable is readonly. In this case the IDE puts the new executable into the config directory (Linux, FreeBSD, macOS, Solaris: $HOME/.lazarus/bin/lazarus, Windows: ?). The startlazarus program automatically starts the correct executable. If you start the Lazarus executable manually you must start the correct one yourself.
  * **Save and exit dialog** : Save changes and exit dialog. The next time you rebuild the IDE the packages will be compiled into the IDE.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Installed_Packages](https://web.archive.org/web/20250122112513/https://wiki.freepascal.org/IDE_Window%3A_Installed_Packages)_
