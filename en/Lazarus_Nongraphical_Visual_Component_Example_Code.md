# Lazarus Nongraphical Visual Component Example Code

## Contents

  * 1 Introduction
  * 2 Code to copy and paste
  * 3 Download
  * 4 Author
  * 5 License
  * 6 See also



### Introduction

There are many property types in Lazarus, and when writing a descendent of TComponent it is not always straightforward how to implement some of them. After searching many sources I have written a component template that includes most property types. 

The code below is a working component that could be packaged and compiled in the IDE, but its purpose is to serve as a source of copy/paste snippets. 

Here's what it looks like in the Lazarus Object Inspector: 

[![myvisualcomponent properties11.png](https://wiki.freepascal.org/images/5/54/myvisualcomponent_properties11.png)](</File:myvisualcomponent_properties11.png>)

### Code to copy and paste
    
    
    unit MyCustomControl;
    
    {
    = Example of a generic non-graphical component for Lazarus 1.x
    = - example code for various types of properties and events to show and act correctly in the Object Inspector
    = - including a custom 'About' dialog property editor
    =
    = Although this unit will compile as written, the purpose is to provide template code
    = for various property types and events that can be copy/pasted into your own component
    =
    = Example Property types:
    =  1) Bitmap
    =  2) Font
    =  3) Icon
    =  4) Stringlist
    =  5) Options expandable true/false list
    =  6) User type drop-down list
    =  7) Simple String
    =  8) String with pre and post-processing
    =  9) String Array with indexed properties
    = 10) String with default value
    = 11) Integer with default value
    = 12) Overridden (custom) Tag property
    = 13) Inherited drop-down list of types
    = 14) Custom dialog
    = 15) Custom events
    = 16) Property hidden from the Object Inspector
    = 17) Filename property with custom properties for the OpenDialog
    =
    = Author: minesadorada@charcodelvalle.com
    = Date: May 2014
    = License: LGPL
    =
    }
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Buttons,
      LCLIntf, LCLType, LResources,
      ExtCtrls, StdCtrls, Dialogs, PropEdits;
    
    const
      C_VERSION = '1.1'; // Remember to co-ordinate with the Package manager VersionInfo
    
      C_ERRORMESSAGE = '%s error:' + LineEnding + '%s';
    // Used with Exception.CreateFmt
    
    type
      // Drop-down list in Object Inspector
      tmcType = (mcType1, mcType2, mcType3);
    
      // Expandable list of Options in Object Inspector
      tOptions = (Opt1, Opt2, Opt3);
      tOptionsFlags = set of tOptions;
    
      tAboutString = string; // Unique string type used in 'About' property editor
      tFilenameString = string; // Unique string type used in MyFilename property editor
    
      TSampleEvent = procedure(MyText: string) of object;  // Custom Event type
    
      TMyVisualComponent = class(TComponent) // Non-graphical ancestor
      private
        { Private declarations }
        fAboutString: string; // Dummy string for 'About' property
        fFileNameString: tFilenameString; // Filename
        fmcType: tmcType; // Type defined above
        fSimpleString: string; // Direct read and write (no methods)
        fProcessedString: string; // Methods used to read and write the property
        fStringWithDefault: string; // Object Inspector shows default
        fIntegerWithDefault: integer; // Object Inspector shows default
        fTag: string; // Property overrides normal Tag property
        fOptions: tOptionsFlags; // Variable of Set type. * If Opt1 IN fOptions then...
        fIcon: TIcon; // Assigned to Application Icon by default
        fFont: TFont; // Assigned only if MyFont property set
        fBitMap: TBitMap; // Assigned only if MyBitmap property set
        fSizeConstraints: TSizeConstraintsOptions;
        // Built-in set (part of TSizeConstraints object)
        fStringArray: array[0..3] of string;
        // Stores values of String1,String2,String3 and String4 properties
        fStringList: TStrings; // Holds MyStringList property values
        fVersion: string; // Holds read-only version property
        FOnSample: TSampleEvent; // Custom event
    
        fHiddenString: string; // Hidden property
        fOnChangeHiddenString: TNotifyEvent; // Custom event
    
        { Private methods }
    
        procedure SetProcessedString(AValue: string);
        function GetProcessedString: string;
        // Read and Write procedures for property ProcessedString
    
        procedure SetTag(AValue: string); // Write procedure for overridden property 'Tag'
    
        function GetStringValue(const AIndex: integer): string;
        // Indexed Get Method
        procedure SetStringValue(const AIndex: integer; AValue: string);
        // Indexed Set method
    
        procedure SetStrings(const AValue: TStrings);
        // Needed to use a TStrings property.  FStringList also needs to be created in the Constructor
    
        procedure SetFont(const AValue: TFont);
        // Needed to use a Font property. fFont also needs to be created in the Constructor
    
        procedure SetBitMap(const AValue: TBitmap);
        // Needed to use a BitMap property. fBitMap also needs to be created in the Constructor
    
        // Sets the HiddenString property
        procedure SetHiddenString(const AValue: string);
      protected
        { Protected declarations }
        // Can be used if you plan to subclass this component
      public
        { Public declarations }
        constructor Create(AOwner: TComponent); override; // Constructor must be public
        destructor Destroy; override; // Destructor must be public
    
        { Public properties }
        // This section used for properties you don't want to display in the Object Inspector (or array properties)
        property HiddenString: string read fHiddenString write SetHiddenString;
        // Public property hidden from the Object Inspector. * SetHiddenString method can trigger custom event OnChangeHiddenString
      published
        {
        Published declarations (Displayed in the Object Inspector)
        Note: Comments immediately above the property declarations are displayed in the Lazarus Object Inspector lower pane
        }
    
        // The custom property editor TAboutPropertyEditor is used in the Object Inspector
        property About: tAboutString read fAboutString; // Example of custom 'About' dialog
    
        // Example of drop-down list in the Object Inspector
        // Default is displayed in different font and is set in Constructor
        property MyTypeList: tmcType read fmcType write fmcType default mcType2;
    
        // Direct access read + write
        property SimpleString: string read fSimpleString write fSimpleString;
    
        // Example of using Read and Write methods
        property ProcessedString: string read GetProcessedString write SetProcessedString;
    
        // Example of pseudo-default property of a string (value is set in Constructor)
        property StringWithDefault: string read fStringWithDefault write fStringWithDefault;
    
        // Example of an expandable list of booleans in the Object Inspector
        property Options: tOptionsFlags read fOptions write fOptions;
    
        // Browse for Icon dialog in the Object Inspector. * No need to Create fIcon in constructor
        property MyIcon: TIcon read fIcon write fIcon;
    
        // Browse for Font dialog in the Object Inspector. * fFont needs to be created in Constructor
        property MyFont: TFont read fFont write SetFont; // * Note the SetFont method
    
        // * Note the SetBitmap method
        // Browse for Bitmap dialog in the Object Inspector. * fBitmap needs to be created in Constructor
        property MyBitMap: TBitmap read fBitMap write SetBitmap;
    
        // Showing a built-in list of booleans in the Object Inspector
        property MySizeConstraints: TSizeConstraintsOptions
          read fSizeConstraints write fSizeConstraints;
    
        // Default is displayed in different font but has to be set to the value 2 in Constructor
        property IntegerWithDefault: integer read fIntegerWithDefault
          write fIntegerWithDefault default 2; // Example of integer default
    
        // The method is passed the index as a parameter
        // Example of using an indexed read and write method to store strings in an array
        property String1: string index 1 read GetStringValue write SetStringValue;
        // Example of using an indexed read and write method to store strings in an array
        property String2: string index 2 read GetStringValue write SetStringValue;
        // Example of using an indexed read and write method to store strings in an array
        property String3: string index 3 read GetStringValue write SetStringValue;
        // Example of using an indexed read and write method to store strings in an array
        property String4: string index 4 read GetStringValue write SetStringValue;
    
        // Dialog shows in the Object Inspector. * fStringlist must be Created in the constructor.  * Note the SetStrings override
        // String list example.
        property MyStringList: TStrings read fStringList write SetStrings;
    
        // Example of a file open dialog property
        property MyFilename: tFilenameString read fFilenameString write fFilenameString;
    
        // Tag property will now only accept a single alpha character
        // Example of overriding an ancestor (TComponent's) Tag property
        property Tag: string read fTag write SetTag;
    
        // No need to specify type, read method or write method
        // Example of publishing a public property from an ancestor (TComponent)
        property ComponentState;
    
        // ReadOnly property
        property Version: string read fVersion;
    
        // Example of Custom Event that does nothing
        property OnSample: TSampleEvent read FOnSample write FOnSample;
    
        // This custom event is always triggered by a change in the value of HiddenString property
        property OnChangeHiddenString: TNotifyEvent
          read fOnChangeHiddenString write fOnChangeHiddenString;
      end;
      // Declaration for the 'About' property editor
      TAboutPropertyEditor = class(TClassPropertyEditor)
      public
        procedure Edit; override;
        function GetAttributes: TPropertyAttributes; override;
        function GetValue: string; override;
      end;
      // Declaration for 'MyFilename' property editor
      // (See PropEdits unit for a list of property editors to modify)
      TMyFileNamePropertyEditor = class(TFileNamePropertyEditor)
      public
        // Override the Edit method for total control
        function GetFilter: string; override;
        function GetDialogOptions: TOpenOptions; override;
        function GetDialogTitle: string; override;
      end;
    
    procedure Register; // Must be declared here just before implementation
    
    implementation
    
    procedure Register;
    begin
      {$I myvisualcomponent_icon.lrs}
      // Register this component into the IDE on the 'Additional' component palette
      RegisterComponents('Additional', [TMyVisualComponent]);
    
      // Register the custom property editor for the 'About' property
      RegisterPropertyEditor(TypeInfo(tAboutString),
        TMyVisualComponent, 'About', TAboutPropertyEditor);
      // Register the custom property editor for the 'MyFilename' property
      RegisterPropertyEditor(TypeInfo(tFilenameString),
        TMyVisualComponent, 'MyFilename', TMyFileNamePropertyEditor);
    
      // Note the TypeInfo parameter is made into a unique string type
      // (defined earlier in this unit)
      // so that the regular property editors in other components don't get confused
      // You could simply use TypeInfo(String)
    end;
    
    constructor TMyVisualComponent.Create(AOwner: TComponent);
      // Called when form is loaded into the IDE
    begin
      inherited Create(AOwner);
      // Initialisation goes here
      // Set any properties to their defaults and intialise objects
      fmcType := mcType2; // because this was the default value specified
      fStringWithDefault := 'Default String'; // Unspecified default
      fIntegerWithDefault := 2; // Default value specified
      fTag := 'A'; // Unspecified default
      fIcon := Application.Icon; // Unspecified default
      fFont := TFont.Create; // Needs to be created here for Object Inspector to show options
      fBitmap := TBitMap.Create;
      // Needs to be created here for Object Inspector to show the bitmap dialog
      fStringList := TStringList.Create;
      // Needs to be created here for Object Inspector to show dialog
      fVersion := C_VERSION;
      // Assign read-only property
    end;
    
    destructor TMyVisualComponent.Destroy;
    begin
      // Clean-up code goes here
      // FreeandNil any user-created objects here
      FreeAndNil(fBitmap);
      FreeAndNil(fStringList);
      FreeAndNil(fFont);
      inherited Destroy;
    end;
    
    
    // == START PROPERTY EDITOR CODE ==
    procedure TAboutPropertyEditor.Edit;
    // Shows a dialog when About property is double-clicked
    var
      tAboutForm: TForm;
      OKbutton: TBitBtn;
      lbl_Description: TLabel;
      sz: string;
    begin
      // Make up message string
      sz := 'My component for Lazarus' + LineEnding + 'by email@domain.com' +
        LineEnding + LineEnding;
      sz += 'Methods:' + LineEnding;
      sz += 'MyVisualComponent.Method1' + LineEnding;
      sz += 'MyVisualComponent.Method2' + LineEnding;
      sz += LineEnding + 'Version: ' + C_VERSION + LineEnding;
      sz += 'License: LGPL';
      // Create a new dialog
      tAboutForm := TForm.CreateNew(nil);
      try  //.. finally FreeAndNil everything
        with tAboutForm do
        begin
          // Set Form properties
          position := poScreenCenter;
          borderstyle := bsToolWindow;
          Caption := 'About My Component';
          formstyle := fsSystemStayOnTop;
          color := clSkyBlue;
          Height := 240;
          Width := 320;
          // Create a BitBtn button
          okbutton := TBitBtn.Create(tAboutForm);
          // Set BitBtn properties
          okbutton.Kind := bkClose;
          okbutton.left := (Width div 2) - okbutton.Width div 2;
          okbutton.top := Height - okbutton.Height - 10;
          okbutton.parent := tAboutForm;
          // Create a label control
          lbl_Description := Tlabel.Create(tAboutForm);
          // Set label properties
          lbl_Description.left := 8;
          lbl_Description.Top := 30;
          lbl_Description.Width := 304;
          lbl_Description.Height := 200;
          lbl_Description.Autosize := False;
          lbl_Description.Alignment := taCenter;
          lbl_Description.Caption := sz;
          lbl_Description.parent := tAboutForm;
          // Display the dialog modally
          ShowModal;
        end;
      finally
        // Free all resources
        FreeAndNil(lbl_Description);
        FreeAndNil(okbutton);
        FreeAndNil(tAboutForm);
      end;
    end;
    
    function TAboutPropertyEditor.GetAttributes: TPropertyAttributes;
      // Show the ellipsis
    begin
      Result := [paDialog];
    end;
    
    function TAboutPropertyEditor.GetValue: string;
      // Override standard string read method
    begin
      Result := '(Double-click)';
    end;
    
    function TMyFileNamePropertyEditor.GetFilter: string;
    begin
      Result := 'All Files|*.*|Bitmaps|*.bmp|JPegs|*.jpg';
    end;
    
    function TMyFileNamePropertyEditor.GetDialogOptions: TOpenOptions;
    begin
      // To see the full list, drop an OpenDialog onto a form and see the Options property
      Result := [ofFileMustExist, ofPathMustExist];
    end;
    
    function TMyFileNamePropertyEditor.GetDialogTitle: string;
    begin
      Result := 'My Custom Title';
    end;
    // == END PROPERTY EDITOR CODE ==
    
    // == PROPERTY GET/SETS ==
    procedure TMyVisualComponent.SetProcessedString(AValue: string);
    begin
      // Check to see if a change is necessary
      // Avalue can be amended before it is assigned to fProcessedString
      if (fProcessedString <> AValue) then
        fProcessedString := UpperCase(AValue);
    end;
    
    function TMyVisualComponent.GetProcessedString: string;
    begin
      // Do any error-checking or processing of fProcessedString here.
      Result := fProcessedString;
    end;
    
    procedure TMyVisualComponent.SetTag(AValue: string);
    // Overridden Tag property will only accept letters
    // If changed via Object Inspector to a non-alpha string, then show custom error message
    begin
      if (fTag <> AValue) and (Length(AValue) > 0) then
        if AValue[1] in ['A'..'Z'] + ['a'..'z'] then
          fTag := AValue[1]
        else
        if ComponentState = [csDesigning] then
          raise Exception.CreateFmt(C_ERRORMESSAGE,
            [Name, 'You can only set the tag property to an alpha value']);
      // 'Name' is the Name property of this TComponent instance
    end;
    
    function TMyVisualComponent.GetStringValue(const AIndex: integer): string;
    begin
      // Example of an indexed property Get method
      // Retrieve from private string Array
      Result := fStringArray[AIndex - 1];
    end;
    
    procedure TMyVisualComponent.SetStringValue(const AIndex: integer; AValue: string);
    // Example of an indexed property Set method
    // Value is stored in private string array
    begin
      if (fStringArray[AIndex - 1] <> AValue) then
        fStringArray[AIndex - 1] := AValue;
    end;
    
    procedure TMyVisualComponent.SetStrings(const AValue: TStrings);
    begin
      // this is correct statement
      FStringList.Assign(AValue);
      // this is not correct
      // FStrings := AValue;
    end;
    
    procedure TMyVisualComponent.SetFont(const AValue: TFont);
    begin
      // this is correct statement
      fFont.Assign(AValue);
      // this is not correct
      // fFont := AValue;
    end;
    
    procedure TMyVisualComponent.SetBitMap(const AValue: TBitmap);
    begin
      // this is correct statement
      fBitmap.Assign(AValue);
      // this is not correct
      // fBitmap := AValue;
    end;
    
    procedure TMyVisualComponent.SetHiddenString(const AValue: string);
    // If value is changed then triggers the custom events OnChangeHiddenString and OnSample
    begin
      if (fHiddenString <> AValue) then
      begin
        fHiddenString := AValue;
        // Trigger custom events
        if Assigned(fOnChangeHiddenString) then
          OnChangeHiddenString(Self);
        if Assigned(fOnSample) then
          OnSample(AValue);
      end;
    end;
    // Sample application code that uses the Custom Events as coded
    // Both are triggered by changing the HiddenString property in SetHiddenString
    {
    procedure TForm1.MyVisualComponent1ChangeHiddenString(Sender: TObject);
    begin
      ShowMessage('Hidden String was changed to something new');
    end;
    
    procedure TForm1.MyVisualComponent1Sample(MyText: String);
    begin
      ShowMessageFmt('Hidden String was changed to "%s"',[MyText]);
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      MyVisualComponent1.HiddenString:='Hello World';
    end;
    }
    end.
    

### Download

You can download this code as a package ready to install and play with [here](<http://www.charcodelvalle.com/myvisualcomponent/myvisualcomponent_code.zip>)

### Author

minesadorada@charcodelvalle.com 

### License

All code is released under the LGPL license 

## See also

  * [Components and code examples](<Components_and_Code_examples.md> "Components and Code examples")
  * [How to write a Lazarus component](<How_To_Write_Lazarus_Component.md> "How To Write Lazarus Component")
  * [Object-oriented programming with FreePascal and Lazarus](<Object_Oriented_Programming_with_FreePascal_and_Lazarus.md> "Object Oriented Programming with FreePascal and Lazarus")

---

_Source: [https://wiki.freepascal.org/Lazarus_Nongraphical_Visual_Component_Example_Code](https://web.archive.org/web/20250416152202/https://wiki.freepascal.org/Lazarus_Nongraphical_Visual_Component_Example_Code)_
