# Basic Pascal Tutorial/Hello, World

│ **[العربية (ar)](</Basic_Pascal_Tutorial/Hello,_World/ar> "Basic Pascal Tutorial/Hello, World/ar")** │  **[български (bg)](</Basic_Pascal_Tutorial/Hello,_World/bg> "Basic Pascal Tutorial/Hello, World/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Hello,_World/de> "Basic Pascal Tutorial/Hello, World/de")** │  **English (en)** │  **[español (es)](</Basic_Pascal_Tutorial/Hello,_World/es> "Basic Pascal Tutorial/Hello, World/es")** │  **[français (fr)](</Basic_Pascal_Tutorial/Hello,_World/fr> "Basic Pascal Tutorial/Hello, World/fr")** │  **[italiano (it)](</Basic_Pascal_Tutorial/Hello,_World/it> "Basic Pascal Tutorial/Hello, World/it")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Hello,_World/ja> "Basic Pascal Tutorial/Hello, World/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Hello,_World/ko> "Basic Pascal Tutorial/Hello, World/ko")** │  **[русский (ru)](<../../ru/Basic_Pascal_Tutorial/Hello,_World.md> "Basic Pascal Tutorial/Hello, World/ru")** │  **[svenska (sv)](</Basic_Pascal_Tutorial/Hello,_World/sv> "Basic Pascal Tutorial/Hello, World/sv")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Hello,_World/zh_CN> "Basic Pascal Tutorial/Hello, World/zh CN")** │    
****

[ ◄ ](<Compilers.md> "Basic Pascal Tutorial/Compilers") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure")  
---|---|---  
  
Hello, World (author: Tao Yue, state: unchanged) 

In the short history of computer programming, one enduring tradition is that the first program in a new language is a "Hello, world" to the screen. So let's do that. Copy and paste the program below into your IDE or text editor, then compile and run it. 

If you have no idea how to do this, return to the Table of Contents. Earlier lessons explain what a compiler is, give links to downloadable compilers, and walk you through the installation of an open-source Pascal compiler on Windows. 
    
    
    program Hello;
    begin
      writeln ('Hello, world.');
    end.
    

The output on your screen should look like: 
    
    
    Hello, world.
    

If you're running the program in an IDE, you may see the program run in a flash, then return to the IDE before you can see what happened. See the bottom of the previous lesson for the reason why. One suggested solution, adding a readln to wait for you to press Enter before ending the program, would alter the "Hello, world" program to become: 
    
    
    program Hello;
    begin
      writeln ('Hello, world.');
      readln;
    end.
    

[ ◄ ](<Compilers.md> "Basic Pascal Tutorial/Compilers") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Hello,_World](https://web.archive.org/web/20250420130125/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Hello,_World)_
