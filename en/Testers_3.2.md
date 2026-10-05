# Testers 3.2.2

Anybody willing to contribute FPC and help to increase release quality by participating in systematic testing of release candidates and short checking of the final release. Note that this testing differs at least partly from just using the particular build for your own purposes, because you are supposed to perform a minimum set of defined operations, let us know when the testing is finished and report your results in certain time period (usually not more than one week - exact dates are provided when the builds are available). 

To test a release at least do the following. If some part is not available for the specific target you can skip it offcourse 
    
    
       Install the release (check that the version-numbers are correct)
       make sure readme.txt & whatsnew.txt are for the current version
       Read updated text files as distributed in release zip files
           readme.txt
           faq.txt
           whatsnew.txt
       run all distributed executables (in bin/*)
       open the installed hello.pp in IDE
       make a minor change in the demo in IDE & save it
       compile the demo file in IDE
       run the demo within the IDE (debugger)
       view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
       make cycle with newly installed binaries and sources
       run testsuite
       Check PDF documentation (open all files)
       Check HTML documentation, if any
       Check TXT documentation, if any
       Check CHM documentation, if any
    

Instructions for the make cycle and testsuite are on the [How to start](<How_to_start.md> "How to start") page. Compare testsuite results against results from the previous published release; any new failures are regressions that should be reported. 

Report any findings on the [issues](<Issues_3.2.md> "Issues 3.2.2") page name and e-mail (account at domain) CPU - operating system - version/distribution RC1 testing status (how many percents done, when expected to finish) RC2 testing status First Last, somebody at some.domain Intel Septium HyperSuperSomething - Linux - SuSE 23.1 100% 95% (unable to finish XYZ due to lack of time) 

## Candidates for testing of 3.2.2

Anybody willing to contribute FPC and help to increase release quality by participating in systematic testing of release candidates and short checking of the final release. Note that this testing differs at least partly from just using the particular build for your own purposes, because you are supposed to perform a minimum set of defined operations, let us know when the testing is finished and report your results in certain time period (usually not more than one week - exact dates are provided when the builds are available). 

To test a release at least do the following. If some part is not available for the specific target you can skip it offcourse 

  1. Install the release (check that the version-numbers are correct)
  2. make sure readme.txt & whatsnew.txt are for the current version
  3. Read updated text files as distributed in release zip files 
     1. readme.txt
     2. faq.txt
     3. whatsnew.txt
  4. run all distributed executables (in bin/*)
  5. open the installed hello.pp in IDE
  6. make a minor change in the demo in IDE & save it
  7. compile the demo file in IDE
  8. run the demo within the IDE (debugger)
  9. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
  10. make cycle with newly installed binaries and sources
  11. run testsuite
  12. Check PDF documentation (open all files)
  13. Check HTML documentation, if any
  14. Check TXT documentation, if any
  15. Check CHM documentation, if any



Report any findings on the [Issues 3.2.2](<Issues_3.2.md> "Issues 3.2.2") page. 

name and e-mail (account at domain) | CPU - operating system - version/distribution | RC1 testing status (how many percents done, when expected to finish) | RC2 testing status   
---|---|---|---  
First Last, somebody at some.domain | Intel Septium HyperSuperSomething - Linux - SuSE 23.1 | 100% | 95% (unable to finish XYZ due to lack of time)   
Karl-Michael.Schindler (at) web.de | MacBook Pro (Core i5) - macOS 11.2.3 | building x86_64-darwin and numerous cross compilers from sources, 100% | to be done   
Kirinn (at) mooncore.eu | Core i3-2328M - Linux - Arch 5.11.11 | 100% |   
Kirinn (at) mooncore.eu | Core i5-4590S - Windows 7 32-bit in QEMU VM | 100% |   
Kirinn (at) mooncore.eu | AMD R5 1500X - Windows 10 Home - 64-bit 20H2 | 100% |

---

_Source: [https://wiki.freepascal.org/Testers_3.2.2](https://web.archive.org/web/20241104214208/https://wiki.freepascal.org/Testers_3.2.2)_
