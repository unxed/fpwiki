# Issues 3.0.4

## Contents

  * 1 Issues with 3.0.4 release candidates
    * 1.1 Issues with 3.0.4-rc1
    * 1.2 Merge requests
    * 1.3 Post RC1 merges



# Issues with 3.0.4 release candidates

This page is supposed to contain all issues discovered while testing release candidates and their status. Make sure to include information about the platform where the problem appears (if applicable). Once the issue gets resolved, you should strike it over and add number of SVN revision fixing it. If the revision number refers to a SVN repository different from "fpc" (i.e. "fpcbuild" or "fpcdocs"), include information about the SVN repository too. 

  


## Issues with 3.0.4-rc1

  1. building the crosscospiler for arm-nds (make clean all OPT="-ap" CPU_TARGET=arm OS_TARGET=nds) fails. Merging with commits 30017 and 30023 fixes it.
  2. building the crosscospiler for arm-gba (make clean all OPT="-ap" CPU_TARGET=arm OS_TARGET=gba) fails. Copying rtl/gba/rtl.cfg from trunk fixes it.



## Merge requests

  1. Request to merge 30017 and 30023. Fixes building arm-nds.
  2. Request to copy rtl/gba/rtl.cfg from trunk to the fixes branch.



## Post RC1 merges

---

_Source: [https://wiki.freepascal.org/Issues_3.0.4](https://web.archive.org/web/20211207131749/https://wiki.freepascal.org/Issues_3.0.4)_
