# QOTD

## Contents

  * 1 Summary
  * 2 History
  * 3 Accessing a QOTD server with Lazarus
  * 4 QOTD Servers



## Summary

  * Code example of using Synapse to query a public QOTD server.



## History

[A wiki article about QOTD](<http://en.wikipedia.org/wiki/QOTD>)

## Accessing a QOTD server with Lazarus

  * You need to download and compile the [Synapse package](<Synapse.md> "Synapse")
  * Put **tlntsend** in your Implementation/Uses clause
  * You will be using the QOTD port #17


    
    
    procedure DisplayQOTD;
    Var 
    MyClient:TTelnetSend;
    szQuote:String;
    
    begin
    MyClient := TTelNetSend.Create;
    With MyClient do
    begin
      TargetPort := '17';
      Timeout:=3000;
      TargetHost := 'alpha.mike-r.com';
    end;
    If MyClient.Login then
      begin
           szQuote := MyClient.RecvString;
           If szQuote = '' then
           begin
              MyClient.Send(LineEnding + LineEnding);
              szQuote := MyClient.RecvString;
           end;
      end
      else szQuote:='Could not retrieve quote';
    
    MyClient.Logout;
    ShowMessage(szQuote);
    FreeAndNil(MyClient.);
    end;
    

## QOTD Servers

Here are some public QOTD servers: 

  * qotd.nngn.net
  * qotd.atheistwisdom.com
  * djxmmx.net
  * alpha.mike-r.com



Windows users can install the 'Simple TCPIP services' and activate the Windows built-in QOTD server (localhost)

---

_Source: [https://wiki.freepascal.org/QOTD](https://web.archive.org/web/20250515162921/https://wiki.freepascal.org/QOTD)_
