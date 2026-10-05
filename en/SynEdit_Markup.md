# SynEdit Markup

│ **English (en)** │  **[polski (pl)](</SynEdit_Markup/pl> "SynEdit Markup/pl")** │ 

For more info on SynEdit go to: [SynEdit](<SynEdit.md> "SynEdit")   
This page is a draft .... 

## Contents

  * 1 Terminology
  * 2 About TSynEditMarkup
  * 3 Where the color comes from
  * 4 TSynEditMarkup
  * 5 See also



# Terminology

Coloring
    Applying attributes to the Text. Fore- and Background-color, Bold, Italic, Underline, Colored Frame or partial Frame (includes zig-zag line). Also referred to as **highlighting** , though this term is ambiguous as it can refer to either all coloring, or coloring by the Highlighter only.
Highligher
    [SynEdit Highlighter](<SynEdit_Highlighter.md> "SynEdit Highlighter")
Markup
    TSynEditMarkup
token
    A fragment of text. Usually a part of a line. This can be at word boundaries, but also go down to individual letters.

# About TSynEditMarkup

TSynEditMarkup allows to alter the coloring provided by the Highlighter. 

It is used for: 

  * coloring the selection
  * coloring the current line
  * coloring special lines (error, breakpoint....)
  * coloring matching brackets "()"
  * coloring "ctrl links" => when you can click an identifier to jump to its declaration
  * coloring the current word (word at caret)
  * coloring matching begin/end
  * coloring "lowlight" inactive ifdef (using codetools)



and more 

# Where the color comes from

SynEdits paint method retrieves the text as a serious of "token". 

The primary source of those tokens is the Highligher. In the line: 
    
    
    a := 1; // comment
    

The tokens might be 

  * "a " normal text
  * ":=" a symbol (painted in a different color)
  * " " normal text
  * "1" number (painted in a different color)
  * ";" a symbol (painted in a different color)
  * " " normal text
  * "// coment" a comment (painted in a different color)



See also [SynEdit Highlighter](<SynEdit_Highlighter.md> "SynEdit Highlighter"). 

Those token a retrieved via a list of TLazSynDisplayView (coupled with TSynEditStrings wrappers of the TextBuffer). This is not important for coloring. Though the TLazSynDisplayView could change color. 

Each token is then fed to the list of TSynEditMarkup. Each TSynEditMarkup can define additional coloring. If that happens the tokens will be broken into smaller parts, and the coloring will be combined/mixed/replaced as needed. 

# TSynEditMarkup

TODO 

During paint the following method are called 

  * Procedure PrepareMarkupForRow(aRow : Integer); virtual;
            Procedure FinishMarkupForRow(aRow : Integer); virtual;
        



  * Procedure BeginMarkup; virtual;
            Procedure EndMarkup; virtual;
        



  * Procedure GetNextMarkupColAfterRowCol(const aRow: Integer;
                                                  const aStartCol: TLazSynDisplayTokenBound;
                                                  const AnRtlInfo: TLazSynDisplayRtlInfo;
                                                  out   ANextPhys, ANextLog: Integer); virtual; abstract;
        




Given the current position (painting line number ARow, X is specified in AStartCol), the Markup must set ANextPhys or ANextLog to the next X pos at which it wants to change coloring. 

If the Markup wants to change text to Bold from X=5 to X=10 and X=15 to X=20, then 

  * for any AStartCol in 1 to 4 => ANext... = 5 (switch to bold at 5)
  * for any AStartCol in 5 to 9 => ANext... = 10 (switch to not modifying at 10)
  * for any AStartCol in 10 to 14 => ANext... = 15 (switch to bold at 15)
  * for any AStartCol in 15 to 19 => ANext... = 20 (switch to not modifying at 20)



  


  * Function  GetMarkupAttributeAtRowCol(const aRow: Integer;
                                                 const aStartCol: TLazSynDisplayTokenBound;
                                                 const AnRtlInfo: TLazSynDisplayRtlInfo) : TSynSelectedColor; virtual; abstract;
        




If AStartCol is within any range for that the Markup want to change color, then it must return the correct * for any AStartCol in 5 to 9 => ANext... = 10 (switch to not modifying at 10) 

# See also

  * <https://forum.lazarus.freepascal.org/index.php/topic,47778.msg342734.html#msg342734>

---

_Source: [https://wiki.freepascal.org/SynEdit_Markup](https://web.archive.org/web/20241207110520/https://wiki.freepascal.org/SynEdit_Markup)_
