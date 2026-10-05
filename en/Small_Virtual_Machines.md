# Small Virtual Machines

## Contents

  * 1 Overview
  * 2 Windows WSL and WSL2
  * 3 Levinux virtual machine
  * 4 ReactOS virtual machine
  * 5 WinXP virtual machine
  * 6 Debian console virtual machine
  * 7 Debian LXDE virtual machine
  * 8 Notes
  * 9 To do
  * 10 Further Reading
  * 11 See also



## Overview

Shows how to create small ReactOS and Debian virtual machines using Qemu on Ubuntu, and then compile and run a Free Pascal program. 

The small virtual machines should serve two purposes:  
1) They show how a "standard PC" can be used to run a program. So if you are having problems running the program on your PC you can see how it works on a different PC.  
2) If the small VM is easy to download and run then you can simply do that. The LevinUx stuff below shows how easy that could be. 

## Windows WSL and WSL2

[Basic Info](<https://docs.microsoft.com/en-us/windows/wsl/install-win10>)

Note that at this time hardware and GUI will not work! 

If upgrade to Win10 Insider Preview can install WSL2: [wsl2 install](<https://docs.microsoft.com/en-us/windows/wsl/wsl2-install>) but be aware that other virtual machine software (like VMware, Parallels) may no longer work (may eventually work [they are working together](<https://techcommunity.microsoft.com/t5/Virtualization/VMware-Workstation-and-Hyper-V-Working-Together/ba-p/825831>)). 

Note that the WSL command may not work in Powershell but might work in Command Prompt. After exit from the terminal (in Linux) do **WSL --shutdown** to free lots of memory. 

On July 29, 2019 I've installed Ubuntu 18.04 LTS and Debian (stretch = 9). **sudo apt install fpc** on Ubuntu gets 3.0.4 whereas the same on Debian only gets 3.0.0 . 

**Optional below for FPC 3.2 Beta:**

**Enter the command below (to determine your CPU type):**

neofetch **(see CPU specs)**

**Next we'll download the appropriate zip file (from[here](<https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/releases/>)):** ****

**(need to get appropriate fpcup release for your CPU type below)**

sudo wget <https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/releases/download/1.6.2v/fpcup-x86_64-linux>

sudo chmod 755 fpcup-x86_64-linux 

sudo apt install subversion zip unzip build-essential libx11-dev 

sudo ./fpcup-x86_64-linux --fpcurl=<https://svn.freepascal.org/svn/fpc/branches/fixes_3_2> \--verbose 

**Can run "./fpc" or "./fp" when done if go to: root/development/fpc/bin/x86_64-linux** ****

## Levinux virtual machine

Levinux is a small (~20 MB) QEMU download for x86 PCs (Windows, macOS, Linux) that provides a small Tiny Core Linux VM. You can visit the two links below to read more about it, or just follow the simple instructions below (on Windows) to try it out with Free Pascal.  

    
    
     <http://mikelev.in/ux/>
     <http://levinux.com> (**just** a 21 page JavaScript slideshow about it)
    

Tried on Windows XP in May 2016 

Go to <https://github.com/miklevin/levinux> and click the **Download Zip** or **Clone or download** button near the upper-right corner. 

Unzip the small (18.6 MB) file levinux-master.zip and run WindowsLevinux.vbs (may need to **unblock** for the firewall). **If you need to learn how to operate things in Windows to do this** you might want to read a tutorial that introduces the Pascal programming language, file managers, the command prompt, the Free Pascal compiler, and the minimal files needed to compile a program. The Win32 Hello World Tutorial is available in three formats: [doc](<http://turbocontrol.com/Hello%20World%20Tutorial.doc>) \- [PDF](<http://turbocontrol.com/Hello%20World%20Tutorial.pdf>) \- [web](<http://turbocontrol.com/Hello%20World%20Tutorial.htm>)

Select 2 - Exit to Console **then** log in (tc and foo, do **sudo poweroff** later to shut down) 

**Do these steps only if want a simple test**  
See <http://turbocontrol.com/helloworld.htm> if more information is needed.  
**Note** that Linux is case sensitive (unlike Windows) so you must type the letters as shown below!  
In the VM terminal do: (**Note** that we are renaming the filename by using the capital Oh parameter)  
wget http://turbocontrol.com/HelloWorld-fpc-2.2.2.i386-linux.tar.gz -O hello.tar.gz  
tar xzvf hello.tar.gz  
./ppc386 hello  
tce-load -wi binutils.tcz  
./ppc386 hello  
./hello 

**Do these steps if want a FPC 3.0.0 compiler (doesn't work yet)**  
In the VM terminal do:  
tce-load -wi svn.tcz  
tce-load -wi zip-unzip.tcz  
tce-load -wi compiletc.tcz (**larger**)  
wget https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/releases/download/1.4.0k/fpcup-i386-linux?raw=true --no-check-certificate -O fpcup  
chmod u+rx fpcup  
./fpcup --verbose **Then "y" but this doesn't work**

I'd like to change levinux to have all the files and tools needed to pull the latest source code and cross-compile FPC (also with Debian instead of Tiny Core?). 

## ReactOS virtual machine

Built (on Ubuntu 16.04 on 1 June 2017) a minimal i386 VM with QEMU with ReactOS 0.4.5. 

Download [ReactOS-0.4.5-iso.zip](<http://sourceforge.net/projects/reactos/files/ReactOS/0.4.5/ReactOS-0.4.5-iso.zip>) (103MB) 

qemu-img create -f qcow2 reactos045.img 8G 

qemu-system-i386 -enable-kvm -m 512 -hda reactos045.img -cdrom ReactOS-0.4.5.iso -boot d -net nic,model=rtl8139 -net user 

For the install just press <Enter> for everything. When it reboots (twice) don't hit any key so it won't boot from the CD. 

Shut down ReactOS. The QEMU image file is 349MB. 

Make a backup copy then restart: 

qemu-system-i386 -enable-kvm -m 512 -hda reactos045.img -boot c -net nic,model=rtl8139 -net user 

To see that networking is working, in the VM double-click on the Command Prompt and run ipconfig. 

As a simple test, install PeaZip (a Lazarus application) from the Application Manager in the Tools section, then in command prompt do: 

cd \ 

md test 

cd test 

dwnl http://turbocontrol.com/HelloWorld-fpc-2.4.2.i386-win32.zip 

unzip HelloWorld-fpc-2.4.2.i386-win32.zip 

ppc386 hello 

hello 

On 1 June 2017 I updated text below for the image file name (to reactos045.img) but did not test anything else! 

**Needs more work** but getting there! In command prompt do: 

cd \ 

md test 

cd test 

**This whole line is a comment!** dwnl http://downloads.sourceforge.net/gnuwin32/wget-1.11.4-1-setup.exe wgetsetup.exe **[didn't work](<https://www.reactos.org/forum/viewtopic.php?f=2&t=14370>)**

**This whole line is a comment!** so I download and put here (which does work): 

dwnl http://turbocontrol.com/wget-1.11.4-1-setup.exe wgetsetup.exe 

wgetsetup 

"c:\program files\gnuwin32\bin\wget" https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/releases/download/1.4.0k/fpcup-i386-win32.exe?raw=true --no-check-certificate -O fpcup.exe 

dir **(verify that fpcup.exe is about 1.09 MB)**

Exit the command prompt, shut down ReactOS, make a backup copy then restart: 

qemu-system-i386 -enable-kvm -m 512 -hda reactos045.img -boot c -net nic,model=rtl8139 -net user 

fpcup --verbose **then** y to continue  


Stops progressing while **compiling** jwawindows.pas (with **100%** CPU usage, **can't** Ctrl-C to stop) 

Ctrl-Alt then stop QEmu, replace with backup copy, restart (with **much** larger memory): 

qemu-system-i386 -enable-kvm -m 4000 -hda reactos045.img -boot c -net nic,model=rtl8139 -net user 

Stops compiling with error, restarted the VM, and did just fpcup several times (no --verbose) and it finished! 

**This works!**

I used dwnl to download the 6 arm-embedded-*.exe files and ran fpcup to compile for arm-embedded (see the Windows XP VM section below and everything seems to work. 

Made folder c:\development\cross\bin\arm-embedded and copied 6 files 

dwnl http://svn2.freepascal.org/svn/fpcbuild/binaries/i386-win32/arm-embedded-ar.exe **then** similar for other 5 files 

Did: fpcup --ostarget=embedded --cputarget=arm --subarch=armv7em 

No, you don't want to use ReactOS for your desktop operating system, but it might be good enough to use for some specific things (like putting in a small VM or in a single-app kiosk or embedded device). 

## WinXP virtual machine

**Temporarily** I used a 32-bit WinXP VM (3.8GB) on VMware and **got this to work for Teensy** : 

**\- (do this if want trunk Lazarus)** Copied in fpcup.exe and ran as (~35 minutes, VM increased to 7.3GB): fpcup --skip=lhelp --fpcURL=trunk --lazURL=trunk 

**\- (do this for only FPC)** Copied in fpcup.exe and ran as (~11 minutes, VM increased to 5.0GB): fpcup --only=FPC --fpcURL=trunk 

\- Made folder c:\development\cross\bin\arm-embedded and copied 6 files from [here](<http://svn2.freepascal.org/svn/fpcbuild/binaries/i386-win32/>)

\- Did (~2.5 minutes): fpcup --ostarget=embedded --cputarget=arm --subarch=armv7em --only=FPCCleanOnly,FPCBuildOnly --fpcURL=trunk 

\- Made folder and copied in [test.pas](<http://forum.lazarus.freepascal.org/index.php/topic,28561.msg179240.html#msg179240>), ppcrossarm.exe (from c:\development\fpc\compiler), system.o & system.ppu & mk20d7.o & mk20d7.ppu (from c:\development\fpc\units\arm-embedded\rtl), and the as.exe, ld.exe, and objcopy.exe from the arm-embedded folder (and renamed those 3 after copied). 

\- Get [teensy.exe](<https://www.pjrc.com/teensy/loader_xp.html>) and copy in. 

\- Compile: ppcrossarm -Oonostackframe -Parm -Tembedded -Cparmv7em -Wpmk20dx256xxx7 test.pas 

\- Run teensy.exe and load test.hex 

\- Repeat the last 2 steps while varying the timing (2666666 to 0666666) to show that you are changing the program! 

**Files released[here](<http://turbocontrol.com/simpleteensy.htm>)**

## Debian console virtual machine

Built (on Ubuntu 16.04 on 1 Aug. 2017) a minimal i386 VM with QEMU with Debian (new version 9.1 release) stretch. 

Dowload from [Debian](<https://www.debian.org/>) debian-9.1.0-i386-netinst.iso (375 MB) and rename to debstr.iso 

qemu-img create -f qcow2 debstr.img 8G  
qemu-system-i386 -enable-kvm -m 512 -hda debstr.img -cdrom debstr.iso -boot d 

Networking worked without having to add other command line parameters! Remove the 3 default software collections (near the end of the install). The QEMU image file is about 1.3 GB. 

In the VM:  
add sudo (There are instruction near the bottom of this page: [Control Pascal PicoScope](<http://controlpascal.com/picoscope.htm>))  
sudo apt-get clean 

**Do these next steps only if you want to minimize the size of the VM:**  
sudo dd if=/dev/zero of=/mytempfile (**note** that this is very slow as it fills the entire 8G)  
sudo rm -f /mytempfile  
sudo halt  
qemu-img convert -O qcow2 debstr.img debstrnew.img **(reduces file to 595 MB)**  
Archive with 7z and the file is now 141 MB. Not very big for a nice new Debian console VM. 

Make a backup copy then start like this:  
qemu-system-i386 -enable-kvm -m 512 -hda debstrnew.img -boot c 

**Do these steps only if want a simple test** In the VM do (see <http://turbocontrol.com/helloworld.htm> if needed):  
wget http://turbocontrol.com/HelloWorld-fpc-2.2.2.i386-linux.tar.gz  
tar xzvf HelloWorld-fpc-2.2.2.i386-linux.tar.gz  
./ppc386 hello  
sudo apt-get install binutils  
./ppc386 hello  
./hello 

**Do these steps if want a trunk FPC and then arm-embedded for Teensy** In the VM do:  
sudo apt-get install build-essential subversion unzip  
wget https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/releases/download/1.4.0k/fpcup-i386-linux?raw=true --no-check-certificate -O fpcup  
chmod u+rx fpcup  
./fpcup --fpcURL=trunk --only=FPC **This works!**  
sudo apt-get install binutils-arm-none-eabi  
./fpcup --ostarget=embedded --cputarget=arm --subarch=armv7em --only=FPCCleanOnly,FPCBuildOnly --fpcURL=trunk  
**The above (for Teensy) does work!**  
Need to find out how to copy files out of VM. 

## Debian LXDE virtual machine

Built (21 Sept. 2015 on Ubuntu 14.04) a minimal i386 VM with QEMU with Debian. 

Dowload from [Debian](<https://www.debian.org/>) debian-8.2.0-i386-netinst.iso (315 MB) and rename to debjes.iso 

qemu-img create -f qcow2 debjes.img 8G  
qemu-system-i386 -enable-kvm -m 512 -hda debjes.img -cdrom debjes.iso -boot d  


Networking worked without having to add other command line parameters! 

For the software collections screen:  
Remove **Debian desktop environment**  
Remove **print server**  
Add **LXDE**  
Leave (keep) **standard system utilities**  


When the VM restarts close the qemu window rather that restarting the install. The QEMU image file is now 4.0 GB. 

Make a backup copy then start like this:  
qemu-system-i386 -enable-kvm -m 512 -hda debjesnew.img -boot c -net nic -net user  


In the VM in a (user) terminal:  
su (and password for root)  
adduser <user name> sudo  
exit (exit su)  
exit (exit user and terminal)  
logout and login again 

Reboot and choose recovery then enter root password:  
apt-get clean  
dd if=/dev/zero of=/mytempfile (**note** that this is very slow as it fills the entire 8G)  
rm -f /mytempfile  
halt 

After this the new img file is 2.9 GB:  
qemu-img convert -O qcow2 debjes.img debjesnew.img 

Archive with 7z and the file is now 733 MB. Not very big for a nice new Debian LXDE VM. 

Make a backup copy then start like this:  
qemu-system-i386 -enable-kvm -m 512 -hda debjesnew.img -boot c -net nic -net user  


In the VM in a (user) terminal:  
sudo apt-get install build-essential mingw32-binutils subversion unzip libgtk2.0-dev  
cd Downloads  
wget https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/releases/download/1.4.0k/fpcup-i386-linux?raw=true --no-check-certificate -O fpcup  
chmod u+rx fpcup 

./fpcup --skip=lhelp --verbose **then** y to continue  
**This works! Other things (Teensy and Android [[[1]](<http://forum.lazarus.freepascal.org/index.php/topic,29605.msg187827.html#msg187827>)]) to be continued ASAP!**

## Notes

On 32-bit Debian **kvm** runs **qemu-system-x86_64** so get 64-bit which don't want. So changed all of the above calls to **qemu-system-i386 -enable-kvm** which still runs fast (due to kvm). If your linux system doesn't have kvm then you may need to leave **-enable-kvm** off. 

On **qemu-img create** the **2G** could be larger but then on linux it will be much slower to minimize the size. Any suggestions? (Look at zerofree and also consider wear on SD card) 

## To do

Add instructions on how to do this on Windows (not just Ubuntu)? 

[Original forum message](<http://forum.lazarus.freepascal.org/index.php/topic,26315.0.html>) where this got started. 

Add instructions for macOS per [discussion](</Talk:Small_Virtual_Machines> "Talk:Small Virtual Machines") there? 

## Further Reading

What is the most simple way to compile an ARM Embedded compiler for Teensy? See [this forum message](<http://forum.lazarus.freepascal.org/index.php/topic,28561.msg178816.html>) for more details on Teensy. 

Use [fpcup](<fpcup.md> "fpcup")? 

## See also

  * [ReactOS](<ReactOS.md> "ReactOS")
  * [Qemu and other emulators](<Qemu_and_other_emulators.md> "Qemu and other emulators")
  * [TARGET Embedded](<TARGET_Embedded.md> "TARGET Embedded")
  * [Control Pascal Tutorial](<Control_Pascal_Tutorial.md> "Control Pascal Tutorial") \- where we hope this stuff ends up being used
  * [Docker Containerization](<Docker_Containerization.md> "Docker Containerization")

---

_Source: [https://wiki.freepascal.org/Small_Virtual_Machines](https://web.archive.org/web/20230602212020/https://wiki.freepascal.org/Small_Virtual_Machines)_
