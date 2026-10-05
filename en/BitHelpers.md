# BitHelpers

## Contents

  * 1 About
  * 2 Installation
  * 3 Usage
    * 3.1 TBooleanBitHelper
    * 3.2 TByteBitHelper
    * 3.3 TWordBitHelper
    * 3.4 TLongwordBitHelper
    * 3.5 TQuadwordBitHelper
    * 3.6 TQuadwordOverlay, TLongwordOverlay, TWordOverlay and TByteOverlay
    * 3.7 HighestBitPos(), LowestBitPos() and BitsCount()
    * 3.8 ToBinString custom formatting
  * 4 License



# About

BitHelpers enable additional bit manipulation for qword, longword, word, byte and boolean types which will make your life much easier if you need such a feature. 

Homepage: <https://bitbucket.org/avra/bithelpers>

FreePascal type helpers TBooleanHelper, TByteHelper, TWordHelper, TCardinalHelper and TQWordHelper do not offer much when bit manipulation and presentation are needed. That's where BitHelpers package jumps in, nicely extending mentioned type helpers. 

IMPORTANT NEWS: I am so glad to announce that BitHelpers are now part of trunk FreePascal RTL. Type helpers for direct access to bit, nibble, byte, word and dword parts in ordinal types are now part of sysutils unit, and customizable boolean, binary and hexadecimal data localized string representation is now part of syshelpers unit. Syntax has been slightly changed to be more in line with FreePascal (using plurals like SomeOrdType.Bits[3] instead of old SomeOrdType.Bit[3] and similar). More details at <https://gitlab.com/freepascal.org/fpc/source/-/issues/39268> and <https://gitlab.com/freepascal.org/fpc/documentation/-/issues/39267>. From now on, all further development will be in FreePascal trunk. Bitbucket repo will continue to exist only for historical reasons and old code. 

# Installation

While you can simply copy bithelpers unit to your project directory and start using it, the recommended way would be to open "bithelpers_pkg.lpk" package and compile it. That would add BitHelpers source directory to Lazarus and make it available to all your projects. 

# Usage

## TBooleanBitHelper

Example code and it's output: 
    
    
      uses
        bithelpers;
      ...
      procedure TForm1.BooleanBitTestBtnClick(Sender: TObject);
      var
        MyBool: boolean;
      begin
        MyBool := true;
        Memo1.Append(MyBool.ToOneZeroString);
        Memo1.Append(MyBool.ToOnOffString); // default is scfUnchangedCase and can be ommited
        Memo1.Append(MyBool.ToOnOffString(scfLowerCase));
        Memo1.Append(MyBool.ToTrueFalseString(scfUpperCase));
        Memo1.Append(MyBool.ToString('OnState', 'OffState')); // true/false custom strings
        Memo1.Append(MyBool.ToString('Укључено', 'Искључено', scfUpperCase)); // when case and unicode matter
      end;
    
    
    
      1
      On
      on
      TRUE
      OnState
      УКЉУЧЕНО
    

## TByteBitHelper

Example code and it's output: 
    
    
      procedure TForm1.ByteBitTestBtnClick(Sender: TObject);
      var
        MyByte: byte;
      begin
        MyByte.Clear;                                  // %00000000 MyByte equals 0
        MyByte.Bit[0] := true;                         // %00000001 MyByte equals 1
        MyByte.Bit[2] := true;                         // %00000101 MyByte equals 5
        Memo1.Append(MyByte.ToString);
        Memo1.Append('$' + MyByte.ToHexString);
        Memo1.Append(MyByte.ToBinString(lzHideLeadingZeros));   // hide leading zeros
        Memo1.Append(MyByte.ToBinString);                       // show leading zeros   
      end;
    
    
    
      5
      $05
      101
      00000101
    

## TWordBitHelper

Example code and it's output: 
    
    
      procedure TForm1.WordBitTestBtnClick(Sender: TObject);
      var
        MyWord: word;
      begin
        MyWord.Clear;                  // %0000000000000000 MyWord equals 0
        MyWord.Byte[0] := 2;           // %0000000000000010 MyWord equals 2
        MyWord.Byte[1] := 1;           // %0000000100000010 MyWord equals 258 (2 + 256)
        MyWord.Byte[1].Bit[7] := true; // %0000000100000010 MyWord equals 258 (Beware!!! This DOES NOT set a bit in MyWord !!!)
        MyWord.Bit[10] := true;        // %0000010100000010 MyWord equals 1282 (258 + 2^10)
        Memo1.Append(MyWord.ToString);
        Memo1.Append('$' + MyWord.ToHexString);
        Memo1.Append(MyWord.ToBinString(lzHideLeadingZeros)); // hide leading zeros
        Memo1.Append(MyWord.ToBinString);                     // show leading zeros
      end;
    
    
    
      1282
      $0502
      10100000010
      0000010100000010
    

## TLongwordBitHelper

