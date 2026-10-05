# Speecher Assistive Kit

## Contents

  * 1 About
  * 2 Download
  * 3 License
  * 4 Usage



## About

**sak** is a Speecher Assistive Kit which transforms your applications into multi-language speaking assisted applications. Easy and independent, you do not have to change anything in your original code. Compatible with Lazarus, fpGUI, MSEgui and console. 

For Linux, FreeBSD, macOS and Windows. 

You do not need external libraries like at-spi, sapi, nor do you need to install anything in your system. Everything is included in the kit. 

sak uses the [eSpeak](<http://espeak.sourceforge.net/>) and [PortAudio](<http://www.portaudio.com/>) open source libraries. 

## Download

GitHub: <https://github.com/fredvs/sak>

## License

sak uses eSpeak who is GPL license. So the license differs if you use eSpeak executable or eSpeak library. 

  * **sak** : for all types of licenses, included commercial applications without source. It uses Speak executable and new PortAudio dynamic library.
  * **sak_dll** : for applications with GPL Open Source licences. It uses eSpeak dynamic library and new PortAudio dynamic library.



## Usage

Copy the **sakit/** (or **sakit_dll/**) folder into your application-folder (from sak/ or sak_dll/, depend of the license you give...). 

  * For **sak** , add **sak.pas** into your application-folder.
  * For **sak_dll** , add **sak_dll.pas** , **uos_portaudio.pas** and **uos_espeak.pas** into your application-folder.



In the "uses" section of your program add: 

  * For **sak** : "uses sak"
  * For **sak_dll** : "uses sak_dll"



In your main program: When you want your application becomes _assistived_ call: 
    
    
    SAKLoadLib();
    

This will seek in program-directory for /sakit/ (if not, in system for eSpeak). 

or 
    
    
    SAKLoadLib('/path/of/sakit/');
    

This loads some custom path. 

  
To unload it : 
    
    
    SAKUnLoadLib;
    

  
You may change the default gender and language. 

  * gender: male or female
  * language: language code



Here example for Portugues/Brasil woman: 
    
    
    SAKSetVoice(female,'pt');
    

In addition you may add your own speech with : 
    
    
    SAKSay(Text: string);
    

When you close your application, call : 
    
    
    SAKUnLoadLib;

---

_Source: [https://wiki.freepascal.org/index.php?title=Speecher_Assistive_Kit&oldid=153340](https://web.archive.org/web/20230324034209/https://wiki.freepascal.org/index.php?title=Speecher_Assistive_Kit&oldid=153340)_
