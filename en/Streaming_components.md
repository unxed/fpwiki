# Streaming components

│ **English (en)** │

## Contents

  * 1 Introduction
  * 2 TComponent / TPersistent
  * 3 TReader / TWriter
  * 4 Streaming Collections
  * 5 Writing your own component - Part 1
  * 6 Writing a component to a stream
  * 7 Reading a component from a stream
  * 8 Streamable properties
  * 9 Streaming custom Data - DefineProperties
  * 10 Writing your own component - Part 2
  * 11 Writing and Reading components from/to LFM
  * 12 Writing and Reading components from/to XML
  * 13 Names
  * 14 Conclusion
  * 15 See also



## Introduction

Normally, when you want to store data on disk or to a network stream, you have to write code which loads and saves each property. This tutorial describes how to write classes that can be loaded from and saved to streams using RTTI (RunTime Type Information) without the need to write extra load/save code. 

The Lazarus sources include an example which demonstrates how to save a TGroupBox with a TCheckBox child to a stream, and how to read the stream back to recreate a copy of both components. See `<lazaruspath>/examples/componentstreaming/`. 

By combining use of appropriate [RTTI controls](<RTTI_controls.md> "RTTI controls") you can minimise the amount of code needed to connect your program's GUI with corresponding Data from Disk/Network storage. 

## TComponent / TPersistent

The class **TPersistent** is defined in the unit Classes and it uses the **{$M+}** compiler switch. This switch tells the compiler to create Run Time Type Information (**RTTI**). This means classes in this unit and all their descendants get a new **published** class section. 'Published' properties are as visible as 'public' properties, but additionally their structure is accessible at run time. That means all published properties can be read and written at run time. For example, the IDE uses RTTI to work with components it otherwise does not know of. 

**TComponent** extends TPersistent with the ability to have child components. This is important for streaming, where one component is the **root component** (also called the **lookup root**) holding a list of child components. 

## TReader / TWriter

TReader and TWriter are worker classes, which read or write any TComponent to or from a stream (See CreateLRSReader and CreateLRSWriter). They use a **Driver** to read/write using a special data format. At the moment there is a reader (TLRSObjectReader) and a writer (TLRSObjectWriter) for binary object format defined in the LResources unit, and a writer (TXMLObjectWriter) for TDOMDocument defined in Laz_XMLStreaming. The LResources unit also contains functions to convert the custom binary data format to text and back (LRSObjectBinaryToText, LRSObjectTextToBinary). The LCL prefers UTF8 for strings, while Delphi prefers Widestrings. So there are some conversion functions provided as well so you can deal easily with streaming component data not only in Lazarus but using the Delphi binary format too. 

## Streaming Collections

