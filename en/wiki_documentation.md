# wiki documentation

│ **English (en)** │    
****

## Contents

  * 1 Guide to Wiki Editing
    * 1.1 Overview
    * 1.2 Syntax Highlighting
    * 1.3 Keyboard shortcuts
    * 1.4 Note
    * 1.5 Tip
    * 1.6 Warning



# Guide to Wiki Editing

## Overview

Tutorials 

  * [WikiPedia Tutorial](<http://en.wikipedia.org/wiki/Wikipedia:Tutorial>)
  * spam, no tut ! : <http://www.chat11.com/30_Second_Quick_Wiki_Tutorial> \--- 30 Second Quick Wiki Tutorial



A [Sand Box](<Sand_Box.md> "Sand Box") is available for practice. 

If you have any problems, please use this [Forum](<https://forum.lazarus.freepascal.org/index.php?board=8.0>). You can also leave a note or suggestion on our [Site Feedback](<Site_Feedback.md> "Site Feedback") page. 

## Syntax Highlighting

A Wiki for a programming language site needs a powerful way of showing source code in readable fashion. Therefore we use an automatic syntax highlighter to for sources. 

Use the "<syntaxhighlight=pascal>" tag for FPC and Pascal code. 

Use the "<syntaxhighlight=delphi>" tag for Delphi code. 

Note: "<source=pascal>" could also work but is not recommended (see [highlighter docs](<http://www.mediawiki.org/wiki/Extension:SyntaxHighlight_GeSHi#Alternative_.3Csource.3E_tag>)) 

More languages are supported; for a list, see the [highlighter docs](<http://www.mediawiki.org/wiki/Extension:SyntaxHighlight_GeSHi#Supported_languages>). 

  * for XML code, use <syntaxhighlight lang=xml>
  * for SQL code, use <syntaxhighlight lang=sql>
  * for DOS/CMD window code, use <syntaxhighlight lang=dos>
  * for bash/shell script code, use <syntaxhighlight lang=bash>
  * for ini file content, use use <syntaxhighlight lang=ini>
  * for Visual BASIC code, use <syntaxhighlight lang=vb>
  * for Java code, use <syntaxhighlight lang=java>
  * for JavaScript code, use <syntaxhighlight lang=javascript>



The following example shows the use of the syntax highlighter in this Wiki: 
    
    
    <syntaxhighlight lang=pascal>
    Program Test;
    Uses Crt;
    
    Var I : Integer;
    
    Begin
      For I := 0 to 12 do
        WriteLn('Test ',I:2);
    End.
    </syntaxhighlight>
    

results in this output 
    
    
    Program Test;
    Uses Crt;
    
    Var I : Integer;
    
    Begin
      For I := 0 to 12 do
        WriteLn('Test ',I:2);
    End.
    

Below is an example for XML sources. 
    
    
    <syntaxhighlight lang="xml">
    <xs:complexType name="DecimalWithUnits">
      <xs:simpleContent>
        <xs:extension base="xs:decimal">
          <xs:attribute name="Units" type="xs:string"
                        use="required"/>
        </xs:extension>
      </xs:simpleContent>
    </xs:complexType> 
    </syntaxhighlight>
    

which results in: 
    
    
    <xs:complexType name="DecimalWithUnits">
      <xs:simpleContent>
        <xs:extension base="xs:decimal">
          <xs:attribute name="Units" type="xs:string"
                        use="required"/>
        </xs:extension>
      </xs:simpleContent>
    </xs:complexType>
    

Below is an example for INI file content: 
    
    
    <syntaxhighlight lang=ini>
    [Section Name]
    Name=Value
    Height=23
    Width=32
    </syntaxhighlight>
    

which results in: 
    
    
    [Section Name]
    Name=Value
    Height=23
    Width=32
    

For program output or other verbatim quotes simply indent the text by one character like this: 
    
    
    This text is indented from the left margin by one character.
    

You can also use <pre> and </pre> tags. For example: 
    
    
    This text begins with the <pre> tag and ends with the </pre> tag
    

## Keyboard shortcuts

To display keyboard shortcuts use template [Template:keypress](</Template:keypress> "Template:keypress"): 
    
    
    {{keypress|Ctrl}}+{{keypress|F12}}
    

This will show as: 

`Ctrl`+`F12`

## Note

To make text visible as a note use template Note: 
    
    
    {{Note|This is a note for the reader}}

This will be displayed as: 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This is a note for the reader

## Tip

To make text visible as a tip use the Tip template: 
    
    
    {{Tip|This is a tip for the reader}}

This will be displayed as: 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** This is a tip for the reader

## Warning

To make text visible as a warning use template Warning: 
    
    
    {{Warning|This is a warning for the reader}}

This will be displayed as: 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This is a warning for the reader

---

_Source: [https://wiki.freepascal.org/wiki_documentation](https://web.archive.org/web/20240716031940/https://wiki.freepascal.org/wiki_documentation)_
