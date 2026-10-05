# Make your own compiler, interpreter, parser, or expression analyzer

## Contents

  * 1 FCL Passrc
  * 2 HTML and XML
  * 3 Math expressions
  * 4 Lex and Yacc
    * 4.1 Plex and Pyacc
    * 4.2 Lazarus Lex and Yacc
  * 5 Gold
  * 6 SynFacilSyn
  * 7 ANTLR
  * 8 Coco-R
  * 9 Anatomy of a compiler
  * 10 Ancient history
  * 11 Useful BNF and EBNF tools
  * 12 See also



## FCL Passrc

FPC comes with a Pascal parser in library form in the [fcl-passrc](<fcl-passrc.md> "fcl-passrc") package. This is not the main compiler parser, but it is the one used for [fpdoc](<fpdoc.md> "fpdoc") and [pas2js](<pas2js.md> "pas2js"). 

## HTML and XML

[fcl-xml](<fcl-xml.md> "fcl-xml") is a FPC package that contains SAX XML and HTML parsers. 

## Math expressions

FPC contains two expression parsers: 

  * [symbolic](</index.php?title=symbolic&action=edit&redlink=1> "symbolic \(page does not exist\)")
  * [TFPExpressionParser](<How_To_Use_TFPExpressionParser.md> "How To Use TFPExpressionParser")



## Lex and Yacc

