# IDE Window: Make ResourceString

│ **[Deutsch (de)](</IDE_Window:_Make_ResourceString/de> "IDE Window: Make ResourceString/de")** │  **English (en)** │  **[français (fr)](</IDE_Window:_Make_ResourceString/fr> "IDE Window: Make ResourceString/fr")** │  **[日本語 (ja)](</IDE_Window:_Make_ResourceString/ja> "IDE Window: Make ResourceString/ja")** │    
****  
****

This wizard helps to convert a string constant into a resourcestring. 

Place the cursor on a string constant or select the part of the constant that should be converted. Then do <Right click> Refactoring | Make Resource String. 

A string constant is for example 'Some text'. 

## Contents

  * 1 Conversion Options
    * 1.1 Identifier prefix
    * 1.2 Identifier length
    * 1.3 Custom identifier
    * 1.4 Resourcestring section
    * 1.5 String with same value
    * 1.6 Where to put the new identifier in the resourcestring section
  * 2 String constant in source
  * 3 Source preview
  * 4 Examples
    * 4.1 Example 1: Setting the caption of a form
    * 4.2 Example 2 : Complex string constants
  * 5 See also



## Conversion Options

### Identifier prefix

You can create automatically a new identifier. The new identifier will start with the prefix, followed by the words of the string constant. If there is already a resourcestring with this value, the old identifier will be suggested. The IDE remembers the prefixes for each unit, so the last used prefix for the current unit is preselected first. 

### Identifier length

This is the maximum length of the automatically created identifier. 

### Custom identifier

Instead of using an automatically created identifier, you can specify the identifier on your own here. 

### Resourcestring section

This combobox contains all available resourcestring sections of all units in scope. That means all sections of the current unit plus all sections in the interface parts of all used units. 

### String with same value

This combobox contains all resourcestring identifiers with the same value as the new resourcestring. You can choose here, if you want to take one of them. 

### Where to put the new identifier in the resourcestring section

If a new identifier is created, this defines where it will be added in the resourcestring section. 

  * Append to section - add the new identifier at the end of the section.
  * Insert alphabetically - insert the identifier alphabetically.
  * Insert context sensitive - search the code above and below the current code position and search for resourcestring identifiers. If it finds one, then the new identifier is added there, otherwise it is appended at the end of the section.



## String constant in source

This shows the old or current source code. 

## Source preview

This is a preview, how the string constant is replaced, followed by a line of dashes and then a preview of how the resourcestring will look like. 

## Examples

### Example 1: Setting the caption of a form

If you do not have already a resource string section, start one in the interface part of the unit. For example right above the 'implementation' keyword: 
    
    
     
      resourcestring
        rsSomeText = 'Some text';
    

Add in the FormCreate event the following line: 
    
    
     
      Caption:='An example caption';
    

Place the cursor on the string constant, i.e. somewhere between the two '. Then do -> <Right click> Refactoring | Make Resource String 

Set in the identifier prefix 'rs' without the '. The new automatically suggested identifier will be 'rsAnExampleCaption'. The preview shows 
    
    
       Caption:=rsAnExampleCaption;
     --------------------------------------------------------------------------------
     rsAnExampleCaption = 'An example caption'   
    

Click ok. 

### Example 2 : Complex string constants

When converting a complex string expression with variables and functions, the tool will use the **Format** function. For instance the statement: 
    
    
     
      Caption:='Left='+IntToStr(Left)+' Top='+IntToStr(Top);
    

will be replaced with 
    
    
     
      Caption:=Format(rsLeftTop, [IntToStr(Left), IntToStr(Top)]);
    

and the new resourcestring will be 
    
    
     
      rsLeftTop = 'Left=%s Top=%s'
    

The Format function is defined in the SysUtils unit. If your unit uses section does not already contain it, you must add it yourself. 

  


## See also

  * [Getting translation strings right](<Getting_translation_strings_right.md> "Getting translation strings right")

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Make_ResourceString](https://web.archive.org/web/20250316152127/https://wiki.freepascal.org/IDE_Window%3A_Make_ResourceString)_
