# TList

## TList or TFPList

TList is a class that can be used to manage collections of pointers. It introduces methods and properties to store the pointers, search in the list of pointers, sort them. It manages its memory by itself, no intervention for that is needed (but, of course, it does not manage the memory of what ever your pointers point to). It has an event notification mechanism which allows to notify of list changes. This slows down some of TList mechanisms, and if no notification is used, TFPList may be used instead. 

Both TList and TFPList are declared in classes and that usually is already in your uses clause. 

The 'managed pointers' may conveniently point to, for example, records, just about any record you care to define. 

Here is a very basic unit that will manage a set of notes about some friends of mine - 
    
    
    type
      	PNote=^TNote;
      	TNote = record
    		Name : ANSIString;
            IsSilly : boolean;
            NumbHeads : integer;
    	end;
    
    type
        // Note we can use TFPList (faster) or TList (and get event notification)
       TNoteList = class(TList)
       private
        	function Get(Index: integer): PNote;
        public
            destructor Destroy; override;
            function Add(ANote : PNote) : integer;
            function FindName(const Name : ANSIString) : PNote;
            property Items[Index: integer]: PNote read Get; default;
        end;
    
    
    implementation
    
    function TNoteList.Get(Index: integer): PNote;
    begin
        Result := PNote(inherited get(Index));
    end;
    
    function TNoteList.Add(ANote: PNote): integer;
    begin
        result := inherited Add(ANote);
    end;
    
    function TNoteList.FindName(const Name: ANSIString): PNote;
    var
        P : PNote;
    begin
        Result := Nil;
        for P in self do
            if P^.Name = Name then exit(P);
    end;
    
    destructor TNoteList.Destroy;
    var
      I : integer;
    begin
    	for I := 0 to Count-1 do begin
        	dispose(Items[I]);
    	end;
    	inherited Destroy;
    end;                                         
    ....
    

In this example, that can use either TList or TFPList, we make a record that will hold one "item of data", and, to make things easy, we also declare a type pointer to that sort of record. Its sensible to put code like this in its own unit, a practical use will almost certainly require a few extra methods to manage the data it self. 

To use this example, we need to declare a variable of type TNoteList and then create and destroy it. So, perhaps in a FormCreate we would have - 
    
    
    NoteList := TNoteList.Create;
    

and in FormDestroy we might have - 
    
    
    NoteList.free;
    

To load some data we would perhaps use - 
    
    
    procedure AddNote(N: string; Silly: boolean; NHeads: integer);
    var
        aPRec : PNote;   // Not a record, just a pointer to one.
    begin
        new(aPRec);
        aPRec^.Name := N;
        aPRec^.IsSilly := Silly;
        aPRec^.NumbHeads := NHeads;
        NoteList.Add(aPRec);
    end;                
            
    procedure TForm1.LoadData();
    begin
        AddNote('Joe', False, 1);
        AddNote('Harry', True, 1);
        AddNote('David', True, 2);     // Tasmanian
        AddNote('Boris', False, 1);
    end;
    

and to use that data we might have - 
    
    
    procedure TForm1.UseData();
    var
        P : PNote;     // P is a pointer to a note record, somewhere !
    begin
        P := NoteList.FindName('David');
        if P <> nil then
            writeln('David has ' + P^.NumbHeads.ToString + ' heads');
        for P in NoteList do
            if P^.IsSilly then
                writeln(P^.Name + ' is silly');
        if NoteList.FindName('George') = nil then
            writeln('George is not listed');
    end;
    

## See also

  * [Data Structures, Containers, Collections](<Data_Structures,_Containers,_Collections.md> "Data Structures, Containers, Collections")
  * An [AVL Tree](<AVL_Tree.md> "AVL Tree") will be faster.

---

_Source: [https://wiki.freepascal.org/TList](https://web.archive.org/web/20240318085517/https://wiki.freepascal.org/TList)_
