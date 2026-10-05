# Frames

│ **English (en)** │  **[русский (ru)](<../ru/Frames.md>)** │

**[TFrames](<TFrame.md> "TFrame")** are named containers for components and very similar to [forms](<TForm.md> "TForm"). Their unique ability is that they can be embedded into forms or other frames in the designer. As forms they are stored in two files: the code is stored in .pas file and the design in the .lfm file. 

## Contents

  * 1 How to create a frame?
  * 2 How to place a frame to the form or another frame?
  * 3 What can they be used for?
  * 4 Initialising private variables
  * 5 Sample of dynamic creation
  * 6 See also



## How to create a frame?

Press File->New... menu item and in the opened dialog select "Frame" item. 

[![Frame new.PNG](https://wiki.freepascal.org/images/6/6d/Frame_new.PNG)](</File:Frame_new.PNG>)

## How to place a frame to the form or another frame?

The [Standard tab](<Standard_tab.md> "Standard tab") of the [Component Palette](<Component_Palette.md> "Component Palette") has a special component item "[TFrame](<TFrame.md> "TFrame")". When you add it to the form the IDE asks you to choose one of the project's frames. You really have to click the frame component in the toolbar and "click it" down on your form first (drag&drop won't work) - the selection dialog will only appear afterwards not upon just clicking the component icon. You have to save the project and the the new frame first, only saved frames will appear in the choose from list. Frames in packages are not yet implemented. But you can create frames in code even in packages. You can't create frame circles - the IDE will forbid this, i.o.w. you can't place FrameA to FrameA and you can't place any frame which contains FrameA to FrameA. 

[![](https://wiki.freepascal.org/images/3/35/Frame_add.PNG)](</File:Frame_add.PNG>)

To add a frame click the component icon in the toolbar then click again on your form (drag&drop will not work)

## What can they be used for?

They are needed when you have a group of components that you want to reuse on multiple forms. The group should have the same set of controls and logic between them at different windows (forms) of your application. You can group that repeated controls and logic into one frame and use that frame in different places. Therefore you don't need to repeat the work of control layouting and writing their logic. 

For example, you have two listboxes and buttons to move items between them. So you can create a frame with two listboxes and needed buttons, write the logic for items moving and then use your frame on all forms where needed. Moreover, if you find an error in the frame code then you can fix it once in the frame code instead of fixing it n-times among all forms. 

a frame in designer: 

[![Frame example.PNG](https://wiki.freepascal.org/images/1/13/Frame_example.PNG)](</File:Frame_example.PNG>)

  
the frame placed on a form: 

[![Frame embedded.PNG](https://wiki.freepascal.org/images/1/17/Frame_embedded.PNG)](</File:Frame_embedded.PNG>)

the same frame placed on another form: 

[![Frame embedded2.PNG](https://wiki.freepascal.org/images/8/88/Frame_embedded2.PNG)](</File:Frame_embedded2.PNG>)

## Initialising private variables

TFrame does not have OnCreate or OnDestroy events in which private variables can be initialised and released. [Override](<Override.md> "Override") the default [constructor](<Constructor.md> "Constructor") and [destructor](<Destructor.md> "Destructor") to do this. 
    
    
    TFrame1 = class(TFrame)
      private
        MyObj: TObject;
      public
        constructor Create(TheOwner: TComponent); override;
        destructor Destroy; override;
      end; 
    
    constructor TFrame1.Create(TheOwner: TComponent);
    begin
      inherited Create(TheOwner);
      MyObj := TObject.Create;
    end;
    
    destructor TFrame1.Destroy;
    begin
      MyObj.Free
      inherited Destroy;
    end;
    

## Sample of dynamic creation

More easy. Because TFrame component in component palette is not required. And you can make a page change type application(like a smart phone application) with save resource. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        GroupBox1: TGroupBox;
        procedure Button1Click(Sender: TObject);
        procedure FormCreate(Sender: TObject);
      private
        { private declarations }
        Frame: TFrame;
      public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
    
    implementation
    uses
      Unit2{TFrame1}, Unit3{TFrame2}, Unit4{TFrame3};
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Frame := TFrame1.Create(GroupBox1);
      Frame.Parent := GroupBox1;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      if not Assigned(Frame) then
      begin
        Frame := TFrame1.Create(GroupBox1);
        Frame.Parent := GroupBox1;
      end else if Frame is TFrame1 then begin
        Frame.Free;
        Frame := TFrame2.Create(GroupBox1);
        Frame.Parent := GroupBox1;
      end else if Frame is TFrame2 then begin
        Frame.Free;
        Frame := TFrame3.Create(GroupBox1);
        Frame.Parent := GroupBox1;
      end else begin
        FreeAndNil(Frame);
      end;
    end;
    
    end.
    

## See also

  * [TFrame](<TFrame.md> "TFrame")

---

_Source: [https://wiki.freepascal.org/Frames](https://web.archive.org/web/20241207122300/https://wiki.freepascal.org/Frames)_
