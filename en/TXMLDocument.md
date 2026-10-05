# TXMLDocument

│ **English (en)** │  **[español (es)](</TXMLDocument/es> "TXMLDocument/es")** │    
****

The TXMLDocument class holds XML data, e.g. from a file. It is a child of [TDOMDocument](<TDOMDocument.md> "TDOMDocument"). 

## Declaration

This is excerpted from dom.pas 
    
    
    TXMLDocument = class(TDOMDocument)
      private
        FXMLVersion: DOMString;
        procedure SetXMLVersion(const aValue: DOMString);
      public
        // These fields are extensions to the DOM interface:
        Encoding, StylesheetType, StylesheetHRef: DOMString;
    
        function CreateCDATASection(const data: DOMString): TDOMCDATASection; override;
        function CreateProcessingInstruction(const target, data: DOMString): TDOMProcessingInstruction; override;
        function CreateEntityReference(const name: DOMString): TDOMEntityReference; override;
        property XMLVersion: DOMString read FXMLVersion write SetXMLVersion;
      end;
    

Go back to [fcl-xml](<fcl-xml.md> "fcl-xml") overview. 

## See also

  * [xmlread](<xmlread.md> "xmlread")
  * [xmlwrite](<xmlwrite.md> "xmlwrite")
  * [TDOMDocument](<TDOMDocument.md> "TDOMDocument")

---

_Source: [https://wiki.freepascal.org/TXMLDocument](https://web.archive.org/web/20230402032411/https://wiki.freepascal.org/TXMLDocument)_
