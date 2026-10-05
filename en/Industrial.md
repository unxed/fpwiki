# Industrial

[Template:Industrial](</index.php?title=Template:Industrial&action=edit&redlink=1> "Template:Industrial \(page does not exist\)")

## Contents

  * 1 About
  * 2 Download and Installation
  * 3 License
  * 4 Components
    * 4.1 Indicators
      * 4.1.1 TAdvLED
      * 4.1.2 TIndLED
      * 4.1.3 TStopLightSensor
    * 4.2 Alphanumeric Displays
      * 4.2.1 TLEDNumber
      * 4.2.2 TLCDDisplay
    * 4.3 Gauges
      * 4.3.1 TAnalogSensor
      * 4.3.2 TA3nalogGauge
      * 4.3.3 TindGnouMeter
    * 4.4 Control Elements
      * 4.4.1 TmKnob
      * 4.4.2 TMultiSlider
    * 4.5 Switches
      * 4.5.1 TOnOffSwitch



## About

The **Industrial** package contains a set of industrial-themed components, such as LED indicators, LED and LCD displays, analog gauges, thermometer gauges, knob, on/off switch, etc. 

[![ex industrialstuff 2.png](https://wiki.freepascal.org/images/0/0b/ex_industrialstuff_2.png)](</File:ex_industrialstuff_2.png>)

## Download and Installation

  * **Released version** distributed by the Online Package Manager: check the "IndustrialStuff" item in the Online Package manager, click the "Install" button and follow the instructions.
  * **Development version** on Lazarus CCR (svn): 
    * If you have access to _svn_ use the following command to check-out the package into folder _industrialstuff_ :   
`svn checkout <svn://svn.code.sf.net/p/lazarus-ccr/svn/components/industrialstuff> industrialstuff`.
    * Otherwise download the zipped snapshot from <https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/industrialstuff/>. Extract the files in a separate directory.
    * In the Lazarus IDE, open the file **industrial.lpk** in "Package" > "Open Package File (.lpk)" and click on "Use" > "Install". Allow to rebuild the IDE.
  * When Lazarus restarts after the installation the components are on palette _Industrial_.



## License

Since the components contained in the _industrial_ package originate from various authors there is no general, package-wide license. Licenses used are MPL, GPL and modified LGPL. Please check the unit headers for the license of the component that you want to use. 

## Components

### Indicators

#### TAdvLED

[![TAdvLED.png](https://wiki.freepascal.org/images/a/a0/TAdvLED.png)](</File:TAdvLED.png>) Indicator light as LED or light bulb 

  * **Original author** : Dejan Crnila (dejancrn@yahoo.com)
  * **License** : modified LGPL (like Lazarus)
  * **Properties**
    * `Kind` (enumeration `TLEDKind = (lkRedLight, lkGreenLight, lkYellowLight, lkBulb, lkCustom)`): red, green or yellow LED, or light bulb, or LED in adjustable colors as defined by the `GlyphDisabled`, `GlyphOff` and `GlyphOn` properties.
    * `State` (enumeration `TLEDState = (lsDisabled, lsOff, lsOn)`): Indicator is disabled (gray), OFF (dark color) or ON (bright color).
    * `Blink: boolean`: The LED toggles automatically between ON and OFF states. The duration during which the LED is in each state is given by the property `BlinkDuration` (in milliseconds).
    * `FlashMode` (enumeration `TFlashMode = (fmFlashOffToOn, fmFlashOnToOff)`): Defines whether the `Flash`method causes a transition from OFF to ON, or from ON to OFF.
  * **Methods**
    * `procedure Toggle`: Switches between the states of the indicator (unless `State` is `lsDisabled`).
    * `procedure Flash(ADuration: Integer)`: The indicator flashes for the `ADuration` time (in milliseconds) to the other state and then returns to the current state again. Whether the transition is from OFF to ON or from ON to OFF is specified by the `FlashMode` property.
  * **Demo project**
    * in folder _TAdvLED_ of the installation folder.



#### TIndLED

[![TIndLED.png](https://wiki.freepascal.org/images/5/5e/TIndLED.png)](</File:TIndLED.png>) LED indicator with configurable shape and color. Can be grouped with others so that only one LED of that group is active. 

  * **Original author** : Mauricio (<https://sourceforge.net/projects/tcycomponents/>)
  * **License** : MPL 1.1
  * **Properties**
    * `Bevels`: a collection of `TcyBevel` items defining bevels around the LED (raised/lowered, left/top/right/bottom, highlight and shadow color, as well as bevel width)
    * `GroupIndex: Integer`: in a group of LEDs sharing the same non-zero `GroupIndex` only a single LED can be lit. When `AllowAllOff` is `true` all LEDs of this group can be turned OFF, otherwise there must always be one LED which is ON.
    * `LedColorDisabled`, `TLedColorOff`, `TLedColorOn` (type `TColor`): colors of the LED in the disabled, OFF and ON states, respectively.
    * `LedValue: boolean`: determines whether the LED is ON (`true`) or OFF (`false`).
    * `ShapeType` (enumeration `TShapeType = (stRectangle, stRoundRect, stEllipse)`): shape of the LED. In case of `stRoundRect` the corner radius is determined by `ShapeRoundRectX` and `ShapeRoundRectY`.
    * `ShapeLedColorDisabled`, `ShapeLedColorOff`, `ShapeLedColorOn` (type `TColor`): pen color of the border of the LED shape.



#### TStopLightSensor

[![tstoplightsensor.png](https://wiki.freepascal.org/images/6/6c/tstoplightsensor.png)](</File:tstoplightsensor.png>) A descendant of TImage which displays images for the states of a traffic light (red, yellow, green). 

  * **Original author** : Mike Skolnik (<http://www.scalabium.com>), Lazarus port by Jurassic Pork.
  * **License** : "My components are freeware for private and commercial use so you may use any in your applications but without any warranties from my side."
  * **Properties**
    * `State: TStopLights = (slUNKNOWN, slRED, slYELLOW, slGREEN)`: Indicates the state of the traffic light.



### Alphanumeric Displays

#### TLEDNumber

[![tlednumber.png](https://wiki.freepascal.org/images/1/1f/tlednumber.png)](</File:tlednumber.png>) 14-segment LED display for alpha-numeric text. 

  * **Author** : Based on a component contained in the _TurboPower VisualPlanIt_ library. Lazarus port by Jurassic Pork.
  * **License** : MIT
  * **Properties**
    * `BgColor: TColor`: Specifies the color of the display's background (default: clBlack)
    * `Borderstyle` (Enumeration `TLedNumberBorderStyle = (lnbNone, lnbSingle, lnbSunken, lnbRaised)`: Defines the border around the control.
    * `Caption: string`: String to be displayed by the component. Note that `TLEDNumber` is able to display only the following characters: `' ', '*', '+', ',', '-', '.', '/', '0'..'9', '<', '>', 'A'..'Z', 'a'..'z', ':'`. Other characters are replaced by spaces.
    * `Columns: Integer; Rows: Integer`: The `Caption` is displayed in predefined cells of a grid consisting of as many columns and rows as specified by these properties.
    * `OffColor: TColor`: Specifies the color of the un-lit (darkended) segments.
    * `OnColor: TColor`: Specifies the color of the lit (bright) segments.
    * `Size: Integer`: Size of the display. To be exact, this is the width of the segments in pixels. Allowed values are between 2 and 10.
    * `Slanted: Boolean`: If `true`, the characters are drawn slanted by the amound specified in the `SlantAngle` property.
    * `Transparent: boolean`: If `true`, the display's background (`BgColor`) is not painted.
    * `ZeroToO: boolean`: If `true`, zero digits (`'0'`) are not drawn with the inner slash and thus are identical with the upper-case `'O'`.


  * **Demo Project**
    * in folder _Example/LEDNumberDemo_



[![LEDNumberDemo.png](https://wiki.freepascal.org/images/a/a0/LEDNumberDemo.png)](</File:LEDNumberDemo.png>)

#### TLCDDisplay

[![tlcddisplay 150.png](https://wiki.freepascal.org/images/2/2d/tlcddisplay_150.png)](</File:tlcddisplay_150.png>) Multiline LCD display for display of alphanumeric text (ASCII characters #32..#126, extendable by property editor) 

  * **Authors** : Based on TLCLLine by Yuriy Tereshchenko. Lazarus port by Boban Spasic (spasic@gmail.com) and Werner Pamler. See forum discussion <https://forum.lazarus.freepascal.org/index.php/topic,58357.msg443845.html#msg443845>
  * **License** : Modified LGPL, like Lazarus LCL
  * **Properties**
    * `AutoSize: boolean`: Adjusts the size of the control so that all characters and lines of the dot matrix are visible.
    * `BoardColor: TColor`: Color of the dot matrix background. `ColorScheme` must be set to `csCustomy` in order to change this property. Default: `clBlack`
    * `CharDefs: TCharDefs`: Defines the dot matrices of the displayable characters. By default, all characters between ' ' (#32) and '~' (#126) are allowed, but a double-click on this property opens the `CharDefs` property editor with which new characters can be added or existing characters can be modified; in principle, all UTF8 codepoints are supported.
    * `CharSpace: Boolean`: If `true`, a one-dot space is inserted between characters. Default: `false`.
    * `ColorScheme: TColorScheme = (csCustom, csBlue, csGreen, csInvGreen)`: Pre-defined color sets. Set to csCustom in order to use the `BoardColor`, `DotColorOn` and `DotColorOff`.
    * `DisplayLineCount: Integer`: Number of lines displayed when `AutoSize` is `true`. Default: `2`
    * `DisplayCharCount: Integer`: Number of characters per line when `AuotSize` is `true`. Default: `10`.
    * `DotColorOn: TColor`: Color of a lit dot in the character dot matrix. In order to use `DotColorOn`, the `ColorScheme` must be set to `csCustom`. Default: `clSkyBlue`
    * `DotColorOff: TColor`: Color of an un-lit dot in the character dot matrix. In order to use `DotColorOff`, the `ColorScheme` must be set to `csCustom`. Default: `clTeal`
    * `DotShape: TDotShape = (stSquare, stRound)`: Defines the shape of the dots in the dot matrix. Default: `stSquare`
    * `DotSize: Integer`: Size of a dot in the character dot matrix. Default: `4`
    * `DotSpace: Integer`: Distance between dots in the character dot matrix. Default: `1`
    * `FrameColor: TColor`: Color of the frame around the LCD display. Default: `clBtnFace`
    * `FrameColorStyle: TFrameColorStyle = (stWindows, stColor)`: Determines whether the control's bevel color is derived from system colors (`stWindows`) or from the `FrameColor`. Default: `stWindows`.
    * `FrameHeight: TFrameHeight = (fhDouble, fhSingle)`: Defines the number of pixels (2 or 1, respectively) used to draw the bevel of the display frame.
    * `FrameSize: Integer`: Width of the frame around the LCD display. Default: `8`
    * `FrameStyle: TFrameStyle = (fsRelief, fsNone, fsLowered, fsRaised)`: Defines style of the frame drawn around the LCD display.
    * `Lines: TStringList`: Text to be diplayed


  * **Events**
    * `OnChange: TNotifyEvent`: Fires whenever the display text changes.


  * **Methods**
    * `procedure AddCharDef(AChar: string; const Dots: TDotRows)`: Adds a character (UTF8 encoding) and its associated dot matrix to the `CharDefs`. `TDotRows` is an `array of integer` where each `integer` refers to a dot row in the dot matrix and each set bit in the `integer` means a lit dot in the corresponding row (the least-signficant bit is at the right of dot row). The `TLCDDisplay.CharMap.ColCount` and `.RowCount` indicate the size of the character dot matrix.


  * **Demo projects**
    * in folder _Examples/LCDDisplay/Properties_ (demonstrates the effect of changing the control's properties)
    * in folder _Examples/LCDDisplay/CharDefsEditor_ (demonstrates usage of the LCD dot matrix editor)



[![LCDDisplay Demo.png](https://wiki.freepascal.org/images/d/d1/LCDDisplay_Demo.png)](</File:LCDDisplay_Demo.png>) [![CharDefsEditor.png](https://wiki.freepascal.org/images/f/fc/CharDefsEditor.png)](</File:CharDefsEditor.png>)

### Gauges

#### TAnalogSensor

[![tanalogsensor.png](https://wiki.freepascal.org/images/c/cf/tanalogsensor.png)](</File:tanalogsensor.png>) Analog display which can be configured to have a round or linear scale. Display ranges can be indicated in different colors. 

Original author: Mike Skolnik (<http://www.scalabium.com>), Lazarus port by Jurassic Pork. 

License: _"My components are freeware for private and commercial use so you may use any in your applications but without any warranties from my side."_

  * **Properties**
    * `AnalogKind` (enumeration `TAnalogKind = (akAnalog, akHorizontal, akVertical)`): type of the display: round scale (`akAnalog`) or horizontal (`akHorizontal`) / vertical (`akVertical`) thermometer-like linear scale
    * `ColorBack: TColor`: Color of the display background (default: black)
    * `ColorFore: TColor`: Color of the display "foreground"; the fraction of the area painted by the `ColorFore` corresponds to the displayed `Value` (default: bright green).
    * `ColorRed`, `ColorYellow` (type `TColor`): Color of two lines indicating special data ranges, e.g. value too small or value too high. Default: red and yellow, respectively. These lines are displayed when the `ShowLevel` property is `true`
    * `ShowLevel: boolean`: If `true`, two lines indicating special data conditions are displayed.
    * `ShowText: boolean`: Displays the numerical `Value` below the display.
    * `Value: integer`: Displayed number (corresponds to the area drawn in color `ColorFore`)
    * `ValueMax: Integer`: Maximum value that can be displayed
    * `ValueMin: Integer`: Minimum value that can be displayed
    * `ValueRed`, `ValueYellow: Integer`: Values displayed as the position of the indicator lines drawn in color `ColorRed` and `ColorYellow`, respectively.
    * `Caption: string`: Text displayed in front of the value when `ShowText` is `true`.


  * **Methods**
    * `SetColorState(slStopLight: TStopLights)`: Changes the color of the `Caption` and the `Value` as specified by the parameter `slStopLights: TStopLights = (slUNKNOWN, slRED, slYELLOW, slGREEN)`.


  * **Demo project**
    * in folder _Example/AnalogSensor_



[![AnalogSensor Demo.png](https://wiki.freepascal.org/images/a/a2/AnalogSensor_Demo.png)](</File:AnalogSensor_Demo.png>)

#### TA3nalogGauge

[![ta3naloggauge.png](https://wiki.freepascal.org/images/7/73/ta3naloggauge.png)](</File:ta3naloggauge.png>) A component for displaying floating point values like with an analog volt or ampere meter. Uses the supersampling anti-aliasing algorithm for improvement the image quality. (The "A3" in the component name stands for "anti-aliased analog".) 

  * **Original author** : Irnis Haliullin (<http://www.irnis.net/>, URL no longer valid), Lazarus port and extended by Werner Pamler
  * **License** : _"This component is free for use in any purposes."_
  * **Properties**
    * `Angle: Integer`: Defines the anglular range over which the meter's display needle can move. Default: `120`.
    * `AntiAliased: TAntiAliased = (aaNone, aaBiline, aaTriline, aaQuadral)`: selects the supersampling anti-aliasing algorithm to improve display quality. Default: `aaNone` (no anti-aliasing). Note that higher supersampling results in lower display refresh rate.
    * `ArrowColor: TColor`: Color of the meter's pointer needle. Default: `clBlack`
    * `ArrowWidth: Integer`: Pen width, in pixels, used for drawing the meter's pointer needle. Default: `1`
    * `Caption: String`: Text displayed inside the meter face, for example to indicate the measured quantity ('Voltage', or 'Current')
    * `CaptionColor: TColor`: Color of the text displayed inside the meter face. Default: `clBlack`
    * `CaptionFont: TFont`: Font used to draw the text inside the meter face.
    * `CenterColor: TColor`: Color of the circle from where the meter needle originates. Default: `clDkGray`.
    * `CenterRadius: Integer`: Radius of the circle at which the needle is attached. Default: `8`
    * `CircleColor: TColor`: Color of the small circles drawn at the labeled scale positions. Default: `clBlue`
    * `CircleRadius: Integer`: Radius of the small circles drawn at the labeled scale positions. Default: `3`
    * `FaceColor: TColor`: Color of the meter face (background color). Default: `clBtnFace`
    * `FaceOptions: TFaceOptions`: A set of options which can be used to turn elements of the meter control on or off. `TFaceOption = (foShowMargin, foShowCircles, foShowMainTicks, foShowSubTicks, foShowIndicatorMin, foShowIndicatorMid, foShowIndicatorMax, foShowValues, foShowCenter, foShowFrame, foShow3D, foShowCaption) `. Default: `[foShowMainTicks, foShowSubTicks, foShowIndicatorMax, foShowValues, foShowCenter, foShowFrame, foShow3D, foShowCaption]`
    * `IndMaximum: Integer`: Maximum value of the "normal" range. Default: `80`
    * `IndMinimum: Integer`: Minimum value of the "normal" range. Default: `20`
    * `LengthMainTicks: Integer`: Length of the main (labeled) ticks at the scale of the meter. Default: `15`
    * `LengthSubTicks: Integer`: Length of the minor (unlabeled) ticks at the scale of the meter. Default: `8`
    * `Margin: Integer`: Distance between the outermost parts of scale/center and the LCL bounds of the control. Default: `10`
    * `MarginColor: TColor`: Color of the rectangle drawn around the outermost parts of the scale if the option `foShowMargin` is included in the `FaceOptions`
    * `MaxColor: TColor`: Color of the segment at the meter scale between `IndMaximum` and `ScaleMax` to highlight the "higher-than-normal" range. Default: `clRed`
    * `MidColor: TColor`: Color of the center segment at the meter scale between `IndMinimum` and `IndMaximum` to highlight the "normal" range. Default: `clYellow`
    * `MinColor: TColor`: Color of the segment at the meter scale between `IndMinimum` and `ScaleMin` to highlight the "lower-than-normal" range. Default: `clGreen`
    * `NumberMainTicks: Integer`: Number of main (labeled) ticks drawn at the meter scale. Default: `5`
    * `Position: single`: Value for the needle position. Must be between `ScaleMin` and `ScaleMax`.
    * `ScaleMax: Integer`: Highest value which can be displayed by the meter. Default: `100`.
    * `ScaleMin: Integer`: Smallest value which can be displayed by the meter. Default: `0`.
    * `Style: TStyle = (agsLeftStyle, agsRightStyle, agsCenterStyle)`: Characterizes the orientation of the meter scale with respect to the center of the needle. Default: `agsCenterStyle` (the needle begins at the center of the scale). In option `agsLeftStyle`, the needle begins at the left side of the scale, in option `agsRightStyle`, at the right side of the scale.
    * `TextDist: Integer`: Distance between the tick labels and the ticks at the meter's scale. Default: `10`.
    * `TicksColor: TColor`: Color of the ticks drawn at the meter's scale. Default: `clBlack`
    * `ValueColor: TColor`: Color of the numbers at the main ticks. Default: `clBlack`


  * **Events**
    * `OnOverMax: TNotifyEvent`: Fires when the `Position` value is greater than the `IndMaximum` value.
    * `OnOverMin: TNotifyEvent`: Fires when the `Position` value is less than then `IndMinimum` value.


  * **Demo project**
    * In folder _Example/OnOffSwitch_Knob_Sample_
    * In folder _Example/A3nalogGaugeSample_



[![A3Gauge Demo.png](https://wiki.freepascal.org/images/0/06/A3Gauge_Demo.png)](</File:A3Gauge_Demo.png>)

#### TindGnouMeter

[![tindgnoumeter.png](https://wiki.freepascal.org/images/a/a1/tindgnoumeter.png)](</File:tindgnoumeter.png>) GnouMeter is a meter which can display an integer or a float value (Single). Just like a progress bar or a gauge, all you have do do is to define the Minimum and maximum values as well as the actual value. 

  * **Original author** : Jérôme Hersant (jhersant@post4.tele.dk)
  * **License** : _"THIS COMPONENT IS ENTIRELY FREEWARE."_
  * **Properties**
    * `Caption: String`: Name of the data represented by the meter
    * `Value: Single`: Value displayed
    * `Color: TColor`: Background color used when `Transparent` is `false`. Default: `clDefault`
    * `ColorFore: TColor`: Color of the bar. Default: `clRed`
    * `ColorBack: TColor`: Background color of the meter. Default: `clBtnFace`
    * `SignalUnit: ShortString`: Unit name displayed with the values (min, max and actual)
    * `ValueMin: Double`: Minimum value. Default: `0`
    * `ValueMax: Double`: Maximum value. Default: `100`
    * `Increment: Double`: Distance between each subdivision. Default: `10`
    * `ShowIncrements: Boolean`: Displays the subdivisions or not. Default: `true`
    * `Transparent: Boolean`: If `true`, the background of the entire control (`Color`) is not painted. Default: `true`
    * `GapTop: Word`: Distance between the meter and the text on top (Caption and actual value). Default: `20`
    * `GapBottom: Word`: Distance between the meter and the bottom of the control. Default: `10`
    * `BarThickness: Word`: Bar width. Default: `5`
    * `MarkerColor: TColor`: Color of the marker representing the actual value. Default: `clBlue`
    * `ShowMarker: Boolean`: Marker visible or not. Default: `true`


  * **Demo project**
    * The control is used in the general sample project _Ex_IndustrialStuff_ in folder _Example_



### Control Elements

#### TmKnob

[![tmknob.png](https://wiki.freepascal.org/images/b/b6/tmknob.png)](</File:tmknob.png>) Emulates the volume knob found on some HiFi devices. 

  * **Original author** : Marco Caselli (mcaselli@iname.com). Lazarus port and extended by Werner Pamler.
  * **License** : _"Feel free to use or give away this software as you see fit. Please leave the credits in place if you alter the source."_
  * **Properties**
    * `AllowUserDrag: Boolean`: If `true`, the user can drag the control to a new value by using the mouse.
    * `AngleRange: TKnobAngleRange = (arTop270, arTop180, arTop120, arTop90, arBottom270, arBottom180, arBottom120, arBottom90, arLeft270, arLeft180, arLeft120, arLeft90, arRight270, arRight180, arRight120, arRight90)`: Defines the sector of the full circle over which the knob can be rotated. Default: `arTop270`
    * `FaceColor: TColor`: Color of the knob's face
    * `MarkSize: Integer`: Size of the tick mark. Depending on the value of the `MarkSizeKind` property, the size is given either as percentage of the knob's enclosing rectangle size, or in pixels. Default: 20%
    * `MarkSizeKind: TKnobMarkSizeKind = (mskPercentage, mskPixels)`: Determines whether the knob's tick mark size is given as percentage or in pixels. Default: `mskPercentage`
    * `MarkStyle: TKnobMarkStyle = (msLine, msCircle, msTriangle)`: Specifies the style of the tick mark: line, circle, or triangle. Default: `msLine`
    * `Max: integer`: Upper limit value for `Position`. Default: `100`
    * `Min: integer`: Lower limit value for `Position`. Default: `0`
    * `Position: Integer`: Current position of the knob ("value")
    * `RotationEffect: Boolean`: If `true`, the knob will shake emulating a visual effect of rotation. Default: `false`
    * `Shadow: Boolean`: If `true`, a shadow is drawn in the `ShadowColor` at the bottom/right side of the knob.
    * `ShadowColor: TColor`: Color of the knob shadow.
    * `TickColor: TColor`: Color of the tick's mark
    * `Transparent: Boolean`: If `false`, the outer area of the control's client rectangle will be painted in the `Color` specified in the corresponding property, otherwise it is transparent.


  * **Events**
    * `OnChange: TKnobChangeEvent`: This event is triggered whenever the knob's `Position` is changed by user interaction or by code. Besides the `Sender` the event has the new `Position` value as a parameter.


  * **Demo**
    * Used, among other _industrial_ controls, in the project in the _Example/OnOffSwitch_Knob_Sample_ folder



[![OnOffSwitch Knob Sample.png](https://wiki.freepascal.org/images/4/42/OnOffSwitch_Knob_Sample.png)](</File:OnOffSwitch_Knob_Sample.png>)

#### TMultiSlider

[![tmultislider.png](https://wiki.freepascal.org/images/d/d3/tmultislider.png)](</File:tmultislider.png>) Provides adjustment of up to three values by sliding grips along a track. 

  * **Author** : forum user _bylaardt_ , see <https://forum.lazarus.freepascal.org/index.php/topic,45063.msg318180.html#msg318180>; extended by Werner Pamler
  * **License** : modified LGPL (with linking exception), like Lazarus LCL
  * **Properties**
    * `ColorAbove: TColor`: Color of the track above the upper slider. Default: `clInactiveCaption`
    * `ColorBelow: TColor`: Color of the track below the lower slider. Default: `clInactiveCaption`
    * `ColorBetween: TColor`: Color of the track between the upper and lower sliders. Default: `clActiveCaption`
    * `ColorThumb: TColor`: Color of the slider grip. Default: `clBtnFace`
    * `DefaultSize: Integer`: Size of the slider grip. Default: `28`
    * `Flat: Boolean`: If `true` 3D bevel effects are not painted. Default: `false`
    * `MaxPosition: Integer`: Defines the position of the upper slider. Default: `80`
    * `MinPosition: Integer`: Defines the position of the lower slider. Default: `20`
    * `Position: Integer`: Defines the position of the main (center) slider. When all three sliders are used the center slider can only be moved between the lower and upper sliders. Default: `50`
    * `RangeMax: Integer`: Maximum allowable position of any slider. Default: `100`
    * `RangeMin: Integer`: Minimum allowable position of any slider. Default: `0`
    * `SliderMode: TSliderMode = (smSingle, smMinMax, smMinValueMax)`: Defines how many sliders are used: `smSingle` \- only the center slider, `smMinMax` \- only the lower and upper sliders, `smMinValueMax` \- all three sliders. Default: `smMinMax`
    * `ThumbStyle: TThumbStyle = (tsGrip, tsCircle, tsRect, tsRoundedRect, tsTriangle, tsTriangleOtherSide)`: Defines the shape of the slider grip. Default: `tsGrip`
    * `TrackThickness: integer`: Width of the slider track. Default: `7`
    * `Vertical: boolean`: Defines whether the slider is aligned vertically (`true`) or horizontally (`false`). Default: `false`


  * **Events**
    * `OnPositionChange: TSliderPositionEvent = procedure (Sender: TObject; AKind: TThumbKind; AValue: Integer) of object`: Fires whenever the position of a slider or the allowed max/min values changes. The slider affected is given by the parameter `AKind: TThumbKind = (tkMin, tkMax, tkValue)`. The new position of the slider is passed as parameter `AValue`.


  * **Demo Project**
    * In folder _Example/MultiSliderSample_



[![MultiSliderDemo.png](https://wiki.freepascal.org/images/2/2a/MultiSliderDemo.png)](</File:MultiSliderDemo.png>)

### Switches

#### TOnOffSwitch

[![tonoffswitch.png](https://wiki.freepascal.org/images/0/0b/tonoffswitch.png)](</File:tonoffswitch.png>) A control resembling an on/off switch which is activated by sliding a button to the other side or by pressing the SPACE bar. The state of the button is displayed as text. When the control's width is greater than its height the switch is painted in horizontal, otherwise in vertical orientation. 

  * **Author** : Werner Pamler
  * **License** : LGPL with linking exception (like Lazarus LCL).
  * **Properties**
    * `BorderColor: TColor`: Color of the control's border when `BorderStyle` is `bsThin` or `bsThick`. Default: `clGray`
    * `BorderStyle: TSwitchBorderStyle = (bsNone, bsThin, bsThick, bsThin3D, bsThick3D)`: Defines the style of the control's border. Default: `bsThin`
    * `ButtonSize: Integer`: Size of the button slider. Default: `24`
    * `CaptionOFF: String`: This text is displayed inside the switch area when the switch is "off" (`Checked = false`). Default: `'OFF'`
    * `CaptionON: String`: This text is displayed inside the switch area when the switch is "on" (`Checked = true`). Default: `'ON'`
    * `Checked: Boolean`: Indicates the "state" of the switch. When `Checked` is `true` the switch is "on", otherwise it is "off". Default: `false`
    * `Color: TColor`: Background color of the switch. Default: `clWindow`
    * `ColorOFF: TColor`: Color of the slider button when `Checked` is `false`. Default: `clMaroon`
    * `ColorON: TColor`: Color of the slider button when `Checked` is `true`.Default: `clGreen`
    * `Inverse: Boolean`: Flips the location of the button slider and the caption. Default: `false` (Slider at the left, caption at the right).
    * `ReadOnly: Boolean`: If `true`, the user cannot change the switch state. Default: `false`
    * `ShowButtonBorder: Boolean`: Allows to show/hide the slider's border. Default: `true`
    * `ShowCaption: Boolean`: Allows to show/hide the control's `Caption`. Default `true`
    * `Orientation: TSwitchOrientation = (soHorizontal, soVertical)`: Orientation of the switch, horizontal or vertical. Note that this is a read-only property, its value is determined by the control's `Width`-to-`Height` ratio.


  * **Events**
    * `OnChange: TNotifyEventy`: Fires whenever the `Checked` property changes its value, either by user interaction or by code.


  * **Sample Project**
    * In folder _Example/OnOffSwitch_Knob_Sample_.

---

_Source: [https://wiki.freepascal.org/Industrial](https://web.archive.org/web/20250113225306/https://wiki.freepascal.org/Industrial)_
