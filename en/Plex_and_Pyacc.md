# Plex and Pyacc

Free Pascal comes with substitutes for the GNU projects Lex and YACC. They are called Plex and Pyacc and they can be used to generate compilers and regular expression analyzers in Pascal instead of C. 

## Contents

  * 1 Library contents
  * 2 Documentation
  * 3 Simple example application
    * 3.1 build.sh
    * 3.2 lexer.l
    * 3.3 parser.y
    * 3.4 How to use it
  * 4 How to do it the right way
  * 5 Additional Information
  * 6 Useful Links



## Library contents

TP Lex and Yacc can be found here: <http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/utils/tply/>

## Documentation

Download the manual here: <http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/utils/tply/tply.doc?revision=1>

## Simple example application

This is a very simple calculator, which shows how to use plex and pyacc. The files in this simple project are: 

  * build.sh
  * lexer.l
  * parser.y



WARNING: The code below compiles but currently doesn't work **< \-- Because it is simply wrong. See below and learn why !**

### build.sh
    
    
    plex lexer.l
    pyacc -d parser.y
    mv parser.pas calculadora.pas
    fpc calculadora.pas
    

### lexer.l
    
    
    { Analisador léxico da calculadora para a disciplina de PCS
    
       Aluno: Felipe Monteiro de Carvalho                           }
    
    %{
    %}
    %%
    
    [0-9]+[dD]?          begin yylval.yyInteger := StrToInt(yytext); yyruleno := NUMBER; end;
    
    [0-9a-fA-F]+[hH]     begin yylval.yyInteger := StrToInt(yytext); yyruleno := NUMBER; end;
    
    [ \t]                begin end; { ignorar espaços em branco }
    
    [-]                  begin yyruleno := MENOS; end;
    
    [+*/()]              begin yyruleno := Integer(yytext[0]); end;
    
    [=][hH]              begin yyruleno := IGUALH; end;
    
    [=][dD]?             begin yyruleno := IGUALD; end;
    
    \n                   begin yyruleno := Integer(yytext[0]); end;
    
    .                    begin yyerror('Caracter inexperado'); end;
    
    %%
    function meuyywrap (): Integer;
    begin
      Result := 1;
    end;
    

### parser.y
    
    
    /* Parser
    
       Aluno: Felipe Monteiro de Carvalho                           */
    %{
    program calculadora;
    
    {$mode delphi}
    
    uses SysUtils, yacclib, lexlib;
    
    %}
    
    %start entrada
    %token <Integer> NUMBER IGUALD IGUALH MENOS
    %type <Integer> expressao termo fator
    
    %%
    
    entrada
        : /* linha vazia */     {  }
        | entrada linha         {  }
        ;
    linha
        : expressao IGUALD '\n' { WriteLn(Format('Resultado: %d', [$1])); }
        | expressao IGUALH '\n' { WriteLn(Format('Resultado: %H', [$1])); }
        ;
    expressao
        : expressao '+' termo   { $$ := $1 + $3; }
        | expressao MENOS termo { $$ := $1 - $3; }
        | termo                 { $$ := $1; }
        ;
    termo
        : termo '*' fator       { $$ := $1 * $3; }
        | termo '/' fator       { if ($3 = 0) then
                                     yyerror('Divisao por zero!')
                                  else
                                     $$ := $1 div $3; }
        | fator                 { $$ := $1; }
        ;
    fator
        : NUMBER                { $$ := $1; }
        | MENOS NUMBER          { $$ := -1 * $2; }
        | '(' expressao ')'     { $$ := $2; }
        ;
    
    %%
    
    procedure meuyyerror(s: PChar);  // Called by yyparse on error
    begin
      WriteLn(Format('Erro: %s', [s]));
    end;
    
    {$include lexer.pas}
    
    begin
      yywrap := @meuyywrap;
    //  yyerror := @meuyyerror;
      yyparse ();
    end.
    

### How to use it

Type for example: 5+3= 

And it will answer with: 8 

You can also try 3*8=H to request an answer in Hexadecimal instead of decimal 

# How to do it the right way

