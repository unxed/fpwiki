# TTreeView

│ **English (en)** │  **[русский (ru)](<../ru/TTreeView.md>)** │

A **TTreeView** [![ttreeview.png](https://wiki.freepascal.org/images/c/cd/ttreeview.png)](</File:ttreeview.png>) is a graphical control element that presents a hierarchical view of information. Each item can have a number of subitems. 

An item can be expanded to reveal subitems, if any exist, and collapsed to hide subitems. 

## Contents

  * 1 A Short example of using TTreeview
  * 2 Adding a new item in code
  * 3 Creating a TreeView which loads items only when expanding
  * 4 How to move a node in TTreeview
  * 5 Using multiple node selection
  * 6 Freeing TreeNode Data
  * 7 Using Drag and Drop in a TreeView
  * 8 Comments on Events
  * 9 See also



## A Short example of using TTreeview

Here is a simple example: 

  * Create a new application.
  * Add an empty treeview to the form, a button1 with caption "Add Child" and a button2 with caption "Delete"
  * Assign the following code to the buttons' OnClick events. Compile and run.


    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var 
      n: integer;
      s: string;
    begin
      // If there are no nodes, create a root node with a parent of nil
      if TreeView1.Items.Count = 0 then
      begin
        Treeview1.Items.Add(nil,'Root Node');
        exit;
      end;
     
      // Set up a simple text for each new node - "Node1", "Node2" etc
      n := Treeview1.Items.Count;
      s := 'Node ' + IntToStr(n);
    
      // Add a new node to the currently selected node
      if TreeView1.Selected <> nil then
        Treeview1.Items.AddChild(Treeview1.Selected, s);
    end;
     
    procedure TForm1.Button2Click(Sender: TObject);
    
      // A procedure to recursively delete nodes
      procedure DeleteNode(ANode: TTreeNode);
      begin
        while ANode.HasChildren do 
          DeleteNode(ANode.GetLastChild);
        TreeView1.Items.Delete(ANode);
      end;
     
    begin
      if TreeView1.Selected = nil then 
        exit;
      
      // If the selected node has child nodes, first ask for confirmation
      if Treeview1.Selected.HasChildren then
        if MessageDlg('Delete node and all children?', mtConfirmation, [mbYes,mbNo], 0 ) <> mrYes then 
          exit;
      DeleteNode(TreeView1.Selected);
    end;
    

When running, the treeview is empty. If you click "Add Child", a root node is created. After that a child will be added to any selected node by clicking "Add Child" again. 

The "Delete" button will delete the currently selected node. If it doesn't have children, it will delete it immediately, but if it has children, it will first ask. 

## Adding a new item in code

To add a sibling node to pre-existing node, use TTreeView.ITems.Add. 
    
    
    TreeView1.Items.Add(ATreeNode, 'Added node');
    

To add the first node, it becomes: 
    
    
    TreeView1.Items.Add(nil, 'First node');
    

Use TTreeView.Items.AddChild or AddChildObject to add child node. 
    
    
    TreeView1.Items.AddChild(ATreeNode, 'Child node');
    

## Creating a TreeView which loads items only when expanding

To add the expansion symbol to a node without subitems use: 
    
    
    MyNode.HasChildren := True;
    

And then set an event handler for the OnExpanding event. In this even you should return if the expansion can actually be made or not and when yes, you should add subitems to the node. If the expansion cannot be done, the expansion symbol will be automatically removed even if you have previously set HasChildren to true. 

## How to move a node in TTreeview

Assuming you have UP and Down buttons to move the selected node in the TTreeview, these procedures will achieve moving the selected node. 
    
    
    procedure TForm1.UpBtnClick(Sender: TObject);
    begin
      // Ensure a node is selected 
      if(Treeview1.Selected <> nil) then
        // Ensure there is a previous sibling node
        if Treeview1.Selected.GetPrevSibling <> nil then
          // If we have made it this far, move it UP
          Treeview1.Selected.MoveTo(Treeview1.Selected.GetPrevSibling, naInsert);
    end;
    
    procedure TForm1.DownBtnClick(Sender: TObject);
    begin
      // Ensure a node is selected 
      if(Treeview1.Selected <> nil) then
        // Ensure there is a next sibling node
        if Treeview1.Selected.GetNextSibling <> nil then
          // If we have made it this far, move it DOWN
          Treeview1.Selected.MoveTo(Treeview1.Selected.GetNextSibling, naInsertBehind);
    end;
    

Note: These procedures will only move nodes which are at the same level in the TTreeview. 

Table of attachment mode of Nodes   
---  
Mode | Effect   
naAdd | add as last sibling of target node   
naAddFirst | add as first sibling of target node   
naAddChild | add as last child of target node   
naAddChildFirst | add as first child of target node   
naInsert | insert in front of target node   
naInsertBehind | insert behind target node   
  
## Using multiple node selection

To allow multiple selection, you need to set the `tvoAllowMultiSelect` flag of the `Options` property. This will allow you to select additional nodes by `Ctrl`+`Mouse click`. 

To get a list of selected nodes from code, you can use the `TreeView1.Selections` property: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      i: integer;
    begin
      Memo1.Lines.Clear;
      for i := 0 to TreeView1.SelectionCount - 1 do
        Memo1.Lines.Add(TreeView1.Selections[i].GetTextPath);
    end;
    

## Freeing TreeNode Data

Use the TreeView's OnDeletion event to free your object. 
    
    
    procedure TForm1.TreeView1Deletion(Sender: TObject; Node: TTreeNode);
    begin
      TMyObject(Node.Data).Free;
      Node.Data := nil;
    end;
    

## Using Drag and Drop in a TreeView

If you want to allow a drag and drop function in your treeview, you need to : 

  1. Set to DmAutomatic, the property "Drag Mode" of your treeview
  2. Create events for `onDragDrop` and `OnDragOver`:


    
    
    ...
    
    Var
      Node: TTreeNode;
    
    ...
    
    procedure TForm1.TreeView1DragDrop(Sender, Source: TObject; X, Y: Integer);
    begin
      Treeview1 := TTreeView(Sender);    { Sender is TreeView where the data is being dropped  }
      Node := Treeview1.GetNodeAt(x,y);  { x,y are drop coordinates (relative to the Sender)   }
                                         {   since Sender is TreeView we can evaluate          }
                                         {   a tree at the X,Y coordinates                     }
      if Source = Sender then            { drop is happening within a TreeView   }
        begin
          if Assigned(Treeview1.Selected) and     {  check if any node has been selected  }
            (Node <> Treeview1.Selected) then     {   and we're dropping to another node  }
              begin
                if Node <> nil then
                  Treeview1.Selected.MoveTo(Node, naAddChild) { complete drop, by moving selected node }
                else
                  Treeview1.Selected.MoveTo(Node, naAdd);     { complete drop, by moving selected node in root }
              end;
        end;
    end;
    
    procedure TForm1.TreeView1DragOver(Sender, Source: TObject; X, Y: Integer; State: TDragState; var Accept: Boolean);
    begin
      Accept := true;
    end;
    

## Comments on Events

Event handlers are convenient places for developing LCL applications. There are some peculiar events for TTreeview. 

**When selected node is changed**
    
    
    procedure TForm1.TreeView1Change(Sender: TObject; Node: TTreeNode);
    

The `OnChange` event is fired when another node is selected. It can be used in a program to update some data. 
    
    
    procedure TForm1.TreeView1Changing(Sender: TObject; Node: TTreeNode; var AllowChange: Boolean);
    

The `OnChanging` event is called before `OnChange`, which allows you to prevent node selection under certain conditions. To do this, just set `AllowChange` to `false`. It is not necessary to set to `true` as this is the default value. 

Please note that in Delphi the `Node` variable points to the node that leaves the selection. In Lazarus, the `Node` variable points to the node receiving the selection, but you can use the `Selected` property to find out the "previous" node. 

In both events, the `Node` and `Selected` can be equal to `nil` (if there was no selection or it is being removed). 

**When a node is inserted**

When a new node is inserted, related events occur in the following order. 
    
    
          1) OnNodeChanged
          2) (OnChanging): This is called if there was a previously selected node.
          3) OnSelectionChanged
          4) OnChange
    

