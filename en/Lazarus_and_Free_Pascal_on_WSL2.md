# Lazarus and Free Pascal on WSL2

## Contents

  * 1 Can a Linux FPC-generated 'hello world' be run unmodified on WSL-2 ?
  * 2 Can the Linux version of FPC be installed and run unmodified on WSL2 ?
  * 3 Can a Windows executable execute a WSL2 program, in particular, the compiler.
  * 4 Can the Lazarus IDE execute the WSL2 Linux compiler ?
  * 5 Can a Windows executable execute & debug a WSL2 program using WSL2 gdb ?
    * 5.1 Setting up a project
  * 6 Can the FPC windows 64-bit compiler cross-compile a -Tlinux program using WSL 2. If yes, how ?
  * 7 Summary
  * 8 See Also



## Can a Linux FPC-generated 'hello world' be run unmodified on WSL-2 ?

Yes
    linux FPC-generated 'hello world' can run unmodified on WSL-2.
    More complex applications can run (i.e. FPC itself installed from a repository)

  


## Can the Linux version of FPC be installed and run unmodified on WSL2 ?

In particular: the program and compiler issue direct kernel calls. 

We need to know if this is working, or if a LIBC linked version of the Linux binaries is needed. 

Yes
    The easiest way is to install through the repository.
    Direct Kernel calls work fine under WSL2.

## Can a Windows executable execute a WSL2 program, in particular, the compiler.

If yes, this must be demonstrated this using a Win32 FPC program. 

If not, document what the problems are. 

Yes (potential problems)
    There's [wslwrap](<https://github.com/skalogryz/wslwrap>) (Win32 FPC program) application provided that's executing WSL2 program.

    There's an outstanding issue with truncating and output buffer, if pipe redirection is used: 

    <https://github.com/microsoft/WSL/issues/4082>
    <https://github.com/microsoft/WSL/issues/4184>
    This limits the application of WSL application, due to possible characters missing in the output.

## Can the Lazarus IDE execute the WSL2 Linux compiler ?

Yes
    The only tested approach is a quick substitute of the compiler executable, that runs WSL2 linux compiler.
    One of the approaches is described at: [https://wiki.freepascal.org/Windows_Subsystem_for_Linux#Quick_and_Dirty_-_Using_WSL_FPC_from_Windows](<Windows_Subsystem_for_Linux.md>)
    Another approach using the latest wslwrap version is to replace fpc.exe (make a backup!) with wslwrap.exe (copying over) and creating fpc.wrp file with 

    exe=fpc
    This will allow to compile -TLinux projects, BUT it's also causing interference in IDE itself, as IDE components (i.e. Codetools) depends on the "windows" version of FPC used.
    Windows vs Linux paths are creating conflicts when using WSL compiler. Examples are of such conflicts are (The project was configured to use a wsl-wrapped FPC):
    
    
    Warning: [TPCTargetConfigCache.Update] cannot find real compiler for this platform: Compiler="C:\FPC_Laz\wsltools\fpc.exe" Options="-FrC:\lazarus\components\codetools\fpc.errore.msg" RealCompiler="C:\Users\Owner\AppData\Local\lazarus\/usr/lib/fpc/3.0.0/ppcx64"
    
    Hint: [TPCTargetConfigCache.NeedsUpdate] TargetOS="linux" TargetCPU="x86_64" Options="" real compiler file changed "C:\Users\Owner\AppData\Local\lazarus\/usr/lib/fpc/3.0.0/ppcx64"
    
    Warning: [TPCTargetConfigCache.Update] fpc.exe found cfg a file, the IDE did not: "C:\Users\Owner\AppData\Local\lazarus\\etc\fpc.cfg"
    

    **Note** : a custom compiler can be specified per project (+per build mode?), rather than the entire environment .

## Can a Windows executable execute & debug a WSL2 program using WSL2 gdb ?

(This is assuming that a WSL2 version of gdb exists. If not, document this.). 

In particular, can Lazarus use the WSL2 gdb if such exists. 

Practically - No (Potentially- Yes)
    Gdb does present in WSL2 and needs to be installed additionally
    While it's possible to run WSL2 gdb (and it's surely possible to configure Lazarus to install Gdb)
    Due to glitches with WSL2 pipe redirection, it's not possible to get gdb reports on time (whenever gdb stops on break point or an error). Lazarus IDE is unable detect the change in the execution.

### Setting up a project

    1\. Copy wslwrap.exe twice. One for fpc.exe and another for gdb.exe.
    2\. Create corresponding .wrp file 

    gdb.wrp should look like this:
    
    
    exe=gdb
    inp.dummy=0
    inp.wintonix=1
    shell=1
    

    

    the debugger doesn't need to have "dummy" input, because a normal commands would be sent by IDE
    the all paths in the commands needs to be replaced. Lazarus is Win, and it sends Windows Paths, those has to be converted to linux paths (so gdb would seet hem)
    shell must be used, as Ggb checks for the environment. If it doesn't detect a proper shell it would use "exec" command in order to launch a child process, which would eventually fail.

