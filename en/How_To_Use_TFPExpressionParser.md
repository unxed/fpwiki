# How To Use TFPExpressionParser

│ **English (en)** │  **[suomi (fi)](</How_To_Use_TFPExpressionParser/fi> "How To Use TFPExpressionParser/fi")** │    
****

` TFPExpressionParser` allows to analyze and calculate [expressions](<expression.md> "expression") such as `sin(x)*cos(2*x)` for any value of the [variable](<Variable.md> "Variable") `x`. Besides mathematical expressions it can also handle [boolean](<Boolean.md> "Boolean") values, string formulas, [date/time](<TDateTime.md> "TDateTime") values etc. Even user-provided functions can be linked in. 

It belongs to [FPC](<FPC.md> "FPC") [Free Component Library (FCL)](<FCL.md> "FCL") and is implemented in the [unit](<Unit.md> "Unit") `fpexprpars.pp`, folder `(fpc_source_dir)/packages/fcl-base/src`. Just add `fpexprpars` to the [uses](<Uses.md> "Uses") clauses to get access to its functionality. See the file “fpexprpars.txt“ (in `(fpc_source_dir)/packages/fcl-base/examples`) for a short documenation. 

## Contents

  * 1 Creating the parser
  * 2 Built-in categories
  * 3 Expressions
    * 3.1 An expression with constants
    * 3.2 An expression with a variable
      * 3.2.1 Defining variables
      * 3.2.2 Defining the expression
      * 3.2.3 Calculating the expression
      * 3.2.4 Changing variables
    * 3.3 Operators
    * 3.4 Aggregate functions
  * 4 Adding user-defined functions
  * 5 Some special code
    * 5.1 Can I change the built-in function sin(x) to accept arguments in degrees, instead of radians?



## Creating the parser

You apply the parser by creating an instance like this: 
    
    
    uses
      fpexprpars;
    
    var
      FParser: TFPExpressionParser;
    begin
      FParser := TFpExpressionParser.Create(self);
      // ... do something (see below)
    

If this is called from a [method](<Method.md> "Method") of a [form](<TForm.md> "TForm"), “[self](<Self.md> "Self")” points to the form. Since the parser inherits from `TComponent`, there is no need to destroy it explicitly since its owner, the form, will do it. On the other hand, it is also possible to create the parser from anywhere in a [program](<Program.md> "Program") without a form or even [class](<Class.md> "Class") being involved; in this case use `[nil](<Nil.md> "Nil")` as the owner of the parser, but don't forget to `.Free` the parser after its usage: 
    
    
    uses
      fpexprpars;
    
    var
      FParser: TFPExpressionParser;
    begin
      FParser := TFPExpressionParser.Create(nil);
      try
        // ... do something (see below)
      finally
        FParser.Free;
      end;
    end;
    

## Built-in categories

The parser is designed in a very flexible way, but the default parser is quite dumb. You have to specify which kind of expressions it will accept. This is done by adding the corresponding [identifier](<Identifier.md> "Identifier") to the set of built-in categories. They are accessible by the parser's [property](</Property> "Property") `BuiltIns`: 
    
    
    type
      TBuiltInCategory = (bcStrings, bcDateTime, bcMath, bcBoolean, bcConversion, bcData, bcVaria, bcUser, bcAggregate);
      TBuiltInCategories = set of TBuiltInCategory;
    

