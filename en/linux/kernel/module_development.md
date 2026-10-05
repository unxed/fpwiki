# linux/kernel/module development

## Contents

  * 1 Overview
    * 1.1 Language enhancement
  * 2 Requirements
    * 2.1 Compiler
    * 2.2 Kernel headers
    * 2.3 GNU make
    * 2.4 Virtual machine
  * 3 Kernel RTL
  * 4 Module code
  * 5 Compilation
    * 5.1 Limitation
  * 6 Building kernel modules (for fun) thread
    * 6.1 Prerequisites
      * 6.1.1 Installation
      * 6.1.2 Suggested reading
      * 6.1.3 Hello World Example
        * 6.1.3.1 Makefile
        * 6.1.3.2 fp_printk.pas
        * 6.1.3.3 fp_hello.pas
        * 6.1.3.4 system.pas
        * 6.1.3.5 module_include.c
      * 6.1.4 Enhancing printk
        * 6.1.4.1 fp_printk.pas
        * 6.1.4.2 fp_hello.pas
      * 6.1.5 dmesg output
      * 6.1.6 Hello World Compilation
      * 6.1.7 Result



# Overview

The purpose of this page is to give basic material to write Linux kernel modules using FPC. As Linux distributions may differ, most actions on installing packages are described for Debian distribution, on which the author of this article works. However, it may be very simple to translate this for other Linux distributions. 

## Language enhancement

The ultimate goal of these wiki pages is to provide a proposal for a future support of natural linux kernel module support in particular and driver support in general (for win32...) A dedicated page [module language feature](<module_language_feature.md> "linux/kernel/module language feature") is intended to provide specification for this future support. 

# Requirements

## Compiler

First of all, you need to have FPC installed 
    
    
    apt-get install fp-compiler
    

Please notice that this will install fp-compiler and fp-units-rtl. The author of this page is using 2.2.0, but 2.0.4 is also working. However other versions were not tested and may not work, especially previous releases. 

## Kernel headers

You need to have the kernel headers packages installed 
    
    
    apt-get install linux-headers-$(uname -r)
    

## GNU make

Then you need the make utility 
    
    
    apt-get install make
    

Please note that installing linux-headers-* will automatically install the required version of GCC and binutils. 

## Virtual machine

It is very recommended to not directly develop kernel modules on your computer. Either you should have a dedicated machine for running your module or have a virtual machine like [User Mode Linux (UML)](<user_mode_VM.md> "linux/kernel/user mode VM") Virtual Machine 

**Using buggy kernel modules may definitely cause harmful effects on your machines**

# Kernel RTL

