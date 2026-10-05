# Build current FPC and Lazarus for Raspberry Pi OS

│ **English (en)** │

Raspberry Pi OS Bullseye (based on Debian 11 Bullseye) has FPC and Lazarus available in the repositories, but they suffer from the same "design decisions" that don't allow for a nice experience, namely the smooth IDE rebuilding capability based on source packages that is possible with FPC/Lazarus official .deb packages vs the crippled experience with the official Debian/Raspberry Pi OS packages. One wishes there were official FPC/Lazarus packages for Raspberry Pi OS as there are for Intel Debian/Ubuntu, which are also frequently updated; not being that the case (at least for now), this page shows the steps for building your own FPC, FPC-source and Lazarus packages for Raspberry Pi OS. Example shows steps for Lazarus 2.2.0 which includes FPC 3.2.2, most recent at the time of writing. Adjust accordingly. 

Requirements: 

  * Raspberry Pi 4 Model B, 4 GB (less memory models would require swap), with appropriate cooling (author uses aluminum case that doubles as heatsink) to avoid throttling due to excessive heat. Raspberry Pi 400 works too.
  * Latest Raspberry Pi OS Bullseye fully updated



Steps: 

  1. Install subversion: `sudo apt install subversion`
  2. Install Free Pascal compiler: `sudo apt install fp-compiler`
  3. Test if all ok: `fpc abcd`. This invokes the just installed compiler trying to compile a non-existent file (abcd). A multiline message will be shown including the compiler version.
  4. Non-step note: the following steps are mostly based on [this](<https://wiki.lazarus.freepascal.org/How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository#Creating_the_deb_files_yourself>)
  5. Install development libraries: `sudo apt install libgtk2.0-dev libgpm-dev libncurses-dev`
  6. `mkdir ~/pascal`
  7. `cd ~/pascal`
  8. `svn checkout https://github.com/fpc/FPCSource/tags/release_3_2_2 fpc`, you have to be online for this and the next step to work.
  9. `git clone https://gitlab.com/freepascal.org/lazarus/lazarus`
  10. `cd lazarus`
  11. `git checkout lazarus_2_2_0`
  12. `cd tools/install`
  13. Edit `create_fpc_deb.sh`, look for line with the following content: `arm) ppcbin=arm; FPCArch=arm;;`, (currently line 109). Copy and paste this line below itself, so it will appear twice. Edit this copy to read like this: `armhf) ppcbin=arm; FPCArch=arm;;`. Now save the changes and exit.
  14. Run the script: `./create_fpc_deb.sh fpc ~/pascal/fpc/`. This will compile the latest compiler using the compiler included with Bullseye. It takes about 11 minutes. A `.deb` file will be created: `fpc-laz_3.2.2-yymmdd_armhf.deb`; the exact name will be shown at the end of the compilation.
  15. Uninstall Free Pascal compiler: `sudo apt purge fp-compiler && sudo apt --purge autoremove`
  16. Install the just created FPC Debian package: `sudo dpkg -i ~/pascal/lazarus/tools/install/fpc-laz_3.2.2-yymmdd_armhf.deb && sudo apt install -f`. Check if it works: `fpc abcd` should show the newly installed FPC compiler.
  17. `cd ~/pascal/lazarus/tools/install`
  18. `./create_fpc_deb.sh fpc-src ~/pascal/fpc/`. This builds the FPC source package in about 3 minutes: `fpc-src_3.2.2-yymmdd_armhf.deb`.
  19. Edit `create_lazarus_deb.sh`, look for line with the following content: `echo "$Arch is not supported."`, (currently line 86). Copy the 3 lines above it and paste them just below themselves: at this time it is copy lines 83 to 85 and paste them between lines 85 and 86. In the just pasted lines (lines 86-88), replace `sparc` with `armhf`, and `ppcsparc` with `ppcarm`. Look for the first `fi` just below; copy and paste it below itself (copy line 91 below itself, so line 91 and 92 read the same). For you coders out there, you are adding another `if` to a nested `if`, with its corresponding `fi`. Now save and exit.
  20. `./create_lazarus_deb.sh`. This will build the Lazarus IDE Debian package `lazarus-project_2.2.0-0_armhf.deb`. It will take about 15 minutes.
  21. Install the FPC source and Lazarus packages: `sudo dpkg -i fpc-src_3.2.2-yymmdd_armhf.deb lazarus-project_2.2.0-0_armhf.deb && sudo apt install -f`
  22. Save `*.deb` to another location, to reinstall later if needed or to share with friends: remember this is free software. Mine are here: <https://app.box.com/s/7g54gml6p9bfn10157go430oyputw2au> (here you will also find 64-bit packages for the recently released Raspberry Pi OS 64-bit, with copies of the two files modified in this article, with 64-bit edits).
  23. `cd && rm -rf ~/pascal ~/tmp`
  24. Edit as root `/etc/fpc.cfg`. Search for `gcclib`. In the line below, replace `cpuaarch64` with `cpuarm`. This removes error about missing `crtbegin.o` when building apps.
  25. Change the debugger backend to gdb: Tools/Options/Debugger backend.



You can now finally enjoy your fully working, fully up to date Lazarus. Find it in the Programming section of the Raspberry menu. Now try to install a package, for example `lazreport`; it will work! 

DISCLAIMER: these are UNOFFICIAL packages created from modified official sources. DO NOT ASK FOR SUPPORT ANYWHERE, neither on the forums or with the author of this article; use at your own risk.

---

_Source: [https://wiki.freepascal.org/Build_current_FPC_and_Lazarus_for_Raspberry_Pi_OS](https://web.archive.org/web/20240121085536/https://wiki.freepascal.org/Build_current_FPC_and_Lazarus_for_Raspberry_Pi_OS)_
