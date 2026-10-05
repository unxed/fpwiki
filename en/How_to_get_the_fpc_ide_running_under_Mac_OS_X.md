# How to get the fpc ide running under Mac OS X

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Legacy information - no longer current

The mouse does not work in revision version 2.0.4. From version 2.1.2 revision 6226 and possibly up the mouse works.  
  
Mandatory Requirements:  


  1. Apple developer tools (read XCode) and X11  
  

  2. The freepascal ide  
  
you need: compiler, rtl, (maybe base packages, fcl and extra packages. note: fcl has been moved into packages in version 2.1.2 revision 6655), fvision (i.e. fv) and ide.  
  
compile fv:  
  
`cd fv`  
`make clean all`  
  
compile the ide (there is no debugger library, just neglect the resulting warning):  
  
`cd ide`  
`make clean all`  
  
You may use fink (<http://www.finkproject.org/>) for the installation of freepascal.  
The package fpc installs all you need including the fv units and the ide.  
  




Optional Requirements:  
  
due to recent updates the next two steps (installing the vga font and make it accessible in xterm) are not mandatory any longer to get at least a partially working IDE, and consequently may be skipped. You will miss the nice fpc background, for example. (;-)  
  


  1. install the vga font for xterm  
  
download the vga font here: <http://home.earthlink.net/~us5zahns/enl/vga.pcf>  
  
for more docs on changing the Xterm font look here: <http://www.linux.org/docs/ldp/howto/Bash-Prompt-HOWTO/x606.html>  
  
copy the font to /usr/X11R6/lib/X11/fonts/misc/  
  
`mv your_download_dir/vga.pcf /usr/X11R6/lib/X11/fonts/misc/`  
  
check and set file permissions (you have to be root to do this):  
  
`cd /usr/X11R6/lib/X11/fonts/misc/`  
  
`chown root:wheel vga.pcf; chmod 444 vga.pcf`  
  
start an xterm in X11 and do:  
  
`cd /usr/X11R6/lib/X11/fonts/misc/`  
`mkfontdir`  
`xset fp rehash`  
  
Now you should be able to start xterm with the vga font by:  
  
`xterm -font vga -r`  
  
It is useful to define this as a customized command in X11 (Menu Applications -> Customize...)  
  

  2. set the terminal type to linux  
  
`export TERM=linux`  
  
or start xterm with  
  
`xterm -font vga -r -tn linux`  
  
Without this, you may get funny characters at the position of box borders and other, similar places.  
You may add this command to your bash startup file. The bash shell of xterm of X11 reads the .bashrc file in your $HOME directory and not .bash_profile. For more information on this topic read the man pages of bash or search for X11 and bash on <http://www.macosxhints.com>.  
For more on fonts, terminals, 8bit characters, ESC codes read [Terminal & Fonts](<Terminal_&_Fonts.md> "Terminal & Fonts").  
  




Starting the IDE:  


  1. start the ide in an xterm  
  
`cd your_path/fpc/ide`  
`./fp`  
  
In order to get Alt-x you have to press ESC and x.  
  
For easier use you may install freepascal and the ide:  
  
`cd your_path/fpc`  
`sudo make install`  
`sudo make -C fv install`  
`sudo make -C ide install`

---

_Source: [https://wiki.freepascal.org/How_to_get_the_fpc_ide_running_under_Mac_OS_X](https://web.archive.org/web/20250315181242/https://wiki.freepascal.org/How_to_get_the_fpc_ide_running_under_Mac_OS_X)_
