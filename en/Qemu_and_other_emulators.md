# Qemu and other emulators

This note covers setting up Qemu on an x86-based development system running Linux. This allows native (rather than cross) development tools to be run, which can be useful: 

  * where the target system has performance/resource issues (e.g. some ARM systems)
  * is not run natively due to company policy (older versions of Microsoft Windows)
  * is quite simply unavailable at a reasonable price (e.g. SGI MIPS systems or the Chinese MIPS-based systems).



It also briefly mentions User Mode Linux for x86 and the Hercules emulator for IBM zSeries mainframes despite the fact that these are not particularly relevant to Free Pascal, and Docker containerisation. It does not consider x86-on-x86 virtualisation systems such as VMware, Microsoft Hyper-V or Oracle VirtualBox, and only considers Linux KVM as a foundation technology for Qemu. 

## Contents

  * 1 The Host System
  * 2 Debian Guest using Qemu
  * 3 Windows 2K Guest using Qemu
  * 4 KVM on a Linux host
  * 5 Common Qemu startup, ifup and ifdown scripts
  * 6 Slackware x86 Guest using User Mode Linux
  * 7 Debian 68k Guest using Aranym
  * 8 Debian zSeries Guest using Hercules, without VM
  * 9 Debian zSeries Guest using Hercules, with VM
  * 10 MUSIC/SP using Sim/390 or Hercules
  * 11 VM/370 using Hercules
  * 12 IBM S/370, S/390, and the S/380 "hack"
  * 13 IA-64 (Itanium, Merced, McKinley etc.)
  * 14 Relative Performance
  * 15 Graphical Access using VNC etc.
    * 15.1 Running Qemu or UML in an xterm or with host VNC support
    * 15.2 Connecting to a Qemu (or UML etc.) guest using X
    * 15.3 Connecting to a Qemu (or UML etc.) guest using VNC
  * 16 Accessing "Unusual" Devices
    * 16.1 Sound/MIDI for Qemu
      * 16.1.1 Identifying the Host's Hardware
      * 16.1.2 Selecting the Guest's Soundcard
    * 16.2 USB for Qemu
    * 16.3 USB for UML
  * 17 QEMU User Emulation Mode in Chrooted Environment
  * 18 Docker images
    * 18.1 Docker fundamentals
    * 18.2 A 64-bit Docker container with basic utilities
    * 18.3 A 32-bit Docker container with FPC and Lazarus
  * 19 Further Reading
  * 20 See also



## The Host System

In the current case, the host is a Compaq rack-mount server running at around 3GHz with several Gb RAM; be warned that performance will drop off drastically with a lower specification system. It has two internal drive cages, the first is connected to a RAID controller and is used for the host operating system and tools, the second is connected to a SCSI controller and contains 6x discs each of which is used for a different guest system. 

The host IP address is 192.168.1.22 and the system is named pye-dev-07, the default gateway and name server are on 192.168.1.1. Guest systems are on the 192.168.22.x subnet and are named pye-dev-07a (192.168.22.16), pye-dev-07b (192.168.22.17) and so on, they have their own gateway 192.168.22.1 which is known to the site router and firewalls. 

The host operating system is Debian "Squeeze", the host normally runs headless and may be accessed by SSH, X using XDMCP, or VNC. The display manager is gdm since this has a better XDMCP implementation than the alternatives, however in practice graphical login is most often handled by VNC. 

The following guests are implemented: 

pye-dev-07a
    Debian on ARM (little-endian, armel) using Qemu

pye-dev-07b
    Debian on MIPS (little-endian, mipsel) using Qemu

pye-dev-07c
    Slackware x86 13.37 using Qemu

pye-dev-07d
    Slackware x86 13.37 using User Mode Linux

pye-dev-07e
    Windows 2K using Qemu

pye-dev-07f
    Debian on zSeries using the Hercules emulator

pye-dev-07g
    Debian on 68k using the Aranym emulator

Originally, pye-dev-07a was earmarked for big-endian ARM, but this appears to be being phased out by Debian so is probably no longer a viable target. Anybody planning to port FPC to the AVR-based Arduino? 

In general, multiple guests can run simultaneously although this has not been exhaustively tested recently. 

In the case of Linux the guest systems are each installed on an 18Gb disc, in the case of Windows a 36Gb disc is used. Each disc is assigned a label using e2label (arm, armel and so on), so that the startup script can mount it by name irrespective of which drive cage slot it's in. 

## Debian Guest using Qemu

Select a suitable Debian mirror and version, for example 
    
    
     <http://ftp.de.debian.org/debian/dists/squeeze/main/>...
    

Fetch a kernel and initrd image for Debian Squeeze, as below. 

_For ARM (little-endian):_
    
    
     .../main/installer-armel/current/images/versatile/netboot/vmlinux-2.6.32-5-versatile
     .../main/installer-armel/current/images/versatile/netboot/initrd.gz
    

In addition for this architecture you also need: 
    
    
     <http://people.debian.org/~aurel32/qemu/armel/initrd.img-2.6.32-5-versatile>
    

_For MIPS (little-endian):_
    
    
    .../main/installer-mipsel/current/images/malta/netboot/vmlinux-2.6.32-5-4kc-malta
    .../main/installer-mipsel/current/images/malta/netboot/initrd.gz
    

Copy these to the disc reserved for the guest, e.g. /export/mipsel. 

Create a filesystem for Qemu, e.g.: 
    
    
    # qemu-img create -f qcow mipsel_hda.img 16G
    

Expect that to round up to around 17.1 Gb, if it doesn't then experiment. Start Qemu, telling it what kernel, initrd and filesystem to use: 

_For ARM (little-endian):_
    
    
    # qemu-system-arm -M versatilepb -kernel vmlinuz-2.6.32-5-versatile -initrd initrd.gz \
    -hda armel_hda.img -append root=/dev/ram
    

Note that the above command requires X access, e.g. ssh with the -X option. 

_For MIPS (little-endian):_
    
    
    # qemu-system-mipsel -M malta -kernel vmlinux-2.6.32-5-4kc-malta -initrd initrd.gz \
    -hda mipsel_hda.img -append "root=/dev/ram console=ttyS0" -nographic
    

Install the guest operating system as usual, splitting the disc into 16.5Gb for / with the remainder (around 700Mb) as swap. This will be slow, 8 or 9 hours is not implausible, so make sure that nobody's about to turn off your mains or disconnect you from the Internet. 

Don't worry if it tells you it's not installing a loader- it's not needed since the kernel and initrd are loaded into memory by the host. 

Boot the operating system and set network addresses etc. Use 192.168.22.16 or similar, with a gateway of 192.168.22.1. 

_For ARM (little-endian):_
    
    
    # qemu-system-arm -M versatilepb -kernel vmlinuz-2.6.32-5-versatile \
            -initrd initrd.img-2.6.32-5-versatile -hda armel_hda.img -append "root=/dev/sda1"
    

_For MIPS (little-endian):_
    
    
    # qemu-system-mipsel -M malta -kernel vmlinux-2.6.32-5-4kc-malta \
             -hda mipsel_hda.img -append "root=/dev/sda1 console=ttyS0" -nographic
    

Finally, you should be able to boot the operating system with an operational network. This relies on having /etc/qemu-ifup and /etc/qemu-ifdown files (see below), and passes additional parameters to them in shell variables. In outline: 

_For ARM (little-endian):_
    
    
    # qemu-system-arm -M versatilepb -m 256 -hda armel_hda.img \
             -kernel vmlinuz-2.6.32-5-versatile -initrd initrd.img-2.6.32-5-versatile \
             -append 'root=/dev/sda1 text' \
             -net nic,macaddr=00:16:3e:00:00:01 -net tap,ifname=tun1
    

_For MIPS (little-endian):_
    
    
    # qemu-system-mipsel -M malta -m 256 -hda mipsel_hda.img \
             -kernel vmlinux-2.6.32-5-4kc-malta -no-reboot \
             -append 'root=/dev/sda1 console=ttyS0' -nographic \
             -net nic,macaddr=00:16:3e:00:00:02 -net tap,ifname=tun2
    

