# Basic Pascal Tutorial/Chapter 1/Program Structure

│ **[العربية (ar)](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/ar> "Basic Pascal Tutorial/Chapter 1/Program Structure/ar")** │  **[български (bg)](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/bg> "Basic Pascal Tutorial/Chapter 1/Program Structure/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/de> "Basic Pascal Tutorial/Chapter 1/Program Structure/de")** │  **English (en)** │  **[español (es)](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/es> "Basic Pascal Tutorial/Chapter 1/Program Structure/es")** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/fr> "Basic Pascal Tutorial/Chapter 1/Program Structure/fr")** │  **[italiano (it)](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/it> "Basic Pascal Tutorial/Chapter 1/Program Structure/it")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/ja> "Basic Pascal Tutorial/Chapter 1/Program Structure/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/ko> "Basic Pascal Tutorial/Chapter 1/Program Structure/ko")** │  **[русский (ru)](<../../../ru/Basic_Pascal_Tutorial/Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure/ru")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_1/Program_Structure/zh_CN> "Basic Pascal Tutorial/Chapter 1/Program Structure/zh CN")** │    
****

[ ◄ ](<../Hello,_World.md> "Basic Pascal Tutorial/Hello, World") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers")  
---|---|---  
  
Basics 1A - Program Structure (author: Tao Yue, state: changed) 

The basic structure of a Pascal program is: 
    
    
    PROGRAM ProgramName (FileList);
    
    CONST
      (* Constant declarations *)
    
    TYPE
      (* Type declarations *)
    
    VAR
      (* Variable declarations *)
    
    (* Subprogram definitions *)
    
    BEGIN
      (* Executable statements *)
    END.
    

The **PROGRAM** statement is optional in Free Pascal. 

The elements of a program must be in the correct order, though some may be omitted if not needed. Here's a program that does nothing, but has all the required elements: 
    
    
    program DoNothing;
    begin
    end.
    

Comments are portions of the code which do not compile or execute. Free Pascal supports two types of comments, free-form and line-based. Free-form comments either start with a (* and end with a *), or more commonly, start with { and end with }. You should be careful if you nest free form comments as they may yield an error, e.g.: 
    
    
            (* (* *) *)
    

will, depending on the compiler, either be accepted, or yield an error because, 

  * if the compiler does not allow nested comments, it matches the first '`(*`' with the first '`*)`', ignoring the second '`(*`' which is between the first set of comment markers, which means everything after the first '`*)`' is treated as code. If there was nothing but blanks there, the second '`*)`' is not recognized and throws an error. OR
  * The second '`(*`' starts a nested comment, everything up to the next '`*)`' is in the nested comment, then the last '`*)`' closes the comment.



The same thing applies to '`{`' comments. Each '`{`' must be closed by '`}`' and if the compiler supports nested comments, all comments must be closed with the same marker as the one that opened it. So if you have code consisting of: 
    
    
            (* {  *)
    

OR 
    
    
            { (*  }
    

Will cause and "unexpected end of file" error, because the inner comment was not closed. 

It's usually best to use one type of comment exclusively, and save the other for when you want to mark off a piece of code you (temporarily) don't want to use, like: 
    
    
    (*
        bad code that does't work {comment}
        more bad code // and comments
    *)
    

This problem with comment markers is one reason why many languages offer line-based commenting systems. 

Free Pascal also supports // as a line-based comment. When two slashes appear, other than in a quoted string or a free-form comment, the rest of the line is ignored. 

Turbo Pascal and most other modern compilers support free-form brace comments, such as `{Comment}`. The opening brace signifies the beginning of a block of comments, and the ending brace signifies the end of a block of comments. Brace comments are also used for compiler directives. 

Commenting makes your code easier to understand. If you write your code without comments, you may come back to it weeks, months, or years later without a guide to why you coded the program that way. In particular, you may want to document the major design of your program and insert comments in your code when you deviate from that design for a good reason. 

In addition, comments are often used to take problematic code out of action without deleting it. Remember the earlier restriction on nesting comments? It just so happens that braces `{}` take precedence over parentheses-stars `(* *)`. You will not get an error if you do this: 
    
    
    { (* Comment *) }
    

Whitespace (spaces, tabs, and end-of-lines) are ignored by the Pascal compiler unless they are inside a literal string. However, to make your program readable by human beings, you should indent your statements and put separate statements on separate lines. Indentation is often an expression of individuality by programmers, but collaborative projects usually select one common style to allow everyone to work from the same page. 

[ ◄ ](<../Hello,_World.md> "Basic Pascal Tutorial/Hello, World") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Program_Structure](https://web.archive.org/web/20241002232943/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Program_Structure)_
