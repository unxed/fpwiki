# Form Tutorial

│ **English (en)** │

A short introduction on using Forms in Lazarus. 

## Contents

  * 1 What is a Form?
  * 2 The First GUI Application
  * 3 The Use of a Second Form
    * 3.1 Difference between Show and ShowModal
  * 4 Two Forms That Can Call Each Other
  * 5 Passing Variables to Other Forms
  * 6 Other Form-Related Subjects
    * 6.1 Use Another Form as the Main Form
    * 6.2 Save properties of the shape at end of program
  * 7 Generate the form dynamically
    * 7.1 Create a Lazarus designed form dynamically
    * 7.2 Creating a new form dynamically
  * 8 See also



## What is a Form?

The form (class [TForm](<TForm.md> "TForm")) represents a window or dialog box that is the user interface of an [application](<Application.md> "Application"). It is the container on which all other [components](<LCL_Components.md> "LCL Components") (e.g. buttons, labels, edit fields, images, etc.) are inserted . 

## The First GUI Application

Upon successfully installing Lazarus and starting a new Application Project, an empty form is created. Otherwise you can create a new [GUI application](<Overview_of_Free_Pascal_and_Lazarus.md> "Overview of Free Pascal and Lazarus") under Main menu -> Project -> New Project -> Application OK. 

Now you have created a new, fully functional project with a form: 

