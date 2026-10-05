# Tips on writing bug reports

## Contents

  * 1 Why you should spend some time on bug reports
  * 2 The four most important things when writing bug reports
    * 2.1 Report only one bug in each report
    * 2.2 Clearly explain what the problem is
    * 2.3 Do not mix the explanation of the problem with solutions, thoughts and other guesses
    * 2.4 Be complete and always include a fully compilable example if possible
  * 3 Other tips
    * 3.1 Give the bug report a good name
    * 3.2 Fill in information about OS, used versions etc
  * 4 See also



## Why you should spend some time on bug reports

First of all, thank you for using Free Pascal and Lazarus, and thank you for your help. 

That is what reporting bugs is: helping the project further. 

As happens with most open-source projects, bug reports submitted to FPC & Lazarus are handled directly by an experienced developer. This is opposed to what happens in many closed-source software: tech support employees are paid to spend a lot of time on the bug report to make it perfect before it goes to a real developer. 

Therefore, please spend some time on your reports so that: 

  * For the project: more bugs are solved each week
  * For the submitters of the bugs: their bugs are fixed sooner
  * For the project's developers: they can do more for the project in the same amount of time



Time spent on making a good bug report is not wasted. On the contrary, it is time invested by you in the open source project you are using. So please take some time to read this carefully and think about these tips when reporting bugs. 

## The four most important things when writing bug reports

The four most important things you have to think of when submitting bugs are: 

  1. **report only one bug in each report** ,
  2. **clearly explain what the problem is** ,
  3. **do not mix the explanation of the problem with solutions, thoughts and other guesses** and finally
  4. **be complete and always include a fully compilable example if possible**



This sounds easy, but when you scroll though the bug reports it soon becomes apparent that it is easy to deviate from these guidelines. 

### Report only one bug in each report

Only one bug per report also means that you should not add comments mentioning other potential bugs. The reason is that the developer cannot close the bug once it is fixed, because he needs a reminder of that remark/other issue. However, each time the bug is scanned that remark has to be identified and checked, which is very time-consuming. 

When two separate but related bugs are submitted, we can however add relations between the bugs in the bug reporting tool so that the context can be easily looked up. Feel free to mention the numbers of related bugs in your comments. 

Obviously, submitting a whole list of bugs in one report is completely out of the question. 

### Clearly explain what the problem is

Explaining the problem is the most difficult part of creating a bug report. Before writing the report down, think it through: what is actually the problem? In general it can only be one of two things: you get an error message or exception you didn't expect, or you get a result you did not expect. Therefore make sure to mention the **result that you expected** as well as describe **the error or the result you actually got**. 

If the result or expected result is missing, realise that you did not describe the problem at all. Also something like "the memory of this variable is freed twice" is not a problem at all! 

The problem is that this may lead to an exception (access violation) or memory corruption, leading to other errors. Or, it can slow something down because the second free is redundant. 

A way to check if what you describe is really the problem, is to add an example. An example to show that memory is freed twice is difficult. A program that raises the exception clearly shows the problem. See below for more information on examples. 

### Do not mix the explanation of the problem with solutions, thoughts and other guesses

Another big problem with writing down what the actual problem is, is that people tend to think that the developers are only interested in the solution of the problem and not the problem itself. Well, without a problem there is no solution so the developers have to guess what the problem is, which is always a bad idea. 

Also a lot of times the solution you kindly thought of is wrong. If the problem is missing this makes the bug report useless. There is also the risk of steering the developer into the wrong direction which can really slow down the problem solving. 

So, as a general rule: **do not add thoughts and guesses to the bug report**. But if you really insist on doing so, do this separately from the explanation of the problem (e.g. in the Additional Information section in the bug tracker). This also applies if you have a [patch](<Creating_A_Patch.md> "Creating A Patch") (which you are of course more than welcome to attach in the bug report). 

  


### Be complete and always include a fully compilable example if possible

Regarding completeness: _write your bug report as if the developers you are writing it for are completely clueless_. 

In fact they are clueless, since if they knew about the problem it probably would have been fixed. So describe the problem as you would explain it to someone who is calling the internet provider helpdesk complaining that 'the internet is broken'. 

Never assume that the developer knows what you mean, so you can omit it. For example, telling that TSomeClass.Method always crashes is not enough. You may think that the developer knows that you have to create the class using TSomeClass.Create, before calling the method. But maybe you missed that there is also a constructor called TSomeClass.CreateNew. What if the crash doesn't happen if you use CreateNew, so that the developer can not reproduce your problem? Explain step by step how you got to the problem, so the developer can reproduce it. 

Adding **compilable examples** (not only snippets/some code listing but a complete program) that are as small as possible is always a good thing. Not only do these help see what the problem is, developers may also adapt these to (regression) test cases that can be run automatically, so it can be checked if the bug ever surfaces again. 

Try to give a complete example that is compilable. When you only give a procedure it could be that we miss that you have added some defines on top of your program. Or that you forgot to add those. So always send in complete examples. 

On the other hand, remove everything from the example that is unnecessary to show your problem. This helps you get a better understanding of what the problem actually is - and helps the developer looking at the bug report to quickly find the problem as well. Maybe removing parts that seemed irrelevant at first even solves your problem. 

If you are reporting database bugs, see [here](<Database_bug_reporting.md> "Database bug reporting") for template programs that will speed up writing your bug report test program. If you are reporting Lazarus bugs, you can use the menu Project>Publish Project to save only your sample project's source code to a separate directory. You can then zip this directory and submit it as an attachment. 

## Other tips

So now you know you have to write one bug report at a time, explain the problem and do not mix it up with guesses and random thoughts. And that the people who are reading bug reports should be considered as being not that smart. Although those were the most important things, below you can find a few more tips. 

### Give the bug report a good name

A good name tells what the problem is. It should contain the following: 

1) In which class/procedure/etc the problem lies 

2) What the problematic behaviour is 

3) What you were doing when the problem happened 

Always mention 1. If you can not get 2 and 3 into a short title, mention only one of them. Do not add anything else. If the three points above are included, that's always enough. 

As mentioned, do not add any ideas of the solution in the title. 

### Fill in information about OS, used versions etc

Always fill in at least which version of Free Pascal and Lazarus you used (these are two different programs, so you have to give two version numbers). If you have used a snapshot, give the exact revision numbers or date of the snapshot. Even if you think that it doesn't matter, report it. This information always matters. Also mention your operating system (eg FreeBSD, Linux, macOS, Windows), its version and the architecture you are using (eg i386, x86_64, PowerPC, ARM). 

## See also

  * [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report") General information on using the bug tracker
  * [Database bug reporting](<Database_bug_reporting.md> "Database bug reporting") Specific info and sample programs for database bugs
  * [Creating A Patch](<Creating_A_Patch.md> "Creating A Patch") If you have modified the source code to implement a solution, this article helps you to add it to your bug report in the most efficient way, so that developers can add it to the main code as fast as possible

---

_Source: [https://wiki.freepascal.org/Tips_on_writing_bug_reports](https://web.archive.org/web/20241209234305/https://wiki.freepascal.org/Tips_on_writing_bug_reports)_
