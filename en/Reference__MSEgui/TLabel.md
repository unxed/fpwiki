# Reference: MSEgui/TLabel

## Contents

  * 1 TLabel
    * 1.1 Alternatives
    * 1.2 Reference
      * 1.2.1 Hierarchy
      * 1.2.2 Properties
        * 1.2.2.1 inherited from TWidget
        * 1.2.2.2 Caption
        * 1.2.2.3 Options
        * 1.2.2.4 TextFlags
      * 1.2.3 Methods
      * 1.2.4 Events
        * 1.2.4.1 Inherited from TWidget
    * 1.3 Known issues
      * 1.3.1 Issues of the class
      * 1.3.2 Issues of this documentation



# TLabel

**ready for revision**

TLabel shows some Text on the form. 

[![msegui TLabel.png](https://wiki.freepascal.org/images/3/33/msegui_TLabel.png)](</File:msegui_TLabel.png>)

### Alternatives

  * You can use TDispWidget successor like TStringDisp
  * If you want display text next to a widget, you can use TFrame.Caption of this component.



## Reference

### Hierarchy

  * TObject
  * TPersistent
  * TComponent
  * [TMseComponent](<TMseComponent.md> "Reference: MSEgui/TMseComponent")
  * TActComponent
  * [TWidget](<TWidget.md> "Reference: MSEgui/TWidget")
  * TActionWidget
  * TActionPublishedWidgetNwr
  * TPublishedWidgetNwr
  * TPublishedWidget
  * TCustomLabel
  * TLabel



### Properties

#### inherited from TWidget

[Anchors](<TWidget.md> "Reference: MSEgui/TWidget")

[Bounds](<TWidget.md> "Reference: MSEgui/TWidget")

[Color](<TWidget.md> "Reference: MSEgui/TWidget")

[Cursor](<TWidget.md> "Reference: MSEgui/TWidget")

[Enabled](<TWidget.md> "Reference: MSEgui/TWidget")

[Face](<TFace.md> "Reference: MSEgui/TFace")

[Font](<TFont.md> "Reference: MSEgui/TFont")

[Frame](<TFrame.md> "Reference: MSEgui/TFrame")

[Hint](<TWidget.md> "Reference: MSEgui/TWidget")

[OptionsWidget](<TWidget.md> "Reference: MSEgui/TWidget")

[OptionsWidget1](<TWidget.md> "Reference: MSEgui/TWidget")

[PopupMenu](<TWidget.md> "Reference: MSEgui/TWidget")

[TabOrder](<TWidget.md> "Reference: MSEgui/TWidget")

[Visible](<TWidget.md> "Reference: MSEgui/TWidget")

#### Caption

The Caption is the text the label displays. 
    
    
    {$ifdef mse_unicodestring}
      msestring = unicodestring;
    {$else}
      msestring = widestring;
    {$endif}
    
    property caption: msestring;
    

#### Options

With the option lao_nogray you can prevent a label to be shown grayed if itself or a parent widget is disabled. 
    
    
    labeloptionty = (lao_nogray,lao_nounderline);
    labeloptionsty = set of labeloptionty;
    
    property Options: labeloptionsty default [];
    

If you use a & char in a caption, the following char gets underlined. To prevent this use lao_nounderline: 

[![msegui label options.png](https://wiki.freepascal.org/images/7/7d/msegui_label_options.png)](</File:msegui_label_options.png>)

  


#### TextFlags

With TextFlags you can specify things like alignment or rotation. See the following image and the image at [CaptionTextFlags](<TFrame.md> "Reference: MSEgui/TFrame")
    
    
    textflagty = (tf_xcentered, tf_right, tf_xjustify, tf_ycentered, tf_bottom, 
                   tf_rotate90, tf_rotate180,
                   tf_clipi, tf_clipo,
                   tf_grayed, tf_wordbreak, tf_softhyphen,
                   tf_noselect, tf_underlineselect,
                   tf_ellipseleft, tf_ellipseright,
                   tf_tabtospace, tf_showtabs,
                   tf_force);
    textflagsty = set of textflagty;
    
    property TextFlags: textflagsty default defaultlabeltextflags;
    

[![msegui label textflags.png](https://wiki.freepascal.org/images/7/74/msegui_label_textflags.png)](</File:msegui_label_textflags.png>)

### Methods

### Events

#### Inherited from TWidget

[OnHint](<TWidget.md> "Reference: MSEgui/TWidget")

[OnEnter, OnFocus, OnActivate](<TWidget.md> "Reference: MSEgui/TWidget")

[OnNavigRequest](<TWidget.md> "Reference: MSEgui/TWidget")

[OnPopup](<TWidget.md> "Reference: MSEgui/TWidget")

[OnPaint, OnPaintBackground, OnAfterPaint, OnPaintBackground](<TWidget.md> "Reference: MSEgui/TWidget")

Inherited from TMseComponent: [OnBeforeUpdateSkin, OnAfterUpdateSkin](<TMseComponent.md> "Reference: MSEgui/TMseComponent")

## Known issues

### Issues of the class

### Issues of this documentation

Feel free to add your points here.

---

_Source: [https://wiki.freepascal.org/Reference%3A_MSEgui/TLabel](https://web.archive.org/web/20210922134631/https://wiki.freepascal.org/Reference%3A_MSEgui/TLabel)_
