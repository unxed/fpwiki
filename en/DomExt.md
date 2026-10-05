# DomExt

## Contents

  * 1 [This entry is obsolete but kept for documentation. Simply use fcl-xml from the standard packages.]
  * 2 About
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies
  * 8 Status
  * 9 Installation and usage
  * 10 Contact



### [This entry is obsolete but kept for documentation. Simply use fcl-xml from the standard packages.]

### About

DomExt unit extends FPC implementation of XML DOM API with set of functions to simplify XML node manipulation. Its functionality is inspired by [SimpleXML Library by Michael Vlasov](<http://www.torry.ru/authorsmore.php?id=4012>)

Its main characteristics are: 

  * Type conversion when working with node and attribute values. E.g. DomReadIntAttr returns attribute value as Integer. This is similar to TIniFile API. Supported data types include String, Integer, Boolean, Double and TDateTime. Non-string data types are coverted to string representation using [XSD primitive data types syntax](<http://www.w3.org/TR/xmlschema-2/#built-in-primitive-datatypes>).
  * Handling default node and attribute values when reading, similar to TIniFile API.
  * Encoding conversion when working with node and attribute names and values. You may need this feature in LCL projects because FCL dom.pp uses UTF-16 (widestring) and LCL uses UTF-8.
  * Flexible child node handling that helps to avoid excessive Assigned(...) checks. DomNeedChild function is guaranteed to return a non-nil node or raise an exception if requested node was not found. DomEnsureChild creates requested node if it does not exist. If you need to check node existence you can use DomGetChild that returns nil if requested child node was not found. All those functions allow to request not only direct descendant nodes but also deeply nested nodes using XPath-like syntax, e.g. 'childLevel1/childLevel2'.
  * Ability to save/load StringLists to/from XML node lists.
  * Ability to read/write node content as XML fragment, similar to innerHTML property in JavaScript.



### Author

Vladimir Zhirov 

### License

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). 

### Download

The latest stable release can be found on _link to the lazarus-ccr sf download location_. 

### Change Log

Version 0.1 _2010-08-25_
    
    
     * initial release
    

### Dependencies

The unit is a single Pascal source file that only depends on FPC RTL and FCL. 

### Status

Beta 

Issues: 

  * Since the unit was primarily written for use in LCL applications, widestring mode ("transparent", without string conversions) is completely untested. To use it one can comment out {$define UTF8Strings} compiler directive.
  * Conversion between TDateTime and String does not fully obey XSD specs, both because TDateTime limitations and implementation details. Thus, years beyond 1..9999 range, fractional seconds and time zones are not supported.



### Installation and usage

Include unit DomExt in the _uses_ section. Set path for it in project settings if needed. 

### Contact

Please send bug reports and patches to the e-mail you find in the unit source code.

---

_Source: [https://wiki.freepascal.org/DomExt](https://web.archive.org/web/20250417154646/https://wiki.freepascal.org/DomExt)_
