# Edit context help for the IDE windows

* Open an IDE dialog (in the running IDE, not in the designer)
  * Focus a control that should get help. To setup the context help of a groupbox, frame, focus one of its childs.
  * Press `Ctrl`+`⇧ Shift`+`F1`



## Example

  * open the options dialog
  * select the frame Editor / Mouse
  * focus the tree on the frame
  * press `Ctrl`+`⇧ Shift`+`F1` to open the dialog:

[![EditContextHelpExample1.png](https://wiki.freepascal.org/images/4/49/EditContextHelpExample1.png)](</File:EditContextHelpExample1.png>)

The left tree shows all components of the current dialog. They are just for orientation. The right tree shows all existing help nodes. You will notice that they have the same names as the dialog or some controls on a dialog 

The deepest help node wins. 

The focused control - in this example the ContextTree is selected on both tree. 

  * to create the help for the whole frame, select on the **right** tree the **EditorMouseOptionsFrame** as shown in the screen shot above.
  * check _Has Help_
  * because EditorMouseOptionsFrame has a wiki page of its own, check 'Is a root control'. This will shorten the text in the third field.
  * change the **Path** to _Editor_Mouse_Options_ as shown in the screen shot above.
  * focus the _Name_. You will notice that the text field has changed to _IDE_Window:_Editor_Mouse_Options_
  * Now click on the **Test** button on the left side.
  * the IDE will open the URL: _<http://wiki.lazarus.freepascal.org/IDE_Window:_Editor_Mouse_Options>_
  * click Ok to save the changes. This will alter the file _docs/IDEWindowHelpTree.xml_.
  * commit your changes

---

_Source: [https://wiki.freepascal.org/Edit_context_help_for_the_IDE_windows](https://web.archive.org/web/20231129022150/https://wiki.freepascal.org/Edit_context_help_for_the_IDE_windows)_
