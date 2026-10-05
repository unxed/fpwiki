# fcl-passrc

This package contains some useful units for dealing with Pascal source files: Parsing, creating structures in memory, and output to files. 

Used by: [fpdoc](<fpdoc.md> "fpdoc")

## Units

Unit | Comment   
---|---  
[pastree](</index.php?title=pastree&action=edit&redlink=1> "pastree \(page does not exist\)") | Pascal parse tree classes, for storing a complete Pascal module in memory.   
[paswrite](</index.php?title=paswrite&action=edit&redlink=1> "paswrite \(page does not exist\)") | A class and helper functions for creating Pascal source files from parse trees   
[pparser](</index.php?title=pparser&action=edit&redlink=1> "pparser \(page does not exist\)") | Parser for Pascal source files. Reads files via the pscanner unit and stores all parsed data in a parse tree, as implemented in the pastree unit.   
[pscanner](</index.php?title=pscanner&action=edit&redlink=1> "pscanner \(page does not exist\)") | Lexical scanner class for Pascal sources   
  
## An example
    
    
    {$mode objfpc}{$H+}
    
    uses SysUtils, Classes, PParser, PasTree;
    
    type
      { We have to override abstract TPasTreeContainer methods.
        See utils/fpdoc/dglobals.pp for an implementation of TFPDocEngine,
        a "real" engine. }
      TSimpleEngine = class(TPasTreeContainer)
      public
        function CreateElement(AClass: TPTreeElement; const AName: String;
          AParent: TPasElement; AVisibility: TPasMemberVisibility;
          const ASourceFilename: String; ASourceLinenumber: Integer): TPasElement;
          override;
        function FindElement(const AName: String): TPasElement; override;
      end;
    
    function TSimpleEngine.CreateElement(AClass: TPTreeElement; const AName: String;
      AParent: TPasElement; AVisibility: TPasMemberVisibility;
      const ASourceFilename: String; ASourceLinenumber: Integer): TPasElement;
    begin
      Result := AClass.Create(AName, AParent);
      Result.Visibility := AVisibility;
      Result.SourceFilename := ASourceFilename;
      Result.SourceLinenumber := ASourceLinenumber;
    end;
    
    function TSimpleEngine.FindElement(const AName: String): TPasElement;
    begin
      { dummy implementation, see TFPDocEngine.FindElement for a real example }
      Result := nil;
    end;
    
    var
      M: TPasModule;
      E: TPasTreeContainer;
      I: Integer;
      Decls: {$ifdef VER2_6} TList {$else} { for FPC > 2.6.0 } TFPList {$endif};
    begin
      E := TSimpleEngine.Create;
      try
        M := ParseSource(E, ParamStr(1), 'linux', 'i386');
    
        { Cool, we successfully parsed the module.
          Now output some info about it. }
        if M.InterfaceSection <> nil then
        begin
          Decls := M.InterfaceSection.Declarations;
          for I := 0 to Decls.Count - 1 do
            Writeln('Interface item ', I, ': ' +
              (TObject(Decls[I]) as TPasElement).Name);
        end else
          Writeln('No interface section --- this is not a unit, this is a ', M.ClassName);
    
        if M.ImplementationSection <> nil then // may be nil in case of a simple program
        begin
          Decls := M.ImplementationSection.Declarations;
          for I := 0 to Decls.Count - 1 do
            Writeln('Implementation item ', I, ': ' +
              (TObject(Decls[I]) as TPasElement).Name);
        end;
    
        FreeAndNil(M);
      finally FreeAndNil(E) end;
    end.
    

Go to back [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/fcl-passrc](https://web.archive.org/web/20250325000925/https://wiki.freepascal.org/fcl-passrc)_
