# Lazarus Examples Window

The new Example Window that exists in Lazarus 3.0 and beyond is documented below. 

## Contents

  * 1 For the end user
    * 1.1 Introduction
    * 1.2 Examples in Packages
    * 1.3 Displaying your own Examples
    * 1.4 FPC Examples
  * 2 For the Developer
    * 2.1 Examples in the Lazarus Source
    * 2.2 Examples in Third Party Packages
    * 2.3 The Metadata File
    * 2.4 Can the IDE find the example ? Debugging
  * 3 Notes
  * 4 See also



## For the end user

### Introduction

A Lazarus user gets to the Example Window after clicking the button on the Project Wizard screen or from the Tools Menu, "Example Projects". In either case you see a window offering a list of known examples. The list can be filtered by key words or by category (double click between category checkboxes turns all off). In basic use, it shows you all the examples shipped with Lazarus. 

The Example Window will allow you to open, build (and edit) and run any example but note some have specific package requirements. When opened, the examples from the Lazarus Source code are copied to a (user writable) area in the Lazarus Config directory ([PCP](<pcp.md> "pcp")) because, on some systems Lazarus Source is in read-only disk space. 

Feel free to edit or make changes to examples in Lazarus Source, if you totally mess up, its easily refreshed from the Examples Window. 

### Examples in Packages

When you install a package that has compliant examples, they will also appear in the Lazarus Examples Window. If they don't appear, the most likely reason is that they do not have an example meta data file and/or an appropriate entry in their .lpk file. See below. 

### Displaying your own Examples

If you have your own examples in one or more Lazarus projects, you can arrange to have them listed along with the built in examples supplied with Lazarus. Following the same rules as below, you would - 

  * Ensure each example is in its own directory and is self contained.
  * Provide meta data files as described below. Perhaps set the category to something like "Private" ?
  * Add an entry to the ./examples/examples.txt file in your Lazarus Source directory. The entry, see the existing ones, should have a relative path from the top of the lazarus source directory to your meta data file. If you are using a packaged version of Lazarus then this examples.txt file might be in read only space and you will need administrative access to it.
  * Examples mentioned in examples.txt are always copied to a work area ensuring write access and allowing you to play without affecting the original.



### FPC Examples

FPC also provides a number of examples relating to its packages and RTL. These examples are generally command line driven and don't have the Lazarus project information so are unsuited to working with the Lazarus Examples Window. (It might be nice to find a way to display them however.) 

## For the Developer

### Examples in the Lazarus Source

You can add an example anywhere you like in the Lazarus Source Tree, there are lots under ~/examples but also many associated with different Lazarus sub system. A few rules apply - 

  * The Example Name must be unique within Lazarus (when lowercased).
  * The example should be self contained within its own directory. That directory (and any sub directories) will be copied to the work area so don't assume particular files are "just up one dir".
  * The example should have an Example Metadata File, with a file name that corresponds to the project name and an extension of .ex-meta. The content of this file is JSON and must have a name, a category and should have a (multiline) description and keywords. See below.



If you add a new example (or remove one) to the Lazarus Source Tree, the file ~/examples/examples.txt needs to be refreshed. Its plain text, edit it directly or generate a new one with this command (on a *nix )- 
    
    
    $> find . -name "*.ex-meta" > examples/examples.txt
    

The file is just plain text, each line specifying an ex-meta file and its path from the top of the Lazarus Source Tree. Windows and Mac users could, easily, edit it by hand. Lazarus does not search its own source tree for ex-meta file, for performance reasons it only looks in examples/examples.txt for source tree examples. 

### Examples in Third Party Packages

Again, you are free to place your package examples directory where you like but in order for the Lazarus Examples Window to find it, some things must be provided, note, different rules than above ! 

  * The package must be currently installed in Lazarus (check ? see if its listed in ([PCP](<pcp.md> "pcp"))/staticpackages.inc)
  * Your example should be in a directory of its own. You should not put more than one example in the same directory.
  * In the case of Package Examples, the example directory is not moved to a work area so it can (but perhaps should not) be dependent on local, relative path files.
  * It must have a Metadata File, see below.
  * The Package file, that is the .lpk file must have entry such as <ExamplesDirectory Value="../demo"/>, typically just below the Author item. The value is the relative path from the .lpk file to a directory containing your example or examples. eg -


    
    
    // directory structure
    [MyPackage]
        [demo]
            mypackage_demo1.ex-meta
            ....
            [demo2]
                mypackage_demo2.ex-meta
                ....
        [package]
            mypackage.lpk
    

(Personally, I would put demo1 in its own directory too but the above would work !) 

Note that, at present, Lazarus, when making a package, does NOT create the ExamplesDirectory entry, it will, but its a work in progress. If you want to test, manually adding that entry is the way to go right now. 

