# Reference: MSEgui

│ **English (en)** │

## Contents

  * 1 Widgets
    * 1.1 TSimpleWidget
    * 1.2 TMainMenuWidget
    * 1.3 TSimpleWidget
    * 1.4 TMseFormWidget
    * 1.5 TDockFormWidget
    * 1.6 TPaintbox
    * 1.7 TEventWidget
    * 1.8 TButton
    * 1.9 TStockGlyphButton
    * 1.10 TRichButton
    * 1.11 TLabel
    * 1.12 TGroupBox
    * 1.13 TStepBox
    * 1.14 TStringDisp
    * 1.15 TByteStringDisp
    * 1.16 TIntegerDisp
    * 1.17 TRealDisp
    * 1.18 TDateTimeDisp
    * 1.19 TBooleanDisp
    * 1.20 TToolBar
    * 1.21 TDrawGrid
    * 1.22 TStringGrid
    * 1.23 TTabBar
    * 1.24 TTabPage
    * 1.25 TTabWidget
    * 1.26 TDockHandle
    * 1.27 TDockPanel
    * 1.28 TSpliter
    * 1.29 TSpacer
    * 1.30 TLayouter
    * 1.31 TListView
    * 1.32 TImage
    * 1.33 TDial
    * 1.34 TChart
    * 1.35 TChartRecorder
    * 1.36 TPolygon
    * 1.37 TPickWidget
    * 1.38 TOpenglWidget
  * 2 Edit
    * 2.1 TStringEdit
    * 2.2 TMemoEdit
    * 2.3 THexStringEdit
    * 2.4 TDropdownListEdit
    * 2.5 THistoryEdit
    * 2.6 TIntegerEdit
    * 2.7 TKeyStringEdit
    * 2.8 TEnumEdit
    * 2.9 TEnumTypeEdit
    * 2.10 TSelector
    * 2.11 TRealEdit
    * 2.12 TRealSpinEdit
    * 2.13 TDateTimeEdit
    * 2.14 TCalendarDateTimeEdit
    * 2.15 TEdit
    * 2.16 TWidgetGrid
    * 2.17 TItemEdit
    * 2.18 TDropDownItemEdit
    * 2.19 TMBDropDownItemEdit
    * 2.20 TTreeItemEdit
    * 2.21 TRecordFieldEdit
    * 2.22 TDialogStringEdit
    * 2.23 TPointerEdit
    * 2.24 TSlider
    * 2.25 TProgressBar
    * 2.26 TBooleanEdit
    * 2.27 TBooleanEditRadio
    * 2.28 TDataButton
    * 2.29 TStockGlyphDataButton
    * 2.30 TDataIcon
    * 2.31 TTextEdit
    * 2.32 TDataImage
    * 2.33 TTerminal
  * 3 NoGui
    * 3.1 TActivator
    * 3.2 TThreadComp
    * 3.3 TStatFile
    * 3.4 TTimer
    * 3.5 TNoGuiAction
    * 3.6 TPipeReadercomp
    * 3.7 TSysEnvManager
    * 3.8 TProcessMonitor
    * 3.9 TFilechangeNotifier
  * 4 Gui
    * 4.1 TFaceList
    * 4.2 TFrameComp
    * 4.3 TFaceComp
    * 4.4 TBitmapComp
    * 4.5 TImageList
    * 4.6 TPopupMenu
    * 4.7 TMainMenu
    * 4.8 TAction
    * 4.9 TShortCutController
    * 4.10 TPostscriptPrinter
    * 4.11 TGdiPrinter
    * 4.12 TWmfPrinter
    * 4.13 TSkinController
    * 4.14 TGuiThreadComp
  * 5 Dialog
    * 5.1 TFileListview
    * 5.2 TFileDialog
    * 5.3 TFaceComp
    * 5.4 TFileNameEdit
    * 5.5 TDirDropdownEdit
    * 5.6 TColorEdit
    * 5.7 TMemoDialogEdit
    * 5.8 TPageSizeSelector
    * 5.9 TPageOrientationSelector
  * 6 DB
  * 7 = DBedit
  * 8 = DBfields
  * 9 = Report
    * 9.1 TRepSpacer
    * 9.2 TRecordBand
    * 9.3 TrepValueDisp
    * 9.4 TRepPageNumdisp
    * 9.5 TRepPrintDateDisp
    * 9.6 TBandGroup
    * 9.7 TTileArea
  * 10 = Design
    * 10.1 TGdbMi
    * 10.2 TSyntaxEdit
    * 10.3 TSyntaxPainter
    * 10.4 Comm
    * 10.5 TCommPort
    * 10.6 TAsciiCommPort
    * 10.7 TAsciiProtPort
    * 10.8 TCommSelector



