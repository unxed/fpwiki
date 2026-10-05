# EasyDockingManager

## Easy Docking Manager Package

The examples/EasyDockMgr package offers several helpers for docking support. This entry is organized in multiple levels, which cover different views on the docking topics. For docking basics see [Drag and Drop Common Principles](<LCL_Drag_Drop.md> "LCL Drag Drop"). 

**Please note: All mentioned names may change in a final release!**

Also not all options are implemented now, see the further options topic below for issues deserving some consideration. 

## Contents

  * 1 Easy Docking Manager Package
    * 1.1 Adding Docking Support to an entire Application
      * 1.1.1 Prerequisites
      * 1.1.2 Making a Form a DockSite
      * 1.1.3 Making a Form dockable
      * 1.1.4 Saving and Restoring a Layout
      * 1.1.5 Hooks for Saving and Restoring components
        * 1.1.5.1 TCustomDockSite
        * 1.1.5.2 TAppDockManager (experimental)
      * 1.1.6 Synopsis
      * 1.1.7 Further Options
      * 1.1.8 IDE Sample
    * 1.2 The EasyDockTree Manager
    * 1.3 Docking Helper Classes
    * 1.4 Glossary



### Adding Docking Support to an entire Application

In the most simple case you make one or more forms of your application DockSites, to which other forms can be docked. Then make selected or all other forms dockable, and everything else will be done for you :-) 

#### Prerequisites

The uMakeSite unit defines a DockMaster class and variable (singleton), that manages all docking issues. An application must create an instance of TDockMaster, before it can use its methods. 

It should be possible to make some of the methods class methods or simple (non-object) functions, depending on the final handling of the dock sites which currently are owned by the DockMaster component. The DockMaster also could be created automatically, in the initialization section of the unit. Any opinions? 

#### Making a Form a DockSite

Use DockMaster.AddElasticSites() to make a form a DockSite. Currently docking regions (panels) can be added to the left, right and bottom of a form. "Elastic" here means that the docking regions are almost invisible, as long as nothing is docked into them. When a first component is docked into such a region, the panel extends either within the form, reducing the form's client area, or it expands the form, depending on where exactly the component is dropped. The DockRect frame indicates which kind of expansion will occur on a drop. The size of every docking region can be adjusted later by moving its associated splitter. 

#### Making a Form dockable

Use DockMaster.MakeDockable() to make any of your forms dockable. This includes setting the related properties (DragKind...), and a solution for making forms dockable on all platforms. While a dedicated dock-grip icon was added to dockable forms as a first workaround, the current solution wraps floating forms into a floathost form. That form allows to dock the client form from its dockheader, and it also allows to dock other components into the same window. When a form is undocked into floating state, another floathost is created for it. When the last form is undocked from a floathost, the now empty form is destroyed. All this automatism should not require any special application code, the required settings will be applied by MakeDockable(). 

When a form doesn't deserve special initialization, DockMaster.CreateDockable() can be used to also create the dockable form, given a form name. Here the form name must correspond to a registered form class name, and it can include an instance number. E.g. "MyForm" will create a form of class "TMyForm", named "TMyForm_1" for the first instance. [To be checked for LCL assignment of form names!] 

#### Saving and Restoring a Layout

If you want your application to restore the previous docked layout on the next start, DockMaster.SaveToFile() or SaveToStream() stores all required layout information in the given file or stream. 

DockMaster.LoadFromFile() and LoadFromStream() restores the previously stored layout. This means that the elastic and floating dock sites are created, and the previously docked forms are searched or created, and then are docked into their previous places. 

#### Hooks for Saving and Restoring components

On a layout restore, existing controls have to be found, or not yet existing controls must be created. Several hooks allow to customize the lookup and creation, details are still subject to optimization. All approaches are designed to minimize unit dependencies, so that only the really used units are linked to an application. This means that the application has to provide callback methods for saving and restoring docked controls. 

