# Coding style

│ **English (en)** │  **[français (fr)](</Coding_style/fr> "Coding style/fr")** │  **[русский (ru)](<../ru/Coding_style.md> "Coding style/ru")** │    
****

## Contents

  * 1 FPC compiler and RTL
    * 1.1 Introduction
    * 1.2 Keywords
    * 1.3 Spaces
    * 1.4 TAB characters
    * 1.5 Indentation
    * 1.6 Newlines
    * 1.7 Comments
    * 1.8 Routines
    * 1.9 Classes/Records
    * 1.10 Misc
    * 1.11 Examples
  * 2 FCL
  * 3 Other packages distributed with FPC
  * 4 Lazarus
  * 5 See also



## FPC compiler and RTL

### Introduction

Some people might think that the coding style used in the FPC compiler and base RTL (run-time library) source code is a little bit strange. But it has been in use for a lot of years and is not subject to discussion. So take the following as a standard to be used. 

### Keywords

Write all keywords in all lower _snake_case_. There is no need to make them unreadble by writing them in upper case. Modern IDEs support syntax highlighting, so keywords will be easily recognizable. 

### Spaces

Don't use spaces around operators, colons, parentheses etc. 
    
    
    { correct } 
    p:=p+i;
    
    { incorrect }
    p := p + i ;
    

Spaces in parameter lists. TODO: ??? 
    
    
    function do_this(a: tdef; var b: tdef): boolean;
    

Spaces in variable lists. TODO: ??? 
    
    
    var
      a,b,c : tprocdef;
    

### TAB characters

Do not use TAB characters ([ASCII](<ASCII.md> "ASCII") HT, 0x09). There is no standard default TAB setting, so the look of source files using TAB characters will depend on client settings. This may result in a chaotic view of source files. Align by space characters (also see _Indentation_). 

### Indentation

Indentation size is always 2 space characters per level. 

Never place a _begin_ on the same line as while..do/if..then/..., but always on its own line (and indent it compared to the while/if/...). 
    
    
    { correct }
    if true then
      begin
      end;
    
    { incorrect }
    if true then begin
      end;
    
    { incorrect }
    if true then begin {...} end;
    

### Newlines

Newlines are set as it is done by most Object Pascal programs (what does this mean? advice: avoid passive voice). Separate subroutines by three newlines, that is, put _two_ blank lines between them. 

### Comments

Comments should use curly brackets, be lower case and contain a single space between the {}. 
    
    
    { this is an example of a comment }
    

### Routines

All sections (var, const, begin etc...) after routine header are indented one level. 
    
    
    procedure do_this;
      var
        sym: tsym;
      begin
      end;
    

No spaces after name and parenthesis for parameters. 
    
    
    procedure do_this(param: integer);
      begin
      end;
    

Nested functions are indented one level (the same level as other sections like var, const etc...) 
    
    
    procedure do_this;
      var
        i: integer;
    
      { nested function }
      function getter: boolean;
        begin
        end;
    
      var
        x: integer;
      begin
      end;
    

Use result instead of function name for return values. 
    
    
    function getter: boolean;
      begin
        result:=true;
        // wrong!
        //getter:=true;
      end;
    

### Classes/Records

Class sections should appear on the same indention level as the class name. 
    
    
    tloadnode = class(tunarynode)
    protected
      fprocdef : tprocdef;
    public
      constructor create(v : tsym;st : TSymtable);virtual;
    end;
    

### Misc

Please note that the `else` in consecutive `if`s is not indented: 
    
    
     if x then
       begin
         ..
       end
     else if y then
       begin
         ...
       end;
    

Split all composite if-conditions over multiple lines, so no "if (x) and (y) then" but 
    
    
    if p1.nodetype=ordconstn and
      not is_boolean(p1.resultdef) and
      not is_enum(p1.resultdef) then
      begin
        ...
    

(except possibly if x and y are simply boolean variables) 

Extraneous parenthesis in if statements: TODO: ??? 
    
    
    if (pd.maxparacount>0) then
      ;
    

### Examples

How it looks like can be easily checked by having a look at the [FPC sources](<http://www.freepascal.org/cgi-bin/viewcvs.cgi/?root=fpc>). 

## FCL

The FCL follows the same rules as above, with 1 exception: routines are not indented, and there are empty lines between the ``var`` section and surrounding code. 

So do not write 
    
    
    procedure do_this;
      var
        sym: tsym;
      begin
      end;
    

But write 
    
    
    procedure do_this;
    
    var
      sym: tsym;
    
    begin
    end;
    

## Other packages distributed with FPC

There are no formal standards. **To do: write me.**

## Lazarus

Since Lazarus and LCL follow Delphi compatibility, a code style similar to the one used in Delphi is used. 

If you're writing a patch or an extension for LCL you should follow its code style. 

If you're developing your own component, you're free to use any style you like, but it's suggested to use the LCL style, too. 

See the Lazarus coding guidelines: [DesignGuidelines](<DesignGuidelines.md> "DesignGuidelines")

For reference, you can find the description of Delphi coding style [here](<http://edn.embarcadero.com/article/10280#8.2.3>)

## See also

  * [Coding Guidelines for Lazarus](<DesignGuidelines.md> "DesignGuidelines")

---

_Source: [https://wiki.freepascal.org/Coding_style](https://web.archive.org/web/20250424003740/https://wiki.freepascal.org/Coding_style)_
