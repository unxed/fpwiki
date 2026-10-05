# Free Pascal on OSv

## Contents

  * 1 OSv
  * 2 Installation
    * 2.1 Install OSv on Linux
  * 3 Building an instance with capstan
    * 3.1 Capstanfile
    * 3.2 Pascal Library
    * 3.3 hello.pas
    * 3.4 Building
    * 3.5 Running
  * 4 Pascal Library without using the System unit
    * 4.1 hello.pas
    * 4.2 Compile a library that does not call the system init
    * 4.3 Building
    * 4.4 Running



# OSv

OSv is an operating system based on Linux optimized for the cloud. [Official page](<http://osv.io/>) It runs a single application together with a small kernel and all needed libraries in a virtual machine. That means to run OSv applications you need to install a hypervisor such as QEMU/KVM or VirtualBox. 

# Installation

## Install OSv on Linux

This worked on 3rd Mar 2015 on Ubuntu 14.10 and on 3rd Sep 2016 on Ubuntu 16.04: 

Please read <http://osv.io/run-locally/>. Especially about the "Data collection". 

Downloading capstan to ~/bin/capstan: 
    
    
    curl <https://raw.githubusercontent.com/cloudius-systems/capstan/master/scripts/download> | bash
    

Install needed qemu-kvm (VirtualBox is also possible, but not handled in this document) 
    
    
    sudo apt-get install qemu-kvm
    

# Building an instance with capstan

Capstan takes a basic image and adds your libraries to create a VM image. It then starts the VM and runs the 'main' function of your library. At the moment only Linux/amd64 dynamic libraries with PIC are supported. 

## Capstanfile

Capstan needs a config file named _Capstanfile_. Read [here](<https://github.com/cloudius-systems/capstan/blob/master/Documentation/Capstanfile.md>) for details. 

Create a directory for the example. For instance ~/osv_fpc_test. 

Here is an example config for a hello world program (~/osv_fpc_test/Capstanfile): 
    
    
    # Name of the base image. Capstan will download this automatically from Capstan repository. 
    base: cloudius/osv-base
    
    # The command line passed to OSv to start up the application.
    cmdline: /tools/hello.so
    
    # The command used to build the application.
    build: ./compile.sh
    
    # List of files that are included in the generated image.
    files:
      /tools/hello.so: hello.so
    

## Pascal Library

The small Linux kernel does not support all syscalls needed by the default FPC Linux System unit. 

See here for some details what OSv does not support: <https://github.com/cloudius-systems/osv/wiki/Porting-native-applications-to-OSv>

You need to build the System unit using Libc (tested with fpc trunk revision 34418): 
    
    
    cd fpc_src_3.1.1
    make clean all OPT='-dFPC_USE_LIBC -Fl/usr/lib/gcc/x86_64-linux-gnu/5'
    sudo make install INSTALL_PREFIX=/usr
    

## hello.pas
    
    
    library hello;
    
    // start function for OSv
    function main: longint; cdecl;
    begin
      Writeln('It works!');
      main:=0;
    end;
    
    exports main name 'main'; // OSv searches for 'main' in the library
    
    end.
    

Here is the _~/osv_fpc_test/compile.sh_ : 
    
    
    #!/bin/bash
    # create a dynamically linked library hello.so
    /usr/lib/fpc/3.1.1/ppcx64 -fPIC -XD -Xc -gw2 -ohello.so hello.pas
    

## Building

**Note** : The first time you build, capstan will download the base VM image "cloudius/osv-base" (~/.capstan/). 
    
    
    []$ cd ~/osv_fpc_test
    []$ ~/bin/capstan build
    Building osv...
    Uploading files...
    1 / 1 [===================================] 100.00 %
    

## Running
    
    
    []$ ~/bin/capstan run
    Created instance: helloworld
    OSv v0.18
    eth0: 192.168.122.15
    It works!
    

  


# Pascal Library without using the System unit

As demonstration here is a library, that does not call the system unit (works the normal FPC system unit): 

## hello.pas

Here is the _~/osv_fpc_test/hello.pas_ : 
    
    
    library hello;
    
    uses
      unixtype;
    
    // use the C function 'write'
    function CWrite(fd : cInt; buf:pChar; nbytes : unixtype.TSize): TSsize;  external name 'write';
    
    // start function for OSv
    function main: longint; cdecl;
    const
      MyText: PChar = 'It works!'#10;
    begin
      CWrite(StdOutputHandle,MyText,strlen(MyText));
      main:=0;
    end;
    
    exports main name 'main'; // OSv searches for 'main' in the library
    
    end.
    

## Compile a library that does not call the system init

Here is the _~/osv_fpc_test/compile.sh_ : 
    
    
    #!/bin/bash
    
    # compile without linking, create a dynamically linked library hello.so
    fpc -fPIC -XD -Xc -g -s -ohello.so hello.pas
    # link without -init FPC_SHARED_LIB_START -fini FPC_LIB_EXIT
    /usr/bin/ld -b elf64-x86-64 -m elf_x86_64 -soname hello.so -shared -L. -o hello.so link.res
    

## Building

**Note** : The first time you build, capstan will download the base VM image "cloudius/osv-base" (~/.capstan/). 
    
    
    []$ cd ~/osv_fpc_test
    []$ ~/bin/capstan build
    Building osv...
    Uploading files...
    1 / 1 [===================================] 100.00 %
    

## Running
    
    
    []$ cd ~/osv_fpc_test
    []$ ~/bin/capstan run
    Created instance: osv
    OSv v0.18
    eth0: 192.168.122.15
    It works!

---

_Source: [https://wiki.freepascal.org/Free_Pascal_on_OSv](https://web.archive.org/web/20250215131809/https://wiki.freepascal.org/Free_Pascal_on_OSv)_
