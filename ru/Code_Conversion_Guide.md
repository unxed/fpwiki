# Code Conversion Guide

│ **[English (en)](<../en/Code_Conversion_Guide.md>)** │  **русский (ru)** │

## Портирование кода из Delphi на FreePascal/Lazarus

**Добавьте в начало модуля следующую строчку:**
    
    
     {$ifdef FPC}{$mode delphi}{$endif}
    

FPC поддерживает синтаксис Delphi, но, по-умолчанию, использует использует свой собственный _fpc_. Lazarus, по-умолчанию использует синтаксис _objfpc_ для нового проекта. Указанная выше строчка указывает, что при компиляции данного модуля необходимо использовать синтаксис Delphi. 

Если модулей очень много, то проще переключить синтаксис для всего проекта: 

если вы используете только FPC компилятор, используя параметр командной строки: 
    
    
     -Mdelphi
    

если вы используете Lazarus, поменяйте свойство проекта: 
    
    
    (англ) Project -> Project Options... -> Compiler Options -> Parsing -> Syntax mode -> Delphi.
    (рус) Проект -> Параметры проекта... -> Параметры компилятора -> Обработка -> Режим синтаксиса -> Delphi
    

Использование Delphi синтаксиса поможет избежать "непонятных" ошибок компилятора, таком коде как: 
    
    
     var
       NE : TNotifyEvent;
     
       procedure TForm1.Notify(Sender: TObject);
       begin
         // do something
       end;
      
       ...
       begin
         NE := Form1.Notify; // без использования Delphi синтаксиса, строчка вызывает ошибку
     
         // для синтаксиса FPC, строчка должна выглядеть так:
         // NE := @Form1.Notify;
       end.
    

Использование синтаксиса Delphi при компиляции с помощью FPC, позволяет компилировать одни и те же модули с помощью обоих компиляторов (FPC/Delphi) без дополнительных исправлений.

---

_Source: [https://wiki.freepascal.org/Code_Conversion_Guide/ru](https://web.archive.org/web/20250123183841/https://wiki.freepascal.org/Code_Conversion_Guide/ru)_
