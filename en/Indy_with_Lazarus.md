# Indy with Lazarus

│ **[Deutsch (de)](</Indy_with_Lazarus/de> "Indy with Lazarus/de")** │  **English (en)** │  **[español (es)](</Indy_with_Lazarus/es> "Indy with Lazarus/es")** │  **[français (fr)](</Indy_with_Lazarus/fr> "Indy with Lazarus/fr")** │  **[日本語 (ja)](</Indy_with_Lazarus/ja> "Indy with Lazarus/ja")** │  **[한국어 (ko)](</Indy_with_Lazarus/ko> "Indy with Lazarus/ko")** │    
****

Indy is an open source client/server communications library that supports TCP/UDP/RAW sockets, as well as over 100 higher level protocols including SMTP, POP3, IMAP, NNTP, HTTP, FTP, and many more 

## Contents

  * 1 Indy for Lazarus
    * 1.1 Requirements
    * 1.2 Downloads / Links
    * 1.3 What works, what not
    * 1.4 Short term plans
    * 1.5 Problems
    * 1.6 Bugs
  * 2 Installation
    * 2.1 How to install
    * 2.2 How to install on Debian/Ubuntu
    * 2.3 How to install [2]
    * 2.4 How to install on Windows and Debian Linux, Indy 10.5 and above
    * 2.5 How to install (confirmed) on Windows - Indy 10.2
    * 2.6 Issue on Windows 64 bits
  * 3 Various tips
    * 3.1 Memory leaking issue
      * 3.1.1 Important developer note
    * 3.2 How to use non-blocking TCP sockets
  * 4 External links



## Indy for Lazarus

A new attempt at converting Indy is being undertaken by the Indy core team. FPC developer Marco van de Voort is working with Indy core to get Indy fully working with FPC/Lazarus. Specially Indy's JP Mugaas did a lot of good work in getting Indy10 to work with FPC. The native unixrtl backend is mostly his work too. 

Currently Indy10 is nearly fully working, and end-user usability has been improved a lot. Only special topics like OpenSSL and compression might need additional knowledge. 64-bit support hasn't entirely been validated either. 

Hint: I'm not very interested in support the Kylix side of Indy. (anything that uses unit libc). I ported it only as step up to native Unix RTL using Indy apps. 

### Requirements

FPC 2.0.4 is very close to being able to use Indy in all its facets, except for the server bug on FreeBSD/macOS (which requires 2.1.1) 

Since Indy10 now works for all major targets, we focus on this version. While conversion to Indy9 might be simpler for people, keep in mind that it only works on Win32 (and theoretically could be ported to Linux/x86). 

Therefore we recommend people to work with 10 as much as possible. 

