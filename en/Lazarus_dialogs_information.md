# Lazarus dialogs information

│ **English (en)** │

# Dialogs to be converted to LFM

**Bold items** are being worked on 

  * **/lazarus/ide/cleandirdlg.pas** (TShowDeletingFilesDialog, will be put into its own unit) <Matthijs>
  * ~~/lazarus/ide/clipboardhistory.pas~~ (not implemented yet, so no need to convert)
  * /lazarus/ide/inputfiledialog.pas
  * **/lazarus/ide/keymapping.pp** <D. Blaszijk>
  * ~~/lazarus/ide/macropromptdlg.pas~~ (not implemented yet, so no need to convert)
  * ~~/lazarus/ide/mainbar.pas~~ (file does not contain anything suitable for conversion)
  * **/lazarus/ide/newprojectdlg.pp** <D. Blaszijk>
  * /lazarus/ideintf/objectinspector.pp (conversion started, but should be done further)
  * ~~/lazarus/debugger/debuggerdlg.pp~~ (appears to be not finished; nothing to convert)
  * /lazarus/designer/menupropedit.pp
  * /lazarus/designer/noncontroldesigner.pas
  * /lazarus/designer/objinspext.pas
  * **/lazarus/packager/addtopackagedlg.pas** <D. Blaszijk>
  * /lazarus/packager/brokendependenciesdlg.pas
  * /lazarus/packager/packagedefs.pas



# Lazarus dialogs

