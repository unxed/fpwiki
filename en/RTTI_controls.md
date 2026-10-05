# RTTI controls

│ **English (en)** │  **[русский (ru)](<../ru/RTTI_controls.md>)** │

This page describes the package **RunTimeTypeInfoControls** in <lazarusdir>/components/rtticontrols/. 

RTTI controls extend many LCL controls by the ability to directly connect to published properties of classes. They greatly reduce writing boring code, by automatically loading/saving data between LCL controls and published properties. In fact, you can create fully functional forms without writing any code at all. 

In combination with [Streaming components](<Streaming_components.md> "Streaming components") you can reduce the amount of code needed for connecting the program Data with the GUI and the Disk/Network to a minimum. 

## Testing Components

The RTTI controls are very useful when writing/testing a component. 

  * Drop the component onto a form
  * Drop an RTTI control onto the form
  * Set the RTTI component's **Link.TIObject** property to your component
  * Set the RTTI component's **Link.TIPropertyName** (from the drop-down list) to the property
  * You can now edit the property interactively and see the effects of your changes at runtime (and sometimes designtime; depending on the component)
  * Each RTTI control will try to invoke the appropriate property editor for your chosen property.



So for instance, if the property is a StringList type, the RTTIButton would bring up the StringList editor, the RTTIMemo would display it - all without writing a line of code! 

## See Also

  * [explanation on RTTI for delphi that can apply to Free Pascal too](<http://www.blong.com/Conferences/BorConUK98/DelphiRTTI/CB140.htm>)
  * [Runtime Type Information (RTTI)](<Runtime_Type_Information_\(RTTI\).md> "Runtime Type Information \(RTTI\)")
  * [RTTI tab](<RTTI_tab.md> "RTTI tab")
  * [explanation on RTTI controls in Lazarus](<https://www.freepascal.org/~michael/articles/#rtti>)

---

_Source: [https://wiki.freepascal.org/RTTI_controls](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/RTTI_controls)_
