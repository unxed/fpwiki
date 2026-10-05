# Using the printer

│ **English (en)** │

## Contents

  * 1 Introduction
  * 2 The basic steps
    * 2.1 Adding the Printer4Lazarus package to your project requirements
    * 2.2 Adding the Printers unit to the uses section of your unit
    * 2.3 Using the existing Printer object
    * 2.4 Printing from console application
  * 3 Next steps
  * 4 Advanced steps: printing controls
  * 5 And on we go: resizing
  * 6 Common Tasks
    * 6.1 Enumerating Available Printers
    * 6.2 Changing/Selecting Printer
    * 6.3 Preparing Paper Size
  * 7 See also



## Introduction

Printing is easy in Lazarus/FreePascal. However, you do need to follow some required steps. First steps first. After those first steps, you can branch out to more advanced printing. This article discusses the required steps, as collected from various forum pages and examples. After the basic steps, we are going to print some text. Finally a hint of some more advanced printing is given. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Currently printing in WinCE (using the Printers4Lazarus package) is unsupported.

## The basic steps

You must do the following to be able to use printers: 

  1. Add the Printer4Lazarus package to your project requirements.
  2. Add the Printers unit to the uses section of your unit.
  3. Use the existing Printer object.



### Adding the Printer4Lazarus package to your project requirements

The Printer4Lazarus package defines a basic printer and provides platform independent printing. The following can thus be used on various platforms. 

In the Lazarus IDE, do the following: 

  1. In the **Project** menu, click **Project Inspector**. A window is shown with a tree view, one of the branches is named _Required Packages_. By default, the _Required Packages_ branch shows the _LCL_ package.
  2. Click the **Add** button, the button with the plus sign at the top of the window.
  3. Open the **New Requirements** page.
  4. From the **Package Name** list box, select _Printer4Lazarus_.
  5. Click **OK**.
  6. _Printer4Lazarus_ is now shown in the _Required Packages_ branch.



### Adding the Printers unit to the uses section of your unit

This step is simple and the result could look like this: 
    
    
    unit MainUnit;
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, SysUtils, Forms, Printers;
    

### Using the existing Printer object

Let's assume you want to click a button to print a text. On your form, put a button called **PrintBtn** and in the **OnClick** event you can now use the following: 
    
    
    procedure TForm1.PrintBtnClick(Sender: TObject);
    const
      LEFTMARGIN = 100;
      HEADLINE = 'I Printed My Very First Text On ';
    var
      YPos, LineHeight, VerticalMargin: Integer;
      SuccessString: String;
    begin
      with Printer do
      try
        BeginDoc;
        Canvas.Font.Name := 'Courier New';
        Canvas.Font.Size := 10;
        Canvas.Font.Color := clBlack;
        LineHeight := Round(1.2 * Abs(Canvas.TextHeight('I')));
        VerticalMargin := 4 * LineHeight;
        // There we go
        YPos := VerticalMargin;
        SuccessString := HEADLINE + DateTimeToStr(Now);   
        Canvas.TextOut(LEFTMARGIN, YPos, SuccessString);
      finally
        EndDoc;
      end;
    end;
    

Did I write _basic_ and _simple_. The above example is somewhat complex. Next to the basic text output in the bold line, it also provides an example of formatting your text. 

From _begin_ to _end;_ the following happens: 

  * With _Printer.BeginDoc_ you start printing - however, nothing is sent to the printer until you finish with _Printer.EndDoc;_.
  * The printer uses a Canvas to draw the output on. It is this drawing that ends up on the printed page. _Canvas.Font_ is the font object used for the output canvas. That is, the _TextOut_ call we use later will output text using the settings of the font object of that moment.
  * Everything you draw on the canvas must be positioned using coordinates. So, we calculate a _LineHeight_ to position text vertically. You could do the same for the horizontal position, which I left here to be _LEFTMARGIN_.
  * The text is drawn with the _TextOut_ call.
  * This magnificent result is sent to the printer by _Printer.EndDoc_.



In some forums it is suggested that the use of _PrintDialog_ (the printer selection dialog) is required for good functioning, but I did not find this to be true (any more). 

