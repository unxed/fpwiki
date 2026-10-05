# OPM External Update json Editor

## Contents

  * 1 Information and Downloads
    * 1.1 Screenshots
    * 1.2 Purpose
    * 1.3 Installation
      * 1.3.1 Directory Structure
    * 1.4 Making a new OnlinePackageManager update json
    * 1.5 Maintaining a package with OnlinePackageManager and OPM External Update json Editor
    * 1.6 Licence
    * 1.7 Languages
  * 2 Download
    * 2.1 Links



## Information and Downloads

### Screenshots

[![jp mainwindow.jpg](https://wiki.freepascal.org/images/4/49/jp_mainwindow.jpg)](</File:jp_mainwindow.jpg>)

[![jp filemenu.jpg](https://wiki.freepascal.org/images/8/80/jp_filemenu.jpg)](</File:jp_filemenu.jpg>)

[![jp helpmenu.jpg](https://wiki.freepascal.org/images/d/d6/jp_helpmenu.jpg)](</File:jp_helpmenu.jpg>)

* * *

### Purpose

  * To make and maintain package update information for [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager") easily, accurately and quickly.



### Installation

  * There is a Windows Installer and a Zipfile of Linux executables (see #Download below) available.
  * Source code is also available to compile with Lazarus v1.62+ and FPC v3.00+ (best with Laz1.7+/fpc3.11+)



#### Directory Structure

  * OPM External Update json Editor is designed to be deployed in the same folder as your component. 
    * If you maintain multiple components in separate folders, a copy of OPM External Update json Editor should be deployed in the main folder of each one.
    * Each copy of OPM External Update json Editor will maintain its own distinct cfg file in the GetAppConfigDir location.
  * When first started up, OPM External Update json Editor will make the subfolders:


    
    
    - \updates
    - \locale
    

  


  * Suggested structure for your component:


    
    
    - \componentfolder
    - \componentfolder\stable_version
    - \componentfolder\stable_version\updates
    - \componentfolder\stable_version\locale
    

  * \componentfolder - contains the working version of your component code (development source, libs, backups, resources, executables and so on)
  * \componentfolder\stable_version - contains the published version (stable source code only), and also a copy of JsonEditor(.exe)
  * \componentfolder\stable_version\updates - contains the update json and update Zipfile
  * \componentfolder\stable_version\locale - contains the language files for OPM External Update json Editor 
    * This works very well in practise, especially if your project is under version control (svn, github etc)



### Making a new OnlinePackageManager update json

  * Fire up OPM External Update json Editor (jsoneditor[.exe]) and fill in all the fields for your package. 
    * If you use the ellipsis button [...] to locate your Zipfile, OPM External Update json Editor will offer to copy the Zipfile to the \updates folder if it was in a different folder
    * **It is essential that the package lpk name exactly matches the one already installed in[Online Package Manager](<Online_Package_Manager.md> "Online Package Manager")**, otherwise you can choose whatever filenames you like.
  * The first time you use OPM External Update json Editor, a pop-up hint window will show you what each field is for.
  * When you click [Save], if any field needs amending a dialog will tell you what is wrong, and problem fields will highlight. 
    * Only a validated json will be saved to disk.
    * By default the json is named update_<zipfilename>.json and saved to the \updates folder
    * OPM External Update json Editor will offer to Auto-Load the same component info when it is next opened.
  * If you have a multiple components entry in OPM (e.g. Designtime and Runtime), you can make an update_json file for them all (Click [Add] in OPM External Update json Editor) 
    * Each can have its own version number and other characteristics
  * Finally, upload the Zipfile and json file to a place where [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager") can find it. 
    * Please note: After you uploaded the JSON the changes are not immediately visible in the tree. Usually it takes 1-2 min. or less.



### Maintaining a package with OnlinePackageManager and OPM External Update json Editor

  * Make a new update Zipfile and put it into the \updates folder 
    * Pay attention to embedded paths in the zip
    * You can include example/demo source code in the Zip
    * Be sure to prune out anything not needed, like /backup /lib folders and executables etc.
  * Fire up JsonEditor and click File/Load 
    * If Auto-Load was chosen, it will already have loaded the last-saved package info
  * Update the version info for your package(s) thus:



**"DownloadZipURL"** \--> Link to your new, updated package 

**"DisableInOPM"** \--> If you want to temporary disable your package, set this boolean to true. It's useful for maintenance, the package will be grayed out in the tree. The user cannot download/install/update the package. 

**"Version"/"ForceNotify"/"InternalVersion"** \--> To trigger an update message on the users computer, you can either: 

  * Increase the **"Version"** number. Useful when you want to release a new version of the package.
  * Set **"ForceNotify"** to true. Useful after a minor change in your source, that does not require a version change or in case of trunk version. To prevent continous error messages, **"ForceNotify"** must be used in combination with **"InternalVersion"**. After the user updates the package, OPM locally stores the **"InternalVersion"** value. If you want to trigger a new message, increase **"InternalVersion"** with one. If **"ForceNotify"** is true, **"Version"** is always ignored.



The most common scenario is this: 

  * Modify **"DownloadZipURL"**
  * Increase version number when necessary.


  * Click [Save] in OPM External Update json Editor
  * Finally, upload the Zipfile and json file to a place where OPM can find it. 
    * Please note: After you uploaded the JSON the changes are not immediately visible in the tree. Usually it takes 1-2 min. or less.



### Licence

  * OPM External Update json Editor is published under a general GPL license.



### Languages

  * OPM External Update json Editor is available in English or Español. Help/Languages



## Download

  * Windows installer: <https://sourceforge.net/projects/lazautoupdate/files/otherpackages/setup_jsoneditor.exe>
  * Linux Zipfile: <https://sourceforge.net/projects/lazautoupdate/files/otherpackages/linux_jsonpackager.zip>
  * Source code: <https://svn.code.sf.net/p/lazarus-ccr/svn/applications/json_packager/>



* * *

### Links

[Online Package Manager](<Online_Package_Manager.md> "Online Package Manager")

[Minesadorada](</User:Minesadorada> "User:Minesadorada")

---

_Source: [https://wiki.freepascal.org/opmjsonupdateeditor](https://web.archive.org/web/20211201095119/https://wiki.freepascal.org/opmjsonupdateeditor)_
