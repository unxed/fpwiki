# Bluetooth

│ **English (en)** │

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Status
  * 6 Dependencies / System Requirements
    * 6.1 Linux
      * 6.1.1 Ubuntu/Debian
      * 6.1.2 Mandriva/Redhat
  * 7 Installation
  * 8 Examples
    * 8.1 Identifying reachable Bluetooth devices
    * 8.2 Communicating using RFCOMM
  * 9 ToDo: Usage/Tutorial/Reference



## About

The bluetoothlaz package provides bindings and functions to access bluetooth devices under various platforms. 

It contains examples, like accessing the Wii Remote. 

## Author

[Mattias Gaertner](</User:Mattias2> "User:Mattias2")

## License

LGPL (please contact the author if the LGPL doesn't work with your project licensing) 

## Download

The latest stable release can be found on the [Lazarus CCR Files page](<http://sourceforge.net/projects/lazarus-ccr/files/Bluetooth/>). 

## Status

**Alpha**

At the moment the package only supports Linux. Eventually it will support Windows, Mac OS X and other platforms and will get some platform independent Wrapper functions/classes. 

## Dependencies / System Requirements

### Linux

The BlueZ libraries plus their development files must be installed. 

#### Ubuntu/Debian

Install the **libbluetooth-dev** package: 
    
    
    sudo apt-get install libbluetooth-dev
    

#### Mandriva/Redhat

Install the **libbluez-devel** package. 

## Installation

Download and unpack the package to a directory of your choice. In the Lazarus IDE use Package / Open package file. A file dialog will appear. Choose the bluetooth/bluetoothlaz.lpk and open it. That's all. The IDE now knows the package. 

## Examples

There are two examples for the Wii Remote. One that demonstrates how to connect to a Wii Remote and shows the infrared sensors and one that demonstrates VR headtracking with the Wii Remote and the 3D package Asmoday. 

### Identifying reachable Bluetooth devices

This is a simple example that will identify one Bluetooth device in the vicinity. 

The code uses the following modules: 
    
    
     uses Bluetooth, ctypes, Sockets;
    

This part can be called from a button click to write Bluetooth information to the console: 
    
    
     procedure TForm1.FindBlueToothClick(Sender: TObject);
     var
      device_id, device_sock: cint;
      scan_info: array[0..127] of inquiry_info;
      scan_info_ptr: Pinquiry_info;
      found_devices: cint;
      DevName: array[0..255] of Char;
      PDevName: PCChar;
      RemoteName: array[0..255] of Char;
      PRemoteName: PCChar;
      i: Integer;
      timeout1: Integer = 5;
      timeout2: Integer = 5000;
     begin
      // get the id of the first bluetooth device.
      device_id := hci_get_route(nil);
      if (device_id < 0) then
        raise Exception.Create('FindBlueTooth: hci_get_route')
      else
        writeln('device_id = ',device_id);
     
      // create a socket to the device
      device_sock := hci_open_dev(device_id);
      if (device_sock < 0) then
        raise Exception.Create('FindBlueTooth: hci_open_dev')
      else
        writeln('device_sock = ',device_sock);
     
      // scan for bluetooth devices for 'timeout1' seconds
      scan_info_ptr:=@scan_info[0];
      FillByte(scan_info[0],SizeOf(inquiry_info)*128,0);
      found_devices := hci_inquiry_1(device_id, timeout1, 128, nil, @scan_info_ptr, IREQ_CACHE_FLUSH);
     
      writeln('found_devices (if any) = ',found_devices);
     
      if (found_devices < 0) then
        raise Exception.Create('FindBlueTooth: hci_inquiry')
      else
          begin
            PDevName:=@DevName[0];
            ba2str(@scan_info[0].bdaddr, PDevName);
            writeln('Bluetooth Device Address (bdaddr) DevName = ',PChar(PDevName));
     
            PRemoteName:=@RemoteName[0];
            // Read the remote name for 'timeout2' milliseconds
            if (hci_read_remote_name(device_sock,@scan_info[0].bdaddr,255,PRemoteName,timeout2) < 0) then
              writeln('No remote name found, check timeout.')
            else
              writeln('RemoteName = ',PChar(RemoteName));
          end;
     
      c_close(device_sock);
     end;
    

### Communicating using RFCOMM

RFCOMM is a simple set of transport protocols, made on top of the L2CAP protocol, providing emulated RS-232 serial ports . 

RFCOMM is sometimes called serial port emulation and the Bluetooth serial port profile is based on this protocol. RFCOMM provides a simple reliable data stream to the user, similar to TCP. Many Bluetooth applications use RFCOMM because of its widespread support and publicly available API on most operating systems. Applications that use a serial port to communicate can often be quickly ported to use RFCOMM. 

This example attempts to connect to a remote device identified by Bluetooth address 34:8A:7B:02:04:4D which is listening on channel 7 and sends a simple message if successful. It can be used with the Android example application [BluetoothChat](<https://android.googlesource.com/platform/development/+/eclair-passion-release/samples/BluetoothChat>). 
    
    
    procedure TForm1.ConnectRFCOMMClick(Sender: TObject);
    var
      loc_addr: sockaddr_rc;
      opt: longint;
      s, status: unixtype.cint;
      bd_addr: bdaddr_t;
      channel: byte;
      bt_addr: array[0..17] of char;
      bt_message: array[0..10] of char;
    begin
      opt := SizeOf(loc_addr);
    
      //remote device address
      bt_addr := '34:8A:7B:02:04:4D';
    
      //allocate socket
      s := fpsocket(AF_BLUETOOTH, SOCK_STREAM, BTPROTO_RFCOMM);
    
      //channel that destination device is listening on
      channel := 7;
    
      loc_addr.rc_family := AF_BLUETOOTH;
      loc_addr.rc_bdaddr := bd_addr;
      loc_addr.rc_channel := channel;
      str2ba(@bt_addr, @bd_addr);
      loc_addr.rc_bdaddr := bd_addr;
      writeln('Trying connection to ', bt_addr);
    
      status := fpconnect(s, @loc_addr, opt);
      writeln('channel: ', channel, '  result: ', status);
      if status = 0 then
      begin
        bt_message := 'Hello there';
        fpsend(s, @bt_message, 11, 0);
        // close socket after message is sent
        fpshutdown(s, 0);
      end;
    end;
    

## ToDo: Usage/Tutorial/Reference

The package is quite new and not yet complete. Documentation will be written, when some more platforms are supported and the API has stabilized. 

The HCI documentation for hci_* functions found in bluetooth.pas can be found in the BLUETOOTH SPECIFICATION PDF files linked at this [Wikipedia section](<http://en.wikipedia.org/wiki/Bluetooth#Specifications_and_features>), specifically at this: [Wikipedia reference](<http://en.wikipedia.org/wiki/Bluetooth#cite_note-bluetooth_specs-31>).

---

_Source: [https://wiki.freepascal.org/Bluetooth](https://web.archive.org/web/20220924204806/https://wiki.freepascal.org/Bluetooth)_
