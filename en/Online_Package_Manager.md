# Online Package Manager

│ **English (en)** │  **[русский (ru)](<../ru/Online_Package_Manager.md>)** │

## Contents

  * 1 About
    * 1.1 Screenshot
    * 1.2 License
    * 1.3 Download
    * 1.4 System Requirements / Dependencies
    * 1.5 Installation
    * 1.6 Support page
  * 2 Using the package manager
    * 2.1 Getting started
    * 2.2 Filter the tree/Locate a package
    * 2.3 Operation with packages
      * 2.3.1 Refresh package list
      * 2.3.2 Download package
      * 2.3.3 Install a package
      * 2.3.4 Update a package
      * 2.3.5 Difference between download/install/update
      * 2.3.6 Cleanup local repository
      * 2.3.7 Create
        * 2.3.7.1 Create repository package
        * 2.3.7.2 Create JSON for updates
      * 2.3.8 Options
    * 2.4 Create a private repository
  * 3 see also



## About

Online Package Manager(OPM) is a tool that automates the process of installing, upgrading, configuring Lazarus packages. The packages are stored in the main repository(see repository column) as zip files, along with a [JSON](<JSON.md> "JSON") file. The JSON contains all necessary information about the packages. On request the packages are downloaded/extracted/compiled/installed into the IDE. If available, it's also possible to update a previously installed package from the maintainer webpage(see update column). There's also a web version of the Lazarus Packages Repository available here: <http://packages.lazarus-ide.org/>. 

**Please note:**

OPM reads the package description from the lpk file. I can understand that the description of many packages is very minimalistic or even non-existent, but there is nothing I can do about it. It's not OK to modify package description or any other property for that matter, without the consent of the package developer. More over testing each and every package, then write a detailed description is not realistic. 

### Screenshot

