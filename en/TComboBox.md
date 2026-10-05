# TComboBox

│ **English (en)** │

A **TComboBox** [![tcombobox.png](https://wiki.freepascal.org/images/b/be/tcombobox.png)](</File:tcombobox.png>) is a combination of an edit box and a (drop-down) list allowing one of several options to be chosen. 

## Contents

  * 1 Usage
    * 1.1 Fill ComboBox
      * 1.1.1 By the Object Inspector
      * 1.1.2 By code when you create the form
    * 1.2 Make that something happens after the selection
    * 1.3 Events
    * 1.4 Owner drawn ComboBox
      * 1.4.1 Draw a filled rectangle
      * 1.4.2 Preceded image
    * 1.5 Styles
      * 1.5.1 csSimple
  * 2 Proposed Features
    * 2.1 ReadOnly
    * 2.2 TextHint
  * 3 See also



# Usage

To use a [TComboBox](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/tcombobox.html> "doc:lcl/stdctrls/tcombobox.html") on a [form](<TForm.md> "TForm"), you can simply select it on the _[Standard tab](<Standard_tab.md> "Standard tab")_ of the [Component Palette](<Component_Palette.md> "Component Palette") and place it by clicking on the form. 

In the ComboBox, the stored [strings](<String.md> "String") are stored in the property _Items_ , that is of type TStrings. Thus you can assign or remove strings in the ComboBox, as in a [TStringList](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial") or its parent [TStrings](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial"). 

Here are a few examples to use a combobox _ComboBox1_ on a form _Form1_ : 

## Fill ComboBox

### By the Object Inspector

  * Select the ComboBox on your form with one click.
  * Go in the Object Inspector in the Properties tab on the property _Items_.
  * Click on the button with the three dots. The String Editor opens.
  * Enter your text and confirm your work with _OK_.



### By code when you create the form

  * Create the _OnCreate_ event handler for the form, by clicking on your form, use the Object Inspector, the tab events, select the _OnCreate_ event and click the button [...] or double click the button in the form.
  * In the source editor, you now insert the desired selection texts, for our example, you write as follows:


    
    
    procedure TForm1.FormCreate(Sender: TObject);  
    begin
      ComboBox1.Items.Clear;             //Delete all existing choices
      ComboBox1.Items.Add('Red');        //Add an choice
      ComboBox1.Items.Add('Green');
      ComboBox1.Items.Add('Blue');
      ComboBox1.Items.Add('Random Color');  
    end;
    

## Make that something happens after the selection

Like all components, even the TComboBox provides various [events](<Event_order.md> "Event order"), that are called when the user use the combobox. To respond to a change of the selection in the ComboBox, you can use the _OnChange_ event: 

  * Doubleclick the ComboBox on the form or choose the _OnChange_ event in the Object Inspector and click on the button [...].
  * The event handler is created, now you can insert your desired source, in our example we want to change the background color of the form:


    
    
    procedure TForm1.ComboBox1Change(Sender: TObject);
    begin
      case ComboBox1.ItemIndex of  //what entry (which item) has currently been chosen
        0: Color:=clRed;
        1: Color:=clGreen;
        2: Color:=clBlue;
        3: Color:=Random($1000000);
      end;
    end;
    

  * Start your application, the selection changes the background color of the form.



## Events

The _OnGetItems_ event is invoked when widgetset items list can be populated, so it is triggered when the button of the combo box is pressed. 

## Owner drawn ComboBox

In general, it is advantageous to let the ComboBox show in the [theme](<http://en.wikipedia.org/wiki/Skin_%28computing%29>) the user has chosen in his settings. In some cases (for example, to program a game with a colorful surface), you can deviate from this standard and draw it according to your own choice. This is how this works: 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** **Parameters of ComboBoxDrawItem:**  
  
**Control:**  
If multiple controls (e.g. multiple ComboBoxes) access this event handle, you know which control caused the event. In our example, instead of **`ComboBox1.Canvas.FillRect(ARect)`** you could also write **`TComboBox(Control).Canvas.FillRect(ARect)`**. However, you should still check in advance, whether it is a TComboBox: 
    
    
      if Control is TComboBox then
        TComboBox(Control).Canvas.FillRect(ARect);
    

**Index:** Specifies the item location, so you have access to the string **`<ComboBox>.Items[Index]`**.  
**ARect:** Describes the rectangle, which is necessary for drawing the background.  
**State:** Status of the items, whether normal, focused, selected etc. 

### Draw a filled rectangle

  * You can modify the example [Fill ComboBox by code when you create the form](<TComboBox.md> "TComboBox").
  * Change from _ComboBox1_ in the Object Inspector the property _Style_ to _csOwnerDrawFixed_.
  * Create in the Object Inspector the event handler for the event _OnDrawItem_ , by clicking on the button [...].
  * Add the following code to the handler:


    
    
    procedure TForm1.ComboBox1DrawItem(Control: TWinControl; Index: Integer;
      ARect: TRect; State: TOwnerDrawState);
    var
      ltRect: TRect;
    
      procedure FillColorfulRect(aCanvas: TCanvas; myRect: TRect);              //paint random color
      // Fills the rectangle with random colours
      var
        y: Integer;
      begin
        for y:=myRect.Top to myRect.Bottom - 1 do begin
          aCanvas.Pen.Color:=Random($1000000);
          aCanvas.Line(myRect.Left, y, myRect.Right, y);
        end;
      end;
    
    begin
      ComboBox1.Canvas.FillRect(ARect);                                         //first paint normal background
      ComboBox1.Canvas.TextRect(ARect, 22, ARect.Top, ComboBox1.Items[Index]);  //paint item text 
    
      ltRect.Left   := ARect.Left   + 2;                                        //rectangle for color
      ltRect.Right  := ARect.Left   + 20;
      ltRect.Top    := ARect.Top    + 1;
      ltRect.Bottom := ARect.Bottom - 1;
    
      ComboBox1.Canvas.Pen.Color:=clBlack;
      ComboBox1.Canvas.Rectangle(ltRect);                                       //draw a border 
    
      if InflateRect(ltRect, -1, -1) then                                       //resize rectangle by one pixel
        if Index = 3 then
          FillColorfulRect(ComboBox1.Canvas, ltRect)                            //paint random color
        else begin
          case Index of
            0: ComboBox1.Canvas.Brush.Color := clRed;
            1: ComboBox1.Canvas.Brush.Color := clGreen;
            2: ComboBox1.Canvas.Brush.Color := clBlue;
          end;
          ComboBox1.Canvas.FillRect(ltRect);                                    //paint colors according to selection
        end;
    end;
    

  * Your example might look like:



[![ComboBoxBsp1.png](https://wiki.freepascal.org/images/1/1d/ComboBoxBsp1.png)](</File:ComboBoxBsp1.png>) -> [![ComboBoxBsp2.png](https://wiki.freepascal.org/images/7/70/ComboBoxBsp2.png)](</File:ComboBoxBsp2.png>)

### Preceded image

In this example, we load a few images in a [TImageList](<TImageList.md> "TImageList") and draw them in front of the items in the combobox. It is a simple example which only generally show what you can do. I don't run explicitly details, such as checking, whether the corresponding image exists etc. in this example. This should be done by you depending on the need. 

  * Create an application analogous example [Fill ComboBox by code when you create the form](<TComboBox.md> "TComboBox").
  * Change from _ComboBox1_ in the Object Inspector the property _Style_ to _csOwnerDrawFixed_.
  * Add a [TImageList](<TImageList.md> "TImageList") from the component palette _Common controls_ on your form.
  * The _Height_ and _Width_ of 16 pixels is preset in _ImageList1_. We allow this. To fit neatly the images into our combo box, we make the property _ItemHeight_ from _ComboBox1_ to _18_ in the Object Inspector.
  * Add four images in the ImageList: 
    * Doubleclick _ImageList1_ or leftclick _ImageList1_ and select _ImageList Editor..._.
    * Click on _Add_ and select an image (see <Lazarus directory>/images/... there are various images or icons in 16x16px size).
    * Have you added four images, confirm your work with [OK].
  * Create in the Object Inspector the event handler for the event _OnDrawItem_ , by clicking on the button [...].
  * Add the following code to the handler:


    
    
    procedure TForm1.ComboBox1DrawItem(Control: TWinControl; Index: Integer;
      ARect: TRect; State: TOwnerDrawState);
    begin
      ComboBox1.Canvas.FillRect(ARect);                                         //first paint normal background
      ComboBox1.Canvas.TextRect(ARect, 20, ARect.Top, ComboBox1.Items[Index]);  //paint item text 
      ImageList1.Draw(ComboBox1.Canvas, ARect.Left + 1, ARect.Top + 1, Index);  //draw image according to index on canvas
    end;
    

  * Your example might look like:



[![ComboBoxBsp1.png](https://wiki.freepascal.org/images/1/1d/ComboBoxBsp1.png)](</File:ComboBoxBsp1.png>) -> [![ComboBoxBsp3.png](https://wiki.freepascal.org/images/c/ca/ComboBoxBsp3.png)](</File:ComboBoxBsp3.png>)

## Styles

### csSimple

**csSimple** represents itself a simple edit box with a list box underneath. The list box is only shown, if there's enough height allocated. By default the height is set to shown the edit box only. 

There's no dropdown icon to show the list box. Items can be changed by using keyboard (arrow keys), or by clicking an an item, if the control is high enough to show the items list. 

**csSimple** style is Windows specific. Non-widows widgetset most likely don't implement and fall back to **csDropDown**. 

  
Here's an example of (non-autosized) csSimple ComboBox on two different widgetset. (as of Lazarus 2.0.8): 

  * [![Win32](https://wiki.freepascal.org/images/a/a2/cssimple_win.png)](</File:cssimple_win.png> "Win32")

Win32 

  * [![Gtk2](https://wiki.freepascal.org/images/8/8c/cssimple_gtk2.png)](</File:cssimple_gtk2.png> "Gtk2")

Gtk2 




# Proposed Features

## ReadOnly

ReadOnly property has been deprecated since 2017. It has been removed as of today. 

However, it's possible to reinstate ReadOnly property as a mirror of TEdit readonly. 

The complexity of ComboBox is that it bears features of both ListBox and Edit (but it's not a descendant of either). While ComboBox does provides the means to change the selection (just like TEdit), it might be helpful also to make the edit box readonly. (Note, that this is different to csDropDownList. ccDropDownList typically implements a dropdown as a button). 

How, ReadOnly property is expected to work: 

  * it's available for any ComboBox style
  * it's does not influence or is not influenced by any ComboBox style selected or changed (unlike the previous implementation of ReadOnly)
  * if set to true, the edit box (if supported by the comboBox style on the widgetset) becomes ReadOnly (in terms of TEdit). The content cannot be changed by any text input actions (such as editing, pasting, or dragging the text around).



    

  * the combobox value CAN be changed by selecting a different item from the dropdown, by either using a mouse (or respective key combinations, i.e. up or down arrows)



## TextHint

As ComboBox bears traits of TEdit, it's should be possible to set TextHint for comboBox, if the value is empty. 

Introduce TextHint property that should act similar to TEdit 

  * if text value of combobox is empty, the TextHint should be presented (in the colors and font style, that's applicable to the current widgetset theme)



# See also

  * [TComboBox](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/tcombobox.html> "doc:lcl/stdctrls/tcombobox.html")
  * [TEdit](<TEdit.md> "TEdit")
  * [TListBox](<TListBox.md> "TListBox")
  * [TDBComboBox](<TDBComboBox.md> "TDBComboBox")
  * [TDBLookupComboBox](<TDBLookupComboBox.md> "TDBLookupComboBox")



  


[LCL Components](<LCL_Components.md> "LCL Components") Component Tab  | Components   
---|---  
[Standard](<Standard_tab.md> "Standard tab") | [TMainMenu](<TMainMenu.md> "TMainMenu") • [TPopupMenu](<TPopupMenu.md> "TPopupMenu") • [TButton](<TButton.md> "TButton") • [TLabel](<TLabel.md> "TLabel") • [TEdit](<TEdit.md> "TEdit") • [TMemo](<TMemo.md> "TMemo") • [TToggleBox](<TToggleBox.md> "TToggleBox") • [TCheckBox](<TCheckBox.md> "TCheckBox") • [TRadioButton](<TRadioButton.md> "TRadioButton") • [TListBox](<TListBox.md> "TListBox") • TComboBox • [TScrollBar](<TScrollBar.md> "TScrollBar") • [TGroupBox](<TGroupBox.md> "TGroupBox") • [TRadioGroup](<TRadioGroup.md> "TRadioGroup") • [TCheckGroup](<TCheckGroup.md> "TCheckGroup") • [TPanel](<TPanel.md> "TPanel") • [TFrame](<TFrame.md> "TFrame") • [TActionList](<TActionList.md> "TActionList")  
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

_Source: [https://wiki.freepascal.org/TComboBox](https://web.archive.org/web/20240907072649/https://wiki.freepascal.org/TComboBox)_