Here is a collection of the built-in symbols which can be used by adding categories to the parser's `BuiltIns` \- it should be clear to anybody who “speaks” [Pascal](<Pascal.md> "Pascal") what these symbols mean... 

  * **bcStrings** : `Length`, `Copy`, `Delete`, `Pos`, `Lowercase`, `Uppercase`, `StringReplace`, `CompareText`
  * **bcDateTime** : `Date`, `Time`, `Now`, `DayOfWeek`, `ExtractYear`, `ExtractMonth`, `ExtractDay`, `ExtractHour`, `ExtractMin`, `ExtractSec`, `Extractmsec`, `EncodeDate`, `EncodeTime`, `ShortDayName`, `ShortMonthName`, `LongDayName`, `LongMonthName`
  * **bcMath** : `cos`, `sin`, `arctan`, `abs`, `sqr`, `sqrt`, `exp`, `ln`, `log`, `frac`, `int`, `round`, `trunc`,
  * **bcBoolean** : `shl`, `shr`, `IFS`, `IFF`, `IFD`, `IFI` (The `IFxxx` symbols have the same effect as fpc's `IfThen` for string (`IFS`), floating point (`IFF`), date/time (`IFD`), or integer (`IFI`) variables)
  * **bcConversion** : `IntToStr`, `StrToInt`, `StrToIntDef`, `FloatToStr`, `StrToFloat`, `StrToFloatDef`, `BoolToStr`, `StrToBool`, `StrToBoolDef`, `DateToStr`, `TimeToStr`, `StrToDate`, `StrToDateDef`, `StrToTime`, `StrToTimeDef`, `StrToDateTime`, `StrToDateTimeDef`
  * **bcAggregate** : `count`, `sum`, `avg`, `min`, `max`



`bcData`, `bcVaria`, and `bcUser` are not used anywhere within `fpexprpars`. The last section gives instructions how to extend the parser with more functions. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** These symbols are not [case-sensitive](<case-sensitive.md> "case-sensitive").

In order to use a mathematical expression the option `bcMath` has to be added to the parser's `Builtins`: 
    
    
      FParser.Builtins := [bcMath];   // or FParser.Builtins := FParser.Builtins + [bcMath];
    

## Expressions

### An expression with constants

As a first example we have the parser calculate a very simple expression `'1+1'`. 

The first step is to tell the parser which expression is to be calculated. There is a property `Expression` for this purpose: 
    
    
      FParser.Expression := '1+1';
    

The next step is to calculate the expression: just call `Evaluate` or `EvaluateExpression` \- the former is is a [function](<Function.md> "Function") while the latter one is a [procedure](<Procedure.md> "Procedure") which passes the result as a parameter. 
    
    
    var
      parserResult: TFPExpressionResult;
    begin
     ....
      parserResult := FParser.Evaluate;  // or: FParser.EvaluateExpression(parserResult);
    

What is that mysterious `TFPExpressionResult`? Since the parser is very flexible and can deal with numbers, strings, date/times or booleans there must be a more complex data type which returns a calculation result: 
    
    
    type
      TResultType = (rtBoolean, rtInteger, rtFloat, tDateTime, rtString);
    
      TFPExpressionResult = record
        ResString   : String;
        Case ResultType : TResultType of
          rtBoolean  : (ResBoolean  : Boolean);
          rtInteger  : (ResInteger  : Int64);
          rtFloat    : (ResFloat    : TExprFloat);
          rtDateTime : (ResDateTime : TDatetime);
          rtString   : ();
      end;
    

The member `ResultType` signals which one of the data fields is valid. It is important to understand this since the expression parser is very strict on [data types](<Data_type.md> "Data type"). 

In our example, we are adding two [integers](<Integer.md> "Integer"), therefore the result is an integer as well. If, on the other had, we had used the expression `'1.0 + 1'`, the first number would have been a floating point value, and the result would have been a float! Therefore, always have a look at the member `ResultType` of the `TFPExpressionResult` before picking the result. To simplify the usage of the expression result data type, `fpexprpars` exposes a function `ArgToFloat` which gets the entire expression result [record](<Record.md> "Record") as a parameter and selects the right component if a floating point result is expected: 
    
    
    var
      parserResult: TFPExpressionResult;
      resultValue: Double;
    ...
      parserResult := FParser.Evaluate;   // or: FParser.EvaluateExpression(parserResult);
      resultValue := ArgToFloat(parserResult);
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Floating point constants in expressions must have a [point](<period.md> "period") as decimal separator, not a [comma](<Comma.md> "Comma") as used in some European countries. If your expression string comes from user input and contains decimal commas you have to replace the commas by points first before assigning it to the parsers's `Expression`. 

### An expression with a variable

In this example, we calculate the value of `sin(x)*cos(2*x)` for `x = 0.5`. 

#### Defining variables

At first we have to define the variables. We have only one, `x`. The parser has a [method](<Method.md> "Method") `AddFloatVariable` to declare a floating point variable; there are also methods 

  * `AddBooleanVariable`
  * `AddStringVariable`
  * `AddDateTimeVariable`



for boolean, string and date/time variables, respectively. 

Each one of these methods expects the name of the variable along with its default value. For our sample function we just call: 
    
    
      FParser.Identifiers.AddFloatVariable('x', 0.5);
    

`0.5` is entered here as a default value because that is the argument at which we want to calculate the expression (it will be shown below how to modify a variable). From now on, the parser will use this number whenever it finds the variable `x` in the expression. 

Of course, you can add other names, e.g. [constants](<Constant.md> "Constant") like `e`, etc. (The number [`pi`](<Pi.md> "Pi") is already built-in). 

#### Defining the expression

In the next step, the expression string has to be passed to the parser: 
    
    
      FParser.Expression := 'sin(x)*cos(2*x)';
    

It is essential to call this after setting up of the variables because the parser needs to know the variables for analyzing the expression. 

#### Calculating the expression

This is done as before with the constant expression - here is a complete procedure which shows the equation and its result in a [message box](<Dialog_Examples.md> "Dialog Examples"): 
    
    
    var
      FParser: TFPExpressionParser;
      resultValue: Double;
    begin
      FParser := TFPExpressionParser.Create(nil);
      try
        FParser.BuiltIns := [bcMath];
        FParser.Identifiers.AddFloatVariable('x', 0.5);
        FParser.Expression := 'sin(x)*cos(2*x)';
        resultValue := FParser.Evaluate.ResFloat;  // or: resultValue := ArgToFloat(FParser.Evaluate);
        ShowMessage(FParser.Expression + ' = ' + FloatToStr(resultValue));
      finally
        FParser.Free;
      end;
    end;
    

#### Changing variables

So far, `x` always has the value 0.5 - it behaves like a constant, we could have used the expression `'sin(0.5)*cos(2*0.5)'` as well. 

To make it behave more like a “variable”, we now calculate the test function for the `x` values between `-10` and `10` at integer steps. 

The main question is: How to replace the value assigned to a variable? There are several possibilities - all of them require the internal variable `Identifier` (type `TFPExprIdentifierDef`) which exposes various ways to access variables and their properties: 

  * Use the return value of the `AddFloatVariable` function.
  * Seek an identifier by calling `FindIdentifierByName` with the variable name as a parameter.
  * Access the identifier from the `Identifiers` collection of the parser by using the known index of the variable: We had added `x` as the only variable, therefore, it must be at index 0.



Once the `Identifier` is known, the value of the variable can be changed by accessing the property `AsFloat` (or `AsDateTime` etc. accordingly): 
    
    
    var
      FParser: TFPExpressionParser;
      argument: TFPExprIdentifierDef;
      s: string;
      x: integer;
      f: double;
    begin
      s:='';
      FParser := TFPExpressionParser.Create(nil);
      try
        // Enable the use of mathematical expressions
        FParser.BuiltIns := [bcMath];
    
        // Add the function argument
        argument := FParser.Identifiers.AddFloatVariable('x', 0.0);
    
        // Define the function, using the argument name x as defined above
        FParser.Expression := 'sin(x)*cos(2*x)';
    
        // Calculate some function values
        for x := -10 to 10 do
        begin
          argument.AsFloat := x;                             // Set the function argument value
          f := FParser.Evaluate.ResFloat;                    // Calculate the function value
          s := s + format('x[%d]:[%g]' + LineEnding, [x,f]); // Demo output to display the result
        end;
    
        // Show the result
        ShowMessage(s);
    
      finally
        FParser.Free;
      end;
    end;
    

### Operators

The expressions discussed above show that standard operators `+`, `-`, `*`, `/`, `<`, `<=`, etc., can be used to setup an expression. Additionally, in recent FPC versions, the operator `^` is available for calculating the power, and `mod` for the remainder of integer divisions (modulo). 

### Aggregate functions

In addition to “normal” function the parser supports also “aggregate” functions which are repeatedly applied to a variable, e.g. `sum(x)` which adds up all the values assigned to the variable `x`. These functions are available: 

  * `min(x)` \- Calculate the minimum of all values assigned to `x`
  * `max(x)` \- Calculate the maximum of all values assigned to `x`
  * `sum(x)` \- Calculate the sum of all values assigned to `x`
  * `avg(x)` \- Calculate the average of the values assigned to `x`
  * `count(x)` \- Count the numbers assigned to `x`



In order to have aggregate functions available the category `bcAggregate` must be added to the `BuiltIns` of the parser. Next step is to define a variable to which the aggregate function will be applied. The aggregation process must be initialized by calling the parser method `InitAggregate`; and whenever the variable value is changed the method `UpdateAggregate` must be called. 

Here is an example which calculates the sum of the elements of an array: 
    
    
    uses
      fpexprpars;
    
    const
      Data: array[0..5] of Double = (1.221, 3.4, -1.0, -0.221, -3.0, -0.4);
    var
      FParser: TFPExpressionParser;
      xVar: TFPExprIdentifierDef;
      x: Double;
    begin
      FParser := TFPExpressionParser.Create(nil);
      try
        FParser.BuiltIns := [bcAggregate];
        xVar := FParser.Identifiers.AddFloatVariable('x', 0);
        FParser.Expression := 'sum(x)';
        FParser.InitAggregate;
        WriteLn('Sum of');
        for x in Data do
        begin
          xVar.AsFloat := x;
          WriteLn(xVar.AsFloat:10:3);
          FParser.UpdateAggregate;
        end;
        WriteLn('----------');
        WriteLn(FParser.Evaluate.ResFloat:10:3);
      finally
        FParser.Free;
      end;
    
      ReadLn;
    end.
    

## Adding user-defined functions

The default parser only knows the built-in functions mentioned above. One of the strengths of of the expression parser is that it is very easy to extend to include other functions. This can be done by calling the method `Identifiers.AddFunction`, e.g. 
    
    
      FParser.Identifiers.AddFunction('tan', 'F', 'F', @ExprTan);
    

In this example, we add the function `tan(x)` by specifying its name as it will be called in the function expressions (first parameter), the type of the result values (second parameter, "F" = float, "I" = integer, "D" = date/time, "S" = string, or "B" = boolean) and the type of the input values (third parameter, same logic). If a function accepts several input parameters the type of each one must be specified, e.g. by 'FF' for two floating point values, or 'FI' for a first float and a second integer parameter. The last parameter points to the function which is called whenever “tan” is found in the expression string. Since this function has a particular syntax we have to implement it in our own source code: 
    
    
    procedure ExprTan(var Res: TFPExpressionResult; Const Args: TExprParameterArray);
    var
      x: Double;
    begin
      x := ArgToFloat(Args[0]);
      Res.resFloat := tan(x);
    end;
    

The result of the calculation is returned as parameter `Res` which is of the `TFPExpressionResult` type that we met above. The arguments for the calculation are passed by `Args` which is just an array of `TFPExpressionResult` values - again because parameters can have several data types. The word `_Result_` in `TFPExpressionResult` is maybe a bit misleading here, because this array holds all the *input* parameters as specified by the input types of the `AddFunction` method. 

When, in fpc 3.2 or later, the input parameter symbol string of the `AddFunction` method ends with a `'+'` then parameters with the previous type can be repeated as often as needed; this way a function with a variable parameter count can be implemented. In the following example, we add the function `SumOf` which adds up as many values as provided: 
    
    
    procedure ExprSumOf(var Res: TFPExpressionResult; Const Args: TExprParameterArray);
    var
      sum: Double;
      arg: TFPExpressionResult;
    begin
      sum := 0;
      for arg in Args do
        sum := sum + ArgToFloat(arg);
      Res.ResFloat := sum;
    end;
    
    begin
      ...
      FParser.Identifiers.AddFunction('SumOf', 'F', 'F+', @ExprSumOf);
      FParser.Expression := 'SumOf(-1,2,3,4.1)';   // arbitrary number of arguments!
      WriteLn(FParser.Evaluate.ResFloat:0:3);
      ...
    

## Some special code

### Can I change the built-in function `sin(x)` to accept arguments in degrees, instead of radians?

Yes. All built-in functions are collected in the instance `BuiltInIdentifiers` of class `TExprBuiltInManager`. Find the built-in identifier (`BuiltInIdentifiers.Find('sin')`, delete it and replace it by a user-defined function having the requested behavior. In FPC 3.2 or later, the identifier can be removed directly (`BuiltInIdentifiers.Remove('sin')`), in order versions the `BuiltInIdentifiers` must be cast to a descendant of `TExprBuiltInManager` in order to access the `Delete` method of its protected collection `Defs`: 
    
    
    uses
      Math, fpExprPars;
     
    // these are needed only for fpc before v3.2
    {
    type
      TMyBuiltinManager = class(TExprBuiltinManager);
    var
      idx: Integer; 
    }
     
    var
      parser: TFPExpressionParser;
     
      Procedure ExprSin(Var Result: TFPExpressionResult; Const Args: TExprParameterArray);
      begin
        Result.resFloat := Sin(DegToRad(ArgToFloat(Args[0])));  // convert radians to degrees
      end;
     
    begin
      // fpc 3.2+
      BuiltInIdentifiers.Remove('sin');
      // or in older versions:
    { idx := BuiltinIdentifiers.IndexOfIdentifier('sin');
      TMyBuiltinManager(BuiltinIdentifiers).Defs.Delete(idx); 
    }
      BuiltinIdentifiers.AddFunction(bcMath, 'sin', 'F', 'F', @ExprSin);
     
      parser := TFPExpressionParser.Create(nil);
      try
        parser.BuiltIns := [bcMath];
        parser.Expression := 'sin(45.0)';
        WriteLn(parser.Evaluate.ResFloat:0:5);
        parser.Expression := 'sin(90.0)';
        WriteLn(parser.Evaluate.ResFloat:0:5);
        parser.Expression := 'sin(180.0)';
        WriteLn(parser.Evaluate.ResFloat:0:5);
      finally
        parser.Free;
      end;
                   
      WriteLn('Press ENTER to close.');
      ReadLn;
    end.

---

_Source: [https://wiki.freepascal.org/How_To_Use_TFPExpressionParser](https://web.archive.org/web/20250403180330/https://wiki.freepascal.org/How_To_Use_TFPExpressionParser)_
