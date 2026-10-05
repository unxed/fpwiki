# linux/kernel/user mode VM

## Contents

  * 1 Overview
  * 2 Installation
    * 2.1 File system image
    * 2.2 Networking
      * 2.2.1 Creating TAP interface
      * 2.2.2 Enabling IP forwarding
      * 2.2.3 Configuring UML switch
  * 3 Starting the UML VM
    * 3.1 Startup script



## Overview

This section is intended to help users interested in developing Linux kernel modules to setup an UML VM to debug it during development phase. 

Please note that this page will focus on configuring a VM running on a Debian distribution based host. 

## Installation

You need to install the packages _user-mode-linux_ and _uml-utilities_. 
    
    
    apt-get install user-mode-linux uml-utilities
    

### File system image

The easiest way to set up your system is to download an already built image: 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This URL is obviously fake. Please insert correct URL.
    
    
    wget ftp://ftp.freepascal.org/some path/umlrootfs.img.bz2
    

Then you need to decompress it: 
    
    
    bunzip2 umlrootfs.img.bz2
    

Please note that this is really a minimal installation, including the fp-compiler and aptitude packages. The latter will allow you to install any package you want providing you set up your network correctly. 

The provided root file system image has networking configured already for the VM, so you just need to configure the host part as described in the next section. 

### Networking

#### Creating TAP interface

In order to setup networking you need to configure a tap interface: 
    
    
    cat - > /etc/network/interfaces
     # /etc/network/interfaces -- configuration file for ifup(8), ifdown(8)
     # In this configuration example we will setup two physical networking
     # cards eth0 and eth1. The first card, eth0, will be automatically
     # configured at host starting up, while the second one will be configured
     # on manual demand using "ifup eth1".
     # We will also configure a virtual networking card tap0 which will be
     # configured at system startup too.
     
     auto lo eth0 tap0
     
     # The loopback interface 
     iface lo inet loopback
     
     iface eth0 inet static
       address 192.168.1.2
       netmask 255.255.255.0
       network 192.168.1.0
       gateway 192.168.1.1
     
     iface eth1 inet static
       address 192.168.2.1
       netmask 255.255.255.0
       network 192.168.2.0
     
     iface tap0 inet static
       address 192.168.3.1
       netmask 255.255.255.0
       network 192.168.3.0
       tunctl_user uml-net
       uml_proxy_arp 192.168.3.2
       uml_proxy_ether eth0
    

#### Enabling IP forwarding

In addition to this networking configuration, you need to instruct your kernel to forward IP packets, so the VM can communicate with the rest of your network and the internet. 
    
    
    cat - > /etc/sysctl.conf
     #
     # /etc/sysctl.conf - Configuration file for setting system variables
     # See sysctl.conf (5) for information.
     #
     
     ##############################################################3
     # Functions previously found in netbase
     #
     
     # Uncomment the next line to enable packet forwarding for IPv4
     net.ipv4.ip_forward=1
    

#### Configuring UML switch
    
    
    cat - > /etc/default/uml-utilities
     # Options to pass to uml_switch.
     
     # set to "false" if you want to prevent uml_switch from
     # starting with SysV scripts in /etc/init.d
     UML_SWITCH_START="true"
     
     # For preconfigured tap setup, see
     # /usr/share/doc/uml-utilities/examples/interfaces.example
     UML_SWITCH_OPTIONS="-tap tap0"
     
     # User as which to run uml_switch
     UML_SWITCH_USER="uml-net"
     
     # Socket file to use
     # Debian's default is:
     UML_SWITCH_CTL="/var/run/uml-utilities/uml_switch.ctl"
     #
     # if you instead use your rolled up kernel from upstream
     # sources you may want to uncomment the following:
     #UML_SWITCH_CTL="/tmp/uml.ctl"
    

## Starting the UML VM

### Startup script

Starting UML VM requires a lot of arguments, but a small script can ease this task: 
    
    
    cat - > ${HOME}/bin/startUML.sh
    #! /bin/sh
    UML_BIN=linux
    UML_ROOT_FS=${HOME}/umlrootfs.img
    UML_SWITCH_SOCKET=/var/run/uml-utilities/uml_switch.ctl
    exec ${UML_BIN} \
    	umid=uml1 \
    	mem=512M \
    	ubd0=${UML_ROOT_FS} \
    	eth0=daemon,fe:fd:fe:ed:b0:0f,,${UML_SWITCH_SOCKET} \
    	con0=xterm xterm=/usr/bin/xterm,-T,-e \

---

_Source: [https://wiki.freepascal.org/linux/kernel/user_mode_VM](https://web.archive.org/web/20241213022541/https://wiki.freepascal.org/linux/kernel/user_mode_VM)_
