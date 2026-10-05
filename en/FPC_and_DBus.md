# FPC and DBus

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Documentation
    * 2.1 Startup
    * 2.2 Shutdown
    * 2.3 Sending a Signal
    * 2.4 Receiving a Signal
    * 2.5 Calling a Method
    * 2.6 Responding to Method calls
    * 2.7 Properties
  * 3 Authors
  * 4 License
  * 5 Status
  * 6 Download
    * 6.1 Installed by default
    * 6.2 Older versions
    * 6.3 Pascal components
  * 7 Bug Reporting
  * 8 Change Log
  * 9 Help
  * 10 See also



## Overview

DBUS is a GNU library that implements inter-process communication. It's the default library of this kind for GNU applications. It can often be found on Linux machines. 

This library provides a simple way for applications to talk to one another, using a bus sytem. These headers are the translation of the low-level C API for DBUS. In the future it is desirable that these headers be wrapped around a class to make their use easier. 

## Documentation

Independently from what you want to achieve, a lot of the code for a program that uses DBUS will be very similar. This is the startup and finalization code. 

### Startup

On startup you need to connect to the bus. Usually there is a system and a session bus. Depending on the configuration of the system, you may not have access to the system bus. Next you need to request a name on the bus. Below is some simple startup code. 
    
    
    var
      err: DBusError;
      conn: PDBusConnection;
      ret: cint;
    begin
      { Initializes the error handling }
      dbus_error_init(@err);
      
      { Connection }
      conn := dbus_bus_get(DBUS_BUS_SESSION, @err);
    
      if dbus_error_is_set(@err) <> 0 then
      begin
        WriteLn('Connection Error: ' + err.message);
        dbus_error_free(@err);
      end;
      
      if conn = nil then Exit;
      
      { Request the name of the bus }
      ret := dbus_bus_request_name(conn, 'test.method.server', DBUS_NAME_FLAG_REPLACE_EXISTING, @err);
    
      if dbus_error_is_set(@err) <> 0 then
      begin
        WriteLn('Name Error: ' + err.message);
        dbus_error_free(@err);
      end;
      
      if ret <> DBUS_REQUEST_NAME_REPLY_PRIMARY_OWNER then Exit;
    
      ....
    

### Shutdown

When you no longer need the bus, you should close the connection. A good place to do this in most cases is right before the application terminates. The method used to do this depends on whether the connection is shared or private. Private connections are created using dbus_connection_open_private() or dbus_bus_get_private() and are closed using dbus_connection_close(); 
    
    
      { Finalization }
      dbus_connection_close(conn);
    

Shared connections, created using dbus_connection_open() or dbus_bus_get() are owned by libdbus and applications should only unref them, never close them. Most connections are shared connections. 
    
    
      { Finalization }
      dbus_connection_unref(conn);
    

### Sending a Signal

Signals are broadcasts from a DBus object to any client applications that require the information that it carries. A signal message contains the name of the interface that specifies the signal; the name of the signal; the bus name of the process sending the signal; and any arguments. 

The originating application first creates a signal message with dbus_message_new_signal(), then appends any arguments before sending with dbus_connection_send(). 
    
    
    procedure SendSignalExample;
    var
      DBusSerial: cardinal;
      err: DBusError;
      conn: PDBusConnection;
      ret: cint;
      msg: PDBusMessage;
      args: DBusMessageIter;
      SignalData: PChar;
    begin
      DBusSerial := 0;
      SignalData := 'This string is sent with the signal';
    
      // initialise the error value
      dbus_error_init(@err);
    
      // connect to the DBUS system bus and check for errors
      conn := dbus_bus_get(DBUS_BUS_SESSION, @err);
      if (0 <> dbus_error_is_set(@err)) then
      begin
        writeln('Connection Error :', err.message);
        dbus_error_free(@err);
      end;
      if conn = nil then
        exit;
    
      // request a name on the bus and check for errors
      ret := dbus_bus_request_name(conn, 'test.signal.source',
        DBUS_NAME_FLAG_REPLACE_EXISTING, @err);
      if (0 <> dbus_error_is_set(@err)) then
      begin
        writeln('Error: ', err.message);
        dbus_error_free(@err);
      end;
      if ret <> DBUS_REQUEST_NAME_REPLY_PRIMARY_OWNER then
        exit;
    
      // create a signal and check for errors
      msg := dbus_message_new_signal('/test/signal/Object', // object name
        'test.signal.Type',     // interface name
        'TestSignal');          // signal name
      if msg = nil then
      begin
        writeln('msg is nil');
        exit;
      end;
      // initialise argument payload
      dbus_message_iter_init_append(msg, @args);
    
      // append arguments onto signal
      if (0 = dbus_message_iter_append_basic(@args, DBUS_TYPE_STRING, @SignalData)) then
      begin
        writeln('Out of memory(1)');
        exit;
      end;
    
      // send the message and flush the connection
      if (0 = dbus_connection_send(conn, msg, @DBusSerial)) then
      begin
        writeln('Out of memory(2)');
        exit;
      end;
      dbus_connection_flush(conn);
    
      // free the message and finish using the connection
      dbus_message_unref(msg);
      dbus_connection_unref(conn);
    end;
    

