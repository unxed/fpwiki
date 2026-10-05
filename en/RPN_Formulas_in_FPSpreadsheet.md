# RPN Formulas in FPSpreadsheet

## Contents

  * 1 Introduction
  * 2 Understanding RPN formulas
  * 3 Using constant values in RPN formulas
  * 4 Using cells in RPN formulas
  * 5 Using ranges of cells
  * 6 Using built-in operations and functions
  * 7 Displaying RPN formulas



## Introduction

[FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet") usually uses **string formulas** , just like the Office applications (Excel, OpenOffice/LibreOffice) do. In this syntax, formulas are expressed as human-readable strings showing the function names with arguments in brackets. 

In addition, FPSpreadsheet supports also **RPN formulas** (RPN = "reverse polish notation) which are used internally in an intermediate stage after parsing a string formula. They are important for the binary Excel file format which stores formulas in this way. 

This type of formulas will be discussed here. 

If one wants to familiarize themself in PRN logic with quick toying platform one should look up some HPs programmable scientific pocket calculators ( or one of those free emulators ie. free42 or still WIP newRPL OS for 50g HW (RPL = Reverse Polish Lisp)) from 1970 to 2016 as most of them are based on RPN notation (and stack)for maths and programming. 

## Understanding RPN formulas

RPN formulas consist of **tokens** , i.e. information on the constituents of the formula in a way that can be immediately used for calculation of the expression result. There are tokens for numbers, operations, functions etc. When parsing, the tokens are extracted from the expression string and pushed onto a stack. In FPSpreadsheet, this stack corresponds to the array `TsRPNFormula`, the array elements correspond to the tokens on the stack. When calculating the formula, Excel traverses the stack bottom-up (meaning in FPSpreadsheet: from low- to high-index array elements). Whenever it finds a token for an operation or function it removes this token from the stack, along with the tokens of the operands, and replaces them by the result of the calculation. 

Here's an example: In a simple expression like `"=4+5"`, the stack contains the tokens for the number constants: 

  * the first argument: `[4]`
  * the second argument: `[5]`
  * the operation `[+]`.



The "+" operation is a binary operation, meaning that it needs two arguments. Therefore, when Excel reaches the `[+]` token, it removes the `[+]` and both operands from the stack and replaces them by the result of the calculation, the token with the value 9. Since there are no other elements on the stack, this is the final result of the calculation. 

Now a more complex examples: `"=ROUND(2+4*3.141592, 2)"` which rounds the result of the calculation `2+4*3.141592` to two decimals. The function `"ROUND"` requires two parameters: the value to be rounded, and the number of decimal places. In total, the stack consists of these elements: 

  * `[2]`
  * `[4]`
  * `[3.141592]`
  * `[*]`
  * `[+]`
  * `[2]`
  * `[ROUND]`



Going from first to last, the first operation/function token met is `[*]`. As this is another binary operation, this requires two arguments. Therefore, `[4]`, `[3.141592]` and `[*]` are removed from the stack and replaced by the result `[12.56637]`. 

Now the stack looks like: 

  * `[2]`
  * `[12.56637]`
  * `[+]`
  * `[2]`
  * `[ROUND]`



Now, the first operation token found is `[+]` replacing `[2]`, `[12.56637]`, `[+]` by `[14.56637]`. Finally, the stack is left with the tokens needed for the ROUND function: 

  * `[14.56637]`
  * `[2]`
  * `[ROUND]`



which immediately leads to the final result `[14.57]`. 

## Using constant values in RPN formulas

For coding above formula `"=4+5"` in FPSpreadsheet the length of the RPNFormula array must be set to 3 (3 elements, "4", "5", "+"). The first and second elements are "numbers" which has to be indicated by setting `ElementKind=fekNum` for these array elements. The value of each number is specified as the `DoubleValue` of the formula element. The last element is the formula which is specified by the `ElementKind` of `fekAdd`. 

In total, this results in the following code: 
    
    
    var
      MyRPNFormula: TsRPNFormula;
    begin
      // Write the formula =4+5
      MyWorksheet.WriteUTF8Text(3, 0, '=4+5'); // A4
      // Write the RPN formula to the spreadsheet
      SetLength(MyRPNFormula, 3);
      MyRPNFormula[0].ElementKind := fekNum;
      MyRPNFormula[0].DoubleValue := 4.0;
      MyRPNFormula[1].ElementKind := fekNum;
      MyRPNFormula[1].DoubleValue := 5.0;
      MyRPNFormula[2].ElementKind := fekAdd;
      MyWorksheet.WriteRPNFormula(3, 2, MyRPNFormula);
    end;
    

This requires quite some typing. For simplification a methodology of nested function calls has been added to FPSpreadsheet in which every element is specified by a function which links to the next element function via its last argument: 
    
    
    begin
      // Write the formula =4+5
      MyWorksheet.WriteUTF8Text(3, 0, '=4+5');
      // Write the RPN formula to the spreadsheet
      MyWorksheet.WriteRPNFormula(3, 2,  // Row and column of the formula cell
        CreateRPNFormula(                // function to create a compact RPN formula
          RPNNumber(4,                   // 1st operand: a number with value 4
          RPNNumber(5,                   // 2nd operand: a number with value 5
          RPNFunc(fekAdd,                // function to be performed: add
          nil)))));                      // end of list
    end;
    

## Using cells in RPN formulas

Of course, the formulas can also contain links to cells. For this purpose the `ElementType` needs to be `fekCellValue`. This instructs Excel to use the value of the cell in the calculation. There are, however, also functions which require other properties of the cell, like format or address. For this case, use `fekCellRef` for the `ElementKind`. Another specialty is the usage of absolute and relative cell addresses (`$A$1` vs. `A1`, respectively). Cell row and column addresses specified in the RPNFormula elements are absolute by default. If you want relative rows/columns add `rfRelRow` or `rfRelCol` to the element's `RelFlags` set. Or, if you prefer the nested function notation simply use the function `RPNCellValue` (or `RPNCellRef`) with the standard notation of the cell adress using the `$` sign. 

Here, as an example, `=A1*$B$1` in array notation: 
    
    
    var
      MyRPNFormula: TsRPNFormula;
    begin
      SetLength(MyRPNFormula, 3);
      MyRPNFormula[0].ElementKind := fekCellValue;
      MyRPNFormula[0].Row := 0;   // A1
      MyRPNFormula[0].Col := 0;
      MyRPNFormula[0].RelFlags := [rfRelRow, rfRelCol];  // relative!
      MyRPNFormula[1].ElementKind := fekCellValue;
      MyRPNFormula[1].Row := 1;
      MyRPNFormula[1].Col := 0;   // $B$1, RelFlags not needed since absolute address
      MyRPNFormula[2].ElementKind := fekMul;
      MyWorksheet.WriteRPNFormula(3, 2, MyRPNFormula);
    end;
    

And now in nested function notation: 
    
    
      MyWorksheet.WriteRPNFormula(3, 2,  // Row and column of the formula cell
        CreateRPNFormula(                // function to create a compact RPN formula
          RPNCellValue('A1',             // 1st operand: contents of cell "A1" (relative!)
          RPNCellValue('$B$1',           // 2nd operand: contents of cell "$B$1" (absolute!)
          RPNFunc(fekMul,                // function to be performed: multiply
          nil)))));                      // end of list
    

## Using ranges of cells

In spreadsheet applications like Excel, the notation `A1:C5` refers to a range of cells: the rectangle between (and including) cells `A1` and `C5`. 

This feature is available in FPSpreadsheet as well: use the `ElementKind` `fekCellRange` and a second set of row/column indices (`Row2` and `Col2`, respectively). There are also flags `rfRelRow2` and `rfRelCol2` to mark the second corner cell as relative. 

## Using built-in operations and functions

Here is a list of the **basic operations** available in FPSpreadsheet RPN formulas: 

ElementKind  | Example  | Meaning  | Operands  | Argument types  | Argument function   
---|---|---|---|---|---  
fekAdd | `=A1+A2` | add numbers | 2 | fekNum, fekCellValue | RPNNumber(), RPNCellValue()   
fekSub | `=A1-A2` | subtract numbers | 2 | fekNum, fekCellValue | RPNNumber(), RPNCellValue()   
fekMul | `=A1*A2` | multiply numbers | 2 | fekNum, fekCellValue | RPNNumber(), RPNCellValue()   
fekDiv | `=A1/A2` | divide numbers | 2 | fekNum, fekCellValue | RPNNumber(), RPNCellValue()   
fekPercent | `=A1%` | divide a number by 100 and add "%" sign | 1 | fekNum, fekCellValue | RPNNumber(), RPNCellValue()   
fekPower | `=A1^2` | power of two numbers | 2 | fekNum, fekCellValue | RPNNumber(), RPNCellValue()   
fekUMinus | `=-A1` | unary minus | 1 | fekNum, fekCellValue | RPNNumber(), RPNCellValue()   
fekUPlus | `=+A1` | unary plus | 1 | fekNum, fekCellValue | RPNNumber(), RPNCellValue()   
fekConcat | `="Hello "&A1 ` | string concatenation | 2 | fekString, fekCellValue | RPNString(), RPNCellValue()   
  
Column "Operands" indicates how many operands are required on the stack before the function. 

Beyond that, Excel provides a huge number of **functions** , many of which have been made available for FPSpreadsheet via the `fekFunc` symbol. To specify the formula you must pass the formula's name to the `FuncName` element of the `FormulaElement`. The formula name can be found in the 1st column of the table in [this page](<FPSpreadsheet__List_of_formulas.md> "FPSpreadsheet: List of formulas"). 

Here is an example which calculates the sine function of the number in cell A1: 
    
    
      MyWorksheet.WriteRPNFormula(3, 2,  // Row and column of the formula cell
        CreateRPNFormula(                // function to create a compact RPN formula
          RPNCellValue('A1',             // 1st operand: contents of cell "A1" 
          RPNFunc('SIN',                 // function to be performed: 'SIN()'
          nil))));                       // end of list
    

or, in array syntax: 
    
    
    var
      MyRPNFormula: TsRPNFormula;
    begin
      SetLength(MyRPNFormula, 2);
      MyRPNFormula[0].ElementKind := fekCellValue;
      MyRPNFormula[0].Row := 0;   // A1
      MyRPNFormula[0].Col := 0;
      MyRPNFormula[0].RelFlags := [rfRelRow, rfRelCol];  // relative!
      MyRPNFormula[1].ElementKind := fekFunc;
      MyRPNFormula[1].FuncName := 'SIN';
      MyWorksheet.WriteRPNFormula(3, 2, MyRPNFormula);
    end;
    

Please note that some functions allow a variable count of parameters. In this case, this value has to be specified as `ParamsNum` in the formula. The function `SUM`, for example, accepts up to 30 parameters. For calculating the sum of all numbers in the range A1:C10, therefore, we have to specify explicitly that a single parameter (the cell block A1:C10) is used: 
    
    
      SetLength(MyRPNFormula, 2);
      MyRPNFormula[0].ElementKind := fekCellRange;
      MyRPNFormula[0].Row := 0;   // A1
      MyRPNFormula[0].Col := 0;
      MyRPNFormula[0].Row2 := 9;  // C10
      MyRPNFormula[0].Col2 := 2;
      MyRPNFormula[0].RelFlags := [rfRelRow, rfRelCol, rfRelRow2, rfRelCol2];
      MyRPNFormula[1].ElementKind := fekFUNC;
      MyRPNFormula[1].FuncName := 'SUM';
      MyRPNFormula[1].ParamsNum := 1;      // 1 argument used in SUM
      MyWorksheet.WriteRPNFormula(1, 2, MyRPNFormula);  // cell C2
    

or, shorter: 
    
    
      MyRPNFormula.WriteRPNFormula(1, 2, CreateRPNFormula(
        RPNCellRange('A1:C10',
        RPNFunc(fekSUM, 1,        // SUM with 1 argument
        nil))));
    

## Displaying RPN formulas

xls files store formulas in RPN notation internally. When such a file is read FPSpreadsheet reconstructs the string formula automatically. 

Please note that the order of calculation is defined by the order of tokens in the RPN formula. The RPN formula by itself does not require parentheses as they would be needed for string formulas. However, this can cause problems when reconstructing string formulas from RPN formulas. For example, suppose the formula "`=(1+2)*(2+3)`". This is parsed to the token sequence `[1], [2], [+], [2], [3], [+], [*]` which makes sure that the correct order of operations is used. When the formula is reconstructed by `ReadRPNFormulaAsString`, however, it will be displayed as "`=1+2*2+3`" which obviously is not correct. To avoid this problem Excel provides a particular "parenthesis" token. In fpspreadsheet, add a `fekParen` token to the token array to put the preceding expression in parenthesis: 
    
    
      MyWorksheet.WriteRPNFormula(0, 0, CreateRPNFormula(
        RPNNumber(1,
        RPNNumber(2,
        RPNFunc(fekAdd,
        RPNParen,        // <--- this sets the parenthesis around the term (1+2)
        RPNNumber(2,
        RPNNumber(3,
        RPNFunc(fekAdd,
        RPNParen,        // <--- and this is the parenthesis around (2+3)
        RPNFunc(fekMul,
        nil))))))))));
    

It should be emphasized again that the parenthesis token does not have an effect on the calculation result, only on the reconstructed string formula.

---

_Source: [https://wiki.freepascal.org/RPN_Formulas_in_FPSpreadsheet](https://web.archive.org/web/20250418095431/https://wiki.freepascal.org/RPN_Formulas_in_FPSpreadsheet)_
