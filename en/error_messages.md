# error messages

│ **English (en)** │  [**русский (ru)**](<../ru/error_messages.md> "error messages/ru") │    
****

## Contents

  * 1 Errors during compilation
  * 2 Errors during program execution
  * 3 Bugs
    * 3.1 If the bug is already known
    * 3.2 If the bug is not already known
  * 4 See also



## Errors during compilation

Unless the error identifies itself as an internal compiler error, it is unlikely that the error is caused by a bug in the compiler or its runtime library. Syntax errors are almost always due to incorrect code. Refer to the FPC [Parser Messages](<https://www.freepascal.org/docs-html/user/userse62.html>) documentation for a listing of the various Error and Warning messages produced by the FPC Parser along with explanations of them. 

If you encounter an error while compiling your code and are unable to resolve it yourself, please use the FPC and Lazarus [Forums](<https://forum.lazarus.freepascal.org/>), write to the [Lazarus](<https://lists.lazarus-ide.org/listinfo/lazarus>) or [Free Pascal](<https://www.freepascal.org/maillist.html>) mailing list or join the [#fpc](<https://webchat.freenode.net/#fpc>) or [#lazarus-ide](<https://webchat.freenode.net/#lazarus-ide>) IRC channel. The problem may then be solved more quickly. 

If an error is encountered during compilation, the compiler does not generate an executable program. See further [compile time errors](<compile-time_error.md> "compile-time error"). 

## Errors during program execution

The Free Pascal Compiler inserts code to detect a large number of error situations which might occur during program execution (eg divide by zero). If such an error situation occurs, the standard run-time library will terminate the program and print a runtime error number and the address at which the error occurred. See further [runtime errors](<runtime_error.md> "runtime error"). 

If you encounter a runtime error while running your program and are unable to resolve it yourself, please use the FPC and Lazarus [Forums](<https://forum.lazarus.freepascal.org/>), write to the [Lazarus](<https://lists.lazarus-ide.org/listinfo/lazarus>) or [Free Pascal](<https://www.freepascal.org/maillist.html>) mailing list or join the [#fpc](<https://webchat.freenode.net/#fpc>) or [#lazarus-ide](<https://webchat.freenode.net/#lazarus-ide>) IRC channel. 

## Bugs

### If the bug is already known

Use the FPC and Lazarus [Bug Tracker's](<http://bugs.freepascal.org/set_project.php>) search capabilities. 

Known issues. Tip: If you are experiencing problems, for example with TEdit.SelStart -> try searching "SelStart" (in quotes). If the bug is known: 

  * reopen if the issue is resolved or the issue is closed - use the **Reopen Issue** button.
  * add your own note to the discussion if you received this error in another situation.



To observe any changes to your bug report - use the **Monitor Issue** button. 

Note: You need to login to your account: [Login/Create an account](<https://bugs.freepascal.org/login_page.php>). 

### If the bug is not already known

  1. Go to the FPC and Lazarus [Bug Tracker](<http://bugs.freepascal.org/set_project.php>).
  2. You must be logged in: [Login/Create account](<https://bugs.freepascal.org/login_page.php>).
  3. Visit [Report Issue](<http://bugs.freepascal.org/bug_report_advanced_page.php>). Fill in as many fields as possible. The more accurate the better.


  * The **OS and Product Version** fields are especially important. If the data is not enough, they will not help you! Don't forget to mention the system features (big endian or 64-bit).
  * It is often helpful to send in a small test program to resolve the problem as soon as possible.
  * If you find graphic artifacts, it would not be superfluous to send a screenshot (in **png** or **jpeg** , but **not** in BMP!).
  * If it fails, try creating a backtrace. More information - [Creating a Backtrace with GDB](<Creating_a_Backtrace_with_GDB.md> "Creating a Backtrace with GDB").
  * If possible, observe the behaviour of the buggy program on different platforms or with different widgetsets.



It is also possible to get a bug fixed by paying for a solution, see [Bounties](<Bounties.md> "Bounties"). 

## See also

  * [Lazarus Help](<Lazarus_Help.md> "Lazarus Help").
  * [How to use the Forums](<Forum.md> "Forum").
  * [How do I create a bug report](<How_do_I_create_a_bug_report.md> "How do I create a bug report").
  * [Creating A Patch](<Creating_A_Patch.md> "Creating A Patch").
  * [Tips on writing bug reports](<Tips_on_writing_bug_reports.md> "Tips on writing bug reports").

---

_Source: [https://wiki.freepascal.org/error_messages](https://web.archive.org/web/20220130035912/https://wiki.freepascal.org/error_messages)_
