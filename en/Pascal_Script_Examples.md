# Pascal Script Examples

│ **English (en)** │

This is a simple example of a actual [script](<Pascal_Script.md> "Pascal Script") that shows how to do try except with raising a exception and doing something with the exception message. 
    
    
    var
       filename,emsg:string;
    begin
        filename = '';
        try
           if filename = '' then
              RaiseException(erCustomError, 'File name cannot be blank');
    
        except
              emsg:=ExceptionToString(ExceptionType, ExceptionParam);
              //do somethign with the exception message i.e. email it or
              //save to a log etc
        end;
    
    end.
    

To run the above script drop a TPSScript component on your form and either copy the above script to the script property or use the script properties LoadFromFile. We will call the TPSScript component "ps_script" for this example. 

Place a button on your form and create a new Onclick event for it and add this to it: 
    
    
    ps_script.Script.LoadFromFile('yourscript.txt');
    if ps_script.compile then
       ps_script.execute
    else
       //show any compile errors
       showmessage(ps_script.CompilerErrorToStr(0));
    

Ok, what if some standard functions are not available in the base scripting engine? No problem, just create the OnCompile event for the TPSScript component. Here we extend the script engine by adding two functions from the standard sysutils that don't seem to be included with the base engine. 
    
    
    procedure TForm1.ps_ScriptCompile(Sender: TPSScript);
    begin
         sender.AddFunction(@ExtractFileExt,'function ExtractFileExt(const FileName: string): string;');
         sender.AddFunction(@ExtractFileName,'function ExtractFileName(const FileName: string): string;');
     end;
    

Your script will now have access to these functions. 

  


  


