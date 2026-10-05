# LCL Interface Redesign Idea

## Why would we want this ?

In the present situation, the handle returned by TWSWinControl.CreateHandle is in case of win32 the winapi handle of the widget and in case of gtk a PGtkWidget. For the QT and Carbon widgetset a private internal object is returned. Using an internal object has some advantages. One of them is that we can use a true object hierarchy, with clear inheritence inside the widgetset. However, many code using the LCL expects the handle being castable. Returning an object would break this. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** In case of gtk, we don't have to return the internal object, we still can use the existing way of retrieving the widgetinfo

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** We also can start to use LCLHandle only, so that a later transition will be easier

## This is current Idea for the structure of the interface and how it relates to handles

Overview: 

  * Fork a branch of lazarus/
  * Replace TWinControl.Handle with TWinControl.LCLHandle (there should exist no Handle yet)
  * Try to get the LCL compiled and maybe the IDE so it doesn't use the (inefficient) .Handle anymore
  * Introduce .Handle again, now as WSclass call
  * Merge the branches



**The change to .Handle will not break existing code.**
    
    
    TWSxxWinControl.CreateHandle Returns a ILCLHandle
    
    ILCLHandle = IInterface; // interfaces as com objects
    
    TxxPrivate = class(TPrivate, ILCLHandle);
    
    TGtkPrivateButton = class(TGtkBin, ILCLHandle);
    

All TWSxx. methods should be moved to the private class. for instance: 
    
    
    class procedure TWSGtkWinControl.SetText(const AWinControl; const AText: String);
    var
      PrivWinControl: TGtkPrivateWinControl;
    begin
      PrivWinControl := TGtkPrivateWinControl(AWinControl.LCLHandle);
      PrivWinControl.SetText(AText);
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** I'm not to sure if we should expose the ILCLHandle as Interface. This will cause an overhead of adding exception frames, addref and releaseref calls. We can do it internally and use TLCLHandle = PtrUInt (MWE).

## Other possibilities

For accessing widgetset native handles or IDs from the LCL, in the current situation, ppl have to cast the handle. In the case of gtk, to retrieve the XID, one needs the following cast: 
    
    
    PGdkPrivateWindow(PGtkWidget(Handle)^.window)^.XID
    

When interfaces are used, one can imagine the following code 
    
    
    {$IFDEF GtkHasX}
    TGtkPrivateWidget = class(TGtkPrivate, ILCLHandle, IX11Handle);
    
    (AWinControl.LCLHandle as IX11Handle).XID
    

or maybe better: 
    
    
    if Supports(AWinControl.LCLHandle, IX11Handle, X11Handle) then
      ... DoSomething with X11Handle
    

Which leads to cleaner constructs, without the need of internal knowledge of the handle and can be the same in all widgetsets that use X11. 

Note that for this we need COM interfaces and not CORBA, however we **don't need reference counting** , so AddRef and ReleseRef can be implemented by just returning -1 

    A problem with interfaces is that Qt for example runs in several operating systems. How do you know what is supported where? We will need to fill the class definition with ifdefs.

---

_Source: [https://wiki.freepascal.org/LCL_Interface_Redesign_Idea](https://web.archive.org/web/20240226045226/https://wiki.freepascal.org/LCL_Interface_Redesign_Idea)_
