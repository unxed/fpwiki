# Gold

Gold is a free parsing system compatible with Free Pascal that you can use to develop your own programming languages, scripting languages, interpreters, and all kind of parsers and expression analyzers. It uses [LALR](<https://en.wikipedia.org/wiki/LALR_parser>) parsing, and a mix of [BNF](<https://en.wikipedia.org/wiki/Backus–Naur_form>) notation, character sets and regular expressions for terminals to define language grammars. Code and grammar are separated, so grammar is not tied to implementation language. This means that the same grammar can be loaded into engines made in different programming languages. 

There is a subjective [feature comparison table](<http://goldparser.org/about/comparison-parsers.htm>) of several parsers on Gold site, with special attention to [Gold vs Yacc](<http://goldparser.org/about/comparison-yacc.htm>) comparison. 

## Contents

  * 1 Gold Parser Builder
  * 2 Gold Engines
    * 2.1 Engines for FreePascal and Lazarus
      * 2.1.1 Lazarus Gold Engine 2018
      * 2.1.2 Gold Parser for Lazarus
      * 2.1.3 Gold Parser for Free Pascal and Lazarus
      * 2.1.4 FPC Gold Engine with VS Code parser plugin and code generator
    * 2.2 Engines Usage Examples
      * 2.2.1 JSON example
        * 2.2.1.1 JSON Grammar
        * 2.2.1.2 JSON Test Code
        * 2.2.1.3 JSON screenshots
      * 2.2.2 Pascal example
        * 2.2.2.1 Pascal Grammar
        * 2.2.2.2 Pascal Test Code
        * 2.2.2.3 Pascal screenshots
  * 3 Goldie Tools
  * 4 Useful links



# Gold Parser Builder

[GOLD Parser Builder](<http://goldparser.org/>) can be used to create, modify and test languages in Windows IDE (runs on Wine, too). Command line tools are also available. 

Features: 

  * Grammar editor with syntax highlighting.
  * Grammar generating wizard.
  * Test window to step through parsing of a sample source.
  * Templating system that can generate lexers/parsers or skeleton programs for various languages (including Delphi and FreePascal).
  * Importing/exporting YACC/Bison format. Exporting XML and HTML format.
  * Interactive inspection of the compiled DFA and LALR tables.



Importing YACC/BISON can be very picky, so you will probably have to convert linux line endings and delete most of the not needed code leaving just BNF rules. Import from lex file does not exist, so you will have to examine lex file yourself to see if there are terminals that you should manually add to your GOLD grammar. 

# Gold Engines

Gold Parser Builder has an option to build skeleton application for Delphi and FreePascal from existing grammar. However, there are also user made Gold Engines for FreePascal/Lazarus which can load compiled grammar (CGT v1.0 or EGT v5.0 file) and parse some file according to loaded grammar. 

Gold Parser Builder has changed CGT file format in version 5 to EGT. EGT v5.0 file format is used for all Gold Parser Builder versions newer then 5.0.0 (including 5.2.0 which is the actual version at the moment of writing this text), and CGT v1.0 for all older versions. This means that with older engines you will need to use older Gold Parser Builder. Although Gold parser Builder can still save CGT v1.0 file format, engines will sometime have trouble with it, so it's best to use older Gold Parser Builder if you need CGT v1.0. You can find old v3.4.4 version [here](<http://www.semitwist.com/download/gold-parser-builder-3-4-4.zip>). 

## Engines for FreePascal and Lazarus

There are several Gold Engines for FreePascal and Lazarus, and one of them supports latest Gold EGT v5.0 file format. 

### Lazarus Gold Engine 2018

This is a refactored cross platform Delphi engine used for JSON parsing example. It supports only CGT v1.0 file format so you need to use older Gold Parser Builder with it. You can find it [here](<http://forum.lazarus.freepascal.org/index.php/topic,33830.msg295876.html#msg295876>). 

### Gold Parser for Lazarus

Another adaptation of the same Delphi engine for Lazarus. It's a little more commented then the 2018 version so it might be more appealing to the ones who want to understand the code. It also supports only CGT v1.0 file format so you need to use older Gold Parser Builder. You can find the engine [here](<https://github.com/cgarciagl/goldparser-Lazarus>). 

### Gold Parser for Free Pascal and Lazarus

There are 2 versions of this engine. One derived from the same old Delphi engine compatible only with CGT v1.0, and the new one derived from Java which is compatible with both CGT v1.0 and EGT v5.0. This means that it will load grammars compiled with latest version of Gold Parser Builder (5.2.0 at the moment of writing). This engine was used for Pascal parsing example. You can find it [here](<https://gitlab.com/teo-tsirpanis-718/gold-parser-lazarus>). 

### FPC Gold Engine with VS Code parser plugin and code generator

[FPC Gold Engine](<https://github.com/Warfley/FPCGoldEngine>) supports both CGT v1.0 and EGT v5.0, has very interesting [VS Code extension](<https://github.com/Warfley/gold-parser-tools>) to parse and debug grammars, and to generate Pascal and TypeScript code out of grammars. Some additional documentation is available [here](<https://github.com/Warfley/GoldEngines/blob/master/docs/index.md>). 

## Engines Usage Examples

A lot of grammars found on the net are pretty old and many can not compile out of the box with recent versions of Gold Parser Builder. Therefore you can find here 2 examples which compile fine. These examples are recommended for your initial testing. They are just meant to show you how to parse sample text or code with a grammar, and will not go further then that. 

### JSON example

You should copy/paste simple JSON grammar from below into 'Grammar Window' of Gold Parser Builder 3.4.4, save the grammar as 'json.grm" file, press 'Continue' button in the bottom right 3 times in a row, the press it 4th time and in file browser save compiled grammar as 'json.cgt' file. Now copy/paste JSON test code from below into 'Test Grammar' window of Gold Parser Builder 3.4.4, press 'Start' button, and then press 'Parse All' button. There should be no errors and you should see the populated 'Parse Tree' tab at the bottom of your 'Test Grammar' window. You will find nice parsing steps info in 'Parse Actions' tab, and you can even execute step by step if you use 'Step' button instead of the mentioned 'Parse All' button. 

After testing is finished, we load project GOLDParserEngineSource from _Lazarus Gold Engine 2018_ into Lazarus, open Project Inspector to add 'sparta_generics' package as a new requirement, compile and start the executable, load file 'json.cgt' into 'Test Input' memo field, paste JSON Test Code into memo, and press 'Parse' button to start the parser. If everything went fine then you should see the memo populated with parsed tree represented in textual form. Of course, instead of showing parsed tree you can do whatever you want. You can implement semantic checks, emit code for template based code generator, interpret tree... 

JSON screenshots from below show what you should expect to see. 

#### JSON Grammar
    
    
    "Name"     = JSON Grammar
    "Author"   = Ars ne von Wyss
    "Version"  = 1.0
    "About"    = 'Grammar for JSON data, following http://www.json.org/'
    ! and compliant with http://www.ietf.org/rfc/rfc4627
    
    "Start Symbol" = <Json>
    "Case Sensitive" = True
    "Character Mapping" = 'Unicode'
    
    ! ------------------------------------------------- Sets
    
    ! Uncomment next line only on Gold 5.x.x and newer when running in new EGT v5.0 engine:
    ! {Unescaped} = {All Valid} - {&1 .. &19} - ["\]
    ! Older engines do not handle correctly {All Valid} with Unicode character mapping,
    ! so we will replace {All Valid} with {Printable}. Just have in mind that mentioned
    ! RFC4627 compliance is therefore lost.
    
    {Unescaped} = {Printable} - {&1 .. &19} - ["\] ! compatible with old CGT v1.0 engines
    {Hex} = {Digit} + [ABCDEFabcdef]
    {Digit9} = {Digit} - [0]
    
    ! ------------------------------------------------- Terminals
    
    Number = '-'?('0'|{Digit9}{Digit}*)('.'{Digit}+)?([Ee][+-]?{Digit}+)?
    String = ["]({Unescaped}|'\'(["\/bfnrt]|'u'{Hex}{Hex}{Hex}{Hex}))*["]
    
    ! ------------------------------------------------- Rules
    
    <Json> ::= <Object>
             | <Array>
    
    <Object> ::= '{' '}'
               | '{' <Members> '}'
    
    <Members> ::= <Pair>
                | <Pair> ',' <Members>
    
    <Pair> ::= String ':' <Value>
    
    <Array> ::= '[' ']'
              | '[' <Elements> ']'
    
    <Elements> ::= <Value>
                 | <Value> ',' <Elements>
    
    <Value> ::= String
              | Number
              | <Object>
              | <Array>
              | true
              | false
              | null
    

#### JSON Test Code
    
    
    {"widget": {
        "debug": "on",
        "window": {
            "title": "Sample Konfabulator Widget",
            "name": "main_window",
            "width": 500,
            "height": 500
        },
        "image": { 
            "src": "Images/Sun.png",
            "name": "sun1",
            "hOffset": 250,
            "vOffset": 250,
            "alignment": "center"
        },
        "text": {
            "data": "Click Here",
            "size": 36,
            "style": "bold",
            "name": "text1",
            "hOffset": 250,
            "vOffset": 100,
            "alignment": "center",
            "onMouseUp": "sun1.opacity = (sun1.opacity / 100) * 90;"
        }
    }}
    

#### JSON screenshots

[![JSON example in Gold 3.4.4](https://wiki.freepascal.org/images/e/e0/Gold_3.4.4_Json_Example.png)](</File:Gold_3.4.4_Json_Example.png> "JSON example in Gold 3.4.4") [![JSON example parsed with CGT v1.0 GoldEngine2018](https://wiki.freepascal.org/images/3/3e/GoldEngine2018_CGT_v1.0_parsed_Json_example.png)](</File:GoldEngine2018_CGT_v1.0_parsed_Json_example.png> "JSON example parsed with CGT v1.0 GoldEngine2018")

### Pascal example

You should copy/paste Pascal grammar from below into 'Grammar' window of Gold Parser Builder 5.2.0, save the grammar as 'pascal.grm" file, press 'Next' button in the bottom right 3 times in a row, then press it 4th time and save compiled grammar as 'pascal.egt' file. Now copy/paste Pascal test code from below into 'Test Grammar' window of Gold Parser Builder 5.2.0, and press button with fast forward symbol icon (that's 'Parse All' button). There should be no errors and you should see the populated 'Actions' tab at the right side of your 'Test Grammar' window with all parsing steps info. You can see the whole parse tree in 'Tree' tab, and you can even execute step by step if you use button with play symbol icon. Now from the test menu choose 'Save The Test File As...' and save it as 'pascal.pas'. 

After testing is finished, we load package 'gold_parser_5.lpk' from 'Gold Parser for Free Pascal and Lazarus', compile it, then open 'GoldTreeParser.lpi' project and compile it too. Now from command prompt execute this command: 
    
    
    goldtrcc pascal.egt pascal.pas pascal.txt
    

If everything went fine then you should see parse tree and parsed steps flying fast on your screen. Don't worry. Just open 'pascal.txt' file in your favorite editor and watch the result. Of course, instead of showing parsed tree and steps you can do whatever you want. You can implement semantic checks, emit code for template based code generator, interpret tree... 

#### Pascal Grammar

Pascal grammar is relatively big, so it is hidden and to show it you need to expand it first with the linked button on the right. 
    
    
    ! ---------------------------------------------------------------------------------
    ! Standard Pascal Grammar
    !
    ! modified by Avra (Zeljko Avramovic) for compatibility with latest version of Gold
    ! ---------------------------------------------------------------------------------
    
    "Name"                    =   'Pascal' 
    "Version"                 =   '1973'
    "Author"                  =   'Niklaus Wirth' 
    "About"                   =   'PASCAL was developed by NIKLAUS WIRTH of the ETH Technical Institute of Zuerich in 1970-1971.(published in 1973)'
    
    "Case Sensitive"          =   False
    "Start Symbol"            =   <Program>
                     
    Comment Start             =   '{'
    Comment End               =   '}'
    Comment Line              =   '//'
    
    {Hex Digit}               =   {Digit} + [ABCDEF]
    
    {Id Head}                 =   {Letter} + [_]
    {Id Tail}                 =   {Id Head} + {Digit}
    
    {String Ch}               =   {Printable} - ['']
    {Char Ch}                 =   {Printable} - ['']
    
    DecLiteral                =   [123456789]{digit}*
    HexLiteral                =   '$'{Hex Digit}+
    FloatLiteral              =   {Digit}*'.'{Digit}+
    
    StringLiteral             =   ''( {String Ch} | '\'{Printable} )* ''
    CharLiteral               =   '' ( {Char Ch} | '\'{Printable} )''
    
    id                        =   {Id Head}{Id Tail}*
    
    <constant>                ::= DecLiteral
                              |   StringLiteral
                              |   FloatLiteral
                              |   HexLiteral
                              |   CharLiteral
    
    
    !=========================================== Program
    
    
    <Program>                 ::= <ProgramHeader> <Declarations> <CompoundStatement> '.'
    
    <ProgramHeader>           ::= PROGRAM id ';'
                              |   PROGRAM id '(' <IdList> ')' ';'
    
    <Declarations>            ::= <ConstantDefinitions> <TypeDefinitions> <VariableDeclarations> <ProcedureDeclarations>
    
    <ConstantDefinitions>     ::= CONST <ConstantDefinitionList>
                              |  
    
    <ConstantDefinitionList>  ::= <ConstantDef>
                              |   <ConstantDef> <ConstantDefinitionList>
    
    <ConstantDef>             ::= id '=' <constant> ';'
    
    <TypeDefinitions>         ::= TYPE <TypeDefinitionList>
                              |
    
    <TypeDefinitionList>      ::= <TypeDef>
                              |   <TypeDef> <TypeDefinitionList>
    
    <TypeDef>                 ::= id '=' <TypeSpecifier> ';'
    
    <VariableDeclarations>    ::= VAR <VariableDeclarationList>
                              | 
    
    <VariableDeclarationList> ::= <VariableDec>
                              |   <VariableDec> <VariableDeclarationList>
    
    <VariableDec>             ::= <IdList> ':' <TypeSpecifier> ';'
    
    <ProcedureDeclarations>   ::= <ProcedureDec> <ProcedureDeclarations>
                              | 
    
    <ProcedureDec>            ::= <ProcedureHeader> FORWARD ';'
                              | <ProcedureHeader> <Declarations> <CompoundStatement> ';'
                              | <FunctionHeader> FORWARD ';'
                              | <FunctionHeader> <Declarations> <CompoundStatement> ';'
    
    <ProcedureHeader>         ::= PROCEDURE id <Arguments> ';'
    
    <FunctionHeader>          ::= FUNCTION id <Arguments> ':' <TypeSpecifier> ';'
    
    <Arguments>               ::= '(' <ArgumentList> ')'
                              | 
    
    <ArgumentList>            ::= <Arg>
                              |   <Arg> ';' <ArgumentList>
    
    <Arg>                     ::= <IdList> ':' <TypeSpecifier>
                              |   VAR <IdList> ':' <TypeSpecifier>
    
    <CompoundStatement>       ::= BEGIN <StatementList> END
    
    <StatementList>           ::= <Statement>
                              |   <Statement> ';' <StatementList>
    
    <Statement>               ::= <CompoundStatement>
                              |   <AssignmentStatement>
                              |   <ProcedureCall>
                              |   <ForStatement>
                              |   <WhileStatement>
                              |   <IfStatement>
                              |   <CaseStatement>
                              |   <RepeatStatement>
                              | 
    
    <AssignmentStatement>     ::= <Variable> ':=' <Expression>
    !                         |   <Variable> ':=' <FunctionCall>
    
    <ProcedureCall>           ::= id <Actuals>
    
    <ForStatement>            ::= FOR id ':=' <Expression> TO <Expression> DO <Statement>
                              |   FOR id ':=' <Expression> DOWNTO <Expression> DO <Statement>
    
    <WhileStatement>          ::= WHILE <Expression> DO <Statement>
    
    <IfStatement>             ::= IF <Expression> THEN <Statement> ELSE <Statement>
    
    <RepeatStatement>         ::= REPEAT <StatementList> UNTIL <Expression>
    
    <CaseStatement>           ::= CASE <Expression> OF <CaseList> END
    
    <CaseList>                ::= <Case>
                              |   <Case> ';' <CaseList>
    
    <Case>                    ::= <ConstantList> ':' <Statement>
    
    <ConstantList>            ::= <constant>
                              |   <constant> ',' <ConstantList>
    
    <Expression>              ::= <SimpleExpression>
                              |   <SimpleExpression> '=' <SimpleExpression>
                              |   <SimpleExpression> '<>' <SimpleExpression>
                              |   <SimpleExpression> '<' <SimpleExpression>
                              |   <SimpleExpression> '<=' <SimpleExpression>
                              |   <SimpleExpression> '>' <SimpleExpression>
                              |   <SimpleExpression> '>=' <SimpleExpression>
                              |   <FunctionCall>
    
    <SimpleExpression>        ::= <Term>
                              |   <SimpleExpression> '+' <Term>
                              |   <SimpleExpression> '-' <Term>
                              |   <SimpleExpression> OR <Term>
    
    <Term>                    ::= <Factor>
                              |   <Term> '*' <Factor>
                              |   <Term> '/' <Factor>
                              |   <Term> 'DIV' <Factor>
                              |   <Term> 'MOD' <Factor>
                              |   <Term> 'AND' <Factor>
    
    <Factor>                  ::= '(' <Expression> ')'
                              |   '+' <Factor>
                              |   '-' <Factor>
                              |   NOT <Factor>
                              |   <constant>
                              |   <Variable>
    
    <FunctionCall>            ::= id <Actuals>
    
    <Actuals>                 ::= '(' <ExpressionList> ')' 
    
    <ExpressionList>          ::= <Expression>
                              |   <Expression> ',' <ExpressionList>
                              |
    
    <Variable>                ::= id
                              |   <Variable> '.' id
                              |   <Variable> '^'
                              |   <Variable> '[' <ExpressionList> ']'
    
    <TypeSpecifier>           ::= id
                              |   '^' <TypeSpecifier>
                              |   '(' <IdList> ')'
                              |   <constant> '..' <constant>
                              |   ARRAY '[' <DimensionList> ']' OF <TypeSpecifier>
                              |   RECORD <FieldList> END
                              |   FILE OF <TypeSpecifier>
    
    <DimensionList>           ::= <Dimension>
                              |   <Dimension> ',' <DimensionList>
    
    <Dimension>               ::= <constant> '..' <constant>
                              |   id
    
    <FieldList>               ::= <Field>
                              |   <Field> ';' <FieldList>
    
    <Field>                   ::= <IdList> ':' <TypeSpecifier>
    
    <IdList>                  ::= id
                              |   id ',' <IdList>
    

#### Pascal Test Code
    
    
    program test; // simple pascal code for testing parser
    
    const
      PI = 3.1415;
    
    var
      a, b, c: real;
    
    procedure hello(s: string; b, c: real);
    begin
      writeln(s);
      writeln(b);
      writeln(c);
    end;
    
    function square(a: real): real;
    begin
      result := a * a;
    end;
    
    function half(a: real): real;
    begin
      result := a / 2;
    end;
    
    function twopi: real;
    begin
      result := pi * 2;
    end;
    
    begin
      a := PI;
      b := a * 10;
      c := square(half(twopi()));
      hello('Hello World!', b, c);
    end.
    

#### Pascal screenshots

[![Pascal example in Gold 5.2.0](https://wiki.freepascal.org/images/4/49/Gold_5.2.0_Pascal_Example.png)](</File:Gold_5.2.0_Pascal_Example.png> "Pascal example in Gold 5.2.0") [![Pascal example parsed with EGT v5.0 Gold Parser for Free Pascal and Lazarus](https://wiki.freepascal.org/images/9/90/Gold_Parser_for_Free_Pascal_and_Lazarus_EGT_v5.0_parsed_Pascal_example.png)](</File:Gold_Parser_for_Free_Pascal_and_Lazarus_EGT_v5.0_parsed_Pascal_example.png> "Pascal example parsed with EGT v5.0 Gold Parser for Free Pascal and Lazarus")

# Goldie Tools

Although [Goldie](<https://www.semitwist.com/goldie/>) is not originaly made for use with Pascal, it has many [command line tools](<https://www.semitwist.com/goldie/Tools/>) which can be useful to manipulate CGT files (Gold compiled grammar). You can show CGT file in human readable format, compile grammar to CGT v1.0 file without Gold Parser Builder executable, and parse source according to some CGT file to save the resulting tokens and parsed tree to JSON file. 

# Useful links

  * [Make your own compiler, interpreter, parser, or expression analyzer](<Make_your_own_compiler,_interpreter,_parser,_or_expression_analyzer.md> "Make your own compiler, interpreter, parser, or expression analyzer")
  * [Gold Parser Builder home page](<http://goldparser.org>)
  * [Gold Parser Builder 3.4.4 setup for older engines not compatible with latest Gold 5.x CGT files](<http://www.semitwist.com/download/gold-parser-builder-3-4-4.zip>)
  * [Gold Parser Builder 5.2.0 setup](<http://goldparser.org/builder/files/GOLD-Builder-5.2.0-Setup.zip>)
  * [Introduction to Gold Parser](<https://www.codeproject.com/articles/10492/introduction-to-gold-parser>)
  * [Let Your Parser Go for the GOLD](<https://www.developer.com/net/csharp/article.php/3689776/Let-Your-Parser-Go-for-the-iGOLDi.htm>)

---

_Source: [https://wiki.freepascal.org/Gold](https://web.archive.org/web/20240616071137/https://wiki.freepascal.org/Gold)_
