# Testing, if form exists

│ **[Deutsch (de)](</Testing,_if_form_exists/de> "Testing, if form exists/de")** │  **English (en)** │  **[français (fr)](</Testing,_if_form_exists/fr> "Testing, if form exists/fr")** │    
****

Sometimes a [form](<TForm.md> "TForm") may be launched from several places in a program. If it already exists, it only needs to be brought to the front. If not, it needs to be created. 

This method is only needed if the form is not [ auto created](<Form_Tutorial.md> "Form Tutorial") (ie not listed under Project|Project Options|Forms|Available forms). 

The easiest way is: 
    
    
    if (MyForm = nil) then Application.CreateForm(TMyForm, MyForm);
    MyForm.Show;
    

Use `CloseAction := caFree` in the form's OnClose event, like so: 
    
    
    procedure TMyForm.FormClose(Sender: Tobject; var Closeaction: Tcloseaction);
    begin
      CloseAction := caFree;
      MyForm := nil;
    End;
    

This method is taken from forum discussions. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This method has some limitations:   
  


  * **Not** _more than one instance_ of that form class should exist at any time.
  * Reference to any created form of that class should be stored in _a single global_ variable.

---

_Source: [https://wiki.freepascal.org/Testing%2C_if_form_exists](https://web.archive.org/web/20241206084450/https://wiki.freepascal.org/Testing%2C_if_form_exists)_
