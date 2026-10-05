# Arduino

│ **[Deutsch (de)](</Arduino/de> "Arduino/de")** │  **English (en)** │  **[español (es)](</Arduino/es> "Arduino/es")** │    
****

[![Arduino Logo.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/Arduino_Logo.svg/50px-Arduino_Logo.svg.png)](</File:Arduino_Logo.svg>)

This article applies to [Arduino boards](</Category:Arduino> "Category:Arduino") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Many [Arduino](<https://en.wikipedia.org/wiki/Arduino>) projects have the need to send data to a PC running a program for data aquisition and visualisation. Lazarus is highly suitable for developing such programs, i.e. to build the PC-sided counterpart of an Arduino project. 

For Lazarus developers Arduino opens a whole new world of smart physical sensors and actuators for monitoring, control and data acquisition. 

The fpc compiler is even capable to compile programs for Arduino boards itself. 

  
[![Arduino Uno R3.png](https://wiki.freepascal.org/images/6/6d/Arduino_Uno_R3.png)](</File:Arduino_Uno_R3.png>)

## Contents

  * 1 FPC on Arduino
  * 2 Communication between a Lazarus application and Arduino
    * 2.1 One-way serial communication example using Synaser
      * 2.1.1 Lazarus side serial application
        * 2.1.1.1 Minimal Lazarus Synaser serial example
      * 2.1.2 Arduino side serial application
        * 2.1.2.1 Minimal Arduino C sketch serial example
        * 2.1.2.2 Minimal Arduino mikroPascal serial example
        * 2.1.2.3 Minimal Arduino E-Lab AvrCo serial example
        * 2.1.2.4 Minimal Arduino FPC serial example
    * 2.2 Bidirectional serial communication example using SDPO
      * 2.2.1 Lazarus Side of Bidirectional SDPO serial example
      * 2.2.2 Arduino side of Bidirectional serial example application
      * 2.2.3 Connecting the Two Sides
    * 2.3 Ethernet communication
      * 2.3.1 Lazarus side ethernet application
      * 2.3.2 Arduino side ethernet application
    * 2.4 Wiki Resources
    * 2.5 External Resources



# FPC on Arduino

Currently (2017) most Arduino boards carry microcontrollers of the AVR family (Atmega). An other popular arduino board carries a STM32 (ARM Cortex). For both microcontroller families there are fpc compilers available. Programming Arduino with FPC of course doesn't mean to use the arduino ecosystem with just a different language. Nevertheless arduino libs (C++) can be used in fpc programms, but mixing languages obviously is an advanced topic. While compilation the fpc creates a .hex-file (binary), this can be loaded to the microcontroller, arduino board respectively, using a programming software by choice. AVRDude e.g. also supports program upload using the Arduino bootloader. 

There are further wiki pages for programing AVR microcontrollers: [AVR](<AVR.md> "AVR"), [AVR Programming](<AVR_Programming.md> "AVR Programming")

There is also a tutorial on how to use Lazarus on a AtMega328p (Arduino Uno/Nano) in German language: [AVR Embedded Tutorial/de](</AVR_Embedded_Tutorial/de> "AVR Embedded Tutorial/de")

  


# Communication between a Lazarus application and Arduino

The following section describes how your Lazarus/FPC application can communicate with Arduino boards, no matter whether fpc or arduino is running on the arduino board. 

## One-way serial communication example using Synaser

Here we show how to communicate with your board through serial communication channel. 

### Lazarus side serial application

There are many ways for [serial](<Hardware_Access.md> "Hardware Access") communication in Lazarus and FPC. 

#### Minimal Lazarus Synaser serial example

We will use [Synaser](<http://synapse.ararat.cz/doc/help/synaser.html>) from [Synapse library](<http://www.ararat.cz/synapse/doku.php>) in this example. Create Lazarus application with one form and two buttons. Include Synaser unit and add [Synapse package](<Synapse.md> "Synapse") as a new requirement to your project (you do this in Project Inspector). In OnClick event of both buttons put something like this: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      ser: TBlockSerial;
    begin
      ser := TBlockSerial.Create;
      try
        ser.Connect('COM1'); // write here Arduino COM port number (on linux it's something like '/dev/ttyUSB0')
        Sleep(250);
        ser.config(9600, 8, 'N', SB1, False, False);
        ser.SendString('on'); // button 2 should have 'off' here
      finally
        ser.free;
      end;
    end;
    

### Arduino side serial application

You can program your Arduino in several languages. 

#### Minimal Arduino C sketch serial example

You can follow [this](<http://www.chrisbrand.co.za/2013/06/30/arduino-serial-port/>) nice step by step tutorial. In short, you need to add a led with proper resistor to your input pin 13, compile this sketch in your [Arduino IDE](<https://www.arduino.cc/en/Main/Software>), and download it to your board: 
    
    
    int led = 13; // Pin 13
    
    void setup()
    {
        pinMode(led, OUTPUT); // Set pin 13 as digital out
    
        // Start up serial connection
        Serial.begin(9600); // baud rate
        Serial.flush();
    }
    
    void loop()
    {
        String input = "";
    
        // Read any serial input
        while (Serial.available() > 0)
        {
            input += (char) Serial.read(); // Read in one char at a time
            delay(5); // Delay for 5 ms so the next char has time to be received
    
            if (input == "on")
            {
                digitalWrite(led, HIGH); // on
            }
            else if (input == "off")
            {
                digitalWrite(led, LOW); // off
            }
        }
    }
    

#### Minimal Arduino mikroPascal serial example

[mikroPascal for AVR](<https://www.mikroe.com/mikropascal/>) can be used to develop Arduino programs. TBD. 

#### Minimal Arduino E-Lab AvrCo serial example

[E-Lab AvrCo Multitasking Pascal](<http://e-lab.de/AVRco/index_en.html>) can be used to develop Arduino programs. TBD. 

#### Minimal Arduino FPC serial example

Yes, your Arduino 8-bit AVR board [can be programmed with FPC](<AVR_Programming.md> "AVR Programming") too. TBD. 

## Bidirectional serial communication example using SDPO

The single directional example is based on synaser, which is not a visual component. SDPO provides a visual wrapper around Synaser, and some people may find this method easier. The following example requires the SDPO package (currently 0.4.0) to be installed in Lazarus. There are other tutorials to show you how to install package. If you cannot find the SDPO component on your component palette, this example will not work. 

#### Lazarus Side of Bidirectional SDPO serial example

Open a new project, and add the following to a form: (Names are the default names) 

  1. the SDPO port (under the SDPO tab after you have installed it).
  2. four buttons. Caption as follows: Button1 SEND; Button2 CLEAR; Button3 UP; Button4 DOWN;
  3. a memo. Used to display what is sent and received.
  4. a panel. Used to display which number is being sent.



Configure the Sdposerial1 properties to match the Arduino with the object inspector. 

  * Baudrate = br__9600;
  * Device = YourSerialPort ( mine is /dev/ttyUSB0 ). This is probably the same port you use to program your Arduino.
  * FlowControl = fcNone;
  * Parity = pNone;
  * StopBits = sbOne;
  * SynSer = (TBlockSerial); (notice this from the previous example?)
  * Tag = 0;



Using the code window, find the spot under var where it defines **Form1: Tform;** , and add the next three variables: 
    
    
    var
      Form1: TForm1;
      incomingstring: String;
      completestring: String;
      messagenum: integer;
    

Add the following events (code is shown below) 

  * OnCreate for Form1
  * OnDestroy for Form1
  * OnRxData for Sdposerial1
  * OnClick for each button (4 events)



In the empty code blocks for each event, add the following code. 
    
    
    procedure TForm1.SdpoSerial1RxData(Sender: TObject);
    begin
      // add whatever is in the serial buffer to the incoming string
      incomingstring := incomingstring + SdpoSerial1.ReadData;
      // arduino terminates a serial.println transmission with a CR (#13) and a LF (#10).
      // check to see if our incming string is terminated by a linefeed
      if (incomingstring[Length(incomingstring)] = #10 ) then
        begin
          // the string ends a linefeed, so the whole string from the arduino is here.
          // remove the extra two characters before adding the incoming string to the memo (adds extra lines if you dont)
          completestring := LeftStr(incomingstring,Length(incomingstring)-2);  // trim two places
          Memo1.Lines.add(completestring);  // echo the new string to the memo
          incomingstring := '';             // clear the incoming string so we can start again
        end;
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      incomingstring := '';
      SdpoSerial1.open;
      SdpoSerial1.Active := true;
    end;
    
    procedure TForm1.Button3Click(Sender: TObject);
    // request to go higher
    begin
      // check to see if we have reached the upper limit
      if (messagenum < 12) then
        begin
          messagenum := messagenum + 1; // increase the number
          panel1.caption := inttostr(messagenum); // display the new number
        end;
      // if we have reached the limit, this won't do anything
    end;
    
    procedure TForm1.Button2Click(Sender: TObject);
    begin
      memo1.lines.clear;  // clear the memo
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    // send a request to the arduino
    begin
      memo1.lines.add(panel1.caption);  // echo the request to the memo
      SdpoSerial1.WriteData(panel1.caption + #13#10);
    end;
    
    procedure TForm1.Button4Click(Sender: TObject);
    // request to go lower
    begin
      // check to see if we have reached the lower limit
      if (messagenum > 0) then
        begin
          messagenum := messagenum - 1; // decrease the number
          panel1.caption := inttostr(messagenum); // display the new number
        end;
      // if we have reached the limit, this won't do anything
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      SdpoSerial1.Active := false;
      SdpoSerial1.close;
    end;
    

Test the program, and verify that it compiles. 

### Arduino side of Bidirectional serial example application

The following code has been tested on a Arduino Mega 2560 clone board. Feel free to alter it as you see fit. It is provided as one block to avoid adding your own strings. 
    
    
    //** This program is to demo simple bidirectional serial communication
    //** between an Arduino a computer running Lazarus.  This is one side 
    //** of the tutorial.  See https://wiki.freepascal.org/Arduino for more
    //** details.  Tested on Linux Mint with off-brand Arduino mega 2560.  
    //** Program draws heavily on the work of others.  Use program freely, 
    //** but at your own risk.
    
    char incoming;
    String textnum;
    int number;
    String txt[12];
    
    void setup()
    {
        // Start up serial connection
        Serial.begin(9600); // baud rate
        Serial.flush();
        delay(1000);  // give the port time to work
        txt[ 0] = "You asked for message 0.";
        txt[ 1] = "You asked for message 1.";
        txt[ 2] = "You asked for message 2.";
        txt[ 3] = "You asked for message 3.";
        txt[ 4] = "You asked for message 4.";
        txt[ 5] = "You asked for message 5.";
        txt[ 6] = "You asked for message 6.";
        txt[ 7] = "You asked for message 7.";
        txt[ 8] = "You asked for message 8.";
        txt[ 9] = "You asked for message 9.";
        txt[10] = "You asked for message 10.";
        txt[11] = "You asked for message 11.";
        txt[12] = "ERROR - SOMETHING WENT WRONG"; 	
    }
    
    void loop()
    {
        textnum = "";  // clear the number
        number = 12;   // set the number to give the error message
    
        // Read any serial input
        while (Serial.available() > 0)
        {
          incoming = (char) Serial.read(); // Read in one char
          if (isdigit(incoming)) textnum += incoming; 
            delay(50); // Delay for 50 ms so the next char has time to be received
        }
    
        if (textnum != "")
          {
            number = String(textnum).toInt();  // convert the incoming string to a number
            Serial.println(txt[number]);    // send the appropriate string back
          }
        
    
    }
    

Compile and upload this code to your board. You should be able to test the program by opening the Serial Monitor in Arduino, and sending a number from 0 to 32 to the Arduino. It should respond by "insulting" you with the appropriate numbered insult. No error checking is provided, so keep your requests in range. 

### Connecting the Two Sides

Serial Ports cannot be opened by more than one program at a time - particularly when a program grabs the port and hogs it like this simple example does. Make sure you close the Arduino Serial Monitor before attempting to run the Lazarus program. Your Arduino should be plugged in and powered up. If you are working under Linux, you may need to run the program as a superuser, or you may need to add your user to the dialout group. In any case, once the Arduino is up and running, you can run the program. It must be able to connect with the Arduino when it creates the main window, and it will hog the port until you properly close the application. Depending on your computer, sleeping or hibernating may mess up your communications. 

## Ethernet communication

Here we show how to communicate with your board through ethernet communication channel. 

### Lazarus side ethernet application

TBD 

### Arduino side ethernet application

TBD 

## Wiki Resources

  * [Embedded Portal](<Portal_Embedded.md> "Portal:Embedded")
  * [AVR](<AVR.md> "AVR")
  * [AVR Programming](<AVR_Programming.md> "AVR Programming")



## External Resources

  * [Forum topic that started this article](<http://forum.lazarus.freepascal.org/index.php?topic=36885>)
  * [Raspberry Pi Lazarus application talking to Arduino over serial](<https://bigdanzblog.wordpress.com/2014/11/07/serial-communications-usb-between-a-raspberry-pi-and-an-arduino-using-lazarus-free-pascal/>)
  * [FPC application talking to ESP8266 over TCP](<https://bigdanzblog.wordpress.com/2016/02/14/using-lazarus-free-pascal-to-communicate-with-esp8266/>)
  * [FPC application talking to Arduino Nano over TCP](<https://bigdanzblog.wordpress.com/2014/11/19/tcp-communications-between-a-pc-and-an-arduino-nano-using-lazarus-free-pascal/>)
  * [Arduino projects](<https://www.allaboutcircuits.com/projects/category/arduino/>)

---

_Source: [https://wiki.freepascal.org/Arduino](https://web.archive.org/web/20250124214554/https://wiki.freepascal.org/Arduino)_
