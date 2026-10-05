# TMainMenu

│ **English (en)** │  **[русский (ru)](<../ru/TMainMenu.md>)** │

A **TMainMenu** [![tmainmenu.png](https://wiki.freepascal.org/images/4/4d/tmainmenu.png)](</File:tmainmenu.png>) is a non-visual component from the [Standard tab](<Standard_tab.md> "Standard tab") of the [Component Palette](<Component_Palette.md> "Component Palette") that provides a main menu on a form. 

## Contents

  * 1 Description
  * 2 Conventions
  * 3 Creating Menus
  * 4 Making the menu actually do something
  * 5 Check-box menu
  * 6 Separators
  * 7 ShortCuts
  * 8 Image in front of a menu
  * 9 Issue: Submenus dropdown to the left (Windows Vista, 7 and 10)
  * 10 Creating in Runtime
  * 11 See also



## Description

The main menu that appears at the top of most windows that form designers can customize by choosing various menu items. 

To see the Menu Editor, right-click on the Main Menu icon on your form. 

## Conventions

It is common practice to name menus starting with _mnu_ or _menu_ and the name of the menu. Submenus continue this by prefixing with the menu they are within, e.g. the 'Cut' submenu in the Edit top-level menu is usually named _mnuEditCut_ or _menuEditCut_. This is a mnemonic and makes it easier to remember, six months from now when you need to make changes, how to put them in. Note that this is a _convention_ , it isn't mandatory, but if you do it this way it will probably make it easier to make changes later, or to understand what the code is doing when you haven't been looking at it for a while, or to allow someone else who has to do maintenance on the program you're writing to be able to fix it later. 

Let's get started. 

## Creating Menus

  1. Select TMainMenu from the component bar and place a component on your form by clicking on the TMainMenu component, then click on the form but do not let go of the mouse button, and while holding the mouse button down, draw a box and let go of the button. The component will appear on your form. This will be a square with a representation of a drop-down menu and the component's name, which will default to _MainMenu1_.
  2. If you don't like the name **MainMenu1** , go to the Object Inspector window and change the Name property to something you like better. Let's say we change it to _XMenu_. Type in **XMenu** in the box to the left of the Name property, and push enter. The name on the component changes.
  3. Right-click on XMenu, and a pop-up menu will appear. For right now, what we want is the first selection, **Menu Editor**. Click on it.
  4. The Menu Editor window will open with a menu item already created with a caption of "New Item1". This will be the top-level menu, similar to the "File Edit View Help" menus you've seen before. You probably want to change this, so click on it, then go to the Object Inspector.
  5. In object inspector, change the Name property from MenuItem1 to something more appropriate. Let's say this is the File menu, so let's change Name by typing in **mnuFile** and press enter.
  6. We want a better caption than New Item1, so go to the Caption property and type in **& File** and press enter. The Ampersand **&** in front of the name is an _accelerator_ , that's what allows you to open a menu by pressing the Alt key and the underlined letter. The caption for the menu changes to **_F_ ile**.



It is at this point that you create additional top-level menus. 

  1. Go back to the Menu Editor window. Click on _F_ ile, then right-click on it. A pop-up menu will appear. Click on _Insert New Item After_ , and a new menu, called _New Item2_ will appear. As explained in the last two items, let's change its name to **mnuHelp** and the caption to **& Help** in the Object Inspector.
  2. Let's make a menu item under _F_ ile. Right-click on it, then click on **Create Submenu**. The file menu now has an arrow on it, and a submenu called _New Item2_ appears.
  3. Change this to something related to what it is to do, let's say "Open". Go to the Object inspector, change the Name property to **mnuFileOpen** , then change the caption to **& Open**.
  4. We need another top-level menu item between **_F_ ile** and **_H_ elp**. You can either right-click on **_F_ ile** and click on **Insert New Item (after)** or right-click on **_H_ elp** and click on **Insert New Item (before)**
  5. Change this item's name property in the Object Inspector to **mnuEdit** and its caption property to **& Edit**.
  6. Continue the above accordingly for each menu and submenu you need.



Now, all this will get you is a menu that displays at run time and will allow the user to click on the menus. It won't actually do anything. To have the menu items do something, you have to add [events](<Event_order.md> "Event order") for each menu or submenu that is to react to being clicked upon. Usually, top-level menus don't react, the submenus do. You have two choices on how to have the menu react; you can insert the events into the menu, or you can use a [TActionList](<TActionList.md> "TActionList") component. The main reason for using a TActionList is if you plan to have an icon toolbar, say that you have a set of menus with "File" then New, Open, Save, Save As, Quit, etc. as submenus, and you're going to have a toolbar with New, Open, Save and Save As as buttons as well. Rather than writing two routines to handle the New and Open functions, you use a TActionList for both the Menu and the toolbar. How to do that using a TActionList will be explained there. For the mean time, I'll explain how to handle a menu click using an event in the Object Explorer. 

## Making the menu actually do something

  1. Go back to the Menu Editor window, click on the **_O_ pen** submenu under **_F_ ile**. Go to the Object Inspector window, click on the **Events** tab. The only event you really want to change is OnClick, which is blank. If you had an existing event handler, you could use it, but since you don't, you can get Lazarus to create it for you. On the right is a button with 3 dots. Click on it, and a new procedure is created in your code, and the view switches to the code window. It will look similar to this. 

    **procedure** TfrmMain.mnuFileOpenClick(Sender: TObject);
    **begin**  
  

    **end** ;
  2. between tbe **begin** and **end** statements you would write the code for handling the Open action on the menu. This might include placing a _TOpenDialog_ control from the Dialogs Tab on your form, and manipulating that dialog to create the standard 'Open' dialog. Same thing applies if you have a **_S_ ave** or **Save _a_ s** submenu.
  3. You repeat the above at the point where I mentioned how to start creating additional menus and submenus, and for each one that the user can click upon, you would create handlers for each menu option as needed.



## Check-box menu

Now, maybe you just want a check-box menu, where when the user clicks on it, it turns a check mark on this box on or off. Let's discuss how to do that. 

  1. Go to the Menu Editor window Click on **_E_ dit**
  2. Let's make a menu item under _E_ dit. Right-click on it, then click on **Create Submenu**. go to the Object Inspector window, click on the **Properties** tab if it's not already selected. Give this submenu the name **mnuEditPreserve** and the caption **P &reserve case** (Since Copy and Paste usually use ALT+P and ALT+C we'll use ALT+R for this submenu which is why the ampersand is before the letter r.
  3. The default value under _checked_ will be False. If the default state of this menu is checked, double click on the value to flip it from False to True.
  4. Select the **Events** tab, choose the **Click** property and click on the ... button.
  5. Lazarus will switch to the code window, and create the Click event for this submenu.
  6. within the **begin** and **end** boxes, all you need is one line, similar to the following: 

    mnuEditPreserve.checked := not mnuEditPreserve.checked ;
  7. This will flip the value from checked to unchecked. To be able to use this checked menu in your code, just reference **`mnuEditPreserve.checked`** (or whatever the name of the menu is, with the property _checked_). It's used just as any other [Boolean](<Boolean.md> "Boolean") value.



## Separators

Sometimes you want a menu that has a line separating entries. For example, an **_E_ dit** menu might have submenus for **Cut** , **Copy** and **Paste** , then have a separator line before the next submenu. To create a separator line, just make another submenu, and make the caption consist of a single dash (-). 

## ShortCuts

If you want, you can assign a menu a specific key combination, proceed as follows: 

  * Select the menu in the menu editor, which should get assigned a keyboard shortcut.
  * In the Object Inspector, go to the property _ShortCut_ and click on the button [...].
  * It will appear a window where you can select the desired ShortCut for your menu.
  * At run time the event handler of your menu is called with this ShortCut, like you would have clicked on this menu.



## Image in front of a menu

If you want to make your menu more visually appealing or establish an optical mapping of the menu entries to a possible toolbar, you can show images in front of the menus. The following steps are necessary: 

  * Add a [TImageList](<TImageList.md> "TImageList") to your [form](<TForm.md> "TForm"). This is found on the component palette _Common Controls_. Choose that component TImageList and click on your Form. Now the ImageList named _ImageList1_ on the form was created. These ImageList will contain all symbols or images to be displayed before the menus.
  * Right click now on the _ImageList1_ and open you the **ImageList Editor**.
  * Add all the images, one after the other, you need for your menus, to the ImageList. Simply click the button _Add_ and select as usual an appropriate image.
  * When you have added all of the required images in the ImageList, you confirm your selection with [OK] button and the ImageList Editor is closed.
  * Now, you have to select your MainMenu, and set the property _Images_ in the Object Inspector to your ImageList. Simply select your ImageList _ImageList1_ in the adjacent combobox.
  * Now open the menu editor of your menu again and select the menu that you want to get a image.
  * Go in the Object Inspector to the property _ImageIndex_ from your menu and select the image to display in the adjacent combobox.
  * In the Menu Editor, select the next menu and choose the corresponding image for it. And so on.



## Issue: Submenus dropdown to the left (Windows Vista, 7 and 10)

If you see the pulldown menu aligned to the right of your mainmenu-item, and a submenu opens to the left, it's because your Windows is set to **Right-handed** in the **Tablet PC Settings**. This is done for when using a tablet and writing/pressing on the screen, the menus are directly visible. If you're right-handed you would want the menus to the left so they are not under your hand. For normal PC operations this is set to left-handed by default. 

You can check this by doing the following: 

  * Press the Window-key + R.
  * Paste in **shell:::{80F3F1D5-FECA-45F3-BC32-752C152E456E}** and press enter.
  * Goto the tab **Other**.
  * The default should be **Left-handed** for the menus to appear on the right otherwise they appear on the left.
  * Note: The arrow for a submenu will still appear on the right of the menu regardless this setting.



## Creating in Runtime

In order to create TMainMenu in runtime, once should specify a form as the owner of TMainMenu 
    
    
      mnuMainMain = TMainMenu.Create(Form1);
    

(note, specifying parent is not necessary, specifying Application as the owner, will not have the desired effect) 

## See also

  * [TMainMenu doc](<http://lazarus-ccr.sourceforge.net/docs/lcl/menus/tmainmenu.html> "doc:lcl/menus/tmainmenu.html")
  * [TPopupMenu](<TPopupMenu.md> "TPopupMenu")
  * [TActionList](<TActionList.md> "TActionList")



  


[LCL Components](<LCL_Components.md> "LCL Components") Component Tab  | Components   
---|---  
[Standard](<Standard_tab.md> "Standard tab") | TMainMenu • [TPopupMenu](<TPopupMenu.md> "TPopupMenu") • [TButton](<TButton.md> "TButton") • [TLabel](<TLabel.md> "TLabel") • [TEdit](<TEdit.md> "TEdit") • [TMemo](<TMemo.md> "TMemo") • [TToggleBox](<TToggleBox.md> "TToggleBox") • [TCheckBox](<TCheckBox.md> "TCheckBox") • [TRadioButton](<TRadioButton.md> "TRadioButton") • [TListBox](<TListBox.md> "TListBox") • [TComboBox](<TComboBox.md> "TComboBox") • [TScrollBar](<TScrollBar.md> "TScrollBar") • [TGroupBox](<TGroupBox.md> "TGroupBox") • [TRadioGroup](<TRadioGroup.md> "TRadioGroup") • [TCheckGroup](<TCheckGroup.md> "TCheckGroup") • [TPanel](<TPanel.md> "TPanel") • [TFrame](<TFrame.md> "TFrame") • [TActionList](<TActionList.md> "TActionList")  
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

_Source: [https://wiki.freepascal.org/TMainMenu](https://web.archive.org/web/20230327104150/https://wiki.freepascal.org/TMainMenu)_
