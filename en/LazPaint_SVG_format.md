# LazPaint SVG format

This is the specification of the SVG extensions used by LazPaint. 

## Contents

  * 1 Used terms
  * 2 Layer transform
    * 2.1 transform
    * 2.2 bgra:originalViewBox
  * 3 See Also



### Used terms

A **layer** refers to a layer in an OpenRaster stack. They are combined together with an integer offset (x and y), a blending mode and an opacity. 

A **group**[[1]](<https://developer.mozilla.org/en-US/docs/Web/SVG/Element/g>) is an SVG element that contains other elements. It can apply a transform to its content. 

A **non trivial transform** is a transform that is not a translation of integer coordinates. 

  


### Layer transform

When LazPaint stores an SVG layer and that this layer has a non trivial transform, it put all the elements of the SVG within a group in order to make the transform part of the SVG (rotation, scaling and skew). 

#### transform

The **transform** attribute of the group stores a transform similar to the one applied to the layer, except that it can be translated along the x and y axes. The inverse translation is specified in the x and y attribute of the layer in the OpenRaster stack, so that the resulting transform matches the one applied to the layer. 

There is a translation because the bounding box of the transformed SVG is generally not the same as the resulting image, in particular when it overflows the image bounds. The SVG bounds is computed to fit the transformed view box. 

#### bgra:originalViewBox

The original view box is stored in the **bgra:originalViewBox** attribute. For example, if the SVG had its custom coordinates going from 0 to 100, it would have the value "0 0 100 100". This allows to draw a highlight rectangle when moving the SVG layer showing the current rotation even though the SVG bounds have been widened. 

In order for the SVG to follow the specifications, there is a **xmlns:bgra** attribute giving an URL pointing to this specification. 

## See Also

  * [LazPaint](<LazPaint.md> "LazPaint")

---

_Source: [https://wiki.freepascal.org/LazPaint_SVG_format](https://web.archive.org/web/20230331163914/https://wiki.freepascal.org/LazPaint_SVG_format)_
