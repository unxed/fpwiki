# LCL Accessibility

Accessibility means planning and developing applications in a way in which allows an individual with visual impairments or other disabilities to perform a task that they would otherwise be unable to do, or increase the ease and safety with which a task can be performed, or anything that assists individuals to carry-out daily activities. 

## Contents

  * 1 LCL Accessibility Introduction
    * 1.1 Obtaining the accessible object associated with a control
    * 1.2 Setting the basic accessible fields for a control
  * 2 Screen Readers
    * 2.1 macOS VoiceOver
    * 2.2 Orca for Linux
  * 3 Speech synthesis
  * 4 Accessibility Inspector Tools
    * 4.1 macOS
  * 5 Accessibility APIs
    * 5.1 Carbon
    * 5.2 Cocoa
    * 5.3 Microsoft Active Accessibility
    * 5.4 AT-SPI for Linux
    * 5.5 Qt
    * 5.6 Concepts mapping



# LCL Accessibility Introduction

The LCL should automatically provide accessibility for all of its standard controls, or allow the underlying widgetset to provide its own accessibility for the controls utilized directly from the widgetset. LCL users need to provide accessibility only for TCustomControl descendents introduced in the application. LCL developers need to provide accessibility for all TCustomControl descendents in the LCL and also for all non-windowed controls in the LCL (such as TLabel). 

The accessibility information provided is a tree of accessible objects which represent the controls hierarchy in the application and which can cover both windowed and non-windowed controls. What the screen reader does exactly with this information is theoretically up to it to decide, but of course it is necessary to test to verify if the most popular screen readers are actually using the information provided in the expected way. The LCL is responsible for making these bridges, since it provides a quite simple accessibility interface. Platform APIs, especially the one from macOS, are more complex. 

TControl will automatically create an accessible object and place it correctly in the accessible objects tree, so it is not necessary to manually create accessible objects unless a control has sub-items which should be represented individually and are not TControl-descendents. One typical example are the items of the TTreeView control. They are not controls, but can be selected individually and therefore each item should be an accessible object on its own. 
    
    
      TLazAccessibilityRole = (
        larAnimation, // An object that displays an animation.
        larButton, // A button.
        larCell, // A cell in a table.
        larChart, // An object that displays a graphical representation of data.
        larCheckBox, // An object that can be checked or unchecked, or sometimes in an intermediary state
        larClock, // A clock displaying time.
        larColorPicker, // A control which allows selecting a color.
        larComboBox, // A list of choices that the user can select from.
        larDateField, // A controls which displays and possibly allows to choose a date.
        larGrid, // A grid control which displays cells
        larGroup, // A control which groups others, such as a TGroupBox.
        larIgnore, // Something to be ignored. For example a blank space between other objects.
        larImage, // A graphic or picture or an icon.
        larLabel, // A text label as usually placed near other widgets.
        larListBox, // A list of items, from which the user can select one or more items.
        larListItem, // An item in a list of items.
        larMenuBar, // A main menu bar.
        larMenuItem, // A item in a menu.
        larProgressIndicator, // A control which shows a progress indication.
        larResizeGrip, // A grip that the user can drag to change the size of widgets.
        larScrollBar, // A control to scroll another one
        larSpinner, // A control which allows to increment / decrement a value.
        larTabControl, // A control with tabs, like TPageControl.
        larTextEditorMultiline, // A multi-line text editor (for example: TMemo, SynEdit)
        larTextEditorSingleline, // A single-line text editor (for example: TEdit)
        larTrackBar, // A control which allows to drag a slider.
        larTreeView, // A list of items in a tree structure.
        larTreeItem, // An item in a tree structure.
        larWindow // A top level window.
      );
    
      // The Child Accessible Objects are designed for non-TControl children
      // of a TCustomControl descendent, for example the items of a TTreeView
    
      { TLazAccessibleObject }
    
      TLazAccessibleObject = class
      private
        FHandle: PtrInt;
        FPosition: TPoint;
        FSize: TSize;
        function GetHandle: PtrInt;
        function GetPosition: TPoint;
        function GetSize: TSize;
        procedure SetPosition(AValue: TPoint);
        procedure SetSize(AValue: TSize);
      protected
        FChildren: TFPList; // of TLazAccessibleObject
        FAccessibleDescription: TCaption;
        FAccessibleValue: TCaption;
        FAccessibleRole: TLazAccessibilityRole;
        class procedure WSRegisterClass; virtual;//override;
      public
        OwnerControl: TControl;
        Parent: TLazAccessibleObject;
        DataObject: TObject; // Available to be used to connect to an object
        SecondaryHandle: PtrInt; // Available for Widgetsets to use
        constructor Create(AOwner: TControl); virtual;
        destructor Destroy; override;
        procedure SetAccessibleDescription(const ADescription: TCaption);
        procedure SetAccessibleValue(const AValue: TCaption);
        procedure SetAccessibleRole(const ARole: TLazAccessibilityRole);
        function FindOwnerWinControl: TWinControl;
        function AddChildAccessibleObject: TLazAccessibleObject; virtual;
        procedure ClearChildAccessibleObjects;
        procedure RemoveChildAccessibleObject(AObject: TLazAccessibleObject);
        function GetChildAccessibleObject(AIndex: Integer): TLazAccessibleObject;
        function GetChildAccessibleObjectWithDataObject(ADataObject: TObject): TLazAccessibleObject;
        function GetChildAccessibleObjectsCount: Integer;
        function GetSelectedChildAccessibleObject: TLazAccessibleObject; virtual;
        function GetChildAccessibleObjectAtPos(APos: TPoint): TLazAccessibleObject; virtual;
        // Primary information
        property AccessibleDescription: TCaption read FAccessibleDescription write SetAccessibleDescription;
        property AccessibleValue: TCaption read FAccessibleValue write SetAccessibleValue;
        property AccessibleRole: TLazAccessibilityRole read FAccessibleRole write SetAccessibleRole;
        property Position: TPoint read GetPosition write SetPosition;
        property Size: TSize read GetSize write SetSize;
        property Handle: PtrInt read GetHandle write FHandle;
      end;
    
      TControl = class(TLCLComponent)
      private
    ...
        // accessibility
        function GetAccessibleObject: TLazAccessibleObject;
        function CreateAccessibleObject: TLazAccessibleObject; virtual;
        function GetSelectedChildAccessibleObject: TLazAccessibleObject; virtual;
        function GetChildAccessibleObjectAtPos(APos: TPoint): TLazAccessibleObject; virtual;
      public
        // standard properties, which should be supported by all descendants
        property AccessibleDescription: TCaption read GetAccessibleDescription write SetAccessibleDescription;
        property AccessibleValue: TCaption read GetAccessibleValue write SetAccessibleValue;
        property AccessibleRole: TLazAccessibilityRole read GetAccessibleRole write SetAccessibleRole;
    ...
      end;
    