### Receiving a Signal

In order to receive a signal a client application needs to register a "match rule" with DBus.The bus daemon examines the signal and identifies processes which are interested in it. It then sends the signal message to these processes. 
    
    
      // receive signals from interface "test.signal.Type"
      dbus_bus_add_match(conn, 'type=''signal'',interface=''test.signal.Type''', @err);
      dbus_connection_flush(conn);
      if (0<>dbus_error_is_set(@err)) then
      begin
        writeln('Match error: ', err.message);
        exit;
      end;
    

### Calling a Method

Client applications call methods to instruct a DBus object to execute a procedure or function. Methods may require input parameters and may return output data. 

This example connects to the System Bus and then calls the GetNetworkInterfaceNameByIndex() method of the org.freedesktop.Avahi.Server interface. The reply includes a string value which contains the network interface name. 
    
    
    procedure CallMethodExample;
    var
      err: DBusError;
      conn: PDBusConnection;
      msg, replymsg: PDBusMessage;
      args, replyargs: DBusMessageIter;
      Value: Int32;
      interfacename: PChar;
    begin
      //initialise parameter value for example
      Value := 1;
    
      // initialise the error value
      dbus_error_init(@err);
    
      // connect to the DBUS system bus and check for errors
      conn := dbus_bus_get(DBUS_BUS_SYSTEM, @err);
      if (0 <> dbus_error_is_set(@err)) then
      begin
        writeln('Connection Error :', err.message);
        dbus_error_free(@err);
      end;
      if conn = nil then
        exit;
    
      // create message and check for errors
      msg := dbus_message_new_method_call('org.freedesktop.Avahi', '/',
        'org.freedesktop.Avahi.Server', 'GetNetworkInterfaceNameByIndex');
      if (msg = nil) then
      begin
        WriteLn('Could not construct message: ' + err.message);
        dbus_error_free(@err);
      end;
    
      // initialise parameter list for appending
      dbus_message_iter_init_append(msg, @args);
    
      // add parameter argument
      if (0 = dbus_message_iter_append_basic(@args, DBUS_TYPE_INT32, @Value)) then
      begin
        writeln('Out Of Memory!');
        exit;
      end;
    
      // send message and block while waiting for reply
      replymsg := dbus_connection_send_with_reply_and_block(conn, msg, 1000, @err);
    
      if (dbus_error_is_set(@err) <> 0) then
      begin
        WriteLn('Error waiting for reply: ' + err.message);
        dbus_error_free(@err);
      end;
    
      // initialise reply argument list then check type of return value
      // before retrieving value
      if (dbus_message_iter_init(replymsg, @replyargs) = 0) then
        writeln('No return value')
      else if (DBUS_TYPE_STRING <> dbus_message_iter_get_arg_type(@replyargs)) then
        Writeln('Return value is not a string')
      else
        dbus_message_iter_get_basic(@replyargs, @interfacename);
      if (interfacename <> nil) then
        Writeln(interfacename);
    
      // free the messages and finish using the connection
      dbus_message_unref(replymsg);
      dbus_message_unref(msg);
      dbus_connection_unref(conn);
    end;
    

This example uses dbus_connection_send_with_reply_and_block() to call the method and retrieve the return message. This is the simplest way of retrieving the reply but will cause problems if the method may take some time to complete. Two other DBus functions exist which can be used in such situations: dbus_connection_send() and dbus_connection_send_with_reply(). The first of these requires code which waits for incoming messages and intercepts the reply, while the second sends a message and registers a [callback](<callback.md> "callback") which will be executed when a reply is received. 

