# WebAssembly/Roadmap

## Short-Term

These steps are critical for completion in order to have WASM a full scope CPU target for FPC compiler. 

Feature  | Status  | Description   
---|---|---  
Assembly (textual wasm)  | working  | Pick an assembler utility that's sequence instruction (FPC) friendly FPC should produce a textual assembly file. Currently, FPC uses the LLVM assembler llvm-mc.   
Linking  | working  | Pick a linker utility that's sequence instruction (FPC) friendly. Linker is a moving target. As a new feature is added, the linking needs to be re-verified. Currently, FPC uses the LLVM linker.   
Basic function calls and local variables  | working  | Implement direct function calls, with passing basic parameters (that don't require any memory. I.e. int and floats)   
Verify float-point values  | working  | As the float points are being converted from the textual format. It's necessary to make sure that the proper values are generated. As of now, they appear to work, including + or - infinity and NaNs.   
Global variables  | working  | Implement symbol reference to global variables (basic or complex) Consider linking - as the index of symbols can change. (Wasm binaries are index based )   
Stack/Call frame support  | working  | WebAssembly doesn't have any native stack and/or frame support. It has to be emulated using global and local variables. Blocking issue: <https://github.com/WebAssembly/wabt/issues/1199>  
Indirect function calls  | working  | WebAssembly has a specific way of indirect calls Needed for vmt and procedure variables (callback) calls   
VMT  | working  | Support for calls via virtual method table   
RTL.Memory Manager  | working  |   
RTL.(Ansi/Wide) String Manager  | working  |   
RTL.WASI.Console I/O  | working  | Standard input, standard output and error output (stderr) works on the WASI target.   
RTL.WASI.Command line parameters  | working  | Both ParamStr and ParamCount work.   
RTL.WASI.Exit codes  | working  | Terminating the program with an exit code via halt(XXX) works.   
RTL.WASI.Randomize  | working  |   
RTL.WASI.File I/O  | working  | Working. Minor issues remain, when there's no mapping for the '/' directory. There's currently no official way in WASI to pass the current directory. Relevant upstream development to follow: [[1]](<https://github.com/WebAssembly/wasi-filesystem/issues/24>) An alternative we could use is to pass it as an environment variable (e.g. PWD), but it would not be automatic, and there's no similar convention, supported by the WASI C library.   
RTL.WASI.File I/O.Basic File I/O  | working  | This covers basic sequential reading and writing of text and binary files. This includes Assign, Reset, Rewrite, Append, Close, Read, Write, Readln, Writeln, EoF, EoLn, BlockRead, BlockWrite.   
RTL.WASI.File I/O.ChDir  | in progress  | Partially working. Needs more testing. Things left TODO: #1: in case there's no mapping for '/', support ChDir to the upper unmapped parts of a mapped directory, e.g. if the user has mapped only '/home/someuser', support ChDir to '/' and '/home' as well. TODO #2: check how Windows paths are implemented and support that, e.g. if needed, support and test paths with drive names e.g. 'C:\'. Also support paths with backslashes as well as forward slashes, etc.   
RTL.WASI.File I/O.MkDir  | working  |   
RTL.WASI.File I/O.RmDir  | working  |   
RTL.WASI.File I/O.GetDir  | working  |   
RTL.WASI.File I/O.Erase  | working  |   
RTL.WASI.File I/O.Rename  | working  |   
RTL.WASI.File I/O.Seek  | working  |   
RTL.WASI.File I/O.FileSize  | working  |   
RTL.WASI.File I/O.FilePos  | working  |   
RTL.WASI.File I/O.Truncate  | working  |   
RTL.WASI.DOS.FindFirst/FindNext/FindClose  | working  |   
RTL.WASI.DOS.Environment variables  | working  | This includes the environment variables of the DOS unit: GetEnv, EnvCount, EnvStr.   
RTL.WASI.DOS.GetDate/GetTime  | working  | Works, but returns UTC time, instead of local time, because WASI lacks timezone support.   
RTL.WASI.DOS.GetMsCount  | working  |   
RTL.WASI.DOS.GetFTime  | working  | Works, but returns UTC time, instead of local time, because WASI lacks timezone support.   
RTL.WASI.DOS.SetFTime  | working  | Works, but returns UTC time, instead of local time, because WASI lacks timezone support.   
RTL.WASI.DOS.GetFAttr  | working  |   
RTL.WASI.DOS.FSearch  | working  |   
RTL.WASI.SysUtils unit  | working  | Compiles and appears to be working. Needs more testing. Enabling one of the exception handling modes is highly recommended.   
RTL.WASI.SysUtils.Platform independent utilities  | working  | This includes various string and pchar utilities, type helpers, conversion routines (e.g. Format, Trim, IntToStr, StrToInt, ...), generic (not related to the system clock) utilities (e.g. IsLeapYear), etc.   
RTL.WASI.SysUtils.File I/O  | working  | Minor issues may occur if there's no mapping for the '/' directory.   
RTL.WASI.SysUtils.File I/O.FileOpen  | working  | Implemented. Note: Sharing/locking is not supported on WASI. Feature request is here: [[2]](<https://github.com/WebAssembly/WASI/issues/446>)  
RTL.WASI.SysUtils.File I/O.FileCreate  | working  | Implemented. Note: Sharing/locking and Unix-style permissions are not supported on WASI.   
RTL.WASI.SysUtils.File I/O.FileClose  | working  |   
RTL.WASI.SysUtils.File I/O.FileRead  | working  |   
RTL.WASI.SysUtils.File I/O.FileWrite  | working  |   
RTL.WASI.SysUtils.File I/O.FileSeek  | working  |   
RTL.WASI.SysUtils.File I/O.FileTruncate  | working  |   
RTL.WASI.SysUtils.File I/O.DirectoryExists  | in progress  | Almost ready. Only thing left TODO: If there's no mapping for '/', but only for a subdirectory, e.g. '/usr/bin', then DirectoryExists('/usr') or DirectoryExists('/') would return false, while it should be expected to return true.   
RTL.WASI.SysUtils.File I/O.FileExists  | working  |   
RTL.WASI.SysUtils.File I/O.FileAge  | working  | Works, but returns UTC time, instead of local time, because WASI lacks timezone support.   
RTL.WASI.SysUtils.File I/O.FileGetSymLinkTarget  | working  |   
RTL.WASI.SysUtils.File I/O.FindFirst/FindNext/FindClose  | working  |   
RTL.WASI.SysUtils.File I/O.FileGetDate  | working  | Works, but returns UTC time, instead of local time, because WASI lacks timezone support.   
RTL.WASI.SysUtils.File I/O.FileSetDate  | working  | Works, but accepts UTC time, instead of local time, because WASI lacks timezone support.   
RTL.WASI.SysUtils.File I/O.FileGetAttr  | working  |   
RTL.WASI.SysUtils.File I/O.DeleteFile  | working  |   
RTL.WASI.SysUtils.File I/O.RenameFile  | working  |   
RTL.WASI.SysUtils.Environment variables  | working  |   
RTL.WASI.SysUtils.Sleep  | working  |   
RTL.WASI.SysUtils.Date and time functions, related to the system clock  | working  | Implemented, but they all work with UTC time, instead of local time, because WASI lacks timezone support.   
RTL.WASI.SysUtils.GetTickCount/GetTickCount64  | working  |   
RTL.WASI.Classes unit  | working  | Compiles and appears to be working. Needs more testing. Enabling one of the exception handling modes is highly recommended.   
  
## Long-Term

Future improvements for wasm target. Some of those are nice to have, but not critical. Others are difficult to implement or need features of WebAssembly, which haven't been standardized yet. 

Feature  | Status  | Description   
---|---|---  
WebAssembly Globals  | in progress  | These are in the planning phase. Main article: [WebAssembly/Globals](<Globals.md> "WebAssembly/Globals")  
Debug Info  | working  | There's experimental DWARF support for WebAssembly. See: [WebAssembly/Debugging](<Debugging.md> "WebAssembly/Debugging")  
Internal Object Writer  | working  | Enabled by default in commit bc76487b878e59133ce3c2cc19ebb13f9a9826ea   
Internal Linker  | working  | Enabled by default in commit e3f4bad1646c9b16fdd672707c0145fdaa356d38   
GOTO support  | working  | Main article: [WebAssembly/Goto](<Goto.md> "WebAssembly/Goto")  
non-local GOTO  | not implemented  | Non-local GOTO is special and needs an implementation, more similar to the exceptions support.   
Exceptions support (no exceptions mode)  | working  | By default, exceptions are disabled. However, try..finally blocks still work and execute their finally section, even when using the 'exit', 'break' or 'continue' statements. Try..except blocks are compiled, however the 'except' part is never executed. And invoking 'raise' terminates the program, since exceptions cannot be caught in this mode.   
Exceptions support (native exceptions mode)  | working  | Requires the exception handling proposal: [[3]](<https://github.com/WebAssembly/exception-handling/blob/main/proposals/exception-handling/Exceptions.md>) Information about the browser support for this new experimental feature is available here: [[4]](<https://webassembly.org/roadmap/>)  
Exceptions support (branchful exceptions mode)  | working  | This is a compatibility mode, that implements exception support by adding branching after each function call. This incurs a large runtime cost, but is compatible with all WebAssembly implementations and doesn't require the WebAssembly implementation to be JavaScript hosted. This is the only approach, that currently works with wasmtime.   
Exceptions support (JavaScript-based exceptions handling mode)  | not implemented  | This is the compatibility mode, used by Emscripten. This also incurs a runtime cost, and also requires the WebAssembly implementation to be JavaScript-based, so this is not supported by wasmtime. In this mode, each function call inside a try block is wrapped inside a JavaScript function call, that wraps the WebAssembly function in a try .. catch JavaScript statement block.   
Timezone support  | not implemented  | WASI currently lacks timezone support. This also applies to WASI's C library in the official SDK. It is possible to parse the TZ environment variable, and read the binary timezone data from the file system, in case that is accessible, however this is probably not portable, and isn't done by their C library. Upstream bug reports: [[5]](<https://github.com/WebAssembly/WASI/issues/167>) [[6]](<https://github.com/bytecodealliance/wasmtime/issues/1200>) [[7]](<https://github.com/WebAssembly/WASI/issues/239>)  
Multithreading support  | working  | Main article: [WebAssembly/Threads](<Threads.md> "WebAssembly/Threads") Threads are not supported in WebAssembly MVP. There is a future proposal here: [[8]](<https://github.com/WebAssembly/threads/blob/main/proposals/threads/Overview.md>) It is already implemented in most browsers: [[9]](<https://webassembly.org/roadmap/>) There's also a WASI threads proposal: [[10]](<https://github.com/WebAssembly/wasi-threads>)  
WASI 0.2 support  | not implemented  |   
WASIX support  | not implemented  |   
Process support - RTL.WASI.DOS.Exec/DosExitCode, fcl-process, etc.  | not implemented  | WASI does not support starting processes, yet. Proposal is here: [[[11]](<https://github.com/WebAssembly/WASI/issues/414>)]   
RTL.WASI.DOS.DiskFree  | not implemented  | Not sure if supported by the WASI API. Can be implemented, if an appropriate WASI API is added in the future.   
RTL.WASI.DOS.DiskSize  | not implemented  | Not sure if supported by the WASI API. Can be implemented, if an appropriate WASI API is added in the future.   
RTL.WASI.SysUtils.DiskFree  | not implemented  | Not sure if supported by the WASI API. Can be implemented, if an appropriate WASI API is added in the future.   
RTL.WASI.SysUtils.DiskSize  | not implemented  | Not sure if supported by the WASI API. Can be implemented, if an appropriate WASI API is added in the future.   
RTL.WASI.DOS.SetFAttr  | not implemented  | Not supported by the WASI API.   
RTL.WASI.SysUtils.File I/O.FileSetAttr  | not implemented  | Not supported by the WASI API.   
RTL.WASI.DOS.SetDate/SetTime  | not implemented  | Setting the system clock is not supported by the WASI API. Can be implemented, if an appropriate WASI API is added in the future.   
  
## See Also

  * [WebAssembly](<../WebAssembly.md> "WebAssembly")

---

_Source: [https://wiki.freepascal.org/WebAssembly/Roadmap](https://web.archive.org/web/20250323031833/https://wiki.freepascal.org/WebAssembly/Roadmap)_
