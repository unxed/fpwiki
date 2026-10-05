# Programming Using Classes

## Using an Object Pascal Class

To use a class one can either call class methods or create an instance of the class and then use it's fields, properties and methods. 

The instance of an Object Pascal class is commonly called "object", but don't confuse it with Object Pascal Objects. To create an instance of a class, one should call one of it's constructors. The constructor will return the instance of the class. Assign the returned instance to a variable in order to be able to use it and remember to free the instance later using the Free method. Descendents of TComponent can have an owner specified in their constructor. The instance will then be freed when the owner is freed and calling Free manually isn't necessary. 

The following examples demonstrates the syntax that can be used to create a class and access it's methods: 
    
    
    var
      MyStringList: TStringList;
    begin
      MyStringList := TStringList.Create;
      try
        MyStringList.LoadFromFile('path_to_my_file.txt');
        // ...
      finally
        MyStringList.Free;
      end;
    end;
    

## See Also

  * [Object_Oriented_Programming_with_FreePascal_and_Lazarus](<Object_Oriented_Programming_with_FreePascal_and_Lazarus.md> "Object Oriented Programming with FreePascal and Lazarus")

---

_Source: [https://wiki.freepascal.org/Programming_Using_Classes](https://web.archive.org/web/20250219190841/https://wiki.freepascal.org/Programming_Using_Classes)_
