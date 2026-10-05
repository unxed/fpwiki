# FPReport Custom elements

│ **English (en)** │


FPReport contains support for 4 basic reporting elements: 

  * Memo 
  * Shape 
  * Image 
  * Checkbox 



While these will get you a long way, sometimes something extra is needed. 

On this page we explain how to create a new element. 

We'll take the sample TReportPolygon as an example. 

FPReport by default contains 2 custom reporting elements: 

  * BarCode 
  * QRCode (based on [Nayuki's QR code generator](<https://www.nayuki.io/page/qr-code-generator-library>)) 



Both elements are developed using the technique explained on this page. 

## Contents

  * 1 Create a new element class
    * 1.1 Copying properties for cloning
    * 1.2 Writing properties to a stream
    * 1.3 Reading properties from a stream
  * 2 Registering the new element
  * 3 Rendering the new element
    * 3.1 Image fallback renderer
    * 3.2 Renderer-specific callback: PDF
    * 3.3 Renderer-specific callback: LCL
  * 4 More info



## Create a new element class

Any printable element must descend from TFPReportElement. It must expose published properties which allow the user to control behaviour of the element. 
    
    
    TReportPolygon = class(TFPReportElement)
    Published
      Property Corners : Cardinal Read FCorners Write SetCorners;
      Property LineWidth : Cardinal Read FLineWidth Write SetCLineWidth;
      Property Color : TFPReportColor Read FColor Write FColor;
      // In degrees
      Property RotateAngle : Double Read FRotateAngle Write FRotateAngle;
    end;

The element itself must only know how to read/write its properties from/to a report streamer, and how to copy properties to another instance. 

### Copying properties for cloning

Copying properties to another instance is done in the usual fashion, using the **Assign** method: 
    
    
    procedure TReportPolygon.Assign(Source: TPersistent);
     
    Var
      P : TReportPolygon;
     
    begin
      if (Source is TReportPolygon) then
        begin
        P:=Source as TReportPolygon;
        Corners:=P.Corners;
        Color:=P.Color;
        LineWidth:=P.LineWidth;
        RotateAngle:=P.RotateAngle;
        end;
      inherited Assign(Source);
    end;

This is important because during layouting, the assign method is used to clone the design elements before layouting them on the page. 

### Writing properties to a stream

When saving a report to file, all elements are written to a stream using a **TFPReportStreamer** class. (unit _fpreportstreamer_) 

Writing properties to the streamer must be implemented in the **WriteElement** method, or the **DoWriteLocalProperties** method: 
    
    
    procedure TReportPolygon.DoWriteLocalProperties(AWriter: TFPReportStreamer;
      AOriginal: TFPReportElement);
     
    Var
      P : TReportPolygon;
     
    begin
      inherited DoWriteLocalProperties(AWriter, AOriginal);
      if AOriginal is TReportPolygon then
        begin
        P:=AOriginal as TReportPolygon;
        AWriter.WriteIntegerDiff('Color', Color, P.Color);
        AWriter.WriteIntegerDiff('Corners',Corners,P.Color);
        AWriter.WriteIntegerDiff('LineWidth',LineWidth,P.LineWidth);
        AWriter.WriteFloatDiff('RotateAngle',RotateAngle,P.RotateAngle);
        end
      else
        begin
        AWriter.WriteInteger('Color', Color);
        AWriter.WriteInteger('Corners',Corners);
        AWriter.WriteInteger('LineWidth',LineWidth);
        AWriter.WriteFloat('RotateAngle',RotateAngle);
        end;
    end;

The WriteElement can be called during layouting. During layouting, a copy is made of each element, and the propertie values can differ from the values set during design. To minimize the size of the stream, only a diff may be written: **AOriginal** is the element as designed, and element, do be able to write only properties that have changed. 

### Reading properties from a stream

When the report design is read from a stream, every element is read from stream. This happens using the same **TFPReportStreamer** class (unit _fpreportstreamer_) as is used to write the element. 

The ReadElement method is responsible for reading the element's design from stream: 
    
    
    procedure TReportPolygon.ReadElement(AReader: TFPReportStreamer);
    begin
      inherited ReadElement(AReader);
      Color:= AReader.ReadInteger('Color', clBlack);
      Corners:=AReader.ReadInteger('Corners',3);
      LineWidth:=AReader.ReadInteger('LineWidth',1);
      RotateAngle:=AReader.ReadFloat('RotateAngle',0);
    end;

## Registering the new element

Before the element can be used in a stream or rendered (in PDF, image, HTML or on screen) it must be registered. This is done using the element factory, gElementFactory= 
    
    
      gElementFactory.RegisterClass('Polygon',TReportPolygon);

When reading a report design from stream, this tells the streaming engine that whenever it encounters a 'Polygon' element type, it must create an instance of **TReportPolygon** , and read it from stream. 

The element factory also keeps the various hooks for rendering the element. 

## Rendering the new element

An element by itself does not know how to render: it is the task of a renderer to know this. The renderers know how to render the standard fpreport elements. 

For all unknown elements, the renderer engine will look in the element factory for a handler to actually render the list. It has already rendered the frame (if any) There are 2 types of render callback: 

  1. Renderer-Specific handler. 



    

     This render callback is specific to the current renderer. It's parameters are: 

  * aPos: Offset of the position on the page. 
  * aElement: the element to render 
  * aExporter: the renderer which is currently rendering 
  * aDPI : the DPI the renderer is using. 


    
    
      TFPReportElementExporterCallBack = Procedure(aPos : TFPReportPoint; aElement : TFPReportElement; AExporter : TFPReportExporter; ADPI: Integer);

  1. Fallback Image renderer: 



    

     This handler must render the element to a TFPCustomImage instance. The image has the expected size. 
     The image will then be placed correctly on the page by the renderer. 
    
    
      TFPReportImageRenderCallBack = Procedure(aElement : TFPReportElement; aImage: TFPCustomImage);

It is sufficient to have a fallback renderer, but it can be advantageous to have renderer-specific callbacks. 

For the HTML renderer, complex graphics can be rendered using the fallback renderer. 

### Image fallback renderer

To render an element on a **TFPCustomImage** descendent, the **TFPImageCanvas** canvas class can be used: 
    
    
    Procedure RenderPolygonToImage(aElement : TFPReportElement;AImage : TFPCustomImage);
     
    Var
      C : TFPImageCanvas;
      P : TReportPolygon;
     
    begin
      P:=AElement as TReportPolygon;
      C:=TFPImageCanvas.Create(AImage);
      try
        if (AElement.Frame.BackgroundColor<>fpreport.clNone) then
          begin
          C.Brush.FPColor:=ColorToRGBTriple(AElement.Frame.BackgroundColor);
          C.Brush.Style:=bsSolid;
          C.FillRect(0,0,AImage.Width-1,AImage.Height-1);
          end;
        PaintPolygon(C,Point(0,0),AImage.Width,AImage.Height,P.Corners,P.RotateAngle,P.linewidth,ColorToRGBTriple(P.Color));
      finally
        C.Free;
      end;
    end;

The actual paint is then done in PaintPolygon: 
    
    
    Procedure PaintPolygon(Canvas : TFPCustomCanvas; AOffset : TPoint; AWidth,AHeight : Integer; ANumber : Integer; AStartAngle : Double; ALineWidth : Integer; AColor : TFPColor);
     
    Var
      CX,CY,R,I : Integer;
      P : Array of TPoint;
      A,Step : Double;
     
    begin
      Canvas.Pen.FPColor:=AColor;
      Canvas.Pen.Width:=aLineWidth;
      Canvas.Pen.Style:=psSolid;
      if ANumber<3 then
        exit;
      CX:=AOffset.x+AWidth div 2;
      CY:=AOffset.y+AHeight div 2;
      if aWidth<aHeight then
        R:=AWidth div 2
      else
        R:=AHeight div 2;
      SetLength(P,ANumber);
      A:=AStartAngle;
      Step:=(2*Pi)/ANumber;
      For I:=0 to ANumber-1 do
        begin
        P[i].X:=CX+Round(R*Cos(a));
        P[i].Y:=CY-Round(R*Sin(a));
        A:=A+Step;
        end;
      For I:=0 to ANumber-2 do
        Canvas.Line(P[I],P[I+1]);
      Canvas.Line(P[ANumber-1],P[0]);
      SetLength(P,0);
    end;

This routine will be more or less verbatim copied for all other renderers. 

To tell the rendering engin about this routine, it must be registered in the element factory. A good place for this is right after the registration of the new element: 
    
    
    begin
      gElementFactory.RegisterClass('Polygon',TReportPolygon);
      // Fallback renderer
      gElementFactory.RegisterImageRenderer(TReportPolygon,@RenderPolygonToImage);

### Renderer-specific callback: PDF

Converting an element to image and then rendering the image on the page may not always be the most efficient or accurate method. 

For example, the PDF creator has built in support for rendering polygons. This can be used to substantually speed up the drawing of a polygon when rendering to PDF (and at the same time makes the PDF file much smaller): 
    
    
    Procedure RenderPolygonInPDF(AOffset : TFPReportPoint; E: TFPReportElement; RE : TFPReportExporter; ADPI : Integer);
     
    Var
      PR : TFPReportExportPDF;
      PG : TReportPolygon;
      C : TPDFCoord;
      I,ANumber : Integer;
      P : Array of TPDFCoord;
      R,A,Step : Double;
      APage: TPDFPage;
     
    begin
      PR:=RE as TFPReportExportPDF;
      PG:=E as TReportPolygon;
      APage:=PR.CurrentPage;
      ANumber:=PG.Corners;
      if ANumber<3 then
        exit;
      C.X:=AOffset.Left+E.RTLayout.Left+E.RTLayout.Width / 2;
      C.Y:=AOffset.Top+E.RTLayout.Top+E.RTLayout.Height / 2;
      if E.RTLayout.Width<E.RTLayout.Height then
        R:=E.RTLayout.Width / 2
      else
        R:=E.RTLayout.Height / 2;
      SetLength(P,ANumber);
      A:=PG.RotateAngle;
      Step:=(2*Pi)/ANumber;
      For I:=0 to ANumber-1 do
        begin
        P[i].X:=C.X+R*Cos(a);
        P[i].Y:=C.Y-R*Sin(a);
        A:=A+Step;
        end;
      APage.SetColor(PG.Color,True);
      APage.DrawPolyGon(P,PG.LineWidth);
      APage.StrokePath;
    end;

Registering this callback is again done on the element factory. 

In this case in addition to the element class and element class, the renderer class must be specified as well. 
    
    
     gElementFactory.RegisterElementRenderer(TReportPolygon,TFPReportExportPDF,@RenderPolygonInPDF);

### Renderer-specific callback: LCL

For completeness' sake we'll also show how to render in the LCL renderer: this renderer is used in the designer tool to design and preview the report. It is therefor a good idea to always idea to register an LCL renderer. 

It resembles the FPImage fallback renderer: 
    
    
    Procedure RenderPolygonInLCL(AOffset : TFPReportPoint; E: TFPReportElement; RE : TFPReportExporter; ADPI : Integer);
    var
      PR : TFPReportExportCanvas;
      PG : TReportPolygon;
      R :  Trect;
      rPt : TFPReportPoint;
      pt : TPoint;
     
    begin
      PR:=RE as TFPReportExportCanvas;
      PG:=E as TReportPolygon;
      rpt.Left:=AOffset.Left+E.RTLayout.Left;
      rpt.Top:=AOffset.Top+E.RTLayout.top;
      Pt:=PR.CoordToPoint(rpt,0,0);
      R.TopLeft:=pt;
      R.Right:=R.Left+PR.HmmToPixels(E.RTLayout.Width);
      R.Bottom:= R.Top+PR.VmmToPixels(E.RTLayout.Height);
      PR.Canvas.Brush.Color:=e.Frame.BackgroundColor;
      PR.Canvas.Brush.Style:=bsSolid;
      PR.Canvas.FillRect(R);
      LCLPaintPolygon(PR.Canvas,R,PG.Corners,PG.RotateAngle,PG.LineWidth,PR.RGBToBGR(PG.Color));
    end;

The paint procedure is a copy of the one for the image renderer: 
    
    
    Procedure LCLPaintPolygon(Canvas : TCanvas; ARect : Trect; ANumber : Integer; AStartAngle : Double; ALineWidth : Integer; AColor : TColor);
     
    Var
      CX,CY,R,I : Integer;
      P : Array of TPoint;
      A,Step : Double;
      AWidth,AHeight : Integer;
     
    begin
      AWidth:=ARect.Right-ARect.Left+1;
      AHeight:=ARect.Bottom-ARect.Top+1;
      Canvas.Pen.Color:=AColor;
      Canvas.Pen.style:=psSolid;
      Canvas.Pen.Width:=aLineWidth;
      if ANumber<3 then
        exit;
      CX:=ARect.Left+AWidth div 2;
      CY:=ARect.Top+AHeight div 2;
      if aWidth<aHeight then
        R:=AWidth div 2
      else
        R:=AHeight div 2;
      SetLength(P,ANumber);
      A:=AStartAngle;
      Step:=(2*Pi)/ANumber;
      For I:=0 to ANumber-1 do
        begin
        P[i].X:=CX+Round(R*Cos(a));
        P[i].Y:=CY-Round(R*Sin(a));
        A:=A+Step;
        end;
      For I:=0 to ANumber-2 do
        Canvas.Line(P[I],P[I+1]);
      Canvas.Line(P[ANumber-1],P[0]);
    end;

Again, the render callback must be registered: 
    
    
    gElementFactory.RegisterElementRenderer(TReportPolygon,TFPReportExportCanvas,@RenderPolygonInLCL);

After this, the rendering in LCL will be done directly, without copying bitmaps. 

## More info

See the **reportpolygon.pas** unit in the demo applications, it contains renderers for the 5 known renderers.

---

_Source: [https://wiki.freepascal.org/FPReport_Custom_elements](https://web.archive.org/web/20190523035639/https://wiki.freepascal.org/FPReport_Custom_elements)_
