# LazarusScripts

## Windows

Here is a windows batch script that takes care of checking out and updating **Lazarus** from **GIT** to a subdirectory and even basic error fixing, because i have no way of knowing where Lazarus is installed it cannot compile or copy the files to your working Lazarus installation. Please save the script as **getlaz.bat** or some other name, however there is no warranty of any kind, use at your own risk, put it in a test directory first and make sure there is no subdirectory already called **lazarus**. 
    
    
    @echo off
    REM Script for checkout and update Lazarus from SVN
    echo.
     
    REM Check if lazarus directory exists to update or checkout first
    if exist lazarus (
    echo Updating Lazarus, please wait ...
    cd lazarus
    git pull
    if %errorlevel% NEQ 0 goto CleanupLaz
    cd ..
    goto Exit
    )
     
    echo Checking out Lazarus for the first time, this might take a while ...
    git clone https://gitlab.com/freepascal.org/lazarus/lazarus.git
    goto Exit
     
    :CleanupLaz
    echo.
    echo Something went wrong, please check or delete/move/rename the lazarus subdirectory ...
    goto Exit
    
    :Exit
    

## GNU/Linux

A trivial script to update the local repository for Lazarus etc.. 

In home-directory, have a special cvsroot-subdirectory. 

In this directory have a script for downloading Lazarus and other interesting projects. 

Lazarus is installed in /usr/share/lazarus. 

Before building a new version with the current remote repository from GitLab I make a backup, just in case the current remote repository is unstable. 

This is done by moving the /usr/share/lazarus directory to /usr/share/lazarus.BAK 

  
To adapt the script to your system, you need to alter the constants defined at the start of the script. 

It may not be working, as usual and does not catch any errors. 
    
    
    #!/bin/bash
     
    # get a local copy of the repository prior running this script - so first do:
    # git clone https://gitlab.com/freepascal.org/lazarus/lazarus.git
     
    # Define some directories , much better to not put stuff below /usr/ but below /home/ 
    # must point the the same root as LAZDIR
    BASEDIR=/usr/share/
    # dir of the git repository
    LAZCVSDIR=~/cvsroot/lazarus/
    # dir where lazarus is installed to
    LAZDIR=/usr/share/lazarus/
    # backup dir with the previous lazarus installation
    LAZBACKUP=/usr/share/lazarus.BAK
     
    # Remove old backup if it exists
    date +"%T %tStart"
    if [ -d $LAZBACKUP ]
    then 
      date +"%T %tRemoving old backup"
      rm $LAZBACKUP -fr
    else
      date +"%T %tNo backup to remove"
    fi
     
    date +"%T %tCreating copy of lazarus dir"
    cp -R $LAZDIR $LAZBACKUP
     
    # Getting stuff from GitLab
    date +"%T %tUpdate lazarus"
    git pull > ~/cvs_update.log
     
    # Copying cloned files to our laz dir.
    date +"%T %tcopying files"
    cp -Rf $LAZCVSDIR $BASEDIR --reply=yes
     
    # Make and building of lazarus
    date +"%T %tmake lazarus" 
    cd $LAZDIR
    make > /dev/null                   ### so you don't see all the errors which will inevitably break the script
    date +"%T %tmake the packages"
    make idepkg > /dev/null
    date +"%T %tFinished"

---

_Source: [https://wiki.freepascal.org/LazarusScripts](https://web.archive.org/web/20220704115220/https://wiki.freepascal.org/LazarusScripts)_
