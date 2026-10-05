# CryptINI

## Contents

  * 1 CryptINI
    * 1.1 Screenshots
    * 1.2 Purpose
    * 1.3 Disclaimer
    * 1.4 Encryption
    * 1.5 FPC versions
    * 1.6 Use and Example Code
      * 1.6.1 Simple use
      * 1.6.2 Using the WriteIdent etc in Form.Create
    * 1.7 Encrypting/Decrypting the whole INI file
    * 1.8 Original IniFiles methods and properties
    * 1.9 Testing App/Demo
    * 1.10 Documentation
    * 1.11 Licence
  * 2 Download
    * 2.1 Package Installation



## CryptINI

### Screenshots

  


  * Screenshot of Test/Demo app



[![latest testcryptini.jpg](https://wiki.freepascal.org/images/b/b9/latest_testcryptini.jpg)](</File:latest_testcryptini.jpg>)   


  * Contents of INI written by CryptINI



[![testcryptini1.jpg](https://wiki.freepascal.org/images/e/e5/testcryptini1.jpg)](</File:testcryptini1.jpg>)

### Purpose

TCryptIni is a TiniFile implementation that is resistant to tampering. In normal (PlainTextMode = FALSE) mode, any calls to Write values are accompanied by an embedded MD5 hash value (and also reversed then Base64/IDEA Encrypted) This is invisible in normal use (i.e. read methods return normal results) but there are added methods to internally verify any entries. It also is able to write a standard ident section containing various details including authorship and copyright. A single function allows you to check on app startup whether this section has been altered. It also includes a useful 'First Run' functionality. It's intended purpose is to store information that cannot be easily altered in a text editor (such as HiScores etc) by a weekend scripter. The WriteInteger method is the most secure as it double-encrypts as well as embedding an MD5Hash value as a checksum. Very handy to save scores etc. It is paired with ReadInteger and VerifyInteger 

### Disclaimer

This unit does not claim to pass any security tests nor be used in any environment where a moderate-level hacker could circumvent it. 

### Encryption

By Default CryptINI uses the DCPCrypt package for string encryption. The mode is IDEA cipher with an MD5 hash for the key. The EncryptINI and DecryptINI methods use DCPCrypt RC4 Cipher (With SHA hash) There is a $DEFINE USE_DCPCRYPT directive at the top of the ucryptini file. 

If this DEFINE is commented out, then CryptINI will default to BASE64 string encryption, which is weaker. Encrypt/DecryptINI methods will be unavailable. You can then delete any requirement for dcpcrypt in the project inspector. 

### FPC versions

Starting in FPC V3.1.1 there are additional options in TINIFile which are implemented in TCryptINI if the FPC Version >= 3.1.1 is detected. 

### Use and Example Code

  * **Tip:** Add CryptINI as a requirement in Project Inspector



#### Simple use

Instead of 
    
    
    Uses Inifiles;
    TForm1.SomeProcedure;
    Var INI:TIniFile;
    begin
     INI := TIniFile.Create(ChangeFileExt(Application.EXEName, '.cfg'));
    end;
    

You do: (note the 'uses ucryptini') 
    
    
    Uses ucryptini;
    TForm1.SomeProcedure;
    Var INI:TCryptIniFile;
    begin
     INI := TCryptIniFile.Create(ChangeFileExt(Application.EXEName, '.cfg'));
    end;
    

  * If you do:


    
    
    INI.PlainTextMode=True;
    INI.SectionHashing:=False;
    

.. then TCryptINI will behave *exactly* as TIniFile does, so you can drop it into existing code without changing an existing INI file. 

  * If you are coding a new app, then leave PlainTextmode as its default value (TRUE) and code as you would for Tinifile. The advantage is that you can then use CryptINI's extra methods, like Verify and WriteIdent etc.



#### Using the WriteIdent etc in Form.Create

  * You can hard-code an Ident Section in your INI file and check if it has been altered
  * **Note** to use this function you must include Version Info in your project (Project/Project Options/Version Info)
  * Typical Form.Create()


    
    
    procedure TForm1.FormCreate(Sender: TObject);
    const
      C_KEYPHRASE = 'I do like to be beside the seaside'; // Move this to the top of the unit
    Begin
     ...other code
     // Initialise the encrypted config file
     INI := TCryptIniFile.Create(ChangeFileExt(Application.EXEName, '.cfg'));
    
     // First ever run.  INI is absent
     If INI.IsVirgin then INI.WriteIdent('minesadorada','(c)2016','minesadorada@charcodelvalle.com','Creative Commons',TRUE);
    
     if NOT INI.VerifyIdent('5b319674f5cb55f3ed1e404e33c25868') then // I got this from the INI file
       ShowMessage('This is not a genuine copy of ' + Application.Title + '!')
     else INI.Deflower; // If not Deflowered then the default ini is used.
    
     // After first run, use the encrypted version
     If NOT INI.IsVirgin then
     begin
       INI.DecryptINI(TRUE,C_KEYPHRASE);
     // Check the unencrypted version..
       if NOT INI.VerifyIdent('5b319674f5cb55f3ed1e404e33c25868') then // I got this from the INI file
       ShowMessage('This is not a genuine copy of ' + Application.Title + '!');
     end;
     INI.KeyPhrase:=C_KEYPHRASE; // for subsequent read/writes
     ...other code
    end;
    
    procedure Tmainform.FormDestroy(Sender: TObject);
    const
      C_KEYPHRASE = 'I do like to be beside the seaside'; // Move this to the top of the unit
    Begin
     ...other code
      INI.EncryptINI(TRUE,C_KEYPHRASE);
      ...other code
    end;
    

  * Has the Ident been tampered with? Put something like this in Form.Create:


    
    
    // Use the MD5 value from the INI file (use your own!)
    If INI.VerifyIdent('92abf0deecbb25c435bff507a396d92a') then
      ShowMessage('Ident verified OK') // do nothing
    else
      ShowMessage('Ident failed verification'); // Warning message/exit
    

  * Test for first run


    
    
    If INI.IsVirgin then // note that doing the test Deflowers the app by default
       ShowMessage('First time run of this app');
    

  * Toggle to normal UnEncrypted INI use



(by default it is set to FALSE) 
    
    
    INI.PlainTextMode:=TRUE;
    INI.WriteString('MySection', 'String', 'MyString'); // just writes normally
    

  * When PlainTextMode = FALSE (default) and SectionHashing=TRUE (default),Write<anytype> encrypts the value and prefixes the encrypted value with an MD5Hash. In addition each section has an entry MD5Hash=<32 characters> which is updated on each write.


    
    
    INI.WriteInteger('MySection', 'Integer',1000);
    

  * Note WriteInteger adds a '+' (INTEGER_MARKER) to the written Key



This is so that CryptINI can deal with Integers specially Using CryptINI in either mode, this is invisible in use (i.e. don't add a + to the ident for any methods) 

  * When PlainTextMode = FALSE (default), use these convenient methods if you like:


    
    
    INI.ReadUnencryptedString
    INI.WriteUnencryptedString
    INI.ReadUnEncryptedInteger
    INI.WriteUnencryptedInteger
    

  * Store 'First Run' status using these methods:


    
    
    INI.IsVirgin
    INI.Deflower
    INI.ReFlower (useful for testing)
    

### Encrypting/Decrypting the whole INI file

  * You can encrypt and decrypt the entire INI file using methods EncryptINI and DecryptINI.



This uses a different cipher and hash method, so is extra-secure. 

  * Note: CryptINI methods and properties an only work with an unencrypted INI file, So; Decrypt on startup, and Encrypt before exit.
  * By default the routines use the KeyPhrase property, but you can override this in the EncryptINI/DecryptINI method calls along with whether to delete the "old" file and what file extension to use for the encrypted file.



### Original IniFiles methods and properties

Just use them as normal. Note: you can mix-and-match Plaintext and Encrypted in the same INI file. Just toggle the property PlainTextMode before Writing/Reading 

### Testing App/Demo

  * This can be downloaded from <https://sourceforge.net/projects/lazautoupdate/files/otherpackages/testcryptini_compiled.zip>
  * There are compiled versions for Windows 32/64-bit and Linux 32/64-bit in the archive. (Also untested MacOS 10 'Darwin' 32-bit)



### Documentation

  * As well as this page, ucryptini.pas is heavily commented. The Testing app (source also commented) will help you understand the strengths and limitations of CryptINI, and how to get the best of it in your own applications.



### Licence

  * [ModifiedGPL](<https://www.gnu.org/licenses/gpl-faq.en.html>)



## Download

  * Sourcecode and Testing app Direct link: <https://sourceforge.net/projects/lazautoupdate/files/otherpackages/cryptinitest_source.zip>
  * CryptIni as a component: via [Online_Package_Manager](<Online_Package_Manager.md> "Online Package Manager")



### Package Installation

  * Download the package: <https://sourceforge.net/projects/lazautoupdate/files/otherpackages/cryptini.zip>
  * Unzip it into a new folder (Suggest <LazarusDir>/components/cryptini)
  * In the Lazarus IDE Click Package/Open Package File (lpk), navigate to your new folder and click "cryptini.lpk", then "Open"
  * Just click "Compile". It is not a visual component, so cannot be "Installed"
  * To use it in your project, just add ucryptini to the Uses clause when required, or better still: add CryptIni (the component) to the projects's requirements.



  


* * *

[Minesadorada](</User:Minesadorada> "User:Minesadorada")

---

_Source: [https://wiki.freepascal.org/CryptINI](https://web.archive.org/web/20250417150115/https://wiki.freepascal.org/CryptINI)_
