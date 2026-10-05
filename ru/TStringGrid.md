# TStringGrid

│ **[English (en)](<../en/TStringGrid.md> "TStringGrid")** │  **[français (fr)](</TStringGrid/fr> "TStringGrid/fr")** │  **[日本語 (ja)](</TStringGrid/ja> "TStringGrid/ja")** │  **русский (ru)** │    
****

  


* * *

\--[Zoltanleo](</User:Zoltanleo> "User:Zoltanleo") 00:38, 6 October 2018 (CEST) Ввиду сложности дословного перевода текста с английского на русский слова, требующиеся по смыслу, но отсутствующие в английской версии, указаны в квадратных скобках. 

* * *

  
**TStringGrid** [![tstringgrid.png](https://wiki.freepascal.org/images/c/c4/tstringgrid.png)](</File:tstringgrid.png>) это компонент с вкладки [Additional tab](<Additional_tab.md> "Additional tab/ru") палитры компонент [Component Palette](<Component_Palette.md> "Component Palette/ru"). Stringgrid обеспечивает табличное представление текстовой информации, которая может быть отредактирована. 

  


## Contents

  * 1 Пример программы StringGrid
  * 2 Настройка столбцов и изменение их свойств
  * 3 Использование доступных предопределенных свойств
  * 4 Программная блокировка добавления данных в StringGrid
  * 5 Столбцы с CheckBox
  * 6 Метод InsertRowWithValues
  * 7 См. также



## Пример программы StringGrid

Чтобы создать этот пример, создайте новый проект в Lazarus. Выберите TStringGrid, чтобы добавить на форму, щелкнув компонент TStringGrid из области меню или окна компонента, затем нажмите на форму. В этом случае, как показано [на рисунке], также были выбраны и брошены на форму две кнопки [TButton](<TButton.md> "TButton/ru"). В этом примере также необходимо выбрать компонент [TOpenDialog](<TOpenDialog.md> "TOpenDialog/ru") и поместить его в форму.  
  


[![TStringGrid02.png](https://wiki.freepascal.org/images/7/7c/TStringGrid02.png)](</File:TStringGrid02.png>)

## Настройка столбцов и изменение их свойств

Столбцы могут быть легко добавлены щелчком правой кнопки мыши 'Columns: TGridColumns':  
  
[![TStringGrid09.png](https://wiki.freepascal.org/images/f/f0/TStringGrid09.png)](</File:TStringGrid09.png>)  
  
При выборе AddItem, новый столбец отобразится ниже. На вкладке Properties [в окне] Object Inspector отображается новый список Properties[(свойств)] и Events[(событий)], относящихся к этому столбцу. Отсюда имена столбцов задаются вместе с шириной. Когда закончите [добавлять столбцы], TreeView будет выглядеть так:  
  
[![TStringGrid10.png](https://wiki.freepascal.org/images/3/39/TStringGrid10.png)](</File:TStringGrid10.png>)  
  
В этом примере имя Button1 было изменено на ButtonAddFiles, а имя Button2 было изменено на ButtonExit. StringGrid1 был растянут и кнопки были выровнены, как показано [на рисунке]. Обратите внимание, что есть строка и столбец другого цвета. Это состояние иллюстрирует концепцию, что эта строка и столбец могут быть [предназначена] для ярлыков заголовков соответствующего столбца или строки. Конечно, это состояние по умолчанию можно изменить, просто изменив 'FixedCols' или 'FixedRows' в Object Inspector.  
  
[![TStringGrid03.png](https://wiki.freepascal.org/images/d/de/TStringGrid03.png)](</File:TStringGrid03.png>)  
  
В этом случае вы можете увидеть, что строки заголовков изменены, а компонент StringGrid1 привязан [к краям формы]. Это было достигнуто путем выполнения двух шагов. _**Первый**_ включает в себя просмотр Object Inspector и выбор различных свойств по мере необходимости. Один шаг, который следует предпринять при запуске, - это внимательно посмотреть свойства по умолчанию. Когда вы внесли многочисленные изменения и вам нужно [отменить изменения] , [то сделать] это намного проще, если вы знаете, каково было их состояние в начале или на различных этапах [изменений]. Состояние этих свойств на последнем изображении иллюстрирует только одну фиксированную строку с заголовками столбцов. Это состояние иллюстрируется [следующим кодом]  
  

    
    
                     FixedCols[0], 
                     FixedRows[1], 
                     HeaderHotZones[gzFixedCols], 
                     HeaderPushZones[gzFixedCols], 
                     Options[goFixedVertLine,goFixedHorzLine,goVertLine,goHorzLine,goRangeSelect,goSmoothScroll], 
                     TitleFont[Color[clPurple]], 
                     Style[fsBold], and 
                     RowCount = 1. 
    

  
После просмотра [результатов] вашей работы путем нажатия кнопки Run в Lazarus, возможно вы пожелаете изменить эти свойства. В этом случае были выбраны дополнительные свойства ColClickSorts и AlternateColor. 

_**Второе**_ , что можно сделать, это использовать Редактор Привязок (View -> AnchorEditor), чтобы привязать стороны StringGrid к основной формой. 

## Использование доступных предопределенных свойств

В нижней части Object Inspector вы можете найти полезную информацию о свойствах, показанных в этом примере:  


[![TStringGrid04.png](https://wiki.freepascal.org/images/e/ea/TStringGrid04.png)](</File:TStringGrid04.png>)  
  


Чтобы внести информацию в компонент StringGrid1, необходимо либо добавить данные из TStream/LoadCVSFile, либо связать сетку с базой данных, либо [совершить] другие подобные действия. Если привязываться к базе данных, [для этих целей лучше использовать] компоненты, как TDBGrid. Другие компоненты, такие как OpenDialog, также могут помочь в использовании таких методов, как LoadCVSFile. Во многих случаях необходимо напрямую связывать данные либо с указанными ячейками, либо с диапазонами. В нашем примере мы будем использовать метод InsertRowWithValues. Для этого необходимо добавить к ButtonAddFiles событие [нажатия на кнопку], щелкнув вкладку 'Events' Object Inspector'а и выбрав событие 'OnClick'.  


[![TStringGrid05.png](https://wiki.freepascal.org/images/f/f0/TStringGrid05.png)](</File:TStringGrid05.png>)

## Программная блокировка добавления данных в StringGrid

При создании обработчика события _OnClick_ для кнопки _ButtonAddFilesClick_ редактор кода SourceEditor должен создать блок кода для процедуры _ButtonAddFilesClick_. К этому вы должны добавить следующий код: 
    
    
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
      ...
      procedure TForm1.ButtonAddFilesClick(Sender : TObject);
      var
        FilePathName : string;
      begin
        if OpenDialog1.Execute then
          FilePathName := OpenDialog1.Filename;
        AddFilesToList(FilePathName);
      end;
    

Процедура закрытия приложения показана для полноты. В процедуре _ButtonAddFilesClick_ теперь мы используем _OpenDialog1_ и выбираем метод _Execute_. Как показано в выражении _if-then_ , там проверяется логическое свойство execute. Этот метод по умолчанию имеет значение true, поэтому следующая строка при выполнении дает свойство _OpenDialog1_ 'FileName' нашей переменной 'FilePathName'. 

В последней строке кода показана новая процедура _AddFilesToList_. Теперь нам нужно создать эту процедуру. В объявлении типов в разделе _Public_ или _Private_ нам нужно добавить эту новую процедуру. В процессе реализации [(по умолчанию <Shift>+<Ctrl>+<C>)] создается кодовый блок для процедуры. В этом примере мы будем использовать файлы DVD, как показано на этой иллюстрации:  
  


[![TStringGrid06.png](https://wiki.freepascal.org/images/d/dc/TStringGrid06.png)](</File:TStringGrid06.png>)  


Мы хотим, чтобы эти файлы были перечислены в StringGrid1. 
    
    
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
    

В этом примере используются методы 'FindAllFiles' из 'FileUtils' и 'FindFirstUtf8', 'FindNextUtf8' и 'FindCloseUtf8' из 'LazFileUtils'. 

## Столбцы с CheckBox

Одна из функций, которая может быть очень полезна в приложениях TStringGrid, имеет столбец с checkbox, по которому пользователь либо может щелкнуть, чтобы обозначить его выбор, либо использовать для отображения определенного состояния для некоторого свойства. Добавление этого типа столбца показано в этом коде. В методе _InsertRowWithValues_ первый столбец имеет '0', показанный в первой части кода, когда выбираются файлы, содержащие 'VIDEO_TS*'. '0' иллюстрирует логическое состояние checkbox'а, как отмеченный (1) и не отмеченный (0). При выборе в Object Inspector'е StringGrid1 -> Columns: TGridColumns -> 0-Select в TreeView отображаются значения ValueChecked и ValueUnchecked. Вы можете использовать другие числа или добавить код для изменения состояния. 

## Метод InsertRowWithValues

В этом примере метод InsertRowWithValues используется для добавления данных в наш компонент StringGrid1. Данные каждого столбца вводятся с запятой. Может потребоваться использование функций приведения типов для получения данных в строковом формате. Переменные могут быть ссылками или просто отображаться как текст, так как наши '0' и '1' относятся к столбцу checkbox'а.  
  


[![TStringGrid08.png](https://wiki.freepascal.org/images/d/d3/TStringGrid08.png)](</File:TStringGrid08.png>)

После запуска нашего нового приложения и нажатия кнопки 'AddFiles' открывается диалоговое окно, позволяющее нам выбрать файл(ы), который нужно добавить. Если вы нажмете на заголовок столбца, StringGrid1 будет отсортирован в направлении, указанном зеленой стрелкой. Результат должен выглядеть так, как показано на следующем рисунке:  
  


[![TStringGrid07.png](https://wiki.freepascal.org/images/e/e4/TStringGrid07.png)](</File:TStringGrid07.png>)

В зависимости от ваших потребностей могут быть выбраны другие свойства, позволяющие редактировать, изменять размеры столбцов и т.д. 

## См. также

  * [TDrawGrid](<TDrawGrid.md> "TDrawGrid/ru")
  * [TDBGrid](<TDBGrid.md> "TDBGrid/ru")
  * [Grids Reference Page](<Grids_Reference_Page.md> "Grids Reference Page/ru")



  


[Компоненты LCL](<LCL_Components.md> "LCL Components/ru") Вкладка  | Компоненты   
---|---  
[Standard](<Standard_tab.md> "Standard tab/ru") | [TMainMenu](<TMainMenu.md> "TMainMenu/ru") • [TPopupMenu](<TPopupMenu.md> "TPopupMenu/ru") • [TButton](<TButton.md> "TButton/ru") • [TLabel](<TLabel.md> "TLabel/ru") • [TEdit](<TEdit.md> "TEdit/ru") • [TMemo](<TMemo.md> "TMemo/ru") • [TToggleBox](<TToggleBox.md> "TToggleBox/ru") • [TCheckBox](<TCheckBox.md> "TCheckBox/ru") • [TRadioButton](</index.php?title=TRadioButton/ru&action=edit&redlink=1> "TRadioButton/ru \(page does not exist\)") • [TListBox](</index.php?title=TListBox/ru&action=edit&redlink=1> "TListBox/ru \(page does not exist\)") • [TComboBox](</index.php?title=TComboBox/ru&action=edit&redlink=1> "TComboBox/ru \(page does not exist\)") • [TScrollBar](</index.php?title=TScrollBar/ru&action=edit&redlink=1> "TScrollBar/ru \(page does not exist\)") • [TGroupBox](<TGroupBox.md> "TGroupBox/ru") • [TRadioGroup](<TRadioGroup.md> "TRadioGroup/ru") • [TCheckGroup](<TCheckGroup.md> "TCheckGroup/ru") • [TPanel](<TPanel.md> "TPanel/ru") • [TFrame](<TFrame.md> "TFrame/ru") • [TActionList](<TActionList.md> "TActionList/ru")  
[Additional](<Additional_tab.md> "Additional tab/ru") | [TBitBtn](<TBitBtn.md> "TBitBtn/ru") • [TSpeedButton](<TSpeedButton.md> "TSpeedButton/ru") • [TStaticText](<TStaticText.md> "TStaticText/ru") • [TImage](<TImage.md> "TImage/ru") • [TShape](<TShape.md> "TShape/ru") • [TBevel](<TBevel.md> "TBevel/ru") • [TPaintBox](<TPaintBox.md> "TPaintBox/ru") • [TNotebook](<TNotebook.md> "TNotebook/ru") • [TLabeledEdit](<TLabeledEdit.md> "TLabeledEdit/ru") • [TSplitter](<TSplitter.md> "TSplitter/ru") • [TTrayIcon](<TTrayIcon.md> "TTrayIcon/ru") • [TControlBar](<TControlBar.md> "TControlBar/ru") • [TFlowPanel](<TFlowPanel.md> "TFlowPanel/ru") • [TMaskEdit](<TMaskEdit.md> "TMaskEdit/ru") • [TCheckListBox](<TCheckListBox.md> "TCheckListBox/ru") • [TScrollBox](<TScrollBox.md> "TScrollBox/ru") • [TApplicationProperties](<TApplicationProperties.md> "TApplicationProperties/ru") • TStringGrid • [TDrawGrid](<TDrawGrid.md> "TDrawGrid/ru") • [TPairSplitter](<TPairSplitter.md> "TPairSplitter/ru") • [TColorBox](<TColorBox.md> "TColorBox/ru") • [TColorListBox](<TColorListBox.md> "TColorListBox/ru") • [TValueListEditor](<TValueListEditor.md> "TValueListEditor/ru")  
[Common Controls](<Common_Controls_tab.md> "Common Controls tab/ru") | [TTrackBar](<TTrackBar.md> "TTrackBar/ru") • [TProgressBar](<TProgressBar.md> "TProgressBar/ru") • [TTreeView](<TTreeView.md> "TTreeView/ru") • [TListView](<TListView.md> "TListView/ru") • [TStatusBar](<TStatusBar.md> "TStatusBar/ru") • [TToolBar](<TToolBar.md> "TToolBar/ru") • [TCoolBar](<TCoolBar.md> "TCoolBar/ru") • [TUpDown](<TUpDown.md> "TUpDown/ru") • [TPageControl](<TPageControl.md> "TPageControl/ru") • [TTabControl](<TTabControl.md> "TTabControl/ru") • [THeaderControl](<THeaderControl.md> "THeaderControl/ru") • [TImageList](<TImageList.md> "TImageList/ru") • [TPopupNotifier](</index.php?title=TPopupNotifier/ru&action=edit&redlink=1> "TPopupNotifier/ru \(page does not exist\)") • [TDateTimePicker](<TDateTimePicker.md> "TDateTimePicker/ru")  
[Dialogs](<Dialogs_tab.md> "Dialogs tab/ru") | [TOpenDialog](<TOpenDialog.md> "TOpenDialog/ru") • [TSaveDialog](<TSaveDialog.md> "TSaveDialog/ru") • [TSelectDirectoryDialog](<TSelectDirectoryDialog.md> "TSelectDirectoryDialog/ru") • [TColorDialog](<TColorDialog.md> "TColorDialog/ru") • [TFontDialog](<TFontDialog.md> "TFontDialog/ru") • [TFindDialog](<TFindDialog.md> "TFindDialog/ru") • [TReplaceDialog](<TReplaceDialog.md> "TReplaceDialog/ru") • [TOpenPictureDialog](<TOpenPictureDialog.md> "TOpenPictureDialog/ru") • [TSavePictureDialog](<TSavePictureDialog.md> "TSavePictureDialog/ru") • [TCalendarDialog](<TCalendarDialog.md> "TCalendarDialog/ru") • [TCalculatorDialog](<TCalculatorDialog.md> "TCalculatorDialog/ru") • [TPrinterSetupDialog](<TPrinterSetupDialog.md> "TPrinterSetupDialog/ru") • [TPrintDialog](<TPrintDialog.md> "TPrintDialog/ru") • [TPageSetupDialog](<TPageSetupDialog.md> "TPageSetupDialog/ru") • [TTaskDialog](<TTaskDialog.md> "TTaskDialog/ru")  
[Data Controls](<Data_Controls_tab.md> "Data Controls tab/ru") | [TDBNavigator](<TDBNavigator.md> "TDBNavigator/ru") • [TDBText](<TDBText.md> "TDBText/ru") • [TDBEdit](<TDBEdit.md> "TDBEdit/ru") • [TDBMemo](<TDBMemo.md> "TDBMemo/ru") • [TDBImage](<TDBImage.md> "TDBImage/ru") • [TDBListBox](<TDBListBox.md> "TDBListBox/ru") • [TDBLookupListBox](<TDBLookupListBox.md> "TDBLookupListBox/ru") • [TDBComboBox](<TDBComboBox.md> "TDBComboBox/ru") • [TDBLookupComboBox](</index.php?title=TDBLookupComboBox/ru&action=edit&redlink=1> "TDBLookupComboBox/ru \(page does not exist\)") • [TDBCheckBox](</index.php?title=TDBCheckBox/ru&action=edit&redlink=1> "TDBCheckBox/ru \(page does not exist\)") • [TDBRadioGroup](</index.php?title=TDBRadioGroup/ru&action=edit&redlink=1> "TDBRadioGroup/ru \(page does not exist\)") • [TDBCalendar](</index.php?title=TDBCalendar/ru&action=edit&redlink=1> "TDBCalendar/ru \(page does not exist\)") • [TDBGroupBox](</index.php?title=TDBGroupBox/ru&action=edit&redlink=1> "TDBGroupBox/ru \(page does not exist\)") • [TDBGrid](<TDBGrid.md> "TDBGrid/ru") • [TDBDateTimePicker](<TDBDateTimePicker.md> "TDBDateTimePicker/ru")  
[Data Access](<Data_Access_tab.md> "Data Access tab/ru") | [TDataSource](<TDataSource.md> "TDataSource/ru") • [TBufDataset](</index.php?title=TBufDataset/ru&action=edit&redlink=1> "TBufDataset/ru \(page does not exist\)") • [TMemDataset](</index.php?title=TMemDataset/ru&action=edit&redlink=1> "TMemDataset/ru \(page does not exist\)") • [TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet/ru") • [TFixedFormatDataSet](<TFixedFormatDataSet.md> "TFixedFormatDataSet/ru") • [TDbf](<TDbf.md> "TDbf/ru")  
[System](<System_tab.md> "System tab/ru") | [TTimer](<TTimer.md> "TTimer/ru") • [TIdleTimer](<TIdleTimer.md> "TIdleTimer/ru") • [TLazComponentQueue](<TLazComponentQueue.md> "TLazComponentQueue/ru") • [THTMLHelpDatabase](<THTMLHelpDatabase.md> "THTMLHelpDatabase/ru") • [THTMLBrowserHelpViewer](<THTMLBrowserHelpViewer.md> "THTMLBrowserHelpViewer/ru") • [TAsyncProcess](<TAsyncProcess.md> "TAsyncProcess/ru") • [TProcessUTF8](<TProcessUTF8.md> "TProcessUTF8/ru") • [TProcess](</index.php?title=TProcess/ru&action=edit&redlink=1> "TProcess/ru \(page does not exist\)") • [TSimpleIPCClient](<TSimpleIPCClient.md> "TSimpleIPCClient/ru") • [TSimpleIPCServer](<TSimpleIPCServer.md> "TSimpleIPCServer/ru") • [TXMLConfig](</index.php?title=TXMLConfig/ru&action=edit&redlink=1> "TXMLConfig/ru \(page does not exist\)") • [TEventLog](<TEventLog.md> "TEventLog/ru") • [TServiceManager](<TServiceManager.md> "TServiceManager/ru") • [TCHMHelpDatabase](<TCHMHelpDatabase.md> "TCHMHelpDatabase/ru") • [TLHelpConnector](<TLHelpConnector.md> "TLHelpConnector/ru")  
[Misc](<Misc_tab.md> "Misc tab/ru") | [TColorButton](<TColorButton.md> "TColorButton/ru") • [TSpinEdit](<TSpinEdit.md> "TSpinEdit/ru") • [TFloatSpinEdit](<TFloatSpinEdit.md> "TFloatSpinEdit/ru") • [TArrow](<TArrow.md> "TArrow/ru") • [TCalendar](<TCalendar.md> "TCalendar/ru") • [TEditButton](</index.php?title=TEditButton/ru&action=edit&redlink=1> "TEditButton/ru \(page does not exist\)") • [TFileNameEdit](</index.php?title=TFileNameEdit/ru&action=edit&redlink=1> "TFileNameEdit/ru \(page does not exist\)") • [TDirectoryEdit](</index.php?title=TDirectoryEdit/ru&action=edit&redlink=1> "TDirectoryEdit/ru \(page does not exist\)") • [TDateEdit](<TDateEdit.md> "TDateEdit/ru") • [TTimeEdit](<TTimeEdit.md> "TTimeEdit/ru") • [TCalcEdit](</index.php?title=TCalcEdit/ru&action=edit&redlink=1> "TCalcEdit/ru \(page does not exist\)") • [TFileListBox](</index.php?title=TFileListBox/ru&action=edit&redlink=1> "TFileListBox/ru \(page does not exist\)") • [TFilterComboBox](</index.php?title=TFilterComboBox/ru&action=edit&redlink=1> "TFilterComboBox/ru \(page does not exist\)") • [TComboBoxEx](</index.php?title=TComboBoxEx/ru&action=edit&redlink=1> "TComboBoxEx/ru \(page does not exist\)") • [TCheckComboBox](</index.php?title=TCheckComboBox/ru&action=edit&redlink=1> "TCheckComboBox/ru \(page does not exist\)") • [TButtonPanel](<TButtonPanel.md> "TButtonPanel/ru") • [TShellTreeView](</index.php?title=TShellTreeView/ru&action=edit&redlink=1> "TShellTreeView/ru \(page does not exist\)") • [TShellListView](</index.php?title=TShellListView/ru&action=edit&redlink=1> "TShellListView/ru \(page does not exist\)") • [TXMLPropStorage](<TXMLPropStorage.md> "TXMLPropStorage/ru") • [TINIPropStorage](<TINIPropStorage.md> "TINIPropStorage/ru") • [TIDEDialogLayoutStorage](</index.php?title=TIDEDialogLayoutStorage/ru&action=edit&redlink=1> "TIDEDialogLayoutStorage/ru \(page does not exist\)") • [TMRUManager](</index.php?title=TMRUManager/ru&action=edit&redlink=1> "TMRUManager/ru \(page does not exist\)") • [TStrHolder](</index.php?title=TStrHolder/ru&action=edit&redlink=1> "TStrHolder/ru \(page does not exist\)")  
[LazControls](<LazControls_tab.md> "LazControls tab/ru") | [TCheckBoxThemed](</index.php?title=TCheckBoxThemed/ru&action=edit&redlink=1> "TCheckBoxThemed/ru \(page does not exist\)") • [TDividerBevel](<TDividerBevel.md> "TDividerBevel/ru") • [TExtendedNotebook](</index.php?title=TExtendedNotebook/ru&action=edit&redlink=1> "TExtendedNotebook/ru \(page does not exist\)") • [TListFilterEdit](</index.php?title=TListFilterEdit/ru&action=edit&redlink=1> "TListFilterEdit/ru \(page does not exist\)") • [TListViewFilterEdit](</index.php?title=TListViewFilterEdit/ru&action=edit&redlink=1> "TListViewFilterEdit/ru \(page does not exist\)") • [TLvlGraphControl](</index.php?title=TLvlGraphControl/ru&action=edit&redlink=1> "TLvlGraphControl/ru \(page does not exist\)") • [TShortPathEdit](</index.php?title=TShortPathEdit/ru&action=edit&redlink=1> "TShortPathEdit/ru \(page does not exist\)") • [TSpinEditEx](</index.php?title=TSpinEditEx/ru&action=edit&redlink=1> "TSpinEditEx/ru \(page does not exist\)") • [TFloatSpinEditEx](<TFloatSpinEditEx.md> "TFloatSpinEditEx/ru") • [TTreeFilterEdit](</index.php?title=TTreeFilterEdit/ru&action=edit&redlink=1> "TTreeFilterEdit/ru \(page does not exist\)") • [TExtendedTabControl](</index.php?title=TExtendedTabControl/ru&action=edit&redlink=1> "TExtendedTabControl/ru \(page does not exist\)") •   
[RTTI](<RTTI_tab.md> "RTTI tab/ru") | [TTIEdit](</index.php?title=TTIEdit/ru&action=edit&redlink=1> "TTIEdit/ru \(page does not exist\)") • [TTIComboBox](</index.php?title=TTIComboBox/ru&action=edit&redlink=1> "TTIComboBox/ru \(page does not exist\)") • [TTIButton](</index.php?title=TTIButton/ru&action=edit&redlink=1> "TTIButton/ru \(page does not exist\)") • [TTICheckBox](</index.php?title=TTICheckBox/ru&action=edit&redlink=1> "TTICheckBox/ru \(page does not exist\)") • [TTILabel](</index.php?title=TTILabel/ru&action=edit&redlink=1> "TTILabel/ru \(page does not exist\)") • [TTIGroupBox](</index.php?title=TTIGroupBox/ru&action=edit&redlink=1> "TTIGroupBox/ru \(page does not exist\)") • [TTIRadioGroup](</index.php?title=TTIRadioGroup/ru&action=edit&redlink=1> "TTIRadioGroup/ru \(page does not exist\)") • [TTICheckGroup](</index.php?title=TTICheckGroup/ru&action=edit&redlink=1> "TTICheckGroup/ru \(page does not exist\)") • [TTICheckListBox](</index.php?title=TTICheckListBox/ru&action=edit&redlink=1> "TTICheckListBox/ru \(page does not exist\)") • [TTIListBox](</index.php?title=TTIListBox/ru&action=edit&redlink=1> "TTIListBox/ru \(page does not exist\)") • [TTIMemo](</index.php?title=TTIMemo/ru&action=edit&redlink=1> "TTIMemo/ru \(page does not exist\)") • [TTICalendar](</index.php?title=TTICalendar/ru&action=edit&redlink=1> "TTICalendar/ru \(page does not exist\)") • [TTIImage](</index.php?title=TTIImage/ru&action=edit&redlink=1> "TTIImage/ru \(page does not exist\)") • [TTIFloatSpinEdit](</index.php?title=TTIFloatSpinEdit/ru&action=edit&redlink=1> "TTIFloatSpinEdit/ru \(page does not exist\)") • [TTISpinEdit](</index.php?title=TTISpinEdit/ru&action=edit&redlink=1> "TTISpinEdit/ru \(page does not exist\)") • [TTITrackBar](</index.php?title=TTITrackBar/ru&action=edit&redlink=1> "TTITrackBar/ru \(page does not exist\)") • [TTIProgressBar](</index.php?title=TTIProgressBar/ru&action=edit&redlink=1> "TTIProgressBar/ru \(page does not exist\)") • [TTIMaskEdit](</index.php?title=TTIMaskEdit/ru&action=edit&redlink=1> "TTIMaskEdit/ru \(page does not exist\)") • [TTIColorButton](</index.php?title=TTIColorButton/ru&action=edit&redlink=1> "TTIColorButton/ru \(page does not exist\)") • [TMultiPropertyLink](</index.php?title=TMultiPropertyLink/ru&action=edit&redlink=1> "TMultiPropertyLink/ru \(page does not exist\)") • [TTIPropertyGrid](</index.php?title=TTIPropertyGrid/ru&action=edit&redlink=1> "TTIPropertyGrid/ru \(page does not exist\)") • [TTIGrid](</index.php?title=TTIGrid/ru&action=edit&redlink=1> "TTIGrid/ru \(page does not exist\)")  
[SQLdb](<SQLdb_tab.md> "SQLdb tab/ru") | [TSQLQuery](<TSQLQuery.md> "TSQLQuery/ru") • [TSQLTransaction](<TSQLTransaction.md> "TSQLTransaction/ru") • [TSQLScript](</index.php?title=TSQLScript/ru&action=edit&redlink=1> "TSQLScript/ru \(page does not exist\)") • [TSQLConnector](<TSQLConnector.md> "TSQLConnector/ru") • [TMSSQLConnection](<TMSSQLConnection.md> "TMSSQLConnection/ru") • [TSybaseConnection](<TSybaseConnection.md> "TSybaseConnection/ru") • [TPQConnection](<TPQConnection.md> "TPQConnection/ru") • [TPQTEventMonitor](</index.php?title=TPQTEventMonitor/ru&action=edit&redlink=1> "TPQTEventMonitor/ru \(page does not exist\)") • [TOracleConnection](<TOracleConnection.md> "TOracleConnection/ru") • [TODBCConnection](<TODBCConnection.md> "TODBCConnection/ru") • [TMySQL40Connection](<TMySQL40Connection.md> "TMySQL40Connection/ru") • [TMySQL41Connection](<TMySQL41Connection.md> "TMySQL41Connection/ru") • [TMySQL50Connection](<TMySQL50Connection.md> "TMySQL50Connection/ru") • [TMySQL51Connection](<TMySQL51Connection.md> "TMySQL51Connection/ru") • [TMySQL55Connection](<TMySQL55Connection.md> "TMySQL55Connection/ru") • [TMySQL56Connection](<TMySQL56Connection.md> "TMySQL56Connection/ru") • [TSQLite3Connection](</index.php?title=TSQLite3Connection/ru&action=edit&redlink=1> "TSQLite3Connection/ru \(page does not exist\)") • [TIBConnection](<TIBConnection.md> "TIBConnection/ru") • [TFBAdmin](</index.php?title=TFBAdmin/ru&action=edit&redlink=1> "TFBAdmin/ru \(page does not exist\)") • [TFBEventMonitor](<TFBEventMonitor.md> "TFBEventMonitor/ru") • [TSQLDBLibraryLoader](</index.php?title=TSQLDBLibraryLoader/ru&action=edit&redlink=1> "TSQLDBLibraryLoader/ru \(page does not exist\)")  
[Pascal Script](<Pascal_Script_tab.md> "Pascal Script tab/ru") | [TPSScript](</index.php?title=TPSScript/ru&action=edit&redlink=1> "TPSScript/ru \(page does not exist\)") • [TPSScriptDebugger](</index.php?title=TPSScriptDebugger/ru&action=edit&redlink=1> "TPSScriptDebugger/ru \(page does not exist\)") • [TPSDllPlugin](</index.php?title=TPSDllPlugin/ru&action=edit&redlink=1> "TPSDllPlugin/ru \(page does not exist\)") • [TPSImport_Classes](</index.php?title=TPSImport_Classes/ru&action=edit&redlink=1> "TPSImport Classes/ru \(page does not exist\)") • [TPSImport_DateUtils](</index.php?title=TPSImport_DateUtils/ru&action=edit&redlink=1> "TPSImport DateUtils/ru \(page does not exist\)") • [TPSImport_ComObj](</index.php?title=TPSImport_ComObj/ru&action=edit&redlink=1> "TPSImport ComObj/ru \(page does not exist\)") • [TPSImport_DB](</index.php?title=TPSImport_DB/ru&action=edit&redlink=1> "TPSImport DB/ru \(page does not exist\)") • [TPSImport_Forms](</index.php?title=TPSImport_Forms/ru&action=edit&redlink=1> "TPSImport Forms/ru \(page does not exist\)") • [TPSImport_Controls](</index.php?title=TPSImport_Controls/ru&action=edit&redlink=1> "TPSImport Controls/ru \(page does not exist\)") • [TPSImport_StdCtrls](</index.php?title=TPSImport_StdCtrls/ru&action=edit&redlink=1> "TPSImport StdCtrls/ru \(page does not exist\)") • [TPSCustomPlugin](</index.php?title=TPSCustomPlugin/ru&action=edit&redlink=1> "TPSCustomPlugin/ru \(page does not exist\)")  
[SynEdit](<SynEdit_tab.md> "SynEdit tab/ru") | [TSynEdit](<TSynEdit.md> "TSynEdit/ru") • [TSynCompletion](</index.php?title=TSynCompletion/ru&action=edit&redlink=1> "TSynCompletion/ru \(page does not exist\)") • [TSynAutoComplete](</index.php?title=TSynAutoComplete/ru&action=edit&redlink=1> "TSynAutoComplete/ru \(page does not exist\)") • [TSynMacroRecorder](</index.php?title=TSynMacroRecorder/ru&action=edit&redlink=1> "TSynMacroRecorder/ru \(page does not exist\)") • [TSynExporterHTML](</index.php?title=TSynExporterHTML/ru&action=edit&redlink=1> "TSynExporterHTML/ru \(page does not exist\)") • [TSynPluginSyncroEdit](</index.php?title=TSynPluginSyncroEdit/ru&action=edit&redlink=1> "TSynPluginSyncroEdit/ru \(page does not exist\)") • [TSynPasSyn](<TSynPasSyn.md> "TSynPasSyn/ru") • [TSynFreePascalSyn](<TSynFreePascalSyn.md> "TSynFreePascalSyn/ru") • [TSynCppSyn](<TSynCppSyn.md> "TSynCppSyn/ru") • [TSynJavaSyn](<TSynJavaSyn.md> "TSynJavaSyn/ru") • [TSynPerlSyn](<TSynPerlSyn.md> "TSynPerlSyn/ru") • [TSynHTMLSyn](<TSynHTMLSyn.md> "TSynHTMLSyn/ru") • [TSynXMLSyn](<TSynXMLSyn.md> "TSynXMLSyn/ru") • [TSynLFMSyn](</index.php?title=TSynLFMSyn/ru&action=edit&redlink=1> "TSynLFMSyn/ru \(page does not exist\)") • [TSynDiffSyn](</index.php?title=TSynDiffSyn/ru&action=edit&redlink=1> "TSynDiffSyn/ru \(page does not exist\)") • [TSynUNIXShellScriptSyn](<TSynUNIXShellScriptSyn.md> "TSynUNIXShellScriptSyn/ru") • [TSynCssSyn](<TSynCssSyn.md> "TSynCssSyn/ru") • [TSynPHPSyn](<TSynPHPSyn.md> "TSynPHPSyn/ru") • [TSynTeXSyn](<TSynTeXSyn.md> "TSynTeXSyn/ru") • [TSynSQLSyn](<TSynSQLSyn.md> "TSynSQLSyn/ru") • [TSynPythonSyn](<TSynPythonSyn.md> "TSynPythonSyn/ru") • [TSynVBSyn](<TSynVBSyn.md> "TSynVBSyn/ru") • [TSynAnySyn](</index.php?title=TSynAnySyn/ru&action=edit&redlink=1> "TSynAnySyn/ru \(page does not exist\)") • [TSynMultiSyn](</index.php?title=TSynMultiSyn/ru&action=edit&redlink=1> "TSynMultiSyn/ru \(page does not exist\)") • [TSynBatSyn](<TSynBatSyn.md> "TSynBatSyn/ru") • [TSynIniSyn](<TSynIniSyn.md> "TSynIniSyn/ru") • [TSynPoSyn](</index.php?title=TSynPoSyn/ru&action=edit&redlink=1> "TSynPoSyn/ru \(page does not exist\)")  
[Chart](<Chart_tab.md> "Chart tab/ru") | [TChart](<TChart.md> "TChart/ru") • [TListChartSource](<TListChartSource.md> "TListChartSource/ru") • [TRandomChartSource](<TRandomChartSource.md> "TRandomChartSource/ru") • [TUserDefinedChartSource](</index.php?title=TUserDefinedChartSource/ru&action=edit&redlink=1> "TUserDefinedChartSource/ru \(page does not exist\)") • [TCalculatedChartSource](</index.php?title=TCalculatedChartSource/ru&action=edit&redlink=1> "TCalculatedChartSource/ru \(page does not exist\)") • [TDbChartSource](</index.php?title=TDbChartSource/ru&action=edit&redlink=1> "TDbChartSource/ru \(page does not exist\)") • [TChartToolset](</index.php?title=TChartToolset/ru&action=edit&redlink=1> "TChartToolset/ru \(page does not exist\)") • [TChartAxisTransformations](</index.php?title=TChartAxisTransformations/ru&action=edit&redlink=1> "TChartAxisTransformations/ru \(page does not exist\)") • [TChartStyles](</index.php?title=TChartStyles/ru&action=edit&redlink=1> "TChartStyles/ru \(page does not exist\)") • [TChartLegendPanel](</index.php?title=TChartLegendPanel/ru&action=edit&redlink=1> "TChartLegendPanel/ru \(page does not exist\)") • [TChartNavScrollBar](</index.php?title=TChartNavScrollBar/ru&action=edit&redlink=1> "TChartNavScrollBar/ru \(page does not exist\)") • [TChartNavPanel](</index.php?title=TChartNavPanel/ru&action=edit&redlink=1> "TChartNavPanel/ru \(page does not exist\)") • [TIntervalChartSource](</index.php?title=TIntervalChartSource/ru&action=edit&redlink=1> "TIntervalChartSource/ru \(page does not exist\)") • [TDateTimeIntervalChartSource](</index.php?title=TDateTimeIntervalChartSource/ru&action=edit&redlink=1> "TDateTimeIntervalChartSource/ru \(page does not exist\)") • [TChartListBox](</index.php?title=TChartListBox/ru&action=edit&redlink=1> "TChartListBox/ru \(page does not exist\)") • [TChartExtentLink](</index.php?title=TChartExtentLink/ru&action=edit&redlink=1> "TChartExtentLink/ru \(page does not exist\)") • [TChartImageList](</index.php?title=TChartImageList/ru&action=edit&redlink=1> "TChartImageList/ru \(page does not exist\)")  
[IPro](<IPro_tab.md> "IPro tab/ru") | [TIpFileDataProvider](<TIpFileDataProvider.md> "TIpFileDataProvider/ru") • [TIpHttpDataProvider](<TIpHttpDataProvider.md> "TIpHttpDataProvider/ru") • [TIpHtmlPanel](<TIpHtmlPanel.md> "TIpHtmlPanel/ru")  
  
  
****

---

_Source: [https://wiki.freepascal.org/TStringGrid/ru](https://web.archive.org/web/20250219112016/https://wiki.freepascal.org/TStringGrid/ru)_
