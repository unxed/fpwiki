# Spatialite

│ **English (en)** │

## Contents

  * 1 What is spatialite
  * 2 Spatialite in FPC/Lazarus
  * 3 References
  * 4 See also



## What is spatialite

Spatialite is an extension to SQLite that allows you to use spatial data, including OpenStreetMap road networks, that allow e.g. routing between destinations. It's meant for standalone use and can be thought of as a smaller cousin of PostGIS, and allows you to program your own GIS (Geographical Information System). 

The spatialite developers provide their own GUI, and projects such as QGIS support spatialite as well. 

## Spatialite in FPC/Lazarus

To use spatialite, you can either use an "all in one" package of spatialite and sqlite bundled into one big DLL/.so/library. Alternatively, you can use a regular sqlite DLL/.so/library, and load the sqlite library by calling the SQLite load_extension function. Support for this method has been added in FPC revision 20146, 21 January 2012. It is present in FPC fixes 2.6 and FPC trunk. 
    
    
      // Try to load extensions
      DBConnection.DatabaseName:='osm.sqlite';
      DBConnection.Open;
      DBConnection.LoadExtension('libspatialite-4.dll'); //Note: we need an open db before doing this
    

Once you've loaded the spatialite library, you can use spatial queries just as you would regular queries. 

## References

  * <https://bitbucket.org/reiniero/openrormap> Unfinished project that does demonstrate how to load spatialite. It downloads some OpenStreetmap data and is meant to load that into a spatialite database and create a map for the [Rigs of Rods game](<http://www.rigsofrods.com/content/>)
  * <https://www.gaia-gis.it/fossil/libspatialite/index> Spatialite homepage
  * <http://bugs.freepascal.org/view.php?id=20640> Patch that enabled SQLite load_extension



## See also

  * [Databases#sqldblaz.lpk](<Databases.md> "Databases")

---

_Source: [https://wiki.freepascal.org/Spatialite](https://web.archive.org/web/20240516115421/https://wiki.freepascal.org/Spatialite)_
