# Pascal and GIS

## Pascal for GIS toolkit

<https://macpgmr.github.io/MacXPlatform/PascalDynLibs_3.html>

(Note that this article is part of a larger series on how to use compiled Pascal dynamic libraries with a variety of different programming languages.) 

The Pascal for GIS toolkit includes the source for example apps that demonstrate various ways to use Pascal (and in some cases Lazarus) in developing GIS apps: 

  * Add non-visual geoprocessing functionality to any Pascal app (testgdal, testshapes).


  * Develop a GIS app using the new GIS map and legend controls for Lazarus LCL (PasCast).


  * Place non-GIS Pascal code in a separate compiled library, then call the library from various GIS apps: 
    * Simple mapping with a mobile Web app (QxCast) that uses a Pascal FCGI server app.
    * Simple mapping with a standalone app (SwiftCast).
    * Clickable read-only map with a Web app (AspCast).
    * Clickable read-write map with a GIS plugin (PyCast).



All of the *Cast example apps use the same Pascal dynamic library that retrieves weather forecast data via the National Digital Forecast Database (NDFD) Web service. 

  


## OpenGPSX

<https://sourceforge.net/projects/opengpsx/>

OpenGPSX Lazarus Component for GPS Interface programming. Decode NMEA messages, Display GPS skyplot view, Display GPS signal strength, Display target navigation. 

### Features

This is a Lazarus component to demonstrate the real-time tracking and monitoring NMEA-183 compliant from GPS receiver and decode NMEA sentences likes $GPGGA, $GPGSA, $GPGSV, $GPRMC, $GPGLL, $GPVTG, $GPBOD, $GPWPL, $GPRTE 

  * Display GPS skyplot.
  * Display GPS signal strength.
  * Display target navigation.
  * Interfaced to GPS with Serial Port.
  * Cross-platform, Now works on Windows and Linux.



[![opengpsx.jpg](https://wiki.freepascal.org/images/c/c9/opengpsx.jpg)](</File:opengpsx.jpg>)

---

_Source: [https://wiki.freepascal.org/Pascal_and_GIS](https://web.archive.org/web/20240920204109/https://wiki.freepascal.org/Pascal_and_GIS)_
