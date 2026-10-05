# heaptrc

│ **English (en)** │  **[русский (ru)](<../ru/heaptrc.md> "heaptrc/ru")** │    
****

**`Heaptrc`** is a unit that can be used to debug allocation and deallocation of memory blocks. It keeps track of calls to [`GetMem`](<https://www.freepascal.org/docs-html/rtl/system/getmem.html>)/[`FreeMem`](<https://www.freepascal.org/docs-html/rtl/system/freemem.html>) calls, and, implicitly, of [`New`](<https://www.freepascal.org/docs-html/rtl/system/new.html>)/[`Dispose`](<https://www.freepascal.org/docs-html/rtl/system/dispose.html>) statements. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Do not add the `Heaptrc` unit manually. The `Heaptrc` unit needs to be loaded before `Lineinfo` and only the compiler can do that. `Heaptrc` is also a memory manager, so do not try to use any memory manager – including `Heaptrc` itself – in the [`uses` clause](<Uses.md> "Uses"), because that may lead to memory corruption and false results. This topic contains a note with example code to handle such a case in a generic way.

When a program using Heaptrc ends, Heaptrc can write out the total memory used and a list of allocated but not freed blocks to a file. When run in the console (*nix or Windows), Heaptrc will print this output to screen unless otherwise instructed. In Lazarus GUI programs on Windows, the output will appear in numerous small dialog boxes, which may be very unpractical, although you can redirect output to file. 

[![Standard output of Heaptrc on Windows \(despite of the title there is no error in the application\)](https://wiki.freepascal.org/images/7/79/Win_heaptrc_output_no_error.png)](</File:Win_heaptrc_output_no_error.png> "Standard output of Heaptrc on Windows \(despite of the title there is no error in the application\)")

On *nix (including BSD, Linux and macOS), by default, nothing will be shown for GUI programs. In this case, to see heaptrc output when you terminate your program (GUI or non-GUI), open "Console In/Output" (Ctrl+Alt+O or View -> Debug Window -> ...) 

See [leakview](<leakview.md> "leakview") for details on how to make use of Heaptrc effectively. 

## Contents

  * 1 Usage
    * 1.1 Using Heaptrc in FPC
    * 1.2 Using Heaptrc in Lazarus
    * 1.3 Show report only on leak
    * 1.4 Programmatically disable report
    * 1.5 Continue execution even on heap error
    * 1.6 Redirect report to file
    * 1.7 Why Heaptrc should not be added to the uses clause manually
  * 2 See also



## Usage

### Using Heaptrc in FPC

Add a parameter `-gh` to your compilation command line or to `fpc.cfg`. 
    
    
    fpc -gh Helloworld.pas
    

or usually combined with line info: 
    
    
    fpc -glh Helloworld.pas
    

This will add Heaptrc implicitly as a hidden first unit of the program's uses clause. 

You can test in code if Heaptrc is active by examining the global constant [**`UseHeapTrace`**](<https://www.freepascal.org/docs-html/rtl/heaptrc/useheaptrace.html>) for the presence of Heaptrc and its status like so: 
    
    
    program PossiblyHeaptraced(Input, Output, StdErr);
    begin
    	{$if declared(UseHeapTrace)}
    	WriteLn('Heaptrc is used.');
    	// Heaptrc reports can be turned off when linked in... so true or false
    	WriteLn('Heaptrc is active: ', UseHeapTrace); 
    	// you can subsequently test/set any Heaptrc reporting options
    	{$else}
    	WriteLn('No trace of Heaptrc.');
    	{$endIf}
    end.
    

### Using Heaptrc in Lazarus

To enable this in your Lazarus project: 

  1. Go to _Project Options/Linking_ and
  2. in the Debugging section enable _Use Heaptrc unit (check for mem-leaks) (-gh)_



You can test for the presence of `Heaptrc` in a similar manner as in the above example by testing for the presence of `UseHeapTrace` in your Project.lpr file. 
    
    
    {$if declared(UseHeapTrace)}
    // ... do something
    
    // test if active
    if UseHeapTrace then
    begin
    	// ...
    end;
    {$endIf}
    

### Show report only on leak

If you want to show `Heaptrc` report dialog only if there were leaks in your application, then put this [assignment](<Becomes.md> "Becomes") somewhere in your main project source file: 
    
    
    {$if declared(UseHeapTrace)}
    GlobalSkipIfNoLeaks := True; // supported as of debugger version 3.2.0
    {$endIf}
    

### Programmatically disable report

If you want to disable report from your code then you can directly manipulate `UseHeapTrace` constant at program startup: 
    
    
    UseHeapTrace := False;
    

This will in effect change the behavior to a normal memory manager. 

### Continue execution even on heap error

If you want your application to continue execution even in case of a heap error then manipulate [`HaltOnError` constant](<https://www.freepascal.org/docs-html/rtl/heaptrc/haltonerror.html>) at program startup: 
    
    
    HaltOnError := False;
    

### Redirect report to file

If you want to redirect leak report to file then add this to beginning of you main project file: 
    
    
    SetHeapTraceOutput('Trace.log'); // supported as of debugger version 3.2.0
    

### Why Heaptrc should not be added to the uses clause manually

You should never add Heaptrc manually in a normal program, because the Heaptrc unit needs to be loaded before Lineinfo for debugging to work properly. 

Decoding dwarf debugging info efficiently requires a working heap manager and this means that Heaptrc needs to be loaded before the the [DWARF](<DWARF.md> "DWARF") line info decoding unit which is loaded by -gl, so Heaptrc cannot be used explicitly. 

Only the compiler itself can insert the unit in its proper place as a hidden first unit in the uses clause. Adding the unit by hand can cause all kind of unwanted side effects. E.g. crashes, reported Lineinfo is not reliable and not all leaks can reliably be reported. 

This has always been the case, but is only properly documented from FPC version 3.0.2. So if you still see Heaptrc in your uses clause, remove it! for debugging and reporting to work properly. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If your program is designed to use an alternative memory manager, you can adapt your program's uses clause like so: 
    
    
    program AlternativeMemoryManager(Input, Output, StdErr);
    {$mode ObjFPC}
    uses
    	// This will conditionally switch in a memory manager
    	// based on the presence of Heaptrc
    	{$if not declared(UseHeapTrace)}
    	CMem,
    	{$endIf}
    	SysUtils, Classes; // the latter two are just to test.
    
    begin
    	{$if declared(UseHeapTrace)}
    	WriteLn('Heaptrc is used.', ' Heaptrc is active: ', UseHeapTrace);
    	{$else}
    	WriteLn('No trace of Heaptrc.');
    	{$endIf}
    end.
    

In the above example the conditional in the uses clause makes sure that the CMem memory manager is only compiled in if the Heaptrc option is not used. 

## See also

  * [Documentation issue 30637](<https://gitlab.com/freepascal.org/fpc/documentation/-/issues/30637>)
  * [leakview](<leakview.md> "leakview"): Examples how to enable Heaptrc in Lazarus and Free Pascal.
  * [RTL Reference for unit 'Heaptrc'](<http://lazarus-ccr.sourceforge.net/docs/rtl/heaptrc/> "doc:rtl/heaptrc/") (this link is not yet up-to-date)

---

_Source: [https://wiki.freepascal.org/heaptrc](https://web.archive.org/web/20230610205709/https://wiki.freepascal.org/heaptrc)_
