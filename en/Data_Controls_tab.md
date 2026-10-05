# Data Controls tab

│ **English (en)** │  **[русский (ru)](<../ru/Data_Controls_tab.md>)** │

---  
[**Databases portal**](<Portal_Databases.md> "Portal:Databases")  
References: 

  * [General info](<Databases.md> "Databases")
  * [Libraries](<Database_libraries.md> "Database libraries")
  * [Field types](<Database_field_type.md> "Database field type")
  * Controls
  * [FAQ](<Lazarus_DB_Faq.md> "Lazarus DB Faq")
  * [SQL how-to](<SqlDBHowto.md> "SqlDBHowto")
  * [Working With TSQLQuery](<Working_With_TSQLQuery.md> "Working With TSQLQuery")
  * [In-memory database applications](<How_to_write_in-memory_database_applications_in_Lazarus/FPC.md> "How to write in-memory database applications in Lazarus/FPC")

Tutorials/practical articles: 

  * [Overview](<Lazarus_Database_Overview.md> "Lazarus Database Overview")
  * [0 - Database set-up](<SQLdb_Tutorial0.md> "SQLdb Tutorial0")
  * [1 - Getting started](<SQLdb_Tutorial1.md> "SQLdb Tutorial1")
  * [2 - Editing](<SQLdb_Tutorial2.md> "SQLdb Tutorial2")
  * [3 - Queries](<SQLdb_Tutorial3.md> "SQLdb Tutorial3")
  * [4 - Data modules](<SQLdb_Tutorial4.md> "SQLdb Tutorial4")
  * [SQLdb Programming Reference](<SQLdb_Programming_Reference.md> "SQLdb Programming Reference")

