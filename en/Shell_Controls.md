# Shell Controls

│ **English (en)** │  **[русский (ru)](<../ru/Shell_Controls.md>)** │

The Shell Controls are a series of advanced controls destinated to beaultifully represent files and directories of the system. 

### TShellTreeView

TShellTreeView component under the tab Misc. 

[![TShellTreeView.png](https://wiki.freepascal.org/images/3/3c/TShellTreeView.png)](</File:TShellTreeView.png>)

Component Properties TShellTreeView. 

[![Property.png](https://wiki.freepascal.org/images/4/47/Property.png)](</File:Property.png>)

Add component TShellListView. Set properties 
    
    
    ShellTreeView1.ShellListView := ShellListView1;
    ShellListView1.ShellTreeView := ShellTreeView1;
    

[![Выделение 221.png](https://wiki.freepascal.org/images/1/19/%D0%92%D1%8B%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5_221.png)](</File:%D0%92%D1%8B%D0%B4%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5_221.png>)

Run application. We can see root directory in Linux. 

[![Form1 220.png](https://wiki.freepascal.org/images/f/f5/Form1_220.png)](</File:Form1_220.png>)

Better way to do this is: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
       ShellTreeView2:= TShellTreeView.create(Self);
       ShellTreeView2.Left:=10;
       ShellTreeView2.Top:=10;
       ShellTreeView2.Width:=250;
       ShellTreeView2.Height:=430;
       ShellTreeView2.Parent:=Self;
    end;
    

[![Form1 222.png](https://wiki.freepascal.org/images/4/49/Form1_222.png)](</File:Form1_222.png>)

---

_Source: [https://wiki.freepascal.org/Shell_Controls](https://web.archive.org/web/20250417153109/https://wiki.freepascal.org/Shell_Controls)_
