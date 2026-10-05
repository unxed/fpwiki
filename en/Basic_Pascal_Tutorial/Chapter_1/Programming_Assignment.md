# Basic Pascal Tutorial/Chapter 1/Programming Assignment

│ **English (en)** │  **[русский (ru)](<../../../ru/Basic_Pascal_Tutorial/Chapter_1/Programming_Assignment.md>)** │

[ ◄ ](<Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution")  
---|---|---  
  
1H - Programming Assignment (author: Tao Yue, state: changed) 

Now you know how to use variables and change their value. Ready for your first programming assignment? 

But there's one small problem: you haven't yet learned how to display data to the screen! How are you going to know whether or not the program works if all that information is still stored in memory and not displayed on the screen? 

So, to get you started, here's a snippet from the next few lessons. To display data, use: 
    
    
    writeln (argument_list);
    

The argument list is composed of either strings or variable names separated by commas. An example is: 
    
    
    writeln ('Sum = ', sum);
    

Here's the programming assignment for Chapter 1: 

Find the sum and average of five integers. The sum should be an integer, and the average should be real. The five numbers are: 45, 7, 68, 2, and 34. 

Use a constant to signify the number of integers handled by the program, i.e. define a constant as having the value 5. 

Then print it all out! The output should look something like this: 
    
    
    Number of integers = 5
    Number 1 = 45
    Number 2 = 7
    Number 3 = 68
    Number 4 = 2
    Number 5 = 34
    Sum = 156
    Average = 3.1200000000E+01
    

As you can see, the default output method for real numbers is scientific notation. Chapter 2 will explain you how to format it to fixed-point decimal. 

To see one possible solution of the assignment, go to the next page. 

[ ◄ ](<Punctuation_and_Indentation.md> "Basic Pascal Tutorial/Chapter 1/Punctuation and Indentation") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<Solution.md> "Basic Pascal Tutorial/Chapter 1/Solution")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Programming_Assignment](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_1/Programming_Assignment)_