### Printing from console application

The tutorial shown above is meant for printing out of a Lazarus GUI application. What if you are testing printing in a console application? The difference is that a console application by-passes the Lazarus widgetset architecture and does not know about the LCL. The elemental step to make the LCL available in a consolue application is to add the unit "Interfaces" to the uses clause. But then you need another unit: "OSPrinters" to make the printer units of your operating system available. 

Write your "uses" clause as follows, and your application will work (tested on Windows): 
    
    
    program PrintUtil;
     
    uses
      Interfaces,    // <--- ADDED
      SysUtils, printers,
      osprinters;    // <--- ADDED
      //, printer4lazarus; // <--- Not needed in "uses", but only in "required packages" of project
     
    const
      LEFTMARGIN = 100;
      HEADLINE = 'Printing: ';
    var
      YPos, LineHeight, VerticalMargin: integer;
    begin
      with Printer do
      try
        BeginDoc;
        Canvas.Font.Name := 'Courier New';
        Canvas.Font.Size := 10;
        Canvas.Font.Color := $0;
        LineHeight := Round(1.2 * Abs(Canvas.TextHeight('I')));
        VerticalMargin := 4 * LineHeight;
        YPos := VerticalMargin;
        Canvas.TextOut(LEFTMARGIN, YPos, HEADLINE + DateTimeToStr(Now));
      finally
        EndDoc;
      end;
     
    end.
    

## Next steps

After the above basic steps, you can do the next steps. I leave it to the reader to try: 

  * Make drawings on the paper.
  * Format text nicely.
  * Select another printer and compare results.



In the Lazarus distribution there is an example project that uses Raw printing to the printer. You can find this example with the following location $(lazarusdir)\components\printers\samples\rawmode\rawmodetest.lpi. Another sample shows how to select another printer: $(lazarusdir)\components\printers\samples\dialogs\selectprinter.lpi. 

## Advanced steps: printing controls

The printer object primarily allows you to draw on a canvas and send that canvas image to the printer. If we continue on the path shown above, then you would use the printer's canvas methods to draw text, ellipses, diamonds and what not. 

However, this can hardly be interesting to any serious programmer. You are working on a more perfect CAD program or yet another image file sorter and want to print the wonderful result of your program to your printer. Trying to translate the perfect picture into canvas method calls is not the way: you already have the picture. 

Each and every control you put on a form, also draws a picture on a TCanvas object just like the printer. We can use that to bring the picture from the screen to the printer. 

Imagine you are going to make a preview program. You create a form and on this form you put a TPanel. This panel will provide the nice grayish background of the preview. On the panel, you put another TPanel object called _page_. This _page_ will be white and represents the paper. You can nicely size the page. 

On this page we put a TShape object, for example a nice red, rounded rectangle. Now try the following in the **PrintBtnClick** event method: 
    
    
    MyPrinter.BeginDoc;
      page.PaintTo(myPrinter.Canvas, 0, 0);
    MyPrinter.EndDoc;
    

What happens: 

  * _BeginDoc_ starts the printing (but nothing is sent yet).
  * _page.PaintTo_ sends the output of our TPanel object that represents the page to the canvas of the printer. Note the following: 
    1. You can use the PaintTo method of any control in the control hierarchy. You could also send the output of the whole window to the printer if you want to.
    2. You can send the output of any control with a PaintTo method to the printer, so you can be creative. To send the output of your image sorter to the printer you may send the output of the TImage to the printer.
    3. TCanvas also has a method to copy rectangles from another canvas. However, you can only do that if an object really draws to a canvas. I think most controls rely on containers to provide a real canvas, so you cannot copy rectangles from just any control. At least, it didn't work for me.
    4. Make sure the control you want to paint to the printer is visible. If the control is not visible, nothing will be painted not even to the printer.
  * _EndDoc_ sends the drawing to the printer.



## And on we go: resizing

The printer uses a lot more pixels per inch on paper than the monitor uses pixels per inch on the screen. As a result, the output that is redirected from the screen to the printer ends up rather smallish on the paper. Scaling and controlling the layout is important for good looking output. It would be nice if you can have an exact sized copy of what you see on the screen. 