**Sorry that I have to do this, but this example is wrong !**

First of all I recommend you to read the manual and try to understand it.  
Secondly take a look at the examples provided with original tply package by Albert Graef distributed at: <http://www.musikwissenschaft.uni-mainz.de/~ag/tply>   
There you will find a more sophisticated calculator example named 'expr'. If you take a look at it you will find out that this is a basic example for Lex/Yacc generated parsers and it works.  

    
    
    **Now I will explain you why you can NOT do things that way !**
    

_We start with Lex:_  


  1. Lex returns a token to the parser which is always an integer value. The token is defined as an constant value with the **%token** command. Tokens can only be returned to Yacc through two commands, **return** and **returnc**. 
     1. **return** : This is the normal return of a token = integer
     2. **returnc** : With this it is possible to return one character, it is needed for the literals in Yacc rules



Now we correct the return of a token from example above: 
    
    
    [0-9]+[dD]?          begin yylval.yyInteger := StrToInt(yytext); yyruleno := NUMBER; end; <- nothing, because yyruleno ends up in nirvana
    [0-9]+[dD]?          begin yylval.yyInteger := StrToInt(yytext); return(NUMBER); end;     <- good
    

And the litteral return : 
    
    
    [+*/()]              begin yyruleno := Integer(yytext[0]); end; <- ouch !
    [+*/()]              returnc(yytext[1]);                        <- returns single characters we want
    

Can you see the difference ?  
Why the hell one should use index NULL ? And no need for begins and ends.  
Last but not least the strange function: 
    
    
    function meuyywrap (): Integer; <- ??? what is this for ??? this no overload for yywrap !
    begin
      Result := 1;
    end;
    

The yywrap function is for loading multiple files, do we need this if we read from stdin and write to stdout ? No, we don't !  
  
The working Lex file: 
    
    
    { Analisador léxico da calculadora para a disciplina de PCS
     
       Aluno: Felipe Monteiro de Carvalho                           }
     
    %{
    %}
    %%
     
    [0-9]+[dD]?          begin yylval.yyInteger := StrToInt(yytext); return(NUMBER); end;
     
    [0-9a-fA-F]+[hH]     begin yylval.yyInteger := StrToInt(yytext); return(NUMBER); end;
     
    [ \t]                ; { ignorar espaços em branco }
     
    [-]                  return(MENOS);
     
    [+*/()]              returnc(yytext[1]);
     
    [=][hH]              return(IGUALH);
     
    [=][dD]?             return(IGUALD);
     
    \n                   returnc(yytext[1]);
     
    .                    yyerror('Caracter inexperado');
     
    %%
    

  
_Now we go on with Yacc:_  
The main block:  
If we want to read a file we have to assign and open it before we can parse it. If we want to read more files we have to use yywrap.  
With a parser like this we need only stdio. So we need to do nothing because Lex assigns and opens the 'files' known as stdin and stdout.  
With this we can forget all but the yyparse() call.  
  
Additional functionality:  
As a feature we want to write user defined error messages. For this we simply overload the yyerror procedure.  
  
The working Yacc file: 
    
    
    /* Parser
     
       Aluno: Felipe Monteiro de Carvalho                           */
    %{
    program calculadora;
     
    {$mode delphi}
     
    uses SysUtils, yacclib, lexlib;
     
    procedure yyerror(s: string);  // Called by yyparse on error <- overload !
    begin
      WriteLn(Format('Erro: %s', [s]));
    end;
     
    %}
     
    %start entrada
    %token <Integer> NUMBER IGUALD IGUALH MENOS
    %type <Integer> expressao termo fator
     
    %%
     
    entrada
        : /* linha vazia */     {  }
        | entrada linha         {  }
        | entrada linha '\n'    { writeln('DONE.'); yyaccept; } /* <- nice end with RETURN */
        ;
    linha
        : expressao IGUALD '\n' { WriteLn(Format('Resultado: %d', [$1])); }
        | expressao IGUALH '\n' { WriteLn(Format('Resultado: %Xh', [$1])); } /* <- hex = X , add 'h' to identify this result as a hex number */
        ;
    expressao
        : expressao '+' termo   { $$ := $1 + $3; }
        | expressao MENOS termo { $$ := $1 - $3; }
        | termo                 { $$ := $1; }
        ;
    termo
        : termo '*' fator       { $$ := $1 * $3; }
        | termo '/' fator       { if ($3 = 0) then
                                     yyerror('Divisao por zero!')
                                  else
                                     $$ := $1 div $3; }
        | fator                 { $$ := $1; }
        ;
    fator
        : NUMBER                { $$ := $1; }
        | MENOS NUMBER          { $$ := -1 * $2; }
        | '(' expressao ')'     { $$ := $2; }
        ;
     
    %%
     
    
    {$include lexer.pas}
     
    begin
      yyparse ();
    end.
    

