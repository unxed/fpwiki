# IDE Window: Ide Options - Backend Value Converter

## Contents

  * 1 About Backend-Converters
  * 2 Configuration
    * 2.1 Add / Remove
    * 2.2 "Enabled" Checkbox
    * 2.3 Name
    * 2.4 Action
    * 2.5 Match types by name
    * 2.6 Other ...
  * 3 How/When is a converter applied to a watch
  * 4 Per Project settings
  * 5 Example Backend-Converters
    * 5.1 "Convert variant to value type"
    * 5.2 CallSysVarToLStr
    * 5.3 JsonForDebug



# About Backend-Converters

    Backend-Converters are plugins for the debugger backend. They can be used to enhance/alter the display of specific values.
    As they are executed in the backend, they can fetch additional information and include this in the resulting watch data.

Backend-Converters must be compiled into the Lazarus exe. They can either be included within an existing Debugger-backend, or they can be added by 3rd party packages. Currently only the LazDebuggerFp (FpDebug) supports Backend-Converters. 

For a Backend-Converters to be used, it must be associated with the type-name of the watched data. 

  


# Configuration

## Add / Remove

Will add/remove entries to/from the list on top of the Window. Those entries can then be selected and configured. 

## "Enabled" Checkbox

Each entry in the list has a checkbox. 

  * If enabled, then the "Match type name" will be checked for each watch that is evaluated. Any data will be processed by the first matching Backend-Converter.
  * If disabled, the converter can be explicitly assigned in the "Watches properties". Watches for which a Backend-Converters is assigned will still check if the type-name matches.



For Watches that return structured data or references to other data, and the embedded data is part of the result (e.g. a field of an object), such embedded data will also be checked and converted if matching. If the outer value matches, any embedded data must be included by the Backend-Converters. In this case it is not subject to further conversion. 

## Name

Change the name of the Backend-Converters 

## Action

Select a registered Backend-Converters 

## Match types by name

Each line can contain a type-name or pattern. 

The following is recognized: 

  * "TFoo" exact typename (case insensitive)
  * "*oo", "TF*", "T*o" an asterisk matches zero, one or more chars
  * "is:TFoo" must be an object, and must inherit from TFoo. (also accepts "is:TF*")



For the outer value of the watch, the "is:" operator will take the type of the instance. For nested values, only the declared type is tested. 

## Other ...

Each converter may have it's own additional settings. If they have additional configuration will be shown at the bottom of the Window. 

# How/When is a converter applied to a watch

For each watch that the debugger backend processes, it follows the steps below 

  * Evaluate the watch
  * Check settings in the watch properties



    The following setting are available 

  * Default:



    

    All "Enabled" converters will be considered.
    Converters from the project are first considered, the Converters from the global IDE config
    Both list will be processed from top to bottom, until the first match is found.

  * No Converter:



    

    The watch will not be processed

  * <NAME>:



    

    Only the selected converter <NAME> will be considered (if it matches in the below)
    The converter will be considered even if it is not "Enabled"

  * For each converter from the first step, the "Match types by name" settings are checked



    

  * The type-name of evaluated result will be checked



    If the type-name matches, the converter will be called.
    Even if that converter then fails or rejects:  
No further converter will be considered. No checks on nested (array/struct) values will be performed.

  * If no converter matched the result type-name



    nested values (in case of a structure or array) will be tested and converted if they match.

# Per Project settings

Additional settings can be made on a per Project basis: [IDE_Window:_Project_Options_-_Value_Converter](<IDE_Window__Project_Options_-_Value_Converter.md> "IDE Window: Project Options - Value Converter")

# Example Backend-Converters

Those only work with LazDebuggerFp. 

## "Convert variant to value type"

Convert variants to their value. 

Depending on Dwarf versions variants either show as record with all the available fields, or as record with 2 values "(vtype: 1; xxx: ...)". 

  * The converter simply shows the value. E.g. just the integer, or string.
  * The converter can not add typeinfo. E.g. enum are stored as integer, and as integer they are displayed
  * The converter does not support custom variant types, for that use CallSysVarToLStr. (Unlike CallSysVarToLStr, this converter does not call any functions in the debuggee)



  


## CallSysVarToLStr

This only works for variant data (including custom variants). 

It will invoke the function "VarToLStr()" (from "TVariantManager"). The function will be invoked in the debugged process. The Backend-Converter returns the textual representation given by "VarToLStr()". 

## JsonForDebug

Calls a user defined function (default "JSonForDebug") in the debugged process. 

The function must support exactly the following parameters. 
    
    
     procedure JsonForDebug(
       const Value;   // or  :Pointer;  
       var TypeName: AnsiString;
       out JsonResultText: AnsiString
     );
    

The result text will be returned as watch representation. If it is valid JSon, the IDE will allow to expand/unfold entries. 

If a JSON object has a key matching the "Key for Address", it will be read for the Address column in the watches window. If a JSON object has a key matching the "Key for Typename", it will be read for the type-name column in the inspect window. 

  
  
****  
****

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Ide_Options_-_Backend_Value_Converter](https://web.archive.org/web/20240526175651/https://wiki.freepascal.org/IDE_Window%3A_Ide_Options_-_Backend_Value_Converter)_
