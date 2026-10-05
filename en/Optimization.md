# Optimization

For an overview of optimization possibilities, see Chapter 11 of the Free Pascal Programmer's Guide. 

Free Pascal allows you to use a set of straightforward compiler directives or commandline arguments to considerably power up your programs. (While running debug builds, you may want to keep most code optimizations off to enjoy faster compilation speeds and avoid rare unintended side effects.) 

  


## Contents

  * 1 Target Processor
  * 2 Target FPU
  * 3 Optimization Switches
  * 4 Optimization in Code
  * 5 Notes



## Target Processor

By default, FPC selects a conservative minimum target for code generation, to maximize compatibility. A higher target processor enables the compiler to use different instructions that would not be available on lower processors. How high you want to set the minimum target processor depends on your target audience. To set this, use "-Cp<CPU>". For example, on the x86 platform, -CpPENTIUMM gives the compiler plenty of room and still covers almost all users. 

The compiler can also produce code generally favoring a particular processor, but without requiring that processor as a hard minimum. To set this, use "-Op<CPU>". 

To check which processors your compiler version supports, run "fpc -ic". 

  


## Target FPU

If your 32-bit program uses a lot of single/double type variables, you may gain a significant speed boost from enabling SSE instructions. "-CfSSE" will speed single operations, and "-CfSSE2" will speed single and double operations. On the amd64 platform both are probably already on by default. 

To check which FPU instruction sets your compiler version supports, run "fpc -if". 

  


## Optimization Switches

You can enable general optimization groups using "-O1", "-O2", "-O3", and "-O4". Individual switches can be enabled with "-Oo<switch>". 

As of 13 July 2020, the optimization groups (defined in **/compiler/ <arch>/cpuinfo.pas**, which references **/compiler/globtype.pas**) are: 

-O1: PEEPHOLE 

-O2: O1 + REMOVEEMPTYPROCS + UNUSEDPARA + REGVAR + STACKFRAME + TAILREC + CSE 

-O3: O2 + CONSTPROP + DFA + USELOADMODIFYSTORE + LOOPUNROLL 

-O4: O3 + ORDERFIELDS + DEADVALUES + FASTMATH + USEEBP/USERBP 

Ungrouped: UNCERTAIN, SIZE, STRENGTH, SCHEDULE, AUTOINLINE, DEADSTORE, FORCENOSTACKFRAME 

**WARNING:** In FPC 3.2.0, [the -O3 and -O4 options produce compilation errors on Windows](<https://bugs.freepascal.org/view.php?id=37305>) in some cases. Furthermore, the ungrouped and -O4 group have potential side effects and may break your code, so use responsibly. 

**WARNING:** In FPC 3.2.0 and 3.2.2, [dead store optimization is known to produce bad code](<https://bugs.freepascal.org/view.php?id=38698>) in some cases. Avoid using it for now. 

To check which switches your compiler version supports, run "fpc -io". 

  


## Optimization in Code

You can control what optimization happens in your code, eg {$optimization noloopunroll}. The following are defined in compiler/globtypes.pas 

LEVEL1, LEVEL2, LEVEL3, LEVEL4, REGVAR, UNCERTAIN, SIZE, STACKFRAME, PEEPHOLE, LOOPUNROLL, TAILREC, CSE, DFA, STRENGTH, SCHEDULE, AUTOINLINE, USEEBP, USERBP, ORDERFIELDS, FASTMATH, DEADVALUES, REMOVEEMPTYPROCS, CONSTPROP, DEADSTORE, FORCENOSTACKFRAME, USELOADMODIFYSTORE, UNUSEDPARA 

Putting 'NO' in front of any of those options has the opposite effect. 

  


## Notes

To take optimization a step further, see [Whole Program Optimization](<Whole_Program_Optimization.md> "Whole Program Optimization"). 

If you are interested in optimizing for size rather than speed, see [Size Matters](<Size_Matters.md> "Size Matters"). 

[Vectorization](<Vectorization.md> "Vectorization") is a powerful feature, but still a work in progress. You can already use SIMD instructions in inline assembly.

---

_Source: [https://wiki.freepascal.org/Optimization](https://web.archive.org/web/20250122050938/https://wiki.freepascal.org/Optimization)_
