# Example-TSynEdit-HTML

In the example below is a [TSynEdit](<TSynEdit.md> "TSynEdit") used with a [TSynHTMLSyn](<TSynHTMLSyn.md> "TSynHTMLSyn"). The results are displayed as formatted HTML using a [TIpHtmlPanel](<TIpHtmlPanel.md> "TIpHtmlPanel"). 

[![component-TSynEdit+TIpHtmlPanel.png](https://wiki.freepascal.org/images/1/1a/component-TSynEdit%2BTIpHtmlPanel.png)](</File:component-TSynEdit%2BTIpHtmlPanel.png>)
    
    
    unit TipHtmlTest;
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, SysUtils, FileUtil, IpHtml, SynHighlighterHTML, SynHighlighterAny,
      SynEdit, Forms, Controls, Graphics, Dialogs, ExtCtrls, StdCtrls;
     
    const
      htmfile = 'tiphtmltestdata.html';
     
    type
      TTipHtmlForm = class(TForm)
        IpHtmlPanel1: TIpHtmlPanel;
        Splitter1: TSplitter;
        Memo1: TSynEdit;
        SynHTMLSyn1: TSynHTMLSyn;
        procedure FormClose(Sender: TObject; var CloseAction: TCloseAction);
        procedure FormCreate(Sender: TObject);
        procedure MemoChange(Sender: TObject);
      protected
      public
        procedure ShowText( const txt: string );
      end;
     
    var
      TipHtmlForm: TTipHtmlForm;
     
    implementation
     
    {$R *.lfm}
     
    procedure TTipHtmlForm.FormCreate(Sender: TObject);
    begin
      if FileExists( htmfile ) then
        Memo1.Lines.LoadFromFile( htmfile )
      else
        Memo1.Lines.Text := '<html><head><title>tipmemo</title></head><body><h1>tipmemo</h1>See <b>' +htmfile + '</b></body></html>';
      MemoChange( nil );
    end;
     
     
    procedure TTipHtmlForm.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    begin
      Memo1.Lines.SaveToFile( htmfile );
    end;
     
     
    procedure TTipHtmlForm.MemoChange(Sender: TObject);
    begin
      ShowText( Memo1.Lines.Text );
    end;
     
     
    procedure TTipHtmlForm.ShowText( const txt: string );
    var
      fs: TStringStream;
      pHTML: TIpHtml;
    begin
      try
        fs := TStringStream.Create( txt );
        try
          pHTML:=TIpHtml.Create; // Beware: Will be freed automatically by IpHtmlPanel1
          pHTML.LoadFromStream(fs);
        finally
          fs.Free;
        end;
        IpHtmlPanel1.SetHtml( pHTML );
        Caption := IpHtmlPanel1.Title;
      except
        on E: Exception do begin
          MessageDlg( 'Error: '+E.Message, mtError, [mbCancel], 0 );
        end;
      end;
    end;

[![component-TSynEdit+TIpHtmlPanel-r.png](https://wiki.freepascal.org/images/c/c5/component-TSynEdit%2BTIpHtmlPanel-r.png)](</File:component-TSynEdit%2BTIpHtmlPanel-r.png>)

---

_Source: [https://wiki.freepascal.org/Example-TSynEdit-HTML](https://web.archive.org/web/20190604103215/https://wiki.freepascal.org/Example-TSynEdit-HTML)_