Remember that if you change the network interface type or MAC address you will probably need to delete entries from the guest's /etc/udev/rules.d/z??_persistent-net.rules file. 

## Windows 2K Guest using Qemu

Use dd to save a .iso image of the installation CD. Create a filesystem image: 
    
    
    # qemu-img create -f qcow2 win2k.img 32G
    

Boot using startup script as below. Note that this must specify a non-default network card, since Qemu's current (as of 2011) default is not supported by Windows 2K. 

_TODO: run with kernel support module._

## KVM on a Linux host

KVM (Kernel-based Virtual Machine) is an enabling API supported on more recent x86 and x86-64 (AMD64) systems, this replaces the older KQemu kernel module which is now deprecated by both Qemu and the kernel. 

KVM is typically enabled by the host system BIOS. If not enabled by the BIOS it cannot be enabled by the kernel or by (a suitably privileged) application program, since the x86 architecture requires power to be cycled to change the state of this facility. The result of this is that KVM might be unavailable (no /dev/kvm device) even if it shown as supported by the CPU flags in /proc/cpuinfo. 

## Common Qemu startup, ifup and ifdown scripts

There is much commonality irrespective of whether the guest is running Linux or Windows. 

_First startup script (e.g. /export/C):_
    
    
    #!/bin/sh
     
    mount -L mipsel
    cd /export/mipsel
    . ./C-2
    

_Second startup script for ARM (little-endian):_
    
    
    #!/bin/sh
     
    # Routine startup of a Qemu guest relies on (the host) running /etc/qemu-ifup
    # to condition ARP, forwarding etc.
     
    QEMU_ID=1
    QEMU='qemu-system-arm -M versatilepb'
    QEMU_RAM='-m 256'
    QEMU_HD='-hda armel_hda.img'
    QEMU_CD=''
    QEMU_BOOT="-kernel vmlinuz-2.6.32-5-versatile -append 'root=/dev/sda1 text' -initrd initrd.img-2.6.32-5-versatile"
    # QEMU_MONITOR='-monitor stdio -nographic'
    # QEMU_MONITOR='-nographic'
    QEMU_VGA=''
    VNC_ID=$(($QEMU_ID+1))
    # QEMU_VNC="-vnc :$VNC_ID -k en-gb"
    QEMU_VNC=''
    QEMU_NET="-net nic,macaddr=00:16:3e:00:00:0$QEMU_ID -net tap,ifname=tun$QEMU_ID"
    QEMU_GUEST_IP_ADDRESS=192.168.22.17
    QEMU_GUEST_IP_GATEWAY=192.168.22.1
    QEMU_HOST_GATEWAY_IF=eth1
    export QEMU_GUEST_IP_ADDRESS QEMU_GUEST_IP_GATEWAY QEMU_HOST_GATEWAY_IF
    
    echo \* $QEMU $QEMU_RAM $QEMU_HD $QEMU_CD $QEMU_BOOT \
            $QEMU_MONITOR $QEMU_VGA $QEMU_NET $QEMU_VNC
    
    screen -S QEMU_$QEMU_ID \
    sh -c "$QEMU $QEMU_RAM $QEMU_HD $QEMU_CD $QEMU_BOOT \
            $QEMU_MONITOR $QEMU_VGA $QEMU_NET $QEMU_VNC"
    
    cd ..
    

_Second startup script for MIPS (little-endian):_
    
    
    #!/bin/sh
     
    # Routine startup of a Qemu guest relies on (the host) running /etc/qemu-ifup
    # to condition ARP, forwarding etc.
     
    QEMU_ID=2
    QEMU='qemu-system-mipsel -M malta'
    QEMU_RAM='-m 256'
    QEMU_HD='-hda mipsel_hda.img'
    QEMU_CD=''
    QEMU_BOOT="-kernel vmlinux-2.6.32-5-4kc-malta -append 'root=/dev/sda1 console=ttyS0' -no-reboot"
    # QEMU_MONITOR='-monitor stdio -nographic'
    QEMU_MONITOR='-nographic'
    QEMU_VGA=''
    VNC_ID=$(($QEMU_ID+1))
    # QEMU_VNC="-vnc :$VNC_ID -k en-gb"
    QEMU_VNC=''
    QEMU_NET="-net nic,macaddr=00:16:3e:00:00:0$QEMU_ID -net tap,ifname=tun$QEMU_ID"
    QEMU_GUEST_IP_ADDRESS=192.168.22.18
    QEMU_GUEST_IP_GATEWAY=192.168.22.1
    QEMU_HOST_GATEWAY_IF=eth1
    export QEMU_GUEST_IP_ADDRESS QEMU_GUEST_IP_GATEWAY QEMU_HOST_GATEWAY_IF
    
    echo \* $QEMU $QEMU_RAM $QEMU_HD $QEMU_CD $QEMU_BOOT \
           $QEMU_MONITOR $QEMU_VGA $QEMU_NET $QEMU_VNC
    
    screen -S QEMU_$QEMU_ID \
    sh -c "$QEMU $QEMU_RAM $QEMU_HD $QEMU_CD $QEMU_BOOT \
           $QEMU_MONITOR $QEMU_VGA $QEMU_NET $QEMU_VNC"
    
    cd ..
    

_Second startup script for Windows:_
    
    
    #!/bin/sh
     
    # Routine startup of a Qemu guest relies on (the host) running /etc/qemu-ifup
    # to condition ARP, forwarding etc.
     
    QEMU_ID=4
    QEMU=qemu
    QEMU_RAM='-m 256'
    QEMU_HD='-hda win2k.img'
    QEMU_CD='-cdrom Windows2k-SP4.iso'
    QEMU_BOOT='-boot c'
    QEMU_MONITOR='-monitor stdio'
    QEMU_VGA='-vga cirrus'
    VNC_ID=$(($QEMU_ID+1))
    QEMU_VNC="-vnc :$VNC_ID -k en-gb"
    QEMU_NET="-net nic,macaddr=00:16:3e:00:00:0$QEMU_ID,model=rtl8139 -net tap,ifname=tun$QEMU_ID"
    QEMU_GUEST_IP_ADDRESS=192.168.22.20
    QEMU_GUEST_IP_GATEWAY=192.168.22.1
    QEMU_HOST_GATEWAY_IF=eth1
    export QEMU_GUEST_IP_ADDRESS QEMU_GUEST_IP_GATEWAY QEMU_HOST_GATEWAY_IF
    
    echo \* $QEMU $QEMU_RAM $QEMU_HD $QEMU_CD $QEMU_BOOT \
            $QEMU_MONITOR $QEMU_VGA $QEMU_NET $QEMU_VNC
    
    screen -S QEMU_$QEMU_ID \
    $QEMU $QEMU_RAM $QEMU_HD $QEMU_CD $QEMU_BOOT \
            $QEMU_MONITOR $QEMU_VGA $QEMU_NET $QEMU_VNC
    
    cd ..
    

