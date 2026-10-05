# Programming Assignment 4

│ **English (en)** │

[ ◄ ](<Forward_Referencing.md> "Forward Referencing") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<Solution_4.md> "Solution 4")  
---|---|---  
  
4G - Programming Assignment (author: Tao Yue, state: unchanged) 

A classic recursion problem, taught in all introductory Computer Science courses, is the Towers of Hanoi. In this problem, you have three vertical pegs. There is a cone-shaped tower on the leftmost peg, consisting of a series of donut-shaped discs. For example, this is what a four-story tower looks like: 
    
    
       |       |       |
       |       |       |
       *       |       |
      ***      |       |
     *****     |       |
    *******    |       |
    

The pegs are designated 1, 2, and 3 from left to right. The challenge is to move a tower (any height) from peg 1 to peg 3. In the process, no large disc may be placed on top of a smaller disc, and only one disc (the topmost disc on a peg) may be moved at any one time. 

The problem seems trivial, and it is for one or two discs. For one disc, you simply move it from peg 1 to peg 3. For two discs, move the topmost disc from peg 1 to peg 2, then 1 to 3, and finally move the smaller disc from 2 to 3. 

The problem gets harder for three or more discs. For three discs, you'd move 1 to 3, then 1 to 2, then 3 to 2. This effectively creates a two-story tower on peg 2. Then move the largest disc: 1 to 3. Now move the two-story tower on top of the large disc: 2 to 1, 2 to 3, 1 to 3. 

Your mission, should you choose to accept it -- write a program using a recursive procedure to solve the Towers of Hanoi for any number of discs. First ask the user for the height of the original tower. Then, print out step-by-step instructions for moving individual discs from one peg to another. For example, a three-disc problem should produce the following output: 
    
    
    1 to 3
    1 to 2
    3 to 2
    1 to 3
    2 to 1
    2 to 3
    1 to 3
    

As stated in the section on [recursion](<Recursion.md> "Recursion"), recursion is one of the more difficult topics to grasp. Some people will look at this problem and find it extremely easy. Others will have a difficult time with it. However, once you get past the hurdle of understanding recursion, the actual coding of the program is relatively simple. 

So, if you'd like to challenge yourself, stop reading right here. If you have a little trouble, keep reading for a small _hint_. 

  
  
  


* * *

_Hint_ : the problem, like all recursive problems, reduces itself, becoming simpler with each step. Remember the three-disc problem? You first create a two-disc tower on peg 2, which allows you to move the bottommost disc on peg 1 to peg 3. Then you move the two-disc tower on top of peg 3. 

It's the same with four discs. First create a three-disc tower on peg 2, then move the biggest disc over to peg 3 and move the three-disc tower to peg 3. How do you create the three-disc tower? Simple. We already know how to move a three-disc tower from peg 1 to peg 3. This time, you're just moving from peg 1 to peg 2, then when the biggest peg is in place, you're moving the tower from peg 2 to peg 3. In this whole procedure, we can act as though the big disc doesn't exist, since it's guaranteed to be bigger than the others and thus poses no problem. Just utilize the three-disc solution, switching the numbers around. 

Good luck! 

[ ◄ ](<Forward_Referencing.md> "Forward Referencing") | [ ▲ ](<Contents.md> "Contents") | [ ► ](<Solution_4.md> "Solution 4")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Programming_Assignment_4](https://web.archive.org/web/20211215162711/https://wiki.freepascal.org/Programming_Assignment_4)_
