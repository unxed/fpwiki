# FreeBSD specific Release Engineering

## Contents

  * 1 scripts using sh instead of bash
  * 2 install.sh
  * 3 notes from recent packaging
  * 4 See also



## scripts using sh instead of bash

(originally by Andres K. Foerster) 

Current scripts: samplecfg/install.sh/cross* known scripts not affected: makepack (no chance of running on a TARGET computer) 

The script sometimes uses one "=" and sometimes two "==" for comparisions. The /bin/sh of FreeBSD accepts just one "=" in comparisions. See test(1). or sometimes the "x" trick works: if [ "x$something" = "xsomething" ] ... 

"echo -n" is supported in the shell of FreeBSD, but it's not standard. You could use "printf" instead of "echo -n". That seems more standardized. 

Also the tar parameter "--directory" is a extension of GNU-tar. You could use "-C" instead of "--directory", but be careful with that. It seems "-C" is implemented differently. (My FreeBSD still comes with GNU tar, but I've heard that they switch to their own version of tar... I'll have a look at it.) 

That's all as far as I can see. Then you can use "#!/bin/sh". 

(Marco van de Voort:) 

  * Also test using BSD sed and tar from time to time. (some sed constructs are already replaced by awk) 
  * In case of bash use #!/usr/bin/env bash NEVER hardcode the shell path! 



## install.sh

The documentation currently generates the PDF docs with a directory in the .tar.gz, which doesn't play nice with FreeBSD hierarchy. Moreover, to workaround this on Linux, they use gnu tar specific options (--directory). 

So every release the following changes are made: \- the docs are repacked without the directory \- the --directory option is removed from the doc tar x line in the install.sh script 

  


## notes from recent packaging

  * synced all ELF ABI tag versions to make it easier to patch with the i386/identpatch.sh script. 
  * FreeBSD 8.2 bsdtar seems to understand --strip fine. 
  * See sometimes "grab_vcsa" missing error. 
  * debugger of 9.0/i386 gave funny results/output. Constantly gave "errorcode=0 3 hidden steps" msgs. 
  * newer freebsd seems to have a kiconv. Maybe the RTL should use kiconv instead of iconv. If only because the iconv package has a dependency on perl 5. 



## See also

  * [FreeBSD](<FreeBSD.md> "FreeBSD")

---

_Source: [https://wiki.freepascal.org/FreeBSD_specific_Release_Engineering](https://web.archive.org/web/20210421145510/https://wiki.freepascal.org/FreeBSD_specific_Release_Engineering)_