_/etc/qemu-ifup (for both Linux and Windows):_
    
    
    #!/bin/bash
     
    # if-up file for qemu, heavily cribbed from the command sequence embedded in
    # User Mode Linux. MarkMLl.
    
    echo Running /etc/qemu-ifup $1 $2...
    
    # For compatibility with UML the only parameter here is $1 which is the
    # interface name. I've put in a reference to $2 so we can see it if anything
    # changes.
    
    # I'm going to assume that qemu is always run by root. This is fairly
    # reasonable since it allows guest OSes to be fired up which themselves might
    # give access to confidential data etc. if compromised.
    
    # Here's my equivalent to the host-side UML setup for Qemu. We're hamstrung
    # here by the fact that the emulator is not telling us what IP address it's
    # trying to enable, there isn't a 1:1 correspondence between IP addresses and
    # interfaces since the latter depends on the order the sessions are started.
    #
    # As a hack, assume that the caller exports QEMU_GUEST_IP_ADDRESS (e.g.
    # 192.168.17.16), QEMU_GUEST_IP_GATEWAY (e.g. 192.168.17.1) and
    # QEMU_HOST_GATEWAY_IF (e.g. eth0).
    
    echo \* modprobe tun
    modprobe tun
    
    echo \* ifconfig $1 $QEMU_GUEST_IP_GATEWAY netmask 255.255.255.255 up
    ifconfig $1 $QEMU_GUEST_IP_GATEWAY netmask 255.255.255.255 up
    
    X=`cat /proc/sys/net/ipv4/ip_forward`
    if [ "$X" == "0" ]; then
    
     # Use either this...
    
    #  echo Global forwarding is not enabled. Please refer to the administrator
    #  echo responsible for this machine, enabling it might be a security hazard.
    
     # ...or this.
    
      echo Forcibly enabling global forwarding, note that this might be a security hazard.
      echo \* echo 1 \> /proc/sys/net/ipv4/ip_forward
      echo 1 > /proc/sys/net/ipv4/ip_forward
      X=`cat /proc/sys/net/ipv4/ip_forward`
      if [ "$X" == "0" ]; then
        echo Unable to enable global forwarding. Please refer to the administrator
        echo responsible for this machine.
      fi
    fi
    
    echo \* route add -host $QEMU_GUEST_IP_ADDRESS dev $1
    route add -host $QEMU_GUEST_IP_ADDRESS dev $1
    
    echo \* echo 1 \> /proc/sys/net/ipv4/conf/$1/proxy_arp
    echo 1 > /proc/sys/net/ipv4/conf/$1/proxy_arp
    X=`cat /proc/sys/net/ipv4/conf/$1/proxy_arp`
    if [ "$X" == "0" ]; then
      echo -n Retrying
      while [ "$X" == "0" ]; do
        sleep 1
        echo -n .
        echo 1 > /proc/sys/net/ipv4/conf/$1/proxy_arp
        X=`cat /proc/sys/net/ipv4/conf/$1/proxy_arp`
      done
      echo OK
    fi
     
    echo \* arp -Ds $QEMU_GUEST_IP_ADDRESS $1 pub
    arp -Ds $QEMU_GUEST_IP_ADDRESS $1 pub
    
    echo \* arp -Ds $QEMU_GUEST_IP_ADDRESS $QEMU_HOST_GATEWAY_IF pub
    arp -Ds $QEMU_GUEST_IP_ADDRESS $QEMU_HOST_GATEWAY_IF pub
    
    # Set up experimental UDP proxies. Depending on the protocol of interest
    # messages in one or both directions might need to be relayed.
    #
    # UDP port 79 is used for Dialarm signals, a unidirectional proxy is
    # adequate for this but detection of hosts changing state (i.e. being
    # added to or removed from the population of cooperating systems) is far
    # more responsive if a bidirectional proxy is available.
    
    PROXY_ID=1
    case "$1" in
      tun1)
        PROXY_ID=2
        ;;
      tun2)
        PROXY_ID=3
        ;;
      tun3)
        PROXY_ID=4
        ;;
      tun4)
        PROXY_ID=5
        ;;
      tun5)
        PROXY_ID=6
        ;;
      tun6)
        PROXY_ID=7
        ;;
      tun7)
        PROXY_ID=8
        ;;
    esac
     
    # echo \* udp-broadcast-relay -f $PROXY_ID 79 $QEMU_HOST_GATEWAY_IF $1
    # /usr/local/src/udp-broadcast-relay/udp-broadcast-relay-0.3/udp-broadcast-relay \
    #        -f $PROXY_ID 79 $QEMU_HOST_GATEWAY_IF $1
    
    # Alternatively use this one which is oriented towards IP addresses
    # rather than interfaces.
    
    # Note attempt to counteract any niceness applied to Qemu itself.
    
    ps ax | grep 'udp-proxy[ ]-z 79 ' >/dev/null 2>&1
    if [ $? != 0 ]; then
      echo \* udp-proxy -z 79 $QEMU_GUEST_IP_ADDRESS
      /usr/bin/nice --adjustment=20 /usr/local/src/udp-proxy/udp-proxy -z 79 $QEMU_GUEST_IP_ADDRESS
    else
      echo \* Already running udp-proxy -z 79 $QEMU_GUEST_IP_ADDRESS
    fi
    
    # echo \* udp-proxy -z 13264 $QEMU_GUEST_IP_ADDRESS
    # /usr/local/src/udp-proxy/udp-proxy -z 13264 $QEMU_GUEST_IP_ADDRESS
    
    echo .../qemu/qemu-ifup completed.
    

_/etc/qemu-ifdown (for both Linux and Windows):_
    
    
    #!/bin/sh
     
     echo \* route del -host $QEMU_GUEST_IP_ADDRESS dev $1
    route del -host $QEMU_GUEST_IP_ADDRESS dev $1
    
    echo \* ifconfig $1 down
    ifconfig $1 down
    

In actual fact, these operations were cribbed from User Mode Linux (below) where they are embedded inside a host library. 

## Slackware x86 Guest using User Mode Linux

User Mode Linux runs a guest kernel as a standard program, i.e. there is no emulation or virtualisation involved. The guest kernel can be allocated either physical discs or filesystems contained in files. 

Put a .iso corresponding to a recent Slackware DVD in /export/uml. Unpack the initrd using zcat and cpio, save it as an ext3 image initrd_unpacked. Using dd create an empty file root_fs_slackware which will be partitioned and formatted during installation. 

Use the sources from e.g. a recent Slackware to compile kernel plus modules with ARCH=um using a suffix -uml. Save the kernel to /export/uml/linux, install the modules and then copy them into the initrd filesystem. 

Boot the UML kernel, telling it to use the initrd image and DVD iso: 
    
    
    # ./linux ubd0=initrd_unpacked ubd1=root_fs_slackware fake_ide ubd2r=slackware-13.37-install-dvd.iso rw
    

Run fdisk and setup as normal, you might need to tell it to install to /dev/ubd1 and use /dev/ubd2 for source. Finally, copy the modules onto the target filesystem. 

When complete start up like this: 
    
    
    # Routine startup of a UML guest relies on (the host) running /usr/lib/uml/uml_net
    # to condition ARP, forwarding etc.
     
    echo \* ./linux ubd0=initrd_unpacked ubd1=root_fs_slackware fake_ide ubd2r=slackware-13.37-install-dvd.iso \
            root=/dev/ubdb1 eth0=tuntap,,,192.168.1.22
    
    screen -S UML_3 \
    ./linux ubd0=initrd_unpacked ubd1=root_fs_slackware fake_ide ubd2r=slackware-13.37-install-dvd.iso \
            root=/dev/ubdb1 eth0=tuntap,,,192.168.1.22
    
    cd ..
    

Note that this is usually run from an X session, since the multiple virtual consoles appear as separate xterms. Also see [[1]](<linux/kernel/user_mode_VM.md>). 

## Debian 68k Guest using Aranym
    
    
    # apt-get install aranym
    

I (MarkMLl) note this installs bridge-utils but instead am going to use my standard Qemu-style startup scripts, which themselves were originally based on sequences executed internally by UML; note that Hercules for zSeries (above) is the odd-one-out here since the guest uses SLIP for networking. 

Referring to <https://wiki.debian.org/Aranym/Quick>, download <https://www.freewrt.org/~tg/dc11m68k.tgz> and check its signature. Unpack, noting a kernel vmlinuz-2.6.39-2-atari, a filesystem image dc11.img.xz and a configuration file aranym.config. 

In aranym.config, change the ETH0 section to read: 
    
    
    Type = ptp
    HostIP = 192.168.22.1
    AtariIP = 192.168.22.22
    Netmask = 255.255.255.0
    

Change the startup script runaranym to read: 
    
    
    #!/bin/sh
     
    QEMU_GUEST_IP_ADDRESS=192.168.22.22
    QEMU_GUEST_IP_GATEWAY=192.168.22.1
    QEMU_HOST_GATEWAY=eth0
    export QEMU_GUEST_IP_ADDRESS QEMU_GUEST_IP_GATEWAY QEMU_HOST_GATEWAY
    
    /etc/qemu-ifup tap7
    
    cd "$(dirname "$0")"
    SDL_AUDIODRIVER=dummy; export SDL_AUDIODRIVER
    aranym-mmu -l -c aranym.config
     
    /etc/qemu-ifdown tap7
    