### The Metadata File

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you add a new example (or remove one) to the Lazarus Source Tree, the file ~/examples/examples.txt needs to be refreshed. Its plain text, edit it directly or generate a new one with this command from the top level Lazarus directory (on a *nix )- 
    
    
    $> find . -name "*.ex-meta" > examples/examples.txt
    

All examples that appear in the Example Window have a json metadata file with an extension ".ex-meta" in the top level directory of the example (not the package). Without a valid metadata file, the example will not be listed in the Example Window. An individual example project file would look like this - 
    
    
    { 
    "Laz_Hello" : {
        "Category" : "Beginner",
        "Keywords" : ["Lazarus", "Hello World", "TButton", "TMemo"],
        "Description" : "This might be your first Lazarus project, its the traditional Hello World.  Two buttons have been dropped on the form, renamed and their captions set. A memo was then added and each button was double clicked and what they are expected to do was set."}
    }
    

In this case, the example project files would be in a subdirectory called laz_hello, same spelling but all lowercase consistent with Lazarus. Note that JSOM requires you to escape **double inverted commas** , easier to use single inverted commas. 

The three fields are free form text, there is no dictionary so, you are free to introduce both new Categories and new Key Words. Its desirable that we do not have too many Categories so, perhaps consider posting a message on the forum before using a new one ? At present, the following Categories are in use - 

  * Beginner
  * General
  * TAChart
  * DBase
  * LazReport
  * ThirdParty



There is a rough and ready GUI app to make, edit and importantly, validate metadata files available at <https://github.com/davidbannon/ExampleMetaData> . 

### Can the IDE find the example ? Debugging

If the IDE does not find an example, some things to check for - 

  * Does it have a metadata file ? Nothing happens without a meta data file.
  * Is there a message about the metadata on the **console** ? If there is a syntax problem in the JSON or missing data, there is a message in the console stream when the Examples Window is opened. Linux users can start Lazarus from the console and see that stream. Windows and Mac users need to capture that stream to a file. There are a number of on-line JSON parsers, paste your suspect ex-meta file in one.
  * If its a **Lazarus Source Example** , it must be mentioned in examples/examples.txt. This file, one directory down from the top of the Lazarus tree, lists all the examples in the standard Lazarus install. If its not in there, the Examples Window will not display it. Unix developers can easily regenerate this file with the 'find' command, Windows users will probably edit it by hand. Remember, Lazarus does not search for ex-meta files in its own source tree, it looks in examples/examples.txt
  * If a **Package Example, a third party example** , different rules apply. The ex-meta file must be mentioned in ([PCP](<pcp.md> "pcp"))/staticpackages.inc file and in ([PCP](<pcp.md> "pcp"))/packagefiles.xml file. Unlike the examples.txt file, do not manually edit these files, they should be updated when a package is installed into the the IDE if the package has an appropriate entry in its .lpk file, see above.



## Notes

The user can view the project content and, if they choose, build the project. Later, if the user users the Example Window to again open that project, they will be asked if they want to refresh it, doing so will erase any false edits they may have made. 

Important to note - 

  * Example Projects will not be displayed in the Examples Window unless they have a valid metadata file. An error is dropped to the console if a metadata file with bad json is encountered.
  * The original copy of the Example is not altered when the Example Window is used to open an Example. So, play away !
  * New examples can, potentially be added via the normal Gitlab pull request system.
  * When examples are added to or removed from the Lazarus Source, changes need be made to the (eg) ~/lazarus_3_0/examples/examples.txt file. See the Metadata section.
  * Examples demonstrating aspects of third party packages are probably best added to that package rather than to Lazarus itself.



## See also

  * <https://forum.lazarus.freepascal.org/index.php/topic,57680.0.html>
  * <https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/37509>
  * <https://gitlab.com/dbannon/laz_examples> \- this is just a temp home.
  * <https://gitlab.com/dbannon/laz_examples/-/tree/main/Utility/ExScanner> \- a tool for manipulating Example Projects in and around the Lazarus Src Tree. Probably past its use by date. Also has a framework to 'exercise' the Lazarus Examples Window in a stand alone, easier to manage environment.
  * <https://github.com/davidbannon/ExampleMetaData> \- a very simple editor to make and validate the Example Meta Data files.
  * <https://gitlab.com/freepascal.org/lazarus/lazarus/-/blob/main/components/exampleswindow/uexampledata.pas> the unit making decisions about your examples.

---

_Source: [https://wiki.freepascal.org/Lazarus_Examples_Window](https://web.archive.org/web/20250323040855/https://wiki.freepascal.org/Lazarus_Examples_Window)_
