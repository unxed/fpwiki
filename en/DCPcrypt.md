# DCPcrypt

│ **English (en)** │

## Contents

  * 1 About
  * 2 Author(s)
  * 3 License
  * 4 Download
  * 5 Installation
  * 6 Usage
    * 6.1 Encrypting using plain key
    * 6.2 Lazarus example
  * 7 Bugs
  * 8 External links



### About

DCPcrypt is a collection of cryptographic components for the Lazarus. 

The idea behind DCPcrypt is that it should be possible to "drop in" any algorithm implementation to replace another with minimum or no code changes. To aid in this goal all cryptographic components are descended from one of several base classes, TDCP_cipher for encryption algorithms and TDCP_hash for message digest algorithms. 

Since v2.0.4 it compiles with FPC 2.4.1 and has been tested under 32-bit & 64-bit Linux and 32-bit Windows. 

  * Status: Production/Stable
  * Latest version: 2.0.4.1



  


Notice that DCPcrypt does not have an (un)official maintainer anymore. 

  


Screenshots: 

[![Dcp1.png](https://wiki.freepascal.org/images/b/be/Dcp1.png)](</File:Dcp1.png>)

[![Dcp2.png](https://wiki.freepascal.org/images/7/74/Dcp2.png)](</File:Dcp2.png>)

### Author(s)

  * David Barton, [home page](<http://www.cityinthesky.co.uk>)



Contributors: 

  * Port to Lazarus by barko \- 2006
  * [Graeme Geldenhuys](<http://geldenhuys.co.uk>) \- 2009



### License

The MIT License 

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions: 

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software. 

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE. 

### Download

Since March 2010, DCPCrypt source code is hosted as part of the [Lazarus-CCR project](<http://sourceforge.net/projects/lazarus-ccr/>) on SourceForge.net. 

DCPCrypt is also hosted in a [Git](<http://git-scm.com>) repository. To download the latest source code from the repository you need to run the following command. 
    
    
    git clone <git://lazarus-ccr.git.sourceforge.net/gitroot/lazarus-ccr/dcpcrypt>
    
    
    
    git clone http://git.code.sf.net/p/lazarus-ccr/dcpcrypt lazarus-ccr-dcpcrypt
    

After that you can stay updated by simply running 
    
    
    git pull
    

### Installation

Since the v2.0.4 release, DCPCrypt is split into two packages: 

dcpcrypt.lpk
    This is a run-time only package and only contains the core non-gui code. You can use the package for console applications or other non-LCL projects (eg: fpGUI or MSEgui based projects).
dcpcrypt_laz.lpk
    This is the design time package to register the various components with the Lazarus IDE component palette.

To use DCPCrypt with Lazarus IDE for the first time: 

  * In 'Package > Open Package File', open dcpcrypt.lpk and click **Compile**
  * If you want to register the DCPCrypt components with the Lazarus IDE component palette, open the dcpcrypt_laz.lpk and click **Compile** and then **Install**. Lazarus IDE will now rebuild itself.
  * Restart Lazarus IDE and you are ready to use DCPCrypt.



### Usage

Please note that an appreciation of the basic principles of encryption/decryption and key management is needed to ensure the correct usage of the ciphers implemented within this package. A good introduction on this subject is provided by Bruce Schneier's "Applied Cryptography" (ISBN: 0-471-11709-9) also see the NIST publication SP800-38A for information on the block cipher chaining modes. 

  * Ciphers - the basic building block of DCPcrypt, the TDCP_cipher component.
  * Block Ciphers - the base of all block ciphers, the TDCP_blockcipher component.
  * Hashes - the base of all hash algorithms, the TDCP_hash component.



See included documentation. 

#### Encrypting using plain key

Normally, for encryption, dpcpcrypt expects keys that are hashed when they are supplied through _.InitStr_. 

You can pass a plain string key by using _[.Init](<http://is.muni.cz/th/51468/fi_b/priloha/DCPcrypt/Docs/Ciphers.html#Init>)_ , e.g. like this: 
    
    
    var s:string;
    begin
      s:='1234567890ABCDEF1234567890ABCDEF';
      DCP_3des1.Init(s[1],length(s)*8,nil);
    

Source: [forum thread](<http://lazarus.freepascal.org/index.php/topic,18108.msg101683.html#msg101683>). 

#### Lazarus example
    
    
    unit Unit1;
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      DCPrc4, DCPsha1;
     
    type
     
      { TForm1 }
     
      TForm1 = class(TForm)
        Button1: TButton;
        Button2: TButton;
        DCP_rc4_1: TDCP_rc4;
        DCP_sha1_1: TDCP_sha1;
        Edit1: TEdit;
        Edit2: TEdit;
        Label1: TLabel;
        Label2: TLabel;
        procedure Button1Click(Sender: TObject);
        procedure Button2Click(Sender: TObject);
        procedure FormCreate(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end;
     
    var
      Form1: TForm1;
      KeyStr: string;
     
    implementation
     
    {$R *.lfm}
     
    { TForm1 }
     
    procedure TForm1.Button1Click(Sender: TObject);
    var
       Cipher: TDCP_rc4;
       Source, Dest: TFileStream;
     begin
       //if InputQuery('Passphrase','Enter passphrase',KeyStr) then  // get the passphrase
       begin
         try
           Source:= TFileStream.Create(Edit1.Text,fmOpenRead);   // Edit1.Text, PATH SOURCE OF FILE
           Dest:= TFileStream.Create(Edit2.Text,fmCreate);       // Edit2.Text, PATH DESTINATION OF FILE
           Cipher:= TDCP_rc4.Create(Self);
           Cipher.InitStr(KeyStr,TDCP_sha1);              // initialize the cipher with a hash of the passphrase
           Cipher.EncryptStream(Source,Dest,Source.Size); // encrypt the contents of the file
           Cipher.Burn;
           Cipher.Free;
           Dest.Free;
           Source.Free;
           MessageDlg('File encrypted',mtInformation,[mbOK],0);
         except
           MessageDlg('File IO error',mtError,[mbOK],0);
         end;
       end;
     end;
     
    procedure TForm1.Button2Click(Sender: TObject);
    var
        Cipher: TDCP_rc4;
        //KeyStr: string;
        Source, Dest: TFileStream;
      begin
        //if InputQuery('Passphrase','Enter passphrase',KeyStr) then  // get the passphrase
        begin
          try
            Source:= TFileStream.Create(Edit1.Text,fmOpenRead);   // Edit1.Text, PATH SOURCE OF FILE
            Dest:= TFileStream.Create(Edit2.Text,fmCreate);       // Edit2.Text, PATH DESTINATION OF FILE
            Cipher:= TDCP_rc4.Create(Self);
            Cipher.InitStr(KeyStr,TDCP_sha1);              // initialize the cipher with a hash of the passphrase
            Cipher.DecryptStream(Source,Dest,Source.Size); // decrypt the contents of the file
            Cipher.Burn;
            Cipher.Free;
            Dest.Free;
            Source.Free;
            MessageDlg('File decrypted',mtInformation,[mbOK],0);
          except
            MessageDlg('File IO error',mtError,[mbOK],0);
          end;
        end;
      end;
     
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      KeyStr         := 'casa777';     // my password
    end;
     
    end.
    

[![dcrypt form.jpg](https://wiki.freepascal.org/images/a/a6/dcrypt_form.jpg)](</File:dcrypt_form.jpg>)

### Bugs

If you are using DCPrijndael, be advised that key sizes that are not in the AES standard are known to produce key schedules that are **not** inter-operable with other Rijndael implementations. Few implementations provide the non-standard key (or block) sizes. [Brian Gladman](<https://web.archive.org/web/20090201173123/http://fp.gladman.plus.com/cryptography_technology/rijndael/>) and [Jeffrey Clement](<https://web.archive.org/web/20110826031605/http://jclement.ca/software/pyrijndael/>) provide implementations (C and Python) that are known to be incompatible with DCPrijndael for non-standard key sizes. Test program provided below with comments showing the expect results: 
    
    
    program ctst;
    {$mode objfpc}{$H+}
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Sysutils, DCPrijndael;
    const
      InData: array[0..15] of byte =
        ($32,$43,$f6,$a8,$88,$5a,$30,$8d,$31,$31,$98,$a2,$e0,$37,$07,$34);
      Key1: array[0..19] of byte =
        ($2b,$7e,$15,$16,$28,$ae,$d2,$a6,$ab,$f7,$15,$88,$09,$cf,$4f,$3c,$76,$2e,$71,$60);
      Key2: array[0..31] of byte =
        ($2b,$7e,$15,$16,$28,$ae,$d2,$a6,$ab,$f7,$15,$88,$09,$cf,$4f,$3c,$76,$2e,$71,$60,
         $f3,$8b,$4d,$a5,$6a,$78,$4d,$90,$45,$19,$0c,$fe);
      OutData1: array[0..15] of byte =
        ($23,$1d,$84,$46,$39,$b3,$1b,$41,$22,$11,$cf,$e9,$37,$12,$b8,$80);
      OutData2: array[0..15] of byte =
        ($1a,$6e,$6c,$2c,$66,$2e,$7d,$a6,$50,$1f,$fb,$62,$bc,$9e,$93,$f3);
    var
      Cipher: TDCP_rijndael;
      Block: array[0..15] of byte;
    begin
    // lengths:  block = 16 bytes, key = 20 bytes
    // key     = 2b7e151628aed2a6abf7158809cf4f3c762e7160
    // input   = 3243f6a8885a308d313198a2e0370734
    // encrypt = 231d844639b31b412211cfe93712b880
    // decrypt = 3243f6a8885a308d313198a2e0370734
      Cipher := TDCP_rijndael.Create(nil);
      Cipher.Init(Key1, Sizeof(Key1) * 8, nil);
      Cipher.EncryptECB(InData, Block);
      writeln('128-bit block & 160-bit key results:');
      writeln(boolean(CompareMem(@Block, @OutData1, 16)));
      Cipher.DecryptECB(Block, Block);
      writeln(boolean(CompareMem(@Block, @InData, 16)));
      Cipher.Free;
    // lengths:  block = 16 bytes, key = 32 bytes
    // key     = 2b7e151628aed2a6abf7158809cf4f3c762e7160f38b4da56a784d9045190cfe
    // input   = 3243f6a8885a308d313198a2e0370734
    // encrypt = 1a6e6c2c662e7da6501ffb62bc9e93f3
    // decrypt = 3243f6a8885a308d313198a2e0370734
      Cipher := TDCP_rijndael.Create(nil);
      Cipher.Init(Key2, Sizeof(Key2) * 8, nil);
      Cipher.EncryptECB(InData, Block);
      writeln('128-bit block & 256-bit key results:');
      writeln(boolean(CompareMem(@Block, @OutData2, 16)));
      Cipher.DecryptECB(Block, Block);
      writeln(boolean(CompareMem(@Block, @InData, 16)));
      Cipher.Free;
    end.
    

The output that freepascal 2.2.0 produces is: 
    
    
     128-bit block & 160-bit key results:
     FALSE
     TRUE
     128-bit block & 256-bit key results:
     TRUE
     TRUE
    

What this meas is that using 160-bit keys you can only decrypt the output with DCPrijndael, but it won't work with other implementations. On the other hand, 256-bit keys are interoperable. The 160-bit case is important because SHA-1 outputs 160-bit hashes, which makes it a convenient way to generate a key from a password, so it's used by some (hardware) systems. 

### External links

  * [HMAC/PBKDF1/PBKDF2](<http://keit.co/p/dcpcrypt-hmac-rfc2104/>) implementation based on DCPcrypt.

---

_Source: [https://wiki.freepascal.org/DCPcrypt](https://web.archive.org/web/20240910012950/https://wiki.freepascal.org/DCPcrypt)_
