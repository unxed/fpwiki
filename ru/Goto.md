# Goto

│ **[English (en)](<../en/Goto.md>)** │  **русский (ru)** │

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
