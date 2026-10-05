# SQLDBRestSchemaEditor

## Contents

  * 1 Introduction
  * 2 Workings
  * 3 Resource editor
  * 4 Field editor
  * 5 Fields editor



## Introduction

The SQLDBRestBridge schema editor allows you to edit schema files for a [SQLDBRestBridge](<SQLDBRestBridge.md> "SQLDBRestBridge") schema. 

The editor is implemented in the sqldbrestschemadesigner.lpk package and included in the Lazarus IDE if you register the lazsqldbrest.lpk package. It can also be compiled as a standalone application, the application is located in the editor project under the components/sqldbrestb directory. 

The standalone editor can be used to create schema files, which can be used to load schemas using the **LoadFromFile** method. 

## Workings

The schema editor has one form, which looks as follows when a schema is loaded. 

  
[![sqldbrestschemaeditor-schema.png](https://wiki.freepascal.org/images/0/01/sqldbrestschemaeditor-schema.png)](</File:sqldbrestschemaeditor-schema.png>)

The left side _Available connections_ shows the known connections, and for each connection to a database, the tables in the database can be shown. Using drag & drop, the tables (you can select multiple tables at once) can be dragged to the middle part of the screen. The known connections can be hidden using the button above the tree. 

  
The middle part of the screen _Available Resources_ shows the resources defined in the schema. You can add new resources by dragging them from the connections, or using the 'Add resource' button. 

  
The right part of the screen shows the details of the item selected in the _Available resources_. This can be one of 4 details 

  1. When the schema node is selected, the list of tables is shown (see the above screenshot). You can double click a resource, and the selected resource will be shown for editing.
  2. When a resource node is selected, the resource editor is shown (see below)
  3. When the 'fields' node below a resource node is selected, the list of fields is shown. You can double click a field, and the selected field will be shown for editing.
  4. When a field node is selected, the field editor is shown.



Changes to a current node are automatically saved when selecting another node. 

A double click on a selected node will show the same editor as selecting a node, but in a modal dialog. 

  
You can add new resources by dragging them from the connections, or using the 'Add resource' button. The 'expose connection' above the list of connections will add all tables of a connection to the schema. 

The menu allows several actions: 

[![sqldbrestschemaeditor-menu.png](https://wiki.freepascal.org/images/e/e7/sqldbrestschemaeditor-menu.png)](</File:sqldbrestschemaeditor-menu.png>)

  


  1. **Read connections** Loads a list of connection details from an .ini file.
  2. **Write connections** Saves the list of connections shown in the left panel to an .ini file.
  3. **New schema** Creates a new empty schema.
  4. **Load schema** Loads a schema file and displays the resources.
  5. **Save schema** Saves the resources to a schema file.
  6. **Recent schemas** a MRU menu with recently loaded or saved schema files.
  7. **Quit** exits the program.



A schema file has the .srs extension, but this is not obligatory. It is a JSON file created by the SaveToFile method of the TSQLDBRestSchema class, and can be loaded with the LoadFromFile method. 

  


## Resource editor

The resource editor is shown when a resource is selected in the available resources tree. It looks as follows: 

[![sqldbrestschemaeditor-table.png](https://wiki.freepascal.org/images/d/de/sqldbrestschemaeditor-table.png)](</File:sqldbrestschemaeditor-table.png>)

The various edits correspond to the properties of the TSQLDBRestResource class. 

The 4 tabs at the bottom can be used to edit the SQL Statements. 

The _SELECT_ statement can be automatically generated from the table name. The entered statement can also be validated, to check if the SQL is correct. When the SQL SELECT Statement has changed, the list of published fields can be automatically refreshed using the 'Update Fields' button. This will reset the fields list, so any adjustments that were made to the list of fields will be lost. 

The last tab displays the list of fields defined for the resource. 

[![sqldbrestschemaeditor-table-fields.png](https://wiki.freepascal.org/images/2/2c/sqldbrestschemaeditor-table-fields.png)](</File:sqldbrestschemaeditor-table-fields.png>)

This list should be identical to the list in the treeview. Double-clicking a field will display the field editor. 

  


## Field editor

The field editor shows the properties of a selected field. These are the properties of the TSQLDBRestField class. It looks as follows: 

[![sqldbrestschemaeditor-field.png](https://wiki.freepascal.org/images/7/70/sqldbrestschemaeditor-field.png)](</File:sqldbrestschemaeditor-field.png>)

## Fields editor

The fields editor is shown when the 'Fields' node is selected in the resource tree. It is currently read-only, and looks as follows: 

[![sqldbrestschemaeditor-fields.png](https://wiki.freepascal.org/images/7/75/sqldbrestschemaeditor-fields.png)](</File:sqldbrestschemaeditor-fields.png>)

Double clicking a field will take you to the field editor for the selected field.

---

_Source: [https://wiki.freepascal.org/SQLDBRestSchemaEditor](https://web.archive.org/web/20210617204741/https://wiki.freepascal.org/SQLDBRestSchemaEditor)_
