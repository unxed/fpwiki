# xmlwrite

│ **English (en)** │

Writes a DOM structure as XML data into a file or stream. It can deal both with XML files and XML fragments. 

## Contents

  * 1 Declarations
  * 2 Encodings
  * 3 Character escaping
  * 4 Exceptions
  * 5 End-of-line handling
  * 6 Notes
  * 7 See also



## Declarations

Declarations excerpted from XMLWrite.pas. 
    
    
    procedure WriteXMLFile(doc: TXMLDocument; const AFileName: String); overload;
    procedure WriteXMLFile(doc: TXMLDocument; var AFile: Text); overload;
    procedure WriteXMLFile(doc: TXMLDocument; AStream: TStream); overload;
    
    procedure WriteXML(Element: TDOMNode; const AFileName: String); overload;
    procedure WriteXML(Element: TDOMNode; var AFile: Text); overload;
    procedure WriteXML(Element: TDOMNode; AStream: TStream); overload;
    

## Encodings

At the moment it supports only the [UTF-8](<UTF-8.md> "UTF-8") output encoding. The encoding label is not written because it is optional for UTF-8. 

## Character escaping

The writer provides proper character escaping as required by XML: 

  * for normal text nodes, the following replacements will be done: 
    * '<' => '&lt;'
    * '>' => '&gt;'
    * '&' => '&amp;'
  * For attribute values, additionally '"' (#34) gets replaced by '&quot;', and characters #9, #10 and #13 are escaped using numerical references.
  * Single apostrophes (') don't need to get converted, as values are already written using "" quotes.



The [XML reader](<xmlread.md> "xmlread") (in xmlread.pp) will convert these entity references back to their original characters. 

## Exceptions

Will raise EConvertError if any DOM node contains an unpaired UTF-16 surrogate in its name or value. 

## End-of-line handling

At the moment always uses [line endings](<End_of_Line.md> "End of Line") which are default for the target platform (#13#10 for Windows, #10 for Linux). Moreover, any #13, #10 or #13#10 in input data is treated as a single line-ending. 

## Notes

  * The sets of WriteXML() and WriteXMLFile() procedures duplicate each other, one of them is actually redundant.



  
Back to [fcl-xml](<fcl-xml.md> "fcl-xml") overview. 

  


## See also

  * [xmlread](<xmlread.md> "xmlread")

---

_Source: [https://wiki.freepascal.org/xmlwrite](https://web.archive.org/web/20240121090035/https://wiki.freepascal.org/xmlwrite)_
