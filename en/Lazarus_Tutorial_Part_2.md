# Lazarus Tutorial Part 2

│ **English (en)** │    
****

This is part 2 of the [Lazarus Tutorial](<Lazarus_Tutorial.md> "Lazarus Tutorial"). 

## Contents

  * 1 A simple text editor
  * 2 Learning the language
  * 3 Project Files
  * 4 LCL Internals
  * 5 Debugging
  * 6 Code completion
  * 7 See also



## A simple text editor

The second part of the tutorial features a small but useful program to show some of the possibilities and techniques to create an application with Lazarus. This part is more comprehensive in comparison to [part one](<Lazarus_Tutorial.md> "Lazarus Tutorial") assuming that the previous topics are already understood. 

The application will be a very simple text editor. Create a new project and save it. Populate the _Form1_ with the components as you see in the picture below. These are four _TButtons_ , a _TMemo_ , a _TOpenDialog_ , a _TSaveDialog_ and a _TFontDialog_. Give the components proper names by changing each 'name' property in the [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"). For the buttons names _ButtonFileOpen_ , _ButtonFileSave_ , _ButtonFont_ and _ButtonNew_ are used in this tutorial, the memo and the dialogs keep their original names. Adjust the captions of the buttons to fit to the picture. 

[![EasyEditor01.png](https://wiki.freepascal.org/images/e/e7/EasyEditor01.png)](</File:EasyEditor01.png>)

Change the anchors of each control (controls are the visual components) by clicking the control and opening the anchor editor in the [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector") at the property _Anchors_ by clicking the ... ellipsis. The anchors define which side of the form (actually the parent control) the control will stick to, or if it should adjust its size according to the size of the Form. The last is done by setting both anchors of one direction (eg the left+right anchors). _Memo1_ should stretch itself in both directions when the Form size is increased, so all four anchors should be set. The _ButtonNew_ should be right aligned, so deactivate its left anchor and activate the right anchor in the anchor editor. 

You can compile and run the project now by pressing `F9`. Change the size of the application by dragging its border and see if the controls move or stay as you would expect it. You can close the application by simply clicking the x or with the key combination `Ctrl+F2`. Or, which is the same, pressing the squared stop button in the [Lazarus menu](<Main_menu.md> "Main menu"). 

The above described 'movement' of the controls when dragging the application border can result in overlapping controls. This can be prevented by constraining the size of the form to a minimum width and height. That is done in the [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector") for _Form1_ in the item _Constraints_. Choose adequate values for a minimum height and a minimum width. Size constraints are available for all controls and are very useful in combination with anchoring and autosize properties. 

Now we should add some actual functionality to our application. Create an event handler for the _ButtonFileOpen_ , by going to the Events tab in the [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"), go to the _OnClick_ event and press the ... ellipsis OR just double-click on the control on the form editor. The editor window shows up and you can see the event handler _ButtonFileOpenClick_. Copy the code below so that the event handler procedure looks as follows: 
    
    
    procedure TForm1.ButtonFileOpenClick(Sender:TObject);
    var FileData: TStringList;
    begin
      if OpenDialog1.Execute then begin
        //Create the Stringlist so we can use it
        FileData:= TStringList.Create;
        //Load the Data from the file that was chosen with the file dialog
        FileData.LoadFromFile(OpenDialog1.FileName);
        //Copy loaded data to the Memo
        Memo1.Lines.Assign(FileData);
        //Enable the control
        Memo1.Enabled:= true;
        //Destroy the string list so that it doesn't further use memory space
        FileData.Free;
      end;
    
    end;
    

Press `F9` to compile and run the project and test the new functionality by pressing the button _Open_. A file open dialog appears and you can choose a file anywhere in your file system. It would be best if you already prepared a test file, but you also can just choose one of the project files. Be sure that it is a text file (like a .pas source file or similar). Press _OK_ and the file is displayed in the memo. You already loaded a file! You even can modify it by clicking and typing! 

Let’s explain some of the code: _OpenDialog1.Execute_ is a function that shows the dialog and blocks the application as long as the dialog stays open. When the user clicks _OK_ or _Abort_ to close the dialog, then the function will return to that place and return the value true if a file was chosen and the value false if the dialog was aborted. So that the following block is only processed, if a file was chosen. 

With `FileData := TStringList.Create;` we create an instance of the class _TStringList_ , this is - as the name says - a list of strings. Into this string list we will load the file data, which is done in the next statement with `FileData.LoadFromFile`. The file name we can read from the open dialog property _FileName_. Now the file is already loaded into the string list. The memo contains a property _Lines_ of the type _TString_ , which is very similar to a string list. 

For copying the data into the memo we use the _Lines_ property procedure _Assign_(). Now the data is in the memo and we can discard the string list with the statement FileData.Free to avoid a memory leak. If you use a class's instant variable before creating it or after freeing, i.e. if you try to access variables or procedures of the class, than this (most likely) results in an access violation, as there is no actual instance at the place the variable points to, crashing the application. 

Actually the file could be loaded directly into the memo instead of taking the intermediate step of loading it into a string list first, but by using a string list you could modify the data before displaying it in the memo. And of course this is to introduce the _TStringList_ to you, as it is rather more common to work with data in a program than just to display it. 

So loading a file works now, the next step is we want to save a modified file. Therefore create the _OnClick_ event handler for the _ButtonFileSave_ like you did with the one before. Add the following code: 
    
    
    procedure TForm1.ButtonFileSaveClick(Sender:TObject);
    begin
      if SaveDialog1.Execute 
      then Memo1.Lines.SaveToFile(SaveDialog1.FileName);
    
    end;
    

The code should be self explanatory. 

Run the program, load a file, modify and save it. (Make sure not to override a project file, there is no override protection, yet.) Normally when you override an existing file with a program you expect a warning that the file already exists. We have to implement such functionality our selves. Modify the _ButtonFileSave_ event handler as follows, compile and test the program. 
    
    
    procedure TForm1.ButtonFileSaveClick(Sender:TObject);
    var shouldSave: Boolean;
    begin
      shouldSave:= false;
    
      If SaveDialog1.Execute Then Begin
        if FileExists(SaveDialog1.FileName) then begin
          if MessageDlg('File Exists', 'Do you wish to override the existing file?',
          mtConfirmation, [mbYes, mbNo],0) = mrYes
          then shouldSave:= true;
    
        end else shouldSave:= true;
      End;
    
      //Save the memo to the file that was chosen with the file dialog
      if shouldSave
      then Memo1.Lines.SaveToFile(SaveDialog1.FileName);
    
    end;
    

What the code does: After running the file dialog with SaveDialog1.Execute we check if the file already exists, therefore the function _FileExists_ is called. If the file actually is there, then it will return the value true. And in this case we open a message window with the function _MessageDlg_. The code configures two options, _yes_ and _no_. The local variable _shouldSave_ is set accordingly and also is set if the file doesn't exist, yet. Finally the file is saved according to the temporary variable. 

Now we want to give the user the possibility to change the font. This works nearly exactly the same as with a file dialog: 
    
    
    procedure TForm1.ButtonFontClick(Sender:TObject);
    begin
      if FontDialog1.Execute
      then Memo1.Font.Assign(FontDialog1.Font);
    
    end;
    

Assigning the font to the memo could be even done in a shorter way by `Memo1.Font:= FontDialog1.Font;` But I don't recommend doing so. The issue is, assigning a class instance (as _Font_ is) with the ':=' operator only copies the instances address instead of the whole instance. i.e. both variables then contain the same instance which could cause trouble. In this specific case, thought, _Memo1.Font_ is a property that won't directly assign the variable but call a function that makes sure that a copy of the instance is made. So here we are save in either way, but if you are not sure you should choose the safe way from the beginning. 

Also populate the event handler for the button _New_. Hint: _Memo1.Clear_ erases the memo content. 

Now the basic functions of a text editor are already implemented and we should do some cosmetics. The program needs a name and icon. Open the _project settings_ in the menu _project_ in the menu bar. in the first 'line' you can define a name and an icon, both are shown on the tab bar. Design your own icon or just download the one here by rightclicking -> save as. 

The caption of the application window can be changed in the object inspector (_Caption_ entry of _Form1_). 

[![EasyEditorIcon.png](https://wiki.freepascal.org/images/1/1d/EasyEditorIcon.png)](</File:EasyEditorIcon.png>)

The names we also can modify on runtime and when opening a file we want to do exactly that. Add the following lines in the _ButtonFileOpen_ event handler: 
    
    
        Form1.Caption:= 'Easy Editor - ' + OpenDialog1.FileName;
        Application.Title:= ExtractFilename(OpenDialog1.FileName);
    

As you can see, I named the application _Easy Editor_. _OpenDialog1.FileName_ contains the full path, for the title bar this is allright, but in the task bar with the limited space only the file name should be shown. The folder path is stripped off with the function _ExtractFilename_. 

In the documentation you can find more file handling and file name functions: <https://www.freepascal.org/docs-html/rtl/sysutils/filenameroutines.html>

In this application we use the easy-to-use but limited string list functionalities to load and save files. Other options are discussed in the Wiki article [File Handling In Pascal](<File_Handling_In_Pascal.md> "File Handling In Pascal"). 

By now only the button-OnClick-event handlers were used. We want to add two more functionalities to show the usage of other events. Place a _TPanel_ below the memo and populate two _TLabels_ on the panel as you see it on the picture. Set the names as shown. Also add a _TCheckbox_ above the memo, set its name to _CheckBoxDetails_ and change its caption. Adjust all anchors as appropriate. 

[![EasyEditor02.png](https://wiki.freepascal.org/images/8/8d/EasyEditor02.png)](</File:EasyEditor02.png>)

The left label _LabelCursorPos_ will be used to show the current position of the cursor or when moving the cursor with keys. The right label _LabelMemoChanges_ we use to indicate the number of changes that were done since the document was opened. To do the latter, you should create an event handler for the memos _OnChange_ event (tab event in the object inspector). _OnChange_ will fire each time the content of _Memo1_ changes, i.e. for each pressed key individually. To take track of the quantity of the changes, we need a variable to count them. This we can place in the form's class in either the private or public section. So yes, you may freely add stuff to the class _TForm1_. We call the variable _fMemoChanges_. The prefix f stands for 'field' or 'field variable', which indicates the variable is part of a class or record (member variable). That prefix is commonly used in Lazarus programs, helping to distinguish from according properties, where a prefix is omitted. For the same benefit of better distinction, the identifiers of classes always should start with a capital T which stands for 'type'. That as a side note. 
    
    
    type
      TForm1 = class(TForm) 
        [...]
      private
        fMemoChanges: Integer; // <- Add this line
      public
    end;
    

The variable can be used in the event handler (or anywhere in the code). Add the following in the _OnChange_ event handler of _Memo1_ : 
    
    
    procedure TForm1.Memo1Change(Sender:TObject);
    begin
      fMemoChanges:= fMemoChanges + 1;
      LabelMemoChanges.Caption:= IntToStr(fMemoChanges);
    
    end;
    

As you can see, the variable is incremented each time the event handler is called and afterwards is written into the label. _IntToStr_ converts the integer to a string, other functions for conversions can be found in the documentation: <https://www.freepascal.org/docs-html/rtl/sysutils/conversionroutines.html>

Compile and test the program, it should work as expected. As you saw in the code, at no place did we initialize this variable. For field variables the compiler does that for us in the moment the instance of the class is created. The initialization value always is zero or nil for pointers. 

Now to the second last functionality, the cursor position. We want to update it, when the position of the cursor in the memo changes. There is no explicit event for that (as far as I am aware of), so we have to use mouse and keyboard events instead. As that requires more than one event handler for one action, we prepare a procedure called _UpdateLinePos_ with the functionality first, the procedure than will just be called in the event handlers. The procedure should be part of the class _TForm1_ , as the counting variable is. 
    
    
    type
      TForm1 = class(TForm) 
        [...]
      private
        MemoChanges: Integer;
        Procedure UpdateCursorPos;  // <- Add this procedure
      public
    
      end;
    

With the cursor still in the same line after writing the procedure declaration into the class, press the keys `Ctrl+Shift+C`. This advices the Lazarus IDE to autocomplete the procedure body in the interface section of the unit, saving you some typing effort. Add the below code to the procedure, the code should be easy to understand: 
    
    
    Procedure TForm1.UpdateCursorPos;
    begin
      LabelCursorPos.Caption:= 'Line: ' + Inttostr(Memo1.CaretPos.Y)
                                       + ' Position: ' + Inttostr(Memo1.CaretPos.X);
    
    end;
    

Create the event handlers OnClick and OnKeyUp for Memo1. 
    
    
    procedure TForm1.Memo1Click(Sender:TObject);
    begin
      UpdateCursorPos;
    end;
    
    procedure TForm1.Memo1KeyUp(Sender:TObject;var Key:Word;Shift:TShiftState);
    begin
      UpdateCursorPos;
    end;
    

Compile and test the program. Instead of the _OnKeyUp_ you can try using _OnKeyDown_ , but you will see that the position is not correct, as the key event is fired before the cursor was moved. I don't see an easy solution to solve that issue other than just using the _OnKeyUp_ event, which is delayed to pressing down the key. The reason for that lies in the structure of the LCL, as the control (here the _TMemo_) when receiving a key, first checks if it should be forwarded to an event handler, then fires the event and just after that performs its own actions which in this case is adding a character and/or moving the cursor. 

As last functionality we want the labels in the bottom to be hideable by the user. Therefore a _TCheckbox_ already was placed on the form. Create the _OnChange_ event for the checkbox: 
    
    
    procedure TForm1.CheckBoxDetailsChange(Sender:TObject);
    begin
      if CheckBoxDetails.Checked then begin
        PanelStatus.Visible:= true;
        Memo1.Height:= self.Height - Memo1.Top - 28;
      end else begin
        PanelStatus.Visible:= false;
        Memo1.Height:= self.Height - Memo1.Top - 8;
      end;
    end;
    

By hiding the panel also its child controls, both labels, are hidden, i.e. are not seen on the form anymore. With the properties _Left_ , _Top_ , _Width_ and _Height_ the position and size of a control can be changed, here only the height shall be increased, when the area below the memo is not needed for the labels. Here the self keyword is used, which represents the current instance, for which the procedure was invoked. In this case that is _Form1_. 
    
    
      Memo1.Height:= self.Height - Memo1.Top - 8;
    

**Self** can be just omitted, its exactly the same as 
    
    
      Memo1.Height:= Height - Memo1.Top - 8;
    

All functionality is included now, the last thing is that at startup of the program before loading the labels names, the default text of the memo and the panel's name is shown. You could tweak each caption in the object inspector, but e.g. a blank label and panel is (nearly) invisible on the form editor, so we prefer to do these settings when the program is started. Therefore you should create an event handler for the OnCreate event of the form. 
    
    
    procedure TForm1.FormCreate(Sender:TObject);
    begin
      Memo1.Clear;
      LabelMemoChanges.Caption:= '';
      LabelCursorPos.Caption:= '';
      PanelStatus.Caption:= '';
      PanelStatus.BevelOuter:= bvNone;
    
    end;
    

The _OnCreate_ event handler is a good place for all kinds of initialization stuff at program start up, as the components at that moment already exist. 

We're done here, the running application should look like in the picture. It's up to you now to implement and distinguish between 'save' and 'save as', to save the font settings (e.g. one could use _SessionProperties_ or _TIniFile_) to use a proper menu bar (_TMainMenu_) and so on. 

[![EasyEditor03.png](https://wiki.freepascal.org/images/c/c7/EasyEditor03.png)](</File:EasyEditor03.png>)

## Learning the language

As you could see in the code the graphic libraries heavily depend on object oriented language support. Usage and especially reading the user code is intuitive, but for writing applications it is necessary to be familiar with the language details of FreePascal. This topics couldn't be covered in this tutorial. 

## Project Files

When you start a new project, the IDE will create several files in the project folder. The .lps-file contains all project information excluding source code and form settings. 

The file unit1 you already know, its the source file we worked with in the tutorial. It actually is the file for _Form1_. If you create another form (menu _file_ -> _new_ -> _form_), it will have its own unit. Each form has a .lfm file with the same name as the form, there the components populated on the form and all their settings are saved. 

You may have recognized that the units don't have any entry point, where the program starts. They just contain procedures, functions and data, which could be called or accessed from outside. The entry and exit point of the program is in the ".lpr" file, the core of the program. 

## LCL Internals

In the (very compact) .lpr-file you can see several calls to functions of the class _Application_. This class is part of the LCL and contains the program structure. Open the lpr file of your project (via menu file -> open OR via the project inspector, where all source files are listed). All the magic of a Lazarus application is called in the three lines: 
    
    
      Application.Initialize;
      Application.CreateForm(TForm1,Form1);
      Application.Run;
    

**Application.Initialize** speaks for it self, all the initialization stuff for the program is done there, except the form initializations, which are done seperately for each form afterwards. **Application.CreateForm** will use the .lfm-file to populate and initialize the components on the form. The form's .lfm-file is compiled into the executable, so you could share the application binary without any other file. At that point all initializations are finished. 

**Application.Run** contains the Main Loop of the program. A lazarus program is event based, that means the program litererally does nothing until an event, like a mouse click occurs. The event handling is done in the main loop, a _repeat ... until_ structure that is endlessly looped until the program is closed. Events on application level are called messages. The application class contains a message queue. When the operating system detects e.g. a mouse click that is associated with the application, i.e. the mouse position is on the application form, then a message telling the mouse click, is queued in the message queue. What the application main loop actually does, is polling this message queue, dequing the first message, processing it, i.e. calling the associated message handler doing its stuff, dequeueing the next message and so on, until all messages from the queue are processed. Note that the event handlers we commonly use, like _OnClick_ and so on, are triggered by the control's event handler, triggered by the message handler, so there are several levels of handlers. 

After all messages are processed the program returnes the task for a defined time to the operating system for not wasting CPU-time. When that waiting time is over (around 10ms depending on the system), the operating system again schedules CPU time for our application, running the next cycle in the main loop checking if some messages were enqueued in the meantime. Processing the messages and returning CPU-time to the OS. Note that a message often triggers other messages. E.g. if you change the size of an other control in your _ButtonOnClick_ event (trigered by a mouse message), then the size is not directly changed on the screen, but the code only changes the size values in the controls class and requests a repaint of the control. That request is done via a message that is enqueued. All that means, that when an event handler is executed, the application cannot do anything else in the meantime, so keep your event handlers short. If you perform a heavy calculation in e.g. a button's OnClick event that takes several seconds of time, then no clicks to any control can be performed, even the application can't be closed regularly. In that time the application is not responsive (and the operating system may tell so). Obviously an application behaving like this would be considered broken. 

A quick-and-dirty solution could be to intermediately use **Application.ProcessMessages**. The usage, when its appropriate and other measures you can learn in the multithreading application tutorial (beware, advanced topic). 

## Debugging

In this tutorial you could mostly just copy the code from here what shouldn't result in errors, crashes and so on. If you write your own application this obviously is different. You have several options in Lazarus to to make debugging easier including a live debugger. 

By default the debugger is enabled but a lot of runtime checks are disabled. Open the _Project settings_ -> _Compiler settings_ -> _Debugging_. In the very top you can choose a compile mode. On default there is only one mode, called Default. Click the ... ellipsis and press the button "_Create Debug and Release Modes_ ". Now you have three modes, you can delete the _Default_ mode and set the mode to _Debug_. When you go back to the debug settings, you will see, that a lot of checkboxes are checked now, also debugging is enabled. When you compile and run the application (F9) then the debugger will be started automatically. Breakpoints can be defined in the code editor by pressing F5 or by clicking in the left border beside the line. When running the application the code execution will be paused in the moment the line of code is reached. In the paused state you can check contents of variables by hovering with the mouse over the variable in the code editor. In this state you can also check the call trace, i.e. from which functions the function that the breakpoint was set in, was called. This is especially helpful when trying to understand third party libraries, but also when a function is called from more than one place in your program and you are not sure, where the error occured. Open the call stack in the menu _view_ -> _debugger windows_ -> _call stack_. By pressing F9 or clicking Run, the code execution will continue, you can also perform single steps and so on. 

If you experience a program crash and the debugger is enabled, Lazarus will show you the line of code where the crash happened. Sometimes just an "assembly" window opens, then the debugger doesn't have information about the line number, which is the case when the access violation took place in library code that wasn't compiled with enabled debug options, which is the case for the LCL. The fatal memory access (although being in the library code) could have its origin in user code. So here again the call stack may help you identifying which procedure called the library code that crashed with e.g. invalid call parameters. 

In the debug mode the checkbox for using the _heaptrace_ unit is checked, this is useful to track the usage of dynamic memory. The _heaptrace_ unit will compare all allocated and disallocated memory throughout the program execution and show you if allocated memory is left in the end. If this is the case, you know that you didn't fully clean up your allocations. For the LCL components you don't need to take care, the LCL does. But if you create class instances manually, you have to make sure to destroy them after usage. Although the operating system cleans up after closing the application anyways, memory leaks can make long time running programs unstable. The notifications of the memory usage may be annoying sometimes, then just disable heap tracing in the debugging options. 

Often it is useful to output short statements to show what the program is and was doing. One possibility is to just write into a file for that purpose. If you want to directly see the outputs you may use the standard-out instead. Under Linux there always is a standard output. When running the program in the IDE you can see the output written by **Write** /**WriteLn** in the window _Debug Output_ (menu _view_ -> _debugger windows_ -> _Debug Output_). In the _ButtonFileOpenClick_ event you may add a short write statement: 
    
    
    procedure TForm1.ButtonFileOpenClick(Sender:TObject);
    begin
      WriteLn('TForm1.ButtonFileOpenClick');
      [...]
    

If the program is started in the shell instead through Lazarus, then the output will be shown there. 

Under Windows you can add the compiler directive **{$APPTYPE CONSOLE}** in the very top of the projects lpr under the 'program' line: 
    
    
    program EasyEditor;
    {$APPTYPE CONSOLE} 
    ...
    

In contrast to the name it is still a GUI application but with additional console window, where your writes (using Write or WriteLn) will be displayed. Under Linux this compiler directive has no effect and does no harm either. 

  


## Code completion

The Lazarus IDE can help you to save a lot of typing and also simplifies coding by code completion. See [Lazarus IDE Tools](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools"). 

## See also

  * [Lazarus Tutorial](<Lazarus_Tutorial.md> "Lazarus Tutorial") Part 1
  * [Lazarus Form Tutorial](<Form_Tutorial.md> "Form Tutorial")
  * [Howdy World (Hello World on steroids)](<Howdy_World_\(Hello_World_on_steroids\).md> "Howdy World \(Hello World on steroids\)")
  * [Peg Solitaire tutorial](<Peg_Solitaire_tutorial.md> "Peg Solitaire tutorial")
  * [Lazarus IDE Tools](<Lazarus_IDE_Tools.md> "Lazarus IDE Tools")
  * [Lazarus project files](<Lazarus_project_files.md> "Lazarus project files")

---

_Source: [https://wiki.freepascal.org/Lazarus_Tutorial_Part_2](https://web.archive.org/web/20241206064547/https://wiki.freepascal.org/Lazarus_Tutorial_Part_2)_
