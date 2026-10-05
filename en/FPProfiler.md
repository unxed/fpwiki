# FPProfiler

## Contents

  * 1 Introduction
    * 1.1 FPP
      * 1.1.1 FPPInsert
      * 1.1.2 FPPRemove
    * 1.2 FPPView
    * 1.3 LazProf
    * 1.4 Add or Remove fpprof calls to functions via codetools
  * 2 Getting the sources
  * 3 Installing
  * 4 Platforms / architectures
  * 5 Reporting bugs / feature requests
  * 6 See also



## Introduction

The Free Pascal Profiler is an effort to create an FPC native profiler, deployable on all platforms and architectures the Free Pascal compiler itself supports. It was initially developed by Darius Blaszyk. 

Free Pascal Profiler consists of two tools (fpp and fppview) and some units containing the profiling code that needs to be linked in. 

### FPP

To compile your project, you pass the same parameters to FPP as you would to FPC. FPP then scans your source code and inserts profiling code in each begin..end block. It also amends the uses clause to link in the fpprof unit. Once done, it compiles your project, which now includes the profiling code. After that, it restores your units to their original state. 

#### FPPInsert

This tool will only insert profiling code, nothing else. It can also do this recursively on one or more folders at a time. 

#### FPPRemove

This tool will only remove profiling code, nothing else. It can also do this recursively on one or more folders at a time. 

### FPPView

When you run your program that contains the profiling code, it generates an xml file containing all the profiling information. FPPView reads this xml file and generates reports from it. Call counts, timing and call graphs are supported at the moment. 

### LazProf

A graphical frontend for Lazarus exists. For now it only is able to analyse the log files, but future versions will also enable to compile the application directly with fpp and analyse the results. 

Sample screenshots: 

[![Fpprofiler flatreport.png](https://wiki.freepascal.org/images/8/80/Fpprofiler_flatreport.png)](</File:Fpprofiler_flatreport.png>) [![Fpprofiler callgraph.png](https://wiki.freepascal.org/images/5/56/Fpprofiler_callgraph.png)](</File:Fpprofiler_callgraph.png>) [![Fpprofiler memorychart.png](https://wiki.freepascal.org/images/e/e2/Fpprofiler_memorychart.png)](</File:Fpprofiler_memorychart.png>)

### Add or Remove fpprof calls to functions via codetools

There is a small command line utility that adds or removes fpprof calls to start and end of functions. See Lazarus soures 
    
    
    components/codetools/examples/addfpprofcalls.lpr
    

## Getting the sources

You can get the latest sources from SVN here: 
    
    
     svn checkout <http://svn.freepascal.org/svn/fpcprojects/fpprofiler>
    

Alternatively Graeme Geldenhuys has created a git repository: 
    
    
     <http://github.com/graemeg/fpprofiler>
    

This repository is an unofficial one. The SVN repository is therefore advised. 

You can view the SVN repository online through this URL: 
    
    
     <http://svn.freepascal.org/cgi-bin/viewvc.cgi/fpprofiler/?root=fpcprojects>
    

## Installing

Currently building the tools from commandline is broken. To build them you should use Lazarus. Start by opening the lazarus project files: 
    
    
     .\fpp\fpp.lpi
     .\fpp\fppinsert.lpi
     .\fpp\fppremove.lpi
     .\fppview\fppview.lpi
    

and simply build the tools from the IDE. The executables will be placed in the directory: 
    
    
     .\bin\$(TargetCPU)-$(TargetOS)\
    

To install the lazarus package, open the package file in Lazarus called: 
    
    
     .\lazprof\lazprof.lpk
    

Click on compile and then install. Lazarus will be rebuilt and additionally the package will register two menu items under: 
    
    
     View > Profile viewer
    
    
    
     _Run > Run with profiling enabled_
    

The latter command is still under development, so don't use it. 

## Platforms / architectures

Because it is completely FPC native, it should work on all platforms and architectures that FPC supports. 

Tested platforms/architectures: 

  


Platform | Architecture   
---|---  
Linux  | i386   
Windows  | i386   
Mac OS X  | i386   
  
  
People are encouraged to add their platform/architecture here if not listed. 

## Reporting bugs / feature requests

If you find a bug and would like to report it, or you have an idea to expand fpprofiler, please report the issue in [mantis](<http://bugs.freepascal.org/set_project.php?project_id=10;11>) and drop a note on the FPC-devel mailinglist. This ensures the issue does not get forgotten. Of course, when a patch is provided, the issue is likely to be resolved much quicker. It's also highly recommended to provide a test program with the reported issue. This program will be added to the test suite to ensure no regressions will go unnoticed. 

## See also

  * FPC [Profiling](<Profiling.md> "Profiling") wiki page
  * Delphi sampling profiler [[1]](<http://delphitools.info/samplingprofiler/>)
  * GpProfiler [[2]](<http://17slon.com/gp/gpprofile/index.htm>) [[3]](<http://code.google.com/p/gpprofile/>)
  * Asmprofiler [[4]](<http://code.google.com/p/asmprofiler/>)

---

_Source: [https://wiki.freepascal.org/FPProfiler](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/FPProfiler)_
