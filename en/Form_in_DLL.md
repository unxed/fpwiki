# Form in DLL

│ **English (en)** │  **[русский (ru)](<../ru/Form_in_DLL.md>)** │

The following application and library demonstrate you how to show a modal and a non-modal form and create a custom component on application form from a DLL library. Tested on win32 and Linux/Gtk2 with Lazarus 1.7 trunk. 

Main application: 
    
    
    program MainApp;
    
    uses
      Interfaces, Classes, LCLType, Controls, StdCtrls, Forms, ExtCtrls;
    
    type
      TEnableDisableFormsCallBack = procedure(var FormList: Pointer);
      TCreateButtonCallBack = procedure(Caption: PChar; OnClick: TProcedure);
    
    const
    {$IFDEF WINDOWS}
      DLLDialogLib = 'DLLDialog.dll';
    {$ELSE}
      DLLDialogLib = 'DLLDialog.so';
    {$ENDIF}
    
    procedure DLLDialog_Init(DisableFormsCallBack, EnableFormsCallback: TEnableDisableFormsCallBack); external DLLDialogLib;
    procedure DLLDialog_Final; external DLLDialogLib;
    procedure DLLDialog_ShowModal(ParentWindow: HWND); external DLLDialogLib;
    procedure DLLDialog_Show(ParentWindow: HWND); external DLLDialogLib;
    procedure DLLDialog_CreateDLLButton(ParentWindow: HWND); external DLLDialogLib;
    procedure DLLDialog_CreateButton(CreateButtonCallBack: TCreateButtonCallBack); external DLLDialogLib;
    
    type
      TMainForm = class(TForm)
      private
        PnlParent: TPanel;
    
        procedure BtnAddDLLButtonClick(Sender: TObject);
        procedure BtnAddButtonClick(Sender: TObject);
        procedure ShowModalDLLDialog(Sender: TObject);
        procedure ShowDLLDialog(Sender: TObject);
      public
        constructor Create(aOwner: TComponent); override;
      end;
    
    var
      MainForm: TMainForm;
    
    { TMainForm }
    
    procedure DisableFormsCallBack(var FormList: Pointer);
    begin
      FormList := Screen.DisableForms(nil, TList(FormList));
    end;
    
    procedure EnableFormsCallback(var FormList: Pointer);
    begin
      Screen.EnableForms(TList(FormList));
    end;
    
    constructor TMainForm.Create(aOwner: TComponent);
    var
      BtnShow, BtnShowModal, BtnAddDLLButton, BtnAddButton: TButton;
    begin
      inherited CreateNew(aOwner);
    
      Position := poWorkAreaCenter;
      Width := 600;
      Height := 200;
    
      BtnShow := TButton.Create(Self);
      BtnShow.Parent := Self;
      BtnShow.Caption := 'Show form';
      BtnShow.AutoSize := True;
      BtnShow.OnClick := @ShowDLLDialog;
    
      BtnShowModal := TButton.Create(Self);
      BtnShowModal.Parent := Self;
      BtnShowModal.Caption := 'Show modal form';
      BtnShowModal.AutoSize := True;
      BtnShowModal.OnClick := @ShowModalDLLDialog;
      BtnShowModal.AnchorSide[akLeft].Control := BtnShow;
      BtnShowModal.AnchorSide[akLeft].Side := asrRight;
      BtnShowModal.BorderSpacing.Left := 10;
    
      BtnAddDLLButton := TButton.Create(Self);
      BtnAddDLLButton.Parent := Self;
      BtnAddDLLButton.Caption := 'Create real DLL button';
      BtnAddDLLButton.AutoSize := True;
      BtnAddDLLButton.OnClick := @BtnAddDLLButtonClick;
      BtnAddDLLButton.AnchorSide[akLeft].Control := BtnShowModal;
      BtnAddDLLButton.AnchorSide[akLeft].Side := asrRight;
      BtnAddDLLButton.BorderSpacing.Left := 10;
    
      BtnAddButton := TButton.Create(Self);
      BtnAddButton.Parent := Self;
      BtnAddButton.Caption := 'Create fake DLL button';
      BtnAddButton.AutoSize := True;
      BtnAddButton.OnClick := @BtnAddButtonClick;
      BtnAddButton.AnchorSide[akLeft].Control := BtnAddDLLButton;
      BtnAddButton.AnchorSide[akLeft].Side := asrRight;
      BtnAddButton.BorderSpacing.Left := 10;
    
      PnlParent := TPanel.Create(Self);
      PnlParent.Parent := Self;
      PnlParent.AnchorSide[akTop].Control := BtnShow;
      PnlParent.AnchorSide[akTop].Side := asrBottom;
      PnlParent.BorderSpacing.Top := 10;
      PnlParent.Width := 220;
    end;
    
    procedure CreateButtonCallBack(ACaption: PChar; AOnClick: TProcedure);
    var
      Btn: TButton;
      MyMethod: TMethod;
    begin
      Btn := TButton.Create(MainForm);
      Btn.Caption := ACaption;
      Btn.Left := 100;
      Btn.Width := 100;
      Btn.Height := 20;
      MyMethod.Code := AOnClick;
      MyMethod.Data := nil;
      Btn.OnClick := TNotifyEvent(MyMethod);
      Btn.Parent := MainForm.PnlParent;
    end;
    
    procedure TMainForm.BtnAddButtonClick(Sender: TObject);
    begin
      DLLDialog_CreateButton(@CreateButtonCallBack);
    end;
    
    procedure TMainForm.BtnAddDLLButtonClick(Sender: TObject);
    begin
      DLLDialog_CreateDLLButton(PnlParent.Handle);
    end;
    
    procedure TMainForm.ShowDLLDialog(Sender: TObject);
    begin
      DLLDialog_Show(0);
    end;
    
    procedure TMainForm.ShowModalDLLDialog(Sender: TObject);
    begin
      DLLDialog_ShowModal(Self.Handle);
    end;
    
    {$R *.res}
    
    begin
      Application.Initialize;
      Application.CreateForm(TMainForm, MainForm);
      DLLDialog_Init(@DisableFormsCallBack, @EnableFormsCallback);
      try
        Application.Run;
      finally
        DLLDialog_Final;
      end;
    end.
    

