# LazReport Tutorial

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Starting with LazReport
    * 2.1 Setting up your form
    * 2.2 Setting up variables
    * 2.3 Objects and bands
    * 2.4 Report title
    * 2.5 The main event: showing data in the master data band
      * 2.5.1 Formatting output
      * 2.5.2 A picture is worth...
    * 2.6 Detail data
    * 2.7 Loading the report from the form
    * 2.8 Printing
    * 2.9 Report options
    * 2.10 General purpose variables
  * 3 See also



## Overview

This page started as a translation of the Portuguese language Tutorial de LazReport (which itself appears to be a translation from a French document). It was updated for LazReport (instead of FastReport) on Lazarus (instead of Delphi), using T*Connection instead of a DBase table. 

## Starting with LazReport

We're going to use a demo database which is provided by Delphi (DBDEMOS). You'll need to create a new application with a main form _Form1_. 

We're assuming you already have your database set up as specified in [SQLdb Tutorial0](<SQLdb_Tutorial0.md> "SQLdb Tutorial0"). If not, please do so. 

### Setting up your form

Place the following components on the form: 

  * A T*Connection, e.g. TIBConnection: 
    * DatabaseName := your database, e.g. C:\Program Files\Firebird\Firebird_2_5\examples\empbuild\EMPLOYEE.FDB
    * UserName, Password, HostName: as appropriatie (e.g. SYSDBA,password,<none> for Firebird embedded)
    * Transaction: TSQLTransaction1
  * TSQLTransaction
  * TSQLQuery: 
    * Database := IBConnection1
    * Active: true
    * SQL: select * from employee
  * TDataSource 
    * DataSource1.DataSet := SQLQuery1
  * TfrDBDataSet 
    * frDbDataSet.DataSource := DataSource1
  * TfrReport
  * TfrDesigner
  * 2 TButton
  * Button1.Caption := 'Editing'
  * Button2.Caption := 'Preview'



This results in: 

