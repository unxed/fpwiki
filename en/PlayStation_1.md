# PlayStation 1

## Contents

  * 1 Overview
  * 2 Requirements
    * 2.1 GNU BinUtils
    * 2.2 PSY-Q-SDK
    * 2.3 Real Hardware
    * 2.4 Emulator
      * 2.4.1 PSX BIOS
  * 3 Building
  * 4 Compiling programs
    * 4.1 Hello World example
    * 4.2 Cube example
  * 5 Create a PSX .iso
  * 6 Information Sources



## Overview

Every Instruction on this page is for Linux. 

The PlayStation 1 port is currently under development in the Git "main" branch. 

If I use the word PSX it is equivalent to the PlayStation 1. 

## Requirements

### GNU BinUtils

Compiler needs GNU Binutils. Download a recent version from <https://ftp.gnu.org/gnu/binutils/> . Then run shell commands: 
    
    
    ./tar -xf <binutils.tar.gz>
    ./cd <binutils>
    ./mkdir build
    ./cd build
    ../configure --prefix=/usr/local/mipsel-unknown-elf --target=mipsel-unknown-elf --with-float=soft
    make -j$(nproc)
    sudo make install
    export PATH="/usr/local/mipsel-unknown-elf/bin:$PATH"
    

Now you can go two ways: 

1) Let the binaries as they are and always use the compiler option 
    
    
    -XP/usr/local/mipsel-unknown-elf/bin/mipsel-unknown-elf-
    

  
2) Create symlinks 
    
    
    sudo ln -s /usr/local/mipsel-unknown-elf/bin/mipsel-unknown-elf-as /usr/bin/mipsel-ps1-as
    sudo ln -s /usr/local/mipsel-unknown-elf/bin/mipsel-unknown-elf-ld /usr/bin/mipsel-ps1-ld
    sudo ln -s /usr/local/mipsel-unknown-elf/bin/mipsel-unknown-elf-objcopy /usr/bin/mipsel-ps1-objcopy
    

### PSY-Q-SDK

To compile something You need the converted PSY-Q-SDK to the ELF format 

You can go two ways: 

1) Clone it directly 
    
    
    git clone <https://key-real@git.code.sf.net/p/psy-q-sdk-elf/code> psy-q-sdk
    

2) Use the original and convert it by your self 

\- Download the official SDK: 

<https://www.psxdev.net/downloads.html>

\- Download the converter: 

<https://gitlab.com/jype/psyq2elf>

  
from now on I assume the SDK is in following Folder: 
    
    
    /home/ps1/psy-q-sdk
    

### Real Hardware

You need a PlayStation 1 with the MOD CHIP. 

### Emulator

\- I recommend to use PCSX-REDUX <https://github.com/grumpycoders/pcsx-redux>

(I use Arch Linux and it is in my AUR Repo: pcsx-redux-git) 

  
\- DuckStation <https://github.com/stenzek/duckstation?tab=readme-ov-file#downloading-and-running>

(I use Arch Linux and it is in my AUR Repo: duckstation-git) 

  
\- ePSXe <https://www.epsxe.com/>

(I use Arch Linux and it is in my AUR Repo: epsxe) 

  


#### PSX BIOS

You can use 

a) the original Sony BIOS: 

<https://gist.github.com/juanbrujo/cf55d223ad01927a48f9ebac9f50bdee>

b) OpenBIOS from the PCSX-REDUX project: 

<https://github.com/grumpycoders/pcsx-redux/tree/main/src/mips/openbios>

## Building

1) Get the newest version of FPC trunk: 
    
    
    git clone <https://gitlab.com/freepascal.org/fpc/source.git> fpc
    

2) Compile and install the the compiler: 
    
    
    make clean
    

If you created the symlinks to the mipsel binutils just do: 
    
    
    sudo make crossinstall OS_TARGET=ps1 CPU_TARGET=mipsel
    

If not do: 
    
    
    sudo make crossinstall OS_TARGET=ps1 CPU_TARGET=mipsel CROSSOPT="-XP/usr/local/mipsel-unknown-elf/bin/mipsel-unknown-elf-"
    

## Compiling programs

When compiling programs, you should always enable smartlinking by adding an option to the compiler: 
    
    
    -XX
    

and you have to specify the path to the PSY-Q-SDK: 
    
    
    -Fl/home/ps1/psy-q-sdk
    

so your compile command should look like this: 
    
    
    ppcrossmipsel test.pas -Tps1 -XX -Fl/home/ps1/psy-q-sdk
    

