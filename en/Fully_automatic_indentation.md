# Fully automatic indentation

## Contents

  * 1 Overview
  * 2 How it works
    * 2.1 Indent on paste from clipboard
  * 3 Examples
    * 3.1 begin end
    * 3.2 repeat until
    * 3.3 try finally
    * 3.4 case of end
    * 3.5 if then else



# Overview

Since 0.9.29 the lazarus source editor has a new algorithm for automatic indentation on pressing Enter or on pasting code from clipboard. This new algorithm works only for pascal and does not use static rules, but parses the sources to imitate the indentation. Most other editors use either a fixed set of rules or a set of options to configure the rules. This is _semi automatic_ indentation. These options are either too simple or too complex. And the other editors only allow one set of rules, so editing sources with different policies is difficult. 

**Fully automatic indentation** tries to guess the rules from the surrounding code. It does that by searching for similar code and copying the indent. 

Because it works fully automatic there are only a few options. You can disable it, you can setup the search scope and you can provide an example code: [Codetools Options: Indentation](<IDE_Window__Codetools_Options.md> "IDE Window: Codetools Options"). You do not need to learn rules, just write pascal. 

# How it works

## Indent on paste from clipboard

For example you have the code: 
    
    
    procedure TMainForm.Button1Click(Sender: TObject);
    begin
    |
    end;
    

You place the cursor at column 1 and paste the code 
    
    
        if Visible then
        begin
        end;
    

Note that the code on the clipboard is already indented by 4. 

The indenter first scans the code, finds out, that the insertion position is in a _begin..end_ block of a _procedure_. With the standard options it searches the code in front for other _procedure begin..end_ blocks. If it does not find the code it searches behind the code, then in all units of the project (or package) and finally in the example code of the options. For instance it finds: 
    
    
    procedure TMainForm.FormPaint(Sender: TObject);
    begin
      with Canvas do FillRect(0,0,Width,Height);
    end;
    

The indentation of the first token _with_ is 2. The indentation of the first token of clipboard code is 4, so the indenter unindents the code by 2, resulting in: 
    
    
    procedure TMainForm.Button1Click(Sender: TObject);
    begin
      if Visible then
      begin
      end;|
    end;
    

The indenter always indents the whole block, not every line. For example pasting 
    
    
    DoSomething(Param1,
                Param2);
    

will keep the indent both lines by the same amount, keeping the two _Params_ aligned. 

The indenter scans what is inserted. For instance pasting 
    
    
    end;
    

will end the the begin block, resulting in: 
    
    
    procedure TMainForm.Button1Click(Sender: TObject);
    begin
    end;|
    end;
    

# Examples

Sometimes record and classes are aligned to the keywords instead of start of last line. 
    
    
    type TMyRecord = record
                     i: integer;
                     end;
    

At the moment the indenter supports only amount of spaces, not aligning to keywords or symbols. 

## begin end
    
    
    procedure Do;
    var
      i: integer; // indent after var
    begin         // no indent after procedure, no indent before procedure-begin, unindent after var section
      if expr then
      begin       // no indent after then, no indent before then-begin
        Code;     // indent after begin
      end;        // unindent before end
    end;
    
    
    
    procedure Do;
    begin
      if expr then begin  
        Code;             // indent after begin
      end;                // unindent before end
    end;
    
    
    
    procedure Do;
    begin
      if expr then
        begin   // indent after then and unindent after then-statement
        Code;   // no indent after begin
        end;    // no unindent before end
    end;
    
    
    
    procedure Do;
    begin
      if expr then
        begin     // indent after then and unindent after then-statement
          Code;   // indent after begin
        end;      // unindent before end
    end;
    

## repeat until

I never saw code other than this: 
    
    
    repeat
      Code; // indent after repeat
    until ; // unindent before until
    

## try finally

I never saw code other than this: 
    
    
    try
      Code;     // indent after try
    finally     // unindent before finally
      on e do ; // indent after finally
    end;        // unindent before finally-end
    

## case of end
    
    
    case expr of
    1: ;   // no indent after case-of
    2: 
      Code;    // indent after case-colon
    3: 
      begin    // indent after case-colon
        Code;  // indent after case-colon-begin
      end;     // unindent before case-colon-end, unindent after case-colon-end
    else       // no indent case-else 
      Code;    // indent after case-else
    end;       // unindent after case-else statements
    
    
    
    case expr of
               
      1: ; // indent after case-of
    
      2: 
      begin    // no indent after case-colon
      end;
    else       // no indent before case-else
    end;       // no indent before case-end
    

## if then else
    
    
    if expr then
      Code
    else
      Code;
    
    
    
    if expr or
      expr or
      expr then
      Code
    else
      Code;
    
    
    
    if expr 
      or expr
    then
      Code
    else
      Code;
    
    
    
    if SrcEdit.SelectionAvailable
    and SrcEdit.CaretInSelection(CaretPos)
    then Expression := SrcEdit.GetText(True)
    else Expression := Identifier;
    if not DebugBoss.Evaluate(Expression, DebugEval)
    or (DebugEval = '')
    then DebugEval := '???';

---

_Source: [https://wiki.freepascal.org/Fully_automatic_indentation](https://web.archive.org/web/20240430010437/https://wiki.freepascal.org/Fully_automatic_indentation)_
