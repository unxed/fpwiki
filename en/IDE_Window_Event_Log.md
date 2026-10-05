# IDE Window: Event Log

│ **English (en)** │

## Contents

  * 1 Navigation
  * 2 Important
  * 3 Event Log
    * 3.1 Context menu



## Navigation

[Main Menu](<Main_menu.md> "Main menu") > [View](<Main_menu.md> "Main menu") > Debug Windows > Event Log 

## Important

You must [setup the debugger](<Debugger_Setup.md> "Debugger Setup") and start the project to debug it. Only then this window will be useful. 

## Event Log

[![Event Log.png](https://wiki.freepascal.org/images/9/98/Event_Log.png)](</File:Event_Log.png>)

The Event Log provides a place into which the debugger will log a record for the occurrence of various events. You can configure the Event Log by using its context menu or the Debugger page of the Tools, Environment Options dialog box. 

The types of events that are logged include process information such as process start, process stop, and module load debugger breakpoints, as well as windows messages sent to the application. 

Application output using the [OutputDebugString()](<OutputDebugString.md> "OutputDebugString") function provides a handy means to help you debug Windows applications. The single parameter to OutputDebugString(aString) is a string which will be added to the Event Log. This allows you to keep track of variable values or similar debug information without having to use [Watches](<IDE_Window__Watch_list.md> "IDE Window: Watch list") or displaying intrusive debug dialog boxes. 

[TCustomApplication.Log()](<https://www.freepascal.org/docs-html/current/fcl/custapp/tcustomapplication.log.html>) also writes its output to the Event Log (if it has been implemented). 

### Context menu

[![Event Log popup.png](https://wiki.freepascal.org/images/a/a1/Event_Log_popup.png)](</File:Event_Log_popup.png>)

**Clear Events** : Clears all the events from the Events Log window. 

**Save Events to File** : Enables you to save the contents of the Event Log to a file. 

**Add Comment** : Enables you to add a comment to the Event Log. 

[**Event Log Options**](<IDE_Window__Debugger_Options.md> "IDE Window: Debugger Options"): Options to configure the events which appear in the Event Log.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3AEvent_Log](https://web.archive.org/web/20230330140347/https://wiki.freepascal.org/IDE_Window%3AEvent_Log)_
