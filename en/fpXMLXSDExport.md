# fpXMLXSDExport

│ **English (en)** │  **[français (fr)](</fpXMLXSDExport/fr> "fpXMLXSDExport/fr")** │    
****

## Contents

  * 1 Overview
    * 1.1 FPC
    * 1.2 Lazarus
  * 2 Export formats
    * 2.1 AccessCompatible
    * 2.2 ADONETCompatible
    * 2.3 ExcelCompatible
    * 2.4 DelphiClientDataset
  * 3 Other settings
  * 4 Lazarus and examples
  * 5 License
  * 6 See also



## Overview

### FPC

The fpXMLXSDExport module is part of [fpDBExport](<fpDBExport.md> "fpDBExport") in the FreePascal FCL database modules. 

This export module provides export to various forms of [XML](<XML.md> "XML"), selectable by the ExportFormat setting. Note that not exporting indexes appears as a limitation in most formats below, but indexes probably will probably need to be changed anyway in an new environment/database. 

There is also a [fpsimplexmlexport](</index.php?title=fpsimplexmlexport&action=edit&redlink=1> "fpsimplexmlexport \(page does not exist\)") module which can export a single format XML. 

### Lazarus

The component is located on the _Data Export_ tab. 

[![DataExportTab.png](https://wiki.freepascal.org/images/6/69/DataExportTab.png)](</File:DataExportTab.png>)

## Export formats

This component supports various XML formats. 

### AccessCompatible

This format generates Microsoft Access XP/2002-2010 (and presumably SQL Server) compatible XML. 

You can specify to include or exclude inline XSD data definition with the CreateXSD setting. Recommended to leave it to true so Access can build up the table structure. If you have an existing compatible table, you wouldn't need the XSD. 

Index definitions are exported in this format. 

The Access format as emitted by Access XP at least uses the decimal separator of the computer the export is running on. E.g. a US locale will emit 42.42, while a Dutch locale will emit 42,42. If you want to override the decimal separator to a value regardless of locale (recommended if exchanging data across multiple locales), please set the DecimalSeparator (e.g. to '.'). 

Limitations: 

  * Import of BLOB/binary type data in Access is problematic as Access has no real binary datatype, only an OLE datatype. Access stores OLE metadata, after that the binary object. When exporting to XML, Access stores the entire packet as base64 data. We cannot replicate this functionality, so we just base64 encode the BLOB, and lose the OLE metadata. If you want to, in Access, you can probably programmatically read the data as blob data and copy it to a "real" OLE field. See e.g. <http://support.microsoft.com/?kbid=103257> for hints on how to save the contents of the export BLOB to a file.
  * Date/time field data outside of Access limits are discarded (a NULL is inserted).
  * GUIDs are exported as text as Access has no equivalent field type.
  * Large numeric values may overflow Access import capabilities. Patches welcome.



### ADONETCompatible

This format generates XML compatible with the .Net framework (specifically the ADO.NET data access libraries). It should work for versions 2 to 4; version 1/1.1 has not been tested. 

In this format you can also specify XSD or no XSD using the CreateXSD setting. This output format is fairly generic and could be usable for import in other applications, as well. 

Limitations: 

  * Index definitions are not exported in this format.



### ExcelCompatible

This format generates Microsoft Excel XP/2002-2010 compatible XML. 

This format recreates the data as Excel worksheet data. It does not include formatting and formulas, just plain data. 

The CreateXSD setting has no effect in this format - the Excel XML format doesn't seem to support compatible metadata. 

Limitations: 

  * Index definitions are not exported in this format.
  * Binary data is not supported in the Excel XML format.
  * The Boolean (true/false) data format is not supported in this Excel XML format; instead 0/1 integers are exported.
  * Line endings within text cells/fields (generated in Excel by pressing Alt-Enter) are not supported as we'd need to output in the XML which is not possible using current FCL XML libraries.



The [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet") code could be a good alternative to this format: it offers native format output and probably multiline support. 

### DelphiClientDataset

This format generates Delphi ClientDataset compatible XML. 

This format allows Delphi applications to use the ClientDataset.LoadFromFile method to read the data. 

It has been written based on TurboDelphi (Delphi 2006) behaviour and exports UTF-8 encoded XML. 

The CreateXSD setting has no effect in this format - Clientdataset metadata is always exported. 

Limitations: 

  * Index definitions are not exported in this format.
  * Variants are exported as binary data; patches for conversion are welcome.
  * Apparently, ClientDatasets have insert new data and update data modes; as of now, only the insert new data mode is supported.



## Other settings

As the XML formats used above defines how date/time formats, Boolean formats etc are used, these general export settings have no effect: 

  * BooleanFalse
  * BooleanTrue
  * DateFormat
  * DateTimeFormat
  * DecimalSeparator (only has effects in Access compatible export mode, no effect in other modes)
  * CurrencyDigits
  * CurrencySymbol
  * IntegerFormat
  * TimeFormat



## Lazarus and examples

For an example, please see the xmlxsdexporttest.lpr test program in packages\fcl-db\tests. 

An additional example can be found in your FPC installatoin in packages\fcl-db\examples\dbftool.lpr. 

In Lazarus, you can use the lazdbexport package (the **TFPDataExporter** and **TStandardExportFormats** components in particular) to use this method, or the **TXMLXSDExport** component. 

## License

The fpXMLXSDExport module is freeware, licensed under the MIT license: all use free, but no liability accepted. It is also licensed under the license used for the FreePascal Free Component Library (FCL), so take your pick. 

## See also

  * [fpDBExport](<fpDBExport.md> "fpDBExport")
  * [fpdbfexport](<fpdbfexport.md> "fpdbfexport") Similar module that exports to DBF/(Visual) FoxPro formats.

---

_Source: [https://wiki.freepascal.org/fpXMLXSDExport](https://web.archive.org/web/20250323145310/https://wiki.freepascal.org/fpXMLXSDExport)_
