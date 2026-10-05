# Angle16Deg

│ **English (en)** │

The one angle16deg is 1/16th of a degree. For example, a full circle equals 5760 (= 16*360). 

## function Angle16DegToRad

Convert Angle16Deg to [radian](<Radian.md> "Radian")
    
    
    function Angle16DegToRad(const a_angle16deg:integer):double;
    begin
      result := a_angle16deg * Pi / ( 180 * 16 );
    end;
    

## function RadtoAngle16Deg

Convert radian to Angle16Deg 
    
    
    function RadtoAngle16Deg(const a_radian:double):integer;
    begin
      result := round ( a_radian * 180 * 16 / Pi );
    end;
    

## See also

  * [Radian](<Radian.md> "Radian")
  * [Chord](<http://lazarus-ccr.sourceforge.net/docs/lcl/graphics/tcanvas.chord.html> "doc:lcl/graphics/tcanvas.chord.html")

---

_Source: [https://wiki.freepascal.org/Angle16Deg](https://web.archive.org/web/20240121085935/https://wiki.freepascal.org/Angle16Deg)_
