# xmlread

│ **English (en)** │

Provides an XML reader, which can read XML data from a file or stream. The data is stored in a [TXMLDocument](<TXMLDocument.md> "TXMLDocument"). 

## Contents

  * 1 Declarations
  * 2 Encodings
  * 3 White space handling
  * 4 DTD support
  * 5 See also



## Declarations

Declarations excerpted from xmlread.pas. 
    
    
    procedure ReadXMLFile(out ADoc: TXMLDocument; const AFilename: WideString); overload;
    procedure ReadXMLFile(out ADoc: TXMLDocument; var f: Text); overload;
    procedure ReadXMLFile(out ADoc: TXMLDocument; var f: TStream); overload;
    procedure ReadXMLFile(out ADoc: TXMLDocument; var f: TStream; const ABaseURI: WideString); overload;
    
    procedure ReadXMLFragment(AParentNode: TDOMNode; const AFilename: WideString); overload;
    procedure ReadXMLFragment(AParentNode: TDOMNode; var f: Text); overload;
    procedure ReadXMLFragment(AParentNode: TDOMNode; var f: TStream); overload;
    procedure ReadXMLFragment(AParentNode: TDOMNode; var f: TStream; const ABaseURI: WideString); overload;
    
    procedure ReadDTDFile(out ADoc: TXMLDocument; const AFilename: WideString); overload;
    procedure ReadDTDFile(out ADoc: TXMLDocument; var f: Text); overload;
    procedure ReadDTDFile(out ADoc: TXMLDocument; var f: TStream); overload;
    procedure ReadDTDFile(out ADoc: TXMLDocument; var f: TStream; const ABaseURI: WideString); overload;
    

## Encodings

The parser can read files encoded in [UTF-8](<UTF-8.md> "UTF-8"), [UTF-16](</index.php?title=UTF-16&action=edit&redlink=1> "UTF-16 \(page does not exist\)") (both endianness), and ISO-8859-1. 

## White space handling

Regarding whitespace handling: By default, whitespace directly after the beginning of a tag is discarded, and sections of the XML file which contain only whitespace and no other text content are discarded as well. However, whitespace-preserving mode can be enabled by setting TDOMParser.Options.PreserveWhitespace property to True. 

## DTD support

It supports DTD validation. Regarding entity references: The pre-defined entities "lt", "gt", "amp", "apos" and "quot", and internal entities declared in DTD, are replaced by their defined values during reading. Ability to resolve external entities is currently limited to the file system. 

  
Back to [fcl-xml](<fcl-xml.md> "fcl-xml") overview. 

  


## See also

  * [xmlwrite](<xmlwrite.md> "xmlwrite")

---

_Source: [https://wiki.freepascal.org/xmlread](https://web.archive.org/web/20240121090039/https://wiki.freepascal.org/xmlread)_
