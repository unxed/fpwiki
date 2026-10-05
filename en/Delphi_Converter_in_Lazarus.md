# Delphi Converter in Lazarus

│ **English (en)** │

****A Delphi project, Delphi package and a single unit file can be converted for Lazarus. The entries can be found in Lazarus Tools menu:

  * **Convert Delphi unit to Lazarus unit ...**
  * **Convert Delphi project to Lazarus project ...**
  * **Convert Delphi package to Lazarus package ...**



There is also: 

  * **Convert binary DFM file to text LFM and check syntax ...**



which is seldom needed because the DFM form file is always converted while converting a unit. 

## Contents

  * 1 Conversions
    * 1.1 Unit file
    * 1.2 Form file
  * 2 Settings dialog
    * 2.1 Target
    * 2.2 Unit name replacement and removal
    * 2.3 Unknown property removal
    * 2.4 Type replacement
    * 2.5 Function call replacement
    * 2.6 Coordinate offsets
  * 3 Issues



# Conversions

Here the different conversions are listed briefly. See the sections below for details. 

## Unit file

  * All unit source files get a **{$mode delphi}** directive. It makes the compiler support language syntax exactly like Delphi has it. This makes sense because we are converting a Delphi program after all. The user can take advantage of **{$mode objfpc}** features later if he wants, by making the necessary changes manually.


  * **{$R *.DFM}** is replaced with **{$R *.lfm}** if the form file is renamed and converted. Conditional compilation is used if the target is **Lazarus and Delphi**.


  * Unit names in Uses section and include file names are fixed to match the real case sensitive names found in the file system. This is needed for case sensitive file systems.


  * Some unit names in Uses section are either replaced or removed.


  * Some Delphi types are replaced with "fall-back" LCL types (same as in form files).


  * Some function names called in the code are replaced.



## Form file

  * Older Delphi versions used a binary format for .dfm form files. They are always converted to ascii text format.


  * .dfm form file is renamed or copied to .lfm and converted as needed. There is also an option to use the same Delphi form file directly. See section **Target** below.


  * Unknown properties are removed.


  * Some Delphi types are replaced with "fall-back" LCL types (same as in unit files).


  * Top and Left coordinate offset adjustment for controls inside visual containers.



# Settings dialog

After selecting an entry from the Tools menu, and then selecting the unit / project / package to convert, the user is presented with a settings dialog. All settings are saved in file **delphiconverter.xml** in local Lazarus configuration directory. The dialog allows to configure many details of conversion: 

  * **Project Path** shows the selected path.


  * **Make backup of changed files** saves the original files to directory ConverterBackup under the selected directory.


  * **Keep converted files open in editor**. When checked, the converted unit files remain open after conversion.



