# Goto

│ **[Deutsch (de)](</Goto/de> "Goto/de")** │  **[English (en)](<../en/Goto.md> "Goto")** │  **[français (fr)](</Goto/fr> "Goto/fr")** │  **русский (ru)** │    
****

**Goto** \- безусловный переход на предварительно объявленную [метку](<Label.md> "Label/ru") (либо до, либо после команды goto). 

Пример объявления метки и использования команды goto: 
    
    
    var
      fWaterIsBoiling: Boolean;
     
    label
      SwitchOffKettle;
     
    begin
      ...
      if fWaterIsBoiling = True then Goto SwitchOffKettle;
      ...
    SwitchOffKettle:
      ...
    end;

---

_Source: [https://wiki.freepascal.org/Goto/ru](https://web.archive.org/web/20250315114301/https://wiki.freepascal.org/Goto/ru)_
