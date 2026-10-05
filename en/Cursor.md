# Cursor

│ **[Deutsch (de)](</Cursor/de> "Cursor/de")** │  **English (en)** │  **[suomi (fi)](</Cursor/fi> "Cursor/fi")** │    
****

Cursor - [property](</Property> "Property") of the TCursor Object. 

## Contents

  * 1 List of cursor constants
  * 2 Examples
    * 2.1 Example 1: To View All The Cursor Types
    * 2.2 Example 2: Change An Objects Cursor
    * 2.3 Example 3: Change All Controls To An Hour Glass, Except TBitBtn Controls



## List of cursor constants

Constant | Integer value | Shape   
---|---|---  
crDefault | 0 | [![crArrow.png](https://wiki.freepascal.org/images/e/ed/crArrow.png)](</File:crArrow.png>)  
crNone | -1 | Invisible mouse pointer   
crArrow | -2 | [![crArrow.png](https://wiki.freepascal.org/images/e/ed/crArrow.png)](</File:crArrow.png>)  
crCross | -3 | [![crCross.png](https://wiki.freepascal.org/images/d/d9/crCross.png)](</File:crCross.png>)  
crIBeam | -4 | [![crIBeam.png](https://wiki.freepascal.org/images/3/3a/crIBeam.png)](</File:crIBeam.png>)  
crSize | -22 | [![crSize.png](https://wiki.freepascal.org/images/8/88/crSize.png)](</File:crSize.png>)  
crSizeNESW | -6 | [![crSizeNESW.png](https://wiki.freepascal.org/images/0/00/crSizeNESW.png)](</File:crSizeNESW.png>)  
crSizeNS | -7 | [![crSizeNS.png](https://wiki.freepascal.org/images/3/3e/crSizeNS.png)](</File:crSizeNS.png>)  
crSizeNWSE | -8 | [![crSizeNWSE.png](https://wiki.freepascal.org/images/9/94/crSizeNWSE.png)](</File:crSizeNWSE.png>)  
crSizeWE | -9 | [![crSizeWE.png](https://wiki.freepascal.org/images/3/37/crSizeWE.png)](</File:crSizeWE.png>)  
crUpArrow | -10 | [![crUpArrow.png](https://wiki.freepascal.org/images/d/d1/crUpArrow.png)](</File:crUpArrow.png>)  
crHourGlass | -11 | [![crHourGlass.png](https://wiki.freepascal.org/images/b/bd/crHourGlass.png)](</File:crHourGlass.png>)  
crDrag | -12 | [![crDrag.png](https://wiki.freepascal.org/images/3/38/crDrag.png)](</File:crDrag.png>)  
crNoDrop | -13 | [![crNoDrop.png](https://wiki.freepascal.org/images/a/a9/crNoDrop.png)](</File:crNoDrop.png>)  
crHSplit | -14 | [![crHSplit.png](https://wiki.freepascal.org/images/5/50/crHSplit.png)](</File:crHSplit.png>)  
crVSplit | -15 | [![crVSplit.png](https://wiki.freepascal.org/images/d/d3/crVSplit.png)](</File:crVSplit.png>)  
crMultiDrag | -16 | [![crMultiDrag.png](https://wiki.freepascal.org/images/d/d4/crMultiDrag.png)](</File:crMultiDrag.png>)  
crSQLWait | -17 | [![crSQLWait.png](https://wiki.freepascal.org/images/f/f4/crSQLWait.png)](</File:crSQLWait.png>)  
crNo | -18 | [![CrNo.png](https://wiki.freepascal.org/images/3/32/CrNo.png)](</File:CrNo.png>)  
crAppStart | -19 | [![crAppStart.png](https://wiki.freepascal.org/images/a/a6/crAppStart.png)](</File:crAppStart.png>)  
crHelp | -20 | [![crHelp.png](https://wiki.freepascal.org/images/b/b1/crHelp.png)](</File:crHelp.png>)  
crHandPoint | -21 | [![crHandPoint.png](https://wiki.freepascal.org/images/8/8a/crHandPoint.png)](</File:crHandPoint.png>)  
  
## Examples

### Example 1: To View All The Cursor Types

(1) On [Form1](<TForm.md> "TForm"), drag a [ComboBox](<TComboBox.md> "TComboBox") control onto the Form. 

(2) Set the ComboBox1, Items (TStrings) to the following: 
    
    
    crAppStart
    crArrow
    crCross
    crDefault
    crDrag
    crHandPoint
    crHelp
    crHourGlass
    crHSplit
    crIBeam
    crMultiDrag
    crNo
    crNoDrop
    crNone
    crSizeAll
    crSizeNESW
    crSizeNS
    crSizeNWSE
    crSizeWE
    crSQLWait
    crUpArrow
    crVSplit
    

  
(3) On the ComboBox1Change procedure add this code: 
    
    
    procedure TForm1.ComboBox1Change(Sender: TObject);
    begin
         Cursor := StringToCursor(ComboBox1.Text);
    end;
    

This will allow you to select the cursor type from the ComboBox and see it when you move the mouse over the Form. Only when you hover over the ComboBox will the cursor return to the default (crDefault). 

### Example 2: Change An Objects Cursor
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
         Cursor := crHourGlass;
         // Changes the Form1 cursor to an hour glass.
    
         Button1.Cursor := crHourGlass;
         // Changes the Button1 cursor to an hour glass.
    
         Memo1.Cursor := crHourGlass;
         // Changes the Memo1 cursor to an hour glass.
    end;
    

### Example 3: Change All Controls To An Hour Glass, Except TBitBtn Controls
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
       I: Integer;
    begin
         Cursor := crHourGlass;
         for I := 0 to ControlCount - 1 do
         begin
              if (Controls[I].ClassType <> TBitBtn) then
                 Controls[I].Cursor := crHourGlass;
         end;
    end;
    

However, if you had a GroupBox, you would have to address the controls within it separately. 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
       I: Integer;
    begin
         Cursor := crHourGlass;
         for I := 0 to ControlCount - 1 do
         begin
              if (Controls[I].ClassType <> TBitBtn) then
                 Controls[I].Cursor := crHourGlass;
         end;
         for I := 0 to GroupBox1.ControlCount - 1 do
         begin
              if (GroupBox1.Controls[I].ClassType <> TBitBtn) then
                 GroupBox1.Controls[I].Cursor := crHourGlass;
         end;
    end;
    

With the above example, in which you had Form1, as well as other controls on the form such as [Memo](<TMemo.md> "TMemo"), [Edit](<TEdit.md> "TEdit"), [Image](<TImage.md> "TImage"), [BitBtn](<TBitBtn.md> "TBitBtn"), etc., this would change all but the BitBtn controls to the Hour Glass. There are controls such as the [TGroupBox](<TGroupBox.md> "TGroupBox"), [TPanel](<TPanel.md> "TPanel"), that act as individual containers for controls, and these items must be addresses separately from the main Form in order to change their internal control set to a different cursor at [runtime](<runtime.md> "runtime").

---

_Source: [https://wiki.freepascal.org/Cursor](https://web.archive.org/web/20230328024845/https://wiki.freepascal.org/Cursor)_