The following examples are FPC code and do not show a script. 
    
    
    program psce;
    //enhanced with compiler messages to the shell and output to shell
    //bytecode and dissasembly output
    //jan 2011 www.softwareschule.ch/maxbox.htm, loc's =218
    {$APPTYPE CONSOLE}
    
    {$IFDEF FPC}
    {$mode delphi}{$H+}
    {$ENDIF}
    
    uses
      SysUtils,
      Classes,
      Forms,
      uPSCompiler,
      uPSR_std,
      uPSC_std,
      uPSR_classes,
      uPSC_classes,
      uPSC_controls,
      uPSR_controls,
      uPSC_forms,
      uPSR_forms,
      uPSRuntime,
      uPSComponent,
      uPSDisassembly,
      uPSR_dateutils,
      uPSC_dateutils,
      uPSR_dll,
      uPSC_dll;
    
    type
      TPSCE = class
       protected
        FScr: TPSScript;
        procedure SaveCompiled(var Data: String);
        procedure SaveDissasembly(var Data: String);
        procedure OnCompile(Sender: TPSScript);
        procedure OnExecImport(Sender: TObject; se: TPSExec;
                                       x: TPSRuntimeClassImporter);
       public
        constructor Create;
        destructor Destroy; override;
        function Compile(const FileName: string): Boolean;
        function Execute: Boolean;
      end;
    
    
    var
      aPSCE: TPSCE;
      SFile, sData: String;
    
    
    procedure MWritedt(d : TDateTime);
    var
     s: String;
    begin
      s:= DateToStr(d) + ' ' + TimeToStr(d);
      Write(s);
    end;
    
    
    procedure MWrites(const s: string);
    begin
      Write(s);
    end;
    
    procedure MWritei(const i: Integer);
    begin
      Write(i);
    end;
    
    procedure MWrited(const d: Double);
    begin
      Write(d:0:1);
    end;
    
    procedure MWriteln;
    begin
      Writeln;
    end;
    
    procedure MyVal(const s: string; var n, z: Integer);
    begin
      Val(s, n, z);
    end;
    
    constructor TPSCE.Create;
    begin
      FScr:= TPSScript.Create(nil);
      FScr.OnCompile:= OnCompile;
      FScr.OnExecImport:= OnExecImport;
    end;
    
    destructor TPSCE.Destroy;
    begin
      FScr.Free;
    end;
    
    procedure TPSCE.SaveCompiled(var Data : String);
    var
      OutFile: string;
      Fx: Longint ;
    begin
      OutFile:= ExtractFilePath(ParamStr(0)) + ChangeFileExt(SFile,'.out');
      Fx:= FileCreate(OutFile) ;
      FileWrite(Fx,Data[1],Length(Data));
      FileClose(Fx) ;
    end;
    
    procedure TPSCE.SaveDissasembly(var Data: String);
    var
      OutFile: string;
      Fx: Longint ;
    begin
      OutFile:= ExtractFilePath(ParamStr(0)) + ChangeFileExt(SFile,'.dis');
      Fx:= FileCreate(OutFile) ;
      FileWrite(Fx, Data[1], Length(Data));
      FileClose(Fx) ;
    end;
    
    
    procedure TPSCE.OnExecImport(Sender: TObject; se: TPSExec; x: TPSRuntimeClassImporter);
    begin
      RIRegister_Std(x);
      RIRegister_Classes(x,true);
      RIRegister_Controls(x);
      RIRegister_Forms(x);
      RegisterDateTimeLibrary_R(se);
      RegisterDLLRuntime(se);
    end;
    
    procedure TPSCE.OnCompile(Sender: TPSScript);
    begin
      RegisterDateTimeLibrary_C(Sender.Comp);
      Sender.AddFunction(@MWrites, 'procedure Writes(const s: string)');
      Sender.AddFunction(@MWritedt,'procedure WriteDT(d : TDateTime)');
      Sender.AddFunction(@MWritei, 'procedure Writei(const i: Integer)');
      Sender.AddFunction(@MWrited, 'procedure Writed(const f: Double)');
      Sender.AddFunction(@MWriteln, 'procedure Writeln');
      Sender.AddFunction(@MyVal, 'procedure Val(const s: string; var n, z: Integer)');
      Sender.AddFunction(@FileCreate, 'Function FileCreate(const FileName: string): integer)');
      Sender.AddFunction(@FileWrite, 'function FileWrite(Handle: Integer; const Buffer: pChar; Count: LongWord): Integer)');
      Sender.AddFunction(@FileClose, 'Procedure FileClose(handle: integer)');
      //Sender.AddRegisteredVariable('Application', 'TApplication');
      SIRegister_Std(Sender.Comp);
      SIRegister_Classes(Sender.Comp,true);
      SIRegister_Controls(Sender.Comp);
      SIRegister_Forms(Sender.Comp);
    end;
    
    
    function TPSCE.Compile(const FileName: string): Boolean;
    var
      S: TStringList;
      i: Integer;
    begin
      Result:= False;
      if FileExists(FileName) then begin
        S:= TStringList.Create;
        S.LoadFromFile(FileName);
        FScr.Script:= S;
        Result:= FScr.Compile;
        for i:= 0 to aPSCE.FScr.CompilerMessageCount - 1 do
          writeln(aPSCE.FScr.CompilerMessages[i].MessageToString);
        S.Free;
        if not Result then
          if FScr.CompilerMessageCount > 0 then
            for i:= 0 to FScr.CompilerMessageCount-1 do
              Writeln(FScr.CompilerErrorToStr(i));
          end else Writeln('Script File not found: ', FileName);
    end;
    
    function TPSCE.Execute: Boolean;
    begin
      //FScr.SetVarToInstance('APPLICATION', Application);
      //FScr.SetVarToInstance('SELF', Self);
      Result:= FScr.Execute;
      //writeln(FScr.About);
      if not Result then
        Writeln('Run-time error:' + FScr.ExecErrorToString);
    end;
    
    
    begin  //main
      Application.Initialize;
      aPSCE:= TPSCE.Create;
      if ParamCount = 0 then begin
        Writeln('Usage: ', ExtractFileName(ParamStr(0)), ' [--compile|--dissasembly] <script.pss>');
        Writeln('');
        Writeln('--compile : Save compiled script bytecode');
        Writeln('--dissasembly: Save dissasembly of script');
        Exit;
      end;
     SFile:= ParamStr(1);
     if (ParamStr(1)='--compile') or (ParamStr(1)='--dissasembly') then begin
      SFile:= ParamStr(2);
      aPSCE.Compile(SFile);
      aPSCE.Execute;   //output on shell
      aPSCE.FScr.GetCompiled(sData);
      if Paramstr(1)='--compile' then begin
        aPSCE.FScr.Comp.GetOutput(sData);
        aPSCE.SaveCompiled(sData);
      end;
      if Paramstr(1)='--dissasembly' then begin
        aPSCE.FScr.GetCompiled(sData);
        if not IFPS3DataToText(sData, sData)
          then begin
            Writeln('Create or create not dissasembly!');
            aPSCE.SaveDissasembly(sData);  //do it anyway
        end else
        aPSCE.SaveDissasembly(sData);
      end;
      Exit;
     end;
     aPSCE.Compile(SFile);
     aPSCE.Execute;
     aPSCE.Free;
    end.
    

  
