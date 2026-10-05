# Xtensa

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The Xtensa support is highly experimental and prone to be broken. This description is work in progress.

* * *

So far Linux and macOS hosts only, currently most of the work concentrates on the ESP32 variant with the lx6 cores. 

* * *

## Contents

  * 1 Xtensa-FreeRTOS
    * 1.1 Preparing FPC
    * 1.2 ESP32
      * 1.2.1 Preparations
      * 1.2.2 Build FPC
      * 1.2.3 Tips & Tricks
      * 1.2.4 Compile and run test program
    * 1.3 ESP8266
      * 1.3.1 Preparations
      * 1.3.2 Build FPC
      * 1.3.3 Tips & Tricks
      * 1.3.4 Compile and run test program
    * 1.4 Using both ESP32 and ESP8266
  * 2 Xtensa-Linux
    * 2.1 To use with QEMU
      * 2.1.1 Build binutils
      * 2.1.2 Build FPC
      * 2.1.3 Test FPC
  * 3 Xtensa-Embedded
  * 4 Emulation and debugging using qemu (ESP32)
    * 4.1 Install qemu (from source)
    * 4.2 Automated testing with qemu



## Xtensa-FreeRTOS

### Preparing FPC

A native FPC development branch ("main" in git; formerly "trunk" in svn) must be installed and working on the system and will be used to compile the xtensa cross compiler. Download the latest development source code for FPC. One straightforward option is to download the code directly from [GitLab](<https://gitlab.com/freepascal.org/fpc/source>). Click on the download button the the landing page, this will show different archive file formats to be downloaded. Click on your preferred file format to start downloading: 

[![download fpc source.png](https://wiki.freepascal.org/images/7/73/download_fpc_source.png)](</File:download_fpc_source.png>)

Once downloaded, extract all the files into a folder, such as `~/fpc/main`. 

Or use [git to clone the FPC source repository](<FPC_git.md>). Although the clone operation will download a large amount of data, subsequent updates to later revisions will only consume a small amount of data. 

Another alternative is to use [fpcupdeluxe](<fpcupdeluxe.md>) which automates all the steps below (not extensively tested). 

### ESP32

#### Preparations

Install esp-idf based on the instructions given here: 

[Get Started (for Stable Release)](<https://docs.espressif.com/projects/esp-idf/en/stable/get-started/index.html>)

Follow the instructions to a least step 8 where you build the hello_word demo project. 

Copy all the freshly compiled .a files from the current project directory to a directory of your choice: 
    
    
     mkdir ~/esp/xtensa-esp32-elf-libs 
     find . -path ./build/bootloader -prune -o -name "*.a" -exec cp {} ~/esp/xtensa-esp32-elf-libs \;
     find ~/esp/esp-idf/components -name "*.a" -exec cp {} ~/esp/xtensa-esp32-elf-libs \;
    

  
Optionally (but recommended) also copy the _esp32_out.ld_ and _esp32.project.ld_ files from the _/build/esp32/_ folder, or _./build/esp-idf/esp32_ and _./build/esp-idf/esp32/ld_ folders, into _~/esp/xtensa-esp32-elf-libs_ (with the rest of the SDK libraries). These linker scripts are required when linking a fpc project and needs to be reconstructed if not found by fpc. 

  
Notes: 

  * Make sure that you are in the directory where you have built the hello_world demo project when executing the find commands above.
  * One should not copy libraries from the project/build/bootloader directory, since these libraries do not contain full functionality.
  * Some of the required libraries are located in the esp-idf/components folder. Important to also copy the libraries in the ~/esp/esp-idf/components directory if the demo project was copied to outside the esp-idf directory.



Now you are ready to compile fpc and rtl. 

execute 
    
    
    export IDF_PATH=~/esp/esp-idf
    source ~/esp/esp-idf/export.sh
    

to get the path and environment set (namely tools path and $IDF_PATH are needed) 

#### Build FPC

Change into the fpc (e.g. ~/fpc/main) directory and run 
    
    
     make FPC=fpc CPU_TARGET=xtensa OS_TARGET=freertos SUBARCH=lx6 "CROSSOPT=-XPxtensa-esp32-elf- -Cfhard" all -j
    

#### Tips & Tricks

If linking fails due to missing libraries, you can search them by (replace missing_symbol by the missing symbol): 
    
    
     xtensa-esp32-elf-objdump -t ~/esp/xtensa-esp32-elf-libs/*.a | grep 'missing_symbol\|xtensa-esp32-elf-libs'
    

fpc doesn't automatically allocate heap space for this target. When using dynamic memory allocation remember to specify a heap size (for example -Ch1024). 

#### Compile and run test program

Create a hello world program: 
    
    
      program hello;
      begin
        WriteLn('Hello world.');
      end.
    

in a new folder and compile it with: 
    
    
     ~/fpc/main/compiler/ppcrossxtensa -Fu~/fpc/main/rtl/units/xtensa-freertos/ -Tfreertos -XPxtensa-esp32-elf- -O3 -Wpesp32 -Fl~/esp/xtensa-esp32-elf-libs  -Fl~/.espressif/tools/xtensa-esp32-elf/esp-2021r2-8.4.0/xtensa-esp32-elf/xtensa-esp32-elf/lib/ hello
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** You may need to change "esp-2021r2-8.4.0" in the above command to the appropriate release name for your installation.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Different versions of esp-idf require different libraries to link. Use the -WP command line option to specify the version number, e.g. _-WP4.3.2_.

  
Get partition-table.bin and bootloader.bin from the hello_world example compiled above and copy them into the same directory as the compiled hello.bin. For the default setup, use: 
    
    
     cp ~/esp/hello_world/build/partition_table/partition-table.bin ~/esp/hello_world/build/bootloader/bootloader.bin .
    

Flash with: 
    
    
     esptool.py -p /dev/ttyUSB0 -b 460800 --before default_reset --after hard_reset --chip esp32  write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x1000 bootloader.bin 0x8000 partition-table.bin 0x10000 hello.bin
    

  * The maximum feasible baud rate depends on the USB-serial chip interfacing with the ESP32 chip, see [this link](<https://www.esp32.com/viewtopic.php?t=420>). A baud rate of 921600 has worked successfully for flashing over a CP210x chip.



  
Monitor the output with: 
    
    
     idf_monitor.py  --port /dev/ttyUSB0 hello.elf
    

Note: 

  * Once the bootloader and partition table has been flashed once one could also just flash the application only:


    
    
     esptool.py -p /dev/ttyUSB0 -b 460800 --before default_reset --after hard_reset --chip esp32  write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x10000 hello.bin
    

### ESP8266

#### Preparations

Install the ESP8266-RTOS-SDK (currently up to v3.4 supported by FPC) based on the instructions given here: [Get Started](<https://docs.espressif.com/projects/esp8266-rtos-sdk/en/v3.4/get-started/>)

Follow the tutorial till the test of the hello world program so it is ensured, that everything works. 

Copy all the freshly compiled .a files from the current project directory to a directory of your choice: 
    
    
     mkdir ~/esp/xtensa-lx106-elf-libs 
     find . -path ./build/bootloader -prune -o -name "*.a" -exec cp {} ~/esp/xtensa-lx106-elf-libs \;
    

Optionally (but recommended) also copy the _esp8266_out.ld_ and _esp8266.project.ld_ files from the _/build/esp8266/_ folder into _~/esp/xtensa-lx106-elf-libs_ (with the rest of the SDK libraries). These linker scripts are required when linking a fpc project and needs to be reconstructed if not found by fpc. 

  
Notes: 

  * Make sure that you are in the directory where you have built the hello_world demo project when executing the find commands above.
  * One should not copy libraries from the project/build/bootloader directory, since these libraries do not contain full functionality.



Now you are ready to compile fpc and rtl. 

#### Build FPC

Change into the fpc (e.g. `~/fpc/main`) directory and run: 
    
    
     make FPC=fpc CPU_TARGET=xtensa OS_TARGET=freertos SUBARCH=lx106 BINUTILSPREFIX=xtensa-lx106-elf- all -j
    

#### Tips & Tricks

fpc doesn't automatically allocate heap space for this target. When using dynamic memory allocation remember to specify a heap size (for example -Ch1024). 

  


#### Compile and run test program

Create a hello world program: 
    
    
      program hello;
      begin
        WriteLn('Hello world.');
      end.
    

in a separate directory and compile it with: 
    
    
     ~/fpc/main/compiler/ppcrossxtensa -Fu~/fpc/main/rtl/units/xtensa-freertos/ -Tfreertos -XPxtensa-lx106-elf- -O3 -Wpesp8266 hello -Fl~/esp/xtensa-lx106-elf-libs/ -Fl$IDF_PATH/components/esp8266/lib -Fl$IDF_PATH/components/newlib/newlib/lib -Fl~/esp/xtensa-lx106-elf/xtensa-lx106-elf/lib/ -WP3.4
    

Get partitions_singleapp.bin and bootloader.bin from the hello_world example compiled above and copy them into the same dir as the compiled hello.bin 

Flash with: 
    
    
     $IDF_PATH/components/esptool_py/esptool/esptool.py --chip esp8266 --port "/dev/ttyUSB0" --baud 921600 --before "default_reset" --after "hard_reset" write_flash -z --flash_mode "dio" --flash_freq "40m" --flash_size "2MB"   0x0 bootloader.bin 0x10000 hello.bin 0x8000 partitions_singleapp.bin
    

  
Monitor the output with: 
    
    
     $IDF_PATH/tools/idf_monitor.py --baud 74880 --port /dev/ttyUSB0 hello.elf
    

Note: 

  * Different versions of the SDK may require different libraries to link. Use the -WP command line option to specify the SDK version number, e.g. _-WP3.4_.



### Using both ESP32 and ESP8266

Both controllers use the same ppcrossxtensa compiler, but different versions of the RTL. The default folder structure of fpc does not cater for this, so each time the cross compiler is built, it will overwrite the RTL files in rtl/units/xtensa-freertos. To use both ESP32 and ESP8266 in parallel (i.e. without rebuilding the RTL to switch to a different compiler), the following approach can be used: 

1\. Once the cross compiler is built, rename the rtl/units/xtensa-freertos folder to rtl/units/xtensa-freertos/$subarch, where $subarch is either lx6 or lx106. 

2\. Edit fpc.cfg. Locate the line with the path to the rtl (e.g. -Fu~/fpc/main/rtl/units/$fpctarget) and modify it as follows: 
    
    
     #ifdef cpuxtensa
       -Fu~/fpc/main/rtl/units/$fpctarget/$fpcsubarch
     #else
       -Fu~/fpc/main/rtl/units/$fpctarget
     #endif
    

  
Both esp-idf and ESP8266-RTOS-SDK use an environment variable named IDF_PATH to point to the path of the respective SDK. If both SDKs are installed, the path to the desired SDK can be passed to the compiler using the -Ff command line switch. This path is used to locate scripts and linker configuration files - the location of library files still need to be specified using -Fl. 

## Xtensa-Linux

### To use with QEMU

#### Build binutils

Get binutils-gdb sources, e.g. by 
    
    
     git clone <git://sourceware.org/git/binutils-gdb>
    

Binutils 2.34 is known to work, so optionally do 
    
    
     git checkout binutils-2_34
    

Copy <https://github.com/FPK/binutils-xtensa-config/blob/master/lx6/xtensa-config.h> into binutils-gdb/include to configure binutils for the CPU type support by FPC for xtensa-linux. This replaces the original file. 

  
Configure and build binutils with 
    
    
     ./configure --target=xtensa-linux --disable-gdb --disable-sim --disable-readline --disable-libdecnumber
     make -j `nproc`
     sudo make install
    

#### Build FPC

Get latest trunk and build it with: 
    
    
     make FPC=fpc all -j "CROSSOPT=-Cfhard" OS_TARGET=linux CPU_TARGET=xtensa
    

#### Test FPC

Create a hello world in the FPC top level directory. Compile it with: 
    
    
     compiler/ppcrossxtensa hello.pp -Tlinux -Furtl/units/xtensa-linux
    

Run it with: 
    
    
     qemu-xtensa ./hello
    

## Xtensa-Embedded

## Emulation and debugging using qemu (ESP32)

### Install qemu (from source)

The official release of qemu supports the xtensa architecture but lacks support for the ESP32 and ESP8266 boards. To use qemu with ESP32 (ESP8266 support is not yet officially available) one can use Espressif's port of qemu (refer to this for detail information: <https://github.com/espressif/esp-toolchain-docs/blob/main/qemu/README.md>). 

### Automated testing with qemu

For automated testing one needs to return an exit code from qemu to indicate the outcome of a test. One way to achieve this is to call the simcall instruction passing 1 in register a2 and the exit code in register a3: 
    
    
      procedure qemu_exit(const exitcode: uint32); assembler; noreturn;
      asm
        mov  a3, a2   // copy exitcode value into a3
        movi a2, 1    // set a2 to 1 - exit request
        simcall
      end;

---

_Source: [https://wiki.freepascal.org/Xtensa](https://web.archive.org/web/20250124214355/https://wiki.freepascal.org/Xtensa)_
