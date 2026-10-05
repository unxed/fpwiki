# lazdbexport

│ **English (en)** │

**lazdbexport** is a package that supplies components to facilitate database export. It is delivered with lazarus and may be installed by using [Package|Install/Uninstall packages]. After install the components are accessible via the Data Export tab. It provides a template class for descendants that can provide export of datasets. Also included are various ready-made descendants for: 

icon | Component | Description   
---|---|---  
[![tcsvexporter.png](https://wiki.freepascal.org/images/1/1b/tcsvexporter.png)](</File:tcsvexporter.png>) | [TCSVExporter](<TCSVExporter.md> "TCSVExporter") | CSV   
[![tfixedlengthexporter.png](https://wiki.freepascal.org/images/0/0c/tfixedlengthexporter.png)](</File:tfixedlengthexporter.png>) | [TFixedlengthExporter](</index.php?title=TFixedlengthExporter&action=edit&redlink=1> "TFixedlengthExporter \(page does not exist\)") | Fixed length   
[![tsqlexporter.png](https://wiki.freepascal.org/images/e/e7/tsqlexporter.png)](</File:tsqlexporter.png>) | [TSQLExporter](<TSQLExporter.md> "TSQLExporter") | SQL   
[![txmlxsdexporter.png](https://wiki.freepascal.org/images/4/4a/txmlxsdexporter.png)](</File:txmlxsdexporter.png>) | [TXMLXSDExporter](<fpXMLXSDExport.md> "fpXMLXSDExport") | XML/XSD   
[![tsimplexmlexporter.png](https://wiki.freepascal.org/images/1/17/tsimplexmlexporter.png)](</File:tsimplexmlexporter.png>) | [TSimpleXMLExporter](</index.php?title=TSimpleXMLExporter&action=edit&redlink=1> "TSimpleXMLExporter \(page does not exist\)") | XML   
[![tsimplejsonexporter.png](https://wiki.freepascal.org/images/f/f0/tsimplejsonexporter.png)](</File:tsimplejsonexporter.png>) | [TSimpleJSONExporter](</index.php?title=TSimpleJSONExporter&action=edit&redlink=1> "TSimpleJSONExporter \(page does not exist\)") | json   
[![tfpdbfexport.png](https://wiki.freepascal.org/images/0/0a/tfpdbfexport.png)](</File:tfpdbfexport.png>) | [TFPDbfExport](<fpdbfexport.md> "fpdbfexport") | dbf   
[![ttexexporter.png](https://wiki.freepascal.org/images/d/d0/ttexexporter.png)](</File:ttexexporter.png>) | [TTeXExporter](</index.php?title=TTeXExporter&action=edit&redlink=1> "TTeXExporter \(page does not exist\)") | TeX   
[![trtfexporter.png](https://wiki.freepascal.org/images/3/37/trtfexporter.png)](</File:trtfexporter.png>) | [TRTFExporter](</index.php?title=TRTFExporter&action=edit&redlink=1> "TRTFExporter \(page does not exist\)") | RTF   
[![tstandardexportformats.png](https://wiki.freepascal.org/images/6/65/tstandardexportformats.png)](</File:tstandardexportformats.png>) | [TStandardExportFormats](</index.php?title=TStandardExportFormats&action=edit&redlink=1> "TStandardExportFormats \(page does not exist\)") |   
[![tfpdataexporter.png](https://wiki.freepascal.org/images/0/0f/tfpdataexporter.png)](</File:tfpdataexporter.png>) | [TFPDataExporter](</index.php?title=TFPDataExporter&action=edit&redlink=1> "TFPDataExporter \(page does not exist\)") |   
  
As indicated, developers can write their own export classes using the export framework. An example of this is the Excel/spreadsheet format exporter in [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet")

## Import

There is no corresponding import to dataset code in FPC/Lazarus, but there is third party code like dbimport (<https://bitbucket.org/reiniero/smalltools/src>, directory dbimport). 

DbImport is used in the [LazSQLX](<http://lazsqlx.wordpress.com/>) and [TurboBird](<http://code.sd/products/turbobird/>) database management tools for importing CSV (like) data into datasets. 

Another form of import is the use of [TSQLScript](<TSQLScript.md> "TSQLScript"); an exported database with [TSQLExporter](<TSQLExporter.md> "TSQLExporter") can be imported with the use of [TSQLScript](<TSQLScript.md> "TSQLScript"). 

## Example

See the examples in your FPC source directory $(fpcdir)\source\packages\fcl-db\tests (see [Running FPC database tests](<Databases.md> "Databases")), specifically testdbexport.pas. 

Following is a simplest form of using TCSVExporter. **fpcsvexport** must be included in uses clause. 
    
    
     if SaveDialog1.Execute then 
        with TCSVExporter.Create(nil) do begin
           Dataset:= BufDataSet1;
           FileName:= SaveDialog1.FileName;
           Execute;
           Free;
        end;

---

_Source: [https://wiki.freepascal.org/lazdbexport](https://web.archive.org/web/20250122034526/https://wiki.freepascal.org/lazdbexport)_
