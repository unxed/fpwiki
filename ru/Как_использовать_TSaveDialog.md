# Как использовать TSaveDialog

From Free Pascal wiki

│ **русский (ru)** │

##  Как использовать TSaveDialog 

Простой пример: 

1\. Поместите виджет SaveDialog на форму (в любое место, он все равно не будет отображаться). 
    
    
     [![Component Palette Dialogs.png](https://wiki.freepascal.org/images/7/72/Component_Palette_Dialogs.png)](</File:Component_Palette_Dialogs.png>) 
     (Вторая слева иконка на [Dialogs tab](<../en/Dialogs_tab.md> "Dialogs tab"))
    

  
2\. Добавьте на форму компонент Memo. 
    
    
     [![tmemo.png](https://wiki.freepascal.org/images/f/f3/tmemo.png)](</File:tmemo.png>)
    

  
3\. Добавьте на форму кнопку. 
    
    
     [![tbutton.png](https://wiki.freepascal.org/images/b/b2/tbutton.png)](</File:tbutton.png>)
    

  
[Инспектор объектов](<../en/IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector") отображает свойства кнопки Button1. Измените свойство Caption, которое сейчас 'Button1', на 'Сохранить'. Нажмите на вкладку События в Инспекторе объектов. Выберите строку с текстом OnClick: справа появится маленькая кнопка с тремя точками. Нажмите на эту кнопку и в редакторе исходного кода автоматически создастся выбранное событие. Допишите код: 
    
    
     procedure TForm1.Button1Click( Sender: TObject );
     begin
       if SaveDialog1.Execute then
        Memo1.Lines.SaveToFile( SaveDialog1.Filename );
     end;

Метод [ Execute](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/tcommondialog.execute.html> "doc:lcl/dialogs/tcommondialog.execute.html") отображает диалог сохранения файла. Возвражает [true](<../en/True.md> "True") если пользователь выбрал сохранение файла, [false](<../en/False.md> "False") если пользователь отменил сохранение. 

Свойство [ Filename](<http://lazarus-ccr.sourceforge.net/docs/lcl/dialogs/tfiledialog.filename.html> "doc:lcl/dialogs/tfiledialog.filename.html") возвращает полный путь к файлу, включая имя диска. 

##  См. также 

  * [ Как использовать TOpenDialog](<../en/Howto_Use_TOpenDialog.md> "Howto Use TOpenDialog")
  * [ Dialogs tab](<../en/Dialogs_tab.md> "Dialogs tab")
  * [ TButton](<../en/Standard_tab.md> "Standard tab")
  * [ TMemo](<../en/Standard_tab.md> "Standard tab")

---

_Source: [https://wiki.freepascal.org/%D0%9A%D0%B0%D0%BA_%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D1%8C_TSaveDialog/ru](https://web.archive.org/web/20130324130515/https://wiki.freepascal.org/%D0%9A%D0%B0%D0%BA_%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D1%8C_TSaveDialog/ru)_
