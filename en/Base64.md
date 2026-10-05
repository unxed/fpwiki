# Base64

Various code examples to handle Base64 encoded strings. 

## Contents

  * 1 Convert Base64 string to picture, 1
  * 2 Convert Base64 string to picture, 2
  * 3 Convert Base64 string to picture, using BGRABitmap



# Convert Base64 string to picture, 1

Example from forum member **wp**. 
    
    
    uses
      Classes, Base64;
    
    procedure DecodeBase64ToStream(AStream: TStream; const s: string; strict: boolean=false);
    var
      SD: String;
      InStream: TStringStream;
      Decoder: TBase64DecodingStream;
    begin
      if Length(s)=0 then
        Exit;
      SD := S;
      while Length(Sd) mod 4 > 0 do
        SD := SD + '=';
      InStream:= TStringStream.Create(SD);
      try
        if strict then
          Decoder:=TBase64DecodingStream.Create(InStream, bdmStrict)
        else
          Decoder:=TBase64DecodingStream.Create(InStream, bdmMIME);
        try
          AStream.CopyFrom(Decoder, Decoder.Size);
          AStream.Position := 0;
        finally
          Decoder.Free;
        end;
      finally
        InStream.Free;
      end;
    end;
    
    var
      base64String: String =
        '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';
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      stream: TMemoryStream;
    begin
      stream := TMemoryStream.Create;
      try
        DecodeBase64ToStream(stream, base64String);
        Image1.Picture.LoadFromStream(stream);
      finally
        stream.Free;
      end;
    end;
    

# Convert Base64 string to picture, 2

This unit is from forum member **KodeZwerg**. He created a pretty easy to handle method that supports now all (?) formats that Lazarus Graphics offers. To name them: JPEG, PNG, GIF, BMP, ICO and TIFF. 
    
    
    unit uBase64Image;
    
    {$mode ObjFPC}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Graphics, ExtCtrls, Base64;
    
    function FileToBytes(const AFilename: string): TBytes;
    function BytesToBase64(const AData: TBytes): AnsiString;
    function Base64ToBytes(const ABase64String: AnsiString): TBytes;
    function GetGraphicClassFromBytes(const AData: TBytes): TGraphicClass;
    procedure AssignBase64ToImage(const ABase64String: AnsiString; const AImage: TImage);
    
    implementation
    
    { convert any file into bytes }
    
    function FileToBytes(const AFilename: string): TBytes;
    var
      FileStream: TFileStream;
    begin
      FileStream := TFileStream.Create(AFileName, fmOpenRead or fmShareDenyWrite);
      try
        SetLength(Result, FileStream.Size);
        FileStream.ReadBuffer(Result[0], FileStream.Size);
      finally
        FileStream.Free;
      end;
    end;
    
    { convert bytes to base64 }
    
    function BytesToBase64(const AData: TBytes): AnsiString;
    var
      encodedStream: TStringStream;
      encodingStream: TBase64EncodingStream;
    begin
      encodedStream := TStringStream.Create('');
      try
        encodingStream := TBase64EncodingStream.Create(encodedStream);
        try
          encodingStream.WriteBuffer(AData[0], Length(AData));
        finally
          encodingStream.Free;
        end;
        Result := encodedStream.DataString;
      finally
        encodedStream.Free;
      end;
    end;
    
    { convert base64 to bytes }
    
    function Base64ToBytes(const ABase64String: AnsiString): TBytes;
    var
      decodedStream: TBase64DecodingStream;
      base64stream: TStringStream;
    begin
      base64stream := TStringStream.Create(ABase64String);
      try
        decodedStream := TBase64DecodingStream.Create(base64stream);
        try
          SetLength(Result, decodedStream.Size);
          decodedStream.ReadBuffer(Result[0], decodedStream.Size);
        finally
          decodedStream.Free;
        end;
      finally
        base64stream.Free;
      end;
    end;
    
    { determine the image format }
    
    function GetGraphicClassFromBytes(const AData: TBytes): TGraphicClass;
    begin
      Result := nil;
      // JPEG
      if (((Length(AData) >= 8) and (CompareMem(@AData[0], @[$FF, $D8, $FF, $DB], 4)
       or CompareMem(@AData[0], @[$FF, $D8, $FF, $E0, $00, $10, $4A, $46], 8)
       or CompareMem(@AData[0], @[$49, $46, $00, $01], 4)
       or CompareMem(@AData[0], @[$FF, $D8, $FF, $EE], 4)
       or (CompareMem(@AData[0], @[$FF, $D8, $FF, $E1], 4) and CompareMem(@AData[6], @[$45, $78], 2))
       or CompareMem(@AData[0], @[$69, $66, $00, $00], 4)
       or CompareMem(@AData[0], @[$FF, $D8, $FF, $E0], 4)))) then
        Result := TJPEGImage
      else
      // PNG
      if ((Length(AData) >= 8) and CompareMem(@AData[0], @[$89, $50, $4E, $47, $0D, $0A, $1A, $0A], 8)) then
        Result := TPortableNetworkGraphic
      else
      // GIF
      if ((Length(AData) >= 6) and (CompareMem(@AData[0], @[$47, $49, $46, $38, $37, $61], 6)
       or CompareMem(@AData[0], @[$47, $49, $46, $38, $39, $61], 6))) then
        Result := TGIFImage
      else
      // BMP
      if ((Length(AData) >= 2) and CompareMem(@AData[0], @[$42, $4D], 2)) then
        Result := TBitmap
      else
      // ICON
      if ((Length(AData) >= 4) and CompareMem(@AData[0], @[$00, $00, $01, $00], 4)) then
        Result := TIcon
      else
      // TIFF
      if ((Length(AData) >= 4) and (CompareMem(@AData[0], @[$49, $49, $2A, $00], 4)
       or CompareMem(@AData[0], @[$4D, $4D, $00, $2A], 6))) then
        Result := TTiffImage
      else
        raise Exception.Create('Unknown Graphic Type!');
    end;
    
    { analyze and display a found image format into a TImage }
    
    procedure AssignBase64ToImage(const ABase64String: AnsiString; const AImage: TImage);
    var
      bytes: TBytes;
      graphic: TGraphic;
      stream: TStream;
    begin
      AImage.Picture.Clear;
      bytes := Base64ToBytes(ABase64String);
      graphic := GetGraphicClassFromBytes(bytes).Create;
      try
        stream := TBytesStream.Create(bytes);
        try
          graphic.LoadFromStream(stream);
        finally
          stream.Free;
        end;
        AImage.Picture.Assign(graphic);
      finally
        graphic.Free;
      end;
    end;
    
    end.
    

