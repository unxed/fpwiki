# Var

The [keyword](<Keyword.md> "Keyword") `var` is used to: 

  * start section of [variable](<Variable.md> "Variable") declarations, and
  * declare a formal parameter as penetrating mutable.



## Variable declaration section

In a [block](<Block.md> "Block") the word `var` starts a section of one or more variable [declarations](<Declaration.md> "Declaration"). 
    
    
    var
    	age: integer;
    

Variables bearing the same [data type](<Type.md> "Type") can be grouped, by separating their respective [identifiers](<Identifier.md> "Identifier") by a [comma](<Comma.md> "Comma"): 
    
    
    var
    	firstName, lastName, address: string;
    

## Variable parameter

The word `var` prior a formal parameter declaration indicates that this [parameter is variable](<Variable_parameter.md> "Variable parameter"), that means assigning values to it will affect the named parameter at the call site. 
    
    
    procedure censor(var xxx: longWord);
    begin
    	if (xxx = $4655434B) or (xxx = $6675636B) then
    	begin
    		xxx := $2A2A2A2A;
    	end;
    end;
    

After invoking `procedure censor` the value of the variable that was supplied at the call site (i. e. where the procedure was called) will (possibly) have changed, too. 

## See also

  * [`const`](<Const.md> "Const")

---

_Source: [https://wiki.freepascal.org/Var](https://web.archive.org/web/20250314125458/https://wiki.freepascal.org/Var)_
