# LazMapViewer-Plugins

## Contents

  * 1 About
  * 2 TMvPluginManager
  * 3 TMvCustomPlugin
  * 4 Example of a Plugin



## About

Plugins are an easy way to extent the functionality of the MapViewer, without the need to change the source of the component itself. Together with the PluginManager, who is the glue between the MapView and the plugins, and the RAD-method of connecting existing parts within the IDE, the system becomes a powerful and user friendly tool. For a simplified explanation see [LazMapViewer#Plugins](<LazMapViewer.md> "LazMapViewer"). 

## TMvPluginManager

To use Plugins within a MapView, you have to create an instance of a `TMvPluginManager` and attach the MapView instance to the PluginManager. The easiest way to do so, is to drop the PluginManager within the IDE from the components-bar on the Form where your MapView resides. 

In the MapView properties you simply select the new PluginManager in the corresponding field. 

Caution: Without assigning the PluginManager to the corresponding field of a MapView, the plugins are not called for this MapView! 

The PluginManager does not need too much attention. You simply add the Plugins and go. 

If you want to use one of the predefined and installed Plugins, you can double click to the PluginManager on the form and add, delete or reorder the plugins. For other plugins you have to create the instances during runtime and add them manually. 

The PluginManager is able to serve more than one MapView. This allows to use the same Plugins on more than one MapView. While some of the Plugins support this kind of operation others does not. Having well configured Plugins, the PluginManager can deal with such situations. 
    
    
      TMvPluginManager = class(TMvCustomPluginManager)
        ...
        property Item[AIndex: Integer]: TMvCustomPlugin read GetItem; default;
        property MapList: TFPList read FMapList;
      published
        property PluginList: TMvPluginList read FPluginList;
      end;
    

  * The indexed property `Item` allow the access to each Plugin stored in the PluginManager. Due to the wide range of derived plugins, it is a good idea, to access the plugins properties by a local reference instead via this property.
  * The property `MapList` allows the access to all MapViews who are attached to this PluginManager. The list is automatically updated when the field PluginManager of the MapView is altered.
  * The property `PluginList` allows a different access to the Plugins of this PluginManager.



## TMvCustomPlugin

The class `TMvCustomPlugin` is the root class of all plugins. The major definitions are: 
    
    
      TMvIndexedComponent = class(TComponent)
      ...
      public
      ...
        property Index: Integer read GetIndex write SetIndex;
      end;
    
      TMvCustomPlugin = class(TMvIndexedComponent)
      ...
      protected
        procedure AfterDrawObjects(AMapView: TMapView; var Handled: Boolean); virtual;
        procedure AfterPaint(AMapView: TMapView; var Handled: Boolean); virtual;
        procedure BeforeDrawObjects(AMapView: TMapView; var Handled: Boolean); virtual;
        procedure CenterMove(AMapView: TMapView; var Handled: Boolean); virtual;
        procedure CenterMoving(AMapView: TMapView; var NewCenter: TRealPoint;
          var Allow, Handled: Boolean); virtual;
        procedure DrawGPSPoint(AMapView: TMapView; ADrawingEngine: TMvCustomDrawingEngine;
          APoint: TGPSPoint; var Handled: Boolean); virtual;
        procedure DrawMissingTile(AMapView: TMapView; ADrawingEngine: TMvCustomDrawingEngine;
          ATileID: TTileID; ARect: TRect; var Handled: Boolean); virtual;
        procedure GPSItemsModified(AMapView: TMapView; ModifiedList: TGPSObjectList;
          ActualObjs: TGPSObjList; Adding: Boolean; var Handled: Boolean); virtual;
        procedure MouseDown(AMapView: TMapView; Button: TMouseButton; Shift: TShiftState;
          X, Y: Integer; var Handled: Boolean); virtual;
        procedure MouseEnter(AMapView: TMapView; var Handled: Boolean); virtual;
        procedure MouseLeave(AMapView: TMapView; var Handled: Boolean); virtual;
        procedure MouseMove(AMapView: TMapView; AShift: TShiftState; X,Y: Integer;
          var Handled: Boolean); virtual;
        procedure MouseUp(AMapView: TMapView; Button: TMouseButton; Shift: TShiftState;
          X,Y: Integer; var Handled: Boolean); virtual;
        procedure MouseWheel(AMapView: TMapView; AShift: TShiftState;
          AWheelDelta: Integer; AMousePos: TPoint; var Handled: Boolean); virtual;
        procedure ZoomChange(AMapView: TMapView; var Handled: Boolean); virtual;
        procedure ZoomChanging(AMapView: TMapView; NewZoom: Integer; var Allow, Handled: Boolean); virtual;
      protected
        property MapView: TMapView read FMapView write SetMapView;
        property Enabled: Boolean read FEnabled write SetEnabled default true;
      public
        property PluginManager: TMvPluginManager read FPluginManager write SetPluginManager;
      ...
      end;
    

  * The property `PluginManager` links the Plugin to a PluginManager and must be set to allow the operation. To reorder the plugin within the PluginManager simply use the `Index` property of the derived class. Plugins will be called in order of their index.
  * The property `MapView` limit the operation of this plugin to the assigned MapView. If not assigned, the PluginManager will call this plugin for all attached MapViews. While some Plugins can work on multiple MapViews, others can't. See in the documentation for the actual plugin, whether it is mandatory to assign a MapView or not.
  * The property `Enabled` allows the operation. Set to false will temporary disable the plugin.



To create an actual plugin, some of the predefined protected and virtual methods have to be overwritten an filled with the code. The following section will describe what to expect within the different methods. 

All Methods have two common parameters `AMapView` and `Handled`. 

  * The parameter `AMapView` contains the MapView which is operated on. If the property `MapView` is assigned, then the parameter will be always identical to the property and could be ignored. If the property `MapView` is not assigned, then the parameter has be used to interact with the current MapView. It's up to the plugin to deal with the different settings of the MapView to avoid unwanted behavior.
  * The var parameter `Handled` informs the plugin whether prior called plugins have already consumed the call. This parameter allows plugins to interact save. For example, if the user press the mouse button and one of the plugins identify that the location of the mouse fits to its use, it will set the Handled parameter to inform the subsequently called plugins, to keep the hands of this MouseEvent.



You may imagine two plugins. The first one operates with markers, the second one displays a clickable link. If the marker is located in front of the link and the user hover the mouse above, then only the first one should alter the mouse pointer according to the possible action and not the second one. 

The methods are grouped into three main groups: Mouse interaction, Drawing and changes within the MapView. 

  * The procedure AfterDrawObjects is called when the MapViewer has painted the map and all objects, stored in the Layer property. At this time it is possible to access the DrawingEngine and paint something on top of the current view. Since using the [MapViewer#The_Drawing_Engine](</index.php?title=DrawingEngnise&action=edit&redlink=1> "DrawingEngnise \(page does not exist\)") allows to alter the displayed map (instead of the screen display), those changes could be seen on images which are saved to file.
  * The procedure AfterPaint is called if the painting of the MapViewer is finished. Access to the DrawingEngine will not change the display. Instead it is possible to draw direct to the Canvas of the MapViewer. This allows to display temporary elements (e.g. Selectors, Arrows, UI-Items, etc.) which should be not saved to a file.
  * The procedure BeforeDrawObjects is called just before the objects of the Layers are painted. This point allows various change of the objects, including the complete rebuilding of the content. A different usage is the painting to the map prior the objects.
  * The procedure CenterMove is called to inform the plugin that the center of the map is moved
  * The procedure CenterMoving is called when the user is about to move the center. The plugin may alter the parameter `NewCenter` or set the parameter `Allow` to false to omit the change.
  * The procedure DrawGPSPoint allows the drawing of the given `APoint`. The parameter `Handled` should be set if the drawing happened to omit the further drawing of the MapView or the call to the OnDrawGPSPoint-Event. If `Handled` remains false, the MapView will perform the paint of the object.
  * The procedure DrawMissingTile is called if a needed map tile is not available. The Plugin may fill the area given in the parameter `ARect` as desired. The parameter `ATileID` informs about the coords of the tile. If the parameter `Handled` remains false, the MapView will continue with the default processing.
  * The procedure GPSItemsModified informs the plugin, that the content of the parameter `ModifiedList` might have changed. If some items has been added the parameter `Adding` is `true` and if the parameter `ActualObjs` is assigned, then ActualObjs contains the added items. If the parameter `Adding` is `false` and the parameter `ActualObjs` is assigned, than ActualObjs contains the removed items. If `ActualObjs` is not assigned than the change of the list is unknown due to a encapsulated BeginUpdate..EndUpdate. If the parameter `ModifiedList` is not assigned, than all local references of items to this MapView should be removed.
  * The procedure MouseDown is called if the user pressed one of the mouse buttons.
  * The procedure MouseEnter is called if the mouse pointer enters the MapView.
  * The procedure MouseLeave is called if the mouse pointer leaves the MapView.
  * procedure MouseMove is called if the mouse pointer changed it's position.
  * procedure MouseUp is called if the user released one of the mouse buttons.
  * procedure MouseWheel is called if the mouse wheel changed.
  * procedure ZoomChange is called to inform the Plugin that the Zoom-factor of the Map changed.
  * procedure ZoomChanging is called if the Zoom-factor of the map is about to change. The plugin may deny the change by setting the paramete `Allow` to `false`



## Example of a Plugin

The following example implements a plugin, to measure distances on the map. The user can click into the map to set an anchor and then on the mouse move the actual distance between the anchor and the mouse position is calculated. The plugin can only operate on a single MapView. 
    
    
    unit uMeasureDistPlugin;
    
    {$mode ObjFPC}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Controls, Graphics,
      mvMapViewer, mvTypes, mvGeoMath, mvPluginCore;
    
    type
    
      { TMeasureDistPlugin }
    
      TMeasureDistPlugin = class(TMvPlugin)
      private
        FMeasureActive : Boolean;
        FAnchorCoord : TRealPoint;
        FCurrentCoord : TRealPoint;
        FDistance : Double;
      protected
        procedure AfterPaint(AMapView: TMapView; var Handled: Boolean); override;
        procedure MouseMove(AMapView: TMapView; AShift: TShiftState; X,Y: Integer;
              var Handled: Boolean); override;
        procedure MouseUp(AMapView: TMapView; Button: TMouseButton; Shift: TShiftState;
              X,Y: Integer; var Handled: Boolean); override;
      public
      end;
    implementation
    
    { TMeasureDistPlugin }
    
    procedure TMeasureDistPlugin.AfterPaint(AMapView: TMapView; var Handled: Boolean);
    var
      ptAnchorScreen, ptCurrentScreen : TPoint;
      s : String;
      r : Integer;
    begin
      if FMeasureActive then
      begin
        MapView.Canvas.Pen.Style:= psSolid;
        MapView.Canvas.Pen.Width:= 3;
        ptAnchorScreen := MapView.LatLonToScreen(FAnchorCoord);
        ptCurrentScreen := MapView.LatLonToScreen(FCurrentCoord);
        r := 3;
        MapView.Canvas.Pen.Color:= clLime;
        MapView.Canvas.Ellipse(ptAnchorScreen.X-r,ptAnchorScreen.Y-r,
                                ptAnchorScreen.X+r,ptAnchorScreen.Y+r);
        MapView.Canvas.Pen.Color:= clRed;
        MapView.Canvas.Ellipse(ptCurrentScreen.X-r,ptCurrentScreen.Y-r,
                                ptCurrentScreen.X+r,ptCurrentScreen.Y+r);
    
        s := Format('%1.1f Km',[FDistance]);
        MapView.Canvas.TextOut(ptCurrentScreen.X+10, ptCurrentScreen.Y+10, s);
      end;
    end;
    
    procedure TMeasureDistPlugin.MouseMove(AMapView: TMapView; AShift: TShiftState;
      X, Y: Integer; var Handled: Boolean);
    begin
      if FMeasureActive then
      begin
        FCurrentCoord := MapView.ScreenToLatLon(Point(X,Y));
        if (FCurrentCoord.Lat = FAnchorCoord.Lat) and
           (FCurrentCoord.Lon = FAnchorCoord.Lon) then
          FDistance := 0.0
        else
          FDistance := CalcGeoDistance(FAnchorCoord.Lat,FAnchorCoord.Lon,
                                       FCurrentCoord.Lat, FCurrentCoord.Lon,
                                       duKilometers, esEllipsoid);
        MapView.Invalidate;
      end;
    end;
    
    procedure TMeasureDistPlugin.MouseUp(AMapView: TMapView; Button: TMouseButton;
      Shift: TShiftState; X, Y: Integer; var Handled: Boolean);
    begin
      if Button <> mbRight then Exit;
      FMeasureActive := not FMeasureActive;
      if FMeasureActive then
      begin
        FAnchorCoord := MapView.ScreenToLatLon(Point(X,Y));
        FCurrentCoord := FAnchorCoord;
      end;
      if not FMeasureActive then
        MapView.Invalidate; // This will remove the two circles and the text
    end;
    
    end.
    

  
In the main form add a TMapView and a PluginManager. Add the PluginManager in the MapView. setup the MapProvider and activate the MapView. 

In the FormCreate-event add the following code 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      lMeasureDistPlugin : TMeasureDistPlugin;
    begin
      lMeasureDistPlugin := TMeasureDistPlugin.Create(Self);
      lMeasureDistPlugin.MapView := MapView1;
      MvPluginManager1.PluginList.Add(lMeasureDistPlugin);
    end;

---

_Source: [https://wiki.freepascal.org/LazMapViewer-Plugins](https://web.archive.org/web/20250515130157/https://wiki.freepascal.org/LazMapViewer-Plugins)_
