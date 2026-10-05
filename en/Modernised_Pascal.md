# Modernised Pascal

This page was originally a summary from an enthusiast user to "create" a more modern (!?!) Pascal. The suggestions are in practice a mix of random Basic syntax (which is already pretty random in itself), the regularly occuring namespace proposal etc. The page, and the rebutal was left in as an example, since it demonstrates a few traits common in these kinds of requests: 

  * Random borrowing from other languages on instinct, not reason
  * Failure to grasp basic concept of Pascal parsing and philosophy.
  * Syntax that saves minor typing, while IDE can do quite complex handling nowadays like declaring vars of for loops (this goes doubly for Pascal since it is parsing model makes it easy for IDEs)
  * Missing the consequences (and advantages of) the unit concept.
  * Pretty shallow description of the feature, no implementation, minimal code examples only. (see e.g. the elsif. Example what it solves(dangling else) can be found anywhere) It is a long way from idea to final implementation, and most ideas stumble on that road.
  * No implementor offered. Who is going to do the hard work is the hard nut to crack. Everybody has more ideas then he can realise, the core developers even more likely so.



The experiences with this page and similar request lead to the following FAQ entry: [There is a new extension that will be really useful. Will you include it](<http://www.freepascal.org/faq.var#extensionselect>)

## Contents

  * 1 Original points (some comments inlined)
    * 1.1 Elsif operator
    * 1.2 Between operator syntax
    * 1.3 For-loops step
    * 1.4 Extensive code Body for While-loop
    * 1.5 Alias for long name
    * 1.6 Block variables & initialization in declaration section
    * 1.7 (?) Property information 'access' abstraction ( on code compilation phase )
    * 1.8 Unified as and is,
    * 1.9 Merging of interface/implementation,
    * 1.10 string*integer operation.
    * 1.11 FOR syntax extensions
    * 1.12 Easier access to memory
    * 1.13 Namespaces
    * 1.14 For statement for collections. Read the next page for-in loop
    * 1.15 Unified try / except / finally
    * 1.16 Case-string-of
    * 1.17 case-type-of-var
    * 1.18 array element as FOR iterator
    * 1.19 Large Strings
    * 1.20 threadlocalvar unLOCKed variables
    * 1.21 Stringfy an identifier
    * 1.22 Add C++ style objects which have copy/default constructors and destructors that are called automatically.
  * 2 Rejection Justification
  * 3 See Also



## Original points (some comments inlined)

more strongly syntax 

### Elsif operator
    
    
    if ... then
      ...
    elsif {!}
      ...
    else
      ...
    end; {!}
    

### Between operator syntax

if 1 > a < 3 then ... {!} end 

### For-loops step
    
    
     for i := 1 to 30 {+} step 5 {+.} do
     end {!}
    

_Remark: "step" keyword seems to be cloned from Basic. If this to be implemented, then it is better to be replaced with "by" from Component Pascal and other later Wirthian languages._

### Extensive code Body for While-loop
    
    
     while ... do ...
     end; {!}
    

### Alias for long name
    
    
     when a = other_long_name1, b = other_long_name2 {!} do
       a.method1;
     end; {!}
    

### Block variables & initialization in declaration section
    
    
      declare // ?
        var
          a: integer := DataSet1.Field[1].AsInteger;
        const
          b: integer = 1;
      begin
        ...
      end;
    

### (?) Property information 'access' abstraction ( on code compilation phase )
    
    
      TClass1 = class
      ...
      published
        property FieldInt: Integer read fFieldInt write fFieldInt default 100;
      end;
    
    
    
      constructor TClass1.Create(...   
      begin
        inherited;
        fFieldInt := TClass1.FieldInt.DefaultValue; {!} // for non object types
      end;
    
    
    
      procedure TClass1.Check(...
      begin
        if fFieldInt = TClass1.FieldInt.DefaultValue then
           ...
        end
    

### Unified as and is,

as made in Component Pascal (aka Oberon/F, last Wirth's language): 
    
    
       //   usual code is like that
       if MyVar is MyClass then
          with MyVar as MyClass do begin
             ...
          end else begin
             ...
          end;
    

_(* obvious, that as operator is not required here from practical point of view. Since both as and is operators seems to be procedures running over class'es RTTI, it is hard for compiler to remove call to as. It also places a rooks at the doorstep of programmer, since he might, when later renaming classes or variables, rename only one from the pair. In Component Pascal it was decided that IS operator, if met inside execution flow control statements (if, case, while) and evaluated to true, assures the corresponded execution path that MyVar really belongs to MyClass, in other words, acting like implicit AS but without extra work through classes inheritance tree. So the code, beeing simplified, looks like that: *)_
    
    
       if MyVar is MyClass then begin
             ... //here MyVar is MyClass, compiler knows that
          end else begin
             ... //here MyVar is **not** MyClass, compiler knows that
          end;
     
    

_That is not to break any compatibility with existing code or coding habits._ As well, this seems to be relatively simple change.__

### Merging of interface/implementation,

like in Component Pascal: 

Borland's idea of interface/implementations perhaps was born analyzing C's .h files. They just merged them into single file with, say, .cpp file. that allowed quick modular compilation. It seems pretty and rigorous, but has its disadvantages, discovered with extensions made since Turbo Pascal 4 :) Some persons hate when changing prototype of overloaded function in interface section they need to hunt for it in implementation section too. Other ones raise a question of encapsulation leaks. They argue that there easily can be a situation, when unit publishes Class A, in the unit's interface section. Next, unit has some private Class B, which is not to be published. Next, Class B is the type on some private member variable within Class A. With current syntax rules that enforces the programmer to move Class B to interface section, thus publishing it, while he did wanted to hide, encapsulate it. In Component Pascal instead the visibility of procedure,etc is determined by presence of asterisk in it's declaration. Something like that: "Procedure* PublishedFunction(vars);" and the like for classes, vars, etc. That is a big change, i guess, it would require special switch, like Mac Mode, or automagical detection if it was interface met first, or declaration with asterisk. It also looks less rigorous than habit-blessed interface/implementation pair. However it seems that might be more rigouruos in terms of encapsulating indeed. 

  


### string*integer operation.

If Pascal having <string or char> \+ <string or char> operation, then why not have <string or char> * <positive integer> operation, meaning duplicationg strings ? :) 

### FOR syntax extensions

\- can't determine if i am jokling or am talking seriously. I always felt that FOR is not straight about its variables. One one hand it is not variable, but rather specfic local semi-constant: it cannot be assigned to, it becomes garbage after loop is executed|jumped out. One another hand it is semi-variable since it needs declaration of variable in outer block. And it destroys previous content of that variable. So, just like about var-parameter in procedures, it makes me think of two orthogonal extensions of FOR: 
    
    
    for i:integer := 10 to 20 do ...; // (1)
    
    for var i := 10 to 20 do ...; // (2)
    
    
    
    var i: integer;
    begin 
      i := 5;
      for  i: integer := i to 10 do write(i:3); //(3)
      writeln (^M^J'i is still ', i);
    end;
    

The examples demonstrate how this specialised syntax patterns would differ from current FOR, making it choose some consistent modus operandi. 

    (1) means that i is for-local. It does not need declaration of i outside of FOR, if it was declared, that outer i would be pushed out of scope within the loop. It is read-only and is truely undefined (non-existent) before and after the loop.
    (2) moves back to Turbo Pascal code generator, where for was more like C++ one. No optimisations, like running ECX register from 9 to 0 instead of incrementing from 1 to 10. Variable is to be declared in outer block (it is like passed to FOR like var-parameters to procedures). It can be changed at any time within the loop. It really works over that variable, so after loop is escaped variable is keeping last value and after loop completion it would keep to-margin+1 ot downto-margin-1 value.
    (3) shows the last point thatouter i is visible and may be used in localisation of local i. After local i is initialised, outer i is get out of scope, and heince its value is not destroyed by loop.

I feel that both (1) and (2) models are more strict and consistend that the current Delphi for model. 

### Easier access to memory

for example 
    
    
     var
       p: Pointer;
       str: String;
     begin
       str := 'abc';
       p := PChar(str);
       writeln(Char(p[1]), Char(p[2]));
     end;
    

  


### Namespaces

Why not introduce [namespaces](<Namespaces.md> "Namespaces") (like in C#) to library names. 
    
    
    unit ThisIs.AUnits.Namespace
    interface
    uses
        OtherUnit.Namespace
    

... in code you can than easily distinct between classes with same names. 
    
    
     SomeUnit.Class1.Method1(SomeOtherUnit.Class1.Method1);
    

### For statement for collections. Read the next page [for-in loop](<for-in_loop.md> "for-in loop")

Moving throug a list or collection now requires some work. For instance: 
    
    
    for i := 1 to ACollection.Count
        do begin 
           SomeObject := TTypeOfObject(ACollection.Items[i - 1]);
           // Add some code here
           end;
    

Why not: 
    
    
    foreach TTypeOfObject SomeObject in ACollection
            do begin
               // Add some code here
               end;
    

It is clearer and more readable. 

  


### Unified try / except / finally

Allow a try-except-finally Block 

For instance: 
    
    
    _//Some Code here..._
    anInstance := TSomeClass.Create;
    **try**
      _//Here some Code..._
      anInstance.DoSomewhat;
    **except**
      _//if an exception raised..._
      Showmessage(anInstance.LastErrorMessage);
    **finally**
      anInstance.Free;
    **end;**
    _//Some Code here... (not executed if an Exception raised was before)_
    

This would save a nested try-except in a try-finally. Idea stolen from new [Python 2.5](<http://docs.python.org/dev/whatsnew/pep-341.html>)

### Case-string-of

Already implemented. 
    
    
    procedure TFormExecCmd.btnExecuteClick(Sender: TObject);
    begin
      case string edtCommand.Text of
        'Hallo', 'Hello':
          ShowMessage('Hallo');
        'Exit':
          Close;
      else
        ShowMessage('Unknown command: ' + edtCommand.Text);
      end;
    end;
    

  


### case-type-of-var
    
    
    var
      a,b,c : integer;
    begin
      ...
      {$CaseCodeGen cgALL/cgFIRST}
      case a of
        b: do_something_1;
        c: do_something_2;
        7: do_something_3;
      else
        do_something_else;
      end;
    end;
    

What can we do if b = c: execute do_something_1 only ('cause it firt in textual order) or execute do_something_2 too? Maybe directive {$CaseCodeGen cgALL/cgFIRST} can help us. 

  


### array element as FOR iterator
    
    
    var
      a: array[1..4] of integer;
    begin
      for a[2] := 0 to 73 do
        writeln(a[2]); _// it's will be usefull_
    end;
    

based on <http://andy.jgknet.de/dlang/>

### Large Strings

Large Strings going beyound one line limit (255 chars lenght too), like HEREDOC from ruby, python, lua,... This can make the code easier to write and read instead of lots of short strings concatenated by '+'. 
    
    
    var
      sql: string;
    begin
      sql := <<EOS
        select id, name, order
        from atable
        where id = %d
        and order > 100
        order by 2
      EOS;
      process(sql);
    end;
    

### threadlocalvar unLOCKed variables

i.e. strings and dynamic arrays local to the current thread: reference-counted without the asm LOCK, "copy and write" only with other threadlocalvar, thread-local heap (without any asm LOCK either). It will increase the speed a lot in multi-core architectures, which don't like asm LOCK (freezes all cores for safe multi-thread). Note: this LOCK is implemented in the RTL in inclocked() and declocked() processor-specific functions. 
    
    
    function TServer.ComputePage: string;
    threadlocalvar
      tmp: string; // differs from var tmp: string
      i: integer;  // i is defined as threadlocalvar, but is the same as normal var
    begin
      for i := 0 to high(ListStr) do // ListStr[] is a dynamic array 
        tmp := tmp+ListStr[i]+#13#10; // very fast computation, without LOCK
      result := tmp; // copy from local heap to global heap
    end;
    

To implement this at the compiler and RTL level, we could use reference count < -2 for these variables (-1=const, -2=ref 0, -3=ref 1..). For example: 
    
    
    Procedure fpc_AnsiStr_Incr_Ref (S : Pointer); [Public,Alias:'FPC_ANSISTR_INCR_REF'];  compilerproc; {$IFNDEF VER2_0} Inline; {$ENDIF}
    Begin
      If S<>Nil then
        If PAnsiRec(S-FirstOff)^.Ref<0 then begin
          If PAnsiRec(S-FirstOff)^.Ref<-1 then  // -1 for const
            dec(PAnsiRec(S-FirstOff)^.Ref); // threadlocalvar fast reference count
        end else
          inclocked(PAnsiRec(S-FirstOff)^.Ref); // threadsafe reference count
    end;
    

And a local threadheap should be implemented for such threadlocalvar, with threadlocalgetmem() and such functions. 

### Stringfy an identifier

In several places we need a string key and several times it is repeated along the program and the compiler can't check it for bad spelling and that can generates hard to find bugs, could be nice if freepascal compiler could stringfy identifiers like the c compiler does. 
    
    
    const
     keyfield = 3;
    begin
      AIntegerIndexedProperty[keyfield] := 45;
      AStringIndexedProperty[#keyfield] := 65; // -> # can be used to denote an identifier stringfied
    end;
    

### Add C++ style objects which have copy/default constructors and destructors that are called automatically.

## Rejection Justification

About language extensions, be sure to read: <http://www.freepascal.org/faq.html#extensionselect>

  1. Rejected. A proposal like this show exactly why begin/end is elegant, you don't need elseif.
  2. Rejected. Only purpose is to save some typing.
  3. "step" proposal not rejected (but not accepted as well), removal of "begin" rejected
  4. Rejected ... for removal of "begin"
  5. Rejected; aliases makes code harder to read.
  6. Don't understand what you want here.
  7. const field_default=100 does the same. Rejected.
  8. (Marco) Too ambigous, declaring undeclared identifiers, see also #11
  9. (Marco) press ctrl-shift arrow up/down in Lazarus and see a better solution.
  10. (Marco) the base principle of language extension is to do make something that is not doable otherwise. "why not" is not a reason
  11. (Marco) You are obviously joking :-) Seriously, now the principle that all identifiers are defined in clear locations. It makes no sense to make exceptions to that clarity just for the case of typing. Note that most next gen IDE's allow to autodeclare the var by pressing some key combo on the FOR. For example in Lazarus Ctrl+Shift+C.
  12. (Florian) what's the point about it? It's already supported? (DF) this feature was taken from C, using this you can access to memory (by pointer addr) in the same mode as you access arrays, I didn't saw this feature in FPC if it is probably I should enable some options.
  13. (Marco) You can already with units. They already form a namespace
  14. (Marco) They are both one key to instantiate the relevant template. Reducing typing is a task of the editor/ide, not of syntax. Readability IMHO doesn't improve by creating two ways to do the same, nor is it really much clearer.
  15. (Marco) Typing only. Still this is the most sane one yet.



[Daniel-fpc](</index.php?title=User:Daniel-fpc&action=edit&redlink=1> "User:Daniel-fpc \(page does not exist\)") 11:02, 17 Dec 2005 (CET) 

Note that the "begin" removal (for all but procedure/mainprogram begin) is a very common solution for dangling else. Wirth himself saw this as an improvement on Pascal. (which is why he implemented it in Modula2). Note that it only works reliably if you disallow "begin" use, which breaks compat. This is also the reason why this shouldn't be accepted. If you want this, implement a real Modula(-2) mode. 

I'm not 100% against something like point 5, as long as the alias is properly defined as a variable in the var section too. But I'm not going to implement it. 

[Marcov](</User:Marcov> "User:Marcov") 19:48, 4 June 2006 (CEST) 

  


**Unit aliases (point 5) are useful**

For example I use a unit that has a dynamic library or a static code compiled right into the executable. I use two units. One is called DynUnit.pas and the other is called StaticUnit.pas. I like to flip between using the library and the static code without hardcoding dynunit.function into the source code. This would also allow to flip between using mysql5 or mysql4 units by making an alias called 'mysql'. 

It avoids ugly code like web_SomeFunction or mysql_SomeFunction since you can do mysql.SomeFunction instead. You don't want to hardcode mysql5.somefunction or mysql_somefunction into the source because that is ugly and inelegant. Also staticweb.webout and dynweb.webout() is again hardcoding the unit into the sources or using ugly global namespace style naming such as WebOut(), versus the more elegant web.out(). 'Web' is an alias for 'dynweb' or 'staticweb' depending on which unit was chosen (equivalent unit functionality, but different versions..dynamic or static, or mysql5 vs mysql4). 

[L505](</User:L505> "User:L505") 03:24, 1 October 2007 (CEST) 

## See Also

  * [Feature Ideas](<Feature_Ideas.md> "Feature Ideas")

---

_Source: [https://wiki.freepascal.org/Modernised_Pascal](https://web.archive.org/web/20240915144045/https://wiki.freepascal.org/Modernised_Pascal)_
