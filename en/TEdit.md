# TEdit

│ **English (en)** │  **[русский (ru)](<../ru/TEdit.md>)** │

A **TEdit** [![tedit.png](https://wiki.freepascal.org/images/0/07/tedit.png)](</File:tedit.png>) is a control with a single line of editable text. It is available from the [Standard tab](<Standard_tab.md> "Standard tab") of the [Component Palette](<Component_Palette.md> "Component Palette"). 

## Contents

  * 1 Usage
    * 1.1 Password input
    * 1.2 TextHint
  * 2 Requested Features
    * 2.1 Embedding Button/Other Controls in TEdit
  * 3 See also



## Usage

You can add a TEdit text input field to your [form](<TForm.md> "TForm") by clicking on it in the Standard tab of the component palette and place it on your form with one click. You can now edit this single-line text field at run time. 

If you want to use this text elsewhere, you can use it like every other [String](<String.md> "String"). 

For example, if you have a TEdit _Edit1_ on your form, you can use `myString := Edit1.Text;`. To change the shown text in _Edit1_ , you can use `Edit1.Text := myString;`. 

If you want to display another text instead of the default text (e.g. _Edit1_) in a TEdit _Edit1_ at the start of your application, you can proceed as follows: 

  * Select the TEdit on your form with one click.
  * Go to the properties tab in the Object Inspector.
  * Select the property **Text** and change it in the neighbouring input field.
  * In the same way, you can select the property **Name** and give the TEdit a better name.



### Password input

You can also easily use a TEdit to enter passwords. Then instead of the actually entered [Char](<Char.md> "Char") a PasswordChar is displayed. 

Little Example: 

  * Create a new GUI application with a TEdit _Edit1_ and a TButton _Button1_ on the form.
  * In the Object Inspector set the property PasswordChar of _Edit1_ to am asterisk ( * ).
  * In the event handler of _OnClick_ of _Button1_ show the entered password: `ShowMessage(Edit1.Text);`.



### TextHint

Have you used a GUI that has, for example, a Search Box where you can type in a search term. And when unused, that Search Box displays something like "Search Here" so you know what it is for ? Well, TEdit has a property called TextHint for that purpose. Set TextHint to some appropriate text and if the normal 'Text' is empty and the Control is not in use, your Search Here message is displayed. On most Widget Sets, the TextHint is automatically cleared once the user starts typing in there. The exception is GTK2/3, they like to clear things as soon as the Control gets focus. 

The text from TextHint is displays a little greyer than normal text so the user is aware it is somehow special. Most people find this interface quite intuitive. 

## Requested Features

### Embedding Button/Other Controls in TEdit

It's a WinAPI feature to allow embedding any sort of control into a TEdit. 

Considerations. 

  * The feature is not supported by all Widgetsets natively. For example: for Gtk2/Gtk3 it's not possible, as GtkEntry is not derived from a container. The SpinEdit control is a special kind of control in Gtk2. (This is the main reason on why [TSpinEdit](<TSpinEdit.md> "TSpinEdit") has its own WS class).


  * The placement of an embedded control (or controls) cannot be set by Left or Top properties (as normally happens). Instead the "Align" property should be used.


    
    
    button1.Parent:=edit1;
    button1.Width:=20;
    button1.Align:=alRight;
    button1.Constraints.MaxHeight:=18; //edit1.Height;
    

  * The "text" entry field must be limited by some TEdit property



    OR this should be done automatically by the widgetset.

Without such a limitation, the entered text goes "underneath" the embedded button. 

[![edit1.gif](https://wiki.freepascal.org/images/9/98/edit1.gif)](</File:edit1.gif>)

If using pure WinAPI, this can be achieved by using an EM_SETMARGINS message: 
    
    
    Uses
      Windows, ...
    
    ...
    
      SendMessage(Edit1.Handle, EM_SETMARGINS, EC_LEFTMARGIN or EC_RIGHTMARGIN, MakeLParam(0, button1.Width))
    

[![edit2.gif](https://wiki.freepascal.org/images/4/43/edit2.gif)](</File:edit2.gif>)

The Delphi VCL does provide a Margins property, but it is not mapped to EM_SETMARGINS and has a different meaning in the VCL. 

## See also

  * [TEdit doc](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/tedit.html> "doc:lcl/stdctrls/tedit.html")
  * [TMemo](<TMemo.md> "TMemo") \- A multiline text edit box
  * [TLabeledEdit](<TLabeledEdit.md> "TLabeledEdit") \- An edit field like a TEdit with a label
  * [TMaskEdit](<TMaskEdit.md> "TMaskEdit") \- An edit field with an edit mask
  * [TSpinEdit](<TSpinEdit.md> "TSpinEdit") \- An edit field for integers defined numbers range limits
  * [TFloatSpinEdit](<TFloatSpinEdit.md> "TFloatSpinEdit") \- An edit field for tiles floating-point numbers with defined numbers range limits
  * [TEditButton](</index.php?title=TEditButton&action=edit&redlink=1> "TEditButton \(page does not exist\)") \- An edit field like a TEdit with an attached button as a [TSpeedBtn](<TSpeedBtn.md> "TSpeedBtn")
  * [TFileNameEdit](</index.php?title=TFileNameEdit&action=edit&redlink=1> "TFileNameEdit \(page does not exist\)") \- An edit field with attached [TOpenDialog](<TOpenDialog.md> "TOpenDialog"), to enter a file name
  * [TDirectoryEdit](</index.php?title=TDirectoryEdit&action=edit&redlink=1> "TDirectoryEdit \(page does not exist\)") \- An edit field with attached [TSelectDirectoryDialog](<TSelectDirectoryDialog.md> "TSelectDirectoryDialog"), to enter a file path
  * [TDateEdit](<TDateEdit.md> "TDateEdit") \- An edit field with attached [TCalendarDialog](<TCalendarDialog.md> "TCalendarDialog") (calendar), to enter a date
  * [TCalcEdit](</index.php?title=TCalcEdit&action=edit&redlink=1> "TCalcEdit \(page does not exist\)") \- An input field with attached [TCalculatorDialog](<TCalculatorDialog.md> "TCalculatorDialog") (Calculator), to enter a number
  * [TTIEdit](</index.php?title=TTIEdit&action=edit&redlink=1> "TTIEdit \(page does not exist\)") \- An RTTI capable input field



  


[LCL Components](<LCL_Components.md> "LCL Components") Component Tab  | Components   
---|---  
[Standard](<Standard_tab.md> "Standard tab") | [TMainMenu](<TMainMenu.md> "TMainMenu") • [TPopupMenu](<TPopupMenu.md> "TPopupMenu") • [TButton](<TButton.md> "TButton") • [TLabel](<TLabel.md> "TLabel") • TEdit • [TMemo](<TMemo.md> "TMemo") • [TToggleBox](<TToggleBox.md> "TToggleBox") • [TCheckBox](<TCheckBox.md> "TCheckBox") • [TRadioButton](<TRadioButton.md> "TRadioButton") • [TListBox](<TListBox.md> "TListBox") • [TComboBox](<TComboBox.md> "TComboBox") • [TScrollBar](<TScrollBar.md> "TScrollBar") • [TGroupBox](<TGroupBox.md> "TGroupBox") • [TRadioGroup](<TRadioGroup.md> "TRadioGroup") • [TCheckGroup](<TCheckGroup.md> "TCheckGroup") • [TPanel](<TPanel.md> "TPanel") • [TFrame](<TFrame.md> "TFrame") • [TActionList](<TActionList.md> "TActionList")  
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

_Source: [https://wiki.freepascal.org/TEdit](https://web.archive.org/web/20240920204156/https://wiki.freepascal.org/TEdit)_
