# Raspberry Pi : BerryClip

│ **English (en)** │

[![Raspberry Pi Logo.png](https://wiki.freepascal.org/images/8/85/Raspberry_Pi_Logo.png)](</File:Raspberry_Pi_Logo.png>)

This article applies to [Raspberry Pi](</Category:Raspberry_Pi> "Category:Raspberry Pi") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  


## Contents

  * 1 Raspberry Pi : BerryClip
    * 1.1 Hardware
    * 1.2 Software
      * 1.2.1 User interface
        * 1.2.1.1 unit1.pas
        * 1.2.1.2 unit1.lfm
      * 1.2.2 Main
      * 1.2.3 Board
        * 1.2.3.1 uberryclip.pas
        * 1.2.3.2 uraspberrypi.pas
        * 1.2.3.3 pi_io_unit.pas



# Raspberry Pi : BerryClip

[![pi berryclip.png](https://wiki.freepascal.org/images/c/c7/pi_berryclip.png)](</File:pi_berryclip.png>)

The BerryClip 6 LED Board is a add-on board for the Raspberry Pi. It consists of a PCB populated with 6 coloured LEDs, one switch and one buzzer. It is easy to attach to the Pi’s GPIO header. 

## Hardware

[![pi berryclip sch.png](https://wiki.freepascal.org/images/7/7b/pi_berryclip_sch.png)](</File:pi_berryclip_sch.png>)

The following list shows the mapping between the components, the header pins and the GPIO references : 

  * LED 1 - Pin 7 - GPIO4
  * LED 2 - Pin 11 - GPIO17
  * LED 3 - Pin 15 - GPIO22
  * LED 4 - Pin 19 - GPIO10
  * LED 5 - Pin 21 - GPIO9
  * LED 6 - Pin 23 - GPIO11
  * Buzzer - Pin 24 - GPIO8
  * Switch - Pin 26 - GPIO7



  


GPIO  | Pin  |  | Pin  | GPIO   
---|---|---|---|---  
GPIO7  | 26  |  | 25  | GND   
GPIO8  | 24  |  | 23  | GPIO11   
GPIO25  | 22  |  | 21  | GPIO9   
GND  | 20  |  | 19  | GPIO10   
GPIO24  | 18  |  | 17  |  +3.3V   
GPIO23  | 16  |  | 15  | GPIO22   
GND  | 14  |  | 13  | GPIO27   
GPIO18  | 12  |  | 11  | GPIO17   
GPIO15  | 10  |  | 9  | GND   
GPIO14  | 8  |  | 7  | GPIO4   
GND  | 6  |  | 5  | GPIO3   
+5V  | 4  |  | 3  | GPIO2   
+5V  | 2  |  | 1  |  +3.3V   
  
  


  


  


  


## Software

[![pi berryclip uml.png](https://wiki.freepascal.org/images/f/f4/pi_berryclip_uml.png)](</File:pi_berryclip_uml.png>)

The following example lists a program that controls berryClip. 

The code requires to be run as root (eg. sudo ./berryclipdemo). 

  


Level  |  | Unit   
---|---|---  
1  | User interface, Application level  | unit1.pas, unit1.lfm, berryclipdemo.lpr   
2  | Board level  | uberryclip.pas   
3  |  | uraspberrypi.pas   
4  | I/O driver  | pi_io_unit.pas   
  
### User interface

[![pi berryclipdemo.png](https://wiki.freepascal.org/images/4/4e/pi_berryclipdemo.png)](</File:pi_berryclipdemo.png>)

The user interface consists of the buttons that control the LEDs and buzzer. In addition, the switch status is displayed on LED. LEDs state on the circuit board is displayed with the LEDs on the form. The berryclipdemo program uses the following components: [TApplicationProperties](<TApplicationProperties.md> "TApplicationProperties"), [TButton](<TButton.md> "TButton"), [TShape](<TShape.md> "TShape") and [TToggleBox](<TToggleBox.md> "TToggleBox"). 

#### unit1.pas
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      ExtCtrls, uberryclip;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        ApplicationProperties1: TApplicationProperties;
        Button1: TButton;
        Led4ToggleBox: TToggleBox;
        Led6ToggleBox: TToggleBox;
        Led2ToggleBox: TToggleBox;
        Led3ToggleBox: TToggleBox;
        Led1ToggleBox: TToggleBox;
        Led5ToggleBox: TToggleBox;
        Shape1: TShape;
        Shape2: TShape;
        Shape3: TShape;
        Shape4: TShape;
        Shape5: TShape;
        Shape6: TShape;
        Shape7: TShape;
        procedure ApplicationProperties1Idle(Sender: TObject; var Done: Boolean);
        procedure Button1MouseDown(Sender: TObject; Button: TMouseButton;
          Shift: TShiftState; X, Y: Integer);
        procedure Button1MouseUp(Sender: TObject; Button: TMouseButton;
          Shift: TShiftState; X, Y: Integer);
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
        procedure Led1ToggleBoxChange(Sender: TObject);
        procedure Led2ToggleBoxChange(Sender: TObject);
        procedure Led3ToggleBoxChange(Sender: TObject);
        procedure Led4ToggleBoxChange(Sender: TObject);
        procedure Led5ToggleBoxChange(Sender: TObject);
        procedure Led6ToggleBoxChange(Sender: TObject);
      private
        { private declarations }
        Berry_Clip:TBerryClip;
     public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
    
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    
    
    procedure TForm1.ApplicationProperties1Idle(Sender: TObject; var Done: Boolean);
    begin
      if Berry_Clip.ReadSwitch
        then  shape7.Brush.Color:=clBlack
        else  shape7.Brush.Color:=clWhite;
    end;
    
    procedure TForm1.Button1MouseDown(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    begin
      Berry_Clip.SetBuzzerOn;
    end;
    
    procedure TForm1.Button1MouseUp(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    begin
      Berry_Clip.SetBuzzerOff;
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Berry_Clip := TBerryClip.Create;
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      Berry_Clip.Free;
    end;
    
    
    procedure TForm1.Led1ToggleBoxChange(Sender: TObject);
    begin
      if Led1ToggleBox.Checked then
        begin
          Berry_Clip.SetLedOn(1);
          shape1.Brush.Color:=clRed ;
        end
        else
        begin
          Berry_Clip.SetLedOff(1);
          shape1.Brush.Color:=clWhite;
        end;
    end;
    
    procedure TForm1.Led2ToggleBoxChange(Sender: TObject);
    begin
      if Led2ToggleBox.Checked then
        begin
          Berry_Clip.SetLedOn(2);
          shape2.Brush.Color:=clRed;
        end
        else
        begin
          Berry_Clip.SetLedOff(2);
          shape2.Brush.Color:=clWhite;
        end;
    end;
    
    procedure TForm1.Led3ToggleBoxChange(Sender: TObject);
    begin
      if Led3ToggleBox.Checked then
        begin
          Berry_Clip.SetLedOn(3);
          shape3.Brush.Color:=clYellow;
        end
        else
        begin
          Berry_Clip.SetLedOff(3);
          shape3.Brush.Color:=clWhite;
        end;
    end;
    
    procedure TForm1.Led4ToggleBoxChange(Sender: TObject);
    begin
      if Led4ToggleBox.Checked then
        begin
          Berry_Clip.SetLedOn(4);
          shape4.Brush.Color:=clYellow;
        end
        else
        begin
          Berry_Clip.SetLedOff(4);
          shape4.Brush.Color:=clWhite;
        end;
    end;
    
    procedure TForm1.Led5ToggleBoxChange(Sender: TObject);
    begin
      if Led5ToggleBox.Checked then
        begin
          Berry_Clip.SetLedOn(5);
          shape5.Brush.Color:=clGreen;
        end
        else
        begin
          Berry_Clip.SetLedOff(5);
          shape5.Brush.Color:=clWhite;
        end;
    end;
    
    procedure TForm1.Led6ToggleBoxChange(Sender: TObject);
    begin
      if Led6ToggleBox.Checked then
        begin
          Berry_Clip.SetLedOn(6);
          shape6.Brush.Color:=clGreen;
        end
        else
        begin
          Berry_Clip.SetLedOff(6);
          shape6.Brush.Color:=clWhite;
        end;
    end;
    
    
    end.
    

  


#### unit1.lfm
    
    
    object Form1: TForm1
      Left = 269
      Height = 184
      Top = 175
      Width = 176
      Caption = 'BerryClip'
      ClientHeight = 184
      ClientWidth = 176
      OnCreate = FormCreate
      OnDestroy = FormDestroy
      object Led2ToggleBox: TToggleBox
        Left = 8
        Height = 24
        Top = 32
        Width = 128
        Caption = 'Led2ToggleBox'
        OnChange = Led2ToggleBoxChange
        TabOrder = 0
      end
      object Led1ToggleBox: TToggleBox
        Left = 8
        Height = 24
        Top = 8
        Width = 128
        Caption = 'Led1ToggleBox'
        OnChange = Led1ToggleBoxChange
        TabOrder = 1
      end
      object Led3ToggleBox: TToggleBox
        Left = 8
        Height = 24
        Top = 56
        Width = 128
        Caption = 'Led3ToggleBox'
        OnChange = Led3ToggleBoxChange
        TabOrder = 2
      end
      object Led4ToggleBox: TToggleBox
        Left = 8
        Height = 24
        Top = 80
        Width = 128
        Caption = 'Led4ToggleBox'
        OnChange = Led4ToggleBoxChange
        TabOrder = 3
      end
      object Led5ToggleBox: TToggleBox
        Left = 8
        Height = 24
        Top = 104
        Width = 128
        Caption = 'Led5ToggleBox'
        OnChange = Led5ToggleBoxChange
        TabOrder = 4
      end
      object Led6ToggleBox: TToggleBox
        Left = 8
        Height = 24
        Top = 128
        Width = 128
        Caption = 'Led6ToggleBox'
        OnChange = Led6ToggleBoxChange
        TabOrder = 5
      end
      object Shape1: TShape
        Left = 144
        Height = 18
        Top = 12
        Width = 16
        Shape = stCircle
      end
      object Shape2: TShape
        Left = 144
        Height = 18
        Top = 34
        Width = 16
        Shape = stCircle
      end
      object Shape3: TShape
        Left = 144
        Height = 18
        Top = 58
        Width = 16
        Shape = stCircle
      end
      object Shape4: TShape
        Left = 144
        Height = 18
        Top = 82
        Width = 16
        Shape = stCircle
      end
      object Shape5: TShape
        Left = 144
        Height = 18
        Top = 106
        Width = 16
        Shape = stCircle
      end
      object Shape6: TShape
        Left = 144
        Height = 18
        Top = 130
        Width = 16
        Shape = stCircle
      end
      object Shape7: TShape
        Left = 144
        Height = 18
        Top = 158
        Width = 16
        Shape = stCircle
      end
      object Button1: TButton
        Left = 8
        Height = 25
        Top = 151
        Width = 64
        Caption = 'Buzzer'
        OnMouseDown = Button1MouseDown
        OnMouseUp = Button1MouseUp
        TabOrder = 6
      end
      object ApplicationProperties1: TApplicationProperties
        OnIdle = ApplicationProperties1Idle
        left = 72
        top = 152
      end
    end
    

### Main
    
    
    program berryclipdemo;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, Unit1, pi_io_unit, uberryclip, uraspberrypi
      { you can add units after this };
    
    {$R *.res}
    
    begin
      RequireDerivedFormResource := True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

### Board

#### uberryclip.pas
    
    
    unit uberryclip;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils,
      uRaspberrypi;
    type
    
      { TBerryClip }
    
      TBerryClip = class
      private
        RaspBerryPi : TRaspBerryPi;
      public
        constructor Create;
        procedure SetLedOn( led_no :integer );
        procedure SetLedOff( led_no :integer );
        procedure SetBuzzerOn;
        procedure SetBuzzerOff;
        function ReadSwitch:boolean;
        destructor Destroy; override;
      end;
    implementation
    
    
    { TBerryClip }
    
    
    constructor TBerryClip.Create;
    begin
      RaspBerryPi := TRaspBerryPi.Create;
      RaspBerryPi.SetGpioPinOutput(GPIO4); // Led 1
      RaspBerryPi.SetGpioPinOutput(GPIO9); // Led 2
      RaspBerryPi.SetGpioPinOutput(GPIO10);// Led 3
      RaspBerryPi.SetGpioPinOutput(GPIO11);// Led 4
      RaspBerryPi.SetGpioPinOutput(GPIO17);// Led 5
      RaspBerryPi.SetGpioPinOutput(GPIO22);// Led 6
      RaspBerryPi.SetGpioPinInput(GPIO7);  // Switch
      RaspBerryPi.SetGpioPinOutput(GPIO8); // Buzzer
    end;
    
    procedure TBerryClip.SetLedOn(led_no: integer);
    begin
      case led_no of
        1 : RaspBerryPi.SetGpioBit(GPIO4);
        2 : RaspBerryPi.SetGpioBit(GPIO17);
        3 : RaspBerryPi.SetGpioBit(GPIO22);
        4 : RaspBerryPi.SetGpioBit(GPIO10);
        5 : RaspBerryPi.SetGpioBit(GPIO9);
        6 : RaspBerryPi.SetGpioBit(GPIO11);
      end;
    end;
    
    procedure TBerryClip.SetLedOff(led_no: integer);
    begin
      case led_no of
        1 : RaspBerryPi.ClearGpioBit(GPIO4);
        2 : RaspBerryPi.ClearGpioBit(GPIO17);
        3 : RaspBerryPi.ClearGpioBit(GPIO22);
        4 : RaspBerryPi.ClearGpioBit(GPIO10);
        5 : RaspBerryPi.ClearGpioBit(GPIO9);
        6 : RaspBerryPi.ClearGpioBit(GPIO11);
      end;
    
    end;
    
    procedure TBerryClip.SetBuzzerOn;
    begin
      RaspBerryPi.SetGpioBit(GPIO8);
    end;
    
    procedure TBerryClip.SetBuzzerOff;
    begin
      RaspBerryPi.ClearGpioBit(GPIO8);
    end;
    
    function TBerryClip.ReadSwitch: boolean;
    begin
      result := RaspBerryPi.GetGpioBit(GPIO7);
    end;
    
    destructor TBerryClip.Destroy;
    begin
      RaspBerryPi.Free;
      inherited Destroy;
    end;
    
    end.
    

#### uraspberrypi.pas
    
    
    unit uraspberrypi;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, pi_io_unit, dialogs;
    
    const
    
      GPIO4 = 4;
      GPIO7 = 7;
      GPIO8 = 8;
      GPIO9 = 9;
      GPIO10= 10;
      GPIO11= 11;
      GPIO17= 17;
      GPIO22= 22;
    
    type
    
      { TRaspBerryPi }
    
      TRaspBerryPi = class
      private
         io_port: T_IO_Port;
      public
        constructor Create;
        procedure ClearGpioBit(no:byte);
        function GetGpioBit(no:byte):boolean;
        procedure SetGpioBit(no:byte);
        procedure SetGpioPinInput( no:byte);
        procedure SetGpioPinOutput( no:byte);
        destructor Destroy; override;
      end;
    
    implementation
    
    
    { TRaspBerryPi }
    
    
    constructor TRaspBerryPi.Create;
    begin
      if not MapIo
        then ShowMessage('Error mapping gpio')
        else
          begin
            io_port := T_IO_Port.Create;
            io_port.f_gpio := CreatePort(GPIO_BASE);
          end ;
    end;
    
    procedure TRaspBerryPi.ClearGpioBit(no: byte);
    begin
      io_port.ClearBit(no);
    end;
    
    function TRaspBerryPi.GetGpioBit(no: byte): boolean;
    begin
      result := io_port.GetBit(no);
    end;
    
    procedure TRaspBerryPi.SetGpioBit(no: byte);
    begin
      io_port.SetBit(no);
    end;
    
    procedure TRaspBerryPi.SetGpioPinInput(no: byte);
    begin
      io_port.SetPinMode(no,INPUT);
    end;
    
    procedure TRaspBerryPi.SetGpioPinOutput(no: byte);
    begin
      io_port.SetPinMode(no,OUTPUT);
    end;
    
    destructor TRaspBerryPi.Destroy;
    begin
      io_port.ClearBit(GPIO4);
      io_port.ClearBit(GPIO9);
      io_port.ClearBit(GPIO10);
      io_port.ClearBit(GPIO11);
      io_port.ClearBit(GPIO17);
      io_port.ClearBit(GPIO22);
      io_port.ClearBit(GPIO8);
      UnMapIo(io_port);
      io_port.free;
      inherited Destroy;
    end;
    
    end.
    

#### pi_io_unit.pas
    
    
    unit pi_io_unit;
    // This code is tested only RPI2B Processor
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Unix, BaseUnix;
    
    const
      BCM2709_PBASE= $3F000000; 	// Peripheral Base in Bytes (RPI2B Processor)
      // BCM2708_PBASE= $20000000; 	// Peripheral Base (maybe RPI1 )
      REG_GPIO = BCM2709_PBASE div $1000; // $3F000 ;
      PAGE_SIZE = $1000;
    
      GPIO_BASE =  (REG_GPIO + $200);
    
      INPUT  = 0;
      OUTPUT = 1;
    
      GPSET  = $1C; // GPIO Pin Output Set
      GPCLR  = $28; // GPIO Pin Output Clear
      GPLEV  = $34; // GPIO Pin Level
    
    
    type
    
      { T_IO_Port }
    
      T_IO_Port = class
      private
      public
        f_gpio: PLongWord;
        procedure SetPinMode(a_io_pin, a_mode: byte);
        function GetBit(a_io_pin : byte):boolean;inline;
        procedure ClearBit(a_io_pin : byte);inline;
        procedure SetBit(a_io_pin : byte);inline;
      end;
    
    
      function MapIo:boolean;// creates io mapping
      procedure UnMapIo(a_Map: T_IO_Port);// close io
      function CreatePort(PortGpio: LongWord): PLongWord;
    
    implementation
    
    var
      fd: integer;// /dev/mem file handle
    
    function MapIo: boolean;
    begin
      result := true;
      fd := fpopen('/dev/mem', O_RdWr or O_Sync);
      if fd < 0 then result := false; // unsuccessful mapping
    end;
    
    procedure UnMapIo( a_Map: T_IO_Port);
    begin
      if a_Map.f_gpio <> nil then
        begin
          fpMUnmap(a_Map.f_gpio, PAGE_SIZE);
          a_Map.f_gpio := nil;
        end;
    end;
    
    function CreatePort(PortGpio: LongWord): PLongWord;
    begin
      result := FpMmap(nil, PAGE_SIZE, PROT_READ or PROT_WRITE, MAP_SHARED, fd, PortGpio);
    end;
    
    procedure T_IO_Port.SetPinMode(a_io_pin, a_mode: byte);
    var
      fSel, shift : byte;
      iof : PLongWord;
    begin
      fSel := (a_io_pin div $A)*4 ;
      shift := (a_io_pin mod $A)*3 ;
      iof := Pointer(LongWord(Self.f_gpio)+fSel);
      case a_mode of
        INPUT  : iof^ := iof^ and ($FFFFFFFF - (7 shl shift));
        OUTPUT : iof^ := iof^ and ($FFFFFFFF - (7 shl shift)) or (1 shl shift);
      end;
    end;
    
    procedure T_IO_Port.SetBit(a_io_pin : byte);
    var
      iof : PLongWord;
    begin
      iof := Pointer(LongWord(Self.f_gpio) + GPSET + (a_io_pin shr 5) shl 2);
      iof^ := 1 shl a_io_pin;
    end;
    
    procedure T_IO_Port.ClearBit(a_io_pin : byte);
    var
      iof : PLongWord;
    begin
      iof := Pointer(LongWord(Self.f_gpio) + GPCLR + (a_io_pin shr 5) shl 2);
      iof^ := 1 shl a_io_pin;
    end;
    
    function T_IO_Port.GetBit(a_io_pin : byte):boolean;
    var
      iof : PLongWord;
    begin
      result := true;
      iof := Pointer(LongWord(Self.f_gpio) + GPLEV + (a_io_pin shr 5) shl 2);
      if (iof^ and (1 shl a_io_pin)) = 0 then result := false;
    end;
    end.

---

_Source: [https://wiki.freepascal.org/Raspberry_Pi_%3A_BerryClip](https://web.archive.org/web/20211129145743/https://wiki.freepascal.org/Raspberry_Pi_%3A_BerryClip)_