(For indy9, you might need to remove the overriding of TMemoryStream.SetSize method (Marcov: ???) to get telnet components working, but Indy9 hasn't been tested in a while). 

The port is fairly stable, and JP Mugaas has done a great deal to improve the quality of the release, a set of makefiles inclusive. 

### Downloads / Links

Current snapshots (for die hards only) are at 

  * Indy9: 
    * [Indy9, includes demos](<http://www.stack.nl/~marcov/indy/indy9.zip>) and
  * Indy10: 
    * converted Indy10 demos : [converted Indy10 demos](<http://www.stack.nl/~marcov/demo10.zip>)
    * [Indy 10 website](<http://www.indyproject.org>) Recently, this archive started to include demos too.



### What works, what not

**Indy9** : 

  * Windows should largely work. Lots of demos ported.
  * Linux not or barely. No hope at non Linux/x86 (non-Kylix) targets.



**Indy10** : 

  * Windows and Unix: clients work and servers should work fine in principle.
  * The main work for native Unix RTL done. Servers Clients seem to work with FreeBSD and OS X



All: secure versions not tested yet. 

### Short term plans

  * Convert demos to demonstrate/test workings.
  * Fix bugs and enter them back into FPC or Indy RCS.



### Problems

  * Indy9 and Indy10 are mutually exclusive, which causes problems for deployment, even the default is a problem: 
    * Indy9 is more used and proven in the industry.
    * Indy10 is more portable, and better long term. However uptake seems to be low, and there is not much movement at Indy anymore.
  * How to deploy? Parts belong to Lazarus, parts to FPC.
  * Lot of pkgs and demos have paths in them. How to centrally fix them so that inexperienced users can build them?
  * Lazarus problems 
    * Transparency problems of icons
    * component tabs don't scroll, if you put a lot of components in one tab, they are not selectable. **workaround implemented** : JP Mugaas separated them into Indy clients A-M and N-Z etc. Also, meanwhile Lazarus changed to two rows of components per tab.
  * Indy demos (specially Indy10's) are not very suitable for multiplatform usage. Their setup is awfully windows specific, with spaces in paths, deep nested dirs, and useless baggage (C# code). A short working set of demos will have to be created.



### Bugs

  * Indy10 servers didn't work before due to an exception occurring during startup, which shut everything down. You need FPC 2.2(.0) or later to fix this (pthread_kill problem). This probably also affects Mac OS X.


  * During demo conversion, the bindings property is often wrongly converted. This can result in strange errors, specially because Indy seems to bind to the same port using both IPV4 and IPV6 at the same time.



Ubuntu 10.04 + Lazarus 0.9.28 + Indy 10(Server Component) -> launch & do nothing (just crash) 

Solution (bruce0829@yahoo.com.tw collect and organize data): 

(1).Lazarus Main menu -> Project -> Compiler Options -> Other Page , add the "-dUseCThreads" parameter. (2).force the IdTCPServer to work in Id_IPV4 mode. 
    
    
    procedure TForm1.FormActivate(Sender: TObject);
    var
      Binding: TIdSocketHandle;
    begin 
      //uses idGlobal
      //explicitly adding a Binding object prevents TIdTCPServer
      //from creating its own default IPv4 and IPv6 Binding objects
      //on the same listening IP/Port pair...
      Binding := IdTCPServer1.Bindings.Add;
      Binding.IPVersion := Id_IPv4; //optional: forces the Binding to work in Id_IPV4 mode.
      Binding.IP := '127.0.0.1';
      Binding.Port := 6501; //customization 
      IdTCPServer1.Active := True;  
    end;
    

(3)use TIdSync to Control your GUI in IdTCPServer's OnExecute() event. 

## Installation

### How to install

Comment from [RLebeau](</index.php?title=User:RLebeau&action=edit&redlink=1> "User:RLebeau \(page does not exist\)"): Indy is available in [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager"). 

Comment from [Matthijs](</User:Matthijs> "User:Matthijs"): So it is not perfect yet, but you want to try it. Here is how you install it in Lazarus. 

  * Open and compile indysystemlaz.lpk
  * Open and compile indycorelaz.lpk
  * Open and compile indyprotocolslaz.lpk
  * Open, compile and install dclindycorelaz.lpk
  * Open, compile and install dclindyprotocolslaz.lpk



Then rebuild Lazarus and you will see a 15 tab's with Indy components. 

### How to install on Debian/Ubuntu

Commands in Terminal: 
    
    
    cd indy-10.2.0.1/fpc
    chmod +x debian/rules
    touch README
    sudo apt-get install fakeroot
    dpkg-buildpackage -rfakeroot -uc
    cd ..
    sudo dpkg -i *.deb
    

Then install the package indylaz.lpk in Lazarus. 

### How to install [2]

If you couldn't install Indy for Lazarus with tips above, you can try this... 

From [Yahoo! Brazil, Lazarus Group](<http://br.groups.yahoo.com/group/lazarus-brasil/>) (User Everaldo) 

Version installed on Lazarus 0.9.24 using component indy-10.2.0.1. 

  * Unzip the component from file tar.gz (must be this file, the "zip file" does not work) in directory $(LazarusDir)\components;
  * Go to directory $(LazarusDir)\components\indy-10.2.0.1\ and to create inside them a directory named "saved";
  * Copy the content of the directory $(LazarusDir)\components\indy-10.2.0.1\lazarus\ to directory "saved" created above;
  * Copy the content of directory $(LazarusDir)\components\indy-10.2.0.1\fpc\ to directory $(LazarusDir)\components\indy-10.2.0.1\lazarus, and rewrite all files, ok;
  * Get the content of directory "saved" and copy inside of $(LazarusDir)\components\indy-10.2.0.1\lazarus and you can delete "saved" directory now;
  * Install the package laz.lpk (available in $(LazarusDir)\components\indy-10.2.0.1\lazarus\indy) of conventional way.



On LINUX the path is: /usr/lib/lazarus/components/indy-10.2.0.1/ 

### How to install on Windows and Debian Linux, Indy 10.5 and above

  * Need working Lazarus (and FPC) for this
  * Download from <http://www.indyproject.org/Sockets/Download/DevSnapshot.EN.aspx>. Or from the alternate web site for the updated and current versions: <http://indy.fulgan.com/>
  * unpack into a directory
  * copy the *.pas, *.lrs and *.inc of the \Lib\System, \Lib\Core, and \Lib\Protocols subdirectories into a new directory of your choosing
  * copy the indylaz.lpk in the \Lib directory to the directory you created above
  * from within Lazarus do a Package -> Open package file, open the indylaz.lpk
  * in the Package Options dialog, add the path to the directory you created above to the "Unit" field.
  * compile then install (will rebuild Lazarus). EDIT: Currently, due to a know bug in FPC, you must compile the Indy package TWICE before installing it.



In order to write & compile Indy10 applications, you need to put the path to the Indy directory in the "Other Unit files" textbox of every project that you write. You'll find "Other Unit files" under Project Options -> Compiler Options -> Paths. 

### How to install (confirmed) on Windows - Indy 10.2

For a lot of users, I'm sure that the information given above won't work, or will be somewhat confusing given the differences between versions and inconsistent explanations. 

The website points you to download the latest version from the snapshots page. This wiki tells you to copy a lot of files over into directories. For me, neither worked, and ended up making a mess of my Lazarus installations. 

  1. Firstly, the page I would retrieve this from is [here](<http://www.indyproject.org/Sockets/fpc/index.EN.aspx>). Other sites I tried had problems with some files inside the archive.
  2. When you open up the archive above, you will see there are folders: "fpc" and "lazarus". 
     1. You can copy the contents of "fpc" into: LAZARUS_DIR\fpc\2.6.0\source\packages\indylaz if you want to have things neat and tidy.
     2. The "lazarus" folder, you copy into LAZARUS_DIR\components\indylaz
     3. With both of these, make sure that there isn't a sub-directory inside the folders given. i.e. LAZARUS_DIR\components\indylaz\lazarus\
  3. Go into Lazarus and go to "Package" -> "Open package file" and point it to the "indylaz.lpk" inside the LAZARUS_DIR\components\indylaz directory.
  4. Once the package loads inside your project, click on the "options" button, which resembles an image of a parcel with a cog next to it.
  5. Click the "Compiler Options" on the left-hand side and Click on the ".." button next to "Other unit files (-Fu) (delimiter is semicolon)".



Select the "fpc" then "lazarus" folders you created above and click OK. Lazarus will sort out the relative path for you. Don't change it. 

  1. Click OK and compile and then direct angry bile towards the individual who has steered you wrong with the previous, unhelpful, instructions.



### Issue on Windows 64 bits

On Windows 64 bits, the compilation of the archive mentioned above will not work. This can be fixed by editing IdAntiFreeze.pas, replacing: 
    
    
     type
       TIdAntiFreeze = class(TIdAntiFreezeBase)
       public
         procedure Process; override;
       end;
    

by 
    
    
     type
       TIdAntiFreeze = class(TIdAntiFreezeBase)
       public
         {$IFDEF WIN32}
         procedure Process; override;
         {$ENDIF}
       end;
    

## Various tips

### Memory leaking issue

If you have memory leaks in your Indy application, that is because you use **IdStack** or **IdThread** units, or components that depend on them. It is intended by design, and in Delphi the leak reports are suppressed by registering the leaks with the memory manager. This feature is not available with Free Pascal, but you can remove the deliberate memory leaks. In file **IdCompilerDefines.inc** you should replace this line: 
    
    
     {.$DEFINE FREE_ON_FINAL}
     {$UNDEF FREE_ON_FINAL}
    

with this: 
    
    
     {$DEFINE FREE_ON_FINAL}
     {.$UNDEF FREE_ON_FINAL}
    

and recompile Lazarus and Indy. Beware that IdCompilerDefines.inc exists in 5 different directories and you need to make changes in all of them. 

![<translate> Warning: </translate>](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/OOjs_UI_icon_notice-destructive.svg/18px-OOjs_UI_icon_notice-destructive.svg.png) **Warning**|  The leaks are gone, but you should read the define related warnings in **IdStack.pas** and **IdThread.pas** units in order to avoid potential GPF situations. Read the developer note below.  
---|---  
  
#### Important developer note

The leaks are not a matter of not being able to free the objects (obviously, they can be, as seen when FREE_ON_FINAL is defined), but more a matter of unit dependency. The objects in question are shared with other units, and it has been noticed that sometimes units may not always be finalized in the order you are expecting, and that has been observed in the wild. So rather than freeing these objects by default, they are leaked instead, in case they are still accessed later. The leak is minor (only a few bytes), and only during unit finalization, which 9x% of the time will only be during application shutdown (unless you are using Indy in a DLL that gets unloaded dynamically during runtime). 

### How to use non-blocking TCP sockets

Advice from FPC developer Marco van de Voort: Indy can be used nonblocking if you regularly check for data on source: 
    
    
      fclient.IOHandler.CheckForDataOnSource(5);
    

and then never read more than InputBuffer.Size: 
    
    
      insiz:= IOHandler.InputBuffer.Size;
      IOHandler.ReadStream(strmbuf, insiz, false);
    

While Indy 10 does a lot with streams (due to its .NET heritage) you can workaround it by using a TCustomMemoryStream derivative that just points to your own buffers, with nearly no overhead. 

I myself use this for a set of client-servers that do work in threads, but use events to try to reach (relatively) low latency comms. 

## External links

  * [The Indy Project official website](<http://www.indyproject.org>)

---

_Source: [https://wiki.freepascal.org/Indy_with_Lazarus](https://web.archive.org/web/20250324160852/https://wiki.freepascal.org/Indy_with_Lazarus)_
