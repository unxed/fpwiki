# Area Calculations Library

│ **[Deutsch (de)](</Area_Calculations_Library/de> "Area Calculations Library/de")** │  **English (en)** │    
****

A library for area calculation. 
    
    
    unit  uArea ;
    
    {$mode objfpc} {$H +} 
    {$IMPLICITEXCEPTIONS OFF}
    
    interface
    
    uses 
      SysUtils ;
    
    function  funSquareArea (dblL : double) : double; 
    function  funSquareDiagonalLength (dblL : double) : double; 
    function  funSquareLength (dblA : double) : double; 
    function  funSquarePerimeter (dblL : double) : double;
    
    function  funRectangleArea (dblL, dblB : double) : double;
    function  funRectangleDiagonalLength (dblL, dblB : double) : double; 
    function  funRectanglePerimeter (dblL, dblB : double) : double;
    
    function  funRhombusArea (dblL, dblB : double) : double; 
    function  funRhombusPerimeter (dblL, dblB : double) : double;
    
    function  funeTrapezoidArea (dblL1, dblL2, dblB : double) : double; 
    function  funTrapezMedian (dblL1, dblL2 : double) : double; 
    function  funTrapezoidPerimeter (dblL1, dblL2, dblL3, dblL4 : double) : double;
    
    function  funTriangleArea (dblL, dblB : double) : double; 
    function  funTrianglePerimeter (dblL1, dblL2, dblL3 : double) : double;
    
    function  funEquilateralTriangleHeight (dblL : double) : double;
    
    function  funCircleArea (dblD : double) : double; 
    function  funCircleCircumference (dblD : double) : double; 
    function  funCircleRadius (dblA : double) : double;
    
    function  funCircleSegmentArea (dblA, dblD : double) : double; 
    function  funCircleSegmentChordLength (dblR, dblA : double) : double;
    
    function  funCircleSegmentArcLength (dblR, dblA : double) : double;
    function  funCircleSegmentChordLength (dblR, dblB : double) : double; 
    
    function  funCircleSegmentSagitta (dblR, dblL : double) : double;
    
    function  funEllipseArea (dblD1, dblD2 : double) : double; 
    function  funEllipseCircumference (dblD1, dblD2 : double) : double;
    
    
    implementation
    
    function  funSquareArea (dblL : double) : double; 
    begin 
      // dblL is the length 
      Result := dblL * dblL; 
    end;
    
    function  funSquareDiagonalLength (dblL : double) : double; 
    begin 
      // dblL is the length 
      Result := (sqrt (2)) * dblL; 
    end;
    
    function funSquareLength (dblA : double) : double; 
    begin 
      // dblA is the area 
      Result := sqrt (dblA); 
    end;
    
    function SquarePerimeter (dblL : double) : double; 
    begin 
      // dblL is the length 
      Result := 4 * dblL; 
    end;
    
    function funRectangleArea (dblL, dblB : double) : double; 
    begin 
      // dblL is the length 
      // dblB is the width 
      Result := dblL * dblB; 
    end;
    
    function funRectangleDiagonalLength (dblL, dblB : double) : double; 
    begin 
      // dblL is the length 
      // dblB is the width 
      Result := sqrt (dblL + dblB); 
    end;
    
    function funRectanglePerimeter (dblL, dblB : double) : double; 
    begin 
      // dblL is the length 
      // dblB is the width 
      Result := 2 * (dblL + dblB); 
    end;
    
    function funRhombusArea (dblL, dblB : double) : double; 
    begin 
      // rhombus = parallelogram 
      // dblL is the length 
      // dblB is the width 
      Result := dblL * dblB; 
    end;
    
    function funRhombusPerimeter (dblL, dblB : double) : double; 
    begin 
      // rhombus = parallelogram 
      // dblL is the length 
      // dblB is the width 
      Result := 2 * (dblL + dblB); 
    end;
    
    function funTrapezoidArea (dblL1, dblL2, dblB : double) : double; 
    begin 
      // dblL1 is the great length 
      // dblL2 is the short length 
      // dblB is the width 
      Result := ((dblL1 * dblL2) / 2) * dblB; 
    end;
    
    function funTrapezoidMedian (dblL1, dblL2 : double) : double; 
    begin 
      // dblL1 is the large length 
      // dblL2 is the small length 
      Result := (dblL1 * dblL2) / 2; 
    end;
    
    function funTrapezoidPerimeter (dblL1, dblL2, dblL3, dblL4 : double) : double; 
    begin 
      // dblL1 to dblL4 are the lengths of the trapezoid 
      Result := dblL1 + dblL2 + dblL3 + dblL4; 
    end;
    
    function funTriangleArea (dblL, dblB : double) : double; 
    begin 
      // dblL is the length 
      // dblB is the width 
      Result := (dblL * dblB) / 2; 
    end;
    
    function funTrianglePerimeter (dblL1, dblL2, dblL3 : double) : double; 
    begin 
      // dblL1 to dblL3 are the lengths of the triangle 
      Result := dblL1 + dblL2 + dblL3; 
    end;
    
    function funEquilateralTriangleHeight (dblL : double) : double; 
    begin 
      // dblL is the length 
      Result := 0.5 * sqrt(3) * dblL; 
    end;
    
    function funCircleArea (dblD : double) : double; 
    begin 
      // dblD is the diameter 
      Result := (PI * (dblD * dblD)) / 4; 
    end;
    
    function funCircleCircumference (dblD : double) : double; 
    begin 
      // dblD is the diameter 
      Result := PI * dblD; 
    end;
    
    function funCircleRadius (dblA : double) : double; 
    begin 
      // dblA is the area 
      Result := sqrt (dblA / PI); 
    end;
    
    function funCircleSegmentArea (dblA, dblD : double) : double; 
    begin 
      // dblD is the circle diameter 
      // dblA is the central angle
      Result := (PI * (dblD * dblD) / 4) * (dblA / 360); 
    end;
    
    function funCircleSegmentChordLength (dblR, dblA : double) : double; 
    begin 
      // dblR is the radius 
      // dblA is the central angle 
      Result := 2 * (dblR * (sin (dblA / 2))); 
    end;
    
    function funCircleSegmentArcLength (dblR, dblA : double) : double; 
    begin 
      // dblR is the radius 
      // dblA is the central angle 
      Result := (PI * dblR * dblA) / 180; 
    end;
    
    function funCircleSegmentChordLength (dblR, dblB : double) : double; 
    begin 
      // dblR is the radius 
      // dblB is the width 
      Result := 2 * sqrt (dblB * ((2 * dblR) - dblB)); 
    end;
    
    function funCircleSegmentSagitta (dblR, dblL : double) : double; 
    begin 
      // dblR is the radius 
      // dblL is the chord length 
      Result := dblR - (sqrt ((dblR * dblR) - ((dblL * dblL) / 4))); 
    end;
    
    function funEllipseArea (dblD1, dblD2 : double) : double; 
    begin 
      // dblD1 is the large diameter of the ellipse 
      // dblD2 is the small diameter of the ellipse 
      Result := (Pi * dblD1 * dblD2) / 4; 
    end;
    
    function funEllipseCircumference (dblD1, dblD2 : double) : double; 
    begin 
      // dblD1 is the large diameter of the ellipse 
      // dblD2 is the small diameter of the ellipse 
      Result := (Pi / 2) * (dblD1 + dblD2); 
    end;
    
    end.

---

_Source: [https://wiki.freepascal.org/Area_Calculations_Library](https://web.archive.org/web/20250219040533/https://wiki.freepascal.org/Area_Calculations_Library)_
