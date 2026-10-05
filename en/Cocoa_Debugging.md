# Cocoa Debugging

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Having problems running a project compiled for Cocoa? Here are steps to try 

## Contents

  * 1 Run from Terminal
    * 1.1 Console App
  * 2 Run with lldb debugger
  * 3 Troubleshooting
  * 4 Creating a Minimal Sample Project
  * 5 See also



## Run from Terminal

  * open up a Terminal (Applications -> Utilities -> Terminal).
  * cd to your project directory
  * cd to the bundle (a bundle is a specially structured directory where binary resides. The bundle directory extension is ".app" )


    
    
     cd project1.app/Contents/MacOS
    

(for your project "project1.app" needs to be changed to your project's name) 

  * run the project


    
    
    ./project1
    

See the output! The output might suggest what's going on. Any OS or error warning messages should be shown. 

### Console App

The log entries could also be acquired from Console utillity. 

## Run with lldb debugger

Repeat the same steps as in "Run from Terminal", but run the program as 
    
    
    lldb project1
    

(loading of lldb requires admin rights, and well as takes some time) once loaded enter command 
    
    
    r
    

and wait for it to stop or catch the application crash. See more [here](<lldb.md> "lldb")

## Troubleshooting

  * if a project can compile and run for Carbon, yet it cannot do the same for Cocoa, then the issue is likely with Cocoa widgetset. Creating a minimal sample project is suggested
  * if a project runs but does nothing and provides a constant CPU load (i.e. 99%, 50% or 25%, depending on the number of cores), then there's an endless loop triggered by some code.



## Creating a Minimal Sample Project

It's likely you would encounter a problem when working with a big production project. 

It's likely you might have some closed-source code that you're not allowed to share. 

It's preferred to have a simple project demonstrating the problem. "Simple" meaning: 

  * it's small in size one (or two) forms only.
  * minimal set of controls
  * NO external package dependencies (packages supplied with Lazarus are ok)



In order to get a simple project from the big production project you can do the following: 

  * copy the project stand-alone (not to harm the existing code base)
  * remove (delete) pieces that are not related to the bug



    

  * (i.e. if you're having problems with placement of a control, you might want to remove main menu, TActions, if any)
  * remove bigger pieces first (it might be much easier than you think)



  * after each piece is removed, run the project, to make sure you're still getting the error.
  * keep removing code (and reference to any other external dependencie) until you reach the point that you cannot remove anything without affecting the bug.



At this point you should have small enough project to demonstrate the problem and [bug report it](<How_do_I_create_a_bug_report.md> "How do I create a bug report"). 

## See also

  * [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report")
  * [lldb](<lldb.md> "lldb")
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface")
  * [Apple: Appendix A: LLDB for GDB Users – Command Summary](<https://developer.apple.com/library/archive/documentation/General/Conceptual/lldb-guide/chapters/A3-GDB-Summary.html>)

---

_Source: [https://wiki.freepascal.org/Cocoa_Debugging](https://web.archive.org/web/20240912152524/https://wiki.freepascal.org/Cocoa_Debugging)_
