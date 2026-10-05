# user should not be able to close form

│ **[Deutsch (de)](</user_should_not_be_able_to_close_form/de> "user should not be able to close form/de")** │  **English (en)** │  **[français (fr)](</user_should_not_be_able_to_close_form/fr> "user should not be able to close form/fr")** │    
****

You can use the OnCloseQuery event of TForm to be notified when a form is about to be closed. To program such an event handler for a form, select the form in the Form Designer and click on the Events tab of the Object Inspector. Then double-click on the OnCloseQuery event (it is on row 9 of the listed events). The Lazarus IDE will generate a new skeleton event handler for you to complete, and the focus will move to the Source Editor where you can type appropriate code. Your code should set the value of the var parameter CanClose which is passed when the OnCloseQuery event is called. Setting CanClose to False prevents the form from closing. Setting CanClose to True allows the form to close. 

For example, here is an over-simple example which prevents a form closing until an edit field on the form (named Edit1) has been at least partly filled: 

Example: 
    
    
    uses
      Forms, ...;
      
      ...
      
    procedure TForm1.FormCloseQuery(Sender: TObject; var CanClose: Boolean);
    begin 
      CanClose := (Length(Edit1.Text) > 0); 
    end;
      
      ...

---

_Source: [https://wiki.freepascal.org/user_should_not_be_able_to_close_form](https://web.archive.org/web/20240917061002/https://wiki.freepascal.org/user_should_not_be_able_to_close_form)_
