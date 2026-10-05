# conSock

## Contents

  * 1 About
  * 2 Usage
    * 2.1 Frame structure
    * 2.2 Units
    * 2.3 Example
  * 3 Download



## About

conSock is a Lazarus library that implements TCP/IP connections, in Client-Server mode, using sockets. It is based on [Synapse](<Synapse.md> "Synapse"). Tested only in Windows. 

Author: Tito Hinostroza 

License: GPL v3 

## Usage

Using the library is easy because it presents objects that communicate using events. At the low level the objects are implemented using blocking sockets and threads. 

The connection model is similar to a Client-Server architecture, with the following rules: 

  * To implement a communication, there must be one Client and one Server part.
  * Client part must be set with the IP address of the Server part. Server doesn't need to be set.
  * Server part can connect only to one client. If there are several clients connecting to the same server, only the first will be accepted.
  * Once the connection is established, only client can start the communication. The server can respond only to a client request.
  * All communications are implemented using the same Frame structure.



Default IP for Client is 127.0.0.1. Default port for Client and Server is 80. 

### Frame structure

Library implements a Frame including: 

  * Header part: 9 bytes size
  * Data part: Optional. Can be as big as 16777215 bytes



Header part is small (fixed size 9 bytes) and has the following structure: 

  * 1 byte: Header ID. Header ID is always the hexadecimal value $0F, and is a way to identify the start of a frame.
  * 3 bytes: Data part size. The "Data part size" indicates the size of the "Data part".
  * 1 byte: Command. User can send any command in the frame field "Command". But the values $03 and $83 are reserved to internal dialog of the protocol.
  * 2 bytes: X parameter. Parameters X and Y can be used to send additional information for a command.
  * 2 bytes: Y parameter.



### Units

The library contains 3 units: 

  * conSockFrames.pas - Defines the protocol and the routines to code/decode the Frame packet.
  * conSockClient.pas - Defines the base client object TConSockClient.
  * conSockServer.pas - Defines the base server object TConSockServer.



To implement a basic connection, it's needed to create a client and server object. 

  * Client Part - Modelled with the object TConSockClient.
  * Server Part - Modelled with the object TConSockServer.



### Example

To implement a simple connection, it's needed to create a client part and a server part: 
    
    
      //Set client
      client := TConSockClient.Create('127.0.0.1');
      client.OnFrameReady := @clientFrameReady;
      client.Connect;
      
      //Set server
      Server := TConSockServer.Create;
      Server.OnFrameReady := @ServerFrameReady;
      Server.Connect;
    

Event OnFrameReady is triggered every time a Frame is received. 

The state of the connection can be read in the field client.state and in server.State. This state can be: 

  * cecCONNECTING
  * cecCONNECTED
  * cecSTOPPED
  * cecDEAD



To send data from the client, we can use the following instruction: 
    
    
      client.SendCommand($FF, 0, 0, sometext);
    

Where $FF represents some command, and "sometext" represents the data part of the command. We can use different commands number, except the values $03 and $83 that are reserved for the library. 

For a complete code, check the sample projects. 

## Download

GitHub: <https://github.com/t-edson/conSock>

---

_Source: [https://wiki.freepascal.org/conSock](https://web.archive.org/web/20230924211430/https://wiki.freepascal.org/conSock)_
