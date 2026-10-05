# Build current FPC and Lazarus for Raspbian

│ **English (en)** │

Raspbian Buster (based on Debian 10 Buster) has FPC and Lazarus available in the repositories, but they suffer from the same "design decisions" that don't allow for a nice experience, namely the smooth IDE rebuilding capability based on source packages that is possible with FPC/Lazarus official .deb packages vs the crippled experience with the official Debian/Raspbian packages. One wishes there were official FPC/Lazarus packages for Raspbian as there are for Intel Debian/Ubuntu, which are also frequently updated; not being that the case (at least for now), this page shows the steps for building your own FPC, FPC-source and Lazarus packages for Raspbian. Example shows steps for Lazarus 2.0.6 which includes FPC 3.0.4, most recent at the time of writing. Adjust accordingly. 

Requirements: 

  * Raspberry Pi 4 Model B, 4 GB (less memory models would require swap), with appropriate cooling (author uses aluminum case that doubles as heatsink) to avoid throttling due to excessive heat
  * Latest Raspbian Buster fully updated



Steps: 

  1. Install subversion: `sudo apt install subversion`
  2. Get bootstrap [FPC compiler](<https://sourceforge.net/projects/freepascal/files/Linux/3.0.2/fpc-3.0.2.arm-linux-eabihf-raspberry.tar/download>). The latest compiler is compiled with the previous version of itself.
  3. Extract compiler. This will create a `fpc-3.0.2.arm-linux` directory. (NOTE: this folder must be on a linux FS [like Documents folder] as FAT doesn't allow the next step to work).
  4. In this directory, as pi user, run `./install.sh`
  5. At the four prompts, answer `Enter, n, n, n`: this is, accept suggested directory `/home/pi/fpc-3.0.2`, no Textmode IDE, no documentation, no demos. Ignore `tar` warnings. Note the additional files written to `/home/pi`: `.fpc.cfg`, `.config/fppkg.cfg`, `.fppkg/config/default`. When the build is done, to delete this temporary bootstrap compiler these files and the `/home/pi/fpc-3.0.2` install directory need to be deleted.
  6. Temporarily add FPC install directory to PATH: `export PATH=$PATH:$HOME/fpc-3.0.2/bin`. If you have to reboot for any reason before completing the FPC build, you have to do this step again.
  7. Test if all ok: `fpc abcd`. This invokes the just installed compiler trying to compile a non-existent file (abcd). A multiline message will be shown including the compiler version.
  8. Non-step note: the following steps are mostly based on [this](<https://wiki.lazarus.freepascal.org/How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository#Creating_the_deb_files_yourself>)
  9. Install development libraries: `sudo apt install libgtk2.0-dev libgpm-dev libncurses-dev`
  10. `mkdir ~/pascal`
  11. `cd ~/pascal`
  12. `svn co https://svn.freepascal.org/svn/fpc/tags/release_3_0_4 fpc`, you have to be online for this and the next step to work.
  13. `svn co https://svn.freepascal.org/svn/lazarus/tags/lazarus_2_0_8 lazarus`
  14. `cd lazarus/tools/install`
  15. Edit `create_fpc_deb.sh`, look for line with the following content: `arm) ppcbin=arm; FPCArch=arm;;`, (currently line 109). Copy and paste this line below itself, so it will appear twice. Edit this copy to read like this: `armhf) ppcbin=arm; FPCArch=arm;;`. Now save the changes and exit.
  16. Run the script: `./create_fpc_deb.sh fpc ~/pascal/fpc/`. This will compile the latest compiler with the bootstrap compiler. It takes about 11 minutes. A `.deb` file will be created: `fpc-laz_3.0.4-yymmdd_armhf.deb`; the exact name will be shown at the end of the compilation. Rename the file with (NOTE: do not type the brackets): mv [file name shown at end of compilation] `fpc-laz_3.0.4_armhf.deb`
  17. You have completed the FPC build. Reboot the Pi to get rid of the temporary PATH.
  18. Uninstall the bootstrap FPC: `rm -rf ~/.fpc.cfg ~/.config/fppkg.cfg ~/.fppkg ~/fpc-3.0.2 ~/tmp`. Check what you just did: `fpc abcd` should say command is not found.
  19. Install the just created FPC Debian package: `sudo dpkg -i ~/pascal/lazarus/tools/install/fpc-laz_3.0.4_armhf.deb`. Check if it works: `fpc abcd` should show the newly installed FPC compiler.
  20. `cd ~/pascal/lazarus/tools/install`
  21. `./create_fpc_deb.sh fpc-src ~/pascal/fpc/`. This builds the FPC source package in about 3 minutes: `fpc-src_3.0.4-yymmdd_armhf.deb`. Rename with: mv `fpc-src_3.0.4-yymmdd_armhf.deb` `fpc-src_3.0.4_armhf.deb`
  22. Edit `create_lazarus_deb.sh`, look for line with the following content: `echo "$Arch is not supported."`, (currently line 86). Copy the 3 lines above it and paste them just below themselves: at this time it is copy lines 83 to 85 and paste them between lines 85 and 86. In the just pasted lines (lines 86-88), replace `sparc` with `armhf`, and `ppcsparc` with `ppcarm`. Look for the first `fi` just below; copy and paste it below itself (copy line 91 below itself, so line 91 and 92 read the same). For you coders out there, you are adding another `if` to a nested `if`, with its corresponding `fi`. Now save and exit.
  23. `./create_lazarus_deb.sh`. This will build the Lazarus IDE Debian package `lazarus-project_2.0.8-0_armhf.deb`. It will take about 15 minutes.
  24. Install the FPC source and Lazarus packages: `sudo dpkg -i fpc-src_3.0.4_armhf.deb lazarus-project_2.0.8-0_armhf.deb`
  25. Save `*.deb` to another location, to reinstall later if needed or to share with friends: remember this is free software.
  26. `cd`, `rm -rf ~/pascal ~/tmp`



You can now finally enjoy your fully working, fully up to date Lazarus. Find it in the Programming section of the Raspberry menu. A message titled "Error in EditorMacroScript" will be shown on first run; this also appears in the Raspbian Lazarus, reason still unknown to the original author of this page. Now try to install a package, for example `lazreport`; it will work! 

Want to help the FPC/Lazarus community? Host the resulting `.deb` packages somewhere on the Internet and drop a note on the forum to let everyone know; rinse and repeat for every release. 

DISCLAIMER: these are UNOFFICIAL packages created from modified official sources. DO NOT ASK FOR SUPPORT ANYWHERE, neither on the forums or with the author of this article; use at your own risk. 

### Upgrading

The following is not written by whoever worked out this process in the first place. It is completely un-authoritative, but it worked for me to upgrade from Laz 2.0.8 to 2.0.10. I make no other claims what-so-ever - Use at your own risk. _The bracketed numbers in the steps refer back to the original version (until someone edits it)_

Once you have followed the above steps once, you can shortcut the process a little to upgrade. In particular, Steps 1 - 8 should not be necessary, as we have the previous version already installed. 

  1. (9) Update development libraries: `sudo apt upgrade libgtk2.0-dev libgpm-dev libncurses-dev`
  2. (10) if required, `mkdir ~/pascal`
  3. (11) ` cd ~/pascal`
  4. If you didn't clean out the temp files from last time, do so now: `rm -rf fpc lazarus`
  5. (12) `svn co https://svn.freepascal.org/svn/fpc/tags/_[required-release]_ fpc`. _(if necessary, browse<https://svn.freepascal.org/svn/fpc/tags> to find the exact release.)_
  6. (13) `svn co https://svn.freepascal.org/svn/lazarus/tags/_[required-release]_ lazarus`. _(if necessary, browse<https://svn.freepascal.org/svn/lazarus/tags> to find the exact release.)_
  7. (14) `cd lazarus/tools/install`
  8. (15) Edit `create_fpc_deb.sh`, look for line with the following content: `arm) ppcbin=arm; FPCArch=arm;;`, (currently line 109). 
     1. Copy and paste this line below itself, so it will appear twice.
     2. Edit this copy to read like this: `armhf) ppcbin=arm; FPCArch=arm;;`. Now save the changes and exit.
  9. (16) Run the script: `./create_fpc_deb.sh fpc ~/pascal/fpc/`. This will compile the latest compiler with the bootstrap compiler. It takes about 11 minutes. A `.deb` file will be created: `fpc-laz__[fpc release]_ -yymmdd_armhf.deb`; the exact name will be shown at the end of the compilation.
  10. Rename the file with (NOTE: do not type the brackets): `mv _[file name shown at end of compilation]_ fpc-laz__[fpc release]_ _armhf.deb`
  11. (17) You have completed the FPC build. _Rebooting the Pi should**not** be necessary, as we made no temporary PATH. (Yay)_
  12. (18) Check/uninstall the bootstrap FPC: `rm -rf ~/.fpc.cfg ~/.config/fppkg.cfg ~/.fppkg ~/fpc-_[fpc_release]_ ~/tmp`. _(If you cleaned up after the initial build, you should only find`~/tmp` this time)_
  13. Check what you just did: `fpc abcd` should say command is not found.
  14. (19) Install the just created FPC Debian package: `sudo dpkg -i ~/pascal/lazarus/tools/install/fpc-laz__[fpc-release]_ _armhf.deb`. _(-i can also upgrade - the message should be "installing [new ver] over [old ver]")_
  15. Check if it works: `fpc abcd` should show the newly installed FPC compiler.
  16. (20) `cd ~/pascal/lazarus/tools/install`
  17. (21) `./create_fpc_deb.sh fpc-src ~/pascal/fpc/`. This builds the FPC source package `fpc-src__[fpc-release]_ -yymmdd_armhf.deb` in about 3 minutes.
  18. Rename with: `mv fpc-src__[fpc-release]-yymmdd_ _armhf.deb` `fpc-src__[fpc-release]_ _armhf.deb`
  19. (22) Edit `create_lazarus_deb.sh`, 
     1. Look for line with the following content: `echo "$Arch is not supported."`, (currently line 86).
     2. Copy the 3 lines above it and paste them just below themselves: at this time it is copy lines 83 to 85 and paste them between lines 85 and 86.
     3. In the just pasted lines (lines 86-88), replace `sparc` with `armhf`, and `ppcsparc` with `ppcarm`.
     4. Look for the first `fi` just below; copy and paste it below itself (copy line 91 below itself, so line 91 and 92 read the same).
     5. (For you coders out there, you are adding another `if` to a nested `if`, with its corresponding `fi`.)
     6. Now save and exit.
  20. (23) `./create_lazarus_deb.sh`. This will build the Lazarus IDE Debian package `lazarus-project__[version]_ -0_armhf.deb`. It will take about 15 minutes.
  21. (24) Install the FPC source and Lazarus packages: `sudo dpkg -i fpc-src__[version]_ _armhf.deb lazarus-project__[version]_ -0_armhf.deb`
  22. (25) Save `*.deb` to another location, to reinstall later if needed or to share with friends: remember this is free software.
  23. (26) `cd ~; rm -rf ~/pascal ~/tmp`



DISCLAIMER: Same disclaimer as in the original article above - if anything, more so!

---

_Source: [https://wiki.freepascal.org/Build_current_FPC_and_Lazarus_for_Raspbian](https://web.archive.org/web/20221004044259/https://wiki.freepascal.org/Build_current_FPC_and_Lazarus_for_Raspbian)_
