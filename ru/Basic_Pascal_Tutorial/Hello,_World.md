# Basic Pascal Tutorial/Hello, World

│ **[العربية (ar)](</Basic_Pascal_Tutorial/Hello,_World/ar> "Basic Pascal Tutorial/Hello, World/ar")** │  **[български (bg)](</Basic_Pascal_Tutorial/Hello,_World/bg> "Basic Pascal Tutorial/Hello, World/bg")** │  **[Deutsch (de)](</Basic_Pascal_Tutorial/Hello,_World/de> "Basic Pascal Tutorial/Hello, World/de")** │  **[English (en)](<../../en/Basic_Pascal_Tutorial/Hello,_World.md> "Basic Pascal Tutorial/Hello, World")** │  **[español (es)](</Basic_Pascal_Tutorial/Hello,_World/es> "Basic Pascal Tutorial/Hello, World/es")** │  **[français (fr)](</Basic_Pascal_Tutorial/Hello,_World/fr> "Basic Pascal Tutorial/Hello, World/fr")** │  **[italiano (it)](</Basic_Pascal_Tutorial/Hello,_World/it> "Basic Pascal Tutorial/Hello, World/it")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Hello,_World/ja> "Basic Pascal Tutorial/Hello, World/ja")** │  **[한국어 (ko)](</Basic_Pascal_Tutorial/Hello,_World/ko> "Basic Pascal Tutorial/Hello, World/ko")** │  **русский (ru)** │  **[svenska (sv)](</Basic_Pascal_Tutorial/Hello,_World/sv> "Basic Pascal Tutorial/Hello, World/sv")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Hello,_World/zh_CN> "Basic Pascal Tutorial/Hello, World/zh CN")** │    
****

[ ◄ ](<Compilers.md> "Basic Pascal Tutorial/Compilers/ru") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure/ru")  
---|---|---  
  
Hello, World

Hello, World (author: Tao Yue, state: unchanged) 

  
В недолгой истории компьютерного программирования есть устоявшаяся традиция, что первая прграмма на новом языке должна выводить на экран текст "Hello, world". Так давайте сделаем это. Скопируйте программу, приведённую ниже, в текстовый редактор вашей IDE, затем скомпилируйте и запустите её. 

Если вы не знаете, как сделать это, вернитесь к оглавлению. Предыдущие уроки объяснят, что такое компилятор, дадут ссылки на страницы, где можно скачать компилятор и проведут вас через через установку компилятора Pascal с открытым исходным кодом под Windows. 
    
    
    program Hello;
    begin
      writeln ('Hello, world.')
    end.
    

Вывод на ваш экран должен выглядеть так: 
    
    
    Hello, world.
    

Если вы запустите программу в IDE, то сможете увидеть, как программа промелькнёт, а затем вернётся в IDE прежде, чем вы успеете увидеть, что произошло. Смотрите завершающую часть предыдущего урока, чтобы понять причину этого. Одно из предложенных решений, а именно - добавление **readln** для ожидания нажатия клавиши **Enter** перед завершением программы, изменит программу "Hello, world", и она станет выглядеть так: 
    
    
    program Hello;
    begin
      writeln ('Hello, world.');
      readln
    end.
    

[ ◄ ](<Compilers.md> "Basic Pascal Tutorial/Compilers/ru") | [ ▲ ](<Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Chapter_1/Program_Structure.md> "Basic Pascal Tutorial/Chapter 1/Program Structure/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Hello%2C_World/ru](https://web.archive.org/web/20250117043501/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Hello%2C_World/ru)_
