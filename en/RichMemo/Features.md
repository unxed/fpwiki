# RichMemo/Features

Here're a number of features, that a considered and/or explained. 

## Load/Save from File

There're no methods to load or save to file from TRichMemo. (There're methods however to load or save to a TStream) 

The reason why no file save/load is because accessing a file might require a certain rights. I.e. when file is written, it might be necessary to either create a file or overwrite an existing file. If file is read, it must/should be shared with certain condition. 

In every program such rights and access restrictions should/could be different, depending on the task of the program. 

Thus, the access file flags should either become a parameters of load/save method, or **not even introduced** to TRichMemo base interface at all. (Leaving a developer with an option, to create TFileStream by themselves with whatever access they find desirable). 

There're utility functions introduced LoadRTFFile/SaveRTFFile, but they're not part of the TRichMemo base interface. Also, extending TRichMemo with a class helper, that introduced these method, is an option. 

  
Please, note, if at some point Load/Save File methods are to be added to RichMemo, they must also be implemented via Widgetset interface. Since a native widget might implement load/save file more efficiently (i.e. Cocoa interface). 

  
RichMemo.Lines is TStrings class, which has Load/Save file method. Using these methods you can load/save plain text (without formatting) 

## Links

Hyperlinks in text allows the user to use parts of the text as a navigation. 

Creating a link typically requires the following: 

  * text of the link (visible by the user)
  * reference data (i.e. "href" - the machine readable information on what needs to happen, if the link is clicked. Typically an address of another document)
  * hyper link text - an optional text that's shown whenever a user hovers above the link.



WinXP APIs allows to recognize the link, but doesn't provide a way to get the "reference data". Raw RTF text should be parsed to extract the data. (The position of the cursor is known, thus it's possible to extract "subsection" of RTF and parse it). Explained on [microsoft msdn blogs](<https://blogs.msdn.microsoft.com/murrays/2009/09/24/richedit-friendly-name-hyperlinks/>)

Please note that the method has its own drawback. Whenever a HYPERTEXT field is inserted, the text inserted as a reference data (and thus not visible), still affects the cursor position (SelStart). 

For example if you had text. Selstart is 0 at "h", SelStart is 6 at "w" 
    
    
    hello world
    

then you mark "hello" as a link with reference data "<http://freepascal.org>". While visually the text might look the same, you SelStart values would change: SelStart is 33 at "h", SelStart is 39 at "h". It appears there's no API to identify that. 

## See Also

  * [RichMemo](<../RichMemo.md> "RichMemo")
  * <http://www.catch22.net/tuts/neatpad> a site about creating a text editor using uniscribe

---

_Source: [https://wiki.freepascal.org/RichMemo/Features](https://web.archive.org/web/20250512152501/https://wiki.freepascal.org/RichMemo/Features)_
