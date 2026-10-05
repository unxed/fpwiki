# Target Darwin

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

![<translate> Warning: </translate>](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/OOjs_UI_icon_notice-destructive.svg/18px-OOjs_UI_icon_notice-destructive.svg.png) **Warning**|  In Lazarus 2.2.0/FPC 3.2.2 and later, the target for building iOS applications was changed from Darwin to iOS due to the advent of the Apple Silicon M1 (ARM64) processor in Mac computers.  
---|---  
  
## Contents

  * 1 Introduction
  * 2 Installation
  * 3 Usage
  * 4 Universal binaries
  * 5 See also



## Introduction

Darwin is the target for macOS and iOS, both PowerPC, ARM and ARM64, i386 and X86_64. Programs may also be run on a machine with only Darwin installed. 

## Installation

See [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS"). 

## Usage

1) [Lazarus IDE](<http://forum.lazarus.freepascal.org>)

Lazarus is a Delphi-style RAD environment 

2) [Lightweight IDE](<http://www.ragnemalm.se/lightweight/>)

A free IDE in the classic Mac style 

3) Any Editor (AlphaX, BBedit, ...) and command line (fpc your_pascal_program.pas) 

## Universal binaries

Normally for each processor - operating system combination there is one executable, but in macOS you can combine an aarch64 (ARM64) binary and an x86_64 binary into a so-called "universal binary" or "multi-architecture binary" that will run on both Apple ARM64 processors and Intel 64 bit processors. To do this the ARM64 and x86_64 executables have to be compiled separately and then combined using the `lipo` command line utility. 

It is also possible to combine a PowerPC binaries and an x86 binaries into a single combined binary using the `lipo` command line tool. You would need to first [download the PowerPC cross-compiler](<https://sourceforge.net/projects/freepascal/files/Mac%20OS%20X/>) so you only need to use `ppcppc` instead of `fpc` to build your project to generate the PowerPC binary. If you have a PowerPC computer, then the simplest solution is to build the x86 binary on a different computer with x86 architecture. 

## See also

  * [Mac Portal](<Portal_Mac.md> "Portal:Mac").
  * [ Creating a universal binary for aarch64 and x86_64](<macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers").
  * [Target MacOS](<Target_MacOS.md> "Target MacOS") for PowerPC processors.

---

_Source: [https://wiki.freepascal.org/Target_Darwin](https://web.archive.org/web/20230128165140/https://wiki.freepascal.org/Target_Darwin)_
