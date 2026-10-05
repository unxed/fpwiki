# WCHTTPServer

## Contents

  * 1 What is WCHTTPServer
  * 2 Author
  * 3 Licence
  * 4 Download



## What is WCHTTPServer

WCHTTPServer is an HTTP/2 + HTTP 1.1 + WebSocket server written in Free Pascal.

[![WCHTTPServer logo](https://wiki.freepascal.org/images/e/ed/wclogo.png)](</File:wclogo.png> "WCHTTPServer logo")

The project builds on the [fcl-web](<fcl-web.md> "fcl-web") library and extends it to increase functionality: 

  * Client management using cookies (saving and maintaining sessions).
  * Saving information about clients and sessions in SQLite database.
  * Saving information about the latest requests and saving logs in SQLite database.
  * Multithreading preparation and execution of requests based on thread pools e.g. helpful classes to work with EventSources.
  * Client rankings based on the frequency of client requests.
  * Built-in support for gzip and deflate compression methods including decompression of client-side requests.
  * WebSocket protocol supported.
  * Ability to start the server both in HTTP/2 mode and in HTTP 1.1 mode.
  * Modified OpenSSL modules (added necessary TLS extensions) in order to create and maintain HTTP/2 connections.
  * Added the ability to save the master key and a random set of client data on the server-side (necessary for debugging TLS dumps using WireShark).
  * Works under both Windows and Linux.



## Author

  * Ilya Medvedkov



## Licence

  * GNU Lesser General Public License v2.1



## Download

  * [WCHTTPServer project on github](<https://github.com/iLya2IK/wchttpserver>)

---

_Source: [https://wiki.freepascal.org/WCHTTPServer](https://web.archive.org/web/20240121091049/https://wiki.freepascal.org/WCHTTPServer)_
