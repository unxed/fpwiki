# Drop files event

The drop files event will be invoked when the user drops one or multiple dragged files on one of application's forms. 

First this event should be fired for target form (or main form if drop target is unknown), then for the application. 

## Contents

  * 1 Possible implementation for LCL
  * 2 Possible implementation per widgetsets
    * 2.1 Win32/64
    * 2.2 GTK1/2
    * 2.3 Qt
    * 2.4 Carbon
  * 3 Related bug reports
  * 4 TODO



## Possible implementation for LCL
    
    
    TDropFilesEvent = procedure (Sender: TObject; const FileNames: Array of String) of Object;
    

Add OnDropFiles: TDropFilesEvent to TCustomForm, TApplication and TApplicationProperties. Each form will have property AllowDropFiles: Boolean, which enables this event. 

## Possible implementation per widgetsets

The widgetsets should call method IntfDropFiles of target form (or main form if drop target is unknown) and the application. 

### Win32/64

  * set DragAcceptFiles for every form
  * respond to WM_DROPFILES message



### GTK1/2

  * enable: gtk_drag_dest_set
  * respond to drag_data_received signal



### Qt

  * enable widget by setAcceptDrops(true)
  * respond to dragEnterEvent by event->acceptProposedAction for mime-type text/uri-list
  * respond to dropEvent
  * docs: [[1]](<http://doc.trolltech.com/4.2/dnd.html#dropping>)



### Carbon

  * respond to kAECoreSuite/kAEOpenDocuments event to open application associated files
  * modify Application Bundle



## Related bug reports

  * ~~[bug 1772](<http://www.freepascal.org/mantis/view.php?id=1772>)~~
  * ~~[bug 8976](<http://www.freepascal.org/mantis/view.php?id=8976>)~~



## TODO

  1. ~~implement for Qt~~ already implemented (during 0.9.27)

---

_Source: [https://wiki.freepascal.org/Drop_files_event](https://web.archive.org/web/20241213024502/https://wiki.freepascal.org/Drop_files_event)_
