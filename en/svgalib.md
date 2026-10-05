# svgalib

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Requirements
  * 3 Installation
  * 4 Testing
  * 5 Caveats
  * 6 Future Plans



## Overview

Under Linux/BSD, you can use libvga to access the graphical screen from the console. Free Pascal has a unit which makes linking to the libvga possible. You can find the unit, as well as a makefile and some test programs here. You just need to compile it, put a reference to it in your program or unit, and that is it. There are 2 units in this package: 

  * **svgalib** the actual libvha interface.
  * **vgamouse** mouse extensions to work with the mouse under svgalib.



## Requirements

To use this unit, you need at least: 

  * Free Pascal 0.99.5 or higher
  * libsvga 1.2.10 or higher



## Installation

The svga lib unit comes with the packages of Free Pascal. This contains a subdirectory svgalib. Change to this directory. 

Edit the makefile to suit your setup (compiler, options, where do you want to install) type 
    
    
    make
    

and all units will be compiled. Then type 
    
    
    make install
    

To install the units. 

## Testing

Two small testprograms have been provided: 

  * **vgatest** : This is a translation of the C program that comes with svgalib.
  * **testvga** : A Small program to demonstrate that you can draw lines on the screen in any mode.



typing 
    
    
    make test
    

will compile the programs. 

## Caveats

It is possible that you must be root to run these programs, The SVGAlib docs I have aren't clear about that. If the programs should be able run as another user, you should make them setuid root. I tested everything as root, and it ran smoothly, your mileage may vary, however. 

## Future Plans

It would be nice to have a borland style GRAPH unit wrapped around this; just for beginners or people who wish to port some old DOS code. 

Go to back [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/svgalib](https://web.archive.org/web/20230327042113/https://wiki.freepascal.org/svgalib)_
