# Forum

│ **English (en)** │

The [Lazarus and Free Pascal forum](<https://forum.lazarus.freepascal.org>), which acted for a while as the welcome page for the Lazarus project, is a common place to ask questions and the best place for discussions. It's a more user friendly way of communication and might be preferred by beginning users than the mailing lists. 

Its web address is: <https://forum.lazarus.freepascal.org>

While the forum is usually a rather friendly place, you'll have a better experience if you follow the following guidelines, incomplete as they are. 

## Contents

  * 1 How to ask for help effectively
  * 2 Use code tags
  * 3 Sharing large pieces of code
    * 3.1 What to zip
  * 4 Share the Outcome
  * 5 Mark a thread as resolved



## How to ask for help effectively

These are just a few tips on how to ask for help in a way which will help others to help you in the most effective way. These tips should save you, and the other forum users who try to help you, time in arriving at an answer to your question or solution to your problem. 

  * Please **do not** add your unrelated question or unrelated problem to someone else's existing forum thread. Instead, start your own new topic thread with a new post. This will help both you with your question or problem _and_ the original topic poster with their question or problem.


  * Please try to put your question or problem in the forum that is most relevant to your question or problem. While this may appear obvious, anyone who has used forums knows that a lot of questions end up in the wrong place. As a result, users with expertise or knowledge of the particular topic may never see the post and will therefore not be able to help you.


  * Please think carefully when choosing a title for your forum post. It should briefly and accurately describes your problem. A title like "Question ?!!!!", "Can you help me" or "Need a little help please" (these are actual forum post titles) may be ignored. Almost any problem can be given a relevant title with a few key words that will attract the interest of somebody who is familiar with the subject. A corollary to this is to avoid using ALL CAPITAL LETTERS or a lot of exclamation points. A forum post title of "HELP!!!" is not likely to get you any help.


  * In your forum post, please briefly describe your question or the problem you have in a paragraph or two. Try to leave out any unnecessary details. Save everybody time by mentioning any solutions that you may have tried but which did not work.


  * In your forum post, please describe the relevant system details. For example, it is essential to provide: 
    * the name of your operating system
    * the version of your operating system
    * the version of your Free Pascal Compiler
    * the version of your Lazarus IDE
    * the version of any Lazarus IDE add-on components or libraries
    * all error messages that were displayed
    * any additional details that you may be asked to provide.


  * Please describe what you were doing when you encountered the problem. If the problem is reproducible, please list the steps that another forum user can follow to reproduce the problem.


  * Please be courteous and thank those who help you whether or not you get a solution. Somebody is giving you their time and expertise for free. You may want to return to the forum again, so it pays to be friendly and courteous to those who help you or try to help you.


  * If a suggested solution works, please return to the forum and report your success (see share the outcome below). This is the least you can do in exchange for the help you have received. It is also likely to make you welcome in the forum the next time you go there for help.



## Use code tags

When asking for help it is particularly important to format your code so it is clearly shown as code. Most programmers are used to see code formated "as code" and resent (even if mildly) having to deal with it badly- or un-formated, which may naturally reflect both in their willingness to answer your question and the tone of their answers. 

The forum treats any code you paste (whether a big or small chunk) as normal text. Default text formatting will be proportionally spaced (not fixed-width) and indentation and alignment of successive lines will be unreliable, making the code hard to read. [![forum1.PNG](https://wiki.freepascal.org/images/8/8c/forum1.PNG)](</File:forum1.PNG>)

Also, parts of your pasted code might be considered as mark up by the forum engine, causing an undesirable look. 

A code snippet such as this: 

[![forum2.PNG](https://wiki.freepascal.org/images/9/97/forum2.PNG)](</File:forum2.PNG>)

...ends up looking like this: 

[![forum3.PNG](https://wiki.freepascal.org/images/9/9e/forum3.PNG)](</File:forum3.PNG>)

As a result, instead of actually getting help, you might only get "use code tags!" responses. Using code tags is easy. All you need to do is add 
    
    
    [code=pascal]
    

at the start of your code and add 
    
    
    [/code]
    

at the end of your code. 

A shortcut for this is to click the # (code) button when writing your thread text: 

[![forum8.png](https://wiki.freepascal.org/images/c/cd/forum8.png)](</File:forum8.png>)

  
If you have already made a post without remembering to insert [code=pascal] and [/code] tags, you can "MODIFY" your post and add them later. Just like this: 

[![forum4.PNG](https://wiki.freepascal.org/images/7/7a/forum4.PNG)](</File:forum4.PNG>)

[![forum9.png](https://wiki.freepascal.org/images/4/49/forum9.png)](</File:forum9.png>)

[![forum6.PNG](https://wiki.freepascal.org/images/d/d7/forum6.PNG)](</File:forum6.PNG>)

Once the changes are made and you hit "Save", you should see a nice Code section appearing in your post. The code section shows your code using a fixed-width font. This is placed within a smaller box with a "Select" button that allows you to select just that code section quickly. Your code will not then be misinterpreted as forum markup. It will as will be displayed as the code it is. 

[![forum7.PNG](https://wiki.freepascal.org/images/a/a5/forum7.PNG)](</File:forum7.PNG>)

Additionally, you can HIGHLIGHT particular lines of code by using an opening code tag like **[code=pascal,2,4]** which will highlight lines 2 and 4 as below: 

[![forum10.png](https://wiki.freepascal.org/images/2/2f/forum10.png)](</File:forum10.png>)

Most importantly, taking the trouble to use code tags is one way to support on-line politeness. 

Note that the code tag supports more than just Pascal, in case you need to add snippets in other languages (C, C++, Java, Assembler, etc). 

You can find a list of supported languages in [this page at MediaWiki](<https://www.mediawiki.org/wiki/Extension:SyntaxHighlight#Supported_languages>). You'll have to scroll down a little and expand the list of "Previously supported lexers" (with GeSHi), which is what the forum software supports. 

## Sharing large pieces of code

While short snippets of code can be inserted on your post as explained above (by using the [code] tag), sometimes you'll need to share more of your code, up to a complete example project, if you want to get meaningful help. 

As is easy to see, dumping all this code into a single post is a bad idea and you might run afoul of a post text limits (around 20K characters). What you should do instead is zip it and add it to your post as an attachment. 

### What to zip

Don't simply zip all the files in your project's directory: If you do that you'll add lots of files, some of them very large, which add nothing to the comprehension of your code. 

The rule of thumb is: add the source files and, if any, essential data files. 

"Source" files for a normal project are: *.LPI, *.LPR, *.LFM, *.PAS (or *.PP) and *.INC; i.e. what we can term "real" source, the project info file and the forms. A simple way to prepare these files is to use "Publish project", in the "Project" menu of Lazarus; if you check the option "Compress" in the "Publish project" dialog you'll end up with a nice .zip file, ideal to attach to your post. 

Add data files only if they are essential either for the question asked at the forum or to be able to run your program at all. Most times these files will be database files, test documents in some specific format, configuration files of some kind or icons/graphics loaded at run-time by your program. Try to make them as small as possible consistent with the use for which they are intended. 

Don't try to add a multi-megabyte database, because the forum limits attachments to 250K. If a huge file is really needed, use a "locker" service like Google Drive or similar. 

## Share the Outcome

The Forum is a good place to get help with Object Pascal and Lazarus. If you run into trouble and cannot figure out a solution by yourself even after Googling for answers to similar problems, the forum is the best place to ask. 

You will usually find there are many experienced people reading the Forum who will help you. 

But, as soon as you get an answer which you find resolves your problem, please let everyone else know that the proposed solution worked for you. 

It's very easy to get overexcited, once things start working again, and you might omit to tell everyone else. Please DON'T forget, because we'd like to know, and anyone else who later runs into the same issue, would also like to know that the solution proposed is a good one. 

The same applies, if you've created a thread, where a working solution has not yet been given and you find a solution (by yourself, or somewhere else). Please DO NOT LEAVE the thread you've started orphaned. Just return to the thread and briefly indicate the solution you found. 

## Mark a thread as resolved

The easiest way to indicate that your problem is solved is to add "[SOLVED]" to the thread subject. Provided you're the original thread's author it is easy to do. 

  * Go to the very first post of the thread
  * Modify the first message
  * You should see the "Subject" of the message. The subject defines the subject of the thread.
  * Simply add [SOLVED] to the beginning of the subject (leave the rest of the subject as it is).
  * Hit Save.



This procedure can also be used to fix typos in the thread subject if you later notice it has spelling mistakes.

---

_Source: [https://wiki.freepascal.org/Forum](https://web.archive.org/web/20250126183110/https://wiki.freepascal.org/Forum)_