Hint: One does not need to type tokens as done in this example as the default yylval is always integer. If you do type tokens pyacc will generate a corresponding YYStype for you.  
**WOW. The calculator works as expected !**  


[GVS](</index.php?title=User:GVS&action=edit&redlink=1> "User:GVS \(page does not exist\)") 00:09, 9 March 2014 (CET)   


# Additional Information

If you are a beginner you may have more questions than answers after reading the above. Thus I will try to show you things to make live easier.  
We start with Lex and its regular expressions. I don't explain how to build them, read the documentation to find this out.  
I will try explain some implementation details.  
  
A lexer can use multiple rules that start with the same character or set of characters. This is not recommended because it can cause a problem. The main problem is that the lexer uses the rule with the longest match and returns that token. Now if you return different tokens with these rules you may not be able to match the yacc rule you expected. This is the common error if you write wrong lex rules and wonder why yacc returns syntax errors. If you use such rules in a way they can be identified correctly this can be a benefit. But with such rules you should take care if you need the 'loop' as in (a|b)* which can consist of a unknown number of 'a' or 'b' or even be empty. This is not a good start or end for your rule if the 'constant' characters do not cause a significant match.  
One common use is the detection of numbers where you have two rules, one for integers and one for reals. The rule for integer is then the beginning of the rule for real and there is no problem. There is also no problem if these rules return the same token. Then the only question is if you can find a better rule that includes both single rules in one rule.  
Example: Integer and Real numbers without sign 
    
    
    [0-9]+                  ReturnVerb(INUM,1)
    '.'[0-9]+               ReturnVerb(RNUM,2)
    [0-9]+"."[0-9]+         ReturnVerb(RNUM,3)
    

Now you ask: Why should I parse numbers without sign ? This is a yacc problem, because the sign can be in an expression as a operator e.g. 'a+b' or as a prefix sign e.g. '-1'. Then you may have a token defined for MINUS and you don't need to know if the _sign_ is a prefix sign or an operator, the yacc rules will handle it.  
Taking a look to the code you will see that I returned the token via the 'ReturnVerb' procedure. This is for debugging the lexer to see the token at the time it is matched. The procedure writes the token number and rule index and then calls the normal return procedure. This is useful because I see the token and the text representation of the matching rule. The text can be read from yytext which is always the current match.  
One issue you should keep in mind: The lexer is a function that is called from the parser and the token is returned at the point of time the function exits. The parser uses yylval only if he needs this value and one can not find out at which time this will be. The only valid way to return a value to your parser is to use yylval AND to set this value right before the call of return or returnc, otherwise the value is lost or you run into trouble. For details see below at the returnc explanation.  
  
You can combine rules with predefined rule parts standing before the rule part in your file. This makes it easier to write longer rules and you can use them in multiple rules.  
Example: Rules consisting of predefined parts 
    
    
    L				[A-Za-z]
    D				[0-9]
    
    %%
    
    {D}+(\.{D}+)?([Ee][+-]?{D}+)?	return(RNUM);
    
    {L}				return(CH);
    
    " "             		;
    
    .				|
    \n				returnc(yytext[1]);
    

