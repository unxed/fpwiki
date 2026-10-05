# for-in loop

│ **English (en)** │  **[русский (ru)](<../ru/for-in_loop.md>)** │

This iterates over a collection, but where a [basic for-loop](<For.md> "For") uses a numerical index counter, a for-in loop instead retrieves collection elements into the counter variable for immediate use. For-in works on strings, arrays, sets, and any other custom collection that implements the required iterators. Looping over an empty collection does nothing. The counter variable can not be modified inside the loop. 

The **for-in loop** construct is supported in Delphi from Delphi 2005 onwards. It was implemented in FPC 2.4.2. 

The official documentation is here: [Reference guide chapter 13](<https://www.freepascal.org/docs-html/ref/refsu58.html>)

  


## Contents

  * 1 Delphi and FPC implementation
    * 1.1 String loop
    * 1.2 Array loop
    * 1.3 Set loop
    * 1.4 Loop variables are temporary copies of the container value
    * 1.5 Traversing container
    * 1.6 Multiple enumerators for one class
  * 2 FPC extensions
    * 2.1 Traversing enumeration and subrange types
    * 2.2 Declaring enumerators
      * 2.2.1 Traversing UTF-8 strings
    * 2.3 Using any identifiers instead of builtin MoveNext and Current
  * 3 Proposed extensions
    * 3.1 Get enumerator Position if available
  * 4 Reference



## Delphi and FPC implementation

A **for in** loop has the following syntax: 

### String loop
    
    
    procedure StringLoop(S: String);
    var
      C: Char;
    begin
      for C in S do
        DoSomething(C);
    end;
    

### Array loop
    
    
    procedure ArrayLoop(A: Array of Byte);
    var
      B: Byte;
    begin
      for B in A do
        DoSomething(B);
    end;
    

### Set loop
    
    
    type
      TColor = (cRed, cGren, cBlue);
      TColors = set of TColor;
    procedure SetLoop(Colors: TColors);
    var
      Color: TColor;
    begin
      for Color in Colors do
        DoSomething(Color);
    end;
    

### Loop variables are temporary copies of the container value

The loop variable in a for-in loop is a [copy of the value that is stored inside a container over which the for-in loop is performed](<https://stackoverflow.com/questions/18575736/delphi-dynamic-array-iteration-and-record-copyingsimple>). 
    
    
    program tempcopy;
    
    {$IFDEF FPC}
      {$mode Delphi}
    {$ENDIF}
    
    uses SysUtils;
    
    type
      PointerAddress = NativeUInt;
    
    var
      pIntArr: Pointer;
      IntArr: Array of Integer;
      
    procedure ArrayLoop(const AArr: Array of Integer);
    var
      i: Integer;
    begin
      pIntArr := @AArr;
      writeln('Argument: ', PointerAddress(pIntArr)); // <-- same memory address as Item 1
      pIntArr := @i;
      writeln('Local loop variable: ', PointerAddress(pIntArr)); // <-- local temporary variable i
      for i in AArr do
      begin
        pIntArr := @i;
        writeln('Loop variable: ', PointerAddress(pIntArr)); // <-- local temporary variable i
      end;
    end;
      
    begin
      // allow 3 items in the array
      SetLength(IntArr, 3);
    
      pIntArr := @pIntArr;
      writeln('global pIntArr address: ', PointerAddress(pIntArr));
    
      // IntArr is separate variable that points to first element
      pIntArr := @IntArr;
      writeln('global IntArr address: ', PointerAddress(pIntArr));
      pIntArr := @IntArr;
      writeln('IntArr points to: ', PointerAddress(pIntArr^));
    
      // addresses of the single items
      pIntArr := @IntArr[0];
      writeln('Item 1: ', PointerAddress(pIntArr)); // <-- memory address of the first item
      pIntArr := @IntArr[1];
      writeln('Item 2: ', PointerAddress(pIntArr));
      pIntArr := @IntArr[2];
      writeln('Item 3: ', PointerAddress(pIntArr));
    
      writeln('array loop');
      ArrayLoop(IntArr);
    end.
    

### Traversing container

To traverse a container class you need to add an **enumerator** for it. An **Enumerator** is a class structured according to the following template: 
    
    
    TSomeEnumerator = class
    public
      function MoveNext: Boolean;
      property Current: TSomeType;
    end;
    

There are only 2 things required for the enumerator class: a **MoveNext** method which asks the enumerator to step forward and a **Current** property which can return any appropriate type. 

Thereafter you need to add the magic **GetEnumerator** method to the container class which returns an **enumerator** instance. 

For example: 
    
    
    type
      TEnumerableTree = class;
    
      TTreeEnumerator = class
      private
        FTree: TEnumerableTree;
        FCurrent: TNode;
      public
        constructor Create(ATree: TEnumerableTree); 
        function MoveNext: Boolean;
        property Current: TNode read FCurrent;
      end;
    
      TEnumerableTree = class
      public
        function GetEnumerator: TTreeEnumerator;
      end;
    
    constructor TTreeEnumerator.Create(ATree: TEnumerableTree);
    begin
      inherited Create;
      FTree := ATree;
      FCurrent := nil;
    end;
    
    function TTreeEnumerator.MoveNext: Boolean;
    begin
      // some logic to get the next node from a tree
      if FCurrent = nil then
        FCurrent := FTree.GetFirstNode
      else
        FCurrent := FTree.GetNextNode(FCurrent);
      Result := FCurrent <> nil;
    end;
    
    function TEnumerableTree.GetEnumerator: TTreeEnumerator;
    begin
      Result := TTreeEnumerator.Create(Self);
      // Note: the Result is automatically freed by the compiler after the loop.
    end;
    

After this you are able to execute the following code: 
    
    
    procedure TreeLoop(ATree: TEnumerableTree);
    var
      ANode: TNode;
    begin
      for ANode in ATree do
        DoSomething(ANode);
    end;
    

You will find that several basic classes (such as TList, TStrings, TCollection, TComponent ...) already have built-in enumerator support. 

It is also possible to make any class enumerable if you implement the following interface in your enumerable container class: 
    
    
      IEnumerable = interface(IInterface)
        function GetEnumerator: IEnumerator;
      end;
    

Where **IEnumerator** is declared as: 
    
    
      IEnumerator = interface(IInterface)
        function GetCurrent: TObject;
        function MoveNext: Boolean;
        procedure Reset;
        property Current: TObject read GetCurrent;
      end;
    

### Multiple enumerators for one class

You can add additional enumerators to classes, objects and records. 

Here is an example of adding an enumerator which traverses a TEnumerableTree in reverse order: 
    
    
    type
      TEnumerableTree = class;
    
      TTreeEnumerator = class
      ...for traversing in order, see above...
      end;
    
      TTreeReverseEnumerator = class
      private
        FTree: TEnumerableTree;
        FCurrent: TNode;
      public
        constructor Create(ATree: TEnumerableTree); 
        function MoveNext: Boolean;
        property Current: TNode read FCurrent;
        function GetEnumerator: TTreeReverseEnumerator; // returns itself
      end;
    
      TEnumerableTree = class
      public
        function GetEnumerator: TTreeEnumerator;
        function GetReverseEnumerator: TTreeReverseEnumerator;
      end;
    
    ...see above for an implementation of the TTreeEnumerator...
    
    constructor TTreeReverseEnumerator.Create(ATree: TEnumerableTree);
    begin
      inherited Create;
      FTree := ATree;
    end;
    
    function TTreeReverseEnumerator.MoveNext: Boolean;
    begin
      // some logic to get the next node from a tree in reverse order
      if FCurrent = nil then
        FCurrent := FTree.GetLastNode
      else
        FCurrent := FTree.GetPrevNode(FCurrent);
      Result := FCurrent <> nil;
    end;
    
    function TTreeReverseEnumerator.GetEnumerator: TTreeReverseEnumerator;
    begin
      Result := Self;
    end;
    
    function TEnumerableTree.GetReverseEnumerator: TTreeReverseEnumerator;
    begin
      Result := TTreeReverseEnumerator.Create(Self);
      // Note: the Result is freed automatically by the compiler after the loop.
    end;
    

After this you are able to execute the following code: 
    
    
    procedure TreeLoop(ATree: TEnumerableTree);
    var
      ANode: TNode;
    begin
      for ANode in ATree.GetReverseEnumerator do
        DoSomething(ANode);
    end;
    

## FPC extensions

The following code examples illustrate constructs that are implemented only by FPC, constructs which are not supported by Delphi. 

### Traversing enumeration and subrange types

In Delphi, it is [not possible to traverse either enumerated types](<http://qc.embarcadero.com/wc/qcmain.aspx?d=106212>) or subrange types, whereas in Free Pascal we can write the following: 
    
    
    type
      TColor = (clRed, clBlue, clBlack);
      TRange = 'a'..'z';
    var
      Color: TColor;
      ch: Char;
    begin
      for Color in TColor do
        DoSomething(Color);
      for ch in TRange do
        DoSomethingOther(ch);
    end.
    

An example of this can further be demonstrated by the following code taken from bugreport 0029147, where the type system is disabled because a hardcast is used on a value that is not necessarily part of the enum value: 
    
    
    type
      TSomeEnums = (One, Two, Three);
    
    resourcestring
      SOne = 'One ape';
      STwo = 'Two apes';
      SThree = 'Three apes';
    
    const
      SSomeEnumStrings: array [Low(TSomeEnums)..High(TSomeEnums)] of string = (
        SOne, STwo, SThree);
    
    var
      i: Integer;
      SE: TSomeEnums;
    
    begin
      for i := 0 to 4 do begin
        SE := TSomeEnums(i);  // hardcast. i can be higher than 2, but the type system is now disabled
        WriteLn(SSomeEnumStrings[SE]);
      end;
    end.
    

The programmer instructs the compiler that i is of the enum type and the compiler will not check any further, assuming that the programmer knows best.  
Of course the programmer is wrong and the compiler would have known better...  
By traversing the enumerated type with **for in do** the hard cast is superfluous and the code becomes type safe: 
    
    
    type
      TSomeEnums = (One, Two, Three);
    
    resourcestring
      SOne   = 'One ape';
      STwo   = 'Two apes';
      SThree = 'Three apes';
    
    const
      SSomeEnumStrings: array [Low(TSomeEnums)..High(TSomeEnums)] of string = (
        SOne, STwo, SThree);
    
    var
      SE: TSomeEnums;
    begin
        for SE in TSomeEnums do
          WriteLn(SSomeEnumStrings[SE]);
    end.
    

### Declaring enumerators

It is also not possible in Delphi to add an enumerator without modifying the class, nor to add an enumerator to the non-class/object/record/interface type. FPC makes this possible using the new syntax **operator** type **Enumerator**. As in the following example: 
    
    
    type
      TMyRecord = record F1: Integer; F2: array of TMyType; end;
      TMyArrayEnumerator = class
      private
        function GetCurrent: TMyType;
      public
        constructor Create(const A: TMyRecord);
        property Current: TMyType read GetCurrent;
        function MoveNext: Boolean;
      end;
    
      // This is new built-in operator.
      operator Enumerator(const A: TMyRecord): TMyArrayEnumerator;
      begin
        Result := TMyArrayEnumerator.Create(A);
      end;
    
    var
      A: MyRecord;
      V: TMyType
    begin
      for V in A do
        DoSomething(V);
    end.
    

#### Traversing UTF-8 strings

As a particularly useful example, the above extension allows very efficient traversal of UTF-8 strings: 
    
    
    uses
      LazUTF8;
    interface
    type
      { TUTF8StringAsStringEnumerator
        Traversing UTF8 codepoints as strings is useful when you want to know the
        exact encoding of the UTF8 character or if you like to use string
        constants in your code.
        For security reasons you should use the codepoints values (cardinals) instead.
        If speed matters, don't use enumerators. Instead use the PChar directly as 
        shown in the MoveNext method and read about UTF8. It has some interesting features. }
    
      TUTF8StringAsStringEnumerator = class
      private
        fCurrent: UTF8String;
        fCurrentPos, fEndPos: PChar;
        function GetCurrent: UTF8String;
      public
        constructor Create(const A: UTF8String);
        property Current: UTF8String read GetCurrent;
        function MoveNext: Boolean;
      end;
    
      operator Enumerator(A: UTF8String): TUTF8StringAsStringEnumerator;
    
    var
      Form1: TForm1;
    
    implementation
    
    operator Enumerator(A: UTF8String): TUTF8StringAsStringEnumerator;
    begin
      Result := TUTF8StringAsStringEnumerator.Create(A);
    end;
    
    { TUTF8StringAsStringEnumerator }
    
    function TUTF8StringAsStringEnumerator.GetCurrent: UTF8String;
    begin
      Result:=fCurrent;
    end;
    
    constructor TUTF8StringAsStringEnumerator.Create(const A: UTF8String);
    begin
      fCurrentPos:=PChar(A); // Note: if A='' then PChar(A) returns a pointer to a #0 string
      fEndPos:=fCurrentPos+length(A);
    end;
    
    function TUTF8StringAsStringEnumerator.MoveNext: Boolean;
    var
      l: Integer;
    begin
      if fCurrentPos<fEndPos then
      begin
        l:=UTF8CharacterLength(fCurrentPos);
        SetLength(fCurrent,l);
        Move(fCurrentPos^,fCurrent[1],l);
        inc(fCurrentPos,l);
        Result:=true;
      end else
        Result:=false;
    end;
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      s, ch: UTF8String;
      i: SizeInt;
    begin
      s:='mäßig';
    
      // using UTF8Length and UTF8Copy this way is slow, requiring O(n)^2
      for i:=1 to UTF8Length(s) do
        writeln('ch=',UTF8Copy(s,i,1));
    
      // using the above enumerator is shorter and quite fast, requiring O(n)
      for ch in s do
        writeln('ch=',ch);
    end;
    

### Using any identifiers instead of builtin MoveNext and Current

In Delphi you must use a function with the name 'MoveNext' and a property with the name 'Current' in enumerators. With FPC you can choose whatever names you wish. This is enabled by the use of the **enumerator** modifier, with the syntax 'enumerator MoveNext;' and 'enumerator Current;' modifiers. As in the following example: 
    
    
    type
      { TMyListEnumerator }
    
      TMyListEnumerator = object
      private
        FCurrent: Integer;
      public
        constructor Create;
        destructor Destroy;
        function StepNext: Boolean; enumerator MoveNext;
        property Value: Integer read FCurrent; enumerator Current;
      end;
    
      TMyList = class
      end;
    
    { TMyListEnumerator }
    
    constructor TMyListEnumerator.Create;
    begin
      FCurrent := 0;
    end;
    
    destructor TMyListEnumerator.Destroy;
    begin
      inherited;
    end;
    
    function TMyListEnumerator.StepNext: Boolean;
    begin
      inc(FCurrent);
      Result := FCurrent <= 3;
    end;
    
    operator enumerator (AList: TMyList): TMyListEnumerator;
    begin
      Result.Create;
    end;
    
    var
      List: TMyList;
      i: integer;
    begin
      List := TMyList.Create;
      for i in List do
        WriteLn(i);
      List.Free;
    end.
    

## Proposed extensions

### Get enumerator Position if available

It is impossible to extract any information from the iterator except the current item. Sometimes other data, such as the current index, might be useful: 
    
    
    type
      TUTF8StringEnumerator = class
      private
        FByteIndex: Integer;
        FCharIndex: Integer;
      public
        constructor Create(const A: UTF8String);
        function Current: UTF8Char;
        function CurrentIndex: Integer;
        function MoveNext: Boolean;
      end;
    
      operator GetEnumerator(A: UTF8String): TUTF8StringEnumerator;
      begin
        Result := TUTF8String.Create(A);
      end;
    
    var
      s: UTF8String;
      ch: UTF8Char;
      i: Integer;
    begin
    
      // Inefficient, as discussed above
      for i := 1 to Length(s) do
        Writeln(i, ': ', ch[i]);
    
      // Ok, but ugly
      i := 1;
      for ch in s do begin
        Writeln(i, ': ', ch);
        Inc(i);
      end;
    
      // Proposed extension
      for ch in s index i do
        Writeln(i, ': ', ch);
    
      // Proposed extension for traversing backwards (equivalent to downto)
      for ch in reverse s do
        Writeln(i, ': ', ch);
    
      // With proposed index extension
      for ch in reverse s index i do
        Writeln(i, ': ', ch);
    end.
    

Note that index could be designed to return an arbitrary type (i. e. not necessarily an integer). For example, in the case of tree traversal, the index might return an array of nodes describing the path from the tree root to the current node. 

## Reference

  * <http://17slon.com/blogs/gabr/2007/03/fun-with-enumerators.html>

---

_Source: [https://wiki.freepascal.org/for-in_loop](https://web.archive.org/web/20230127072329/https://wiki.freepascal.org/for-in_loop)_
