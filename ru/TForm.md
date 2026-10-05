# TForm

│ **[Deutsch (de)](</TForm/de> "TForm/de")** │  **[English (en)](<../en/TForm.md> "TForm")** │  **[suomi (fi)](</TForm/fi> "TForm/fi")** │  **[français (fr)](</TForm/fr> "TForm/fr")** │  **[日本語 (ja)](</TForm/ja> "TForm/ja")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</TForm/zh_CN> "TForm/zh CN")** │    
****

**TForm** является классом объекта '**форма'**. Все формы, созданные во время разработки, могут быть получены из **TForm**. 

Форма представляет обычное или диалоговое окно, которое формирует интерфейс приложения. Она является контейнером, на котором могут быть размещены другие компоненты, например, [кнопки](<TButton.md> "TButton/ru"), [метки](<TLabel.md> "TLabel/ru"), [поля редактирования текста](<TEdit.md> "TEdit/ru"), [элементы с изображениями](<TImage.md> "TImage/ru") и т.д. 

Новый экземпляр класса **TForm** может быть создан в среде Lazarus с помощью команд [File|New...](<../en/IDE_Window__New_Item.md> "IDE Window: New Item"). 

## Contents

  * 1 Приложение
  * 2 Свойства
  * 3 См. также



## Приложение

При запуске программы главная форма (как и любая другая форма), которая должна быть создана автоматически, фактически так и создается. Создаваемые автоматически формы могут быть выбраны из списка доступных форм в [Project|Project Options|Forms]. Если по какой-либо причине среди перечисленных доступных форм нет формы, которая должна быть создана автоматически, добавьте необходимое имя формы в раздел [Uses](</index.php?title=Uses/ru&action=edit&redlink=1> "Uses/ru \(page does not exist\)") и строку `Application.CreateForm` для этой формы. 

Пример: 
    
    
    program PTest;
    uses
      Forms,
      UMainForm,
      UOtherForm;
    {$R *.res}
    
    begin
      Application.Title:='Test';
      RequireDerivedFormResource := True;
      Application.Initialize();
      Application.CreateForm(TMainForm, MainForm);
      Application.CreateForm(TOtherForm, OtherForm);
      Application.Run();
    end.
    

## Свойства

  * Menu - связь с элементом [TMainMenu](<TMainMenu.md> "TMainMenu/ru"), который будет отображен в верхней части формы во время выполнения программы
  * Popupmenu - связь с элементом [TPopupMenu](<TPopupMenu.md> "TPopupMenu/ru"), который будет отображен при щелчке правой кнопкой мыши по форме
  * PopupParent -
  * SessionProperties -
  * ActiveControl -



## См. также

  * [Документация по TForm](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tform.html> "doc:lcl/forms/tform.html")
  * [Руководство по работе с формами](</index.php?title=Form_Tutorial/ru&action=edit&redlink=1> "Form Tutorial/ru \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/TForm/ru](https://web.archive.org/web/20250113144344/https://wiki.freepascal.org/TForm/ru)_
