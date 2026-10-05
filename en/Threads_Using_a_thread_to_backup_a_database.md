# Threads Using a thread to backup a database

│ **English (en)** │

## Sample code illustrating use of threads

\- by forum user ALAU2007 
    
    
    {
     ***************************************************************************
     *                                                                         *
     *   This source is free software; you can redistribute it and/or modify   *
     *   it under the terms of the GNU General Public License as published by  *
     *   the Free Software Foundation; either version 2 of the License, or     *
     *   (at your option) any later version.                                   *
     *                                                                         *
     *   This code is distributed in the hope that it will be useful, but      *
     *   WITHOUT ANY WARRANTY; without even the implied warranty of            *
     *   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU     *
     *   General Public License for more details.                              *
     *                                                                         *
     *   A copy of the GNU General Public License is available on the World    *
     *   Wide Web at <http://www.gnu.org/copyleft/gpl.html>. You can also      *
     *   obtain it by writing to the Free Software Foundation,                 *
     *   Inc., 59 Temple Place - Suite 330, Boston, MA 02111-1307, USA.        *
     *                                                                         *
     ***************************************************************************
    
      Abstract:
        Demo to show, how to start a thread and how synchronize with the main
        thread.
        Important: The cthread unint must be added to the uses section of the .lpr
                   file. See multithreadingexample1.lpr.
    }
    unit MainUnit;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls, ComCtrls,
      sqldb, mssqlconn;
    
    type
    
      { TMyBackupThread }
    
      TMyBackupThread = class(TThread)
      private
        fStatusText: string;
        procedure ShowStatus;
      protected
        procedure Execute; override;
      public
        constructor Create(CreateSuspended: boolean);
      end;
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        Label1: TLabel;
        MSSQLConnection1: TMSSQLConnection;
        ProgressBar1: TProgressBar;
        SQLQuery1: TSQLQuery;
        SQLTransaction1: TSQLTransaction;
        procedure Button1Click(Sender: TObject);
      private
      public
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      MyBackupThread : TMyBackupThread;
    begin
    
      MyBackupThread := TMyBackupThread.Create(True); // With the True parameter it doesn't start automatically
      if Assigned(MyBackupThread.FatalException) then
        raise MyBackupThread.FatalException;
    
      // Here the code initialises anything required before the threads starts executing
    
      //Test_Dummy
      MyBackupThread.Start;
    
      Label1.Caption := 'Start';
      SQLQuery1.Active := False;
      while ( ProgressBar1.Position < 100 ) do begin
        SQLQuery1.Active := true;
        SQLQuery1.ExecSQL;
        ProgressBar1.Position := SQLQuery1.FieldByName('Percent Complete').AsInteger;
        Label1.Caption := FormatFloat( '##0%', SQLQuery1.FieldByName('Percent Complete').AsInteger ) 
              + '%, Estimated completion Time ' + FormatFloat( '#0.00', SQLQuery1.FieldByName('ETA Min').AsFloat ) + ' min.';
        Application.ProcessMessages;
        SQLQuery1.Active := False;
      end;
    end;
    
    {TBackupThread}
    
    procedure TMyBackupThread.ShowStatus;
    // this method is only called by Synchronize(@ShowStatus) and therefore
    // executed by the main thread
    // The main thread can access GUI elements, for example Form1.Caption.
    begin
      Form1.Label1.Caption := fStatusText;
    end;
    
    procedure TMyBackupThread.Execute;
    var
      Conn: TMSSQLConnection;
      Tran: TSQLTransaction;
    begin
      fStatusText := 'Backup Starting ...';
      Synchronize(@Showstatus);  //If I remark this, it causes "access violation" error
      Conn:=TMSSQLConnection.create(nil);
      Tran:=TSQLTransaction.create(nil);
      try
        Conn.HostName:='127.0.0.1\sqlexpress';
        Conn.UserName:=''; //trusted authentication/SSPI
        Conn.Password:=''; //trusted authentication/SSPI
        Conn.DatabaseName:='Test_Dummy';
        Conn.Params.Add('AutoCommit=true');
        Conn.Transaction:=Tran;
        Conn.Open;
        Conn.ExecuteDirect('backup database Test_Dummy to disk = N''C:\TEMP\TEST_Dummy.bak'' with format, init');
        Conn.Close;
        fStatusText := 'Backup Completed';
        Synchronize(@Showstatus);
      finally
        Tran.Free;
        Conn.Free;
      end;
    end;
    
    constructor TMyBackupThread.Create(CreateSuspended: boolean);
    begin
      FreeOnTerminate := True;
      inherited Create(CreateSuspended);
    end;
    
    end.

---

_Source: [https://wiki.freepascal.org/Threads_Using_a_thread_to_backup_a_database](https://web.archive.org/web/20250120194658/https://wiki.freepascal.org/Threads_Using_a_thread_to_backup_a_database)_
