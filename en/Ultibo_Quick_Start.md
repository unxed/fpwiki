# Ultibo Quick Start

Our goal is to use a Raspberry Pi (optional - Zero Wireless With Headers with [Sense HAT](<https://ultibo.org/wiki/Unit_RPISENSEHAT>) documented more [here](<https://www.raspberrypi.org/products/sense-hat/>), [here](<https://astro-pi.org/wp-content/uploads/2018/09/T05.2_Meet-the-Sense-HAT.pdf>), and [here](<http://www.esa.int/Education/AstroPI/European_Astro_Pi_Challenge_2019-20_now_open>)).  
  
\- Use a PC to install [PINN Lite](<https://www.matthuisman.nz/2017/02/how-to-install-pinn-lite.html>) onto a 8 GB SD card.  
\- Set up hardware similar to [this](<https://www.raspberrypi.org/learning/parents-guide/>), but the Zero is a little different as need USB hub and several micro to standard cables.  
**PINN works with wireless keyboards and mice (unlike NOOBS)!**  
\- Power up your Raspberry Pi, configure Internet access (if using WiFi)  
\- Select your Language near the bottom of the screen  
\- Make sure Raspbian Lite is selected to install, then click the Install button.  
\- When reboots, delay login as Raspbian does updates automatically by itself, login (user pi and password raspberry)  
\- We are basically following the "headless" set up [here](<https://learn.sparkfun.com/tutorials/python-programming-tutorial-getting-started-with-the-raspberry-pi/introduction>).  
\- Enter the following lines **(without bold comment text)**  
sudo sh -c "apt update && apt dist-upgrade && apt autoremove"  
sudo raspi-config **(to select timezone under 4-Localisation Options)**  
sudo reboot  
**(login)**  
**The reason binutils-arm-none-eabi is needed when running natively:** [arm_none_eabi](<https://ultibo.org/wiki/Building_for_Raspbian#Installing_the_arm-none-eabi_Toolchain>)  
sudo apt install binutils-arm-none-eabi  
wget <https://github.com/ultibohub/Tools/releases/latest/download/ultiboinstaller.sh>  
chmod u+rx ultiboinstaller.sh  
**Note: the script below takes about 30 minutes on a RPi3B, about 80 minutes on a Zero.  
** ****./ultiboinstaller.sh**(confirm don't want Lazarus, do want to build the Hello World examples)**  
**Completes on RPi3B and Zero.**  
sudo poweroff **(wait for LED off before remove power)**  


## See also

  * [Ultibo_core](<Ultibo_core.md> "Ultibo core")
  * [EasyUltibo](<http://turbocontrol.com/easyultibo.htm>) \- where this wiki page started, more standard FPC & Python for SenseHat

---

_Source: [https://wiki.freepascal.org/Ultibo_Quick_Start](https://web.archive.org/web/20221003212916/https://wiki.freepascal.org/Ultibo_Quick_Start)_
