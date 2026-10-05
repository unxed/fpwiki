# IDE Window: Object Inspector

│ **[Deutsch (de)](</IDE_Window:_Object_Inspector/de> "IDE Window: Object Inspector/de")** │  **English (en)** │  **[suomi (fi)](</IDE_Window:_Object_Inspector/fi> "IDE Window: Object Inspector/fi")** │  **[日本語 (ja)](</IDE_Window:_Object_Inspector/ja> "IDE Window: Object Inspector/ja")** │    
****  
****

## Contents

  * 1 Object Inspector
  * 2 Keyboard Shortcuts
  * 3 Object Inspector Popup Menu
    * 3.1 Set to default
    * 3.2 Add to Favorites
    * 3.3 View restricted properties
    * 3.4 Undo
    * 3.5 Jump to declaration of...
    * 3.6 Show Component Tree
    * 3.7 Show Property Filter
    * 3.8 Show hints
    * 3.9 Show Information Box
    * 3.10 Show Status Bar
    * 3.11 Options



# Object Inspector

[![ObjectInspectormacOS.png](https://wiki.freepascal.org/images/b/ba/ObjectInspectormacOS.png)](</File:ObjectInspectormacOS.png>)

The "Object Inspector" window allows you to explore and edit the properties of objects. Objects here are TPersistent descendants like a TForm or a TButton or a TOpenDialog. 

On the top there is a tree showing the components of the currently edited designer form (or datamodule or frame or whatever). 

The biggest part of the Object Inspector are the 3 property grids called 'Properties', 'Events' and 'Favorites'. Each property grid show a subset of the published properties of the selected components. 

The Object Inspector is a useful tool for debugging your own programs. How to add the Object Inspector to your own programs is demonstrated in examples/objectinspector. 

# Keyboard Shortcuts

Shortcut | Description   
---|---  
Up, Down | Navigate rows   
Alt + Left | Collapse level   
Alt + Right | Expand level   
Ctrl + Enter | Cycle enumerated values   
  
You can use keyboard for fast Property Name search and edit. 

Let's assume you want to modify Form's Caption and Width properties 
    
    
    - Click Form.
    - Go to Object inspector (caret is on Property Value column).
    - Press Tab to switch to Property Name column (caret will disappear from Value column).
    - Type "caption". Property row "Caption" will be found.
    - Press Tab or Enter to go to Property Value and edit it.
    
    
    
    - Press Tab to switch to Property Name column again.
    - Type "width". Width row will be found.
    - Press Tab or Enter and edit it.
    

If you misspelled property name you can press Esc and retype it. Or you can use Backspace to remove few last chars. 

# Object Inspector Popup Menu

[![ObjectInspectorPopupMenu.png](https://wiki.freepascal.org/images/6/65/ObjectInspectorPopupMenu.png)](</File:ObjectInspectorPopupMenu.png>)

Right-click in the Properties tab of the Object Inspector to get the Object Inspector's Popup Menu. The items on the Popup Menu are described below. 

## Set to default

Set the property value to the default value. For example: 
    
    
     property Position: TPosition read FPosition write SetPosition default poDesigned;
    

Here poDesigned is the default. 

Note: Not every property has a default value. Default values defined by the **stored** functions are ignored by this function. 

## Add to Favorites

Add the property to the Favorites tab of the Object Inspector. 

## View restricted properties

Opens the [Restriction Browser](<IDE_Window__Restriction_Browser.md> "IDE Window: Restriction Browser"). 

## Undo

Discard changes to the property edit field and restore the value of the current property. Do not confuse this with the value from the disk. 

## Jump to declaration of...

Jump to the source declaration of the property. This will be the last one. For example TForm.Left is published in the class TForm and defined in TControl. You can jump easily to the other declaration, by doing a find declaration on the name again and again, until you are the lowest definition (Alt+Up or Ctrl+LeftClick). You can jump back with Ctrl+H. 

## Show Component Tree

Show or hide the component tree at the top of the Object Inspector. 

## Show Property Filter

Show or hide the Properties (filter) edit field above the Properties tab of the Object Inspector. 

## Show hints

Enable or disable the showing of hints. Hints are shown, when the mouse stops for a few seconds over a property. 

## Show Information Box

Enable or disable the showing of the yellow background Information Box at the bottom of the Object Inspector. The Information Box describes the currently highlighted property and may contain links to related properties and the package to which the component belongs. If the box is not big enough to show the full description, it can be scrolled. 

## Show Status Bar

Enable or disable the showing of the Status Bar on the last line at the bottom of the Object Inspector. 

## Options

Configure some options of the Object Inspector including the colours used.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Object_Inspector](https://web.archive.org/web/20240917063132/https://wiki.freepascal.org/IDE_Window%3A_Object_Inspector)_
