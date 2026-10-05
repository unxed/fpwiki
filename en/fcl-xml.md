# fcl-xml

The package FCL-XML contains units that parse XML and HTML files to DOM, and can render the DOM tree to HTML, XHTML and XML. 

Most notably the HTML part still needs some work. 

## Contents

  * 1 Units
  * 2 Notes
  * 3 Tutorial
  * 4 Links
  * 5 Cleanup
  * 6 See also



## Units

Unit | Unit group | Comment   
---|---|---  
[dom](<dom.md> "dom") | - | Implements the DOM level 2 Core specification and some of the DOM level 3 Core properties/methods.   
[dom_html](</index.php?title=dom_html&action=edit&redlink=1> "dom html \(page does not exist\)") | - | DOM extensions for HTML, THTMLDocument   
[dtdmodel](</index.php?title=dtdmodel&action=edit&redlink=1> "dtdmodel \(page does not exist\)") | - | Classes represententing the DTD (document type definition), used by xmlread and dom.   
htmldefs | - | Contains basic HTML declarations.   
[htmlelements](<htmlelements.md> "htmlelements") | - | Implements a DOM for HTML content. Contains a TDOMElement descendent for all valid HTML 4.1 tags.   
[htmlwriter](<htmlwriter.md> "htmlwriter") | - | Implements a verified HTML producer.   
[htmwrite](</index.php?title=htmwrite&action=edit&redlink=1> "htmwrite \(page does not exist\)") | - | Writes a DOM structure as UTF-8 encoded HTML data into file or stream.   
[sax](</index.php?title=sax&action=edit&redlink=1> "sax \(page does not exist\)") | - | Base classes for a parser after [SAX](<http://www.wikipedia.org/wiki/Simple_API_for_XML> "wikipedia:Simple API for XML") model.   
[sax_html](</index.php?title=sax_html&action=edit&redlink=1> "sax html \(page does not exist\)") | - | HTML plugin (implementation) for the html SAX parser.   
[sax_xml](</index.php?title=sax_xml&action=edit&redlink=1> "sax xml \(page does not exist\)") | - | XML plugin for the SAX parser.   
[xhtml](</index.php?title=xhtml&action=edit&redlink=1> "xhtml \(page does not exist\)") | - | XHTML helper classes(?)   
[xmlcfg](</index.php?title=xmlcfg&action=edit&redlink=1> "xmlcfg \(page does not exist\)") | - | Implements TXMLConfig class, which enables applications to store their configuration data in XML files.   
[xmlconf](<xmlconf.md> "xmlconf") | - | An improved version of xmlcfg, based on DOMString instead of AnsiString. Provides better Unicode support, faster/smaller code due to absense of conversions, and TRegistry-like OpenKey/CloseKey methods.   
[xmliconv](</index.php?title=xmliconv&action=edit&redlink=1> "xmliconv \(page does not exist\)") | - | Registers an any-to-UTF-16 decoder based on libiconv ([iconvenc](<iconvenc.md> "iconvenc") package)   
[xmliconv_windows](</index.php?title=xmliconv_windows&action=edit&redlink=1> "xmliconv windows \(page does not exist\)") | - | Registers an any-to-UTF-16 decoder based on libiconv (windows dependant iconv header), Windows version. (still uses iconv!)   
[xmlread](<xmlread.md> "xmlread") | - | Provides routines and classes to read XML data from a file or stream into DOM.   
[xmlstreaming](</index.php?title=xmlstreaming&action=edit&redlink=1> "xmlstreaming \(page does not exist\)") | - | (Not functional) An initial attempt to support standard component streaming in XML format. The working variant of this unit is available in Lazarus (components/codetools/laz_xmlstreaming.pas).   
[xmlutils](</index.php?title=xmlutils&action=edit&redlink=1> "xmlutils \(page does not exist\)") | - | Implements utility functions and classes that are used by other units in the package.   
[xmlwrite](<xmlwrite.md> "xmlwrite") | - | Writes a DOM structure as XML data into a file or stream. It can deal both with XML files and XML fragments.   
[xpath](<xpath.md> "xpath") | - | Just an XPath implementation. Should be fairly completed, but there hasn't been further development recently.   
[xmlreader](</index.php?title=xmlreader&action=edit&redlink=1> "xmlreader \(page does not exist\)") | - | Contains TXMLReader, an abstract base class for .NET style streamed XML reading.   
[xmltextreader](</index.php?title=xmltextreader&action=edit&redlink=1> "xmltextreader \(page does not exist\)") | - | Contains TXMLTextReader, a TXMLReader descendant which parses text. This is the core of XML reading functionality.   
  
Include files 

include file | comment   
---|---  
names.inc | Included by xmlutils. Contains character tables for XML names.   
tagsimpl.inc |   
tagsintf.inc | contains all possible tags for htmlelements   
wtagsimpl.inc |   
wtagsintf.inc | contains all possible tags for htmlwriter   
xpathkw.inc | contains the perfect hash function for XPath keywords   
  
## Notes

  * beware, both dom_html and htmlelements seem to define a THTMLDocument class.
  * The nodemanager of DOM is not Delphi (2009 in my case) compatible, which can lead to strange crashes when attempting to free code. This might have to do with different minimal size of an object.



## Tutorial

  * Here: [XML Tutorial](<XML_Tutorial.md> "XML Tutorial")



## Links

  * [Dom specs](<http://www.w3.org/DOM/DOMTR>)
  * [Sax based html parser](<http://htmlparser.sourceforge.net/>) (Java)
  * [The Sax project](<http://www.saxproject.org/>) (Java)



## Cleanup

  * ~~lots of "dynamic" methods. Afaik FPC doesn't implement it (always virtual iirc), since it was mainly needed in 16-bit envs.~~ Fixed in r13382
  * ifdef usedynarrays in sax.pp is so 1.0.x



## See also

  * [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/fcl-xml](https://web.archive.org/web/20240506084533/https://wiki.freepascal.org/fcl-xml)_
