# LazAutoUpdater/Systray Service

### Summary

  * This application is designed to run at OS startup
  * It silently checks for application updates, and if it finds an update will silently perform the update - even if the application is running
  * The LazAutoUpdate component (>=v0.21) has methods that enable your application to integrate seamlessly with the Systray Updater service
  * The Systray application is intended to be installed with your LazAutoUpdate-enabled application
  * The LazAutoUpdate Pack application automatically generates a configuration file for the Sytray Application



### Using the LazAutoUpdater Systray Service

This is even simpler to code in your app. 

  * Drop a LazAutoUpdate component on your main form
  * Set the VersionsININame, ZipfileName and SFProjectName to unique values for your app (the same values as your Update Pack uploads)
  * In the form.create() add the following lines


    
    
    If LazAutoUpdate1.CreateLocalLauImportFile
      then LazAutoUpdate1.RelocateLauImportFile;
    

That's it! 

  * If the user is running Systray Service, it will will automatically import your app's configuration and update it on a schedule (default 9am Daily)
  * To deploy updates, configure (using the values specified in your app) and run AutoUpdate Pack, then upload as usual.


  * An absolute minimum of coding effort to keep your users updated.



### How it works

  * Using a [longtimer](<../longtimer.md> "longtimer") the systray app will periodically query whether the configured app needs updating. If it does, then:


  1. If the app is not running, it downloads and then copies over the new version of the app
  2. If the app is running, it calls lauupdate(.exe) to remotely close, update then re-open the app.


  * Every so often (default=1minute) the systray app checks for a file named 'lauimport.ini' in the user's appdata folder, and if it finds it it will:


  1. Merge its contents with the app configuration file
  2. Delete the original



In this way, an update's information can be reflected in the configuration file.

---

_Source: [https://wiki.freepascal.org/LazAutoUpdater/Systray_Service](https://web.archive.org/web/20250417161406/https://wiki.freepascal.org/LazAutoUpdater/Systray_Service)_