Example code and it's output: 
    
    
      procedure TForm1.LongwordBitTestBtnClick(Sender: TObject);
      var
        MyLongword: longword;
      begin
        MyLongword.Clear;                  // %00000000000000000000000000000000 MyLongword equals 0
        MyLongword.Word[0] := 250;         // %00000000000000000000000011111010 MyLongword equals 250
        MyLongword.Word[1].Byte[0] := 100; // %00000000000000000000000011111010 MyLongword equals 250 (Beware!!! This DOES NOT set a byte in MyLongword !!!)
        MyLongword.Byte[1] := 4;           // %00000000000000000000010011111010 MyLongword equals 1274 (250 + 2^(8 + 2), 2^2 = 4)
        MyLongword.Bit[26] := true;        // %00000100000000000000010011111010 MyLongword equals 67110138 (1274 + 2^26)
        Memo1.Append(MyLongword.ToString);
        Memo1.Append('$' + MyLongword.ToHexString);
        Memo1.Append(MyLongword.ToBinString(lzHideLeadingZeros)); // hide leading zeros
        Memo1.Append(MyLongword.ToBinString);                     // show leading zeros
        Memo1.Append('');
      end;
    
    
    
      67110138
      $040004FA
      100000000000000010011111010
      00000100000000000000010011111010
    

## TQuadwordBitHelper

Example code and it's output: 
    
    
      procedure TForm1.QuadwordBitTestBtnClick(Sender: TObject);
      var
        MyQuadword: qword;
      begin
        MyQuadword.Clear;                      // %0000000000000000000000000000000000000000000000000000000000000000 MyQuadword equals 0
        MyQuadword.Longword[0] := 12345;       // %0000000000000000000000000000000000000000000000000011000000111001 MyQuadword equals 12345
        MyQuadword.Longword[1].Word[0] := 100; // %0000000000000000000000000000000000000000000000000011000000111001 MyQuadword equals 12345 (Beware!!! This DOES NOT set a word in MyQuadword !!!)
        MyQuadword.Byte[3] := 2;               // %0000000000000000000000000000000000000010000000000011000000111001 MyQuadword equals 33566777 (12345 + 2^(8 + 8 + 8 + 2), 2^1 = 2)
        MyQuadword.Bit[50] := true;            // %0000000000000100000000000000000000000010000000000011000000111001  MyQuadword equals 1125899940409401 (33566777 + 2^50)
        Memo1.Append(MyQuadword.ToString);
        Memo1.Append('$' + MyQuadword.ToHexString);
        Memo1.Append(MyQuadword.ToBinString(lzHideLeadingZeros)); // hide leading zeros
        Memo1.Append(MyQuadword.ToBinString);                     // show leading zeros
      end;
    
    
    
      1125899940409401
      $0004000002003039
      100000000000000000000000010000000000011000000111001
      0000000000000100000000000000000000000010000000000011000000111001
    

## TQuadwordOverlay, TLongwordOverlay, TWordOverlay and TByteOverlay

Variant records are also provided for qword, longword, word and byte. Sometimes they are more convenient to use then type helpers, and nothing stops you to mix them when needed. Here is an example code and it's output: 
    
    
      procedure TForm1.OverlaysTestBtnClick(Sender: TObject);
      var
        MyQuadOverlay: TQuadwordOverlay;
      begin
        MyQuadOverlay.AsQuadword.Clear;
        MyQuadOverlay.AsByte[0] := 100;
        Memo1.Append(MyQuadOverlay.AsQuadword.ToBinString);
        MyQuadOverlay.AsLongword[1] := 1;
        Memo1.Append(MyQuadOverlay.AsQuadword.ToBinString);
        MyQuadOverlay.AsQuadword.Bit[32] := false;
        Memo1.Append(MyQuadOverlay.AsQuadword.ToBinString);
        MyQuadOverlay.AsWordOverlay[3].AsByte[1] := $FF; // recursive overlays are allowed
        Memo1.Append(MyQuadOverlay.AsQuadword.ToBinString);
        MyQuadOverlay.AsWord[3].Byte[1].Bit[5] := false; // NO CHANGE !!! Bit is set in a result byte, not in a byte that belongs to MyQuadOverlay
        Memo1.Append(MyQuadOverlay.AsQuadword.ToBinString);
        MyQuadOverlay.AsBit[63] := false;
        Memo1.Append(MyQuadOverlay.AsQuadword.ToBinString);
      end;
    
    
    
      0000000000000000000000000000000000000000000000000000000001100100
      0000000000000000000000000000000100000000000000000000000001100100
      0000000000000000000000000000000000000000000000000000000001100100
      1111111100000000000000000000000000000000000000000000000001100100
      1111111100000000000000000000000000000000000000000000000001100100
      0111111100000000000000000000000000000000000000000000000001100100
    

## HighestBitPos(), LowestBitPos() and BitsCount()

