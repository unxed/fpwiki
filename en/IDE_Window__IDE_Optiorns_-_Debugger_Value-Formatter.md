# IDE Window: IDE Options - Debugger Value-Formatter

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page applies to Lazarus 3.99

[![ValueFormatters-List.png](https://wiki.freepascal.org/images/3/38/ValueFormatters-List.png)](</File:ValueFormatters-List.png>)

## Contents

  * 1 About Value-Formatter
  * 2 General Configuration
    * 2.1 Search Order
      * 2.1.1 Project vs IDE list
      * 2.1.2 Ordering
      * 2.1.3 Checked (Enabled) or direct selection (watch properties)
    * 2.2 Adding / Removing
    * 2.3 Filters
      * 2.3.1 Match type by name
        * 2.3.1.1 Match the variable name or expression
      * 2.3.2 Limit to nest-level
    * 2.4 Settings
      * 2.4.1 Show original
  * 3 Available Value-Formatter
    * 3.1 Date Time
    * 3.2 Color
    * 3.3 Alpha Color
    * 3.4 CharArray as string
    * 3.5 Custom DisplayFormat
    * 3.6 Convert Ordinal to name



  


# About Value-Formatter

Value-Formatter change the format in which a value is displayed. 

They do not affect the content. The underlying data retrieved from the debugged process is not affected by a value formatter. Value formatter can only change the presentation of that data. This can range from formatting (spacing, line breaking, quoting), over simple conversion (decimal/hex or float-as-date) to performing calculations/lookups on/with the data. 

Value formatters can act on the entire result of a watched value, including the entire structure or array. Or they can act on individual entries/fields. 

# General Configuration

## Search Order

A value can only be formatted by one formatter. In case of a structure or array, that can mean each field is a value that can be formatted. 

The IDE needs to find decide which Value-Formatter to use. If several Value-Formatter match their Filter, then the following criteria are used. 

### Project vs IDE list

Value-Formatter can be configured globally for the entire IDE, or for the current project. If enabled, the IDE will first look for a Value-Formatter from the project, and only if none is found it will look for a Value-Formatter from the IDE config. 

In the Project settings it is possible to enable/disable each of those two lists.  
[![ValueFormatters-Project.png](https://wiki.freepascal.org/images/9/93/ValueFormatters-Project.png)](</File:ValueFormatters-Project.png>)

"Store Formatter Config in session"
    The project list of Display-Formatters is stored in the session, rather than the lpi file.  
The settings to "store in session" itself remains in the lpi.

### Ordering

Value-Formatter can be ordered in their list (Up/Down buttons). Each list is searched top to bottom for the first match. 

### Checked (Enabled) or direct selection (watch properties)

Each Value-Formatter has a checkbox. The IDE will only apply enabled/active Value-Formatter automatically (if they match). 

Non active Value-Formatter will be ignored by default. They can be manually applied to an individual watch (or in the inspect/evaluate windows). For this the watch properties have a drop down that allows to select a Value-Formatter. From this list active and non-active Value-Formatter can be selected. Please note, that selected Value-Formatter must still have matching filter settings. But for non-active Value-Formatter the filters can be set more relaxed to make sure they do match. 

## Adding / Removing

To add a new Value-Formatter, first select it from the drop down. Then press "Add". 

## Filters

Some Value-Formatter have build in filters. Such as a Value-Formatter that takes a number as input will only work on values of numeric type. If you are watching a structure or array the Value-Formatter can however act on matching entries/fields. 

### Match type by name

You must specify which data type or types the Value-Formatter should be applied to. At least one value must be given. You can specify more than one type, by adding one one each line. In this case at least one must be matched. 

This can be the exact name of a type: boolean, word, TForm (leading or trailing spaces will be ignored) 

Or you can match the typename using "*" as placeholder (for zero, one or more characters) 
    
    
     TData*
     *Ptr
     T*Foo
     T*Frame*
    

Any amount of * can be used. 

For class types inheritance can be checked by starting a line with "is:" 
    
    
      is:TControl
      is:TCustom*
      is:*Form 
    

  


![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** FPC resolves typenames to their un-aliased form. So "Integer" will be known as "longint" (except on 16bit systems). 

#### Match the variable name or expression

It is also possible to match the name of the variable or field. 

Lines prefixed with either "var:" or "expr:" will do that. 
    
    
      var:MyForm
      var:*Data
      expr:*.SomeField
    

  * **"var:"** will be matched against the term that you entered into the watch "expression" property, or the name of a local, or the identifier in your hint or inspect/evaluate Window.  
If you watch "MyPoint: TPoint" then



    

  * The watch window will display: (X: 123; y: 99)
  * If your Value-Formatter is matching the entire structure, then "var:My*" will match "MyPoint"
  * If your Value-Formatter is matching each field then for each field the term "MyPoint" will be matched by "var:My*".   
But if you tried "var:*X" to match only the X value, then that does not work. The term being matched is "MyPoint"
  * if you expand the watch in the watch window, so you have in-lined extra rows for x and y then those extra rows are new watches and their term is "MyPoint.X" or "MyPoint.Y". They will be matched by both "var:My*" or "var:*.X". But they would not be matched by "var:MyPoint" (since there is no * and this is an exact match)


    

  * **"expr:"** will be matched against the var + path-to-field. (Albeit, in a simplified form).  
The same "MyPoint" watch.



    

  * If you are matching fields in the single line view of the watch windows, the fields will have the term "MyPoint.X" (or Y).
  * If you are matching fields in the expanded view of the watch windows, the fields will also have the term "MyPoint.X" (or Y).
  * Array indexes are omitted



If you have a field "MyFoo" on an object, then your watch can either be 
    
    
      MyObject
      MyObject.MyFoo
      MyFoo
    

The last one, if you are in a method of the object. 

To match the field in all cases, use the following two: 
    
    
      expr:*.MyFoo
      expr:MyFoo
    

The second could also be "var:MyFoo". The first matches if MyFoo is in the structure. For the watch "MyObject" the IDE will append the ".MyFoo" when displaying the field. 

Note, if you click the + to expand a watch, then the new watches on the expanded rows may contain a typecast (depending on the watch settings). So the Watch-name to match may be "TMyObject(MyObject).MyFoo". On the other hand the term passed to "expr:" prefixed matches for fields on the non-expanded line does not have such a typecast. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** When matching the name, there must still be a match for the type. If the type does not matter then specify a standalone "*" 

### Limit to nest-level

If you have structures or arrays, then entries/fields are seen as "nested values". 

You may have situations were you want to exclude those from being formatted. Then you can set the Min/Max nest-level to 0. (This also saves the IDE the work to perform matching on type names). 

Or you may only want to have formatting if a value is nested, then set the min-level to 1. (or higher). Note, that 

  * if you specify "MyObject", then the fields are at level 1
  * If you expand that watch to have individual rows to display each field, then they are at level 0 (top level)
  * if you enter a watch "MyObject.FieldFoo" then that is at level 0 (top level).



The reason is, that if it has a row on its own, it has more space to display, and therefore different handling may be wanted. 

If your watch is just "FieldFoo" (while in a method of the owning object), then you can not check if this is a field, or what the name of the owning variable is (in this case it would be "self"). 

## Settings

### Show original

Most Value-Formatter have the option to show the original/unchanged value next to the formatted value. 

# Available Value-Formatter

## Date Time

Convert a float to human read-able date/time. 

You can set the format for the output according to <https://www.freepascal.org/docs-html/rtl/sysutils/formatchars.html>

There are 3 fields for specifying a format: 

  * "Date Time" if the value has non-zero parts for date and time
  * "Date" if the value has a zero part for time
  * "Time" if the value has a zero part for date



By default the IDE comes with 3 instances of the formatter setup, for the data types: 

  * TDateTime
  * TDate
  * TTime



This means it is possible to set up a different format for a TTime that for some reason has a date part, and a TDateTime which is expected to have a date part. 

## Color

Will lookup the name of the color. Such as "clRed" or "clBlue". 

Alternatively it can display an RGB list. 

  
The color names are looked up from those registered in the IDE. For use with FPC's "FindIntToIdent". If the application registers its own list, that is not taken into account. In such a case consider #Convert_Ordinal_to_name . 

## Alpha Color

As TColor, but for TAlphaColor 

## CharArray as string

For data type 
    
    
      Array of char
      Array [n..m] of char
    

Displays the data as string. This improves readability over a list of individual chars. 

The string can be cut at the first #0. 

## Custom DisplayFormat

Set a specific DisplayFormat. (e.g. Hex or Binary). 

If you have data types (or fields/variables by name) that you always want in a specific DisplayFormat 

  


  * The option "Override watch settings"



    

  * If not checked then the DisplayFormat acts as default, but you can choose a different setting in the watch properties
  * If checked then the DisplayFormat will override the setting from the watch properties



The latter case can be useful if the format is to be applied to some fields in a structure. The settings in the watch properties can then be used for other fields, but the field matched by the Value-Formatter will be forced to the format given here. 

It can be useful to have 2 Value-Formatters 

  * Min/Max level = 0 (top level) with "Override watch settings" **not** checked
  * Min level = 1 (nested) with "Override watch settings" **checked**



## Convert Ordinal to name

Find the name for named integer constants. 

if you have code like 
    
    
     const
     ecLeft            = 1;    // Move cursor left one char
     ecRight           = 2;    // Move cursor right one char
     ecUp              = 3;    // Move cursor up one line
     ecDown            = 4;    // Move cursor down one line
     ecWordLeft        = 5;    // Move cursor left one word
     ecWordRight       = 6;    // Move cursor right one word
    

You can copy that code into the config for the Value-Formatter (it will ignore the comments). 

It expects 

  * one value per line
  * NAME = 123
  * optional comment / optional ; at end
  * comment are skipped multi-line
  * NAME=TMyType(123) // TMyType is ignored
  * NAME=PREVIOUS_NAME + 123 // in that exact order only



If the value formatter is applied to a value it will look up the number and print the text.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_IDE_Optiorns_-_Debugger_Value-Formatter](https://web.archive.org/web/20250427002054/https://wiki.freepascal.org/IDE_Window%3A_IDE_Optiorns_-_Debugger_Value-Formatter)_
