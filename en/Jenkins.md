# Jenkins

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Jenkins set up
  * 3 Resource compiler setup
  * 4 Database driver setup
  * 5 Setting up jobs
  * 6 Background: the regular build & problems
    * 6.1 Stable FPC setup
    * 6.2 Jenkins build commands
    * 6.3 A problem: /etc/fpc.cfg
    * 6.4 A problem: make clean
  * 7 Solution: getting FPC using fpcup
  * 8 FPCUnit/DBTestframework tests with database output
    * 8.1 Database setup
      * 8.1.1 Tested databases
      * 8.1.2 Test results database
    * 8.2 Adding the tests to your Jenkins job
  * 9 Windows/Linux... build slave
  * 10 Enhancements
  * 11 Troubleshooting
    * 11.1 Build fails
    * 11.2 Find out which user Jenkins runs under
  * 12 See also
  * 13 External links



## Overview

Jenkins is a continuous integration server that can be used to automatically build and test projects based on commits to your source control system. 

This article describes how to set up Jenkins to automatically build FPC when a commit happens in SVN. You can of course adapt these instructions to your own projects. 

## Jenkins set up

A possible version to use is Turnkey Linux Jenkins on Debian 6.0.5 (Squeeze); this article tested with Jenkins 1.464) 

Whichever way you install Jenkins, make sure the SVN plugin is enabled 

## Resource compiler setup

At least the Debian FPC packages don't install a [resource compiler](<Lazarus_Resources.md> "Lazarus Resources"), which is needed in sections below: 
    
    
    # Get resource compiler, not provided by fpc 2.6 packages:
    apt-get install mingw32-binutils
    # symlink:
    ln -s /usr/bin/i586-mingw32msvc-windres /usr/bin/windres
    

## Database driver setup

For testing database connections/writing results to databases, make sure the relevant operating system database drivers/client connection libraries are installed, e.g.: 
    
    
    apt-get install libfbembed2.5 firebird2.5-super firebird2.5-dev \
    postgresql-client libpq-dev \
    freetds-bin freetds-dev \
    mysql-client libmysqlclient-dev \
    libsqlite3-0 libsqlite3-dev \
    unixodbc unixodbc-dev tdsodbc #adjust to taste/distribution
    # we install the -dev packages so we get the correct driver names for older FPC versions (e.g. libfbclient.so.2.5 instead of libfbclient). Alternatively: symlink these or specify explicit .so names in any code using databases
    #freetds should pull in unixodbc, but we specify it explicitly.
    #Strangely, on Debian, we need to explicilty specify tdsodbc to get the actual FreeTDS ODBC driver!?
    

## Setting up jobs

