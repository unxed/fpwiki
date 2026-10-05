# Drag and Drop sample

Drag and Drop is a common operation that makes the interface user friendly: a user can drag/drop information to controls instead of having to type etc. 

The following sample explains basics of drag and drop. For detailed information you should refer to other articles in the wiki and reference documentation. 

Please note, since LCL is partially compatible with Delphi's VCL, some articles/examples about Delphi drag-and-drop may also apply to LCL. 

## Contents

  * 1 Drag and Drop
  * 2 Example
    * 2.1 Dragging between controls
    * 2.2 Dragging within a control
  * 3 Hints
  * 4 Dragging from other applications
    * 4.1 Files
    * 4.2 Text etc
      * 4.2.1 Windows
  * 5 See also



## Drag and Drop

Despite of the operation's simplicity from the user's point of view, it might give hard times to an inexperienced developer. 

For the code, Drag and Drop operation always consists of at least these 3 steps: 

  1. Some control starts the drag-and-drop operation. This is called the Source
  2. User drags the mouse cursor around, above other controls or the Source itself. Now a dragged over control needs to decide, if it is able to accept the dragged data.
  3. Drop happens if a control agrees to accept the dragged data. The accepting control is called the Sender.



To simplify drag-and-drop handling, the LCL provides "automatic" mode. It doesn't mean, that LCL does the whole drag-and-drop for you, but it will handle low-level drag object managing (which is not covered in this article). 

## Example

