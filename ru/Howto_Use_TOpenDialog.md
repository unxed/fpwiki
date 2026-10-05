# Howto Use TOpenDialog

│ [**Deutsch (de)**](</Howto_Use_TOpenDialog/de> "Howto Use TOpenDialog/de") │  [**English (en)**](<../en/Howto_Use_TOpenDialog.md> "Howto Use TOpenDialog") │  [**español (es)**](</Howto_Use_TOpenDialog/es> "Howto Use TOpenDialog/es") │  [**suomi (fi)**](</Howto_Use_TOpenDialog/fi> "Howto Use TOpenDialog/fi") │  [**français (fr)**](</Howto_Use_TOpenDialog/fr> "Howto Use TOpenDialog/fr") │  [**日本語 (ja)**](</Howto_Use_TOpenDialog/ja> "Howto Use TOpenDialog/ja") │  [**polski (pl)**](</Howto_Use_TOpenDialog/pl> "Howto Use TOpenDialog/pl") │  **русский (ru)** │  [**slovenčina (sk)**](</Howto_Use_TOpenDialog/sk> "Howto Use TOpenDialog/sk") │    
****

Как использовать TOpenDialog 

[![topendialog.png](https://wiki.freepascal.org/images/1/1c/topendialog.png)](</File:topendialog.png>)

Простое и краткое руководство: 

1\. Поместите OpenDialog на вашу форму (в любое место, поскольку это - невизуальный компонент). 
    
    
      [![Component Palette Dialogs.png](https://wiki.freepascal.org/images/7/72/Component_Palette_Dialogs.png)](</File:Component_Palette_Dialogs.png>)
     
      (На вкладке Dialogs он крайний слева)
    

2\. В вашем коде напишите примерно следующее: 
    
    
    var 
      filename: string;
    
    if OpenDialog1.Execute then
    begin
      filename := OpenDialog1.Filename;
      ShowMessage(filename);
    end;

Метод [ Execute](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/tcommondialog.execute.html> "doc:lcl/dialogs/tcommondialog.execute.html") отображает диалог открытия файла. Он возвращает [true](<../en/True.md> "True"), если пользователь выбрал файл, или [false](<../en/False.md> "False"), если отказался от выбора. 

Свойство [ Filename](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/tfiledialog.filename.html> "doc:lcl/dialogs/tfiledialog.filename.html") возвращает полное имя файла, включая устройство и путь. 

## См. также

  * [Howto Use TSaveDialog/ru](<Howto_Use_TSaveDialog.md> "Howto Use TSaveDialog/ru")
  * [ Dialogs tab](<../en/Dialogs_tab.md> "Dialogs tab")

---

_Source: [https://wiki.freepascal.org/Howto_Use_TOpenDialog/ru](https://web.archive.org/web/20230127072409/https://wiki.freepascal.org/Howto_Use_TOpenDialog/ru)_