**When deleting a node**

When the selected node is deleted, the following events occur. 
    
    
         1) OnDelete
         2) OnChanging
         3) SelectionChanged
         4) OnChange :  This is called because when you delete a node, another node is selected. 
    

If an unselected node is deleted, e.g. programmatically, then only the OnDelete event handler is called. 

**When Form is Closed**

OnChanging event occurs, as the selected node is "de"selected. But OnChanging eventhander is executed AFTER the form is closed. This means that you cannot access the form's other data (other controls, other variables, etc.) within the OnChanging event handler procedure. 

**When Drag &Drop is done**

When you drag a node to another node and drop, the following events occur. 
    
    
         1) OnSelectionChanged
         2) OnNodeChanged
         3) OnChange
    

## See also

  * [Drag and Drop sample](<Drag_and_Drop_sample.md> "Drag and Drop sample")
  * [ TTreeView doc](<http://lazarus-ccr.sourceforge.net/docs//lcl/comctrls/ttreeview.html> "doc:/lcl/comctrls/ttreeview.html")
  * [TListView](<TListView.md> "TListView")
  * [VirtualTreeview](<VirtualTreeview.md> "VirtualTreeview")



  


[LCL Components](<LCL_Components.md> "LCL Components") Component Tab  | Components   
---|---  
[Standard](<Standard_tab.md> "Standard tab") | [TMainMenu](<TMainMenu.md> "TMainMenu") • [TPopupMenu](<TPopupMenu.md> "TPopupMenu") • [TButton](<TButton.md> "TButton") • [TLabel](<TLabel.md> "TLabel") • [TEdit](<TEdit.md> "TEdit") • [TMemo](<TMemo.md> "TMemo") • [TToggleBox](<TToggleBox.md> "TToggleBox") • [TCheckBox](<TCheckBox.md> "TCheckBox") • [TRadioButton](<TRadioButton.md> "TRadioButton") • [TListBox](<TListBox.md> "TListBox") • [TComboBox](<TComboBox.md> "TComboBox") • [TScrollBar](<TScrollBar.md> "TScrollBar") • [TGroupBox](<TGroupBox.md> "TGroupBox") • [TRadioGroup](<TRadioGroup.md> "TRadioGroup") • [TCheckGroup](<TCheckGroup.md> "TCheckGroup") • [TPanel](<TPanel.md> "TPanel") • [TFrame](<TFrame.md> "TFrame") • [TActionList](<TActionList.md> "TActionList")  
[Additional](<Additional_tab.md> "Additional tab") | [TBitBtn](<TBitBtn.md> "TBitBtn") • [TSpeedButton](<TSpeedButton.md> "TSpeedButton") • [TStaticText](<TStaticText.md> "TStaticText") • [TImage](<TImage.md> "TImage") • [TShape](<TShape.md> "TShape") • [TBevel](<TBevel.md> "TBevel") • [TPaintBox](<TPaintBox.md> "TPaintBox") • [TNotebook](<TNotebook.md> "TNotebook") • [TLabeledEdit](<TLabeledEdit.md> "TLabeledEdit") • [TSplitter](<TSplitter.md> "TSplitter") • [TTrayIcon](<TTrayIcon.md> "TTrayIcon") • [TControlBar](<TControlBar.md> "TControlBar") • [TFlowPanel](<TFlowPanel.md> "TFlowPanel") • [TMaskEdit](<TMaskEdit.md> "TMaskEdit") • [TCheckListBox](<TCheckListBox.md> "TCheckListBox") • [TScrollBox](<TScrollBox.md> "TScrollBox") • [TApplicationProperties](<TApplicationProperties.md> "TApplicationProperties") • [TStringGrid](<TStringGrid.md> "TStringGrid") • [TDrawGrid](<TDrawGrid.md> "TDrawGrid") • [TPairSplitter](<TPairSplitter.md> "TPairSplitter") • [TColorBox](<TColorBox.md> "TColorBox") • [TColorListBox](<TColorListBox.md> "TColorListBox") • [TValueListEditor](<TValueListEditor.md> "TValueListEditor")  
[Common Controls](<Common_Controls_tab.md> "Common Controls tab") | [TTrackBar](<TTrackBar.md> "TTrackBar") • [TProgressBar](<TProgressBar.md> "TProgressBar") • TTreeView • [TListView](<TListView.md> "TListView") • [TStatusBar](<TStatusBar.md> "TStatusBar") • [TToolBar](<TToolBar.md> "TToolBar") • [TCoolBar](<TCoolBar.md> "TCoolBar") • [TUpDown](<TUpDown.md> "TUpDown") • [TPageControl](<TPageControl.md> "TPageControl") • [TTabControl](<TTabControl.md> "TTabControl") • [THeaderControl](<THeaderControl.md> "THeaderControl") • [TImageList](<TImageList.md> "TImageList") • [TPopupNotifier](<TPopupNotifier.md> "TPopupNotifier") • [TDateTimePicker](<TDateTimePicker.md> "TDateTimePicker")  
[Dialogs](<Dialogs_tab.md> "Dialogs tab") | [TOpenDialog](<TOpenDialog.md> "TOpenDialog") • [TSaveDialog](<TSaveDialog.md> "TSaveDialog") • [TSelectDirectoryDialog](<TSelectDirectoryDialog.md> "TSelectDirectoryDialog") • [TColorDialog](<TColorDialog.md> "TColorDialog") • [TFontDialog](<TFontDialog.md> "TFontDialog") • [TFindDialog](<TFindDialog.md> "TFindDialog") • [TReplaceDialog](<TReplaceDialog.md> "TReplaceDialog") • [TTaskDialog](<TTaskDialog.md> "TTaskDialog") • [TOpenPictureDialog](<TOpenPictureDialog.md> "TOpenPictureDialog") • [TSavePictureDialog](<TSavePictureDialog.md> "TSavePictureDialog") • [TCalendarDialog](<TCalendarDialog.md> "TCalendarDialog") • [TCalculatorDialog](<TCalculatorDialog.md> "TCalculatorDialog") • [TPrinterSetupDialog](<TPrinterSetupDialog.md> "TPrinterSetupDialog") • [TPrintDialog](<TPrintDialog.md> "TPrintDialog") • [TPageSetupDialog](<TPageSetupDialog.md> "TPageSetupDialog")  
[Data Controls](<Data_Controls_tab.md> "Data Controls tab") | [TDBNavigator](<TDBNavigator.md> "TDBNavigator") • [TDBText](<TDBText.md> "TDBText") • [TDBEdit](<TDBEdit.md> "TDBEdit") • [TDBMemo](<TDBMemo.md> "TDBMemo") • [TDBImage](<TDBImage.md> "TDBImage") • [TDBListBox](<TDBListBox.md> "TDBListBox") • [TDBLookupListBox](<TDBLookupListBox.md> "TDBLookupListBox") • [TDBComboBox](<TDBComboBox.md> "TDBComboBox") • [TDBLookupComboBox](<TDBLookupComboBox.md> "TDBLookupComboBox") • [TDBCheckBox](<TDBCheckBox.md> "TDBCheckBox") • [TDBRadioGroup](<TDBRadioGroup.md> "TDBRadioGroup") • [TDBCalendar](<TDBCalendar.md> "TDBCalendar") • [TDBGroupBox](<TDBGroupBox.md> "TDBGroupBox") • [TDBGrid](<TDBGrid.md> "TDBGrid") • [TDBDateTimePicker](<TDBDateTimePicker.md> "TDBDateTimePicker")  
[Data Access](<Data_Access_tab.md> "Data Access tab") | [TDataSource](<TDataSource.md> "TDataSource") • [TCSVDataSet](<TCSVDataSet.md> "TCSVDataSet") • [TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet") • [TBufDataset](<TBufDataset.md> "TBufDataset") • [TFixedFormatDataSet](<TFixedFormatDataSet.md> "TFixedFormatDataSet") • [TDbf](<TDbf.md> "TDbf") • [TMemDataset](<TMemDataset.md> "TMemDataset")  
[System](<System_tab.md> "System tab") | [TTimer](<TTimer.md> "TTimer") • [TIdleTimer](<TIdleTimer.md> "TIdleTimer") • [TLazComponentQueue](<TLazComponentQueue.md> "TLazComponentQueue") • [THTMLHelpDatabase](<THTMLHelpDatabase.md> "THTMLHelpDatabase") • [THTMLBrowserHelpViewer](<THTMLBrowserHelpViewer.md> "THTMLBrowserHelpViewer") • [TAsyncProcess](<TAsyncProcess.md> "TAsyncProcess") • [TProcessUTF8](<TProcessUTF8.md> "TProcessUTF8") • [TProcess](<TProcess.md> "TProcess") • [TSimpleIPCClient](<TSimpleIPCClient.md> "TSimpleIPCClient") • [TSimpleIPCServer](<TSimpleIPCServer.md> "TSimpleIPCServer") • [TXMLConfig](<TXMLConfig.md> "TXMLConfig") • [TEventLog](<TEventLog.md> "TEventLog") • [TServiceManager](<TServiceManager.md> "TServiceManager") • [TCHMHelpDatabase](<TCHMHelpDatabase.md> "TCHMHelpDatabase") • [TLHelpConnector](<TLHelpConnector.md> "TLHelpConnector")  
[Misc](<Misc_tab.md> "Misc tab") | [TColorButton](<TColorButton.md> "TColorButton") • [TSpinEdit](<TSpinEdit.md> "TSpinEdit") • [TFloatSpinEdit](<TFloatSpinEdit.md> "TFloatSpinEdit") • [TArrow](<TArrow.md> "TArrow") • [TCalendar](<TCalendar.md> "TCalendar") • [TEditButton](</index.php?title=TEditButton&action=edit&redlink=1> "TEditButton \(page does not exist\)") • [TFileNameEdit](</index.php?title=TFileNameEdit&action=edit&redlink=1> "TFileNameEdit \(page does not exist\)") • [TDirectoryEdit](</index.php?title=TDirectoryEdit&action=edit&redlink=1> "TDirectoryEdit \(page does not exist\)") • [TDateEdit](<TDateEdit.md> "TDateEdit") • [TTimeEdit](<TTimeEdit.md> "TTimeEdit") • [TCalcEdit](</index.php?title=TCalcEdit&action=edit&redlink=1> "TCalcEdit \(page does not exist\)") • [TFileListBox](<TFileListBox.md> "TFileListBox") • [TFilterComboBox](</index.php?title=TFilterComboBox&action=edit&redlink=1> "TFilterComboBox \(page does not exist\)") • [TComboBoxEx](</index.php?title=TComboBoxEx&action=edit&redlink=1> "TComboBoxEx \(page does not exist\)") • [TCheckComboBox](</index.php?title=TCheckComboBox&action=edit&redlink=1> "TCheckComboBox \(page does not exist\)") • [TButtonPanel](<TButtonPanel.md> "TButtonPanel") • [TShellTreeView](</index.php?title=TShellTreeView&action=edit&redlink=1> "TShellTreeView \(page does not exist\)") • [TShellListView](<TShellListView.md> "TShellListView") • [TXMLPropStorage](<TXMLPropStorage.md> "TXMLPropStorage") • [TINIPropStorage](<TINIPropStorage.md> "TINIPropStorage") • [TJSONPropStorage](</index.php?title=TJSONPropStorage&action=edit&redlink=1> "TJSONPropStorage \(page does not exist\)") • [TIDEDialogLayoutStorage](</index.php?title=TIDEDialogLayoutStorage&action=edit&redlink=1> "TIDEDialogLayoutStorage \(page does not exist\)") • [TMRUManager](</index.php?title=TMRUManager&action=edit&redlink=1> "TMRUManager \(page does not exist\)") • [TStrHolder](</index.php?title=TStrHolder&action=edit&redlink=1> "TStrHolder \(page does not exist\)")  
[LazControls](<LazControls_tab.md> "LazControls tab") | [TCheckBoxThemed](</index.php?title=TCheckBoxThemed&action=edit&redlink=1> "TCheckBoxThemed \(page does not exist\)") • [TDividerBevel](<TDividerBevel.md> "TDividerBevel") • [TExtendedNotebook](</index.php?title=TExtendedNotebook&action=edit&redlink=1> "TExtendedNotebook \(page does not exist\)") • [TListFilterEdit](</index.php?title=TListFilterEdit&action=edit&redlink=1> "TListFilterEdit \(page does not exist\)") • [TListViewFilterEdit](<TListViewFilterEdit.md> "TListViewFilterEdit") • [TLvlGraphControl](</index.php?title=TLvlGraphControl&action=edit&redlink=1> "TLvlGraphControl \(page does not exist\)") • [TShortPathEdit](</index.php?title=TShortPathEdit&action=edit&redlink=1> "TShortPathEdit \(page does not exist\)") • [TSpinEditEx](<TSpinEditEx.md> "TSpinEditEx") • [TFloatSpinEditEx](<TFloatSpinEditEx.md> "TFloatSpinEditEx") • [TTreeFilterEdit](</index.php?title=TTreeFilterEdit&action=edit&redlink=1> "TTreeFilterEdit \(page does not exist\)") • [TExtendedTabControl](</index.php?title=TExtendedTabControl&action=edit&redlink=1> "TExtendedTabControl \(page does not exist\)") •   
[RTTI](<RTTI_tab.md> "RTTI tab") | [TTIEdit](</index.php?title=TTIEdit&action=edit&redlink=1> "TTIEdit \(page does not exist\)") • [TTIComboBox](<TTIComboBox.md> "TTIComboBox") • [TTIButton](</index.php?title=TTIButton&action=edit&redlink=1> "TTIButton \(page does not exist\)") • [TTICheckBox](</index.php?title=TTICheckBox&action=edit&redlink=1> "TTICheckBox \(page does not exist\)") • [TTILabel](</index.php?title=TTILabel&action=edit&redlink=1> "TTILabel \(page does not exist\)") • [TTIGroupBox](</index.php?title=TTIGroupBox&action=edit&redlink=1> "TTIGroupBox \(page does not exist\)") • [TTIRadioGroup](</index.php?title=TTIRadioGroup&action=edit&redlink=1> "TTIRadioGroup \(page does not exist\)") • [TTICheckGroup](</index.php?title=TTICheckGroup&action=edit&redlink=1> "TTICheckGroup \(page does not exist\)") • [TTICheckListBox](<TTICheckListBox.md> "TTICheckListBox") • [TTIListBox](</index.php?title=TTIListBox&action=edit&redlink=1> "TTIListBox \(page does not exist\)") • [TTIMemo](</index.php?title=TTIMemo&action=edit&redlink=1> "TTIMemo \(page does not exist\)") • [TTICalendar](</index.php?title=TTICalendar&action=edit&redlink=1> "TTICalendar \(page does not exist\)") • [TTIImage](</index.php?title=TTIImage&action=edit&redlink=1> "TTIImage \(page does not exist\)") • [TTIFloatSpinEdit](</index.php?title=TTIFloatSpinEdit&action=edit&redlink=1> "TTIFloatSpinEdit \(page does not exist\)") • [TTISpinEdit](</index.php?title=TTISpinEdit&action=edit&redlink=1> "TTISpinEdit \(page does not exist\)") • [TTITrackBar](</index.php?title=TTITrackBar&action=edit&redlink=1> "TTITrackBar \(page does not exist\)") • [TTIProgressBar](</index.php?title=TTIProgressBar&action=edit&redlink=1> "TTIProgressBar \(page does not exist\)") • [TTIMaskEdit](</index.php?title=TTIMaskEdit&action=edit&redlink=1> "TTIMaskEdit \(page does not exist\)") • [TTIColorButton](</index.php?title=TTIColorButton&action=edit&redlink=1> "TTIColorButton \(page does not exist\)") • [TMultiPropertyLink](<TMultiPropertyLink.md> "TMultiPropertyLink") • [TTIPropertyGrid](<TTIPropertyGrid.md> "TTIPropertyGrid") • [TTIGrid](</index.php?title=TTIGrid&action=edit&redlink=1> "TTIGrid \(page does not exist\)")  
[SQLdb](<SQLdb_tab.md> "SQLdb tab") | [TSQLQuery](<TSQLQuery.md> "TSQLQuery") • [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction") • [TSQLScript](<TSQLScript.md> "TSQLScript") • [TSQLConnector](<TSQLConnector.md> "TSQLConnector") • [TMSSQLConnection](<TMSSQLConnection.md> "TMSSQLConnection") • [TSybaseConnection](<TSybaseConnection.md> "TSybaseConnection") • [TPQConnection](<TPQConnection.md> "TPQConnection") • [TPQTEventMonitor](</index.php?title=TPQTEventMonitor&action=edit&redlink=1> "TPQTEventMonitor \(page does not exist\)") • [TOracleConnection](<TOracleConnection.md> "TOracleConnection") • [TODBCConnection](<TODBCConnection.md> "TODBCConnection") • [TMySQL40Connection](<TMySQL40Connection.md> "TMySQL40Connection") • [TMySQL41Connection](<TMySQL41Connection.md> "TMySQL41Connection") • [TMySQL50Connection](<TMySQL50Connection.md> "TMySQL50Connection") • [TMySQL51Connection](<TMySQL51Connection.md> "TMySQL51Connection") • [TMySQL55Connection](<TMySQL55Connection.md> "TMySQL55Connection") • [TMySQL56Connection](<TMySQL56Connection.md> "TMySQL56Connection") • [TMySQL57Connection](<TMySQL57Connection.md> "TMySQL57Connection") • [TSQLite3Connection](<TSQLite3Connection.md> "TSQLite3Connection") • [TIBConnection](<TIBConnection.md> "TIBConnection") • [TFBAdmin](<TFBAdmin.md> "TFBAdmin") • [TFBEventMonitor](<TFBEventMonitor.md> "TFBEventMonitor") • [TSQLDBLibraryLoader](<TSQLDBLibraryLoader.md> "TSQLDBLibraryLoader")  
[Pascal Script](<Pascal_Script_tab.md> "Pascal Script tab") | [TPSScript](</index.php?title=TPSScript&action=edit&redlink=1> "TPSScript \(page does not exist\)") • [TPSScriptDebugger](</index.php?title=TPSScriptDebugger&action=edit&redlink=1> "TPSScriptDebugger \(page does not exist\)") • [TPSDllPlugin](</index.php?title=TPSDllPlugin&action=edit&redlink=1> "TPSDllPlugin \(page does not exist\)") • [TPSImport_Classes](</index.php?title=TPSImport_Classes&action=edit&redlink=1> "TPSImport Classes \(page does not exist\)") • [TPSImport_DateUtils](</index.php?title=TPSImport_DateUtils&action=edit&redlink=1> "TPSImport DateUtils \(page does not exist\)") • [TPSImport_ComObj](</index.php?title=TPSImport_ComObj&action=edit&redlink=1> "TPSImport ComObj \(page does not exist\)") • [TPSImport_DB](</index.php?title=TPSImport_DB&action=edit&redlink=1> "TPSImport DB \(page does not exist\)") • [TPSImport_Forms](</index.php?title=TPSImport_Forms&action=edit&redlink=1> "TPSImport Forms \(page does not exist\)") • [TPSImport_Controls](</index.php?title=TPSImport_Controls&action=edit&redlink=1> "TPSImport Controls \(page does not exist\)") • [TPSImport_StdCtrls](</index.php?title=TPSImport_StdCtrls&action=edit&redlink=1> "TPSImport StdCtrls \(page does not exist\)") • [TPSCustomPlugin](</index.php?title=TPSCustomPlugin&action=edit&redlink=1> "TPSCustomPlugin \(page does not exist\)")  
[SynEdit](<SynEdit_tab.md> "SynEdit tab") | [TSynEdit](<TSynEdit.md> "TSynEdit") • [TSynCompletion](</index.php?title=TSynCompletion&action=edit&redlink=1> "TSynCompletion \(page does not exist\)") • [TSynAutoComplete](</index.php?title=TSynAutoComplete&action=edit&redlink=1> "TSynAutoComplete \(page does not exist\)") • [TSynMacroRecorder](</index.php?title=TSynMacroRecorder&action=edit&redlink=1> "TSynMacroRecorder \(page does not exist\)") • [TSynExporterHTML](</index.php?title=TSynExporterHTML&action=edit&redlink=1> "TSynExporterHTML \(page does not exist\)") • [TSynPluginSyncroEdit](</index.php?title=TSynPluginSyncroEdit&action=edit&redlink=1> "TSynPluginSyncroEdit \(page does not exist\)") • [TSynPasSyn](<TSynPasSyn.md> "TSynPasSyn") • [TSynFreePascalSyn](<TSynFreePascalSyn.md> "TSynFreePascalSyn") • [TSynCppSyn](<TSynCppSyn.md> "TSynCppSyn") • [TSynJavaSyn](<TSynJavaSyn.md> "TSynJavaSyn") • [TSynPerlSyn](<TSynPerlSyn.md> "TSynPerlSyn") • [TSynHTMLSyn](<TSynHTMLSyn.md> "TSynHTMLSyn") • [TSynXMLSyn](<TSynXMLSyn.md> "TSynXMLSyn") • [TSynLFMSyn](</index.php?title=TSynLFMSyn&action=edit&redlink=1> "TSynLFMSyn \(page does not exist\)") • [TSynDiffSyn](</index.php?title=TSynDiffSyn&action=edit&redlink=1> "TSynDiffSyn \(page does not exist\)") • [TSynUNIXShellScriptSyn](<TSynUNIXShellScriptSyn.md> "TSynUNIXShellScriptSyn") • [TSynCssSyn](<TSynCssSyn.md> "TSynCssSyn") • [TSynPHPSyn](<TSynPHPSyn.md> "TSynPHPSyn") • [TSynTeXSyn](<TSynTeXSyn.md> "TSynTeXSyn") • [TSynSQLSyn](<TSynSQLSyn.md> "TSynSQLSyn") • [TSynPythonSyn](<TSynPythonSyn.md> "TSynPythonSyn") • [TSynVBSyn](<TSynVBSyn.md> "TSynVBSyn") • [TSynAnySyn](</index.php?title=TSynAnySyn&action=edit&redlink=1> "TSynAnySyn \(page does not exist\)") • [TSynMultiSyn](</index.php?title=TSynMultiSyn&action=edit&redlink=1> "TSynMultiSyn \(page does not exist\)") • [TSynBatSyn](<TSynBatSyn.md> "TSynBatSyn") • [TSynIniSyn](<TSynIniSyn.md> "TSynIniSyn") • [TSynPoSyn](</index.php?title=TSynPoSyn&action=edit&redlink=1> "TSynPoSyn \(page does not exist\)")  
[Chart](<Chart_tab.md> "Chart tab") | [TChart](<TChart.md> "TChart") • [TListChartSource](</index.php?title=TListChartSource&action=edit&redlink=1> "TListChartSource \(page does not exist\)") • [TRandomChartSource](</index.php?title=TRandomChartSource&action=edit&redlink=1> "TRandomChartSource \(page does not exist\)") • [TUserDefinedChartSource](</index.php?title=TUserDefinedChartSource&action=edit&redlink=1> "TUserDefinedChartSource \(page does not exist\)") • [TCalculatedChartSource](</index.php?title=TCalculatedChartSource&action=edit&redlink=1> "TCalculatedChartSource \(page does not exist\)") • [TDbChartSource](</index.php?title=TDbChartSource&action=edit&redlink=1> "TDbChartSource \(page does not exist\)") • [TChartToolset](</index.php?title=TChartToolset&action=edit&redlink=1> "TChartToolset \(page does not exist\)") • [TChartAxisTransformations](</index.php?title=TChartAxisTransformations&action=edit&redlink=1> "TChartAxisTransformations \(page does not exist\)") • [TChartStyles](</index.php?title=TChartStyles&action=edit&redlink=1> "TChartStyles \(page does not exist\)") • [TChartLegendPanel](</index.php?title=TChartLegendPanel&action=edit&redlink=1> "TChartLegendPanel \(page does not exist\)") • [TChartNavScrollBar](</index.php?title=TChartNavScrollBar&action=edit&redlink=1> "TChartNavScrollBar \(page does not exist\)") • [TChartNavPanel](</index.php?title=TChartNavPanel&action=edit&redlink=1> "TChartNavPanel \(page does not exist\)") • [TIntervalChartSource](</index.php?title=TIntervalChartSource&action=edit&redlink=1> "TIntervalChartSource \(page does not exist\)") • [TDateTimeIntervalChartSource](</index.php?title=TDateTimeIntervalChartSource&action=edit&redlink=1> "TDateTimeIntervalChartSource \(page does not exist\)") • [TChartListBox](</index.php?title=TChartListBox&action=edit&redlink=1> "TChartListBox \(page does not exist\)") • [TChartExtentLink](</index.php?title=TChartExtentLink&action=edit&redlink=1> "TChartExtentLink \(page does not exist\)") • [TChartImageList](</index.php?title=TChartImageList&action=edit&redlink=1> "TChartImageList \(page does not exist\)")  
[IPro](<IPro_tab.md> "IPro tab") | [TIpFileDataProvider](<TIpFileDataProvider.md> "TIpFileDataProvider") • [TIpHtmlDataProvider](</index.php?title=TIpHtmlDataProvider&action=edit&redlink=1> "TIpHtmlDataProvider \(page does not exist\)") • [TIpHttpDataProvider](<TIpHttpDataProvider.md> "TIpHttpDataProvider") • [TIpHtmlPanel](<TIpHtmlPanel.md> "TIpHtmlPanel")  
[Virtual Controls](</index.php?title=Virtual_Controls_tab&action=edit&redlink=1> "Virtual Controls tab \(page does not exist\)") | [TVirtualDrawTree](</index.php?title=TVirtualDrawTree&action=edit&redlink=1> "TVirtualDrawTree \(page does not exist\)") • [TVirtualStringTree](</index.php?title=TVirtualStringTree&action=edit&redlink=1> "TVirtualStringTree \(page does not exist\)") • [TVTHeaderPopupMenu](</index.php?title=TVTHeaderPopupMenu&action=edit&redlink=1> "TVTHeaderPopupMenu \(page does not exist\)")  
  
  
****

---

_Source: [https://wiki.freepascal.org/TTreeView](https://web.archive.org/web/20240920142824/https://wiki.freepascal.org/TTreeView)_
