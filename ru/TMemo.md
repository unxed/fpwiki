# TMemo

│ **[English (en)](<../en/TMemo.md>)** │  **русский (ru)** │

**TMemo** [![tmemo.png](https://wiki.freepascal.org/images/f/f3/tmemo.png)](</File:tmemo.png>) является элементом управления с многострочным полем для редактирования текста. Данный компонент доступен на вкладке [Standard](<Standard_tab.md> "Standard tab/ru") [палитры компонентов](<Component_Palette.md> "Component Palette/ru"). 

## Contents

  * 1 Использование
    * 1.1 Присваивание строк из TStrings или TStringList
    * 1.2 Непосредственное добавление строк
    * 1.3 Чтение стоки
    * 1.4 Выделенный текст
    * 1.5 Поиск текста
    * 1.6 Сохранение и загрузка текста
  * 2 См. также



## Использование

Для использования элемента **TMemo** на [форме](<TForm.md> "TForm/ru") вам необходимо просто выбрать его на вкладке _Standard_ палитры компонентов и разместить на форме с помощью щелчка мыши. В данном текстовом поле вы можете редактировать многострочный текст во время выполнения программы. 

Например, если вы добавили элемент **TMemo** с именем _Memo1_ на форму с именем _Form1_ , то для того чтобы присвоить значение типа [String](<String.md> "String/ru") данному элементу, вы можете использовать такую инструкцию **`Memo1.Text := 'Это однострочный текст';`**. 

Также вы можете использовать текст из элемента с именем _Memo1_ в любом месте исходного кода с помощью следующей инструкции **`myString := Memo1.Text;`**. 

Кроме того можно присвоить многострочный текст элементу с именем _Memo1_ с помощью такой инструкции **`Memo1.Text:='This'+LineEnding+'is'+LineEnding+'a'+LineEnding+'multiline'+LineEnding+'text';`**` `**.**

### Присваивание строк из TStrings или TStringList

В общем, для присваивания текста элементу **TMemo** используется класс [TStringList](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial/ru") или его родительский класс - [TStrings](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial/ru"). Это показано в следующем примере (данный код располагается в обработчике события [кнопки](<TButton.md> "TButton/ru") с именем _Button1_ , расположенной на [форме](<TForm.md> "TForm/ru") с именем _Form1_ ; также на форме должен быть расположен элемент **TMemo** с именем _Memo1_): 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      myStringList: TStringList;
    begin
      myStringList:=TStringList.Create;               //создаем объект myStringList
      myStringList.Add('Это первая строка.');         //добавляем в него строки
      myStringList.Add('Это вторая строка.');
      myStringList.Add('Это третья строка.');
      myStringList.Add('и т.д.');
      Memo1.Lines.Assign(myStringList);               //присваиваем текст элементу Memo1
      myStringList.Free;                              //уничтожаем объект myStringList
    end;
    

### Непосредственное добавление строк

Вы можете непосредственно добавлять строки в элемент **TMemo** , как показано в следующем примере: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Memo1.Lines.Clear;                              //удалить все строки из элемента Memo1
      Memo1.Lines.Add('Это первая строка.');          //добавить строку
      Memo1.Lines.Add('Это вторая строка.');
      Memo1.Lines.Add('Это третья строка.');
      Memo1.Lines.Add('и т.д.');
    end;
    

### Чтение стоки

Если вы хотите узнать, что содержится в определенной строке, вы можете непосредственно проверить это с помощью такой инструкции **`myString:=Memo1.Lines[Index];`**. Обратите внимание, что индексы строк в _TMemo.Lines_ начинаются с 0, т.е. первая строка будет иметь индекс 0: **`myString:=Memo1.Lines[0];`**

Добавьте ещё одну кнопку с именем _Button2_ к предыдущему примеру для того, чтобы отобразить содержимое третьей строки, как показано в следующем примере: 
    
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      ShowMessage(Memo1.Lines[2]);
    end;
    

### Выделенный текст

Вы можете отметить часть текста в режиме выполнения программы с помощью левой кнопки мыши или удерживать клавишу [Shift] и выбрать текст с помощью мыши или стрелок на клавиатуре. Выделенный текст ([String](<String.md> "String/ru")) можно отобразить с помощью следующей инструкции: 
    
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      ShowMessage(Memo1.SelText); 
    end;
    

### Поиск текста

В отличие от предыдущего примера вы также можете искать текст ([String](<String.md> "String/ru")) в элементе **TMemo** и определять место, где он находится: **`Position:=Memo1.SelStart;`**

В следующем примере показано, как вы можете искать текст в элементе **TMemo** : 

  * Создайте новое приложение со следующими элементами: [TEdit](<TEdit.md> "TEdit/ru") с именем _Edit1_ , **TMemo** с именем _Memo1_ и две [кнопки](<TButton.md> "TButton/ru") с именами _Button1_ и _Button2_.
  * Добавьте в раздел [Uses](</index.php?title=Uses/ru&action=edit&redlink=1> "Uses/ru \(page does not exist\)") строки **LCLProc** , **strutils** и **LazUTF8**.
  * В обработчике события _OnClick_ кнопки _Button1_ заполните строки элемента _Memo1_ любым текстом, как в примере [Непосредственное добавление строк](<TMemo.md> "TMemo/ru").
  * В редакторе исходного кода добавьте следующую функцию (основана на этом примере [[1]](<http://www.lazarusforum.de/viewtopic.php?p=39260#p39260>) с German Lazarusforum):


    
    
    // Функция FindInMemo: Возвращает позицию найденной строки
    function FindInMemo(AMemo: TMemo; AString: String; StartPos: Integer): Integer;
    begin
      Result := PosEx(AString, AMemo.Text, StartPos);
      if Result > 0 then
      begin
        AMemo.SelStart := UTF8Length(PChar(AMemo.Text), Result - 1);
        AMemo.SelLength := Length(AString);
        AMemo.SetFocus;
      end;
    end;
    

  * Добавьте следующий код в обработчик события _OnClick_ кнопки _Button2_ :


    
    
    procedure TForm1.Button2Click(Sender: TObject);
    const
      SearchStr: String = '';                     // Строка, которую ищем
      SearchStart: Integer = 0;                   // Позиция, с которой начинаем искать строку
    begin
      if SearchStr <> Edit1.Text then begin       
        SearchStart := 0;
        SearchStr := Edit1.Text;
      end;
      SearchStart := FindInMemo(Memo1, SearchStr, SearchStart + 1);
    
      if SearchStart > 0 then
        Caption := 'Найдена в позиции['+IntToStr(SearchStart)+']!'
      else
        Caption := 'Нет совпадений!';
    end;
    

  * Теперь вы можете заполнить текстом элемент _Memo1_ в режиме выполнения программы с помощью кнопки _Button1_ , вставить текст, который нужно найти в элемент _Edit1_ и найти его с помощью кнопки _Button2_.



### Сохранение и загрузка текста

Вы можете довольно легко сохранять и загружать содержимое в элемент **TMemo** , используя методы _SaveToFile_ и _LoadFromFile_ класса [TStrings](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial/ru"). 

В следующем примере показано, как это можно сделать: 

  * Создайте новое приложение со следующими элементами: **TMemo** с именем _Memo1_ и три [кнопки](<TButton.md> "TButton/ru") с именами _Button1_ , _Button2_ и _Button3_.
  * Дополнительно поместите на форму элементы [TSaveDialog](<TSaveDialog.md> "TSaveDialog/ru") и [TOpenDialog](<TOpenDialog.md> "TOpenDialog/ru"), расположенные на вкладке _[Dialogs](<Dialogs_tab.md> "Dialogs tab/ru")_ палитры компонентов.
  * Измените свойство _Caption_ кнопки _Button1_ на "Fill memo".
  * В обработчике события _OnClick_ кнопки _Button1_ заполните элемент **TMemo** любым текстом, как в примере [Непосредственное добавление строк](<TMemo.md> "TMemo/ru").
  * Измените свойство _Caption_ кнопки _Button2_ на "Save memo".
  * Измените свойство _Caption_ кнопки _Button3_ на "Load memo".
  * Теперь отредактируйте обработчики событий _OnClick_ кнопок _Button2_ и _Button3_ :


    
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      if SaveDialog1.Execute then
        Memo1.Lines.SaveToFile(SaveDialog1.FileName);
    end;
    
    procedure TForm1.Button3Click(Sender: TObject);
    begin
      if OpenDialog1.Execute then
        Memo1.Lines.LoadFromFile(OpenDialog1.FileName);
    end;
    

## См. также

  * [Документация по TMemo](<http://lazarus-ccr.sourceforge.net/docs/lcl/stdctrls/tmemo.html> "doc:lcl/stdctrls/tmemo.html")
  * [TRichMemo](<RichMemo.md> "RichMemo/ru") \- Delphi-подобный компонент TRichEdit: работа с форматированным текстом (изменение цвета текста, размера шрифта и т.д.)
  * [TListBox](</index.php?title=TListBox/ru&action=edit&redlink=1> "TListBox/ru \(page does not exist\)") \- Список строк с прокруткой



  


[Компоненты LCL](<LCL_Components.md> "LCL Components/ru") Вкладка  | Компоненты   
---|---  
[Standard](<Standard_tab.md> "Standard tab/ru") | [TMainMenu](<TMainMenu.md> "TMainMenu/ru") • [TPopupMenu](<TPopupMenu.md> "TPopupMenu/ru") • [TButton](<TButton.md> "TButton/ru") • [TLabel](<TLabel.md> "TLabel/ru") • [TEdit](<TEdit.md> "TEdit/ru") • TMemo • [TToggleBox](<TToggleBox.md> "TToggleBox/ru") • [TCheckBox](<TCheckBox.md> "TCheckBox/ru") • [TRadioButton](</index.php?title=TRadioButton/ru&action=edit&redlink=1> "TRadioButton/ru \(page does not exist\)") • [TListBox](</index.php?title=TListBox/ru&action=edit&redlink=1> "TListBox/ru \(page does not exist\)") • [TComboBox](</index.php?title=TComboBox/ru&action=edit&redlink=1> "TComboBox/ru \(page does not exist\)") • [TScrollBar](</index.php?title=TScrollBar/ru&action=edit&redlink=1> "TScrollBar/ru \(page does not exist\)") • [TGroupBox](<TGroupBox.md> "TGroupBox/ru") • [TRadioGroup](<TRadioGroup.md> "TRadioGroup/ru") • [TCheckGroup](<TCheckGroup.md> "TCheckGroup/ru") • [TPanel](<TPanel.md> "TPanel/ru") • [TFrame](<TFrame.md> "TFrame/ru") • [TActionList](<TActionList.md> "TActionList/ru")  
[Additional](<Additional_tab.md> "Additional tab/ru") | [TBitBtn](<TBitBtn.md> "TBitBtn/ru") • [TSpeedButton](<TSpeedButton.md> "TSpeedButton/ru") • [TStaticText](<TStaticText.md> "TStaticText/ru") • [TImage](<TImage.md> "TImage/ru") • [TShape](<TShape.md> "TShape/ru") • [TBevel](<TBevel.md> "TBevel/ru") • [TPaintBox](<TPaintBox.md> "TPaintBox/ru") • [TNotebook](<TNotebook.md> "TNotebook/ru") • [TLabeledEdit](<TLabeledEdit.md> "TLabeledEdit/ru") • [TSplitter](<TSplitter.md> "TSplitter/ru") • [TTrayIcon](<TTrayIcon.md> "TTrayIcon/ru") • [TControlBar](<TControlBar.md> "TControlBar/ru") • [TFlowPanel](<TFlowPanel.md> "TFlowPanel/ru") • [TMaskEdit](<TMaskEdit.md> "TMaskEdit/ru") • [TCheckListBox](<TCheckListBox.md> "TCheckListBox/ru") • [TScrollBox](<TScrollBox.md> "TScrollBox/ru") • [TApplicationProperties](<TApplicationProperties.md> "TApplicationProperties/ru") • [TStringGrid](<TStringGrid.md> "TStringGrid/ru") • [TDrawGrid](<TDrawGrid.md> "TDrawGrid/ru") • [TPairSplitter](<TPairSplitter.md> "TPairSplitter/ru") • [TColorBox](<TColorBox.md> "TColorBox/ru") • [TColorListBox](<TColorListBox.md> "TColorListBox/ru") • [TValueListEditor](<TValueListEditor.md> "TValueListEditor/ru")  
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

_Source: [https://wiki.freepascal.org/TMemo/ru](https://web.archive.org/web/20250324165204/https://wiki.freepascal.org/TMemo/ru)_
