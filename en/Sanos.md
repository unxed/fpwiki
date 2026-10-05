# Sanos

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Sanos does not seem to be actively developed anymore. The latest release is version 1.3.4 form March 2012

Sanos is a minimal open-source operating system implementing a cut-down Win-32 API for console-mode applications, and as such is a potential upgrade path for developers targeting [GO32V2](<GO32V2.md> "GO32V2"). The basic OS is distributed on a single floppy, and comprises the kernel, drivers for popular hardware, and daemons for FTP and Telnet. There is also an SDK floppy including the Tiny C Compiler (TCC) plus libraries etc., and demonstration floppies with MySQL and a wiki server. 

**Please note that I am not pretending to be an expert on the Sanos kernel, and would much rather not be contacted by anybody outside the FPC/Lazarus community. If you have problems, ask the original author.**

## FPC and Sanos

Sanos requires that all executables have full relocation information, so fpc must be run with the -WR option. However as of version 2.4.2, the FPC runtimes as shipped require the oleaut32 DLL which does not exist in Sanos, see fpc-devel 2011-03-24 (Sven) for a way to work around this. They also require CharLowerBuffW and CharUpperBuffW from user32 which are unimplemented, this is probably fixable. 

More seriously, the standard runtimes require a number of unimplemented kernel32 functions, it is probably impractical to either add these to the Sanos kernel or remove them from the FPC RTL. As a result, it is unlikely that Sanos will ever be a viable target for FPC. 

## Developing on Sanos

In case anybody does attempt to progress this further, below are my notes on getting the Sanos SDK set up. 

The Sanos SDK is packaged separately from the basic operating system, and merging them can be problematic: the 1.3.4 ("latest") kernel refused to read the 1.3.3 SDK floppy and I was completely unable to find an IDE disc which could be handled by the SDK floppy's kernel. In the end I mounted the 1.3.3 Qemu live floppy and an image of the SDK floppy together (i.e. two floppies running on Qemu), copied the eepro100 driver, ftpd.exe and login.exe onto the SDK floppy image, wrote that to a physical floppy and booted it standalone. At that point I was able to FTP the /usr tree containing the SDK onto a desktop system, and then onto a running 1.3.4 system. 

Simple programs may be built natively using TCC, but the operating system itself must be cross-built on Windows 2000 (or later) using Microsoft Visual Studio 2005 Express[[1]](<http://go.microsoft.com/fwlink/?LinkId=51410&clcid=0x409>) and the corresponding Microsoft Macro Assembler[[2]](<http://www.microsoft.com/downloads/info.aspx?na=41&SrcFamilyId=7A1C9DA0-0510-44A2-B042-7EF370530C64&SrcDisplayLang=en&u=http%3a%2f%2fdownload.microsoft.com%2fdownload%2f7%2f4%2f0%2f740977da-3ad4-46a1-ad41-92758be79b01%2fMASMsetup.EXE>). This appeared to work. 

The os.dll file tailored for the Windows host didn't work on my system when built using VS2005, I was unable to determine why and in the end used FTP as described above. In principle TCC etc. should self-build, but the ar.exe and objectcopy.exe files in the SDK don't contain relocation information so can only be run on a Windows system. Fixing this and adding make etc. is left as an exercise. 

In actual fact I am very suspicious that the combination of kernel 1.3.4 and SDK 1.3.3 is (at best) unreliable. However since I have very little pretension to being a C hacker and have absolutely no inclination to mess around with Visual Studio on a version of MS Windows that I don't normally run I'm giving up at this point. 

## External links

  * [Sanos homepage](<http://www.jbox.dk/sanos/index.htm>)

---

_Source: [https://wiki.freepascal.org/Sanos](https://web.archive.org/web/20250124213054/https://wiki.freepascal.org/Sanos)_
