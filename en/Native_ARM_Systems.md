# Native ARM Systems

The ARM architecture is unusual in that it was designed for desktop computers, is now overwhelmingly used for embedded systems, but can still occasionally be found in desktops or small servers. The only comparable architecture is [MIPS](<Native_MIPS_Systems.md> "Native MIPS Systems"), although in that case much depends on whether Chinese implementations hit the market at a reasonable price. 

Like almost all computers, ARM-based systems will contain an internal loader, usually in Flash memory. However there are three basic categories of system: 

  * Those that boot an operating system from internal Flash, such as the Linksys/Cisco NSLU2 "Slugs" [[1]](<http://www.cyrius.com/debian/nslu2/install.html>) (using RedBoot), and the SheevaPlug (using U-Boot).


  * Those that boot an operating system from a conventional disc, connected via IDE, SCSI or USB.


  * Those running as guests using an emulator such as [Qemu](<Qemu_and_other_emulators.md> "Qemu and other emulators") on a general-purpose computer.



In any of these cases it is necessary to be able to install a general-purpose operating system such as Linux in order to be able to run development tools. This writer (MarkMLl) favours Debian, since he finds that using it results in a very similar system on a wide range of hardware (x86, SPARC, PPC, MIPS, ARM, zSeries, 68K). 

**If you intend to build Lazarus, make sure you have at least 512Mb memory (RAM + swap) available.**

## Contents

  * 1 Basic OS installation (in Flash)
  * 2 Basic OS installation (conventional disc)
  * 3 Development tools prerequisites
  * 4 Installing FPC from binary release
  * 5 Library prerequisites for FPC
  * 6 Getting and compiling FPC sources
  * 7 Library prerequisites for Lazarus
  * 8 Getting and compiling Lazarus sources
  * 9 Prerequisites for running Lazarus
  * 10 Known gotchas



## Basic OS installation (in Flash)

This is characterised by both the initial loader and the operating system (Linux, plus its associated initial ramdisk image) being stored in internal Flash memory, and as such not being visible to standard software once the system is running. The internal Flash is typically filled using a JTAG connection, or over either a LAN or USB connection using a special program running on a host computer. Where a LAN connection is used the fill protocol might use raw Ethernet, which means that the host and target have to be on the same LAN segment. 

## Basic OS installation (conventional disc)

This relies on the internal firmware (i.e. in Flash) being able to load and transfer control to an operating system from external media, e.g. a USB-connected disc or a Flash card. An alternative is to have something like Open Firmware [[2]](<http://www.openfirmware.info/Open_Firmware>) as a second-stage loader, this has limited capability of parsing partition tables etc. but will be familiar to anybody used to Sun, Apple etc. systems. 

## Development tools prerequisites

Once the operating system is running it will typically have a default host name and IP address, it is necessary to login using e.g. SSH to change these and to load additional packages. 

In the case of Debian or derivatives (Ubuntu etc.) there are, by default, no development tools on a newly-installed system. In order of descending importance, the following packages are needed: 

  * build-essential


  * gdb


  * subversion



I'm assuming that developers have their own preferences as far as things like sudo vs su are concerned. I also find the screen package useful, since it protects a time-consuming build from being killed if the network connection goes down. 

## Installing FPC from binary release

Start off with a binary release from <http://www.freepascal.org/down/arm/linux.var>. This is currently (March 2011) version 2.2.2, which is competent to build 2.4.2 on e.g. a "Slug". 

_That's for an IXP420-based "Slug", your mileage might vary on different ARM variants, due to different FPU implementations._

## Library prerequisites for FPC

Again assuming Debian or a derivative: 

  * libgpm-dev (formerly libgpmg1-dev)


  * libncurses5-dev


  * libncursesw5-dev



This assumes that the debugger interface is not built into the IDE, since building [libgdb](</index.php?title=libgdb&action=edit&redlink=1> "libgdb \(page does not exist\)") is non-trivial. 

## Getting and compiling FPC sources

See <http://www.freepascal.org/down/source/sources.var>, or use Subversion. 

Unpack the sources in e.g. /usr/local/fpc, which will typically result in a new directory /usr/local/fpc/fpcbuild-2.4.2; optionally set up a symlink fpcbuild -> fpcbuild-2.4.2. 

In /usr/local/fpc/fpcbuild/fpcsrc use a command such as this to build FPC: 

$ make NOGDB=1 OPT="-O- -gl -dFPC_ARMEL -CfSOFT" clean all 

_That's for an IXP420-based "Slug", your mileage might vary on different ARM variants, due to different FPU implementations._

Provided that that runs successfully, i.e. leaving a recently-created build-stamp.arm-linux file in the current directory, it should be safe to overwrite the existing binary compiler: 

# make install 

Running fpc -? should now show the current date in the banner. 

## Library prerequisites for Lazarus

The development libraries vary depending on the widget set(s) required. 

  * For gtk1: libgdk-pixbuf-dev libgtk1.2-dev


  * For gtk2: libgtk2.0-dev


  * For Qt: Get the Free Pascal Qt4 Binding from [[3]](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>)



As of 2011, GTK1 is deprecated by both the Lazarus and upstream (Debian etc.) developers. Qt works for Debian "Squeeze" and Lazarus 0.9.30, but might be problematic with older versions. 

## Getting and compiling Lazarus sources

Get the latest stable version from <http://sourceforge.net/projects/lazarus/files/>, or use Subversion. 

Assuming that the sources are unpacked in /usr/local/share/lazarus, in that directory run 

$ make bigide 

Provided that that builds, consider setting up symbolic links in /usr/local/bin to point to lazarus, lazbuild and startlazarus. 

## Prerequisites for running Lazarus

There are three ways of running Lazarus: 

  * Using an X server on the same system as is running the program, either with internal video hardware or over VNC.


  * Using an X server on the developer's desktop system, logged into a desktop on the system running the program.


  * Using X tunneled over SSH.



The first two of those generally require some sort of desktop manager on the development system. In general, things like an NSLU2 "Slug" are too "resource challenged" to be able to run KDE or Gnome, my (MarkMLl) preference is to install gdm for the sake of the XDMCP protocol and FluxBox as a minimal desktop (on Debian, the fluxbox and xterm packages). 

## Known gotchas

At present (March '11) there is a problem which prevents the IDE from running on ARM and SPARC systems, since it's fine on x86 and PPC I suspect this is an alignment issue.

---

_Source: [https://wiki.freepascal.org/Native_ARM_Systems](https://web.archive.org/web/20250124220322/https://wiki.freepascal.org/Native_ARM_Systems)_
