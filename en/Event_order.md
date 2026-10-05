# Event order

│ **[Deutsch (de)](</Event_order/de> "Event order/de")** │  **English (en)** │  **[suomi (fi)](</Event_order/fi> "Event order/fi")** │    
****

## Contents

  * 1 Overview
  * 2 Lazarus documentation
  * 3 Forms
    * 3.1 Form.OnCreate
    * 3.2 Form.OnShow
    * 3.3 Form.OnActivate
    * 3.4 Form.OnDeactivate
    * 3.5 Form.OnDestroy
  * 4 General controls
    * 4.1 OnEditingDone
  * 5 Applicable Delphi information
  * 6 See also
  * 7 External links



## Overview

Lazarus offers various events that you can use to enter your own procedures to handle things that happen in your application (e.g. a user clicks a button). 

There are rather a lot of possible events to cater for a lot of different scenarios. Somebody who does not know Lazarus or Delphi could well pick the wrong event. 

## Lazarus documentation

The [Lazarus tutorial](<Lazarus_Tutorial.md> "Lazarus Tutorial") has some information on what events do what. 

## [Forms](<TForm.md> "TForm")

Typical event order for [forms](<TForm.md> "TForm") is: 
    
    
    OnCreate => OnShow => OnActivate => OnPaint => OnResize => OnPaint => ... 
    OnCloseQuery => OnClose => OnDeactivate => OnHide => OnDestroy
    

### Form.OnCreate

This is the equivalent to the class constructor in forms. Use this to initialize form-level variables etc. The form is not yet shown at this time. 

### Form.OnShow

When the form is shown (e.g. when loading the form or setting its `.Visible` property to true), this event is fired - just before the form is visible. This allows you to modify the visual appearance of controls (e.g. disable certain controls) without flickering. 

### Form.OnActivate

This occurs after Form.Show.  
At the OnActivate stage controls instantiated in OnCreate are present and correct, and should have the minimum needed properties set (such as Parent) plus anything else OnCreate specified.  
OnActivate signals that this form now has focus, so mouse/key events will start arriving. 

### Form.OnDeactivate

Fired after the form loses focus.   


![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Switching between different applications in the OS will not cause either Form.OnActivate or Form.OnDeactivate to fire. Thus Form.OnActivate/Form.OnDeactivate only track different forms changing focus within an application.  


To track whether the Application _itself_ receives or loses focus, use Application.OnActivate and Application.OnDeactivate.

### Form.OnDestroy

The equivalent to a class destructor in forms. Use this to clean up/free variables.  
When the main form of an application is destroyed, the application terminates. 

## General controls

These events apply to various controls. Please use the Object Inspector to check if it is available for the control you're currently using. 

### OnEditingDone

The OnEditingDone event for controls can act like the Validate event in other programming languages: it indicates the user is done changing the control and intends to keep that value. The program can now check the control content for correctness, show error messages, update database fields, etc. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** As soon as you click outside the control (even on a control that cannot receive focus) OnEditingDone is triggered.  


(See TControl.MouseDown procedure, introduced in r11778).

## Applicable Delphi information

Because the implementation of events in Lazarus and Delphi is similar, a lot of Delphi-related documentation is applicable to Lazarus with minor modifications. 

[Delphi documentation](<http://docwiki.embarcadero.com/RADStudio/en/Events>) is often handy. You can search for the actual control you want to know more about to get a list of its events. 

If you want to know about the order in which events fire on a form, [this article from about.com](<http://delphi.about.com/od/formsdialogs/a/delphiformlife.htm>) can be useful. 

## See also

  * [Lazarus tutorial](<Lazarus_Tutorial.md> "Lazarus Tutorial")
  * [Drop files event](<Drop_files_event.md> "Drop files event")
  * [Execute action after form is shown](<Execute_action_after_form_is_shown.md> "Execute action after form is shown")



## External links

  * [Article about event order](<http://delphi.about.com/od/formsdialogs/a/delphiformlife.htm>)
  * [Delphi documentation](<http://docwiki.embarcadero.com/RADStudio/en/Events>)

---

_Source: [https://wiki.freepascal.org/Event_order](https://web.archive.org/web/20250420135503/https://wiki.freepascal.org/Event_order)_