Rest of the dialog settings are covered below. 

  
[![Picture of settings dialog of Delphi converter](https://wiki.freepascal.org/images/0/06/ConvertSettings.png)](</File:ConvertSettings.png> "Picture of settings dialog of Delphi converter")

## Target

The conversion target is defined in settings dialog. It can be: 

  * **Cross-platform** \-- Replace Windows specific units with their cross-platform equivalents.


  * **Support Delphi** \-- Tries to make units compatible with both Delphi and Lazarus by using {$IFDEF...} in Uses section. The Delphi form file .dfm is copied to Lazarus form file .lfm and converted.


  * **Use the same DFM form file** \-- The Delphi form file .dfm is used also with Lazarus. It is not modified except for converting binary format to ascii text format when needed.



However maintaining the file later in both Delphi and Lazarus is problematic because components have slightly different properties. 

## Unit name replacement and removal

When a unit listed in Uses section is not found, it can be replaced or removed. The replacements are configurable, see "Unit Replacements" in settings dialog. An empty "New name" means removal. For example by default "Windows" is replaced with "LCLIntf, LCLType, LMessages". 

The replacement supports regular expression syntax. For example: 

  * ^Q(.+) --> $1



means something like: match a string starting with 'Q', followed by anything, and save the part after 'Q', then replace with the saved part. In essence this removes 'Q' in the beginning. 

Replacement can be interactive or automatic. When interactive, the user can edit the replacements before they are applied. 

If there are still missing units after the replacement, the user will be asked about them in any case. He can then choose to comment them out or search for their unit paths. 

[![Picture of Units Not Found dialog of Delphi converter](https://wiki.freepascal.org/images/6/63/ConvertUnitsNotFound.jpg)](</File:ConvertUnitsNotFound.jpg> "Picture of Units Not Found dialog of Delphi converter")

## Unknown property removal

Not all Delphi component properties are implemented in LCL components. These properties can be removed. There is a system in LCL where non-existent properties are registered and the form loader ignores them. The converter also removes them, automatically or interactively. The interactive removal currently happens in the same dialog that is used for component type replacements (see below). The property can be replaced, too, but that is usually not needed. 

## Type replacement

Some Delphi components do not exist in LCL. Many 3rd party components also exist only for Delphi. Those types can be replaced with fall-back LCL types. For example: 

  * TCoolBar --> TToolBar.



It is configurable, too, see "Type Replacements" in settings dialog. 

The same regular expression syntax as used in unit name replacement is supported here. 

The types are replaced in both the Pascal source file and the form file (.lfm). The replacement works also in a nested component structure where a component of replaced type has child components that must also be replaced. 

Different properties between the original component and the replacement component can cause problems after conversion. There is no easy solution for this. 

The best solution would be to make more Delphi compatible components and port all 3rd party components to Lazarus. Because of these potential problems the type conversion is always interactive. 

[![Picture of Fix LFM dialog of Delphi converter](https://wiki.freepascal.org/images/2/2a/ConvertFixLfm.jpg)](</File:ConvertFixLfm.jpg> "Picture of Fix LFM dialog of Delphi converter")

## Function call replacement

Some Windows-only function calls in the source code can be replaced with a functionally similar RTL / LCL library function calls. It is configurable, see "Function Replacements" in settings dialog. 

There is a category defined for each function. The replacement can be enabled or disabled by category. For example there is "UTF8Names" category for file operation related functions. 

The syntax for replacements is: 

  * FileExists --> FileExistsUTF8($1)



where ($1) means the first parameter from the original function call is used for replacement function. There is also a more advanced syntax available. Like this: 

  * ShellExecute --> if $3 match ":/" then OpenURL($3); OpenDocument($3)



ShellExecute can map to 2 different LCL functions. The string after "match" keyword is again a regular expression. If it matches, the replacement right after it is used, otherwise the next replacement. 

## Coordinate offsets

In Delphi and Lazarus, components have different meaning for Top and Left coordinates when they are inside a visual container. In Delphi the coordinates are relative to container's Top and Left. In Lazarus they are relative to container's client area which leaves out the border. It causes the most visible problem for TGroupBox because it has a title text on the border. 

Subtracting an offset from the coordinates makes the form layout look the same as it looked in Delphi. The offsets are defined in "Coordinate offsets" in settings dialog. 

# Issues

When a Delphi program contains Windows API calls which are not ported in RTL/LCL, there is no automatic way to convert it. Other Windows specific technologies like OLE cause the same problem. 

Database connections often require changes. Either the DB or the connection component is not compatible. 

Another problem are some libraries which are not compatible with Delphi libraries. For example XML is used in a slightly different way. Maybe the converter can be improved in this area in the future. 

As mentioned earlier, Delphi components that don't exist in Lazarus/LCL cause problem, even if they are replaced with a fall-back component type, because they have different properties. 

Yet, if the program uses only VCL classes and functions, there is a big chance for the conversion to succeed.

---

_Source: [https://wiki.freepascal.org/Delphi_Converter_in_Lazarus](https://web.archive.org/web/20250120192234/https://wiki.freepascal.org/Delphi_Converter_in_Lazarus)_
