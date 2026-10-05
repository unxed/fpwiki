# fpdbfexport

│ **English (en)** │  **[français (fr)](</fpdbfexport/fr> "fpdbfexport/fr")** │    
****

## Contents

  * 1 Overview
    * 1.1 FPC
    * 1.2 Lazarus
  * 2 Limitations and known bugs
  * 3 Example
  * 4 See also



## Overview

### FPC

The fpdbfexport module is part of [fpDBExport](<fpDBExport.md> "fpDBExport") in the FreePascal FCL database modules. 

This export module provides export to DBF formats (e.g. DBase IV format). The format is still fairly popular because it is simple, and - opposed to e.g. [CSV](<CSV.md> "CSV") \- is clear about what data type is enclosed in its fields. Many spreadsheet and database programs can import DBF files. 

### Lazarus

In Lazarus, the DBF export component is located on the _Data Export_ tab. 

[![DataExportTab.png](https://wiki.freepascal.org/images/6/69/DataExportTab.png)](</File:DataExportTab.png>)

## Limitations and known bugs

In FPC before 2.6.4/Lazarus before 1.2.2, exporting to e.g. dbase IV format - that has a 10 character field length limit - of fields with longer field names will throw an error unless you populate the export fieldname mapping yourself. 

## Example

See the examples in your FPC source directory $(fpcdir)\source\packages\fcl-db\tests (see [Databases#Running_FPC_database_tests](<Databases.md> "Databases")), specifically testdbexport.pas. 

The various export formats are run in 

  * TestDBFExport_DBaseIV
  * TestDBFExport_DBaseVII
  * TestDBFExport_FoxPro
  * TestDBFExport_VisualFoxpro



etc. 

The procedure TTestDBExport.GenericExportTest shows how you can export to only supported field types (handy if you do not know in advance what field types are used in a dataset). 

Another useful example is the the dbftool example included in FPC 2.6.4+: 

  * creating DBF files
  * using DBF files
  * exporting data (to DBF and other formats) using db export



The source is also downloadable via <http://svn.freepascal.org/svn/fpc/trunk/packages/fcl-db/examples/>

## See also

  * [Lazarus Tdbf Tutorial](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") Tutorial and documentation on TDBF, the DBF support in FPC SQLDB (which is also used in Lazarus). Link to official TDBF documentation.
  * [fpDBExport](<fpDBExport.md> "fpDBExport")
  * [fpXMLXSDExport](<fpXMLXSDExport.md> "fpXMLXSDExport") Similar module that exports to various XML formats.

---

_Source: [https://wiki.freepascal.org/fpdbfexport](https://web.archive.org/web/20230130094402/https://wiki.freepascal.org/fpdbfexport)_
