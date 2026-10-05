# Fresnel CSS Algorithm

## Contents

  * 1 Overview
  * 2 Delayed computation
  * 3 ApplyCSS
    * 3.1 Collecting type stylesheets
    * 3.2 Parsing stylesheets
    * 3.3 Collect CSS attributes for each element
    * 3.4 Box-Modell
    * 3.5 Layout
      * 3.5.1 Layout Top-Down
      * 3.5.2 Layout Final
  * 4 Flow Layouter
  * 5 Flex Layouter
  * 6 Grid Layouter



# Overview

This page describes the how [Fresnel](<Fresnel.md> "Fresnel") parses and applies CSS, layouts and renders. 

# Delayed computation

Whenever an element or CSS is added, removed, altered TFresnelElement.DomChanged is called. This does not compute anything immediately in contrary to LCL or VCL. Instead a change: 

  * calls _TFresnelCustomForm.DomChanged_ to _LayoutQueued_ to true,
  * the first change **queues** a call of _TFresnelCustomForm.OnQueuedLayout_.
  * which eventually calls _ApplyCSS_ and _Invalidate_ ,
  * which queues a _WSDraw_ ,
  * which eventually calls _Renderer.Draw_.



Each form has its own queued call. 

If you need to compute a change immediately call _Form.ApplyCSS_. 

# ApplyCSS

The article gives an overview of the order and how the CSS is parsed and computed in Fresnel. The central method is **TFresnelViewport.ApplyCSS**. 

## Collecting type stylesheets

The **Viewport** gathers all used **type stylesheets** via _GetCSSTypeStyle_ and adds them to resolver. These have _origin user-agent_. 

  * E.g. if there is a _TDiv_ and a _TButton_ on the form, it will use the stylesheets returned by _TDiv.GetCSSTypeStyle_ and _TButton.GetCSSTypeStyle_.
  * If there is a TMyButton, with _TMyButton = class(TButton)_ , it will contain both stylesheets. This allows to create a 'button' descendant, adding the 'button' stylesheet only once.



## Parsing stylesheets

Each form has one **Resolver: TCSSResolver**. The resolver parses all stylesheets: 

  * Starts with the stylesheets from the used element types, e.g. all used _TFresnelElement_ classes. These have origin user-agent.
  * Next it parses the _application_ stylesheets. These have origin user.
  * Finally it parses the _form_ stylesheet. This has origin author.
  * Checks every attribute: 
    * The resolver searches all attribute names and stores the numerical ID for later faster lookup.
    * Every attribute is registered in the **CSSRegistry** and has a _OnCheck_ method. E.g. the 'display' attribute calls _TFresnelCSSRegistry.CheckDisplay_.
    * Every value is checked for syntax. If the value contains a _var_ function, its syntax cannot be checked at this point.
    * If the value has a syntax error, then it is marked _invalid_ and will be skipped. A warning can be logged.



## Collect CSS attributes for each element

The _Viewport_ traverses all nodes (TFresnelElement) to compute basic attributes: 

  * It calls ComputeInlineStyle, which parses the _Style_ property as _inline style_ , similar to the above.
  * It calls ComputeCSSValues: 
    * Calls **Resolver.Compute**
      * Collects all matching rules for this element
      * Merges the attributes of these rules and the inline style to a list of attributes. Considering **origin** , **rule-specifity** , **short-hands** , **all** and **!important**.
      * Substitutes **var()** calls using the custom-attributes of the element, or the inherited values.
      * Applies **shorthands**. E.g. _padding_ is split into _padding-top_ , _padding-right_ , _padding-bottom_ and _padding-left_.
      * Returns a list of attributes.
    * Substitute the base keywords **initial** , **inherit** , **unset**. Note: _revert_ and _revert-layer_ are currently treated as _unset_.
    * Compute base attributes: **visibility, display, position, box-sizing, direction, writing-mode, overflow-x, overflow-y, float**
      * if _display:none_ then _ComputedVisibility_ becomes _CSSRegistry.kwCollapse_



## Box-Modell

  * MarginBox: outer rectangle
  * BorderBox: outer rectangle of the border (Note: a border-width is only relevant with a border-style)
  * GutterBox: inside the border, the gutter is space for scrollbars. Scrollbars might be painted bigger than the gutter space, overlapping into the clientbox.
  * ClientBox: inside the gutterbox, scrollable area. 
    * child elements left, top are relative to the container's ClientBox
  * ContentBox: ClientBox without the padding.
  * **width** and **height** are MarginBox minus margins, border and padding (box-sizing: content-box). Keep in mind, that this ignores gutter space.



## Layout

The _Viewport_ calls **Layouter.Apply** , which is **TViewportLayouter.Apply**. 

### Layout Top-Down

