# source code editor

What's a source code editor? 

Let's first exclude, what it is **not**. It is not a textprocessor, i.e. 

  * uses an single monospaced western font of unique size and spacing [Courier].
  * lines have a unique height and spacing.
  * does not add hidden attributes to the stored text.
  * text attributes are used only for #syntax highlighting or similar predefined purposes.
  * has left justified tabstops [of unique width?]
  * does not normally wrap long lines.
  * has no document, page or paragraph formatting or layout.



Some common and desireable features are: 

  * adjustable tab expansion.
  * right margin marker (soft).
  * block selection and processing (single block).
  * read-only, insert and overwrite input modes.
  * rich choice of keyboard commands.
  * scrollbars, mouse and mousewheel support.
  * undo/redo support.
  * clipboard support.
  * line number and special marker display (gutter).



Additional useful extensions: 

  * #syntax highlighting.
  * #bookmarks.
  * hyperlink and block #navigation.
  * #code completion.
  * visualization of #white characters.
  * display and editing of embedded #Unicode literals.
  * #macro capabilites.
  * block #folding.
  * #block movement.
  * #EOL adjustment.
  * #tab replacement.
  * smart #indentation.
  * multiple text #windows, easy content switch, multiple display of same text source.
  * persistent user #settings.
  * #context menu.



Also desireable, but not for source code editing: 

  * #word wrap.
  * #Unicode support.
  * #hexdump of binary sources.



## Unsorted

(to be categorized) 

### Font

For traditional and performance reasons, a monospaced font shall be used. This allows to manage the display in form of a grid, with easy mapping between screen coordinates and character indices. Most compilers only accept ASCII code, other characters can be used only in comments. 

Of course support for other character sets (input and display) and input orientation (right-to-left) can be added - by persons with according knowledge and skills. I have no idea how to e.g. mix RTL string literals with LTR program code. OTOH I'm not comfortable with ASCII text myself, as are many more non-English speakers. I'll support any (every?) attempt to remove language barreers from [SynEdit](<SynEdit.md> "SynEdit"), but cannot contribute many ideas or techniques to this subject.

---

_Source: [https://wiki.freepascal.org/source_code_editor](https://web.archive.org/web/20191118173222/https://wiki.freepascal.org/source_code_editor)_
