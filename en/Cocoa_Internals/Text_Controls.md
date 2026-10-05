# Cocoa Internals/Text Controls

## Contents

  * 1 Edit
    * 1.1 MaxLength
  * 2 ComboBox
    * 2.1 NSComboBox features
    * 2.2 Programmatic Drop
  * 3 TMemo (NSTextView)
    * 3.1 Inserting into Scroll View
    * 3.2 Line padding oddity
    * 3.3 Forcing Lines Breaks
  * 4 TSpinEdit
  * 5 Text Input
    * 5.1 Dead Keys
  * 6 See Also



## Edit

NSTextField. The field itself is readonly. Whenever it starts to edit, it's actually TCocoaFormEditor. 

### MaxLength

The maximum length is not supported natively in Cocoa. There are a few ways to do that: 

  * textDidChange - (currently used) once the text changed - adjust it to the max length, before sending a notification
  * NSFormatter - there's an example of [stackoverflow](<https://stackoverflow.com/questions/827014/how-to-limit-nstextfield-text-length-and-keep-it-always-upper-case>) on how to do that. But it doesn't work in Pascal



  


## ComboBox

Non-readonly ComboBox is implemented via NSComboBox type. Readonly combobox is implemented via NSPopupButton 

### NSComboBox features

  * It doesn't like to be 29px or taller in height. The height is fixed to 26 pixels
  * It doesn't like custom fonts (other than default) to be assigned to itself (see [#33626](<https://bugs.freepascal.org/view.php?id=33626>))



[![cocoa combobox issue.png](https://wiki.freepascal.org/images/f/f6/cocoa_combobox_issue.png)](</File:cocoa_combobox_issue.png>)

### Programmatic Drop

NSAppKit doesn't provide any means to trigger the comboBox dropdown via an API call. Thus any approach would be hacking solution anyway. 

There are a few options, that can be found online. 

1) Using NSAccessibility APIs [stackoverflow](<https://stackoverflow.com/questions/4499262/how-to-programmatically-open-an-nscomboboxs-list>)

The solution suggests using a deprecated APIs. The solution itself is pretty old, and that's why deprecated methods are used. 

    It should not be applied for macOS 10.11 and later.
    
    
    - (void) setExpanded: (BOOL)expanded
    {
        id ax = NSAccessibilityUnignoredDescendant(self);
        [ax accessibilitySetValue: [NSNumber numberWithBool: expanded]
                     forAttribute: NSAccessibilityExpandedAttribute];
    }
    

2) The same link as the method above, but it's suggesting to call **popUp:** selector. The selector isn't documented (but can be seen if studying backtrace) 
    
    
     [(NSComboBoxCell*)self.acomboBox.cell performSelector:@selector(popUp:)];
    

This is a hack and undesired approach, as it might cause problems in future. 

3) Instead of using **popUp:** , call for **moveDown:** which is a documented API However, it should not be called while processing the event. Instead it should be scheduled to be called after the current event is processed. As the key pressed stucks in the event queue 
    
    
    procedure TCocoaComboBox.lclPopup;
    var
      c : NSCell;
    begin
      if isDown then Exit;
      c:=cell;
      if not Assigned(c) then Exit;
      // cannot implement it immediately, as it blocks the current event processing
      c.performSelector_withObject_afterDelay(
        ObjCSelector('moveDown:'), nil, 0
      );
    end;
    

## TMemo (NSTextView)

TMemo widgetset is implemented over NSTextView and NSScrollView 

By default NSTextView is designed to be constantly word-wrapped. Disabling word-wrapping could be quite complicated from a start due to odd-default values chosen by Apple, as well as complex (yet flexible) Text Layout system. 

  * NSTextView is a "cocoa" control, however it's not drawing the text by it's own it's also using:
  * NSTextContainer. Both NSTextContainer and NSTextView settings influence on how the text is rendered in the end.



The example shows, of creating NSTextView that automatically resizes itself horizontally. However NSTextView doesn't provide its own scrollbars, thus no scrollbars would be seen. 
    
    
    procedure TForm1.FormShow(Sender: TObject);
    var
      txt : NSTextView;
    begin
      txt := NSTextView.alloc.initWithFrame(NSMakeRect(10,ClientHeight-10-50,50,50));
    
      txt.setFont(NSFont.systemFontOfSize(NSFont.systemFontSizeForControlSize(NSRegularControlSize)));
    
      // making the maximum size - maximum!
      // 10000000 is a "constant" could be found in Apple documentation
      txt.setMaxSize( NSMakeSize(10000000, 10000000));
      // preventing textContainer from following the width of NSTextView
      txt.textContainer.setWidthTracksTextView(false);
      // making TextContainer large enough.
      txt.textContainer.setContainerSize ( NSMakeSize( 10000000, 1024));
      // making NSTextView to resize automatically to the text boundries (max width)
      txt.setHorizontallyResizable(true);
    
      NSView(Self.Handle).addSubView(txt);
    end;
    

The next step is actually to embed NSTextView into ScrollView (as a documentView). 

### Inserting into Scroll View