It is obvious that when programming Linux kernel modules, one can not use the standard FPC RTL. As no _standard kernel RTL_ is provided, you have to write your own one. This is not very easy. A good starting point could be the following 
    
    
     unit system;
     {$TYPEINFO OFF}
     interface
     {Paѕcal common type aliases}
     type
     	{Basic embedded types}
     	u8 = Byte;
     	u16 = Word;
     	u32 = LongWord;
     	u64 = QWord;
     	s8 = ShortInt;
     	s16 = SmallInt;
     	s32 = LongInt;
     	s64 = Int64;
     	{Integer types}
     	DWord = LongWord;
     	Cardinal = LongWord;
     	Integer = SmallInt;
     	UInt64 = QWord;
     {$ifdef CPU64}
     	SizeInt = Int64;
     	SizeUInt = QWord;
     	PtrInt = Int64;
     	PtrUInt = QWord;
     	ValSInt = int64;
     	ValUInt = qword;
     {$endif CPU64}
     {$ifdef CPU32}
     	SizeInt = Longint;
     	SizeUInt = DWord;
     	PtrInt = Longint;
     	PtrUInt = DWord;
     	ValSInt = Longint;
     	ValUInt = Cardinal;
     {$endif CPU32}
     	{Zero - terminated strings }
     	PChar = ^Char;
     	PPChar = ^PChar;
     	{Pointers}
     	PSmallInt = ^Smallint;
     	PShortInt = ^Shortint;
     	PInteger  = ^Integer;
     	PByte     = ^Byte;
     	PWord     = ^word;
     	PDWord    = ^DWord;
     	PLongWord = ^LongWord;
     	PLongint  = ^Longint;
     	PCardinal = ^Cardinal;
     	PQWord    = ^QWord;
     	PInt64    = ^Int64;
     	PPtrInt   = ^PtrInt;
     	PPtrUInt  = ^PtrUInt;
     	PSizeInt  = ^SizeInt;
     	PPointer  = ^Pointer;
     	PPPointer = ^PPointer;
     	PBoolean  = ^Boolean;
     	PWordBool = ^WordBool;
     	PLongBool = ^LongBool;
     	{Other types}
     	HRESULT = type Longint;
     	TDateTime = type Double;
     	TError = type Longint;
     const
     	KERN_INFO='KERNEL:INFO:';
     	KERN_ALERT='KERNEL:ALERT:';
     	DEVICE_NAME='kpmod';
     	EPERM = 1;{Operation not permitted}
     	ENOENT = 2;{No such file or directory}
     	ESRCH = 3;{No such process}
     	EINTR = 4;{Interrupted system call}
     	EIO = 5;{I/O error}
     	ENXIO = 6;{No such device or address}
     	E2BIG = 7;{Argument list too long}
     	ENOEXEC = 8;{Exec format error}
     	EBADF = 9;{Bad file number}
     	ECHILD = 10;{No child processes}
     	EAGAIN = 11;{Try again}
     	ENOMEM = 12;{Out of memory}
     	EACCES = 13;{Permission denied}
     	EFAULT = 14;{Bad address}
     	ENOTBLK = 15;{Block device required}
     	EBUSY = 16;{Device or resource busy}
     	EEXIST = 17;{File exists}
     	EXDEV = 18;{Cross-device link}
     	ENODEV = 19;{No such device}
     	ENOTDIR = 20;{Not a directory}
     	EISDIR = 21;{Is a directory}
     	EINVAL = 22;{Invalid argument}
     	ENFILE = 23;{File table overflow}
     	EMFILE = 24;{Too many open files}
     	ENOTTY = 25;{Not a typewriter}
     	ETXTBSY = 26;{Text file busy}
     	EFBIG = 27;{File too large}
     	ENOSPC = 28;{No space left on device}
     	ESPIPE = 29;{Illegal seek}
     	EROFS = 30;{Read-only file system}
      	EMLINK = 31;{Too many links}
     	EPIPE = 32;{Broken pipe}
     	EDOM = 33;{Math argument out of domain of func}
     	ERANGE = 34;{Math result not representable}
     type
     	{Kernel types}
     	mode_t = Word;
     	nlink_t = DWord;
     	uid_t = Word;
     	fl_owner_t = ^files_struct;
      	gid_t = Word;
     	off_t = LongInt;
     	loff_t = Int64;
     	Ploff_t = ^loff_t;
     	size_t = DWord;
     	ssize_t = LongInt;
     	{$PACKRECORDS C}
     	atomic_t = record
     		counter:LongInt;
     	end;
     	files_struct = record
       		{read mostly part}
     		count:atomic_t;
     		//struct fdtable *fdt;
     		//struct fdtable fdtab;
       		{written part on a separate cache line in SMP}
     		//spinlock_t file_lock ____cacheline_aligned_in_smp;
     		//int next_fd;
     		//struct embedded_fd_set close_on_exec_init;
     		//struct embedded_fd_set open_fds_init;
     		//struct file * fd_array[NR_OPEN_DEFAULT];
      	end;
     	filldir_t = function(arg1:Pointer; arg2:PChar; arg3:LongInt; arg4:loff_t; arg5:u64; arg6:DWord):LongInt; cdecl;
     	read_actor_t = function(arg1:Pointer{read_descriptor_t *}; arg2:Pointer{struct page *}; arg3:DWord; arg4:DWord):LongInt; cdecl;
     	Pfile_operations = ^file_operations;
     	file_operations = record
     		owner:Pointer;{struct module *}
     		llseek:function(filp:Pointer{struct file *}; offset:loff_t; whence:LongInt):loff_t; cdecl;
     		read:function(filp:Pointer{struct file *}; buf:PChar; count:size_t; offset:Ploff_t):ssize_t; cdecl;
     		write:function(filp:Pointer{struct file *}; buf:PChar; count:size_t; offset:Ploff_t):ssize_t; cdecl;
     		aio_read:function(kiocb:Pointer{struct kiocb *}; iovec:Pointer{const struct iovec *}; count:DWord; offset:loff_t):ssize_t; cdecl;
     		aio_write:function(kiocb:Pointer{struct kiocb *}; iovec:Pointer{const struct iovec *}; count:DWord; offset:loff_t):ssize_t; cdecl;
     		readdir:function(filp:Pointer{struct file *}; buf:Pointer; filldir:filldir_t):LongInt; cdecl;
     		poll:function(filp:Pointer{struct file *}; poll_table:Pointer{struct poll_table_struct *}):DWord; cdecl;
     		ioctl:function(inode:Pointer{struct inode *}; filp:Pointer{struct file *}; arg1:DWord; arg2:DWord):LongInt; cdecl;
     		unlocked_ioctl:function(filp:Pointer{struct file *}; arg1:DWord; arg2:DWord):LongInt; cdecl;
     		compat_ioctl:function(filp:Pointer{struct file *}; arg1:DWord; arg2:DWord):LongInt; cdecl;
     		mmap:function(filp:Pointer{struct file *}; vm_area_struct:Pointer{ struct vm_area_struct *}):LongInt; cdecl;
     		open:function(inode:Pointer{struct inode *}; filp:Pointer{struct file *}):LongInt; cdecl;
     		flush:function(filp:Pointer{struct file *}; id:fl_owner_t):LongInt; cdecl;
     		release:function(inode:Pointer{struct inode *}; filp:Pointer{struct file *}):LongInt; cdecl;
     		fsync:function(filp:Pointer{struct file *}; arg1:Pointer{ struct dentry *}; datasync:LongInt):LongInt; cdecl;
     		aio_fsync:function(kiocb:Pointer{struct kiocb *}; datasync:LongInt):LongInt; cdecl;
     		fasync:function(arg1:LongInt; filp:Pointer{struct file *}; arg2:LongInt):LongInt; cdecl;
     		lock:function(filp:Pointer{struct file *}; arg1:LongInt; arg2:Pointer{ struct file_lock *}):LongInt; cdecl;
     		sendfile:function(filp:Pointer{struct file *}; arg1:Pointer{ loff_t *}; arg2:size_t; arg3:read_actor_t; arg4:Pointer{ void *}):ssize_t; cdecl;
     		sendpage:function(filp:Pointer{struct file *}; arg1:Pointer{ struct page *}; arg2:LongInt; arg3:size_t; arg4:Pointer{ loff_t *}; arg5:LongInt):ssize_t; cdecl;
     		get_unmapped_area:function(filp:Pointer{struct file *}; arg1:DWord; arg2:DWord; arg3:DWord; arg4:DWord):DWord; cdecl;
     		check_flags:function(falgs:LongInt):LongInt; cdecl;
     		dir_notify:function(filp:Pointer{struct file *}; arg:DWord):LongInt; cdecl;
     		flock:function(filp:Pointer{struct file *}; arg1:LongInt; arg2:Pointer{ struct file_lock *}):LongInt; cdecl;
     		splice_write:function(arg1:Pointer{struct pipe_inode_info *}; filp:Pointer{struct file *}; arg2:Pointer{ loff_t *}; arg3:size_t; arg4:DWord):ssize_t; cdecl;
     		splice_read:function(filp:Pointer{struct file *}; arg1:Pointer{ loff_t *}; arg2:Pointer{ struct pipe_inode_info *}; arg3:size_t; arg4:DWord):ssize_t; cdecl;
     	end;
     	Pproc_dir_entry = ^proc_dir_entry;
     	proc_dir_entry = record
     		low_ino:Cardinal;
     		namelen:Integer;
     		name:PChar;
     		mode:mode_t;
     		nlink:nlink_t;
     		uid:uid_t;
     		gid:gid_t;
     		size:loff_t;
     		proc_iops:Pointer;{const struct inode_operations *}
     		proc_fops:Pfile_operations;{const struct file_operations *}
     		get_info:Pointer;{get_info_t *}
     		owner:Pointer;{struct module *}
     		next, parent, subdir:Pproc_dir_entry;
     		data:Pointer;
     		read_proc:Pointer;{read_proc_t *}
     		write_proc:Pointer;{write_proc_t *}
     		count:atomic_t;{use count}
     		deleted:LongInt;{delete flag}
     		_set:Pointer;
     	end;
     var
     	proc_root:proc_dir_entry; cvar; external;
     //function copy_from_user(_to:Pointer; _from:Pointer; count:Cardinal):Cardinal; cdecl;
     function create_proc_entry(name:PChar; mode:mode_t; parent:Pproc_dir_entry):Pproc_dir_entry; cdecl;
     procedure remove_proc_entry(name:PChar; parent:Pproc_dir_entry); cdecl;
     procedure printk(fmt:PChar); cdecl;
     procedure printk(fmt:PChar; param:LongInt); cdecl;
     procedure printk(fmt:PChar; param1:PChar); cdecl;
     procedure printk(fmt:PChar; param1:PChar; param2:LongInt); cdecl;
     function register_chrdev(devMajor:LongInt; devName:PChar; fops:Pfile_operations):LongInt; cdecl;
     function unregister_chrdev(devMajor:LongInt; devName:PChar):LongInt; cdecl;
     implementation
     {$LINK kernel_module_info}
     //function copy_from_user(_to:Pointer; _from:Pointer; count:Cardinal):Cardinal; cdecl; external;
     function create_proc_entry(name:PChar; mode:mode_t; parent:Pproc_dir_entry):Pproc_dir_entry; cdecl; external;
     procedure remove_proc_entry(name:PChar; parent:Pproc_dir_entry); cdecl; external;
     procedure printk(fmt:PChar); cdecl; external;
     procedure printk(fmt:PChar; param:LongInt); cdecl; external;
     procedure printk(fmt:PChar; param1:PChar); cdecl; external;
     procedure printk(fmt:PChar; param1:PChar; param2:LongInt); cdecl; external;
     function register_chrdev(devMajor:LongInt; devName:PChar; fops:Pfile_operations):LongInt; cdecl; external;
     function unregister_chrdev(devMajor:LongInt; devName:PChar):LongInt; cdecl; external;
     end.
    

