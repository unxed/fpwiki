# TStringGrid

│ **English (en)** │  **[русский (ru)](<../ru/TStringGrid.md>)** │

**TStringGrid** [![tstringgrid.png](https://wiki.freepascal.org/images/c/c4/tstringgrid.png)](</File:tstringgrid.png>) is a component on the [Additional tab](<Additional_tab.md> "Additional tab") of the [Component Palette](<Component_Palette.md> "Component Palette"). A stringgrid provides a tabular display of textual information that may be edited as well. 

## Contents

  * 1 Example StringGrid Program
  * 2 Adjusting Columns & Changing Their Properties
  * 3 Using Available Predefined Properties
  * 4 Program Block To Add Data To The StringGrid
  * 5 CheckBox Column
  * 6 InsertRowWithValues Method
  * 7 Content based row background coloring
  * 8 See also



## Example StringGrid Program

To create this example create a new project in Lazarus. Select TStringGrid to add to the form, by clicking the TStringGrid component from the menu area or component window, then click on the form. In this case 2 [TButtons](<TButton.md> "TButton") were also selected and dropped onto the form as shown. In this example it is also necessary to select a [TOpenDialog](<TOpenDialog.md> "TOpenDialog") component and drop it onto the form. 

[![TStringGrid02.png](https://wiki.freepascal.org/images/7/7c/TStringGrid02.png)](</File:TStringGrid02.png>)

## Adjusting Columns & Changing Their Properties

Columns can be easily by added by right mouse clicking 'Columns: TGridColumns': 

[![TStringGrid09.png](https://wiki.freepascal.org/images/f/f0/TStringGrid09.png)](</File:TStringGrid09.png>)

By selecting AddItem, a new column is shown below. Under the Properties tab of the Object Inspector a new list of Properties and Events relating to that column are then shown. From here the Names of the columns are set along with the width. When finished, the TreeView looks like this: 

[![TStringGrid10.png](https://wiki.freepascal.org/images/3/39/TStringGrid10.png)](</File:TStringGrid10.png>)

In this example, Button1's name was changed to ButtonAddFiles and Button2's name was changed to ButtonExit. The StringGrid1 was stretched out and the buttons were aligned as shown. Note that there is a row and a column that are of a different color. That state illustrates the concept that this row and column could be for title labels for their respective column or row. Of course this default state can be changed simply by changing the 'FixedCols' or 'FixedRows' in the Object Inspector. 

[![TStringGrid03.png](https://wiki.freepascal.org/images/d/de/TStringGrid03.png)](</File:TStringGrid03.png>)

You can see in this case that the title lines have been changed and the StringGrid1 component has been anchored. This was achieved by taking two steps. The first one involves looking at the Object Inspector and selecting various properties as needed. One step that should be taken when starting out is to carefully note the default properties. When you have made numerous changes and need to go back, it is much easier if you know what their state was when you started or at various points along the way. The state of these properties in the last image illustrates only a single fixed row with column titles. This state illustrates: 
    
    
      FixedCols[0], 
      FixedRows[1], 
      HeaderHotZones[gzFixedCols], 
      HeaderPushZones[gzFixedCols], 
      Options[goFixedVertLine,goFixedHorzLine,goVertLine,goHorzLine,goRangeSelect,goSmoothScroll], 
      TitleFont[Color[clPurple]], 
      Style[fsBold], and 
      RowCount = 1. 
    

After viewing your work by clicking the Run button on Lazarus you may find it desirable to change these properties. In this case, the additional properties of ColClickSorts and AlternateColor were selected. 

The second thing that can be done is to use the Anchor Editor (View-->AnchorEditor) to link the sides of the StringGrid to the main form. 

  


## Using Available Predefined Properties

At the bottom of the Object Inspector you can find useful information about the properties shown as seen in this example: 

[![TStringGrid04.png](https://wiki.freepascal.org/images/e/ea/TStringGrid04.png)](</File:TStringGrid04.png>)

To add information to the StringGrid1 component, it is necessary to either add data from a TStream, LoadCVSFile, link the grid to a database or other similar actions. If linking to a database there are other components that should be considered like the TDBGrid. Other components such as the OpenDialog may also assist using methods like the LoadCVSFile. In many cases, it is necessary to either directly link data to given cells or ranges. In our example, we will use the InsertRowWithValues method. It is now necessary to add to the ButtonAddFiles an Event by clicking the Events tab of the Object Inspector and selecting the 'OnClick' event. 

[![TStringGrid05.png](https://wiki.freepascal.org/images/f/f0/TStringGrid05.png)](</File:TStringGrid05.png>)

## Program Block To Add Data To The StringGrid

By clicking on the OnClick the SourceEditor should have added a code block for the ButtonAddFilesClick procedure. To this you should add the following code: 
    
    
      uses
        Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
        Grids, LazFileUtils, LazUtf8; 
      . . . .
      types
          { TForm1 }
      TForm1 = class(TForm)
        ButtonAddFiles : TButton;
      . . . .
        procedure ButtonAddFilesClick(Sender : TObject);
        procedure ButtonExitClick(Sender : TObject); 
      . . . .
      var
        Form1 : TForm1;
      implementation
      {$R *.lfm}
      { TForm1 }  
      procedure TForm1.ButtonExitClick(Sender : TObject);
      begin
        Close;
      end;
      .
      procedure TForm1.ButtonAddFilesClick(Sender : TObject);
      var
        FilePathName : string;
      begin
        if OpenDialog1.Execute then
          FilePathName := OpenDialog1.Filename;
        AddFilesToList(FilePathName);
      end;
    

The procedure for closing the application is shown for completeness. In the ButtonAddFilesClick procedure we now use the OpenDialog1 and select the Execute method. If is shown in a if - then statement where the boolean property of execute is tested. This method by default is true so the following line then when executed gives the OpenDialog1 property 'FileName' to our variable 'FilePathName'. 

In the last line of procedure a new procedure is shown for 'AddFilesToList'. We now need to create this procedure. In the type declaration under either 'Public' or 'Private' we need to add this new procedure. Under implementation, the code block for the procedure is created. In this example we are going to use the files of a DVD as can be seen in this illustration: 

[![TStringGrid06.png](https://wiki.freepascal.org/images/d/dc/TStringGrid06.png)](</File:TStringGrid06.png>)

We want these files to be listed upon StringGrid1. 
    
    
      Type
      . . . .
      private
      procedure AddFilesToList(FilePathName : String);
      . . . .
       procedure TForm1.AddFilesToList(FilePathName : String);
       var
         D, R, K : integer;
         FileName, FilePath : string;
         SearchRec1, SearchRec2 : TSearchRec;
         FileListVideo, FileListVst : TStringList;
       begin
         FileListVideo := TStringList.Create;
         FileListVst := TStringList.Create;
         FileName := ExtractFileName(FilePathName);
         FilePath := ExtractFilePath(FilePathName);
         FileListVideo := FindAllFiles(FilePath,'VIDEO_TS.*',true, faDirectory);
         R := 1;
         K := 0;
         for D := 0 to FileListVideo.Count -1 do
         begin
           if FindFirstUtf8(FilePath, faAnyFile and faDirectory, SearchRec1)=0 then
           begin
             repeat
               With SearchRec1 do
               begin
                 FileName := ExtractFileName(FileListVideo.Strings[D]);
                 K := FileSizeUtf8(FileListVideo.Strings[D]);
                 StringGrid1.InsertRowWithValues(R,['0', FileName, IntToStr(K)]);
                 R := R + 1;
             end;
           until FindNextUtf8(SearchRec1) <> 0;
         end;
         FindCloseUtf8(SearchRec1);
       end;
       .
       FileListVst := FindAllFiles(FilePath, 'VTS_*.*', true, faDirectory);
       K := 0;
       for D := 0 to FileListVst.Count -1 do
       begin
         if FindFirstUtf8(FilePath, faAnyFile and faDirectory,SearchRec2)=0 then
         begin
           repeat
             With SearchRec2 do
             begin
               FileName := ExtractFileName(FileListVst.Strings[D]);
               K := FileSizeUtf8(FileListVst.Strings[D]);
               StringGrid1.InsertRowWithValues(R,['1', FileName, IntToStr(K)]);
               R := R + 1;
              end;
            until FindNextUtf8(SearchRec2) <> 0;
          end;
          FindCloseUtf8(SearchRec2);
        end;
        StringGrid1.SortColRow(true, 1,1,StringGrid1.RowCount-1);
        FileListVst.Free;
        FileListVideo.Free;
      end;
    

This example uses the methods 'FindAllFiles' from 'FileUtils' and 'FindFirstUtf8', 'FindNextUtf8' and 'FindCloseUtf8' from 'LazFileUtils'. 

## CheckBox Column

One feature that can be very helpful in applications of the TStringGrid is having a checkbox column that either a user can click to signify their selection or used to show a certain state for some property. Adding this type of column is illustrated in this code. In the method 'InsertRowWithValues', the first column has '0' shown in the first part of the code when files containing 'VIDEO_TS*' are being selected. The '0' is illustrating the boolean state of a checkbox with 1 being checked and 0 unchecked. In selecting from the Object Inspector under StringGrid1-->Columns: TGridColumns-->0-Select in the TreeView, the ValueChecked and ValueUnchecked are shown. You can use other numbers, or have added code to change the state. 

## InsertRowWithValues Method

In this example the method InsertRowWithValues is used to add data to our StringGrid1 component. Each column's data is entered followed by a comma. It may be necessary to use typecasting functions to get data into a string format. Variables can be referenced, or simply shown as text as our '0' and '1' are for checkbox column. 

[![TStringGrid08.png](https://wiki.freepascal.org/images/d/d3/TStringGrid08.png)](</File:TStringGrid08.png>)

Upon running our new application and clicking the button 'AddFiles' a dialog box opens allowing us to select file(s) to be added. If you click on a column header, StringGrid1 is sorted in the direction as shown by the green arrow. The result should be as shown in the following illustration: 

[![TStringGrid07.png](https://wiki.freepascal.org/images/e/e4/TStringGrid07.png)](</File:TStringGrid07.png>)

Depending on your needs other properties can be selected to allow editing, resizing columns, etc. 

## Content based row background coloring

With PrepareCanvas event, you can alter the background color of every row based on the row's text content. 

[![stringgridcoloring.png](https://wiki.freepascal.org/images/a/a2/stringgridcoloring.png)](</File:stringgridcoloring.png>)
    
    
    const
      csPASSED = 'Passed';
      csFAILED = 'Failed';
      csERROR  = 'Error';
    
    procedure TForm1.StatisticsGridPrepareCanvas(Sender: TObject; aCol,
      aRow: Integer; aState: TGridDrawState);
    begin
      if not(sender is TStringGrid) then Exit;
    
      if (sender as TStringGrid).Rows[aRow].Text.Contains(csERROR) then begin
         (sender as TStringGrid).Canvas.Brush.Color := clRed;
      end else if (sender as TStringGrid).Rows[aRow].Text.Contains(csFAILED) then begin
          (sender as TStringGrid).Canvas.Brush.Color := clYellow;
      end else if (sender as TStringGrid).Rows[aRow].Text.Contains(csPASSED) then begin
          (sender as TStringGrid).Canvas.Brush.Color := clGreen;
      end;
    end;
    

## See also

  * [TDrawGrid](<TDrawGrid.md> "TDrawGrid")
  * [TDBGrid](<TDBGrid.md> "TDBGrid")
  * [Grids Reference Page](<Grids_Reference_Page.md> "Grids Reference Page") (for more details on grids in Lazarus)



  


[LCL Components](<LCL_Components.md> "LCL Components") Component Tab  | Components   
---|---  
[Standard](<Standard_tab.md> "Standard tab") | [TMainMenu](<TMainMenu.md> "TMainMenu") • [TPopupMenu](<TPopupMenu.md> "TPopupMenu") • [TButton](<TButton.md> "TButton") • [TLabel](<TLabel.md> "TLabel") • [TEdit](<TEdit.md> "TEdit") • [TMemo](<TMemo.md> "TMemo") • [TToggleBox](<TToggleBox.md> "TToggleBox") • [TCheckBox](<TCheckBox.md> "TCheckBox") • [TRadioButton](<TRadioButton.md> "TRadioButton") • [TListBox](<TListBox.md> "TListBox") • [TComboBox](<TComboBox.md> "TComboBox") • [TScrollBar](<TScrollBar.md> "TScrollBar") • [TGroupBox](<TGroupBox.md> "TGroupBox") • [TRadioGroup](<TRadioGroup.md> "TRadioGroup") • [TCheckGroup](<TCheckGroup.md> "TCheckGroup") • [TPanel](<TPanel.md> "TPanel") • [TFrame](<TFrame.md> "TFrame") • [TActionList](<TActionList.md> "TActionList")  
[Additional](<Additional_tab.md> "Additional tab") | [TBitBtn](<TBitBtn.md> "TBitBtn") • [TSpeedButton](<TSpeedButton.md> "TSpeedButton") • [TStaticText](<TStaticText.md> "TStaticText") • [TImage](<TImage.md> "TImage") • [TShape](<TShape.md> "TShape") • [TBevel](<TBevel.md> "TBevel") • [TPaintBox](<TPaintBox.md> "TPaintBox") • [TNotebook](<TNotebook.md> "TNotebook") • [TLabeledEdit](<TLabeledEdit.md> "TLabeledEdit") • [TSplitter](<TSplitter.md> "TSplitter") • [TTrayIcon](<TTrayIcon.md> "TTrayIcon") • [TControlBar](<TControlBar.md> "TControlBar") • [TFlowPanel](<TFlowPanel.md> "TFlowPanel") • [TMaskEdit](<TMaskEdit.md> "TMaskEdit") • [TCheckListBox](<TCheckListBox.md> "TCheckListBox") • [TScrollBox](<TScrollBox.md> "TScrollBox") • [TApplicationProperties](<TApplicationProperties.md> "TApplicationProperties") • TStringGrid • [TDrawGrid](<TDrawGrid.md> "TDrawGrid") • [TPairSplitter](<TPairSplitter.md> "TPairSplitter") • [TColorBox](<TColorBox.md> "TColorBox") • [TColorListBox](<TColorListBox.md> "TColorListBox") • [TValueListEditor](<TValueListEditor.md> "TValueListEditor")  
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

_Source: [https://wiki.freepascal.org/TStringGrid](https://web.archive.org/web/20230610055816/https://wiki.freepascal.org/TStringGrid)_
