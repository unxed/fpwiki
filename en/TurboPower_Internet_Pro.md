# TurboPower Internet Pro

[Template:TurboPower Internet Pro](</index.php?title=Template:TurboPower_Internet_Pro&action=edit&redlink=1> "Template:TurboPower Internet Pro \(page does not exist\)")

## Contents

  * 1 About
  * 2 Layouts
  * 3 CSS
  * 4 Development information
    * 4.1 About
    * 4.2 Units
    * 4.3 Parsing
      * 4.3.1 Parse tree classes hierarchy
    * 4.4 Classes
      * 4.4.1 TIpNodeBlockLayouter
    * 4.5 Enqueue
    * 4.6 Layout
    * 4.7 Render



### About

Lazarus provides the package TurboPowerIPro (lazarus/components/turbopower_ipro/turbopoweripro.lpk) with the following features: 

  * It contains a visual control to put onto a form. When you install the package in the IDE, you get some new components in the palette, so you can drop them onto a form just like any LCL control.
  * It is written completely in Pascal and therefore works on all platforms out of the box without any extra installation.
  * You have the full control, what files/urls are opened.
  * It does not have all the features of a full webbrowser. No multimedia stuff, javascript or flash. This must be implemented by you.



### Layouts

  * Flow Layout (Normal Layout) - [![Checkmark.png](https://upload.wikimedia.org/wikipedia/commons/5/5a/Checkmark.png)](</File:Checkmark.png>)
  * Flexbox Layout - not supported
  * Grid layout - not supported



### CSS

  * width - [![Checkmark.png](https://upload.wikimedia.org/wikipedia/commons/5/5a/Checkmark.png)](</File:Checkmark.png>)
  * padding - is not supported
  * background - supports only color argument. image, image repeat, attachment, position are not supported
  * -border
  * border - [![Checkmark.png](https://upload.wikimedia.org/wikipedia/commons/5/5a/Checkmark.png)](</File:Checkmark.png>)
  * border-bottom - not supported
  * border-width - [![Checkmark.png](https://upload.wikimedia.org/wikipedia/commons/5/5a/Checkmark.png)](</File:Checkmark.png>)
  * border-color - [![Checkmark.png](https://upload.wikimedia.org/wikipedia/commons/5/5a/Checkmark.png)](</File:Checkmark.png>)
  * border-style - [![Checkmark.png](https://upload.wikimedia.org/wikipedia/commons/5/5a/Checkmark.png)](</File:Checkmark.png>)
  * height - not supported



### Development information

#### About

  * Parsing the html document is important stage where a tree of nodes is generated, which represents the html document
  * Rendering - in this stage the controls are ordered and painted



#### Units

  * IpHtml -
  * IpHtmlNodes - contains classes for Tree structure representing the html document (like DOM). Some of the nodes classes are in IpHtml
  * IpHtmlParser - contains TIpHtmlParser class which parses the html document and generates tree.
  * IpCSS - Classes related to CSS. Classes for poperties, also parsing related routines,



#### Parsing

Important methods in TIpHtmlParser PARSE 

  * Create - AOwner: TIpHtml; - The result of parsing goes to AOwner. AStream: TStream - Here is the full html content to be parsed.


  * Execute -


  * ParseHtml


  * ParseBody


  * ParseInline


  * ParseBodyText - parses the string content between the open and end tag - 
        
        <div>This is what is parsed</div>




It could be just text or other html elements inside the tag 

  * ParseBlock


  * ParseBaseProps - Parses the attributes of a Tag - 
        
        <div width="150px">




##### Parse tree classes hierarchy

Here you can see hierarchy of classes of nodes for parse tree 

**TIpHtmlNode** \- The base class for all HTML nodes 

  * **TIpHtmlNodeNv** \- Base class for all Non-visual nodes 
    * **TIpHtmlNodeTITLE**
    * **TIpHtmlNodeMETA**
    * **TIpHtmlNodePARAM**
    * **TIpHtmlNodeSCRIPT**


  * **TIpHtmlNodeMulti**
    * **TIpHtmlNodeCore** \- Introduces id, class, style ... attributes 
      * **TIpHtmlNodeInline**
        * **TIpHtmlNodeAlignInline**
          * **TIpHtmlNodeControl**
            * **TIpHtmlNodeIFRAME**
            * **TIpHtmlNodeBUTTON**
            * **TIpHtmlNodeINPUT**
            * **TIpHtmlNodeSELECT**
            * **TIpHtmlNodeTEXTAREA**
          * **TIpHtmlNodeHR**
          * **TIpHtmlNodeIMG**
          * **TIpHtmlNodeLI**
          * **TIpHtmlNodeTABLE**
        * **TIpHtmlNodeHeader**
        * **TIpHtmlNodeP**
        * **TIpHtmlNodeA**
        * **TIpHtmlNodeADDRESS**
        * **TIpHtmlNodeAPPLET**
        * **TIpHtmlNodeBLINK**
        * **TIpHtmlNodeBLOCKQUOTE**
        * **TIpHtmlNodeBR**
        * **TIpHtmlNodeDD**
        * **TIpHtmlNodeDIV**
        * **TIpHtmlNodeDL**
        * **TIpHtmlNodeDT**
        * **TIpHtmlNodeFORM**
        * **TIpHtmlNodeGenInline**
          * **TIpHtmlNodeBASEFONT**
          * **TIpHtmlNodeDEL**
          * **TIpHtmlNodeFONT**
          * **TIpHtmlNodeFontStyle**
          * **TIpHtmlNodeINS**
          * **TIpHtmlNodeNOBR**
          * **TIpHtmlNodePhrase**
          * **TIpHtmlNodeSPAN**
        * **TIpHtmlNodeLABEL**
        * **TIpHtmlNodeList**
          * **TIpHtmlNodeUL**
          * **TIpHtmlNodeDIR**
          * **TIpHtmlNodeMENU**
        * **TIpHtmlNodeNOSCRIPT**
        * **TIpHtmlNodeOBJECT**
        * **TIpHtmlNodePRE**
        * **TIpHtmlNodeQ**
        * **TIpHtmlNodeOL**
      * **TIpHtmlNodeBlock**
        * **TIpHtmlNodeBODY**
        * **TIpHtmlNodeCAPTION**
        * **TIpHtmlNodeTableHeaderOrCell**
          * **TIpHtmlNodeTH**
          * **TIpHtmlNodeTD**
      * **TIpHtmlNodeFRAMESET**
      * **TIpHtmlNodeAREA**
      * **TIpHtmlNodeCOL**
      * **TIpHtmlNodeCOLGROUP**
      * **TIpHtmlNodeFIELDSET**
      * **TIpHtmlNodeFRAME**
      * **TIpHtmlNodeLEGEND**
      * **TIpHtmlNodeLINK**
      * **TIpHtmlNodeMAP**
      * **TIpHtmlNodeNOFRAMES**
      * **TIpHtmlNodeOPTGROUP**
      * **TIpHtmlNodeOPTION**
      * **TIpHtmlNodeTHeadFootBody**
        * **TIpHtmlNodeTBODY**
        * **TIpHtmlNodeTFOOT**
        * **TIpHtmlNodeTHEAD**
      * **TIpHtmlNodeTR**


  * **TIpHtmlNodeHEAD**
  * **TIpHtmlNodeSTYLE**
  * **TIpHtmlNodeHtml**
  * **TIpHtmlNodeText**



#### Classes

##### TIpNodeBlockLayouter

Parsing generated a tree of nodes representing the html document.  
After that enqueue is preparing list of elements from the DOM tree structure  
And TIpNodeBlockLayouter class works on this list to layout 

  1. Method Calls Structure for TIpNodeBlockLayouter



Below is a list of all methods in the class with the methods they call from the class.   
If the method name has number next to it like CalcVRemain (2) it means the method is called from 2 methods in the class. No number indicates that just one method calls it.   
If the called method has a number next to it, like DoQueueAlign (3) it means that the method is called 3 times in the current method. No number indicates that it is called just one time in the method. 

  * **Layout** (0) 
    * **RemoveLeadingLFs**
    * **ProcessDuplicateLFs**
    * **RelocateQueue**
    * **LayoutQueue**
  * **LayoutQueue**
    * **QueueInit**
    * **InitMetrics**
    * **QueueLeadingObjects**
    * **TrimTrailingBlanks**
    * **DoQueueAlign** (3)
    * **InitInner**
    * **ApplyQueueProps**
    * **DoQueueElemWord**
    * **DoQueueElemObject**
    * **DoQueueElemSoftLF**
    * **DoQueueElemHardLF**
    * **DoQueueElemClear**
    * **DoQueueElemIndentOutdent** (2)
    * **DoQueueElemSoftHyphen**
    * **ContinueRow**
    * **EndRow**
    * **OutputQueueLine**
    * **DoQueueClear**
    * **CalcVRemain** (2)
    * **SetWordInfoLength**
    * **NextElemIsSoftLF**
  * **UpdateCurrent**
  * **Destroy**
    * **ClearWordList**
  * **UpdSpaceHyphenSize**
  * **UpdPropMetrics** (2)
  * **QueueInit**
  * **InitMetrics**
  * **QueueLeadingObjects**
  * **TrimTrailingBlanks**
  * **DoQueueAlign**
  * **OutputQueueLine**
  * **DoQueueClear**
  * **ApplyQueueProps**
    * **UpdPropMetrics**
  * **DoQueueElemWord**
  * **DoQueueElemObject**
    * **ObjectVertical**
    * **ObjectHorizontal**
  * **DoQueueElemSoftLF**
  * **DoQueueElemHardLF**
  * **DoQueueElemClear**
  * **DoQueueElemIndentOutdent**
  * **DoQueueElemSoftHyphen**
  * **CalcVRemain**
  * **SetWordInfoLength**
  * **NextElemIsSoftLF**
  * **RelocateQueue**
  * **CalcMinMaxQueueWidth**
    * **UpdateCurrent**
    * **UpdSpaceHyphenSize**
    * **ApplyMinMaxProps**
    * **UpdPropMetrics**
    * **TrimTrailingBlanks**
  * **CalcMinMaxPropWidth** (0) 
    * **CalcMinMaxQueueWidth**
  * **DoRenderFont**
  * **DoRenderElemWord**
    * **saveCanvasProperties**
    * **RestoreCanvasProperties**
  * **RenderQueue**
    * **DoRenderFont**
    * **DoRenderElemWord**
  * **Render** (0) 
    * **RenderQueue**



#### Enqueue

  1. Intro



The layouter is not using directly the DOM tree to layout the elements, it uses FElementQueue: TFPList list (in TIpHtmlBaseLayouter class) to layout elements.   
FElementQueue is filled based on the Parsed Nodes Tree (DOM) 

  1. What items are in the list



In the FElementQueue list we can find 
    
    
      TIpHtmlElement = record
        ElementType : TElementType;
        AnsiWord: string;
        IsBlank : Integer;
        SizeProp: TIpHtmlPropA;
        Size: TSize;
        WordRect2 : TRect;
        Props : TIpHtmlProps;
        Owner : TIpHtmlNode;
        LFHeight : Integer;  // Height of LineFeed elements
        IsSelected: boolean;
      end;
      PIpHtmlElement = ^TIpHtmlElement;
    

Note that Owner : TIpHtmlNode; is holding the actual node from the DOM tree.  
One node from the DOM tree can have multiple items in the FElementQueue list. 

  1. Trigger of process



The enqueue process can start in 3 places in TIpNodeBlockLayouter   
when on of these methods is called - Layout, CalcMinMaxPropWidth or Render   
In each of these 3 methods we can see the code   

    
    
      if FElementQueue.Count = 0 then
        FOwner.Enqueue;
    

If the list is empty it assumes that the Enque was not done and needs to be done.   
Layout, CalcMinMaxPropWidth, Render need to have FElementQueue properly filled to proceed.   
Calling enqueue in these 3 methods ensures that it will be done just before the first time the filled FElementQueue is needed.   


FOwner which enqueue method is called is declared as   
FOwner : TIpHtmlNodeCore but it is usually TIpHtmlNodeBlock descendant 
    
    
    constructor TIpHtmlNodeBlock.Create(ParentNode: TIpHtmlNode;
      LayouterClass: TIpHtmlBaseLayouterClass);
    begin
      inherited Create(ParentNode);
      FBgColor := clNone;
      FTextColor := clNone;
      FBackground := '';
      FLayouter := LayouterClass.Create(Self);
    end;
    

and most of the time it is the body node - TIpHtmlNodeBODY 

  1. Filling FElementQueue



As we said FOwner.enqueue is the starting point and FOwner is typically TIpHtmlNodeBODY.  
What it does is to itterate all the children and call their enqueue method. 
    
    
    procedure TIpHtmlNodeMulti.Enqueue;
    var
      i : Integer;
    begin
      for i := 0 to Pred(FChildren.Count) do
        TIpHtmlNode(FChildren[i]).Enqueue;
    end;
    

So, it does not add anything in FElementQueue directly but its children or their children will add. 

By default enqueue does nothing as we can see the implementation in TIpHtmlNode (top most Node class in the Nodes class hierarchy) 
    
    
    procedure TIpHtmlNode.Enqueue;
    begin
    
    end;
    

Different node classes overrides the Enqueue method to add elements in the FElementQueue list Such nodes classes are - TIpHtmlNodeText, TIpHtmlNodeDIV, TIpHtmlNodeBLOCKQUOTE, TIpHtmlNodeBR, TIpHtmlNodeDD ... 

#### Layout

  1. Intro



#### Render

  1. Intro

---

_Source: [https://wiki.freepascal.org/TurboPower_Internet_Pro](https://web.archive.org/web/20260116032055/https://wiki.freepascal.org/TurboPower_Internet_Pro)_