### Responding to Method calls

### Properties

A DBus object may also have properties which can be read and, optionally, set. Properties are accessed by calling the Get method of the org.freedesktop.DBus.Properties interface. 
    
    
      // create message and check for errors
      msg := dbus_message_new_method_call('org.freedesktop.NetworkManager',
        '/org/freedesktop/NetworkManager', 'org.freedesktop.DBus.Properties', 'Get');
      if (msg = nil) then
      begin
        WriteLn('Could not construct message: ' + err.message);
        dbus_error_free(@err);
      end;
    
      v1 := 'org.freedesktop.NetworkManager';
      v2 := 'WirelessEnabled';
    
      dbus_message_iter_init_append(msg, @args);
      if (0=dbus_message_iter_append_basic(@args, DBUS_TYPE_STRING, @v1)) then
      begin
        writeln('Out Of Memory!');
        exit;
      end;
      if (0=dbus_message_iter_append_basic(@args, DBUS_TYPE_STRING, @v2)) then
      begin
        writeln('Out Of Memory!');
        exit;
      end;
    

A call to org.freedesktop.DBus.Properties.Get returns a value of type DBUS_TYPE_VARIANT which points to another structure of type DBusMessageIter which contains the return values. This is accessed using dbus_message_iter_recurse(). 
    
    
      if (dbus_message_iter_init(replymsg, @replyargs) = 0) then
        writeln('No return value')
      else if (DBUS_TYPE_VARIANT = dbus_message_iter_get_arg_type(@replyargs)) then
      begin
        dbus_message_iter_recurse(@replyargs, @subargs);
        if (DBUS_TYPE_BOOLEAN <> dbus_message_iter_get_arg_type(@subargs)) then
          Writeln('Return value is not boolean')
        else
          dbus_message_iter_get_basic(@subargs, @dbusbool);
        if (dbusbool = 1) then
          writeln('TRUE')
        else
          writeln('FALSE');
      end;
    

## Authors

[Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")

  


## License

## Status

Status: Beta 

## Download

### Installed by default

The FPC package for DBUS resides in packages/[dbus](</index.php?title=dbus&action=edit&redlink=1> "dbus \(page does not exist\)") in the FPC (2.2.2+) packages tree. 

You can download a full FPC repository and it will be included. 

### Older versions

For older versions: you can download the subversion version of this project using this command: 
    
    
    svn checkout http://svn.freepascal.org/svn/fpc/trunk/packages/dbus dbus
    

### Pascal components

Michael Van Canneyt has written DBus components which can be found in dbuscomp.pp in the [source code](<https://www.freepascal.org/~michael/articles/dbus2/dbus-code.zip>) which accompanies one of his articles on DBus. The components provide a flying start to using the protocol with Pascal although they do not cover every aspect of using DBus (eg. handling Properties) and do contain some bugs: the code in procedure TDBusMessage.EndGet should be commented out because this makes it impossible to retrieve more than one value from a DBus message. 

## Bug Reporting

Tests are necessary to verify if all functions and structures are declared properly. Tests were done on Windows, FreeBSD and Linux operating systems. It is necessary to test if the modules work correctly in other operating system. 

You can post Bug Reports to the [regular bug tracker](<http://bugs.freepascal.org>) (make sure to choose the FPC project and select the Packages category) 

## Change Log

  * ??.??.06 DBus headers version 0.1


  1. Under construction


  * 03.11.2010 Updated DBus headers to version 1.2.16



## Help

Please send help requests to the Free Pascal mailling list or on apache-modules mailing list. 

## See also

  * [[1]](<http://www.matthew.ath.cx/misc/dbus>) \- A Tutorial about using the DBus C API
  * [[2]](<https://www.freepascal.org/~michael/articles/#dbus2>) \- An introduction to using DBus with FPC, with source code
  * [[3]](<https://blog.avichalp.me/an-introduction-to-dbus-e89ad93c874d>) \- An overview of DBus with a concise glossary of concepts

---

_Source: [https://wiki.freepascal.org/FPC_and_DBus](https://web.archive.org/web/20250126023656/https://wiki.freepascal.org/FPC_and_DBus)_