## Widgets

### TSimpleWidget

see also [Reference:_MSEgui/TSimpleWidget](<Reference__MSEgui/TSimpleWidget.md> "Reference: MSEgui/TSimpleWidget")

### TMainMenuWidget

### TSimpleWidget

### TMseFormWidget

Use it in order to insert a tcustomform descendant into another widget at designtime. Does not try to load resources in "create". 

### TDockFormWidget

### TPaintbox
    
    
    How to draw line (or circle) on tpaintbox? 
    In event onpaint: 
    procedure tmainfo.paintboxonpaint(const sender: twidget; const canvas: tcanvas); 
    begin 
     with sender,canvas do begin 
      drawline(nullpoint,pointty(size),cl_yellow);  
      //diagonal line across widget 
      drawellipse(makerect(makepoint(bounds_cx div 2,bounds_cy div 2), size),cl_red); 
      //circle (or ellipse) centered in widget                             
     end; 
    end; 
    Makepoint and makerect are in msegraphutils.
    

### TEventWidget

A widget which publishes all possible events of a twidget. Normally it is better to implement your own specialized descendant of an existing widget instead to use teventwidget. 

### TButton

  * A rectangular clickable area that can show text/bitmap.



\- Main properties: 
    
    
    Caption: read/write the text that appear on top of it.
    onexecute: read/write the address of a procedure (event handler) to be executed when clicked.
    

### TStockGlyphButton

### TRichButton

### TLabel

  * Draws a piece of text on the given surface (canvas: screen/printer/bitmap).



\- Main properties: 
    
    
    Caption: read/write the piece of text.
    

See also [TLabel](<Reference__MSEgui/TLabel.md> "Reference: MSEgui/TLabel")

### TGroupBox

### TStepBox

### TStringDisp

  * A read only version of TStringEdit, difference from TLabel: has a frame around it.



-Main properties: 
    
    
    Value: read/write the text that are showed.
    Caption: A label normally describing the purpose or meaning of the presented text, it can be positioned around the frame.
    

### TByteStringDisp

### TIntegerDisp

### TRealDisp

### TDateTimeDisp

### TBooleanDisp

### TToolBar

### TDrawGrid

### TStringGrid

### TTabBar

### TTabPage

### TTabWidget

### TDockHandle

### TDockPanel

### TSpliter
    
    
    A widget very similar to "tspacer" but :
    - designed to rearrange areas occupied by adjacent widgets
    - a linked widget may only enlarge by "eating" the opposite one, so the summary area of both widgets don't change
    - has GUI look ( hatched grip, color etc) switched on by default
    - facilitates run-time repositioning oneself and linked widgets
    - linked widgets may even be other splitters, spacers (with their linked widgets ),..
    

### TSpacer
    
    
    * a regular widget which creates a kind of positional link between surrounding widgets
    * designed to maintain distances between widgets
    * may have GUI look, caption etc switched off by default
    * resizing a spacer repositions its linked widgets
    

### TLayouter

  * a tspacer descendant designed to (auto)resize or/and move its contained widgets acc to some size/positon dependencies
  * may have GUI look, frame caption etc switched off by default
  * layouters may be nested to achieve copmplex layouts



Each layout change/assignment is divided into performing 2 consequent stages : 

Stage 1: 

Widgets may be auto resized in 5 consequent steps using the following options: \- if plo_syncmaxautosize in place_options : = all widgets are autosized then their client areas are syncronized to the clientareas of the highest and the widest of the widget 

  * calls "msegui.syncmaxautosize"



\- if plo_syncpaintwidth in place_options : = the paintwidths of all widgets are synchronized to the widget with the widest outer frame width ( ex. width of "frame.caption" ) 

  * mainly makes sense if "lao_alignx" set and {align_glue = wam_start or wam_end} ( see below ) when the widgets will be adjusted in order to fit into the inner client width of tlayouter:



x-aligh level V 
    
    
          			| Widget_1 the_widest_frame_caption|
          			| Widget_2 frame_caption2          |
          			| Widget_N wider_frame_captionN    |
    
    
    
     here, the effect is shown for "cp_right" frame captions
     // otherwise syncronizes to the outer ( of the frame except its caption ) width 
    // of the Z-top widget
    * calls "msegui.syncpaintwidth"
    * paintwidth is the outer width
    - if plo_syncpaintheight in place_options :
       = the paintheights of all widgets are synchronized to the widget with the highest outer frame width ( ex. width of "frame.caption" ).
    * mainly makes sense if lao_aligny set and {align_glue = wam_start or wam_end} ( see below ) the widgets will be adjusted in order to fit into the inner client height of tlayouter :
    

+------------------------------+ 
    
    
          			| The_                         |
          			| tallest_            taller_  |
          			| frame_    frame_    frame_   |
          			| caption   caption2  captionN |  
          			|                              |
    

| Widget1 Widget_2 Widget_N |<== y-align level 
    
    
    here, the effect is shown for "cp_topleft" frame captions
    // otherwise syncronizes to the outer ( of the frame except its caption ) 
    // height of the Z-top widget
    * calls "msegui.syncpaintheight"
    - plo_synccaptiondistx in place_options :
      = causes all widgets to have the widest common room for their cp_(left/right)* frame captions
    * calls "msegui.synccaptiondistx"
    - plo_synccaptiondisty in place_options :
      = causes all widgets to have the highest common room for their cp_(top/bottom)* frame captions
    * calls "msegui.synccaptiondisty"
    
    
    
    Stage 2: the widgets may be (re)arranged within the layouter 
    There're 2 modes of such (re)arrangement which can be partially ( orthogonally ) combined ( see later ):
      1) the place(ment) mode ( lao_place* in optionslayout ) :
    

\- widgets are placed at some distances between each other, possibly with some margins, rooms of invisible widgets ( having visible=false) are also allocated unless "plo_noinvisible in place_options" 
    
    
    * the widgets are placed in the order of decreasing their "widgetrect.x" coordinates before alignment
    * the inter-widget distances and the side margins ( if apllied ) in both dimentions are identical and limited between "place_mindist" and "place_maxdist"
     = if {lao_placex in optionslayout} and {place_mode <> wam_none} then the following relevant settings aplly :
    

* non-limiting value of "place_maxdist" : # |Widget_1------Widget_2------Widget_3| 
    
    
           * non-limiting value of "place_maxdist" and 
    

{plo_propmargin in place_options} : # |---Widget_1---Widget_2---Widget_3---| * limiting value of "place_maxdist" and {place_mode = wam_start} : # |Widget_1----Widget_2----Widget_3????| * limiting value of "place_maxdist" and {place_mode = wam_start} and {plo_propmargin in place_options} : # |---Widget_1---Widget_2---Widget_3???| * limiting value of "place_maxdist" and 
    
    
           {place_mode = wam_end} :
    

# |??????Widget_1---Widget_2---Widget_3| * limiting value of "place_maxdist" and {place_mode = wam_end} and {plo_propmargin in place_options} : # |???Widget_1---Widget_2---Widget_3---| * limiting value of "place_maxdist" and {place_mode = wam_center} : # |???Widget_1---Widget_2---Widget_3???| * limiting value of "place_maxdist" and {plo_endmargin in place_options} : # |Widget_1----Widget_2----Widget_____3|, or # |Widget_1----Widget_____2----Widget_3|, or # |Widget_____1----Widget_2----Widget_3|, here, the most left amongst widgets having both [an_left,an_right] set is expanded otherwise the most right widget ( Widget_3 in the example ) * limiting value of "place_maxdist" and {place_mode = wam_end} and {plo_propmargin in place_options} and {plo_endmargin in place_options} : # |--Widget_1--Widget_____2--Widget_3--|, The legend: limiting value of "place_maxdist" : such value which produce some visuall effect on the ayouter "----" : distance ( = number of minuses, limited by place_maxdis ) "????" : some remaining space ( = number of questmarks ) "Widget_1" : widget of the original size "Widget__..__1" : (auto)resized widget = if {lao_placey in optionslayout} and {place_mode <> wam_none} then the things are handled in the same manner as with "lao_placex" but for the vertical "top2bottom" direction of placement instead of the horizontal "left2right" one 2) the align(ment) mode ( optionslayout.lao_align* ) : \- widgets are gathered into a visual group to a dedicated "leader" widget of the layout ( set by "align_leader" and defaults to the lowest in Z-Order = twidget.widgets[0] ) the leader stays in place while the others : = if lao_alignx in optionslayout ( the hor alignment mode ): * if align_mode = wam_start : snap their left borders to the left border of leader * else if align_mode = wam_end : snap their right borders to the right border of leader * else if align_mode = wam_center : snap their v-axes to the v-axis of leader after that, = if lao_aligny in optionslayout ( the vert alignment mode ): * if align_mode = wam_start : snap their top borders to the top border of leader * else if align_mode = wam_end : snap their bottom borders to the bottom border of leader * else if align_mode = wam_center : snap their h-axes to the h-axis of leader 
    
    
           - after that, the whole widget group can be aligned within the layouter:
    