Class name | File | Comments   
---|---|---  
TDebuggerDlg | \debugger\debuggerdlg.pp | \-   
TDebugTestForm | \debugger\test\debugtestform.pp | \-   
TWatchPropertyDlg | \debugger\watchpropertydlg.pp | \-   
TAlignComponentsDialog | \designer\aligncompsdlg.pp | \-   
TAnchorDesigner | \designer\anchoreditor.pas | \-   
TChangeClassDlg | \designer\changeclassdialog.pas | \-   
TTemplateMenuForm | \designer\designermenu.pp | \-   
TMainMenuEditorForm | \designer\menueditorform.pas | \-   
TMenuItemsPropertyEditorDlg | \designer\menupropedit.pp | \-   
TNonControlDesignerForm | \designer\noncontroldesigner.pas | \-   
TOIAddRemoveFavouriteDlg | \designer\objinspext.pas | \-   
TScaleComponentsDialog | \designer\scalecompsdlg.pp | \-   
TSizeComponentsDialog | \designer\sizecompsdlg.pp | \-   
TTabOrderDialog | \designer\taborderdlg.pas | \-   
TMakeSkelForm | \doceditor\fmmakeskel.pp | \-   
TAboutForm | \doceditor\frmabout.pp | \-   
TBuildForm | \doceditor\frmbuild.pp | \-   
TExampleForm | \doceditor\frmexample.pp | \-   
TLinkForm | \doceditor\frmlink.pp | \-   
TMainForm | \doceditor\frmmain.pp | \-   
TMakeSkelForm | \doceditor\frmmakeskel.pp | \-   
TNewNodeForm | \doceditor\frmnewnode.pp | \-   
TOptionsForm | \doceditor\frmoptions.pp | \-   
TSourceForm | \doceditor\frmsource.pas | \-   
TTableForm | \doceditor\frmtable.pp | \-   
TAboutForm | \ide\aboutfrm.pas | \-   
TAddToProjectDialog | \ide\addtoprojectdlg.pas | \-   
THelpSelectorDialog | \ide\backup\helpmanager.pas.bak | \-   
TfrmGoto | \ide\backup\uniteditor.pp.bak | \-   
TBuildFileDialog | \ide\buildfiledlg.pas | \-   
TConfigureBuildLazarusDlg | \ide\buildlazdialog.pas | \-   
TCharacterMapDialog | \ide\charactermapdlg.pas | \-   
TCheckCompilerOptsDlg | \ide\checkcompileropts.pas | \-   
TCheckLFMDialog | \ide\checklfmdlg.pas | \-   
TCleanDirectoryDialog | \ide\cleandirdlg.pas | \-   
TShowDeletingFilesDialog | \ide\cleandirdlg.pas | \-   
TClipBoardHistory | \ide\clipboardhistory.pas | \-   
TCodeContextFrm | \ide\codecontextform.pas | \-   
TCodeExplorerDlg | \ide\codeexplopts.pas | \-   
TCodeExplorerView | \ide\codeexplorer.pas | \-   
TCodeMacroPromptDlg | \ide\codemacroprompt.pas | \-   
TCodeMacroSelectDlg | \ide\codemacroselect.pas | \-   
TCodeTemplateDialog | \ide\codetemplatesdlg.pas | \-   
TCodeTemplateEditForm | \ide\codetemplatesdlg.pas | \-   
TCodeToolsDefinesEditor | \ide\codetoolsdefines.pas | \-   
TCodeToolsDefinesDialog | \ide\codetoolsdefpreview.pas | \-   
TCodeToolsOptsDlg | \ide\codetoolsoptions.pas | \-   
TfrmCompilerOptions | \ide\compileroptionsdlg.pp | \-   
TCondForm | \ide\condef.pas | \-   
TDebuggerOptionsForm | \ide\debugoptionsfrm.pas | \-   
TDelphi2LazarusDialog | \ide\delphiunit2laz.pas | \-   
TDiffDlg | \ide\diffdialog.pas | \-   
TDiskDiffsDlg | \ide\diskdiffsdialog.pas | \-   
TEditorOptionsForm | \ide\editoroptions.pp | \-   
TKeyMapErrorsForm | \ide\editoroptions.pp | \-   
TEncloseSelectionDialog | \ide\encloseselectiondlg.pas | \-   
TEnvironmentOptionsDialog | \ide\environmentopts.pp | \-   
TExtractProcDialog | \ide\extractprocdlg.pas | \-   
TExternalToolDialog | \ide\exttooldialog.pas | \-   
TExternalToolOptionDlg | \ide\exttooleditdlg.pas | \-   
TLazFindInFilesDialog | \ide\findinfilesdlg.pas | \-   
TFindRenameIdentifierDialog | \ide\findrenameidentifier.pas | \-   
TLazFindReplaceDialog | \ide\findreplacedialog.pp | \-   
TSearchForm | \ide\frmsearch.pas | \-   
THelpSelectorDialog | \ide\helpmanager.pas | \-   
THelpOptionsDialog | \ide\helpoptions.pas | \-   
TImExportCompOptsDlg | \ide\imexportcompileropts.pas | \-   
TInputFileDialog | \ide\inputfiledialog.pas | \-   
TKeyMappingEditForm | \ide\keymapping.pp | \-   
TChooseKeySchemeDlg | \ide\keymapschemedlg.pas | \-   
TLazDocForm | \ide\lazdocfrm.pas | \-   
TMacroPrompDialog | \ide\macropromptdlg.pas | \-   
TMainIDEBar | \ide\mainbar.pas | \-   
TMakeResStrDialog | \ide\makeresstrdlg.pas | \-   
TMessagesView | \ide\msgview.pp | \-   
TMsgViewEditorDlg | \ide\msgvieweditor.pas | \-   
TMultiReplaceDialog | \ide\multireplacedlg.pas | \-   
TNewOtherDialog | \ide\newdialog.pas | \-   
TNewProjectDialog | \ide\newprojectdlg.pp | \-   
TPathEditorDialog | \ide\patheditordlg.pas | \-   
TIDEProgressDialog | \ide\progressdlg.pas | \-   
TProjectInspectorForm | \ide\projectinspector.pas | \-   
TProjectOptionsDialog | \ide\projectopts.pp | \-   
TPublishProjectDialog | \ide\publishprojectdlg.pas | \-   
TRunParamsOptsDlg | \ide\runparamsopts.pas | \-   
TSearchResultsView | \ide\searchresultview.pp | \-   
TShowCompilerOptionsDlg | \ide\showcompileropts.pas | \-   
TSortSelectionDialog | \ide\sortselectiondlg.pas | \-   
TSplashForm | \ide\splash.pp | \-   
TSysVarUserOverrideDialog | \ide\sysvaruseroverridedlg.pas | \-   
TfrmTodo | \ide\todolist.pp | \-   
TUnitDependenciesView | \ide\unitdependencies.pas | \-   
TfrmGoto | \ide\uniteditor.pp | \-   
TUnitInfoDialog | \ide\unitinfodlg.pp | \-   
TViewUnitDialog | \ide\viewunit_dlg.pp | \-   
TActionListEditor | \ideintf\actionseditor.pas | \-   
TFormActStandard | \ideintf\actionseditorstd.pas | \-   
TColumnDlg | \ideintf\columndlg.pp | \-   
TStringGridEditorD | \ideintf\componenteditors.pas | \-   
TCheckListBoxEditorDlg | \ideintf\componenteditors.pas | \-   
TCheckGroupEditorDlg | \ideintf\componenteditors.pas | \-   
TDSFieldsEditorFrm | \ideintf\fieldseditor.pas | \-   
TFieldsListFrm | \ideintf\fieldslist.pas | \-   
TSelectPropertiesForm | \ideintf\frmselectprops.pas | \-   
TGraphicPropertyEditorForm | \ideintf\graphicpropedit.pas | \-   
TImageListEditorDlg | \ideintf\imagelisteditor.pp | \-   
TListViewItemsEditorForm | \ideintf\listviewpropedit.pp | \-   
TMaskEditorForm | \ideintf\maskpropedit.pas | \-   
TNewFieldF | \ideintf\newfield.pas | \-   
TObjectInspector | \ideintf\objectinspector.pp | \-   
TStringsPropEditorDlg | \ideintf\propedits.pp | \-   
TCollectionPropertyEditorForm | \ideintf\propedits.pp | \-   
TFileFilterPropertyEditorForm | \ideintf\propedits.pp | \-   
TSourceEditorWindowInterface | \ideintf\srceditorintf.pas | \-   
TTreeViewItemsEditorForm | \ideintf\treeviewpropedit.pas | \-   
TDirSelDlg | \lcl\dirsel.pas | \-   
TCalculatorForm | \lcl\extdlgs.pas | \-   
TPromptDialog | \lcl\include\promptdialog.inc | \-   
TQuestionDlg | \lcl\include\promptdialog.inc | \-   
TLazDockControlEditorDlg | \lcl\ldockctrledit.pas | \-   
TAddFileToAPackageDialog | \packager\addfiletoapackagedlg.pas | \-   
TAddToPackageDlg | \packager\addtopackagedlg.pas | \-   
TBrokenDependenciesDialog | \packager\brokendependenciesdlg.pas | \-   
TInstallPkgSetDialog | \packager\installpkgsetdlg.pas | \-   
TBasePackageEditor | \packager\packagedefs.pas | \-   
TPkgGraphExplorerDlg | \packager\pkggraphexplorer.pas | \-   
TPackageOptionsDialog | \packager\pkgoptionsdlg.pas | \-   
TEditVirtualUnitDialog | \packager\pkgvirtualuniteditor.pas | \-   
TFrmComponentMan | \packager\ucomponentmanmain.pas | \-   
TFrmAddComponent | \packager\ufrmaddcomponent.pas | \-   
TApiWizForm | \tools\apiwizz\apiwizard.pp | \-   
  
