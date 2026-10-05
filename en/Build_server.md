# Build server

Build server with automated build process is not useful just for big developers teams but also for single developer who working on multiple small applications. Build server can run daily or on-demand build for multiple projects and send notification emails if build was not successful. Build server can also help with cross-platform projects where developer would need to run build for all supported builds manually. 

# Setup on Ubuntu

Assume 64-bit Ubuntu server. 

Install binutils for required targets: 
    
    
    sudo apt-get install binutils-powerpc-linux-gnu
    sudo apt-get install binutils-powerpc64le-linux-gnu
    sudo apt-get install binutils-arm-linux-gnueabi
    

Create **ld** and **as** scripts needed for FPC: 
    
    
    #!/bin/bash
    
    cat >/usr/bin/i386-linux-ld << EOF
    #!/bin/bash
    ld -A elf32-i386 $@
    EOF
    chmod +x /usr/bin/i386-linux-ld
    
    cat >/usr/bin/i386-linux-as << EOF
    #!/bin/bash
    as --32 $@
    EOF
    chmod +x /usr/bin/i386-linux-as
    
    cat >/usr/bin/arm-linux-ld << EOF
    #!/bin/bash
    /usr/bin/arm-linux-gnueabi-ld $@
    EOF
    chmod +x /usr/bin/arm-linux-ld
    
    cat >/usr/bin/arm-linux-as << EOF
    #!/bin/bash
    /usr/bin/arm-linux-gnueabi-as $@
    EOF
    chmod +x /usr/bin/arm-linux-as
    
    cat >/usr/bin/powerpc-linux-ld << EOF
    #!/bin/bash
    /usr/bin/powerpc-linux-gnu-ld $@
    EOF
    chmod +x /usr/bin/powerpc-linux-ld
    
    cat >/usr/bin/powerpc-linux-as << EOF
    #!/bin/bash
    /usr/bin/powerpc-linux-gnu-as $@
    EOF
    chmod +x /usr/bin/powerpc-linux-as
    

# See also

  * [Jenkins](<Jenkins.md> "Jenkins")

---

_Source: [https://wiki.freepascal.org/Build_server](https://web.archive.org/web/20200814031242/https://wiki.freepascal.org/Build_server)_
