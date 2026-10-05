# SimpleIPC Library

SimpleIPC are units for Freepascal that allow client and server to send interprocess communication messages between processes. SimpleIPC-Lib is a library (DLL) that allows SimpleIPC to be used in any program, written in any language. 

How does one send messages from a Freepascal program, to a Delphi program using SimpleIPC, or a program written in another (any) language, such as C++, GoLang, Rust, C, Python? The SimpleIPC **"library"** intends to solve this issue. It now allows you to use SimpleIPC not just for freepascal programs communicating with other freepascal programs, but for any program written in any language that can load/use a DLL. Simply load the SimpleIPC dll into your program (written in any language) and now you can send messages from any program written in any language, and receive messages to any program also written in any language. 

## Github Project Page

<https://github.com/z505/SimpleIPC-lib>

## Use Cases

This library allows all sorts of potential use cases, such as having a freepascal communicate to a delphi program, if you write some code in delphi but some code in freepascal... or, if you have a program or code in C++ that you found that do not want to port to freepascal, you can communicate with the C++ program back to a freepascal codebase. 

An example would be to create a plugin mechanism, that allows multiple programs in multiple programming languages, to communicate back to the main program. Therefore allowing any exe/elf to now load separate programs (plugins) that communicate back to the main program. Each plugin is a separate program, so if the plugin crashes it is in it's own safe separate process (an advantage over DLL plugin systems where the DLL is loaded into the same process). 

There are other use cases of course: many people have Delphi source code, that they want to use - but have trouble porting it to FPC - so one advantage of SimpleIPC Lib is you can load your delphi program, then communicate to the FPC program and send it messages, integers, strings - therefore reusing existing delphi code without having to port it to FPC. Just load two programs: one fpc program, one delphi program, and they can now communicate. Example: a help system written in FPC, which Delphi communicates with and sends messages to when you hit "F1" to find a help topic in your editor. Or vice versa: a help system written in Delphi, which freepascal communicates with and sends messages to. 

## Example

How would one send messages from freepascal to GoLang, as an example? The github project page has a demo showing exactly that. 

  * <https://github.com/z505/SimpleIPC-lib/tree/master/demo-golang>
  * <https://github.com/z505/SimpleIPC-lib/tree/master/demo-lazarus>

---

_Source: [https://wiki.freepascal.org/SimpleIPC_Library](https://web.archive.org/web/20250325102313/https://wiki.freepascal.org/SimpleIPC_Library)_
