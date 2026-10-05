# Peg Solitaire tutorial

│ **English (en)** │

This tutorial is the second Lazarus tutorial that aims at introducing the basics of Lazarus application development. It's best to start this tutorial after having finished the first one ([Howdy World (Hello World on steroids)](<Howdy_World_\(Hello_World_on_steroids\).md> "Howdy World \(Hello World on steroids\)")). This tutorial exlpains a bit about how to work with graphics and how to make a program modular. The final product of this tutorial is a basic but working version of the Peg Solitaire game ([[1]](<http://en.wikipedia.org/wiki/Peg_solitaire>)). If all goes well in the end it will look something like this: 

[![tutpeg solitaire.png](https://wiki.freepascal.org/images/1/10/tutpeg_solitaire.png)](</File:tutpeg_solitaire.png>)

## Contents

  * 1 Start the project
  * 2 First steps
  * 3 It's all about events
  * 4 Intermezzo
  * 5 Let's get artistic
  * 6 Populate the board
  * 7 Events revisited
  * 8 Gluing things together
  * 9 Cleaning up
    * 9.1 What's next?
  * 10 Creating more board layouts
  * 11 Eye candy
  * 12 Final tweaks



## Start the project

As mentioned in the previous tutorial it's best to start with a clean, separate directory for each project. A quick recap: 

  * Create a new directory for this game.
  * Start a new Application (Project/New Project... and select Application).
  * Save the project as PegSolitaire.
  * Save the main [form](<TForm.md> "TForm") as ufrmMain.
  * In the object inspector change the form's name to _frmMain_.
  * Change the caption to _Lazarus Peg Solitaire_.
  * In the project options insert _bin\_ in front of the target filename (or _bin/_ for Linuxes).



And extra for this project: 

  * Open the project options dialog ( `⇧ Shift`+`Ctrl`\+ `F11`).
  * Select Compiler _Options/Code Generation_.
  * Enable Range checking and Overflow error checking (see image belows).



[![tutpeg compiler options.png](https://wiki.freepascal.org/images/2/2c/tutpeg_compiler_options.png)](</File:tutpeg_compiler_options.png>)

## First steps

It's always a good idea to seperate gui related code from data structure definitions. So the first step will be the creation of a separate [unit](<Unit.md> "Unit") for our Solitaire data structures. 

  * From the menu choose _File/New Unit_.
  * Save the unit as _PegDatastructures.pas_ (and press the lowercase button that pops up).



The basic elements of a Peg Solitaire board are the marbles, the board structure and the empy places. We'll simulate this by a simple matrix that has cells of a certain type (empty, occupied and not accessible). And we'll encapsulate all this in a [class](<Class.md> "Class") that handles all the data manipulation. 

  * Add the following code to the PegDatastructures unit (after the classes in the _[uses](<Uses.md> "Uses")_ section and before the _[implementation](<Implementation.md> "Implementation")_ section):


    
    
    const
      C_MAX = 7;  // Max board size: 7x7
    
    type
      TCellNums = 1..C_MAX;
      TCellType = (ctNoAccess, ctEmpty, ctPeg);
      TPegCells = array[TCellNums, TCellNums] of TCellType;
    
      TPegSolitaire = class
      private
        Size: TCellNums;
        PegCells: TPegCells;
        
      public
        constructor Create(const pSize: TCellNums);
      end;
    

It's fair to assume that other code that is going to use this class needs access to the cells contents (i.e. PegCells). The way to handle this is either by defining a set of [functions](<Function.md> "Function") to access the cells or define a so called [array](<Array.md> "Array") property. Let's go for the latter approach and add the following line to the public section of the TPegSolitaire class: 
    
    
    property Cell[const pRow, pCol: TCellNums]: TCellType;
    

  * Position the text cursor on the constructor line.
  * Press `Ctrl`+`⇧ Shift`+`C`: the IDE generates the constructor body (as we expected) but it also generates the empty bodies for the 2 [methods](<Method.md> "Method") that give us access to _PegCells_ via the _Cells_ property.
  * GetCell retrieves data from the private [variable](<Variable.md> "Variable") PegCells. Add the following code to the [function](<Function.md> "Function"):


    
    
    result := PegCells[pRow,pCol]
    

  * SetCell populates the PegCells array with data. Add the following code to the [procedure](<Procedure.md> "Procedure"):


    
    
    PegCells[pRow,pCol] := pValue
    

  * And now finalize the Create [constructor](<Constructor.md> "Constructor"). Add this code to it's body:


    
    
    var iRow,iCol: integer;
    begin
      // Store the size of the board locally
      Size := pSize;
    
      // Initialize all cells to 'not accessible'
      for iRow := 1 to C_MAX do
        for iCol := 1 to C_MAX do
          Cell[iRow,iCol] := ctNoAccess;
    

Now that the basic data structure is in place, let's have a look at the graphics we are going to need. There are many ways to display a solitaire board. We are going to use a paintbox. That will give us full control over the graphic features that Lazarus gives us out of the box. 

Our main form is going to use the data structure we defined in the PegDatastructure file. 

  * Open the main form's sourcefile.
  * Add PegDatastructures it to the uses list at the top of the file:


    
    
    uses
      PegDatastructures,
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs;
    

  * Press `F12` (this will give us the form editor).
  * From the _[Standard](<Standard_tab.md> "Standard tab")_ palette choose a [TButton](<TButton.md> "TButton") and drop it on the form (in the upper left corner).
  * Change the caption to _Test paint_.
  * On the _[Additional tab](<Additional_tab.md> "Additional tab")_ select [TPaintBox](<TPaintBox.md> "TPaintBox") and drop it on the form.
  * Change Align to _alRight_.
  * Change BordSpacing.Around to _4_.
  * Change Anchors.akLeft to _true_.
  * Change Name to _pbPeg_.
  * Resize the form so it looks something like this:



[![tutpeg empty.png](https://wiki.freepascal.org/images/5/59/tutpeg_empty.png)](</File:tutpeg_empty.png>)

Next step is to draw the cells matrix on this paintbox by splitting it in rows and columns that will contain each cell of the board. To make it scaleable we'll calculate the width and height independantly. We'll need a couple of variables to hold the results. First the width and height of the cells, so all cells (7) fit exactly on the form. We'll develop the painting code interactively and we are going to use the button that was dropped on the form for that. 

  * Double click the test paint button (this creates the event handler).
  * Add 2 variables:


    
    
    var
      CellWidth : integer;
      CellHeight: integer;
    

We'll need some extra variables to hold intermediate results: 

  * Add 3 local variables:


    
    
      iRow, iCol: TCellNums;
      CellArea  : TRect;
    

CellArea is used to limit the rectangular area on screen where a cell will be drawn. 

To process all rows and cols two straightforward [for loops](<For.md> "For") will do. 

  * Add the following code to the event handler:


    
    
      // Calculate the width/height of each cell to accomodate for all cells in the paintbox
      CellWidth := pbPeg.Width div 7;
      CellHeight := pbPeg.Height div 7;
    
      // Draw boxes for all cells
      for iRow := 1 to 7 do
        for iCol := 1 to 7 do
        begin
          // Calculate the position of the cell in the paintbox
          CellArea.Top    := (iRow-1) * CellHeight;
          CellArea.Left   := (iCol-1) * CellWidth;
          CellArea.Right  := CellArea.Left + CellWidth;
          CellArea.Bottom := CellArea.Top  + CellHeight;
          // And now draw the cell
          pbPeg.Canvas.Rectangle(CellArea);
        end;
    

A Canvas, as it's name suggests, is a control that helps us drawing things like lines, rectangles, circles etc. A paintbox control has an embedded canvas. Therefore the line _pbPeg.Canvas.Rectangle(CellArea)_ will draw a rectangle on the paintbox, limited to the area defined in _CellArea_. And because the paintbox is placed on the form, we can see the result there. 

  * Compile and run the program (press `F9`).
  * Press the _Test paint_ button.
  * Maximize the form (the cells disappear; don't worry we'll fix that).
  * Press the _Test paint_ button again.



This proves that our calculations were spot on and that we now have the means to draw whatever is necessary in the right spot. One thing needs to be done though before adding more drawing functionality. Drawing of the cells has no relation with the main form whatsoever (or any form for that matter). The only thing we need for drawing things is a Canvas and some measurements for the cells. So we are going to create a supporting class to clean up the main form. 

  * Create a new unit (File/New Unit).
  * Save it as PegSolPainter.pas (File/Save and then and choose _Rename to lowercase_).
  * Add a new class to the new unit file that will do all of the drawing (after the uses section, before the implementation section).


    
    
    type
      TPegSolPainter = class
      private
        PegSol      : TPegSolitaire;
        Canvas      : TCanvas;
    
      public
        constructor Create(pPegSol: TPegSolitaire; pCanvas: TCanvas);
      end;
    

Note that a TPegSolitaire variable is also added, because obviously we are going to use that class to retrieve a cells' state. 

  * Position the text cursor in the constructor line and press `Ctrl`+`⇧ Shift`+`C`.
  * The constructor must store the 2 parms locally:


    
    
    constructor TPegSolPainter.Create(pPegSol: TPegSolitaire; pCanvas: TCanvas);
    begin
      PegSol := pPegSol;
      Canvas := pCanvas;
    end;
    

Trying to compile this code will failse because we haven't added the PegDatastrucures unit to the uses section. And because we use a TCanvas we'll have to add the Graphics unit as well. 

  * Add _PegDatastructures_ and _Graphics_ to the _uses_ list.


    
    
    uses
      PegDatastructures,
      Graphics,
      Classes, SysUtils;
    

The reason we built this class was to remove all drawing and painting code from the form. So we need to create a method that will do the drawing. Before adding that method there is something that needs to be addressed: to calculate the width of a cell, we divide the paintbox width by the number of cells. In theory we could use the property Canvas.Width for this. However that property does not always give us the right width at the right time. So to be able to draw cells, we must provide our draw method with the correct values for the width and height of the canvas. 

Now we know this, we can add a paint method to our class. 

  * Add procedure Repaint to the class.


    
    
      TPegSolPainter = class
      private
        PegSol      : TPegSolitaire;
        Canvas      : TCanvas;
    
      public
        constructor Create(pPegSol: TPegSolitaire; pCanvas: TCanvas);
        procedure Repaint(const pCanvasWidth, pCanvasHeight: integer);
      end;
    

  * Generate the body of the procedure (`Ctrl`+`⇧ Shift`+`C`).
  * Copy the code from _TfrmMain.Button1Click(Sender: TObject)_ to this newly created method:


    
    
    procedure TPegSolPainter.Repaint(const pCanvasWidth, pCanvasHeight: integer);
    var
      CellWidth    : integer;
      CellHeight   : integer;
      iRow, iCol   : TCellNums;
      CellArea     : TRect;
    begin
      // Calculate the width of each cell to accomodate for all cells
      CellWidth := pbPeg.Width div 7;
      CellHeight := pbPeg.Height div 7;
    
      // Draw boxes for all cells
      for iRow := 1 to 7 do
        for iCol := 1 to 7 do
        begin
          // Calculate the position of the cell in the paintbox
          CellArea.Top    := (iRow-1) * CellHeight;
          CellArea.Left   := (iCol-1) * CellWidth;
          CellArea.Right  := CellArea.Left + CellWidth;
          CellArea.Bottom := CellArea.Top  + CellHeight;
          // And now draw the cell
          pbPeg.Canvas.Rectangle(CellArea);
        end;
    end;
    

Because we no longer need the paintbox pbPeg we must remove the references to it. Three changes are needed: 

  * Change the calculation of the CellWidth and CellHeight to:


    
    
      // Calculate the width of each cell to accomodate for all cells
      CellWidth := pCanvasWidth div 7;
      CellHeight := pCanvasHeight div 7;
    

  * Change the drawing of the rectangle to:


    
    
          // And now draw the cell
          Canvas.Rectangle(CellArea);
    

Now that we have a class that can do the fancy painting for us, it's time to make use of it. We are going to use this paint class together with the PegSolitare class to draw the cells. 

  * In the main form source, locate the Button1Click method.
  * Remove all statements and variables.
  * Add 2 new variables: one for the game class and one for the painter class:


    
    
    procedure TfrmMain.Button1Click(Sender: TObject);
    var
      pegsol  : TPegSolitaire;  // The game data
      pegpaint: TPegSolPainter; // The paint class for the game
    begin
    end;
    

  * Add _PegSolPainter_ to the uses list.



To use the paint class is fairley straightforward: create a new instance and call the repaint method like so: 

  * Add the following code to the Button1Click method:


    
    
      // Create a new game object
      pegsol := TPegSolitaire.Create(7);
    
      // Create a new painter object to paint this game
      pegpaint := TPegSolPainter.Create(pegsol, pbPeg.Canvas);
    
      // And paint the board
      pegpaint.Repaint(pbPeg.Width, pbPeg.Height);
    
      // Clean up
      pegpaint.Free;
      pegsol.Free
    

Run and test the program to see that it works exactly as before. The result will look something like this: [![tutpeg empty cells.png](https://wiki.freepascal.org/images/6/68/tutpeg_empty_cells.png)](</File:tutpeg_empty_cells.png>)

## It's all about events

What have we accomplished so far? There is a datastructure that holds all the data for a Peg Solitaire game, there is a class that can paint on a Canvas and we have a fairly simple form with a test button. So what's next? Events! 

As we have seen in the previous section, the cell matrix we painstakingly drew wasn't to last. This happens because the form doesn't know anything about our little game. As soon as the form thinks it's time to redraw itself, it does so and ignores our cell grid. What it dóes do is send a message to it's child controls that a refresh is necessary. This message is available to us as an event: a paint event. 

  * Open the form editor (select ufrmMain in the editor and press `F12`).
  * Select the paintbox pbPeg.
  * Open the Events tab in the Object Inspector.
  * One of the events in the list is OnPaint.



[![tutpeg onpaint.png](https://wiki.freepascal.org/images/1/15/tutpeg_onpaint.png)](</File:tutpeg_onpaint.png>)

This event is 'fired' everytime the paintbox needs to redraw it's surface. That is the place were we are going to do our drawing. 

  * Select the OnPaint event in the Object Inspector and click on the small button with three dots. This will generate the event handler body.
  * Copy/Paste the exact code from the _Button1Click_ event handler to this new method.


    
    
    procedure TfrmMain.pbPegPaint(Sender: TObject);
    var
      pegsol  : TPegSolitaire;  // The game data
      pegpaint: TPegSolPainter; // The paint class for the game
    begin
      // Create a new game object
      pegsol := TPegSolitaire.Create(7);
    
      // Create a new painter object to paint this game
      pegpaint := TPegSolPainter.Create(pegsol, pbPeg.Canvas);
    
      // And paint the board
      pegpaint.Repaint(pbPeg.Width, pbPeg.Height);
    
      // Clean up
      pegpaint.Free;
      pegsol.Free
    end;
    

  * Remove all code and variables from method _Button1Click_. Remember: the IDE will automatically remove this now empty method for us.
  * Remove the _Test paint_ button from the form.
  * Run the program to see what happens (ignore the fact that the background color is now different).



Now what is clear is that we create a game object in the OnPaint method, paint the empty cells and then destroy it. But we need to store the game object until a game has finished. The same goes for the paint object. So the OnPaint event is not the most logical place to create these objects. The form's class declaration is a better place to store them. 

  * Find the forms's declaration.
  * Add the 2 variables we created in the OnPaint event:


    
    
      TfrmMain = class(TForm)
        pbPeg: TPaintBox;
        procedure pbPegPaint(Sender: TObject);
      private
        { private declarations }
        pegsol  : TPegSolitaire;  // The game data
        pegpaint: TPegSolPainter; // The paint class for the game
      public
        { public declarations }
      end;
    

These variables must be initialized as soon as the form opens (or anytime we want to start a new game). So let's create a procedure that does that for us and add it to the private section of the form. 

  * Add procedure StartNewGame to the form.


    
    
      private
        { private declarations }
        pegsol  : TPegSolitaire;  // The game data
        pegpaint: TPegSolPainter; // The paint class for the game
    
        procedure StartNewGame;
    

  * Generate the body.
  * Add the initialzation code:


    
    
    procedure TfrmMain.StartNewGame;
    begin
      // Clean up the previous game
      pegpaint.Free;
      pegsol.Free;
    
      // Start with a new game
      pegsol := TPegSolitaire.Create(7);
      pegpaint := TPegSolPainter.Create(pegsol, pbPeg.Canvas);
    end;
    

Now that the initialization code is created, it must be executed. A logical time to do this is as soon as the form is created (i.e. the application is started). For this another event is available to us: the FormCreate event. We can create it in two ways: find the OnFormCreate event in the Object Inspector and click on the '...' button. But another way to generate it is double clicking the form itself. 

  * Open the form editor (press `F12`).
  * Double click somewhere in a free area on the form; do nót click on the paintbox.



[![tutpeg form create.png](https://wiki.freepascal.org/images/0/08/tutpeg_form_create.png)](</File:tutpeg_form_create.png>)

  * Add the call to the StartNewGame procedure:


    
    
    procedure TfrmMain.FormCreate(Sender: TObject);
    begin
      StartNewGame
    end;
    

Now that the game and paint objects are created at program start, they are no longer required in the OnPaint method. 

  * Locate procedure _procedure TfrmMain.pbPegPaint(Sender: TObject);_
  * Remove the local variables and all code except for the line that actualle does the painting.


    
    
    procedure TfrmMain.pbPegPaint(Sender: TObject);
    begin
      // Paint the board
      pegpaint.Repaint(pbPeg.Width, pbPeg.Height);
    end;
    

  * Run the program and see what happens.



## Intermezzo

So far we have focused on building a program to play the classic solitaire game in the 7x7 grid. Is it now possible to create smaller or bigger grids for other type games? Let's test this. 

  * In the main form locate the StartNewGame method.
  * Change the line _pegsol := TPegSolitaire.Create(7);_ to _pegsol := TPegSolitaire.Create(5);_. So a smaller board with 5x5 squares is created.
  * Run the program and see what happens.



As we can see on screen we still see a 7x7 matrix! This is the result of using magic numbers, which we should have avoided (see <http://en.wikipedia.org/wiki/Magic_number_%28programming%29#Unnamed_numerical_constants>). 

Using magic numbers equals disaster: it's a matter of _when_ , not _if_ a program will fail. 

  * Open PegSolPainter.
  * Locate the procedure _TPegSolPainter.Repaint(const pCanvasWidth, pCanvasHeight: integer);_.



In there we see the number of 7 a couple of times. This the magic number that plays tricks on us. In the calculation of the cell width and height, we need the size of the board as stored in the pegsol game variable. And the same goes for the iRow and iCol loops. So let's fix this once and for all: 
    
    
      // Calculate the width of each cell to accomodate for all cells
      CellWidth := pCanvasWidth div pegsol.Size;
      CellHeight := pCanvasHeight div pegsol.Size;
    
      // Draw boxes for all cells
      for iRow := 1 to pegsol.Size do
        for iCol := 1 to pegsol.Size do
        begin
          // Calculate the position of the cell in the paintbox
          CellArea.Top    := (iRow-1) * CellHeight;
          CellArea.Left   := (iCol-1) * CellWidth;
          CellArea.Right  := CellArea.Left + CellWidth;
          CellArea.Bottom := CellArea.Top  + CellHeight;
          // And now draw the cell
          Canvas.Rectangle(CellArea);
        end;
    

There's a caveat here: pegsol doesn't have a publicly accessible Size variable. And this is how it should be: all variables in a class should be private. The way to access those private _**values**_ is via functions or properties (the interface of the class). For this simple value we will use a property. 

  * Open PegDatastructures.
  * Rename the private variable in TPegSolitaire to _FSize_.
  * Add a public read only property to the class: property Size: TCellNums read FSize;



The class will now look like this: 
    
    
      TPegSolitaire = class
      private
        FSize: TCellNums;
        PegCells: TPegCells;
        function GetCell(const pRow, pCol: TCellNums): TCellType;
        procedure SetCell(const pRow, pCol: TCellNums; const pValue: TCellType);
    
      public
        constructor Create(const pSize: TCellNums);
        property Cell[const pRow, pCol: TCellNums]: TCellType read GetCell write SetCell;
        property Size: TCellNums read FSize;
      end;
    

It's common practice to prefix variables that are accessed via properties with a letter _F_. We've made the public access to the size property read-only, because it should never be changed, once the game is started. The only place where the private variable should get it's final value is in the Create constructor. 

  * Locate the constructor.
  * Change _Size := pSize;_ to _FSize := pSize;_ (if you don't you'll get a compilation error).
  * And while we're at it, the initialization needs a minor fix as well. There is no need to initialize cells we are not going to use. So the constructor should look like this (_C_MAX_ is replaced with _Size_):


    
    
    constructor TPegSolitaire.Create(const pSize: TCellNums);
    var iRow,iCol: integer;
    begin
      FSize := pSize;
      for iRow := 1 to Size do
        for iCol := 1 to Size do
          Cell[iRow,iCol] := ctNoAccess;
    end;
    

We are now ready to test the program and see if it now draws a nice 5x5 matrix. 

  * Run the program and see what happens (a 5x5 matrix is drawn).



Now that the basics of the game are in place, it's time to flesh out the gui. 

## Let's get artistic

So far so good. We now have a game class, a supporting paint class and an unimpressive checker board on our form (well, sort of). In our solitaire game a cell can have 3 states: not accessible, empty or occupied. For each cell type we want a different graphic representation. Let's get to it. 

  * In the editor open PegSolPainter (the unit with the paint class for our board).
  * Locate the Repaint method.
  * Locate the line where the cell is drawn: _Canvas.Rectangle(CellArea);_.



Obviously we need to make a change here. It depends on the cell state what needs to be drawn (not accessible cell, empty cell or peg cell). The case... operation comes to the rescue. 

  * Change the drawing of cells like so:


    
    
      // Draw boxes for all cells
      for iRow := 1 to pegsol.Size do
        for iCol := 1 to pegsol.Size do
        begin
          // Calculate the position of the cell in the paintbox
          CellArea.Top    := (iRow-1) * CellHeight;
          CellArea.Left   := (iCol-1) * CellWidth;
          CellArea.Right  := CellArea.Left + CellWidth;
          CellArea.Bottom := CellArea.Top  + CellHeight;
    
          // And now draw the cell based on the cell's contents
          case pegsol.Cell[iRow,iCol] of
    
            ctNoAccess: // Draw cells that are not accessible
              begin
                Canvas.Brush.Color := clGray;
                Canvas.Rectangle(CellArea);
              end;
    
            ctEmpty:    // Draw cells that are currently empty
              begin
                Canvas.Brush.Color := clBlue;
                Canvas.Rectangle(CellArea);
              end;
    
            ctPeg:      // Draw cells that are occupied
              begin
                Canvas.Brush.Color := clBlue;
                Canvas.Rectangle(CellArea);    // Erase the background first
                Canvas.Brush.Color := clGreen; 
                Canvas.Ellipse(CellArea);      // Draw the pegs as green circles
              end;
          end;
        end;
    

We could run the program at this stage (just try it), but it will only display a boring 5x5 grayish grid. This is because we didn't define any game setup yet. Let's fix this first. 

  * Open the main form source.
  * Locate the StartNewGame method.
  * Create a 7x7 game instead of 5x5.
  * Initialize a couple of cells. For example:


    
    
    procedure TfrmMain.StartNewGame;
    begin
      // Clean up the previous game
      pegpaint.Free;
      pegsol.Free;
    
      // Start with a new 7x7 game
      pegsol := TPegSolitaire.Create(7);
      pegpaint := TPegSolPainter.Create(pegsol, pbPeg.Canvas);
    
      // Initialize some cells
      pegsol.Cell[3,4] := ctEmpty;
      pegsol.Cell[4,2] := ctPeg;
      pegsol.Cell[4,3] := ctPeg;
      pegsol.Cell[4,4] := ctEmpty;
      pegsol.Cell[4,5] := ctPeg;
      pegsol.Cell[4,6] := ctPeg;
      pegsol.Cell[5,4] := ctEmpty;
    end;
    

  * Run the program (it'll look something like the image below).



[![tutpeg first pegs.png](https://wiki.freepascal.org/images/5/5c/tutpeg_first_pegs.png)](</File:tutpeg_first_pegs.png>)

## Populate the board

As we know a classic solitaire board should look something like this: 

[![tutpeg classic.png](https://wiki.freepascal.org/images/6/6e/tutpeg_classic.png)](</File:tutpeg_classic.png>)

And if we were to populate all cells individually, that would result in a lot of code. What if we could intialize the game by just passing it some text that would symbolically describe the board? Something like this: 
    
    
      // Initialize the cells to the classic game
      pegsol.InitializeBoard( '  ooo  ' + LineEnding +
                              '  ooo  ' + LineEnding +
                              'ooooooo' + LineEnding +
                              'ooo.ooo' + LineEnding +
                              'ooooooo' + LineEnding +
                              '  ooo  ' + LineEnding +
                              '  ooo  ' );
    

  1. **o** is an occupied cell.
  2. **.** is an empty but playable cell.
  3. The spaces indicate cells that are not accessible.



Let's assume this is going to work and create a method in the TPegSelitaire class that can handle this. 

  * Let's be optimistic (also called 'Top down design') and add the above code to TfrmMain.StartNewGame:


    
    
    procedure TfrmMain.StartNewGame;
    begin
      // Clean up the previous game
      pegpaint.Free;
      pegsol.Free;
    
      // Start with a new 7x7 game
      pegsol := TPegSolitaire.Create(7);
      pegpaint := TPegSolPainter.Create(pegsol, pbPeg.Canvas);
    
      // Initialize the cells to the classic game
      pegsol.InitializeBoard( '  ooo  ' + LineEnding +
                              '  ooo  ' + LineEnding +
                              'ooooooo' + LineEnding +
                              'ooo.ooo' + LineEnding +
                              'ooooooo' + LineEnding +
                              '  ooo  ' + LineEnding +
                              '  ooo  ' );
    end;
    

  * Open the PegDatastructures sourcefile.
  * Add this procedure to the public section of the class: _InitializeBoard(const pBoard: ansistring);_


    
    
      public
        constructor Create(const pSize: TCellNums);
        procedure InitializeBoard(const pBoard: ansistring);
    
        property Cell[const pRow, pCol: TCellNums]: TCellType read GetCell write SetCell;
        property Size: TCellNums read FSize;
    

  * Generate the body of InitializeBoard (`Ctrl`+`⇧ Shift`+`C`...).



Now what does this procedure actually need to do? It must split the textstring into seperate lines and then process those lines, Because we used _LineEnding_ tokens to separate the lines, we can use a _TStringList_ class to split them. 

  * Code _InitalizeBoard_ like so:


    
    
    procedure TPegSolitaire.InitializeBoard(const pBoard: ansistring);
    var lst      : TStringList;
        iRow,iCol: integer;
        s        : string;
    begin
      // Create a list with the board text in it. This will split all lines
      // into individual lines, because of the LineEnding 'splitter'.
      lst := TStringList.Create;
      lst.Text := pBoard;
    
      // Process all lines one at a time
      for iRow := 0 to lst.Count-1 do
        if iRow < Size then // Make sure there is no overflow in the rows
        begin
          // Process a single line of text
          s := lst[iRow];
          for iCol := 1 to length(s) do
            if iCol <= Size then  // Make sure there is no overflow in the columns
              case s[iCol] of
                ' ': Cell[iRow+1,iCol] := ctNoAccess;
                '.': Cell[iRow+1,iCol] := ctEmpty;
                'o': Cell[iRow+1,iCol] := ctPeg;
              end;
        end;
    
      // Clean up the list
      lst.Free;
    end;
    

  1. a TStringList is used as a buffer. This works because we used LineEnding as a separator between all lines.
  2. There are _Count_ number of lines, but they are numbered 0..Count-1. Our cells are numbered starting with 1. That's why you see _iRow+1_ in the cell assignments.



The above procedure contains a lot of extra variables and shouldn't be that difficult to understand. It's possible to reduce the procedure to the bare minimum like so: 
    
    
    procedure TPegSolitaire.InitializeBoard(const pBoard: ansistring);
    var iRow,iCol: integer;
    begin
      with TStringList.Create do
      begin
        Text := pBoard;
        for iRow := 0 to Min(Count-1, Size-1) do
          for iCol := 1 to Min(length(Strings[iRow]),Size) do
            case Strings[iRow][iCol] of
              ' ': Cell[iRow+1,iCol] := ctNoAccess;
              '.': Cell[iRow+1,iCol] := ctEmpty;
              'o': Cell[iRow+1,iCol] := ctPeg;
            end;
        Free;
      end;
    end;
    

This procedure does exactly the same but it uses the handy feature that you can use the With statement together with dynamically created objects. For this procedure to work add the Math unit to the uses section. 

  * Run the program. All cells are now populated like in the classic Peg Solitaire game.



## Events revisited

It doesn't look too impressive yet, but we have done quite a lot. The main thing missing at the moment is being able to leap cells to remove them from the board. And for that we are going to use the mouse. But how? Let's investigate the possibilities. 

  * Drop a [TMemo](<TMemo.md> "TMemo") on the form, to the left of the paint box pbPeg (it's on the _Standard_ tab of the component palette).
  * In the Object Inspector change Align to _alClient_. It will occupy all remaining space that is left of the paintbox.
  * Change BorderSpacing.Around to _4_.
  * Clear the Lines property (click on the 3-dotted button and erase the text). The result should look like this:



[![tutpeg with memo.png](https://wiki.freepascal.org/images/9/91/tutpeg_with_memo.png)](</File:tutpeg_with_memo.png>)

  * Select the paintbox pbPeg.
  * In the Object Inspector select the Events tab.



There are 2 events that are interesting to us: OnMouseDown and OnMouseUp. Let's see what happens whith the OnMouseDown event. 

  * In the Object Inspector select the OnMouseDown event.
  * Click on the 3-dotted button (this generates the body of the event procedure).



The header of the generated procedure contains a number of variables. Two of them are X and Y. They contain the position on the paintbox where the mouse button was pressed. Let's make them visible by adding some information to the memo component we've placed on the form. 

  * Add the following statement to the procedure; it adds a formatted line to the memo displaying the position where the mouse button was pressed:


    
    
    Memo1.Append( format('Mouse down @ %dx%d',[X,Y]) )
    

  * Run the program (press `F9`).
  * Click on the game board at several different locations. The memo box will display where the mouse button was pressed.
  * End the program.



Obviously we cannot use the 'raw' X and Y coordinates; they are too high for our 7x7 game board. They must be translated to the cell's row and column coordinates. For that we need a function that translates a X/Y coordinate to a cell coordinate. For that we need the width and height of the paintbox, the number of cells in the grid and the width/height of an individual cell. The one place where all this comes together is... the paint class. So that is the obvious place to do this calculation. 

  * Open the PegDatastructures file.
  * Add the _TCellPosition_ record type to store cell coordinates:


    
    
    type
      TCellNums = 1..C_MAX;
      TCellType = (ctNoAccess, ctEmpty, ctPeg);
      TPegCells = array[TCellNums, TCellNums] of TCellType;
    
      TCellPosition = record
        Row: TCellNums;
        Col: TCellNums;
      end;
    

  * Open the PegSolPainter class file.
  * Add the public function CanvasXYtoCell(...) to the class:


    
    
      public
        constructor Create(pPegSol: TPegSolitaire; pCanvas: TCanvas);
    
        procedure Repaint(const pCanvasWidth, pCanvasHeight: integer);
        function CanvasXYtoCell(const pX, pY: integer): TCellPosition;
    

  * Generate the body (`Ctrl`+`⇧ Shift`+`C`).
  * Add the following code:


    
    
    function TPegSolPainter.CanvasXYtoCell(const pX, pY: integer): TCellPosition;
    begin
      result.Col := (pX div CellWidth)  + 1;
      result.Row := (pY div CellHeight) + 1;
    end;
    

The above code maps the large X/Y coordinate on the actual cell's row/col numbers. There is one problem though: the CellWidth and CellHeight are not available. They are intermediate results in the Repaint procedure and not stored. Let's fix that as well. 

  * Add _CellWidth_ and _CellHeight_ as private variables to the _TPegSolPainter_ class:


    
    
      TPegSolPainter = class
      private
        PegSol      : TPegSolitaire;
        Canvas      : TCanvas;
        CellWidth   : integer;
        CellHeight  : integer;
    

  * Remove the variables _CellWidth_ and _CellHeight_ from the variable list in the Repaint procedure:


    
    
    procedure TPegSolPainter.Repaint(const pCanvasWidth, pCanvasHeight: integer);
    var
      iRow, iCol   : TCellNums;
      CellArea     : TRect;
    begin
    

Now we can modify the procedure where the mouse down coordinate was processed. With the new function the X/Y position can now be translated to an actuel cell coordinate. 

  * Open the main form sourcefile.
  * Locate the _pbPegMouseDown_ procedure.
  * Change the code to:


    
    
    procedure TfrmMain.pbPegMouseDown(Sender: TObject; Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
    var CellRC: TCellPosition;
    begin
      CellRC := pegpaint.CanvasXYtoCell(X,Y);
      Memo1.Append( format('Mouse down @ %dx%d',[CellRC.Row, CellRC.Col]) )
    end;
    

Let's do the same for the OnMouseUp event: 

  * Generate the procedure body for the OnMouseUp event.
  * Add this code:


    
    
    procedure TfrmMain.pbPegMouseUp(Sender: TObject; Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
    begin
      with pegpaint.CanvasXYtoCell(X,Y) do
        Memo1.Append( format('Mouse up @ %dx%d',[Row, Col]) )
    end;
    

Note that this method does exactly the same as the OnMouseDown, but using the _with_ construct saves us the trouble of declaring a local variable (less is more...). 

Let's test this: 

  * Run the program.
  * Click on the upper left most cell and hold the mouse down.
  * Move the mouse cursor to another cell.
  * Release the mouse button.



Every time you press the mouse button and release it, the exact cell where the mouse button was pressed or released is displayed in the memo box. 

## Gluing things together

This program works a bit like the calculator program: 

  1. The user selects a peg to move by pressing the mouse button.
  2. The user moves the peg over another peg to an empty cell (leaps).
  3. The user release the mouse button to execute the leap.



Here we do not remember a number but a cell (-position). So as soon as the user presses the mouse button on a cell, we must store that position somewhere. 

  * Open the main form's sourcefile.
  * Add a variable _FromCell_ the the form's private variables.


    
    
      private
        { private declarations }
        pegsol  : TPegSolitaire;  // The game data
        pegpaint: TPegSolPainter; // The paint class for the game
        FromCell: TCellPosition;  // The peg that is going to leap
    

  * Locate the OnMouseDown event handler.
  * Add the following line to that procedure:


    
    
    FromCell := pegpaint.CanvasXYtoCell(X,Y);
    

Now as soon as the user releases the mouse button, we must execute the leap (verify if it's ok and update the board). Let's be optimistic (remember the Top Down remark?) and assume there is a procedure that does that for us: Leap(<FromCell>, <ToCell>). 

  * Locate the OnMouseUp event handler and add the following line:


    
    
    pegsol.Leap(FromCell, pegpaint.CanvasXYtoCell(X,Y));
    

_FromCell_ we already populated when the mouse button was pressed. The target cell is simply calculated by calling pegpaint.CanvasXYToCell(...). We could introduce a local variable for that, but it's not really necessary. 

  * Add the following line to the procedure as well:


    
    
    pbPeg.Repaint;
    

This will force the paintbox to repaint itself, so all updated cells show the correct contents. 

The complete OnMouseUp method now looks like this: 
    
    
    procedure TfrmMain.pbPegMouseUp(Sender: TObject; Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
    begin
      with pegpaint.CanvasXYtoCell(X,Y) do
        Memo1.Append( format('Mouse up @ %dx%d',[Row, Col]) );
      pegsol.Leap(FromCell, pegpaint.CanvasXYtoCell(X,Y));
      pbPeg.Repaint;
    end;
    

Before going into the Leap procedure, a bit of extra functionality is introduced. Remember that we can retrieve the state of a cell (empty, peg or no access) via the Cell property of TPegSolitaire? For example _pegsol.Cell[2,2]_ would give us the state of the cell in row 2 and column 2. But in the mean time we have introduced a new datastructure: TCellPosition. So for example if we want to get the state of the _FromCell_ we would need the following awkward statement: 
    
    
    state := pegsol.Cell[ FromCell.Row, FromCell.Col ]
    

It would be nice if we could write something like: 
    
    
    state := pegsol.GetCell(FromCell)
    

Let's add this to our game class. 

  * Open sourcefile PegDatastructures.
  * Add the private function: _GetCell(const pPosition: TCellPosition): TCellType;_


    
    
      TPegSolitaire = class
      private
        FSize: TCellNums;
        PegCells: TPegCells;
    
        function GetCell(const pRow, pCol: TCellNums): TCellType;
        procedure SetCell(const pRow, pCol: TCellNums; const pValue: TCellType);
        function GetCell(const pPosition: TCellPosition): TCellType;
    

There now are two functions with the name GetCell, but the parameter list is different. So Lazarus knows exactly when to call which function. 

  * Generate the body of the function (`Ctrl`+`⇧ Shift`+`C`).
  * Insert the following code:


    
    
    result := Cell[pPosition.Row, pPosition.Col]
    

Back to the main storyline. We have introduced a procedure _Leap(...)_ , but it doesn't exist yet. It is a method that changes the game board, so the place to put it is the TPegSolitaire class. 

  * Open sourcefile PegDatastructures.
  * Add procedure Leap to the public section of the TPegSolitaire class.


    
    
      public
        constructor Create(const pSize: TCellNums);
        procedure InitializeBoard(const pBoard: ansistring);
        procedure Leap(const pFromCell, pToCell: TCellPosition);
    

  * Generate the procedure body (`Ctrl`+`⇧ Shift`+`C`).



Now this procedure does all the hard work: check if the cells are valid and if the leap is at all possible. This is not difficult, but the number of checks is quite extensive for something that looks simple. The code is fairly self-explanatory and if not, hopefully the comments explain what is going on. Note that the procedure uses the GetCell(<cellposition>) function we created earlier. 
    
    
    procedure TPegSolitaire.Leap(const pFromCell, pToCell: TCellPosition);
    var dx, dy: integer;
        JumpedCell: TCellPosition;
    begin
      // Verify that the start cell is occupied and the target cell is empty
      // If not, leave the procedure via the EXIT.
      if (GetCell(pFromCell) <> ctPeg) or
         (GetCell(pToCell) <> ctEmpty) then
        EXIT;
    
      // Calculate the horizontal and vertical distance between the cells
      dx := abs(pFromCell.Col - pToCell.Col);
      dy := abs(pFromCell.Row - pToCell.Row);
    
      // A valid move has one direction equal to zero and the other equal to 2
      if    ((dx = 2) and (dy = 0))
         or ((dx = 0) and (dy = 2)) then
      begin
        // Determine the position of the jumped cell; it's in the middle
        JumpedCell.Col := (pFromCell.Col + pToCell.Col) div 2;
        JumpedCell.Row := (pFromCell.Row + pToCell.Row) div 2;
    
        // Final check: is there a peg in the jumped cell?
        if GetCell(JumpedCell) = ctPeg then
        begin
          // Jump: clear the FromCell, empty the jumped cell and populate the ToCell
          Cell[pFromCell.Row, pFromCell.Col]   := ctEmpty;
          Cell[JumpedCell.Row, JumpedCell.Col] := ctEmpty;
          Cell[pToCell.Row, pToCell.Col]       := ctPeg;
        end;
      end;
    end;
    

Believe it or not but we now have a working Peg Solitaire game program! 

  * Run the program.
  * Press the mouse button on one peg cell.
  * Leap over another peg cell to an empty cell.
  * Release the mouse button.
  * Repeat...



## Cleaning up

On the form we still have the memo component that displays mouse clicks. We don't really need it anymore. 

  * Delete the memo from the main form.
  * Delete the memo updates from the MouseDown and MouseUp events.


    
    
    procedure TfrmMain.pbPegMouseDown(Sender: TObject; Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
    begin
      FromCell := pegpaint.CanvasXYtoCell(X,Y);
    end;
    
    procedure TfrmMain.pbPegMouseUp(Sender: TObject; Button: TMouseButton; Shift: TShiftState; X, Y: Integer);
    begin
      pegsol.Leap(FromCell, pegpaint.CanvasXYtoCell(X,Y) );
      pbPeg.Repaint;
    end;
    

  * Open the form editor (`F12`).
  * Select the paintbox pbPeg
  * In the Object Inspector change property Align to _alClient_ (look on the Properties tab).
  * Remove the unit _stdctrls_ from the uses list. The compiler would othwerwise give a hint in the Messages window that it isn't used (it was automatically added when we dropped the memo component on the form).



### What's next?

To make it a fully functional game we need a way to change the board without recompiling the application. And the graphics could be improved as well. Let's address the first issue first. 

## Creating more board layouts

It would be nice if we could add more boards to our game without recompiling the application. The obvious place would be to store the board layouts in external textfiles. The user can then start a new game by opening one of the textfiles. The fun thing is, it's easy to add because we've already done most of the hard work. First let's make two board layouts. 

  * From the menu choose _File/New..._ ; this opens a pop up dialog.
  * Choose the option Module/Text and click OK.
  * Type the following text (it should look familiar :-) ):


    
    
      ooo
      ooo
    ooooooo
    ooo.ooo
    ooooooo
      ooo
      ooo
    

  * From the menu choose File/Save.
  * Save the file in the projects' bin folder as _classic.txt_.



Add another file: 

  * From the menu choose _File/New..._.
  * Choose the option Module/Text and click OK.
  * Type the following text:


    
    
      ...
      .o.
    ..ooo..
    .ooooo.
    ooooooo
      ...
      ...
    

  * From the menu choose File/Save.
  * Save the file in the bin folder as _triangle.txt_.



There are many ways to create and open new boards. In this tutorial we'll use the menu driven approach (a thorough description of the menu is something for another tutorial). 

  * Open the main form's editor.
  * Select the [TMainMenu](<TMainMenu.md> "TMainMenu") control from the Standard components palette.
  * Click on the form (it doesn't matter if you 'hit' the paintbox). An icon is displayed showing that the menu is now availabe.
  * Double click on the icon with label MainMenu1. This opens the menu editor with one item aldready available, labeled _New Item1_.
  * In the Object Inspector change Caption to _& File_ (include the ampersand; it's used to draw the underscore symbol when you press Alt to access the main menu).
  * Right click on the File item in the Menu Editor and select _Create Submenu_ from the pop up window.
  * Change the Caption to _E &xit_ (this adds a familiar application exit button).
  * Right click on the File item.
  * Select _Insert New Item (after)_ from the pop up menu.
  * Change the caption of that new menu to _& Game_.
  * Right click on the Game menu and select _Create Submenu_ from the pop up..
  * Change the caption to _Load from file..._.



The menu will now look something like this: 

[![tutpeg mainmenu.png](https://wiki.freepascal.org/images/c/c7/tutpeg_mainmenu.png)](</File:tutpeg_mainmenu.png>)

The menu is also visible at the top of the main form. 

  * Click on the _File_ menu item; this opens the submenu and _Exit_ is visible.
  * Double click on _Exit_. As you might have guessed this creates an event handler. In other words the procedure that gets executed as soon as the user clicks on this menu item.
  * Add the following not too complex code to the generated procedure:


    
    
    Close
    

This will close the main form. And because it is the only form of our application it will end the application. 

Okay now it's time to look at how to open the board files. There are standard components and menu's available to do this, but let's do it the manual way. 

  * Open the main form's editor.
  * In the component palette select the _[Dialogs tab](<Dialogs_tab.md> "Dialogs tab")_.
  * Select the [TOpenDialog](<TOpenDialog.md> "TOpenDialog") component (click on it).
  * Click on the main form (anywhere). A OpenDialog1 component is now visible. This component provides us with a kind of explorer pop up with which we can select files. The main form now looks like this:



[![tutpeg mainform.png](https://wiki.freepascal.org/images/b/bd/tutpeg_mainform.png)](</File:tutpeg_mainform.png>)

  * In the Object Inspector click on the Filter property and then on the 3-dotted button. This pops up the filter dialog. This is the place where we tell the file dialog what files we allow the user to select.
  * We only want to see text files so add this filter line: _Peg Solitaire textfiles (*.txt) | *.txt_ (see image below).
  * Press OK.



[![tutpeg filterdialog.png](https://wiki.freepascal.org/images/d/d3/tutpeg_filterdialog.png)](</File:tutpeg_filterdialog.png>)

  * In the Object Inspector change Options.ofFileMustExist to _true_. This forces the dialog to only allow existing files to be selected.



So far so good. Now to actually open a file when the menu item is selected: 

  * On the main form click on the Game menu item. This opens the drop down menu.
  * Click on _Load from file..._. As expected this generates an.... event handler.
  * Add the following code to the Event handler:


    
    
      if OpenDialog1.Execute then
        ShowMessage(OpenDialog1.FileName);
    

Remember: small steps. We have now added a menu to the form to exit the application and to load a game board set up from disk. We haven't actually coded the loading part, but let's first try the code we have so far. 

  * Run the application.
  * From the menu choose _Game/Load from file..._.
  * Select _triangle.txt_ and press Open.
  * A window pops up that shows us the selected filename.
  * End the program.



Now let's finish the board loading code. The code is pretty self-explanatory and reuses some of the procedures we have created so far. 

  * Open the main form's editor.
  * From the menu choose _Game/Load from file..._. This will open the event handler.
  * Replace the code with this:


    
    
    procedure TfrmMain.MenuItem4Click(Sender: TObject);
    begin
      // Open the pop up dialog
      if OpenDialog1.Execute then
      begin
        // Start with a new empty board
        StartNewGame;
    
        // Dynamically create a stringlist to load the board layout
        with TStringList.Create do
        begin
          // Load the board layout from the textfile
          LoadFromFile(OpenDialog1.FileName);
    
          // Initialize the board with the file's contents
          pegsol.InitializeBoard(Text);
    
          // Clean up the stringlist
          Free
        end;
    
        // After loading the new board update the paintbox
        pbPeg.Repaint;
      end;
    end;
    

And that's all there is to it! 

Run the program, open a board set up file and play the game! 

## Eye candy

We now have a working Peg Solitaire application. There's still plenty of room for improvent, but the game itself is working nicely. The last part of this tutorial is about improving the looks of the game. Sure the game works, but a bit more detail in the graphics would be nice. And that's pretty easy to add. All we need are three images for the cell types, update the painter class and in the main form, tell the painter to use the images. So let's add the three images to the painter class: 

  * Open the PegSolPainter sourcefile.
  * Add the 3 images to the private variables section:


    
    
      TPegSolPainter = class
      private
        PegSol     : TPegSolitaire;
        Canvas     : TCanvas;
        CellWidth  : integer;
        CellHeight : integer;
    
        ImgNoAccess: TPicture;
        ImgEmpty   : TPicture;
        ImgPeg     : TPicture;
    

  * Add a procedure to the public section for loading the images.


    
    
      public
        constructor Create(pPegSol: TPegSolitaire; pCanvas: TCanvas);
        procedure Repaint(const pCanvasWidth, pCanvasHeight: integer);
        function CanvasXYtoCell(const pX, pY: integer): TCellPosition;
        procedure LoadImage(const pCellType: TCellType; const pFilename: string);
    

  * Generate the body for the procedure.
  * Add the code:


    
    
    procedure TPegSolPainter.LoadImage(const pCellType: TCellType; const pFilename: string);
    
      procedure UpdateImage(var pImg: TPicture; const pNewImage: TPicture);
      begin
        pImg.Free;
        pImg := pNewImage
      end;
    
    var pic: TPicture;
    begin
      // Make sure the file exists
      if FileExists(pFilename) then
      begin
        // First load the picture in a local variable
        pic := TPicture.Create;
        pic.LoadFromFile(pFilename);
    
        // Now update the required image based on the pCelltype parm
        case pCelltype of
          ctNoAccess: UpdateImage(ImgNoAccess, pic);
          ctEmpty   : UpdateImage(ImgEmpty, pic);
          ctPeg     : UpdateImage(ImgPeg, pic);
        end;
      end;
    end;
    

The code again is self-explanatory. Note that we use a short local procedure to update an image. If we wouldn't do that then the case statement (_case pCellType of_) would be more complex (i.e. contain more redundant code). Now each cell type only needs one line to update the right image. 

We've added the pictures to the class. Now we must use them and that of course happens in the Repaint procedure. For each celltype we check whether an image is available. And if so, use that image instead of the plain drawing. 

  * Locate the Repaint method.
  * In the case statement, change the ctNoAccess, ctEmpty and ctPeg sections to:


    
    
          // And now draw the cell based on the cell's contents
          case pegsol.Cell[iRow,iCol] of
    
            ctNoAccess: // Draw cells that are not accessible
              if not assigned(ImgNoAccess) then
                begin
                  Canvas.Brush.Color := clGray;
                  Canvas.Rectangle(CellArea);
                end
              else with ImgNoAccess do
                Canvas.CopyRect(CellArea, Bitmap.Canvas, Rect(0,0,Width,Height));
    
    
            ctEmpty:    // Draw cells that are currently empty
              if not assigned(ImgEmpty) then
                begin
                  Canvas.Brush.Color := clBlue;
                  Canvas.Rectangle(CellArea);
                end
              else with ImgEmpty do
                Canvas.CopyRect(CellArea, Bitmap.Canvas, Rect(0,0,Width,Height));
    
    
            ctPeg:      // Draw cells that are occupied
              if not assigned(ImgPeg) then
                begin
                  Canvas.Brush.Color := clBlue;
                  Canvas.Rectangle(CellArea);  // Erase the background first
                  Canvas.Brush.Color := clGreen;
                  Canvas.Ellipse(CellArea);
                end
              else with ImgPeg do
                Canvas.CopyRect(CellArea, Bitmap.Canvas, Rect(0,0,Width,Height));
    
          end;
    

In the main form locate the StartNewGame method and add the image loading code to the painter class. 

  * Open the main form editor.
  * Locate method _StartNewGame_.
  * Add LoadImage statements after the creation of the _pegpaint_ object:


    
    
      // Start with a new 7x7 game
      pegsol := TPegSolitaire.Create(7);
      pegpaint := TPegSolPainter.Create(pegsol, pbPeg.Canvas);
    
      pegpaint.LoadImage(ctNoAccess, 'tutpeg_cellforbidden.jpg');
      pegpaint.LoadImage(ctEmpty,    'tutpeg_cellempty.jpg');
      pegpaint.LoadImage(ctPeg,      'tutpeg_celloccupied.jpg');
    

Make sure that the above images are downloaded from below and put in the program's _bin_ folder, or better yet, create your own images. Note: to download these images, click on them first thén download them. 

[![tutpeg cellforbidden.jpg](https://wiki.freepascal.org/images/7/76/tutpeg_cellforbidden.jpg)](</File:tutpeg_cellforbidden.jpg>) [![tutpeg cellempty.jpg](https://wiki.freepascal.org/images/c/c0/tutpeg_cellempty.jpg)](</File:tutpeg_cellempty.jpg>) [![tutpeg celloccupied.jpg](https://wiki.freepascal.org/images/e/eb/tutpeg_celloccupied.jpg)](</File:tutpeg_celloccupied.jpg>)

  * Now test the solitaire game and hopefully it looks something like the image at the beginning of this tutorial...



## Final tweaks

Most programs have their own icons that are displayed in menu's and in explorer windows. It's easy to add your own custom icon to an application. 

  * From the menu choose _Project/Project Options..._.
  * Click on _Project Options_ in the pop up dialog (right at the top). This will display the window with the application icon.
  * Click on _Load Icon_ and load your icon of choice; or download the marble that comes with this tutorial:



[![tutpeg marble.png](https://wiki.freepascal.org/images/4/47/tutpeg_marble.png)](</File:tutpeg_marble.png>)

  * Press OK.



The application will now have a nice marble icon in the form's title bar and in explorer windows.

---

_Source: [https://wiki.freepascal.org/Peg_Solitaire_tutorial](https://web.archive.org/web/20250405075554/https://wiki.freepascal.org/Peg_Solitaire_tutorial)_