or if you have no symlinks to binutils: 
    
    
    ppcrossmipsel test.pas -Tps1 -XX -Fl/home/ps1/psy-q-sdk -XP/usr/local/mipsel-unknown-elf/bin/mipsel-unknown-elf-
    

### Hello World example
    
    
    {$MODE OBJFPC}
    uses libstd;
    begin
    
    	printf('HELLO from FPC'#10);
    
    end.
    

Here's what this program produces in the PCSX-Redux emulator when You switch MainMenu->Debug->Show Logs: 
    
    
    HELLO from FPC
    

### Cube example
    
    
    {$MODE OBJFPC}
    uses libstd, libetc, libgpu, libgte;
    
    const
    	MODE_NTSC = 0;									
    	MODE_PAL = 1;
    
    	OTSIZE = 4096;									// Ordering table size
    
    	vertices : array [0..7] of SVECTOR = (			// Cube
    	  (vx: -128; vy: -128; vz: -128; pad: 0 ),
    	  (vx:  128; vy: -128; vz: -128; pad: 0 ),
    	  (vx:  128; vy: -128; vz:  128; pad: 0 ),
    	  (vx: -128; vy: -128; vz:  128; pad: 0 ),
    	  (vx: -128; vy:  128; vz: -128; pad: 0 ),
    	  (vx:  128; vy:  128; vz: -128; pad: 0 ),
    	  (vx:  128; vy:  128; vz:  128; pad: 0 ),
    	  (vx: -128; vy:  128; vz:  128; pad: 0 ));
    
    	faces : array [0..35] of smallint = (			// Cube
    	  0, 3, 2, // top
    	  0, 2, 1, // top
    	  4, 0, 1, // front
    	  4, 1, 5, // front
    	  7, 4, 5, // bottom
    	  7, 5, 6, // bottom
    	  5, 1, 2, // right
    	  5, 2, 6, // right
    	  2, 3, 7, // back
    	  2, 7, 6, // back
    	  0, 4, 7, // left
    	  0, 7, 3  // left 
    	  );
    
    
    type
    
    	DoubleBuff = packed record						// Screen
      		draw : DRAWENV;
      		disp : DISPENV;
      		ot : array[0..OTSIZE] of dword;				// Ordering table
     	end;
    
    
    var
    
    	screen : array [0..1] of DoubleBuff;           	// Struct to hold the display & draw buffers
    	currbuff : byte;            					// Holds the current buffer number (0 or 1)
    
    	i : longint;
    	otz : longint;									// Current Ordering table Entry
    
    	poly : array [0..11] of POLY_G3;				// Cube
    
    	rotation : SVECTOR;								
        translation : VECTOR;
        transform : MATRIX;
    
        p : pointer;
        flg : pointer;
        nclip : longint;								// Is cliped?
    
    
    procedure setRGB0(var c: DRAWENV; r, g, b: byte);
    begin
    	c.r0:=r;
    	c.g0:=g;
    	c.b0:=b;
    end;
    
    
    procedure ScreenInit(width, height: dword);
    var
    	mode : byte;
    
    begin
    		
    	// Init
      	ResetGraph(0);
    	InitGeom();
    
    	SetGraphDebug(0);
    	
    	// Set Display Mode
    	if height = 240 then mode:= MODE_NTSC;
    	if height = 256 then mode:= MODE_PAL;
    	SetVideoMode(mode);
    
    	// Init Buffers
    	SetDefDispEnv(@screen[0].disp, 0, 0, width, height);
    	SetDefDispEnv(@screen[1].disp, 0, height, width, height);
    
    	SetDefDrawEnv(@screen[0].draw, 0, height, width, height);
    	SetDefDrawEnv(@screen[1].draw, 0, 0, width, height);
    
    	// Set screen Offset
    	screen[0].disp.screen.x:= 0;
    	screen[0].disp.screen.y:= 0;
    	screen[1].disp.screen.x:= 0;
    	screen[1].disp.screen.y:= 0;
    
    	screen[0].disp.screen.h:= height;
    	screen[0].disp.screen.w:= 0;
    	screen[1].disp.screen.h:= height;
    	screen[1].disp.screen.w:= 0;
    
    	// Enable background clearing
    	screen[0].draw.isbg:= 1;
    	screen[1].draw.isbg:= 1;
    
    	// Set the background clear color
    	setRGB0(screen[0].draw, 0, 0, 0);
    	setRGB0(screen[1].draw, 0, 0, 0);
    
    	
    	// Initialize and setup the GTE geometry offsets
    	SetGeomOffset(width div 2, height div 2);
    	SetGeomScreen(100);
    	
    	SetDispMask(1);
    
    	// Set the current initial buffer
    	currbuff:= 0;
    	PutDispEnv(@screen[currbuff].disp);
    	PutDrawEnv(@screen[currbuff].draw);
    
    end;
    
    
    procedure DisplayFrame;
    begin
    	
    	// Set the current display & draw buffers
    	PutDispEnv(@screen[currbuff].disp);
    	PutDrawEnv(@screen[currbuff].draw);
    
    	// Draw Primetive
    	DrawOTag(@screen[currbuff].ot[OTSIZE - 1]);
    
    	// Draw Font
    	FntFlush(-1);
    
    	if currbuff = 0 then currbuff:= 1 else currbuff:= 0;
    
    	// Sync and wait for vertical blank
    	DrawSync(0);
    	VSync(0);
    
    end;
    
    
    begin
    	
    	ScreenInit(320, 256);
    
    	// Setup Font
    	FntLoad(960, 256);
    	SetDumpFnt(FntOpen(0, 100, 200, 200, 0, 512));
    
    	rotation.vx:= 0;
    	rotation.vy:= 0;
    	rotation.vz:= 0;
    
    	translation.vx:= 0;
    	translation.vy:= 0;
    	translation.vz:= 500;
    
    	repeat
    
    		// Clear Ordering table
    		ClearOTagR(@screen[currbuff].ot[currbuff], OTSIZE);
    		
    		// Transform
    		rotation.vx +=  6;
    	  	rotation.vy +=  8;
    	  	rotation.vz += 12;
    
    	    RotMatrix(@rotation, @transform);
    	    TransMatrix(@transform, @translation);
    			
    		SetRotMatrix(@transform);
        	SetTransMatrix(@transform);
    
        	// Draw Polys
    	    for i:= 0 to 11 do begin
    			setPolyG3(@poly[i]);
    
    			poly[i].r0:= 255; poly[i].g0:=0;   poly[i].b0:= 0;
    			poly[i].r1:= 0;   poly[i].g1:=255; poly[i].b1:= 0;
    			poly[i].r2:= 0;   poly[i].g2:=0;   poly[i].b2:= 255;
    
    			nclip:= RotAverageNclip3(@vertices[faces[i * 3 + 0]], @vertices[faces[i * 3 + 1]], @vertices[faces[i * 3 + 2]], @poly[i].x0, @poly[i].x1, @poly[i].x2, @p, @otz, @flg);
    			if nclip <= 0 then continue;
    
    			if (otz > 0) and (otz < OTSIZE) then addPrim(@screen[currbuff].ot[otz], @poly[i]);
    		end;
    
    
    		FntPrint('Hello from FPC');
    		
    		DisplayFrame;
    		
    	until false;
    
    end.
    

Here's what this program produces in the PCSX-Redux emulator: 

[![psx-cube.jpg](https://wiki.freepascal.org/images/d/d9/psx-cube.jpg)](</File:psx-cube.jpg>)

## Create a PSX .iso

You need: 

<https://github.com/Lameguy64/mkpsxiso>

and run: 
    
    
    mkpsxiso iso.xml 
    

File iso.xml: 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
     <iso_project image_name="myimage.iso" cue_sheet="myimage.cue">
      <track type="data">
       <identifiers system="PLAYSTATION" application="PLAYSTATION" volume="MYDISC" volume_set="MYDISC" publisher="MYPUBLISHER" data_preparer="MKPSXISO"/>
       <directory_tree>
        <file name="system.cnf" type="data" source="system.cnf"/>
        <file name="MAIN.PSX-EXE" type="data" source="test.psx-exe"/>	
       </directory_tree>
      </track>
     </iso_project> 
    

File system.cnf: 
    
    
    BOOT=cdrom:\MAIN.PSX-EXE;1
    TCB=4
    EVENT=10
    STACK=801FFFF0
    

this file describes the boot process. 

# Information Sources

\- full specification: <https://psx-spx.consoledev.net/>

\- Psy-Q-SDK reference doc: <https://psx.arthus.net/sdk/Psy-Q/DOCS/LibRef47.pdf>

\- MIPS Assembly: <https://www.cs.unibo.it/~solmi/teaching/arch_2002-2003/AssemblyLanguageProgDoc.pdf>

\- Cool Course: <https://pikuma.com/courses/ps1-programming-mips-assembly-language>

\- Discord Server: PSX.Dev

---

_Source: [https://wiki.freepascal.org/PlayStation_1](https://web.archive.org/web/20250208184759/https://wiki.freepascal.org/PlayStation_1)_
