# TCollection

│ **[Deutsch (de)](</TCollection/de> "TCollection/de")** │  **English (en)** │  **[français (fr)](</TCollection/fr> "TCollection/fr")** │  **[polski (pl)](</TCollection/pl> "TCollection/pl")** │    
****

A **TCollection** is a base class for (unordered) collections of [TCollectionItems](</index.php?title=TCollectionItem&action=edit&redlink=1> "TCollectionItem \(page does not exist\)"). 

To use a TCollection, a derived TCollectionItem has to be created 

## Contents

  * 1 Inheritance
  * 2 Simple example
  * 3 Streaming
  * 4 Generics
  * 5 See also



## Inheritance

  * [TObject](<TObject.md> "TObject") \- Base class of all classes.; [IFPObserved](</index.php?title=IFPObserved&action=edit&redlink=1> "IFPObserved \(page does not exist\)") \- Interface implemented by an object that can be observed. 
    * [TPersistent](</index.php?title=TPersistent&action=edit&redlink=1> "TPersistent \(page does not exist\)") \- Base class for streaming system and persistent properties. 
      * **TCollection** \- Base class to manage collections of named objects.



## Simple example
    
    
    unit uhair;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Dialogs;
    
    type
    
      { THairItem }
    
      THairItem = class(TCollectionItem)
      private
        FLength: integer;
      public
        constructor Create(ACollection: TCollection); override;
      published
        property Length: integer read FLength write FLength;
      end;
    
      { THairList }
    
      THairList = class(TCollection)
      private
        function GetItems(Index: integer): THairItem;
        procedure SetItems(Index: integer; AValue: THairItem);
      public
        constructor Create;
      public
        function Add: THairItem;
        function AddEx(length: integer): THairItem;
        property Items[Index: integer]: THairItem read GetItems write SetItems; default;
      end;
    
    var
      hairs: THairList;
    
    implementation
    
    { THairItem }
    
    constructor THairItem.Create(ACollection: TCollection);
    begin
      if Assigned(ACollection) {and (ACollection is THairList)} then
        inherited Create(ACollection);
    end;
    
    { THairList }
    
    function THairList.GetItems(Index: integer): THairItem;
    begin
      Result := THairItem(inherited Items[Index]);
    end;
    
    procedure THairList.SetItems(Index: integer; AValue: THairItem);
    begin
      Items[Index].Assign(AValue);
    end;
    
    constructor THairList.Create;
    begin
      inherited Create(THairItem);
    end;
    
    function THairList.Add: THairItem;
    begin
      Result := inherited Add as THairItem;
    end;
    
    function THairList.AddEx(length: integer): THairItem;
    begin
      Result := inherited Add as THairItem;
      Result.Length := length;
    end;
    

Example of use: 
    
    
    initialization
      hairs := THairList.Create;
      hairs.AddEx(10);
      hairs.Add.Length := 100;
      hairs.Delete(0);
    
      ShowMessage(IntToStr(hairs.Count));
    
    finalization
      hairs.Free;
    
    end.
    

## Streaming

This adds a TComponent class that can stream the list loading and saving to text files with LResources. See [Streaming components](<Streaming_components.md> "Streaming components"). 
    
    
    unit uhair;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, Graphics, SysUtils, LResources, Dialogs;
    
    type
    
      { THairItem }
    
      THairItem = class(TCollectionItem)
      private
        FLength: integer;
        FColor: TColor;
      public
        constructor Create(ACollection: TCollection); override;
      published
        property Length: integer read FLength write FLength;
        property Color: TColor read FColor write FColor;
      end;
    
      { THairList }
    
      THairList = class(TCollection)
      private
        function GetItems(Index: integer): THairItem;
        procedure SetItems(Index: integer; AValue: THairItem);
      public
        constructor Create;
      public
        function Add: THairItem;
        function AddEx(length: integer): THairItem;
        property Items[Index: integer]: THairItem read GetItems write SetItems; default;
      end;
    
      { THairComponent }
    
      THairComponent = class(TComponent)
      private
        FHairList: THairList;
      public
        constructor Create(AOwner: TComponent); override;
        destructor Destroy; override;
      public
        procedure SaveToFile(AFileName: string);
        procedure LoadFromFile(AFileName: string);
        procedure OnFindClass(Reader: TReader; const AClassName: string;
          var ComponentClass: TComponentClass);
      published
        property HairList: THairList read FHairList write FHairList;
      end;
    
    var
      hairs: THairComponent;
    
    implementation
    
    { THairComponent }
    
    constructor THairComponent.Create(AOwner: TComponent);
    begin
      inherited Create(AOwner);
      Self.SetSubComponent(True);
      // add this line if you want to put this class inside other and also be streamed
      HairList := THairList.Create;
    end;
    
    destructor THairComponent.Destroy;
    begin
      HairList.Free;
      inherited Destroy;
    end;
    
    procedure THairComponent.SaveToFile(AFileName: string);
    var
      AStream: TMemoryStream;
    begin
      AStream := TMemoryStream.Create;
      try
        WriteComponentAsTextToStream(AStream, Self);
        AStream.SaveToFile(AFileName);
      finally
        AStream.Free;
      end;
    end;
    
    procedure THairComponent.LoadFromFile(AFileName: string);
    var
      AStream: TMemoryStream;
    begin
      AStream := TMemoryStream.Create;
      try
        AStream.LoadFromFile(AFileName);
        ReadComponentFromTextStream(AStream, TComponent(Self), @OnFindClass);
      finally
        AStream.Free;
      end;
    end;
    
    procedure THairComponent.OnFindClass(Reader: TReader; const AClassName: string;
      var ComponentClass: TComponentClass);
    begin
      if CompareText(AClassName, 'THairComponent') = 0 then
        ComponentClass := THairComponent;
    end;
    
    { THairItem }
    
    constructor THairItem.Create(ACollection: TCollection);
    begin
      if Assigned(ACollection) and (ACollection is THairList) then
        inherited Create(ACollection);
    end;
    
    { THairList }
    
    function THairList.GetItems(Index: integer): THairItem;
    begin
      Result := THairItem(inherited Items[Index]);
    end;
    
    procedure THairList.SetItems(Index: integer; AValue: THairItem);
    begin
      Items[Index].Assign(AValue);
    end;
    
    constructor THairList.Create;
    begin
      inherited Create(THairItem);
    end;
    
    function THairList.Add: THairItem;
    begin
      Result := inherited Add as THairItem;
    end;
    
    function THairList.AddEx(length: integer): THairItem;
    begin
      Result := inherited Add as THairItem;
      Result.Length := length;
    end;
    

