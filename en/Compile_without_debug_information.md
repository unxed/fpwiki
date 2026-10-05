# Compile without debug information

Q: I can't find the option to compile without debug information. How can I compile without debug information? 

* * *

A-1, from forum user _howardpc_ : 

User "Project -> Project Options...". In the "Options for Project: xxx" dialog click the "Debugging" node under "Compiler Options" in the tree-view on the left. Then uncheck the "Generate info for the debugger (slower / increases exe-size)" checkbox. 

* * *

A-2, from forum user _Martin_fr_ : 

This affects only your project. If packages (LCL or other) are compiled with debug info, then that does not change => each package has it's own setting. And many package also mix in the "Configure build IDE" options. 

You can use "Additions and Overrides" in the "Project Options" to make changes. This also allows to re-compile packages with more/less optimization and/or with/without range/stack/overflow-checks etc. 

Note: The RTL (and FCL) are pre-build. The installers that come with Lazarus usually does install them without debug info. The IDE has no method to rebuild them. To change those (well you do likely not need, since they should match your requirements) you would need to rebuild them from the command line. See [buildfaq](<buildfaq.md> "buildfaq"). 

Some function names may be left in the exe, even if debug info is switched off (IIRC that happens while linking, but not sure) => just use "strip.exe" on the your compiled exe. 

On top of that, note that your app always carries RTTI (well almost always...). So if you use a hex viewer you will still find some names from your source code. But that is not debug info. 

* * *

A-3, from forum user _Soner_ : 

You can also create external debug file, then you have debug information if you need it and to have a small application file. Look at the picture, screenshot is from IDE menu "Project > Project Options". 

[![ide external debug info.jpg](https://wiki.freepascal.org/images/1/12/ide_external_debug_info.jpg)](</File:ide_external_debug_info.jpg>)

---

_Source: [https://wiki.freepascal.org/Compile_without_debug_information](https://web.archive.org/web/20250516133214/https://wiki.freepascal.org/Compile_without_debug_information)_
