# CDDB

English (en)

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The source mentioned here is no longer available. However FPC itself provides a unit for CDDB as [fpcddb](<https://www.freepascal.org/daily/packages/cdrom/fpcddb/index.html>). An article that explains its use is available [here](<https://www.freepascal.org/~michael/articles/cddb/cddb.pdf>). 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The CDDB service on FreeDB is no longer available. An alternative is provided by GNUDB as [http://gnudb.gnudb.org](<http://gnudb.gnudb.org/>).

  


## Contents

  * 1 About
  * 2 License
  * 3 Download
  * 4 Dependencies / System Requirements
  * 5 Installation
  * 6 Use



### About

_CDDB_ is a simple unit for accessing CD track information from CDDB/FreeDB. There is little documentation but sample code is provided below. 

### License

[BSD](<http://www.opensource.org/licenses/bsd-license.php>)

### Download

The latest can be found [here](<http://www.simmonsclan.net/fpc/cddb.pas>) \- This link appears to be dead. Is there an active copy of this somewhere? 

### Dependencies / System Requirements

  * Linux
  * Synapse httpsend unit



### Installation

Copy the unit into the source directory for your project. 

### Use

The first step to accessing CDDB is creating TCDDB. This is where the connection and application information is defined. TCDDB has some default values so you don't have to define them all. The default server is [FreeDB](<http://freedb.freedb.org>). 
    
    
    procedure CreateCDDB;
    var
      CDDB: TCDDB;
    begin
      CDDB := TCDDB.Create;
      CDDB.ServerURL := 'http://freedb.freedb.org/~cddb/cddb.cgi'; // this is the server to access
      CDDB.ClientName := 'fpccddb'; // your application name that is passed to the server
    end;
    

First try using the MOTD (Message of the Day) command. 
    
    
    procedure TForm1.btnMOTDClick(Sender: TObject);
    var
      CDDB: TCDDB;
      cmd: TCDDBMOTD;
    begin
      CDDB := TCDDB.Create;
      cmd := TCDDBMOTD.Create(CDDB);
      try
        cmd.Execute;
        Memo1.Lines.Assign(cmd.MOTDResponse.Response);
      finally
        cmd.Free;
        CDDB.Free;
      end;
    end;
    

You can get a list of supported categories using TCDDBListCategories 
    
    
    procedure TForm1.btnCategoriesClick(Sender: TObject);
    var
      CDDB: TCDDB;
      cmd: TCDDBListCategories;
    begin
      CDDB := TCDDB.Create;
      cmd := TCDDBListCategories.Create(CDDB);
      try
        cmd.Execute;
        lbxCategories.Items.Assign(cmd.ListCategoriesResponse.Categories);
      finally
        cmd.Free;
        CDDB.Free;
      end;
    end;
    

This last example will query the database using the cdrom '/dev/cdrom'. The result is loaded into a TStringGrid named grdQueryResponse. 
    
    
    procedure TForm1.btnQueryClick(Sender: TObject);
    var
      CDDB: TCDDB;
      Cmd: TCDDBQuery;
      i: Integer;
    begin
      CDDB := TCDDB.Create;
      Cmd := TCDDBQuery.Create(CDDB);
      try
        Cmd.Device := '/dev/cdrom'
        try
          Cmd.Execute;
        except on E:Exception do
          begin
            ShowMessage(E.Message);
            Exit;
          end;
        end;
      
        with Cmd.QueryResponse do
        begin
          for i := 0 to ResponseCount-1 do
          begin
            grdQueryResponse.RowCount := grdQueryResponse.RowCount+1;
            grdQueryResponse.Cells[0,i+1] := Artist[i];
            grdQueryResponse.Cells[1,i+1] := Album[i];
            grdQueryResponse.Cells[2,i+1] := Category[i];
            grdQueryResponse.Cells[3,i+1] := DiscID[i];
          end;
        end;
      finally
        cmd.Free;
        CDDB.Free
      end;
    end;

---

_Source: [https://wiki.freepascal.org/CDDB](https://web.archive.org/web/20250417160734/https://wiki.freepascal.org/CDDB)_
