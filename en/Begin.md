# Begin

│ **[Deutsch (de)](</Begin/de> "Begin/de")** │  **English (en)** │  **[español (es)](</Begin/es> "Begin/es")** │  **[suomi (fi)](</Begin/fi> "Begin/fi")** │  **[français (fr)](</Begin/fr> "Begin/fr")** │  **[русский (ru)](<../ru/Begin.md> "Begin/ru")** │  **[中文（中国大陆）‎ (zh_CN)](</Begin/zh_CN> "Begin/zh CN")** │    
****

The [reserved word](<Reserved_word.md> "Reserved word") `begin` marks the start of the definition of the executable portion of a [block](<Block.md> "Block"). In conjunction with [`end`](<End.md> "End") it is also used to group [statements](<statement.md> "statement") into a so-called “compound statement”. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** In [Pascal](<Pascal.md> "Pascal") a compound statement does not create a new scope. 

Only blocks do.

In [extended Pascal](<Extended_Pascal.md> "Extended Pascal") `to begin do …` starts the definition of the [`initialization` part of a module](<Initialization.md> "Initialization"). 

While every `begin` must have a corresponding `end`, not all occurrences of `end` have a corresponding `begin`. 

## syntax justification

Lots of programming languages use a pair of single characters to indicate boundaries. Typing out the words `begin` and `end` is indeed more cumbersome than writing `{ }`. However, the meaning of such characters is not as obvious as words are. 

## matching

  * The source editor of the [Lazarus IDE](<Lazarus_IDE.md> "Lazarus IDE") supports a “find matching begin/end” function.
  * [vim](<vim.md> "vim") can be configured to support matching Pascal’s `begin`/`end`, too.



  
**Keywords:** begin — [do](<Do.md> "Do") — [else](<Else.md> "Else") — [end](<End.md> "End") — [for](<For.md> "For") — [if](<If.md> "If") — [repeat](<Repeat.md> "Repeat") — [then](<Then.md> "Then") — [until](<Until.md> "Until") — [while](<While.md> "While")

---

_Source: [https://wiki.freepascal.org/Begin](https://web.archive.org/web/20231204160417/https://wiki.freepascal.org/Begin)_
