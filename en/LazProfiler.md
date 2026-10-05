# LazProfiler

## Contents

  * 1 About
    * 1.1 Screenshot
    * 1.2 Download
    * 1.3 System Requirements / Dependencies
    * 1.4 Installation
    * 1.5 Support page
  * 2 Using LazProfiler
    * 2.1 Start profiling
    * 2.2 Show output
    * 2.3 Cleanup profiler
    * 2.4 Influencing profiling
      * 2.4.1 Hints in source code
      * 2.4.2 Disable instrumenting
  * 3 How does it work?
  * 4 Known issues
  * 5 Version History



## About

LazProfiler is an IDE addon which adds a One-Click-Profiler to Lazarus. 

### Screenshot

[![LP01.png](https://wiki.freepascal.org/images/8/88/LP01.png)](</File:LP01.png>)  
  
[![LP01 2.png](https://wiki.freepascal.org/images/b/b0/LP01_2.png)](</File:LP01_2.png>)

### Download

  * It's available on [GitHub](<https://github.com/PascalRiekenberg/LazProfiler>).



### System Requirements / Dependencies

  * FPC trunk (needs generics and additional PascalParser funktionality) or fixes_3_2
  * Lazarus trunk (revision 60719 and above) or fixes_2_0



### Installation

Download from [here](<https://github.com/PascalRiekenberg/LazProfiler/releases>) and install manually.  
It depends on LCLExtension and EpikTimer. 

### Support page

<http://forum.lazarus.freepascal.org/index.php/topic,38983.0.html>

## Using LazProfiler

### Start profiling

  * Open your project (if not done already)
  * Activate Profiler: 
    * Open Configuration:  
[![LP03.png](https://wiki.freepascal.org/images/2/2b/LP03.png)](</File:LP03.png>)
    * Check "Activate LazProfiler":  
[![LP06.png](https://wiki.freepascal.org/images/7/7a/LP06.png)](</File:LP06.png>)
  * Choose "Profile" from Run menu:  
[![LP02.png](https://wiki.freepascal.org/images/5/59/LP02.png)](</File:LP02.png>)
  * Use your program
  * Close your program
  * study LazProfiler output:  
[![LP01.png](https://wiki.freepascal.org/images/8/88/LP01.png)](</File:LP01.png>)



### Show output

  * Choose "Profiler (Results and Configuration)" from View menu:  
[![LP03.png](https://wiki.freepascal.org/images/2/2b/LP03.png)](</File:LP03.png>)



### Cleanup profiler

In case the instrumenting of the sources produce not compileable code you can reset the profiler and tidy things up by choosing  
"Cleanup Profiler and restore original files" from Run menu:  
[![LP04.png](https://wiki.freepascal.org/images/9/98/LP04.png)](</File:LP04.png>)

### Influencing profiling

#### Hints in source code

By default the profiling automatically starts. If you just want to profile some parts of your code you can surround it by two comments:  

    
    
    // start-profiler
    code_to_profile;
    // stop-profiler
    

#### Disable instrumenting

If you want to exclude procedures/functions/classes/units/packages from beeing instrumented,  
just uncheck the checkbox in front of the procedure/function/class/unit/package name  
in the profiler output window.  
[![LP05.png](https://wiki.freepascal.org/images/3/3a/LP05.png)](</File:LP05.png>)  


![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Packages are only visible if tree is sorted by package  
Units are onyl visible if tree is sorted by package or unit.  


Classes are only visible if tree is sorted by package, unit or class.

## How does it work?

When profiling is started the following steps are done: 

  * project is build to test if project is okay
  * all sources in the projects unit and include directories are backuped (except units below fpc and lazarus source directories)
  * procedures and funtions in units are instrumented with special profiling code
  * project is build with instrumented code
  * instumented sources are deleted and backups are restored
  * program is run with no debugger
  * profiling result is shown when program ends



## Known issues

  * Profiling does not pause counting at ShowModal, ShowMessage, ...



## Version History

  * 0.1.0.0 
    * Initial release
  * 0.1.1.0 
    * leaving sources instrumented if compile after instrumenting fails
  * 0.1.2.0 
    * checking if backup was successfully created before writing instrumented code to original file
  * 0.1.3.0 
    * renamed menu entries
    * include IncludePath. Patch by zamtmn.
    * minor refactoring
    * fixed recognition of declared record in procedure var section
    * fixed memory leaks
  * 0.2.0.0 
    * new Result-TreeView  
\- group by Unit  
\- group by Object
    * fixed include handling
    * last group and sort column and order is saved to settings file now
    * refactoring
  * 0.2.1.0 
    * fixed recursive directory scan
    * do not scan include path
    * ignore units belonging to packages
    * fixed memory leak
  * 0.2.2.0 
    * Update for Lazarus 2.0
  * 0.3.0.0 
    * profiling of packages (only if outside of Lazarus source directory)
    * renamed columns "Net" -> "Σ Net", "Gross" -> "Σ Gross"
    * new columns: "% Net", "% Gross", "Ø Net", "Ø Gross"
    * fixed parsing of operator functions
    * fixed parsing of "START-PROFILER" and "STOP-PROFILER" if inside un-instrumented procedure/function
    * Activation of LazProfiler (default is inactive)

---

_Source: [https://wiki.freepascal.org/LazProfiler](https://web.archive.org/web/20250211184722/https://wiki.freepascal.org/LazProfiler)_
