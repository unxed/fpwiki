# Testers 3.0.4

## Candidates for testing of 3.0.4

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



Report any findings on the [issues](<Issues_3.0.md> "Issues 3.0.4") page 

name and e-mail (account at domain) | CPU - operating system - version/distribution | RC1 testing status (how many percents done, when expected to finish) | RC2 testing status   
---|---|---|---  
First Last, somebody at some.domain | Intel Septium HyperSuperSomething - Linux - SuSE 23.1 | 100% | 95% (unable to finish XYZ due to lack of time)   
karl-michael.schindler@web.de | Intel Core i5 - macOS - 10.12 | binary missing, from sources: make all and all sorts of cross-compilers work, except arm-gba and arm-nds. Testsuite: Total = 7037 (20:7017) |

---

_Source: [https://wiki.freepascal.org/Testers_3.0.4](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/Testers_3.0.4)_
