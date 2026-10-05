# Basic Pascal Tutorial/Chapter 1/Constants

│ **[English (en)](<../../../en/Basic_Pascal_Tutorial/Chapter_1/Constants.md>)** │  **русский (ru)** │

[ ◄ ](<Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Variables_and_Data_Types.md> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types/ru")  
---|---|---  
  
Константы

1C - Constants (author: Tao Yue, state: unchanged) 

  
Идентификаторам, ссылающимся на константы, может быть присвоено только одно значнение в начале программы. Значение, хранящееся в константе, не может быть изменено. 

Константы объявляются в секции констант программы: 
    
    
    const
      Identifier1 = value;
      Identifier2 = value;
      Identifier3 = value;
    

Для примера, давайте объявим несколько констант различных типов данных: строки, символы, целые, вещественные и логические. Эти типы данных будут дополнительно объяснены в следующем разделе. 
    
    
    const
      Name = 'Tao Yue';
      FirstLetter = 'a';
      Year = 1997;
      pi = 3.1415926535897932;
      UsingNCSAMosaic = TRUE;
    

Обратите внимание, что в Pascal символы заключаются в апострофы (')! Это контрастирует с более новыми языками, которые часто используют или разрешают кавычки (") или Heredoc-нотацию. Стандартный Pascal не использует и не разрешает кавычки для обозначения символов или строк. 

Константы полезны для определения значения, которое используется в разных местах вашей программы, но может измениться в будущем. Вместо изменения каждого экземпляра значения, вы можете изменить только определение константы. 

Типизированные константы заставляют константу иметь конкретный тип. Например, 
    
    
    const
      a : real = 12;
    

даст идентификатор **a** , который содержит вещественное значение 12.0 вместо целого 12. 

[ ◄ ](<Identifiers.md> "Basic Pascal Tutorial/Chapter 1/Identifiers/ru") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents/ru") | [ ► ](<Variables_and_Data_Types.md> "Basic Pascal Tutorial/Chapter 1/Variables and Data Types/ru")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Constants/ru](https://web.archive.org/web/20250418104813/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Constants/ru)_
