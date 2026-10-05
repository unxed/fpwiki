# Kaldemonia

## Contents

  * 1 Kaldemonia
    * 1.1 About
    * 1.2 Requirements for Kaldemonia
    * 1.3 Requirements for Kaldemonia2
    * 1.4 Requirements for Kaldemonia3
    * 1.5 Notes on Kaldemonia
    * 1.6 Notes on Kaldemonia2
    * 1.7 Notes on Kaldemonia3
    * 1.8 Additional Notes
    * 1.9 Author
    * 1.10 License
    * 1.11 Download



## Kaldemonia

### About

Kaldemonia is a program where user can enter a search string that is used to search Google and return the results to three files: A Cache file, A Results file, and one large Htm file containing the full result from Google. This Program was done as practice for [me](</User:Blue1> "User:Blue1") in coding. 

Kaldemonia compiles on Linux and can be Cross-Compiled to Win32 system from Linux with minor modification. Kaldemonia shows using a worker thread and Gui thread so GUI remains responsive while still functioning. 

Kaldemonia2 used a new component instead of TidyPas due to TidyPas not compiling with fpc 2.0.4. The new component works very well to parse html document to extract the links needed. 

Kaldemonia3 goes a step further by having 10 threads fetching the Google pages simultanously. The Threaded part is seperated into it's own unit besides the main unit. Again everything works on both Linux and Win32. 

I used several examples to make it and asked many questions. So I hope you like it. 

### Requirements for Kaldemonia

1\. TidyPas can be downloaded here: [TidyPas](<http://curlpas.sourceforge.net/>)
    
    
         N.B. TidyPas requires TidyLib to be installed and on win32 LibTidy.dll.
    

2\. Synapse can be found here: [Ararat Synapse](<http://www.ararat.cz/synapse/>)
    
    
         I used Synapse version 37b6. I don't see that version anymore so I include it.
    

### Requirements for Kaldemonia2

1\. Synapse can be found here: [Ararat Synapse](<http://www.ararat.cz/synapse/>)
    
    
       Latest release should be fine.
    

2\. THTMLPars can be downloaded here: [HTMLPars](<http://www.spreendigital.de/DevTools.3.0.html>)
    
    
       I included it, with very minor modification to work on Linux.
    

### Requirements for Kaldemonia3

1\. Synapse can be found here: [Ararat Synapse](<http://www.ararat.cz/synapse/>)
    
    
       Latest release should be fine.
    

2\. THTMLPars can be downloaded here: [HTMLPars](<http://www.spreendigital.de/DevTools.3.0.html>)
    
    
       I included it, with very minor modification to work on Linux.
    

### Notes on Kaldemonia

  * Kaldemonia was compiled with FPC 2.0.2 and Lazarus 0.9.16
  * TidyPas release date 2005-12-25
  * Synapse version 37b6



### Notes on Kaldemonia2

  * Kaldemonia2 was compiled with FPC 2.0.4 and Lazarus 0.9.16 & 0.9.18
  * Synapse release number 37
  * THTMLParser v1.17



### Notes on Kaldemonia3

  * Kaldemonia3 was compiled with FPC 2.0.4 and Lazarus 0.9.21 beta
  * Synapse release number 38b4
  * THTMLParser v1.17



### Additional Notes

Kaldemonia compiles on both Linux and Windows without errors. However, it runs well on Linux, but not on Windows. The reason is not clear, but debugging it shows Synchronize(@ShowStatus) to be where the problem occurs. 

Added October, 9th 2006: 
    
    
     Although Lazarus executable for Win32 freezes, same source 
     (with very minor modification) compiles with delphi and runs 
     fine on Win32 environment.
    

  


### Author

[Blue1](</User:Blue1> "User:Blue1")

### License

GPL. 

### Download

You can download Kaldemonia source from [Lazarus CCR](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=148359&release_id=448040>). 

You can download Kaldemonia2 source from [Lazarus CCR](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=148359&release_id=454415>). 

You can download Kaldemonia3 source from [Lazarus CCR](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=148359&release_id=484669>).

---

_Source: [https://wiki.freepascal.org/Kaldemonia](https://web.archive.org/web/20241202160524/https://wiki.freepascal.org/Kaldemonia)_
