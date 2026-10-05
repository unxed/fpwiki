# Lazarus directory permissions on Linux and Unix

│ **English (en)** │

### Common misconceptions

When you install Lazarus on a *nix system ([Linux](<Portal_Linux.md> "Portal:Linux"), [FreeBSD](<Portal_FreeBSD.md> "Portal:FreeBSD"), [macOS](<Portal_Mac.md> "Portal:Mac")), using your distro's package system, Lazarus binaries and source files are installed in a location where typically only root has write access. 

Somehow this fact has lead to the misconception that a user without root privileges cannot rebuild Lazarus (which is done when you install new packages), and therefor one would need to change the Lazarus directory permissions.  
This, however is _**definitely not**_ true. 

When you rebuild Lazarus in this scenario, Lazarus will detect that it does not have write access to the place where the Lazarus binary is installed. It will then build the new Lazarus in a subfolder of the [primary config path](<pcp.md> "pcp"). When the IDE has finished building the new Lazarus binary, it will launch [startlazarus](<startlazarus.md> "startlazarus"). The startlazarus application will detect that there is a more recent lazarus binary somewhere in your primary config path and it will launch that binary instead of the original one 

This is also the reason why it is better to start Lazarus by means of the `startlazarus` application. 

#### Possible problems when trying to rebuild Lazarus in this scenario

When rebuiling the IDE, you should **not** select the "Clean all" options in the "Configure Build Lazarus" dialog, because this option will try to delete (amongst others) all of Lazarus' .ppu files, to which you do not have write access. 

### If you want to modify the Lazarus source code

Users that want to be able to modify the Lazarus source code (e.g. for fixing bugs and helping us improving Lazarus) should install the Lazarus sources in a subfolder of their home directory and build Lazarus from source (see [getting](<Getting_Lazarus.md> "Getting Lazarus") and [compiling an running Lazarus](<Getting_Lazarus.md> "Getting Lazarus")). 

This is a common *nix practice.

---

_Source: [https://wiki.freepascal.org/Lazarus_directory_permissions_on_Linux_and_Unix](https://web.archive.org/web/20250219110456/https://wiki.freepascal.org/Lazarus_directory_permissions_on_Linux_and_Unix)_
