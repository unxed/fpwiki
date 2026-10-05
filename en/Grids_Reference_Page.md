# Grids Reference Page

│ **[Deutsch (de)](</Grids_Reference_Page/de> "Grids Reference Page/de")** │  **English (en)** │  **[español (es)](</Grids_Reference_Page/es> "Grids Reference Page/es")** │  **[polski (pl)](</Grids_Reference_Page/pl> "Grids Reference Page/pl")** │  **[русский (ru)](<../ru/Grids_Reference_Page.md> "Grids Reference Page/ru")** │    
****

## Contents

  * 1 Objective
  * 2 Overview
  * 3 Inheritance Tree
  * 4 A Starting Example
  * 5 Differences between Lazarus and Delphi grids
    * 5.1 Differences
    * 5.2 New Functionality
    * 5.3 Ways in which you can make a Lazarus grid more Delphi-compatible
  * 6 Grids Reference
    * 6.1 Information
    * 6.2 TCustomGrid
      * 6.2.1 property AllowOutboundEvents
      * 6.2.2 property Columns
      * 6.2.3 property AutoFillColumns
      * 6.2.4 procedure SaveToFile(AFileName: String);
      * 6.2.5 procedure LoadFromFile(AFileName: String);
    * 6.3 TCustomDBGrid
      * 6.3.1 procedure AutoAdjustColumns
      * 6.3.2 procedure InplaceEditor
    * 6.4 TCustomStringGrid
      * 6.4.1 procedure AutoSizeColumn(aCol: Integer);
      * 6.4.2 procedure AutoAdjustColumns or AutoSizeColumns
      * 6.4.3 procedure Clean; overload;
      * 6.4.4 procedure Clean(CleanOptions: TGridZoneSet); overload;
      * 6.4.5 procedure Clean(StartCol,StartRow,EndCol,EndRow: integer; CleanOptions:TGridZoneSet); overload;
      * 6.4.6 procedure Clean(aRect: TRect; CleanOptions: TGridZoneSet); overload;
      * 6.4.7 procedure SaveToCSVFile(AFileName: string; ADelimiter:Char=','; WithHeader:boolean=true);
      * 6.4.8 procedure LoadFromCSVFile(AFileName: string; ADelimiter:Char=','; WithHeader:boolean=true);
      * 6.4.9 property Cols[index: Integer]: TStrings read GetCols write SetCols;
        * 6.4.9.1 Examples
        * 6.4.9.2 Notes.
      * 6.4.10 property Rows[index: Integer]: TStrings read GetRows write SetRows;
        * 6.4.10.1 Notes.
        * 6.4.10.2 Examples
      * 6.4.11 property UseXORFeatures;
    * 6.5 TValueListEditor
      * 6.5.1 Property DisplayOptions
      * 6.5.2 Property TitleCaptions
      * 6.5.3 Property Strings
      * 6.5.4 Property ItemProps
      * 6.5.5 Property KeyOptions
      * 6.5.6 Property DropDownRows
      * 6.5.7 Function DeleteRow
      * 6.5.8 Function InsertRow
      * 6.5.9 Function IsEmptyRow
      * 6.5.10 Function FindRow
      * 6.5.11 Function RestoreCurrentRow
      * 6.5.12 Altered behaviour of some properties derived from TCustomStringGrid
        * 6.5.12.1 Property Options
        * 6.5.12.2 Property FixedRows
        * 6.5.12.3 Property ColCount
        * 6.5.12.4 Property Objects
      * 6.5.13 General comments on the use of TValueListEditor
      * 6.5.14 Saving and loading the contenst of a TValueListEditor
        * 6.5.14.1 Using the Strings property
        * 6.5.14.2 Using TValueList.SaveToFile and TValueList.LoadFromFile
        * 6.5.14.3 Using SaveToCSVFile and LoadFromCSV methods
  * 7 Working with grids
    * 7.1 Customizing grids
    * 7.2 Properties and Events for customizing grids
    * 7.3 Description of grid's drawing process
      * 7.3.1 Differences with Delphi
    * 7.4 Grid's cell selection
      * 7.4.1 Differences with Delphi
    * 7.5 When built-in properties are not enough: derived grids
    * 7.6 What happens in the TCustomGrid.Paint method?
      * 7.6.1 Additional methods to draw in TCustomGrid
      * 7.6.2 Drawing methods introduced by TCustomDrawGrid
    * 7.7 Save and Retrieve Grid Content
    * 7.8 Grid Cell Editors
      * 7.8.1 Builtin cell editors
      * 7.8.2 Using cell editors
      * 7.8.3 Description of editor styles
    * 7.9 Editing grids
      * 7.9.1 Options and properties that affect editing
    * 7.10 Cell hints
  * 8 Grids Howto and Examples
    * 8.1 Focusing a cell
    * 8.2 Example: How to set a custom cell editor
    * 8.3 Example: How to add a button editor
    * 8.4 Example: Working with Picklist, How to make it read only and How to fill it at run time
    * 8.5 Aligning text in StringGrids
    * 8.6 Multilines in Grids
    * 8.7 Validating Entered Values
    * 8.8 Sorting Columns or Rows
    * 8.9 Override automatic sort (ColumnClickSorts set to False). Avoid sort of some columns. Show sort arrows in column header
    * 8.10 Highlighting the selected cell column and row
    * 8.11 Avoiding the empty space at the right of the last column
    * 8.12 How to scroll a Grid in "realtime" (moving the scrollbar via mouse)
    * 8.13 How to resize a row based on cells content
  * 9 DBGrids Howto and Examples
    * 9.1 Example: How to set a memo editor for dbgrids
    * 9.2 Example: Avoid displaying text fields as "(Memo)" for DBGrids
    * 9.3 Sorting columns or rows in DBGrid with sort arrows in column header
    * 9.4 Selecting Records in a DBGrid using checkboxes
    * 9.5 Example: How to export a DBGrid to a text file



## Objective

This text will try to show the user some aspects of the grids components in Lazarus. It is also intended to serve as a guide for users who have never used grids before (as experienced users generally only need a reference for new functionality). This text will therefore try to reach the following objectives: 

  1. To introduce the grids components to people with little or no previous Delphi experience.
  2. To document the differences with respect to Delphi grids components.
  3. To document the new functionality in Lazarus grids.
  4. Create reference and examples for the components.



## Overview

A grid is a component that provides a means for displaying data in tabular format. The most obvious characteristic of grids is that they are composed of cells forming rows and columns. 

The type of information that can be shown in a grid varies and mainly depends on what the user wants to show. Generally this information consists of text, colors, images or a combination of those three. 

