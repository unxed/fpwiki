# Install Packages

│ **English (en)** │  **[русский (ru)](<../ru/Install_Packages.md>)** │

## Contents

  * 1 Adding packages using the IDE
    * 1.1 Adding known packages
    * 1.2 Adding new packages
    * 1.3 New versions
  * 2 Compile an IDE with packages at command line
  * 3 Questions
  * 4 See also



## Adding packages using the IDE

### Adding known packages

Open "Package -> Install/Uninstall Packages". The list on the left shows already installed packages or packages marked for installation (which have a green plus sign in their icon). The list to the right shows packages that you can install. Simply double-click on a package in either list to move them to the other - and mark them for installation/uninstall. 

Finally, click "Save and Rebuild" to rebuild the IDE and the packages. 

You can also compile and install packages the IDE knows nothing about. See below. 

### Adding new packages

The package should have come with a number of source files, especially one with a .lpk extension. To install this package one goes to "Package -> Open Package File (.lpk)". Navigate to the folder containing the .lpk file and open it. If everything works out as planned you will see the Package Manager. The Package Manager shows a treeview with the files in the package and the required packages. 

If you don't want to install the component into the IDE, e.g. because it is a non graphical component, press **Compile**. Otherwise you should press **Use >>** and then choose **Install**. Next we get a warning about Lazarus only supporting static linked libraries and the question if you want to rebuild Lazarus. Just press Yes and sit back. Lazarus will be rebuilt, and depending on your configuration restarted. 

If the package was a graphical component, and you selected the **Use >>** option, you should now have a new tab in your IDE by which you can add that component to your project. 

### New versions

When receiving a new source version of an already installed package, you must 

  1. overwrite the previous source version with the new version
  2. either open the new .lpk and press **Use >>** and then choose **Install** as above,  
or rebuild the IDE using the menu "Tools" -> "Build Lazarus with Profile [...]" if you want more control on how the package is compiled.



## Compile an IDE with packages at command line

You can use [lazbuild](<lazbuild.md> "lazbuild") to compile and install your package, and add --build-ide to rebuild the IDE. 

## Questions

If you have any questions, go to the lazarus [forum](<http://www.lazarus.freepascal.org/index.php?name=PNphpBB2&file=index>) or post it to the Lazarus mailing list. 

## See also

  * [Lazarus Packages](<Lazarus_Packages.md> "Lazarus Packages")
  * [Install IDE packages without the IDE](<Install_IDE_packages_without_the_IDE.md> "Install IDE packages without the IDE")

---

_Source: [https://wiki.freepascal.org/Install_Packages](https://web.archive.org/web/20250404185440/https://wiki.freepascal.org/Install_Packages)_
