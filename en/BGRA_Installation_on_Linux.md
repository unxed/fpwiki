# BGRA Installation on Linux

**BGRA Installation on Linux**

**Prerequisites:**

  * 1\. Proxmox 5.2 as virtualization solution 
  * 2\. Linux MINT beta 19 XFCE 64 bits as operating system 



Using a virtualization solution has the advantage of starting with a clean system very easy. The resources of the host a well utilized in terms of, example hosting several systems on a computer instead of only one. Proxmox as open source virtualization solution has Enterprise capabilities that are found only in commercial versions of ESXi for example. 

Linux MINT XFCE offers a graphic environment with a very light overhead making it a good choice for virtualization where a GUI is needed. 

**Steps:**

  * 1\. Install Proxmox and have a clone of the Linux MINT as sandbox. 
  * 2\. Install SSH server: 


    
    
    sudo apt install openssh-server

  * 3\. Install needed libraries: 


    
    
    sudo aptitude install -y make binutils build-essential gdb subversion zip unzip libx11-dev libgtk2.0-dev libgdk-pixbuf2.0-dev libcairo2-dev libpango1.0-dev unrar
    
    
    sudo apt-get update
    sudo apt-get install -y libxtst-dev libgl1-mesa-dev
    sudo apt-get install -y libatk-adaptor

  * 4\. Go into downloads folder and download FPCUPDeluxe with 


    
    
    wget --content-disposition https://github.com/newpascal/fpcupdeluxe/releases/download/v1.6.0r/fpcupdeluxe-x86_64-linux

  * 5\. Make the download executable with 


    
    
    chmod +x fpcupdeluxe-x86_64-linux

  * 6\. Launch the executable in the graphic console with 


    
    
    ./fpcupdeluxe-x86_64-linux

  * 7\. Now select CPU x8664 and OS linux, FPC stable and Lazarus stable. 
  * 8\. Run the install option which will install Lazarus 1.8.4 as per June 2018. 
  * 9\. After installation is done install (Package -> Install Packages) and open Online Package Manager from the Package Menu. Select BGRA Bitmap and Controls and wait for download and compilation. 
  * 10\. Open Lazarus and enjoy the new pallet entries. 



[Installation on Linux with Ansible](<Installation_on_Linux_with_Ansible.md> "Installation on Linux with Ansible")

---

_Source: [https://wiki.freepascal.org/BGRA_Installation_on_Linux](https://web.archive.org/web/20200928164552/https://wiki.freepascal.org/BGRA_Installation_on_Linux)_