## Obtaining the accessible object associated with a control

Use TControl.GetAccessibleObject: TLazAccessibleObject; 

## Setting the basic accessible fields for a control

Use TControl.AcessibleDescription, TControl.AccessibleValue, TControl.AccessibleRole. TControl.GetAccessibleObject().AccessibleDescription is the same thing as TControl.AccessibleDescription and they are connected. 

# Screen Readers

The first step in testing screen reader support is installing one and being able to use it to check what users will see when using your application. 

## macOS VoiceOver

The screen reader for the vast majority of users in macOS is VoiceOver which comes pre-installed with the operating system. To activate it go to System Preferences > Accessibility and turn VoiceOver on. It couldnt be any simpler. Using it is also mostly easy and intuitive. 

[![Voiceover screenshot.png](https://wiki.freepascal.org/images/4/49/Voiceover_screenshot.png)](</File:Voiceover_screenshot.png>)

## Orca for Linux

Orca is currently the most popular screen reader in Linux for both GNOME and KDE. See: <http://en.wikipedia.org/wiki/Orca_(assistive_technology)>

# Speech synthesis

An intelligible text-to-speech program allows individuals with visual impairments or reading disabilities to listen to written words on their computer. See the [Speech Synthesis](<Speech_Synthesis.md> "Speech Synthesis") article for details of cross-platform and native operating system solutions. 

# Accessibility Inspector Tools

Accessibility inspectors can help a lot at checking if the application is giving correct information. 

## macOS

In /Applications/Xcode.app/Contents/Applications/ there is an Accessibility Inspector application which will be very useful when developing in this platform. 

See also: 

  * Apple's [macOS accessibility documentation](<https://developer.apple.com/accessibility/macos/>)
  * Apple's [Using the Accessibility Inspector](<https://developer.apple.com/library/archive/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html>)



# Accessibility APIs

## Carbon

Search Google for: 
    
    
    site:developer.apple.com carbon accessibility
    

The main accessibility code for Carbon is in lcl/interfaces/carbon/carbonprivatecontrol.inc in the function CarbonControl_Accessibility 

The events for this handler are: 

  * kEventAccessibleGetChildAtPoint
  * kEventAccessibleGetFocusedChild
  * kEventAccessibleGetAllAttributeNames
  * kEventAccessibleGetAllParameterizedAttributeNames
  * kEventAccessibleGetNamedAttribute
  * kEventAccessibleSetNamedAttribute
  * kEventAccessibleIsNamedAttributeSettable
  * kEventAccessibleGetAllActionNames
  * kEventAccessiblePerformNamedAction
  * kEventAccessibleGetNamedActionDescription



For kEventAccessibleGetNamedAttribute the main named attributes are: 

  * AXRole
  * AXRoleDescription
  * AXDescription
  * AXHelp
  * AXParent
  * AXChildren
  * AXWindow
  * AXTopLevelUIElement
  * AXEnabled
  * AXSize
  * AXPosition



And besides the ones listed here there are tons more. See which attributes each role should implement here: <http://developer.apple.com/library/mac/#documentation/UserExperience/Reference/Accessibility_RoleAttribute_Ref/Role.html#//apple_ref/doc/uid/TP40007870-Roles-AXStaticText>

Table explaining the events: <http://developer.apple.com/library/mac/#documentation/Accessibility/Reference/AccessibilityCarbonRef/Reference/reference.html>

Documentation explaining in details which attributes are required for each Role: <http://developer.apple.com/library/mac/#documentation/UserExperience/Reference/Accessibility_RoleAttribute_Ref/Attributes.html>

List of routines to handle a AXUIElementRef: <http://developer.apple.com/library/mac/#documentation/Accessibility/Reference/AccessibilityLowlevel/AXUIElement_h/CompositePage.html>

## Cocoa

  * Apple [Accessibility Programming Guide](<https://developer.apple.com/library/archive/documentation/Accessibility/Conceptual/AccessibilityMacOSX/index.html>)
  * Apple docs for the [NSAccessibility Protocol](<http://developer.apple.com/library/mac/#documentation/Cocoa/Reference/ApplicationKit/Protocols/NSAccessibility_Protocol/Reference/Reference.html>)
  * Apple docs for [Accessibility Roles and Attributes Reference](<http://developer.apple.com/library/mac/#documentation/UserExperience/Reference/Accessibility_RoleAttribute_Ref/Notifications.html>)



## Microsoft Active Accessibility

See: <http://en.wikipedia.org/wiki/Microsoft_Active_Accessibility>

Also: <http://www.stevetrefethen.com/blog/UsingtheMSAAIAccessibleinterfacewithinaDelphiApplication.aspx>

Concrete code from MS: <http://msdn.microsoft.com/en-us/library/windows/desktop/cc307844.aspx>

## AT-SPI for Linux

See: <http://en.wikipedia.org/wiki/AT-SPI>

For making an application support AT-SPI there are two solutions: 

1-> Make the application a DBus server and serve the interface described in the following XML files: <http://git.gnome.org/browse/at-spi2-core/tree/xml>

2-> Use libatk, but this library requires a GLib main loop 

## Qt

See: <http://doc.qt.nokia.com/4.3/qaccessiblewidget.html>

Also: <http://doc.qt.nokia.com/4.3/qaccessible.html#Role-enum>

Also: <http://doc.qt.nokia.com/qq/qq24-accessibility.html>

## Concepts mapping

TLazAccessibleObject | Qt | macOS | MSAA | AT-SPI | Comments   
---|---|---|---|---|---  
AccessibleRole | QAccessible::Role | AXRole | Role | ? | \-   
AccessibleDescription | QAccessibleWidget::setDescription | AXRoleDescription, AXDescription, AXTitleUIElement | ? | ? | \-   
AccessibleValue | QAccessibleWidget::setValue | AXValue | Value, Name, State | ? | \-   
Position | ? | AXPosition | ? | ? | \-   
Size | ? | AXSize | ? | ? | \-   
Handle | ? | AXUIElementRef | ? | ? | \-   
Parent | ? | AXParent | ? | ? | \-   
SecondaryHandle | Vacant | Vacant | Vacant | Vacant | \-

---

_Source: [https://wiki.freepascal.org/LCL_Accessibility](https://web.archive.org/web/20250422000954/https://wiki.freepascal.org/LCL_Accessibility)_