One important thing you can see in this example is that one always has to catch all characters of input. Even if you don't need them like the SPACE. Typically in programming languages the so called WHITE SPACE is ignored at all. This is the most difficult thing to match all the unused characters to empty rules without breaking the rules for tokens (difficult because they do not appear in your output where wrong parsed and returned tokens can be found, things you don't see are hard to find). Empty rules do not call return or returnc. If you can not find a rule you can use built in functions to to read the characters yourself or write additional characters to input. This is commonly used with comments. After a initial match of a short rule to match the start of a comment the input is read until the end of the comment or EOF is found.  
Example: Line Comment 
    
    
    %{
    procedure CommentEOL;
     var c:char;
    begin
      c:= get_char;
      while (c <> #0)and(c <> #10) do  c:= get_char;
    end; 
    %}
    "//"         CommentEOL;
    

With this all the characters the get_char function reads from input are lost. If one is reading a multi line comment the ending maker may have to be written back. This is done with unget_char. This characters can than be read in next call of yylex to be evaluated.  
Example: A language with '/' as an ID, '//' as a unused sequence until EOL or #26 
    
    
    "/"        begin
                 c := get_char;
                 if c <> '/' then
                 begin
                   unget_char(c);
                   ReturnC(yytext[1]);
                 end
                 else
                 begin  (* line comment *)
                   while ((c<>#10)and(c<>#13)and(c<>#26)) do c := get_char;
                   if c=#26 then unget_char(c);
                 end;
               end;
    

  
Lets talk about returnc.  
As I said above (at the correction part) that tokens are always integer but returnc gets an character as an argument you might say that is not true. But it is, returnc uses ORD to convert the character to integer. Now you will understand why your token constants (have you look at yacc generated pasacl code) are always higher than 256; its the full ASCII range for litteral tokens so other tokens are higher.  
If you use returnc you can deal with literals in yacc rules but you have to make sure to return ALL the characters you use in yacc. Yacc does not care about not returned characters and your rules will never match.  
  
The short way to debug lex is to overload the return procedures.  
Example: Overload lex return 
    
    
    procedure return ( n : Integer );
    begin
       writeln('return  >',yytext,'< = ',n);
      (* now do the 'inherited' code return would do *)
        yyretval := n;
        yydone := true;
    end(*return*);
    
    procedure returnc ( c : Char );
    begin
       writeln('returnc >',yytext,'< c = ',c,' , n = ',ord(c));
       (* now do the 'inherited' code returnc would do *)
        yyretval := ord(c);
        yydone := true;
    end(*returnc*);
    

The result of this is shown next for calculating 4*5 as a hex value with the calculator above: 
    
    
    4*5=h
    return  >4< = 257
    returnc >*< c = * , n = 42
    return  >5< = 257
    return  >=h< = 259
    returnc >
    < c = 
     , n = 10
    Resultado: 14h
    

Remark: You see the NEWLINE from input causing the same on output (n = 10).  
  
Now its time for Yacc.  
  
Benefits of literals in rules are that you can use them with less trouble. Another thing is that the rules are more readable. The trouble with non literal tokens is that you easily generate conflicts in yacc rules. This is a problem with tokens that appear very often in your rules and thus the parser has to do different things with them. It is always a good idea to write rules in such a way that their beginning is significant. If you can not write such rules you might have to change your lexer.  
Using EMPTY with rules cause trouble too if you use them in combination with loops. In loops you can have only one rule that produces empty. The other rules have to have a significant beginning/ending or part (this needs to be unique in this loop of rules). It is also a problem if you have different rules that end with the same token and the loop separator is this token. Then the parser can not decide what to do if this token is found. Typically the use of SEMICOLON can cause this in a pascal parser (semicolons are not always allowed or close different rules that are not significant itself).  
Example: FPC Parser snippet to parse CLASS  

    
    
    Class_Typed_Type : _CLASSFORWARD /* 'Class ;' */
                    | LClass_PRE _OF { LDisable(co_Class); } id
    /* Class Type */
                    | LClass_PRE Class_Component_List { LDisable(co_Class); }
                    | LClass_PRE '(' Class_Implemented_Interface ')'
                                 Class_Component_List { LDisable(co_Class); }
                    ;
    LClass_PRE : _CLASS { LEnable(co_Class); } ;
    Class_Implemented_Interface :
     id
     | id ',' Class_Interface_Identifiers
     ;
    Class_Interface_Identifiers : Y_Id_Comma_List ;
    Class_Component_List : OPT_Class_Components _END ;
    OPT_Class_Components :
     /* EMPTY */
     | Class_Component_LOOP
     ;
    Class_Component_LOOP : /* LOOP */
     Class_Component
     | Class_Component_LOOP Class_Component
     ;
    Class_Component :
     Class_Visibility_Specifier
     | Class_Field_Definition
     | Class_Method_Definition { LClassModifiers(false); }
     | Class_Property_Definition
     ;
    Class_Visibility_Specifier : /* 'private' | 'public' | 'published' | 'protected' | 'strict' 'private' ; */
     CVI_PRIVATE
     | CVI_PROTECTED
     | NKW_PUBLIC
     | CVI_PUBLISHED
     | CVI_STRICT CVI_PRIVATE
     ;
    
    Class_Field_Definition : /* all decl starting with id as a field */
     /* Const
        type */
     Class_Variable_Declaration
     | Class_ClassVariable_Declaration
     ;
    Class_Variable_Declaration :
     Y_Id_Comma_List ':' { LEnable(co_ClassVarStatic); LHints(true); } RClass_Variable_Declaration ;
    RClass_Variable_Declaration :
     Result_Type OPT_Hint ';' OPT_Static { LDisable(co_ClassVarStatic); LHints(false); }
     ;
    OPT_Static :
     /* EMTY */
     | NKW_STATIC ';'
     ;
    Class_ClassVariable_Declaration : _CLASS Class_Variable_Declaration ;
    Class_Method_Definition :
     Class_PuF_Definition
     | Class_CuD_Definition
     | _CLASS Class_PuF_Definition
     ;
    
    Class_PuF_Definition : PuF_Definition ;
    Class_CuD_Definition :
       _CONSTRUCTOR PLike_Definition
     | _DESTRUCTOR  PLike_Definition
     ;
    

This are context sensitive rules to switch between the different cases CLASS can be used in a class type declaration. The LEnable and LDisable switches the lexer to a different mode. To do this the line 'LClass_PRE : _CLASS { LEnable(co_Class); } ;' is a own rule that has to be completed first to give the lexer time to switch the modes (the next lookahead character of the parser triggers the rule end; the parser has to reduce the rule to get to the next state before the next token is evaluated which we want to switch to a different token).  
Remark: This example parser switches between different sets of KEYWORDS because not all tokens that are used are real RESERVED words of the language. This means that e.g. 'public' is not a keyword and is only enabled on context as 'NKW_PUBLIC'. The '_CLASSFORWARD' is a typical conflict where the string 'Class ;' terminates with SEMICOLON. Thus the lexer does a lookahead if a semicolon is found behind 'Class' and then returns the token '_CLASSFORWARD' instead of '_CLASS' and the problem is solved.  
  
The rule in between pitfall:  
Example: Hello in between 
    
    
    rule1 : ID rule2 { writeln($2); }
    
    rule2 : ID1 { $$:='Hello'; } ID3 { $$:= $2; }
    

'Hello' can only be assigned with the last statement '{ $$:= $2; }' as yacc generates a new state for the rule in between. The assignment must be at the end of this rule.  
  
Additional explanation to this last two examples:  
This are different cases ! The first example is for switching the lexer and the second example is for parsers internal value handling to return the right value to the next rule.  
  


# Useful Links

[Make your own compiler, interpreter, parser, or expression analyzer](<Make_your_own_compiler,_interpreter,_parser,_or_expression_analyzer.md> "Make your own compiler, interpreter, parser, or expression analyzer")

  
[GVS](</index.php?title=User:GVS&action=edit&redlink=1> "User:GVS \(page does not exist\)") 04:48, 9 March 2014 (CET)

---

_Source: [https://wiki.freepascal.org/Plex_and_Pyacc](https://web.archive.org/web/20240719121046/https://wiki.freepascal.org/Plex_and_Pyacc)_
