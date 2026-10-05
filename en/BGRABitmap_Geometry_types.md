# BGRABitmap Geometry types

Here are all the basic geometry types used in [BGRABitmap](<BGRABitmap.md> "BGRABitmap") library. They are provided by _BGRABitmapTypes_ unit. 

### Geometry types

_EmptySingle_ : single = -3.402823e38;  
---  
| Value indicating that there is nothing in the single-precision floating point value. It is also used as a separator in lists  
_PPointF_ = ^TPointF;  
| Pointer to a _TPointF_ structure  
_TPointF_ = **packed** **record** x, y: single;  
| Contains a point with single-precision floating point coordinates  
| _EmptyPointF_ : TPointF = (x: -3.402823e38; y: -3.402823e38);  
| | Value indicating that there is an empty _TPointF_ structure. It is also used as a separator in lists of points  
| **function** PointF(x, y: single): TPointF;  
| | Creates a new structure with values _x_ and _y_  
| **function** isEmptyPointF(pt: TPointF): boolean;  
| | Checks if the structure is empty (equal to _EmptyPointF_)  
| **operator** = (**const** pt1, pt2: TPointF): boolean; **inline** ;  
| | Checks if both _x_ and _y_ are equal  
| **operator** \+ (**const** pt1, pt2: TPointF): TPointF; **inline** ;  
| | Adds _x_ and _y_ components separately. It is like adding vectors  
| **operator** \- (**const** pt1, pt2: TPointF): TPointF; **inline** ;  
| | Subtract _x_ and _y_ components separately. It is like subtracting vectors  
| **operator** \- (**const** pt2: TPointF): TPointF; **inline** ;  
| | Returns a point with opposite values for _x_ and _y_ components  
| **operator** * (**const** pt1, pt2: TPointF): single; **inline** ;  
| | Scalar product: multiplies _x_ and _y_ components and returns the sum  
| **operator** * (**const** pt1: TPointF; factor: single): TPointF; **inline** ;  
| | Multiplies both _x_ and _y_ by _factor_. It scales the vector represented by (_x_ ,_y_)  
| **operator** * (factor: single; **const** pt1: TPointF): TPointF; **inline** ;  
| | Multiplies both _x_ and _y_ by _factor_. It scales the vector represented by (_x_ ,_y_)  
| **function** VectLen(dx,dy: single): single; **overload** ;  
| | Returns the length of the vector (_dx_ ,_dy_)  
| **function** VectLen(v: TPointF): single; **overload** ;  
| | Returns the length of the vector represented by (_x_ ,_y_)  
_ArrayOfTPointF_ = **array** **of** TPointF;  
| Contains an array of points with single-precision floating point coordinates  
| **function** PointsF(**const** pts: **array** **of** TPointF): ArrayOfTPointF;  
| | Creates an array of _TPointF_  
| **function** ConcatPointsF(**const** APolylines: **array** **of** ArrayOfTPointF): ArrayOfTPointF;  
| | Concatenates arrays of _TPointF_  
| **function** PolylineLen(**const** pts: **array** **of** TPointF; AClosed: boolean = false): single;  
| | Compute the length of the polyline contained in the array. _AClosed_ specifies if the last point is to be joined to the first one  
_TBGRAPenStyle_ = **array** **of** Single;  
| A pen style can be dashed, dotted, etc. It is defined as a list of floating point number. The first number is the length of the first dash, the second number is the length of the first gap, the third number is the length of the second dash... It must have an even number of values. This is used as a complement to [TPenStyle](<BGRABitmap_Types_imported_from_Graphics.md> "BGRABitmap Types imported from Graphics")  
| **function** BGRAPenStyle(dash1, space1: single; dash2: single=0; space2: single = 0; dash3: single=0; space3: single = 0; dash4 : single = 0; space4 : single = 0): TBGRAPenStyle;  
| | Creates a pen style with the specified length for the dashes and the spaces  
_TSplineStyle_ = (  
| Different types of spline. A spline is a series of points that are used as control points to draw a curve. The first point and last point may or may not be the starting and ending point  
| _ssInside_ ,  
| | The curve is drawn inside the polygonal envelope without reaching the starting and ending points  
| _ssInsideWithEnds_ ,  
| | The curve is drawn inside the polygonal envelope and the starting and ending points are reached  
| _ssCrossing_ ,  
| | The curve crosses the polygonal envelope without reaching the starting and ending points  
| _ssCrossingWithEnds_ ,  
| | The curve crosses the polygonal envelope and the starting and ending points are reached  
| _ssOutside_ ,  
| | The curve is outside the polygonal envelope (starting and ending points are reached)  
| _ssRoundOutside_ ,  
| | The curve expands outside the polygonal envelope (starting and ending points are reached)  
| _ssVertexToSide_);  
| | The curve is outside the polygonal envelope and there is a tangeant at vertices (starting and ending points are reached)  
_TCubicBezierCurve_ = **object**  
| Definition of a Bézier curve of order 3. It has two control points _c1_ and _c2_. Those are not reached by the curve  
| _p1_ : TPointF;  
| | Starting point (reached)  
| _c1_ : TPointF;  
| | First control point (not reached by the curve)  
| _c2_ : TPointF;  
| | Second control point (not reached by the curve)  
| _p2_ : TPointF;  
| | Ending point (reached)  
| **function** ComputePointAt(t: single): TPointF;  
| | Computes the point at time _t_ , varying from 0 to 1  
| **procedure** Split(**out** ALeft, ARight: TCubicBezierCurve);  
| | Split the curve in two such that _ALeft.p2_ = _ARight.p1_  
| **function** ComputeLength(AAcceptedDeviation: single = 0.1): single;  
| | Compute an approximation of the length of the curve. _AAcceptedDeviation_ indicates the maximum orthogonal distance that is ignored and approximated by a straight line.  
| **function** ToPoints(AAcceptedDeviation: single = 0.1; AIncludeFirstPoint: boolean = true): ArrayOfTPointF;  
| | Computes a polygonal approximation of the curve. _AAcceptedDeviation_ indicates the maximum orthogonal distance that is ignored and approximated by a straight line. _AIncludeFirstPoint_ indicates if the first point must be included in the array  
| **function** BezierCurve(origin, control1, control2, destination: TPointF) : TCubicBezierCurve; **overload** ;  
| | Creates a structure for a cubic Bézier curve  
_TQuadraticBezierCurve_ = **object**  
| Definition of a Bézier curve of order 2. It has one control point  
| _p1_ : TPointF;  
| | Starting point (reached)  
| _c_ : TPointF;  
| | Control point (not reached by the curve)  
| _p2_ : TPointF;  
| | Ending point (reached)  
| **function** ComputePointAt(t: single): TPointF;  
| | Computes the point at time _t_ , varying from 0 to 1  
| **procedure** Split(**out** ALeft, ARight: TQuadraticBezierCurve);  
| | Split the curve in two such that _ALeft.p2_ = _ARight.p1_  
| **function** ComputeLength: single;  
| | Compute the **exact** length of the curve  
| **function** ToPoints(AAcceptedDeviation: single = 0.1; AIncludeFirstPoint: boolean = true): ArrayOfTPointF;  
| | Computes a polygonal approximation of the curve. _AAcceptedDeviation_ indicates the maximum orthogonal distance that is ignored and approximated by a straight line. _AIncludeFirstPoint_ indicates if the first point must be included in the array  
| **function** BezierCurve(origin, control, destination: TPointF) : TQuadraticBezierCurve; **overload** ;  
| | Creates a structure for a quadratic Bézier curve  
| **function** BezierCurve(origin, destination: TPointF) : TQuadraticBezierCurve; **overload** ;  
| | Creates a structure for a quadratic Bézier curve without curvature  
_PArcDef_ = ^TArcDef;  
| Pointer to an arc definition  
_TArcDef_ = **record**  
| Definition of an arc of an ellipse  
| _center_ : TPointF;  
| | Center of the ellipse  
| _radius_ : TPointF;  
| | Horizontal and vertical of the ellipse before rotation  
| _xAngleRadCW_ : single;  
| | Rotation of the ellipse  
| _startAngleRadCW_ , endAngleRadCW: single;  
| | Start and end angle, in radian and clockwise. See angle convention in _BGRAPath_  
| _anticlockwise_ : boolean  
| | Specifies if the arc goes anticlockwise  
| **function** ArcDef(cx, cy, rx,ry, xAngleRadCW, startAngleRadCW, endAngleRadCW: single; anticlockwise: boolean) : TArcDef;  
| | Creates a structure for an arc definition  
_TArcOption_ = (  
| Possible options for drawing an arc of an ellipse (used in _BGRACanvas_)  
| _aoClosePath_ ,  
| | Close the path by joining the ending and starting point together  
| _aoPie_ ,  
| | Draw a pie shape by joining the ending and starting point to the center of the ellipse  
| _aoFillPath_);  
| | Fills the shape  
| _TArcOptions_ = set **of** TArcOption;  
| | Set of options for drawing an arc  
_TPoint3D_ = **record** x,y,z: single;  
| Point in 3D with single-precision floating point coordinates  
| **function** Point3D(x,y,z: single): TPoint3D;  
| | Creates a new structure with values (_x_ ,_y_ ,_z_)  
| **operator** = (**const** v1,v2: TPoint3D): boolean; **inline** ;  
| | Checks if all components _x_ , _y_ and _z_ are equal  
| **operator** \+ (**const** v1,v2: TPoint3D): TPoint3D; **inline** ;  
| | Adds components separately. It is like adding vectors  
| **operator** \- (**const** v1,v2: TPoint3D): TPoint3D; **inline** ;  
| | Subtract components separately. It is like subtracting vectors  
| **operator** \- (**const** v: TPoint3D): TPoint3D; **inline** ;  
| | Returns a point with opposite values for all components  
| **operator** * (**const** v1,v2: TPoint3D): single; **inline** ;  
| | Scalar product: multiplies components and returns the sum  
| **operator** * (**const** v1: TPoint3D; **const** factor: single): TPoint3D; **inline** ;  
| | Multiplies components by _factor_. It scales the vector represented by (_x_ ,_y_ ,_z_)  
| **operator** * (**const** factor: single; **const** v1: TPoint3D): TPoint3D; **inline** ;  
| | Multiplies components by _factor_. It scales the vector represented by (_x_ ,_y_ ,_z_)  
| **procedure** VectProduct3D(u,v: TPoint3D; **out** w: TPoint3D);  
| | Computes the vectorial product _w_. It is perpendicular to both _u_ and _v_  
| **procedure** Normalize3D(**var** v: TPoint3D); **inline** ;  
| | Normalize the vector, i.e. scale it so that its length be 1  
_TLineDef_ = **record**  
| Defition of a line in the euclidian plane  
| _origin_ : TPointF;  
| | Some point in the line  
| _dir_ : TPointF;  
| | Vector indicating the direction  
| **function** IntersectLine(line1, line2: TLineDef): TPointF;  
| | Computes the intersection of two lines. If they are parallel, returns the middle of the segment between the two origins  
| **function** IntersectLine(line1, line2: TLineDef; **out** parallel: boolean): TPointF;  
| | Computes the intersection of two lines. If they are parallel, returns the middle of the segment between the two origins. The value _parallel_ is set to indicate if the lines were parallel  
| **function** IsConvex(**const** pts: **array** **of** TPointF; IgnoreAlign: boolean = true): boolean;  
| | Checks if the polygon formed by the given points is convex. _IgnoreAlign_ specifies that if the points are aligned, it should still be considered as convex  
| **function** DoesQuadIntersect(pt1,pt2,pt3,pt4: TPointF): boolean;  
| | Checks if the quad formed by the 4 given points intersects itself  
| **function** DoesSegmentIntersect(pt1,pt2,pt3,pt4: TPointF): boolean;  
| | Checks if two segment intersect  
_IBGRAPath_ = **interface**  
| A path is the ability to define a contour with _moveTo_ , _lineTo_... Even if it is an interface, it must not implement reference counting.  
| **procedure** closePath;  
| | Closes the current path with a line to the starting point  
| **procedure** moveTo(**const** pt: TPointF);  
| | Moves to a location, disconnected from previous points  
| **procedure** lineTo(**const** pt: TPointF);  
| | Adds a line from the current point  
| **procedure** polylineTo(**const** pts: **array** **of** TPointF);  
| | Adds a polyline from the current point  
| **procedure** quadraticCurveTo(**const** cp,pt: TPointF);  
| | Adds a quadratic Bézier curve from the current point  
| **procedure** bezierCurveTo(**const** cp1,cp2,pt: TPointF);  
| | Adds a cubic Bézier curve from the current point  
| **procedure** arc(**const** arcDef: TArcDef);  
| | Adds an arc. If there is a current point, it is connected to the beginning of the arc  
| **procedure** openedSpline(**const** pts: **array** **of** TPointF; style: TSplineStyle);  
| | Adds an opened spline. If there is a current point, it is connected to the beginning of the spline  
| **procedure** closedSpline(**const** pts: **array** **of** TPointF; style: TSplineStyle);  
| | Adds an closed spline. If there is a current point, it is connected to the beginning of the spline  
| **procedure** copyTo(dest: IBGRAPath);  
| | Copy the content of this path to the specified destination  
| **function** getPoints: ArrayOfTPointF;  
| | Returns the content of the path as an array of points  
| **function** getCursor: TBGRACustomPathCursor;  
| | Returns a cursor to go through the path. The cursor must be freed by calling _Free_.  
_TBGRACustomPathCursor_ = **class**  
| Class that contains a cursor to browse an existing path  
| **function** MoveForward(ADistance: single; ACanJump: boolean = true): single; **virtual** ; **abstract** ;  
| | Go forward in the path, increasing the value of _Position_. If _ADistance_ is negative, then it goes backward instead. _ACanJump_ specifies if the cursor can jump from one shape to another without a line or an arc. Otherwise, the cursor is stuck, and the return value is less than the value _ADistance_ provided. If all the way has been travelled, the return value is equal to _ADistance_  
| **function** MoveBackward(ADistance: single; ACanJump: boolean = true): single; **virtual** ; **abstract** ;  
| | Go backward, decreasing the value of _Position_. If _ADistance_ is negative, then it goes forward instead. _ACanJump_ specifies if the cursor can jump from one shape to another without a line or an arc. Otherwise, the cursor is stuck, and the return value is less than the value _ADistance_ provided. If all the way has been travelled, the return value is equal to _ADistance_  
| **property** CurrentCoordinate: TPointF **read** ;  
| | Returns the current coordinate in the path  
| **property** CurrentTangent: TPointF **read** ;  
| | Returns the tangent vector. It is a vector of length one that is parallel to the curve at the current point. A normal vector is easily deduced as PointF(y,-x)  
| **property** Position: single **read** **write** ;  
| | Current position in the path, as a distance along the arc from the starting point of the path  
| **property** PathLength: single **read** ;  
| | Full arc length of the path  
| **property** StartCoordinate: TPointF **read** ;  
| | Starting coordinate of the path  
| **property** LoopClosedShapes: boolean **read** **write** ;  
| | Specifies if the cursor loops when there is a closed shape  
| **property** LoopPath: boolean **read** **write** ;  
| | Specifies if the cursor loops at the end of the path. Note that if it needs to jump to go to the beginning, it will be only possible if the parameter _ACanJump_ is set to True when moving along the path  
_EmptyRect_ : TRect = (left:0; top:0; right:0; bottom: 0);  
| A value for an empty rectangle  
**function** PtInRect(**const** pt: TPoint; r: TRect): boolean; **overload** ;  
| Checks if a point is in a rectangle. This follows usual convention: _r.Right_ and _r.Bottom_ are not considered to be included in the rectangle.  
**function** RectWithSize(left,top,width,height: integer): TRect;  
| Creates a rectangle with the specified _width_ and _height_  
_TRoundRectangleOption_ = (  
| Possible options for a round rectangle  
| _rrTopLeftSquare_ ,rrTopRightSquare,rrBottomRightSquare,rrBottomLeftSquare,  
| | specify that a corner is a square (not rounded)  
| _rrTopLeftBevel_ ,rrTopRightBevel,rrBottomRightBevel,rrBottomLeftBevel,  
| | specify that a corner is a bevel (cut)  
| _rrDefault_);  
| | default option, does nothing particular  
| _TRoundRectangleOptions_ = set **of** TRoundRectangleOption;  
| | A set of options for a round rectangle  
_TPolygonOrder_ = (  
| Order of polygons when rendered using _TBGRAMultiShapeFiller_ (in unit _BGRAPolygon_)  
| _poNone_ ,  
| | No order, colors are mixed together  
| _poFirstOnTop_ ,  
| | First polygon is on top  
| _poLastOnTop_);  
| | Last polygon is on top  
_TIntersectionInfo_ = **class**  
| Contains an intersection between an horizontal line and any shape. It is used when filling shapes  
| _ArrayOfTIntersectionInfo_ = **array** **of** TIntersectionInfo;  
| | An array of intersections between an horizontal line and any shape  
_TBGRACustomFillInfo_ = **class**  
| Abstract class defining any shape that can be filled  
| **function** SegmentsCurved: boolean; **virtual** ; **abstract** ;  
| | Returns true if one segment number can represent a curve and thus cannot be considered exactly straight  
| **function** GetBounds: TRect; **virtual** ; **abstract** ;  
| | Returns integer bounds for the shape  
| **function** IsPointInside(x,y: single; windingMode: boolean): boolean; **virtual** ; **abstract** ;  
| | Check if the point is inside the shape  
| **function** CreateIntersectionArray: ArrayOfTIntersectionInfo; **virtual** ; **abstract** ;  
| | Create an array that will contain computed intersections. To augment that array, use _CreateIntersectionInfo_ for new items  
| **function** CreateIntersectionInfo: TIntersectionInfo; **virtual** ; **abstract** ;  
| | Create a structure to define one single intersection  
| **procedure** FreeIntersectionArray(**var** inter: ArrayOfTIntersectionInfo); **virtual** ; **abstract** ;  
| | Free an array of intersections  
| **procedure** ComputeAndSort(cury: single; **var** inter: ArrayOfTIntersectionInfo; **out** nbInter: integer; windingMode: boolean); **virtual** ; **abstract** ;  
| | Fill an array _inter_ with actual intersections with the shape at the y coordinate _cury_. _nbInter_ receives the number of computed intersections. _windingMode_ specifies if the winding method must be used to determine what is inside of the shape  
_TGradientType_ = (  
| Shape of a gradient  
| _gtLinear_ ,  
| | The color changes along a certain vector and does not change along its perpendicular direction  
| _gtReflected_ ,  
| | The color changes like in _gtLinear_ however it is symmetrical to a specified direction  
| _gtDiamond_ ,  
| | The color changes along a diamond shape  
| _gtRadial_);  
| | The color changes in a radial way from a given center  
| _GradientTypeStr_ : **array**[TGradientType] **of** **string**  
| | List of string to represent gradient types  
| **function** StrToGradientType(str: **string**): TGradientType;  
| | Returns the gradient type represented by the given string  
_TBGRACustomGradient_ = **class**  
| Defines a gradient of color, not specifying its shape but only the series of colors  
| **function** GetColorAt(position: integer): TBGRAPixel; **virtual** ; **abstract** ;  
| | Returns the color at a given _position_. The reference range is from 0 to 65535, however values beyond are possible as well  
| **function** GetColorAtF(position: single): TBGRAPixel; **virtual** ;  
| | Returns the color at a given _position_. The reference range is from 0 to 1, however values beyond are possible as well  
| **function** GetAverageColor: TBGRAPixel; **virtual** ; **abstract** ;  
| | Returns the average color of the gradient  
| **property** Monochrome: boolean **read** ;  
| | This property is True if the gradient contains only one color, and thus is not really a gradient

---

_Source: [https://wiki.freepascal.org/BGRABitmap_Geometry_types](https://web.archive.org/web/20181024054625/https://wiki.freepascal.org/BGRABitmap_Geometry_types)_