2\. Example of Lazarus with GUI Components 
    
    
    unit unit1pscript2;
    //compiled by max
    ////oct 2014: www.softwareschule.ch/maxbox.htm
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, SynMemo, SynHighlighterPas, uPSComponent,
      uPSComponent_Default, uPSComponent_StdCtrls, uPSComponent_Forms, Forms,
      Controls, Graphics, Dialogs, StdCtrls, Buttons, ExtCtrls, uPSRuntime,
      uPSComponent_DB, uPSCompiler;
    
    type
    
      { TpsForm1 }
       TpsForm1 = class(TForm)
        btnImport: TBitBtn;
        btnCompile: TBitBtn;
        btnSaveScript: TBitBtn;
        btnSaveComp: TBitBtn;
        btnLoadScript: TBitBtn;
        btngetCompiled: TBitBtn;
        btnExecute: TButton;
        btnRunbytecode: TButton;
        Image1: TImage;
        Image2: TImage;
        Memo1: TMemo;
        PSImport_Classes1: TPSImport_Classes;
        PSImport_DateUtils1: TPSImport_DateUtils;
        PSImport_DB1: TPSImport_DB;
        PSImport_Forms1: TPSImport_Forms;
        PSImport_StdCtrls1: TPSImport_StdCtrls;
        PSScript1: TPSScript;
        SynMemo1: TSynMemo;
        SynPasSyn1: TSynPasSyn;
        procedure btnImportClick(Sender: TObject);
        procedure btnLoadScriptClick(Sender: TObject);
        procedure btnRunbytecodeClick(Sender: TObject);
        procedure Compile1Click(Sender: TObject);
        procedure btnSaveScriptClick(Sender: TObject);
        procedure btnSaveCompClick(Sender: TObject);
        procedure btngetCompiledClick(Sender: TObject);
        procedure btnExecuteClick(Sender: TObject);
        procedure FormActivate(Sender: TObject);
        procedure PSScript1AfterExecute(Sender: TPSScript);
        procedure PSScript1Compile(Sender: TPSScript);
        procedure PSScript1CompImport(Sender: TObject; x: TPSPascalCompiler);
        procedure PSScript1ExecImport(Sender: TObject; se: TPSExec;
          x: TPSRuntimeClassImporter);
        procedure SynMemo1Change(Sender: TObject);
      private
        function RunCompiledScript2(Bytecode: AnsiString; out
          RuntimeErrors: AnsiString): Boolean;
        { private declarations }
      public
        { public declarations }
      end;
    
    Const SCRIPTFILE = 'paswiki2.txt';
    
    var
      psForm1: TpsForm1;
    
    implementation
    
    {$R *.lfm}
    
     uses  uPSDisassembly;
    
    { TpsForm1 }
    
    procedure TpsForm1.btnExecuteClick(Sender: TObject);
    var res: boolean;
    begin
      //showmessage('run max box');
       Res:= PSScript1.Execute;
       if not Res then
         memo1.lines.add('Run-time error:'+ PSScript1.ExecErrorToString) else
           image1.Show;
    end;
    
    procedure MWritedt(d : TDateTime);
    var
     s: String;
    begin
      s:= DateToStr(d) + ' ' + TimeToStr(d);
      psForm1.memo1.lines.add(s);
    end;
    
    procedure MWrites(const s: string);
    begin
      psForm1.memo1.lines.add(s);
    end;
    
    procedure MWritei(const i: Integer);
    begin
      psForm1.memo1.lines.add(inttostr(i));
    end;
    
    procedure MVal(const s: string; var n, z: Integer);
    begin
      Val(s, n, z);
    end;
    
    
    procedure TpsForm1.FormActivate(Sender: TObject);
    begin
      synmemo1.Text:= '';
      synmemo1.Lines.LoadFromFile(SCRIPTFILE);
      self.caption:= SCRIPTFILE +' loaded '+caption;
      btnsaveComp.enabled:= false;
      btnExecute.enabled:= false;
      image1.hide;
    end;
    
    procedure TpsForm1.PSScript1AfterExecute(Sender: TPSScript);
    begin
      //
    end;
    
    procedure TpsForm1.PSScript1Compile(Sender: TPSScript);
    begin
      //your own executables
      Sender.AddFunction(@MWrites, 'procedure Writes(const s: string)');
      Sender.AddFunction(@MWritedt,'procedure WriteDT(d : TDateTime)');
      Sender.AddFunction(@MWritei, 'procedure Writei(const i: Integer)');
      Sender.AddFunction(@MWrites, 'procedure Writeln(const s: string)');  //alias
      Sender.AddFunction(@MVal, 'procedure Val(const s: string; var n, z: Integer)');
    end;
    
    procedure TpsForm1.PSScript1CompImport(Sender: TObject; x: TPSPascalCompiler);
    begin
      {uPSC_std.SIRegister_Std(X);
      uPSC_classes.SIRegister_Classes(X,true);
      SIRegister_Forms(x);
      SIRegister_Controls(x);}
    end;
    
    procedure TpsForm1.PSScript1ExecImport(Sender: TObject; se: TPSExec;
      x: TPSRuntimeClassImporter);
    begin
      //add lib at run- or designtime
      {  RIRegister_Std(x);
      RIRegister_Classes(x,true);
      RIRegister_Forms(x);
      RIRegister_Controls(x);
      RegisterDateTimeLibrary_R(se);
      RegisterDLLRuntime(se); }
      {Se.RegisterDelphiFunction(@MWrites, 'procedure Writes(const s: string)', cdRegister);
      Se.RegisterDelphiFunction(@MWritedt,'procedure WriteDT(d : TDateTime)', cdRegister);
      Se.RegisterDelphiFunction(@MWritei, 'procedure Writei(const i: Integer)', cdRegister);
      Se.RegisterDelphiFunction(@MWrites, 'procedure Writeln(const s: string)', cdRegister);  //alias
      Se.RegisterDelphiFunction(@MVal, 'procedure Val(const s: string; var n, z: Integer)', cdRegister);
      }
      // showmessage('import PORT ')
      //x.RegisterMethod(@MWrites, 'procedure Writes(const s: string)');
    end;
    
    procedure TpsForm1.SynMemo1Change(Sender: TObject);
    begin
      //showmessage('to debug gutter');
    end;
    
    procedure TpsForm1.Compile1Click(Sender: TObject);
    var
    //S: TStringList;
    i: Integer;
    result: boolean;
      //showmessage('compile file');
    begin
      Result:= False;
      //if FileExists(FileName) then begin
        //S:=TStringList.Create;
        //S.LoadFromFile(FileName);
        PSScript1.Script.Text:= Synmemo1.Text;
        result:= Psscript1.Compile;
        for i:= 0 to Psscript1.CompilerMessageCount - 1 do
          memo1.lines.add(Psscript1.CompilerMessages[i].MessageToString);
        //S.Free;
        if not Result then
          if Psscript1.CompilerMessageCount > 0 then
            for i:= 0 to Psscript1.CompilerMessageCount-1 do
              memo1.lines.add(Psscript1.CompilerErrorToStr(i));
        //else memo1.lines.add('Script File not found: ', FileName); }
        if Result then begin
          btnExecute.Enabled:= true;
          btnsaveComp.enabled:= true;
        end;
    end;
    
    procedure TpsForm1.btnLoadScriptClick(Sender: TObject);
    begin
     synMemo1.lines.loadFromFile(SCRIPTFILE)
    end;
    
    procedure TpsForm1.btnImportClick(Sender: TObject);
    begin
      //psForm1.Close;
     {if synmemo1.Focused then} synMemo1.PasteFromClipboard;
    end;
    
    
    function TpsForm1.RunCompiledScript2(Bytecode: AnsiString; out RuntimeErrors: AnsiString): Boolean;
    var Runtime: TPSExec;  //to debug
    begin
      Runtime:= TPSExec.Create;
      try
        //IFPS3ClassesPlugin1ExecImport(Self, runtime, classImporter);
        //PSScript1.RuntimeImporter.CreateAndRegister(runtime, false);
         result:= PSScript1.Exec.LoadData(bytecode)
                 and PSScript1.Exec.RunScript and (PSScript1.Exec.ExceptionCode = erNoError);
         if not result then
           RunTimeErrors:= PSErrorToString(PSScript1.Exec.ExceptionCode,'');
    
        //PSScript1.SetCompiled(Bytecode);
        //IFPS3DataToText(Bytecode,Bytecode);
        //memo1.lines.add(bytecode);
      finally
        Runtime.Free;
      end;
    end;
    
    
    function LoadFile(const FileName: TFileName): string;
       begin
         with TFileStream.Create(FileName, fmOpenRead or fmShareDenyWrite) do begin
           try
             SetLength(Result, Size);
             Read(Pointer(Result)^, Size);
           except
             Result := '';  // Deallocates memory
             Free;
             raise;
           end;
           Free;
         end;
       end;
    
    
    procedure TpsForm1.btnRunbytecodeClick(Sender: TObject);
    var sdata, filename, bcerrorcode: string;
      fhandle: THandle;
    begin
       //sdata:= synmemo1.Text;
       //Compile1Click(self);
       //PSScript1.GetCompiled(sData);
        filename:= ExtractFilePath(ParamStr(0)) + ChangeFileExt(SCRIPTFILE,'.out');
        //fhandle:= fileopen(filename, 2);
        //fileread(fhandle, sdata, 100);
       sdata:= loadFile(filename);
       if RunCompiledScript2(sdata, bcerrorcode) then begin
           sysutils.beep;
           showmessage('Byte Code run success')
       end else
          Memo1.lines.add('ByteCode Error Message: '+bcerrorcode);
        // fileclose(fhandle)
       //PSScript1.SetCompiled(sData);
       //synmemo1.Text:= sData;
       //btnExecuteClick(self)
    end;
    
    procedure TpsForm1.btnSaveScriptClick(Sender: TObject);
    begin
      synMemo1.lines.saveToFile(SCRIPTFILE)
    end;
    
    procedure TpsForm1.btnSaveCompClick(Sender: TObject);
    var
      OutFile, sdata: string;
      Fx: Longint ;
    begin
      PSScript1.GetCompiled(sData);
      OutFile:= ExtractFilePath(ParamStr(0)) + ChangeFileExt(SCRIPTFILE,'.out');
      Fx:= FileCreate(OutFile) ;
      FileWrite(Fx,sData[1],Length(sData));
      FileClose(Fx) ;
    end;
    
    procedure TpsForm1.btngetCompiledClick(Sender: TObject);
    var sdata: string;
    begin
       PSScript1.GetCompiled(sData);
       // {if not} PSScript1.SetCompiled(sData);
       if not IFPS3DataToText(sData,sData)
         then memo1.lines.add('Â¡No puedo crear el desensamblado!')
       else
         synmemo1.Text:= sData;
      //aPSCE.SaveDissasembly(sData);
    end;
    
    end.
    

[![maXbox mini LAZARUS.png](https://wiki.freepascal.org/images/e/e1/maXbox_mini_LAZARUS.png)](</File:maXbox_mini_LAZARUS.png>) [![maXbox mini LAZARUS2.png](https://wiki.freepascal.org/images/2/22/maXbox_mini_LAZARUS2.png)](</File:maXbox_mini_LAZARUS2.png>)

---

_Source: [https://wiki.freepascal.org/Pascal_Script_Examples](https://web.archive.org/web/20240907140300/https://wiki.freepascal.org/Pascal_Script_Examples)_
