# IDE Window: Sort selection

│ **English (en)** │

## Contents

  * 1 Direction
  * 2 Domain
    * 2.1 Lines
    * 2.2 Words
    * 2.3 Paragraphs
  * 3 Options



## Direction

  * Ascending - from low to high: for example a,b, ... z
  * Descending - from high to low: for example z, y, ..., a



## Domain

### Lines

Sort lines. 

### Words

Sort words. The selection is splitted on every word start and sorted. For example: 
    
    
     enum3, enum1, enum2,
    

becomes 
    
    
     enum1, enum2,enum3,
    

### Paragraphs

Sort paragraphs. The selection is splitted on every start of a paragraph. A paragraph starts with a line less indented. For example: 
    
    
       procedure Button1Click(Sender: TObject);
       procedure FormCreate(Sender: TObject);
       procedure FormMouseMove(Sender: TObject; Shift: TShiftState; X, Y: Integer);
       procedure FormPaint(Sender: TObject);
       procedure FormMouseUp(Sender: TOBject; Button: TMouseButton;
         Shift: TShiftState; X, Y: Integer);
       procedure FormMouseDown(Sender: TOBject; Button: TMouseButton;
         Shift: TShiftState; X, Y: Integer);
       procedure FormDestroy(Sender: TObject);
    

becomes 
    
    
       procedure Button1Click(Sender: TObject);
       procedure FormCreate(Sender: TObject);
       procedure FormDestroy(Sender: TObject);
       procedure FormMouseDown(Sender: TOBject; Button: TMouseButton;
         Shift: TShiftState; X, Y: Integer);
       procedure FormMouseMove(Sender: TObject; Shift: TShiftState; X, Y: Integer);
       procedure FormMouseUp(Sender: TOBject; Button: TMouseButton;
         Shift: TShiftState; X, Y: Integer);
       procedure FormPaint(Sender: TObject);
    

  


## Options

  * Case sensitive - distinguish lower and upper case
  * Ignore space - multiple spaces are treated as one, when comparing lines

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Sort_selection](https://web.archive.org/web/20240413094514/https://wiki.freepascal.org/IDE_Window%3A_Sort_selection)_
