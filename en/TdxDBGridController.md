# TdxDBGridController

## Contents

  * 1 About
  * 2 Features
    * 2.1 Main functionalities
    * 2.2 TDBGrid events to achive the task
    * 2.3 Overriden event of the dataset linked to the TDBGrid
    * 2.4 TdxDBGridController events
  * 3 Examples
    * 3.1 Aggregation example on 2 lines
    * 3.2 Localization
  * 4 Screenshots
    * 4.1 Global search
    * 4.2 Multi sort
    * 4.3 Column filter
    * 4.4 Column search and aggregation
    * 4.5 TdxDBGridController properties
  * 5 See also



## About

TdxDBGridController is a non visual component with no dependencies providing added functionalities for the TDBGrid object. 

Download: <https://gitlab.com/lazaruscomponent/dbgridcontroller>

Demo project: <https://gitlab.com/lazaruscomponent/dbgridcontroller/-/tree/master/demo>

Licenses: MPL 2.0 or LGPL. 

[![TdxDBGridController 200.png](https://wiki.freepascal.org/images/5/5c/TdxDBGridController_200.png)](</File:TdxDBGridController_200.png>) TdxDBGridController component 

[![TdxDBGridController DataControl.png](https://wiki.freepascal.org/images/c/cb/TdxDBGridController_DataControl.png)](</File:TdxDBGridController_DataControl.png>)

TdxDBGridController will be installed in the Data Controls tab 

## Features

### Main functionalities

A grid controller that provides extra features to your existing TDBGrid. 

  * Searching expression in the grid
  * Searching expression in column
  * Column filter editor
  * Datetime editor, memo editor and lookup editor
  * Saving the current filter view (JSON)
  * Multi column sorting
  * Column chooser editor
  * Column grouping visual separator on one level
  * No data indicator
  * Footer and aggregation on columns
  * Perform 50000 records under a second



### TDBGrid events to achive the task

For all these events the inherited events are called before they are triggered, it is possible that these cause unexpected behavior, it is therefore necessary to validate the relevance of keeping them or at least validating their execution. 

  * OnDrawColumnTitle
  * OnDrawColumnCell
  * OnEditButtonClick
  * OnGetCellHint
  * OnKeyDown
  * OnMouseMove
  * OnMouseDown
  * OnTitleClick



### Overriden event of the dataset linked to the TDBGrid

If an OnFilterRecord event is used by the dataset, it will be override by the builtin event of the controller, no inheritance is taken into account in this case. Need this to achive the filtering with no interference. 

  * OnFilterRecord



### TdxDBGridController events

OnAfterFilterGrid  | Event handler triggered after filter execution   
---|---  
OnAfterSortColumn  | Event handler triggered after sorting a column   
OnAggregation  | Event handler to define which column is showing an aggregation   
OnBeforeSortColumn  | Event handler triggered before sorting a column   
OnLocalize  | Event handler triggered to translate string resources   
OnPrepareLookupDataset  | Event handler triggered to prepare the lookup dataset if needed   
OnSortColumn  | If your dataset is inherited from **TBufDataset** , **TSQLQuery** (Lazarus), **TMSQuery** , **TMSTable** (Devart), **TZQuery** , **TZTable** (ZeosLib) you don't need to use this event handler, the controller will automatically detect the class and set the **IndexDef** , **IndexFieldNames** or **SortedFields** properties accordingly to the dataset you're using. You can use the SQLOrderBy value to requery your dataset on the db server side if needed.   
  
## Examples

### Aggregation example on 2 lines
    
    
    Procedure TForm1.dxDBGridController1Aggregation(Sender: TdxDBGridController);
    Begin
       Sender.ColumnPropertyList.ColumnPropertyByName('ProjectManager').FooterAlignment   := taRightJustify;
       Sender.ColumnPropertyList.ColumnPropertyByName('ProjectManager').FooterDisplayText :=
          'Count Distinct : ' + Sender.ColumnPropertyList.Aggregation(agDistinct, 'ProjectManager').AsString;
       
       // Use the FooterPanel property to set the height : dxDBGridControler1.FooterPanel.Height := Self.DBGrid1.DefaultRowHeight * 2;  
       Sender.ColumnPropertyList.ColumnPropertyByName('OpeningDate').FooterAlignment   := taCenter;
       Sender.ColumnPropertyList.ColumnPropertyByName('OpeningDate').FooterDisplayText :=
          'Min : ' + Sender.ColumnPropertyList.Aggregation(agMin, 'OpeningDate').AsString + Char(13) + Char(10) +
          'Max : ' + Sender.ColumnPropertyList.Aggregation(agMax, 'OpeningDate').AsString;
    End;
    ‎
    

### Localization

This is an example, French translation of some of the component string resources: 
    
    
    Procedure TForm1.dxDBGridController1Localize(Sender: TObject; Component: TComponent; ID_Ressource: String; Var Translation: String);
    Begin
        If ID_Ressource = msg_search Then
             Translation := 'Recherche...'
          Else If ID_Ressource = msg_first Then
             Translation := 'Aller au début'
          Else If ID_Ressource = msg_prior Then
             Translation := 'Précédent'
          Else If ID_Ressource = msg_next Then
             Translation := 'Suivant'
          Else If ID_Ressource = msg_last Then
             Translation := 'Aller à la fin'
          Else If ID_Ressource = msg_add Then
             Translation := 'Ajouter'
          Else If ID_Ressource = msg_delete Then
             Translation := 'Détruire'
          Else If ID_Ressource = msg_edit Then
             Translation := 'Éditer'
          Else If ID_Ressource = msg_save Then
             Translation := 'Enregistrer'
          Else If ID_Ressource = msg_cancel Then
             Translation := 'Annuler'
          Else If ID_Ressource = msg_refresh Then
             Translation := 'Actualiser'
          Else If ID_Ressource = msg_btncancel Then
             Translation := 'Annuler'
          Else If ID_Ressource = msg_of Then
             Translation := 'de'
          Else If ID_Ressource = msg_nodata Then
             Translation := 'Aucune donnée trouvée!'
          Else If ID_Ressource = msg_columnchooser Then
             Translation := 'Sélectionner les colonnes...'
          Else If ID_Ressource = msg_clearselection Then
             Translation := 'Annuler sélection'
          Else If ID_Ressource = msg_selectall Then
             Translation := 'Tout sélectionner'
          Else If ID_Ressource = msg_sortingasc Then
             Translation := 'Trier en mode croissant'
          Else If ID_Ressource = msg_sortingdesc Then
             Translation := 'Trier en mode décroissant'; 
    End; 
    ‎
    

List of ressourcestring used by the TdxDBGridController 

  


  * msg_search = 'Search...';
  * msg_first = 'First';
  * msg_prior = 'Prior';
  * msg_next = 'Next';
  * msg_last = 'Last';
  * msg_add = 'Add';
  * msg_delete = 'Delete';
  * msg_edit = 'Edit';
  * msg_save = 'Save';
  * msg_cancel = 'Cancel';
  * msg_refresh = 'Refresh';
  * msg_btncancel = 'Cancel';
  * msg_btnok = 'OK';
  * msg_of = 'of';
  * msg_nodata = 'No Data Found!';
  * msg_columnchooser = 'Column Chooser...';
  * msg_clearselection = 'Clear Selection';
  * msg_selectall = 'Select All';
  * msg_sortingasc = 'Sort by ascending order';
  * msg_sortingdesc = 'Sort by descending order';



## Screenshots

### Global search

[![TdxDBGridController SearchGrid.png](https://wiki.freepascal.org/images/7/75/TdxDBGridController_SearchGrid.png)](</File:TdxDBGridController_SearchGrid.png>)

### Multi sort

A Ctrl+Left clic is use to performe a multi column sorting, the index will appear under the sort indicator. 

[![TdxDBGridController MultiSort.png](https://wiki.freepascal.org/images/1/13/TdxDBGridController_MultiSort.png)](</File:TdxDBGridController_MultiSort.png>)

### Column filter

Once you have focused on the grid you can have access to the column filter by clicking the filter icon or with the default shortcut Ctrl+J, this shortcut could be changed with the ColumnFilterShortCut property. 

[![TdxDBGridController ColumnFilter.png](https://wiki.freepascal.org/images/1/1b/TdxDBGridController_ColumnFilter.png)](</File:TdxDBGridController_ColumnFilter.png>)

### Column search and aggregation

It is not mandatory to use the footer panel to display the aggregation, you can use any other control at your convenience. The values are calculated and set in the OnAggregation event. In this example, the FooterPanel height of the TdxDBGridController was set to Self.DBGrid1.DefaultRowHeight * 2 in order to display the result on 2 lines. 

[![TdxDBGridController SearchAndAggregation.png](https://wiki.freepascal.org/images/c/cc/TdxDBGridController_SearchAndAggregation.png)](</File:TdxDBGridController_SearchAndAggregation.png>)

### TdxDBGridController properties

[![TdxDBGridController Properties.png](https://wiki.freepascal.org/images/6/6c/TdxDBGridController_Properties.png)](</File:TdxDBGridController_Properties.png>)

# See also

  * [TDBGrid doc](<http://lazarus-ccr.sourceforge.net/docs/lcl/dbgrids/tdbgrid.html> "doc:lcl/dbgrids/tdbgrid.html")
  * [TDBGrid](<TDBGrid.md> "TDBGrid")
  * [TStringGrid](<TStringGrid.md> "TStringGrid")
  * [Grids Reference Page](<Grids_Reference_Page.md> "Grids Reference Page")

---

_Source: [https://wiki.freepascal.org/TdxDBGridController](https://web.archive.org/web/20250114065508/https://wiki.freepascal.org/TdxDBGridController)_
