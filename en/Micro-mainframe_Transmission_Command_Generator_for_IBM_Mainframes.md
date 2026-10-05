# Micro-mainframe Transmission Command Generator for IBM Mainframes

## Contents

  * 1 About
  * 2 License
  * 3 Author
  * 4 Download
  * 5 Change log
  * 6 Usage
    * 6.1 Micro-Mainframe Transmission
    * 6.2 MainFrame-Micro Transmission
  * 7 Description



## About

The application was written to run under Windows. I suppose that it runs under any version of Windows. It uses basic constructions and commands of Lazarus. 

## License

This application can be used under Lazarus modified LGPL and MPL, this allows making commercial projects that use this code but NO WARRANTY: you are the only one responsible if this code doesn't work or causes any problems to your computer. 

## Author

[Arí Ricardo Ody - Sao Paulo - Brazil](</index.php?title=User:Nightrider&action=edit&redlink=1> "User:Nightrider \(page does not exist\)")

## Download

You can download a zip with Windows executable and source or just a source zip from the [Lazarus CCR sourceforge download area](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=148359&release_id=378755>). 

## Change log

  * 16 December 2005: Initial version.
  * Last version: May, 2006.



## Usage

To use the program, simply run it. In the first screen click the left screen button to go to the next screen (the idea is to implement new functions in this screen in the future). In the second screen you must inform the name of a partitioned (PDS) or a sequential data set existing on the mainframe disks. 

One of these data sets will be used as input or output of the transmissions. 

You will set the type of transmission (texto for ASCII-EBCDIC and vice versa conversion or binario for no conversion), the type of emulator you will use and the direction of the transmission. You can go to the next screen, clean the fields of the current screen or go to one level above. 

Depending upon the selection you do the processing goes to one of two screens: 

### Micro-Mainframe Transmission

In this screen, when you click the first button on the top of the screen, you will receive a dialog to select the file(s) you want to transmit. You can click this button more than once and select another files in another directories. You can select files in the list and exclude them with enter or space keys. When you click the “Confirma” button the program shows a dialog where you must inform the name of the file where you want the program to write the output of its processing(it will be the command that the emulator will use). You can clean the screen via “Limpa” button or exit and go to one level above via “Sair” button. 

### MainFrame-Micro Transmission

In this screen you can 

  * Set the name of a text file that will contain the list of file names that you want to download from the mainframe (these files must be present in one of the data sets mentioned in the second screen above).
  * Next you will set the file extension the downloaded files must be recorded in the destination hard disk.
  * Next you will set the directory where the downloaded files will be written in the destination hard disk.
  * Finally you will set the path and the name of the file where the command to the emulator will be written.



When you click "executa" the command is generated and written in the file you have informed early in this screen. You can clean the screen via the "Limpa" button or exit and go to one level above via "Sair" button. 

If some people really need the screens of this program translated to English I can do this. But only if you'll really use it, please. 

## Description

This tool permits us a huge work economy in massive transmissions to and from the IBM mainframes we use. I work in a company that performs a migration from old databases to IBM DB2.

---

_Source: [https://wiki.freepascal.org/Micro-mainframe_Transmission_Command_Generator_for_IBM_Mainframes](https://web.archive.org/web/20241209223845/https://wiki.freepascal.org/Micro-mainframe_Transmission_Command_Generator_for_IBM_Mainframes)_
