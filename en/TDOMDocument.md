# TDOMDocument

│ **English (en)** │    
****

The TDOMDocument class holds document data according to the [Document Object Model (DOM)](<dom.md> "dom"). 

## Declaration

Excerpted from dom.pas 
    
    
    TDOMDocument = class(TDOMNode_WithChildren)
      protected
        FIDList: TList;
        FRevision: Integer;
        FXML11: Boolean;
        FImplementation: TDOMImplementation;
        function GetDocumentElement: TDOMElement;
        function GetDocType: TDOMDocumentType;
        function GetNodeType: Integer; override;
        function GetNodeName: DOMString; override;
        function IndexOfNS(const nsURI: DOMString): Integer;
        function FindID(const aID: DOMString; out Index: LongWord): Boolean;
        procedure ClearIDList;
      public
        property DocType: TDOMDocumentType read GetDocType;
        property Impl: TDOMImplementation read FImplementation;
        property DocumentElement: TDOMElement read GetDocumentElement;
    
        function CreateElement(const tagName: DOMString): TDOMElement; virtual;
        function CreateElementBuf(Buf: DOMPChar; Length: Integer): TDOMElement;
        function CreateDocumentFragment: TDOMDocumentFragment;
        function CreateTextNode(const data: DOMString): TDOMText;
        function CreateTextNodeBuf(Buf: DOMPChar; Length: Integer): TDOMText;
        function CreateComment(const data: DOMString): TDOMComment;
        function CreateCommentBuf(Buf: DOMPChar; Length: Integer): TDOMComment;
        function CreateCDATASection(const data: DOMString): TDOMCDATASection; virtual;
        function CreateProcessingInstruction(const target, data: DOMString): TDOMProcessingInstruction; virtual;
        function CreateAttribute(const name: DOMString): TDOMAttr;
        function CreateAttributeBuf(Buf: DOMPChar; Length: Integer): TDOMAttr;
        function CreateEntityReference(const name: DOMString): TDOMEntityReference; virtual;
        // Free NodeList with TDOMNodeList.Release!
        function GetElementsByTagName(const tagname: DOMString): TDOMNodeList;
    
        // DOM level 2 methods
        function ImportNode(ImportedNode: TDOMNode; Deep: Boolean): TDOMNode;
        function CreateElementNS(const NamespaceURI, QualifiedName: DOMString): TDOMElement;
        function CreateAttributeNS(const NamespaceURI, QualifiedName: DOMString): TDOMAttr;
        function GetElementsByTagNameNS(const namespaceURI, localName: DOMString): TDOMNodeList;
        function GetElementById(const ElementID: DOMString): TDOMElement;
        // Extensions to DOM interface:
        // TODO: obsolete now, but must check for usage dependencies
        constructor Create;
        destructor Destroy; override;
        function AddID(Attr: TDOMAttr): Boolean;
        procedure RemoveID(Attr: TDOMAttr);
      end;
    

Back to [fcl-xml](<fcl-xml.md> "fcl-xml") overview. 

## See also

  * [TXMLDocument](<TXMLDocument.md> "TXMLDocument"),
  * [Document Object Model](<dom.md> "dom")



## External links

  * [Document Object Model at Wikipedia](<https://en.wikipedia.org/wiki/Document_Object_Model>)

---

_Source: [https://wiki.freepascal.org/TDOMDocument](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/TDOMDocument)_
