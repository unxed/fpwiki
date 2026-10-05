# Emmet-Pascal

## Contents

  * 1 About
  * 2 Usage
  * 3 Wrap with abbreviation
  * 4 Filters
  * 5 Options
  * 6 Abbreviations for Markdown
  * 7 Download
  * 8 Authors



# About

Emmet is mostly used by web-developers to simplify and speed up editing. Emmet can take an abbreviation and expand it into a structured code block. Standard Emmet is written in JavaScript and available in many text editors or web development tools. Emmet for Delphi and Free Pascal is written from scratch in Delphi and is used in RJ TextEd and [CudaText](<CudaText.md> "CudaText"). 

Cheat sheets: 

  * Emmet-Pascal - <https://www.rj-texted.se/Help/Emmetcheatsheet.html>
  * Standard Emmet - <https://docs.emmet.io/cheat-sheet/>



Engine gives special abbreviations for different file types: 

  * HTML: about 200 items
  * XSL: about 40 items
  * SVG: about 50 items
  * CSS: about 650 items
  * Markdown: about 20 items



# Usage

First you need to create an Emmet object. 
    
    
    FEmmet := TEmmet.Create(snippetsPath, loremPath);
    

  * snippetsPath: The file path to Snippets.ini e.g. "c:\foo\Snipptes.ini"
  * loremPath: The file path to Lorem.txt e.g. "c:\foo\Lorem.txt"



To expand an abbreviation, use 
    
    
    sExpanded := FEmmet.ExpandAbbreviation(sAbbr, sSyntax, sSelText, sSection, bMultiCursorTabs);
    

Parameters: 

  * sAbbr: Abbreviation e.g. "ul>li*5"
  * sSyntax: Code language in lowercase e.g. "html". Available values are: html, css, xsl, svg, xml, jsx, less, sass, scss.
  * sSelText: Text is used to wrap with abbreviation
  * sSection: Gets the section used in snippets.ini e.g. "html"
  * bMultiCursorTabs: Gets True if cursor positions in expanded string should be handled as multi cursor positions



Result: sExpanded is the resulting expanded code. It may contain cursor | positions or selected tab ${1:charset} positions. 

# Wrap with abbreviation

To use this feature, use parameter sSelText. E.g. if you call ExpandAbbreviation() with: 

  * sAbbrev: ul>li*
  * sSelText:


    
    
    Line 1
    Line 2
    Line 3
    Line 4
    

You get the result 
    
    
    <ul>
      <li>Line 1</li>
      <li>Line 2</li>
      <li>Line 3</li>
      <li>Line 4</li>
    </ul>
    

# Filters

TEmmet can handle some filters. A filter is added at the end of the abbreviation using a pipe |. E.g. 
    
    
    ul>li*|t
    

  * c - Comment important tags (containing class or id attributes).
  * e - Escape XML-unsafe characters: <, > and &.
  * s - Single line. Expand everything to a single line of code.
  * t - Trim line markers from wrapped lines e.g. "* ", "- " or "1."
  * w - Wordwrap selected or lorem generated text. Default width is 80.
  * w<x> \- Wordwrap at column x. E.g. |w120 will wrap lines at column 120.



Example: 
    
    
            sAbbrev = ul>li*|t
        
            sSelText =
              * Line 1
              * Line 2
        
            Result =
              <ul>
                <li>Line 1</li>
                <li>Line 2</li>
              </ul>
    

# Options

Several options exist in a record "TExpandOptions" that can be passed to the expand function. Options are: 

  * AddSlashToEmptyTags: Boolean; // Add a slash to empty tags e.g. <img src="" />
  * AlwaysAddNewLine: Boolean; // Always add linefeed after each tag (usually used in XML)
  * CommentTags: Boolean; // Comment important tags (containing class or id attributes).
  * IndentChilds: Boolean; // Indent child tags. If you set this to false - no indention will be used.
  * SingleLine: Boolean // Expand everything to a single line of code.
  * TabSize: Integer; // Tab size in characters. This is only used with wordwrap.
  * TrimLineMarkers: Boolean; // Trim line markers from wrapped lines e.g. "* ", "- " or "1."
  * Wordwrap: Boolean; // Word wrap selected or lorem generated text.
  * WordwrapAt: Integer; // Wrap at given column. The nearest space or symbol will be used as wrap position.



Use the overloaded version of ExpandAbbreviation to set these options. 
    
    
    var
      rOptions: TExpandOptions;
    begin
      rOptions.Wordwrap := True;
      rOptions.AddSlashToEmptyTags := False;
      sExpanded := FEmmet.ExpandAbbreviation(sAbbr, sSyntax, sSelText, sSection, bMultiCursorTabs, rOptions);
    

# Abbreviations for Markdown

In files with .md extension, special abbreviations are supported: 
    
    
         a           = link
         b           = bold
         bq          = blockquote
         code        = inline code snippet
         h1..h6      = heading
         hr          = horizontal rule
         i           = italic
         img         = image
         ol          = ordered list
         pre         = code block with language based highlighting
         strike      = strike through
         table       = table
         ul          = unordered list
         @l or @l80  = create lorem generated text
    

# Download

GitHub: <https://github.com/rickard67/Emmet-Pascal>

Originally was in the "Alexey-T" account, later was moved to Rickard's account, the author of Emmet-Pascal. 

# Authors

  * Rickard Johansson - "emmet" unit
  * Alexey Torgashin - "emmethelper" unit and FPC demos

---

_Source: [https://wiki.freepascal.org/Emmet-Pascal](https://web.archive.org/web/20250126092210/https://wiki.freepascal.org/Emmet-Pascal)_
