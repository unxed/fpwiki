# Basic Pascal Tutorial/Chapter 2/EOLN and EOF

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_2/EOLN_and_EOF/bg> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Chapter_2/EOLN_and_EOF/de> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF/de")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_2/EOLN_and_EOF/fr> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_2/EOLN_and_EOF/ja> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_2/EOLN_and_EOF/zh_CN> "Basic Pascal Tutorial/Chapter 2/EOLN and EOF/zh CN")** │    
****

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_2/Files.md> "Basic Pascal Tutorial/Chapter 2/Files") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_2/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 2/Programming Assignment")  
---|---|---  
  
2E - EOLN and EOF (author: Tao Yue, state: unchanged) 

`EOLN` is a Boolean function that is `TRUE` when you have reached the end of a line in an open input file. 
    
    
    eoln (file_variable)
    

If you want to test to see if the standard input (the keyboard) is at an end-of-line, simply issue `eoln` without any parameters. This is similar to the way in which `read` and `write` use the console (keyboard and screen) if called without a file parameter. 
    
    
    eoln
    

`EOF` is a Boolean function that is `TRUE` when you have reached the end of the file. 
    
    
    eof (file_variable)
    

Usually, you don't type the `end-of-file` character from the keyboard. On DOS/Windows machines, the character is `Control-Z`. On UNIX/Linux machines, the character is `Control-D`. 

[ ◄ ](<Basic_Pascal_Tutorial/Chapter_2/Files.md> "Basic Pascal Tutorial/Chapter 2/Files") | [ ▲ ](<Basic_Pascal_Tutorial/Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Basic_Pascal_Tutorial/Chapter_2/Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 2/Programming Assignment")  
---|---|---

---

_Source: [https://wiki.freepascal.org/EOLN_and_EOF](https://web.archive.org/web/20250321095651/https://wiki.freepascal.org/EOLN_and_EOF)_
