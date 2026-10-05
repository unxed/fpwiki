# Geometry in Pascal

│ **[Deutsch (de)](</Geometry_in_Pascal/de> "Geometry in Pascal/de")** │  **English (en)** │  **[français (fr)](</Geometry_in_Pascal/fr> "Geometry in Pascal/fr")** │    
****

## Contents

  * 1 Checking if a point is inside a polygon (integer version)
  * 2 Checking if a point is inside a polygon, version 2
  * 3 See also



## Checking if a point is inside a polygon (integer version)
    
    
    //  The function will return True if the point x,y is inside the polygon, or
    //  False if it is not.
    //
    //  Original C code: http://www.visibone.com/inpoly/inpoly.c.txt
    //
    //  Translation from C by Felipe Monteiro de Carvalho
    //
    //  License: Public Domain
    function IsPointInPolygon(AX, AY: Integer; APolygon: array of TPoint): Boolean;
    var
      xnew, ynew: Cardinal;
      xold,yold: Cardinal;
      x1,y1: Cardinal;
      x2,y2: Cardinal;
      i, npoints: Integer;
      inside: Integer = 0;
    begin
      Result := False;
      npoints := Length(APolygon);
      if (npoints < 3) then Exit;
      xold := APolygon[npoints-1].X;
      yold := APolygon[npoints-1].Y;
      for i := 0 to npoints - 1 do
      begin
        xnew := APolygon[i].X;
        ynew := APolygon[i].Y;
        if (xnew > xold) then
        begin
          x1:=xold;
          x2:=xnew;
          y1:=yold;
          y2:=ynew;
        end
        else
        begin
          x1:=xnew;
          x2:=xold;
          y1:=ynew;
          y2:=yold;
        end;
        if (((xnew < AX) = (AX <= xold))         // edge "open" at left end
          and ((AY-y1)*(x2-x1) < (y2-y1)*(AX-x1))) then
        begin
          inside := not inside;
        end;
        xold:=xnew;
        yold:=ynew;
      end;
      Result := inside <> 0;
    end;
    

## Checking if a point is inside a polygon, version 2

Code from [German forum](<https://www.lazarusforum.de/viewtopic.php?p=118119#p118119>), by winni. 
    
    
    function PointInPoly(p : TPointF; const poly :  array of TPointF) : Boolean; 
    var
     i,k : integer;
    Begin
     result := false;
     k := High(poly);
     For i := 0 to high(poly) do begin
      if (
         ( ((poly[i].y <= p.y) and (p.y < poly[k].y)) or ((poly[k].y <= p.y) and (p.y < poly[i].y)) ) and
            (p.x < ((poly[k].x - poly[i].x) * (p.y - poly[i].y) / (poly[k].y - poly[i].y) + poly[i].x) )
         ) then result := not result;
      k := i
     end;
    end;
    

## See also

  * [A Point about Polygons](<http://www.visibone.com/inpoly/>)

---

_Source: [https://wiki.freepascal.org/Geometry_in_Pascal](https://web.archive.org/web/20241212115937/https://wiki.freepascal.org/Geometry_in_Pascal)_