HighestBitPos(), LowestBitPos() and BitsCount() are used for getting position of left most bit, right most bit, and number of set bits. Beware that for a value of 0, result of HighestBitPos() and LowestBitPos() will be 255 (unsigned equivalent of -1). Here is an example code and it's output: 
    
    
      procedure TForm1.BitsPosAndCountClick(Sender: TObject);
      var
        MyQuadWord: qword;
      begin
        MyQuadWord := $7F00000000000064; // 0111111100000000000000000000000000000000000000000000000001100100
        Memo1.Append(MyQuadword.ToBinString);
        Memo1.Append('MyQuadword.HighestBitPos = ' + MyQuadword.HighestBitPos.ToString);
        Memo1.Append('MyQuadword.LowestBitPos = ' + MyQuadword.LowestBitPos.ToString);
        Memo1.Append('MyQuadword.BitsCount = ' + MyQuadword.BitsCount.ToString);
        Memo1.Append('');
      end;
    
    
    
     0111111100000000000000000000000000000000000000000000000001100100
     MyQuadword.HighestBitPos = 62
     MyQuadword.LowestBitPos = 2
     MyQuadword.BitsCount = 10
    

## ToBinString custom formatting

ToBinString() can follow custom formatting when needed. Binary string output can be changed permanently by modifying fields of DefaultHelperFormatSettings global record, or temporary by creating a record of THelperFormatSettings type, populating it's fields and making it as a parameter of ToBinString(). It will be more clear in following examples - first how to use temporary format: 
    
    
      procedure TForm1.TmpFormatBtnClick(Sender: TObject);
      var
        MyQuadWord: qword;
        MyTmpFormat: THelperFormatSettings;
      begin
        MyQuadWord := 1125899940409401;
     
        MyTmpFormat.PrefixString      := '< ';
        MyTmpFormat.SufixString       := ' >';
        MyTmpFormat.ByteSeparator     := ' - ';
        MyTmpFormat.WordSeparator     := ' = ';
        MyTmpFormat.LongwordSeparator := '   ';
        MyTmpFormat.NibbleSeparator   := '.';
     
        Memo1.Append('MyQuadword.ToBinString(MyTmpFormat, true);');
        Memo1.Append(MyQuadword.ToBinString(MyTmpFormat, true)); // true is default so it can be ommited and leading zeros will still be shown
        Memo1.Append('MyQuadword.Longword[1].ToBinString(MyTmpFormat);');
        Memo1.Append(MyQuadword.Longword[1].ToBinString(MyTmpFormat)); // default true is ommited, show higher longword with leading zeros
        Memo1.Append('MyQuadword.Word[3].ToBinString(MyTmpFormat, false);');
        Memo1.Append(MyQuadword.Word[3].ToBinString(MyTmpFormat, false)); // show highest word without leading zeros
      end;
    
    
    
      MyQuadword.ToBinString(MyTmpFormat, true);
      < 0000.0000 - 0000.0100 = 0000.0000 - 0000.0000   0000.0010 - 0000.0000 = 0011.0000 - 0011.1001 >
      MyQuadword.Longword[1].ToBinString(MyTmpFormat);
      < 0000.0000 - 0000.0100 = 0000.0000 - 0000.0000 >
      MyQuadword.Word[3].ToBinString(MyTmpFormat, false);
      < 100 >
    

and then permanent format example: 
    
    
      procedure TForm1.ChangeFormatBtnClick(Sender: TObject);
      var
        MyQuadWord: qword;
      begin
        MyQuadWord := 1125899940409401;
    
        // all DefaultHelperFormatSettings fields are empty strings by default, so we change them here:
        DefaultHelperFormatSettings.PrefixString      := '{';
        DefaultHelperFormatSettings.SufixString       := '}';
        DefaultHelperFormatSettings.NibbleSeparator   := '_';
        DefaultHelperFormatSettings.ByteSeparator     := '  ';
        DefaultHelperFormatSettings.WordSeparator     := '  ' + Chr(39) + '  '; // single quote
        DefaultHelperFormatSettings.LongwordSeparator := '  "  ';
    
        Memo1.Append(MyQuadword.ToBinString(true));
        Memo1.Append(MyQuadword.Longword[1].ToBinString);
        Memo1.Append(MyQuadword.Word[3].ToBinString(false)); 
      end;
    
    
    
      {0000_0000  0000_0100  '  0000_0000  0000_0000  "  0000_0010  0000_0000  '  0011_0000  0011_1001}
      {0000_0000  0000_0100  '  0000_0000  0000_0000}
      {100}
    

# License

BitHelpers package is released under triple license: 

  * [[LGPLv3](<https://www.gnu.org/licenses/lgpl-3.0.en.html>)] - chosen for compatibility with Pasettimino.
  * [[FPC modified LGPL](<FPC_modified_LGPL.md>)] - chosen for compatibility with FreePascal and Lazarus.
  * [[BSD3](<https://opensource.org/licenses/BSD-3-Clause>)] - chosen for compatibility with everything else.



│ **English (en)** │

---

_Source: [https://wiki.freepascal.org/BitHelpers](https://web.archive.org/web/20250401132655/https://wiki.freepascal.org/BitHelpers)_
