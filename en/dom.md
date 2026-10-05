# dom

Implements the [DOM level 2 Core](<http://www.w3.org/TR/2000/REC-DOM-Level-2-Core-20001113/>) specification and some of the [DOM level 3 Core](<http://www.w3.org/TR/2004/REC-DOM-Level-3-Core-20040407/>) properties/methods. 

## Supported DOM level 3 properties and methods:

  * TDOMNode.TextContent
  * TDOMNode.BaseURI
  * TDOMText.IsElementContentWhitespace
  * TDOMNode.LookupNamespaceURI()
  * TDOMDocument.DocumentURI
  * TDOMAttr.IsID
  * TDOMNode.LookupPrefix()
  * TDOMNode.IsDefaultNamespace()
  * XmlVersion property (stub in TDOMDocument, functional in TXMLDocument)
  * TDOMDocument.XmlEncoding, TDOMEntity.XmlEncoding



## Issues

  * The specification says that `TDOMNode.lookupNamespaceURI()` should return the default namespace if its argument is `null` and `null` if the argument is an empty string. Pascal, however, does not allow to distinguish between `null` and empty string, so we have no other choice than returning default namespace for empty string arguments.



  
Back to [fcl-xml](<fcl-xml.md> "fcl-xml") overview.

---

_Source: [https://wiki.freepascal.org/dom](https://web.archive.org/web/20250217111327/https://wiki.freepascal.org/dom)_