[![DnDTest.PNG](https://wiki.freepascal.org/images/3/30/DnDTest.PNG)](</File:DnDTest.PNG>)

The example covers automatic drag-and-drop feature between 2 controls (Edit->Treeview) as well as inside a single control (Treeview->Treeview) 

  * Start the new application.
  * Put a TreeView component and Edit on the form.
  * Enable Automatic drag-and-drop mode for TreeView and Edit in Object Inspector:



**DragMode: dkAutomatic**

You can launch the application now, and try to drag anything around. You should not get anything working for now. But, if you press the left mouse button on the Treeview, you'll probably see that the cursor icon changed, but still nothing happens when releasing the mouse . 

### Dragging between controls

Let's make a drag-and-drop operation between Edit and TreeView. There the content of Edit will be "dragged" to TreeView and a new tree node created. 

To initiate the drag, controls have a special method: `BeginDrag()`

  
Create OnMouseDown event for the Edit: 
    
    
    procedure TForm1.Edit1MouseDown(Sender: TObject; Button: TMouseButton; 
      Shift: TShiftState; X, Y: Integer);
    begin
      if Button = mbLeft then   {check if left mouse button was pressed}
        Edit1.BeginDrag(true);  {starting the drag operation}
    end;

If you launch the application right now and try drag and drop, you'll notice that Edit starts the operation, but still nothing happens then you're trying to drop to TreeView. 

This is because TreeView doesn't accept the data. None of the controls accept data by default, so you'll always need to provide the proper event handler. 

Assign TreeView.OnDragOver operation: 
    
    
    procedure TForm1.TreeView1DragOver(Sender, Source: TObject; X, Y: Integer; 
      State: TDragState; var Accept: Boolean);
    begin
      Accept := true;
    end;

Of course in some cases, TreeView might deny dropping (if Source or data cannot be handled), but for now, TreeView always accepts the dragging. 

Run application and test. Everything should be better now, though still nothing happens on drop. 

Assign TreeView.OnDragDrop operation: 
    
    
    procedure TForm1.TreeView1DragDrop(Sender, Source: TObject; X, Y: Integer);
    var
      tv     : TTreeView; 
      iNode  : TTreeNode;  
    begin
      tv := TTreeView(Sender);      { Sender is TreeView where the data is being dropped  }
      iNode := tv.GetNodeAt(x,y);   { x,y are drop coordinates (relative to the Sender)   }   
                                    {   since Sender is TreeView we can evaluate          }
                                    {   a tree at the X,Y coordinates                     } 
                                    
      { TreeView can also be a Source! So we must make sure }                                
      { that Source is TEdit, before getting its text }      
      if Source is TEdit then       
        tv.Items.AddChild(iNode, TEdit(Source).Text); {now, we can add a new node, with a text from Source }
    end;

Run and test. It should be working now. Dragging a text from Edit to TreeView should create a new node. 

### Dragging within a control

Sender and Source can be the same control! It's not prohibited in any way. Let's add the ability to a TextView, to change its nodes location. Since TextView is in automatic DragMode, you don't need to start the drag by using `DragBegin()`. It's started automatically on mouse moved with left button hold. 

Make sure you have "Accept:=true;" inside the DragOver operation for TreeView source. 

Modify the DragDrop event handler to the following: 
    
    
    procedure TForm1.TreeView1DragDrop(Sender, Source: TObject; X, Y: Integer);
    var
      tv     : TTreeView; 
      iNode  : TTreeNode;  
    begin
      tv := TTreeView(Sender);      { Sender is TreeView where the data is being dropped  }
      iNode := tv.GetNodeAt(x,y);   { x,y are drop coordinates (relative to the Sender)   }   
                                    {   since Sender is TreeView we can evaluate          }
                                    {   a tree at the X,Y coordinates                     } 
                                    
      { TreeView can also be a Source! So we must make sure }                                
      { that Source is TEdit, before getting its text }      
      if Source is TEdit then       
        tv.Items.AddChild(iNode, TEdit(Source).Text) {now, we can add a new node, with a text from Source }
        
      else if Source = Sender then begin         { drop is happening within a TreeView   }
        if Assigned(tv.Selected) and             {  check if any node has been selected  }
          (iNode <> tv.Selected) then            {   and we're dropping to another node  }
        begin
          if iNode <> nil then 
            tv.Selected.MoveTo(iNode, naAddChild) { complete the drop operation, by moving the selectede node }
          else
            tv.Selected.MoveTo(iNode, naAdd); { complete the drop operation, by moving in root of a TreeView }
        end;
      end;
    end;

That's it. If you run the application now, you should have both features working. 

  * Adding a new node by dragging text from Edit to TreeView
  * Dragging nodes inside the treeview



## Hints

  * You can?/cannot? use some global data to inspect what is being dragged now. Don't use global variables, but your form class's fields. 
    * Put the data when you start the drag
    * Inspect the data, during control's drag over event, and modify Accept flag accordingly
    * Read and use the data on drop event



## Dragging from other applications

You can drag/drop 

### Files

Files can be dropped and handled easily by implementing the FormDropFiles event, once AllowDropFiles on the form is set: 
    
    
    procedure TForm1.FormDropFiles(Sender: TObject; const FileNames: array of String);
    var FileName : String;
    begin
      for FileName in FileNames do
      begin
        ShowMessage(FileName);
      end;
    end;

### Text etc

You can drag and drop e.g. text from another application (e.g.t notepad) to a control on your application. The way this is implemented is platform-dependent. 

#### Windows

This code lets you drag and drop selected text from other applications onto an edit control. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      Windows, ActiveX, ComObj;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm, IDropTarget)
        Memo1: TMemo;
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      private
        // IDropTarget
        function DragEnter(const dataObj: IDataObject; grfKeyState: DWORD; pt: TPoint; var dwEffect: DWORD): HResult;StdCall;
        function DragOver(grfKeyState: DWORD; pt: TPoint; var dwEffect: DWORD): HResult;StdCall;
        function DragLeave: HResult;StdCall;
        function Drop(const dataObj: IDataObject; grfKeyState: DWORD; pt: TPoint; var dwEffect: DWORD):HResult;StdCall;
        // IUnknown
        // Ignore referance counting
        function _AddRef: Integer; stdcall;
        function _Release: Integer; stdcall;
      public
        { public declarations }
      end;
    
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      OleInitialize(nil);
      OleCheck(RegisterDragDrop(Handle, Self));
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      RevokeDragDrop(Handle);
      OleUninitialize;
    end;
    
    function TForm1.DragEnter(const dataObj: IDataObject; grfKeyState: DWORD;
      pt: TPoint; var dwEffect: DWORD): HResult; StdCall;
    begin
      dwEffect := DROPEFFECT_COPY;
      Result := S_OK;
    end;
    
    function TForm1.DragOver(grfKeyState: DWORD; pt: TPoint; var dwEffect: DWORD
      ): HResult; StdCall;
    begin
      dwEffect := DROPEFFECT_COPY;
      Result := S_OK;
    end;
    
    function TForm1.DragLeave: HResult; StdCall;
    begin
      Result := S_OK;
    end;
    
    function TForm1._AddRef: Integer; stdcall;
    begin
       Result := 1;
    end;
    
    function TForm1._Release: Integer; stdcall;
    begin
       Result := 1;
    end;
    
    function TForm1.Drop(const dataObj: IDataObject; grfKeyState: DWORD;
      pt: TPoint; var dwEffect: DWORD): HResult; StdCall;
    var
      aFmtEtc: TFORMATETC;
      aStgMed: TSTGMEDIUM;
      pData: PChar;
    begin
      {Make certain the data rendering is available}
      if (dataObj = nil) then
        raise Exception.Create('IDataObject-Pointer is not valid!');
      with aFmtEtc do
      begin
        cfFormat := CF_TEXT;
        ptd := nil;
        dwAspect := DVASPECT_CONTENT;
        lindex := -1;
        tymed := TYMED_HGLOBAL;
      end;
      {Get the data}
      OleCheck(dataObj.GetData(aFmtEtc, aStgMed));
      try
        {Lock the global memory handle to get a pointer to the data}
        pData := GlobalLock(aStgMed.hGlobal);
        { Replace Text }
        Memo1.Text := pData;
      finally
        {Finished with the pointer}
        GlobalUnlock(aStgMed.hGlobal);
        {Free the memory}
        ReleaseStgMedium(aStgMed);
      end;
      Result := S_OK;
    end;
    
    
    end.

Source forum: <http://forum.lazarus.freepascal.org/index.php/topic,25769.msg156933.html#msg156933>

## See also

  * [LCL Drag Drop](<LCL_Drag_Drop.md> "LCL Drag Drop")

---

_Source: [https://wiki.freepascal.org/Drag_and_Drop_sample](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/Drag_and_Drop_sample)_
