# Building a Pico Compiler

## Contents

  * 1 Building a Pico Compiler
    * 1.1 fpcupdeluxe
    * 1.2 The constraints
    * 1.3 Dependencies
    * 1.4 The Process
    * 1.5 Assumed Dir Structure
      * 1.5.1 The commands
    * 1.6 Setup a Lazarus project
    * 1.7 Compile from Commandline
    * 1.8 See also



# Building a Pico Compiler

One of FPC's great features is its ability to provide cross compilers. A cross compiler allows you to run on one system and make a binary for another, possibly completely different system. When making binaries for embedded targets, its the only way to go ! 

### fpcupdeluxe

For most users, the most convenient way to get a cross compiler for the Raspberry Pi Pico is to use [fpcupdeluxe](<https://github.com/LongDirtyAnimAlf/fpcupdeluxe>) to do it all. It will install just FPC or FPC and Lazarus for you. Note it installs the 'main' version of each, from time to time, one or the other may have bugs (thats the nature of development branches) but not often. Try again in a day or so if you have problems. 

But this page is really for those who want to build their own, or just understand the process. Maybe adapt it to another CPU. These instructions focus on using a 64bit Linux system as the host, Windows and perhaps Mac users can probably do something similar. 

Much more details at [ARM Embedded Tutorial - Installing Lazarus and Free Pascal](<ARM_Embedded_Tutorial_-_Installing_Lazarus_and_Free_Pascal.md> "ARM Embedded Tutorial - Installing Lazarus and Free Pascal")

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** and this is important, this recipe produces a different file layout than fpupdeluxe. That is for simplicity. Do not mix this with a fpcupdeluxe or results may be unpredictable.

### The constraints

  * If you are a Lazarus user, it makes sense to have a stand alone Lazarus install for your Pico work, you could switch a single install (by changing Tools->Options->Files->Compiler_executable) but it could get annoying. Multiple installs means installing Lazarus from source, remarkably easy.
  * If you already have a Source install of FPC main December 2023 or later, you can use that, with some care. Again, might be easier and safer to have a dedicated version. You do need an existing install of FPC => 3.2.2, if yours is installed in read only space (installed from a deb or rpm) then I suggest you work here with with a fresh copy of FPC source and "install" it into your own space.



### Dependencies

  * An existing, working, FPC322 or later.
  * Install binutils-arm-none-eabi - that means ones for arm, no operating system (or embedded) and eabi indicates the arm subtype, no 'hf' means 'soft float'.
  * And, of course, you need Michael's git repo with pico-fpcexamples. These come from <https://github.com/michael-ring/pico-fpcexamples> Note that Michael's repo contain two separate things, some nicely set example projects and, in 'units' a number of libraries (*.obj) and pascal units to link them into a pascal project.



### The Process

Here are a series of command lines, I do not recommend you run them as a script (although I have) because you MUST check for errors at each step. I keep this in my home directory in a structure like this, change it of course but change the commands if you do ! The commands here will create two fpc-3.3.1 directories, if you have them already, ensure they are empty or change the commands. Note that I have already downloaded fpc.zip to my Downloads dir. 

### Assumed Dir Structure
    
    
       $HOME/bin/FPC
           fpc-3.2.2  .....
           fpc-3.3.1
           SRC
               fpc-3.2.2 ....
               fpc-3.3.1
    

##### The commands
    
    
       #!/bin/bash
       export FPC_VER="fpc-3.3.1"        # note 1
       export SRC_DIR="$HOME/testFPC/FPC/SRC/$FPC_VER"
       export FPC_DIR="$HOME/testFPC/FPC/$FPC_VER"
       UNITDIR="$FPC_DIR"/units/arm-embedded/armv6m
       
       mkdir -p "$FPC_DIR"
       mkdir -p "$SRC_DIR"
       cd "$SRC_DIR"
       unzip ~/Downloads/fpc.zip         # note 2
       cd fpc
       make all                          # note 3
       make install INSTALL_PREFIX="$FPC_DIR"
       
                                     # note 4
       make crossinstall CPU_TARGET=arm OS_TARGET=embedded SUBARCH=armv6m  CROSSBINDIR=/usr/bin  BINUTILSPREFIX=arm-none-eabi-  CROSSOPT="-CpARMV6M -CaEABI" INSTALL_PREFIX="$FPC_DIR" INSTALL_UNITDIR="$UNITDIR"
       
       cp compiler/ppcx64 "$FPC_DIR"/bin/ppcx64          # note 5
       cp compiler/ppcrossarm "$FPC_DIR"/bin/ppcrossarm  # note 6
       
       cp rtl/embedded/arm/* "$UNITDIR"  # note 7
       
       cd "$FPC_DIR"
       mkdir -p etc
       bin/fpcmkcfg -d basepath="$FPC_DIR" -o etc/fpc.cfg  # note 8
       
       cd bin
       echo "#!/bin/sh" > fpc.bash       # note 9
       echo "# A script that ensure when we start fpc here, it ignores all other fpc.cfg" >> fpc.bash
       echo "$FPC_DIR""/bin/fpc -n @$FPC_DIR/etc/fpc.cfg \"\$@\"" >> fpc.bash
       chmod u+x fpc.bash
    

  


  * note 1 - right now (March 2024) fpc main thinks of it self as 3.3.1, this may change after the next release of FPC and its possible the necessary updates to make this all work will not be in that release.


  * note 2 - make sure you have that fpc.zip file there, FPC main Dec 2024 or later. cd ~/Downloads; wget <https://gitlab.com/freepascal.org/fpc/source/-/archive/main/source-main.zip>


  * note 3 - this takes a while.


  * note 4 - this takes a while too. And note it places the RTL files in a different place than fpcupdeluxe.


  * note 5 - this is your host system compiler.


  * note 6 - and this is the cross compiler, we call fpc and it calls this one.


  * note 7 - these are some units that the RTL, built earlier.


  * note 8 - this creates a pretty stock standard fpc.cfg file.


  * note 9 - this makes a script that calls fpc and passes parameters that ensure fpc takes no regard of any other fpc.cfg file, only the one we just made. The fpc.cfg that controls here is the one in note 8.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Whats different from fpcupdeluxe ? This model puts the Run Time Libraries in $FPC_DIR/units/arm-embedded/armv6m whereas fpcupdeluxe puts them in a more correct $FPC_DIR/units/arm-embedded/armv6m/eabi/rtl. The problem with the fpcupdeluxe model is that fpc does not look there by default, a patch to fpc.cfg is needed. As this is intended as a simple demo, I decided to keep it as simple as possible. If you prefer, change the UNITDIR variable to "$FPC_DIR"/units/arm-embedded/armv6m/eabi/rtl and patch your fpc.cfg to include in its search path. 

### Setup a Lazarus project

Doing your Pico development in Lazarus does make things simpler once setup. You can use (some of) Lazarus's help and code tools systems, click compile (but obviously not run) and have all your errors nicely presented. You do not need to make any changes to Lazarus (apart from settings mentioned below), just about any recent Lazarus will do. But, as mentioned above, makes sense to dedicate an install to Pico because of an unusual setting, next papa. 

Start a new Lazarus project in the usual way, to keep life simple, save it in a directory under 'projects' in Michael's directory tree. Firstly, a one off for this Lazarus instance, note this is NOT saved with the project. Open Options -> Files and look for the "Compiler executable" entry. Put in, with a full path, 'fpc.bash'. eg /home/YOUR-USER-NAME/bin/FPC/fpc-3.3.1/bin/fpc.bash (note fpcupdeluxe uses fpc.sh) 

Then, open the Project Options and set the following - 

Paths Other unit files (-Fu): ../pico-fpcexamples/units Include files (-Fi) : $(ProjOutDir) Libraries (-Fl); Unit output directory (-FU): lib/$(TargetCPU)-$(TargetOS) Target file name (-o) : bin/project1 [x] Apply conventions 

Config and Target [x] Use standard compiler config... [ ] Use additional Target OS (-T) : Embedded Target CPU family (-P) : arm Target processor (-Cp) : ARMV6M 

Custom Options -Wpraspi_pico -godwarfsets -godwarfcpp -Xu 
    
    
            -XParm-none-eabi-
    

Note the last of the Custom Options, when you installed binutils-arm-none-eabi, you added a set of binutils for this particular processor. Some systems may call them arm-embedded-??? if yours uses that name scheme, change that Option accordingly. 

### Compile from Commandline

Its also reasonable to build from the command line, here is an example - 
    
    
       #!/bin/bash
       echo "------- Using the fpc from 3.3.1 ---------"
       export MICHAELS_UNITS="$HOME"/Pico2/projects/pico-fpcexamples/units 
       "$HOME"/bin/FPC/fpc-3.3.1/bin/fpc.bash -va -Xu -XParm-none-eabi- -Tembedded -Parm -CpARMV6M -Fu"$MICHAELS_UNITS" -Wpraspi_pico project1
    

Note: -XParm-none-eabi- indicates how the binutils are named. If yours are named differently, change it ! 

### See also

  * [ARM Embedded Raspberry Pi Pico Tutorials](<ARM_Embedded_Tutorial_-_FPC_and_the_Raspberry_Pi_Pico.md> "ARM Embedded Tutorial - FPC and the Raspberry Pi Pico")

---

_Source: [https://wiki.freepascal.org/Building_a_Pico_Compiler](https://web.archive.org/web/20241207122056/https://wiki.freepascal.org/Building_a_Pico_Compiler)_
