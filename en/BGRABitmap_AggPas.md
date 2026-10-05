# BGRABitmap AggPas

Here is a series of projects demonstrating advanced capabilities in [BGRABitmap](<BGRABitmap.md> "BGRABitmap") similar to [AggPas](<http://www.crossgl.com/aggpas/>)

The projects are in the **bgraaggtest** folder in [BGRABitmap repository](<https://github.com/bgrabitmap/bgrabitmap/tree/master/test/bgraaggtest>)

## Contents

  * 1 Antialiasing demo (AA demo)
  * 2 Alpha gradient
  * 3 Blur
  * 4 Bézier like splines (B-spline)
  * 5 Distortions
  * 6 Gouraud shading
  * 7 Image filters 2
  * 8 Image perspective



### Antialiasing demo (AA demo)

This program shows how the gamma factor affects how antialiasing is rendered. 

[![aa demo.png](https://wiki.freepascal.org/images/6/69/aa_demo.png)](</File:aa_demo.png>)

### Alpha gradient

This program shows how to apply a complex alpha mask based on a custom gradient. See [Tutorial 11](<BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") on how to create a custom scanner. 

[![alpha gradient.png](https://wiki.freepascal.org/images/e/e2/alpha_gradient.png)](</File:alpha_gradient.png>)

### Blur

This program shows the different types of blurs and their speed. 

[![blur.png](https://wiki.freepascal.org/images/7/72/blur.png)](</File:blur.png>)

### Bézier like splines (B-spline)

This program to demonstrate various kind of spline interpolations and how to use a path cursor. 

[![bspline.png](https://wiki.freepascal.org/images/2/28/bspline.png)](</File:bspline.png>)

The TBGRAPath objet takes a series of instructions to draw a path in a similar way as with HTML canvas. To define a polygon, you can call `moveTo(x,y)` to define the first point, and then `lineTo(x,y)` for each subsequent point. Finally you can call `closePath` to end the polygon. 

Once the path is created, you can draw it and fill it, either using its `stroke` and `fill` functions, or by calling `TBGRABitmap.DrawPath` or `FillPath` functions. 

Do draw a symbol along the path, you can use `TBGRAPath.CreateCursor` function to create a cursor. The cursor has `MoveForward` and `MoveBackward` for example to move along the path. They return the actual moved length, which will be 0 when the end of the path has been reached. 

The `CurrentCoordinate` property contains the current position and the `CurrentTangent` contains a unit vector to align the shape. 

### Distortions

This program shows how to apply a distortion to an image or a gradient. 

[![distortions.png](https://wiki.freepascal.org/images/b/ba/distortions.png)](</File:distortions.png>)

### Gouraud shading

This program demonstrates how to do a multi-polygon Gouraud shading. 

[![gouraud.png](https://wiki.freepascal.org/images/5/56/gouraud.png)](</File:gouraud.png>)

### Image filters 2

This program shows the various interpolation filters that can be used when resampling. 

[![image filters2.png](https://wiki.freepascal.org/images/1/1b/image_filters2.png)](</File:image_filters2.png>)

### Image perspective

This program demonstrates the different ways of applying a texture to a quad. 

[![image perspective.png](https://wiki.freepascal.org/images/7/7a/image_perspective.png)](</File:image_perspective.png>)

---

_Source: [https://wiki.freepascal.org/BGRABitmap_AggPas](https://web.archive.org/web/20250120190847/https://wiki.freepascal.org/BGRABitmap_AggPas)_
