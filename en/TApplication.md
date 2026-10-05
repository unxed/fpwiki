# TApplication

│ **English (en)** │

**TApplication** (with instance Application) that is available in every Lazarus program. 

Unlike [forms](<TForm.md> "TForm"), and components on the forms a (single) instance of TApplication is created before any user line of code. In a .lpr file (a pascal file containing the [keyword](<Reserved_words.md> "Reserved words") '[program](<Program.md> "Program")' ) the Application-variable is used to create the applications form(s): 
    
    
    program pMyTest;
    uses
      Forms, uMyForm;
    
    {$R *.res}
    
    begin
      Application.Initialize();
      Application.CreateForm( TMyForm, MyForm );
      Application.Run();
    end.
    

`TMyForm` is assumed a descendant of [TForm](<TForm.md> "TForm") and defined in `uMyForm.pas`

## Properties

Properties  Property | type | Writable | Description   
---|---|---|---  
ExeName | string | - | Name of the executable.   
HelpFile | string | rw | Location of the application help file.   
Terminated | Boolean | - | Was Terminate called or not   
Title | string | rw | Application title   
ConsoleApplication | Boolean | - | Is the application a console application or not   
Location | string | - | Application location   
Params[] | string | - | Command-line parameters   
ParamCount | Integer | - | Number of command-line parameters   
EnvironmentVariable[] | string | - | Environment variable access   
OptionChar | Char | rw | Command-line switch character   
CaseSensitiveOptions | Boolean | rw | Are options interpreted case sensitive or not   
StopOnException | Boolean | rw | Should the program loop stop on an exception   
ExceptionExitCode | LongInt | rw | ExitCode to use then terminating the program due to an exception   
EventLogFilter | TEventLogTypes | rw | Event to filter events, before they are sent to the system log   
SingleInstance | TBaseSingleInstance | - | Single instance used to control single application instance behaviour   
SingleInstanceClass | TBaseSingleInstanceClass | rw | Class to use when creating single instance   
SingleInstanceEnabled | Boolean | rw | Enable single application instance control.   
Active | Boolean |  | True if the application has the focus.   
ApplicationType | TApplicationType | rw | The type of the application (i.e. the sort of device for which it is intended).   
BidiMode | TBiDiMode | rw | The bidirectional mode to use.   
CaptureExceptions | Boolean | rw | Enables exception handling by Application.   
DoubleBuffered | TApplicationDoubleBuffered | rw | platform   
ExtendedKeysSupport | Boolean | rw |   
ExceptionDialog | TApplicationExceptionDlg | rw |   
FindGlobalComponentEnabled | Boolean | rw | Enables searching for global components by name.   
Flags | TApplicationFlags | rw | Application state flags.   
Handle | THandle | rw | platform ;   
Hint | string | rw | The current Hint text.   
HintColor | TColor | rw | The color of Hint windows.   
HintHidePause | Integer | rw | How long a Hint stays visible, after an mouse move.   
HintHidePausePerChar | Integer | rw | How much a keystroke extends the time a Hint stays visible (added to HintHidePause).   
HintPause | Integer | rw | The delay before a Hint is shown.   
HintShortCuts | Boolean | rw | Enables the display of keyboard shortcuts (e.g. on menu entries).   
HintShortPause | Integer | rw | The pause before a different Hint is displayed.   
Icon | TIcon | rw | The icon associated with this application.   
LayoutAdjustmentPolicy | TLayoutAdjustmentPolicy | rw |   
Navigation | TApplicationNavigationOptions | rw | Allows to switch between controls by pressing keys.   
MainForm | TForm |  | The application terminates when this form is closed.   
MainFormHandle | HWND |  | the Handle of the MainForm   
MainFormOnTaskBar | Boolean | rw | platform   
ModalLevel | Integer |  | The number of modal forms shown.   
MoveFormFocusToChildren | Boolean | rw |   
MouseControl | TControl |  | The control currently under the mouse.   
TaskBarBehavior | TTaskBarBehavior | rw | How forms are represented in the TaskBar.   
UpdateFormatSettings | Boolean | rw | platform;   
Scaled | Boolean | rw |   
  
## Events

Events  Event | type | Description   
---|---|---  
OnException | TExceptionEvent | Exception handling event   
OnActionExecute | TActionEvent | Default handler for unhandled Actions   
OnActionUpdate | TActionEvent | Default handler for unhandled Action updates   
OnActivate | TNotifyEvent | activation of the application.   
OnDeactivate | TNotifyEvent | deactivation of the application.   
OnGetMainFormHandle | TGetHandleEvent |   
OnIdle | TIdleEvent | for application entering Idle state.   
OnIdleEnd | TNotifyEvent | for application leaving Idle state.   
OnEndSession | TNotifyEvent | for ending a session of the application   
OnQueryEndSession | TQueryEndSessionEvent | can deny to end the session.   
OnMinimize | TNotifyEvent | for application minimized.   
OnMessageDialogFinished | TModalDialogFinished |   
OnModalBegin | TNotifyEvent | for a modal form opened (application entering modal mode).   
OnModalEnd | TNotifyEvent | all modal forms closed (application exiting modal mode).   
OnRestore | TNotifyEvent | for application restored (from minimized/maximized state).   
OnDropFiles | TDropFilesEvent | The default handler for dropped files   
OnHelp | THelpEvent | when help is requested.   
OnHint | TNotifyEvent | when a hint is requested.   
OnShortcut | TShortCutEvent | interpret keystrokes as shortcuts, before the normal target handles the key.   
OnShowHint | TShowHintEvent | showing a Hint.   
OnUserInput | TOnUserInputEvent |   
OnDestroy | TNotifyEvent |   
OnCircularException | TExceptionEvent | Handler invoked when there is an exception circle before killing the app with Halt   
ShowButtonGlyphs | TApplicationShowGlyphs | Allows to override the platform default for glyphs on buttons.   
ShowMenuGlyphs | TApplicationShowGlyphs | Sets the default policy for showing or hiding glyph images in menus   
ShowHint | Boolean | Enables Hint display   
ShowMainForm | Boolean | Allows to show or hide the main form, on application start. Default TRUE.   
  
## see also

  * [TCustomApplication](</index.php?title=TCustomApplication&action=edit&redlink=1> "TCustomApplication \(page does not exist\)")
  * [TApplicationProperties](<TApplicationProperties.md> "TApplicationProperties")

---

_Source: [https://wiki.freepascal.org/TApplication](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/TApplication)_