Uncompress the image file: 
    
    
    # unxz dc11.img.xz
    

Using xterm on a graphical login, run the startup script: 
    
    
    # runaranym
    

The result of that should be a console for the guest Debian system. 

On the guest console, login as root with password root, and immediately change the password to something appropriate using the passwd command. Change the hostname in /etc/hostname and /etc/hosts, the IP address etc. in /etc/network/interfaces, and the nameserver in /etc/resolv.conf. Reboot and check that the network is operational by pinging from the guest to the site router (e.g. 192.168.1.1) and then pinging the guest (192.168.22.22) from any convenient system; if this doesn't work fix it before continuing. 

Then as described on <https://wiki.debian.org/Aranym/Quick> run the three commands: 
    
    
    # dpkg-reconfigure openssh-server
    # apt-get update
    # apt-get install popularity-contest
    

Finally edit /root/profile to remove the reminder to run the above. It should now be possible to login using SSH, and to continue to configure and use the system like any Debian guest. 

## Debian zSeries Guest using Hercules, without VM

Hercules is a commercial-grade emulator for IBM mainframes, it is available as a standard package for e.g. Debian and related Linux distributions. Once the emulator is running, enter 
    
    
    ipl 120
    

to boot Linux from device 120. Hopefully SSH will be operational so you won't need to interact with the console, but if you do then prefix each line that is to go to the guest operating system (i.e. rather than to the console itself) with a dot. 

Refer to the URL in the script below for more details. 

Startup: 
    
    
    #!/bin/sh
     
    # PREREQUISITE: Boot with ipl 120
     
    # Note that this makes no attempt to support IPv6.
    
    iptables -t nat -A POSTROUTING -o eth1 -s 192.168.22.0/24 -j MASQUERADE
    iptables -A FORWARD -s 192.168.22.0/24 -j ACCEPT
    iptables -A FORWARD -d 192.168.22.0/24 -j ACCEPT
    echo 1 > /proc/sys/net/ipv4/ip_forward
    echo 1 > /proc/sys/net/ipv4/conf/all/proxy_arp
    
    # http://www.josefsipek.net/docs/s390-linux/hercules-s390.html
    
    screen -S HERC_5 \
    hercules
    
    cd ..
    

Configuration: 
    
    
    CPUSERIAL 000069        # CPU serial number
    CPUMODEL  9672          # CPU model number
    MAINSIZE  256           # Main storage size in megabytes
    XPNDSIZE  0             # Expanded storage size in megabytes
    CNSLPORT  3270          # TCP port number to which consoles connect
    NUMCPU    2             # Number of CPUs
    LOADPARM  0120....      # IPL parameter
    OSTAILOR  LINUX         # OS tailoring
    PANRATE   SLOW          # Panel refresh rate (SLOW, FAST)
    ARCHMODE  ESAME         # Architecture mode ESA/390 or ESAME
    
    # .-----------------------Device number
    # |     .-----------------Device type
    # |     |       .---------File name and parameters
    # |     |       |
    # V     V       V
    #---    ----    --------------------
    
    # console
    001F    3270
    
    # terminal
    0009    3215
    
    # reader
    000C    3505    /export/zlinux/rdr/kernel.debian /export/zlinux/rdr/parmfile.debian /export/zlinux/rdr/initrd.debian autopad eof
    
    # printer
    000E    1403    /export/zlinux/prt/print00e.txt crlf
    
    # dasd
    0120    3390    /export/zlinux/dasd/3390.LINUX.0120
    0121    3390    /export/zlinux/dasd/3390.LINUX.0121
    
    # tape
    0581    3420
    
    # network                               s390     realbox
    # 0A00,0A01  CTCI -n /dev/net/tun -t 1500 10.1.1.2 10.1.1.1
    0A00,0A01  CTCI -n /dev/net/tun -t 1500 192.168.22.21 192.168.1.22
    

Note that the guest network is configured as SLIP via a simulated CTC (Channel To Channel) device. Best not fooled with. 

## Debian zSeries Guest using Hercules, with VM

This combination is unlikely to work using freely-available software, since Linux requires at least an S/390 G3 system while the most recent IBM VM available is VM/370. It might be technically possible to run a non-free VM on Hercules, but at the time of writing it is at best unclear whether this could be done legally. 

This means that it is not, for example, possible to run a VM host with simultaneous Linux and MUSIC/SP guests. 

## MUSIC/SP using Sim/390 or Hercules

_This section is an outline only. Note that in the IBM ecosystem OS and VM are the names of specific operating systems, rather than being a generic abbreviation; in addition DOS is a '60s-era IBM operating system with no connection to Microsoft or PCs._

MUSIC/SP is a freely-available (but not open source) operating system which implements a subset of IBM's MVS API, i.e. is to some extent compatible with operating systems of the OS/360 lineage in particular OS/VS1, extended with some novel features including a filesystem with user-accessible directories. It does not provide an API compatible with Linux, and uses the EBCDIC character set. 