[![OPM6.png](https://wiki.freepascal.org/images/6/6e/OPM6.png)](</File:OPM6.png>)

[![OPM7.png](https://wiki.freepascal.org/images/b/b7/OPM7.png)](</File:OPM7.png>)

### License

[GNU General Public License](<https://opensource.org/licenses/gpl-license>)

### Download

Online package manager is part of Lazarus sources, in directory ($LazarusDir)/components/onlinepackagemanager. 

### System Requirements / Dependencies

  * FPC 3.0.0 or newer
  * Lazarus 1.8 or newer
  * You can download an older version for the 1.6 series: [OPM 1.6](<https://drive.google.com/open?id=0B9Me_c5onmWobVZOdVlXSlZRRmc>)
  * These versions were available at the time of writing this update.
  * Tested on the following widgeset: win32/64, gtk2, carbon, qt(win)
  * The package manager does not depend on any external package



### Installation

In Lazarus "Install/Uninstall Packages" window select "OnlinePackageManager" from the available packages, click "Install selection" and then rebuild the IDE. 

### Support page

<http://forum.lazarus.freepascal.org/index.php/topic,34297.0.html>

## Using the package manager

### Getting started

Go to Lazarus Menu-->Package-->Online Package Manager(Shortcut key: `Ctrl`+`Alt`+`⇧ Shift`+`O`). On startup the package manager will automatically download a list with available repository packages, the list is displayed in a tree(see screenshots above). 

### Filter the tree/Locate a package

[![OPM3.png](https://wiki.freepascal.org/images/b/b3/OPM3.png)](</File:OPM3.png>)

You can filter the package list by: 

  * Packagename
  * Package file(.lpk)-->a repository package can contain multiple lpk files
  * Package category
  * Package status
  * Version
  * Description
  * Author
  * Lazarus compatibility
  * FPC compatibility
  * Supported widget sets
  * Packagetype
  * Dependencies
  * License



### Operation with packages

[![OPM4.png](https://wiki.freepascal.org/images/a/a5/OPM4.png)](</File:OPM4.png>)

#### Refresh package list

Press the "Refresh" button to update the package list. 

#### Download package

Check one or more package then press the "Download" button. This will download/extract the packages to a preselected directory. Nothing gets installed. Useful when you want to install the packages manually or study their sources without installing. 

#### Install a package

Check one or more package then press the "Install" button. The package manager will automatically download/extract/compile/install the selected packages(from the main repository), then rebuild the IDE if necessary(Designtime/Designtime_and_runtime packages). Confirm the rebuild by clicking the "Yes" button. Please note: The compile feature is only available in Lazarus 1.7+. 

#### Update a package

Check one or more package then press the "Update" button. The package manager will update the selected packages from the maintainer's homepage(if available). The package must be installed first, a direct update although is possible, it's not allowed to prevent unresolved dependency error. **Please note:** downloading/installing packages from external link is not without a risk. Only update the package if you trust the package maintainer. 

#### Difference between download/install/update

Download/Install --> Operations between you and the main repository. 

Update --> Operations between you and the package maintainers. 

[![OPM10.png](https://wiki.freepascal.org/images/6/61/OPM10.png)](</File:OPM10.png>)

Video link: [Difference between download/install/update](<https://www.youtube.com/watch?v=wiN2GoCvgjc>)

#### Cleanup local repository

This operation will delete all non-installed packages/archives from the local repository. To cleanup the local repository press the "Cleanup" button. 

#### Create

_This section is for package maintainers. First you must go to Options-- >Profiles and select package maintainer._

##### Create repository package

To create a repository package press "Create repository package" menu item, then: 

  * select the package main directory(package manager will recursively search for all package files inside the folder)
  * select the repository package(root node) 
    * choose category(obligatory)
    * type display name(optional)
    * home page(optional)
    * update link(optional, for more details see "Create JSON for updates"[[1]](<Online_Package_Manager.md>)" below)
    * create JSON for updates(optional), has the same functionality as "Create JSON for updates"[[2]](<Online_Package_Manager.md>)" dialog. It was also added here to speed up the process.



[![OPM15.png](https://wiki.freepascal.org/images/6/6c/OPM15.png)](</File:OPM15.png>)

  * check each package you wish to include, adjust package info if necessary(lazarus/fpc compatibility, supported widgetset, etc)



[![OPM16.png](https://wiki.freepascal.org/images/6/6d/OPM16.png)](</File:OPM16.png>)

  * press the create button, if all goes well you should get a zip and a json file. Upload somewhere the files and send the link to opm@lazarus-ide.org or
  * press the submit button, the files are directly uploaded to a remote server.



Every package will be accepted after a short check(malware, license, etc). Multiple versions of the same package is also allowed. 

##### Create JSON for updates

To allow the user to update the package directly from your webpage, follow this steps: 

  * Make sure your package is already in the main repository(see previous section)
  * You can download and use the [Online Package Manager External Update json Editor](<OPM_External_Update_json_Editor.md> "OPM External Update json Editor")
    * or via OnlinePackageManager:


  * Check the package, press the "Create JSON for upates" menu item then create the JSON.



[![OPM11.png](https://wiki.freepascal.org/images/d/d1/OPM11.png)](</File:OPM11.png>)

  * Edit the JSON, upload to your webpage, then send me the link.



The JSON will look like this: 

[![OPM12.png](https://wiki.freepascal.org/images/0/09/OPM12.png)](</File:OPM12.png>)

Editable items in the JSON: 

**"DownloadZipURL"** \--> Link to your new, updated package 

**"DisableInOPM"** \--> If you want to temporary disable your package, set this boolean to true. It's useful for maintenance, the package will be grayed out in the tree. The user cannot download/install/update the package. 

**"Version"/"ForceNotify"/"InternalVersion"** \--> To trigger an update message on the users computer, you can either: 

  * Increase the **"Version"** number. Useful when you want to release a new version of the package.
  * Set **"ForceNotify"** to true. Useful after a minor change in your source, that does not require a version change or in case of trunk version. To prevent continous error messages, **"ForceNotify"** must be used in combination with **"InternalVersion"**. After the user updates the package, OPM locally stores the **"InternalVersion"** value. If you want to trigger a new message, increase **"InternalVersion"** with one. If **"ForceNotify"** is true, **"Version"** is always ignored.



The most common scenario is this: 

  * Generate the JSON
  * Modify **"DownloadZipURL"**
  * Increase version number when necessary.



Please note: After you edited the JSON the changes are not immediately visible in the tree. Usually it takes 1-2 min. It also depends on the users setting(Options-->General-->Check for package updates). 

#### Options

To display the options dialog press the "Options" button. 

### Create a private repository

In order to create a private repository please do the following: 

1\. First make sure profile "Package maintainer" is enabled in Options-->Profiles, then open the private repository form(see screenshot below) 

[![OPM17.png](https://wiki.freepascal.org/images/b/b0/OPM17.png)](</File:OPM17.png>)

  


2\. Press the "Create" button. Enter repository Name, Address(optional), Description(optional). Save the repository to an empty folder 

[![OPM18.png](https://wiki.freepascal.org/images/c/c7/OPM18.png)](</File:OPM18.png>)

  


  * If filled, the repository address(in this case <http://localhost/packages/>) is automatically added to available repository list(Options form)



[![OPM19.png](https://wiki.freepascal.org/images/8/8e/OPM19.png)](</File:OPM19.png>)

  


3\. Start to add/delete packages. You have two choices: 

a. Create a new package via [http://wiki.freepascal.org/Online_Package_Manager#Create_repository_package](<Online_Package_Manager.md>)

b. Add a previously saved package 

[![OPM20.png](https://wiki.freepascal.org/images/5/5b/OPM20.png)](</File:OPM20.png>)

  
4.You can also open a previously saved repository. The last repository is automatically opened on startup. 

5.Upload the files(packagelist.json, *.zip) to your local or remote server. 

6.Make sure the server is running, the change "Remote repository" in Options-->General 

## see also

  * [OPM video about the install process](<https://www.youtube.com/watch?v=IRJkyiOn3AA>)

---

_Source: [https://wiki.freepascal.org/Online_Package_Manager](https://web.archive.org/web/20250208183816/https://wiki.freepascal.org/Online_Package_Manager)_