DLL with a form: 
    
    
    library DllDialog;
    
    {$mode objfpc}{$H+}
    
    uses
      Interfaces, Classes, LCLType, StdCtrls, Controls, Forms, Dialogs;
    
    type
      TDLLDialog = class(TForm)
      protected
        procedure CreateParams(var Params: TCreateParams); override;
      public
        procedure BtnShowMessageClick(Sender: TObject);
        constructor Create(aOwner: TComponent); override;
      public
        ParentFormHandle: HWND;
      end;
    
      TEnableDisableFormsCallBack = procedure(var FormList: Pointer);
      TCreateButtonCallBack = procedure(Caption: PChar; OnClick: TProcedure);
    
      TApplicationCallback = class(TComponent)
      private
        DisableFormsCallBack: TEnableDisableFormsCallBack;
        EnableFormsCallback: TEnableDisableFormsCallBack;
        FormList: Pointer;
      public
        procedure DisableForms(Sender: TObject);
        procedure EnableForms(Sender: TObject);
        procedure BtnClick(Sender: TObject);
    
        constructor Create(aOwner: TComponent); override;
        destructor Destroy; override;
      end;
    
    var
      ApplicationCallback: TApplicationCallback;
    
    procedure DLLDialog_Init(DisableFormsCallBack, EnableFormsCallback: TEnableDisableFormsCallBack);
    begin
      ApplicationCallback := TApplicationCallback.Create(nil);
      ApplicationCallback.DisableFormsCallBack := DisableFormsCallBack;
      ApplicationCallback.EnableFormsCallback := EnableFormsCallback;
    end;
    
    procedure DLLDialog_Final;
    begin
      ApplicationCallback.Free;
    end;
    
    procedure DLLDialog_ShowModal(ParentWindow: HWND);
    var
      DLLDialog: TDLLDialog;
    begin
      DLLDialog := TDLLDialog.Create(ApplicationCallback);
      try
        DLLDialog.ParentFormHandle := ParentWindow;
        DLLDialog.ShowModal;
      finally
        DLLDialog.Free;
      end;
    end;
    
    procedure DLLDialog_Show(ParentWindow: HWND);
    var
      DLLDialog: TDLLDialog;
    begin
      DLLDialog := TDLLDialog.Create(ApplicationCallback);
      DLLDialog.ParentFormHandle := ParentWindow;
      DLLDialog.Show;
    end;
    
    procedure DLLDialog_CreateDLLButton(ParentWindow: HWND);
    var
      Btn: TButton;
      BtnParentForm: TForm;
    begin
      BtnParentForm := TForm.CreateNew(ApplicationCallback);
      BtnParentForm.ParentWindow := ParentWindow;
      BtnParentForm.Width := 100;
      BtnParentForm.Height := 20;
      BtnParentForm.BorderStyle := bsNone;
      BtnParentForm.Visible := True;
    
      Btn := TButton.Create(ApplicationCallback);
      Btn.Caption := 'Real DLL Button';
      Btn.Width := BtnParentForm.Width;
      Btn.Height := BtnParentForm.Height;
      Btn.OnClick := @ApplicationCallback.BtnClick;
      Btn.Parent := BtnParentForm;
    end;
    
    procedure FakeBtnClick;
    begin
      ShowMessage('You clicked a fake button from a DLL!');
    end;
    
    procedure DLLDialog_CreateButton(CreateButtonCallBack: TCreateButtonCallBack);
    begin
      CreateButtonCallBack('Fake DLL Button', @FakeBtnClick);
    end;
    
    exports
      DLLDialog_Init, DLLDialog_Final, DLLDialog_ShowModal, DLLDialog_Show,
      DLLDialog_CreateButton, DLLDialog_CreateDLLButton;
    
    { TApplicationCallback }
    
    constructor TApplicationCallback.Create(aOwner: TComponent);
    begin
      inherited Create(aOwner);
    
      Application.AddOnModalBeginHandler(@DisableForms);
      Application.AddOnModalEndHandler(@EnableForms);
    end;
    
    procedure TApplicationCallback.BtnClick(Sender: TObject);
    begin
      ShowMessage('You clicked a real button from a DLL!');
    end;
    
    destructor TApplicationCallback.Destroy;
    begin
      Application.RemoveOnModalBeginHandler(@DisableForms);
      Application.RemoveOnModalEndHandler(@EnableForms);
    
      inherited Destroy;
    end;
    
    procedure TApplicationCallback.DisableForms(Sender: TObject);
    begin
      DisableFormsCallBack(FormList);
    end;
    
    procedure TApplicationCallback.EnableForms(Sender: TObject);
    begin
      EnableFormsCallback(FormList);
    end;
    
    { TDLLDialog }
    
    constructor TDLLDialog.Create(aOwner: TComponent);
    var
      BtnShowMessage: TButton;
    begin
      inherited CreateNew(aOwner);
    
      Caption := 'This form is in a DLL !!!';
      Position := poWorkAreaCenter;
      Width := 200;
      Height := 100;
    
      BtnShowMessage := TButton.Create(Self);
      BtnShowMessage.Parent := Self;
      BtnShowMessage.Caption := 'Show message';
      BtnShowMessage.AutoSize := True;
      BtnShowMessage.OnClick := @BtnShowMessageClick;
    end;
    
    procedure TDLLDialog.BtnShowMessageClick(Sender: TObject);
    begin
      ShowMessage('Hello from DLL!');
    end;
    
    procedure TDLLDialog.CreateParams(var Params: TCreateParams);
    begin
      inherited CreateParams(Params);
    
      {$IFDEF LCLWin32}
      Params.WndParent := ParentFormHandle;
      {$ENDIF}
    end;
    
    begin
      Application.Initialize;
    end.

---

_Source: [https://wiki.freepascal.org/Form_in_DLL](https://web.archive.org/web/20250415002924/https://wiki.freepascal.org/Form_in_DLL)_