Unlike other freely-available OS-compatible operating systems (see for example [[2]](<http://www.ibiblio.org/jmaynard/>), [[3]](<http://gunkies.org/wiki/Installing_VM/370_on_Hercules>), and the section below), MUSIC/SP provides TCP/IP networking. However this requires that the IUCV (Inter User Communication Vehicle) be provided by the underlying platform and that there is a suitable network support kernel for it to talk to: these are normally made available by IBM's VM operating system (VM/SP or VM/ESA, a sufficiently-recent version of which is not freely available). 

Considering emulated environments, IUCV can be provided either by running a recent VM on top of Hercules, or by running the freely-available (but not open source) Sim/390 emulator on Windows. Hercules does not provide IUCV or a network support kernel directly (although as of early 2012 this might be being addressed), so while MUSIC/SP will run on Hercules it will not have TCP/IP-based networking facilities: use Sim/390 if you really need this. 

Regrettably, the maintainer of MUSIC/SP and Sim/390 is no longer active, and while binaries and documentation remain available for download via [[4]](<http://www.canpub.com/teammpg/de/mcgweb/>) the sources are not. 

## VM/370 using Hercules

_This section is an outline only. Note that in the IBM ecosystem OS and VM are the names of specific operating systems, rather than being a generic abbreviation; in addition DOS is a '60s-era IBM operating system with no connection to Microsoft or PCs._

VM/370, which is freely available as the "SixPack" (currently v1.2 as of early 2013), provides a hypervisor running on the "bare metal" which can host multiple single- or multitasking operating systems such as the (provided) Conversational Monitor System (CMS) or derivatives of DOS or OS (e.g. VSE or MVS). 

The CMS interactive environment is a distant cousin to unix, and is probably usable by anybody who remembers MS-DOS or older operating systems; the "SixPack" includes extensive but not complete online help. There is no networking API exposed to user programs: code and data may be moved between the host computer (running Hercules) and guest sessions by mounting files simulating tape devices, by simulations of an 80-column card punch and reader, or by a simulation of a line printer. 

In common with other IBM operating systems of the era, the documented API is in the form of macros for an IBM-compatible assembler; other types of interface are available including diag codes for communication between CMS and VM, and virtualised hardware access using Channel Control Words (CCWs). The services provided are roughly comparable with MS-DOS v1 or CP/M, i.e. there are separate sets of macros for handling different classes of peripherals: do not expect to open the terminal, card reader or printer as a file. Particularly if the S/380 hack (see below) is applied, the GCC compiler and standard libraries may be used but most software development and maintenance is done using assembler. 

## IBM S/370, S/390, and the S/380 "hack"

_This section is an even sketchier outline._

As discussed elsewhere, the S/360 and S/370 architectures are limited to 24-bit addresses while the S/390 allows 31-bit addresses without, in general, breaking compatibility. There is an unofficial extension of the Hercules emulator (frowned upon by purists) that implements a non-standard "S/380" architecture and modifies the most-recent freely-available IBM mainframe operating systems (VSE nee DOS, MVS nee OS, and VM/370) to exploit this extension. Using this, there is sufficient available memory to run a large-scale compiler such as GCC _natively_ on one of the classic IBM operating systems, with the important caveats that only one program can use this facility at a time (i.e. while a 31-bit GCC and a 24-bit make should work, two copies of the 31-bit GCC won't), and that in the case of VM one program means one program per computer rather than one program per virtualised system/login. 

To make use of this, you need the Hercules-380 patch from [[5]](<http://mvs380.sourceforge.net/>), a classic operating system such as the VM/CMS "sixpack" from [[6]](<http://vm370.31bits.net/vm370sixpack-1_2.zip>), and possibly the MECAFF enhancement for additional terminal sessions and the IND$FILE program. In practice, it is impossible to do any of this without joining Yahoo!, and subscribing to the Hercules-VM370, hercules-os380 and H390-VM groups. 

## IA-64 (Itanium, Merced, McKinley etc.)

_Terminally sketchy._

The FPC port targeting IA-64 exists in absolutely minimal form, i.e. a few skeleton files and that's about it. Since the IA-64 architecture appears to be heading in the same direction as its predecessor the iAPX-432, it's highly questionable whether any more work will be done on this architecture. However, for completeness (and because this author wants to be able to inspect some Itanium binaries)... 

While a few systems turn up on eBay etc., the asking price tends to be dictated by what the seller paid a few years ago rather than by what anybody expects to pay today. There is a simulator, written by HP, called "Ski", which is now open source. See [[7]](<http://www.gelato.unsw.edu.au/IA64wiki/SkiSimulator>), [[8]](<http://www.hpl.hp.com/research/linux/ski/download.php>) plus the Sourceforge project repository. 

(On a fairly complete development system running Debian) get the most recent Ski sources from Sourceforge. Run autogen.sh, installing e.g. autoconf, libtool and libelf-dev if necessary. Run make, install additional libraries such as gperf, bison, flex etc. as necessary. It might be necessary to edit syscall-linux.c to comment out the reference to asm/page.h which apparently isn't needed for very recent (Linux 3.x) kernels. On success get root and run make install. 

Assuming the disc image is a file named /export/ia-64/sda (e.g. renamed from the downloaded sda-debian) then 
    
    
    bski bootloader vmlinux simscsi=/export/ia-64/sd
    

Note the dropped final letter, that parameter is used as a prefix rather than being the name of a file or directory. 

As with a number of other emulators described on this page, this requires an X session for the console. 

This writer (MarkMLl) can't get networking running, either using the instructions at [[9]](<http://www.hpl.hp.com/research/linux/ski/HOWTO-ski-networking.php>) or using a tun/tap solution very roughly as suggested by [[10]](<http://www.gelato.unsw.edu.au/IA64wiki/SkiVirtualNetwork>). It might be necessary to try a much older host kernel, i.e. late 2.4 or early 2.6, however this is not currently being pursued due to lack of relevance. 

The kernel from HP is 2.4.20, the filesystem is described as "Debian Sid" but from the kernel version is probably somewhere between "Woody" (3.0) and "Sarge" (3.1). I don't know how easy it's going to be to get this up to scratch, at the very least a working network is going to be a prerequisite. 

* * *

## Relative Performance

Performance is, in general, disappointing. Using an informal "torture test" which exercises the CPU and disc access: 
    
    
    Compaq ProLiant ML530 G2, 2.8GHz, 3Gb, 8 jobs, 390W 0m12.170         79
    
    Linksys NSLU2, 266MHz 32Mb, 1 job, 7W               6m35.014s        46
    
    Qemu ARM, 1 job, 390W                              42m58.925s    16,757
    
    Qemu MIPS, 1 job, 390W                             17m49.103s     6,949
    
    Qemu x86, 1 job, 390W                              47m0.441s     18,330
    
    UML, 1 job, 390W                                    8m26.529s     3,289
    
    Hercules zSeries, 4 jobs, 390W                      9m43.330s     3,790
    

The final column in the above list is the W-Mins to complete the job. These timings are without the benefit of kernel support from kqemu (obsolete) or KVM, but the fact that Qemu's support for MIPS is significantly better than that for other architectures, and the fact that the Hercules emulator is in all cases better than Qemu, does make one wonder how efficient the code is. 

Compiling FPC 2.4.2 using time make NOGDB=1 OPT='-O- -gl' all : 
    
    
    Host (x86, Linux, Debian):
    
      real    4m47.842s
      user    3m42.126s
      sys     0m33.506s
    
    Slug (32Mb, ARM, Linux, Debian):
    
      real    284m58.543s
      user    86m45.570s
      sys     20m46.500s
    
    Qemu (ARM, Linux, Debian):
    
      real    406m31.931s
      user    236m49.030s
      sys     148m58.110s
    
    Qemu (x86, Linux, Slackware):
    
      real    141m45.700s
      user    122m40.724s
      sys     17m15.670s
    
    Qemu (x86, Windows 2000):
    
      Elapsed 108m
    
    UML (x86, Linux, Slackware):
    
      real    238m41.257s
      user    45m54.460s
      sys     3m44.140s
    

Compiling Lazarus 0.9.30 using time make LCL_PLATFORM=gtk2 bigide : 
    
    
    Host (x86, Linux, Debian):
    
      real    2m21.072s
      user    2m6.452s
      sys     0m12.285s
    
    Slug (32Mb, ARM, Linux, Debian):
    
      real    9385m49.319s
      user    67m23.460s
      sys     430m9.870s
    
    Qemu (ARM, Linux, Debian):
    
      real    281m55.536s
      user    153m3.150s
      sys     53m27.470s
    
    Qemu (x86, Linux, Slackware):
    
      real    70m53.957s
      user    60m3.474s
      sys     8m8.801s
    
    Qemu (x86, Windows 2000, default platform):
    
      Elapsed 53m
    
    UML (x86, Linux, Slackware):
    
      real    489m40.233s
      user    81m43.740s
      sys     7m51.280s
    

## Graphical Access using VNC etc.

All of the above assumes that SSH is available so that shell (command line) sessions can be run over the network. Where necessary install e.g. the Debian ssh (or openssh, openssh-server etc.) package. It is also possible to run a program using a GUI over SSH by X forwarding, see the SSH client's -X option and the corresponding server configuration-file settings. 

There are several ways of running a program using a GUI without forwarding the X protocol over SSH: 

  * Running Qemu or UML in an xterm.


  * Using Qemu's host VNC support.


  * Connecting to a Qemu (or UML etc.) guest using X.


  * Connecting to a Qemu (or UML etc.) guest using VNC.



In all cases it is worth using a low-resource window manager such as FluxBox or xfwm rather than KDE or Gnome. 

### Running Qemu or UML in an xterm or with host VNC support

This depends on the build options of the guest system kernel etc. In general, display resolution and depth will be limited by the emulated hardware, the network ports are associated with the host (rather than guest) system. See the Qemu documentation for details. 

### Connecting to a Qemu (or UML etc.) guest using X

Install the Gnome Display Manager (gdm) on the guest system. This allows a desktop system to log into it using XDMCP, the desktop system determines the display resolution and depth. 

### Connecting to a Qemu (or UML etc.) guest using VNC

This method also works well for arbitrary systems, even if Qemu etc. is not being used. 

Install gdm and RealVNC (or, with limitations, TightVNC) on the guest system; the latter might be needed for ARM guests but RealVNC is to be preferred when available. This allows a desktop system to log into it using VNC, the guest system determines the display resolution and depth and network ports are associated with the guest (rather than host) system. 

In /etc/inittab, insert something like this: 
    
    
    6:23:respawn:/sbin/getty 38400 tty6
    
    # Added in lieu of trying to get it working in inetd.conf. MarkMLl.
    
    8:23:respawn:/usr/local/vnc/vncshim-0-20
    # 9:23:respawn:/usr/local/vnc/vncshim-4-24
    
    # Example how to put a getty on a serial line (for a terminal)
    #
    #T0:23:respawn:/sbin/getty -L ttyS0 9600 vt100
    

Many systems limit the number of VNC servers that may be run per system (including localhost) as the side effect of a window manager XDMCP security precaution. If necessary, edit the window manager configuration file (gdm.conf, kdmrc etc.) to increase the number of sessions that each XDMCP client can request, e.g. in the case of gdm.conf: 
    
    
    [xdmcp]
    Enable=true
    DisplaysPerHost=6
    

Create /usr/local/vnc/vncshim-0-20 to read: 
    
    
    #!/bin/bash
     
    # This forms a shim between /etc/inittab and the VNC server, necessitated by
    # the facts that (a) I can't get this command working reliably in inetd.conf
    # and (b) lines in initab have restricted length.
    #
    # Note that the name of the file reflects the fact that it might request non-
    # standard combinations of VNC port and X display number. MarkMLl.
    
    # My preference is to select default linear dimensions that appear somewhere
    # in the list of conventional graphics modes. 1200x960 fits inside a 1280x1024
    # screen, even allowing that the title bar etc. will take additional space.
     
    SIZE=1200x960
     
    # The DPI setting can usefully be set to match the user's normal screen, leave
    # undefined (commented out) if in doubt.
    
    DPI='-dpi 96'
    
    # Each VNC server running on a host must have a distinct suffix to prevent port
    # and display number clashes.
    
    SUFFIX=0
    
    # Some option formats vary between RealVNC (top) and TightVNC (bottom).
    
    OPTIONS='DisconnectClients=0 -NeverShared'
    # OPTIONS='-dontdisconnect -nevershared'
    
    ####################### CHANGE NOTHING BELOW HERE ###########################
    
    VNCPORT=570$SUFFIX
    XDISPLAY=:2$SUFFIX
    
    # Try to work out what this system is. Note particular care to allow spaces
    # in the desktop name.
    
    WHATAMI=`dirname $0`/whatami
    if [ -x $WHATAMI ]; then
      WHATAMI2=`$WHATAMI`
    else
      if [ -r $WHATAMI ]; then
        WHATAMI2=`cat $WHATAMI`
      else
        WHATAMI2=$$
      fi
    fi
    DESKTOP='-desktop '\'$WHATAMI2\'
    
    # Optional Java viewer.
    
    if [ -r `dirname $0`/classes/index.vnc ]; then
      JAVA="-httpd `dirname $0`/classes -httpPort 580$SUFFIX"
    fi
     
    # This is the only way I can get it working the way I want, i.e. with reliable
    # handling of spaces in the desktop name. I'm hardly proud of it.
    
    rm -f `dirname $0`/.vncshim-$SUFFIX-1$SUFFIX
    
    cat >`dirname $0`/.vncshim-$SUFFIX-1$SUFFIX <<EOT
    #!/bin/sh
    exec /usr/bin/Xvnc -geometry $SIZE $DPI -depth 16 $DESKTOP \
            -rfbport $VNCPORT -rfbauth /root/.vnc/passwd-$SUFFIX $JAVA \
            $OPTIONS -query localhost -once $XDISPLAY
    EOT
    
    chmod +x `dirname $0`/.vncshim-$SUFFIX-1$SUFFIX
    exec `dirname $0`/.vncshim-$SUFFIX-1$SUFFIX
    

Create /usr/local/vnc/whatami to read: 
    
    
    #!/bin/bash
    
    echo -n "`hostname`, "
    _WHATAMI=`uname -m`
    if [[ $_WHATAMI == i?86 ]]; then
      _WHATAMI='i586'
    fi
    if [[ $_WHATAMI == arm* ]]; then
      _WHATAMI='arm'
    fi
    
    if [ -x `dirname $0`/whatami-$_WHATAMI ]; then
      `dirname $0`/whatami-$_WHATAMI
    else
      uname -m
    fi
    

Additional scripts e.g. whatami-arm can query /proc/cpuinfo, which is different for each architecture. Make sure that all scripts are set +x for root. 

Use vncpasswd to set up password files /root/.vnc/passwd-0 etc. 

So far, I find that RealVNC works fine when invoked as above, but TightVNC doesn't correctly invoke the display manager. 

## Accessing "Unusual" Devices

This section covers devices such as audio (i.e. used for playing .wav files) and MIDI, and USB devices which are not claimed by the host or guest kernel. 

### Sound/MIDI for Qemu

Don't expect this to be hi-fi quality, but it should be adequate for operator alerts etc. 

#### Identifying the Host's Hardware

Obviously, a good starting point for this is using lspci and lsusb to determine what physical hardware is installed. The next stage is usually to consult dmesg output and to use lsmod to determine what subsystems the kernel has loaded hence what APIs are available. Graphical tools such as the KDE Info Centre can also be useful here, however identifying which API is most useful can be a bit of a black art. 

The current writer (MarkMLl) has a Debian "Squeeze" host with USB-connected audio and MIDI devices, with the latter connected to a Yamaha tone generator. An alternative (and more conventional) configuration would be to have an internal soundcard, with both audio and MIDI output feeding a loudspeaker. 

Knowing what hardware and subsystems are available, the remaining question is which subsystems Qemu can make use of. Running this command goes some way towards providing an answer: 
    
    
    $ qemu -audio-help |grep ^Name:
    Name: alsa
    Name: oss
    Name: sdl
    Name: esd
    Name: pa
    Name: none
    Name: wav
    

For reasons that are unclear _(roughly translated: I'd like some help here)_ I've had most success with OSS, starting Qemu with a script that includes these lines: 
    
    
    export QEMU_AUDIO_DRV=oss
    export QEMU_OSS_DAC_DEV=/dev/dsp1
    export QEMU_OSS_ADC_DEV=/dev/dsp1
    

#### Selecting the Guest's Soundcard

Assuming that the guest is to have both audio and MIDI, common sense would suggest that the guest operating system should see both audio and MIDI hardware implemented by Qemu. According to <http://rubenerd.com/qemu-ad-lib-midi-win-31/>, this implies that Qemu should be recompiled with the capability of emulating an Adlib card, which for current versions of Qemu means running something like this: 
    
    
    make clean distclean
    ./configure --audio-card-list=ac97,es1370,sb16,adlib
    make
    

Irrespective of whether a custom Qemu is built or not, it's useful to check what devices it emulates: 
    
    
    $ qemu -soundhw ?
    Valid sound card names (comma separated):
    pcspk       PC speaker
    sb16        Creative Sound Blaster 16
    ac97        Intel 82801AA AC97 Audio
    es1370      ENSONIQ AudioPCI ES1370
    

For some guest operating systems (e.g. Windows NT 4), adding Adlib emulation to Qemu is sufficient to get MIDI working with the Sound Blaster, i.e. 
    
    
    $ qemu ... -soundhw sb16,adlib ...
    

In this case the guest uses only the standard Sound Blaster driver, possibly ignoring MPU-401 support. 

For guest systems which don't benefit from this, there's little point adding something like an Adlib card if the guest operating system doesn't have a driver for it- which appears to be the case with Windows 2000. The current writer's preference is to select the ES13770, since as a straightforward PCI device it should be enumerated without difficulty by almost any guest operating system: 
    
    
    $ qemu ... -soundhw es1370 ...
    

Having started Qemu, it's possible to query the emulated devices: 
    
    
    (qemu) info pci
    ...
      Bus  0, device   4, function 0:
        Audio controller: PCI device 1274:5000
          IRQ 11.
          BAR0: I/O at 0xc200 [0xc2ff].
          id ""
    

The expected result of this is that the guest operating system would see audio hardware, but not MIDI. However in the case of Windows 2000 a MIDI device is emulated, so programs which use MIDI for e.g. operator alerts will work properly. 

_Anybody: need help here generalising this to other guest operating systems._

### USB for Qemu

As an example, a Velleman K8055 board is plugged into the host. First use lsusb to gets its vid:pid identifiers: 
    
    
    $ lsusb
    Bus 001 Device 005: ID 10cf:5500 Velleman Components, Inc. 8055 Experiment Interface Board (address=0)
    

Now add these parameters to the Qemu command line: 
    
    
    qemu ... -usb -usbdevice host:10cf:5500 ...
    

On the guest, lsusb should show the device available for use. 

The Velleman board is unusual in that it has ID jumpers which set the last digit, i.e. multiple devices appear as 10cf:5500 through 10cf:5503. In cases where this facility is not available, the bus and device number can be used: 
    
    
    qemu ... -usb -usbdevice host:1.5 ...
    

Obviously, there's a risk here that devices will move around between invocations. 

### USB for UML

There was supposedly a patch that mapped unused USB devices on the host into the guest. I'm not sure it made it into the standard release. 

## QEMU User Emulation Mode in Chrooted Environment

In previous chapters QEMU's full system emulation mode was described: a complete guest system is emulated including bios and hardware. Although this approaches a real environment as close as possible, the overhead created by emulating a complete system is considerable. QEMU's user emulation mode allows to run "guest" code directly on the "host" system. The cpu is still emulated but system calls are translated into system calls on the host system. Libraries used by the "guest" code are the "guest" libraries, not the host libraries. To run a small program created for fe. ARM with no dependencies you can simply do a 
    
    
    $ qemu-arm program
    

When the program has more and more dependencies, it becomes increasingly difficult to put all dependencies in locations that can be found by qemu-arm without messing up your host system. The solution that will be developed here is to create a chroot environment on the host in which all code is "guest" code. Advantage of this solution: 

  * the chroot isolates the guest from the host
  * no need for vnc, ssh or any other solution to export screens.
  * no booting of a guest OS system.
  * guest "immersion" for user programs is almost complete. Only very low level and hardware related programming will notice the difference.
  * a spectacular speed improvement for graphics (X) related tasks (running lazarus f.e.)
  * hard disk resources are common to host and guest. Host can read and modify "guest" files but not the other way around.



_Setting up the system._

For a better comprehension of what follows, here is the directory structure used in this how-to: 
    
    
    home
     qemu
       arm-linux-user
       sparc-linux-user
       scripts	
       disks       // here are qemu images for the different systems emulated
       chroot
           mount	
           arm	// here we'll chroot the arm environment
           sparc	
    

Get and build the latest qemu 
    
    
    $ cd 
    $ git clone git://git.savannah.nongnu.org/qemu.git qemu
    $ cd qemu
    

Build all emulations (full system and user emulation mode, all processors) 
    
    
    $ ./configure --static to build
    

or only the user emulations needed 
    
    
    $ ./configure --static --target-list=sparc-linux-user,arm-linux-user
    
    $ make clean all
    

The --static option makes that qemu-arm and qemu-sparc are linked statically. Since they will be running in the chroot environment later on, they must be build without dependencies. Some distributions provide already statically linked binaries. fe qemu-arm-static on debian. Although they are not the latest and greatest, using these packages makes it somewhat easier. Building QEMU from source is really just a matter of minutes. 

Now we need to copy the complete guest system into chroot/arm or chroot/sparc. There are several ways of doing this but when you don't have a real arm or sparc system at hand, using a QEMU image of a full system emulation as described in previous chapters is the easiest. 

Mount the QEMU image. 

WARNING: do not mount a qemu image when the associated guest is running! 

Only raw QEMU images can be mounted as a loop-back device. If you have a qcow formatted image, convert it to a raw image. 
    
    
    $ qemu-img convert disks/armel_hda.img  -O raw disks/armel_hda.raw
    
    $ cd chroot 
    $ su
    # mount ../disks/armel_hda.raw mount -o loop,offset=32256
    

The offset is the start of the first partition on the ARM disk when the default disk layout was used during installation. If mount fails then the offset is probably wrong. To calculate the offset: 
    
    
    # fdisk -lu ../disks/armel_hda.raw
    

Find the first sector of the partition that will be mounted as root. Calculate offset as (start sector number * 512). For the default SPARC debian etch installation using a sun partition table with a /boot first partition the offset is 98703360. 

Check if the correct partition is mounted: 
    
    
    # ls mount
    

Copy the complete system 
    
    
    # cp -r mount/* arm/
    # umount mount
    

Copy the QEMU arm emulator to the arm directory 
    
    
    # cd arm
    # cp ../../arm-linux-user/qemu-arm usr/local/bin
    

Our "guest" system is complete. Since we are sharing our "host" devices and system, we need to mount a few special directories: 
    
    
    # mount --bind /proc proc
    # mount --bind /dev dev
    # mount --bind /sys sys
    # mount --bind /tmp tmp
    

Why /tmp? The /tmp directory is used by the X window system. When you don't mount tmp on the host /tmp, you will get "can't open display 0:0" errors when trying to open a window in the guest. 

We are going to use the host network: 
    
    
    # sudo cp /etc/resolv.conf etc
    

Remains one last thing: the emulator is copied into the guest but how do we tell the system that the emulator needs to kick in when a "quest" elf is detected? This is done with binfmt_misc. A configuration script is included with QEMU that registers the QEMU user mode emulators with binfmt_misc: (on my system I had to do a chmod +x on the script) 
    
    
    # ../../scripts/qemu-binfmt-conf.sh
    

When you look at this script at the end of the binary data you'll notice that the emulator is registered as /usr/local/bin/qemu-arm. And that is where we put it a few lines ago. 

Launch the system: 
    
    
    # chroot .
    

If all goes well: 
    
    
    # uname -m
    armv7l
    

Everything you do from here is in your "guest" system. To exit the "guest" system. 
    
    
    # exit
    

And unmount the special directories. 
    
    
    # umount proc
    # umount dev
    # umount sys
    # umount tmp
    

_Conclusion._

The ARM emulation works very well, except for gdb. On the contrary there are still quite some problems with the SPARC emulation. There is a bug in duplicating pipes which makes the use of bash impossible. The sh shell works but several unix commands don't function correctly. What works fine is fpc and lazarus! Programs can't be launched from inside lazarus. Because of the improved graphics speed when using the user emulation mode it is still worthwhile for using lazarus. Instead of starting a shell in chroot, you start directly lazarus: 
    
    
    $ sudo chroot . /your/lazarus/dir/lazarus
    

To run the program compiled in lazarus, you can launch it from another "host" terminal: 
    
    
    $ cd qemu/chroot/arm
    $ sudo chroot . /your/program/dir/project
    

Here also gdb is not working. 

## Docker images

Docker uses various APIs provided by the underlying operating system to isolate programs in separate namespaces. 

The implication of this is that every Docker container has its own process space (i.e. processes in different containers might have the same numeric PID), network space (processes in different containers can have different routing, firewall rules and so on), filesystems (except that they share kernel files and a number of crucial libraries and configuration files) and so on. 

Containerisation is distinct from virtualisation in that all containers which are running on the same host computer make use of the same host kernel, it is not possible for one container to use a different kernel (e.g. for development or deployment test purposes), and there is no absolute guarantee that a container taken over by malicious code cannot subvert the remainder of the system. 

_**Note that there are pitfalls for the unwary. For example, having Docker installed and the Docker daemon running can break Qemu networking if this expects to have bridged access to the host interface rather than to be sitting on its own private subnet.**_

One thing that does work however, is running a 32-bit container on a 64-bit host, and in most cases the Linux kernel APIs are sufficiently backwards-compatible that the programs and libraries from an older release of a distro can run on a much more recent host. One of the cases which will be discussed here runs a container containing 32-bit Debian "Lenny" from 2008 on a host running Debian "Buster" from 2019, this combination allows e.g. compatibility with the GTK v1 widget set (no longer supported by more recent distreaux) to be tested. 

### Docker fundamentals

This is not the place for a full discussion of the care and feeding of Docker, but it's worth establishing a few basic concepts. 

  * Host: the physical computer running e.g. Qemu (with or without KVM) or Docker.


  * Guest: on a virtualised system (Qemu etc.), a completely separate memory space with its own kernel.


  * Container: on a Docker based system, a group of application programs sharing the host kernel but largely isolated from the rest of the host system using various namespace mechanisms.



Looking specifically at Docker: 

  * Dockerfile: a succinct description of what is to go into a Docker image.


  * Image: the Docker analogue of a bootable filesystem, initially built with reference to a Dockerfile.


  * Container: an image which has been run, whether or not it is currently running.



Once running, a container has an associated parameter list which specifies the main program together with any sockets etc. exposed to the host. Neither an image nor a container correspond to a single file on the host, but: 

  * A container may be stopped and committed to an image.


  * An image may be saved to a single file, in which state it can be moved between hosts.


  * An image may be run a set of parameters different from those of the container committed to it.



The result of this is that if something is installed in the context of a container (e.g. Apache is added from the operating system's main repository) the container should be stopped, committed to a new image and then run with an additional parameter specifying what sockets etc. should be exposed. There are ways around that, but that combination appears to be the easiest to manage. 

The Docker community has a master repository, and the docker program on a host computer may search it to locate images. However in the context of those images there appears to be no "official" way of searching the tags that allow a specific version to be pulled, hence something "messy" and subject to change like 
    
    
    $ docker search debian
    
    $ wget -q https://registry.hub.docker.com/v1/repositories/debian/tags -O - \
            | sed -e 's/[][]//g' -e 's/"//g' -e 's/ //g' | tr '}' '\n'  | awk -F: '{print $3}' | grep buster
    
    $ docker pull debian:buster
    

or alternatively (for the really hard to reach bits) 
    
    
    $ docker search vicamo
    
    $ wget -q https://registry.hub.docker.com/v1/repositories/vicamo/debian/tags -O - \
            | sed -e 's/[][]//g' -e 's/"//g' -e 's/ //g' | tr '}' '\n'  | awk -F: '{print $3}'
    
    ...
    buster-i386
    buster-i386-sbuild
    buster-i386-slim
    ...
    
    $ docker pull vicamo/debian:buster-i386-slim
    

Generally speaking, if a developer on e.g. Github says that he has a special-purpose image, some combination of the above commands will allow the required version (e.g. in the above example tagged buster-i386-slim) to be pulled. 

Neither a container nor an image is directly mountable e.g. by using losetup. It is possible to copy files to or from a container using the `docker container cp` command, subject to the same precautions that one would take on a running unix system. 

### A 64-bit Docker container with basic utilities

This is a basic installation of a 64-bit Debian stable release ("Buster" at the time of writing) on the same host version. This is useful in a situation where a developer wants to experiment with libraries etc. that he does not normally have installed, it should probably not be trusted to quarantine untrusted code. 

In the instructions below, commands on the host are run in the context of a normal user so are shown prefixed by a $ prompt, while commands run in the context of the container are run by root so shown prefixed by a # prompt. 
    
    
    $ docker pull debian:buster
    
    $ docker run --interactive --tty debian:buster
    
    # apt-get update
    
    # apt-get install dialog tasksel python3
    
    # tasksel --new-install
    
    # dpkg-reconfigure locales
    

Using tasksel, remove desktop and print server and add SSH. The author favours installing net-tools so that the `netstat -an` command can be used to check socket accessibility. 
    
    
    # apt-get install net-tools elvis-tiny
    

So far there is no provision for starting the SSH daemon. Add or edit /etc/rc.local to contain 
    
    
    #!/bin/sh
    
    rm -rf /tmp/*
    
    # For cases where a 32-bit container is running (by implication on a 64-bit host), make
    # sure that commands such as  uname -a  return consistent information to avoid breaking 
    # the traditional  ./configure && make  operation.
    
    (file -L /bin/sh | grep -q 'ELF 32-bit LSB')
    if [ $? -eq 0 ] ; then
      LINUX32=linux32
    fi
    
    cd /etc/init.d
    $LINUX32 ./ssh start
    # $LINUX32 ./openbsd-inetd start
    
    cd
    exec $LINUX32 /bin/bash
    

The ID of the current container is in the prompt. Exit it and commit to a new image, then start with a parameter to expose the SSH socket: 
    
    
    # exit
    
    $ docker container commit 4efab4804fb6 debian_minimal
    
    $ docker run -it -p 65022:22 \
        --entrypoint=/etc/rc.local debian_minimal
    
    # groupadd -g 1000 markMLl
    
    # useradd -g markMLl -u 1000 -m -d /home/markMLl markMLl
    
    # passwd markMLl
    

Add newly created user name to sudo group (/etc/group and /etc/gshadow files). Note the container ID, if it is now stopped the parameters are preserved and it should be possible to start it in the background and connect as the above user via SSH. 
    
    
    $ docker container stop c5be7fd96866
    
    $ docker container start c5be7fd96866
    
    $ ssh localhost -p 65022
    

The above is a good starting point for a general-purpose containerised system. The section below will also show how to get a graphical program (e.g. the Lazarus IDE) running in a container. 

### A 32-bit Docker container with FPC and Lazarus

An understanding of the principles and commands described above is a prerequisite to continuing with this section. 

Unlike Qemu, a container will run at the host computer's full speed. Unlike User Mode Linux (UML), a 32-bit container will run on a 64-bit host. Subject to security issues, this makes Docker the preferred choice when setting up a temporary 32-bit environment to e.g. investigate libraries which would not normally be installed on the host. 

This section discusses installing a 32-bit Debian "Lenny" container, which allows a program to be checked with libraries etc. dating from roughly 2008. 

Referring to <https://github.com/lpenz/docker-debian-releases/>
    
    
    $ docker pull lpenz/debian-lenny-i386
    
    $ docker image ls
    
    $ docker run --interactive --tty lpenz/debian-lenny-i386
    

Lenny authentication is broken and is not going to be fixed (short of forking the entire repository). In /etc/apt/apt.conf put 
    
    
    APT 
    {
      // Options for apt-get
      Get 
      {
         AllowUnauthenticated "true";
      }
    }
    

which should allow tasksel etc. to work. Add contrib and non-free to /etc/apt/sources.list, then 
    
    
    # apt-get update
    
    # apt-get upgrade
    
    # tasksel --new-install
    
    # dpkg-reconfigure locales
    
    # apt-get install openssh-server sudo
    

Populate /etc/rc.local as discussed above, comment out the temporary setting from /etc/apt/apt.conf 

Create group and user as discussed above, add user to sudo group. 

Add the build-essential, gdb, subversion and zip packages as basic prequisites. Add libncurses5-dev, libncursesw5-dev and libgpm-dev to allow FPC to be rebuilt, plus possibly libpango1.0-dev for the FP IDE. Install FPC v2.2.4 binary for i386 and corresponding sources, the compiler should run and be able to recompile itself. 

Install the libgtk1.2-dev package then Lazarus 0.9.24.1, don't attempt to run it yet. In the per-user shell startup file e.g. ~/.bashrc put the line 
    
    
    export DISPLAY=:0
    

then exit and commit the container as described above and restart with something like 
    
    
    $ docker run -it -p 65022:22 -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix \
           --entrypoint=/etc/rc.local lenny_with_lazarus
    

Again, details as discussed in the section above. 

It should now be possible to login to the container from the host over SSH, invoke e.g. /usr/local/share/lazarus/lazarus by name, and have the IDE appear on the host computer's screen. 

## Further Reading

  * [Debian on an emulated ARM machine](<http://www.aurel32.net/info/debian_arm_qemu.php>)
  * [Debian 11 on an emulated ARM/ARM64 machine](<https://forum.lazarus.freepascal.org/index.php/topic,56098.msg420159.html#msg420159>)
  * [Debian on an emulated MIPS(EL) machine](<http://www.aurel32.net/info/debian_mips_qemu.php>)
  * [QEMU/Windows XP on Wikibooks](<http://en.wikibooks.org/wiki/QEMU/Windows_XP>)
  * [The User-mode Linux Kernel Home Page](<http://user-mode-linux.sourceforge.net/>)
  * [Installing Debian under Hercules](<http://www.josefsipek.net/docs/s390-linux/hercules-s390.html>)



## See also

  * [Native ARM Systems](<Native_ARM_Systems.md> "Native ARM Systems")
  * [Native MIPS Systems](<Native_MIPS_Systems.md> "Native MIPS Systems")
  * [Small Virtual Machines](<Small_Virtual_Machines.md> "Small Virtual Machines")
  * [Docker Containerization](<Docker_Containerization.md> "Docker Containerization")

---

_Source: [https://wiki.freepascal.org/Qemu_and_other_emulators](https://web.archive.org/web/20241013003940/https://wiki.freepascal.org/Qemu_and_other_emulators)_