Databases  


    [Advantage](<Advantage_Database_Server.md> "Advantage Database Server") \- [MySQL](<MySQLDatabases.md> "MySQLDatabases") \- [MSSQL](<mssqlconn.md> "mssqlconn") \- [Postgres](<postgres.md> "postgres") \- [Interbase](<Firebird.md> "Firebird") \- [Firebird](<Firebird.md> "Firebird") \- [Oracle](<Oracle.md> "Oracle") \- [ODBC](<ODBCConn.md> "ODBCConn") \- [Paradox](<TParadox.md> "TParadox") \- [SQLite](<SQLite.md> "SQLite") \- [dBASE](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial") \- [MS Access](<MS_Access.md> "MS Access") \- [Zeos](<Zeos_tutorial.md> "Zeos tutorial")  
  
The **Data controls tab** of the [Component Palette](<Component_Palette.md> "Component Palette") contains database-related control components. 

[![Component Palette Data Controls.png](https://wiki.freepascal.org/images/7/75/Component_Palette_Data_Controls.png)](</File:Component_Palette_Data_Controls.png>)

Icon | Component | Description | Online Docs   
---|---|---|---  
[![tdbnavigator.png](https://wiki.freepascal.org/images/9/95/tdbnavigator.png)](</File:tdbnavigator.png>) | [TDBNavigator](<TDBNavigator.md> "TDBNavigator") | navigation control for use with a connected database  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbnavigator.html>)  
[![tdbtext.png](https://wiki.freepascal.org/images/9/9d/tdbtext.png)](</File:tdbtext.png>) | [TDBText](<TDBText.md> "TDBText") | text contents for a [TDataSet](<TDataSet.md> "TDataSet") field value, not editable  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbtext.html>)  
[![tdbedit.png](https://wiki.freepascal.org/images/9/9f/tdbedit.png)](</File:tdbedit.png>) | [TDBEdit](<TDBEdit.md> "TDBEdit") | single-line edit control for a [TDataSet](<TDataSet.md> "TDataSet") field value  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbedit.html>)  
[![tdbmemo.png](https://wiki.freepascal.org/images/1/13/tdbmemo.png)](</File:tdbmemo.png>) | [TDBMemo](<TDBMemo.md> "TDBMemo") | multi-line edit control for a [TDataSet](<TDataSet.md> "TDataSet") field value  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbmemo.html>)  
[![tdbimage.png](https://wiki.freepascal.org/images/d/d6/tdbimage.png)](</File:tdbimage.png>) | [TDBImage](<TDBImage.md> "TDBImage") | image stored in a field of a [TDataSet](<TDataSet.md> "TDataSet") | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbimage.html>)  
[![tdblistbox.png](https://wiki.freepascal.org/images/7/73/tdblistbox.png)](</File:tdblistbox.png>) | [TDBListBox](<TDBListBox.md> "TDBListBox") | list with several string items, the selected item corresponds to a [TDataSet](<TDataSet.md> "TDataSet") field  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdblistbox.html>)  
[![tdblookuplistbox.png](https://wiki.freepascal.org/images/c/cf/tdblookuplistbox.png)](</File:tdblookuplistbox.png>) | [TDBLookupListBox](<TDBLookupListBox.md> "TDBLookupListBox") | similar to [TDBListBox](<TDBListBox.md> "TDBListBox"), but the string items are taken from a lookup [TDataSet](<TDataSet.md> "TDataSet") | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdblookuplistbox.html>)  
[![tdbcombobox.png](https://wiki.freepascal.org/images/a/a2/tdbcombobox.png)](</File:tdbcombobox.png>) | [TDBComboBox](<TDBComboBox.md> "TDBComboBox") | dropdown list with several options, the selected item corresponds to a [TDataSet](<TDataSet.md> "TDataSet").  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbcombobox.html>)  
[![tdblookupcombobox.png](https://wiki.freepascal.org/images/e/ed/tdblookupcombobox.png)](</File:tdblookupcombobox.png>) | [TDBLookupComboBox](<TDBLookupComboBox.md> "TDBLookupComboBox") | similar to [TDBComboBox](<TDBComboBox.md> "TDBComboBox"), but the string items are taken from a lookup [TDataSet](<TDataSet.md> "TDataSet") | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdblookupcombobox.html>)  
[![tdbcheckbox.png](https://wiki.freepascal.org/images/3/33/tdbcheckbox.png)](</File:tdbcheckbox.png>) | [TDBCheckBox](<TDBCheckBox.md> "TDBCheckBox") | checkbox corresponding with a [TDataSet](<TDataSet.md> "TDataSet") field value  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbcheckbox.html>)  
[![tdbradiogroup.png](https://wiki.freepascal.org/images/3/3d/tdbradiogroup.png)](</File:tdbradiogroup.png>) | [TDBRadioGroup](<TDBRadioGroup.md> "TDBRadioGroup") | group of [TRadioButtons](<TRadioButton.md> "TRadioButton"), the selected index corresponds to a [TDataSet](<TDataSet.md> "TDataSet") field.  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbradiogroup.html>)  
[![tdbcalendar.png](https://wiki.freepascal.org/images/b/b5/tdbcalendar.png)](</File:tdbcalendar.png>) | [TDBCalendar](<TDBCalendar.md> "TDBCalendar") | date from the [TDataSet](<TDataSet.md> "TDataSet") displayed in a month calendar  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbcalendar.html>)  
[![tdbgroupbox.png](https://wiki.freepascal.org/images/e/e0/tdbgroupbox.png)](</File:tdbgroupbox.png>) | [TDBGroupBox](<TDBGroupBox.md> "TDBGroupBox") | container for other controls. Data field information is displayed in header of the box.  | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbctrls/tdbgroupbox.html>)  
[![tdbgrid.png](https://wiki.freepascal.org/images/6/66/tdbgrid.png)](</File:tdbgrid.png>) | [TDBGrid](<TDBGrid.md> "TDBGrid") | grid displaying fields of several records of s [TDataSet](<TDataSet.md> "TDataSet") | [Link](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbgrids/tdbgrid.html>)  
[![tdbdatetimepicker.png](https://wiki.freepascal.org/images/7/7f/tdbdatetimepicker.png)](</File:tdbdatetimepicker.png>) | [TDBDateTimePicker](<TDBDateTimePicker.md> "TDBDateTimePicker") | date/time value editor for a [TDataSet](<TDataSet.md> "TDataSet") field  | (no LCL control)   
  
  


[Component Palette](<Component_Palette.md> "Component Palette")  
---  
Standard/ja \- Additional/ja \- Common Controls/ja \- Dialogs/ja \- Data Controls/ja \- Data Access/ja \- [System](<System_tab.md> "System tab") \- [Misc](<Misc_tab.md> "Misc tab") \- [LazControls](<LazControls_tab.md> "LazControls tab") \- [RTTI](<RTTI_tab.md> "RTTI tab") \- [SQLdb](<SQLdb_tab.md> "SQLdb tab") \- [Pascal Script](<Pascal_Script_tab.md> "Pascal Script tab") \- [SynEdit](<SynEdit_tab.md> "SynEdit tab") \- [Chart](<Chart_tab.md> "Chart tab") \- [IPro](<IPro_tab.md> "IPro tab")

---

_Source: [https://wiki.freepascal.org/Data_Controls_tab](https://web.archive.org/web/20241104221730/https://wiki.freepascal.org/Data_Controls_tab)_
