# RunError

A **RunError** is a low-level error emitted by the Free Pascal [Run Time Library](<RTL.md> "RTL"). 

[![runerror232.png](https://wiki.freepascal.org/images/2/29/runerror232.png)](</File:runerror232.png>)

# | Short description | Explanation   
---|---|---  
1 | Invalid function number | An invalid operating system call was attempted.   
2 | File not found | Reported when trying to erase, rename or open a non-existent file.   
3 | Path not found | Reported by the directory handling routines when a path does not exist or is invalid. Also reported when trying to access a non-existent file.   
4 | Too many open files | The maximum number of files currently opened by your process has been reached. Certain operating systems limit the number of files which can be opened concurrently, and this error can occur when this limit has been reached.   
5 | File access denied | Permission to access the file is denied. This error might be caused by one of several reasons: 

  * Trying to open for writing a file which is read-only, or which is actually a directory.
  * File is currently locked or used by another process.
  * Trying to create a new file, or directory while a file or directory of the same name already exists.
  * Trying to read from a file which was opened in write-only mode.
  * Trying to write from a file which was opened in read-only mode.
  * Trying to remove a directory or file while it is not possible.
  * No permission to access the file or directory.

  
6 | Invalid file handle | If this happens, the file variable you are using is trashed; it indicates that your memory is corrupted.   
12 | Invalid file access code | Reported when a reset or rewrite is called with an invalid FileMode value.   
15 | Invalid drive number | The number given to the Getdir or ChDir function specifies a non-existent disk.   
16 | Cannot remove current directory | Reported when trying to remove the currently active directory.   
17 | Cannot rename across drives | You cannot rename a file such that it would end up on another disk or partition.   
100 | Disk read error | An error occurred when reading from disk. Typically happens when you try to read past the end of a file.   
101 | Disk write error | Reported when the disk is full, and you’re trying to write to it.   
102 | File not assigned | This is reported by Reset, Rewrite, Append, Rename and Erase, if you call them with an unassigned file as a parameter.   
103 | File not open | Reported by the following functions : Close, Read, Write, Seek, EOf, FilePos, FileSize, Flush, BlockRead, and BlockWrite if the file is not open.   
104 | File not open for input | Reported by Read, BlockRead, Eof, Eoln, SeekEof or SeekEoln if the file is not opened with Reset.   
105 | File not open for output | Reported by write if a text file isn’t opened with Rewrite.   
106 | Invalid numeric format | Reported when a non-numeric value is read from a text file, and a numeric value was expected.   
107 | Invalid enumeration | Reported when a text representation of an enumerated constant cannot be created in a call to str or write(ln).   
150 | Disk is write-protected | (Critical error)   
151 | Bad drive request struct length | (Critical error)   
152 | Drive not ready | (Critical error)   
154 | CRC error in data | (Critical error)   
156 | Disk seek error | (Critical error)   
157 | Unknown media type | (Critical error)   
158 | Sector Not Found | (Critical error)   
159 | Printer out of paper | (Critical error)   
160 | Device write fault | (Critical error)   
161 | Device read fault | (Critical error)   
162 | Hardware failure | (Critical error)   
200 | Division by zero | The application attempted to divide a number by zero.   
201 | Range check error | If you compiled your program with range checking on, then you can get this error in the following cases: 

  1. An array was accessed with an index outside its declared range.
  2. Trying to assign a value to a variable outside its range (for instance an enumerated type).

  
202 | Stack overflow error | The stack has grown beyond its maximum size (in which case the size of local variables should be reduced to avoid this error), or the stack has become corrupt. This error is only reported when stack checking is enabled.   
203 | Heap overflow error | The heap has grown beyond its boundaries. This is caused when trying to allocate memory explicitly with New, GetMem or ReallocMem, or when a class or object instance is created and no memory is left. Please note that, by default, Free Pascal provides a growing heap, i.e. the heap will try to allocate more memory if needed. However, if the heap has reached the maximum size allowed by the operating system or hardware, then you will get this error.   
204 | Invalid pointer operation | You will get this in several cases: 

  * if you call Dispose or Freemem with an invalid pointer
  * in case New or GetMem is called, and there is no more memory available. The behavior in this case depends on the setting of ReturnNilIfGrowHeapFails. If it is True, then Nil is returned. if False, then runerror 204 is raised.

  
205 | Floating point overflow | You are trying to use or produce real numbers that are too large.   
206 | Floating point underflow | You are trying to use or produce real numbers that are too small.   
207 | Invalid floating point operation | Can occur if you try to calculate the square root or logarithm of a negative number.   
210 | Object not initialized | When compiled with range checking on, a program will report this error if you call a virtual method without having called its object’s constructor.   
211 | Call to abstract method | Your program tried to execute an abstract virtual method. Abstract methods should be overridden, and the overriding method should be called.   
212 | Stream registration error | This occurs when an invalid type is registered in the objects unit.   
213 | Collection index out of range | You are trying to access a collection item with an invalid index (objects unit).   
214 | Collection overflow error | The collection has reached its maximal size, and you are trying to add another element (objects unit).   
215 | Arithmetic overflow error | This error is reported when the result of an arithmetic operation is outside of its supported range. Contrary to Turbo Pascal, this error is only reported for 32-bit or 64-bit arithmetic overflows. This is due to the fact that everything is converted to 32-bit or 64-bit before doing the actual arithmetic operation.   
216 | [General Protection fault](</index.php?title=General_Protection_fault&action=edit&redlink=1> "General Protection fault \(page does not exist\)") | The application tried to access invalid memory space. This can be caused by several problems: 

  1. Dereferencing a nil pointer.
  2. Trying to access memory which is out of bounds (for example, calling move with an invalid length).

  
217 | Unhandled exception occurred | An exception occurred, and there was no exception handler present. The [sysutils](<sysutils.md> "sysutils") unit installs a default exception handler which catches all exceptions and exits gracefully.   
218 | Invalid value specified | Error 218 occurs when an invalid value was specified to a system call, for instance when specifying a negative value to a seek() call.   
219 | Invalid typecast | Thrown when an invalid typecast is attempted on a class using the as operator. This error is also thrown when an object or class is typecast to an invalid class or object and a virtual method of that class or object is called. This last error is only detected if the -CR compiler option is used.   
222 | Variant dispatch error | No dispatch method to call from variant.   
223 | Variant array create | The variant array creation failed. Usually when there is not enough memory.   
224 | Variant is not an array | This error occurs when a variant array operation is attempted on a variant which is not an array.   
225 | Var Array Bounds check error | This error occurs when a variant array index is out of bounds.   
227 | Assertion failed error | An assertion failed, and no [AssertErrorProc](</index.php?title=AssertErrorProc&action=edit&redlink=1> "AssertErrorProc \(page does not exist\)") procedural variable was installed.   
229 | Safecall error check | This error occurs is a safecall check fails, and no handler routine is available.   
231 | Exception stack corrupted | This error occurs when the exception object is retrieved and none is available.   
232 | Threads not supported | Thread management relies on a separate driver on some operating systems (notably, Unixes). The unit with this driver needs to be specified on the uses clause of the program, preferably as the first unit (cthreads on unix). Add **{$define UseCThreads}** in the top of the first Pascal unit (usually the project .lpr file)   
  
## See also

  * [Compile time errors](<compile-time_error.md> "compile-time error")
  * [List of Run-time errors](<http://www.freepascal.org/docs-html/user/userap4.html>)

---

_Source: [https://wiki.freepascal.org/RunError](https://web.archive.org/web/20240807030840/https://wiki.freepascal.org/RunError)_