= if align_glue = wam_start * if lao_alignx in optionslayout: the left extent of group snaps to the left border of layouter * if lao_aligny in optionslayout: the top extent of group snaps to the top border of layouter = else if align_glue = wam_end * if lao_alignx in optionslayout: the right extent of group snaps to the right border of layouter * if lao_aligny in optionslayout: the bottom extent of group snaps to the bottom border of layouter = else if align_glue = wam_center * if lao_alignx in optionslayout: the v-axis of group snaps to the v-axis of layouter * if lao_aligny in optionslayout: the h-axis of group snaps to the h-axis of layouter 
    
    
    Mutually exclusive settings:
    

* only one of "align_mode" can be choosen * only one of "glue_mode" can be choosen * "optionslayout.lao_alignx" & "optionslayout.lao_placex" * "optionslayout.lao_aligny" & "optionslayout.lao_placey" 
    
    
    V-alignment ( optionslayout.lao_aligny ) may be combined with h-placement ( optionslayout.lao_placex ), and h-alignment ( optionslayout.lao_alignx ) may be combined with v-placement ( optionslayout.lao_placey )
    

!!! The effects of the above described { resizing / placement / alignment } are irreversible. So, the only way to revert is to set "wan_none" then to revert manually !!! 

### TListView

### TImage

### TDial

### TChart

### TChartRecorder

### TPolygon

### TPickWidget

### TOpenglWidget

## Edit

### TStringEdit

### TMemoEdit

### THexStringEdit

### TDropdownListEdit
    
    
    A tstringedit with a dropdownlist to choose text values. Important dropdown.options members:
    - deo_autodropdown dropdown on keypress
    - deo_selectonly don't allow entering arbitrary text.
    - deo_forceselect don't allow entering empty text.
    

### THistoryEdit
    
    
    A tstringedit which shows the previously entered values in a dropdownlist for selection.
    

### TIntegerEdit

### TKeyStringEdit
    
    
    Maps string to string.
    

### TEnumEdit
    
    
    Maps integer to string, zero based and sequencial (first item 0, next 1, ...).
    

### TEnumTypeEdit

A tenumedit which maps Pascal enums to their names. Use oninit to store the typeinfo pointer of the enum type into sender.typeinfopo. 

### TSelector

TSelector is the most specialized widget of the dropdown editwidget group, it is based on tenumedit (tenumedit maps an integer to a string) and uses for the dropdownlist a second map which must be created on the fly in ongetdropdowninfo. An example is tcommselector where the enumedit maps commnrty to commname and the dropdownlist shows the available RS232 ports only. 

### TRealEdit

### TRealSpinEdit

### TDateTimeEdit

### TCalendarDateTimeEdit

### TEdit

MSEgui counterpart of Delphi TEdit. You will never use it. 

### TWidgetGrid

### TItemEdit

### TDropDownItemEdit
    
    
    A tstringedit with a dropdownlist to choose text values. Important dropdown.options members:
    - deo_autodropdown dropdown on keypress
    - deo_selectonly don't allow entering arbitrary text.
    - deo_forceselect don't allow entering empty text.
    

### TMBDropDownItemEdit

### TTreeItemEdit

### TRecordFieldEdit
    
    
    Used in twidgetgrid in order to edit fields of a ttreeitemedit. Example is MSEide projecttreeform.pas.
    

### TDialogStringEdit

A tstringedit with an ellipse button. Use "onexecute" to show the dialog. 

### TPointerEdit

### TSlider

### TProgressBar

### TBooleanEdit

### TBooleanEditRadio

### TDataButton

A button with an integer value. Clicking increments the value until "max", then it restarts with "min". Can be inserted into a twidgetgrid. The current value selects the showed image and face by the items of "imagenums" and "valuefaces". 