The process is straight-forward - allocate scroll view, use NSTextView as it's document view 
    
    
    procedure TForm1.FormShow(Sender: TObject);
    var
      txt : NSTextView;
      sc  : NSScrollView;
    begin
      txt := NSTextView.alloc.initWithFrame(NSMakeRect(10,ClientHeight-10-50,50,50));
    
      txt.setMaxSize( NSMakeSize(10000000, 10000000));
      txt.textContainer.setWidthTracksTextView(false);
      txt.textContainer.setContainerSize ( NSMakeSize( 10000000, 1024));
      txt.setHorizontallyResizable(true);
    
      // allocating scroll view and placing NSTextView inside
      sc := NSScrollView.alloc.initWithFrame(NSMakeRect(10,ClientHeight-10-150,150,150));
      sc.setHasVerticalScroller(true);
      sc.setHasHorizontalScroller(true);
      sc.setAutohidesScrollers(true);
      sc.setDocumentView(txt);
    
      NSView(Self.Handle).addSubView(sc);
    end;
    

Now text-view and scrollbars would act as expected. 

### Line padding oddity

By default, there's 5 pixels padding between border of NSTextView and left-side of the text. 

It's possible to decrease it, however, setting it to zero is causing a very weird and unpleasant glitch. 
    
    
    var
      txt : NSTextView;
      sc  : NSScrollView;
    begin
      txt := NSTextView.alloc.initWithFrame(NSMakeRect(10,ClientHeight-10-50,50,50));
    
      txt.setMaxSize( NSMakeSize(10000000, 10000000));
      txt.textContainer.setWidthTracksTextView(false);
      txt.textContainer.setContainerSize ( NSMakeSize( 10000000, 1024));
      txt.setHorizontallyResizable(true);
    
      // setting this value to 0 is causing the problem.
      txt.textContainer.setLineFragmentPadding(0);
    
      sc := NSScrollView.alloc.initWithFrame(NSMakeRect(10,ClientHeight-10-150,150,150));
      sc.setHasVerticalScroller(true);
      sc.setHasHorizontalScroller(true);
      sc.setAutohidesScrollers(true);
      sc.setDocumentView(txt);
    
      NSView(Self.Handle).addSubView(sc);
    end;
    

Now. Everytime a new line break is added **to the end of the text** the view scrolls to the top (while having cursor at the bottom). 

Either by pressing Enter on keyboard having cursor at the end of the text. Or by inserting a line break at the end of the text programmatically. 

  
The only way to avoid the problem is to have Fragment padding set to a value greater than zero. 

### Forcing Lines Breaks

NSTextView is a rich-text controls, thus it can (visually) handle all variety of Line breaks 

  * CRLF (#13#10) - Windows
  * LF (#10) - Unix
  * CR (#13) - Classic Mac



For consistency and Carbon compatibility, unix style (#10) is enforced. 

## TSpinEdit

Spin edit control is implemented using 2 controls NSEdit and NSStepper (up/down arrow). 

  * Handle returned is NSEdit class. Even though the exact bounds of NSEdit ARE NOT identical to the SpinEdit boundaries. There's an additional offset to fit NSStepper
  * It's possible to implement **EditorEnabled** property, simply by only setting NSEdit to readonly mode and allowing NSStepper to change values



## Text Input

### Dead Keys

By default Cocoa doesn't support dead keys entries for non NSTextView controls. The sample of they dead-keys can be seen, by installing (qwerty) Spanish keyboard layout (where accents are used). An accent is placed by pressing "'" followed by vowel character (a, e, i, o). 

If following code is used, you should see that two key presses followed by a single unicode character generated. 

Also, if "'" is followed by non-vowel character, exactly 2 separate characters would be generated 
    
    
    procedure TForm1.FormKeyDown(Sender: TObject; var Key: Word; Shift: TShiftState
      );
    begin
      case Key of
        VK_SPACE, VK_BACK, VK_DELETE:
          Caption := '';
      else
        writeln('down: ', Integer(Key));
      end;
    end;
    
    procedure TForm1.FormUTF8KeyPress(Sender: TObject; var UTF8Key: TUTF8Char);
    begin
      if (UTF8Key <> ' ') and (UTF8Key <> #8) then begin
        Caption:=Caption+UTF8Key;
        writeln(UTF8Key);
      end;
    end;
    

## See Also

  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals")
  * [Tracking the Size of a Text View](<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/TextStorageLayer/Tasks/TrackingSize.html>) \- Apple's official documentation explains how text size is tracked between NSViewText and NSViewContainer
  * [Putting an NSTextView Object in an NSScrollView](<https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/TextUILayer/Tasks/TextInScrollView.html>) \- Apple's official documentation on how to place NSTextView into a scrollview.
  * [How to Disable Word-Wrap of NSTextview](<https://stackoverflow.com/questions/3174140/how-to-disable-word-wrap-of-nstextview>) \- stackoverflow discussion of the problem.

---

_Source: [https://wiki.freepascal.org/Cocoa_Internals/Text_Controls](https://web.archive.org/web/20241206081921/https://wiki.freepascal.org/Cocoa_Internals/Text_Controls)_
