# TTIComboBox

RTTI controls link to other components' published property, and changes in the either control are reflected to the other without any coding. TTIComboBox allows users to select any predefined item. 

As a simple example, let's create new form, and drop one TEdit (name=Edit1) and one TTIComboBox (name=TICombobox1) on the form. And then open TIComboBox1's Link property. Select Edit1 in the TIObject drop-down list, and Text in tht TIProperty dropdown list. 

As with TCombobox, TTIComboBox needs items to drop down. This is not provided in the object inspector. Users have to hard code them. It is necessary to define it before running. We'll put it in the OnCreate event of the form. 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
       with TIComboBox1.Items do begin
          Add('Sunday');
          Add('Monday');
          Add('Tuesday');
          Add('Wednesday');
          Add('Thursday');
          Add('Friday');
          Add('Saturday');
       end;
    end;
    

Now run the program, and change selection in the TIComboBox1. You will see that the text in the Edit1 changes as well. And if you type in any text in the Edit1, the text will be displayed in the TIComboBox1 as well. 

  
TTIComboBox can provide other value (alias values) that is different from displayed its text box. Now add to FromCreate event handler as follows. 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
       with TIComboBox1.Items do begin
          Add('Sunday');
          Add('Monday');
          Add('Tuesday');
          Add('Wednesday');
          Add('Thursday');
          Add('Friday');
          Add('Saturday');
       end;
       with TIComboBox1.Link.AliasValues do begin
          Add('0=Sunday');
          Add('1=Monday');
          Add('2=Tuesday');
          Add('3=Wednesday');
          Add('4=Thursday');
          Add('5=Friday');
          Add('6=Saturday');
       end;
    end;
    

And then run the program again. Now, you'll see Sunday, Monday, Tuesday, etc. in the TIComboBox1. But if Mondayh is selected, then Edit1 will show 1, instead of "Monday". The alias value of Monday, which is 1, is displayed. If you type in 3 in the Edit1, then TIComboBox1 will show Wednesday.

---

_Source: [https://wiki.freepascal.org/TTIComboBox](https://web.archive.org/web/20230327103236/https://wiki.freepascal.org/TTIComboBox)_
