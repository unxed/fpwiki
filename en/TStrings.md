# TStrings

A **TStrings** is a datatype that can hold an arbitrary length list of [strings](<String.md> "String"). The strings in a TStrings are accessible as concatenated plain text or as a series of strings. 

Inheritance

  * [TObject](<TObject.md> "TObject") \- Base class of all classes. 
    * [TPersistent](</index.php?title=TPersistent&action=edit&redlink=1> "TPersistent \(page does not exist\)") \- Base class for streaming system and persistent properties - [IFPObserved](</index.php?title=IFPObserved&action=edit&redlink=1> "IFPObserved \(page does not exist\)") interface implemented by an object that can be observed. 
      * TStrings \- Class to manage arrays or collections of strings



  
Basic functionality: 

Methods
    
    Add() - add a string to the list
    AddStrings() - Add contents of another stringlist to this list.
    Assign() - Assign the contents of another stringlist to this one.
    Clear() - Removes all strings and associated objects from the list.
    IndexOf() - Find a string in the list and return its position.
    IndexOfName()- Finds the index of a name in the name-value pairs.
    Insert() - Insert a string in the list.
    LoadFromFile() - Load the contents of a file as a series of strings.
    LoadFromStream() - Load the contents of a stream as a series of strings.
    SaveToFile() - Save the contents of the list to a file.
    SaveToStream() - Save the contents of the string to a stream.
    GetNameValue() - Return both name and value of a name,value pair based on it's index.
    ExtractName() - Extract the name part of a string
properties
    TextLineBreakStyle - Determines which line breaks to use in the Text property
    Delimiter - Delimiter character used in DelimitedText.
    DelimitedText - Get or set all strings in the list in a delimited form.
    StrictDelimiter - Should only the delimiter character be considered a delimiter
    QuoteChar - Quote character used in DelimitedText.
    NameValueSeparator - Value of the character used to separate name,value pairs
    ValueFromIndex - Return the value part of a string based on it's index.
    Capacity - Capacity of the list, i.e. number of strings that the list can currently hold before it tries to expand.
    CommaText - Contents of the list as a comma-separated string.
    Count - Number of strings in the list.
    Names - Name parts of the name-value pairs in the list.
    Objects - Indexed access to the objects associated with the strings in the list.
    Values - Value parts of the name-value pairs in the list.
    Strings - Indexed access to the strings in the list.
    Text - Contents of the list as one big string.

## See also

  * [TStrings doc](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/tstrings.html> "doc:rtl/classes/tstrings.html")
  * [TStringList](<TStringList.md> "TStringList")
  * [TStringList-TStrings Tutorial](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial")

---

_Source: [https://wiki.freepascal.org/TStrings](https://web.archive.org/web/20250217101614/https://wiki.freepascal.org/TStrings)_
