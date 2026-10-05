# Lazarus Documentation Editor

│ **[Deutsch (de)](</Lazarus_Documentation_Editor/de> "Lazarus Documentation Editor/de")** │  **English (en)** │  **[español (es)](</Lazarus_Documentation_Editor/es> "Lazarus Documentation Editor/es")** │  **[français (fr)](</Lazarus_Documentation_Editor/fr> "Lazarus Documentation Editor/fr")** │  **[日本語 (ja)](</Lazarus_Documentation_Editor/ja> "Lazarus Documentation Editor/ja")** │  **[polski (pl)](</Lazarus_Documentation_Editor/pl> "Lazarus Documentation Editor/pl")** │  **[русский (ru)](<../ru/Lazarus_Documentation_Editor.md> "Lazarus Documentation Editor/ru")** │  **[slovenčina (sk)](</Lazarus_Documentation_Editor/sk> "Lazarus Documentation Editor/sk")** │    
****

## Contents

  * 1 Introduction
  * 2 The Rationale
  * 3 The task of documenting Lazarus is underway
    * 3.1 The Lazarus Documentation Tool
    * 3.2 Documenting the LCL
  * 4 The Outcome
  * 5 Adding documentation for new units
  * 6 The Final result
  * 7 Submitting your work
  * 8 Improvements welcome



## Introduction

Good, comprehensive documentation is an important missing part of Lazarus. To help produce the needed documentation a tool called LazDE has been developed. This page describes the use of this tool to edit existing documentation or to create new documentation files. In this wiki document I use [$LazDir] to denote the Lazarus base directory. So when you read [$LazDir] simply replace it with the directory appropriate for your Lazarus installation. 

## The Rationale

The primary goal of this documentation project is to make available an online help file. To make the help file platform-independent we use XML files to record and store documentation data. Each Lazarus unit has its own XML documentation data file. 

A large number of such XML files have already been written, and they can be used: 

  * to create HTML pages you can access at <http://lazarus-ccr.sourceforge.net/docs/lcl/>
  * to create CHM files that you can browse offline (once placed in a correctly registered local directory) as help for Lazarus that is available even when you have no internet access.



## The task of documenting Lazarus is underway

As mentioned above, XML was chosen as the documentation data format since this is platform-independent. The current documentation can be found in [$LazDir]/docs/xml/. So far the /lcl/ directory contains mainly empty skeleton files, which contain little (or no) useful documentation. The XML files you will find in this directory are auto-generated and need to be adapted to be useful. So now we know where to find the files, let's look at the tool we're developing to create and adapt the documentation data files. 

### The Lazarus Documentation Tool

