# Howto Use TOpenDialog

│ **[Deutsch (de)](</Howto_Use_TOpenDialog/de> "Howto Use TOpenDialog/de")** │  **English (en)** │  **[español (es)](</Howto_Use_TOpenDialog/es> "Howto Use TOpenDialog/es")** │  **[suomi (fi)](</Howto_Use_TOpenDialog/fi> "Howto Use TOpenDialog/fi")** │  **[français (fr)](</Howto_Use_TOpenDialog/fr> "Howto Use TOpenDialog/fr")** │  **[日本語 (ja)](</Howto_Use_TOpenDialog/ja> "Howto Use TOpenDialog/ja")** │  **[polski (pl)](</Howto_Use_TOpenDialog/pl> "Howto Use TOpenDialog/pl")** │  **[русский (ru)](<../ru/Howto_Use_TOpenDialog.md> "Howto Use TOpenDialog/ru")** │  **[slovenčina (sk)](</Howto_Use_TOpenDialog/sk> "Howto Use TOpenDialog/sk")** │    
****

Simple and short guidelines: 

1\. Place a [TOpenDialog](<TOpenDialog.md> "TOpenDialog") widget [![topendialog.png](https://wiki.freepascal.org/images/1/1c/topendialog.png)](</File:topendialog.png>) on your [form](<TForm.md> "TForm"). It can be placed anywhere on your form as it is not visible during program [run time](<runtime.md> "runtime") but only during design time. 
    
    
    [![Component Palette Dialogs.png](https://wiki.freepascal.org/images/7/72/Component_Palette_Dialogs.png)](</File:Component_Palette_Dialogs.png>) 
    

It is located in the [Dialogs tab](<Dialogs_tab.md> "Dialogs tab") of the [component palette](<Component_Palette.md> "Component Palette") and is the leftmost component. 

2\. In your code write something similar to: 
    
    
    if OpenDialog1.Execute then
      begin
        if fileExists(OpenDialog1.Filename) then
          ShowMessage(OpenDialog1.Filename);
      end
    else
      ShowMessage('No file selected');
    

The dialog [ Execute](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/tcommondialog.execute.html> "doc:lcl/dialogs/tcommondialog.execute.html") [method](<Method.md> "Method") displays the file open dialog. It returns [true](<True.md> "True") when user has selected a file, [false](<False.md> "False") when user has aborted. 

The dialog [ Filename](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/tfiledialog.filename.html> "doc:lcl/dialogs/tfiledialog.filename.html") [property](</Property> "Property") returns the full filename including drive and path. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This control only collects the filename. It does not actually open the file. Your code must do that.

## See also

  * [Howto Use TSaveDialog](<Howto_Use_TSaveDialog.md> "Howto Use TSaveDialog")
  * [CopyFile](<CopyFile.md> "CopyFile")

---

_Source: [https://wiki.freepascal.org/Howto_Use_TOpenDialog](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Howto_Use_TOpenDialog)_
