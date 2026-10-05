# Fresnel

## Contents

  * 1 Overview
    * 1.1 Platforms
  * 2 What is working?
  * 3 Goals
  * 4 Roadmap



# Overview

Fresnel is a visual component library based on CSS and custom drawn components. It is in an early stage of development. 

git repo: <https://gitlab.com/freepascal.org/lazarus/fresnel>

Requirements: fpc 3.2.2, for skia: fpc 3.3.1 

## Platforms

Fresnel runs natively on _Linux/gtk3_ and _Windows_ using _Skia_ and on _Webassembly_ in the browser. It is planned to write backends for other platforms as well. 

Fresnel forms can also run inside an LCL application. This allows to migrate LCL forms one by one. So, Fresnel runs everywhere the LCL runs. Skia can be used as renderer instead of the LCL canvas. 

Finally there is a LCL control, allowing to embed a Fresnel viewport as a control on a LCL form. 

# What is working?

As Fresnel is still in an early pre-alpha stage, a few things do work already: 

  * fcl-css supports big part of CSS parsing, even some CSS-5 feats (the actual attributes are done by TFresnelElement)
  * A Lazarus package src/fresnel.lpk containing the runtime units for standalone applications.
  * A Lazarus package src/lcl/fresnellcl.lpk containing the runtime units for Fresnel inside a LCL applications.
  * A Lazarus designtime package design/fresneldsgn.lpk registering a Fresnel Form, allowing to put TBody, TDiv, TSpan, etc onto the designer.
  * Current platforms: 
    * Linux/Gtk3 with [Skia](<Skia.md> "Skia") \- for a native application
    * Windows with [Skia](<Skia.md> "Skia") \- for a native application
    * WebAssembly - for a website
    * Inside a LCL application as window: TFresnelForm - for LCL applications 
      * Using LCL canvas for rendering (default)
      * Using Skia as renderer: add an _IDEMacro_ value _FresnelLCLRenderer:=skia_ to [Additions and Overrides](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")
    * Inside a LCL form as control: TFresnelLCLControl - to embed Fresnel inside an LCL form
    * Lazarus Designer for TFresnelForm
  * TFresnelElement 
    * a TComponent that works similar to an HTML element
    * supports many common CSS attributes like border, padding, margin, etc
    * has inline CSS in the _Style_ property and CSS classes in the CSSClasses property
    * Elements can have child elements, building a tree similar to the browser DOM.
    * CSS attributes are evaluated to their _computed_ values.
    * You can define descendants adding their own CSS attributes. See for example TCustomLabel
  * TFresnelViewport 
    * a TFresnelElement that is the root of the tree.
    * Base class for TFresnelForm.
    * Defines the Width and Height of the rendering area.
    * Contains a CSS _StyleSheet_ property, which is applied to all its child elements. Like the _author_ css in a browser.
    * Contains a Layouter, which computes all the element render boxes and has a pluggable system to support various node layouters - needed for flow, flex and grid layuout.
  * TDiv - a TFresnelElement similar to HTML div element, i.e. block
  * TSpan - a TFresnelElement similar to HTML span element, i.e. inline
  * TReplacedElement - a TFresnelElement leaf node
  * TCustomLabel - a TReplacedElement with a Caption property, the text specifies the min-width and min-height and is rendered.
  * TLabel - a TCustomLabel with published properties
  * TBody - a TDiv, works similar to HTML body element. Only needed if you use some internet css stylesheet.
  * TImage - a TElement, similar to HTML img element
  * TFresnelForm - a window with a viewport
  * Unit Fresnel.LCLControls providing controls for using the Fresnel framework in a LCL application.



# Goals

The main goals are at the moment: 

  * a CSS engine, capable of parsing common stylesheets and skipping unknown attributes and optionally log errors.
  * scrolling and clipping
  * Clipboard
  * Drag & Drop on a form, between forms, from external, to external
  * a text edit element
  * native cocoa backend
  * css development tools
  * set form and application title



# Roadmap

  * CSS engine: 
    * A application wide CSS stylesheets with origin _user_.
    * CSS attributes are skipped if invalid. One log entry per CSS error.
    * CSS functions _calc, min, max, minmax, clamp_.
    * CSS selector @media
    * type styles should be collected only once
    * changing an inline style, should not re-compute everything
    * Extend Lazarus lfm repair wizard to support duplicate classnames
    * Extend Lazarus Change Class to support duplicate classnames
  * Layouter: 
    * Flow layouter: 
      * fragments for elements, needed for wordwrap and toolbars,
      * floats (left, right),
      * direction,
      * position sticky
      * line-height
      * align at baseline
    * Flex layouter: 
      * wrap
    * Grid layouter
    * Overflow: scrolling, clipping
  * Fresnel in Lazarus designer: 
    * CSS errors
    * CSS completion box
    * a view for exploring computed CSS attributes and errors
    * macos keyboard shortcuts
  * Renderer-LCL: 
    * Testsuite
    * colors: rgb, rgba, hsl, hsla
    * borders: style
    * invalidate changed containers, paint only parts
  * Renderer-Skia: 
    * [Skia](<https://skia.org/>)
    * Testsuite
    * colors: rgb, rgba, hsl, hsla
    * borders: style
    * invalidate changed containers, paint only parts
  * Mouse capture, drag and drop
  * Events 
    * keyboard, focus, mouse wheel, minimize, maximize, restore, idle, clipboard
  * Fresnel backend 
    * cocoa
  * Animation
  * Fresnel components: 
    * label fragmentation aka multi line
    * link (CSS a), pseudo elements: :active, :link, :visited
    * edit aka input type=text
    * textarea
  * Other: 
    * background image
    * stylesheets for windows, linux, macos
    * lists
    * special funding page on foundation site.
    * HTML importer

---

_Source: [https://wiki.freepascal.org/Fresnel](https://web.archive.org/web/20250120201225/https://wiki.freepascal.org/Fresnel)_
