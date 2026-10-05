# Amiga TagList

## Contents

  * 1 Problem
  * 2 Proposal
    * 2.1 Handling of types
    * 2.2 Additional feature ideas



## Problem

The Amiga API contains a heavy number of varargs functions. These are essentially just C wrappers to underlying library functions which accept a pointer to a so-called taglist. An Amiga taglist is essentially and array of tagitems, which are basically key-value pairs. To keep it simple and fast, an Amiga tag is defined as follows: 
    
    
      type
        TTagItem = record
          ti_Tag: PtrUInt;
          ti_Data: PtrUInt;
        end;
    

Basically it's just two pointer-sized field, where the first one determines the content of the second one. Currently, Free Pascal's AROS, Amiga and MorphOS port mimics these APIs with various array of const (TVarRec) and array of PtrUInt based solutions, neither of which is ideal, the first one requires Object Pascal mode, and involves reference counting and tons of managed type juggling, the second one without compiler help requires casting everything to PtrUInt manually, which is very tedious and ugly and also not very effective. Additionally, the array of const method causes a lot of range check warnings, since the Amiga ABI defines user defined tags to be unsigned $80000000 and above, which then the compiler warns about being outside the 32bit signed range, so you end up casting everything manually again... 

## Proposal

Much like the compiler knows how to assemble C-style varargs, it ideally should know how to assemble Amiga-style taglists too. This job is usually heavily macroed with a C compiler, which makes reproducing it in Pascal without compiler help is very manual labor intensive and error prone. 
    
    
      function sometaglistfunction(a: longint; b: longint; const tags: array of const): longint; taglist;
    

On the caller side, the compiler would assemble a const array of TTagItem on the stack, which is then passed by reference to the callee. The array is zero terminated, with an additional entry with both ti_Tag and ti_Data fields being zero. The compiler should error if there's an odd number of values are specified. On the callee side, the compiler would handle this argument as a const array of TTagItem. The reference could be passed further to system functions which accept a PTagItem argument, or to make further processing on a copy of the array if necessary. 

### Handling of types

  * any ordinal type which doesn't fit by value to sizeof(PtrUInt) should cause an error. Smaller ordinal and boolean values should be sign or zero extended to sizeof(PtrUInt). Amiga taglists on 32bit CPUs never pass larger than 32bit values by value, so this is not a problem.
  * sign and zero extension must follow the rules of the passed type.
  * strings should be passed as PChars when possible.
  * ideally it should error or warn about managed and record/class types to avoid potential problems.
  * on the callee side, TTagItem should be extended by an union, a bit similar to TVarRec, to allow reading values of different types w/o casts.


    
    
    type
      {$IFDEF CPU32}
      PtrBoolean = Boolean32;
      PtrCBool = LongBool;
      {$ELSE}
      {$IFNDEF CPU64}
      {$ERROR We don't support other than 32 and 64 bit for now. }
      {$ENDIF}
      PtrBoolean = Boolean64;
      PtrCBool = QWordBool;
      {$ENDIF}
    
    type
      PPasTagItem = ^TPasTagItem;
      TPasTagItem = record
        ti_Tag: Tag;
        case byte of
        0:  (ti_Data: PtrInt);
        1:  (ti_UData: PtrUInt);
        2:  (ti_PChar: PChar);
        3:  (ti_Pointer: Pointer);
        4:  (ti_Boolean: PtrBoolean);
        5:  (ti_CBool: PtrCBool);
        { do not add types here with sizeof(type) <> sizeof(pointer)
          because that will cause alignment and padding issues, as
          the padding bytes won't be cleaned properly on assignment.
          Also we're not only 32 bit. Remember AROS64. }
      end;
    

### Additional feature ideas

The compiler could support pre-defined const taglist, outside the scope of a function call somehow. Additionally, as a related feature, simply handling const array of ptruint more flexibly, w/o strict type checking could ease some scenarios in Amiga API programming.

---

_Source: [https://wiki.freepascal.org/Amiga_TagList](https://web.archive.org/web/20250122170928/https://wiki.freepascal.org/Amiga_TagList)_
