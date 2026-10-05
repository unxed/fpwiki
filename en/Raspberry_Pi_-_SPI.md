# Raspberry Pi - SPI

│ **[Deutsch (de)](</Raspberry_Pi_-_SPI/de> "Raspberry Pi - SPI/de")** │  **English (en)** │    
****

## Contents

  * 1 Using the SPI interface on the Raspberry Pi with Free Pascal
  * 2 SPI on Linux
    * 2.1 Header
    * 2.2 Initialization
    * 2.3 Exit
    * 2.4 Read out data
    * 2.5 Send data
    * 2.6 SPI Transfer
    * 2.7 SPI Transmit
    * 2.8 SPI FastShift
    * 2.9 Dismissed!



## Using the SPI interface on the Raspberry Pi with Free Pascal

First of all: It is a good idea to use the pascalio Lib (<https://github.com/SAmeis/pascalio>) for the hardware components such as SPI, I2C and the GPIOs of the Raspberry Pi. 

However, the pascalio is also very extensive and those who want to understand what is happening may be well served with this guide. 

## SPI on Linux

The Linux principle "Everything is a file" also applies to the SPI interface. However, the advantage that you can access the interface with simple file access is bought at a slower speed - usually uncritically - and that other programs can also access the same interface. 

On the Raspberry Pi (we're talking about version 3 with Raspbian Stretch, as of Oct 2017) there is an SPI interface that can address 2 devices: _/dev/spidev0.0_ and _/ dev / spidev0.1_ (Strictly speaking, there are several, but only one is led outside.) 

For the basics of SPI, master-slave, wiring see Wikipedia: <https://wikipedia.org/wiki/Serial_Peripheral_Interface>

### Header
    
    
    unit test_spi;
    
    {$ mode objfpc} {$ H +}
    
    interface
    
    uses
      Classes, SysUtils, BaseUnix;
    
    type
      Tspi = class
    
      procedure Init ();
      procedure Close ();
    
      function DataIn (var din; cnt: byte): integer;
      function DataOut (const dout; cnt: byte): integer;
    
      procedure TransferSync (const dout; var din; len: byte);
      procedure TransmitSync (const dout; len: byte);
      function FastShift (dout: byte): byte;
    
      private
        {private declarations}
      public
        {public declarations}
      end;
    
    const
      // SPI mode flags
    
      cSPI_CPHA = $ 01;
      cSPI_CPOL = $ 02;
    
      cSPI_MODE_0 = (0 or 0);
      cSPI_MODE_1 = (0 or SPI_CPHA);
      cSPI_MODE_2 = (SPI_CPOL or 0);
      cSPI_MODE_3 = (SPI_CPOL or SPI_CPHA);
    
      cSPI_CS_HIGH = $ 04;
      cSPI_LSB_FIRST = $ 08;
      cSPI_3WIRE = $ 10;
      cSPI_LOOP = $ 20;
      cSPI_NO_CS = $ 40;
      cSPI_READY = $ 80;
    
      cSPI_CTRL = $ 6B; // is the "magic byte"
    
      // control register
      // Read / Write + Size + MagicByte + Register
    
      cSPI_RD_MODE: uint32 = $ 80016B01;
      cSPI_WR_MODE: uint32 = $ 40016B01;
      cSPI_RD_LSB_FIRST: uint32 = $ 80016B02;
      cSPI_WR_LSB_FIRST: uint32 = $ 40016B02;
      cSPI_RD_BITS_PER_WORD: uint32 = $ 80016B03;
      cSPI_WR_BITS_PER_WORD: uint32 = $ 40016B03;
      cSPI_RD_MAX_SPEED_HZ: uint32 = $ 80046B04;
      cSPI_WR_MAX_SPEED_HZ: uint32 = $ 40046B04;
    
      cSPI_DEVICE = '/dev/spidev0.0'; // Device 0, ChipSelect 0
    // cSPI_DEVICE = '/dev/spidev0.1'; // Device 0, ChipSelect 1
      cSPI_SPEED = 1000000; // data rate in Hz
      cSPI_BITS = 8; // data bits
      cSPI_LSBF = 0; // LSB first = -1
      cSPI_MODE = cSPI_MODE_0;
    
    type
      spi_ioc_transfer = record // Control register required for FileIO
        tx_buf: uint64; //pointer;
        rx_buf: uint64; //pointer; // always 64-bit
        len: uint32; // Number of characters
        speed: uint32; // data rate in Hz
        delay: uint16; // delay CS in usec
        bpw: uint8; // bits per word
        csc: uint8; // CS change
        txn: uint8;
        rxn: uint8;
        pad: uint16;
      end; // a total of 32 bytes
    
    var
      spi: Tspi;
      spihnd: longint; // the filehandle for the interface
    
    implementation
    

### Initialization
    
    
    // initialize SPI
    
    procedure Tspi.Init ();
    var
      val8: byte;
      val32: longword;
    begin
      try
        spihnd: = FpOpen (cSPI_DEVICE, O_RdWr); // Open the interface for read / write
    
        if spihnd <> -1 then begin
          val8: = cSPI_MODE;
          FpIOCtl (spihnd, cSPI_WR_MODE, @ val8); // set fashion
          val8: = cSPI_BITS;
          FpIOCtl (spihnd, cSPI_WR_BITS_PER_WORD, @ val8); // set data bits per byte
          val8: = cSPI_LSBF; //-1
          FpIOCtl (spihnd, cSPI_WR_LSB_FIRST, @ val8); // Set MSB or LSB first
          val32: = cSPI_SPEED;
          FpIOCtl (spihnd, cSPI_WR_MAX_SPEED_HZ, @ val32); // set speed
        end;
      finally
    
      end;
    end;
    

The interface is opened when the program starts and the parameters are set. The parameters can also be changed later. For example, the clock rate can be set slow and then increased, which is necessary for some SD cards. 

_'Caution!'_ The clock rate is derived from the current system clock. If the clock changes, the clock rate changes. A clock rate of 1Mhz with 600MHz system clock with higher processor load and 1200MHz system clock goes up to 2MHz. 

And: The clock rate is derived from the system clock by a divider 2^n and is never exactly correct. If you specify 1MHz, the resulting rate is somewhere around 800kHz. 

### Exit
    
    
    // exit SPI
    
    procedure Tspi.Close ();
    begin
      if spihnd <> -1 then begin
        FpClose (spihnd);
      end;
    end;
    

Yes, at the end of the program we close the interface again. 

### Read out data
    
    
    // SPI Buffer Read
    
    function Tspi.DataIn (var din; cnt: byte): integer;
    begin
      if spihnd <> -1 then begin
        DataIn: = FpRead (spihnd, din, cnt);
      end;
    end;
    

Since "everything is a file" we can apply a simple read to the SPI interface. This outputs the clock for the specified number of bytes and reads the response of the device in an array _din_. 

### Send data
    
    
    // SPI Buffer Write
    
    function Tspi.DataOut (const dout; cnt: byte): integer;
    begin
      if spihnd <> -1 then begin
        DataOut: = FpWrite (spihnd, dout, cnt);
      end;
    end;
    

We can also send data with Write. The specified number of bytes is output from the _dout_ array. 

### SPI Transfer

However, most SPI devices require a slightly more complex interaction than simply sending and receiving. Often data is sent and received at the same time, and that overwhelms the capabilities of read and write. Here we need a transfer function. 
    
    
    // Send, receive SPI data
    
    procedure Tspi.TransferSync (const dout; var din; len: byte);
    var
      outbuf: array of byte;
      inbuf: array of byte;
      transfer: spi_ioc_transfer;
    begin
      if len> 0 then begin
        SetLength (outbuf, len);
        FillByte (outbuf [0], len, 0);
        Move (dout, outbuf [0], len);
    
        SetLength (inbuf, len);
        FillByte (inbuf [0], len, 0);
    
        FillByte (transfer, SizeOf (transfer), 0);
        transfer.tx_buf: = uint64 (@outbuf [0]);
        transfer.rx_buf: = uint64 (@inbuf [0]);
        transfer.len: = len;
        transfer.delay: = 0;
        transfer.speed: = cSPI_SPEED;
        transfer.bpw: = cSPI_BITS;
        transfer.csc: = 0;
    
        try
          FpIOCtl (spihnd, $ 40206B00, @transfer);
        finally
          Move (inbuf [0], din, len);
        end;
      end;
    end;
    

Now it becomes more difficult. To be able to send and receive at the same time, we need two buffers _outbuf_ and _inbuf_ , in which we mirror the data. We transfer their start addresses - absolutely as 64-bit values, even with a 32-bit OS - into the transfer register. The register also receives the number of bytes to be transferred and a few settings. 

Funnily enough, you can change the settings here every time, e.g. adjust the clock rate. 

We then transfer the transfer register with the settings to the file handler, which neatly scans the arrays and sends out and reads the data. The mystical _$ 40206B00_ is again a write ($ 40), followed by the number of bytes in the transfer register ($ 20 = 32 bytes), the magic byte ($ 6B, see above) and the control register ($ 00) the interface. Just leave it like that and don't fiddle with it. 

_'Disclaimer:'_ This works here and now on the Raspberry Pi 3 under Raspbian. This can look different on other Linux OS and hardware platforms, especially if the byte order of the addresses is different. 

What if I only want to receive data? Then I just pass an empty array. I can also specify the same array for _dout_ and _din_ , then the old array data will be overwritten with the new data. 

### SPI Transmit
    
    
    // Send SPI data without receiving
    
    procedure Tspi.TransmitSync (const dout; len: byte);
    var
      outbuf: array of byte;
      transfer: spi_ioc_transfer;
    begin
      if len> 0 then begin
        SetLength (outbuf, len);
        FillByte (outbuf [0], len, 0);
        Move (dout, outbuf [0], len);
    
        FillByte (transfer, SizeOf (transfer), 0);
        transfer.tx_buf: = uint64 (@outbuf [0]);
        transfer.rx_buf: = uint64 (@outbuf [0]); // same buffer
        transfer.len: = len;
        transfer.delay: = 0;
        transfer.speed: = cSPI_SPEED;
        transfer.bpw: = cSPI_BITS;
        transfer.csc: = 0;
    
        try
          FpIOCtl (spihnd, $ 40206B00, @transfer);
        finally
    
        end;
      end;
    end;
    

If data is only to be written, one can use the above DataOut, or this slimmed-down variant of the transfer routine. 

### SPI FastShift
    
    
    // Send, receive SPI single byte
    
    function Tspi.FastShift (dout: byte): byte;
    var
      din: byte;
      transfer: spi_ioc_transfer;
    begin
      din: = 0;
      FillByte (transfer, SizeOf (transfer), 0);
      transfer.tx_buf: = uint64 (@dout);
      transfer.rx_buf: = uint64 (@din);
      transfer.len: = 1;
      transfer.delay: = 0;
      transfer.speed: = cSPI_SPEED;
      transfer.bpw: = cSPI_BITS;
      transfer.csc: = 0;
    
      try
        FpIOCtl (spihnd, $ 40206B00, @transfer);
      finally
        FastShift: = din;
      end;
    end;
    

If only a single byte is sent and received, for example for a status query, this version is recommended. Here the result is not in a buffer, but is returned as a function value. 

### Dismissed!
    
    
    end.

---

_Source: [https://wiki.freepascal.org/Raspberry_Pi_-_SPI](https://web.archive.org/web/20250114092933/https://wiki.freepascal.org/Raspberry_Pi_-_SPI)_
