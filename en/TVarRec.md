# TVarRec

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

  
A **TVarRec** is a [variant record](</index.php?title=variant_record&action=edit&redlink=1> "variant record \(page does not exist\)") used with the [array of const](<Const.md> "Const") parameter declaration. The VType member is checked by the user to determine which data type is stored in the record. 
    
    
         TVarRec = record
             case VType : sizeint of
    {$ifdef ENDIAN_BIG}
               vtInteger       : ({$IFDEF CPU64}integerdummy1 : Longint;{$ENDIF CPU64}VInteger: Longint);
               vtBoolean       : ({$IFDEF CPU64}booldummy : Longint;{$ENDIF CPU64}booldummy1,booldummy2,booldummy3: byte; VBoolean: Boolean);
               vtChar          : ({$IFDEF CPU64}chardummy : Longint;{$ENDIF CPU64}chardummy1,chardummy2,chardummy3: byte; VChar: Char);
               vtWideChar      : ({$IFDEF CPU64}widechardummy : Longint;{$ENDIF CPU64}wchardummy1,VWideChar: WideChar);
    {$else ENDIAN_BIG}
               vtInteger       : (VInteger: Longint);
               vtBoolean       : (VBoolean: Boolean);
               vtChar          : (VChar: Char);
               vtWideChar      : (VWideChar: WideChar);
    {$endif ENDIAN_BIG}
    {$ifndef FPUNONE}
               vtExtended      : (VExtended: PExtended);
    {$endif}
               vtString        : (VString: PShortString);
               vtPointer       : (VPointer: Pointer);
               vtPChar         : (VPChar: PAnsiChar);
               vtObject        : (VObject: TObject);
               vtClass         : (VClass: TClass);
               vtPWideChar     : (VPWideChar: PWideChar);
               vtAnsiString    : (VAnsiString: Pointer);
               vtCurrency      : (VCurrency: PCurrency);
               vtVariant       : (VVariant: PVariant);
               vtInterface     : (VInterface: Pointer);
               vtWideString    : (VWideString: Pointer);
               vtInt64         : (VInt64: PInt64);
               vtUnicodeString : (VUnicodeString: Pointer);
               vtQWord         : (VQWord: PQWord);
           end;

---

_Source: [https://wiki.freepascal.org/TVarRec](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/TVarRec)_