For the moment we are not striving for the ideal, just for the idea. As said before, controls do not have their own canvas but rely on the canvas of a container or owner. However, there are components that have their own canvas. I chose TBitMap and then it works as follows. 
    
    
    procedure TForm1.PrintBtnClick(Sender: TObject);
    var
      MyPrinter : TPrinter;
      myBitMap : TBitMap;
    begin
      myBitMap := TBitMap.Create;
      myBitMap.Width := page.Width;
      myBitMap.Height := page.Height;
      page.BorderStyle:=bsNone;
      page.PaintTo(myBitMap.Canvas, 0, 0);
      page.BorderStyle:=bsSingle;
      //
      MyPrinter := Printer;
      MyPrinter.BeginDoc;
        //page.PaintTo(myPrinter.Canvas, 0, 0);
        //myPrinter.Canvas.Draw(0,0, myBitMap);
        myPrinter.Canvas.CopyRect(Classes.Rect(0, 0, myPrinter.PaperSize.Width, myPrinter.PaperSize.Height),
           myBitMap.Canvas, Classes.Rect(0, 0, myBitMap.Width, myBitMap.Height));
      MyPrinter.EndDoc;
      myBitMap.Free;
    end;
    

For this to work, do not use the _Windows_ unit. The _Windows_ unit has other definitions for **Rect**. What you see in the example is the following: 

  * A bitmap is created and made the same size as the page control.
  * To avoid the border to print, the **BorderStyle** of the page is switched off before painting it to the bitmap and set back to its old value afterwards.
  * Then printing is started and the _BitMap_ canvas content is copied to the printer canvas.



But notice one thing, in the process _page_ is magnified. The **Printer.papersize** is considerably bigger than the the size of the bitmap, but the copy process fills the destination rectangle nicely. So, when we want to do this, we must make sure that the _page_ dimensions have the same ratio as the paper dimension. You can figure out how to do it. 

A problem with this way of working is of course that the pixels of the screen will show on the printed paper. As said, it is not ideal but it shows a principle. Controls do not have their own canvas; to print controls we first paint them on an object that does have its own canvas: the TBitMap. Now you know how it works, you can figure out a way to create fine artwork or documents. This will need objects that draw themselves properly in a TPanel with low resolution, but also on a TBitMap with high resolution. But, that is food for another article. 

## Common Tasks

Keep in mind that **Printer: TPrinter** is a singleton. It's a single object that serves all printers installed in the system. A particular system implementation (WinAPI, Cocoa, etc) should implement the object. 

### Enumerating Available Printers

The list of system printers is available via **Printer.Printers** property. 

**Printers** property is TStrings containing the name of each printer. 
    
    
    procedure TForm1.FormShow(Sender: TObject);
    begin
      ListBox1.Items.AddStrings(Printer.Printers);
    end;
    

The name given in the property should be used by setting printer via SetPrinter() method. 

**Internals:** any class implementing **TPrinter** class should implement **DoEnumPrinters()** method. DoEnumPrinters should populate the passed Lst parameters with the name of printers. It's expected that the name of the printer device is unique. 

**DoEnumPrinters()** should return the system default printer as the first entry (index 0) in the list. 

### Changing/Selecting Printer

By default Printer should be have a "system default" printer pre-selected. 

If needed the printer can be changed by the user either via PrinterDialogs or programmatically. 

In order to change the printer programmatically either **SetPrinter** method or **PrinterIndex** property should be used. 

Both methods are based on the printers listed via **PrintersProperty**. 

  * **PrinterIndex** determines the printer name based on the specified index from the **PrintersList** and after that calls **SetPrinter** method.


    
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      Printer.SetPrinter(ListBox1.ItemIndex);
    end;
    

    

  * If Index specified is -1 then the default printer is selected



  * **SetPrinter** method, verifies, if the requested name exists in **Printers** property list.


    
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      Printer.SetPrinter(ListBox1.GetSelectedText);
    end;
    

    

  * Note, that there's a special name used for **SetPrinter** method. If name specified as '*' the first (default) printer available is selected.