# Module code
    
    
     unit kernel_module;
     {$TYPEINFO OFF}
     interface
     
     implementation
     const
     	PROCFS_MAX_SIZE = 1024;
     	PROCFS_NAME = 'kpmod';
     	BUF_LEN = 256;
     var
     	{Major number assigned to our device driver}
     	devMajor:LongInt;
     	fops:file_operations;
     	i, j, k:LongInt;
     	{Is device open? Used to prevent multiple access to device}
     	devIsOpen:Boolean = False;
     	{pattern:Array[0..1, 0..7, 0..7]of LongInt = (
     		(($A,$9,$5,$6,$A,$9,$5,$6), ($A,$8,$9,$1,$5,$4,$6,$2)),
     		(($6,$5,$9,$A,$6,$5,$9,$A), ($2,$6,$4,$5,$1,$9,$8,$A)));}
     	{This structure hold information about the /proc file}
     	Our_Proc_File:Pproc_dir_entry;
     	{The buffer used to store character for this module}
     	//procfs_buffer:Array[0..PROCFS_MAX_SIZE - 1]of Char;
     	{The size of the buffer}
     	procfs_buffer_size:Cardinal = 0;
     	{The msg the device will give when asked}
     	msg:Array[0..BUF_LEN-1]of Char;
     	msg_Ptr:PChar;
     
     {* This function is called then the /proc file is read
      *
      * Arguments
      * =========
      * 1. The buffer where the data is to be inserted, if
      *	you decide to use it.
      * 2. A pointer to a pointer to characters. This is
      *	useful if you don't want to use the buffer
      *	allocated by the kernel.
      * 3. The current position in the file
      * 4. The size of the buffer in the first argument.
      * 5. Write a "1" here to indicate EOF.
      * 6. A pointer to data (useful in case one common 
      *	read for multiple /proc/... entries)
      *
      * Usage and Return Value
      * ======================
      * A return value of zero means you have no further
      * information at this time (end of file). A negative
      * return value is an error condition.
      *
      * For More Information
      * ====================
      * The way I discovered what to do with this function
      * wasn't by reading documentation, but by reading the
      * code which used it. I just looked to see what uses
      * the get_info field of proc_dir_entry struct (I used a
      * combination of find and grep, if you're interested),
      * and I saw that  it is used in <kernel source
      * directory>/fs/proc/array.c.
      *
      * If something is unknown about the kernel, this is
      * usually the way to go. In Linux we have the great
      * advantage of having the kernel source code for
      * free - use it.
      *}
     function procfile_read(buffer:PChar; buffer_location:PPChar; offset:off_t; buffer_length:LongInt; eof:PLongInt; data:Pointer):LongInt; cdecl;
     begin
     		printk(KERN_INFO + 'procfile_read (/proc/%s) called'#10, PROCFS_NAME);
     		{* 
     		 * We give all of our information in one go, so if the
     		 * user asks us if we have more information the
     		 * answer should always be no.
     		 *
     		 * This is important because the standard read
     		 * function from the library would continue to issue
     		 * the read system call until the kernel replies
     		 * that it has no more information, or until its
     		 * buffer is filled.
     		 *}
     		if offset > 0 then begin
     			{we have finished to read, return 0}
     			procfile_read := 0;
     		end else begin
     				{fill the buffer, return the buffer size}
     				//move('HelloWorld!'#10#0, buffer, 13);
     				//memcpy(buffer, procfs_buffer, procfs_buffer_size);
     				buffer[0] := 'H';
     				buffer[1] := 'e';
     				buffer[2] := 'l';
     				buffer[3] := 'l';
     				buffer[4] := 'o';
     				buffer[5] := #10;
     				buffer[6] := #0;
     				procfile_read := 6;
     		end;
     end;
     {This function is called with the /proc file is written}
     function procfile_write(_file:Pointer{struct file *}; buffer:PChar; count:Cardinal; data:Pointer):LongInt; cdecl;
     begin
     	printk(KERN_INFO + 'procfile_write (/proc/%s) called'#10, PROCFS_NAME);
     	{get buffer size}
     	procfs_buffer_size := count;
     	if procfs_buffer_size > PROCFS_MAX_SIZE then
     		procfs_buffer_size := PROCFS_MAX_SIZE;
     	{write data to the buffer}
     	//if copy_from_user(@procfs_buffer, buffer, procfs_buffer_size) <> 0 then
     	//	procfile_write := -EFAULT
     	//else
     		procfile_write := procfs_buffer_size;
     end;
     {Called when a process tries to open the device file, like "cat /dev/kpmod"}
     function device_open(inode:Pointer{struct inode *}; filp:Pointer{struct file *}):LongInt; cdecl;
     begin
     	//static int counter = 0;
     	if devIsOpen then
     		Exit(-EBUSY);
     	printk('Ouverture en mode Lecture/Ecriture ...'#10);
     	//sprintf(msg, "I already told you %d times Hello world!'#10, counter += 1);
     	//msg_Ptr = msg;
     	//try_module_get(THIS_MODULE);
     	devIsOpen := True;
     	device_open := 0;
     end;
     {Called when a process, which already opened the dev file, attempts to read from it.}
     function device_read(filp:Pointer{struct file *}; buffer:PChar; len:size_t; offset:Ploff_t):ssize_t; cdecl;
     var
     	bytes_read:LongInt;
     begin
     	{Number of bytes actually written to the buffer}
     	bytes_read := 0;
     	printk('Writing %d data into buffer from offset %%lld'#10, len{, offset^});
     	{Actually put the data into the buffer}
     	//while(len > 0) and (msg_Ptr^ <> #0)do begin
     		{The buffer is in the user data segment, not the kernel 
     		 segment so "*" assignment won't work.  We have to use 
     		 put_user which copies data from the kernel data segment to
     		 the user data segment.}
     		//put_user(msg_Ptr^, buffer += 1);
     	//	msg_Ptr += 1;
     	//	len -= 1;
     	//	bytes_read += 1;
     	//end;
     	{If we're at the end of the message, return 0 signifying end of file}
     	{Most read functions return the number of bytes put into the buffer}
     	device_read := bytes_read;
     end;
     {Called when a process writes to dev file: "echo 'hi' > /dev/hello"}
     function device_write(filp:Pointer{struct file *}; buff:Pchar; len:size_t; offset:Ploff_t):ssize_t; cdecl;
     	function step:LongInt;
     	begin
     		if k < 8 then begin
     			//printk('%d'#10,pattern[i][j][k]);
     			//outb(pattern[i][j][k],LPT_BASE);
     			k += 1;
     		end else begin
     			k := 0;
     			//outb(pattern[i][j][k],LPT_BASE);
     			//printk('%d'#10,pattern[i][j][k]);
     			k += 1;
     		end;
     		step := 0;
     	end;
     var
     	data:Char;
     	cmd:Char;
     begin
     	printk(KERN_ALERT + 'Sorry, write operation on "%s" isn''t supported yet.'#10, '/dev/kpmod');
     	device_write := -EINVAL;
     {
     	//get_user(data,buffer);
     	cmd := data;
     	case cmd of
     	'H':begin 
     	   printk('Reportez-vous au fichier README'#10);
     	   end;
     	'h':begin
     	   printk('Initialisation en mode demi pas'#10);
     	   j=0; 
     	   end;
     	'f':begin
     	   printk('Initialisation en mode pas entier'#10);
     	   j=1;
     	   end;
     	'F':begin
     	   i=0; 
     	   step();
     	   end;
     	'R':begin
     		i=1;
     		step();
     		end;
     	else
     		printk('Reportez-vous au fichier README, les commandes sont H, h, F, f, et R'#10);
     	end;
     	device_write := 1;}
     end;
     {Called when a process closes the device file.}
     function device_release(inode:Pointer{struct inode *}; filp:Pointer{struct file *}):LongInt; cdecl;
     begin
     	printk('Fermeture ...'#10);
     	{Decrement the usage count, or else once you opened the file, you'll never get get rid of the module.}
     	//module_put(THIS_MODULE);
     	{We're now ready for our next caller}
     	devIsOpen := False;
     	device_release := 0;
     end;
     {This function is called when the module is loaded}
     function init_module: Integer; cdecl; export;
     begin
     	with fops do begin
     		open := @device_open;
     		read := @device_read;
     		write := @device_write;
     		release := @device_release;
     	end;
     	devMajor := register_chrdev(0, DEVICE_NAME, @fops);
     	if devMajor < 0 then begin
     		printk(KERN_ALERT + 'Registering char device failed with %d'#10, devMajor);
     		Exit(devMajor);
     	end;
     	printk(KERN_INFO + 'I was assigned major number %d. To talk to'#10, devMajor);
     	printk(KERN_INFO + 'the driver, create a dev file with'#10);
     	printk(KERN_INFO + '"mknod /dev/%s c %d 0".'#10, DEVICE_NAME, devMajor);
     	printk(KERN_INFO + 'Try various minor numbers. Try to cat and echo to'#10);
     	printk(KERN_INFO + 'the device file.'#10);
     	printk(KERN_INFO + 'Remove the device file and module when done.'#10);
     	printk(KERN_INFO + '@fops = $%X'#10, LongInt(@fops));
     	{create the /proc file}
     	Our_Proc_File := create_proc_entry(PROCFS_NAME, 0644, Nil);
     	if Our_Proc_File = Nil then begin
     		remove_proc_entry(PROCFS_NAME, @proc_root);
     		printk(KERN_ALERT + 'Could not initialize /proc/%s'#10, PROCFS_NAME);
     		Exit(-ENOMEM);
     	end;
     	printk(KERN_INFO + 'Our_Proc_File = 0x%X'#10, PtrUInt(Our_Proc_File));
     	with Our_Proc_File^ do begin
     		read_proc := @procfile_read;
     		write_proc := @procfile_write;
     		//owner := THIS_MODULE;
     		//mode := S_IFREG OR S_IRUGO;
     		uid := 0;
     		gid := 0;
     		size := 37;
     	end;
     	printk(KERN_INFO + '/proc/%s created'#10, PROCFS_NAME);
     	{everything is ok}
     	init_module := 0;
     end;
     
     {This function is called when the module is unloaded}
     procedure cleanup_module; cdecl; export;
     var
     	ret:LongInt;
     begin
     	remove_proc_entry(PROCFS_NAME, @proc_root);
     	printk(KERN_INFO + '/proc/%s removed'#10, PROCFS_NAME);
     	ret := unregister_chrdev(devMajor, DEVICE_NAME);
     	if ret < 0 then
     		printk(KERN_ALERT + 'Error in unregister_chrdev: %d'#10, ret);
     end;
     
     end.
    

# Compilation

In order to compile the kernel module, you need an object file called **kernel_module_info.c**. This file should contain information about the driver : its description string, its author and its licence. 
    
    
     #include <linux/module.h>
     
     MODULE_DESCRIPTION("Test Module written using FPC");
     MODULE_AUTHOR("Mazen Neifer");
     MODULE_LICENSE("GPL");
    

All you need now to generate your kernel module is to integrate your module sources within the kernel build system. The following make file gives an example how to do that. 
    
    
     obj-m   := kernel_pmodule.o
     kernel_pmodule-objs := kernel_module_info.o kernel_module.o system.o
     KDIR    := /lib/modules/$(shell uname -r)/build
     PWD     := $(shell pwd)
     
     default:kernel_module.o
     	$(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules
     kernel_module.o: kernel_module.pas system.pas
     	fpc kernel_module
     clean:
     	${RM} *.ko *.mod.c *.o *.ppu Module.symvers .*.ko.cmd .*.o.cmd
     	${RM} -r .tmp_versions
    

Now, to compile the module, just type 
    
    
    make
    

You will get a **kernel_pmodule.ko** as output. Then you can load it using 
    
    
    insmod kernel_pmodule.ko
    

## Limitation

Due to a mysterious bug somewhere, the result of the first compilation may hang your kernel. A known workaround is to recompile the **kernel_module.pas** without recompiling **system.pas**. 
    
    
    make
    rm kernel_module.o
    make
    

Then the **kernel_pmodule.ko** is now usable. It seems that when **system** unit is compiled within compilation process of **kernel_module** unit, the object code is not suitable for kernel usage. _To be monitored_

# Building kernel modules (for fun) thread

As posted at <http://thread.gmane.org/gmane.comp.compilers.free-pascal.general/21846> We all know writing in Pascal is fun, creating kernel modules is easy, so why not combine both. How hard can it be ? ;) 

Oh well, the below code is what's made of the _above_ code, maybe resulting in no significant result changes (apart from being stripped down to just create a proof-of-concept Hello World program). Frankly I don't even know whether all these definitions in system.pas are really required at all. 

Oh and I don't usually have _much_ time to play with this. If the concept is more or less along with what you Readers like, maybe I'll put the resulting headers and examples onto a git repo for eternity ;) 

I don't post this to Linux kernel mailing list (yet), because they think that basing some new development (especially in kernel space) on _stable_ and longterm version of kernel is [censored] and [censored] and generally retarded. And that completely apart from the old Pascal vs C war(s). But I just wanted to have some fun with it, see where I can get to, with some help of you guys out there. So, expect newbie questions ;) 

So, usual warnings apply : if it eats your hard disk, draws smileys over your family photos, whatever, don't call me. I wouldn't know why, anyway... It may even try to kill your dog. My predecessor's advice to use User Mode Linux is a good and fair point. As for me, I don't have family photos on this hard disk (any more) and the hardware starts to die, so I don't care that much. 

[El es](</index.php?title=User:El_es&action=edit&redlink=1> "User:El es \(page does not exist\)") 14:05, 23 January 2011 (CET) 

## Prerequisites

### Installation

All is happening on Ubuntu 10.04 LTS, installation instructions above are valid, only use **sudo** in front of them. It also is good to have the kernel-source package installed; it gives you the source tarball into **/usr/src** which you may want to unpack into another directory. 

### Suggested reading

**< <linux source>>/Documentation/stable_api_nonsense.txt**

**< <linux source>>/Documentation/DocBook/kernel-hacking.tmpl** (or make htmldoc in the main source directory - warning: this needs **xmlto** installed. Which on my system wanted to download 447 MB from the Internet and take up 756 MB on my disk, so I didn't do it; the tmpl file is sort of readable without making it into a doc) 

The latter contains a nice insider technical view of what we expect the kernel to be doing. The former contains the reasons why they think hacking on a stable kernel is... see above. 

### Hello World Example

Copy and paste the file contents from here to appropriately named file(s) in your directory of choice. 

#### Makefile
    
    
     #  Free Pascal Demo Kernel Module
     # * the Makefile
     # * Based on work by Mazen Neifer, originally posted on.
     # * http://wiki.freepascal.org/linux/kernel/module_development
     # * Stripped down to bare minimum needed for usable Hello World!
     # * Some definitions also rearranged.
     # * (c) Lukasz Sokol, 2011 License : GPL
     # *)
     
     obj-m   := fpkm_hello.o
     fpkm_hello-objs := module_include.o fp_hello.o system.o
     KDIR    := /lib/modules/$(shell uname -r)/build
     PWD     := $(shell pwd)
     
     default:fp_hello.o
             $(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules
     
     fp_hello.o: system.pas fp_printk.pas fp_hello.pas
             fpc fp_hello
     
      
     clean:
     	${RM} *.ko *.mod.c *.o *.ppu Module.symvers .*.ko.cmd .*.o.cmd
     	${RM} -r .tmp_versions makeout modules.order
    

#### fp_printk.pas
    
    
     (* Free Pascal Headers for Linux Kernel
      * Pascal Code for kernel printk function
      * Based on work by Mazen Neifer, originally posted on
      * http://wiki.freepascal.org/linux/kernel/module_development
      * (c) Lukasz Sokol, 2011 License : GPL
      *)
     unit fp_printk;
     {$TYPEINFO OFF}
     interface 
     const
       KERN_INFO='KERNEL:INFO:';
       KERN_ALERT='KERNEL:ALERT:';  
     
     procedure printk(fmt:PChar); cdecl; external;
     
     implementation 
     
     end.
    

#### fp_hello.pas
    
    
     (* Free Pascal Demo Kernel Module
      * Pascal Module Code
      * Based on work by Mazen Neifer, originally posted on
      * http://wiki.freepascal.org/linux/kernel/module_development
      * Stripped down to bare minimum needed for usable Hello World!
      * Some definitions also rearranged.
      * (c) Lukasz Sokol, 2011 License : GPL
      *)
     
     unit kernel_module;
     {$TYPEINFO OFF}
     interface
     
     
     implementation
     uses fp_printk; 
     
     {$LINK kernel_module_info}
     
     {This function is called when the module is loaded}
     function init_module: Integer; cdecl; export;
     begin
             printk(KERN_INFO + 'Hello World!'+#10+#0);
             init_module := 0;
     end;
     
     {This function is called when the module is unloaded}
     procedure cleanup_module; cdecl; export;
     begin
             printk(KERN_INFO + 'Bye-bye!'+#10+#0);
     end;
     
     end.
    

#### system.pas
    
    
     (* Free Pascal Demo Kernel Module
      * System Unit Code
      * Based on work by Mazen Neifer, originally posted on 
      * http://wiki.freepascal.org/linux/kernel/module_development
      * Stripped down to bare minimum needed for usable Hello World!
      * Some definitions also rearranged.
      * (c) Lukasz Sokol, 2011 License : GPL
      *)
     unit system;
     {$TYPEINFO OFF}
     interface
     {Paѕcal common type aliases}
     type 
     	{Basic embedded types}
     	u8 = Byte;
     	u16 = Word;
     	u32 = LongWord;
     	u64 = QWord;
     	s8 = ShortInt;
     	s16 = SmallInt;
     	s32 = LongInt;
     	s64 = Int64;
     	{Integer types}
     	DWord = LongWord;
     	Cardinal = LongWord;
     	Integer = SmallInt;
     	UInt64 = QWord;
     {$ifdef CPU64}
      	SizeInt = Int64; 
     	SizeUInt = QWord;
     	PtrInt = Int64;
     	PtrUInt = QWord;
     	ValSInt = int64;
     	ValUInt = qword;
     {$endif CPU64}
     {$ifdef CPU32}
     	SizeInt = Longint;
     	SizeUInt = DWord;
     	PtrInt = Longint;
     	PtrUInt = DWord;
     	ValSInt = Longint;
     	ValUInt = Cardinal;
     {$endif CPU32}
     	{Zero - terminated strings }
     	PChar = ^Char;
     	PPChar = ^PChar;
     	{Pointers}
     	PSmallInt = ^Smallint;
     	PShortInt = ^Shortint;
     	PInteger  = ^Integer;
     	PByte     = ^Byte;
     	PWord     = ^word;
     	PDWord    = ^DWord;
     	PLongWord = ^LongWord;
     	PLongint  = ^Longint;
     	PCardinal = ^Cardinal;
     	PQWord    = ^QWord;
     	PInt64    = ^Int64;
     	PPtrInt   = ^PtrInt;
     	PPtrUInt  = ^PtrUInt;
     	PSizeInt  = ^SizeInt;
     	PPointer  = ^Pointer;
     	PPPointer = ^PPointer;
     	PBoolean  = ^Boolean;
     	PWordBool = ^WordBool;
     	PLongBool = ^LongBool;
     	{Other types}
     	HRESULT = type Longint;
     	TDateTime = type Double;
     	TError = type Longint;
     const
     	EPERM = 1;{Operation not permitted} 
     	ENOENT = 2;{No such file or directory}
     	ESRCH = 3;{No such process}
     	EINTR = 4;{Interrupted system call}
     	EIO = 5;{I/O error}
     	ENXIO = 6;{No such device or address}
     	E2BIG = 7;{Argument list too long}
     	ENOEXEC = 8;{Exec format error}
     	EBADF = 9;{Bad file number}
     	ECHILD = 10;{No child processes}
     	EAGAIN = 11;{Try again}
     	ENOMEM = 12;{Out of memory}
     	EACCES = 13;{Permission denied}
     	EFAULT = 14;{Bad address}
     	ENOTBLK = 15;{Block device required}
     	EBUSY = 16;{Device or resource busy}
     	EEXIST = 17;{File exists}
     	EXDEV = 18;{Cross-device link}
     	ENODEV = 19;{No such device}
     	ENOTDIR = 20;{Not a directory}
     	EISDIR = 21;{Is a directory}
     	EINVAL = 22;{Invalid argument}
     	ENFILE = 23;{File table overflow}
     	EMFILE = 24;{Too many open files}
     	ENOTTY = 25;{Not a typewriter}
     	ETXTBSY = 26;{Text file busy}
     	EFBIG = 27;{File too large}
     	ENOSPC = 28;{No space left on device}
     	ESPIPE = 29;{Illegal seek}
     	EROFS = 30;{Read-only file system}
      	EMLINK = 31;{Too many links}
     	EPIPE = 32;{Broken pipe}
     	EDOM = 33;{Math argument out of domain of func}
     	ERANGE = 34;{Math result not representable}
     type
      	{Kernel types}
     	mode_t = Word;
     	nlink_t = DWord;
     	uid_t = Word;
      	gid_t = Word;
     	off_t = LongInt;
     	loff_t = Int64;
     	Ploff_t = ^loff_t;
     	size_t = DWord;
     	ssize_t = LongInt;
     	
     implementation
      
     end.
    

#### module_include.c
    
    
     /* Free Pascal Demo Kernel Module
      * c code that is linked into Pascal after compile - needed for symbols
      * Based on work by Mazen Neifer, originally posted on
      * http://wiki.freepascal.org/linux/kernel/module_development
      * Stripped down to bare minimum needed for usable Hello World!
      * Some definitions also rearranged.
      * (c) Lukasz Sokol, 2011 License : GPL
      */
     
     #include <linux/module.h> 
     
     MODULE_DESCRIPTION("Test Module written using FPC");
     MODULE_AUTHOR("Lukasz Sokol (fork)");
     MODULE_LICENSE("GPL");
    

### Enhancing printk

If you make your fp_printk file look like this: 

#### fp_printk.pas
    
    
     (* Free Pascal Headers for Linux Kernel
     * Pascal Code for kernel printk function
     * Based on work by Mazen Neifer, originally posted on
     * http://wiki.freepascal.org/linux/kernel/module_development
     * (c) Lukasz Sokol, 2011 License : GPL
     *)
     unit fp_printk;
     {$TYPEINFO OFF}
     interface
     
     const
       // codes for printk() call, copied from <src>include/linux/kernel.h
     KERN_EMERG   =    '<0>';   //* system is unusable                   */
     KERN_ALERT   =    '<1>';   //* action must be taken immediately     */
     KERN_CRIT    =    '<2>';   //* critical conditions                  */
     KERN_ERR     =    '<3>';   //* error conditions                     */
     KERN_WARNING =    '<4>';   //* warning conditions                   */
     KERN_NOTICE  =    '<5>';   //* normal but significant condition     */
     KERN_INFO    =    '<6>';   //* informational                        */
     KERN_DEBUG   =    '<7>';   //* debug-level messages                 */ 
    
     procedure printk(fmt:PChar); cdecl; varargs; external;
     
     implementation
     
     end.
    

I.e. using varargs clause after the cdecl, as per [this link [[1]](<http://www.freepascal.org/docs-html/ref/refsu77.html#x159-16900011.9.17>)], you can use the full power of kernel printk, as described among others [here [[2]](<http://www.mjmwired.net/kernel/Documentation/printk-formats.txt>)] making your main module source look like : 

#### fp_hello.pas
    
    
     (* Free Pascal Demo Kernel Module
      * Pascal Module Code
      * Based on work by Mazen Neifer, originally posted on
      * http://wiki.freepascal.org/linux/kernel/module_development
      * Stripped down to bare minimum needed for usable Hello World!
      * Some definitions also rearranged.
      * (c) Lukasz Sokol, 2011 License : GPL
      *) 
     
     unit fp_hello;
     {$TYPEINFO OFF}
     interface 
     
     implementation
     uses fp_printk; 
     
     const LFEOL = #10#0;
     {$LINK kernel_module_info}
     
     {This function is called when the module is loaded}
     function init_module: Integer; cdecl; export;
     begin
             printk(KERN_INFO + 'Hello World!'+LFEOL);
             printk(KERN_INFO + 'Also works with variable number of arguments like %d, 0x%.2x, %u '+LFEOL, -10, 15, 10);
             init_module := 0;
     end; 
     
     {This function is called when the module is unloaded}
     procedure cleanup_module; cdecl; export;
     begin
             printk(KERN_INFO + 'Bye-bye!'+LFEOL);
     end; 
     
     end.
    

producing 

### dmesg output
    
    
    [39432.662017]  Hello World!
    [39432.662028] Also works with variable number of arguments like -10, 0x0f, 10 
    [39432.679324] Bye-bye!
    

Nice innit ? 

### Hello World Compilation

Having Midnight Commander on hand is, erm, handy. Change directory into where your above files are, and call **make**. 
    
    
     fpc fp_hello
     Free Pascal Compiler version 2.4.0-2 [2010/03/06] for i386
     Copyright (c) 1993-2009 by Florian Klaempfl
     Target OS: Linux for i386
     Compiling fp_hello.pas
     Compiling system.pas
     Compiling fp_printk.pas
     189 lines compiled, 0.1 sec 
     make -C /lib/modules/2.6.32-27-generic/build SUBDIRS=/home/lukasz/projects/kfpc/hello_world modules
     make[1]: Entering directory `/usr/src/linux-headers-2.6.32-27-generic'
      CC [M]  /home/lukasz/projects/kfpc/hello_world/module_include.o
      LD [M]  /home/lukasz/projects/kfpc/hello_world/fpkm_hello.o
      Building modules, stage 2.
      MODPOST 1 modules
     WARNING: could not find /home/lukasz/projects/kfpc/hello_world/.fp_hello.o.cmd for /home/lukasz/projects/kfpc/hello_world/fp_hello.o
       CC      /home/lukasz/projects/kfpc/hello_world/fpkm_hello.mod.o
       LD [M]  /home/lukasz/projects/kfpc/hello_world/fpkm_hello.ko
     make[1]: Leaving directory `/usr/src/linux-headers-2.6.32-27-generic'
     lukasz@lukasz-laptop:~/projects/kfpc$ 
    

### Result

With the tools at current versions of Ubuntu 10.04(LTS), the #Limitation does not seem to apply - it works straight away, i.e. did not hang MY machine. After that **sudo insmod kernel_pmodule.ko** and **sudo rmmod kernel_pmodule** will give you 
    
    
     lukasz@lukasz-laptop:~/projects/kfpc$ sudo insmod fp_hello.ko && sudo rmmod fp_hello
     [sudo] password for lukasz: 
     lukasz@lukasz-laptop:~/projects/kfpc$ 
    

(this means : no errors) 

and **tail /var/log/messages** yields 
    
    
    [54219.552565] Hello World!
    [54219.587641] Bye-bye!

---

_Source: [https://wiki.freepascal.org/linux/kernel/module_development](https://web.archive.org/web/20250114033227/https://wiki.freepascal.org/linux/kernel/module_development)_
