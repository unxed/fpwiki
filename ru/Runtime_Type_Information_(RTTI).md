# Runtime Type Information (RTTI)

│ **[English (en)](<../en/Runtime_Type_Information_(RTTI).md>)** │  **русский (ru)** │

  
****

Информация времени выполнения (RTTI) может быть использована для получения мета-данных в приложениях Pascal.

## Contents

  * 1 Преобразование перечислимого типа в строку
  * 2 См. также



## Преобразование перечислимого типа в строку

Можно использовать RTTI для получения строки из перечисляемого типа. 
    
    
    type
      TProgrammerType = (tpDelphi, tpVisualC, tpVB, tpJava) ;
    
    uses TypInfo;
    
    var 
      s: string;
    begin
      s := GetEnumName(TypeInfo(TProgrammerType), integer(tpDelphi));
      // Здесь s = 'tpDelphi'
    

  
Вы также можете сделать это, без использования RTTI: 
    
    
    program noRTTI;
    type
      TProgrammerType = (tpDelphi, tpVisualC, tpVB, tpJava) ; 
    var 
      s: string;
    begin
      writestr(s,tpDelphi);
      writeln(s);
    end.
    

## См. также

  * [Компоненты RTTI](<RTTI_controls.md> "RTTI controls/ru")
  * [Вкладка RTTI](<RTTI_tab.md> "RTTI tab/ru")
  * <http://www.blong.com/Conferences/BorConUK98/DelphiRTTI/CB140.htm>

---

_Source: [https://wiki.freepascal.org/Runtime_Type_Information_(RTTI)/ru](https://web.archive.org/web/20250323133853/https://wiki.freepascal.org/Runtime_Type_Information_(RTTI)/ru)_
