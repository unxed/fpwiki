# TRadioButton

│ **[Deutsch (de)](</TRadioButton/de> "TRadioButton/de")** │  **English (en)** │  **[suomi (fi)](</TRadioButton/fi> "TRadioButton/fi")** │  **[français (fr)](</TRadioButton/fr> "TRadioButton/fr")** │  **[日本語 (ja)](</TRadioButton/ja> "TRadioButton/ja")** │    
****

[![](https://wiki.freepascal.org/images/a/a0/RadioButtonsRadioGroup.png)](</File:RadioButtonsRadioGroup.png>)

Comparison of several TRadioButton and a [TRadioGroup](<TRadioGroup.md> "TRadioGroup") with the characteristical properties given in the infobox.

**TRadioButton** [![tradiobutton.png](https://wiki.freepascal.org/images/e/ef/tradiobutton.png)](</File:tradiobutton.png>) provides an option to select "one-of-many" in a mutually exclusive way - if one RadioButton is selected, all others in the group are automatically deselected. The state of the **Checked** property is managed automatically among all RadioButtons belonging to the same parent component (there is no _ButtonGroup_ property that needs to be set manually).   


TRadioButtons are offered in the [Standard tab](<Standard_tab.md> "Standard tab") of the [Component Palette](<Component_Palette.md> "Component Palette"). To use a TRadioButton on a [Form](<TForm.md> "TForm"), you can simply select it on the component palette and place it, with one click on the form. 

It usually does not make sense to use a single radiobutton, because radiobuttons are intended to provide several options to chose from. Instead of individual radiobuttons you can also use a [TRadioGroup](<TRadioGroup.md> "TRadioGroup") or other elements like a [TComboBox](<TComboBox.md> "TComboBox") to let the user select an option from a list. 

In your source code, you can get the status of the radiobuttons, whether active or inactive, by using the _Checked_ property. This is of the type [Boolean](<Boolean.md> "Boolean"), thus the querying and the assignment of Boolean values are both possible. 
    
    
      // Assignment: 
      RadioButton1.checked := true;
    
      // Query:
      var x: Boolean;
      x := RadioButton1.checked;
    
      // Conditional Check:
      if RadioButton1.checked then
      begin
          ...
      end;
    

## Contents

  * 1 Examples
    * 1.1 Using an OnPaint event handler
    * 1.2 Using an OnChange event handler
  * 2 Grouping
    * 2.1 Example
  * 3 See also



## Examples

### Using an OnPaint event handler

  * Create a new application and drop three TRadioButtons on the form.
  * In the Object Inspector tab properties change the name the _RadioButton1...3_ to _rbRed_ , _rbGreen_ and _rbBlue_.
  * Similarly, you change the captions of the radiobuttons to _Red_ , _Green_ and _Blue_ there.
  * Add your form a [TButton](<TButton.md> "TButton") and change its caption to _Draw new_ and its name to _btnPaint_.
  * Create the _OnClick_ event handler for the TButton, by using the Object Inspector tab events, select the _OnClick_ event and click the button [...] or double click the button in the form.
  * Add following code:


    
    
    procedure TForm1.btnPaintClick(Sender: TObject);
    begin
      if rbRed.Checked   then Color:=clRed;
      if rbGreen.Checked then Color:=clLime;
      if rbBlue.Checked  then Color:=clBlue;
    end;
    

  * Open your application, it should look something like:



[![RadioButtonExample1.png](https://wiki.freepascal.org/images/9/92/RadioButtonExample1.png)](</File:RadioButtonExample1.png>) -> [![RadioButtonExample2.png](https://wiki.freepascal.org/images/1/12/RadioButtonExample2.png)](</File:RadioButtonExample2.png>)

### Using an OnChange event handler

The difference to the previous example is, we repaint the form not by a button click, but already by clicking one of the radio buttons themselves. 

You can modify the previous example, by deleting the button and its _OnClick_ event handler in the source code. You can create a new example but also easy: 

  * Create a new application and drop three TRadioButtons on the form.
  * In the Object Inspector tab properties change the name the _RadioButton1...3_ to _rbRed_ , _rbGreen_ and _rbBlue_.
  * Similarly, you change the captions of the radiobuttons to _Red_ , _Green_ and _Blue_ there.
  * Now you can create the _OnChange_ event handlers for the radiobuttons. For every radiobutton, you can use the Object Inspector tab events, select the _Onchange_ event and click the button [...], but you can also doubleclick on it.
  * Let the event handler _OnChange_ of the radio buttons change the colors of the form, according to clicked radio button:


    
    
    procedure TForm1.rbRedChange(Sender: TObject);
    begin
      Self.Color:=clRed;    //with "Self", you select the object in which the method exists (method: rbRedChange / object: Form1)
    end;
    
    procedure TForm1.rbGreenChange(Sender: TObject);
    begin
      Form1.Color:=clLime;  //You can directly select the object ''Form1'', but poor, 
                            //because then no other object of class 'TForm1' can be created
    end;
    
    procedure TForm1.rbBlueChange(Sender: TObject);
    begin
      Color:=clBlue;        //or you leave out "Self" and the compiler will automatically detect its own object
    end;
    

  * Open your application, it should look something like:



[![RadioButtonExample3.png](https://wiki.freepascal.org/images/b/bd/RadioButtonExample3.png)](</File:RadioButtonExample3.png>) -> [![RadioButtonExample4.png](https://wiki.freepascal.org/images/7/7c/RadioButtonExample4.png)](</File:RadioButtonExample4.png>)

## Grouping

If you add a radiobutton to your form it is added to a parent control (normally the [TForm](<TForm.md> "TForm") or a [TPanel](<TPanel.md> "TPanel")) which also determines their _group_ a TRadioButton belongs to. With each change of the state of one of the radio buttons in the group (no matter whether via code or user clicking on it) it is automatically checked whether a different radiobutton is already Checked and if yes, the property _Checked_ of this would be changed to _False_. 

If you need to have multiple selections by RadioButtons on your form, which are meant to provide different, independent choices, you must group the radio buttons by placing them into a common parent container component like a [TGroupBox](<TGroupBox.md> "TGroupBox") or a [TPanel](<TPanel.md> "TPanel") for example (there are many others like a [TPageControl](<TPageControl.md> "TPageControl"), [TNotebook](<TNotebook.md> "TNotebook"), [TScrollBox](<TScrollBox.md> "TScrollBox") that are capable of providing a container for child elements). There is also a [TRadioGroup](<TRadioGroup.md> "TRadioGroup") component which provides a combination of grouping and layouting several radio buttons together. 

### Example

The following example shows how you can group radio buttons: 

You can change the example [A simple example](<TRadioButton.md> "TRadioButton") or create a new application: 

  * As first you would need to place a [TGroupBox](<TGroupBox.md> "TGroupBox") of the standard component palette onto your form.
  * You change its name to _gbColor_ and its caption to _Color_.
  * Now you subclass this GroupBox that radio buttons _rbRed_ , _rbGreen_ and _rbBlue_ : 
    * In the modified project, you can sequentially move the radiobuttons **in the Object Inspector** by drag and drop to _gbColor_.
    * In a new project, you can insert the three radiobuttons one after the other, by clicking to insert in the GroupBox, then change the names to _rbRed_ , _rbGreen_ and _rbBlue_ and the captions to _Red_ , _Green_ and _Blue_.
  * Now place a second TGroupBox on your form named _gbBrightness_ with the caption _Brightness_.
  * Add this GroupBox also three radio buttons and give it the name _rbBrightDark_ , _rbBrightMedDark_ and _rbBrightBright_ and the captions _Dark_ , _MediumDark_ and _Bright_.
  * If you have created a new application, you must add a button with name _btnPaint_ and caption _Draw new_ to the form.
  * In the _OnClick_ event handler of _btnPaint_ change the code to:


    
    
    procedure TForm1.btnPaintClick(Sender: TObject);
    begin
      if rbRed.Checked   then Color:=Brightness or clRed;
      if rbGreen.Checked then Color:=Brightness or clLime;
      if rbBlue.Checked  then Color:=Brightness or clBlue;  
    end;
    

  * Now create even the function Brightness, by enter the _private_ section of TForm1, write **`function Brightness: TColor;`** and press the keys [CTRL] + [Shift] + [c] (code completion). The function is created. Enter there following code:


    
    
    function TForm1.Brightness: TColor;
    begin
      Result:=0;
      if rbBrightMedDark.Checked then Result:=$888888;
      if rbBrightBright.Checked  then Result:=$DDDDDD;
    end;
    

  * Start your application, you can use the grouped radio buttons separate, so it could look like:



[![RadioButtonExample5.png](https://wiki.freepascal.org/images/b/bf/RadioButtonExample5.png)](</File:RadioButtonExample5.png>) -> [![RadioButtonExample6.png](https://wiki.freepascal.org/images/8/87/RadioButtonExample6.png)](</File:RadioButtonExample6.png>)

## See also

  * [TRadioButton doc](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/tradiobutton.html> "doc:lcl/stdctrls/tradiobutton.html")
  * [![tradiogroup.png](https://wiki.freepascal.org/images/b/b8/tradiogroup.png)](</File:tradiogroup.png>)[TRadioGroup](<TRadioGroup.md> "TRadioGroup") \- Combined element providing some more layouting and grouping options
  * [![tcheckbox.png](https://wiki.freepascal.org/images/3/3c/tcheckbox.png)](</File:tcheckbox.png>)[TCheckBox](<TCheckBox.md> "TCheckBox") \- Alternative to radio buttons, representing an on/off state of a single option
  * [![tcheckgroup.png](https://wiki.freepascal.org/images/0/0b/tcheckgroup.png)](</File:tcheckgroup.png>)[TCheckGroup](<TCheckGroup.md> "TCheckGroup") \- Similar group as a TRadioGroup but with checkboxes as elements
  * [![tchecklistbox.png](https://wiki.freepascal.org/images/6/62/tchecklistbox.png)](</File:tchecklistbox.png>)[TCheckListBox](<TCheckListBox.md> "TCheckListBox") \- Alternative element to select several options in a list with checkmark boxes
  * [![tpanel.png](https://wiki.freepascal.org/images/7/7d/tpanel.png)](</File:tpanel.png>)[TPanel](<TPanel.md> "TPanel") \- General grouping element, usable to group TRadioButtons for example
  * [![tgroupbox.png](https://wiki.freepascal.org/images/1/18/tgroupbox.png)](</File:tgroupbox.png>)[TGroupBox](<TGroupBox.md> "TGroupBox") \- General grouping of elements with a titled box
  * [![tflowpanel.png](https://wiki.freepascal.org/images/3/3b/tflowpanel.png)](</File:tflowpanel.png>)[TFlowPanel](<TFlowPanel.md> "TFlowPanel") \- General grouping element which provides an automatic flow layouting ("line breaks") of its children
  * [![tcombobox.png](https://wiki.freepascal.org/images/b/be/tcombobox.png)](</File:tcombobox.png>)[TComboBox](<TComboBox.md> "TComboBox") \- Alternative element to select a single option from a drop-down list
  * [![ttogglebox.png](https://wiki.freepascal.org/images/5/5e/ttogglebox.png)](</File:ttogglebox.png>)[TToggleBox](<TToggleBox.md> "TToggleBox") \- Alternative button element providing a toggle option (pressed down, not pressed down)



  


[LCL Components](<LCL_Components.md> "LCL Components") Component Tab  | Components   
---|---  
[Standard](<Standard_tab.md> "Standard tab") | [TMainMenu](<TMainMenu.md> "TMainMenu") • [TPopupMenu](<TPopupMenu.md> "TPopupMenu") • [TButton](<TButton.md> "TButton") • [TLabel](<TLabel.md> "TLabel") • [TEdit](<TEdit.md> "TEdit") • [TMemo](<TMemo.md> "TMemo") • [TToggleBox](<TToggleBox.md> "TToggleBox") • [TCheckBox](<TCheckBox.md> "TCheckBox") • TRadioButton • [TListBox](<TListBox.md> "TListBox") • [TComboBox](<TComboBox.md> "TComboBox") • [TScrollBar](<TScrollBar.md> "TScrollBar") • [TGroupBox](<TGroupBox.md> "TGroupBox") • [TRadioGroup](<TRadioGroup.md> "TRadioGroup") • [TCheckGroup](<TCheckGroup.md> "TCheckGroup") • [TPanel](<TPanel.md> "TPanel") • [TFrame](<TFrame.md> "TFrame") • [TActionList](<TActionList.md> "TActionList")  
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

_Source: [https://wiki.freepascal.org/TRadioButton](https://web.archive.org/web/20250121220321/https://wiki.freepascal.org/TRadioButton)_