Example of use: 
    
    
    initialization
      hairs := THairComponent.Create(nil);
    
      hairs.HairList.AddEx(10);
      hairs.HairList.AddEx(20);
      hairs.HairList.AddEx(30);
      hairs.HairList[0].Color := clRed;
    
      hairs.SaveToFile('test.txt');
    
      hairs.HairList.Delete(2);
      hairs.HairList.Delete(1);
      hairs.HairList.Delete(0);
    
      hairs.LoadFromFile('test.txt');
    
      ShowMessage(IntToStr(hairs.HairList[0].Length));
    
    
    finalization
      hairs.Free;
    
    end.
    

Output text file: 
    
    
    object THairComponent
      HairList = <  
        item
          Length = 10
          Color = clRed
        end  
        item
          Length = 20
          Color = clBlack
        end  
        item
          Length = 30
          Color = clBlack
        end>
    end
    

## Generics

This is a ready-to-use collection [generic class](<Generics.md> "Generics"). It implements the most elemental structure. Save time overriding the same methods each time you want to create a collection quickly. It does it for you. 
    
    
    unit ugenericcollection;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils;
    
    type
    
      { TGenericCollection }
    
      generic TGenericCollection<T> = class(TCollection)
      private
        function GetItems(Index: integer): T;
        procedure SetItems(Index: integer; AValue: T);
      public
        constructor Create;
      public
        function Add: T;
      public
        property Items[Index: integer]: T read GetItems write SetItems; default;
      end;
    
    implementation
    
    { TGenericCollection }
    
    function TGenericCollection.GetItems(Index: integer): T;
    begin
      Result := T(inherited Items[Index]);
    end;
    
    procedure TGenericCollection.SetItems(Index: integer; AValue: T);
    begin
      Items[Index].Assign(AValue);
    end;
    
    constructor TGenericCollection.Create;
    begin
      inherited Create(T);
    end;
    
    function TGenericCollection.Add: T;
    begin
      Result := T(inherited Add);
    end;
    
    end.
    

Example of use: 
    
    
    unit usaving;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, LResources, ugenericcollection;
    
    type
    
      { TBookItem }
    
      TBookItem = class(TCollectionItem)
      private
        FCode: string;
        FTitle: string;
      published
        property Code: string read FCode write FCode;
        property Title: string read FTitle write FTitle;
      end;
    
      { TPeopleItem }
    
      TPeopleItem = class(TCollectionItem)
      private
        FName: string;
        FChoice: string;
      published
        property Name: string read FName write FName;
        property Choice: string read FChoice write FChoice;
      end;
    
      { TBookCollection }
    
      TCustomBookCollection = specialize TGenericCollection<TBookItem>;
    
      TBookCollection = class(TCustomBookCollection)
      public
        function AddEx(aCode, aTitle: string): TBookItem;
      end;
    
      { TPeopleCollection }
    
      TCustomPeopleCollection = specialize TGenericCollection<TPeopleItem>;
    
      TPeopleCollection = class(TCustomPeopleCollection)
      public
        function AddEx(aName, aChoice: string): TPeopleItem;
      end;
    
    implementation
    
    { TBookCollection }
    
    function TBookCollection.AddEx(aCode, aTitle: string): TBookItem;
    begin
      Result := inherited Add as TBookItem;
      Result.Code := aCode;
      Result.Title := aTitle;
    end;
    
    { TPeopleCollection }
    
    function TPeopleCollection.AddEx(aName, aChoice: string): TPeopleItem;
    begin
      Result := inherited Add as TPeopleItem;
      Result.Name := aName;
      Result.Choice := aChoice;
    end;
    
    end.
    

## See also

  * [TCollection doc](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/tcollection.html> "doc:rtl/classes/tcollection.html")

---

_Source: [https://wiki.freepascal.org/TCollection](https://web.archive.org/web/20250415151323/https://wiki.freepascal.org/TCollection)_