Two of the oldest unix tools. [Lex](<https://en.wikipedia.org/wiki/Lex_\(software\)>) is a lexical analyser (token parser), and [Yacc](<https://en.wikipedia.org/wiki/Yacc>) is a [LALR](<https://en.wikipedia.org/wiki/LALR_parser>) parser generator. [BNF](<https://en.wikipedia.org/wiki/Backus–Naur_form>) notation is used as a formal way to express context free grammars. Code and grammar are mixed, so grammar is tied to implementation language. 

### Plex and Pyacc

[Plex and Pyacc](<Plex_and_Pyacc.md> "Plex and Pyacc") are Pascal implementations of Lex and Yacc. They are part of Free Pascal distribution. 

### Lazarus Lex and Yacc

You can find unfortunately abandoned Lazarus Lex and Yacc [here](<https://bitbucket.org/avra/lazlexyacc/src/master/>). 

## Gold

[Gold](<Gold.md> "Gold") is a free parsing system that you can use to develop your own programming languages, scripting languages and interpreters. It uses [LALR](<https://en.wikipedia.org/wiki/LALR_parser>) parsing, and a mix of [BNF](<https://en.wikipedia.org/wiki/Backus–Naur_form>) notation, character sets and regular expressions for terminals to define language grammars. Code and grammar are separated, so grammar is not tied to implementation language. This means that the same grammar can be loaded into engines made in different programming languages. 

Gold Parser Builder can be used to create, modify and test languages in Windows IDE which can also run on Wine. Command line tools are also available. 

Gold Parser Builder can also be used as a parser code generator using internal templates (FreePascal included), but there are also 3rd party engines to process compiled grammars. 

Gold Parser Builder has grammar editor with syntax highlighting, grammar generating wizard, test window to step through parsing of a sample source, templating system that can generate lexers/parsers or skeleton programs for various languages (including Delphi and FreePascal), import/export YACC/Bison, XML and HTML export, and interactive inspection of the compiled DFA and LALR tables. 

There is a subjective [feature comparison table](<http://goldparser.org/about/comparison-parsers.htm>) of several parsers on Gold site, with special attention to [Gold vs Yacc](<http://goldparser.org/about/comparison-yacc.htm>) comparison. 

## SynFacilSyn

[SynFacilSyn](<SynFacilSyn.md> "SynFacilSyn") is a Lazarus library that includes a SynEdit highlighter that also can work as a lexer because of its flexible syntax definition file. It's well documented and has been used in several projects as both a highlighter and a lexer. 

## ANTLR

ANTLR (ANother Tool for Language Recognition) is a powerful parser generator for reading, processing, executing, or translating structured text or binary files. It's widely used to build languages, tools, and frameworks. From a grammar, ANTLR generates a parser that can build and walk parse trees. [Home page](<https://www.antlr.org/>). 

## Coco-R

Coco/R is a compiler generator based on L- attributed grammars which generates a scanner and a parser. [Home page](<http://www.ssw.uni-linz.ac.at/Coco/>). 

Two chapters of this book give an introduction about Coco/R and show some sample studies. 

  * Compilers and Compiler Generators - an introduction with C++
  * P.D. Terry, [Rhodes University](<https://www.ru.ac.za/computerscience/>), 1996



## Anatomy of a compiler

Here is graphical representation of a typical compiler anatomy: 

[![Anatomy of a compiler](https://wiki.freepascal.org/images/c/c6/Anatomy_of_a_compiler.gif)](</File:Anatomy_of_a_compiler.gif> "Anatomy of a compiler")

The parse tree is typically stored in RAM, where an optimiser can recognise and simplify idioms e.g. to unroll loops. Some early compilers attempted to store the entire program's parse tree which was only converted to lower-level code on completion of the pass that generated it; one notable example was the Pastel ("an off-colour Pascal") compiler[[1]](<https://en.wikipedia.org/wiki/Pastel_\(programming_language\)>) running on a DEC PDP-11 which famously did not become the basis of Stallman's GCC compiler. According to the Pastel developers:[[2]](<http://www.mit.edu/~cbf/thesis.htm#Languages%20and%20Hosts>)

> At one point in the project Richard Stallman visited, and had the Pastel compiler explained to him. He left with a copy of the source, and used it to produce the Gnu C compiler. Most of the techniques that gave the Gnu C compiler its reputation for good code generation came from the Amber Pastel compiler. 

According to Stallman:[[3]](<https://www.gnu.org/gnu/thegnuproject.html>)

> Hoping to avoid the need to write the whole compiler myself, I obtained the source code for the Pastel compiler, which was a multiplatform compiler developed at Lawrence Livermore Lab. It supported, and was written in, an extended version of Pascal, designed to be a system-programming language. I added a C front end, and began porting it to the Motorola 68000 computer. But I had to give that up when I discovered that the compiler needed many megabytes of stack space, and the available 68000 Unix system would only allow 64k. 
> 
> I then realized that the Pastel compiler functioned by parsing the entire input file into a syntax tree, converting the whole syntax tree into a chain of “instructions”, and then generating the whole output file, without ever freeing any storage. At this point, I concluded I would have to write a new compiler from scratch. That new compiler is now known as GCC; none of the Pastel compiler is used in it, but I managed to adapt and use the C front end that I had written. But that was some years later; first, I worked on GNU Emacs. 

As a general principle, the code generator takes (fragments of) the parse tree and generates either binary object files or assembler source. At this stage there is typically further ("peephole") optimisation, in particular to recognise e.g. writes to variables that are never read. 

## Ancient history

A wiki entry hosted by a Pascal community obviously has to start off with Niklaus Wirth, who was supervised by Harry Huskey at UC Berkeley; Wirth's doctoral work implemented a language named Euler on an IBM 704. After Berkeley Wirth moved to Stanford where he reimplemented Euler in ALGOL-60 on either a Burroughs B5000 or B5500 (drum or disc-based respectively, the system was upgraded at about the same time), then he moved on to PL/360 and ALGOL-W which he proposed as a successor to ALGOL-60.[[4]](<https://inf.ethz.ch/personal/wirth/projects.html>) Broadly speaking, Wirth's early compilers used recursive ascent, later editions of his books introduced recursive descent as an alternative. 

There is a wealth of information on compiler implementation in that era on Huub de Beer's website[[5]](<https://heerdebeer.org/ALGOL/implementation.html>) which is the source of some of the conjecture that follows. In addition Thomas Haigh has some very useful material on the history of ALGOL standardisation, in particular throwing light on the crisis caused by the ALGOL-68 committee's rejection of Wirth's ALGOL-W proposal:[[6]](<http://www.tomandmaria.com/Tom/Writing/DijkstrasCrisis_LeidenDRAFT.pdf>)

> According to Hoare’s famous account, given years later in his Turing Award lecture, the group took a decisive wrong turn in 1965 when it inexplicably rejected an elegant and pragmatic draft design compiled by Niklaus Wirth in favor of an “incomplete and incomprehensible” alternative design for a radically different language submitted by Adriaan van Wijngaarden. 

Huub de Beer investigates the earliest published algorithms for compiling ALGOL-like languages, and in effect demonstrates that they were in the style of Dijkstra's "shunting yard" algorithm using multiple queues and stacks to handle the entire syntax. Edgar ("Ned") irons at Princeton used this approach to compile using recursive ascent (with backtracking where necessary), A.A.Grau effectively implemented recursive descent but still used multiple explicit data structures. 

At Burroughs, Richard Waychoff had the idea of using the run-time structure of ALGOL to hide much of the complexity of a recursive descent compiler, he was apparently in informal contact with Irons but did not know of Grau's work and the idea of using the implementation language's capability of recursion (i.e. with nested local variables etc.) appears to be his alone.[[7]](<https://web.archive.org/web/20160401000000/http://ianjoyner.name/files/waychoff.pdf>) It's worth noting at this point that both Donald Knuth and Edsger Dijkstra cooperated to varying degrees with the Burroughs language and operating system developers, Knuth's copy of the document just cited is annotated "half-true"[[8]](<https://archive.computerhistory.org/resources/text/Knuth_Don_X4100/PDF_index/k-8-pdf/k-8-u2779-B5000-People.pdf>) but his oral history account does not dispute Waychoff's claim.[[9]](<https://conservancy.umn.edu/bitstream/handle/11299/107413/oh332dk.pdf>)

Separately, Val Schorre at UCLA wrote Meta-II for an IBM 1401, which both allows the syntax of a language to be defined in a BNF-like fashion and associates output fragments with each recognised syntactic structure.[[10]](<https://en.wikipedia.org/wiki/META_II>) This allows a fairly comprehensive ALGOL-like language to be specified in a few hundred lines, and while the output- which is typically assembler-like source for a dedicated interpreter- lacks any attempt at optimisation it can be a useful tool when implementing e.g. an application-specific scripting language. As observed by Alan Kay: 

> The paper itself is a wonderful gem which includes a number of excellent examples, including the bootstrapping of Meta II in itself (all this was done on an 8K (six bit byte) 1401!) 

Associates of Schorre went on to design Tree Meta, which extended Meta-II by first generating a parse tree in memory and then using the tree equivalent to a source statement to generate output code[[11]](<https://en.wikipedia.org/wiki/TREE-META>). The advantage of this approach is that while the initial _parse_ stage might be unable to distinguish between e.g. loading a constant and loading the result of an expression into a register, the trees that are generated by the two cases are recognisably different and can be optimised appropriately before being _unparsed_ into assembler. Tree Meta was used by Douglas Englebart's team to write specialist compilers for "the mother of all demos", which is widely cited as a landmark in the design of interactive systems. 

Recursive descent is now generally accepted as one of the dominant techniques of compiler implementation. Chronologically, it appears to have been invented by Grau, but the major implementations that made it usable were by Waychoff and Schorre. 

## Useful BNF and EBNF tools

  * [BNF and EBNF metasyntax formal notations for writing grammars](<http://www.garshol.priv.no/download/text/bnf.html>)
  * [EBNF to Syntax Diagram online convertor](<http://bottlecaps.de/rr/ui>)
  * [EBNF Visualizer](<https://sourceforge.net/projects/ebnfvisualizer/>)
  * [Syngen - tool for generating syntax diagrams from BNF in LaTeX format](<https://www.ctan.org/tex-archive/support/syngen>)



## See also

  * [Compiler construction](<https://www.inf.ethz.ch/personal/wirth/CompilerConstruction/>), by Niklaus Wirth.
  * _Let's build a compiler_ , by Jack Crenshaw; [original site](<https://compilers.iecc.com/crenshaw/>), [PDF version](<http://web.archive.org/web/20180107011717/http://www.stack.nl:80/~marcov/compiler.pdf>), or [updated version](<http://www.pp4s.co.uk/main/tu-trans-comp-jc-intro.html>).
  * [Compiler Basics](<http://www.cs.man.ac.uk/~pjj/farrell/compmain.html>) (August 1995), by James Alan Farrell
  * [Writing a C Compiler, Part 1](<https://norasandler.com/2017/11/29/Write-a-Compiler.html>) (Nov 29, 2017), by Nora Sandler
  * [Writing an interpreter](<http://memphis.compilertools.net/interpreter.html>)
  * [Notations for context-free grammars](<http://www.cs.man.ac.uk/~pjj/bnf/bnf.html>)
  * [Parsing Techniques - A Practical Guide](<https://dickgrune.com/Books/PTAPG_1st_Edition/>) (1st ed., 1990), by Dick Grune and Ceriel J.H. Jacobs
  * [FPC internals](<FPC_internals.md> "FPC internals") \- Details about how FreePascal compiler is built
  * [The School of Wirth](<http://pascal.hansotten.com/>) Hans Otten's website discussing Pascal and related languages

---

_Source: [https://wiki.freepascal.org/Make_your_own_compiler%2C_interpreter%2C_parser%2C_or_expression_analyzer](https://web.archive.org/web/20230326131121/https://wiki.freepascal.org/Make_your_own_compiler%2C_interpreter%2C_parser%2C_or_expression_analyzer)_
