# XML Decoders

│ **English (en)** │  **[русский (ru)](<../ru/XML_Decoders.md>)** │

## Contents

  * 1 XML Decoders
    * 1.1 Available decoders
    * 1.2 Decoder structure
      * 1.2.1 GetDecoder
      * 1.2.2 Cleanup
      * 1.2.3 Decode
    * 1.3 Sample decoder
  * 2 See also



## XML Decoders

Starting from SVN revision 12582, XML reader is able to process data in any encoding by using external decoders. Following is the brief description of how it works. 

### Available decoders

Currently, the decoder using libiconv is available. It has two distinct implementations. The first one, in **xmliconv.pas** unit, uses the existing iconvenc package and supports Linux, FreeBSD and Darwin targets. The second one, in **xmliconv_windows.pas** unit, is for Windows targets. It links to the native-build iconv.dll that you should distribute with the application. 

### Decoder structure

Interfacing with the external decoders is done in a plain procedural style. Writing the decoder is essentially implementing the following three procedures: 

  1. `GetDecoder`
  2. `Decode`
  3. `Cleanup` (optional)



Here is the brief desription of external decoder operation: 

#### GetDecoder
    
    
    function GetDecoder(const AEncoding: string; out Decoder: TDecoder): Boolean; stdcall;
    

At the program initialization time, decoder registers itself by calling `XMLRead.RegisterDecoder` procedure, supplying its `GetDecoder` function as the argument. Whenever the reader encounters the encoding label which it does not handle internally, it calls all registered `GetDecoder` functions in the same order they were registered, until one of them returns _True_. The `GetDecoder` function arguments are the name of encoding and the `TDecoder` record that the function should fill. The encoding name is restricted to characters in range ['A'..'Z', 'a'..'z', '0'..'9', '.', '-', '_'], and must be compared case-insensitive. If the decoder supports given encoding, the function should set at least the `Decode` member of the supplied record and return _True_. Setting other members of `Decoder` is optional. 

#### Cleanup
    
    
    procedure Cleanup(Context: Pointer); stdcall;
    

If `GetDecoder` sets the `Decoder.Cleanup` member, it is called by reader once, after processing of the current entity is finished. As the name suggests, the decoder should then free all resources it allocated. 

The value of `Decoder.Context` is passed to `Decode` and `Cleanup` procedures each time they are called. The reader does not assign any meaning to this value. 

#### Decode
    
    
    function Decode(Context: Pointer; InBuf: PChar; var InCnt: Cardinal;
      OutBuf: PWideChar; var OutCnt: Cardinal): Integer; stdcall;
    

The `Decode` function does the main job. It should convert the input data pointed by `InBuf` into UTF-16 in the current platform endianness and place it into `OutBuf`. The size of input buffer is supplied in `InCnt`, the space avaliable in output buffer is in `OutCnt`. 

The important difference to note is that `InCnt` is given in **bytes** , while `OutCnt` is in **WideChars**. 

The function must decrement `InCnt` and `OutCnt` according to the amount of data it processes. Each processed character decrements `OutCnt` by one (or by two in case the surrogate pair is written); the amount of `InCnt` decrement depends on the actual encoding. 

No assumptions should be made about initial size of buffers: for example, the reader may call decoder with only a few bytes in input buffer. The decoder function then should return zero indicating nothing is processed, and the reader will fetch more input and call decoder again. 

The function should return positive value if it had processed something, zero if it had not (e.g. because no space available in either input or output buffer), and negative value in cause the input data contains illegal sequence. In the future, there may be attempt to categorize the decoding errors, but currently any negative return simply aborts the reader with the 'Decoding error' message. 

In case of error in input data the decoder should still decrement `OutCnt` to reflect the number of successfully processed characters. This will be used by reader to provide location information in the exception error message. 

### Sample decoder

Following is a sample unit that decodes cp866. This decoder is stateless, so it does not use the `Cleanup` and `Context` members. It should be very easy to modify this sample to handle any similar single-byte encoding by just replacing the conversion table. 
    
    
    unit xmlcp866;
    
    interface
    
    implementation
    
    uses
      SysUtils, xmlread;
    
    const
      cp866table: array[#128..#255] of WideChar=(
          #$0410, #$0411, #$0412, #$0413, #$0414, #$0415, #$0416, #$0417,
          #$0418, #$0419, #$041A, #$041B, #$041C, #$041D, #$041E, #$041F,
          #$0420, #$0421, #$0422, #$0423, #$0424, #$0425, #$0426, #$0427,
          #$0428, #$0429, #$042A, #$042B, #$042C, #$042D, #$042E, #$042F,
          #$0430, #$0431, #$0432, #$0433, #$0434, #$0435, #$0436, #$0437,
          #$0438, #$0439, #$043A, #$043B, #$043C, #$043D, #$043E, #$043F,
          #$2591, #$2592, #$2593, #$2502, #$2524, #$2561, #$2562, #$2556,
          #$2555, #$2563, #$2551, #$2557, #$255D, #$255C, #$255B, #$2510,
          #$2514, #$2534, #$252C, #$251C, #$2500, #$253C, #$255E, #$255F,
          #$255A, #$2554, #$2569, #$2566, #$2560, #$2550, #$256C, #$2567,
          #$2568, #$2564, #$2565, #$2559, #$2558, #$2552, #$2553, #$256B,
          #$256A, #$2518, #$250C, #$2588, #$2584, #$258C, #$2590, #$2580,
          #$0440, #$0441, #$0442, #$0443, #$0444, #$0445, #$0446, #$0447,
          #$0448, #$0449, #$044A, #$044B, #$044C, #$044D, #$044E, #$044F,
          #$0401, #$0451, #$0404, #$0454, #$0407, #$0457, #$040E, #$045E,
          #$00B0, #$2219, #$00B7, #$221A, #$2116, #$00A4, #$25A0, #$00A0);
    
    function cp866Decode(Context: Pointer; InBuf: PChar; var InCnt: Cardinal; OutBuf: PWideChar;
                         var OutCnt: Cardinal): Integer; stdcall;
    var
      I: Integer;
      cnt: Cardinal;
    begin
      cnt := OutCnt;         // num of widechars
      if cnt > InCnt then
        cnt := InCnt;
      for I := 0 to cnt-1 do
      begin
        if InBuf[I] < #128 then
          OutBuf[I] := WideChar(ord(InBuf[I]))
        else
          OutBuf[I] := cp866table[InBuf[I]];
      end;
      Dec(InCnt, cnt);
      Dec(OutCnt, cnt);
      Result := cnt;
    end;
    
    function GetCP866Decoder(const AEncoding: string; out Decoder: TDecoder): Boolean; stdcall;
    begin
    // Most encodings typically have one or more alias names.
      if SameText(AEncoding, 'IBM866') or
         SameText(AEncoding, 'cp866') or
         SameText(AEncoding, '866') or
         SameText(AEncoding, 'csIBM866') then
      begin
        Decoder.Decode := @cp866Decode;
        Decoder.Cleanup := nil;
        Decoder.Context := nil;
        Result := True;
      end
      else
        Result := False;
    end;
    
    initialization
      RegisterDecoder(@GetCP866Decoder);
    end.
    

## See also

  * [XML Tutorial](<XML_Tutorial.md> "XML Tutorial")

---

_Source: [https://wiki.freepascal.org/XML_Decoders](https://web.archive.org/web/20250217115124/https://wiki.freepascal.org/XML_Decoders)_
