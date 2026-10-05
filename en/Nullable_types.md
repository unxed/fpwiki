# Nullable types

│ **English (en)** │

Nullable types are types which can have no value (can be unassigned). One such type in Pascal is [Pointer](<Pointer.md> "Pointer") type which can have [`nil`](<Nil.md> "Nil") value which means that it isn't assigned to any specific address. Same behavior can be implemented using [generic types](<Generics.md> "Generics") and advanced records with [operator overloading](<Operator_overloading.md> "Operator overloading"). 

The Nullable unit is part of FPC since 3.2.2 and its code can be seen in the GitLab repository: [nullable.pp](<https://gitlab.com/freepascal.org/fpc/source/-/blob/main/packages/rtl-objpas/src/inc/nullable.pp>). 

Then you can define nullable types like: 
    
    
    NullableChar = TNullable<Char>;
    NullableInteger = TNullable<Integer>;
    NullableString = TNullable<string>;
    

## See also

  * [How to use nullable types](<How_to_use_nullable_types.md> "How to use nullable types")



The standard nullable type in Object Pascal is Variant. 

  * [Variant](<Variant.md> "Variant")



## External links

  * [FPC GitLab Repository nullable.pp](<https://gitlab.com/freepascal.org/fpc/source/-/blob/main/packages/rtl-objpas/src/inc/nullable.pp>)


  * [Nullables in Free Pascal and Delphi](<https://www.arbinada.com/en/node/1439>)

---

_Source: [https://wiki.freepascal.org/Nullable_types](https://web.archive.org/web/20240304014819/https://wiki.freepascal.org/Nullable_types)_
