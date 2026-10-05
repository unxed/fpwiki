# SynFacilSyn

## Contents

  * 1 About
  * 2 Download
  * 3 Features
  * 4 Related tools



## About

SynFacilSyn is a Lazarus cross-platform library that includes a [SynEdit](<SynEdit.md> "SynEdit") highlighter that also can work as a lexer because of its flexible syntax definition file. It's well documented and has been used in several projects as both a highlighter and a lexer. 

SynFacilSyn also includes the "adapter" for [ATSynEdit](<ATSynEdit.md> "ATSynEdit"). 

Author: Tito Hinostroza 

License: GPL 2 

## Download

GitHub repository: <https://github.com/t-edson/SynFacilSyn>

## Features

The main advantage of using SynFacilSyn as a lexer is that in a SynEdit editor you can actually see the tokens colored appropriately, without doing anything else. So if you define a lexer, you are defining a source highlighter too. 

You can use an XML file to define the syntax, defining the tokens using tags and various RegEx constructs. You can also specify colouring of the tokens if you need it. 

You can also define Syntax programmatically which means it's possible to change individual syntax elements at runtime. 

SynFacilSyn's highlighter is fast and lightweight. It also includes useful methods for accessing the text content after the scan. Since it allows you to define recursive blocks and sections you can also use it as a parser. 

## Related tools

SynFacilSyn is the base library for other tools that can be used together to create a powerful compiler, interpreter or IDE: 

  * [SynFacilCompletion](<https://github.com/t-edson/SynFacilCompletion>) \- Scriptable Highlighter with code-completion for the SynEdit Component of Lazarus
  * [SynFacilUtils](<https://github.com/t-edson/SynFacilUtils>) Library with utilities to create editors using SynFacilSyn and SynFacilCompletion.
  * [t-Xpres](<https://github.com/t-edson/t-Xpres>) Framework to create compilers/interpreters based on SynFacilSyn. Includes lexer, parser, syntaxTree, expression evaluator and support to implement code generators or virtual machines.



The following programs have been created using the above tools: 

  * [Microchip PIC compiler](<https://github.com/t-edson/PicPas>)
  * [6502 CPU compiler](<https://github.com/t-edson/P65Pas>)
  * [Terminal with interpreter](<https://github.com/t-edson/Tito-s-Terminal>)

---

_Source: [https://wiki.freepascal.org/SynFacilSyn](https://web.archive.org/web/20250417145519/https://wiki.freepascal.org/SynFacilSyn)_
