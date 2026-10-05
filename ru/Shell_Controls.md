# Shell Controls

│ **[English (en)](<../en/Shell_Controls.md>)** │  **русский (ru)** │

## Contents

  * 1 TShellTreeView



### TShellTreeView

Компонент TShellTreeView находится на вкладке Misc. 

[![TShellTreeView.png](https://wiki.freepascal.org/images/3/3c/TShellTreeView.png)](</File:TShellTreeView.png>)

Свойства компонента TShellTreeView. 

[![Property.png](https://wiki.freepascal.org/images/4/47/Property.png)](</File:Property.png>)

Добавим компонент TShellListView. Соответственно укажем эти компоненты в свойствах друг друга. 

[![Выделение 221.png](https://wiki.freepascal.org/images/1/19/%D0%92%D1%8B%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5_221.png)](</File:%D0%92%D1%8B%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5_221.png>)

Запустим программу. В Linux отображается корневой каталог. 

[![Form1 220.png](https://wiki.freepascal.org/images/f/f5/Form1_220.png)](</File:Form1_220.png>)

А лучше сделать текстовым способом. 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
       ShellTreeView2:= TShellTreeView.create(Form1);
       ShellTreeView2.Left:=10;
       ShellTreeView2.Top:=10;
       ShellTreeView2.Width:=250;
       ShellTreeView2.Height:=430;
       ShellTreeView2.Parent:=Form1;
    end;
    

[![Form1 222.png](https://wiki.freepascal.org/images/4/49/Form1_222.png)](</File:Form1_222.png>)

---

_Source: [https://wiki.freepascal.org/Shell_Controls/ru](https://web.archive.org/web/20240227033149/https://wiki.freepascal.org/Shell_Controls/ru)_
