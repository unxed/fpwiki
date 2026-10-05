# Testers 2.2.4

## Candidates for testing of 2.2.4

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
  13. Check HTML documentation
  14. Check TXT documentation



Report any findings on the [Issues 2.4.2](<Issues_2.4.md> "Issues 2.4.2") page. 

name and e-mail (account at domain) | CPU - operating system - version/distribution | RC1 testing status (how many percents done, when expected to finish) | RC2 testing status   
---|---|---|---  
First Last, somebody at some.domain | Intel Septium HyperSuperSomething - Linux - SuSE 23.1 | 100% | 95% (unable to finish XYZ due to lack of time)   
  
## See also

  * [fppkg field test](<fppkg_field_test.md> "fppkg field test")

---

_Source: [https://wiki.freepascal.org/Testers_2.2.4](https://web.archive.org/web/20230326205555/https://wiki.freepascal.org/Testers_2.2.4)_
