# TSqlite3 Master Detail Example

│ **English (en)** │

****TSqlite3 Master Detail Example

## Contents

  * 1 About Demo
  * 2 Author
  * 3 Components Used
  * 4 Licence
  * 5 Download
    * 5.1 Screen shots
    * 5.2 Example program code



## About Demo

This demo is a working example of how to use the [SQLite](<SQLite.md> "SQLite") TSqlite3 dataset component in a master detail relationship. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** this component is not part of the general SQLDB database components. SQLDB provides TSQLite3Connection that match other databases connectors. You are strongly recommended to look into using TSQLite3Connection because of the ease of switching to other databases; only use TSQlite3 if it is really needed.

See [MasterDetail](<MasterDetail.md> "MasterDetail") for instructions on how to implement master/detail relationships using standard sqldb (e.g. sqlite3) components. 

## Author

[David Stewart](</User:Davesimplewear> "User:Davesimplewear") .. davesimplewear at yahoo dot com 

  


## Components Used

  * TSqlite3
  * Standard Lazarus database components



## Licence

  * Free to use as you will



  


## Download

The TSQLite3 example can be downloaded from [The Lazarus -ccr sf download location](<http://sourceforge.net/projects/lazarus-ccr/files/Demos%20and%20examples/>). Also from [David's Freeware](<http://www.users.on.net/~dave.stewart/index.html>). 

### Screen shots

[![Screenshot-Master Detail Example - SQLLite3.png](https://wiki.freepascal.org/images/b/b8/Screenshot-Master_Detail_Example_-_SQLLite3.png)](</File:Screenshot-Master_Detail_Example_-_SQLLite3.png>)

[![Screenshot-Master Detail Example - SQLLite3-1.png](https://wiki.freepascal.org/images/8/82/Screenshot-Master_Detail_Example_-_SQLLite3-1.png)](</File:Screenshot-Master_Detail_Example_-_SQLLite3-1.png>)

[![Screenshot-Master Detail Example - SQLLite3-2.png](https://wiki.freepascal.org/images/7/71/Screenshot-Master_Detail_Example_-_SQLLite3-2.png)](</File:Screenshot-Master_Detail_Example_-_SQLLite3-2.png>)

[![Screenshot-Select Customer Name.png](https://wiki.freepascal.org/images/1/14/Screenshot-Select_Customer_Name.png)](</File:Screenshot-Select_Customer_Name.png>)

[![Screenshot-Select Item Number.png](https://wiki.freepascal.org/images/2/2e/Screenshot-Select_Item_Number.png)](</File:Screenshot-Select_Item_Number.png>)

This screenshot shows the table setting for master detail 

[![Screenshot.png](https://wiki.freepascal.org/images/e/e6/Screenshot.png)](</File:Screenshot.png>)

### Example program code
    
    
    unit uMain;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, db, sqlite3ds, FileUtil, LResources, Forms, Controls,
      Graphics, Dialogs, ComCtrls, ExtCtrls, Menus, DbCtrls, StdCtrls, DBGrids;
    
    type
    
      { TfMain }
    
      TfMain = class(TForm)
        btnSelCust: TButton;
        btnAddSale: TButton;
        btnSaveEntry: TButton;
        btnDelEntry: TButton;
        DBNavigator3: TDBNavigator;
        dsCust: TDatasource;
        dsSales: TDatasource;
        dsStock: TDatasource;
        DBEdit1: TDBEdit;
        DBEdit10: TDBEdit;
        DBEdit12: TDBEdit;
        DBEdit13: TDBEdit;
        DBEdit14: TDBEdit;
        DBEdit15: TDBEdit;
        DBEdit3: TDBEdit;
        DBEdit4: TDBEdit;
        DBEdit5: TDBEdit;
        DBEdit6: TDBEdit;
        DBEdit7: TDBEdit;
        DBEdit8: TDBEdit;
        DBEdit9: TDBEdit;
        dgSales: TDBGrid;
        DBNavigator1: TDBNavigator;
        DBNavigator2: TDBNavigator;
        Label1: TLabel;
        Label10: TLabel;
        Label11: TLabel;
        Label2: TLabel;
        Label3: TLabel;
        Label4: TLabel;
        Label5: TLabel;
        Label7: TLabel;
        Label8: TLabel;
        Label9: TLabel;
        miClose: TMenuItem;
        miFile: TMenuItem;
        mmMain: TMainMenu;
        nbMain: TNotebook;
        Panel1: TPanel;
        pnlSales: TPanel;
        pnlStock: TPanel;
        pnlCustomer: TPanel;
        pnlSelectCust: TPanel;
        pStock: TPage;
        pCustomer: TPage;
        pSales: TPage;
        sbMain: TStatusBar;
        TCustAddr: TStringField;
        TCustCustName: TStringField;
        TCustcustState: TStringField;
        TCustID: TAutoIncField;
        TCustpostCode: TStringField;
        TCustSuburb: TStringField;
        TSalescustID: TLongintField;
        TSalesID: TAutoIncField;
        TSalesitem: TStringField;
        TSalesitemNum: TStringField;
        TSalesprice: TFloatField;
        TSalessaleDate: TDateField;
        TSalesshipDate: TDateField;
        TStock: TSqlite3Dataset;
        TSales: TSqlite3Dataset;
        TCust: TSqlite3Dataset;
        TStockID: TAutoIncField;
        TStockitem: TStringField;
        TStockitemNum: TStringField;
        TStockprice: TFloatField;
        procedure btnAddSaleClick(Sender: TObject);
        procedure btnDelEntryClick(Sender: TObject);
        procedure btnSaveEntryClick(Sender: TObject);
        procedure btnSelCustClick(Sender: TObject);
        procedure dgSalesEditButtonClick(Sender: TObject);
        procedure FormCloseQuery(Sender: TObject; var CanClose: boolean);
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
        procedure FormShow(Sender: TObject);
        procedure miCloseClick(Sender: TObject);
        private
        { private declarations }
      public
        { public declarations }
      end; 
    
    var
      fMain: TfMain;
    
    implementation
    uses uCust, uSales;
    { TfMain }
    
    procedure TfMain.miCloseClick(Sender: TObject);
    begin
      Close;
    end;
    
    procedure TfMain.FormCloseQuery(Sender: TObject; var CanClose: boolean);
    begin
       CanClose := MessageDlg('Are You Sure ?',mtConfirmation,[mbYes,mbNo],0)=mrYes;
    end;
    
    procedure TfMain.FormCreate(Sender: TObject);
    var
    n:integer;
    c:TComponent;
    FName:string;
    begin
        fName := ExtractFilePath(ParamStr(0)) +'data/md.db3';
    for n := 0 to ComponentCount -1 do
        begin
         c := Components[n];
         if c is TSqlite3Dataset then
         TSqlite3Dataset(c).FileName:= fName;
        end;
     for n := 0 to ComponentCount -1 do
        begin
         c := Components[n];
         if c is TSqlite3Dataset then
         TSqlite3Dataset(c).Open;
        end;
    end;
    
    procedure TfMain.FormDestroy(Sender: TObject);
    var
    n:integer;
    c:TComponent;
    begin
      for n := 0 to ComponentCount -1 do
        begin
         c := Components[n];
         if c is TSqlite3Dataset then
         TSqlite3Dataset(c).Close;
        end;
    end;
    
    procedure TfMain.FormShow(Sender: TObject);
    begin
      nbMain.PageIndex:=0;
    end;
    
    procedure TfMain.dgSalesEditButtonClick(Sender: TObject);
    begin
      if SearchDlg.ShowModalParts =mrOk then
         begin
           TSales.Edit;
           TSalesItemNum.Value := SearchDlg.PartNum;
           TSalesItem.Value := TStockitem.Value;
           TSalesPRICE.Value:= TStockPrice.Value;
         end;
    end;
    
    procedure TfMain.btnSelCustClick(Sender: TObject);
    begin
      custDlg.CustName := TCustCUSTNAME.Value ;
         if CustDlg.ShowModalCust =mrOk then
         begin
           TCust.Edit;
           TCustCUSTNAME.Value := custDlg.CustName;
        end;
    end;
    
    procedure TfMain.btnAddSaleClick(Sender: TObject);
    begin
      TSales.Append;
    end;
    
    procedure TfMain.btnDelEntryClick(Sender: TObject);
    begin
      TSales.Delete;
    end;
    
    procedure TfMain.btnSaveEntryClick(Sender: TObject);
    begin
      TSales.ApplyUpdates;
    end;
    
    initialization
      {$I uMain.lrs}
    
    end.
    

uMain.lfm (edited) object fMain: TfMain 
    
    
     Left = 395
     Height = 332
     Top = 218
     Width = 534
     ActiveControl = nbMain
     BorderIcons = [biSystemMenu, biMinimize]
     BorderStyle = bsSingle
     Caption = 'Master Detail Example - SQLLite3'
     ClientHeight = 305
     ClientWidth = 534
     Font.Height = -13
     Font.Name = 'Sans'
     Menu = mmMain
     OnCloseQuery = FormCloseQuery
     OnCreate = FormCreate
     OnDestroy = FormDestroy
     OnShow = FormShow
     Position = poScreenCenter
     LCLVersion = '0.9.27'
     object sbMain: TStatusBar
       Left = 0
       Height = 19
       Top = 286
       Width = 534
       AutoHint = True
       Panels = <>
     end
     object nbMain: TNotebook
       Left = 0
       Height = 286
       Top = 0
       Width = 534
       Align = alClient
       PageIndex = 0
       TabOrder = 1
       object pSales: TPage
         Caption = 'Sales'
         ClientWidth = 532
         ClientHeight = 259
         object Label3: TLabel
           Left = 0
           Height = 18
           Top = 241
           Width = 532
           Align = alBottom
           Caption = 'Select Customer First, then click in item number field to select item, then save'
           ParentColor = False
         end
         object pnlSelectCust: TPanel
           Left = 15
           Height = 216
           Top = 14
           Width = 232
           BevelInner = bvLowered
           BevelWidth = 2
           ClientHeight = 216
           ClientWidth = 232
           TabOrder = 0
           object Label1: TLabel
             Left = 4
             Height = 18
             Top = 4
             Width = 224
             Align = alTop
             Alignment = taCenter
             Caption = 'Customer Detail'
             Font.Height = -13
             Font.Name = 'Sans'
             Font.Style = [fsBold, fsItalic]
             ParentColor = False
             ParentFont = False
           end
           object Label2: TLabel
             Left = 24
             Height = 18
             Top = 31
             Width = 73
             Caption = 'First Name'
             Font.Height = -13
             Font.Name = 'Sans'
             Font.Style = [fsItalic]
             ParentColor = False
             ParentFont = False
           end
           object DBEdit1: TDBEdit
             Left = 24
             Height = 23
             Hint = 'Cust name'
             Top = 48
             Width = 184
             DataField = 'CustName'
             DataSource = dsCust
             ReadOnly = True
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 0
           end
           object DBEdit3: TDBEdit
             Left = 24
             Height = 23
             Hint = 'Address'
             Top = 72
             Width = 184
             DataField = 'Addr'
             DataSource = dsCust
             ReadOnly = True
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 1
           end
           object DBEdit4: TDBEdit
             Left = 24
             Height = 23
             Hint = 'Suburb/Town'
             Top = 96
             Width = 184
             DataField = 'Suburb'
             DataSource = dsCust
             ReadOnly = True
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 2
           end
           object DBEdit5: TDBEdit
             Left = 24
             Height = 23
             Hint = 'Postal Code'
             Top = 120
             Width = 80
             DataField = 'postCode'
             DataSource = dsCust
             ReadOnly = True
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 3
           end
           object DBEdit6: TDBEdit
             Left = 128
             Height = 23
             Hint = 'State'
             Top = 119
             Width = 80
             DataField = 'custState'
             DataSource = dsCust
             ReadOnly = True
             CharCase = ecUppercase
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 4
           end
           object btnSelCust: TButton
             Left = 24
             Height = 25
             Hint = 'Click to select a Customer'
             Top = 176
             Width = 184
             Caption = 'Select Customer'
             OnClick = btnSelCustClick
             ParentShowHint = False
             ShowHint = True
             TabOrder = 5
           end
           object DBNavigator3: TDBNavigator
             Left = 30
             Height = 22
             Top = 150
             Width = 170
             BevelOuter = bvNone
             ClientHeight = 22
             ClientWidth = 170
             DataSource = dsCust
             TabOrder = 6
             VisibleButtons = [nbFirst, nbPrior, nbNext, nbLast]
           end
         end
         object pnlSales: TPanel
           Left = 255
           Height = 216
           Top = 14
           Width = 260
           BevelInner = bvLowered
           BevelWidth = 2
           ClientHeight = 216
           ClientWidth = 260
           TabOrder = 1
           object Label4: TLabel
             Left = 4
             Height = 18
             Top = 4
             Width = 252
             Align = alTop
             Alignment = taCenter
             Caption = 'Sales Detail'
             Font.Height = -13
             Font.Name = 'Sans'
             Font.Style = [fsBold, fsItalic]
             ParentColor = False
             ParentFont = False
           end
           object btnAddSale: TButton
             Left = 16
             Height = 25
             Hint = 'Add Another Sale'
             Top = 176
             Width = 72
             Caption = 'Add Sale'
             OnClick = btnAddSaleClick
             ParentShowHint = False
             ShowHint = True
             TabOrder = 0
           end
           object Panel1: TPanel
             Left = 4
             Height = 13
             Top = 22
             Width = 252
             Align = alTop
             BevelOuter = bvNone
             TabOrder = 1
           end
           object dgSales: TDBGrid
             Left = 4
             Height = 125
             Hint = 'Click Item Number Button to Select Item'
             Top = 35
             Width = 252
             Align = alTop
             Columns = <          
               item
                 ButtonStyle = cbsEllipsis
                 Title.Caption = 'item Number'
                 Width = 100
                 FieldName = 'itemNum'
               end          
               item
                 Width = 150
                 FieldName = 'item'
               end          
               item
                 FieldName = 'price'
               end          
               item
                 Title.Caption = 'sale Date'
                 FieldName = 'saleDate'
               end          
               item
                 Title.Caption = 'ship Date'
                 FieldName = 'shipDate'
               end>
             DataSource = dsSales
             ShowHint = True
             TabOrder = 2
             TitleFont.Height = -13
             TitleFont.Name = 'Sans'
             OnEditButtonClick = dgSalesEditButtonClick
           end
           object btnSaveEntry: TButton
             Left = 95
             Height = 25
             Hint = 'Save Entry'
             Top = 176
             Width = 51
             Caption = 'Save'
             OnClick = btnSaveEntryClick
             ParentShowHint = False
             ShowHint = True
             TabOrder = 3
           end
           object btnDelEntry: TButton
             Left = 168
             Height = 25
             Hint = 'Delete Entry'
             Top = 176
             Width = 75
             Caption = 'Delete'
             OnClick = btnDelEntryClick
             ParentShowHint = False
             ShowHint = True
             TabOrder = 4
           end
         end
       end
       object pCustomer: TPage
         Caption = 'Customer'
         ClientWidth = 532
         ClientHeight = 259
         object pnlCustomer: TPanel
           Left = 95
           Height = 194
           Top = 30
           Width = 339
           BevelInner = bvLowered
           BevelWidth = 2
           ClientHeight = 194
           ClientWidth = 339
           TabOrder = 0
           object Label5: TLabel
             Left = 72
             Height = 18
             Top = 31
             Width = 73
             Caption = 'First Name'
             Font.Height = -13
             Font.Name = 'Sans'
             Font.Style = [fsItalic]
             ParentColor = False
             ParentFont = False
           end
           object Label7: TLabel
             Left = 4
             Height = 18
             Top = 4
             Width = 331
             Align = alTop
             Alignment = taCenter
             Caption = 'Customer Entry'
             Font.Height = -13
             Font.Name = 'Sans'
             Font.Style = [fsBold, fsItalic]
             ParentColor = False
             ParentFont = False
           end
           object DBEdit10: TDBEdit
             Left = 72
             Height = 23
             Hint = 'Cust name'
             Top = 48
             Width = 184
             DataField = 'CustName'
             DataSource = dsCust
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 0
           end
           object DBEdit12: TDBEdit
             Left = 72
             Height = 23
             Hint = 'Address'
             Top = 72
             Width = 184
             DataField = 'Addr'
             DataSource = dsCust
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 1
           end
           object DBEdit13: TDBEdit
             Left = 72
             Height = 23
             Hint = 'Suburb/Town'
             Top = 96
             Width = 184
             DataField = 'Suburb'
             DataSource = dsCust
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 2
           end
           object DBEdit14: TDBEdit
             Left = 72
             Height = 23
             Hint = 'Postal Code'
             Top = 120
             Width = 80
             DataField = 'postCode'
             DataSource = dsCust
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 3
           end
           object DBEdit15: TDBEdit
             Left = 176
             Height = 23
             Hint = 'State'
             Top = 119
             Width = 80
             DataField = 'custState'
             DataSource = dsCust
             CharCase = ecUppercase
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 4
           end
           object DBNavigator2: TDBNavigator
             Left = 45
             Height = 25
             Top = 154
             Width = 241
             BevelOuter = bvNone
             ClientHeight = 25
             ClientWidth = 241
             DataSource = dsCust
             TabOrder = 5
           end
         end
       end
       object pStock: TPage
         Caption = 'Stock'
         ClientWidth = 532
         ClientHeight = 259
         object pnlStock: TPanel
           Left = 95
           Height = 218
           Top = 22
           Width = 339
           BevelInner = bvLowered
           BevelWidth = 2
           ClientHeight = 218
           ClientWidth = 339
           TabOrder = 0
           object Label8: TLabel
             Left = 4
             Height = 18
             Top = 4
             Width = 331
             Align = alTop
             Alignment = taCenter
             Caption = 'Stock Entry'
             Font.Height = -13
             Font.Name = 'Sans'
             Font.Style = [fsBold, fsItalic]
             ParentColor = False
             ParentFont = False
           end
           object Label9: TLabel
             Left = 75
             Height = 18
             Top = 24
             Width = 83
             Caption = 'Part Number'
             Font.Height = -13
             Font.Name = 'Sans'
             Font.Style = [fsItalic]
             ParentColor = False
             ParentFont = False
           end
           object Label10: TLabel
             Left = 75
             Height = 18
             Top = 72
             Width = 75
             Caption = 'Description'
             Font.Height = -13
             Font.Name = 'Sans'
             Font.Style = [fsItalic]
             ParentColor = False
             ParentFont = False
           end
           object Label11: TLabel
             Left = 75
             Height = 18
             Top = 128
             Width = 33
             Caption = 'Price'
             Font.Height = -13
             Font.Name = 'Sans'
             Font.Style = [fsItalic]
             ParentColor = False
             ParentFont = False
           end
           object DBEdit7: TDBEdit
             Left = 75
             Height = 23
             Top = 40
             Width = 181
             DataField = 'itemNum'
             DataSource = dsStock
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 0
           end
           object DBEdit8: TDBEdit
             Left = 75
             Height = 23
             Top = 89
             Width = 181
             DataField = 'item'
             DataSource = dsStock
             MaxLength = 8192
             ParentShowHint = False
             ShowHint = True
             TabOrder = 1
           end
           object DBEdit9: TDBEdit
             Left = 75
             Height = 23
             Top = 145
             Width = 181
             DataField = 'price'
             DataSource = dsStock
             ParentShowHint = False
             ShowHint = True
             TabOrder = 2
           end
           object DBNavigator1: TDBNavigator
             Left = 48
             Height = 25
             Top = 183
             Width = 241
             BevelOuter = bvNone
             ClientHeight = 25
             ClientWidth = 241
             DataSource = dsStock
             ParentShowHint = False
             ShowHint = True
             TabOrder = 3
           end
         end
       end
     end
     object mmMain: TMainMenu
       left = 16
       top = 277
       object miFile: TMenuItem
         Caption = '&File'
         object miClose: TMenuItem
           Caption = '&Close'
           GlyphShowMode = gsmAlways
           Hint = 'Close Application'
           OnClick = miCloseClick
         end
       end
     end
     object dsCust: TDatasource
       DataSet = TCust
       left = 16
       top = 53
     end
     object dsSales: TDatasource
       DataSet = TSales
       left = 16
       top = 141
     end
     object dsStock: TDatasource
       DataSet = TStock
       left = 16
       top = 214
     end
     object TCust: TSqlite3Dataset
       AutoIncrementKey = True
       Options = []
       PrimaryKey = 'ID'
       SaveOnClose = True
       SaveOnRefetch = True
       SQL = 'Select * from cust;'
       TableName = 'cust'
       FieldDefs = <    
         item
           Name = 'ID'
           DataType = ftAutoInc
           Precision = -1
           Size = 0
         end    
         item
           Name = 'CustName'
           DataType = ftString
           Precision = -1
           Size = 8192
         end    
         item
           Name = 'Addr'
           DataType = ftString
           Precision = -1
           Size = 8192
         end    
         item
           Name = 'Suburb'
           DataType = ftString
           Precision = -1
           Size = 8192
         end    
         item
           Name = 'postCode'
           DataType = ftString
           Precision = -1
           Size = 8192
         end    
         item
           Name = 'custState'
           DataType = ftString
           Precision = -1
           Size = 8192
         end>
       left = 66
       top = 53
       object TCustID: TAutoIncField
         DisplayWidth = 10
         FieldKind = fkData
         FieldName = 'ID'
         Index = 0
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
       end
       object TCustCustName: TStringField
         DisplayWidth = 8192
         FieldKind = fkData
         FieldName = 'CustName'
         Index = 1
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Size = 8192
       end
       object TCustAddr: TStringField
         DisplayWidth = 8192
         FieldKind = fkData
         FieldName = 'Addr'
         Index = 2
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Size = 8192
       end
       object TCustSuburb: TStringField
         DisplayWidth = 8192
         FieldKind = fkData
         FieldName = 'Suburb'
         Index = 3
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Size = 8192
       end
       object TCustpostCode: TStringField
         DisplayWidth = 8192
         FieldKind = fkData
         FieldName = 'postCode'
         Index = 4
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Size = 8192
       end
       object TCustcustState: TStringField
         DisplayWidth = 8192
         FieldKind = fkData
         FieldName = 'custState'
         Index = 5
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Size = 8192
       end
     end
     object TSales: TSqlite3Dataset
       AutoIncrementKey = True
       IndexFieldNames = 'CustID'
       Options = []
       PrimaryKey = 'ID'
       SaveOnClose = True
       SaveOnRefetch = True
       SQL = 'Select * from sales;'
       TableName = 'sales'
       MasterSource = dsCust
       MasterFields = 'ID'
       FieldDefs = <    
         item
           Name = 'itemNum'
           DataType = ftString
           Precision = -1
           Size = 8192
         end    
         item
           Name = 'ID'
           DataType = ftAutoInc
           Precision = -1
           Size = 0
         end    
         item
           Name = 'custID'
           DataType = ftInteger
           Precision = -1
           Size = 0
         end    
         item
           Name = 'saleDate'
           DataType = ftDate
           Precision = -1
           Size = 0
         end    
         item
           Name = 'shipDate'
           DataType = ftDate
           Precision = -1
           Size = 0
         end    
         item
           Name = 'item'
           DataType = ftString
           Precision = -1
           Size = 8192
         end    
         item
           Name = 'price'
           DataType = ftFloat
           Precision = -1
           Size = 0
         end>
       left = 66
       top = 141
       object TSalesitemNum: TStringField
         DisplayWidth = 8192
         FieldKind = fkData
         FieldName = 'itemNum'
         Index = 0
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Size = 8192
       end
       object TSalesID: TAutoIncField
         DisplayWidth = 10
         FieldKind = fkData
         FieldName = 'ID'
         Index = 1
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
       end
       object TSalescustID: TLongintField
         DisplayWidth = 10
         FieldKind = fkData
         FieldName = 'custID'
         Index = 2
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
       end
       object TSalessaleDate: TDateField
         DisplayWidth = 10
         FieldKind = fkData
         FieldName = 'saleDate'
         Index = 3
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
       end
       object TSalesshipDate: TDateField
         DisplayWidth = 10
         FieldKind = fkData
         FieldName = 'shipDate'
         Index = 4
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
       end
       object TSalesitem: TStringField
         DisplayWidth = 8192
         FieldKind = fkData
         FieldName = 'item'
         Index = 5
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Size = 8192
       end
       object TSalesprice: TFloatField
         DisplayWidth = 10
         FieldKind = fkData
         FieldName = 'price'
         Index = 6
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Currency = True
         MaxValue = 0
         MinValue = 0
         Precision = -1
       end
     end
     object TStock: TSqlite3Dataset
       AutoIncrementKey = True
       Options = []
       PrimaryKey = 'ID'
       SaveOnClose = True
       SaveOnRefetch = True
       SQL = 'Select * from stock;'
       TableName = 'stock'
       FieldDefs = <    
         item
           Name = 'ID'
           DataType = ftAutoInc
           Precision = -1
           Size = 0
         end    
         item
           Name = 'item'
           DataType = ftString
           Precision = -1
           Size = 8192
         end    
         item
           Name = 'price'
           DataType = ftFloat
           Precision = -1
           Size = 0
         end    
         item
           Name = 'itemNum'
           DataType = ftString
           Precision = -1
           Size = 8192
         end>
       left = 66
       top = 214
       object TStockID: TAutoIncField
         DisplayWidth = 10
         FieldKind = fkData
         FieldName = 'ID'
         Index = 0
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
       end
       object TStockitem: TStringField
         DisplayWidth = 8192
         FieldKind = fkData
         FieldName = 'item'
         Index = 1
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Size = 8192
       end
       object TStockprice: TFloatField
         DisplayWidth = 10
         FieldKind = fkData
         FieldName = 'price'
         Index = 2
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Currency = True
         MaxValue = 0
         MinValue = 0
         Precision = -1
       end
       object TStockitemNum: TStringField
         DisplayWidth = 8192
         FieldKind = fkData
         FieldName = 'itemNum'
         Index = 3
         LookupCache = False
         ProviderFlags = [pfInUpdate, pfInWhere]
         ReadOnly = False
         Required = False
         Size = 8192
       end
     end
    

end 
    
    
    unit uCust;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, LResources, Forms, Controls, Graphics, Dialogs,
      StdCtrls, ExtCtrls, Buttons, DBGrids,DB;
    
    type
    
      { TcustDlg }
    
      TcustDlg = class(TForm)
        cancelBtn: TButton;
        dgCust: TDBGrid;
        edSearch: TEdit;
        Label1: TLabel;
        okBtn: TButton;
        pnlCust: TPanel;
        sbSearch: TSpeedButton;
        procedure dgCustDblClick(Sender: TObject);
        procedure edSearchChange(Sender: TObject);
        procedure sbSearchClick(Sender: TObject);
      private
        function GetCust: String;
        procedure SetCust(const AValue: String);
        { private declarations }
      public
        { public declarations }
        property CustName: String Read GetCust Write SetCust;
        function ShowModalCust:integer;
      end; 
    
    var
      custDlg: TcustDlg;
    
    implementation
    uses uMain;
    { TcustDlg }
    
    procedure TcustDlg.edSearchChange(Sender: TObject);
    begin
      sbSearch.Enabled:=edSearch.Text<>'';
    end;
    
    procedure TcustDlg.dgCustDblClick(Sender: TObject);
    begin
      ModalResult := mrOk;
    end;
    
    procedure TcustDlg.sbSearchClick(Sender: TObject);
    begin
      if not fMain.TCust.Locate('CustName', edSearch.Text,[loCaseInsensitive, loPartialKey])
       then
          MessageDlg('No matching record found.', mtInformation, [mbOK], 0);
          edSearch.Color:=clRed;
    end;
    
    function TcustDlg.GetCust: String;
    begin
      Result := fMain.TCustCustName.Value;
    end;
    
    procedure TcustDlg.SetCust(const AValue: String);
    begin
      fMain.TCust.Locate('CustName',AValue,[loPartialKey,loCaseInsensitive]);
    end;
    
    function TcustDlg.ShowModalCust: integer;
    begin
      Caption:='Select Customer Name';
      Result := ShowModal;
    end;
    
    initialization
      {$I uCust.lrs}
    
    end.
    
    
    
    unit uSales;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, LResources, Forms, Controls, Graphics, Dialogs,
      StdCtrls, ExtCtrls, Buttons, DBGrids, DB;
    
    type
    
      { TsearchDlg }
    
      TsearchDlg = class(TForm)
        cancelBtn: TButton;
        dgParts: TDBGrid;
        edSearch: TEdit;
        Label1: TLabel;
        okBtn: TButton;
        pnlParts: TPanel;
        sbSearch: TSpeedButton;
        procedure dgPartsDblClick(Sender: TObject);
        procedure edSearchChange(Sender: TObject);
        procedure sbSearchClick(Sender: TObject);
      private
        function GetPartNum: String;
        procedure SetPartNum(const AValue: String);
        { private declarations }
      public
        { public declarations }
        property PartNum:String Read GetPartNum Write SetPartNum;
        function ShowModalParts: Integer;
      end; 
    
    var
      searchDlg: TsearchDlg;
    
    implementation
    uses uMain;
    { TsearchDlg }
    
    procedure TsearchDlg.edSearchChange(Sender: TObject);
    begin
      sbSearch.Enabled:=edSearch.Text<>'';
    end;
    
    procedure TsearchDlg.dgPartsDblClick(Sender: TObject);
    begin
      ModalResult := mrOk;
    end;
    
    procedure TsearchDlg.sbSearchClick(Sender: TObject);
    begin
      if not fMain.TStock.Locate('itemNum', edSearch.Text,[loCaseInsensitive, loPartialKey])
       then
          MessageDlg('No matching record found.', mtInformation, [mbOK], 0);
          edSearch.Color:=clRed;
    end;
    
    function TsearchDlg.GetPartNum: String;
    begin
      Result := fMain.TStockitemNum.Value;
    end;
    
    procedure TsearchDlg.SetPartNum(const AValue: String);
    begin
      fMain.TStock.Locate('itemNum',AValue,[loPartialKey,loCaseInsensitive]);
    end;
    
    function TsearchDlg.ShowModalParts: Integer;
    begin
      Caption:='Select Item Number';
      Result := ShowModal;
    end;
    
    initialization
      {$I uSales.lrs}
    
    end.

---

_Source: [https://wiki.freepascal.org/TSqlite3_Master_Detail_Example](https://web.archive.org/web/20231216235239/https://wiki.freepascal.org/TSqlite3_Master_Detail_Example)_
