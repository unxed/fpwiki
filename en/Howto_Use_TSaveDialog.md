# Howto Use TSaveDialog

│ **[Deutsch (de)](</Howto_Use_TSaveDialog/de> "Howto Use TSaveDialog/de")** │  **English (en)** │  **[español (es)](</Howto_Use_TSaveDialog/es> "Howto Use TSaveDialog/es")** │  **[suomi (fi)](</Howto_Use_TSaveDialog/fi> "Howto Use TSaveDialog/fi")** │  **[français (fr)](</Howto_Use_TSaveDialog/fr> "Howto Use TSaveDialog/fr")** │  **[日本語 (ja)](</Howto_Use_TSaveDialog/ja> "Howto Use TSaveDialog/ja")** │  **[polski (pl)](</Howto_Use_TSaveDialog/pl> "Howto Use TSaveDialog/pl")** │  **[русский (ru)](<../ru/Howto_Use_TSaveDialog.md> "Howto Use TSaveDialog/ru")** │  **[slovenčina (sk)](</Howto_Use_TSaveDialog/sk> "Howto Use TSaveDialog/sk")** │    
****

[![tsavedialog.png](https://wiki.freepascal.org/images/4/4a/tsavedialog.png)](</File:tsavedialog.png>)

Simple guideline: 

  1. Place the [SaveDialog](<TSaveDialog.md> "TSaveDialog") widget [![tsavedialog.png](https://wiki.freepascal.org/images/4/4a/tsavedialog.png)](</File:tsavedialog.png>) on your [form](<TForm.md> "TForm") (anyplace, since it will be not visible).   
[![Component Palette Dialogs.png](https://wiki.freepascal.org/images/7/72/Component_Palette_Dialogs.png)](</File:Component_Palette_Dialogs.png>)   
(It is the second left dialog under [Dialogs tab](<Dialogs_tab.md> "Dialogs tab"))
  2. Add a [memo](<TMemo.md> "TMemo") [![tmemo.png](https://wiki.freepascal.org/images/f/f3/tmemo.png)](</File:tmemo.png>) in the form.
  3. Add a [button](<TButton.md> "TButton") [![tbutton.png](https://wiki.freepascal.org/images/b/b2/tbutton.png)](</File:tbutton.png>) in the form.



  


The [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector") will display the properties of the object Button1. Change a [property](</Property> "Property") named 'Caption', with the displayed value 'Button1' to 'Save'. Click on the Events tab on the Object Inspector. Select the box to the right of OnClick: a smaller box with three dots (... ellipsis) appears. Click on this, you are taken automatically into the Source Editor and your cursor will be placed in a piece of code starting. Completion code: 
    
    
     
    procedure TForm1.Button1Click( Sender: TObject );
     begin
       if SaveDialog1.Execute then
        Memo1.Lines.SaveToFile( SaveDialog1.Filename );
     end;
    

The [ Execute](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/tcommondialog.execute.html> "doc:lcl/dialogs/tcommondialog.execute.html") [method](<Method.md> "Method") displays the file save dialog. It returns [true](<True.md> "True") when user has selected a file, [false](<False.md> "False") when user has aborted. 

The [ Filename](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/tfiledialog.filename.html> "doc:lcl/dialogs/tfiledialog.filename.html") property returns the full filename including drive and path. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This control only collects the filename. It does not actually open the file for writing. Your code must do that.

## See also

  * [Howto Use TOpenDialog](<Howto_Use_TOpenDialog.md> "Howto Use TOpenDialog")
  * [CopyFile](<CopyFile.md> "CopyFile")

---

_Source: [https://wiki.freepascal.org/Howto_Use_TSaveDialog](https://web.archive.org/web/20250420133231/https://wiki.freepascal.org/Howto_Use_TSaveDialog)_