[![Form1 Designmodus.png](https://wiki.freepascal.org/images/7/73/Form1_Designmodus.png)](</File:Form1_Designmodus.png>)

To run the newly created project, you can simply press the `F9` key or click with the mouse on the main menu icon [![Start.png](https://wiki.freepascal.org/images/b/b8/Start.png)](</File:Start.png>) (or Main menu: Run -> Run). The project will be compiled and executed. 

Nothing really exciting happens. The form (Form1) changes slightly in appearance and the points from the raster grid (help for positioning of individual components) disappear (as long as the grid points are visible, you also know that it is still in design mode):  
[![Form1 Designmodus.png](https://wiki.freepascal.org/images/7/73/Form1_Designmodus.png)](</File:Form1_Designmodus.png>) -> [![Form1 Runmodus.png](https://wiki.freepascal.org/images/d/dc/Form1_Runmodus.png)](</File:Form1_Runmodus.png>)

Next, you can place a [TButton](<TButton.md> "TButton") onto the form. This will be used later to allow the opening of a second form: 

Select the TButton from the [Standard tab](<Standard_tab.md> "Standard tab") on the [Component Palette](<Component_Palette.md> "Component Palette"). 

[![TButtonStandardpalette.png](https://wiki.freepascal.org/images/4/49/TButtonStandardpalette.png)](</File:TButtonStandardpalette.png>)

and click on the form: There is now a button placed on Form1 with the name and caption "Button1". 

In order for this button to do something, you have to announce that you intend to do something when you click it. This can be regarded as a simple event. For this you need an [ event handler](<Event_order.md> "Event order") that is called after the click. The event handler for a mouse click can be quite easily reached by double-clicking on Button1 (or in the Object Inspector, select Button1 -> Events tab -> OnClick, then click on the [...] button). This creates a procedure TForm1.Button1Click in the code editor, which is always called (at [runtime](<runtime.md> "runtime"), not in design time) when Button1 is clicked: 

[![NeuesProjekt2.png](https://wiki.freepascal.org/images/9/9b/NeuesProjekt2.png)](</File:NeuesProjekt2.png>)

To make the application do something after clicking on Button1, add some code between the [Begin](<Begin.md> "Begin") and [End](<End.md> "End") of the procedure TForm1.Button1Click like this: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Caption:='My first Form';  //Caption of Form1 is changed (text in formhead)
      //or
      Button1.Caption:='Hello';  //Caption of button is changed (shown text of button)
    end;
    

Now run the form by pressing `F9`, and notice that when the button is clicked the form caption and the button caption both change. 

You can now experiment a little bit and learn how to use the standard components. To get started, I would recommend you to try samples of the following components: 

[TButton](<TButton.md> "TButton") | [![tbutton.png](https://wiki.freepascal.org/images/b/b2/tbutton.png)](</File:tbutton.png>)  
---|---  
[TLabel](<TLabel.md> "TLabel") | [![tlabel.png](https://wiki.freepascal.org/images/3/3a/tlabel.png)](</File:tlabel.png>)  
[TEdit](<TEdit.md> "TEdit") | [![tedit.png](https://wiki.freepascal.org/images/0/07/tedit.png)](</File:tedit.png>)  
[TMemo](<TMemo.md> "TMemo") | [![tmemo.png](https://wiki.freepascal.org/images/f/f3/tmemo.png)](</File:tmemo.png>)  
[TToggleBox](<TToggleBox.md> "TToggleBox") | [![ttogglebox.png](https://wiki.freepascal.org/images/5/5e/ttogglebox.png)](</File:ttogglebox.png>)  
[TCheckBox](<TCheckBox.md> "TCheckBox") | [![tcheckbox.png](https://wiki.freepascal.org/images/3/3c/tcheckbox.png)](</File:tcheckbox.png>)  
[TRadioButton](<TRadioButton.md> "TRadioButton") | [![tradiobutton.png](https://wiki.freepascal.org/images/e/ef/tradiobutton.png)](</File:tradiobutton.png>)  
[TListBox](<TListBox.md> "TListBox") | [![tlistbox.png](https://wiki.freepascal.org/images/7/7f/tlistbox.png)](</File:tlistbox.png>)  
[TComboBox](<TComboBox.md> "TComboBox") | [![tcombobox.png](https://wiki.freepascal.org/images/b/be/tcombobox.png)](</File:tcombobox.png>)  
[TScrollBar](<TScrollBar.md> "TScrollBar") | [![tscrollbar.png](https://wiki.freepascal.org/images/0/05/tscrollbar.png)](</File:tscrollbar.png>)  
  
There is additional helpful information in the [Lazarus Tutorial](<Lazarus_Tutorial.md> "Lazarus Tutorial"). 

## The Use of a Second Form

The tutorial shows how to use multiple forms in a project. In this example, only two forms, Form1 (main form) and Form2, are created, but the process is identical for additional forms. 

There is a main form with a button, which when clicked opens the new form. The new form also receives a button which when clicked closes the new form and returns you to the main form. 

If you have worked through the first tutorial [ The first GUI app](<Form_Tutorial.md> "Form Tutorial"), you will need to delete the code between the begin and end of procedure TForm1.Button1Click. Otherwise you will need to create a new project (application), drop a button on the form, and create the OnClick event handler for this Button1.  
Click the new button to enter a caption (visible text on the button). To do this, select the button (just click it once) and in the Object Inspector click Properties -> Caption, and enter the text "Open Form2". 

[![Objektinspektor.png](https://wiki.freepascal.org/images/5/52/Objektinspektor.png)](</File:Objektinspektor.png>)

In the Main menu click File -> New Form, to add a second form (Form2) to the project. Place a button (Button1) on Form2 and create the OnClick event handler for it. Change the caption of this button to "Close". 

Now the project has two forms, each of which can be selected and displayed with Main menu Project -> Forms (or key combination `⇧ Shift` \+ `F12`). Alternatively, you can select the tab for a Unit in the Source Editor, then use `F12` to switch between the code editor and form designer for the form of the associated [Unit](<Unit.md> "Unit"). 

In the [source code editor](<source_code_editor.md> "source code editor"), go to the Form1 associated [Unit](<Unit.md> "Unit") (Unit1) and add "Unit2" to the [Uses](<Uses.md> "Uses") clause: 
    
    
    uses
      Classes, SysUtils, ... , Unit2;  // It is important that the individual units are separated by a comma(,)
                                       // and the uses clause ends with a semicolon(;).
    

Now you can call Unit2 (and thus Form2) from Unit1. 

Next, edit the OnClick event of the button belonging to Form1: 
    
    
    unit Unit1;
    ...
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Form2.ShowModal;  //Displays Form2, the focus is placed on Form2.
    end;
    
    
    
    unit Unit2;
    ...
    procedure TForm2.Button1Click(Sender: TObject);
    begin
      Close;  //Close Form2.
    end;
    

Now you can start the project ([![Start.png](https://wiki.freepascal.org/images/b/b8/Start.png)](</File:Start.png>) or `F9`) and open Form2 by clicking the button on Form1. 

### Difference between Show and ShowModal

Both methods _Show_ and _ShowModal_ make a form visible. To see the difference between _Show_ and _ShowModal_ : 

  * Extend the previous example [The use of a second form](<Form_Tutorial.md> "Form Tutorial") by adding a second TButton to _Form1_.
  * Select _Button1_ and change the properties _Name_ to _btnShow_ and _Caption_ to _Show_.
  * Select _Button2_ and change the properties _Name_ to _btnShowModal_ and _Caption_ to _ShowModal_.
  * Create (or modify) the _OnClick_ event handler of the two buttons as follows:


    
    
    procedure TForm1.btnShowClick(Sender: TObject);
    begin
      Form2.Show;
      ShowMessage('Form2 is opened!');
    end;
    
    procedure TForm1.btnShowModalClick(Sender: TObject);
    begin
      Form2.ShowModal;
      ShowMessage('Form2 is opened and closed again!');
    end;
    

  * Start and watch the difference!



Show
    

This procedure makes a form visible and continues to execute the code of the calling control (_Form1_). In our example, the message ([ShowMessage](<Dialog_Examples.md> "Dialog Examples")) appears almost simultaneously with _Form2_. You can also continue to use the calling form _Form1_. For example, it is possible to move it, or click the _Show_ button again (but clicking _ShowModal_ will cause an error). 

Instead of using **`<myForm>.Show;`**, it is also possible to use **`<myForm>.Visible:=True;`**. The method _Show_ does the following: 
    
    
    procedure TCustomForm.Show;
    begin
      Visible := True;
      BringToFront;
    end;
    

The only difference between **`<myForm>.Show;`**, and **`<myForm>.Visible:=True;`** is that the form displayed using _Show_ will be on top of all other forms currently displayed. 

ShowModal
    

_ShowModal_ halts the processing of code in the calling control (_Form1_) until the newly opened form (_Form2_) has been closed. In the example, the _ShowMessage_ line immediately following _ShowModal_ is executed only after _Form2_ is closed. The calling form is frozen: You can neither move _Form1_ nor click any of its buttons. 

ShowModal is a function that returns an integer result, see [TModalResult](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tmodalresult.html> "doc:lcl/forms/tmodalresult.html"). The returned value allows branching based on how the form was closed. 

## Two Forms That Can Call Each Other

It is generally good program design to avoid calling the main form from a second form. It is better to return to the main form by closing the second form (as in the previous example) and letting focus revert to the main form. It is, however, possible to call the main from from a sub-form, as this example will show. 

In order for Form1 to call Form2, Unit1 must have Unit2 in its Uses clause. Conversely, for Form2 to call Form1, Unit2 must have Unit1 in its Uses clause. This leads to a potential Circular Unit Reference compiler error, with each Unit referring to the other. This example also shows how to avoid the Circular Unit Reference error. 

To set up this example, either modify the forms from the previous example ([The Use of a Second Form](<Form_Tutorial.md> "Form Tutorial")), or create a new project with two forms, each having a single button. Set the caption of Form1.Button1 to "Open Form2", and set the caption of Form2.Button1 to "Open Form1". 

If you are modifying the previous example, remove "Unit2" from the Uses clause in Unit1. 

Add a Uses clause to the [Implementation](<Implementation.md> "Implementation") section of each [Unit](<Unit.md> "Unit"). Add Unit2 to the Unit1 clause, and add Unit1 to the Unit2 clause, as below: 
    
    
    unit Unit1;
    ...
    implementation
    
    uses
      Unit2;  
    ...
    
    
    
    unit Unit2;
    ...
    implementation
    
    uses
      Unit1;  
    ...
    

Now change the code located behind the OnClick event of the button event handler: 
    
    
    unit Unit1;
    ...
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Form2.Show;  
    end;
    
    
    
    unit Unit2;
    ...
    procedure TForm2.Button1Click(Sender: TObject);
    begin
      Form1.Show; 
    end;
    

## Passing Variables to Other Forms

In accordance with the above example [ The Use of a Second Form](<Form_Tutorial.md> "Form Tutorial"), it is possible to declare a [ global variable](<Global_variables.md> "Global variables") in the interface part of Unit2. This would allow access in both Unit1 and Unit2 to one and the same [ variable](<Variable.md> "Variable") (This is because by putting Unit2 in the Uses clause of Unit1, all variables declared in the interface part of Unit2 are also available in Unit1). This practice should be limited to a minimum, as it quickly becomes difficult to remember which individual variables are in scope, potentially increasing errors. It is better to use [local variables](<Local_variables.md> "Local variables"), define a [ Property](<Object_Oriented_Programming_with_Free_Pascal_and_Lazarus.md> "Object Oriented Programming with Free Pascal and Lazarus") in a [Class](<Class.md> "Class") or, alternatively, as a variable in a class. 

In the next project, a second form is opened by clicking on the button. In this case, in the unit of the main form is counted how often the second shape has been shown. In this second form you can ask to see by clicking on the button, how often it has been opened: 

  * a new project with two forms, each a button on it and their OnClick events to evaluate.
  * now, in the public part of the [Class](<Class.md> "Class") TForm2 (Unit2) a variable (in this case a [ constant](<Const.md> "Const"), which we defined as variable abuse) of type integer with the name Count create:


    
    
    unit Unit2;
    ...
      TForm2 = class(TForm)
        Button1: TButton;
        procedure Button1Click(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
        const Count: Integer = 0;   //here
      end;
    

  * now even customize the Button event handler accordingly:


    
    
    unit Unit1;
    ...
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      inc(Form2.Count);  //Increase the counter by 1 on each call to Form2
      Form2.ShowModal;   
    end;
    
    
    
    unit Unit2;
    ...
    procedure TForm2.Button1Click(Sender: TObject);
    begin
      ShowMessage('Form2 would be displayed '+IntToStr(Count)+' times');
    end;
    

In this way you can in Unit1 access all public variables/properties/functions/procedures (general methods) of Unit2. 

## Other Form-Related Subjects

### Use Another Form as the Main Form

If you determine after a while that you want to have displayed rather a different form or new form for the view at startup, you can under the main menu Project -> Project Options -> Forms: 

[![Projekteinstellungen Formulare.png](https://wiki.freepascal.org/images/7/76/Projekteinstellungen_Formulare.png)](</File:Projekteinstellungen_Formulare.png>)

Alternatively, you can display, in Mainmenu -> Project -> Show .lpr file, that Project.lpr (Pascal code of the main program) and create the first form to be displayed as the first: 
    
    
    program Project1;
    ...
      Application.CreateForm(TForm2, Form2);  //Form2 is created in first and shown when the application starts
      Application.CreateForm(TForm1, Form1);
    

### Save properties of the shape at end of program

  * [Remember form position and size#Using_TForm.SessionProperties and TXMLPropStorage](<Remember_form_position_and_size.md> "Remember form position and size")



## Generate the form dynamically

### Create a Lazarus designed form dynamically

You do not have to create all the forms that may be called during the term of an application at startup. Some developers generally keep none of it and delete the code automatically created when you insert a new form in a project from the Projekt.lpr out immediately. If you want to write a library that contains a couple of GUIs, you can not get around the dynamic creation of forms. 

An example: 

  * new application with two forms, Form1 a button (add "Unit2" in the uses clause of unit1)
  * open the Projekt.lpr (Project -> lpr file.)
  * delete the line "Application.CreateForm(TForm2, Form2);"
  * in the OnClick event of Button1, Form1 add following code:


    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      Form2:=TForm2.Create(Nil);  //Form2 is created
      Form2.ShowModal;            //Form2 is displayed
      FreeAndNil(Form2);          //Free Form2 
    end;
    

  * now simply start



### Creating a new form dynamically

The following example will demonstrate how new forms can be generated in code, without using the Form Designer,. 

In this example clicking a button should open another form that itself contains a button. Clicking this second button makes a message appear warning that the form will close; then the form is closed. 

  * Create a new application with a form and a button
  * In the OnClick event of Button1, in Form1, add the following code:


    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      MyForm: TForm;
      MyButton: TButton;
    begin
      MyForm:=TForm.Create(nil);             
      MyForm.SetBounds(100, 100, 220, 150);  
      MyForm.Caption:='My dynamic created form';
    
      MyButton:=TButton.create(MyForm);  
      MyButton.Caption:='Close my form';
      MyButton.SetBounds(10, 10, 200, 30);
      MyButton.Parent:=MyForm;
    
      MyButton.OnClick:=@MyButtonClick;     
    
      MyForm.ShowModal;                     
    
      FreeAndNil(MyForm);     
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If a component has been created with an owner (TButton.Create(Owner: TComponent)), it belongs to the owner, who is then responsible to free the component. Thus MyButton is freed automatically by freeing MyForm.

  * now the event handler for MyButton.OnClick must be created. To this end, write in the private section of class TForm1 the following procedure declaration:


    
    
    TForm1 = class(TForm)
    ...
    private
      { private declarations }
      procedure MyButtonClick(Sender: TObject);
    

  * press `Ctrl]`+`⇧ Shift`+`C` (code completion) and insert the following code as the handler implementation:


    
    
    procedure TForm1.MyButtonClick(Sender: TObject);
    begin
      Showmessage('Attention, closing my dynamically created form!');
      if Sender is TButton then
        TForm(TButton(Sender).Parent).Close;
    end;
    

  * now the project can be compiled and executed



## See also

  * [Tips for manual generation of controls](<LCL_Tips.md> "LCL Tips")
  * [Example to create a complete GUI application by code](<LCL_Tips.md> "LCL Tips")
  * [Testing, if form exists](<Testing,_if_form_exists.md> "Testing, if form exists")
  * Canvas draw vertical line on form/de
  * [Drawing with canvas](<Drawing_with_canvas.md> "Drawing with canvas") on form
  * [user should not be able to close form](<user_should_not_be_able_to_close_form.md> "user should not be able to close form")
  * [Creating a non-rectangular window or control](<LCL_Tips.md> "LCL Tips")
  * [Remember form position and size](<Remember_form_position_and_size.md> "Remember form position and size")
  * [Form in DLL](<Form_in_DLL.md> "Form in DLL")

---

_Source: [https://wiki.freepascal.org/Form_Tutorial](https://web.archive.org/web/20250123133136/https://wiki.freepascal.org/Form_Tutorial)_
