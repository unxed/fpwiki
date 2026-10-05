# LazMapViewer

[Template:LazMapViewer](</index.php?title=Template:LazMapViewer&action=edit&redlink=1> "Template:LazMapViewer \(page does not exist\)")

[![lazmapviewer.png](https://wiki.freepascal.org/images/1/12/lazmapviewer.png)](</File:lazmapviewer.png>)

Map provider: [OpenStreatMap Mapnik](<https://www.openstreetmap.org>), [Open Data Commons Open Database License](<https://opendatacommons.org/licenses/odbl/>)

## Contents

  * 1 About
  * 2 Download and Installation
    * 2.1 Development version
    * 2.2 Release version
    * 2.3 Requirements
  * 3 The Main Map Viewer: TMapView
    * 3.1 Getting Started
      * 3.1.1 Preparation
      * 3.1.2 Magnification
      * 3.1.3 Location
    * 3.2 Basic Documentation
      * 3.2.1 Properties
      * 3.2.2 Events
      * 3.2.3 Main methods
    * 3.3 Map Providers
    * 3.4 The Drawing Engine
    * 3.5 Map Overlays
      * 3.5.1 Basic overlay architecture
        * 3.5.1.1 Classes
        * 3.5.1.2 Examples
          * 3.5.1.2.1 Adding a point-of-interest
          * 3.5.1.2.2 Overlaying another tile layer
          * 3.5.1.2.3 Creating a custom track
          * 3.5.1.2.4 Reading and displaying a track from a gpx file
      * 3.5.2 Creating overlays at designtime
        * 3.5.2.1 Classes
        * 3.5.2.2 Examples
          * 3.5.2.2.1 Overlaying a second map as a layer
          * 3.5.2.2.2 Overlaying a layer with points-of-interest
          * 3.5.2.2.3 Reading and displaying a track from a gpx file
          * 3.5.2.2.4 Editing points by using the built-in points editor
  * 4 Plugins
    * 4.1 What is a plugin and what can I do with it?
    * 4.2 Using existing plugins
      * 4.2.1 Creating plugins at design-time
      * 4.2.2 Creating plugins at run-time
    * 4.3 Available plugins
    * 4.4 Creating a new plugin class
  * 5 Finding Geo Locations: TMvGeoNames
    * 5.1 Getting Started
    * 5.2 Documentation
      * 5.2.1 Methods
      * 5.2.2 Properties
      * 5.2.3 Events



## About

**LazMapViewer** is a component for embedding maps obtained from the internet, such as Google maps or OpenStreetView, into a Lazarus form. 

The initial version of the package was written by Maciej Kaczkowski and later improved by members of the Lazarus forum. The package is currently maintained by Werner Pamler (wp_xxyyzzz-at-gmx-dot-net). 

License: Modified LGPL (with linking exception, like Lazarus LCL). 

## Download and Installation

### Development version

**Download**

Use an SVN client to download the current trunk version from <svn://svn.code.sf.net/p/lazarus-ccr/svn/components/lazmapviewer>. 

Alternatively download the zipped snapshot from <https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/lazmapviewer/> and unpack it to some folder. 

**Installation**

In Lazarus, go to "Package" > "Open Package File .lpk". Navigate to the folder with the LazMapViewer sources, and select `lazmapviewerpkg.lkp`. Click "Compile", then "Use" > "Install". This will rebuild the IDE (it may take some time). When the process is finished the IDE will restart, and you'll find the new components on the palette **MapViewer**. 

There are optional, supporting packages to extend the basic functionality. They must be installed in the same way, after `lazmapviewerpkg.lpk`. 

Package file name | Contained components | Purpose   
---|---|---  
lazmapviewerpkg.lpk | TMapView | Main component to display maps   
| TMvDEFpc | Default download engine using the internal fpc routines (fphttpclient).   
| TMvDECache | Offline-only access to map tiles   
| TMvPluginManager | Extends the functionality of TMapView by plugins   
| TMvGeoNames | Access to geo coordinates of cities   
lazmapviewer_synapse.lpk | TMvDESynapse | Alternative download engine based on the [Synapse](<Synapse.md> "Synapse") library which must be installed first.   
lazmapviewer_rgbgraphics.lpk | TMvRGBGraphicsDrawingEngine | Alternative drawing engine utilizing the [LazRGBGraphics](<LazRGBGraphics.md> "LazRGBGraphics") package which must be installed first.   
lazmapviewer_bgra.lpk | TMvBGRADrawingEngine | Alternative drawing engine based on the [BGRABitmap](<BGRABitmap.md> "BGRABitmap") package which must be installed first.   
  
### Release version

Occasionally, release versions are made available via the [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager") for easy one-click-installation directly from Lazarus: Just check the package (and the sub-packages needed) in the OPM window and click on the "Install" button. 

### Requirements

Release v0.2.6 was tested successfully on Windows, Linux gtk2/gtk3/qt5/qt6, macOS Cocoa with Lazarus/FPC combinations from current Laz 4.99/FPC 3.3.1 back to "ancient" Laz 1.6.4/FPC 3.0.2. 

## The Main Map Viewer: TMapView

### Getting Started

Here is a short tutorial to create your first map. 

#### Preparation

  * Drop a `TMapView` component on the form and size it as needed. The component is on the _MapViewer_ tab of the Lazarus component palette.
  * With the `MapView` selected in the object inspector, pick one of the items from the `MapProvider` combobox, and set the `Active` property to `true` \- after a short time of loading you'll see your first map!
  * In the background the `MapView` component has established an internet connection to the specified map provider and downloaded the default map. The maps are delivered as a series of **tiled images** at given size, usually 256x256 pixels in png or jpeg format.
  * When property `CacheOnDisk` is set to `true` the tile images are stored in a cache directory and used primarily to reduce internet traffic; only when an image is not found another download from the internet is triggered again. Depending on the `CacheLocation` property the cache can be set up to be in the user profile (`clProfile`), the temp (`clTemp`) or a custom directory (`clCustom`, path `CachePath`).
  * It is recommended to turn off the `Active` property during normal work because the time to load the form with the `MapView` sometimes can be annoyingly long. In order to see the map at run time, you should add the following handler for the form's `OnShow` or `OnActivate` event and set the `Active` there:


    
    
    procedure TForm1.FormActivate(Sender: TObject);
    begin
      MapView1.Active := true;
    end;
    

  * Another important optimization is to make sure that the `UseThreads` property is `true`. This delegates downloading and drawing to several tasks and considerably speeds up viewing of the maps.
  * That's all. Compile, run and see your fist map! Use the left mouse button to drag the map to another location, and rotate the mouse wheel to change the magnfication.



#### Magnification

  * The map images are provided in different magnifications depending on the `Zoom` level of the `MapView`. Each step in `Zoom` level corresponds to doubling the magnification. The lowest zoom level (`0`) displays the entire world in a single tile, level `1` holds a quarter in each tile, etc. The maximum `Zoom` level for most map providers is 18 -- this means that the most detailed maps cover a fraction of `1/2^18` of the earth circumference, about 150 m (at the equator).
  * So, if you want to see **more details** you must increase the `Zoom` value of the `MapView`. You can enter the requested value in the Object Inspector at **designtime** , or add a `TScrollbar` or `TTrackbar` to the form to change the magnfication interactively by using the following simple `OnChange` event handler (set the `Max` of the bar to, say, 18:


    
    
    procedure TForm1.TrackBar1Change(Sender: TObject);
    begin
      MapView1.Zoom := TrackBar1.Position;
    end;
    

  * Alternatively, zooming can also be achieved by rotating the **mouse wheel** provided that `mvoMouseZooming` is set in the `Options`. When `ZoomToCursor` is `true` the zoom operation is relative to the mouse cursor, otherwise to the map center.



#### Location

  * By default the **center of the map** is the intersection of the 0-degree meridian with the equator - which is a bit off of the coast of western Africa. In order to focus onto a different location you must specify its longitude and latitude in the `Center` property of the `MapView`. This is a `TRealPoint` record having the longitude and latitude in the `Lon` and `Lat` record elements.
  * Suppose we want to zoom into Manhattan. Use your favorite search engine to determine the geo coordinates of Manhattan: Longitude = -73.985130°, Latitude = 40.758896°. Alternatively you can also use the `TGeoNames` component which comes with the LazMapViewer package and provides access to a database of the geo coordinates of a huge number of locations via internet.


    
    
    procedure TForm1.FormActivate(Sender: TObject);
    var
      P: TRealPoint;
    begin
      MapView1.Active := true;
      P.Zoom := 13;     // for the magnification used in the introductory screenshot
      P.Lon := -73.985130;
      P.Lat := 40.758896;
      MapView1.Center := P;
    end;
    

  * In addition to specifying the location numerically you can also change the location by **dragging the map with the mouse** (with `mvoMouseDragging` set in the `Options`): Just press the left mouse button and slowly move the mouse in the direction where you want to see more details - the map will follow. But you should be aware that usually neighboring maps will have to be loaded from the internet, and, depending on the speed of your internet connection, this may make the entire action a bit sluggish.
  * At **designtime** , you can enter the coordinates in the `MapCenter` property of the object inspector, and when `Active` is `true` you'll get immediate feedback in the map.



### Basic Documentation

[![tmapview 200.png](https://wiki.freepascal.org/images/d/dd/tmapview_200.png)](</File:tmapview_200.png>)

#### Properties

These are the main properties of the `TMapView` component: 

  * `Active (boolean)`:



    

    Must be set to `true` before the `MapView` component can display maps.

  * `CacheLocation` (enumeration `clProfile`, `clTemp`, `clCustom`):



    

    Determines whether the directory for the tile image cache is the user profile, the temp directory or a custom directory specified by `CachePath`

  * `CacheMaxAge`:



    

    Is the maximum number of days files are kept in the cache. Default: quasi-infinite.

  * `CacheOnDisk (boolean)`:



    

    When set to `true` (default) dowloaded map tiles are stored in a directory to reduce internet traffic and for faster access. It is not recommended to turn this property off. Files are deleted from the cache after a storage period of `CacheMaxAge` days, by default: never.

  * `CachePath (string)`:



    

    Name of the directory in which downloaded map images are buffered when `CacheLocation` is set to `clCustom`.

  * `Cyclic (boolean)`:



    

    When switched to `true` the map is repeated at the date boundary to completely fill the width of the mapviewer.

  * `DownloadEngine (TMvCustomDownloadEngine)`:



    

    Determines the control which downloads the tile images from the map servers. In Windows, this is by default the `TMvDEWin` engine based on the _WinInet_ library. Linux and macOS, by default, use the `TMvDEFPC` engine which takes advantage of the `TFPHttpClient` class coming with FPC. If a different download engine is required the corresponding component can be hooked to this property. The alternative `TMvDESynapse` engine based on the _Synapse_ library can be installed as a separate component. Note that there is also a "download" engine `TMvDECache` which only loads images from the cache but does not actually access the internet - this is interesting of off-line applications. It is required that you already have pre-loaded tile images and your application covers only a small region at the highest magnification.

  * `DrawingEngine (TMvCustomDrawingEngine)`:



    

    Painting of the tiled images is a speed-critical task. By default, this is done using routines from the _IntfGraphics_ unit; the corresponding drawing engine is in the `TMvInfGraphicsDrawingEngine` class. However, it is possible to provide a different drawing engine here. The standard installation of _LazMapViewer_ comes with the alternative `TMvRGBGraphicsDrawingEngine` which is based on the _RGBGraphics_ package, and with the `TMvBGRADrawingEngine` utilizing the _BGRABitmap_ package. The external libraries required must be installed from separate packages.

  * `DrawPreviewTiles (boolean)`:



    

    When a tile currently is not available an existing tile is stretched to fit into the tile's place in the map. Otherwise, when `DrawPreviewTiles = false`, the missing tile's area is filled by the uniform background color of the `MapView`.

  * `Layers`:



    

    A collection of `TMapLayer` instances which can be drawn independently over the base map - see below.

  * `MapCenter`:



    

    A helper class needed for entering the geo coordinates of the map's center point (`MapCenter.Latitude` and `MapCenter.Longitude`) in the object inspector. When the option `mvoLatOnInDMS` is set the values can be entered in the degree-minute-second format, otherwise as standard fractional floating point numbers.

  * `MapProvider (string)`:



    

    This string identifies the provider from which the maps are downloaded. Built-in providers allow to choose between _OpenStreetMaps_ , _GoogleMaps_ , _Virtual Earth_ , _Yandex_ etc. Details are given in section Map Providers.

  * `Options`:



    

    A set with the following elements
    \- `mvoMouseDragging`: Allows to enable/disable dragging of the map by the mouse. Default: on
    \- `mvoMouseZooming`: Allows to enable/disable zooming by rotating the mouse wheel. Default: on
    \- `mvoEditorEnabled`: Allows to edit the position of points and tracks with the mouse. Default: off
    \- `mvoLatLonInDMS`: When set, geo coordinates can be displayed/entered in the degree-minute-seconds format, otherwise as standard float number of the degrees. Default: off

  * `UseThreads (boolean)`:



    

    When set to `true` downloading and drawing of maps is delegated to multiple threads in order to achieve a smoother response. It is not recommended to turn this property off.

  * `Zoom (integer)`:



    

    Magnfication of the map: 0 = coarsest magnification, earth view, 17 or 18 = highest magnification. Each zoom step results in doubling of the magnfication factor.

  * `ZoomMax (integer)` and `ZoomMin`:



    

    Specify the range in which the zoom can be varied.

  * `ZoomToCursor (boolean)`:



    

    When this is `true` zooming is relative to the mouse position, otherwise to the center of the map. This feature is interesting for zooming with the mouse wheel.

Here are properties related to overlayed GPS objects (points of interest, tracks): 

  * `GPSItems: TGPSObjectList`:



    

    List of GPS objects assigned to the MapView. See below for details.

  * `DefaultTrackColor: TColor`, `DefaultTrackWidth: Integer`:



    

    The default color and line width, respectively, of overlayed tracks. See below for details.

  * `POIImage: TBitmap`:



    

    Bitmap which will be overlayed to mark a "point of interest" (POI).

  * `POImages: TCustomImageList`:



    

    Image list from which images can be selected as markers for points-of-interest. Note that the _LazMapViewer_ installation comes with a large selection of POI icons in the folder _marker-images_.

  * `POITextBgColor: TColor`:



    

    Background color of the overlayed text describing a point of interest. The transparency of the background is determined by `POITextBgOpacity`. Turn off the text background by selecting `clNone` or `POITextBgOpacity = 0.0`.

  * `POITextBgOpacity: Single`:



    

    Opacity of the overlayed text background of a point of interest for better readability. The floating point value is expected to be between 0.0 (fully transparent) and 1.0 (fully opacque). Default: 0.55

#### Events

  * `OnAfterDrawObjects, OnBeforeDrawObjects`:



    

    Events which are executed after/before overlay objects of the map (points of interest, tracks) are drawn.

  * `OnAfterDrawTile`:



    

    Comes after a tile has been painted. Helpful for custom-drawing information about this specific tile since the event has the tile's x and y indices, as well as the zoom level as a parameter in the `TileID` record.

  * `OnAfterPaint`:



    

    Is triggered after the map has been drawn completely.

  * `OnCenterMove`:



    

    Fires whenever the `Center` of the `MapView` have been changed.

  * `OnCenterMoving`:



    

    Is executed before a request to move the map center is handled. It has parameters `NewCenter` and `Allow` which can be used to restrict panning to a given region only (useful for off-line mode).

  * `OnChange`:



    

    Fires whenever the `Center`, size or `Zoom` factor of the `MapView` change.

  * `OnDrawEditMark`:



    

    Can be used to override painting of the mark which is currently edited. By default, the edited mark is a red square with green background.

  * `OnDrawGpsPoint`:



    

    See below.

  * `OnDrawMissingTile`:



    

    Can be used primarily to custom-paint the background of missing tiles when `DrawPreviewTiles` is `false`. It is also helpful when the tiles contain (semi-) transparent areas where the background is supposed to shine through.

  * `OnEditStartDrag`, `OnEditEndDrag`, `OnEditDrag`, `OnEditSelectionCompleted`, `OnEditIsDirty`:



    

    Are mainly used for internal communication between mapview and editmark during editing.

  * `OnKeyDown`, `OnKeyPress`, `OnKeyUp`: standard keyboard events inherited from `TControl`. Useful, for example, if you want to move the map center by the keyboard.
  * `OnMouseDown`, `OnMouseUp`:



    

    Standard mouse events inherited from `TControl` fired when a mouse button is clicked.

  * `OnMouseEnter`, `OnMouseLeave`:



    

    standard mouse events inherited from `TControl` fired when the mouse is moved into/out of the map

  * `OnResize`:



    

    Becomes active after the size of the map window has changed.

  * `OnZoomChange`:



    

    Fires whenever the `Zoom` factor of the `MapView` changes.

  * `OnZoomChanging`:



    

    Is executed before a request to change to magnification is handled. It has parameters `ANewZoom` and `Allow` which can be used to restrict zooming only to specific ranges.

#### Main methods

  * `procedure GetMapProviders(lstProviders: TStrings)`:



    

    Returns in `lstProviders` the names of all registered map providers. When one of these strings is assigned to the `MapProvider` property of the `MapVieew` the servers of the corresponding provider will become the source of the displayed maps.

  * `function GetVisibleArea: TRealArea`:



    

    Returns the top/left and bottom/right corner points of the displayed rectangle in geo coordinates. `TRealArea` is a record consisting of the `TRealPoint` records `TopLeft` and `BottomRight` (`TRealPoint = record Lon, Lat: Double`).

  * `function LatLonToScreen(aPt: TRealPoint): TPoint`:



    

    Maps a geo point (latitude, longititude) to screen pixels (relative to the `TMapView` instance).

  * `function ScreenToLatLon(aPt: TPoint): TRealPoint`:



    

    Maps the coordinates of a screen pixel (relative to the `MapView` instance) to geo coordinates (latitude, longitude). Here is an example how the geo coordinates of the mouse cursor can be updated in two labels by means of the `OnMouseMove` event of the `MapView`:
    
    
    uses
      mvTypes,    // for TRealPoint
      mvEngine;   // for LonToStr() and LatToStr() funtions
    
    procedure TForm1.MapView1MouseMove(Sender: TObject; Shift: TShiftState;
      X, Y: Integer);
    var
      P: TRealPoint;
    begin
      P := MapView1.ScreenToLatLon(Point(X, Y));
      Label1.Caption := 'Longitude: ' + LonToStr(P.Lon, true);
      Label2.Caption := 'Latitude: ' + LatToStr(P.Lat, true);
    end;
    

  * `procedure SaveToFile(AClass: TRasterImageClass; const AFileName: String)`:



    

    Save the currently displayed map as bitmap of the given class to a file. Example for saving to a jpg image:
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      MapView1.SaveToFile(TJpegImage, 'mapview.jpg');
    end;
    

  * `procedure SaveToStream(AClass: TRasterImageClass; AStream: TStream)`:



    

    Similar to `SaveToFile`, but output is not in a file, but in the stream provided.

  * `function SaveToImage(AClass: TRasterImageClass): TRasterImage`:



    

    Creates an instance of the given image class and writes the currently visible view port image to it.

  * `procedure CenterOnObj(obj: TGPSObj)`:



    

    Centers the map on the provided GPS object. More on GPS objects below.

  * `function CyclicPointsOf(APoint: TPoint): TPointArray`:



    

    Calculates the positions of specified screen point which appear on the reapeted maps in "cyclic" mode.

  * `procedure ZoomOnArea(const aArea: TRealArea)`:



    

    Adjusts the `Zoom` level such that the given area is completely shown and fills the component bounds as much as possible.

  * `procedure ZoomOnObj(obj: TGPSObj)`:



    

    Adjusts the `Zoom` level such that the given GPS object is completely seen at highest magnification. More about GPS objects below.

  * `function ObjsAtScreenPt(X, Y: Integer; ATolerance: Integer = -1; AClass: TGPSObjClass = nil): TGPSObjArray`:



    

    Returns an array of all GPS objects found within the given tolerance (in pixels) at the screen point X, Y. The search can be restricted to a specified class of GPS objects (e.g. `TGPSPointOfInterest` only).

  * `function VisibleObjsAtScreenPt(X, Y: Integer; ATolerance: Integer = -1; AClass: TGPSObjClass = nil): TGPSObjArray`:



    

    Similar to `ObjsAtScreenPt`, but only visible objects are considered.

  * `function VisiblePointsAtScreenPt(X, Y: Integer; ATolerance: Integer = -1; APointTypes: TMvPointTypes = ptAll): TGPSObjArray`:



    

    Similar to `VisibleObjsAtScreenPt`, but now both GPS-type and map-type of points are found (`TMvPointType = (ptGPSPointOfInterest, ptGPSTrackPoint, ptGPSAreaPoint, ptMapPointOfInterest, ptMapTrackPoint, ptMapAreaPoint)`, see below).

  * `function VisiblePointsInArea(Area: TRealArea; APointTypes: TMvPointTypes = ptAll): TGPSObjArray`:



    

    Similar to `VisiblePointsAtScreenPt`, but now an area in geo coordinates is search for the points rather than the vicinity of a screen point.

### Map Providers

The `MapView` component can display maps from various providers. To activate a given provider its name must be specified in the `MapProvider` property of the component. 

Here is a list of the names of the built-in map providers: 

  * OpenStreetMap Mapnik ([Open Data Commons Open Database License](<https://opendatacommons.org/licenses/odbl/>), copyright <https://www.openstreetmap.org/copyright/en>; the introductory screenshot uses a map of this provider)
  * OpenStreetMap Wikipedia
  * OpenStreetMap Sputnik
  * OpenStreetMap.fr Hot
  * OpenStreetMap.fr Cycle Map
  * Open Topo Map
  * Google Maps (Terms and conditions: <https://www.google.com/intl/en_en/help/terms_maps/>)
  * Google Satellite
  * Yandex.Maps (Terms and conditions: <https://yandex.ru/legal/maps_termsofuse/index.html>)
  * Yandex.Maps Satellite
  * Virtual Earth Bing
  * Virtual Earth Aerial
  * Virtual Earth Hybrid



The following providers require an API key to access their service. The API key can be obtained by registering at the provider sites. Usually the API keys are free, however, a charge may be required for commercial usage - please see the details on the provider sites. The strings received as API keys must be stored in global variables of your application. 

  * Registration at <https://www.thunderforest.com/docs/apikeys/>, store API key in the variable `ThunderForest_ApiKey` (Terms and conditions: <https://www.thunderforest.com/terms/>) 
    * Open Cycle Map
    * OpenStreetMap Transport
  * Registration at [https://developer.here.com/?create=Freemium-Basic&keepState=true&step=account](<https://developer.here.com/?create=Freemium-Basic&keepState=true&step=account>), store the received APP ID and APP CODE in the variables `HERE_AppID` and `HERE_AppCode`, respectively. Terms and conditions: <https://legal.here.com/us-en/terms/here-end-user-terms>
    * Here WeGo Map
    * Here WeGo Grey Map
    * Here WeGo Reduced Map
    * Here WeGo Transit Map
    * Here WeGo POI Map
    * Here WeGo Pedestrian Map
    * Here WeGo DreamWorks Map
  * Registration at <https://home.openweathermap.org/users/sign_up>, store the API Key in variable `OpenWeatherMap_ApiKey`: 
    * OpenWeatherMap Clouds
    * OpenWeatherMap Precipitation
    * OpenWeatherMap Pressure
    * OpenWeatherMap Temperature
    * OpenWeatherMap Wind



Please respect the license requirements of the map providers. 

### The Drawing Engine

For flexible output, all the drawing operations are handled by a dedicated class, the "drawing engine". The basic ancestor is `TMvCustomDrawingEngine` which provides methods for the elemental drawing operations, such as drawing (scaled) bitmaps, writing text, drawing graphics primitives (line, polygon, rectangle, ellipse). This is similar to what `TCanvas` does in the LCL, but it is more general because basically any graphics library can be interfaced to the mapviewer. 
    
    
    type
      TMvCustomDrawingEngine = class(TComponent)
      public
        function GetCacheItemClass: TPictureCacheItemClass; virtual; abstract;
        procedure CreateBuffer(AWidth, AHeight: Integer); virtual; abstract;
        procedure DrawBitmap(X, Y: Integer; ABitmap: TCustomBitmap; UseAlphaChannel: Boolean); virtual; abstract;
        procedure DrawCacheItem(X, Y: Integer; AImg: TPictureCacheItem; ADrawMode: TItemDrawMode = idmDraw; AOpacity: Single = 1.0); virtual; abstract;
        procedure DrawScaledCacheItem(DestRect, SrcRect: TRect; AImg: TPictureCacheItem); virtual; abstract;
        procedure Ellipse(X1, Y1, X2, Y2: Integer); virtual; abstract;
        procedure FillPixels(X1, Y1, X2, Y2: Integer; AColor: TColor); virtual; abstract;
        procedure FillRect(X1, Y1, X2, Y2: Integer); virtual; abstract;
        function GetFont: TMvFont;
        function GetPen: TMvPen;
        procedure Line(X1, Y1, X2, Y2: Integer); virtual; abstract;
        procedure Polyline(const Points: array of TPoint); virtual; abstract;
        procedure Polygon(const Points: array of TPoint); virtual; abstract;
        procedure PolyBezier(const Points: array of TPoint; Filled: Boolean = False; Continuous: Boolean = True);  virtual; abstract;
        procedure PaintToCanvas(ACanvas: TCanvas); overload;
        procedure PaintToCanvas(ACanvas: TCanvas; Origin: TPoint); overload; virtual; abstract;
        procedure Rectangle(X1, Y1, X2, Y2: Integer); virtual; abstract;
        function SaveToImage(AClass: TRasterImageClass): TRasterImage; virtual; abstract;
        procedure SetFont(AFont: TMvFont);
        procedure SetFont(AFontName: String; AFontSize: Integer; AFontStyle: TFontStyles; AFontColor: TColor);
        procedure SetPen(APen: TMvPen);
        procedure SetPen(APenStyle: TPenStyle; APenWidth: Integer; APenColor: TColor);
        function TextExtent(const AText: String): TSize; virtual; abstract;
        function TextHeight(const AText: String): Integer;
        procedure TextOut(X, Y: Integer; const AText: String); virtual; abstract;
        function TextWidth(const AText: String): Integer;
    
        property BrushColor: TColor read GetBrushColor write SetBrushColor;
        property BrushStyle: TBrushStyle read GetBrushStyle write SetBrushStyle;
        property FontColor: TColor read GetFontColor write SetFontColor;
        property FontName: String read GetFontName write SetFontName;
        property FontSize: Integer read GetFontSize write SetFontSize;
        property FontStyle: TFontStyles read GetFontStyle write SetFontStyle;
        property PenColor: TColor read GetPenColor write SetPenColor;
        property PenStyle: TPenStyle read GetPenStyle write SetPenStyle;
        property PenWidth: Integer read GetPenWidth write SetPenWidth;
        property Opacity: Single read GetOpacity write SetOpacity;
      end;
    

Since an arbitrary graphics engine does not store images in the LCL classes (TBitmap etc) a more general `TPictureCacheItem` class was introduced which must be implemented by each drawing engine class. 

The default drawing engine is `TMvIntfImageDrawingEngine` which is based on the routines of `FCL-Image` and `TLazIntfImage`. It does not require installation of any third-party packages because everything comes with Lazarus and FPC. However, drawing may be a bit sluggish from time to time. Better performance can be achieved with the `TMvRGBGraphicsDrawingEngine` or with the `TMvBGRADrawingEngine`; they require installation of the _RGBGraphics_ or _BGRABitmap_ packages, respectively. They are external, but can be installed from the Online-Package-Manager. 

### Map Overlays

`TMapView` is able to overlay various objects on the basic map discussed so far: Points of interest, tracks, areas, even other tile maps. For historical reasons, this can be achieved at two levels of access: \- a RAD-like approach at designtime in the Object Inspector (`TMapXXX` classes) \- a more direct approach requiring some code (`TGPSXXX` classes) 

#### Basic overlay architecture

##### Classes

The public `TMapView` property `GPSItems` collects data on points of interest and tracks to be overlayed on the map. The common ancestor type of these overlay items is **`TGPSObj`** : 
    
    
    type
      TGPSObj = class
        ...
      public
        destructor Destroy; override;
        property Name: String read FName write FName;
        property ExtraData: TObject read FExtraData write SetExtraData;
        property IdOwner: Integer read FIdOwner;
        property BoundingBox: TRealArea read GetBoundingBox write SetBoundingBox;
        property Visible: Boolean read FVisible write FVisible;
        property ZOrder: Integer read FZOrder;
    
      end;
    

  * The `Name` can be displayed near the item in the map.
  * `ExtraData` can be used freely, for example to define the color of a track. An example how this could be done is found in unit `mvExtraData`, classes `TDrawingExtraData` and `TTrackExtraData`.
  * `IdOwner` is the ID of the owner TGPSObj instance to which the current item belongs.
  * `BoundingBox` gives the geo coordinates of the top/left and bottom/right corner points of the rectangle over which the object extends.
  * `Visible` allows to show/hide a `TGPSObj` instance.
  * `ZOrder` determines the order in which various `TGPSObj` instances are drawn.



A specialized `TGPSObj` class is a **`TGPSPoint`** , for example a scenic "Point-of-Interest" (POI), a way point along a hiking path etc. 
    
    
      TGPSPoint = class(TGPSObj)
      public
        constructor Create(ALon,ALat: double; AEle: double=NO_ELE; ADateTime: TDateTime=NO_DATE);
        class function CreateFrom(aPt: TRealPoint): TGPSPoint;
    
        function HasEle: boolean;
        function HasDateTime: Boolean;
        function DistanceInKmFrom(OtherPt: TGPSPoint; UseEle: boolean=true): double;
    
        property Lon: Double read GetLon;
        property Lat: Double read GetLat;
        property Ele: double read FEle;
        property DateTime: TDateTime read FDateTime;
        property RealPoint: TRealPoint read FRealPt;
      end;
    

In its constructor, the geo coordinates longitude and latitude must be specified. They are also available as read-only properties `Lon` and `Lat`, as well as a record `RealPoint`. Optional parameters are the elevation of the point (property `Ele` as well as some date/time information (property `DateTime`), for example the date and time when this point was visited. A special function, `DistanceInKmFrom`, is available to calculate the on-earth distance from another GPS point. 

**`TGPSPointOfInterest`** is a further specialization of `TGPSPoint`. It provides an `ImageIndex` property to select a marker image from the `TMapView`'s `POIImages`: 
    
    
    type
      TGPSPointOfInterest = class(TGPSPoint)
      public
        property ImageIndex: Integer read FImageIndex write FImageIndex default -1;
      end;
    

A sequence of `TGPSPoint` instances sets up a **`TGPSPolyLine`** , like a polygon. The individual vertices are collected in the list `Points`. The `TGPSPolyLine` provides the method `GetArea` to determine the enclosing rectangle `Area` (`TRealArea` is a record with the top/left and bottom/right corner points in geo coordinates). 
    
    
    type
      TGPSPolyLine = class(TGPSObj)
      private
        FPoints: TGPSPointList;
        function GetAllObjs: TGPSObjEnumerator; override;
      public
        constructor Create;
        destructor Destroy; override;
        procedure GetArea(out Area: TRealArea); override;
        property Points: TGPSPointList read FPoints;
      end;
    

The descendant class **`TGPSTrack`** inherits the `Points` property which is to be understood as the points along a path, for example points of a hike. There is also a `DateTime` property usable for example to hold the date of the hike; by default it is taken from the `DateTime` value of the first track point. The function `TrackLengthInKm` calculates the length of the entire path with the points being connected by straight line segments. There are also properties to determine the order the track is painted on the map: 
    
    
    type
      TGPSTrack = class(TGPSPolyLine)
      public
        function TrackLengthInKm(UseEle: Boolean=true): double;
        property DateTime: TDateTime read GetDateTime write FDateTime;
        property LineColor: TColor read FLineColor write FLineColor;
        property LineWidth: Double read FLineWidth write FLineWidth;
        property ConnectColor: TColor read FConnectColor write FConnectColor;
        property ConnectWidth: Double read FConnectWidth write FConnectWidth;
        property Opacity: Single read FOpacity write FOpacity;
      end;
    

There is also a **`TGPSArea`** which is a closed polygon (`TGPSLine`) and can be filled by a specified color. Note the parameter `Opacity` for semitransparent rendering so that the underlying map shines through. The user can take advantage of this feature to highlight some areas in the original map. 
    
    
    type
      TGPSArea = class(TGPSPolyLine)
      public
        property FillColor: TColor read FFillColor write FFillColor;
        property LineColor: TColor read FLineColor write FLineColor;
        property LineWidth: Double read FLineWidth write FLineWidth;
        property Opacity: Single read FOpacity write FOpacity;
      end;
    

The **`TGPSTileLayer`** , finally, which also descends from `TGPSObj`, is able to display an entire map of tiles, just like the `TMapView`. It can have its own setting for the `MapProvider` and the `UseThreads` property. The enumerated property `DrawMode` determines the way how the layer is drawn over the background: 

  * `idmDraw` \- fully overwrites the background
  * `idmUseOpacity` \- uses the `Opacity` value (a `single` between `0`=transparent) and `1`=opaque) for semi-transparent rendering
  * `idmUseSourceAlpha` \- some map providers deliver tile images with alpha channel, e.g. _OpenRailwayMap_ which only contains the tracks of railway lines on a transparent background. In this setting the overlay map is perfectly mixed with the background map.


    
    
    type
      TGPSTileLayer = class(TGPSTileLayerBase)  // TGPSTileLayerBase inherits from TGPSObj
      public
        // all the following properties are inherited from the ancestor, TGPSTileLayerBase
        property MapProvider; // String
        property DrawMode;    // TItemDrawMode = (idmDraw, idmUseOpacity, idmUseSourceAlpha)
        property Opacity;     // Single
        property UseThreads;  // boolean
    

All these GPS objects are be stored in a **`TGPSObjectList`** : 
    
    
    type
      TGPSObjectList = class(TGPSObj)
      ...
        constructor Create;
        destructor Destroy; override;
        Procedure ClearAll;
        Procedure Clear(OwnedBy: integer);
        procedure ClearExcept(OwnedBy: integer; const ExceptLst: TIdArray; out Notfound: TIdArray);
        procedure GetArea(out Area: TRealArea); override;
        function GetObjectsInArea(const Area: TRealArea): TGPSObjList;
        function GetIdsArea(const Ids: TIdArray; AIdOwner: integer): TRealArea;
    
        function Add(aItem: TGpsObj; AIdOwner: integer; AZOrder: integer = 0): integer;
        function ChangeZOrder(AItem: TGPSObj; ANewZOrder: Integer): Integer;
        procedure Delete(AObj: TGPSObj);
        procedure DeleteById(const Ids: Array of integer);
        function  FindTrackById(const ID: Integer): TGPSTrack;
    
        procedure BeginUpdate;
        procedure EndUpdate;
    
        property Count: integer read GetCount;
        property Items[AIndex: Integer]: TGpsObj read GetItem; default;
    
        property OnModified: TModifiedEvent read FOnModified write FOnModified;
      end;
    

Objects can be added to the list by calling the `Add` method. Since each addition is accompanied by a repaint of the `MapView` you should call `BeginUpdate` before and `EndUpdate` after adding the objects. Every added object must be accompanied by an arbitrary numeric ID which is used to group similar objects. 

Method `DeleteByID` deletes all objects sharing the same ID. `Clear`, on the other hand, clears all the objects which are owned by object having the specified ID. An extension is `ClearExcept` which does the same but skips IDs listed in the `ExceptLst` array. Clearing a list with many objects, may take a painful long time, caused by the internal notification handling. The methode `ClearAll` will shorten this time to a minimum, by the price of lacking the notification, which may, if used, cause exception on accessing deleted items. So if using this method, make sure all other instances (e.g. marker-related plugins) which access the list are treated subsequently. 

`GetObjectsInArea` creates a TFPObjectlist (of type `TGpsObjList` here) with all the objects enclosed by the given rectangle (`Area`). `GetIDsArea`, conversely, returns the rectangle enclosing all the objects with the IDs listed in parameter `IDs` (which is a simple array here). 

The `TMapView` component provides 10 pre-allocated `TGPSObjectList` instances which are accessible via the array property `GPSLayer[L: Integer]`. They serve like a stack of layers drawn the in order of their Z value (see `TGPSObj`). For historic reasons, the center layer `GPSLayer[5]` is also made available as property `GPSItems`. 

##### Examples

###### Adding a point-of-interest

Suppose you want to mark a special point in the map by a mouse click and identify it with some descriptive text. Write a handler for the OnMouseUp event in which you query the description and then take the pixel coordinates of the clicked point, convert it to geo coordinates and create a `TGpsPoint` from it which you add to the `GpsItems` of the `MapView`: 
    
    
    uses
      mvTypes, mvGPSObj;
    
    const
      _POI_ = 10;
    
    procedure TForm1.MapView1MouseUp(Sender: TObject; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer);
    var
      pt: TRealPoint;
      gpsPt: TGpsPoint;
      ptName: String;
    begin
      ptName := '';
      if InputQuery('Point of Interest', 'Enter name:', ptName) then
      begin
        pt := MapView1.ScreenToLatLon(Point(Y, X));
        gpsPt := TGpsPoint.CreateFrom(pt, NO_ELE, Now() );
        gpsPt.Name := ptName;
        Mapview1.GPSItems.Add(gpsPt, _POI_);
      end;
    end;
    

In this example the constant `_POI_` is used as ID of the added point. By default, the point is drawn as a red cross, the name is drawn underneath it. You can highlight the text by changing its background color in property `POITextBgColor` of the `MapView`; for semi-transparent text background adjust the `POITextBgOpacity` property. You can also replace the default red cross by an arbitrary bitmap which must be loaded into the `POIImage` property (the folder _marker-images_ of the LazMapViewer installation contains a larger number of ready-to-use pin-cushion-like icons). And when the point is created as an instance of `TGPSPointOfInterest` rather than `TGPSPoint` you can select for it an image from the `MapView.POIImages` image list. 

There is also an event, `OnDrawGpsPoint`, which can be used to replace the entire drawing process by custom routines. The event has the `GpsPoint` as parameter. The other event parameter is the `DrawingEngine` which provides methods for drawing as explained above. Here is an example in which at first the screen coordinates of the point are calculated; and then a blue circle is drawn at this position as a marker and labeled by a large bold font: 
    
    
    procedure TForm1.MapView1DrawGpsPoint(Sender: TObject;
      ADrawer: TMvCustomDrawingEngine; APoint: TGpsPoint);
    const
      R = 8;   // Circle radius, in pixels
    var
      p: TPoint;
    begin
      // Screen coordinates of the GPS point
      p := MapView1.LatLonToScreen(APoint.RealPoint);
    
      // Draw a blue circle
      ADrawer.PenColor := clBlack;
      ADrawer.BrushColor := clBlue;
      ADrawer.BrushStyle := bsSolid;
      ADrawer.Ellipse(p.X-R, p.Y-R, P.X+R, p.Y+R);
    
      // Draw the point label
      ADrawer.BrushStyle := bsClear;
      ADrawer.FontName := 'SegoeUI';
      ADrawer.FontSize := 16;
      ADrawer.FontStyle := [fsBold];
      ADrawer.TextOut(
        p.X - ADrawer.TextWidth(APoint.Name) div 2,
        p.Y + R + 4,
        APoint.Name
      );
    end;
    

###### Overlaying another tile layer

The following code displays central Europe and overlays the railway lines from the _OpenRailwayMap Standard_ provider. The usual steps are: 

  * Create a instance of `TGPSTileLayer`.
  * Set its `MapProvider` and `DrawMode` as needed; the `DrawMode` `idmUseSourceAlpha` respects the alpha channel of the layer tile images (if available) and makes the bottom layer shine through.
  * Add it to one of the `MapView`'s `GPSLayer`s. Provide an appropriate ID to identify the layer.


    
    
    uses
      mvGPSObj, mvDrawingEngine;
    
    const
      _TILELAYERS_ID_ = 42;
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      layer: TGPSTileLayer;
    begin
      MapView1.MapProvider := 'OpenStreetMap Standard';
      MapView1.Zoom := 5;
      MapView1.MapCenter.Longitude := 10;
      MapView1.MapCenter.Latitude := 49;
      layer := TGPSTilelayer.Create;
      layer.MapProvider := 'OpenRailwayMap Standard';
      layer.DrawMode := idmUseSourceAlpha;
      MapView1.GPSLayer[0].Add(layer, _TILELAYERS_ID_);
      MapView1.Active := true; 
    end;
    

###### Creating a custom track

The following code creates a track and defines its points by adding instances of `TGPSPoint` to it. Then the track is added to the `GPSItems` of the `MapView`. For identification it is given a unique ID value (`TRACK_ID`). Finally the smallest area enclosing all track points is calculated so that the `MapView` can zoom to it: 
    
    
    uses
      ..., mvMapViewer, mvGpsObj, mvTypes;
    const
      TRACK_ID = 293;   // unique number
    
    procedure CreateGPSTrack;
    var
      track: TGPSTrack;
      area: TRealArea;
    begin
      track := TGPSTrack.Create;
      track.Points.Add(TGPSPoint.Create(-16.123,  65.491));  // Latitude, Longitude pairs
      track.Points.Add(TGPSPoint.Create(-18.34,   64.374));
      track.Points.Add(TGPSPoint.Create(-19.979,  63.895));
    
      MapView.GPSItems.Add(track, TRACK_ID);
      track.GetArea(area);
      MapView.ZoomOnArea(area);
    end;
    

###### Reading and displaying a track from a gpx file

Suppose you recorded your nice mountain hike by means of a gps tracker, and now you want to use the MapView to display the path in a map. In unit _mvGPX_ , you find a reader class for gpx files which directly loads the track into the `GPSItems` of the `MapView` component: 
    
    
    uses
      ..., mvMapViewer, mvTypes, mvGPX;
    
    procedure TMainForm.BtnLoadGPXFileClick(Sender: TObject);
    var
      reader: TGpxReader;
      b: TRealArea;
    begin
      if OpenDialog.Execute then begin
        reader := TGpxReader.Create;
        try
          reader.LoadFromFile(OpenDialog.FileName, MapView.GPSItems, b);
          MapView.Engine.ZoomOnArea(b);
        finally
          reader.Free;
        end;
      end;
    end;
    

#### Creating overlays at designtime

##### Classes

In order to facilitate using overlay objects several collections were added to the `TMapView` so that objects can be added, edited and deleted at designtime. For this reason, the basic list elements descend from `TCollection` and the elements themselves descend from `TCollectionItem`. The most fundamental collection here is `TMapCollectionBase` and its descendant `TMapCollection`. The most fundamental collection item classes are `TMapItem` and the derived `TMapPoint` (corresponding to `TGPSPoint`) with these most important properties: 
    
    
    type
      TMapItem = class(TCollectionItem)
      ...
      public
        property View: TMapView read GetView;
        property Layer: TMapLayer read GetLayer;
        property GPSObj: TGPSObj read GetGPSObj;
      published
        property Caption: TCaption read FCaption write SetCaption;
        property Visible: Boolean read FVisible write SetVisible default True;
      end; 
    
      TMapPoint = class(TMapItem)
      ...
      public
        property LatLonInDMS: Boolean read GetLatLonInDMS;
        property RealPoint: TRealPoint read GetRealPoint write SetRealPoint;
        property ToScreen: TPoint read GetToScreen;
      published
        property Longitude: Double read FLongitude write SetLongitude;
        property Latitude: Double read FLatitude write SetLatitude;
        property Elevation: Double read FElevation write SetElevation stored IsElevationStored;
        property DateTime: TDateTime read FDateTime write SetDateTime stored IsDateTimeStored;  
      end;
    

More specialized classes are `TMapPointOfInterest`, `TMapTrackPoint` and `TMapAreaPoint`

  * `TMapPointOfInterest` is equivalent to `TGPSPointOfInterest` and introduces the `ImageIndex` property.
  * `TMapTrackPoint` publishes a new property `Mark` which determines whether this point is at the begin, end, somewhere in the middle, or between track segments (`smStart`, `smEnd`, `smMid`, `smNone`, respectively).
  * `TMapAreaPoint` is a vertex of a polygon (`TMapArea`); it has no additional properties.



Another imporant descendant of `TMapItem` is `TMapLayer`. Similarly to `TGPSTileLayer` it refers to each one of the layer stacks in the map. Like `TGPSTileLayer` it can contain a complete tile map (based on its `MapProvider` setting). But it can also consist of points-of-interest, tracks or areas (or a combination of them). The points-of-interest are administrated by the `PointsOfInterest` collection (type `TMapPointsOfInterest`), the tracks in the `Tracks` collection (type TMapTracks), and the areas in the `Areas` collection (type `TMapAreas`). The link between the `TMapLayer` and the GPS-type of objects is established by the class `TGPSComboLayer` which consists of a `TGPSTileLayer` for its background and a collection of other objects to draw on top. Actually, the `Draw` method is overridden to draw first the background and then - the rest of the objects contained into. 

The layers themselves are collected in a `TMapLayers` collection which descends from `TMapCollection`. The instance `Layers` is a direct member of `TMapView`. 
    
    
    type
      TMapLayer = class(TMapItem)
      ...
      public
        function HitTest(constref Area: TRealArea): TMapObjectList; override;
        function AddPointOfInterest(APoint: TRealPoint; ACaption: String = ''): TMapPointOfInterest;
        procedure AssignFromGPSList(AList: TGPSObjectList);
        property ComboLayer: TGPSComboLayer read FComboLayer;
      published
        property MapProvider: String read GetMapProvider write SetMapProvider;
        property UseThreads: Boolean read GetUseThreads write SetUseThreads default True;
        property DrawMode: TItemDrawMode read FDrawMode write SetDrawMode default idmUseOpacity;
        property Opacity: Single read FOpacity write SetOpacity default 0.25;
        property PointsOfInterest: TMapPointsOfInterest read GetPointsOfInterest write SetPointsOfInterest;
        property Areas: TMapAreas read GetAreas write SetAreas;
        property Tracks: TMapTracks read GetTracks write SetTracks;
      end;
    

##### Examples

###### Overlaying a second map as a layer

  * Click on the '...' button next to the `Layers` property (or double-click on the map, or right-click on the map and select "Layer Editor...") to open the layer editor.
  * Click on '+ Add' to create a new layer and to add it.
  * Specify the map provider in the `MapProvider` property.
  * Moreover, select the `DrawMode`: If the images received from the map server have their own transparency in an alpha channel the option `idmUseSourceAlpha` is the correct one. Otherwise, maybe you want the original map base layer to shine through the overlay layer - select `idmUseOpacity` and adjust the `Opacity` property as needed.



If you want to overlay a TMapLayer by code you can do it this way: 
    
    
    function AddMapLayer(AMapView: TMapView): TMapLayer;
    begin
      Result := AMapView.Layers.Add as TMapLayer;
    end;
    

###### Overlaying a layer with points-of-interest

  * Open the layer editor and add a new layer. Or, if you want to add the points to an existing layer, select this layer in the layer editor.
  * Click on the '...' button next to the `PointsOfInterest` item among the layer's properties in the object inspector.
  * This opens the points editor where you can enter the point properties, in particular the `Latitude` and `Longitude`. `Caption` specifies the text to be displayed below the point symbol. If an imagelist is attached to the `MapView`'s `POIImages` property you can also pick one of these icons by means of the `ItemIndex` property.
  * Repeat with all other points that you want to add.



The following code shows how a point-of-interest can be added to an existing MapLayer at runtime: 
    
    
    function AddMapPointOfInterest(AMapView: TMapView; ALayerIndex: Integer; ALatitude, ALongitude: Double; ACaption: String; AImageIndex: Integer): TMapPointOfInterest;
    var
      layer: TMapLayer;
    begin
      layer := AMapView.Layers[ALayerIndex];
      Result := layer.PointsOfInterest.Add as TMapPointOfInterest;  // or := TMapPointOfInterest.Create(layer.PointsOfInterest);
      Result.RealPoint := RealPoint(ALatitude, ALongitude);
      Result.Caption := ACaption;
      Result.ImageIndex := AImageIndex;
    end;
    

In an analogous way you can also define the points of a track or of an area. 

###### Reading and displaying a track from a gpx file

  * Again open the layer editor
  * Select the layer to which you want to add the track, or add a new layer.
  * In the toolbar of the layer editor, there is a "Load GPX" button to select the gpx file to be loaded. There is a dropdown menu with two options: 
    * Load... -- this simply loads the gpx file, the viewport does not change.
    * Load and zoom ... -- does the same, but also adjusts the zoom and center of the viewport to fully see the loaded gpx objects.



If you want to load a gpx file into a MapLayer by code you can do it this way: 
    
    
    uses
      mvMapViewer, mvTypes, mvGPSObj, mvGPX;
    
    procedure TMainForm.LoadGPXFileInMapLayer(AFileName: string; AMapView: TMapView; ALayerIndex: Integer);
    var
      reader: TGpxReader;
      b: TRealArea;
    begin
      list := TGPSObjectList.Create;
      try
        reader := TGpxReader.Create;
        try
          reader.LoadFromFile(AFileName, list, b);
          AMapView.Engine.ZoomOnArea(b);
          AMapView.MapCenter.Longitude := (b.TopLeft.Lon - b.BottomRight.Lon) / 2;
          AMapView.MapCenter.Latitude := (b.TopLeft.Lat - b.BottomRight.Lat) / 2;
          AMapView.Layers[ALayerIndex].AssignFromGPSList(list);
        finally
          reader.Free;
        end;
      finally
        list.Free;
      end;
    end;
    

###### Editing points by using the built-in points editor

A points editor has been incorporated into the mapviewer to give the user the opportunity to edit map points at design-time. In order to activate the editor add the element `mvoEditorEnabled` to the `MapView.Options`. When now the mouse is moved over map points (points of interest, area points, track points) the point is highlighted by a green square with red border which can be dragged to the new location. The points editor is available also at run-time (see _points_of_interest_demo_ in the _examples_ folder of the MapViewer installation). 

Additionally, the window of the points editor can be opened by selecting the option "Edit MapView Points" from the context menu of the `MapView` component at design-time. At run-time, you must invoke the following code: 
    
    
    uses
      mvMapViewerPathEditForm;
    
    procedure TMainForm.FormCreate(Sender: TObject);
    begin
      with TMapViewerPathEditForm.Create(Self) do
      begin
        MapView := Self.MapView;
        Show;
      end;
    end;
    

[![mapviewer pointseditor.png](https://wiki.freepascal.org/images/b/b1/mapviewer_pointseditor.png)](</File:mapviewer_pointseditor.png>)

  * When the editor opens, it is in **Select/drag mode** (the first toolbar button displaying the arrow icon is pressed): 
    * You can **select** an individual point (from a track, area or POIs) by clicking on it. Click and pull a rubber-band to select multiple points. Ctrl+Click adds to the selection.
    * You can click and **drag** selected points with the mouse (Ctrl+Click to drag multiple selected points).
    * When you have points on top of each other then you can use Alt+Click to select the next one.
  * For adding new points you have several options: 
    * Selecting the "orange marker" icon (2nd toolbutton from the left) brings you into **POI mode**. Now a click into the map adds a new point-of-interest. The caption of the POI can be specified in the "Caption" box the points of the editor. If you need exact locations of the point you can enter its GPS coordinates numerically.
    * The third icon ("curve with end-points") enters **Track mode** for adding a new track of points. Each subsequent click into the map adds a point to the track. Add the last point of the track by holding the CTRL key down during clicking. Or click on the "arrow" icon (1st toolbutton) to return to selection mode. At least two points are required for a track.
    * In the same way, the corner points of an area can be specified after selecting the fourth button with the "area" symbol (**Area mode**). Note that at least three points are required to define an area.
  * The next two icons ("curve icon" with "+"/"x" overlay) are for editing existing points: In order to **add a point** to a track or area you must select two points of the shape and then click on the "curve +" icon. In order to **delete a point** you first select the point to be deleted and then click on the "curve x" symbol (in case of multiple selections, only the last selected point is deleted).
  * The final two buttons with the magnifier icons are **zoom** buttons.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This kind of editing is possible only for points descending from `TMapPoint`, but not for points descending from `TGPSPoint`.

## Plugins

Plugins are an easy way to extend the functionality of the MapViewer, without the need to change the source of the component itself. Together with the PluginManager which is the glue between the MapView and the plugins, and the RAD-method of connecting existing parts within the IDE, the system becomes a powerful and user friendly tool. For a more detailed explanation see the [LazMapViewer-Plugins](<LazMapViewer-Plugins.md> "LazMapViewer-Plugins") page. 

### What is a plugin and what can I do with it?

The question could be answered by describing an example. 

Let's assume that your application needs the capability to measure the linear distance between two points on the map. The user will click on the map with the mouse to define the anchor-point and then hover the mouse around to see the distance between the anchor and the place indicated by the mouse cursor. If the user clicks again, the measurement mode is left. 

Without the use of a plugin you would need to implement three event methods for the MapView: `OnMouseUp`, `OnMouseMove` and `OnAfterPaint`. 

  * In the `OnMouseUp`-event, you would toggle the measurement mode and if entered you store the current position as the anchor-position.
  * In the `OnMouseMove`-event, you have to store the actual position and calculate distance. And you will invalidate the MapView to initiate a redraw of the map.
  * In the `OnAfterPaint`-event, you would paint two circles as markers, one on the anchor-position and one on the current position and draw a text with the distance.



In total you have four additional form fields and three additional event methods in your form. For this simple example this may be a simple and suitable way, but if you think about other extensions, you will bloat the form's source file with code which could be better encapsulated somewhere else. And if another project needs the same functionality you will have to write the code again. 

The plugin approach provides handlers for these three events in a re-usable way. There is no need to write any code to get the wanted behaviour. 

### Using existing plugins

[![tmvpluginmanager 200.png](https://wiki.freepascal.org/images/c/c4/tmvpluginmanager_200.png)](</File:tmvpluginmanager_200.png>)

The key component for the MapViewer's plugin technology is the `TMvPluginManager`. It contains a list of all registered plugins from which the user can select the needed one at design-time. 

#### Creating plugins at design-time

Follow these steps to activate plugins in your map application: 

  * Drop a `TMvPluginManager` component on the form.
  * Link it to the MapView requiring a plugin. This is done in the Object Inspector by assigning it to the `PluginManager` property of the MapView.
  * Double-click on the PluginManager component (or click on the '...' next to the `PluginManger` property) to open the plugin editor.
  * Click on the '+ Add' button to see the list of the available plugins.
  * Select the plugin needed.
  * It is possible to have multiple plugins in the PluginManager. They are processed from top to bottom. You may adjust their order by means of the arrow buttons.
  * Every plugin type has its own set of properties. They can be altered in the object inspector after selecting the plugin in the plugin editor's list (or in the object tree of the object inspector).



The same PluginManager can be shared between several maps. All plugins and their properties are applied to each of these maps in the same way. While this is a useful feature for some plugin types (e.g. the `TMapGridPlugin`, `TLinkedMapsPlugin` or `TCenterMarkerPlugin`, see below), it can result in undesired behaviour in other cases. To avoid this, it may be required to duplicate plugins in the PluginManager for each map, set properties accordingly and specify in the plugin's `MapView` property the map on which it will be active (with each own set of properties). But there are also plugin types which can handle multiple maps automatically (`TDraggableMarkerPlugin, TSpreadMarkerPlugin`, see below). 

#### Creating plugins at run-time

Of course plugins can be used also at run-time: 

  * Since plugins work only in compbination with the plugin manager you must create a `TMvPluginManager` instance first - either use the one dropped from the component palette, or create a new one by code (see below),
  * Create the plugin by calling its constructor and add it to the plugin manager. There are several ways to do this: 
    * Specify the plugin manager as Owner in the plugin constructor.
    * Or, if you prefer to specify another owner, call `PluginManager.AddPlugin(plugin)`, or `PluginManager.Plugins.Add(plugin)` after creation



The following code creates a `TLegalNoticePlugin` (see below). 
    
    
    uses
      mvPluginCommon;
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      PluginManager: TMvPluginManager;
      plugin: TLegalNoticePlugin;
    begin
      // Create the plugin manager
      PluginManager := TMvPluginManager.Create(Self);
    
      // Link the plugin manager to the MapView which is assumed to exist already.
      MapView.PluginManager := PluginManager;
    
      // Create the plugin and add it to the plugin manager
      plugin := TLegalNoticePlugin.Create(PluginManager);
    
      // Set plugin properties
      plugin.LegalNotice := 'Map data from [https://www.openstreetmap.org/copyright OpenStreetMap and contributors]';
    end;
    

### Available plugins

Here is a list of the built-in plugins: 

  * `**TCenterMarkerPlugin**`



    

    It simply draws a cross in the center of the map. The size and color of the cross can be controlled by properties.

  * `**TTileInfoPlugin**`



    

    Draws a border around the picture tiles from which the total map is composed. The zoom level as well as the x/y indices are indicated for each tile. The displayed info text can be modified by the `InfoMask` property which is a mask string for the `Format()` function accepting the `x`/`y`/`zoom` parameters as argument indices 0..2, respectively. Example: `InfoMark := 'x=%0:d, y=%1:d, zoom=%2:d'`

  * `**TLegalNoticePlugin**`



    

    Draws a copyright text in a corner of the map. The text can be specified in the property `LegalNotice`. Using square brackets the clickable URL of the map provider's copyright page can be inserted by means of a wiki-like syntax: 

    `Map data from [https://www.openstreetmap.org/copyright OpenStreetMap and contributors]`
    displays the text _"Map data from OpenStreetMap and contributors"_. The text in brackets consists of two parts separated by the first space character: The first part (_"https://www.openstreetmap.org/copyright"_) denotes the URL of the link, and the second part (_"OpenStreetMap and contributors"_) is shown instead of the URL for better readability and formatted like a html link (property `URLFontColor`). When the user clicks on this text, the URL is opened in the web browser of the system.

  * `**TLinkedMapsPlugin**`



    

    When a form contains two (or more) maps and the user changes the viewport by zooming or panning in one map the same changes are applied to the other maps sharing this plugin.

  * `**TMarkerHintPlugin**`



    

    Displays a hint popup window when the mouse pointer hovers above a point-of-interest. Both GPS type and Map-type points are supported. By default, the hint text is composed of the POI's caption and geo-coordiates, but there is also an `OnHint` event in which any text can be prepared.

  * `**TMarkerClickPlugin**`



    

    Fires an `OnMarkerClick` event when the user click on a point-of-interest of both GPS type and Map-type.

  * `**TMarkerEditorPlugin**`



    

    The user can add new points-of-interest to the map. Existing POIs can be selected by several methods and dragged to different locations. Again both GPS type and Map-type points are supported.

  * `**TDraggableMarkerPlugin**`



    

    Allows the user to drag points of the GPS type to another location. The mouse button to be used for dragging is specified in the `DragMouseButton` property, and the accuracy required to hit the point by clicking is determined by the `Tolerance` property. The main difference to the `TMarkerEditorPlugin` is that it can handle multiple maps without further measures.

  * `**TSpreadMarkerPlugin**`



    

    This is a very complex plugin which is useful when several GPS points are on the same location. When the mouse is moved over this stack of points and remains there for longer than `MarkerSpreadDelayMS` milliseconds the individual points are "exploded" so that every point is visible and, for example, can be dragged by a `TDraggableMarkerPlugin` if present. After `MarkerCollapseDelayMS` milliseconds, the point cloud collapses to a single point again.

  * `**TMapGridPlugin**`



    

    Draws a grid of constant longitude/latitude lines over the map

  * `**TMapScalePlugin**`



    

    Draws a horizontal scale bar over the map. The units of the scale bar labels are kilometers/meters, or, when `Imperial` is true, miles/feet. The position of the bar depends of the elements in the `AlignSet` (`alTop, alBottom, alLeft, alRight`). The maximum length of the bar (in pixels) is given by `WidthMax`. Note that due to the projection of the earth onto a plane the length scale may be different across the latitudes of viewport; the length of the displayed bar always refers to the center of the map. The property `ZoomMin` is used to turn off the scale bar for smaller zoom levels so that confusion due to a noticeable difference in scaling over the viewport latitudes can be avoided.

  * `**TAreaSelectionPlugin**`



    

    Allows the user to select an area (rectangular in terms of latitude and longitude) on the map.

  * `**TGreatCirclePainterPlugin**`



    

    Displays a great circle and/or the orthodrome (=shortest line) between by two points on the map.

  * `**TUserDefinedPlugin**`



    

    This plugin does not have a predefined behaviour, but provides all events handled for specific code.

The folder _examples/plugin_demos_ contains sample projects for these built-in plugins, as well as additional plugin samples. 

### Creating a new plugin class

All plugins must be derived from the `TMvCustomPlugin` class (`unit mvPluginCore`) or from one of it's descendands. 

These classes could be added to an instance of a `TMvPluginManager`. 

In the derived class you override all needed methods. According to the example above this would be 
    
    
    TMyFirstPlugin = class(TMvPlugin)
    ...
    protected
    procedure AfterPaint(AMapView: TMapView; var Handled: Boolean); override;
    procedure MouseMove(AMapView: TMapView; AShift: TShiftState; X,Y: Integer;
          var Handled: Boolean); override;
    procedure MouseUp(AMapView: TMapView; Button: TMouseButton; Shift: TShiftState;
          X,Y: Integer; var Handled: Boolean); override;
    

As you see, the methods are very similar to the events in the example. The parameter `Sender` is replaced by `AMapView` and additionally you will find a boolean var parameter `Handled`. This parameter could be used to control the interaction between plugins, since the system allows more than one plugin to be attached. For example, if our plugin has found a marker (either in `MouseMove` or `MouseDown`) and change some state(s) it may set the `Handled`-parameter to True and signalize the following plugins that the event is already processed. And also the plugin itself should check the parameter to keep hands of actions consumed by previous plugins. 

In the Form with the `MapView` you have to attach a `PluginManager` to the `MapView`. 

  * The `TMvPluginManager` is found in the component gallery (tab "Misc"), drop him to the form.
  * In the ObjectEditor of the `MapView` select the PluginManager in the corresponding field
  * Create a `FormCreate`-method of the form and create an instance of your plugin and add it to the plugin manager.


    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      FMyFirstPlugin := TMyFirstPlugin.Create(Self);
      FMyFirstPlugin.MapView := MapView1;
      MvPluginManager1.PluginList.Add(FMyFirstPlugin);
    end;
    

As soon as the mouse is interacting with the MapView, the overridden methods are called. 

You can include your own plugins into the IDE by using the 
    
    
      RegisterPluginClass(FMyFirstPlugin, 'My fiorst plugin');
    

method. You have to create your own package and install it. (The details will be added a a later time). 

## Finding Geo Locations: TMvGeoNames

[![tmvgeonames 200.png](https://wiki.freepascal.org/images/c/c5/tmvgeonames_200.png)](</File:tmvgeonames_200.png>)

`TMvGeoNames` is a component which searches the geo coordinates (longitude, latitude) of many cities or other locations in the world. Using the download engine of the `MapView` component it sends a query to the site <http://geonames.org/search.html> and analyzes the returned html string. 

### Getting Started

  * Assuming that you already have set up an application for the `TMapView` component as described above, you simply drop a `TMvGeoNames` component on the form; it is located next to the `TMapView` icon in the _Misc_ component palette.
  * Add a button which is supposed to trigger the search.
  * Write an `OnClick` handler for the button which calls the `TMvGeoNames` method `Search`. Enter the search destination as first parameter, and the download engine to be used as second parameter. The function returns the found longitude and latitude as a `TRealPoint` record which can be passed immediately to the `Center` property of the `MapView` component. This way, the viewer can immediately jump to the found location:


    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      MapView1.Center := MvGeoNames1.Search('Grand Canyon National Park', MapView1.DownloadEngine);
    end;
    

### Documentation

#### Methods

  * `function Search(ALocationName: String; ADownloadEngine: TMvCustomDownloadEngine): TRealPoint`:



    

    Searches the requested location using the specified download engine. Return value is a `TRealPoint` with the geo coordinates of the location. The search returns also locations with similar names, however, they must be extracted by handling the `OnNameFound` event.

#### Properties

  * `LocationName: string`:



    

    Is the name of the (first) found location as listed by the _geonames_ site.

#### Events

  * `OnNameFound`:



    

    This event is triggered while the received response of the _geonames_ server is being analyzed. The html string usually contains a variety of locations. Whenver a location has been extracted from the received string the event is fired and tells the name, a description, and the longitude/latitude `TRealPoint` record for this location. Handling this event makes it possible to list all the found locations for example in a combobox or listbox to give the user the opportunity to select any one of them:
    
    
    type
      TGeoLocation = class
        Name: String;
        Descr: String;
        Coords: TRealPoint;
      end;
    
    procedure TForm1.MVGeoNames1NameFound(const AName: string;
      const ADescr: String; const ALoc: TRealPoint);
    var
      loc: TGeoLocation;
    begin
      loc := TGeoLocation.Create;
      loc.Name := AName;
      loc.Descr := ADescr;
      loc.Coords := ALoc;
      ComboBox1.Items.AddObject(AName, loc);
    end;

---

_Source: [https://wiki.freepascal.org/LazMapViewer](https://web.archive.org/web/20250430003424/https://wiki.freepascal.org/LazMapViewer)_
