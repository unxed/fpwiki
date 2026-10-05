# Lazarus Season of Code

The Lazarus Season of Code (LSoC) is a project designed to attract students and developers to help developing FPC/Lazarus as well as to implement some tasks which are strategically important, but which would remain unimplemented due to lack of developers. Some tasks will be available only for students, while other tasks may be open as bounties, for anyone to participate. We lack enough money to pay for the full value of the development, so the payment should be considered more like a schoolarship, or a costs help. 

All information in this page is preliminary and subject to change or cancelation at any moment. 

## Contents

  * 1 Basic concepts
  * 2 Timeline and Rules
  * 3 List of ideas for Tasks
  * 4 List of Open Tasks
  * 5 Subscription for Students
  * 6 List of tasks being executed
  * 7 Project Manager



## Basic concepts

  * Task - Each task should be substantial, but also not too large, and preferably something which has been stuck for some time without anyone fixing it, and which is of strategic importance for Lazarus/FPC to be a viable software development solution for most Desktop platforms, most PDA platforms, and the Web.
  * Mentor - someone who would guide the implementor(s) and verify that the solution is of good quality and fixes the problem. There is no budget to pay mentors.
  * Student - A student who could finish the required task in a reasonable time period (let's say 3 months). He/she will be awarded the value of the task on successful completion.
  * Coder - Another idea would be offering tasks not only to students but to other people who are interested. This idea is still being considered.



Rules: 

  * All work on a task needs to be very well documented
  * The typical maximum time for completion of a task will be 3 months, but aim to finish in less than 3 months. Tasks not completed in time will most probably not be paid
  * Write the code with good identation, preferably Borland Style or the style of the surrounding code.
  * All code needs to be well commented



## Timeline and Rules

Initial timeframe for 2010 - Winter / Summer (Northern / Southern Hemisphere): 

  * November: 1st until 15th - Subscriptions
  * November: 3rd week - Subscription Review
  * December, January, February - Implementation, Documentation, Validation and Payment



Initial timeline for 2011 - Summer / Winter (Northern / Southern Hemisphere): 

  * June: 1st until 15th - Subscriptions
  * June: 3rd week - Subscription Review
  * July, August, September - Implementation, Documentation, Validation and Payment



## List of ideas for Tasks

This list is a collection of tasks which may be added to a particular LSoC depending on the priority of the task, available money, available mentors, available students, etc. 

#  | Task  | Platforms  | Skills  | Mentor  | Value  | Comments   
---|---|---|---|---|---|---  
1 | Implement a cross-platform API to support complex input methods (japanese, chinese) in the LCL | Win32/64, Gtk2, Qt4, Carbon | ? | Dmitry | EUR100 | \-   
2 | Start a Java port of Free Pascal, generating assembler for Jasmin. The compiler should support units, procedures, records, arrays and all numeric data types. The RTL should support WriteLn and ReadLn. | Win32 | ? | Felipe Monteiro de Carvalho | EUR200 | Strategic importance in the long run and testing the viability of such a port   
5 | XML Syntax Checker and fix most XML bugs. See: [[1]](<http://bugs.freepascal.org/view.php?id=16671>) [[2]](<http://bugs.freepascal.org/view.php?id=14695>) | N/A | ? | Mattias? | EUR100 | \-   
6 | Create a table in the wiki mapping all JIL cross-platform PDA APIs to the appropriate API in various PDA operating systems. Also implement about 10% of JIL in the PDA platforms supported by Lazarus/FPC | Windows CE, Android, MaeMo, iPhone | ? | Felipe Monteiro de Carvalho | EUR100 | \-   
8 | Fix most bugs regarding documentation display and lhelp. | N/A | ? | ? | EUR100 | \-   
9 | Create a tool attached to the Lazarus IDE to generate UML class diagrams from the source code. | N/A | ? | Mattias | EUR100 | \-   
10 | Implement the new Unicode String Type and the RTL Unicode Support. | N/A | ? | ? | EUR100 for part 1 | \-   
11 | Create and integrate internet package database as server web pages and as tool for direct access and management from IDE. | N/A | ? | ? | ? | Good inspiration is <http://www.torry.net/>.   
12 | Create/Fix an fully automated Test-suite (including some tests) for the LCL (and/or IDE). There already is some stuff, using the examples, and a few dedicated tests. But it can't be run via make, nor automatically uploaded to a result db. Also create a reasonable amount of tests for a selection of LCL (or IDE) features | N/A | ? | ? | ? | See fpc (compiler, rtl, packages) test suite. Uploading fpcunit results to a database is possible via [[3]](<https://bitbucket.org/reiniero/testdbwriter>) (MIT license, willing to dual license).   
  
## List of Open Tasks

None at the moment 

## Subscription for Students

Not open at the moment 

## List of tasks being executed

#  | Task  | Platforms  | Skills  | Mentor  | Coder  | Value  | Comments   
---|---|---|---|---|---|---|---  
[3](<Lazarus_Season_of_Code/Task_3.md> "Lazarus Season of Code/Task 3") | Finish the MIPS 32-bits Little Endian port of Free Pascal. The target operating system is Debian Linux. | MIPS/Linux | ? | ? | ? | ? | \-   
[4](<Lazarus_Season_of_Code/Task_4.md> "Lazarus Season of Code/Task 4") | Write a Pascal tutorial focused on Lazarus/FPC covering the most important topics: streams, file handling, cross-platform tips, data types, records, arrays, links to the most important rtl/fcl functions, sysutils, etc... | N/A | ? | Mattias | EUR100 | \-   
[7](<Lazarus_Season_of_Code/Task_7.md> "Lazarus Season of Code/Task 7") | Fix database bugs. | N/A | ? | Michael van Canneyt | JiXian Yang | EUR150 for run 1 | \-   
[14](<Android_Interface.md> "Android Interface") | Help creating the Android interface for Lazarus | - | ? | Felipe Monteiro de Carvalho | JiXian Yang | ? | \-   
  
## Project Manager

Felipe Monteiro de Carvalho

---

_Source: [https://wiki.freepascal.org/Lazarus_Season_of_Code](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Lazarus_Season_of_Code)_
