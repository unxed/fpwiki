# Office Automation

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

Эта статья относится только к [Windows](</Category:Windows/ru> "Category:Windows/ru").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[Deutsch (de)](</Office_Automation/de> "Office Automation/de")** │  **[English (en)](<../en/Office_Automation.md> "Office Automation")** │  **[español (es)](</Office_Automation/es> "Office Automation/es")** │  **[français (fr)](</Office_Automation/fr> "Office Automation/fr")** │  **[italiano (it)](</Office_Automation/it> "Office Automation/it")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Office_Automation/zh_CN> "Office Automation/zh CN")** │    
****

Возможность взаимодействовать с офисным программным обеспечением и создание электронных таблиц, текстовых документов и презентаций из кода может быть очень полезным в офисе, и выиграть много времени, для тех, кто может это сделать. Одним из таких примеров является создание приложений, которые могут читать файлы в произвольном формате и сохранять их в Excel. Такую задачу гораздо более эффективно решить программно, нежели вручную. 

## Contents

  * 1 Использование OpenOffice UNO Bridge
  * 2 Использование COM Automation для работы с OpenOffice и Microsoft Office
  * 3 Использование библиотеки Free Pascal Spreadsheet
  * 4 Запись в файл Excel используя ADO
  * 5 Дополнительные ссылки



## Использование OpenOffice UNO Bridge

OpenOffice имеет надстройки для использования в C++ и Java а в Windows также может использоваться автоматизация COM (см. ниже). По состоянию проекта Free Pascal на данный момент времени (ноябрь 2010) достаточно сложно использовать UNO (Universal Network Objects) в среде Object Pascal для OS X и Linux. Если Вы заинтересованны в разработке OO "bridge" для Pascal, пожалуйста следуйте по следующим ссылкам (внимание: these links are quite techie in true Sun fashion): 

[api.openoffice.org](<http://api.openoffice.org/>)

[About_Bridges](<http://wiki.services.openoffice.org/wiki/Uno/Article/About_Bridges>)

## Использование COM Automation для работы с OpenOffice и Microsoft Office

Автоматизация COM существует только в Windows, поэтому следующие два примера не будут работать в OS X и-или Linux. Для этих платформ см. [Making do without Windows COM Automation](<http://wiki.lazarus.freepascal.org/Multiplatform_Programming_Guide#Making_do_without_Windows_COM_Automation>). Если вам нужно только создать и-или просмотреть текстовый документ из вашей программы, то обратите внимание на [XDev Toolkit](<http://web.fastermac.net/~MacPgmr/XDev/XDevStatus.html#RTF>). 

Вот простой пример того, как открыть документ из программы с помощью сервера автоматизации OpenOffice. Не забываем, что это работает только в Windows. 
    
    
    program TestOO;
    
    {$IFDEF FPC}
     {$MODE Delphi}
    {$ELSE}
     {$APPTYPE CONSOLE}
    {$ENDIF} 
    
    uses
      SysUtils, Variants, ComObj;
    
    const
      ServerName = 'com.sun.star.ServiceManager';
    var          
      Server     : Variant;
      Desktop    : Variant;
      LoadParams : Variant;
      Document   : Variant;
      TextCursor : Variant;
    begin
      if Assigned(InitProc) then
        TProcedure(InitProc);
    
      try
        Server := CreateOleObject(ServerName);
      except
        WriteLn('Unable to start OO.');
        Exit;
      end;
    
      Desktop := Server.CreateInstance('com.sun.star.frame.Desktop');
    
      LoadParams := VarArrayCreate([0, -1], varVariant);
    
       {Create new document}
      Document := Desktop.LoadComponentFromURL('private:factory/swriter',
                                               '_blank', 0, LoadParams);
    
      TextCursor := Document.Text.CreateTextCursor;
    
       {Insert existing document}  //Substitute your path and doc
      TextCursor.InsertDocumentFromURL('file:///C|/my/path/mydoc.doc',  
                                       LoadParams);
    end.
    

Вот простой пример того, как открыть документ из программы с помощью сервера автоматизации Word. Не забываем, что это работает только в Windows, и в настоящее время не может быть скомпилировано в Free Pascal 2.2, только в Delphi. Пожалуйста, проверьте эту информацию позже или протестируйте на болеее актуальной версии FPC. 
    
    
    program TestMsOffice;
    
    {$IFDEF FPC}
     {$MODE Delphi}
    {$ELSE}
     {$APPTYPE CONSOLE}
    {$ENDIF} 
    
    uses
      SysUtils, Variants, ComObj;
    
    const
      ServerName = 'Word.Application';
    var
      Server     : Variant;
    begin
      if Assigned(InitProc) then
        TProcedure(InitProc);
    
      try
        Server := CreateOleObject(ServerName);
      except
        WriteLn('Unable to start Word.');
        Exit;
      end;
    
       {Open existing document}  //Substitute your path and doc
      Server.Documents.Open('c:\my\path\mydoc.doc'); 
    
      Server.Visible := True;  {Make Word visible}
    
    end.
    

## Использование библиотеки Free Pascal Spreadsheet

Еще один способ автоматизировать повседневную работу с электронными таблицами заключается в создании файла с помощью библиотеки [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet/ru"). Этот метод не требует наличия в системе какого-либо установленного стороннего (офисного) приложения и при этом поддерживает сразу несколько различных форматов. 

## Запись в файл Excel используя ADO

Пожалуйста, напишите мне. 

## Дополнительные ссылки

  * Описание форматов файлов Excel на [сайте OpenOffice](<http://sc.openoffice.org/excelfileformat.pdf>)

---

_Source: [https://wiki.freepascal.org/Office_Automation/ru](https://web.archive.org/web/20250324155356/https://wiki.freepascal.org/Office_Automation/ru)_
