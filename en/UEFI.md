# UEFI

## Contents

  * 1 Motivations behind this target
  * 2 Technical requirements
  * 3 Current state
  * 4 Build instructions
  * 5 Known problems
  * 6 Testing UEFI applications with qemu
  * 7 History
  * 8 External resources



## Motivations behind this target

The Unified Extensible Firmware Interface (UEFI) now replaces BIOS in most current PCs. 

Currently, there are two main options to develop UEFI applications: 

  * [Tianocore](<http://www.tianocore.org/>)
  * [gnu-efi](<http://sourceforge.net/projects/gnu-efi/>)



Both solutions require a tedious setup process. 

Thank to its internal linker, Free Pascal can become the easiest option to develop UEFI applications, as no external tools would be required to start hacking. 

## Technical requirements

UEFI applications are PE binaries like the Windows target, which Free Pascal already supports. The only difference is the subsystem in the PE header. 

Calling conventions under UEFI are identical to the ones under Windows on the same architecture, which Free Pascal already supports, too. 

## Current state

_My current patch against trunk is here :<http://nan.tf/download/uefi_all_03_01_2016.diff>._

_It is not yet a clean patch : lots of debug output inside._

**I have resume my work on this target. Here is my updated patch against trunk :<http://nan.tf/download/fpc_uefi_28_04_2017.diff>**

Currently, I am focusing on UEFI32 because i have a few cheap atom machines with that version (see <https://software.intel.com/en-us/blogs/2015/07/22/why-cheap-systems-run-32-bit-uefi-on-x64-systems>) and because i use Haiku as my main development platform, without a 64 bits freepascal yet. 

But UEFI64 support should be easy to had with careful API declarations. 

A branch is now available with the current development state at <https://svn.freepascal.org/cgi-bin/viewvc.cgi/branches/olivier/uefi/>

## Build instructions

For UEFI 32 target, the build command line is : 
    
    
    make all OS_TARGET=uefi CPU_TARGET=i386
    

For UEFI 64 target, the build command line is : 
    
    
    make all OS_TARGET=uefi CPU_TARGET=x86_64
    

If you want to cross compile from a different architecture, it might be necessary to specify OS_SOURCE and CPU_SOURCE. Here is for example the command line for Haiku 32 bits. 
    
    
    make all OS_TARGET=uefi CPU_TARGET=x86_64 OS_SOURCE=haiku CPU_SOURCE=i386
    

## Known problems

_Affectation of string constant to string variable does not work. It is currently necessary to create strings through an array of WideChar._
    
    
    var
      s : string;
    begin
      s := 'Hello world !';
    end;
    

_Maybe it is related to Position Independent Code generation in the case of PE executables ?_

_Any hints in this area are welcome ;-)_

**With current trunk (3.1.1), this bug has been fixed. It helps a lot to be able to use WriteLn('Something to write') instead of constructing an array of WideChar and call UEFI functions as before ;-).**

Other known problem : 
    
    
      list := TStringList.Create;
    

While constructing other kind of object works as expected, this one currently crash the program... I will have to investigate. 

Update : it seems related to uses : without any uses in my program, it just works... With only one, it crash. 

Update 2 : I have finally found the problem : SysUtils initialize some static constructions BEFORE unit initialization. That is why it was always crashing when adding a uses to SysUtils in a simple program, but before the **initialization** section. That explain why my debugging messages in SysUtils where never displayed... 

## Testing UEFI applications with qemu

See [OSDev UEFI Emulation with QEMU and OVMF](<http://wiki.osdev.org/UEFI#Emulation_with_QEMU_and_OVMF>)

To avoid creating an image, i use FAT emulation to a local directory where my UEFI application is located : 
    
    
    qemu-system-i386-x86 -pflash /path/to/OVMF.fd -hda fat:local_directory -net none
    

"-net none" avoid net booting to save a few seconds in the boot process. 

From UEFI shell, then write : 
    
    
    fs0:
    

to switch to the directory where are located your application then you can launch it : 
    
    
    app.efi
    

## History

Everything has started after reading this article while i was lurking about UEFI development : <http://blog.theincredibleholk.org/blog/2013/11/18/booting-to-rust/>

What about doing that in Freepascal as well ? 

It appears that Freepascal is even a better target for such a project because of its internal linker for PE targets : no need to install and configure an external one. 

**August 2014**

Proof of concept : Freepascal is able to output a PE binary that can run in an UEFI environment. It can output a static text and the version of the firmware to the screen using an UEFI method. 

**April 2017**

After observing that an annoying bug about string constant was fixed, i have resumed my work on this target. 

Currently, here are the features that are implemented : 
    
    
    - WriteLn support (no more awkward array of WideChar allocation required)
    - Memory allocation
    

**May 2017**

Start of x86_64 support (not functional yet). The idea is to check if the 'uses' problem is related to the use of PE32 binary format. UEFI is supposed to use PE32+ (with 64 bits relocations). UEFI specifications are not clear whether this variant is also used for UEFI32. With a x86_64 CPU and the win64 linker, this uncertainty should disappear... 

Update : after a binary comparison between two different binaries compiled with the official UEFI tools, UEFI32 binaries are plain PE32 ones, with an IMAGE_OPTIONAL_HEADER with a size of 224 bytes while an UEFI64 binary contains an IMAGE_OPTIONAL_HEADER with a size of 240 bytes (some fields are 64 bits instead of 32 bits). This is confirmed in <https://msdn.microsoft.com/en-us/library/windows/desktop/ms680339(v=vs.85).aspx>. Afterthought, it make sense : i suspect Intel has chosen PE binary format and Application Binary Interface (ABI) like the corresponding ones in Windows to minimize the work at Microsoft and make sure they will adopt the UEFI specifications... 

**August 2017**

## External resources

Some interesting development resources around UEFI : 

  * [UEFI specifications](<http://www.uefi.org/>)
  * [OS development under UEFI](<http://wiki.osdev.org/UEFI>)
  * [Managing EFI Boot Loaders for Linux: Basic Principles](<http://www.rodsbooks.com/efi-bootloaders/principles.html>)

---

_Source: [https://wiki.freepascal.org/UEFI](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/UEFI)_
