# Background

I am developing a new system that would use sockets to communicate between different part of programs. Thus I wanted to write a simple test socket example as a proof of concept and based on that decide what design would suit me best for particular application. 

Much of socket programming is required to connect to some existing internet service http/web, ftp or some other. If you wish to do that maybe you can take in consideration this tool (did not test it myself but I trust it would be very useful) - <http://wiki.lazarus.freepascal.org/lNet> This library is packed with ready made functions you can use. 

However, I am not planning to use web server or database server to connect to. I wish to have my own incoming server. So.. I have to decide on following: Use TCP or UDP sockets? You can learn about these two variants on many places, there is no need to discuss differences and suitability here. I jump to decision to make TCP socket for my application as I need somewhat constant "chat" between "client" and "server". 

Second thing I also take as decision is that there is two way communication between client and server. This is critical when making decisions as in some cases you may just with to "broadcast" something (and UDP would be better for that) but when making TCP you typically wish to get some sort of "confirmation" from the server (you are going to transmit some data from one side and receive from another side). 

This also (as experience shows) very quickly lead to two more considerations: 

  1. how messaging is going to be performed (basically developing the "protocol") 
  2. how you will manage connections (single, multiple, local, remote, intranet, internet) 



yet one more decision to make is - current wide use of IPv4 or contemplating IPv6 as in the near future we will be more and more forced to use IP6. However for some time IPv4 will be more than applicable to what I need (will mostly run this in intranet) 

Aside from socket issues, I am starting here with FPC 2.6. Why? Because I just dropped down from the Mars into FPC which proved nice for my application and I downloaded latest version ;) 

Why FPC? For many reasons. Pascal is not strange animal to me, I may be rusty using it but will figure my way around. 

Finally... I work on windows (have vista but that is somewhat irrelevant as I am looking for 32bit application) and mac (actually more on mac than on windows) but, I wish to use as much of same code on either system (as every programmer I am lazy). I currently do not use/have Linux box (mac unix is somewhat different), however, I am almost sure that examples I wrote has high chance of working with linux too. 

  


that is enough of bla bla on background and decisions! 

I do research... 

# Literature

FPC comes with extensive documentation <http://www.freepascal.org/docs.var> and most of the stuff that may interest me in sockets are here <http://www.freepascal.org/docs-html/rtl/sockets/index.html>

I have also hit other people experience: 

[http://community.freepascal.org:10000/bboards/message?message_id=701814&forum_id=24092](<http://community.freepascal.org:10000/bboards/message?message_id=701814&forum_id=24092>) [http://community.freepascal.org:10000/bboards/message?message_id=224499&forum_id=24092](<http://community.freepascal.org:10000/bboards/message?message_id=224499&forum_id=24092>)

and possibly one of most detailed descriptions <http://www.bastisoft.de/programmierung/pascal/pasinet.html>

# Where do I go

I have decided to start with the example from RTL manual... there are two critical functions when it comes to sockets - one is active (client side) which is Connect 

Exploring this leads to examples of client and server on: 

TBC

---

_Source: [https://wiki.freepascal.org/ljgww.fpc.sockets](https://web.archive.org/web/20190519180435/https://wiki.freepascal.org/ljgww.fpc.sockets)_
