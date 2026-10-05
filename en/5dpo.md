# 5dpo

## Contents

  * 1 About
  * 2 Authors
  * 3 License
  * 4 Download
  * 5 Source Code
  * 6 Changelog



## About

The 5dpo Component Library (Sdpo) is a set of components released by the [5dpo Robotic Soccer Team](<http://www.fe.up.pt/~robosoc>) when we ported our robots' software from Kylix to Lazarus. It also includes units that are not components like the SdpoDynmatrix unit that implements a general propose matrix with the standard operations overloaded. 

This library includes a serial communication component (TSdpoSerial), an UVC video driver component (TSdpoVideo4L2), a Gtk form component (TSdpoFastForm), an GigE camera component for cameras using the PvAPI (TSdpoPvAPI and TSdpoPvCamera), a componente to access the images from the Kinect (TSdpoFreenect) and a Joystick component (TSdpoJoystick). 

  * TSdpoSerial allows asynchronous communication using a serial port based on [Synaser](<Projects_using_Lazarus.md> "Projects using Lazarus"). Works in Windows and Linux. Tested on the Raspberry Pi 2, also. 


  * TSdpoPvAPI and TSdpoPvCamera allow to capture images from cameras conforming to the PvAPI. Mostly industrial vision cameras like the GigE camera MAKO G-125C and many others. Works only in Linux. 


  * TSdpoVideo4L2 allows the control of UVC compliant USB cameras, tested with: 
    * PS3 PlayStation Eye Camera 
    * Logitech Quickcam Orbit/Sphere AF 
    * Logitech Quickcam Pro 9000 
    * Logitech Quickcam Ultra Vision 
    * Acer CrystalEye webcam (Acer Aspire One) 
    * Imaging Source industrial and astronomy cameras 



For more information go to <http://linux-uvc.berlios.de/>. Works only in Linux. 

  * TSdpoFreenect can read the RGB and depth images from the original Xbox Kinect. It works in Linux and uses the OpenKinect driver. 


  * TSdpoJoystick returns the state of the joystick's axis and buttons. Works in Windows and Linux. 


  * TSdpoFastForm creates a Gtk window that is useful for fast painting of images (like the ones from the camera) and allows full control of the paint action, useful for real-time image processing. Works only in Linux. 



## Authors

  * Paulo Costa 
  * Paulo Malheiros 
  * Paulo Marques 
  * Joao Paulo Silva 



## License

Modified LGPL (read LICENSE and LICENSE.ADDON included). 

## Download

[[1]](<https://sourceforge.net/projects/sdpo-cl/files/latest/download>)

## Source Code

  * Mercurial 
    * Browse: <http://sdpo-cl.hg.sourceforge.net/hgweb/sdpo-cl/sdpo-cl/>
    * Clone: _hg clone http://hg.code.sf.net/p/sdpo-cl/mercurial sdpo_


  * Subversion (deprecated since 05.03.2012) 
    * ~~Browse:<http://sdpo-cl.svn.sourceforge.net/viewvc/sdpo-cl/>~~
    * ~~Checkout:_svn co https://sdpo-cl.svn.sourceforge.net/svnroot/sdpo-cl Sdpo_~~



## Changelog

  * 01.02.2016 - 0.3.0 released. SdpoDynmatrix: TDMatrix now supports element access like an array: A[r, c] := B[r, c] + 1.0; 


  * 07.04.2012 - 0.2.0 released. Removed support for SdpoVideo1394. Added SdpoPvAPI implementation, SdpoFreenect for Kinect, matrix functions in SdpoDynmatrix and debayer functions in SdpoDebayer. Changed file structure. 


  * 08.11.2010 - 0.1.8 released. Added FlowControl in SdpoSerial. SdpoFastForm ported to Windows 


  * 30.03.2010 - 0.1.7 released. Minor bug fix in SdpoSerial and SdpoVideo1394. Added icon to SdpoFastForm and Lazarus 0.9.28 support. Components tested in Linux 64 bit 


  * 17.02.2009 - 0.1.6 released. Added TSdpoJoystick. Updated SdpoVideo4L2 to work with Monochrome and Bayer cameras. Added form functions to SdpoFastForm 


  * 15.10.2008 - 0.1.5 released. Added SdpoVideo4L2 


  * 02.04.2008 - 0.1.4 released. Updated Canvas in SdpoFastForm 


  * 22.02.2008 - 0.1.3 released. Created Canvas in SdpoFastForm. Fixed non locking in SdpoSerial 


  * 24.01.2008 - 0.1.2 released. Gtk2 support in SdpoFastForm 


  * 12.01.2008 - 0.1.1 released. Created SdpoVideo1394, SdpoSerial and SdpoFastForm 



  
Read the CHANGELOG file included for more details

---

_Source: [https://wiki.freepascal.org/5dpo](https://web.archive.org/web/20171119164209/https://wiki.freepascal.org/5dpo)_
