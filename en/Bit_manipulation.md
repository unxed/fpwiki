# Bit manipulation

│ **English (en)** │

## Contents

  * 1 Masked operations
  * 2 Bitpacked record
  * 3 Set
  * 4 TBits
  * 5 Record property and value index



## Masked operations

This is a basic low level approach to handle bit manipulation. Main advantage is that operations can be performed with groups of bits at once. But the user has to deal with all operations by himself. Another problem is max range of used values in function parameters. There must be separate implemented functions for each ordinal type for best performance (as opposed to C templates using preprocessor). 
    
    
    procedure ClearBit(var Value: QWord; Index: Byte);
    begin
      Value := Value and ((QWord(1) shl Index) xor High(QWord));
    end;
    
    procedure SetBit(var Value: QWord; Index: Byte);
    begin
      Value:=  Value or (QWord(1) shl Index);
    end;
    
    procedure PutBit(var Value: QWord; Index: Byte; State: Boolean);
    begin
      Value := (Value and ((QWord(1) shl Index) xor High(QWord))) or (QWord(State) shl Index);
    end;
    
    function GetBit(Value: QWord; Index: Byte): Boolean;
    begin
      Result := ((Value shr Index) and 1) = 1;
    end;
    

## Bitpacked record

FPC has useful extension which allow not only byte packing but also bit packing of records. This allow not only to define bit structures using Boolean type or subrange type 0..1 but also n-state or n-bits fields in record, e.g. subrange type 0..3 for 2 bits. This conjunction with record case construction combined structure can be defined which allow to access memory as byte or as individual bits. 
    
    
    TByteBits = bitpacked record
      Bit0, Bit1, Bit2, Bit3, Bit4, Bit5, Bit6, Bit7: Boolean;
    end;
    
    TByteEx = packed record
      case Integer of
        0: (ByteAccess: Byte);
        1: (BitAccess: TByteBits);
    end;
    
    TSomeBitLevelStructure = bitpacked record
      OneBit: 0..1;
      TwoBits: 0..3;
      FourBits: 0..15;
      EightBits: 0..255
    end;
    

Bitpacking can be controlled using compiler directive [$BITPACKING](<http://www.freepascal.org/docs-html/prog/progsu6.html>)

## Set

Because sets are basically an array of all states which is of boolean type, then set can be also used as bit array. But it requires use of [$PACKSET](<http://www.freepascal.org/docs-html/prog/progsu61.html>) and [$PACKENUM](<http://www.freepascal.org/docs-html/prog/progsu59.html>) compiler directives to change set size. It has also some limitations on max size of set. 
    
    
    {$packset 1}
    {$packenum 1}
    
    type
      TByteBits = set of (Bit0, Bit1, Bit2, Bit3, Bit4, Bit5, Bit6, Bit7);
    

## TBits

This class is part of FPC RTL library contained in Classes unit and has a similar use as in Delphi. It provides only some basic methods for bit manipulation. 
    
    
       TBits = class(TObject)
       public
          constructor Create(TheSize : longint = 0); virtual;
          destructor Destroy; override;
          function  GetFSize : longint;
          procedure SetOn(Bit : longint);
          procedure Clear(Bit : longint);
          procedure Clearall;
          procedure AndBits(BitSet : TBits);
          procedure OrBits(BitSet : TBits);
          procedure XorBits(BitSet : TBits);
          procedure NotBits(BitSet : TBits);
          function  Get(Bit : longint) : boolean;
          procedure Grow(NBit : longint);
          function  Equals(Obj : TObject): Boolean; override; overload;
          function  Equals(BitSet : TBits) : Boolean; overload;
          procedure SetIndex(Index : longint);
          function  FindFirstBit(State : boolean) : longint;
          function  FindNextBit : longint;
          function  FindPrevBit : longint;
    
          { functions and properties to match TBits class }
          function OpenBit: longint;
          property Bits[Bit: longint]: Boolean read get write SetBit; default;
          property Size: longint read FBSize write setSize;
       end;
    

## Record property and value index

Another interesting implementation of bit aligned structure can be used with capabilities of advanced records (FPC 2.6.0+). For all properties you have to use setter and getter which can handle general bit manipulations and set index which is passed to these methods. For more information refer to [Indexed properties](<http://www.freepascal.org/docs-html/ref/refsu31.html>). Because index is only one, it has to be divided to two parameters to describe location and size on bit value. In case of the following example, the offset can be 0..255 and size of value 0..255 bits. Another problem is that you have to ensure that defined structure components will not overlay. 
    
    
    {$mode delphi}
    
      TSomeBitStructure = record
      private
        RawData: Word;
        function GetBits(const AIndex: Integer): Integer; inline;
        procedure SetBits(const AIndex: Integer; const AValue: Integer); inline;
      public
        // High byte of index offset, low byte of index is bit count
        property OneBit: Integer index $0001 read GetBits write SetBits;
        property TwoBits: Integer index $0102 read GetBits write SetBits;
        property FourBits: Integer index $0304 read GetBits write SetBits;
        property EightBits: Integer index $0708 read GetBits write SetBits;
      end;
    
    {$OPTIMIZATION ON}
    {$OVERFLOWCHECKS OFF}
    function TSomeBitStructure.GetBits(const AIndex: Integer): Integer;
    var
      Offset: Integer;
      BitCount: Integer;
      Mask: Integer;
    begin
      BitCount := AIndex and $FF;
      Offset := AIndex shr 8;
      Mask := ((1 shl BitCount) - 1);
      Result := (RawData shr Offset) and Mask;
    end;
    
    procedure TSomeBitStructure.SetBits(const AIndex: Integer; const AValue: Integer);
    var
      Offset: Integer;
      BitCount: Integer;
      Mask: Integer;
    begin
      BitCount := AIndex and $FF;
      Offset := AIndex shr 8;
      Mask := ((1 shl BitCount) - 1);
      Assert(aValue <= Mask);
      RawData := (RawData and (not (Mask shl Offset))) or (AValue shl Offset);
    end;

---

_Source: [https://wiki.freepascal.org/Bit_manipulation](https://web.archive.org/web/20241213030907/https://wiki.freepascal.org/Bit_manipulation)_
