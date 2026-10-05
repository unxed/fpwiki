# vim

`vim` is an [editor](<source_code_editor.md> "source code editor") well-suited to program. 

## Contents

  * 1 basic usage
  * 2 configuration
    * 2.1 syntax highlighting
    * 2.2 margin
    * 2.3 tabstops
    * 2.4 tab replacement
    * 2.5 white characters
    * 2.6 navigation
    * 2.7 folding
  * 3 see also



## basic usage

The most important difference to many other editors is that `vim` constantly has a _mode_. The mode is like the caps lock key on your keyboard: By default you are in “lowercase mode” and by pressing the key you can switch (or leave) the “uppercase mode”. 

The `vim` has several very useful modes, but to start off you need to know there are 

  * normal mode, and
  * insert mode.



`vim` indicates any modes _other_ than _normal_ in the bottom line, e.g. by showing `‑‑ INSERT ‑‑`. 

In order to _enter_ insert mode _from_ normal mode, you can hit the `i` key (among many other possibilities). _Then_ you can start typing and editing like usual. In order to _leave_ insert mode again, you hit the `Esc` key. 

Saving your source code file is done by typing `:write` in normal mode. If there is no file name associated with the current text buffer, you will furthermore need to specify a file name (`:write helloWorld.pas`). Quitting the `vim` can be done by typing `:quit` in normal mode. This in fact just closes the currently focused window, so you may have to type it repeatedly (e.g. after you have opened a help window by typing `:help`). 

Opening a specific file is usually done by supplying the file name as an argument on the command line. It is also possible to use the command `:open /tmp/foobar.pas`. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you like to get a more comprehensive tutorial, start `vimtutor` which will guide through your first steps. See `vimtutor(1)` for more details.

## configuration

The `vim` editor experience can be enhanced for editing [Pascal](<Pascal.md> "Pascal") source code files. You most probably want to insert any of the configuration settings below into your `vimrc` rather than typing it out every time you start `vim` (confer `:help vimrc` for its file system location). In your `vimrc` normal mode commands are not preceded by a colon, just like shown below. 

### syntax highlighting

Files matching `*.pas` and `*.dpr` are automatically recognized. Additionally, `*.p` files are recognized as Pascal source code files, but only if the first 10 non-empty lines contain keywords typical of Pascal. 

If the automatisms fail, e. g. for `*.inc` files, syntax highlighting can be chosen explicitly: 
    
    
    set filetype=pascal
    

Multiple syntax highlighting definitions can be combined, for example: 
    
    
    set filetype=pascal.doxygen
    

If you want more built-in data types to have special color, or even the single-line comment to be recognized as such, you will need 
    
    
    let g:pascal_fpc=1
    

and/or 
    
    
    let g:pascal_delphi=1
    

For further explanations see `:help pascal.vim`

### margin

The ex-command 
    
    
    set colorcolumn=80
    

will change the background of the 80th column. Depending on the `tabstop` setting you may reach it earlier or later. This is just a _visual_ guidance. The `textwidth` variable will actually thwart insertion of lines longer than a certain width. 

### tabstops

Non-permanent indentation, just for aesthetic reasons, can be a hairy subject. The easiest and most straightforward solution is to use actual tab characters and set their display width with: 
    
    
    set tabstop=5
    

`vim` supports indentation using space characters. The indentation level can be recognized by counting spaces at the beginning of lines. How many spaces one indentation level occupies _on screen_ can altered dynamically. Since proper configuration involves interaction of _several_ settings though, we refer to the documentation for more details. 

### tab replacement

Some people prefer hitting the `Tab ⇆` key to be directly translated to a number of space characters. The `vim` supports this, confer some `:help` for details. The setting `:let pascal_no_tabs=1` will highlight any tab characters as an error. 

Confer `:help retab` if you want to `expand(1)`. 

### white characters

Using 
    
    
    set listchars=space:⍽
    

and then activating the “list mode” `:set list`, you can display all space characters with a custom symbol. This uses a different color than the actual character used. 

### navigation

The [`%`](</index.php?title=%25&action=edit&redlink=1> "% \(page does not exist\)") key allows you to jump matching parenthesis and curly braces. However, extra precautions have to be taken for [`begin`](<Begin.md> "Begin") and [`end`](<End.md> "End"). You have enable the `matchit` plugin (cf. `:help matchit-install`): 
    
    
    packadd! matchit
    

### folding

Confer `:help folding`. 

The [original editor](</User:Kai_Burghardt> "User:Kai Burghardt") of this article recommends manual folds using markers: 
    
    
    set foldmethod=marker
    

This will generate folds on markers. The default markers are `{{{` and `}}}` (confer `:help foldmarker`). Thus, the following code 
    
    
    // procedure foo(integer, Boolean) {{{
    procedure foo(const bar: integer; const able: Boolean);
    begin
    end;
    // }}}
    

will look like 
    
    
    +--  5 lines: procedure foo(integer, Boolean) ----------------------------------
    

when folded. 

Instead of the default triple curly braces, you can honor Lazarus’ IDE directives: 
    
    
    set foldmarker={%region},{%endregion}
    

Note, fold markers are _static_ strings. Regular expressions are not supported, thus a fold marker definition like this one is _case-sensitive_. Fold markers appearing in string literals are still recognized as fold markers. As a workaround you can split the string, of course: `'The IDE directive {%reg' + 'ion} sucks.'`

## see also

  * Wikibook: [Learning the `vi` editor](<https://en.wikibooks.org/wiki/Learning_the_vi_Editor/Vim>)

---

_Source: [https://wiki.freepascal.org/vim](https://web.archive.org/web/20250324164552/https://wiki.freepascal.org/vim)_
