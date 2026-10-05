# Lazarus Can't find unit system used by fcllaz

You typically see this error message following a fresh install or when trying to build Free Pascal and Lazarus from source code. The error means literally that the pre-compiled units of Free Pascal for one reason or another are simply not being found by the compiler. There are generally 2 sources for these errors -- left over remnants from old install and environment problems. 

  


## Contents

  * 1 Program Placement
  * 2 Finding Dangling Files
  * 3 Environment Files
  * 4 fppkg



## Program Placement

There are currently at least 2 views of where FreePascal and Lazarus are to be installed on Linux, Unix and similar systems. The first is a more security conscious thought pattern where the executables are placed in either /root/bin or /usr/bin, libraries are placed in /root/lib or /usr/lib, shared files are placed in /usr/share, and configuration files in /home/username. The other favors control by the user by placing executables in /home/username/FreePascal, libraries and pre-compiled units in /home/username/FreePascal/lib/fpc/version-number/units and configuration files in /home/username. The word 'username' is typically the users name or handle. FreePascal could be a higher directory with lower directories for fpc-3.0.0 and lazarus. I follow this latter paradigm keeping all of my Free Pascal/Lazarus files in one convenient location. 

With Windows the same thing can happen where multiple files exist in C:\Program Files\, C:\Program Files(x86)\, C:\lazarus or another custom directory. If a user is trying to run multiple installations of different versions there are other wiki pages addressing that problem. 

  


## Finding Dangling Files

If you have had previous installations of Free Pascal or Lazarus, a subsequent installation may place the new files in a different location, e.g. instead of /usr/local/bin using /usr/bin. The result is that you may have an existing configuration that is causing confusion. If you only plan on having a single installation, then your path is clear -- either remove all of the files and start over or remove all of the files not from the current installation. I find that if you are unclear, then start over. One thing that can help is to examine the dates of the files. The newer ones go with the current installation. 

  


## Environment Files

FreePascal typically uses a file called fpc.cfg and another fppkg file called default. These files are usually found in /home/username/.fpc.cfg and /home/username/.fppkg/config/default. Windows installations are typically found in C:\lazarus\fpc\<version number>\<cpu>-<operating-system> You may find an old version exists which could be the cause of the error message or you may find that the files simply don't exist. The first thing to do is to find out what your situation is by looking. 

If the file doesn't exist, then from a command window in the .../fpc/bin directory execute: fpcmkcfg. By default it prints out a default fpc.cfg file. The portion that needs focus to address the problem is found in the first half of the listing. Copy the listing to an editor to make changes. The areas we need to focus on is: 
    
    
          [![fpcmkcfg1.png](https://wiki.freepascal.org/images/e/e3/fpcmkcfg1.png)](</File:fpcmkcfg1.png>)
    

In my case under Linux, I changed these files like this: 
    
    
          [![fpccfg2.png](https://wiki.freepascal.org/images/2/29/fpccfg2.png)](</File:fpccfg2.png>)
    

In the first case references are made with variables and contemplates a prefix of /usr/bin - /usr/lib or /usr/local/bin - /usr/local/lib and /usr/share or /usr/local/share. In my case I have input the precise location. 

Under Windows installation this section in the fpc.cfg probably looks something like this: 
    
    
          [![fpccfg3.png](https://wiki.freepascal.org/images/2/23/fpccfg3.png)](</File:fpccfg3.png>)
    

Regardless of which type of installation that is used, the entries need to accurately reflect where files are located. The edit file should be saved as either \fpc.cfg or ../.fpc.cfg. It is typically placed in the /home/username/.fpc.cfg as a hidden file. 

  


## fppkg

The other area that typically causes problems relate to the fppkg file. The fppkg files usually reside in /home/username/.fppkg as a hidden directory. If you cannot see these files, go to the setting for your file browser, e.g. Dolphin, Nautilius, Windows File Explorer, etc. In Linux the option to view hidden files is found in this settings menu, while in Windows you have to open Control Panel and select File Explorer Options in Windows 10, or a similar name in earlier versions. From a console or command window run the program 'fppkg list'. If it comes out looking something like this, you have a problem: 
    
    
          [![fppkg-list1.png](https://wiki.freepascal.org/images/0/0f/fppkg-list1.png)](</File:fppkg-list1.png>)
    

Next, you need to get a listing of where your settings are pointing. From the console or command window run the program 'fppkg listsettings'. The file should like something like this: 
    
    
          [![fppkg-listsettings.png](https://wiki.freepascal.org/images/4/42/fppkg-listsettings.png)](</File:fppkg-listsettings.png>)
    

If your settings refer to '/usr' or '/usr/local' when you have placed your entire installation under '/home/username/FreePascal', then you need to change the file .../fppkg/config/default or .../.fppkg/config/config, or, if doing a Windows installation 'C\Users\username\AppData\Local\FreePascal\fppkg\config\default'. 

Once those settings are correct, you should be able to run from a console or command window 'fppkg list' and get something like this: 
    
    
          [![fppkg-list2.png](https://wiki.freepascal.org/images/a/ae/fppkg-list2.png)](</File:fppkg-list2.png>)
    

Now try to run the install of Lazarus. The problem should be gone.

---

_Source: [https://wiki.freepascal.org/Lazarus_Can%27t_find_unit_system_used_by_fcllaz](https://web.archive.org/web/20241202175306/https://wiki.freepascal.org/Lazarus_Can%27t_find_unit_system_used_by_fcllaz)_
