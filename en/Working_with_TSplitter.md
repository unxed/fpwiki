# Working with TSplitter

From Lazarus wiki

## Contents

  * 1 TSplitter
  * 2 Splitter and Align
    * 2.1 In Form designer
    * 2.2 As lfm
    * 2.3 The same in code
  * 3 Splitter with AnchorSides
    * 3.1 Example in IDE designer
    * 3.2 In lfm
    * 3.3 In code

  
---  
  
#  TSplitter

The LCL control TSplitter can be used as a visual separator between two halves of your form and allows the user of your application to move it either vertically or horizontally. It is defined in unit **ExtCtrls** and on the IDE component palette page _Additional_. 

TSplitter can work basically in two different modes: via Align (the Delphi way) or via AnchorSides (not supported by Delphi). 

#  Splitter and Align

Align can be used for many simple layouts like two controls or a row of controls. For example when you need some freely resizable controls like a memo and a listbox. 

  
The following example demonstrates this. 

##  In Form designer

  1. create a new form 
  2. drop a TMemo on a form (left click on the TMemo icon in component paletter to select, then left click on the form) 
  3. set in Object Inspector Align of Memo1 to alLeft. 
  4. drop a TSplitter on a form 
  5. the default Align is already alLeft 
  6. drop another TMemo on the form. 
  7. set in Object Inspector Align of Memo2 to alClient. 



##  As lfm
    
    
      object Memo1: TMemo
        Left = 0
        Height = 141
        Top = 0
        Width = 150
        Align = alLeft
        Lines.Strings = (
          'Memo1'
        )
        TabOrder = 0
      end
      object Splitter1: TSplitter
        Left = 150
        Height = 141
        Top = 0
        Width = 5
      end
      object Memo2: TMemo
        Left = 155
        Height = 141
        Top = 0
        Width = 100
        Align = alClient
        Lines.Strings = (
          'Memo2'
        )
        TabOrder = 2
      end

##  The same in code
    
    
    procedure TMainForm.FormCreate(Sender: TObject);
    var
      Memo1: TMemo;
      Splitter1: TSplitter;
      Memo2: TMemo;
    begin
      Memo1:=TMemo.Create(Self);
      with Memo1 do begin
        Name:='Memo1';
        Parent:=Self;
        Align:=alLeft;
      end;
      Splitter1:=TSplitter.Create(Self);
      with Splitter1 do begin
        Name:='Splitter1';
        Parent:=Self;
        Left:=1; // position it right of Memo1
        Align:=alLeft;
      end;
      Memo2:=TMemo.Create(Self);
      with Memo2 do begin
        Name:='Memo2';
        Parent:=Self;
        Align:=alClient;
      end;
    end;

#  Splitter with AnchorSides

Anchor sides allows more fine tuned layouts. Align fills all the space. AnchorSides allow to anchor controls to any other sibling control. 

##  Example in IDE designer

  1. create a new form 
  2. drop a TMemo on a form (left click on the TMemo icon in component paletter to select, then left click on the form) 
  3. set in Object Inspector Align of Memo1 to alLeft 
  4. drop a TSplitter on a form 
  5. set the Align property of the Splitter1 to alNone 
  6. select the Memo1 
  7. View -> Anchor Editor 
  8. anchor the right side of Memo1 to the Splitter1 
  9. drop another TMemo on the form. 
  10. set the Align property of Memo2 to alRight. 
  11. anchor the left side of Memo2 to Splitter1. Make sure to anchor to the right side of Splitter1 (the button on the Anchor editor below the combobox). 



##  In lfm

The same as lfm: 
    
    
      object Memo1: TMemo
        AnchorSideRight.Control = Splitter1
        Left = 0
        Height = 141
        Top = 0
        Width = 145
        Align = alLeft
        Anchors = [akTop, akLeft, akRight, akBottom]
        Lines.Strings = (
          'Memo1'
        )
        TabOrder = 0
      end
      object Splitter1: TSplitter
        Left = 145
        Height = 141
        Top = 0
        Width = 5
        Align = alNone
      end
      object Memo2: TMemo
        AnchorSideLeft.Control = Splitter1
        AnchorSideLeft.Side = asrBottom
        Left = 150
        Height = 141
        Top = 0
        Width = 105
        Align = alRight
        Anchors = [akTop, akLeft, akRight, akBottom]
        Lines.Strings = (
          'Memo2'
        )
        TabOrder = 2
      end

##  In code

The same in code: 
    
    
    procedure TMainForm.FormCreate(Sender: TObject);
    var
      Memo1: TMemo;
      Splitter1: TSplitter;
      Memo2: TMemo;
    begin
      Memo1:=TMemo.Create(Self);
      with Memo1 do begin
        Name:='Memo1';
        Parent:=Self;
        Align:=alLeft;
      end;
      Splitter1:=TSplitter.Create(Self);
      with Splitter1 do begin
        Name:='Splitter1';
        Parent:=Self;
        Align:=alNone;
        Left:=100; // some value
        AnchorParallel(akBottom,0,Parent);
      end;
      Memo1.AnchorToNeighbour(akRight,0,Splitter1);
      Memo2:=TMemo.Create(Self);
      with Memo2 do begin
        Name:='Memo2';
        Parent:=Self;
        Align:=alRight;
        AnchorToNeighbour(akLeft,0,Splitter1);
      end;
    end;

---

_Source: [https://wiki.freepascal.org/Working_with_TSplitter](https://web.archive.org/web/20130112192858/https://wiki.freepascal.org/Working_with_TSplitter)_