### TStockGlyphDataButton

### TDataIcon

Shows an imagelist item by lookup from "value" to "imagenums". Clicking increments value until "max" then it restarts with "min". Can be inserted into a twidgetgrid. 

### TTextEdit

Only useful if inserted into a twidgetgrid, builds a text editor, used in MSEide source editor. 

### TDataImage

A pixmap display widget which can be inserted into twidgetgrid. 

### TTerminal

Only useful if inserted into a twidgetgrid, builds a very simple terminal emulator. Used in MSEide target console. 

## NoGui

### TActivator

### TThreadComp

### TStatFile

\- so that to be in effect, it should also be assigned to the form where the widget using the stafile is placed on \- in design, if "onstatwrite" is set and "filedir" is not yet created, deactivate exception "ECreateError" in project settings ( "Debugger" tab ) \- "filedir" may contain "~/" indicating the user's home directory \- options "oe_savestate" & "oe_savevalue" of "client" widgets define what to store to the file \- position etc changes or/and value changes \- in case when a main form shares its stafile with non-main forms, on creating non-main ones, just edited not saved data of the main form ( bound to vars of the statfile) are reset to values read from the statfile upon creating the form; for "sfo_memory", this effect absents unless widgets on the concurring forms share same variable[s]; to avoid this behaviour, disable "fo_autoreadstat" & "fo_autowritestat" of the non-main forms \- each "tstafile" owns: 
    
    
    = tstatwriter:
    

  * provides methods of writing sections & statvars to a memory/file stream



\- tstatreader: 

  * holds list of sections with statvars each
  * provides search & check & reading interface to the statvars
  * provides reading statvars from a memory/file stream



Positioning to a section speeds up accessing its statvars 

\- there also is "tstatfiler" ( exposed by some "tstatfile" events ) which: = may present or "tstatwriter" or "tstatreader" ( there's a check method ) = provides directionless "update" methods with internal switch to needed direction of processing \- "reading" or "writing" statvars on per-section basis 

### TTimer

### TNoGuiAction

### TPipeReadercomp

### TSysEnvManager

### TProcessMonitor

### TFilechangeNotifier

## Gui

### TFaceList

### TFrameComp
    
    
    Terminilogy :
    
    
    
    client area = area of the widget which interacts with a user
    
    
    
    bevelling = additional facets rising/sinking frame & client area, 
    constists of two parts 
    - external: between frame and widget
    - internal: between frame and client area
    
    
    
    frame= flat space between external & internal facets,	floats at the inner level of the external face
    
    
    
    * Both frame & bevelling affect the client area ***
    

### TFaceComp
    
    
    - doesn't affect the widget frame but client area of the frame
    

### TBitmapComp

### TImageList

### TPopupMenu

### TMainMenu

### TAction
    
    
    Shortcut processing order :
    
    
    
    - the smallest piece of processing is "doshortcut" procedure which 
    is called until processed:
    = starting from the sender up to the toplevel widget
    = then by all child widgets with non-set "ow_noparentshortcut" 
    = then, if "ow_nochildshortcut" isn't set, by the parent widget
    = then by the widget oneself
    
    
    
    - "doshortcut" is checked in the following order:
    = starting from form's main menu
    = then from the owning window ( the widget oneself ) 
    = then from the application
    
    
    
    * A shortcut is bound to a widget by :
    - placing an action component on the widget ***
    - direct assigning the shortcut to the widget (menus,..)
    

### TShortCutController

### TPostscriptPrinter

### TGdiPrinter

### TWmfPrinter

### TSkinController

### TGuiThreadComp

## Dialog

### TFileListview

### TFileDialog

### TFaceComp

### TFileNameEdit

### TDirDropdownEdit

### TColorEdit

### TMemoDialogEdit

### TPageSizeSelector

### TPageOrientationSelector

## DB

## = DBedit

## = DBfields

## = Report

### TRepSpacer

### TRecordBand

### TrepValueDisp

### TRepPageNumdisp

### TRepPrintDateDisp

### TBandGroup

### TTileArea

## = Design

### TGdbMi

### TSyntaxEdit

### TSyntaxPainter

### Comm

### TCommPort

### TAsciiCommPort

### TAsciiProtPort

### TCommSelector

---

_Source: [https://wiki.freepascal.org/Reference%3A_MSEgui](https://web.archive.org/web/20230322073410/https://wiki.freepascal.org/Reference%3A_MSEgui)_