Things become somewhat tricky with nested dock sites, e.g. docked notebooks. While DockBooks can be created by a docking manager, on an alCustom drop, the docking manager can not manage the contents of such custom docksites. The Delphi docking model does not cover notebooks or other nested docksites, and restricts the information about all directly docked controls (under control of the DockTree manager) to an string. While a special TWinControl.RestoreDockedControl method can do whatever is required to restore an embedded docksite (control) and all its previous content, no such method exists for the creation of that string :-( 

##### TCustomDockSite

The first extension of the Delphi model is a SaveDockedControl method, that returns an string containing all related information. But this method can not be added to TControl, so that other hooks must be used. The first workaround is a TCustomDockSite class, that must become the base class of all restorable docksites. This class can implement a pair of SaveSite/ReloadSite methods, usable by the docking manager. The ReloadSite method must be a class method, because it has to create (and return) the reloaded site. The class type (name) of the docksite is stored and retrieved by the docking manager, prior to calling Save/RestoreSite. The nested site information still is stored as a string, so that the docking manager can skip over it in case of any failure in the restauration of the nested site. 

##### TAppDockManager (experimental)

The next extension is a connection to an application specific layout save/restore mechanism (still under construction). The first connection is an AppLoadStore callback method, that is invoked prior to any internal dockmanager handling. Since that method has to handle all aspects and special cases of the restauration of a layout, almost no application writer will implement such a method himself. For this reason another hook is provided in uMakeSite, consisting of an extended EasyTree manager (TAppDockManager), that defers all save/restore calls to the DockMaster. When an application only uses dockable forms, and prefabricated custom docksites and floating sites, almost nothing special remains to do in application code. For eventual exceptions the DockMaster can invoke OnSave and OnReload event handlers, which only must be capable of creating special forms, based on the strings stored by OnSave. 

#### Synopsis

In the simplest case your application creates an DockMaster, calls DockMaster.CreateDockable for every reloadable form, and uses DockMaster.LoadFromFile/SaveToFile for saving and restoring layouts. 

DockMaster.AddElasticSites can be called to add elastic docksites to non-dockable forms, like the application main form. 

When your forms need more creation information, provide OnSave and OnRestore handlers to the DockMaster. 

#### Further Options

Above issues currently are reflected in $DEFINEs in uMakeSite, and in a DockMaster.Factory field. These and some more issues deserve further consideration (discussion). I'd like to remove the Defines and to replace them by meaningful conventions, suitable for all application cases. 

When restored forms deserve further parameters, like editor components should reload the previously edited file, then the Delphi-inherited TWinControl.ReloadDockedControl(CtrlName) is insufficient. We could add an inverse method SaveDockedControl, that returns an string containing **all** information about a docked control, not only its name. This method can be added to TWinControl, or to a new TDockingFactory class used for DockMaster.Factory. When SaveDockedControl is implemented to return an string with more than only the control name, then of course that string must be handled in ReloadDockedControl, because the DockMaster has no idea how to extract the control name and class from such an string. 

Another issue is notebook docking. The Delphi docking model offers no special means to restore DockBooks, neither by nesting DockSites nor by integration into an extended DockManager. The EasyDockTree manager already abuses alCustom for notebook docking, and could be extended to handle notebook contents (tabs), and eventually recursively the layout of notebook pages as further DockSites. Any opinions? 

My docking notebooks differ from the IDE SourceEditor notebooks, at least in the handling of the docked pages. Since it depends on the platforms, which components can be made dockable how, I use the notebook tabs as docking grips. The pages of a DockBook are not related to each other, every dockable control can be docked as a notebook page, and can be undocked independently as well. Thus notebooks also can be docked into notebooks, what would allow e.g. to hold the included files of a source unit in a dedicated (nested) notebook, together with the unit source itself. OTOH the IDE SourceNotebook seems to be a specialized form class, handling only docked SourceEditor pages, what does not integrate well into the general layout Save/Load procedures. As long as the SourceNotebook form has no other components apart from the notebook itself, I'd suggest to refactor the SourceEditors into independent components (forms, frames...), that can be docked in whatever way the user likes. 

IMO only one DefaultDockManager class should exist in an application. There exist conflicts between the current IDE implementation, using anchor docking as default, and the EasyDockMgr approach. Also only one default FloatingDockHost class should exist, what IMO suggests a strict separation between different docking models and managers. We could agree about an (abstract) application layout manager class, that would allow to use either anchor-docking or easy-docking in the IDE, by only exchanging the creation of the respective manager instance. Such an exchange will affect also the stored layouts, which are somewhat incompatible between different managers. Using XML configuration files would allow to use the same configuration file(s) for all managers, but the different information types IMO would make it still impossible to save a layout using one manager, and to restore it using an different manager. 

#### IDE Sample

This is an idea about making the IDE dockable. I couldn't make it work just now, but before it's forgotten. Let's assume that EnableIDEdocking defined means anchor docking, else new docking/layout. 

In main.TMainIIDE the following case could be inserted: 
    
    
    TMainIIDE.Create - create DockMaster
    ...
      {$IFDEF EnableIDEdocking}
      FDockingManager:=TLazDockingManager.Create(Self); //anchor docking
      {$ELSE}
      uMakeSite.TDockMaster.Create(self); //easy docking: create DockMaster
      {$ENDIF}
    
    TMainIIDE.OnlApplyWindowLayout - make forms dockable
    ...
      l:=NonModalIDEFormIDToEnum(ALayout.FormID);
      if DockingAllowed then begin
        if l in [nmiwSourceNoteBookName] then
          ALayout.WindowPlacement:=iwpDocked;
      end else if assigned(DockMaster) then begin
      //easy docking
        case l of
        nmiwNone: ;
        nmiwMainIDEName: ; //optional: DockMaster.AddElasticSites(ALayout.Form, [alBottom]);
        nmiwSourceNoteBookName: //make this one an elastic site
          DockMaster.AddElasticSites(ALayout.Form, [alLeft, alBottom, alRight]);
        else //make all other non-modal windows dockable
          DockMaster.MakeDockable(ALayout.Form, True);
        end;
      end;
    

### The EasyDockTree Manager

Almost every DockSite needs an DockManager. The EasyDockTree manager allows to build an dock tree, that grows by docking other components to any side (top/bottom, left/right) of an already docked component. Dropping a component into the middle of another component creates a tabbed notebook, to which more components can be docked as notebook pages. 

### Docking Helper Classes

A FloatingSite is a container form for other components, that possibly cannot exist without a parent form (TControl). It also acts as an entire docking site (form), to which other components can be docked. 

A DockBook also is a container form for other components, that keeps docked components in distinct notebook pages, while a FloatingSite shows the docked components beneath each other. 

### Glossary

DockSite
    A TWinControl with the boolean property DockSite set to True. A DockSite can accept DockClients.
managed DockSite
    A docksite with an installed dockmanager (DockManager<>nil, UseDockManager=True).
docked
    Controls appearing in a DockSite, in contrast to Floating or residing in an "ordinary" container control.
DockClient
    A docked control. In contrast to other controls, its position and size is controlled by the dockmanager of the docksite.
target site
    The docksite where the dragged control actually can or will be docked (when dropped).
DockRect
    A visible rectangle, that reflects the (intended) position and size of the dragged control after it is dropped.
DockHeader
    Visible elements, appearing near a docked control. They are added by the dockmanager, e.g. as a replacement for the invisible title bar of a docked form.
floating
    A dockable control in its own window on the screen, not docked into a docksite.
floating site
    A helper window, containing a dockable control that is not a window itself.
FloatHost
    TControl.FloatHostSite indicates the logical parent (floating site) of a docked control.
ConJoinDockHost sites
    Special docksites (application specific)
wrapping a control into a floating site
    When a dockable control cannot float for itself, a temporary floating site is created, into which the control is docked (wrapped). When the control is docked later, the floating site is destroyed.

---

_Source: [https://wiki.freepascal.org/EasyDockingManager](https://web.archive.org/web/20250517004741/https://wiki.freepascal.org/EasyDockingManager)_
