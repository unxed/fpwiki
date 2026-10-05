# CGI Web Programming

│ **English (en)** │  **[español (es)](</CGI_Web_Programming/es> "CGI Web Programming/es")** │  **[français (fr)](</CGI_Web_Programming/fr> "CGI Web Programming/fr")** │  **[Bahasa Indonesia (id)](</CGI_Web_Programming/id> "CGI Web Programming/id")** │  **[русский (ru)](<../ru/CGI_Web_Programming.md> "CGI Web Programming/ru")** │  **[中文（中国大陆） (zh_CN)](</CGI_Web_Programming/zh_CN> "CGI Web Programming/zh CN")** │    
****

This article contains some information about basic CGI programs. Note that using one of the frameworks will probably be much more productive. 

## Contents

  * 1 About CGI
  * 2 About FreePascal CGI and how you can test your CGIs
  * 3 A very basic example
  * 4 A simple hitcounter
  * 5 Debugging CGI
  * 6 Existing CGI Frameworks



## About CGI

[**CGI** (Common Gateway Interface)](<http://en.wikipedia.org/wiki/Common_Gateway_Interface>) works simply as follows: 

We have two computers: 

  * Server Computer - This computer will store the hosted files (web pages, images and CGIs) and serve them using a web server.
  * Client Computer - Generally, this computer has a browser to request the files hosted in server computer.



You can use CGI only if your hosting web server has CGI support. After hosting your files, CGI can now be used normally as: **http://www.yourserver.com[.xx]/yourlogin/cgi-bin/cginame.exe**

**Notes:**

  * The CGI file doesn't need the **.exe** extension.
  * The CGI should be (obviously) compiled for the platform of the computer server (Linux, Windows...)



## About FreePascal CGI and how you can test your CGIs

A Free Pascal CGI executable is a normal executable program, like any program on your computer. Unlike the CGIs in Perl or Python, Free Pascal CGI isn't interpreted. It's an independently executable program. 

To test a CGI, you must have a **HTTP Server** with support for CGI installed on your PC. A good, and recommended [HTTP Server](<http://en.wikipedia.org/wiki/HTTP_Server>) is [Apache](<http://en.wikipedia.org/wiki/Apache_HTTP_Server>), available both for Windows and Linux. 

## A very basic example

Here is an example of a minimal CGI program to demonstrate... 

  1. Setting a cookie
  2. Outputting the content-type (ie make it put out legal text for HTTP)
  3. Reading Cookies
  4. Reading form data via GET
  5. Reading form data via POST


    
    
    program mini;
    
    uses dos;
    
    var
      a:string;
      c:char;
    begin
      // set a cookie (must come before content-type line below)
      // don't forget to change the expires date
      writeln('Set-cookie:widget=value; path=/; expires= Mon, 21-Mar-2005  18:37:00 GMT');
    
      // output legal http page
      writeln('Content-Type:text/html',#10#13);
    
      // demonstrate get cookies
      a:= GetEnv('HTTP_COOKIE');
      writeln('cookies:',a);
      
      // demonstrate GET result
      a:='';
      a:= GetEnv('QUERY_STRING');
      writeln('GET: ',a);
    	
      // demonstrate POST result 
      a:='';
      while not eof(input) do
      begin
        read(c);
        a:= a+c;
      end;	 
      writeln('POST: ',a);
    end.
    

## A simple hitcounter

The application **hitcounter** presents a simple hitcounter for your website. It is possible to enter blocklists in a configfile so robots and perhaps your own computer are excluded from the statistics. There are still a lot of improvements possible ofcourse like setting a cookie so you get only one hit per session, but it gives a good introduction into CGI programming. If you use the code and modify it, please send me a patch so I can update it. 

You can find the code here: [[1]](<http://sourceforge.net/p/modelbuilder/code/HEAD/tree/html/cgi/>)

## Debugging CGI

Because a CGI application does not write to screen, and is started by another process (the web server), debugging is more difficult than desktop applications. 

One solution is to let the CGI application start a debugger: When used in a program this unit will as part of the initialization launch a new process which opens a terminal (windows:cmd, linux:xterm) and starts the gdb debugger attaching to the calling proces. 

A short "sleep" makes sure gdb breaks the program while still initializing. 

It has been tested on 

  * Windows XP32 with apache 2.2 running as an application or as a service (does not work anymore is higher versions of Windows, see <http://forum.lazarus.freepascal.org/index.php/topic,21931.msg128849.html#msg128849>)
  * on ubuntu 10.04 x64 with Apache 2.2
  * on Debian unstable x64 with Apache 2 (Feb 2013)



Further instructions are in the unit. 
    
    
    unit SelfDebug;
    
    {
    Instructions:
    -Include unit anywhere in the program.
    -Change DEBUGGER constant to match your debugger (gdb, ddd,gdbtui...)
    -For Windows, if the program to debug is running as a service (e.g. CGI application
     from Apache running as a service), make sure the service is configured with
     "Interact with desktop" checked. Failing to do so, the debugger will be started
     without a console, break the program and hang waiting for input. Killing the debugger
     is your only option.
    -For Linux, if the program to debug is running as a different user (e.g. CGI application),
     run "xhost +" in a terminal to enable all users to connect to xserver.
     If needed, change DISPLAY to match your DISPLAY environment variable
    }
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils,Process;
    
    implementation
    
    const
      {$ifdef windows}
      DEBUGGER='C:\lazarus\mingw\bin\7.3\gdb.exe';
      {$endif}
      {$ifdef linux}
      DEBUGGER='gdb';
      DISPLAY=':0.0';
      {$endif}
      MSWAIT=2000;
    
    var
      AProcess: TProcess;
    
    initialization
    AProcess := TProcess.Create(nil);
    {$ifdef windows}
    AProcess.CommandLine := format('cmd /C START "Debugging %s" /WAIT "%s" "%s" %d"',[paramstr(0),debugger,paramstr(0),GetProcessID]);
    {$endif}
    {$ifdef linux}
    AProcess.CommandLine := format('xterm -display %s -T "Debugging %s" -e "%s" "%s" %d',[DISPLAY,paramstr(0),DEBUGGER,paramstr(0),GetProcessID]);
    {$endif}
    AProcess.Execute;
    sleep(MSWAIT);
    finalization
    AProcess.Free;
    end.
    

Source: <http://www.hu.freepascal.org/lists/fpc-pascal/2011-October/030752.html>

Other solutions are writing to a file or event log/syslog. 

## Existing CGI Frameworks

There are existing CGI units and frameworks that make working with CGI trivial (and allow one to set cookies, sessions, retrieve POST and GET). 

  * See [Brook_for_Free_Pascal](<Brook_for_Free_Pascal.md> "Brook for Free Pascal"), the perfect Free Pascal framework for your web applications.
  * See [FastPlaz](<http://www.fastplaz.com/>), Free Pascal framework for your web applications with MVC Style.
  * See [fcl-web](<fcl-web.md> "fcl-web"), an FPC and Lazarus (weblaz) framework
  * See [Powtils](<http://z505.com/powtils/idx.shtml>)
  * [ExtPascal](<http://extpascal.googlecode.com>) an Ext JS wrapper.
  * See [Fano Framework](<https://fanoframework.github.io/>), Web application framework for modern Pascal programming language. It is written in Free Pascal.

---

_Source: [https://wiki.freepascal.org/CGI_Web_Programming](https://web.archive.org/web/20250424124915/https://wiki.freepascal.org/CGI_Web_Programming)_
