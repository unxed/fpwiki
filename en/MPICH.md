# MPICH

## Contents

  * 1 Overview
  * 2 Tutorial
    * 2.1 Windows
    * 2.2 Unix
      * 2.2.1 Ubuntu / Debian
      * 2.2.2 Install from source
      * 2.2.3 Configuration
      * 2.2.4 Test MPD
        * 2.2.4.1 Start mpd on some machines
      * 2.2.5 Test an MPI program
    * 2.3 Get/Compile the MPI bindings for Free Pascal / Lazarus
      * 2.3.1 Windows
    * 2.4 Create the first Free Pascal MPI program
    * 2.5 Run your MPI program
      * 2.5.1 Unix
      * 2.5.2 Windows



# Overview

MPICH is a free implementation of [MPI](<http://www.mcs.anl.gov/mpi/index.htm>) (Message Passing Interface) for distributed memory applications. The latest version MPICH2 implements the MPI-2 standard. 

MPICH is a library plus tools to run a ring of daemons. This means, MPICH must be installed on every machine. 

The MPICH2 lazarus package contains the bindings (= pascal translations of the c header files), which allows pascal programs to use the same variables and functions as c programs would do. So, you can easily convert c examples to pascal. 

# Tutorial

## Windows

You can download an installer from <http://www.mcs.anl.gov/research/projects/mpich2/>. The installer requires Administrative rights to install the smpd service. This will install MPICH2 to C:\Program Files\MPICH2. You must install mpich on all nodes (computers of the cluster). The mpich2 dll(s) are copied to the Windows\system32 directory. The bin directory contains smpd.exe which is the MPICH2 process manager used to launch MPI programs. mpiexec.exe, also found in the bin directory, is used to start MPICH2 jobs. See the README.winbin.rtf for details and hints like minimal installation for nodes. 

See here about authentification: [mpich2-doc-windev.pdf](<http://www.mcs.anl.gov/research/projects/mpich2/documentation/files/mpich2-doc-windev.pdf>)

The best way is to put all nodes into a domain and use SSPI. 

For simple testing mpi under windows, you can use the Start \ Programs \ MPICH2 \ wmpiregister.exe and save username and password: 
    
    
     C:\Program Files\MPICH2\bin\wmpiregister.exe
    

There you must give a valid windows user name and password. 

Then open a console and run: 
    
    
     C:\Program Files\MPICH2\bin\mpiexec.exe -n 10 C:\Program Files\MPICH2\examples\cpi.exe
    

## Unix

First you need to install the MPICH2 library on all computers of your cluster. A cluster can be any set of computers / virtual machines. They do not need to be homogeneous. Your cluster can contain for example Windows and Linux machines, with different number of CPUs and memory. On your development machines, where you compile your application, you must install the development libraries and FPC too. 

### Ubuntu / Debian

Under ubuntu/debian you can install the following packages: libmpich-mpd1.0-dev mpich-mpd-bin 

There's a how-to for installing Mpich2 in Ubuntu/Debian in Ubuntu Wiki : <https://wiki.ubuntu.com/MpichCluster>

### Install from source

Download the mpich2-1.0.6.tar.gz or any newer version from <http://www-unix.mcs.anl.gov/mpi/mpich2/> and unpack it. 

Read the README carefully. It describes, what you need to compile mpich. Under ubuntu feisty: _sudo apt-get install build-essential_. 

You need a shared directory for all nodes. In the following steps it is assumed that the home directory is shared. 
    
    
     ./configure --prefix=/home/you/mpich-install
     make
     sudo make install
    

This will install the libraries in /home/you/mpich-install/lib. This path must be added to the linking search path of FPC. There are two common possibilities: 

  * add the following line to /etc/fpc.cfg



-Fl/home/username/mpich-install/lib 

  * add the path to IDE menu / Project / Compiler Options / Paths / Libraries



Otherwise you will get errors like _/usr/bin/ld: cannot find -lmpich_. 

### Configuration

Make sure your PATH contains the path to the mpich binaries. If not extend your path: 
    
    
     export PATH=/home/you/mpich-install/bin:$PATH
    

Check everything works: 
    
    
     which mpd
     which mpiexec
     which mpirun
    

MPI expects the configuration file in your home directory named **/home/you/.mpd.conf** (/etc/mpd.conf if root). It should contain one line: 
    
    
     secretword=<secretword>
    

where <secretword> is a password, that should **not** be your user password. Make it readable/writable only by you: 
    
    
     chmod 600 .mpd.conf
    

If your home is not shared, it must be copied to all cluster nodes. Check that you can login via ssh without password to all cluster nodes: 
    
    
     ssh othermachine date
    

should not ask for password and give only the date - nothing else. 

Create a file named **/home/you/mpd.hosts** with one line per node (hostnames). For example 
    
    
     host1
     host2
    

### Test MPD

MPD is the MPICH daemon, which controls/runs/stops the proccesses on the cluster nodes. Bring up the ring: 
    
    
      mpd &
      mpdtrace
      mpdallexit
    

mpdtrace should output the hostname of your current working host. mpdallexit stops the daemon. 

#### Start mpd on some machines
    
    
     mpdboot -n <number of daemons to start> -f /home/username/mpd.hosts
    

If the current machine is not part of the cluster (not in the mpd.hosts), then you need one additional mpd (add +1). 

Test: 
    
    
     mpdtrace
     mpdringtest
     mpdringtest 100
     mpiexec -l -n 30 hostname
    

### Test an MPI program

Don't forget to start mpd via **mpdboot**. 

Then, copy the cpi example to a shared location: 
    
    
     cp mpich2-1.0.6/examples/cpi ~/cpi
     mpiexec -n 5 /home/you/cpi
    

The number of proccess (here: 5) can exceed the number of hosts. mpiexec has many options. See **mpiexec -help** and read the README. 

## Get/Compile the MPI bindings for Free Pascal / Lazarus

Download the MPICH bindings as lazarus package from [[1]](<http://sourceforge.net/projects/lazarus-ccr/files/MPICH/>). 

Extract the zip file, use **IDE / components / open package file (.lpk)** to open the mpich2.lpk file and compile the package. 

### Windows

Under windows the library **External_library** constant of the mpi.pas must be changed to 'mpich2'. This will be fixed in the next release. 

## Create the first Free Pascal MPI program

Create a new project (custom project, not an application). Save the project as /home/username/helloworld.lpi. 

Open the mpich2 package (e.g. **IDE / Components / Open recent package / somepath/mpich2.lpk**). This opens the package editor. Then do **More / Add to project**. The project can now use the mpich2 bindings. 

Here is a very small MPI program: 
    
    
    program HelloWorld1;
    
    {$mode objfpc}{$H+}
    {$IDFEF Unix}
    {$Linklib c}
    {$ENDIF}
    // if you get the error: undefined reference to `pthread_getspecific' then enable the following:
    {off $Linklib pthread}
    
    uses
      MPI;
      
    var myid: integer;
    begin
      MPI_Init(@argc,@argv);
      MPI_Comm_rank(MPI_COMM_WORLD,@myid);
      writeln('id=',myid);
      MPI_Finalize;
    end.
    

You can find this program in the examples directory too. 

Compile the program to create the _helloworld_ executable. 

(For Windows users: the examples contain the linklib directive for all platforms, but it must not be used under windows. This will be fixed in the next release. Adapt the examples as seen in the above example.) 

## Run your MPI program

Now run it like the above cpi example. That means: 

### Unix

  * First check with **mpdtrace** if the **mpd** is still running. If not, then use **mpdboot -n <number of hosts> -f /home/username/mpd.hosts** to start the daemons.
  * Copy the helloworld executable to a shared directory:


    
    
     cp helloworld /home/username/helloworld
    

  * start it with


    
    
     mpiexec -n 3 /home/username/helloworld
    

This will give something like: 
    
    
     id=1
     id=0
     id=2
    

Note that programs are started in parallel, so the order of the id lines depends on machine and network speeds. 

### Windows
    
    
     C:\Program Files\MPICH2\bin\mpiexec.exe helloworld.exe
    

This will give something like: 
    
    
     id=1
     id=0
     id=2
    

Note that programs are started in parallel, so the order of the id lines depends on machine and network speeds.

---

_Source: [https://wiki.freepascal.org/MPICH](https://web.archive.org/web/20240909165323/https://wiki.freepascal.org/MPICH)_