Either of the methods can raise an exception, if specified name or index doesn't exist. 

**Internals:** The **TPrinter** deriving class should implement **DoSetPrinter()** method using **aName** parameter as the name of the selected printer. The method doesn't need to do any special handling for '*' name, as it being take care at the higher level of TPrinter. 

### Preparing Paper Size

Note: if you're using multiple printer (especially if you're using specialized printers using non-standard paper size, such as "A4" (European) or "US Letter" (USA)), you always want to prepare the paper size. It is possible that a newly selected printer doesn't support the paper that has been supported by the previously selected printer. The paper size is not reset automatically and you want to do it explicitly. 

There are three properties of **PaperSize** that can be called in to order to reset and prepare paper size: 

  * **PaperName** \- returns currently selected paper name of the selected printer.
  * **DefaultPaperName** \- returns the paper name default for the selected printer
  * **SupportedPapers** \- returns the list of paper sizes supported by the selected printer



Here's an example of code that re-reads the list of selected papers 
    
    
    procedure TForm1.ListBox1SelectionChange(Sender: TObject; User: boolean);
    begin
      // printer changed. But the selected paper size might to be the right one
      Printer.PrinterIndex := ListBox1.ItemIndex; 
      ListBox2.Clear;
      // calling SupportedPapers forcing validation of the paper size
      ListBox2.Items.AddStrings(Printer.PaperSize.SupportedPapers); 
    end;
    

Here's an example of reseting paper size, using default paper name property. 
    
    
    procedure TForm1.ListBox1SelectionChange(Sender: TObject; User: boolean);
    begin
      // printer changed. But the selected paper size might to be the right one
      Printer.PrinterIndex := ListBox1.ItemIndex; 
      // calling DefaultPaperName forcing validation of the paper size
      Printer.PaperSize.DefaultPaperName;
    end;
    

**Internals:** The deriving **TPrinter** class should properly implement : 

  * **DoEnumPapers() method**. The method could use **PrinterIndex** or **PrinterName** property to determine the currently selected printer to which the paper sizes needs to be enumerated.



If the internal method returns no paper sizes, **TPrinter** class substitutes a "default" paper sizes (assumed to be supported by most of printers). Default sizes are: **Letter** , **A4** , **Legal**. However, a good TPrinter class implementation should populate the page sizes. 

  * **DoGetPaperRect()** method. The method returns the physical size of the paper specified by the name for the current printer. The name of the paper is passed via **aName** property. The returned value should be populated to **TPaperRect** structure.



Two fields should be populated: 

  * PhysicalRect - the actual size of the entire page. The size is given in dots. Left and Top are expected to be zeroes.
  * WorkRect - the rectangle where the printer can actually print (due to some mechanical limitations of the printer itself). The rectangle should always be the same or less than PhysicalRect. Left and Top are offsets from the top-left corner of the physical rect. The units are printer dots.



Number of dots is driven by the printer currently selected DPI. (Note that there are two different DPI for horizontal and vertical measurements). 

Here's the table showing the difference in A4 size, driven by different DPI size 

DPI  | Returned size in TPaperRect   
---|---  
72  | 595 x 842   
300  | 2480 x 3507   
600  | 4960 x 7014   
1200  | 9920 x 14028   
  
  


  * **DoGetDefaultPaperName()** \- the method should return the default paper size name of the currently selected printer.
  * **DoGetPaperName** \- the method should return the currently selected paper size name of the currently selected printer.
  * **DoSetPaperName** \- the method should set the paper size by its name for the selected printer.



## See also

  * [Printing](<Printing.md> "Printing")
  * [LazReport Documentation](<LazReport_Documentation.md> "LazReport Documentation") LazReport can be used to design data-driven reports that can be printed, exported to PDF etc.
  * [LazReport Tutorial](<LazReport_Tutorial.md> "LazReport Tutorial")

---

_Source: [https://wiki.freepascal.org/Using_the_printer](https://web.archive.org/web/20250508233218/https://wiki.freepascal.org/Using_the_printer)_
