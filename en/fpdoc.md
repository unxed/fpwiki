# FPDoc Editor

│ **English (en)** │

## Contents

  * 1 Introduction
  * 2 Using the FPDoc Editor
  * 3 Editing FPDoc entries for the LCL sources and Lazarus sources
  * 4 Editing FPDoc entries for the FPC sources, RTL and FCL
  * 5 Future plans
    * 5.1 Already done



## Introduction

FPDoc is the Free Pascal documentation tool which is specifically designed to create help files for individual units of Pascal source code. For more information about FPDoc see here: [Free Pascal documentation tool manual](<http://www.freepascal.org/docs-html/fpdoc/fpdoc.html>). 

[![FPDocEditorDescription.png](https://wiki.freepascal.org/images/6/6c/FPDocEditorDescription.png)](</File:FPDocEditorDescription.png>)

Lazarus provides inline FPDoc help viewer in hints and two help file editors either of which you can use to create or update source code documentation in FPDoc format. If you want to see it correctly formatted, you have to install Package TurboPowerIProDsgn.lpk! The simpler editor, integrated into the Lazarus IDE is called **FPDoc Editor** , and is described on this page. There is also a more complete and complex editor called [LazDE](<Lazarus_Documentation_Editor.md> "Lazarus Documentation Editor"). 

**LazDE** is a bigger brother to **FPDoc Editor** and is the Lazarus Documentation Editor. It is a standalone application (not an integral part of the IDE). It is located at ($LazDir)/doceditor/lazde.lpi. Once you have compiled the lazde project (using Lazarus) LazDE then runs outside the IDE. You can add it into menu Tools as external tool. 

## Using the FPDoc Editor

To use the FPDoc Editor you simply: 

1\. Open the FPDoc Editor. You will find it under the View menu. 

2\. In the Lazarus source code editor move the cursor to some code element. You will notice that the FPDoc Editor's Caption changes to show the selected source element together with the filename of the documentation file. Within FPDoc Editor you can select an appropriate page where you can edit a specific documentation tag. Of course it is also possible to use FPDoc Editor only to view documentation files, without changing them. 

3\. Click on the Create Help button. If you have not yet set up search paths for FPDoc the IDE will ask you where the FPDoc files it generates should be stored. For each of your projects you would normally provide a 'docs' subdirectory. 

4\. Write a short description. 

5\. Click on the Save button to the left or go to the next item (the editor automatically saves a description when you move away from it). 

## Editing FPDoc entries for the LCL sources and Lazarus sources

There are in folder ($LazDir)\docs\xml, You can add this folder to FPDoc paths at Tools / Options / Environment / FPDoc Editor 

## Editing FPDoc entries for the FPC sources, RTL and FCL

The FPDoc entries for the FPC sources can be downloaded from svn: 
    
    
    cd /home/username/yourchoice/
    svn co http://svn.freepascal.org/svn/fpcdocs/trunk fpcdocs
    

Add the path _/home/username/yourchoice/fpcdocs_ to Tools / Options / Environment / FPDoc Editor 

A good item to test FPDoc on is _TComponent.Name_. 

## Future plans

The todo list currently contains the following items, in no particular order: 

  * Write a help editor for topics.
  * Create nicer HTML output for the hint windows.
  * Support Operators
  * Jump to next/previous undocumented identifier



### Already done

  * Extend the link editor to show packages and identifiers
  * Add documentation tags "example" to FPDoc Editor
  * Add documentation tags "topic" to FPDoc Editor
  * Make FPDoc Editor create new elements in documentation
  * Make FPDoc Editor create new documentation files
  * Adding a small toolbar makes it possible to use the available markup tags
  * Make FPDoc Editor work on keydown in source code editor
  * Add FPDoc Editor to IDE settings (showing and position in IDE)
  * Make it work for fpc sources (rtl files already exist)
  * Add settings to environment menu
  * Make it work on project files also
  * Propose to expand documentation tags with: "todo" and "notes" (no need for that, as there are alternatives)
  * Reduce overhead even further
  * All source elements are interpreted by FPDoc Editor using codetools
  * Find inherited entries. For example TControl.Align of TButton.Align
  * Optimization: inherited Entries are parsed on idle
  * Optimization: xml files are cached, and only parsed once or if they changed on disk
  * Add a HTML viewer. This is available by installing the turbopoweriprodsgn package
  * Checks for invalid xml tags and auto repairs them

---

_Source: [https://wiki.freepascal.org/fpdoc](https://web.archive.org/web/20230130095329/https://wiki.freepascal.org/fpdoc)_
