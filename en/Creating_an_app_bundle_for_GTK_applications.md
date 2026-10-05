# Creating an app bundle for GTK applications

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │ 

For macOS you need to create an app bundle for an executable file in order to drop it on the dock or launch it by double-clicking. An app bundle is really just a special folder that can have the same name as the executable, but with an .app extension. Finder doesn't display the .app extension, although you'll see it if you use ls in a Terminal window. 

The Carbon and Qt pages both explain how to create an app bundle for apps compiled with those widgetsets. With a GTK app, it's a bit more complicated since X11 always has to be running in order to run the app. Copy and paste the following script to a file named _create_gtk_app.sh_ , use chmod +x on the file so macOS will let you run it, then run it as follows to create an app bundle (substitute the name of your own GUI app compiled with the GTK widgetset): 
    
    
    ./create_gtk_app.sh execfile
    

Now you can drag and drop the resulting app bundle onto the dock just like any other application. 

For more information, study the script. 

_**Tip:**_ In the script below, one of the lines is really long and may get chopped off when you print this page _(but why would you print this page only to type it in again when you can just copy and paste?) --[BigChimp](</User:BigChimp> "User:BigChimp") 12:37, 24 November 2013 (CET)_. Here's the line on two lines. Just be sure it's all one line in your script file. 
    
    
     echo '<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" 
      "http://www.apple.com/DTDs/PropertyList-1.0.dtd">' >>$plistfile
    
    
    
    #!/bin/sh
    # Force Bourne shell in case another shell is the default.
    #
    appname=$1
    appfolder=$appname.app
    macosfolder=$appfolder/Contents/MacOS
    plistfile=$appfolder/Contents/Info.plist
    if [ $appname = "lazarus" ]
    then
      appfile=/usr/local/share/lazarus/lazarus
    else
      appfile=$appname
    fi
    #
    if [ "$appname" = "" ]
    then
      echo "Usage: $0 executable_file"
      echo "Creates .app bundle (folder) for executable file that uses GTK widgetset"
    elif ! [ -e $appfile ]
    then
      echo "$appfile does not exist"
    elif [ -e $appfolder ]
    then
      echo "$appfolder already exists"
    else
      echo "Creating $appfolder..."
      mkdir $appfolder
      mkdir $appfolder/Contents
      mkdir $appfolder/Contents/MacOS
      mkdir $appfolder/Contents/Resources
    #
      if [ $appname = "lazarus" ]
      then
    # This is special case for lazarus IDE.
    # Create a script file in .app folder that starts X11, then Lazarus.
        echo '#!/bin/sh' >$macosfolder/$appname.sh
        echo '# This script starts X11, then Lazarus.' >>$macosfolder/$appname.sh
        echo 'open -a x11' >>$macosfolder/$appname.sh
        echo "export DISPLAY=':0.0'" >>$macosfolder/$appname.sh
        echo 'open -a '$appfile >>$macosfolder/$appname.sh                #IMPORTANT! If you're running TCSH, or other,
        chmod +x $macosfolder/$appname.sh                                 # "open -a" doesn't work. Use "open" instead
      else
    # Instead of copying executable into .app folder after each compile,
    # simply create a symbolic link to executable.
        ln -s ../../../$appname $macosfolder/$appname
    # Create a little script file in .app folder that opens executable with X11.
        echo '#!/bin/sh' >$macosfolder/$appname.sh
        echo '# This script opens the executable file with X11.' >>$macosfolder/$appname.sh
        echo 'open ${0/%.sh}' >>$macosfolder/$appname.sh
        chmod +x $macosfolder/$appname.sh
      fi
    #
    # Create PkgInfo file.
      echo "APPL????" >$appfolder/Contents/PkgInfo
    #
    # Create information property list file (Info.plist).
      echo '<?xml version="1.0" encoding="UTF-8"?>' >$plistfile
      echo '<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">' >>$plistfile
      echo '<plist version="1.0">' >>$plistfile
      echo '<dict>' >>$plistfile
      echo '  <key>CFBundleDevelopmentRegion</key>' >>$plistfile
      echo '  <string>English</string>' >>$plistfile
      echo '  <key>CFBundleExecutable</key>' >>$plistfile
      echo '  <string>'$appname'.sh</string>' >>$plistfile
      echo '  <key>CFBundleInfoDictionaryVersion</key>' >>$plistfile
      echo '  <string>6.0</string>' >>$plistfile
      echo '  <key>CFBundlePackageType</key>' >>$plistfile
      echo '  <string>APPL</string>' >>$plistfile
      echo '  <key>CFBundleSignature</key>' >>$plistfile
      echo '  <string>????</string>' >>$plistfile
      echo '  <key>CFBundleVersion</key>' >>$plistfile
      echo '  <string>1.0</string>' >>$plistfile
      echo '  <key>CSResourcesFileMapped</key>' >>$plistfile
      echo '  <true/>' >>$plistfile
      echo '</dict>' >>$plistfile
      echo '</plist>' >>$plistfile
    fi

---

_Source: [https://wiki.freepascal.org/Creating_an_app_bundle_for_GTK_applications](https://web.archive.org/web/20231129022816/https://wiki.freepascal.org/Creating_an_app_bundle_for_GTK_applications)_
