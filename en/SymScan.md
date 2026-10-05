# SymScan

## Contents

  * 1 About
  * 2 Functional Description:
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Dependencies / System Requirements
    * 6.1 Development requirements:
    * 6.2 Hardware:
  * 7 Documentation



## About

SymScan provides an Barcode scanning component and API link to the barcode scanner module in Symbol/Motorola MC series Windows CE based barcode scanners. 

## Functional Description:

Two principal units are provided: 

  * symscanapi.pp contains the bindings to the MC's SCNAPI32.DLL library.
  * uscanner.pas provides a TScanner component that is derived from a TThread for easier access to the hardware scanner.



A very simple project is included as an example. 

## Author

Arnold Bosch: arnoldb at mweb dot com dot na 

## License

The sample application is licensed under the GPL, and the libraries under LGPL 

## Download

It can be downloaded from [Lazarus-CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=323770>). 

## Dependencies / System Requirements

### Development requirements:

  * FPC 2.2.2 or later
  * Lazarus 0.9.26 or later
  * The appropriate cross-arm-wince-win32 installation for FPC/Lazarus above.



### Hardware:

  * For most MC series handhelds, a suitable aygshell.dll will have to be installed - see [here](<http://wiki.lazarus.freepascal.org/WinCE_Programming_Tips>)
  * Tested devices include: (If it works on others, please add!) 
    * MC9000 series with Windows CE 4.2
    * MC1000 series with Windows CE 4.2
    * MC70 with Windows Mobile 5.0



  
NOTE: Because there is no normal Windows scanning interface to substitute for the MC's dll, application development can only be done by compiling and downloading the application to the hardware device for testing. In my experience, this is not always practical when developing a fairly big/complicated application, so I can do the following recommendation: 

  * Keep all scanner-related code together in your code as far as possible, and put {$IFDEF MCDEVICE} and {$ENDIF MCDEVICE} before and after the code blocks.
  * At the top of your application, use a {$DEFINE MCDEVICE} if you want to compile with scanner support, or comment it out (// {$DEFINE MCDEVICE}) to test your application without scanner support on normal Windows, the Windows WinCE emulator or WinCE based devices that cannot use this unit.



  


## Documentation

The TScanner component is derived from TThread, and introduces two events: 
    
    
    OnScan(Barcode:String, Symbology: String);
    

Barcode is the barcode returned from a scan. Note that check-digits are NOT removed. Symbology returns the type of barcode that was scanned (EAN13, UTF8, Code 39 etc). 
    
    
    OnError(ErrorMessage: String)
    

When the scanner component encounter an error, ErrorMessage contains a descriptive string. 

  * To use the scanning component, include uscanner in your application's uses clause.
  * Add a component to the main form for the scanner, e.g. Scanner:TScanner
  * Add procedures to your form to handle OnScan and OnError events:


    
    
    TForm1 = class(TForm)
      procedure FormCreate(Sender: TObject);
    private
      Scanner:TScanner;
      Procedure GetScan(barcode:String; Symbology:String);
      Procedure GetScanError(ErrorMessage:string);
      { private declarations }
    public
      { public declarations }
    end;
    

  * At an appropriate location in your code, create the scanner, with the suspended option set to TRUE
  * Set the scanner component's OnScan and OnError properties to the procedures you created for them
  * To enable the scanner, Resume the Scanner component


    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Scanner := TScanner.Create(True);
      Scanner.OnError := @GetScanError;
      Scanner.OnScan := @GetScan;
      Scanner.Resume;
    end;
    

  * Also remember to stop the scanner when your application closes:


    
    
    Scanner.Terminate;

---

_Source: [https://wiki.freepascal.org/SymScan](https://web.archive.org/web/20250422010312/https://wiki.freepascal.org/SymScan)_
