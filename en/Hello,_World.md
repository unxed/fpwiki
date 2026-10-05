# Hello, World

│ **English (en)** │  **[русский (ru)](<../ru/Hello,_World.md>)** │

**Hello, World** refers to a trivial [program](<Program.md> "Program") printing `Hello, World!` to some standard means of output. It is used to illustrate some basic characteristics of a programming language. This page elaborates a _Hello, World_ in [Pascal](<Pascal.md> "Pascal"). 

## Contents

  * 1 standard source code
  * 2 line-by-line description
    * 2.1 header
    * 2.2 definition
  * 3 degenerate examples
    * 3.1 whitespace
    * 3.2 case-insensitive
  * 4 see also



## standard source code

The following [source code](<Source_code.md> "Source code") is a minimal, yet complete _Hello, World_ fully-compliant to [Standard Pascal](<Standard_Pascal.md> "Standard Pascal") (ISO standard 7185): 
    
    
    program helloWorld(output);
    begin
    	writeLn('Hello, World!')
    end.
    

Compilation with the [FPC](<FPC.md> "FPC") is as simple as that: 
    
    
    $ fpc helloWorld.pas
    Target OS: Linux for x86-64
    Compiling helloWorld.pas
    Linking helloWorld
    4 lines compiled, 0.1 sec
    

## line-by-line description

### header

Every Pascal source code file starts off with a word identifying the kind of source code. Here, this kind is `program`. In [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") (ISO standard 10206) `module` is possible, too. As of 2022, the [FPC](<FPC.md> "FPC") supports, beside `program`, only two other kinds: [`unit`](<Unit.md> "Unit") and `library`. 

The next word provides an [identifier](<Identifier.md> "Identifier") for this `program` (or `module`). As per ISO standards, this identifier has no significance. You _could_ reuse the identifier `helloWorld` in the following lines. The FPC, however, reserves this identifier for use in fully-qualified identifiers. 

After that comes a parameter list. Here this parameter list enumerates one item, [`output`](<Output.md> "Output"). Parameters of the spelling `input` and `output` have special meaning. They refer to an implementation-defined standard means of accessing the user interface, that means usually the [console](</index.php?title=Console&action=edit&redlink=1> "Console \(page does not exist\)"). 

The header is _separated_ from the following [block](<Block.md> "Block") by a [semicolon](<Semicolon.md> "Semicolon"). 

### definition

Following the header comes the `program` definition. A `program` is defined with a block. Every block has to have exactly one `begin … end` [frame](<Frame.md> "Frame"). The FPC also accepts `asm … end` ([assembly language](<Assembly_language.md> "Assembly language")) frames for [routines](<Routine.md> "Routine"). 

In our program this frame contains one [statement](<statement.md> "statement"): A call of the built-in [procedure](<Procedure.md> "Procedure") [`writeLn`](<Write.md> "Write"), short for _write line_. Thereafter follows a non-empty comma-separated list of actual parameters. Here we have one string literal `'Hello, world!'`. String literals are delimited by [typewriter straight quotes](<'.md> "'"). 

The `program` definition concludes with a [period](<period.md> "period"). 

## degenerate examples

These examples are meant to demonstrate a point. They do not appear in production programs. 

### whitespace

White space (blanks or newlines) has no significance to the meaning to the program, as long as it does not interfere with words or literal values (i. e. number or string constants): 
    
    
     program  helloWorld (output) ;
            begin
      writeLn   ( 'Hello, world!' )
      
                              end .
    

### case-insensitive

Pascal is case-insensitive. This program has _exactly_ the same meaning and effect as the standard source code: 
    
    
    PrOgRaM HeLLoWorLd(oUtpUt);
    begIn
    	wRiTelN('Hello, world!')
    EnD.
    

## see also

  * [Using resourcestrings](<Using_resourcestrings.md> "Using resourcestrings") for an internationalized (= translated/translatable) _Hello, World_
  * [Hello, World](<Basic_Pascal_Tutorial/Hello,_World.md> "Basic Pascal Tutorial/Hello, World") in the [Basic Pascal Tutorial](<Basic_Pascal_Tutorial.md> "Basic Pascal Tutorial")
  * [Hello, world](<https://RosettaCode.org/wiki/Hello_world/Text#Pascal>) in the _Rosetta code_ project
  * [Programming in Symbian OS](<Programming_in_Symbian_OS.md> "Programming in Symbian OS") for _Hello, World_ using the Symbian OS API
  * [Howdy World](<Howdy_World_\(Hello_World_on_steroids\).md> "Howdy World \(Hello World on steroids\)"): _Hello, World_ -inspired tutorial for using a graphical user interface and the [Lazarus](<Lazarus.md> "Lazarus") integrated development environment ([IDE](<IDE.md> "IDE"))
  * [Video](<Free_Pascal_videos.md> "Free Pascal videos"): [Hello, World](<https://www.youtube.com/watch?v=dVkoeoIpGlE>) in Pascal on _YouTube_

---

_Source: [https://wiki.freepascal.org/Hello%2C_World](https://web.archive.org/web/20240920204158/https://wiki.freepascal.org/Hello%2C_World)_
