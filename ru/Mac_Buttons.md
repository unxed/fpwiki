# Mac Buttons

│ **[English (en)](<../en/Mac_Buttons.md>)** │  **русский (ru)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

По умолчанию кнопка, размещенная на форме в среде Lazarus имеет размер 75 (свойство **Width**) на 25 (свойство **Height**), который является нестандартным для приложений в системе macOS. 

Для того, чтобы кнопки отображались овальными, установите высоту (свойство **Height**) в максимальное значение 22. 

Это можно сделать, установив высоту кнопки напрямую (в режиме разработки) или программно. 

  
**КОД ДЛЯ ОДНОЙ КНОПКИ:**
    
    
        procedure TForm1.FormCreate(Sender: TObject);
        begin
          Button1.Height := 22;
        end;
    

**КОД ДЛЯ ВСЕХ КНОПОК:**
    
    
        procedure TForm1.FormCreate(Sender: TObject);
        var
           I: Integer;
        begin
          for I := 0 to Form1.ControlCount - 1 do
          begin
            if (Form1.Controls[I].ClassType = TButton) then
              Form1.Controls[I].Height := 22;
          end;
        end;

---

_Source: [https://wiki.freepascal.org/Mac_Buttons/ru](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/Mac_Buttons/ru)_
