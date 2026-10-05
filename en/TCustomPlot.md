# TCustomPlot

## Contents

  * 1 About
  * 2 Usage
    * 2.1 License
  * 3 Author



## About

_TCustomPlot_ is a custom plotting component. It allows you to plot a series of points connected with line. 

## Usage

You need to make a set of points, and them will be connected with line. Points should be set via Points property, which is a decendent of TStrings. So, in program you need to set points[i].x and points.y. In ObjectInspector you may edit it as a stringlist, each line is a point in format x:y, where x and y are float falues. XMin,XMax,YMin,YMax - float properties for setting plot borders. Alternatively you may set AutoSizeX and/or AutoSizeY to true, so all points must be shown. HasMarks means whether axes have a marks (float numbers). NMarksX and NMarksY shows how many such marks will be at the correspondend axis. XSpacer and YSpacer means how many space (in pixels) are left for float marks. XMarkDigits, YMarkDigits,XMarkDecimals, YMarkDecimals are about formatting the values. 

### License

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). You can contact the author if the modified LGPL doesn't work with your project licensing. 

## Author

Presently Vasily I.Volchenko

---

_Source: [https://wiki.freepascal.org/TCustomPlot](https://web.archive.org/web/20190918094907/https://wiki.freepascal.org/TCustomPlot)_
