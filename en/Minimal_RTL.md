# Minimal RTL

## Contents

  * 1 Introduction
  * 2 History
  * 3 I want a special RTL, what should I do?
    * 3.1 Making a new port of FPC to target a specific OS
    * 3.2 A new OS
    * 3.3 Cutting down a known platform



## Introduction

In some articles about creating own OSes and do embedded systems with FPC one can find references to a core/bare/minimal/fake RTL. Actually they are different, but they share one feature: they are RTLs with higher level functionality removed. 

Another effort by Carl was the so called "POSIX" port. This can be seen as the direct precursor of the current 2.x Unix RTL, though the current RTL doesn't actually share any code with the POSIX port; it's more that some of (most actually) the original POSIX port principles were adapted for the 2.x Unix RTL. 

See also [Size Matters](<Size_Matters.md> "Size Matters")

## History

The 1.0.x series had a so called "fake" RTL. This pretty much was an empty RTL with the empty functions that the compiler requires to be there. And with empty I mean _empty_. Not even string handling worked. 

The original idea was to start new ports with this RTL; however this never materialised, since in practice, one always took the port of the most similar existing target, and modified that. 

In the 1.1.x branch (what would later become 2.0.x), the connectivity between the compiler and the RTL was totally reworked based on the "compilerproc" directive, and vastly expanded. The fake RTL was not kept up to date at all, and eventually removed, in part because it hadn't proven to be that useful. 

## I want a special RTL, what should I do?

There is none - you will have to create your own. We can't provide one, since there is no single clear cut reason why one wouldn't want to use the default RTL. Admitted, most people that want this will want a simpler RTL, but some will want a totally empty RTL, some only want to avoid starting up exception handling in SysUtils, or comment out the timezone routines for use under embedded Linux. 

It is useful to distinguish the following reasons: 

  1. Make a new OS port
  2. make a new OS (kernel, and a real one, not a shell)
  3. embedded use on Arm/x86 using an existing OS, disabling some features that can cause problems on these.



### Making a new port of FPC to target a specific OS

This one is easy, and we've already seen the practice in the "History" paragraph. Take the nearest OS, and start hacking. Look a lot at existing RTLs to find common workarounds for problems. 

### A new OS

Every OS has its own requirements, so it is very hard to give a general rule. Some embedded ones will be closer to running an app as a kernel anyway. Some will have "filesystems", some not. Some will be able to run multiple applications at the same time, some not, etc. 

In general, I can only give two tips: 

  * Take the system unit of e.g. Linux, and start cutting it down until the bare metal. Implement all missing functions to your own OSes API (e.g. everything that calls do_syscall). Add some assembler to take care of your kernel's startup (e.g. in prt0.as).
  * Try to find other examples. Most notably scan FPC's "contributed units" page. We advise people that manage to write an OS to add a link there. See also [Operating Systems written in FPC](<Operating_Systems_written_in_FPC.md> "Operating Systems written in FPC")



### Cutting down a known platform

Simply make your mods and recompile. Chances that we will ease this from FPC side are slim, since we currently don't really support embedded programming with the existing RTL (at least not the kind where 50kb code overhead matters). This is mainly for maintainability reasons, and because every embedded developer has his own wishes and trade-offs.

---

_Source: [https://wiki.freepascal.org/Minimal_RTL](https://web.archive.org/web/20250122083836/https://wiki.freepascal.org/Minimal_RTL)_