# Other dialogs

Class name | File | Comments   
---|---|---  
TForm1 | \components\chmhelp\democontrol\unit1.pas | \-   
THelpPopupForm | \components\chmhelp\lhelp\chmpopup.pas | \-   
THelpForm | \components\chmhelp\lhelp\lhelpcore.pas | \-   
TTestCaseOptionsForm | \components\fpcunit\ide\testcaseopts.pas | \-   
TImagesExampleForm | \components\images\examples\mainform.pas | \-   
TJPEGExampleForm | \components\jpeg\examples\mainform.pas | \-   
TSelectSrcDatasetForm | \components\memds\frmselectdataset.pp | \-   
TForm1 | \components\opengl\example\mainunit.pas | \-   
TForm1 | \components\printers\sample\frmselprinter.pas | \-   
TdlgPrintersJobs | \components\printers\unix\udlgprintersjobs.pp | \-   
Tdlgpropertiesprinter | \components\printers\unix\udlgpropertiesprinter.pp | \-   
TdlgSelectPrinter | \components\printers\unix\udlgselectprinter.pp | \-   
TTemplateSettingsForm | \components\projecttemplates\frmtemplatesettings.pas | \-   
TProjectVariablesForm | \components\projecttemplates\frmtemplatevariables.pas | \-   
TForm1 | \components\rtticontrols\examples\example1.pas | \-   
TForm1 | \components\rtticontrols\examples\example2.pas | \-   
TForm1 | \components\rtticontrols\examples\example3.pas | \-   
TForm1 | \components\rtticontrols\examples\examplegrid1.pas | \-   
TSqliteTableEditorForm | \components\sqlite\sqlitecomponenteditor.pas | \-   
TSqliteTableEditorForm | \components\sqlite\tableeditorform.pas | \-   
TSynBaseCompletionForm | \components\synedit\syncompletion.pas | \-   
TForm1 | \components\trayicon\examples\frmtest.pas | \-   
TIpHTMLPreview | \components\turbopower_ipro\iphtmlpv.pas | \-   
TMainForm | \examples\address_book\frmmain.pas | \-   
TChildsizingLayoutDemoForm | \examples\autosize\childsizinglayout\mainunit.pas | \-   
TForm1 | \examples\barchart\frmmain.pas | \-   
TForm1 | \examples\bitbtnform.pp | \-   
TForm1 | \examples\checkbox.pp | \-   
TLazConverterForm | \examples\codepageconverter\mainunit.pas | \-   
TForm1 | \examples\combobox.pp | \-   
TSampleDialogs | \examples\dlgform.pp | \-   
TAboutBox | \examples\easter\about.pas | \-   
TForm1 | \examples\easter\main.pas | \-   
TEditTestForm | \examples\edittest.pp | \-   
TExploreIDEMenuForm | \examples\exploremenu\frmexploremenu.pas | \-   
TfrmMain | \examples\fontenum\mainunit.pas | \-   
TForm1 | \examples\grid_semaphor\example\unit1.pas | \-   
TForm1 | \examples\groupbox.pp | \-   
TForm1 | \examples\groupboxnested.pas | \-   
TExampleForm | \examples\gtkglarea\exampleform.pp | \-   
THello | \examples\helloform.pp | \-   
TMainForm | \examples\imgviewer\frmmain.pas | \-   
TForm1 | \examples\lazintfimage\mainunit1.pas | \-   
TListBoxTestForm | \examples\listboxtest.pp | \-   
TForm1 | \examples\listview\testform.pp | \-   
TMyForm | \examples\listviewtest.pp | \-   
TLoadBitmapForm | \examples\loadpicture.pas | \-   
TMemoTestForm | \examples\memotest.pp | \-   
TMainForm | \examples\messagedialogs.pp | \-   
TForm1 | \examples\notebku.pp | \-   
TForm1 | \examples\notebooktest.pp | \-   
TForm1 | \examples\objectinspector\mainunit.pas | \-   
TForm1 | \examples\postscript\usamplepostscriptcanvas.pas | \-   
TForm1 | \examples\progressbar.pp | \-   
TForm1 | \examples\scrollbar.pp | \-   
TForm1 | \examples\selectionform.pp | \-   
TForm1 | \examples\speedtest.pp | \-   
TPlayGroundForm | \examples\sprites\playground.pas | \-   
TForm1 | \examples\synchronize.pp | -"   
TForm1 | \examples\synedit1.pas | \-   
TForm1 | \examples\taborder.pas | \-   
TForm1 | \examples\testallform.pp | \-   
TForm1 | \examples\toolbar.pp | \-   
TForm1 | \examples\trackbar.pp | \-   
TForm1 | \examples\treeview\tv_add_remove_u1.pas | \-   
TMainForm | \examples\turbopower_ipro\mainunit.pas | \-

---

_Source: [https://wiki.freepascal.org/Lazarus_dialogs_information](https://web.archive.org/web/20230320135700/https://wiki.freepascal.org/Lazarus_dialogs_information)_