See it here [TCollection#Streaming](<TCollection.md> "TCollection")

This is a full example how to create a list of items using the classes **TCollectionItem** , **TCollection** and stream it using **TComponent**. 

## Writing your own component - Part 1

A custom component can be as simple as: 
    
    
    type
      TMyComponent = class(TComponent)
      private
        FID: integer;
      published
        property ID: integer read FID write FID;
      end;
    

## Writing a component to a stream

The unit [LResources](<http://lazarus-ccr.sourceforge.net/docs/lcl/lresources> "doc:lcl/lresources") has a function for that: 
    
    
    procedure WriteComponentAsBinaryToStream(AStream: TStream; AComponent: TComponent);
    

It writes a component in binary format to the stream. For example: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      AStream: TMemoryStream;
    begin
      AStream:=TMemoryStream.Create;
      try
        WriteComponentAsBinaryToStream(AStream, AGroupBox);
        ... save stream somewhere ...
      finally
        AStream.Free;
      end;
    end;
    

## Reading a component from a stream

The unit LResources has a function for that: 
    
    
    procedure ReadComponentFromBinaryStream(AStream: TStream;
        var RootComponent: TComponent; OnFindComponentClass: TFindComponentClassEvent; TheOwner: TComponent = nil);
    

  * AStream is the stream containing **one** component in binary format. Everything behind that component in the stream is not read, including other components.
  * RootComponent is either an existing component, which data will be overwritten, or it is nil and a new component will be created.
  * OnFindComponentClass is a function, that is used by TReader to get the class from the classnames in the stream. For example:


    
    
    procedure TCompStreamDemoForm.OnFindClass(Reader: TReader;
      const AClassName: string; var ComponentClass: TComponentClass);
    begin
      if CompareText(AClassName, 'TGroupBox') = 0 then
        ComponentClass := TGroupBox
      else if CompareText(AClassName, 'TCheckBox') = 0 then
        ComponentClass := TCheckBox;
    end;
    

  * TheOwner is the component owner, when creating a new component.



## Streamable properties

TReader and TWriter have several limitations on what types they can stream: 

  * All basic Pascal types can be streamed: string, integer, char, single, double, extended, byte, word, cardinal, shortint, method pointers, etc.


  * Any TPersistent class and any TPersistent descendant can be streamed


  * Records, objects and classes not descending from TPersistent cannot be streamed without extending existing TReader/TWriter methods. To stream records or non-TPersistent classes and objects you need to override certain TReader/TWriter methods. See below #Streaming custom Data - DefineProperties.



## Streaming custom Data - DefineProperties

You can stream additional arbitrary data by overriding DefineProperties. This allows you to stream data that is not a basic Pascal type, and classes that are not TPersistent descendants. For example to stream a record variable **FMyRect: TRect** which is a field in your component, add the following three methods to your component: 
    
    
    procedure DefineProperties(Filer: TFiler); override;
    procedure ReadMyRect(Reader: TReader);
    procedure WriteMyRect(Writer: TWriter);
    

With the following code: 
    
    
    procedure TMyComponent.DefineProperties(Filer: TFiler);
    var
      MyRectMustBeSaved: Boolean;
    begin
      inherited DefineProperties(Filer);
      MyRectMustBeSaved := (MyRect.Left <> 0)
                         or (MyRect.Top <> 0)
                         or (MyRect.Right <> 0)
                         or (MyRect.Bottom <> 0);
      Filer.DefineProperty('MyRect', @ReadMyRect, @WriteMyRect, MyRectMustBeSaved);
    end;
    
    procedure TMyComponent.ReadMyRect(Reader: TReader);
    begin
      with Reader do begin
        ReadListBegin;
        FMyRect.Left := ReadInteger;
        FMyRect.Top := ReadInteger;
        FMyRect.Right := ReadInteger;
        FMyRect.Bottom := ReadInteger;
        ReadListEnd;
      end;
    end;
    
    procedure TMyComponent.WriteMyRect(Writer: TWriter);
    begin
      with Writer do begin
        WriteListBegin;
        WriteInteger(FMyRect.Left);
        WriteInteger(FMyRect.Top);
        WriteInteger(FMyRect.Right);
        WriteInteger(FMyRect.Bottom);
        WriteListEnd;
      end;
    end;
    

This will save MyRect as a property 'MyRect'. 

If you stream a lot of TRect fields, then you probably don't want to repeat this code every time. The unit LResources contains an example of how you can write a procedure that defines a rect property: 
    
    
    procedure DefineRectProperty(Filer: TFiler; const Name: string; ARect, DefaultRect: PRect);
    

Having written a procedure to define a rect property, the above code can be reduced to: 
    
    
    procedure TMyComponent.DefineProperties(Filer: TFiler);
    begin
      inherited DefineProperties(Filer);
      DefineRectProperty(Filer, 'MyRect', @FMyRect, nil);
    end;
    

## Writing your own component - Part 2

Now the example can be extended and we can use arbitrary properties with only a few lines of code: 
    
    
    type
      TMyComponent = class(TComponent)
      private
        FID: integer;
        FRect1: TRect;
        FRect2: TRect;
      protected
        procedure DefineProperties(Filer: TFiler); override;
      public
        property Rect1: TRect read FRect1 write FRect1;
        property Rect2: TRect read FRect2 write FRect2;
      published
        property ID: integer read FID write FID;
      end;
    
    procedure TMyComponent.DefineProperties(Filer: TFiler);
    begin
      inherited DefineProperties(Filer);
      DefineRectProperty(Filer, 'Rect1', @FRect1,nil);
      DefineRectProperty(Filer, 'Rect2', @FRect2,nil);
    end;
    

This component can now be saved, loaded or used by the [RTTI controls](<RTTI_controls.md> "RTTI controls"). You don't need to write any further code. 

## Writing and Reading components from/to LFM

See unit lresources function ReadComponentFromTextStream and WriteComponentAsTextToStream for examples. 

## Writing and Reading components from/to XML

Streaming components is simple: See the example in lazarus/examples/xmlstreaming/. 

## Names

  * All components of one component (Owner) must have distinct names. So two forms owned by applications must have distinct names. And two labels on a form must have distinct names. But two labels on two different forms can have the same name. And a form can have the same name as one of its children.


  * TComponent.Name can be empty and you can have more than one component without a name. TWriter will write it, but TReader will not find the component and reading will fail. Therefore the IDE's Object Inspector does not allow that.


  * Names must be valid Pascal identifiers [IsValidIdent](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/isvalidident.html>).


  * When referencing other forms: All root components (forms, datamodules, ...) referenced by other (forms,etc) must have unique names. They don't need to be owned by the application, but then the programmer himself must make sure the names are unique. Forms and Datamodules are found via the Screen object, where all forms and datamodules register themselves automatically.


  * You can create many forms with the name Form1, for example via _TForm1.Create(nil)_. If a _Form2_ references a _Form1.OpenDialog_ , then the first _Form1_ in _Screen_ is used.


  * A _Form1_ and an embedded frame can have both a child _Label1_. When the _Label1_ is referenced then it should be unique on the whole form including all embedded frames. So it is recommended to give all components unique names.


  * Global fixup: TReader reads a component stream. If it finds an embedded frame a second TReader is created, which reads the frame stream. Then it returns and continues. References to other components (e.g. _Form1.Button1_) are saved to a global fixup list in the unit classes (see [GetFixupReferenceNames](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/getfixupreferencenames.html>)). The references are fixed up after reading.


  * TReader and TWriter use the special name _Owner_ to refer to the current Owner.



## Conclusion

RTTI is a powerful mechanism for streaming entire classes easily. RTTI also helps you to avoid repeatedly writing a lot of boring load/save code. 

## See also

  * [RTTI controls](<RTTI_controls.md> "RTTI controls")
  * [OpenGL Tutorial](<OpenGL_Tutorial.md> "OpenGL Tutorial")

---

_Source: [https://wiki.freepascal.org/Streaming_components](https://web.archive.org/web/20250324174952/https://wiki.freepascal.org/Streaming_components)_
