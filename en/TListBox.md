# TListBox

│ **[Deutsch (de)](</TListBox/de> "TListBox/de")** │  **English (en)** │  **[suomi (fi)](</TListBox/fi> "TListBox/fi")** │  **[français (fr)](</TListBox/fr> "TListBox/fr")** │    
****

A **TListBox** [![tlistbox.png](https://wiki.freepascal.org/images/7/7f/tlistbox.png)](</File:tlistbox.png>) is a component that shows a (scrollable) list of (short) strings where user is to select one. It is available from the [Standard tab](<Standard_tab.md> "Standard tab") of the [Component Palette](<Component_Palette.md> "Component Palette"). 

In the TListBox, the stored [strings](<String.md> "String") are stored in the property _Items_ , that is of type [TStrings](<TStrings.md> "TStrings"). Thus you can assign or remove strings in the ListBox, as in a [TStringList](<TStringList.md> "TStringList") or its parent [TStrings](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial"). 

Here are a few examples of how to use a TListBox _ListBox1_ on a form _Form1_ : 

## Contents

  * 1 Fill ListBox
    * 1.1 by the Object Inspector
    * 1.2 by code in button click
  * 2 Assignment of a StringList
  * 3 Add string
  * 4 Delete string
    * 4.1 at ItemIndex
    * 4.2 all selected strings
  * 5 Owner-drawn ListBox
  * 6 See also



## Fill ListBox

### by the Object Inspector

  * Select the ListBox on your form with one click.
  * Go in the Object Inspector in the Properties tab on the property _Items_.
  * Click on the button with the three dots. The String Editor opens.
  * Enter your text and confirm your work with _OK_.



### by code in button click

Add your form a [TButton](<TButton.md> "TButton") with the name _btnFill_ and caption _fill ListBox_. In the event handler _OnClick_ of the button, you write the following code: 
    
    
    procedure TForm1.btnFillClick(Sender: TObject);
    begin
      ListBox1.Items.Clear;             //Delete all existing strings
      ListBox1.Items.Add('First line');
      ListBox1.Items.Add('Line with random number '+IntToStr(Random(100)));
      ListBox1.Items.Add('Third line');
      ListBox1.Items.Add('Even a random number '+IntToStr(Random(100)));
    end;
    

## Assignment of a StringList

Add your form a [TButton](<TButton.md> "TButton") with the name _btnFill_ and caption _fill ListBox_. In the event handler _OnClick_ of the button, you write the following code: 
    
    
    procedure TForm1.btnFillClick(Sender: TObject);
    var
      myStringList: TStringList;
    begin
      myStringList:=TStringList.Create;               //Create my StringList
      myStringList.Add('This is the first line.');   //This add a row
      myStringList.Add('This is the second first line.');
      myStringList.Add('This is the third line.');
      myStringList.Add('etc.');
      ListBox1.Items.Assign(myStringList);            //assign the ListBox1 the text content of my StringList
      myStringList.Free;                              //Free my StringList in memory 
    end;
    

## Add string

  * Extend the example [Fill ListBox by code in button click](<TListBox.md> "TListBox") to a [TEdit](<TEdit.md> "TEdit") and a [TButton](<TButton.md> "TButton") with the name _btnAdd_ and caption _add string_. Change of _Edit1_ the property _Text_ to "" - empty string.
  * In the event handler _OnClick_ of the button, you write the following code:


    
    
    procedure TForm1.btnAddClick(Sender: TObject);
    begin
      ListBox1.Items.Add(Edit1.Text);
      Edit1.Text:='';
    end;
    

## Delete string

By default is set that you can select only one row in your list box. Do you want to select several of the lines in your ListBox, you would have the property _MultiSelect_ to make _True_. 

### at ItemIndex

  * Extend the example [Add string](<TListBox.md> "TListBox") to a [TButton](<TButton.md> "TButton") with the name "btnDel" and caption "delete string".
  * In the event handler _OnClick_ of the button, you write the following code:


    
    
    procedure TForm1.btnDelClick(Sender: TObject);
    begin
      if ListBox1.ItemIndex > -1 then    //Delete only when a string in the listbox is selected
        ListBox1.Items.Delete(ListBox1.ItemIndex);
    end;
    

### all selected strings

  * Extend the example [Add string](<TListBox.md> "TListBox") to a [TButton](<TButton.md> "TButton") with the name "btnDel" and caption "delete string".
  * In the event handler _OnClick_ of the button, you write the following code:


    
    
    procedure TForm1.btnDelClick(Sender: TObject);
    var
      i: Integer;
    begin
      if ListBox1.SelCount > 0 then                 //Delete only if at least one string in the list box is selected
        for i:=ListBox1.Items.Count - 1 downto 0 do //Iterate through all the items
          if ListBox1.Selected[i] then              //If selected...
            ListBox1.Items.Delete(i);               //...delete the item (String)
    end;
    

## Owner-drawn ListBox

In general, it is advantageous to let the ListBox follow the [theme](<http://en.wikipedia.org/wiki/Skin_%28computing%29>) set by the user. In some cases (for example, to program a game with a colorful surface), you can deviate from this standard and draw the control according to your own choice. You can try this now: 

  * You can modify the previous sample or create a new application with a TListBox _ListBox1_.
  * In the Object Inspector, change _ListBox1_ property _Style_ to _lbOwnerDrawFixed_.
  * With the Object Inspector, tab events, create the event handler for the event _OnDrawItem_ , by clicking on the button [...].
  * You add the following code to the handler:


    
    
    procedure TForm1.ListBox1DrawItem(Control: TWinControl; Index: Integer;
      ARect: TRect; State: TOwnerDrawState);
    var
      aColor: TColor;                       //Background color
    begin
      if (Index mod 2 = 0)                  //Index tells which item it is
        then aColor:=$FFFFFF                //every second item gets white as the background color
        else aColor:=$EEEEFF;               //every second item gets pink background color
      if odSelected in State then aColor:=$0000FF;  //If item is selected, then red as background color
      ListBox1.Canvas.Brush.Color:=aColor;  //Set background color
      ListBox1.Canvas.FillRect(ARect);      //Draw a filled rectangle
    
      ListBox1.Canvas.Font.Bold:=True;      //Set the font to "bold"
      ListBox1.Canvas.TextRect(ARect, 2, ARect.Top+2, ListBox1.Items[Index]);  //Draw Itemtext
    end;
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** **Parameters of ListBoxDrawItem:**  
  
**Control:**  
If multiple controls (E.g. multiple ListBoxes) access this event handle, you know which threw this event. You could in our example, instead of ****
    
    
    **ListBox1.Canvas.FillRect(ARect)
    **

also **
    
    
    TListBox(Control).Canvas.FillRect(ARect)
    

** write, where you should query still possible before, whether it is a TListBox: 
    
    
      if Control is TListBox then
        TListBox(Control).Canvas.FillRect(ARect);
    

**Index:**

Specifies the item location, so you have access to the string **
    
    
    < ListBox>.Items[Index]
    

**.  


**ARect:** Describes the rectangle, which is necessary for drawing the background.  
**State:** Status of the items, whether normal, focused, selected etc. 

  * Your example could look like:



[![ListBoxBsp1.png](https://wiki.freepascal.org/images/2/26/ListBoxBsp1.png)](</File:ListBoxBsp1.png>) -> [![ListBoxBsp2.png](https://wiki.freepascal.org/images/9/9f/ListBoxBsp2.png)](</File:ListBoxBsp2.png>)

## See also

  * [TListBox doc](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/tlistbox.html> "doc:lcl/stdctrls/tlistbox.html")
  * [TDBListBox](<TDBListBox.md> "TDBListBox")
  * [ListBox with separators](<ListBox_with_separators.md> "ListBox with separators")
  * [tutorial about TListBox](<http://forum.codecall.net/topic/73705-the-basic-working-with-tlistbox/>)



  


[LCL Components](<LCL_Components.md> "LCL Components") Component Tab  | Components   
---|---  
[Standard](<Standard_tab.md> "Standard tab") | [TMainMenu](<TMainMenu.md> "TMainMenu") • [TPopupMenu](<TPopupMenu.md> "TPopupMenu") • [TButton](<TButton.md> "TButton") • [TLabel](<TLabel.md> "TLabel") • [TEdit](<TEdit.md> "TEdit") • [TMemo](<TMemo.md> "TMemo") • [TToggleBox](<TToggleBox.md> "TToggleBox") • [TCheckBox](<TCheckBox.md> "TCheckBox") • [TRadioButton](<TRadioButton.md> "TRadioButton") • TListBox • [TComboBox](<TComboBox.md> "TComboBox") • [TScrollBar](<TScrollBar.md> "TScrollBar") • [TGroupBox](<TGroupBox.md> "TGroupBox") • [TRadioGroup](<TRadioGroup.md> "TRadioGroup") • [TCheckGroup](<TCheckGroup.md> "TCheckGroup") • [TPanel](<TPanel.md> "TPanel") • [TFrame](<TFrame.md> "TFrame") • [TActionList](<TActionList.md> "TActionList")  
[Additional](<Additional_tab.md> "Additional tab") | [TBitBtn](<TBitBtn.md> "TBitBtn") • [TSpeedButton](<TSpeedButton.md> "TSpeedButton") • [TStaticText](<TStaticText.md> "TStaticText") • [TImage](<TImage.md> "TImage") • [TShape](<TShape.md> "TShape") • [TBevel](<TBevel.md> "TBevel") • [TPaintBox](<TPaintBox.md> "TPaintBox") • [TNotebook](<TNotebook.md> "TNotebook") • [TLabeledEdit](<TLabeledEdit.md> "TLabeledEdit") • [TSplitter](<TSplitter.md> "TSplitter") • [TTrayIcon](<TTrayIcon.md> "TTrayIcon") • [TControlBar](<TControlBar.md> "TControlBar") • [TFlowPanel](<TFlowPanel.md> "TFlowPanel") • [TMaskEdit](<TMaskEdit.md> "TMaskEdit") • [TCheckListBox](<TCheckListBox.md> "TCheckListBox") • [TScrollBox](<TScrollBox.md> "TScrollBox") • [TApplicationProperties](<TApplicationProperties.md> "TApplicationProperties") • [TStringGrid](<TStringGrid.md> "TStringGrid") • [TDrawGrid](<TDrawGrid.md> "TDrawGrid") • [TPairSplitter](<TPairSplitter.md> "TPairSplitter") • [TColorBox](<TColorBox.md> "TColorBox") • [TColorListBox](<TColorListBox.md> "TColorListBox") • [TValueListEditor](<TValueListEditor.md> "TValueListEditor")  
[Common Controls](<Common_Controls_tab.md> "Common Controls tab") | [TTrackBar](<TTrackBar.md> "TTrackBar") • [TProgressBar](<TProgressBar.md> "TProgressBar") • [TTreeView](<TTreeView.md> "TTreeView") • [TListView](<TListView.md> "TListView") • [TStatusBar](<TStatusBar.md> "TStatusBar") • [TToolBar](<TToolBar.md> "TToolBar") • [TCoolBar](<TCoolBar.md> "TCoolBar") • [TUpDown](<TUpDown.md> "TUpDown") • [TPageControl](<TPageControl.md> "TPageControl") • [TTabControl](<TTabControl.md> "TTabControl") • [THeaderControl](<THeaderControl.md> "THeaderControl") • [TImageList](<TImageList.md> "TImageList") • [TPopupNotifier](<TPopupNotifier.md> "TPopupNotifier") • [TDateTimePicker](<TDateTimePicker.md> "TDateTimePicker")  
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

_Source: [https://wiki.freepascal.org/TListBox](https://web.archive.org/web/20231002175237/https://wiki.freepascal.org/TListBox)_
