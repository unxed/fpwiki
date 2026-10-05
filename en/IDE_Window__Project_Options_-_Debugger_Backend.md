# IDE Window: Project Options - Debugger Backend

## Contents

  * 1 Project Options => Debugger
  * 2 Select a debugger backend for the project
  * 3 Configure project specific debugger backends
    * 3.1 Session or LPI storage
    * 3.2 Backend Config



# Project Options => Debugger

# Select a debugger backend for the project

  * _Use Project Debugger_ (Default) - Falls back to use the IDE default setting.



    Uses the project specific debugger backend, as set in the lower part of the page.
    If none exists, the IDE default debugger backend will be used

  * _Use IDE default debugger_



    Ignores any project settings.

  * >> _Named entries from IDE Setting_ <<



    Uses the selected setting from the IDE

There is no list of "named entries of project specific debugger backend". The project specific debugger backend can be selected in the toolbar below. 

This setting is stored in the project session. 

# Configure project specific debugger backends

## Session or LPI storage

The location to store the project specific debugger backends. 

The setting itself (checked/unchecked) is stored in the LPI. 

If the setting is changed while the IDE is open, the stored config will be moved. If the setting is changed outside the IDE (e.g. shared project) then the config will be loaded according to the setting. Any config stored in the non selected storage is ignored. 

## Backend Config

See [IDE_Window:_DebuggerClassOptionsFrame](<IDE_Window__DebuggerClassOptionsFrame.md> "IDE Window: DebuggerClassOptionsFrame")

  
  
****  
****

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Project_Options_-_Debugger_Backend](https://web.archive.org/web/20250217110712/https://wiki.freepascal.org/IDE_Window%3A_Project_Options_-_Debugger_Backend)_
