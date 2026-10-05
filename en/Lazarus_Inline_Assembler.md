# Lazarus Inline Assembler

│ **English (en)** │  **[español (es)](</Lazarus_Inline_Assembler/es> "Lazarus Inline Assembler/es")** │  **[français (fr)](</Lazarus_Inline_Assembler/fr> "Lazarus Inline Assembler/fr")** │  **[日本語 (ja)](</Lazarus_Inline_Assembler/ja> "Lazarus Inline Assembler/ja")** │  **[한국어 (ko)](</Lazarus_Inline_Assembler/ko> "Lazarus Inline Assembler/ko")** │  **[русский (ru)](<../ru/Lazarus_Inline_Assembler.md> "Lazarus Inline Assembler/ru")** │  **[Tiếng Việt (vi)](</Lazarus_Inline_Assembler/vi> "Lazarus Inline Assembler/vi")** │    
****

This is a stub to encourage others to contribute further. Here is a very simple example to get you started: 
    
    
    unit unt_asm;
    {$mode objfpc}{$H+}
    interface
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs, StdCtrls;
    type
      { TForm1 }
      TForm1 = class(TForm)
        btnGo: TButton;
        edtInput: TEdit;
        edtOutput: TEdit;
        Label1: TLabel;
        Label2: TLabel;
        procedure btnGoClick(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end; 
    var
      Form1: TForm1; 
    implementation
    { TForm1 }
    
    procedure TForm1.btnGoClick(Sender: TObject);
    var
      num, answer : integer;
    begin
      num := StrToInt(edtInput.Text);
      //This is required with Lazarus on x86:
      {$ASMMODE intel}
      asm
        MOV EAX, num
        ADD EAX, 110B //add binary 110
        SUB EAX, 2    //subtract decimal 2
        MOV answer, EAX
      end;
      edtOutput.Text := IntToStr(answer);
    end;
    
    initialization
      {$I unt_asm.lrs}
    end.
    

## See also

  * [VirtualTreeview Example for Lazarus](<VirtualTreeview_Example_for_Lazarus.md> "VirtualTreeview Example for Lazarus")

---

_Source: [https://wiki.freepascal.org/Lazarus_Inline_Assembler](https://web.archive.org/web/20250317150203/https://wiki.freepascal.org/Lazarus_Inline_Assembler)_
