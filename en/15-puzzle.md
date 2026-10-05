# 15-puzzle

│ **English (en)** │  **[suomi (fi)](</15-puzzle/fi> "15-puzzle/fi")** │  **[français (fr)](</15-puzzle/fr> "15-puzzle/fr")** │    
****

## Contents

  * 1 15-puzzle
  * 2 Create code
  * 3 Source code
    * 3.1 unit1.pas
    * 3.2 Unit1.lfm
    * 3.3 unitGameBoard.pas
  * 4 See also



## 15-puzzle

[![puzzle15.png](https://wiki.freepascal.org/images/f/f9/puzzle15.png)](</File:puzzle15.png>)

The 15-puzzle is a sliding puzzle that consists of a frame of numbered square tiles in random order with one tile missing. The object of the puzzle is to place the tiles in order by making sliding moves that use the empty space. 

## Create code

  * Create a new blank GUI application with the form [Form1](<TForm.md> "TForm")
  * Change caption Form1 to _15Puzzle'_.
  * Add New [Unit](<Unit.md> "Unit") [unitGameBoard.pas](<15-puzzle.md> "15-puzzle")
  * Create the OnCreate event handler for the form, by clicking on your form, use the Object Inspector, the tab events, select the OnCreate event and click the button [...] or double click the button in the form.
  * Add following code:


    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      i : Integer;
      aButton: TButton;
      point:TPoint;
    begin
      Randomize;
      Puzzle15:= TGameBoard.Create;
      i := 1;
      while i < 16 do
        begin                              //create 15 Buttons
          aButton:=TButton.Create(Self);   //create Button, Owner is Form1, where the button is released later
          aButton.Parent:=Self;            //determine where it is to be displayed
          aButton.Caption:=IntToStr(i);    //Captions of the buttons
          aButton.Width:=aButton.Height;   //Width should correspond to the height of the buttons
          point := Puzzle15.ButtonPlace(aButton.Caption);
          aButton.Left:= point.x* aButton.Width;    //Distance from left
          aButton.Top := point.y* aButton.Height;
    
          aButton.OnClick:=@aButtonClick; //the event handler for the button -> will be created yet
          inc(i);
        end;
      Self.Height:=aButton.Height*6;      //Height of the form should correspond to the height of the buttons
      Self.Width:=aButton.Width*6;        //Width of the form to match the width of all buttons
    end;
    

  * Create in the Object Inspector the event handler for the event FormDestroy, by clicking on the button [...].
  * Add the following code to the handler:


    
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      FreeAndNil(Puzzle15);
    end;
    

  


  * Now you must create the event handler for the button clicks.
  * In the source editor, entering your [class](<Class.md> "Class") TForm1 in the section [private](<Private.md> "Private").
  * Add [procedure](<Procedure.md> "Procedure") `procedure aButtonClick(Sender: TObject);` and then press the keys `Ctrl`+`⇧ Shift`+`c` (the code completion becomes active and creates the `procedure TForm1.aButtonClick(Sender: TObject);`.
  * Paste following code:


    
    
    procedure TForm1.aButtonClick(Sender: TObject);  //the event handler for the button
    var
      i:integer;
      point : TPoint;
    begin
      if (Sender is TButton) and                     //called the event handler of a button out?
         TryStrToInt(TButton(Sender).Caption, i)       //then try to convert the label in a integer
        then
          begin
            if Puzzle15.CanMove(TButton(Sender).Caption) then
              begin
                point := Puzzle15.Change(TButton(Sender).Caption);
                TButton(Sender).Left:= point.x* TButton(Sender).Width;    //Distance from left
                TButton(Sender).Top := point.y* TButton(Sender).Height;
              end;
          end;
    end;
    

## Source code

### unit1.pas
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      unitGameBoard;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      private
        { private declarations }
        Puzzle15:TGameBoard;
        procedure aButtonClick(Sender: TObject);
    
      public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      i : Integer;
      aButton: TButton;
      point:TPoint;
    begin
      Randomize;
      Puzzle15:= TGameBoard.Create;
      i := 1;
      while i < 16 do
        begin  //create 15 Buttons
          aButton:=TButton.Create(Self);    //create Button, Owner is Form1, where the button is released later
          aButton.Parent:=Self;             //determine where it is to be displayed
          aButton.Caption:=IntToStr(i);     //Captions of the buttons
          aButton.Width:=aButton.Height;    //Width should correspond to the height of the buttons
          point := Puzzle15.ButtonPlace(aButton.Caption);
          aButton.Left:= point.x* aButton.Width;    //Distance from left
          aButton.Top := point.y* aButton.Height;
          aButton.OnClick:=@aButtonClick;   //the event handler for the button -> will be created yet
          inc(i);
        end;
      Self.Height:=aButton.Height*6;        //Height of the form should correspond to the height of the buttons
      Self.Width:=aButton.Width*6;       //Width of the form to match the width of all buttons
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      FreeAndNil(Puzzle15);
    end;
    
    procedure TForm1.aButtonClick(Sender: TObject);  //the event handler for the button
    var
      i:integer;
      point : TPoint;
    begin
      if (Sender is TButton) and                     //called the event handler of a button out?
         TryStrToInt(TButton(Sender).Caption, i)       //then try to convert the label in a integer
        then
          begin
            if Puzzle15.CanMove(TButton(Sender).Caption) then
              begin
                point := Puzzle15.Change(TButton(Sender).Caption);
                TButton(Sender).Left:= point.x* TButton(Sender).Width;    //Distance from left
                TButton(Sender).Top := point.y* TButton(Sender).Height;
              end;
          end;
    end;
    
    end.
    

### Unit1.lfm
    
    
    object Form1: TForm1
      Left = 362
      Height = 184
      Top = 162
      Width = 432
      Caption = '15Puzzle'
      OnCreate = FormCreate
      OnDestroy = FormDestroy
      LCLVersion = '1.6.0.4'
    end
    

### unitGameBoard.pas
    
    
    unit unitGameBoard;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils;
    
    const
      GB_MAX_BUTTONS = 15;
      GB_MAX_X = 4;
      GB_MAX_Y = 4;
      GB_EMPTY_PLACE ='-';
    type
    
    
      { TGameBoard }
    
      TGameBoard = class
      private
         GameArray:array[1..GB_MAX_X, 1..GB_MAX_Y] of string;
         function ValidPoint(a_point:TPoint):boolean;
      public
        constructor Create;
        function ButtonPlace(a_name:string):TPoint;
        function CanMove(a_name:string):Boolean;
        function Change(a_name: string): TPoint;
        procedure RandomArray;
        destructor Destroy; override;
      end;
    implementation
    
    { TGameBoard }
    
    function TGameBoard.ValidPoint(a_point: TPoint): boolean;
    begin
      result := true;
      if a_point.y > GB_MAX_Y then result := false;
      if a_point.x > GB_MAX_X then result := false;
      if a_point.y < 1 then result := false;
      if a_point.x < 1 then result := false;
    end;
    
    constructor TGameBoard.Create;
    var
      i, x, y : integer;
    begin
      i := 1;
      for y := 1 to  GB_MAX_Y do
        for x := 1 to GB_MAX_X do
          begin
            if i < GB_MAX_BUTTONS+1
              then GameArray[x,y] := IntToStr(i)
              else GameArray[x,y] := GB_EMPTY_PLACE;
            inc(i);
          end;
      RandomArray;
    end;
    
    function TGameBoard.ButtonPlace(a_name: string): TPoint;
    var
      x,y:integer;
    begin
      result.x := 0;
      result.y := 0;
      for y := 1 to  GB_MAX_Y do
        for x := 1 to GB_MAX_X do
          if  GameArray[x,y] = a_name then
          begin
            result.x := x;
            result.y := y;
          end;
    end;
    
    function TGameBoard.CanMove(a_name: string): Boolean;
    var
      empty, point:TPoint;
    begin
      result := false;
      empty := ButtonPlace( GB_EMPTY_PLACE );
      point := ButtonPlace(a_name);
      if (abs(empty.x-point.x)=1) and (empty.y=point.y) then result := true;
      if (abs(empty.y-point.y)=1) and (empty.x=point.x) then result := true;
    end;
    
    function TGameBoard.Change(a_name: string): TPoint;
    var
      empty, point:TPoint;
    begin
      empty := ButtonPlace( GB_EMPTY_PLACE );
      result := empty;
      point := ButtonPlace(a_name);
      GameArray[point.x, point.y] := '-';
      GameArray[empty.x, empty.y] := a_name;
    end;
    
    procedure TGameBoard.RandomArray;
    var
       j,i:integer;
       name:string;
       point,point2:TPoint;
    begin
      for i := 0 to 1000 do
        begin
          j := random(4);
          point := ButtonPlace( GB_EMPTY_PLACE );
          point2 := point;
          case j of
            0: point2.x := point.x-1;
            1: point2.x := point.x+1;
            2: point2.y := point.y-1;
            3: point2.y := point.y+1;
          end;
          if ValidPoint( point2 ) then
            begin
               name :=  GameArray[point2.x, point2.y];
               GameArray[point.x, point.y] := name;
               GameArray[point2.x, point2.y] := GB_EMPTY_PLACE;
            end;
        end;
    end;
    
    destructor TGameBoard.Destroy;
    begin
      inherited Destroy;
    end;
    
    end.
    

## See also

  * [TButton](<TButton.md> "TButton")

---

_Source: [https://wiki.freepascal.org/15-puzzle](https://web.archive.org/web/20250422225814/https://wiki.freepascal.org/15-puzzle)_