[![wsl-setting-up.png](https://wiki.freepascal.org/images/3/31/wsl-setting-up.png)](</File:wsl-setting-up.png>)

    3\. Specify the new compiler executable in the project setting (not to mess up with IDE)

[![wsl-compiler-proj.png](https://wiki.freepascal.org/images/5/5e/wsl-compiler-proj.png)](</File:wsl-compiler-proj.png>)

  


    3.1 You also want to uncheck "Apply conventions" for "Target name" of "Compiler Options/Paths" and update "Target file name" by removing ".exe" extension.

[![wsl-project-compiler-options.png](https://wiki.freepascal.org/images/e/e0/wsl-project-compiler-options.png)](</File:wsl-project-compiler-options.png>)

The IDE will start sending a lot of warning regarding non existing configuration files and unsupported targets. Those can be ignored as files would be used by the compiler from WSL and those configuration and files are present in WSL environment. 

    4\. Specify the wrapped gdb file name in debugger options

[![wsl-debugger-options.png](https://wiki.freepascal.org/images/2/2d/wsl-debugger-options.png)](</File:wsl-debugger-options.png>)

    5\. Run the project.

[![wsl debug.png](https://wiki.freepascal.org/images/6/6e/wsl_debug.png)](</File:wsl_debug.png>)

## Can the FPC windows 64-bit compiler cross-compile a -Tlinux program using WSL 2. If yes, how ?

Yes (but undesired approach?)
    It's possible to use WSL2 utils as cross-compiler binary utils. For example an executable x86_64-linux-ld.exe can be substituted with call to "wsl ld", the link.res file generated by FPC is still using windows paths. Preventing the linux linker from being able to build a binary.
    It's possible to update link.res prior to the execution of ld, and convert Windows paths to WSL (Linux) paths.
    Steps (assumption is made that Win32 FPC doesn't have any cross-compile utils installed): 

    1\. have WSL2 installed
    2\. install FPC to WSL2 matching version that is matching the one currently used on Windows. (this is critical as FPC would match units used). (for example the official Lazarus release comes with 3.0.4. Distro's repository would have FPC 3.0.0 available. This won't work for cross-compiling, FPC 3.0.4 needs to be isntalled)
    2.1 download (in win32) linux-xxx.xxx.tar from the official FPC
    2.2 unpack tar file
    2.3 run "wsl sudo ./install.sh" (sudo is need to be able to install to default /usr dir)
    3\. copy x86_64-linux units from WSL to your windows FPC directory (i.e. C:\lazarus\fpc\3.0.4\units\x86_64-linux). You can do that by running the following in Windows command line 

    

    cd "C:\lazarus\fpc\3.0.4\units"
    wsl cp -r /usr/lib/fpc/3.0.4/units/x86_64-linux ./
    4\. Get the latest [wslwrap](<https://github.com/skalogryz/wslwrap>) project and compile it.
    4.1 Move wslwrap.exe to C:\lazarus\fpc\3.0.4\bin\x86_64-linux\x86_64-linux-ld.exe (not the destinatino folder might not exit)
    4.2 create the file C:\lazarus\fpc\3.0.4\bin\x86_64-linux\x86_64-linux-ld.wrp. Add two following lines 

    exe=ld
    linkres=1
    This instructs the current executable when doing call over WSL to cal "ld", but also update link.res file prior to that
    5\. Done. Try to compile hello world pas using command line: 

    C:\lazarus\fpc\3.0.4\bin\x86_64-win64\fpc.exe -Tlinux test.pas
    as a result you should get "test" binary, that can be ran "wsl ./test"
    The problem is that might require to build more "exceptions" and "wsl" hacks in future. Using straight forward Linux cross-compiler might be more reliable approach - OR - instead using WSL calling directly, rather than as a cross-compiler.
    Also attempt to use "Math" unit fails immediately due to IE 200208151: <https://bugs.freepascal.org/view.php?id=35781>

## Summary

  * WSL2 still requires its own fixes.
  * a number of changes are required in Lazarus IDE to make it "wsl" aware. (so it would use WSL paths, instead of Windows paths). Without that changes, even a smarter "wslwrap" utility would have its own challenges.
  * no changes are needed in FPC. Though fixing the internal error would make a better use of wsl as cross-compiler.



## See Also

  * [WSL](<WSL.md> "WSL")

---

_Source: [https://wiki.freepascal.org/Lazarus_and_Free_Pascal_on_WSL2](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Lazarus_and_Free_Pascal_on_WSL2)_
