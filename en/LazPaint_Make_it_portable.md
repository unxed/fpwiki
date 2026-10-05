# LazPaint Make it portable

Go [back](<LazPaint.md> "LazPaint")

By default, LazPaint stores its configuration on the hard drive using the mechanism provided by the operating system to keep a unique configuration. It is however easy to make a portable version of LazPaint. Here are the steps. 

## Contents

  * 1 1\. Download binaries
  * 2 2\. Decompress
  * 3 3\. Configure
  * 4 4\. Create a link



### 1\. Download binaries

Download the compressed binaries for a 32-bit or 64-bit version of LazPaint. Do not use the installer because this will definitely install it on your computer. Instead, you want to put it on a USB key for example. 

### 2\. Decompress

Decompress it in a folder on a portable device such as a USB key. Alternatively you can do this on your hard drive and at the end copy the whole thing on another device. 

On Windows and Linux, you get a directory containing the executable binary file, some example of 3d object files (.obj) and a folder with translations (i18n). 

On MacOS, the structure is a bit different in order to be interpreted as an application. 

### 3\. Configure

Using a plain text editor like Notepad or Gedit, create a text file called _lazpaint.ini_ in the same directory as the binary file. 

Inside write the following: 
    
    
     [General]
     ConfigFile=LazPaint.cfg
    

On MacOS, the binary file is in the MacOS subdirectory. You will need to write a relative path to the Resources directory: 
    
    
     ConfigFile=../Resources/LazPaint.cfg
    

### 4\. Create a link

That's it. If you run the program now, it will write its configuration with the binaries. 

You may want to create a shortcut to the binary file. This will depend on your desktop environment.

---

_Source: [https://wiki.freepascal.org/LazPaint_Make_it_portable](https://web.archive.org/web/20250401001333/https://wiki.freepascal.org/LazPaint_Make_it_portable)_