"Lazde" is a tool designed to edit the XML documentation files. It can also be used to generate the basic XML file skeleton derived from a Pascal unit source file and (by means of the external tool FPDoc) to generate an HTML version of the documentation. An example of FPDoc output can be seen [here](<http://lazarus-ccr.sourceforge.net/docs/lcl/index.html>), a part of the documentation so far. Since compiled executable versions of lazde are not provided, you have to build one for yourself. The sources for "lazde" can be found in [$LazDir]/doceditor/. When you build and run this program, if you open [$LazDir]/docs/xml/lcl/dialogs.xml you will be presented with this screen 

[![Lazdemain.png](https://wiki.freepascal.org/images/a/a8/Lazdemain.png)](</File:Lazdemain.png>)

### Documenting the LCL

[![Lazdeelements.png](https://wiki.freepascal.org/images/9/90/Lazdeelements.png)](</File:Lazdeelements.png>)

Opening the "Packages" node shows the "LCL" node which contains a "Dialogs" node. Selecting this last node fills the lower treeview with elements. These elements list the Constants, Types and Classes defined in the Dialogs.pp unit. Any units used by dialogs.pp are also added as further nodes. Other nodes are added for the properties of a class, the parameters of functions and procedures, and so on. 

Selecting a node in the lower left treeview displays the content of that node on the right side of the window. 

When you look at [this page](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/index-4.html>) you see an overview of classes defined in the "dialogs" unit. After each class you see a line of text which is whatever was entered in lazde's "short" editbox. 

Below the "short" editbox, there is a memo field where you can enter a longer description of the component or property. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you want to insert a line break use <br/>

Below the main description field are further fields labelled "Errors", "See also" and "Example code File". 

"Errors" is used to give information about any errors raised by a function if parameters have values that are out of range. For an example see [this page](<http://lazarus-ccr.sourceforge.net/docs/rtl/dateutils/recodeyear.html>). 

The "See also" and "Example code File" fields are each provided with three buttons ([+], [^], [-]). These buttons enable you to add and remove links to other pages or code examples respectively. 

## The Outcome

Have a look at the description of [InputQuery](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/inputquery.html>). At the top of the page you can see what the page is about (in this case the InputQuery function from dialogs.pp). The first line of text is what has been entered in the "Short" editbox in lazde. 

Next is the declaration of this function taken from the source file dialogs.pp. The declaration part is created by the html-builder and is parsed directly from the sources. Because there are two versions of this function the source position says 0. 

Then the arguments are given. When you open the InputQuery node in lazde you will see each argument separately as a child node. The text shown after each argument corresponds to whatever was entered in the "Short" editbox when the respective child node was selected. 

The function result is shown as a fourth paragraph. The text shown here has been entered in the same way as for the function's arguments. 

Lastly the description is shown. This is the text entered in the description box of lazde. 

## Adding documentation for new units

There are of course many other units waiting to be documented. If the unit is part of a package, it is likely that there is no xml file for you to start from. You will then have to start from scratch, and lazde has a function to get you off to a flying start. Just go to File -> New and the following screen appears: 

[![Lazdenewdocfromfile.png](https://wiki.freepascal.org/images/f/f2/Lazdenewdocfromfile.png)](</File:Lazdenewdocfromfile.png>)

You start by naming the package. All units you want to add to this package should be given the same package name. Afterwards you enter the source file to use. You can browse to this file as well. Then enter a name for the output file - do not forget the xml extension! - and click OK. 

lazde will then generate the skeleton documentation. The generated file will be opened and the treeview will be populated with all the units, classes, types, functions and so on from your source file. Now you are ready to start documenting a new part of Lazarus. 

Have a look at [LCL Documentation Roadmap](<LCL_Documentation_Roadmap.md> "LCL Documentation Roadmap") to see which units still need to be documented. 

You can use [FPDoc Updater](<FPDoc_Updater.md> "FPDoc Updater") to easily update FPDoc files when Pascal units have been changed. 

## The Final result

What I experienced during the use of the program is that I would like to see how the information is shown in its final stage (as a browsable document). For this purpose lazde makes use of a utility to build all necessary HTML-files. 

This utility can be started from the menu Extra -> Build. The following screen will be shown: 

[![Lazdebuild1.png](https://wiki.freepascal.org/images/2/27/Lazdebuild1.png)](</File:Lazdebuild1.png>)

"Package" should be the same as the name you gave when you created the xml files. For "Format" choose HTML. At "Output" you enter the path where the resulting files should be placed. Press "Add all" and all documents your were working on will be added to the project. Then go to the next to tab 

[![Lazdebuild2.png](https://wiki.freepascal.org/images/0/01/Lazdebuild2.png)](</File:Lazdebuild2.png>)

and enter the paths to the source files. After you have pressed build your HD will start to rattle and finally the following output will be shown on the "Build output" tab. 
    
    
    Building docs using command: fpdoc --package="LCL"
    --output="/home/matthijs/documentatie/LCL" --format=html --content 
    --descr="/home/matthijs/Projecten/Lazarus/doceditor/buttons.xml"
    --descr="/home/matthijs/Projecten/Lazarus/doceditor/comctrls.xml"
    --descr="/home/matthijs/Projecten/Lazarus/doceditor/dialogs.xml"
    --descr="/home/matthijs/Projecten/Lazarus/doceditor/controls.xml"
    --input="/home/matthijs/cvsroot/lazarus/lcl/buttons.pp"
    --input="/home/matthijs/cvsroot/lazarus/lcl/comctrls.pp"
    --input="/home/matthijs/cvsroot/lazarus/lcl/dialogs.pp"
    --input="/home/matthijs/cvsroot/lazarus/lcl/controls.pp" 
    
    FPDoc - Free Pascal Documentation Tool
    (c) 2000 - 2003 Areca Systems GmbH / Sebastian Guenther, sg@freepascal.org
    
    Writing 2788 pages...
    Done.
    Documentation successfully built.
    

When you go to the directory you entered at "Output" you will see a index.html file and (in this case 4) sub directories. Open index.html in your favorit browser and see the result of all your hard work on the documentation. You will be able to follow the links and read it all. 

When you plan to continue working on this package of documentation, press "Save" and save the build options. You will be asked to provide a name for the file and the options will be saved. Next time you want to build the HTML-files you can just "load" them again. 

## Submitting your work

When you are satisfied with your work on Lazarus' documentation, you certainly want to share it with the Lazarus community. All you have to do is make a [patch](<Creating_A_Patch.md> "Creating A Patch"), zip the patch and send it in. 

## Improvements welcome

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** lazde is a work-in-progress. It does work, but it's certainly not completely finished yet. You may encounter bugs. Feel free to fix them!

---

_Source: [https://wiki.freepascal.org/Lazarus_Documentation_Editor](https://web.archive.org/web/20240920204108/https://wiki.freepascal.org/Lazarus_Documentation_Editor)_