Given the great variety of information that can be represented, a series of grids exist whose purpose is to facilitate the user in showing specific kinds of information. For instance, there is a grid designed to show text: the **StringGrid**. Documentation for that grid can be found [here](<http://lazarus-ccr.sourceforge.net/docs/lcl/grids/tstringgrid.html>)

## Inheritance Tree
    
    
                         [TCustomControl](</index.php?title=TCustomControl&action=edit&redlink=1> "TCustomControl \(page does not exist\)")           
                                 |                    
                                 |                    
                            TCustomGrid               
                                 |                    
                   +-------------+------------+       
                   |                          |       
             TCustomDrawGrid             TCustomDbGrid
                   |                          |       
          +--------+--------+                 |       
          |                 |              [TDBGrid](<TDBGrid.md> "TDBGrid")    
      [TDrawGrid](<TDrawGrid.md> "TDrawGrid")      TCustomStringGrid                
                            | 
                   ---------+----------
                   |                  |    
             [TStringGrid](<TStringGrid.md> "TStringGrid")      [TValueListEditor](<TValueListEditor.md> "TValueListEditor")
    

## A Starting Example

As one of the objectives of this page is to help people with little or no previous Lazarus knowledge let's do a quick starting example of grids in action. Why not, let's make a traditional "hello world" example using the TStringGrid Component. 

  1. Create a new application. 
     * From the main menu select: project->New Project
     * In the Create New Project dialog press the "Create Button"
     * A new empty form will be shown.
  2. Place a grid on the form 
     * From the component palette select the "additional" tab
     * Click over the TStringGrid icon [![tstringgrid.png](https://wiki.freepascal.org/images/c/c4/tstringgrid.png)](</File:tstringgrid.png>)
     * Click over the form, near to the top left corner. A new empty grid appears.
  3. Place a button on the form 
     * From the component palette select the "Standard" tab
     * Click over the TButton icon [![tbutton.png](https://wiki.freepascal.org/images/b/b2/tbutton.png)](</File:tbutton.png>)
     * Click over a empty area of the form. A new button appears.
  4. Doubleclick the button from step 3, and write down the following code in the click button handler: 
     * StringGrid1.Cells[1,1] := 'Hi World!';
           

  5. Run the program by clicking the play icon [![menu run.png](https://wiki.freepascal.org/images/0/0d/menu_run.png)](</File:menu_run.png>)
     * by pressing the Button1, the hello world text should appear in cell column 1, row 1. (note that left column and top row are 0)



## Differences between Lazarus and Delphi grids

The current grid components differ from Delphi grids in several ways. When first developed the Lazarus grids were created from scratch without any attempt to make them fully Delphi-compatible. 

At a later stage, compatibility with Delphi's grids became a desired objective and the Lazarus grids started to conform more closely to the Delphi grid interface. However even this was done without attempting to make every single Lazarus grid property or method match its Delphi counterpart. Also (because Lazarus grid internals are very different from Delphi grid internals) some Delphi functionality is not possible or needs to be emulated differently in a Lazarus grid from how it would be done in a Delphi grid. We have achieved greater Delphi compatibility as the Lazarus grids have evolved, and this is desirable. 

### Differences

Known differences are listed below, in no special order. 

  * Cell Editors
  * Designtime Behaviour
  * Cell Drawing has some differences, see customizing grid section.



### New Functionality

  * Custom Columns
  * Events
  * Grid Editor



### Ways in which you can make a Lazarus grid more Delphi-compatible

You can make a Lazarus grid look like and behave like a corresponding Delphi. Listed below are property settings which will achieve this. These adjustments are based in a newly created grid. Entries tagged with [Code] need to be set in code, [Design] entries can be changed at design time. 

  * [Design] TitleStyle := tsStandard;
  * [Design] DefaultRowHeight := 24;
  * [Code] EditorBorderStyle := bsNone; // this might work only on Windows.
  * [Code] UseXORFeatures := true;
  * [Code] AllowOutBoundEvents := False; // SVN revision 10992 or later
  * [Code] FastEditing := False; // supported in dbgrid. StringGrid requires SVN revision 10992 or later
  * [Design] AutoAdvance := aaNone;



## Grids Reference

### Information

The starting point for reference about TCustomGrid, TDrawGrid, TCustomDrawGrid, TCustomStringGrid and TStringGrid is the [unit Grids.pas reference](<http://lazarus-ccr.sourceforge.net/docs/lcl/grids/index.html>)

For TCustomDBGrid and TDBgrid it is the [unit DBGrids.pas reference](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbgrids/index.html>)

In general, any Delphi reference about the grids should help us to use Lazarus grids (but don't forget that there are several differences between Delphi and Lazarus grids that we have documented); with this in mind and as a temporary place for reference information, this place will be used to document things that don't work the same as in Delphi, as well as any new functionality. 

TODO: the rest of this section will disappear; its content will be moved to [unit Grids.pas reference](<http://lazarus-ccr.sourceforge.net/docs/lcl/grids/index.html>)

### TCustomGrid

See the full [TCustomGrid Reference](<http://lazarus-ccr.sourceforge.net/docs/lcl/grids/tcustomgrid.html> "doc:lcl/grids/tcustomgrid.html")

#### property AllowOutboundEvents

Protected in TCustomGrid, public in TCustomDrawGrid and descendants. Normally when user click on a point over empty space after cells (for example if grid has three rows but user clicks on an imaginary fourth row), the currently focused cell will move to the nearest cell to the point just clicked. We call this an outbound event. The default value is true as this has been grid's behaviour since begining. This property was added to simulate Delphi behaviour where outbound events are not available, so to enable Delphi compatibility set this property to false. 

#### property Columns

Lazarus includes the property **columns** in TStringGrid and TDrawGrid grids. This property adds what we call custom columns. Custom columns are a collection of objects that hold properties that apply to whole columns, for example column titles (for a StringGrid it will override the value specified in the corresponding Cells[ColumnIndex, RowTitle] property), text alignment, background color, preferred editor, etc. 

Custom columns add extra properties or replace default property values on normal grid columns. Not only this, the **grid.ColCount** value may increase or decrease in order to account for the number of custom columns being attached to the grid. At this point, this means that **grid.ColCount** = **grid.FixedCols** \+ **grid.Columns.Count**. 

For example, if to a base grid with ColCount := 5 and FixedCols := 2 we: 

  * Add 3 custom columns, the base grid will be exactly as before, 2 fixed cols and 3 normal cols.
  * Add 1 custom column, the base grid will have ColCount := 3, that is 2 fixed cols and 1 normal cols.
  * Add 4 custom columns, the base grid will have ColCount := 6, that is 2 fixed cols and 4 normal cols.



From this we can conclude that: 

  * Fixed column properties or count are not enhanced or modified respectively by custom columns.
  * **grid.ColCount** is usually different from **grid.Columns.Count** (**grid.ColCount** =**grid.Columns.Count** only when **FixedCols** =0)



At design time the user can access the **columns** property in the Object Inspector to bring up the columns editor. From there you can add, remove or modify custom columns. The editor shows a list of current custom columns; by selecting items in this list the object inspector gets filled with the properties available for each column. The list of custom columns is also available in the Object Inspector component tree view, where columns can be added, deleted or modified. They appear on a lower level under the container grid. 

At runtime, columns can be modified with code like this: 
    
    
      var
        c: TGridColumn;
      begin
        // add a custom column a grid
        c := Grid.Columns.Add;
        // modify
        c.title.caption := 'Price';       // Set columns caption
        c.align := taRightJustify;        // Align column content to the right
        c.color := clMoneyGreen;          // Change default color to clMoneyGreen
        c.Index := 0;                     // Make it the first column
        // access an existing column
        grid.columns[0].Width := 60;      // Change column 0 width to 60 pixels
        // delete an existing column
        grid.columns.delete(0);           // Delete column 0
        ....
      end;
    

Additionally, when using custom columns, the grids do not allow direct modification of **grids.colcount** ; adding or removing columns should be done using the **columns** property. The explanation is that there is an inconsistency on gradually removing custom columns using **ColCount** , when **ColCount** reaches **FixedCols** , the grid has no more custom columns. If we now increase **ColCount** , the new column will not be a custom column but a normal column. 

Currently there are no plans to make the grids use only custom columns. 

#### property AutoFillColumns

Often the grids are wider than the horizontal space needed by the columns -- this leaves an unpleasant empty area at the right of the last column. The LCL grids provide a mechanism to expand the width of specified columns such that the empty space is filled automatically. 

For this purpose, **Columns** must be added either as described above, or by setting the `ColCount` property, and the grid property `AutoFillColumns` must be set to `true`. Each Column has a property `SizePriority`. When it has the value `0` the column width is taken from the `Width` property of the column. But when it has a non-zero value the column width is adjusted to the average available size remaining for all columns with a non-zero `SizePriority`. 

#### procedure SaveToFile(AFileName: String);

Saves the grid to an XML file. The property `SaveOptions` determines what exactly is to be saved: 
    
    
    type
      TGridSaveOptions = (
        soDesign,             // Save grid structure (col/row count and Options)
        soAttributes,         // Save grid attributes (Font, Brush, TextStyle)
        soContent,            // Save Grid Content (Text in StringGrid)
        soPosition            // Save Grid cursor and selection position
      );
      TSaveOptions = set of TGridSaveOptions;
    

#### procedure LoadFromFile(AFileName: String);

Loads the XML file created by `SaveToFile`. Again, the `SaveOptions` define what exactly is to be loaded from the file. 

### TCustomDBGrid

TCustomDBGrid is the base for TDBGrid. 

They do not expose Col and Row properties. To go to a certain column, use e.g. the SelectedIndex property. 

An interesting public method is AutoAdjustColumns. 

#### procedure AutoAdjustColumns

This procedure sets the column width to the size of the widest text it finds. It can be used after loading a dataset/setting it Active. 

However, contrary to TCustomStringGrid.AutoAdjustColumns (see below), this will set your columns very wide unless you have the property dgAutoSizeColumns enabled. 

#### procedure InplaceEditor

See example from bug 23103 - _and insert explanation of what it does/why it is needed. Validate input values? Change what is shown?_
    
    
    procedure TForm1.DBGrid1KeyPress(Sender: TObject; var Key: char);
    var
      S: String;
    begin
      if (Key in [',','.']) then
      begin
        //unlike Delphi not all InPlaceEditors are editors for string type, so check!
        if (DBGrid1.InplaceEditor is TStringCellEditor) then
        begin
          S := TStringCellEditor(DBGrid1.InplaceEditor).EditText;
          if Pos(',',S) > 0 then
            Key := #0
          else
            Key := ',';
        end;
      end;
    end;
    

### TCustomStringGrid

TCustomStringGrid serves as the base for TStringGrid. It can be used for derived TStringGrid components that want to hide published properties. See [new intermediate grids](</index.php?title=new_intermediate_grids&action=edit&redlink=1> "new intermediate grids \(page does not exist\)") for more information. 

The following properties or methods are public and are also available to TStringGrid. 

See the full [TCustomStringGrid Reference](<http://lazarus-ccr.sourceforge.net/docs/lcl/grids/tcustomstringgrid.html> "doc:lcl/grids/tcustomstringgrid.html")

#### procedure AutoSizeColumn(aCol: Integer);

This procedure sets the column width to the size of the widest text it finds in all rows for the column aCol. Tip: see the goDblClickAutoSize option to allow columns to be automatically resized when doubleClicking the column border. 

#### procedure AutoAdjustColumns or AutoSizeColumns

Automatically resizes all columns by adjusting them to fit in the longest text in each column. This is a quick method of applying AutoSizeColumn() for every column in the grid. 

#### procedure Clean; overload;

Cleans all cells in the grid, fixed or not. 

#### procedure Clean(CleanOptions: TGridZoneSet); overload;

Cleans all cells in the grid subject to the given CleanOptions. See [TGridZoneSet](<http://lazarus-ccr.sourceforge.net/docs/lcl/grids/tgridzoneset.html> "doc:lcl/grids/tgridzoneset.html") for more information. Some examples: 

  * Clean all cells: grid.Clean([]); (the same as grid.clean)
  * Clean all non fixed cells: grid.Clean([gzNormal]);
  * Clean all cells but don't touch grid column headers: Grid.Clean([gzNormal, gzFixedRows]);



#### procedure Clean(StartCol,StartRow,EndCol,EndRow: integer; CleanOptions:TGridZoneSet); overload;

does the same as Clean(CleanOptions:TGridZoneSet) but restricted to the given StartCol,StartRow,EndCol and EndRow. Examples: 

  * Clean column index 4 to 6 but don't touch grid column headers: many variations, Grid.Clean(4,Grid.FixedRows,6,Grid.RowCount-1,[]); Grid.Clean(4,0,6,Grid,RowCount-1, [gzNormal]); etc.



#### procedure Clean(aRect: TRect; CleanOptions: TGridZoneSet); overload;

The same as Clean(StartCol,StartRow,EndCol,EndRow, CleanOptions), just taking a TRect instead of individual cell coordinates. Useful to clean the selection: grid.Clean(Grid.Selection,[]); 

#### procedure SaveToCSVFile(AFileName: string; ADelimiter:Char=','; WithHeader:boolean=true);

Save grid content to a comma separated values format (CSV) file (added in Lazarus r32179). 

The AFilename argument specifies a file name where the content will be saved. If the file exists, the content will be overwritten. If it doesn't exist, the file will be created. 

ADelimiter (an optional argument) is used to supply a custom separator if required. By default a CSV format is produced (that is, ADelimiter:=',';) for a TAB separated file ADelimiter should be #9. 

The WithHeader parameter is used to decide if a "row header" should be included or not. The row header is a list of field names at the beginning of the output file; its content comes from the last fixed row in the grid. There is an exception to this rule: if the grid has custom columns, the row header content comes from the custom column titles and not from fixed row cell content. 

If WithHeader is true and the grid does not include a fixed row or custom columns, the row header content will be taken from the first row in the grid. 

Normal CSV data output will start at the first non-fixed row in the grid. 

#### procedure LoadFromCSVFile(AFileName: string; ADelimiter:Char=','; WithHeader:boolean=true);

Loads grid content from a comma separated values format (CSV) file (added in Lazarus r32179). 

Columns will be added or deleted to or from the grid as needed according to the number of fields included in each line of the CSV file. Note that, in case of an empty (newly created) grid, columns will be added only if at least a single column exists already. Loading a CSV file will not modify the number of fixed rows that already existed in the grid. 

The _AFileName_ argument specifies the name of the source file with the CSV content. 

_ADelimiter_ optional parameter may be used to specify a different separator or delimiter. An example: for a tab-separated file, _ADelimiter_ should be #9. Another popular file format is semicolon-delimited file, where _ADelimiter_ should be ; 

The _WithHeader_ parameter is used to decide if the first row in the CSV file should be considered as the "header row" or not. If the grid has fixed rows and _WithHeader_ is true, the column captions for the last fixed row will be taken from the header row. Note however that if the grid has custom columns, the header row will be used as source for the column titles and custom column titles are always shown in the grid's first fixed row or hidden if there are no fixed rows in the grid. 

If the _LoadFromCSVFile_ procedure has difficulty loading your CSV file (e.g. quotes or spaces being incorrectly interpreted), you could manually load the grid using e.g. [CsvDocument](<CsvDocument.md> "CsvDocument")... and of course a patch for _LoadFromCSVFile_ is always welcome. 

#### property Cols[index: Integer]: TStrings read GetCols write SetCols;

Get/set a list of strings from/to the given grid's column index starting from row index 0 to RowCount-1. 

##### Examples

  * Set Example: Set the content of the third column in the grid from a ListBox:


    
    
    Grid.Cols[2] := ListBox1.Items;
    

  * Get Example: Set the content of a Listbox from the grid's column index 4:


    
    
    procedure TForm1.FillListBox1;
    var 
      StrTempList: TStringList;
    begin
      StrTempList := TStringList(Grid.Cols[4]);
      if StrTempList<>nil then begin
        ListBox1.Items.Assign(StrTempList);
        StrTempList.Free;
      end;
    end;
    

##### Notes.

This property works differently in Lazarus and in Delphi when getting the data from the grid. In Lazarus a temporary TStringList object is created for retrieving the column content. It is the responsibility of the user to free this object after use. 

This means also that changes in the returned list will not affect the grids content or layout. 

See the Get Example. 

#### property Rows[index: Integer]: TStrings read GetRows write SetRows;

Get/set a list of strings from/to the given grid's row index starting from column index 0 to column ColCount-1. 

##### Notes.

This property works differently in Lazarus and in Delphi when getting the data from the grid. In Lazarus a temporary TStringList object is created for retrieving the row content. It is the responsibility of the user to free this object after use. 

This means also that changes in the returned list will not affect the grid's content or layout. 

##### Examples

  * Set Example: Set the content of the third row in the grid from a ListBox:


    
    
    Grid.Rows[2] := ListBox1.Items;
    

  * Get Example: Set the content of a Listbox from the grid's row index 4:


    
    
    procedure TForm1.FillListBox1;
    var 
      StrTempList: TStringList;
    begin
      StrTempList := TStringList(Grid.Rows[4]);
      if StrTempList<>nil then begin
        ListBox1.Items.Assign(StrTempList);
        StrTempList.Free;
      end;
    end;
    

  * An Example that doesn't work, and its Fix: Retrieved string list is read only


    
    
    // this will not work and will cause memory leak
    // because returned StringList is not being freed
    Grid.Rows[1].CommaText := '1,2,3,4,5';
    Grid.Rows[2].Text := 'a'+#13#10+'s'+#13#10+'d'+#13#10+'f'+#13#10+'g'; 
     
    // fixing the first case
    Lst:=TStringList.Create;
    Lst.CommaText := '1,2,3,4,5';
    Grid.Rows[1] := Lst;
    Lst.Free;
    

#### property UseXORFeatures;

Boolean property, default value: False; 

This property controls how the dotted focus rectangle appears in the grid. When True, the rectangle is painted using the XOR raster operation. This allow us to see the focus rectangle no matter what the cells' background color is. When False, the user can control the color of the dotted focus rectangle using the [FocusColor property](</index.php?title=FocusColor_property&action=edit&redlink=1> "FocusColor property \(page does not exist\)")

It also controls the look of the column/row resizing. When True, a line shows visually the size that the the column or row will have if the user ends the operation. When False, the column or row resizing takes effect just as the user drags the mouse. 

### TValueListEditor

[TValueListEditor](<TValueListEditor.md> "TValueListEditor") is a control derived from TCustomStringGrid for editing Key-Value pairs. 

#### Property DisplayOptions

Controls various aspects of the TValueListEditor's appearance by means of a set of the following elements: 

  * `**doColumnTitles**` : Displays column titles
  * `**doAutoColResize**` : Resizes column widths such that the entire client width is filled
  * `**doKeyColFixed**` : Keeps the width of the 1st column fixed when column widths are resized. Otherwise both columns have the same width. Requires `doAutoColSResize` to be included in the `DisplayOptions`.



#### Property TitleCaptions

Sets the values of the title captions (if doColumnTitles is in DisplayOptions). If DisplayOptions lacks the value doColumnTitles then default captions are used. 

#### Property Strings

Provides access to the list of strings that hold the Key-Value pairs.  
Key-Value pairs must be in the form:  
'KeyName=Value' 

#### Property ItemProps

You can use this property to control how the items in the "Value" columns can be edited.  
This is controlled by setting the ItemProp's EditStyle and ReadOnly properties. 

#### Property KeyOptions

KeyOptions is a set of TKeyOptions controlling whether the user can modify the contents of the "Key" column. 

  * KeyEdit: the user can edit the name of the Key
  * KeyAdd: the user can add keys (by pressing Insert in the grid). KeyAdd requires KeyEdit.
  * KeyDelete: the user can delete Key-Value pairs (by pressing Ctrl+Delete).
  * KeyUnique: if set, then Keys must have unique names. Attempting to enter a dulpicate Key will raise an exception.



#### Property DropDownRows

If the cell editor is a picklist (ValueEdit1.ItemProps['key1'].EditStyle=esPickList) this property sets the DropDownCount of the displayed list. The default is 8. 

#### Function DeleteRow

Deletes the Key-Value pair of the indexed row removing the row entirely. 

#### Function InsertRow

Inserts a row in the grid and sets the Key-Value pair. Returns the index of the newly inserted row. 

#### Function IsEmptyRow

Returns true if the indexed row's cells are empty (Keys[aRow]=_; Values[aRow]=_). 

#### Function FindRow

Returns the row that has the specified key name. 

#### Function RestoreCurrentRow

Undoes the editing in the current row (if the editor is still focused). Happens when the user presses the Escape key. 

#### Altered behaviour of some properties derived from TCustomStringGrid

##### Property Options

Due to the nature of TValueListEditor its Options property has certain restrictions 

  * goColMoving is not allowed in Options (you cannot set it).
  * goAutoAddRows can only be set if KeyAdd is in KeyOptions. Setting KeyAdd will automatically set goAutoAddRows.
  * goAutoAddRowsSkipContentCheck is not allowed (for the time being, it causes a crash in TValueListeditor: needs fixing).



##### Property FixedRows

Can only be 1 (show column titles) or 0 (don't show column titles). 

##### Property ColCount

Is always 2. 

##### Property Objects

It is not recommended to use this property at all.  
When some (internal) code manipulates the Strings property, changes are Objects will be lost. 

#### General comments on the use of TValueListEditor

When manipulating the contents of the ValueListEditor (the grid), you can manipulate the underlying Strings property directly.  
Be aware however, that is you do this, you **must** hide the editor, otherwise the Strings may get out of sync with the contents of the grid you will see on the screen.  
  
If you want to insert or delete rows etc. then the preferred way is to use the public methods from TValueListEditor: DeleteRow(), InsertRow(), MoveRow(), ExchangeRow() or the various Sort() methods.  
  
Trying to use ancestor's methods to manipulate rows or columns (e.g. Columns.Add) might result in a crash. 

#### Saving and loading the contenst of a TValueListEditor

##### Using the Strings property

If you just want to save and load the content of the TValueListEditor (e.g. the Key/Value pairs, not the layout) you can use Strings.SaveToFile and Strings.LoadFromFile.  
In case of LoadFromFile the `RowCount` property will be autmatically adjusted.  
No sanity checks are performed upon LoadFromFile (so lines that do not represent a Key/Value pair will end up as Keys without Value). 

##### Using TValueList.SaveToFile and TValueList.LoadFromFile

When using SaveToFile and LoadFromFile you get the additional benefits of also being able to save and load the layout, like with other grids.  
SaveToFile will also save information about wether or not the TValueListEditor uses ColumnTitles, and if so, it saves them.   
  
In contrast to it's ancestors TValueList.LoadFromFile performs sanity checks on the file it tries to read. In particular RowCount must be specified and there cannot be a cell with RowIndex > RowCount or ColumnIndex other than 0 or 1.  
If the file does not seem to be a valid TValueListEditor grid file, and exception is raised.  
  
TValueList.SaveToFile and TValueList.LoadFromFile do not work correctly in Lazarus versions <= 2.1 r62044. 

##### Using SaveToCSVFile and LoadFromCSV methods

These methods should **not** be used at the moment, since for a TValueListEditor they are flawed. 

## Working with grids

### Customizing grids

Grid are components derived from the [TCustomControl](<http://lazarus-ccr.sourceforge.net/docs/lcl/controls/tcustomcontrol.html>) class, and don't have a native widget associated with them which means that grids are not restricted by the look of current interface theme. This can be both an advantage and a disadvantage: usually programmers want to create a uniform-look application. The good news is that Lazarus grids are flexible enough to get something from both worlds; programmers can easily make grids look similar to other native controls, or they can customize the grid to the finest detail so they can obtain almost the same look in any platform or widget interface (that is, with the exception of scrollbars, because their look is still determined by the current theme). 

### Properties and Events for customizing grids

Some properties can affect the way the grid looks by acting when the cell is about to be painted in PrepareCanvas/OnPrepareCanvas by changing default canvas properties like brush color or font. Following is a list of such properties: 

  * **AlternateColor.** With this the user can change the background color appears on alternated rows. This is to allow easy reading off of grid rows data.
  * **Color.** This sets the primary color used to draw non fixed cells background.
  * **FixedColor.** This is the color used to draw fixed cells background.
  * **Flat.** This eliminates the 3d look of fixed cells.  

  * **TitleFont.** Font used to draw the text in fixed cells.  

  * **TitleStyle.** This property changes the 3D look of fixed cells, there are 3 settings: 
    * _tsLazarus._ This is the default look
    * _tsNative._ This tries to set a look that is conforms with the current widgetset theme.
    * _tsStandard._ This style is a more contrasted look, like Delphi grids.
  * **AltColorStartNormal.** Boolean. If true: alternate color is always in the second row after fixed rows, the first row after fixed rows will be always color. If false: default color is set to the first row as if there were no fixed rows.
  * **BorderColor.** This sets the grid's border color used when Flat:=True and BorderStyle:=bsSingle;
  * **EditorBorderStyle.** If set to bsNone under Windows the cell editors will not have the border, like in delphi, set to bsSingle by default because the border can be theme specific in some widgetsets and to allow a uniform look.  

  * **FocusColor.** The color used to draw the current focused cell if UseXORFeatures is not set, by default this is clRed.
  * **FocusRectVisible.** Turns on/off the drawing of focused cell.
  * **GridLineColor.** Color of grid lines in non fixed area.
  * **GridLineStyle.** Pen style used to draw lines in non fixed area, possible choices are: _psSolid_ , _psDash_ , _psDot_ , _psDashDot_ , _psDashDotDot_ , _psinsideFrame_ , _psPattern_ ,_psClear_. Default is _psSolid_.
  * **SelectedColor.** Color used to draw cell background on selected cells.
  * **UseXORFeatures.** If set, focus rect is drawn using XOR mode so it should make visible the focus rect in combination with any cell color ackground. It also affects the moving columns look.
  * **DefaultDrawing.** Boolean. Normally the grids prepare the grid canvas using some properties according to the kind of cell that is being painted. If the user writes an OnDrawCell event handler, a set DefaultDrawing also paints the cell background. If the user draws the cell himself, it is better to turn off this property so painting is not duplicated. In a StringGrid, a set DefaultDrawing draws the text in each cell.
  * **AutoAdvance.** Where the cell cursor will go when pressing Enter, or after editing.
  * **TabAdvance.** Where the cell cursor will go when pressing Tab or Shift-Tab.
  * **ExtendedColSizing.** If true user can resize columns not just at the headers but along the columns height.



Other properties that also affect the grids look. 

**Options**. 

    Options property is a set with some elements to enable diverse functionality but some are related directly with grid's look. This options can be set at designtime or runtime.

  * **goFixedVertLine, goFixedHorzLine** it draws a vertical or horizontal line respectively delimiting cells or columns in fixed area, active by default.
  * **goVertLine, goHorzLine** the same as previous, but for normal browseable area. A grid can be made to simulate a listbox by unsetting both of this elements.
  * **goDrawFocusSelected** if this element is enabled a selection background is painted in focused cell in addition to focused dotted rectangle (note this doesn't work yet when goRowSelect option is set, in such case row is always painted as if goDrawFocusSelected is set)
  * **goRowSelect** select the full row instead of individual cells
  * **goFixedRowNumbering** if set, grid will do numbering of rows in first fixed column
  * **goHeaderHotTracking** if set, the grid will try to show a different look when the mouse cursor is overing any fixed cell. In order for this to work, desired cell zone needs to be enabled with property HeaderHotZones. Try combining this option with property TitleStyle:=tsNative to get themed hot tracking look.
  * **goHeaderPushedLook** if set, this element enables a pushed look when clicking any fixed cell. The zone of "pushable" cells is enabled using HeaderPusedZones property.



(write more) 

### Description of grid's drawing process

Like other custom controls, the grid is drawn using the paint method. In general terms the grid is drawn by painting all rows, and each row by painting its individual cells. 

The process is as follow: 

  * First the visible cells area is determined: each row is tested to see if it intersects the canvas clipping region; if it's ok, then the visible area is painted by drawing columns of each row.
  * The column and row values are used to identify the cell that is about to be painted and again each column is tested for intersection with the clippling region; if everything is ok, some additional properties like the cell's rectangular extent and visual state are passed as arguments to the DrawCell method.
  * As the drawing process is running, the visual state of each cell is adjusted according to grid options and position within grid. The visual state is retained in a varible of type TGridDrawState which is a set with following elements: 
    * _gdSelected_ The cell will have a selected look.
    * _gdFocused_ The cell will have a focused look.
    * _gdFixed_ Cell have to be painted with fixed cell look.
    * _gdHot_ the mouse is over this cell, so paint it with hot tracking look
    * _gdPushed_ the cell is being clicked, paint it with pushed look
  * **DrawCell.** The DrawCell method is virtual and may be overriden in descendant grids to do custom drawing. The information passed to DrawCell helps to identify the particular cell is being painted, the physical area ocuppied in screen and its visible status. See DrawCell reference for details. For each cell the following occurs:
  * **PrepareCanvas.** In this method, if the DefaultDrawing property is set, the grid canvas is setup with default properties for brush and font based on current visual state. For several design and runtime properties, the text alignment is set to match programmer selection in custom columns if they exists. If DefaultDrawing is false, brush color is set to clWindow and Font color to clWindowText, the text alignment is set with grids defaultTextStyle property value.
  * **OnPrepareCanvas.** If the programmer wrote an event handler for OnPrepareCanvas event, it is called at this point. This event can be used for doing simple customization like changing cell's background color, font's properties like color, fontface and style, Text layout like different combinations of left, center, top, bottom, right alignment, etc. Any change made to the canvas in this event would be lost, because the next cell drawing will reset canvas again to a default state. So it's safe doing changes only for particular cell or cells and forget about it for the rest. Using this event sometimes helps to avoid using the OnDrawCell grid event, where users would be forced to duplicate the grid's drawing code. Todo: samples of what can be made and what to leave for OnDrawCell?...
  * **OnDrawCell.** Next if no handler for OnDrawCell event was specified, the grid calls the DefaultDrawCell method which simply paints the cell background using the current canvas brush color and style. If the OnDrawCell handler exists, the grid first paints the cell background but only if DefaultDrawing property was set, then it calls OnDrawCell event to do custom cell painting. Usually programmers want to do custom drawing only for particular cells, but standard drawing for others; in this case, they can restrict custom operation to certain cell or cells by looking into ACol, ARow and AState arguments, and for other cells simply call DefaultDrawCell method and let the grid to take care of it.
  * **Text.** At this point (only for TStringGrid) if DefaultDrawing property is true, the cell's text content is painted.
  * **Grid lines**. The last step for each cell is to paint the grid lines: if grid options goVertLine, goHorzLine, goFixedVertLine and goFixedHorzLine are specified the cell grid is drawn at this point. Grids with only rows or only cols can be obtained by changing these options. If the programmer elected to have a "themed" look it is done at this point also (see property TitleStyle).
  * **FocusRect.** When all columns of current row have been painted it is time to draw the focus rectangle for the current selected cell or for the whole row if goRowSelect option is set.



#### Differences with Delphi

  * In Lazarus TCustomGrid.DrawCell method is not abstract and its default implementation does basic cell background filling.
  * In Delphi, the cell's text is drawn before entering the OnDrawCell event (see [bug report #9619](<http://www.freepascal.org/mantis/view.php?id=9619>)).



### Grid's cell selection

The location of a grid's current (focused) cell (or row) can be changed using keyboard, mouse or through code. In order to change cell focus successfully to another position, we must test the target position to see if it is allowed to receive cell focus. When using keyboard, the property AutoAdvance performs part of the process by finding what should be the next focused cell. When using mouse clicks or moving by code, focus will not move from the current cell unless the target cell is permitted to receive focus. 

The grid calls function SelectCell to see if a cell is focusable: if this function returns true, then the target cell identified with arguments aCol and aRow is focusable (the current implementation of TCustomGrid simply returns true). TCustomDrawGrid and hence TDrawGrid and TStringGrid override this method to check first if cell is any wider than 0; normally you don't want a 0 width cell selected so a cell with these characteristics is skipped automatically in the process of finding a suitable cell. The other thing the overridden method SelectCell does is to call the user configurable event OnSelectCell: this event receives the cell coordinates as arguments and always returns a default result value of true. 

Once a cell is known to be focusable and we are sure a movement will take place, first the method BeforeMoveSelection is called; this in turns triggers the OnBeforeSelection event. This method's arguments are the coordinates for the new focused cell. At this point any visible editor is hidden too. The "before" word means that selection is not yet changed and current focused coordinates can be accessed with grid.Col and grid.Row properties. 

After that, the internal focused cell coordinates are changed and then MoveSelection method is called; this method's purpose is to trigger the OnSelection event if set (this is a notification that the focused cell has, by this time, already changed and cell coordinates are now available through grid.row and grid.col properties). 

Note that is not good to use OnSelectCell event to detect cell focus changes, as this event will be triggered several times even for the same cell in the process of finding a suitable cell. Is better to use OnBeforeSelection or OnSelection events for this purpose. 

#### Differences with Delphi

  * SelectCell and OnSelectCell behaviour is probably different - can't really comment on the differences. In Lazarus they are used in functionality like AutoAdvance which as far as I know doesn't exist in Delphi.



### When built-in properties are not enough: derived grids

Derived grids usually have to override the following methods:  
DrawAllRows: Draws all visible rows.  
DrawRow: Draws all cells in a row.  
DrawRow draws all cells in the row by first checking if cell is within clipping region, and only draws the cell if it is.  
DrawCell:  
DrawCellGrid:  
DrawCellText:  
DrawFocusRect:  
(write me).  


### What happens in the `TCustomGrid.Paint` method?

The following list is to show the internal order of method calls for painting a `TCustomGrid` (or descendant). Every item in the lists represents method calls during the paint operation. This ought to help finding the right point to change behaviour when it comes to descending from `TCustomGrid`. 

  * **`DrawEdges`** : Draws the grid's outer border.
  * **`DrawAllRows`** (virtual): Draws all rows in the visible part of the grid. This is the only method called in the painting process that is declared _`virtual`_. 
    * **`DrawRow`** (virtual): Is called for each row inside the current viewport. 
      * **`DrawCell`** (virtual): Is first called for each 'normal' (i.e. not fixed) cell in the row. 
        * **`PrepareCanvas`** (virtual): Sets the canvas's drawing styles according to the current cell's visual properties.
        * **`DrawFillRect`** : Draws the cell's background with the styles set in `PrepareCanvas`.
        * **`DrawCellGrid`** (virtual): Draws the cell's border lines.
      * **`DrawFocusRect`** (virtual): In `TCustomGrid` this method does nothing. In descendent grids this method is used to draw the focus rectangle inside the active cell.
      * **`DrawCell`** (virtual): (see above) Is called for each fixed cell in the row's visible viewport.
  * **`DrawColRowMoving`** : only active while moving a column or row. This method draws the line that indicates the row's/column's new position.
  * **`DrawBorder`** : If needed (`Flat=TRUE` and `BorderStyle=bsSingle`), this draws an inner border line.



#### Additional methods to draw in `TCustomGrid`

These methods are declared and (partly) implemented in `TCustomGrid`, but they are not called directly from here. They are used by descendant classes to draw the cell's content. 

  * **`DrawCellText`** (virtual): Writes/draws the text that is passes as parameter into the cell. The text is formatted by using the styles that are active in `Canvas.TextStyle` (see also `PrepareCanvas`).
  * **`DrawThemedCell`** (virtual): Is only used for fixed cells and if `TitleStyle=tsNative`. It draws the cell's background using `ThemeServices`.
  * **`DrawColumnText`** (virtual): Is only used for column header cells. 
    * **`DrawColumnTitleImage`** : If the grid as an assigned `TitleImageList` and `Columns.Title[x].Title.ImageIndex` contains a valid value, that image is drawn into the cell.
    * **`DrawCellText`** : (see above)
  * **`DrawTextInCell`** (virtual): Is used for 'normal' (i.e. not fixed) cells.In `TCustomGrid` this method does nothing. In descendent grids this method is used to draw the cells content. In `TStringGrid` theis method calls `DrawCellText` (see above).



#### Drawing methods introduced by `TCustomDrawGrid`

As you can see the base class `TCustomGrid` does not draw any content into the cells. This is done in **`TCustomDrawGrid`**. **`TCustomDrawGrid`** overrides the method `DrawCell` in the following way: 

  * **`DrawCell`** (override): 
    * **`PrepareCanvas`** : calls the inherited method from **`TCustomGrid`**.
    * **`DefaultDrawCell`** (virtual): This method is only called if the event handler for `OnDrawCell` is NOT assigned. 
      * **`DrawFillRect / DrawThemedCell`** (von `TCustomGrid`): Draws the cell's background. If `TitleStyle=tsNative`, the background of fixed cells is drawn using `DrawThemedCell`.
      * **`DrawCellAutonumbering`** (virtual): If `goFixedAutonumbering in Options`, row numbers are drawn into the first fixed column using `TCustomGrid.DrawCellText`.
      * **`DrawColumnText`** (from `TCustomGrid`): Is only called for column header cells (see _Additional methods to draw in` TCustomGrid`_).
      * **`DrawTextInCell`** (from `TCustomGrid`): Is only called for 'normal' (i.e. not fixed) cells (see _Additional methods to draw in` TCustomGrid`_).
    * **`DrawCellGrid`** (virtual): Draws the cell's borders



### Save and Retrieve Grid Content

The **SaveToFile** and **LoadFromFile** methods allows a grid to save and retrieve it's layout and data to/from a XML format file. TStringGrid, as inherited from TCustomStringGrid, has also the ability to "export" and "import" its content to/from a Comma Separated Values format file, best known as CSV files. This is described in the **SaveToCSVFile** and **LoadFromCSVFile** methods reference (TODO: make links). 

The kind of information that can be saved and then retrieved when using **SaveToFile** and **LoadFromFile** is determined by the SaveOptions property (of type TSaveOptions) which is a set of options described as follows: 
    
    
    soDesign:     Save & load ColCount,RowCount,FixedCols,FixedRows,
                  ColWidths, RowHeights and Options (TCustomGrid)
    soPosition:   Save & load Scroll position, Row, Col and Selection (TCustomGrid)
    soAttributes: Save & load Colors, Text Alignment & Layout, etc. (TCustomDrawGrid)
    soContent:    Save & load Text (TCustomStringGrid)
    soAll:        set of all options (soAll:=[soDesign,soPosition,soAttributes,soContent];)
    

Not all options apply to all kind of grids, for example, soContent do not apply to TCustomGrid derived grids like TDrawGrid as this kind of grid does not have the concept of "content". TStringGrid is a special kind of grid that "knows" how to handle strings and can use the soContent option if specified. 

The soAttributes option is not used in Lazarus standard grids, is there for derived grids. 

When using **LoadFromFile** the grid also uses the SaveOptions property in order to know what kind of information needs to retrieve from the file, so is perfectly possible to specify SaveOptions:=[soDesign,soContent] on saving and only SaveOptions:=[soContent] on loading. 

For a TStringGrid the default SaveOptions value is [soContent], for other kind of grids, SaveOptions is the empty set. 

**Note:** One common issue when saving & retrieving grid data occurs when the user specify the SaveOptions property before **SaveToFile** but not before **LoadFromFile**. When using **LoadFromFile** some time after **SaveToFile** have been used, the SaveOptions property is properly set, but if **LoadFromFile** is executed on next program run, the SaveOptions property might not have been properly setup, for this reason is recommended to always specify SaveOptions property just before **LoadFromFile** , or doing it globally at program start like in the following example: 

* * *

**Example:**

  1. First, go to menu "File -> New -> Application";
  2. Put an empty TStringGrid on the form;
  3. Put a TButton and TOpenDialog on the form;
  4. Add the event OnCreate for the form;
  5. Add the event OnClick for the button.


    
    
    unit Unit1; 
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs, Grids,
      Buttons, StdCtrls, XMLCfg;
    
    type
    
      { TForm1 }
      TForm1 = class(TForm)
        StringGrid1: TStringGrid;
        Button1: TButton;
        OpenDialog1: TOpenDialog;
        procedure Button1Click(Sender: TObject);
        procedure Form1Create(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end; 
    
    var
      Form1: TForm1; 
    
    implementation
    
    { TForm1 }
    
    procedure TForm1.Form1Create(Sender: TObject);
    begin
     //sets the SaveOptions at creation time of the form 
     stringgrid1.SaveOptions := [soDesign,soPosition,soAttributes,soContent];
    end;
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
     //Ask if thew Execute method of the OpenDialog was launched 
     //when this occurs, the user selects an XML file to Load
     //wich name was stored in the FileName prop.
    
     if opendialog1.Execute then
     begin
       //Clear the grid 
       StringGrid1.Clear;
       //Load the XML
       StringGrid1.LoadFromFile(OpenDialog1.FileName);
       //Refresh the Grid
       StringGrid1.Refresh;
     end;
    end;
    
    initialization
      {$I unit1.lrs}
    
    end.
    

* * *

**The sample xml file:** (Copy the text below into a txt file. Don't forget put the xml header :-)) 
    
    
    <?xml version="1.0"?>
    <CONFIG>
      <grid version="3">
        <saveoptions create="True" position="True" content="True"/>
        <design columncount="2" rowcount="5" fixedcols="1" fixedrows="1" defaultcolwidth="64" defaultRowHeight="20">
          <options>
            <goFixedVertLine value="True"/>
            <goFixedHorzLine value="True"/>
            <goVertLine value="True"/>
            <goHorzLine value="True"/>
            <goRangeSelect value="True"/>
            <goDrawFocusSelected value="False"/>
            <goRowSizing value="False"/>
            <goColSizing value="False"/>
            <goRowMoving value="False"/>
            <goColMoving value="False"/>
            <goEditing value="False"/>
            <goTabs value="False"/>
            <goRowSelect value="False"/>
            <goAlwaysShowEditor value="False"/>
            <goThumbTracking value="False"/>
            <goColSpanning value="False"/>
            <goRelaxedRowSelect value="False"/>
            <goDblClickAutoSize value="False"/>
            <goSmoothScroll value="True"/>
          </options>
        </design>
        <position topleftcol="1" topleftrow="1" col="1" row="1">
          <selection left="1" top="1" right="1" bottom="1"/>
        </position>
        <content>
          <cells cellcount="10">
            <cell1 column="0" row="0" text="Title Col1"/>
            <cell2 column="0" row="1" text="value(1.1)"/>
            <cell3 column="0" row="2" text="value(2.1)"/>
            <cell4 column="0" row="3" text="value(3.1)"/>
            <cell5 column="0" row="4" text="value(4.1)"/>
            <cell6 column="1" row="0" text="Title Col2"/>
            <cell7 column="1" row="1" text="value(1.2)"/>
            <cell8 column="1" row="2" text="value(2.2)"/>
            <cell9 column="1" row="3" text="value(3.2)"/>
            <cell10 column="1" row="4" text="value(4.2)"/>
          </cells>
        </content>
      </grid>
    </CONFIG>
    

### Grid Cell Editors

The grid uses cell editors to change the content of cells. 

For a specialized grid like TStringGrid, the editor is the usual single line text editor control, but sometimes it's desirable to have other means to enter information. For example: 

  1. show the open file dialog to find the location of a file so the user doesn't have to type the full path manually
  2. if the text in the cell represents a date, popup a calendar so we can choose a specific date easily.



Sometimes the information the user should enter in a cell is restricted to a limited list of words; in this case typing the information directly might introduce errors and validation routines might need to be implemented. We can avoid this by using a cell editor that presents the user with a list containing only the legal values. 

This is also the case for generic grids like TDrawGrid where the user needs some kind of structure to hold the data that will be shown in the grid. In this situation, the information that is entered in the cell editor updates the internal structure to reflect the changes in the grid. 

#### Builtin cell editors

The grids.pas unit already includes some of the most used cell editors ready for use in grids. It is also possible to create new cell editors (custom cell editors) if the built-in editors are not appropiate for a specific task. 

The built-in cell editors are Button, Edit, and Picklist. 

#### Using cell editors

Users can specify what editor will be used for a cell using one of two methods. 

  1. **Using a custom column and selecting the ButtonStyle property of the column**. In this method the user can select the style of the editor that will be shown. Available values are: cbsAuto, cbsEllipsis, cbsNone, cbsPickList, cbsCheckboxColumn, cbsButtonColumn.
  2. **Using OnSelectEditor grid event**. Here the user specifies in the Editor parameter which editor to use for a cell identified for column aCol and row ARow in a TCustomDrawGrid derived grid or TColumn in TCustomDBGrid. For this purpose there is a useful public function of grids, EditorByStyle(), that takes as parameter one of the following values: cbsAuto, cbsEllipsis, cbsNone, cbsPickList, cbsCheckboxColumn, cbsButtonColumn. This method takes precedence over the first one using custom columns. A custom cell editor can be specified here (see chapter 5 of [this document](<http://www.freepascal.org/~michael/articles/grids/grids.pdf>) by Michaël Van Canneyt which explains how to correctly implement a custom editor). This event is also the place to setup the editor with values specific to the cell, row or column (e.g. a custom popupmenu for the cell editor).



Setting the _ButtonStyle_ property only works if a column is created with the _StringGrid1.Columns.Add;_ statement. Using expression like _StringGrid1.ColCount:=X;_ will cause an exception. Setting the ButtonStyle property can be done with a similar code: 
    
    
    if ColCB< StringGrid1.Columns.Count
       then StringGrid1.Columns.Items[ColCB].ButtonStyle:=cbsCheckboxColumn;
    

#### Description of editor styles

The following is a description of the editor styles. They are enumerated values of type TColumnButtonStyle and so they are prefixed by 'cbs'. This type was used to remain compatible with Delphi's DBGrid. 

  * **cbsAuto**



    This is the default editor style for TCustomGrid derived grids. The actual editor class that will be used to edit the cell content depends on several factors. For TCustomGrids it uses a TStringCellEditor class derived from TCustomMaskEdit. This editor is specialized to edit single line strings. It is then used in TStringGrid and TDrawGrid by default. When using Custom Columns, if the programmer filled the Column's PickList property, this behaves as if cbsPickList editor style was set. For a TCustomDBGrid that has a field of type boolean, it behaves as if cbsCheckBoxColumn editor style was specified. This is the recommended value for Custom Cell Editors. TODO: related OnEditingDone.

  * **cbsEllipsis**



    This editor style is the most generic one. When used, a button appears in the editing cell and programmers could use the OnEditButtonClick grid event to detect when the user has pressed the button and take any action programmed for such a cell. For example a programmer could use this editor style to pop up a calendar dialog to allow the user easily to select a specific date. Other possibilities could be to show a file open dialog to find files, a calculator so user can enter the numeric result of calcs, etc.

    OnEditButtonClick is just a notification, to find out in which cell a button has been clicked by taking a look at the grid.Row and grid.Col properties.

    A DBGrid has specific properties to retrieve the active column or field and because this event occurs in the active record, it could update the information in the active field.

    This editor style is implemented using TButtonCellEditor, a direct descendant of TButton.

  * **cbsNone**



    This editor style instructs the grid not to use any editor for a specific cell or column; it behaves then, as if the grid were readonly for such a cell or column.

  * **cbsPickList**



    Used to present the user with a list of values that can be entered. This editor style is implemented using TPickListCellEditor, a component derived from TCustomComboBox. The list of values that are shown is filled in one of two ways depending on the method used to select the editor style. 

  1. When using custom columns, programmers can enter a list of values using the column's PickList property. [FOR BEGINNERS: TODO: exact procedure to edit the list]
  2. In OnSelectEditor, programmers get the TPickListCellEditor instance using the function EditorByStyle(cbsPickList). See here for an example


    The value in a TStringGrid grid will automatically reflect the value selected. If necessary the programmer could detect the moment the value is selected by writing an event handler for the grid's OnPickListSelect event, so additional steps can be taken (for example, to process the new value). TODO: related OnEditingDone.

  * **cbsCheckboxColumn**



    It can be useful when the data content associated with the column is restricted to a pair of values, for example, yes-no, true-false, on-off, 1-0, etc. Instead of forcing the user to type the values for this kind of data in a StringCellEditor or to choose one from a list, cbsCheckboxColumn is used to modify the data of a column by using a checkbox representation that the user can toggle by using a mouse click or pressing the SPACE key (if the column, containing the checkbox is selected and if the StringGrid is editable).
    Getting or setting the value of the checkbox in a cell is done the following way:
    
    
    StringGrid1.Cell[x,y]:='Z';
    

    where Z shall be replaced by _0_ for Unchecked, _1_ for Checked and an empty string for Grayed. Note that any value (string) different from _0_ and _1_ will be displayed as a grayed checkbox.

    If a columns' ButtonStyle property is set to cbsAuto and DBGrid detects that the field associated with the column is a boolean field, then the grid uses this editor style automatically. This automatic selection can be disabled or enabled using DBGrid's OptionsExtra property; setting dgeCheckboxColumn element to false disables this feature.

    The values that are used to recognize the checked or unchecked states are set in a column's properties ValueChecked and ValueUnchecked.

    At any moment, the field value can be in one to three states: Unchecked, Checked or Grayed. Internally these states are identified by the following values of type TDBGridCheckBoxState: gcbpUnChecked, gcbpChecked and gcbpGrayed.

    This editor style doesn't use real TCheckbox components to handle user interaction: the visual representation is given by three built-in bitmap images that corresponds to the possible states of checkbox. The used bitmaps can be customized by writing a handler for DBGrid event OnUserCheckboxBitmap; the handler of this event gets the state of the checkbox in the parameter CheckedState of type TDBGridCheckboxState and a bitmap parameter that the programmer could use to specify custom bitmaps.

  * **cbsButtonColumn**



    This editor style is used to show a button on every cell on column. Like in the case of cbsCheckboxColumn this editor do not use real buttons, the appearance is defined by current widgetset theme.

    The user knows what particular button was pressed by handling the grid's OnEditButtonClick and checking grid's col and row. Note that in this particular case, grid's col and row do not identify the currently selected cell, but the cell of the clicked button. Once the OnEditButtonClick event has been handled, and if the user has not modified the grid's col or row in this handler, the grid automatically resets the col and row to reflect the currently selected cell. While handling the OnEditButtonClick the current grid selection is available in grid.Selection which is a TRect property, left and right represent Column indexes, Top and Bottom are row indexes.

    The button's caption is the corresponding cell string.

### Editing grids

The effect of editing is different depending on what kind of grid is used, for example, TStringGrid stores the edited text internally and TDBGrid affects records in a dataset. TDrawGrid doesn't know what to do with the edited text, if the programmer do not take control on it, it's simply discarded.  
For TDrawGrid (although this should work for all grid classes) in order to access the edited text, the programmer can use the event **OnSetEditText** which is triggered every time the user modify something in the editor, the _aCol_ and _aRow_ parameters of this event identify the cell being edited, the parameter _value_ holds the text, this is the recommended method. Another way of access the edited text is taking it directly from the editor once the editing process has ended by using the event **OnEditingDone**. This could be accomplished by accessing the internal editor used for editing, in this case, the default "String Cell Editor". For this, there are two methods: The first is by using the event **OnSelectEditor** where the parameter _Editor_ is the instance which will be used to edit a cell, you have to store this instance in a variable, for example TheEditor (of type _TStringCellEditor_), for later use in **OnEditingDone**. The second alternative is using the grid's method **EditorByStyle** , this method accepts an "editor style" as parameter and it returns the instace of the editor corresponding to that style. In our case knowing that the style cbsAuto returns the default cell editor, in the **OnEditingDone** event handler we can use directly TheEditor := Grid.EditorByStyle(cbsAuto). You can then get the text with _TheEditor.Text_. If you use this method note that "with great power, comes great responsibility". 

#### Options and properties that affect editing

The editing behavior is controlled by a number of properties and options, virtually any adjustment or options that affects editing requires an editable grid or such adjustment might be ignored.  
At start the editable state of the grids is different, for TDrawGrid and TStringGrid editing is disabled, for TDbGrid is enabled by default.  
The **Options** property has several items that deal with editing, they are described below:  


  * **goEditing, dgEditing (in DbGrid)**. This option changes the editable state of the grid, can be changed at runtime because it is checked when editing is about to be started.
  * **goAlwaysShowEditor**. Normally the editor is hidden and it becomes visible only when it's needed. With this option, the editor will be visible all the time, if grid is not editable, this option is ignored and editor is always hidden.



### Cell hints

In addition to the standard hint of most controls, the descendants of TCustomGrid can display a special hint for the cell over which the mouse hovers. This feature is controlled by the grid options **goCellHints** and **goTruncCellHints** (or **dgCellHints** and **gdTruncCellHints** for DBGrid, respectively): 

  * **goCellHints** evaluates the event **OnGetCellHint** to retrieve the displayed text for the cell specified by the passed parameters. In case of the TDrawGrid and the TStringGrid, column and row indexes are given, while in case of the TDBGrid the cell text can be extracted from the Field assigned to the specified column object.
  * **goTruncCellHints** displays a hint only if the text does not fit into a cell and is truncated; the hint text is the untruncated cell text. In Lazarus 1.9+, this text is passed to the event **OnGetCellHint** for further refinement (e.g. display the entire text of a memo in the hint). In case of the truncated cell texts, it is recommended to also activate the option **goCellEllipsis** (or **dgCellEllipsis** for DBGrid) which adds a '...' to the truncated cell text.



If the grid has a value in its standard property **Hint** then the property **HintPriority** can be used to control how all these hint texts can be combined into a single popup window: 

  * **chpAll** puts the standard Hint into the first line, the hint determined by the OnGetCellHint event into the second line, and the hint for the truncated cells into the third line
  * **chpAllNoDefault** displays only the cell hints given by the OnGetCellHint event and by the truncated cells.
  * **chpTruncOnly** displays only hints for the truncated cells; this is the default settings.



In addition, the standard property **ShowHint** of the grid must be set to true in order to show cell hints. 

## Grids Howto and Examples

### Focusing a cell

Focusing a cell in TStringGrid is easy. Note that counting starts from zero not 1. So to focus the cell in the 10th row and the 9th column, do: 
    
    
    StringGrid1.Row := 9;
    StringGrid1.Col := 8;
    

### Example: How to set a custom cell editor

See lazarus/examples/gridexamples/gridcelleditor/gridcelleditor.lpi (from laz 1.2) 

### Example: How to add a button editor
    
    
    // Conditionally show button editor in column index 1 or 2 if 
    // cell in column index 1 is empty 
    procedure TForm1.StringGrid1SelectEditor(Sender: TObject; aCol, aRow: Integer; 
      var Editor: TWinControl);
    begin
      if (aCol = 1) or (aCol = 2) then
        if StringGrid1.Cells[1,aRow] = '' then
        begin
          Editor := StringGrid1.EditorByStyle(cbsEllipsis);
        end;
      end;
    
    // Triggering Action ...
    procedure TForm1.StringGrid1EditButtonClick(Sender: TObject);
    begin
      if StringGrid1.Col = 1 then Showmessage('column 1 editor clicked');
      if StringGrid1.Col = 2 then Showmessage('column 2 editor clicked');
    end;
    

### Example: Working with Picklist, How to make it read only and How to fill it at run time

Using grid's event OnSelectEditor one can customize how PickList editor (see cbsPickList button style) behaves. In next example the picklist editor from column 1 is modified so on odd rows the user can enter values by typing, on even rows the values are limited to the ones contained in its list. Also, this example show how to fill the list with different values depending on the row being processed. 
    
    
    procedure TForm1.gridSelectEditor(Sender: TObject; aCol, aRow: Integer;
      var Editor: TWinControl);
    begin
      if aCol=1 then begin
        Editor := StringGrid1.EditorByStyle(cbsPickList); 
        if (Editor is TCustomComboBox) then
          with Editor as TCustomComboBox do begin
            if (aRow mod 2=0) then
              Style := csDropDown
            else
              Style := csDropDownList;
            case aRow of
              1:
                Items.CommaText := 'ONE,TWO,THREE,FOUR';
              2:
                Items.CommaText := 'A,B,C,D,E';
              3:
                Items.CommaText := 'MX,ES,NL,UK';
              4:
                Items.CommaText := 'RED,GREEN,BLUE,YELLOW';
            end;
          end;
      end;
    end;
    

### Aligning text in StringGrids

This code shows how to use different text alignments in columns 2 and 3. 
    
    
    procedure TForm1.StringGrid1PrepareCanvas(sender: TObject; aCol, aRow: Integer;
      aState: TGridDrawState);
    var
      MyTextStyle: TTextStyle;
    begin
      if (aCol=2) or (aCol=3) then
      begin
        MyTextStyle := StringGrid1.Canvas.TextStyle;
        if aCol=2 then
          MyTextStyle.Alignment := taRightJustify 
        else 
        if aCol=3 then
          MyTextStyle.Alignment := taCenter;
        StringGrid1.Canvas.TextStyle := MyTextStyle;
      end;
    end;
    

### Multilines in Grids

This sample shows how to make multilined text in cell [3,2]. It works the same for DBGrid where OnPrepareCanvas have parameters for dealing with TColumns and from there with TFields. 
    
    
    procedure TForm1.StringGrid1PrepareCanvas(sender: TObject; aCol, aRow: Integer;
      aState: TGridDrawState);
    var
      MyTextStyle: TTextStyle;
    begin
      if (aRow=2) or (aCol=3) then
      begin
        MyTextStyle := StringGrid1.Canvas.TextStyle;
        MyTextStyle.SingleLine := false;
        StringGrid1.Canvas.TextStyle := MyTextStyle;
      end;
    end;
    

### Validating Entered Values

Lazarus version 0.9.29 introduces the StringGrid OnValidateEntry event of type TValidateEntryEvent which has the following declaration: 
    
    
    type
      TValidateEntryEvent = procedure(sender: TObject; aCol, aRow: Integer; const OldValue: string; var NewValue: string) of object;
    
    
    
    aCol,aRow are the cell coordinates of cell being validated.
    OldValue is the value that was in cells[aCol,aRow] before editing started.
    NewValue is the value that will be finally inserted in cells[aCol,aRow].
    

Because of the way StringGrid works by setting the cell value while user is editing (see grid's OnSetEditText event and SetEditText method), when the OnValidateEntry event triggers, the cell already contains the entered value (valid or not); using the event arguments OldValue and NewValue the cell value can be validated and changed if needed. 

Usually validation occurs when the user has moved to another cell. If validation then fails, it is desirable to keep the cell editor visible/focused so the entered value can be corrected by user. To let the grid know that validation has failed, an exception needs to be raised. The grid will handle the exception to Application.HandleException and any movement is cancelled. 

For example, suppose that cell[1,1] should hold only values 'A' or 'B', validation could be made with: 
    
    
    procedure TForm1.GridValidateEntry(sender: TObject; aCol,
      aRow: Integer; const OldValue: string; var NewValue: String);
    begin
      if (aCol=1) and (aRow=1) then begin
        if grid.Cells[aCol,aRow]<>'A') and grid.Cells[aCol,aRow]<>'B') then begin
          // set a new valid value so user can just press RETURN to continue for example.
          NewValue := 'A';
          // another option is reverting to previous cell value (which is assumed to be valid)
          // NewValue := OldValue;
          // Use EAbort rather than another Exception type to avoid spurious error
          // dialog boxes and backtraces
          raise EAbort.Create('Only A or B are allowed here');
        end else begin
          // if no exception is raised, it is assumed the value is valid, yet if necessary
          // the final value can still be changed by filling NewValue with a different value
           
          // computer knows better :)
          if grid.Cells[1,1]='A' then 
            NewValue := 'B' 
          else 
            NewValue := 'A';
        end;
      end;
    end;
    

### Sorting Columns or Rows

Property ColumnClickSorts allows grid to be sorted automatically when user clicks a column header. Clicking the same column many times switches the sort order. Default column sort images are shown to indicate which column was clicked. 

In code you can use SortColRow() method. 

The first parameter, IsColumn, is a boolean value which indicates: 

  * True if a column is to be sorted
  * False if a row is to be sorted.



The second parameter, index, is an integer value: 

  * Index Indicates Column or Row to be sorted.



The last two parameters are optional, defines subrange of rows or columns to be sorted. 

  * FromIndex (From column or row)
  * ToIndex (To column or row)



If the last two parameters are not specified, the whole column or row is sorted. 

Sorting uses QuickSort algorithm, it could be changed if a descendant grid overrides the sort() method and calls doCompareCells for cell compare. 

By default it sorts cell content as strings either in ascending or descending order which is selectable with property SortOrder. By default it uses ascending order. 
    
    
    // Sort column 3 in ascending order
    grid.SortColRow(true, 3);
    
    // Sort column 3 in descending order, skip fixed rows a top
    grid.SortOrder := soDescending; // or soAscending
    grid.SortColRow(true, 3, grid.FixedRows, grid.RowCount-1);
    

For custom sorting of numbers, dates, states, etc: StringGrid has the OnCompareCells event which users can handle for example this way: 
    
    
    procedure TForm1.GridCompareCells(Sender: TObject; ACol, ARow, BCol, BRow: Integer; var Result: integer);
    begin
      // Result will be either <0, =0, or >0 for normal order.
      result := StrToIntDef(Grid.Cells[ACol,ARow],0)-StrToIntDef(Grid.Cells[BCol,BRow],0);
      // For inverse order, just negate the result (eg. based on grid's SortOrder).
      if StringGrid1.SortOrder = soDescending then
        result := -result;
    end;
    

You can use OnCompareCells also when automatic column sorting is enabled through ColumnClickSorts property. 

### Override automatic sort (ColumnClickSorts set to False). Avoid sort of some columns. Show sort arrows in column header

(ColumnClickSorts set to False will NOT show sort arrows in column header, so we have to set the arrows.) 
    
    
    procedure TForm1.StringGrid1HeaderClick(Sender: TObject; IsColumn: Boolean;
     Index: Integer);
    begin
     if isColumn then with StringGrid1 do
      begin
        col:=Index;                                                  // Set column to sort
        if Tag  = 0 then Tag :=-1                                    // Suppose data in column is Ascending
                    else Tag := Tag  * -1;                           // Using StringGrid1.Tag to hold sortorder  
                                                                     // First click of header will sort descending 
       
         // Activate CompareCells (ativate the sort)                                                                            
        case Index of
          0: col:=-1;                                                // Do not sort column 0 and do not show sort arrow
          1: SortColRow(True, Index);                                // The sort will take place in CompareCells as soon as statement is executed
          2: SortColRow(True, Index);
          3: SortColRow(True, Index);
          4: SortColRow(True, Index);
          5: col:=-1;                                                // Do not sort column 5 and do not show sort arrow
        end;
      end;
    end;
    
    // Compare returns one of the following values:
    // if A<B  return: -1            ( Ascending )
    // if A=B  return:  0
    // if A>B  return:  1            ( Descending)
    
    // How to sort: Text, Integer, Float, DateTime:
    
    procedure TForm1.StringGrid1CompareCells(Sender: TObject; ACol, ARow, BCol,
     BRow: Integer; var Result: integer);
    
      function CompareTextAsText: Integer;                         // Text
      begin
        CompareText(Cells[ACol,ARow], Cells[BCol,BRow]);
      end;
    
      function CompareTextAsInteger: Integer;
      var i1,i2:Integer;
      begin
        if TryStrToInt(StringGrid1.Cells[ACol,ARow],i1) and       // Integer
           TryStrToInt(StringGrid1.Cells[BCol,BRow],i2) then
        Result := CompareValue(i1, i2)
        else Exit(0);
      end;  
    
      function CompareTextAsFloat: Integer;                        // Float
      var f1,f2:Extended;
      begin
        if TryStrToFloat(StringGrid1.Cells[ACol,ARow],f1) and
           TryStrToFloat(StringGrid1.Cells[BCol,BRow],f2) then
        Result := CompareValue(f1, f2)
        else Exit(0);
      end;
    
      function CompareTextAsDateTime: Integer;                  // DateTime
      var
      D1, D2 : TDateTime;
      begin
        if Trim(StringGrid1.Cells[ACol,ARow]) <> Trim(StringGrid1.Cells[BCol,BRow]) then
        begin
          if trim(StringGrid1.Cells[ACol,ARow]) = '' then Exit(1);
          if Trim(StringGrid1.Cells[BCol,BRow]) = '' then Exit(-1);
        end
        else Exit(0);
    
        if TryStrToDate(StringGrid1.Cells[ACol,ARow], D1, 'yy/mm/dd') and
           TryStrToDate(StringGrid1.Cells[BCol,BRow], D2, 'yy/mm/dd') then
        Result := CompareDateTime(D1, D2)
        else Exit(0);
     end;
    
    begin                                        // Don't do anything else here, sort might be done incorrect
      With    StringGrid1 do
      begin
        case ACol of
          0 : ;                                                                     // Do not sort column 0 
          1 :  Result := CompareTextAsText;                                         // Text
          2 :  Result := CompareTextAsInteger;                                      // Integer
          3 :  Result := CompareTextAsFloat;                                        // Float
          4 :  Result := CompareTextAsDateTime;                                     // DateTime
        end; {Case}
      end;
      if  StringGrid1.Tag  = -1 then   Result := -Result;                           // Switch the sort order
    end;  
    
    // DrawCell does the trick, previous arrow images are removed: 
    procedure TForm1.StringGrid1DrawCell(Sender: TObject; aCol, aRow: Integer;
      aRect: TRect; aState: TGridDrawState);
       
       procedure DrawArrow(ImgIndex:Integer);                  // Need Up/Down arrows in a ImageList
       begin
         with StringGrid1 do
           TitleImageList.Draw(Canvas,                         // TitleImageList of StringGrid
           ARect.Right - TitleImageList.Width,
          (ARect.Bottom - ARect.Top - TitleImageList.Height) div 2 + 2,  // Position of arrow, may be adjusted.
           ImgIndex );
       end;
    begin
      if Col > 0 then                                    // In this demo no arrows in col 0  
        if (ARow = 0) and                                // Suppose Row 0 as TitleRow
           Assigned(TitleImageList) then                 // Need Up/Down arrows in a ImageList
             case Columns.Items[aCol].Tag of             // Use Tag of Column as index of Ascending/Descending arrow
               -1: DrawArrow(1);                         // Replace with your Index of down arrow
                0: DrawArrow(-1);                        // Remove previous image
                1: DrawArrow(2);                         // Replace with your Index of Up Arrow
             end;
    end;
    

### Highlighting the selected cell column and row

In Lazarus revision 40276 an option (**gdRowHighlight**) has been added, it works similar to **goRowSelect** but it uses a lighter color for selection and the focus rect is only selected cell and not in the whole row. This works fine for rows, but how about columns?. 

This section presents a way to highlight columns, rows or both (an example that highlights only column and row headers can be found in lazarus/examples/gridexamples/spreadsheet, this howto is really an extension of that example). This uses two grid events: **OnBeforeSelection** and **OnPrepareCanvas** , drawing is not necessary. 

The event **OnBeforeSelection** is triggered when the selection is about the change, in this event we can know what cell is currently selected and what cell will be selected next. We use this information to invalidate the whole row or column of both the old and the new cells. When the next paint cycle starts, the grid will be instructed to paint the cells that belong to the invalidated areas. One of the first steps for painting is calling the **OnPrepareCanvas** event (if it exists) to setup default canvas properties. We use this event to set up the highlighted row or column: 
    
    
    procedure TForm1.gridBeforeSelection(Sender: TObject; aCol, aRow: Integer);
    begin
      // we can decide here if we want highlight columns, rows or both
      // in this example we highlight both
      if Grid.Col<>aCol then
      begin
        // a change on current column is detected
        grid.InvalidateCol(aCol);      // invalidate the new selected cell column
        grid.InvalidateCol(grid.Col);  // invalidate the current (it will be the 'old') selected column
      end;
      if Grid.Row<>aRow then
      begin
        grid.InvalidateRow(aRow);      // invalidate the new selected cell row
        grid.InvalidateRow(grid.Row);  // invalidate the current (it will be the 'old') selected row 
      end;
    end; 
    
    procedure TForm1.gridPrepareCanvas(sender: TObject; aCol, aRow: Integer;
      aState: TGridDrawState);
    begin
      if gdFixed in aState then
      begin
        if (aCol=grid.Col) or (aRow=grid.Row) then
          grid.Canvas.Brush.Color := clInactiveCaption; // this would highlight also column or row headers
      end else
      if gdFocused in aState then begin
        // we leave alone the current selected/focused cell
      end else
      if (aCol=Grid.Col) or (aRow=grid.Row) then
        grid.Canvas.Brush.Color := clSkyBlue; // highlight rows and columns with clSkyBlue color
    end;
    

### Avoiding the empty space at the right of the last column

The grid must be in column mode to achieve this. When the grid property `AutoFillColumns` is `true` the empty space is divided by all columns which have a non-zero `SizePriority`. Therefore, you should follow these steps to fill up all horizontal space of a grid: 

  * Set `AutoFillColumns` to `true`.
  * Using the Columns editor, add as many columns as you need.
  * Select each column in the object tree above the object inspector and set its `SizePriority` to `0`.
  * Now decide while column is widened such that all empty space is used up. Set the `SizePriority` of this column to `1`.



### How to scroll a Grid in "realtime" (moving the scrollbar via mouse)

If you scroll a Grid by moving the scrollbar with the mouse then the Grid per default is not moved "in realtime". Only after you release the mouse, the Grid "jumps" to its new position. You can enable a scrolling in "realtime" by adding Option `goThumbTracking`. 

Example: 
    
    
     StringGrid1.Options:=StringGrid1.Options + [goThumbTracking];
    

Unfortunately this Option is neither documented in `<lazarus-installdir>\lcl\grids.pas` (ver 2.0.6) nor in <https://lazarus-ccr.sourceforge.io/docs/lcl/grids/tgridoptions.html>

### How to resize a row based on cells content

Lacak in bug report [#38473](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38473>) contributed the following code: 
    
    
     
    type
    { TStringGridHelper }
      TStringGridHelper = class helper for TStringGrid
        protected
          procedure AutoSizeRow(aRow: integer);
      end;
    
    { TStringGridHelper }
    procedure TStringGridHelper.AutoSizeRow(aRow: integer);
    var
      aCanvas: TCanvas;
      aCol, maxRowHeight:integer;
      aText: String;
      textRect: TRect;
    begin
      aCanvas := GetWorkingCanvas(Canvas);
      maxRowHeight := DefaultRowHeight;
      for aCol:=0 to ColCount-1 do begin
        aText := Cells[aCol,aRow];
        textRect := Rect(0, 0, ColWidths[aCol], MaxInt);
        DrawText(aCanvas.Handle, PChar(aText), Length(aText), textRect, DT_CALCRECT or DT_WORDBREAK);
        if maxRowHeight < textRect.Height then
          maxRowHeight := textRect.Height
      end;
      if aCanvas<>Canvas then FreeWorkingCanvas(aCanvas);
      RowHeights[aRow] := maxRowHeight+2;
    end;
    

Add that somewhere in your source code and then you can resize a row as if the AutoSizeRow procedure is part of the stringgrid, example: 
    
    
    // Resize the row index 2
    grid.AutoSizeRow(2);

## DBGrids Howto and Examples

### Example: How to set a memo editor for dbgrids

You can, of course, use another control instead of a TMemo. Adjust to taste. Adapted from [[1]](<http://forum.lazarus.freepascal.org/index.php?topic=3640.0>) (simplified to use the SelectedFieldRect property, see [[2]](<http://forum.lazarus.freepascal.org/index.php/topic,8713.msg43288.html#msg43288>)) 

  * Place a memo control (or whatever control you want) on your form, set whatever properties you want and set visible to false. This will be used when editing a grid cell. We'll use GridCellMemo in this example.
  * In the OnSelectEditor event put the following code - adapt if you don't want to edit logical column 3, physical column 4:


    
    
      if (Column.DesignIndex = 3) then
      begin
          GridCellMemo.BoundsRect := DbGrid.SelectedFieldRect;
          GridCellMemo.Text:=Column.Field.AsString;
          Editor := GridCellMemo;
      end;
    

  * Suppose your datasource is called Datasource1 and DBGrid is called ResultsGrid. Then, in the OnEditingDone event for the GridCellMemo put the following:



(The original forum post used the OnChange event, which would fire each time the content is changed. Using OnEditingDone only fires after the user is finished with his edits.) 
    
    
      if not(Datasource1.State in [dsEdit, dsInsert]) then
        Datasource1.Edit;
        
      Datasource1.DataSet.FieldByName(ResultsGrid.SelectedField.FieldName).AsString:=GridCellMemo.Text;
    

**Beware** Using a normal control as a custom editor has many drawbacks: it won't resize or reposition itself when resizing the columns or scrolling through the grid, you have to take care yourself of modifying the data in the grid (as explained above), it doesn't work properly with DbGrids and interferes with normal grid navigation using the keyboard. The grid interacts with the editor using some special grid messages, so it's better to subclass a control in order to manage those messages, as explained in chapter five of [this document](<http://www.freepascal.org/~michael/articles/grids/grids.pdf>). 

**Beware** If you have more than one grid in the same form use a different custom editor for each one: if you use the same control bad things will happen since both grids will try to use it at once. 

### Example: Avoid displaying text fields as "(Memo)" for DBGrids

now (08/2023) exist the dbgrid option dgDisplayMemoText in object inspector - option

  
When you use SQL statements like "Select * from A" you might see "(Memo)" instead of your content in a grid cell. There are many ways to fix this, but maybe the easiest is to change your statement "Select * from A" to "Select Cast(Column as TEXT) as Column from A". This is a common problem when you use a DBGrid component. 
    
    
     var
        sql : UTF8String;
        Queryu : TSQLQuery;
    .....
        sql := 'SELECT cast(Name as TEXT) as Name FROM Client';
        Queryu.SQL.Text:=sql;
    

  


    for SQLITE3, to avoid cast function usage, just give len for fields in your sqlite database :  


    

    \- when you import from csv or create table : create table mytable (myfield TEXT (10))  

    \- or using sqlstudio or other to give len to fields

### Sorting columns or rows in DBGrid with sort arrows in column header

Here is an example that will sort a **DBgrid** using the **OnTitleClick** event and a **TSQLQuery** and indexes. This should also work for any compatible data set such as **TbufDataset**. The function uses the **column.tag** property to store the sort state for each column you click on, so when you go back to one you have already sorted it will pick the correct sort arrow to display. 

**Prerequisites:**

  * You will need an imagelist to store the up/down sort arrows. Assign your imagelist to the dbgrid's **TitleImageList** property.
  * Ensure the **[TSQLQuery.MaxIndexesCount](<http://lazarus-ccr.sourceforge.net/docs/fcl/sqldb/tsqlquery.maxindexescount.html> "doc:fcl/sqldb/tsqlquery.maxindexescount.html")** is large enough to hold the new indexes you will be creating. For this example I had it set to 100.
  * You will also need a private var to store the last column used to sort, for this example I have called it **FLastColumn**.



For this example the **TSQLQuery** used is called OpenQuery. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** As of March 21st 2013, **TSQLQuery** has no way to clear indexes, but as a work around you can set **unidirectional** to true, then set it to false and this will clear the indexes.

In order to reuse the **TSQLQuery** component with another SQL statement, the indexes must be cleared after sorting or an exception will be raised when you open the **TSQLQuery** with a different SQL statement. 
    
    
    FLastColumn: TColumn; //store last grid column we sorted on
    
    procedure TSQLForm.ResultsGridTitleClick(Column: TColumn);
    const
      ImageArrowUp=0; //should match image in imagelist
      ImageArrowDown=1; //should match image in imagelist
    var
      ASC_IndexName, DESC_IndexName:string;
      procedure UpdateIndexes;
      begin
        // Ensure index defs are up to date
        OpenQuery.IndexDefs.Updated:=false; {<<<--This line is critical. IndexDefs.Update will not
        update if already true, which will happen on the first column sorted.}
        Openquery.IndexDefs.Update;
      end;
    begin
      ASC_IndexName:='ASC_'+Column.FieldName;
      DESC_IndexName:='DESC_'+Column.FieldName;
      // indexes can't sort binary types such as ftMemo, ftBLOB
      if (Column.Field.DataType in [ftBLOB,ftMemo,ftWideMemo]) then
        exit;
      // check if an ascending index already exists for this column.
      // if not, create one
      if OpenQuery.IndexDefs.IndexOf(ASC_IndexName) = -1 then
      begin
        OpenQuery.AddIndex(ASC_IndexName,column.FieldName,[]);
        UpdateIndexes; //ensure index defs are up to date
      end;
      // Check if a descending index already exists for this column
      // if not, create one
      if OpenQuery.IndexDefs.IndexOf(DESC_IndexName) = -1 then
      begin
        OpenQuery.AddIndex(DESC_IndexName,column.FieldName,[ixDescending]);
        UpdateIndexes; //ensure index defs are up to date
      end;
    
      // Use the column tag to toggle ASC/DESC
      column.tag := not column.tag;
      if boolean(column.tag) then
      begin
        Column.Title.ImageIndex:=ImageArrowUp;
        Openquery.IndexName:=ASC_IndexName;
      end 
      else
      begin
        Column.Title.ImageIndex:=ImageArrowDown;
        OpenQuery.IndexName:=DESC_IndexName;
      end;
      // Remove the sort arrow from the previous column we sorted
      if (FLastColumn <> nil) and (FlastColumn <> Column) then
        FLastColumn.Title.ImageIndex:=-1;
      FLastColumn:=column;
    end;
    

### Selecting Records in a DBGrid using checkboxes

The objective is to be able to select arbitrary records in a dbgrid using checkboxes, the grid has the ability to show checkboxes automatically when it detects there are boolean fields, for other field types the user can manually choose the cbsCheckboxColumn ButtonStyle for the column. For this kind of columns the user just click the checkbox and the field content is modified accordingly. 

But what happen if there is no such available field in our dataset? or we don't want the grid enter edit state when checking the checkbox?. Adding a fieldless column with ButtonStyle=cbsCheckboxColumn will show all checkboxes grayed and disabled because there is no field linked to this column and so nothing to modify. As we want to handle the checkbox state ourselves we need to store the state somewhere for each record. For this we can use the class TBookmarklist (defined in dbgrids.pas unit) where property CurrentRowSelected can tell if the current record is selected or not. By using dbgrid events OnCellClick and OnUserCheckboxState we can track the checkbox state. 

Note this technique needs Lazarus r31148 or later which implements event OnUserCheckboxState. 
    
    
    ...
    uses ..., dbgrids, stdctrls, ...
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
      ...
        procedure DBGrid1CellClick(Column: TColumn);
        procedure DBGrid1UserCheckboxState(sender: TObject; column: TColumn; var AState: TCheckboxState);
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      ...
      private
        RecList: TBookmarklist;
      ...
      end;
    
    procedure TForm1.DBGrid1CellClick(Column: TColumn);
    begin
      if Column.Index=1 then
        RecList.CurrentRowSelected := not RecList.CurrentRowSelected;
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      RecList := TBookmarkList.Create(DbGrid1);
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      RecList.Free;
    end;
    
    procedure TForm1.DBGrid1UserCheckboxState(sender: TObject; column: TColumn; var AState: TCheckboxState);
    begin
      if RecList.CurrentRowSelected then
        AState := cbChecked
      else
        AState := cbUnchecked;
    end;
    

### Example: How to export a DBGrid to a text file

There are several ways to create a text file from the content of a DBGrid, normally people will tell you that instead of thinking of a DBGrid as a data container one should look a the linked dataset instead, but what happen when the text representation you want must match the grids appearance of the columns visibility, width or order? one solution is to use the LazReport suite that has a component specifically for producing a report from a DbGrid, this report can then be exported to several other formats including a text format. Here another simple solution is presented which can also be used as the basis for other text format exporters, for example, a simple html exporter. 
    
    
    // aGrid: The grid to be exported
    // lpp: Lines per page. Not including the text header
    // pageBreak: insert a page break that some printers could use for starting a new page
    // require: uses LazUTF8
    function GridTextExporter(aGrid: TDbGrid; lpp: Integer; pageBreak:boolean): TStringList;
    const
      PX_PER_CHAR = 7;
    
    var
      Ds: TDataset;
      book: TBookMark;
      recCount: Integer;
      line: string;
      column: TColumn;
    
      function WidthToChar(aWidth: Integer): integer;
      begin
        result := trunc(aWidth/PX_PER_CHAR);
      end;
    
      procedure AddNext(theText: string; alignment: TAlignment);
      var
        width: Integer;
      begin
        if (line<>'') and (line<>#12) then
          line := line + ' ';
    
        width := WidthToChar(Column.Width);
    
        case Alignment of
          taRightJustify: line := line + UTF8PadLeft(theText, width);
          taCenter:       line := line + UTF8PadCenter(theText, width);
          else            line := line + UTF8PadRight(theText, width);
        end;
      end;
    
      procedure CollectHeader;
      begin
        AddNext(Column.Title.Caption, Column.Title.Alignment);
      end;
    
      procedure CollectField;
      var
        field: TField;
      begin
        field := Column.Field;
    
        if (field.DataType=ftMemo) and (dgDisplayMemoText in aGrid.Options) then
          AddNext(field.AsString, Column.Alignment)
        else if Field.DataType<>ftBlob then
          AddNext(field.DisplayText, Column.Alignment)
        else
          AddNext('(blob)', Column.Alignment);
      end;
    
      procedure LoopColumns(printingRecord: boolean);
      var
        c: TCollectionItem;
      begin
        if (not printingRecord) and pageBreak and (result.count>0) then
          line := #12
        else
          line := '';
        for c in aGrid.Columns do begin
          Column := TColumn(c);
          if Column.Visible and (Column.Width>=PX_PER_CHAR) then begin
            if printingRecord then CollectField
            else                   CollectHeader;
          end;
        end;
        result.add(line);
      end;
    
    begin
      result := TStringList.create;
      Ds := aGrid.DataSource.DataSet;
      Ds.DisableControls;
      book := Ds.GetBookmark;
      try
        Ds.First;
        recCount := 0;
        while not DS.EOF do begin
          if RecCount mod lpp = 0 then
            LoopColumns(false);
          LoopColumns(true);
          inc(recCount);
          Ds.Next;
        end;
      finally
        Ds.GotoBookmark(book);
        Ds.FreeBookmark(book);
        Ds.EnableControls;
      end;
    end;
    

To use it one simply does: 
    
    
    var
      L: TStringList;
    begin
      L := GridTextExporter(grid, 80, true);
      L.SaveToFile('grid.txt');
      L.Free;
    end;

---

_Source: [https://wiki.freepascal.org/Grids_Reference_Page](https://web.archive.org/web/20250423234157/https://wiki.freepascal.org/Grids_Reference_Page)_