Use the job screen to set the svn source to the FPC trunk or branch you want to use (e.g. <http://svn.freepascal.org/svn/fpc/trunk>) 

Set 'use svn update as much as possible' 

Polling every 5 minutes could be a good setup. Alternatively, you can set up your own SVN server and write a commit hook script for that. This would result in this entry: 
    
    
    # every 20 minutes
    */20 * * * *
    

## Background: the regular build & problems

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** This section is included for completeness; problems with system wide fpc.cfg and current FPC trunk (October 2012) may make using fpcup much easier. See below for that.

### Stable FPC setup

Install binutils+resource compiler+stable FPC - for some reason the debian packages don't pull in the required dependencies: 
    
    
    apt-get install build-essential mingw32-binutils
    # now make an easier name for the resource compiler; adjust to your specific version
    # you could also adjust -FC<resourcecompilername> in /etc/fpc.cfg
    ln -s /usr/bin/i586-mingw32msvc-windres /usr/bin/windres
    
    #fpc would get old FPC 2.4 on this Debian Squeeze=> symptom in Jenkins build log: 
    #generic.inc(2505,13) Fatal: Internal error 8
    #Fatal: Compilation aborted
    #make[7]: *** [system.ppu] Error 1
    
    #get stable compiler (2.6 at time of writing); we're on a 32 bit system:
    cd /root
    wget http://sourceforge.net/projects/freepascal/files/Linux/2.6.0/deb/fp-compiler-2.6.0_2.6.0-0_i386.deb/download
    mv download fp-compiler-2.6.0_2.6.0-0_i386.deb
    wget http://sourceforge.net/projects/freepascal/files/Linux/2.6.0/deb/fp-compiler_2.6.0-0_i386.deb/download
    mv download fp-compiler_2.6.0-0_i386.deb
    wget http://sourceforge.net/projects/freepascal/files/Linux/2.6.0/deb/fp-units-rtl_2.6.0-0_i386.deb/download
    mv download fp-units-rtl_2.6.0-0_i386.deb
    wget http://sourceforge.net/projects/freepascal/files/Linux/2.6.0/deb/fp-units-rtl-2.6.0_2.6.0-0_i386.deb/download
    mv download fp-units-rtl-2.6.0_2.6.0-0_i386.deb
    wget http://sourceforge.net/projects/freepascal/files/Linux/2.6.0/deb/fp-units-i386-2.6.0_2.6.0-0_i386.deb/download
    mv download fp-units-i386-2.6.0_2.6.0-0_i386.deb
    wget http://sourceforge.net/projects/freepascal/files/Linux/2.6.0/deb/fp-units-i386_2.6.0-0_i386.deb/download
    mv download fp-units-i386_2.6.0-0_i386.deb
    wget http://sourceforge.net/projects/freepascal/files/Linux/2.6.0/deb/fp-utils-2.6.0_2.6.0-0_i386.deb/download
    mv download fp-utils-2.6.0_2.6.0-0_i386.deb
    wget http://sourceforge.net/projects/freepascal/files/Linux/2.6.0/deb/fp-utils_2.6.0-0_i386.deb/download
    mv download fp-utils_2.6.0-0_i386.deb
    
    dpkg -i fp-units-rtl-2.6.0_2.6.0-0_i386.deb
    dpkg -i fp-units-rtl_2.6.0-0_i386.deb
    dpkg -i fp-compiler-2.6.0_2.6.0-0_i386.deb
    dpkg -i fp-utils-2.6.0_2.6.0-0_i386.deb
    #no idea what the other .debs are for that we downloaded
    #todo: this could be trimmed down a bit, but this seems to work
    

  


### Jenkins build commands

As build commands in Jenkins, you could use the shell commnds 
    
    
    make all
    

and, to install inside the "workspace" or top of the build directory environment: 
    
    
    make install PREFIX=$WORKSPACE
    

Note: installing somewhere else may be a good idea, please refer to the Build FAQ. 

Make install does not generate an fpc.cfg for you; you have to do that yourself: 
    
    
    $WORKSPACE/bin/fpcmkcfg -o $WORKSPACE/bin/fpc.cfg
    

### A problem: /etc/fpc.cfg

The stable we compiled using packages works, but puts an fpc.cfg into the /etc directory. This fpc.cfg is picked up by default and there is no way to disable that unless you e.g. call a script fpc.sh as your compiler instead of the fpc executable. This script passes options to the real compiler so it ignores /etc/fpc.cfg 

This will give problems with running e.g. the compiler test suite. These problems may be solved by: 

  * use an PPC_CONFIG_PATH environment variable or fpc.cfg in home dir, which may work.
  * perhaps a simpler solution is just not to install FPC/FPC sources, but only download a bootstrap compiler. Therefore no fpc.cfg, and no problem. Bonus is that we build it closer to the "official way". (which is a full install of the previous release)



To work around is, you can also use fpcup; see the next section. 

### A problem: make clean

Current (October 2012) FPC trunk does not clean all relevant files when running make clean/make distclean. 

## Solution: getting FPC using fpcup

The [fpcup](<Projects_using_Lazarus.md> "Projects using Lazarus") tool can download and install FPC and Lazarus from scratch (no starting compiler needed), so a solution could be: 

  * get fpcup into your workspace
  * change build steps to call fpcup with --only=fpc --fpcrevision=$SVN_REVISION



We could have run fpcup so it downloads the latest SVN version, but then we lose the integration with subversion. 

Make sure you have a resource compiler (see installation section above). Download fpcup (bitness should match your Jenkins machine). Put e.g. fpcup_linux_x86 in your /usr/local/bin directory, then: 
    
    
    chmod ugo+rx /usr/local/bin/fpcpup_linux_x86
    

(If using an x64 Linux, naming will obviously differ) 

Then set your build steps like this. Note: we could have used one big shell script, but the advantage of putting the commands in execute shell commands is that the build will stop on the first error (non-zero exit status) encountered; you'd have to build that error detection into a script. 
    
    
    rm --force $WORKSPACE/fpcup.log #remove any existing logs; use force to not generate error message if log does not exist
    

Download/install using fpcup. Note it gets the SVN URL and desired revision ID from environment variables set by the Jenkins SVN plugin. Once again, adjust if you don't use x86 Linux. 
    
    
    /usr/local/bin/fpcup_linux_x86 --fpcbootstrapdir=$WORKSPACE/fpcbootstrap --fpcdir=$WORKSPACE --fpcuplinkname= --fpcURL=$SVN_URL --fpcrevision=$SVN_REVISION --keeplocalchanges --lazdir=$WORKSPACE/lazarus --lazlinkname= --logfilename=$WORKSPACE/fpcup.log --noconfirm --primary-config-path=$WORKSPACE/lazarusconfig --only=fpc
    

Now check the fpc output in another build step. Note that fpcup uses a custom fpc.sh script to avoid problems with systemwide fpc.cfg. Once again, adjust if not on Linux x86. 
    
    
    $WORKSPACE/bin/i386-linux/fpc.sh -iD; $WORKSPACE/bin/i386-linux/fpc.sh -iW #get FPC version output to check if it was built correctly
    

The last build step runs the "compiler test suite". 
    
    
    cd $WORKSPACE/tests; make all TEST_FPC=$WORKSPACE/bin/i386-linux/fpc.sh #make full doesn't work, can't delete file
    

You could archive the test suite results using a Jenkins post-build step. 

## FPCUnit/DBTestframework tests with database output

If you want to run the [dbtestframework](<Databases.md> "Databases") tests and output the results to a separated database server, you can use the testdbwriter programs. This lets you compare runs from various revisions - which are automatically built by Jenkins. 

This method of outputting test results to databases can also be used by other projects that use fpcunit tests - you'd have to use the testdbwriter listener and adjust the console runner to include that. 

Though this section contains a fair amount of setup, fortunately it is once only; each job will reuse the existing configuration. 

You could of course also run the normal dbtestframework code with output to plain text or XML. 

Check if mercurial/hg is installed: 
    
    
    hg --help
    

if not install it, e.g. 
    
    
    apt-get install mercurial
    

Get the repository version into a fixed directory: 
    
    
    mkdir -p /opt/testdbwriter
    cd /opt
    hg clone https://bitbucket.org/reiniero/testdbwriter #will create /opt/testdbwriter
    cd /opt/testdbwriter
    # the following 2 steps are not necessary but show you how you can update with newest changes in the repository if needed:
    hg pull #get newest changes from remote repo
    hg up #adapt local version to newest changes we just downloaded
    

### Database setup

Now the files in this directory can be used to compile the test program with the FPC in each build. We will also need to setup our database connections. 

#### Tested databases

First the databases where the tests will run on. Please create an empty database (or at least one that the test user can thrash) on each db server that you want to test, and have a username and password ready. 

Then edit database.ini, used by the db test framework tests (see [Databases#Running_FPC_database_tests](<Databases.md> "Databases")
    
    
    cd /opt/testdbwriter/dbtests2db
    cp database.ini.txt database.ini #copy over template to config file
    nano database.ini # or another editor.
    # now set up your credentials in each section
    

#### Test results database

Now the database where the results should go to. 

Set up a database with the instructions pointed to in the readme.txt file (e.g. use the _testdbwriter.sql_ for Firebird/Interbase databases and _testdbwriter_postgresql.sql_ for PostgreSQL databases). 

Now edit the ini file: 
    
    
    cd /opt/testdbwriter/dbtests2db
    cp ../testdbwriter.ini.txt testdbwriter.ini #copy over template to config file
    nano testdbwriter.ini #or another editor
    # now set up your credentials in the right section, and choose your profile/database in the [Database] section
    

### Adding the tests to your Jenkins job

We'll put the dbtests2db.lpr console runner and the db output writer code it needs (testdbwriter.pas) into the directory where the dbtestframework tests live so dbtests2db can pick up those tests. Then we'll compile the test suite, and run it for each database you want to test. 

In your Jenkins job configuration, add an Execute shell build step to set up the test environment. Note: we copy forcing overwrites to avoid the script aborting the Jenkins job if the target exists: 
    
    
    cp -f /opt/testdbwriter/testdb* \
    /opt/testdbwriter/dbtests2db/database.ini \
    /opt/testdbwriter/dbtests2db/dbtests2db.lpr \
    -t $WORKSPACE/packages/fcl-db/tests/; \
    cd $WORKSPACE/packages/fcl-db/tests; \
    $WORKSPACE/bin/i386-linux/fpc.sh $WORKSPACE/packages/fcl-db/tests/dbtests2db.lpr \
    #copy & compile db test program using the fpc dbtestframework tests
    

Then for each database you want to test, add test runs. The names of the database connectors you specify as arguments must match the section names in database.ini, e.g. postgresql, mysql40, mysql41, mysql50, oracle, interbase etc. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** The dbtests2db program requires that the database connector is the first parameter; we also pick up the subversion revision number from the Jenkins subversion module we use. The examples below show how you can add more information to the test results database.

If you are testing multiple branches/versions of FPC (e.g. 2.6 and trunk) at the same time, you have a problem: the SVN revision number does not apply to branches but to the whole repository. 

This means that the code in the FPC fixes26 branch commit 2500 may be older than the code in the FPC trunk branch 2300. Fortunately, there is a solution: we prepend the revision number passed to the executable with a value (e.g. 020701 for FPC 2.7.1, 020600 for FPC 2.6.0). This serves to preserve the relative "newness" of the various branches... and permits running regression queries on the database to show e.g. that FPC trunk has a failing test where 2.6.0 succeeded. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Older versions of this instruction proposed using 
    
    
    --svnbranch="trunk"
    

which can also be done, but then you separate your testsuite hierarchies into different trees in the database. This does not allow running regression queries so easily.

Examples: 
    
    
    cd $WORKSPACE/packages/fcl-db/tests; \
    $WORKSPACE/packages/fcl-db/tests/dbtests2db interbase --revisionid=020701$SVN_REVISION --comment="Jenkins trunk interbase"
    

With MySQL, as usual, the client version you installed on the Jenkins server has to match the version you specify here - in the example version 5.1: 
    
    
    cd $WORKSPACE/packages/fcl-db/tests; \
    $WORKSPACE/packages/fcl-db/tests/dbtests2db mysql51 --revisionid=020701$SVN_REVISION --comment="Jenkins trunk mysql51"
    
    
    
    cd $WORKSPACE/packages/fcl-db/tests; \
    $WORKSPACE/packages/fcl-db/tests/dbtests2db postgresql --revisionid=020701$SVN_REVISION --comment="Jenkins trunk postgresql"
    
    
    
    cd $WORKSPACE/packages/fcl-db/tests; \
    $WORKSPACE/packages/fcl-db/tests/dbtests2db sqlite --revisionid=020701$SVN_REVISION --comment="Jenkins trunk sqlite"
    
    
    
    cd $WORKSPACE/packages/fcl-db/tests; \
    $WORKSPACE/packages/fcl-db/tests/dbtests2db bufdataset --revisionid=020701$SVN_REVISION --comment="Jenkins trunk bufdataset"
    
    
    
    cd $WORKSPACE/packages/fcl-db/tests; \
    $WORKSPACE/packages/fcl-db/tests/dbtests2db dbase4 --revisionid=020701$SVN_REVISION --comment="Jenkins trunk dbase4"
    

Note: with all this building and testing, your job running time may extend a bit. Please make sure you set the SVN polling interval to a big enough value to avoid building all the time. 

If this works, you can see the test results on your database server, analyze regressions, test succcess percentages, etc. 

## Windows/Linux... build slave

You can set up other machines apart from the Jenkins server to act as "slaves": they are controlled from Jenkins and used to run jobs. 

This can be useful when running a Windows build/test run from a Linux server. 

See [[1]](<https://wiki.jenkins-ci.org/display/JENKINS/Windows+slaves+fail+to+start+via+DCOM>) for details on how to set up Windows build slaves via DCOM (note: there are other ways to set up (Windows) build slaves; please refer to the Jenkins documentation) 

## Enhancements

Possible enhancements: 

  * run make install into a different prefix
  * run Lazarus compile
  * save dbtests2db results to XML, see if Jenkins supports that
  * add installer builders



## Troubleshooting

### Build fails

Your builds may fail because the user Jenkins runs under runs a different shell under a different user with a different path than you might expect. 

### Find out which user Jenkins runs under

If you have problems with scripts/shell commands in jobs not having permissions, it helps if you know which user Jenkins runs under. Try this command (or of course the documentation): 
    
    
    ps aux |grep jenkins | grep -v grep | cut -d" " -f1 | uniq #perhaps only works if jenkins is running a job
    

On Debian, it gives the tomcat6 user. 

## See also

  * [Build server](<Build_server.md> "Build server")



## External links

  * [First look on Jenkins pipelines with a simple Lazarus application](<https://medium.com/@Zawuza/first-look-on-jenkins-pipelines-with-a-simple-lazarus-application-36c77af56829>)

---

_Source: [https://wiki.freepascal.org/Jenkins](https://web.archive.org/web/20230329095115/https://wiki.freepascal.org/Jenkins)_
