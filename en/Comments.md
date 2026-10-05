# Comments

│ **English (en)** │  **[русский (ru)](<../ru/Comments.md>)** │

****Go to:[Reserved words](<Reserved_words.md> "Reserved words") | [Operators](<Operators.md> "Operators")

Comments are human-readable notes or other kinds of annotations to support understanding of code. They are not interpreted by the compiler and ignored when building your program. 

  


## Contents

  * 1 Block comments
  * 2 Line comments
  * 3 Comments as declaration hints in Lazarus IDE
  * 4 Best practice for good comments
  * 5 See also



## Block comments

{

}

Block comments are delimited by the characters `{ }`   


(* 

*) 

or by the bigrams `(* *)`. The latter is a relict from times where computer keyboards did not necessarily have curly braces. It is _not wrong_ to use the bigrams, but they are at large superseded by the use of curly braces. There is one place where the `(*` and `*)` bigrams can be useful. if you are testing code and want to "dike out" or disable certain sections by marking them as inoperative, the pieces can be surrounded by these bigrams, and it will not matter if there are `{` and ``} comments inside of them. 

Example: 
    
    
    { public declarations }
    

In Pascal block comments can be nested. However, nested block comment delimiters have to match: If a block comment starts with an opening curly brace, it can not be closed by asterisk-closing-parenthesis but has to be closed by another closing curly brace. 

There is a special class of comment, either beginning `{$` or `(*$`. This indicates the start of a _[compiler directive](<Compiler_directive.md> "Compiler directive")_ , a special instruction to the ompiler about the program, but it does not actually generate instructions to be executed. 

  


## Line comments

Line comments or inline comments start with comment delimiter [`//`](<Slash.md> "Slash") and continue until the [end of the line](<End_of_Line.md> "End of Line"). Example: 
    
    
    // This is a stupid comment
    

  


## Comments as declaration hints in Lazarus IDE

The Lazarus IDE can display tooltip hints when mouseovering identifiers. You can control this in Options > Editor > Completion and Hints. 

The hint shows the function or variable definition, but can also include custom descriptive text taken from comment lines directly above the declaration. 

[![laz-comment-hint.png](https://wiki.freepascal.org/images/b/b8/laz-comment-hint.png)](</File:laz-comment-hint.png>)

  


## Best practice for good comments

Mentioning some general tips for writing “good” comments may not be left out here, too: 

  * Write _why_ something is done. Do not write _what_ is done, since that can (provided _meaningful_ identifiers are in place) be determined from the code itself. It would be redundant to repeat what the code already says by itself.
  * Structure your comments. For example write a summary above each method definition explaining what it does and how to use it, i.e. what impact parameters have on the method's behavior.
  * Especially name (non-trivial) _constraints_ that came from other places, that made you choose writing a particular line of code, or select a particular approach to solve the problem.
  * Write in _one_ language throughout the whole project. De facto English is the most prevalent comment language, but especially in the context of teaching other languages are more suitable. It is important that all team members can express complex circumstances in the used language (that's easier said than done).



  


## See also

  * [program structure](<Program_Structure.md> "Program Structure")
  * [category: compiler directives](</Category:Compiler_directives> "Category:Compiler directives")
  * [§ “Comments” in the “Free Pascal reference guide”](<https://www.freepascal.org/docs-html/ref/refse2.html>)

---

_Source: [https://wiki.freepascal.org/Comments](https://web.archive.org/web/20240920204059/https://wiki.freepascal.org/Comments)_
