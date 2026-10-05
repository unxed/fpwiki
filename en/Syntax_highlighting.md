# Syntax highlighting

│ **English (en)** │

Syntax highlighting is a feature which displays text in different colors and fonts according to the category of terms. It is the feature of text editors that are used for programming ([source code](<Source_code.md> "Source code")), scripting, markup languages ([HTML](</index.php?title=HTML&action=edit&redlink=1> "HTML \(page does not exist\)"), [JSON](<JSON.md> "JSON"), or [XML](<XML.md> "XML")), or configuration files (LFM, or Ini-files). 

## Example
    
    
    program Project1;
    
    const
      text = 'Press any key to continue!';
      { Computer programmers historically used 'Press any key to continue' as
        a prompt to the user when it was necessary to pause processing.
        The system would resume after the user pressed any keyboard button. }
    begin
      WriteLn(text);
      ReadLn;
    end.
    

In the [Pascal](<Pascal.md> "Pascal") example, the editor has recognized the keywords [program](<Program.md> "Program"), [const](<Const.md> "Const"), [begin](<Begin.md> "Begin"), and [end](<End.md> "End"). The [comment](<Comments.md> "Comments") is also highlighted in a specific manner to distinguish it from working code. 

## Components

Icon | Component | Description   
---|---|---  
[![tsynpassyn.png](https://wiki.freepascal.org/images/f/f9/tsynpassyn.png)](</File:tsynpassyn.png>) | [TSynPasSyn](<TSynPasSyn.md> "TSynPasSyn") | Generic Pascal syntax-highlighting   
[![tsynfreepascalsyn.png](https://wiki.freepascal.org/images/a/a2/tsynfreepascalsyn.png)](</File:tsynfreepascalsyn.png>) | [TSynFreePascalSyn](<TSynFreePascalSyn.md> "TSynFreePascalSyn") | Free-Pascal syntax-highlighting   
[![tsyncppsyn.png](https://wiki.freepascal.org/images/4/4d/tsyncppsyn.png)](</File:tsyncppsyn.png>) | [TSynCppSyn](<TSynCppSyn.md> "TSynCppSyn") | C++ syntax-highlighting   
[![tsynjavasyn.png](https://wiki.freepascal.org/images/1/14/tsynjavasyn.png)](</File:tsynjavasyn.png>) | [TSynJavaSyn](<TSynJavaSyn.md> "TSynJavaSyn") | Java syntax-highlighting   
[![tsynperlsyn.png](https://wiki.freepascal.org/images/6/63/tsynperlsyn.png)](</File:tsynperlsyn.png>) | [TSynPerlSyn](<TSynPerlSyn.md> "TSynPerlSyn") | perl syntax-highlighting   
[![tsynhtmlsyn.png](https://wiki.freepascal.org/images/6/69/tsynhtmlsyn.png)](</File:tsynhtmlsyn.png>) | [TSynHTMLSyn](<TSynHTMLSyn.md> "TSynHTMLSyn") | HTML syntax-highlighting   
[![tsynxmlsyn.png](https://wiki.freepascal.org/images/2/29/tsynxmlsyn.png)](</File:tsynxmlsyn.png>) | [TSynXMLSyn](<TSynXMLSyn.md> "TSynXMLSyn") | XML syntax-highlighting   
[![tsynlfmsyn.png](https://wiki.freepascal.org/images/f/f4/tsynlfmsyn.png)](</File:tsynlfmsyn.png>) | [TSynLFMSyn](</index.php?title=TSynLFMSyn&action=edit&redlink=1> "TSynLFMSyn \(page does not exist\)") | LFM syntax-highlighting   
[![tsyndiffsyn.png](https://wiki.freepascal.org/images/b/b2/tsyndiffsyn.png)](</File:tsyndiffsyn.png>) | [TSynDiffSyn](</index.php?title=TSynDiffSyn&action=edit&redlink=1> "TSynDiffSyn \(page does not exist\)") |   
[![tsynunixshellscriptsyn.png](https://wiki.freepascal.org/images/e/e7/tsynunixshellscriptsyn.png)](</File:tsynunixshellscriptsyn.png>) | [TSynUNIXShellScriptSyn](<TSynUNIXShellScriptSyn.md> "TSynUNIXShellScriptSyn") | Unix shell syntax-highlighting   
[![tsyncsssyn.png](https://wiki.freepascal.org/images/b/ba/tsyncsssyn.png)](</File:tsyncsssyn.png>) | [TSynCssSyn](<TSynCssSyn.md> "TSynCssSyn") | CSS syntax-highlighting   
[![tsynphpsyn.png](https://wiki.freepascal.org/images/2/22/tsynphpsyn.png)](</File:tsynphpsyn.png>) | [TSynPHPSyn](<TSynPHPSyn.md> "TSynPHPSyn") | PHP syntax-highlighting   
[![tsyntexsyn.png](https://wiki.freepascal.org/images/0/02/tsyntexsyn.png)](</File:tsyntexsyn.png>) | [TSynTeXSyn](<TSynTeXSyn.md> "TSynTeXSyn") | (La)TeX syntax-highlighting   
[![tsynsqlsyn.png](https://wiki.freepascal.org/images/f/fc/tsynsqlsyn.png)](</File:tsynsqlsyn.png>) | [TSynSQLSyn](<TSynSQLSyn.md> "TSynSQLSyn") | SQL syntax-highlighting   
[![tsynpythonsyn.png](https://wiki.freepascal.org/images/b/b3/tsynpythonsyn.png)](</File:tsynpythonsyn.png>) | [TSynPythonSyn](<TSynPythonSyn.md> "TSynPythonSyn") | Python syntax-highlighting   
[![tsynvbsyn.png](https://wiki.freepascal.org/images/e/ed/tsynvbsyn.png)](</File:tsynvbsyn.png>) | [TSynVBSyn](<TSynVBSyn.md> "TSynVBSyn") | Visual Basic syntax-highlighting   
[![tsynanysyn.png](https://wiki.freepascal.org/images/8/8d/tsynanysyn.png)](</File:tsynanysyn.png>) | [TSynAnySyn](</index.php?title=TSynAnySyn&action=edit&redlink=1> "TSynAnySyn \(page does not exist\)") |   
[![tsynmultisyn.png](https://wiki.freepascal.org/images/a/a9/tsynmultisyn.png)](</File:tsynmultisyn.png>) | [TSynMultiSyn](</index.php?title=TSynMultiSyn&action=edit&redlink=1> "TSynMultiSyn \(page does not exist\)") |   
[![tsynbatsyn.png](https://wiki.freepascal.org/images/3/3e/tsynbatsyn.png)](</File:tsynbatsyn.png>) | [TSynBatSyn](<TSynBatSyn.md> "TSynBatSyn") | Batch-file syntax-highlighting   
[![tsyninisyn.png](https://wiki.freepascal.org/images/4/4c/tsyninisyn.png)](</File:tsyninisyn.png>) | [TSynIniSyn](<TSynIniSyn.md> "TSynIniSyn") | INI-file syntax-highlighting   
[![tsynposyn.png](https://wiki.freepascal.org/images/7/75/tsynposyn.png)](</File:tsynposyn.png>) | [TSynPoSyn](</index.php?title=TSynPoSyn&action=edit&redlink=1> "TSynPoSyn \(page does not exist\)") |

---

_Source: [https://wiki.freepascal.org/Syntax_highlighting](https://web.archive.org/web/20220809113957/https://wiki.freepascal.org/Syntax_highlighting)_
