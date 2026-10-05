# Cocoa Internals/Extensions

Extensions are used to add additional lcl-friendly methods to all Cocoa classes. The default implementation of each method should: 

  * either indicate that a class is not a LCL-specific class (it's Cocoa native) thus should be handled with care
  * provide a basic functionality for LCL types (for example setting a control bounds using TRect type. Where cocoa is using float-point NSRect)



## Contents

  * 1 Extensions
    * 1.1 LCLObjectExtension
      * 1.1.1 Methods
    * 1.2 Convention
  * 2 Enabled State
    * 2.1 Switching Enabled State for Children
  * 3 See Also



## Extensions

### LCLObjectExtension

The extensions adds additional methods to **NSObject** class of cocoa. (This is TObject counterpart, where (almost) all classes in Cocoa inherit from). 

  


A lot of the methods below are control specific methods, thus it might be a good idea to elevate the extension from NSObject to NSView or NSControl. However, this might impact other control-like objects (i.w. NSWindow?) 

#### Methods

Method  | Description  | Default behavior   
---|---|---  
LCLObjectExtension  | LCLViewExtensions   
lclIsEnabled  | Returns true/false if control is enabled  | false  | true   
lclSetEnable  | Enables or disables the control  | does nothing  | traverses child views and passes new enabled status to them   
lclisVisible  | Returns true/false if control is visible  | false  | not [hidden()](<https://developer.apple.com/documentation/appkit/nsview/1483369-hidden>)  
lclSetVisible  | shows or hides a control  | does nothing  | setHidden() (where is the method in the documentation? deprecated?)   
lclisHandle  | Returns true if the instance of the class can be considered a LCL handle  | false  |   
lclClientFrame  | Returns client rectangle (content of the control) in the control's coordinate system  | does nothing  |   
lclFrame  | Returns controls position rectangle in the parent's coordinate system  | does nothing  | frame()   
lclContentView  | Returns NSView that represents the (drawn) contents. In many NSViews or combination of NSViews the content might be different. For example for NSButton (TButton), the content view is NSButton itself. For NSBox (TGroupBox), the content view is contentView. For a NSTextView sunken in NSScrollView (TMemo) the contentView would be documentView of NSScrollView. **Important** : lclContentView is also a view where all children controls should be added-to. **Important** : lclContentView is also a view that should receive focus (become a firstReponder) The problem is that all WidgetAPI calls are made through a control Handle. Handle doesn't always map to a NSView directly (TMemo, TGroupbox) where additional NSViews are in placed. lclContentView is intent to return the view, where the **draw** methods are called. The method is used by lclInvalidate and lclInvaldateRect. It should be overridden in every composite component to return the actual drawn contents. **The alternative** to not having this method is to override lclInvalidateRect for each composition control (which is also doable)  | nil  | returns self   
lclGetCallback  | The method would only be implemented by LCL controls that inherited from Cocoa controls and are used as LCL widgets. (i.e. TCocoaButton) The method returns the callback interface that was allocated during LCL Handle creation. ObjC control is responsible for storing the interfaces in some manner. (typically a field is added to a subclass). Note that the returned callback can be nil, in two cases: 1) the ObjC object queried is NOT an LCL created object 2) the ObjC object is being destroyed (by LCL DestroyHandel call), and thus the callback has already been cleared  | nil  | nil   
lclClearCallback  | The method should be implemented by LCL controls used as widgets. The method clears out the callback interface. Meaning that a subsequent call of lclGetCallback should return nil. The object should also clear out the internal reference. Any methods that are using callback interface (to report events) must always check, if the callback has not been cleared yet. It's not uncommon to destroy the object, that started an event. (i.e. freeing control, during MouseUp or MouseDown). Thus LCL provides it's own kind of protection. The widgetset is expecting itself NOT to cause a conflict (i.e. should not return any event from a handle, that has already been destroyed)  | does nothing  | does nothing.   
  
**Consideration** : if everything is bound to lclContentView. - why is it not used as a Handle anyway? Because Handle needs to match the hierarchy of Cocoa objects. I.e. an actual content residers within a ScrollView. And ScrollView is am immediate child of Form. Not the its contents. 

### Convention

  * All lcl extension methods should start with "lcl" to distinguish them Cocoa native methods.



## Enabled State

Enabled state is a basic attribute of all Controls in LCL. 

In Cocoa Enabled state is only part of NSControl class (derived). Not every LCL-control is mapped to Cocoa NSControl. For example TGroupBox is mapped to NSBox in Cocoa. NSBox is a subclass of NSView (and not NSControl). Thus NSBox doesn't have methods to control if it's enabled or not. (NSBox cannot be "disabled") 

In order to treat all Cocoa classes the same, LCLExtension introduced 2 methods lclIsEnabled and lclSetEnabled. 

  * for subclasses of NSControl these methods are almost direct mapping to **setEnabled** and **enabled** methods.
  * for NSView, NSWindow the implementation may vary



### Switching Enabled State for Children

In LCL design, once a parent control has been disabled, all it's children are disabled automatically. (Yet, they keep their "Enabled" property) 

If a parent control has been enabled, then all its children are changing to the state of their "Enabled" property. 

Traversing children must be done implicitly within Widgetset itself. (There's no LCL code to pass through children and change the state). 

Cocoa implements the loop of state change in LCLViewExtension for **lclSetEnabled** method. 

**ICommonCallBack** provides a method GetShouldBeEnabled, that returns **Enabled** property of a mapped LCL control. Used whenever a lclSetEnabled is requested to enable controls. 

## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Extensions](https://web.archive.org/web/20240909121233/https://wiki.freepascal.org/Cocoa_Internals/Extensions)_