First all elements are traversed top-down. 

  * Every visible, non collapsed element (TFresnelElement) gets an instance of _TUsedLayoutNode_ in _LayoutNode_.
  * Every non static element gets a **z-index**.
  * Every block element gets a layouter: **TFLFlowLayouter** , **TFLFlexLayouter** or**TFLGridLayouter**.
  * Set LayoutNode.Container
  * All used lengths are reset: 
    * **border-, padding-, margin-top/right/bottom/left** , and **line-height** are set 0.
    * **left, top, right, bottom, width, height, min-width, max-width, min-height, max-height** are set to _NaN_.
  * **TUsedLayoutNode.ComputeUsedLengths** is called with _NoChildren=true_ (top-down run). 
    * It computes each length, using only the element's and its parents' attributes.
    * Some values can be computed immediately e.g. 'width:100px'
    * Some values are relative: 
      * font-size:150% means 150% of parent's font-size, that means it can be computed top-down.
      * line-height:150% means 150% of element's font-size, it can be computed top-down
      * line-height:1.2 must be inherited as '1.2', because it can mean different sizes in descendants, that means distinguish _Computed_ and _Used_
    * Some values can be deducted. E.g. given _left, right_ , it can deduct _width_. Well, it is more complicated due to _min-width_ , _max-width_ , _display_ , _position_ , and _box-sizing_ , but you get the idea.



### Layout Final

After the top-down traversal, all fixed or relative to fixed lengths are computed. 

The elements are again traversed top-down, but now the layouters can use child lengths and compute intrinsic sizes recursively. Intrinsic sizes are the width and/or height of the content ignoring the element's min/max-width/height. The intrinsic sizes are cached, so the recursion runs in linear time. 

After the layout was computed, each **TFresnelElement.ComputeCSSLayoutFinished** is called. 

# Flow Layouter

  * **block** elements are using a line of their own. 
    * each block has a block layouter (TFLBlockLayouter) to calculate min-/max-content ignoring outer floats
    * the flow-root block computes the final layout, including the layout of its non-flow-root child blocks, so float elements affects the inline elements.
  * **inline** elements are added till a line is full and broken at line breaks. See fragmentation. Each series of inline elements build a pseudo block consisting of line boxes.
  * elements are vertically aligned at the **baseline**. If an element has no baseline (display: inline flow-root), its bottom is its baseline.
  * vertical **padding** , **border** and **margin** have no layout effect on inline-elements, they effect block and float-elements.
  * horizontal **padding** , **border** , **margin** effect all elements.
  * **margin collapse** : not yet implemented 
    * between blocks
    * child and parent top/bottom-margin collapse if there is no separation (padding, border, block formatting context)
    * empty block: no padding, border and content height 0, then margin-top+bottom collapse
    * Pos and Pos = Max, Pos and Neg = Sum, Neg and Neg = Min
  * **left** , **top** , **right** , **bottom** have no effect on **static** elements
  * the **static position** is the default for the other positions, so it must be always computed
  * **width** , **height** have **no** effect on static inline elements (unless **flow-root** or **overflow**)
  * **direction** : LTR or RTL, start left or right - not yet implemented
  * **writing-mode** : not yet implemented
  * **text-align** : align lines horizontally: **left** , **right** , **start** , **end** , **center** , **justify** , **match-parent**
    * **justify** : the last line is aligned left or right, depending on **direction**
  * **vertical-align** : align inline elements vertically, **baseline** , **sub** , **super** , **text-top** , **text-bottom** , **middle**
    * this effects the baseline of the child elements
  * **float** : **none** , **left** , **right** , Not yet implemented: **inline-start** , **inline-end** , **top** , **bottom**
    * removes element from the flow and puts it left or right, making inline elements flow around and following float elements are stacked
    * its content size is computed using MaxWidth of current block ignoring other floats (not trying to fit other floats)
  * **clear:** : **none** , **left** , **right**. Put block or float element below preceding floats.
  * **line-height** : positive float, 1=element's font ascent+descent, % allowed, inherited, default: normal (1.2)
  * **padding** : positive float, % uses container's width even for top and bottom
  * **border-width** : positive float, no %, only used if border-style is not **none**
  * **margin** : float, can be negative, % uses container's width even for top and bottom
  * spans: 
    * can spread over multiple lines - fragmentation into line-boxes: 
      * all line-boxes get the top and bottom border, only the first box the left border, only the last the right border.
      * the UsedClientBox, UsedBorderBox are the union or hull of the line-boxes.
    * vertical-align changes the baseline of the child elements
    * a span can have position: relative or stick:, the child elements are moved with the span.



# Flex Layouter

# Grid Layouter

---

_Source: [https://wiki.freepascal.org/Fresnel_CSS_Algorithm](https://web.archive.org/web/20260108044308/https://wiki.freepascal.org/Fresnel_CSS_Algorithm)_