[![lazreptut01.png](https://wiki.freepascal.org/images/0/01/lazreptut01.png)](</File:lazreptut01.png>)

Double-click on the **TfrReport** icon (or right-click, Design Report) to bring up the report designer: 

[![lazreptut01b.png](https://wiki.freepascal.org/images/f/f8/lazreptut01b.png)](</File:lazreptut01b.png>)

### Setting up variables

LazReport lets you link variable names to database fields or system values, in order to spare your end users from having to decipher cryptic field names. In our example, we're going to create a variable for our database fields FIRST_NAME, LAST_NAME etc. 

In the File menu, click **Variables list**. Now you get an overview of the defined variables: 

[![lazreptutvariableseditor.png](https://wiki.freepascal.org/images/2/21/lazreptutvariableseditor.png)](</File:lazreptutvariableseditor.png>)

Click on the button **Variables** in order to define some variables. Write the contents according ot the following image. "Demo Variables" represents a category and the other entries (xxx field) represent variables. To differentiate the two, the variables start with a space. 

**Note**. The distinction between variables and variables categories by starting variables with a space is needed unique and exclusively in this dialog (**Variables List**). Anywhere else (Report Designer, main source code and scripts) a variable should be invoked without spaces. 

[![lazreptutdefinevariables.png](https://wiki.freepascal.org/images/2/2c/lazreptutdefinevariables.png)](</File:lazreptutdefinevariables.png>)

Finally click on OK to confirm your entries, and you'll see this: 

[![lazreptutvariablesdefined.png](https://wiki.freepascal.org/images/d/d6/lazreptutvariablesdefined.png)](</File:lazreptutvariablesdefined.png>)

As you can see, the category we defined is shown in the ComboBox above the variables. Each variable must be linked with a field. To do this, select the variable and then the field. Do this for all variables; the screenshot shows the link for Hire date: 

[![lazreptutvarhiredatetofield.png](https://wiki.freepascal.org/images/8/83/lazreptutvarhiredatetofield.png)](</File:lazreptutvarhiredatetofield.png>)

### Objects and bands

After closing the variables editor, we're back in our report designer. You may have noticed a small number of objects in the object bar to the left of the report surface. These are: 

  * the mouse icon pointer
  * rectangle object
  * band
  * picture object
  * subreport
  * draw lines
  * insert barcode object



[![lazreptut01b.png](https://wiki.freepascal.org/images/f/f8/lazreptut01b.png)](</File:lazreptut01b.png>)

Click the band icon, go to the report surface, and click on it to drop a band object. 

[![lazreptutinsertnewband.png](https://wiki.freepascal.org/images/5/5a/lazreptutinsertnewband.png)](</File:lazreptutinsertnewband.png>)

You must click on the type of band you want to create. Select **Report title** and click OK. 

Band type  | Use   
---|---  
Report title | Shown only on the first page   
Report summary | Shown only on the last page   
Page header | Shown at the top of each page   
Page footer | Shown at the bottomof each page   
Master header | Shown at the beginning of the first detail band/level   
Master data | First detail level data; repeated for each record in dataset   
Master footer | Shown at the end of the first detail band/level   
Detail header | Shown at the beginning of the second detail band/level   
Detail data | Second detail level data   
Detail footer | Shown at the end of the second detail band/level   
Subdetail header | Shown at the beginning of the third detail band/level   
Subdetail data | Third detail level data   
Subdetail footer | Shown at the end of the third detail band/level   
Overlay | Shown once on each page lower layer   
Column header | Shown at the top of every column   
Column footer | Shown at the bottom of every column   
Group header | Shown at the top of every group   
Group footer | Shown at the bottom of every group   
Cross header | Shown at the top of cross tables/pivot tables   
Cross data | Shows cross tables/pivot table data   
Cross footer | Shown at the bottom of cross tables/pivot tables   
  
### Report title

In the Report title band, add a rectangle. The text editor will show up. In the first line the title of the report should be indicated. On the the second, system date and time need to be added. They are obtained by clicking **Variable** and then Category **Other** , then double-click on the desired variable (once you are familiar with the variables, you can also just type them). Now we have: 

[![lazreptutreporttitle.png](https://wiki.freepascal.org/images/6/6d/lazreptutreporttitle.png)](</File:lazreptutreporttitle.png>)

Click OK to exit the editor, then adjust the title rectangle to the desired size. Choose a color for the background with the Bucket Fill button on the toolbar. 

### The main event: showing data in the master data band

Let's insert a master data band. LazReport prompts you for a data source; choose **frDBDataSet1** , the main data source for our example. The master band will be repeated for each record in the data source. You can double-click later on the band if you want to change the data source. 

[![lazreptutbanddatasource.png](https://wiki.freepascal.org/images/8/82/lazreptutbanddatasource.png)](</File:lazreptutbanddatasource.png>)

In the left half of the band we insert a rectangle and double click. The familiar text editor appears. 

Now we will add several fields in the one rectangle. If needed though, you can use one rectangle per field if you want to apply different formatting etc. 

The first line **Name** [Full name field] is obtained by writing Name and then, with the **Variable** button, entering the variable [Field Name]. Alternatively, as indicated before, you can write the resulting text directly. 

The next line **Salary** [Salary field] is written similarly. 

The third line - Salary divided by job grade [[Salary field]/[Job grade field]] - is done likewise. This shows you can perform calculations with fields coming from the data source. Note the extra set of square brackets around the calculation. As you may guess, you can not only use the variables listed via the **Variable** button, but also database fields via **DB Field**. 

[![lazreptutmasterdata.png](https://wiki.freepascal.org/images/f/f4/lazreptutmasterdata.png)](</File:lazreptutmasterdata.png>)

You can use all kinds of operators and functions in your calculations; see [LazReport Documentation#Documentation](<LazReport_Documentation.md> "LazReport Documentation") for an overview. 

#### Formatting output

You can control how the text in the rectangles is displayed. 

Let's first add a new rectangle with the _Hire date field_ variable. 

In the variable editor, let's set a format. A format is valid for all variables of a rectangle/object. To show the format editor, double click on a rectangle to get the Text Editor, then click the _Format_ button. 

Let's set the date format you want here: 

[![lazreptutformattingdate.png](https://wiki.freepascal.org/images/4/45/lazreptutformattingdate.png)](</File:lazreptutformattingdate.png>)

The first ComboBox indicates the type of data (text, numeric, date, time, logical/boolean). The second lets you select a preset format.... with some obvious restrictions: 

  * The Text type does not allow any Number type formatting.
  * The Number type requires a number of any type (Integer, Double, Currency...).
  * The Date type: you can choose a pre-defined format or write a custom format like MM/DD/YY for example.



Except for Text and Logic types, you can use the formatting strings you also use in Lazarus. 

  
Other formatting options are present on the button bar below the menu (e.g. bold, italic, highlighting). 

Highlighting is very useful for emphasizing data. You can specify a condition that determines when highlighting is applied. 

For that, use the keyword Value to indicate the value of the variable. Here is the highlighting for all hire dates later than 1989. Because Hire date is a TDateTime field (which internally is a float), I cheated by just using the float value of January 1st, 1990 which is 32874. By default highlighter is a simple bold on white background. You can set these parameters to an area by clicking the Highlight button. 

[![lazreptutconditionalhighlight.png](https://wiki.freepascal.org/images/e/e0/lazreptutconditionalhighlight.png)](</File:lazreptutconditionalhighlight.png>)

#### A picture is worth...

If you haven't already, we'll need to add a PHOTO field to our test database; using an SQL statement like this (note: unfortunately different databases use different syntax; BLOB can be CLOB,IMAGE,OLE...): 
    
    
    ALTER TABLE EMPLOYEE ADD PHOTO BLOB;
    

Now add some test data - the example here uses jpg files - into the PHOTO field with your favourite database tool. Note: this is the way Delphi DBImage works: just binary blobs with image data. By default, Lazarus DBImage is more flexible and stores the file extension before the image data, allowing different file formats to be used. For simplicity's sake, we'll use the Delphi approach. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For this "DBImage BLOB without file extension info" to work, you'll need the patch from issue 25453/at least revision ... of LazReport

Now we have to tell the report you need to treat all data as binary jpg image files. Click on frReport1, go to Object Inspector, events and initiates a procedure OnDBImageReader: 
    
    
    procedure TForm1.frReport1DBImageRead(Sender: TObject; S: TStream;
      var GraphExt: string);
    begin
      // Force TDBImage to Delphi compatible behaviour: únicamente acepta un formato de fichero, e.g. jpg
      // esto significa que los BLOBs de la base de datos deben ser justamente ficheros jpg binarios sin más datos.
      GraphExt:='jpg';
    end;
    

In File>Variables List, add the SQLQuery1.PHOTO field as variable Photo field, similarly to how you did this before. If you don't see the field, you may need to set the form's SQLQuery to inactive and active again to refresh the list of columns. 

Click on the Insert image... icon on the left of the window; now click on the right side of the band to drop the image control. A Picture editor shows up: 

[![lazreptutpictureeditor.png](https://wiki.freepascal.org/images/f/fc/lazreptutpictureeditor.png)](</File:lazreptutpictureeditor.png>)

If you wanted to always show the same picture, you'd just need to click Load ... (or double click on the picture control) and choose your image. 

However, let's press the Memo button and insert your Photo variable: 

[![lazreptutphotofield.png](https://wiki.freepascal.org/images/a/a2/lazreptutphotofield.png)](</File:lazreptutphotofield.png>)

This turns the picture into an object associated with the data field. Press OK as needed to get back to the report designer. 

  


Adjust height and width as needed. 

You can also press F11 to show the Object Inspector which allows you to edit more if needed: 

[![lazreptutobjectinspector.png](https://wiki.freepascal.org/images/b/b5/lazreptutobjectinspector.png)](</File:lazreptutobjectinspector.png>)

Close the report editor and save the report definition (with the same name, **lazreporttutorial.lrf**). 

If you go to File>Preview, you'll see your report includes the images. 

Close the report editor and save the report definition as **lazreporttutorial.lrf**. 

### Detail data

To do: show how to use the detail part of master/detail. see e.g. sample in components/lazreport/samples/editor directory; see the button title "Master-Detail" **Please write me.**

### Loading the report from the form

Now let's write the code required to load and view or edit our report. To this add the following code to the project: 
    
    
    TForm1.Button2Click(Sender procedure: TObject);  
    begin 
      frReport1.LoadFromFile('lazreporttutorial.lrf');  
      frReport1.ShowReport;  
    end;
    TForm1.ButtonClick(Sender procedure: TObject);  
    begin 
      frReport1.LoadFromFile('lazreporttutorial.lrf');  
      frReport1.DesignReport;  
    end;
    

You have a preview option that gives end users a way to modify the report without changing the basic application. 

### Printing

Now we're going to add features to our basic example. 

Add a TButton component with Caption := 'Print...', and a TPrintDialog. Behind the scenes, a project dependency on the **Printer4Lazarus** packages will be added by the IDE, so make sure that package is installed. 

We end up with this: 

[![lazreptutformwithprintbutton.png](https://wiki.freepascal.org/images/b/b6/lazreptutformwithprintbutton.png)](</File:lazreptutformwithprintbutton.png>)

Now add the **printers** unit to your form's uses clauses. 

Put the following code in the OnClick event of the **Print** button: 
    
    
    procedure TForm1.Button3Click(Sender: TObject);
    var
      FromPage, ToPage, NumberCopies: Integer;
      ind: Integer;
      Collap: Boolean;
    begin
      // Load report definition from application directory
      AppDirectory:=ExtractFilePath(ParamStr(0));
      frReport1.LoadFromFile('lazreporttutorial.lrf');
      // Need to keep track of which printer was originally selected to check for user changes
      ind:= Printer.PrinterIndex;
      // Prepare the report and just stop if we hit an error as continuing makes no sense
      if not frReport1.PrepareReport then Exit;
      // Set up dialog with some sensible defaults which user can change
      with PrintDialog1 do
      begin
        Options:=[poPageNums ]; // allows selecting pages/page numbers
        Copies:=1;
        Collate:=true; // ordened copies
        FromPage:=1; // start page
        ToPage:=frReport1.EMFPages.Count; // last page
        MaxPage:=frReport1.EMFPages.Count; // maximum allowed number of pages
        if Execute then // show dialog; if succesful, process user feedback
        begin
          if (Printer.PrinterIndex <> ind ) // verify if selected printer has changed
            or frReport1.CanRebuild // ... only makes sense if we can reformat the report
            or frReport1.ChangePrinter(ind, Printer.PrinterIndex) //... then change printer
            then
            frReport1.PrepareReport //... and reformat for new printer
          else
            exit; // we couldn't honour the printer change
    
          if PrintDialog1.PrintRange = prPageNums then // user made page range selection
          begin
            FromPage:=PrintDialog1.FromPage; // first page
            ToPage:=PrintDialog1.ToPage;  // last page
          end;
          NumberCopies:=PrintDialog1.Copies; // number of copies
          // Print the report using the supplied pages & copies
          frReport1.PrintPreparedReport(inttostr(FromPage)+'-'+inttostr(ToPage), NumberCopies);
        end;
      end;
    end;
    

It's a bit long, but if needed, you can create a descendant of TfrReport with an easier printing method or class , create a PrintReport (Report: string) procedure to load the report and print. 

When double clicking on **frReport1** , in the report designer File>Page Options menu you get this: 

Select paper size: 

[![lazreptutpapersize.png](https://wiki.freepascal.org/images/1/11/lazreptutpapersize.png)](</File:lazreptutpapersize.png>)

Choose desired margins, number of columns, and the space between them, layout order etc. [![lazreptutprintoptions.png](https://wiki.freepascal.org/images/a/ab/lazreptutprintoptions.png)](</File:lazreptutprintoptions.png>)

### Report options

Via File>Report Options you can set a default printer that will be used for the report (using the "Select when report loaded" checkbox). 

If you want to include the total number of pages with the page number on each page, you need to select **Two-pass report** (which tells Lazreport to build the report twice, inserting total number of pages on the second run). 

[![lazreptutreportoptions.png](https://wiki.freepascal.org/images/5/55/lazreptutreportoptions.png)](</File:lazreptutreportoptions.png>)

On the **Other** tab, you can fill out some report metadata, as is done on the screenshot below: [![lazreptutreportoptionsother.png](https://wiki.freepascal.org/images/4/40/lazreptutreportoptionsother.png)](</File:lazreptutreportoptionsother.png>)

### General purpose variables

Now we'll add some report variables unrelated to the database data in our example. They let you e.g. show different names according to the context. 

To do this, open the report editor and select the File menu and the Variable List. Add a Test category, and a test1 variable the same way we did earlier (with a space before the variable name): 

[![lazreptutvariableseditortestvar.png](https://wiki.freepascal.org/images/6/6b/lazreptutvariableseditortestvar.png)](</File:lazreptutvariableseditortestvar.png>)

The only difference is that you will not select a value field, but leave None (the default value): [![lazreptutvariabletest.png](https://wiki.freepascal.org/images/7/78/lazreptutvariabletest.png)](</File:lazreptutvariabletest.png>)

Return to Lazarus (do not forget to save your report, if necessary) and set up the **OnGetValue _event of the_** _frReport1'_ component by adding the following code: 
    
    
    procedure TForm1.frReport1GetValue(const ParName: String; var ParValue: Variant
      );
    begin
     // Conditionally assign value:
     if ParName='test1' then 
       ParValue:='Answer: Variable "test1"'; //perhaps replace with a call to a random function or something?
    end;
    

  
Alternatively, you can also pass the value directly to the variable using the following code: 
    
    
    procedure TForm1.Button1Click(Sender:TObject);
    begin
      frReport1.Values.FindVariable('test').Field := QuotedStr('test1');
      frReport1.ShowReport;
    end;
    

## See also

  * Tutorial de LazReport Version used as a basis for this translation.
  * [LazReport Documentation](<LazReport_Documentation.md> "LazReport Documentation")

---

_Source: [https://wiki.freepascal.org/LazReport_Tutorial](https://web.archive.org/web/20250424005041/https://wiki.freepascal.org/LazReport_Tutorial)_
