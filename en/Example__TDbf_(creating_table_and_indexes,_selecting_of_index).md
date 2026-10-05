# Example: TDbf (creating table and indexes, selecting of index)

This example was taken from the post at the forum:  
<http://forum.lazarus.freepascal.org/index.php/topic,31169.msg199356.html#msg199356>

  
You can copy text to files: 

  * Code: **mainaddindex.pas**
  * Form: **mainaddindex.lfm**



and test it. 

  


### Code
    
    
    unit MainAddIndex;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, db, dbf, Forms, Controls, Graphics, Dialogs,
      DBGrids, DbCtrls, StdCtrls, LazFileUtils;
    
    const
      CountriesFilename = 'countries.dbf';
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        BSortByCountry: TButton;
        BSortByCapital: TButton;
        BSortByID: TButton;
        dbfSource: TDataSource;
        grid: TDBGrid;
        Navigator: TDBNavigator;
        procedure BSortByCapitalClick(Sender: TObject);
        procedure BSortByCountryClick(Sender: TObject);
        procedure BSortByIDClick(Sender: TObject);
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      private
        CountriesDbf: TDbf;
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
      CountriesDbf:=TDbf.Create(Self);
      CountriesDbf.TableName:=CountriesFilename;
      if not FileExistsUTF8(CountriesFilename) then with CountriesDbf do begin
        TableLevel:=7;
        Exclusive:=True;
        FieldDefs.Add('ID', ftAutoInc, 0, True);
        FieldDefs.Add('CountryName', ftString, 25, True);
        FieldDefs.Add('Capital', ftString, 25, True);
        CreateTable;
        Open;
        Append;
        // Fields[0].AsInteger:=100;  // Do not fill 'ID' because its type is ftAutoInc
        fields[1].AsString:='France';
        Fields[2].AsString:='Paris';
        Post;
        Append;
        // Fields[0].AsInteger:=101;
        fields[1].AsString:='Egypt';
        Fields[2].AsString:='Cairo';
        Post;
        Append;
        // Fields[0].AsInteger:=102;
        fields[1].AsString:='Indonesia';
        Fields[2].AsString:='Jakarta';
        Post;
        Append;
        // Fields[0].AsInteger:=103;
        fields[1].AsString:='Austria';
        Fields[2].AsString:='Vienna';
        Post;
        AddIndex('idxByID', 'ID', [ixPrimary,ixUnique]);
        AddIndex('idxByCountry', 'CountryName', [ixCaseInsensitive]);
        AddIndex('idxByCapital', 'Capital', [ixCaseInsensitive]);
      end;
      CountriesDbf.Open;
      dbfSource.DataSet:=CountriesDbf;
      grid.DataSource:=dbfSource;
      Navigator.DataSource:=dbfSource;
    end;
    
    procedure TForm1.BSortByCapitalClick(Sender: TObject);
    begin
      CountriesDbf.IndexName:='idxByCapital';
    end;
    
    procedure TForm1.BSortByCountryClick(Sender: TObject);
    begin
      CountriesDbf.IndexName:='idxByCountry';
    end;
    
    procedure TForm1.BSortByIDClick(Sender: TObject);
    begin
      CountriesDbf.IndexName:='idxByID';
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      with CountriesDbf do begin
        CloseIndexFile('idxByCapital');
        CloseIndexFile('idxByCountry');
        CloseIndexFile('idxByID');
      end;
      CountriesDbf.Close;
    end;
    
    end.
    

  


### Form
    
    
    object Form1: TForm1
      Left = 229
      Height = 270
      Top = 94
      Width = 420
      Caption = 'Form1'
      ClientHeight = 270
      ClientWidth = 420
      OnCreate = FormCreate
      OnDestroy = FormDestroy
      LCLVersion = '1.6.0.2'
      object grid: TDBGrid
        Left = 0
        Height = 200
        Top = 0
        Width = 420
        Align = alTop
        Color = clWindow
        Columns = <>
        TabOrder = 0
      end
      object Navigator: TDBNavigator
        Left = 0
        Height = 25
        Top = 200
        Width = 420
        Align = alTop
        BevelOuter = bvNone
        ChildSizing.EnlargeHorizontal = crsScaleChilds
        ChildSizing.EnlargeVertical = crsScaleChilds
        ChildSizing.ShrinkHorizontal = crsScaleChilds
        ChildSizing.ShrinkVertical = crsScaleChilds
        ChildSizing.Layout = cclLeftToRightThenTopToBottom
        ChildSizing.ControlsPerLine = 100
        ClientHeight = 25
        ClientWidth = 420
        Options = []
        TabOrder = 1
      end
      object BSortByCountry: TButton
        Left = 12
        Height = 23
        Top = 234
        Width = 101
        AutoSize = True
        Caption = 'Sort by country'
        OnClick = BSortByCountryClick
        TabOrder = 2
      end
      object BSortByCapital: TButton
        Left = 168
        Height = 23
        Top = 234
        Width = 95
        AutoSize = True
        Caption = 'Sort by capital'
        OnClick = BSortByCapitalClick
        TabOrder = 3
      end
      object BSortByID: TButton
        Left = 318
        Height = 23
        Top = 234
        Width = 75
        AutoSize = True
        Caption = 'Sort by ID'
        OnClick = BSortByIDClick
        TabOrder = 4
      end
      object dbfSource: TDataSource
        left = 53
        top = 99
      end
    end
    

## See also

  * [Lazarus Tdbf Tutorial](<Lazarus_Tdbf_Tutorial.md> "Lazarus Tdbf Tutorial")
  * [Guide for usage of Tdbf component/ru](<../ru/Guide_for_usage_of_Tdbf_component.md> "Guide for usage of Tdbf component/ru") (rus)

---

_Source: [https://wiki.freepascal.org/Example%3A_TDbf_(creating_table_and_indexes%2C_selecting_of_index)](https://web.archive.org/web/20250419150555/https://wiki.freepascal.org/Example%3A_TDbf_(creating_table_and_indexes%2C_selecting_of_index))_