# Convert Base64 string to picture, using BGRABitmap

This unit is from forum member **KodeZwerg**. You must install the [BGRABitmap](<BGRABitmap.md> "BGRABitmap") component. But you can also use this unit without BGRABitmap, if you disable the define "UseBGRA" in the top of the "interface" part. 
    
    
    unit ubase64image;
     
    (* ************************* *)
    (* *    Base64 to Image    * *)
    (* ************************* *)
    { Created: 2023 by KodeZwerg  }
    { Dialect: FreePascal         }
    { Type: Helper unit           }
    { License: Unlicensed         }
     
    { This unit can convert a Base64 encoded string into a TImage     }
    { Additional you can do the opposite, create from a file a Base64 }
     
    { Thank you Remy for teaching me about TGraphicClass usage!           }
    { Thank you paweld for beta testing and fixing a transparency bug!    }
    { Thank you wp for teaching me more about TGraphicClass usage!        }
    { Thank you wp for complaining about my original names, critism help! }
     
    {$mode ObjFPC}{$H+}
     
    interface
     
    // remove the dot to utilize BGRABitmap package
    // put a dod infront of $ to deactivate BGRABitmap support
    {$DEFINE UseBGRA}
     
    uses
      Classes, SysUtils, Graphics, ExtCtrls, Dialogs, Types, LCLType, Controls,
      Base64
      {$If defined(UseBGRA)},BGRABitmap, BGRABitmapTypes, BGRAGraphics, BGRASVG, BGRAOpenRaster, BGRAPhoxo, BGRAPaintNet{$EndIf};
     
    // this method creates a stream from a file, additional it can throw a warning if a certain mb limit is exceeded
    function CreateStreamFromFile(const AFilename: string; var AStream: TStream; const AFileSizeWarningInMegabyte: Integer = 1): Boolean;
    // this method convert a stream into a base64 string
    function CreateBase64FromStream(const AStream: TStream): string;
    // this method convert a base 64 into a stream
    function CreateStreamFromBase64(const ABase64String: AnsiString): TStream;
    // depending what mode you selected you having either this or that method available to display out of a base64 a timage
    {$If defined(UseBGRA)}
    // this method convert base64 into a timage, additional you can choose between a faster bitmap conversation or a slower png compressed variant
    procedure AssignBase64ToImage(const ABase64String: AnsiString; const AImage: TImage; const AFastMode: Boolean = False);
    {$Else}
    // this method convert a base64 into a timage
    procedure AssignBase64ToImage(const ABase64String: AnsiString; const AImage: TImage);
    {$EndIf}
    implementation
     
    function CreateStreamFromFile(const AFilename: string; var AStream: TStream; const AFileSizeWarningInMegabyte: Integer = 1): Boolean;
      function SizeToString(const ASize: UInt64; const oneKB: Integer = {$IFDEF MSWINDOWS}1024{$ELSE}1000{$ENDIF}): string;
      var
        Calc: Extended;
        Sign: String;
        SizeLength: Integer;
      begin
        SizeLength := 0;
        Calc := ASize;
        while (Calc > oneKB) do
          begin
            Calc := Calc / oneKB;
            Inc(SizeLength);
          end;
        case SizeLength of
          0: Sign := ' byte';
          1: Sign := ' kb'; // Kilo Byte
          2: Sign := ' mb'; // Mega Byte
          3: Sign := ' gb'; // Giga Byte
          4: Sign := ' tb'; // Tera Byte
          5: Sign := ' pb'; // Peta Byte
          6: Sign := ' eb'; // Exa Byte
          7: Sign := ' zb'; // Zetta Byte
          8: Sign := ' yb'; // Yotta Byte
          9: Sign := ' rb'; // Ronna Byte
          10:Sign := ' qb'; // Quetta Byte
        else
          Sign :=' (' + IntToStr(SizeLength) + ')';
        end;
        Result := FormatFloat('#,##0.00', Calc) + Sign;
      end;
    const
      oneKB = Integer({$IFDEF MSWINDOWS}1024{$ELSE}1000{$ENDIF});
      oneMB = Integer(oneKB * oneKB);
    var
      FileStream: TFileStream;
    begin
      FileStream := TFileStream.Create(AFileName, fmOpenRead or fmShareDenyWrite);
      try
        Result := False;
        if (FileStream.Size > (oneMB * AFileSizeWarningInMegabyte)) then
          if QuestionDlg('Filesize Warning!',
               Format('Filesize is bigger than %dmb (%s)!', [AFileSizeWarningInMegabyte, SizeToString(FileStream.Size)])
               + LineEnding + 'Continue loading?',
               mtWarning, [mrYes, mrNo], 0) = mrNo then
                 Exit;
        AStream := TMemoryStream.Create;
        try
          AStream.CopyFrom(FileStream, FileStream.Size);
        finally
          AStream.Position := 0;
          Result := True;
        end;
      finally
        FileStream.Free;
      end;
    end;
     
    function CreateBase64FromStream(const AStream: TStream): string;
    var
      EncodedStream: TStringStream;
      Encoder: TBase64EncodingStream;
    begin
      Result := '';
      EncodedStream := TStringStream.Create('');
      Encoder := TBase64EncodingStream.Create(EncodedStream);
      try
        AStream.Position := 0;
        Encoder.CopyFrom(AStream, AStream.Size);
        Encoder.Flush;
        Result := EncodedStream.DataString;
        AStream.Position := 0;
      finally
        Encoder.Free;
        EncodedStream.Free;
      end;
    end;
     
    function CreateStreamFromBase64(const ABase64String: AnsiString): TStream;
    var
      DecodedStream: TBase64DecodingStream;
      Base64Stream: TStringStream;
    begin
      Base64Stream := TStringStream.Create(ABase64String);
      try
        DecodedStream := TBase64DecodingStream.Create(base64stream);
        try
          Result := TMemoryStream.Create;
          try
            Result.CopyFrom(DecodedStream, DecodedStream.Size);
          finally
            Result.Position := 0;
          end;
        finally
          DecodedStream.Free;
        end;
      finally
        Base64Stream.Free;
      end;
    end;
     
    {$IfNDef UseBGRA}
    function GetGraphicClassFromStream(const AStream: TStream): TGraphicClass;
      function HasIconSignature(const AStream: TStream): Boolean;
      const
        IconSignature: array[0..3] of Byte = ($00, $00, $01, $00);
      var
        SignatureBytes: array[0..3] of Byte;
      begin
        AStream.Position := 0;
        AStream.ReadBuffer(SignatureBytes, SizeOf(SignatureBytes));
        Result := CompareMem(@SignatureBytes, @IconSignature, SizeOf(SignatureBytes));
        AStream.Position := 0;
      end;
    const
      REGISTERED: array[0..5] of TGraphicClass = (TBitmap, TJpegImage,
        TPortableNetworkGraphic, TTiffImage, TGifImage, TPixMap);
    var
      i: Integer;
    begin
      Result := nil;
      AStream.Position := 0;
      for i := 0 to High(REGISTERED) do
        if REGISTERED[i].IsStreamFormatSupported(AStream) then
        begin
          Result := REGISTERED[i];
          AStream.Position := 0;
          Exit;
        end;
      if HasIconSignature(AStream) then
        Result := TIcon;
      AStream.Position := 0;
    end;
     
    procedure AssignBase64ToImage(const ABase64String: AnsiString; const AImage: TImage);
    var
      graphicClass: TGraphicClass;
      graphic: TGraphic;
      stream: TStream;
      size: TSize;
    begin
      AImage.Picture.Clear;
      stream := CreateStreamFromBase64(ABase64String);
      graphicClass := GetGraphicClassFromStream(stream);
      if Assigned(graphicClass) then
      begin
        graphic := graphicClass.Create;
        try
          graphic.LoadFromStream(stream);
          if (graphicClass.ClassType = TIcon) then
            begin
              size.Width := AImage.Width;
              size.Height := AImage.Height;
              TIcon(graphic).Current := TIcon(graphic).GetBestIndexForSize(size);
            end;
          AImage.Picture.Assign(graphic);
        finally
          graphic.Free;
        end;
      end;
      stream.Free;
    end;
    {$EndIf}
     
    {$If defined(UseBGRA)}
    function BGRABitmapToPNGStream(const ABGRABitmap: TBGRABitmap): TStream;
    begin
      Result := TMemoryStream.Create;
      try
        ABGRABitmap.SaveToStreamAsPng(Result);
      finally
        Result.Position := 0;
      end;
    end;
     
    function BGRABitmapToBitmap(const ABGRABitmap: TBGRABitmap): TBitmap;
    begin
      Result := TBitmap.Create;
      try
        Result.PixelFormat := ABGRABitmap.Bitmap.PixelFormat;
        Result.Transparent := ABGRABitmap.HasTransparentPixels or ABGRABitmap.HasSemiTransparentPixels;
        Result.SetSize(ABGRABitmap.Width, ABGRABitmap.Height);
        Result.Canvas.Lock;
        try
          Result.Canvas.Draw(0, 0, ABGRABitmap.Bitmap);
        finally
          Result.Canvas.Unlock;
        end;
      finally
      end;
    end;
     
    procedure AssignBase64ToImage(const ABase64String: AnsiString; const AImage: TImage; const AFastMode: Boolean = False);
    var
      BGRABitmap: TBGRABitmap;
      Stream: TStream;
      bmp: TBitmap;
    begin
      AImage.Picture.Clear;
      Stream := CreateStreamFromBase64(ABase64String);
      if (DetectFileFormat(Stream) <> ifUnknown) then
        begin
          BGRABitmap := TBGRABitmap.Create;
          try
            try
              BGRABitmap.LoadFromStream(Stream);
            finally
              Stream.Free;
            end;
            if AFastMode then
              begin
                bmp := BGRABitmapToBitmap(BGRABitmap);
                try
                  AImage.Transparent := bmp.Transparent;
                  AImage.Picture.Assign(bmp);
                finally
                  bmp.Free;
                end;
              end
            else
              begin
                Stream := BGRABitmapToPNGStream(BGRABitmap);
                try
                  AImage.Transparent := BGRABitmap.HasTransparentPixels or BGRABitmap.HasSemiTransparentPixels;
                  AImage.Picture.PNG.LoadFromStream(Stream);
                finally
                  Stream.Free;
                end;
              end;
          finally
            BGRABitmap.Free;
          end;
        end
        else
          Stream.Free;
    end;
    {$EndIf}
     
    initialization
    {$If defined(UseBGRA)}
      BGRASVG.RegisterSvgFormat;
      BGRAOpenRaster.RegisterOpenRasterFormat;
      BGRAPhoxo.RegisterPhoxoFormat;
      BGRAPaintNet.RegisterPaintNetFormat;
    {$EndIf}
    end.

---

_Source: [https://wiki.freepascal.org/Base64](https://web.archive.org/web/20260115172143/https://wiki.freepascal.org/Base64)_
