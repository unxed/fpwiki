# TAction

│ **English (en)** │  **[español (es)](</TAction/es> "TAction/es")** │    
****

A **TAction** object is a container for specific action-related topics like events, description, help-topic, icon, shortcut(s). When using TActions in the Action-property of buttons, menus, dialogs, controls it is possible to centralize the effects of mouse-clicks, menu-choices, dialog-selections, shortcuts etc. in a single event handler. 

## Example

We want to have a TAction that handles opening of some other form. Make sure to have a [TActionList](<TActionList.md> "TActionList") on the main form. Doubleclick the TActionList to get the [ActionList Editor](</index.php?title=ActionList_Editor&action=edit&redlink=1> "ActionList Editor \(page does not exist\)"). Create a new action by hitting the plus-sign. In the [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"), set the name to actSomeAction, set a desired shortcut, a caption to be uses in menu's, an imageindex if a [TImageList](<TImageList.md> "TImageList") is connected to the parent ActionList, Hint is hints are to be displayed, helpkeyword and other properties where relevant. 

The most important is the OnExecute-event that will be executed if the action is triggered somehow (menu, shortcut, button). 
    
    
    procedure TMyForm.actSomeActionExecute(Sender: TObject);
    var
      f: TSomeForm;
      rv: integer;
    begin
      f := TSomeForm.Create( nil );
      f.Caption := 'SomeForm';
      rv := f.ShowModal();
      if rv=mrOk then
        DoSomethingMeaningful();  
      f.Free();
    end;
    

If you use the newly created action in a [TMenuItem](</index.php?title=TMenuItem&action=edit&redlink=1> "TMenuItem \(page does not exist\)") of a [TMainMenu](<TMainMenu.md> "TMainMenu") or [TPopupMenu](<TPopupMenu.md> "TPopupMenu") you are ready to go. 

## See also

  * [TAction documentation](<http://lazarus-ccr.sourceforge.net/docs/lcl/actnlist/taction.html> "doc:lcl/actnlist/taction.html")
  * [TActionList](<TActionList.md> "TActionList")
  * [TActionLink](</index.php?title=TActionLink&action=edit&redlink=1> "TActionLink \(page does not exist\)")
  * [TActionListEnumerator](</index.php?title=TActionListEnumerator&action=edit&redlink=1> "TActionListEnumerator \(page does not exist\)")
  * [TContainedAction](</index.php?title=TContainedAction&action=edit&redlink=1> "TContainedAction \(page does not exist\)")
  * [TCustomAction](</index.php?title=TCustomAction&action=edit&redlink=1> "TCustomAction \(page does not exist\)")
  * [TCustomActionList](</index.php?title=TCustomActionList&action=edit&redlink=1> "TCustomActionList \(page does not exist\)")
  * [TShortCutList](</index.php?title=TShortCutList&action=edit&redlink=1> "TShortCutList \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/TAction](https://web.archive.org/web/20250513175310/https://wiki.freepascal.org/TAction)_
