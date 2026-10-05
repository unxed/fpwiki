# Assembler and ABI Resources

## Contents

  * 1 The Assembler
  * 2 The ABI
  * 3 Purpose of this note
  * 4 Assembler source formats
    * 4.1 Alpha
    * 4.2 ARM
    * 4.3 AVR
    * 4.4 i386
    * 4.5 IA-64
    * 4.6 M68K
    * 4.7 MIPS
    * 4.8 PowerPC
    * 4.9 PowerPC-64
    * 4.10 SPARC
    * 4.11 VIS
    * 4.12 x86
    * 4.13 x86-64
    * 4.14 zSeries (S/390)
      * 4.14.1 For the GNU assembler (gas):
      * 4.14.2 For the IBM assembler:
  * 5 ABI references
    * 5.1 Alpha
    * 5.2 ARM
    * 5.3 AVR
    * 5.4 i386
    * 5.5 IA-64
    * 5.6 M68K
    * 5.7 MIPS
    * 5.8 PowerPC
    * 5.9 PowerPC-64
    * 5.10 SPARC
    * 5.11 VIS
    * 5.12 x86
    * 5.13 x86-64
    * 5.14 zSeries (S/390)
  * 6 Other resources



## The Assembler

The FPC Pascal Compiler translates Pascal source code into assembly language which is then processed by an assembler running as a separate backend. Some other Pascal compilers directly generate object modules or executable programs directly, i.e. they do not require a separate assembler. 

An assembler is itself an executable program that translates assembly language into an object module. In most cases the object modules are passed to a linker which then produces an executable program, although in some there are additional stages (code signing for secure operating systems, conversion to a binary for embedded systems and so on). 

## The ABI

The interface between an executable program and the underlying operating system is referred to as the Application Binary Interface or ABI. This includes the CPU's operating mode (e.g. whether word and address sizes default to 32 or 64 bits), operand alignment, function calling conventions, system call numbers, and a selection of constants (e.g. file open modes) and structures (e.g. as returned by the stat() function). It is also usually considered to include the format of the object modules, executable and library files. 

Obviously the ABI is grossly different between operating systems: in general a program compiled for Windows will not run on Linux and vice versa. In addition, however, there is a significant amount of variation between different "flavours" of related operating systems, for example not only are the system call numbers different between SPARC Solaris and SPARC Linux but they are different between SPARC Linux and x86 Linux. 

## Purpose of this note

In most cases FPC uses the GNU assembler (as or gas) as its backend. However, the assembly language syntax expected by this is different for each target CPU, sections below give examples of this. The original incentive for this was because the author (MarkMLl) found that he needed to write an assembler reader for the MIPS processor, and that there was no straightforward comparison of existing formats on which he could base new code. 

In addition, in some cases the details of the assembly language format or the ABI specification are only available to users registered with the relevant manufacturer, where possible links to unofficial mirrors are given below for casual reference. 

## Assembler source formats

Assembler source emitted by the compiler's code generator has to be (a subset of what is) acceptable to the assembler for the relevant target CPU. In addition, small portions of the RTL (e.g. prt0.as) are of necessity written in assembler, and some Pascal source files (e.g. syscall.inc) contain inline assembler which the compiler has to be able to parse before it is passed to the backend. 

The list of CPUs below is taken from the compiler as of late 2011. Some of these are no longer supported, or exist merely as minimal stubs. 

### Alpha

This compiler exists only as a minimal stub. 

### ARM

This fragment is from FpSysCall alias FPC_SYSCALL6 in FPC's ./rtl/linux/arm/syscall.inc: 
    
    
    asm
      stmfd sp!,{r4,r5,r6}
      ldr  r4,param4
      ldr  r5,param5
      ldr  r6,param6
      bl FPC_SYSCALL
      ldmfd sp!,{r4,r5,r6}
    end;
    

This fragment is from ret_from_fork in Linux's ./arch/arm/kernel/entry-common.S: 
    
    
    ENTRY(ret_from_fork)
            bl      schedule_tail
            get_thread_info tsk
            ldr     r1, [tsk, #TI_FLAGS]            @ check for syscall tracing
            mov     why, #1
            tst     r1, #_TIF_SYSCALL_TRACE         @ are we tracing syscalls?
            beq     ret_slow_syscall
            mov     r1, sp
            mov     r0, #1                          @ trace exit [IP = 1]
            bl      syscall_trace
            b       ret_slow_syscall
    ENDPROC(ret_from_fork)
    

Note that register names are r0, r1 etc. without a sigil, and that register assignment is right-to-left. 

### AVR

This fragment is from ret_from_fork in Linux's ./arch/avr32/kernel/entry-avr32b.S: 
    
    
    ret_from_fork:
            call   schedule_tail
    
            /* check for syscall tracing */
            get_thread_info r0
            ld.w    r1, r0[TI_flags]
            andl    r1, _TIF_ALLWORK_MASK, COH
            brne    syscall_exit_work
            rjmp    syscall_exit_cont
    

Note that register names are r0, r1 etc. without a sigil, and that register assignment is right-to-left. 

### i386

This fragment is from FpSysCall alias FPC_SYSCALL6 in FPC's ./rtl/linux/i386/syscall.inc: 
    
    
    asm
            push  %ebx
            push  %edx
            push  %esi
            push  %edi
            push  %ebp
            push  %ecx
            cmp   $0, sysenter_supported
            jne   .LSysEnter
            movl  %edx,%ebx         // param1
            pop   %ecx              // param2
            movl  param3,%edx       // param3
            movl  param4,%esi       // param4
            movl  param5,%edi       // param5
            movl  param6,%ebp       // param6
            int   $0x80
            jmp   .LTail
      .LSysEnter:
            movl  %edx,%ebx         // param1
            pop   %ecx              // param2
            movl  param3,%edx       // param3
            movl  param4,%esi       // param4
            movl  param5,%edi       // param5
            movl  param6,%ebp       // param6
            call psysinfo
      .LTail:
            pop   %ebp
            pop   %edi
            pop   %esi
            pop   %edx
            pop   %ebx
            cmpl  $-4095,%eax
            jb    .LSyscOK
            negl  %eax
            call  seterrno
            movl  $-1,%eax
      .LSyscOK:
    end;
    

This fragment is from ret_from_fork in Linux's ./arch/x86/kernel/entry_32.S: 
    
    
    ENTRY(ret_from_fork)
            CFI_STARTPROC
            pushl %eax
            CFI_ADJUST_CFA_OFFSET 4
            call schedule_tail
            GET_THREAD_INFO(%ebp)
            popl %eax
            CFI_ADJUST_CFA_OFFSET -4
            pushl $0x0202                   # Reset kernel eflags
            CFI_ADJUST_CFA_OFFSET 4
            popfl
            CFI_ADJUST_CFA_OFFSET -4
            jmp syscall_exit
            CFI_ENDPROC
    END(ret_from_fork)
    

Note that register names are eax, ebx etc. with % as a mandatory sigil, and that register assignment is left-to-right. 

### IA-64

This compiler exists only as a minimal stub. 

### M68K

This compiler exists in FPC v1 and has lain dormant for a while but has since inclusion in FPC 2.x in the end of 2013. 

### MIPS

This fragment is from FpSysCall alias FPC_SYSCALL6 in FPC's ./rtl/linux/mips/syscall.inc: 
    
    
    asm
      sw  $4,0($23)
      sw  $5,-4($23)
      sw  $6,-8($23)
      sw  $7,-12($23)
      sw  $8,-16($23)
      sw  $9,-20($23)
      sw  $10,-24($23)
      sw  $11,-28($23)
      sw  $12,-32($23)
      sw  $13,-36($23)
      sw  $14,-40($23)
      addiu  $23,$23,-44
    
      move  $2,$4
      move  $4,$5
      move  $5,$6
      move  $6,$7
      move  $7,$8
      move  $8,$9
      lw  $9,0($fp)
    
      subu  $29,32
      sw    $8, 16($29)
      sw    $9, 20($29)
      syscall
      nop
      addiu $29,32
    
      beq $7,$0,.LDone
      nop
      lui   $8,%hi(fpc_threadvar_relocate_proc)
      addiu   $8,%lo(fpc_threadvar_relocate_proc)
      lw   $8,0($8)
      bne  $8,$0,.LThreaded
      nop
      lui   $4,%hi(Errno+4)
      addiu   $4,%lo(Errno+4)
      sw    $2,0($4)
      b     .LFailed
      nop
    .LThreaded:
      sw   $2,-4($fp)#temp#sw $4
      lui   $4,%hi(errno)
      addiu   $4,$4,%lo(errno)
      jal   $8
      nop
      lw  $8,-4($fp)
      sw  $8,0($2)
    .LFailed:
      li    $2,-1
    .LDone:
    
      addiu  $23,$23,44
      lw  $4,0($23)
      lw  $5,-4($23)
      lw  $6,-8($23)
      lw  $7,-12($23)
      lw  $8,-16($23)
      lw  $9,-20($23)
      lw  $10,-24($23)
      lw  $11,-28($23)
      lw  $12,-32($23)
      lw  $13,-36($23)
      lw  $14,-40($23)
    
    end;
    

This fragment is from ret_from_fork in Linux's ./arch/mips/kernel/entry.S: 
    
    
    FEXPORT(ret_from_fork)
            jal     schedule_tail           # a0 = struct task_struct *prev
    
    FEXPORT(syscall_exit)
            local_irq_disable               # make sure need_resched and
                                            # signals dont change between
                                            # sampling and return
            LONG_L  a2, TI_FLAGS($28)       # current->work
            li      t0, _TIF_ALLWORK_MASK
            and     t0, a2, t0
            bnez    t0, syscall_exit_work
    
    FEXPORT(restore_all)                    # restore full frame
            .set    noat
            RESTORE_TEMP
            RESTORE_AT
            RESTORE_STATIC
    FEXPORT(restore_partial)                # restore partial frame
            RESTORE_SOME
            RESTORE_SP_AND_RET
            .set    at
    

Note that register names are 0, 1 etc. with $ as a mandatory sigil, and that register assignment is right-to-left; versions of GNU as from 2.18 onwards also support symbolic register names a0, a1 etc. There is a delay slot after branch etc. instructions. 

### PowerPC

This fragment is from FpSysCall alias FPC_SYSCALL6 in FPC's ./rtl/linux/powerpc/syscall.inc: 
    
    
    asm
      mr  r0,r3
      mr  r3,r4
      mr  r4,r5
      mr  r5,r6
      mr  r6,r7
      mr  r7,r8
      mr  r8,r9
      sc
      bns   .LDone
      lis   r10,(fpc_threadvar_relocate_proc)@ha
      lwz   r10,(fpc_threadvar_relocate_proc)@l(r10)
      cmpwi r10,0
      bne   .LThreaded
      lis   r4,(Errno+4)@ha
      stw   r3,(Errno+4)@l(r4)
      b     .LFailed
    .LThreaded:
      stw   r3,temp
      mflr  r3
      mtctr r10
      lis   r4,(errno)@ha
      stw   r3,retaddress
      lwz   r3,(errno)@l(r4)
      bctrl
      lwz   r4,temp
      lwz   r5,retaddress
      stw   r4,0(r3)
      mtlr  r5
    .LFailed:
     li    r3,-1
    .LDone:
    end;
    

This fragment is from ret_from_fork in Linux's ./arch/powerpc/kernel/entry_32.S: 
    
    
    ret_from_syscall:
            mr      r6,r3
            rlwinm  r12,r1,0,0,(31-THREAD_SHIFT)    /* current_thread_info() */
            /* disable interrupts so current_thread_info()->flags can't change */
            LOAD_MSR_KERNEL(r10,MSR_KERNEL) /* doesn't include MSR_EE */
            /* Note: We don't bother telling lockdep about it */
            SYNC
            MTMSRD(r10)
            lwz     r9,TI_FLAGS(r12)
            li      r8,-_LAST_ERRNO
            andi.   r0,r9,(_TIF_SYSCALL_T_OR_A|_TIF_SINGLESTEP|_TIF_USER_WORK_MASK|_TIF_PERSYSCALL_MASK)
            bne-    syscall_exit_work
            cmplw   0,r3,r8
            blt+    syscall_exit_cont
            lwz     r11,_CCR(r1)                    /* Load CR */
            neg     r3,r3
            oris    r11,r11,0x1000  /* Set SO bit in CR */
            stw     r11,_CCR(r1)
    syscall_exit_cont:
            lwz     r8,_MSR(r1)
    BEGIN_FTR_SECTION
            lwarx   r7,0,r1
    END_FTR_SECTION_IFSET(CPU_FTR_NEED_PAIRED_STWCX)
            stwcx.  r0,0,r1                 /* to clear the reservation */
            lwz     r4,_LINK(r1)
            lwz     r5,_CCR(r1)
            mtlr    r4
            mtcr    r5
            lwz     r7,_NIP(r1)
            FIX_SRR1(r8, r0)
            lwz     r2,GPR2(r1)
            lwz     r1,GPR1(r1)
            mtspr   SPRN_SRR0,r7
            mtspr   SPRN_SRR1,r8
            SYNC
            RFI
    
    66:     li      r3,-ENOSYS
            b       ret_from_syscall
    
    ret_from_fork:
            REST_NVGPRS(r1)
            bl      schedule_tail
            li      r3,0
            b       ret_from_syscall
    

Note that register names are r0, r1 etc. without a sigil, and that register assignment is right-to-left. 

### PowerPC-64

Similar to PowerPC above. 

### SPARC

This fragment is from FpSysCall alias FPC_SYSCALL6 in FPC's ./rtl/linux/sparc/syscall.inc: 
    
    
    asm
            mov     %i0,%g1
            mov     %i1,%o0
            mov     %i2,%o1
            mov     %i3,%o2
            mov     %i4,%o3
            ld      [%i6+92],%o5
            mov     %i5,%o4
            ta      0x10
            bcc     .LSyscOK
            nop
            mov     %o0,%l0
            sethi   %hi(fpc_threadvar_relocate_proc),%o2
            or      %o2,%lo(fpc_threadvar_relocate_proc),%o2
            ld      [%o2],%o3
            subcc   %o3,%g0,%g0
            bne     .LThread
            nop
            sethi   %hi(Errno+4),%o0
            ba      .LNoThread
            or      %o0,%lo(Errno+4),%o0
    .LThread:
            sethi   %hi(Errno),%o0
            or      %o0,%lo(Errno),%o0
            call    %o3
            ld      [%o0],%o0
    .LNoThread:
            st      %l0,[%o0]
            mov     -1,%o0
    .LSyscOK:
            mov     %o0,%i0
    end;
    

This fragment is from ret_from_fork in Linux's ./arch/sparc/kernel/entry.S: 
    
    
    linux_fast_syscall:
            andn    %l7, 3, %l7
            mov     %i0, %o0
            mov     %i1, %o1
            mov     %i2, %o2
            jmpl    %l7 + %g0, %g0
             mov    %i3, %o3
    
    linux_syscall_trace:
            add     %sp, STACKFRAME_SZ, %o0
            call    syscall_trace
             mov    0, %o1
            cmp     %o0, 0
            bne     3f
             mov    -ENOSYS, %o0
            mov     %i0, %o0
            mov     %i1, %o1
            mov     %i2, %o2
            mov     %i3, %o3
            b       2f
             mov    %i4, %o4
    
            .globl  ret_from_fork
    ret_from_fork:
            call    schedule_tail
             mov    %g3, %o0
            b       ret_sys_call
             ld     [%sp + STACKFRAME_SZ + PT_I0], %o0
    
            /* Linux native system calls enter here... */
            .align  4
            .globl  linux_sparc_syscall
    linux_sparc_syscall:
            sethi   %hi(PSR_SYSCALL), %l4
            or      %l0, %l4, %l0
            /* Direct access to user regs, must faster. */
            cmp     %g1, NR_SYSCALLS
            bgeu    linux_sparc_ni_syscall
             sll    %g1, 2, %l4
            ld      [%l7 + %l4], %l7
            andcc   %l7, 1, %g0
            bne     linux_fast_syscall
             /* Just do first insn from SAVE_ALL in the delay slot */
    

Note that register names are i0, i1 etc. with % as a mandatory sigil, and that register assignment is left-to-right. Registers are windowed, and there is a delay slot after branch etc. instructions. 

  


### VIS

This compiler exists only as a minimal stub. 

### x86

Refer to i386 above. 

### x86-64

This fragment is from FpSysCall alias FPC_SYSCALL6 in FPC's ./rtl/linux/x86_64/syscall.inc: 
    
    
    asm
      movq sysnr, %rax        { Syscall number -> rax.  }
      movq param1, %rdi         { shift arg1 - arg5. }
      movq param2, %rsi
      movq param3, %rdx
      movq param4, %r10
      movq param5, %r8
      movq param6, %r9
      syscall                 { Do the system call. }
      cmpq $-4095, %rax       { Check %rax for error.  }
      jnae .LSyscOK           { Jump to error handler if error.  }
      negq  %rax
      movq  %rax,%rdx
      movq  fpc_threadvar_relocate_proc,%rax
      leaq  Errno,%r11
      testq %rax,%rax
      jne   .LThread
      movl  %edx,8(%r11)
      jmp   .LNoThread
    .LThread:
      pushq %rdx
      movq  (%r11),%rdi
      call  *%rax
      popq  %rdx
      movl  %edx,(%rax)
    .LNoThread:
      movq  $-1,%rax
    .LSyscOK:
    end;
    

This fragment is from ret_from_fork in Linux's ./arch/x86/kernel/entry_64.S: 
    
    
    ENTRY(ret_from_fork)
            DEFAULT_FRAME
    
            LOCK ; btr $TIF_FORK,TI_flags(%r8)
    
            push kernel_eflags(%rip)
            CFI_ADJUST_CFA_OFFSET 8
            popf                                    # reset kernel eflags
            CFI_ADJUST_CFA_OFFSET -8
    
            call schedule_tail                      # rdi: 'prev' task parameter
    
            GET_THREAD_INFO(%rcx)
    
            RESTORE_REST
    
            testl $3, CS-ARGOFFSET(%rsp)            # from kernel_thread?
            je   int_ret_from_sys_call
    
            testl $_TIF_IA32, TI_flags(%rcx)        # 32-bit compat task needs IRET
            jnz  int_ret_from_sys_call
    
            RESTORE_TOP_OF_STACK %rdi, -ARGOFFSET
            jmp ret_from_sys_call                   # go to the SYSRET fastpath
    
            CFI_ENDPROC
    END(ret_from_fork)
    

Note that register names are rax, rbx etc. with % as a mandatory sigil, and that register assignment is left-to-right. 

### zSeries (S/390)

As of early 2012, IBM's [zSeries](<ZSeries.md> "ZSeries") (formerly System/390) is not supported by the compiler, refer to [ZSeries/Part 1](<ZSeries/Part_1.md> "ZSeries/Part 1") for the current implementation status. Note that Linux 2.6 probably won't run on anything older than an S/390 G5. 

#### For the GNU assembler (gas):

This fragment is from ret_from_fork in Linux's ./arch/s390/kernel/entry.S: 
    
    
    #define BASED(name) name-system_call(%r13)
    
            .globl  ret_from_fork
    ret_from_fork:
            l       %r13,__LC_SVC_NEW_PSW+4
            l       %r9,__LC_THREAD_INFO    # load pointer to thread_info struct
            tm      SP_PSW+1(%r15),0x01     # forking a kernel thread ?
            bo      BASED(0f)
            st      %r15,SP_R15(%r15)       # store stack pointer for new kthread
    0:      l       %r1,BASED(.Lschedtail)
            basr    %r14,%r1
            TRACE_IRQS_ON
            stosm   __SF_EMPTY(%r15),0x03   # reenable interrupts
            b       BASED(sysc_tracenogo)
    
    sysc_tracenogo:
            tm      __TI_flags+2(%r9),_TIF_SYSCALL
            bz      BASED(sysc_return)
            l       %r1,BASED(.Ltrace_exit)
            la      %r2,SP_PTREGS(%r15)     # load pt_regs
            la      %r14,BASED(sysc_return)
            br      %r1
    

Note that register names are r0, r1 etc. with % as a mandatory sigil. Older assemblers (below) arranged the operands as ordered in the binary opcode (i.e. register assignment order varies by operation), this might have changed for the GNU assembler resulting in obvious compatibility problems. 

#### For the IBM assembler:

For the purpose of comparison and with minimal comment, here are "Hello, World!" programs compiled with GCC for different targets. 
    
    
    GCCMVS on MUSIC/SP hosted by Sim/390 using the command
    
    gccmvs_v30 -S -o hello.asm ... hello.c
    
    generates this:
    
    LC0     EQU   *
            DC    C'Hello, MUSIC/SP!'
            DC    X'15'
            DC    X'0'
            DS    0F
            EXTRN @@CRT0
    * Function main prologue
    MAIN    PDPPRLG CINDEX=0,FRAME=92,BASER=12,ENTRY=YES
            B     FEN0
            LTORG
    FEN0    EQU   *
            DROP  12
            BALR  12,0
            USING *,12
    PG0     EQU   *
            LR    11,1
            L     10,=A(PGT0)
    * Function main code
            MVC   88(4,13),=A(LC0)
            LA    1,88(,13)
            L     15,=V(PRINTF)
            BALR  14,15
            SLR   2,2
            LR    15,2
    

In the example above, note that registers are identified numerically (e.g. BALR 12,0); it is common to load a macro package which introduces a leading R as a sigil (i.e. BALR R12,R0). The operands are ordered as in the binary opcode (i.e. register assignment order varies by operation), this might have changed for the GNU assembler (see section above) resulting in obvious compatibility problems. 
    
    
    GCC on zSeries Linux hosted by Hercules using the command
    
    gcc -S -o hello.asm hello.c
    
    apparently assuming -march=g5, generates this:
    
    .LC0:
           .string "Hello, Linux!"
    .text
           .align  4
    .globl main
           .type   main, @function
    main:
    .LFB2:
           stm     %r11,%r15,44(%r15)
    .LCFI0:
           basr    %r13,0
    .L3:
           ahi     %r15,-96
    .LCFI1:
           lr      %r11,%r15
    .LCFI2:
           l       %r1,.L4-.L3(%r13)
           lr      %r2,%r1
           l       %r1,.L5-.L3(%r13)
           basr    %r14,%r1
           lhi     %r1,0
           lr      %r2,%r1
           l       %r4,152(%r11)
           lm      %r11,%r15,140(%r11)
           br      %r4
           .align  4
    .L5:
           .long   puts
    .L4:
           .long   .LC0
    
    I think that's still effectively generating code similar to traditional
    assembler's "using".
    
    
    
    With the explicit instruction to use a later CPU:
    
    gcc -march=z900 -S -o hello.asm hello.c
    
    it generates this:
    
    .LC0:
           .string "Hello, Linux!"
    .text
           .align  4
    .globl main
           .type   main, @function
    main:
    .LFB2:
           stm     %r11,%r15,44(%r15)
    .LCFI0:
           ahi     %r15,-96
    .LCFI1:
           lr      %r11,%r15
    .LCFI2:
           larl    %r2,.LC0
           brasl   %r14,puts
           lhi     %r1,0
           lr      %r2,%r1
           l       %r4,152(%r11)
           lm      %r11,%r15,140(%r11)
           br      %r4
    
    If I'm reading that correctly, it's referring to the string directly
    without using a register.
    

## ABI references

The list of CPUs etc. is based on those found in the compiler (see above). 

### Alpha

### ARM

### AVR

### i386

### IA-64

### M68K

### MIPS

MIPS has several kinds of ABI, including O32, EABI, n32, n64, nowadays the de facto one should be O32. Some ABI document for MIPS O32 ABI can be found in the internet, unfortunately due to the copyright limit, we cannot provide it here. However, this page [[1]](<http://www.linux-mips.org/wiki/MIPSABIHistory>) has some useful links. 

### PowerPC

### PowerPC-64

### SPARC

SPARC is an open architecture. All design documents are freely available (no signup required) from sparc.org and opensparc.net. sparc.org has documents on the 32 bit SPARC version V8 and the 64 bit SPARC version V9. Earlier versions of the architecture, specifically V7 are available on the net (search for Solbourne). 

You can download the SPARC V8 Architecture Manual here: [[2]](<http://sparc.org/standards/V8.pdf>) The SPARC V8 ABI for UNIX is here: [[3]](<http://sparc.org/standards/psABI3rd.pdf>)

You can download the SPARC V9 Architecture Manual here: [[4]](<http://sparc.org/standards/SPARCV9.pdf>) The SPARC V9 ABI for UNIX is here: [[5]](<http://sparc.org/standards/SCD.2.4.1.ps.Z>) Newer versions than V9 are available on opensparc.net. 

### VIS

### x86

### x86-64

### zSeries (S/390)

The classic IBM operating systems, particularly when running on older hardware (i.e. preceding the S/390 G5) historically adopt a programming style which passes function parameters in registers and blocks of memory rather than using a stack. 

For Linux on a 32-bit S/390, "The first 5 integer (char, short, int, long, long long) and pointer parameters are passed in registers R2, R3, R4, R5 and R6. ... long long parameters are passed in register pairs. Structures of 1, 2, 4 or 8 bytes are passed as integers. ... the first 2 floating point parameters are passed in F0 and F2. ... All other parameters are passed on the stack. If the return value is not an integer, pointer, float, double or 1, 2, 4 or 9-byte structure, a "hidden" parameter in R2 will contain the address of the return area." [[6]](<http://linuxvm.org/present/SHARE99/S8139db.pdf>)

## Other resources

As a general point, there's some useful thoughts on binary disassembly at <http://chdk.wikia.com/wiki/GPL_Disassembling> for situations where IDA or equivalent aren't available.

---

_Source: [https://wiki.freepascal.org/Assembler_and_ABI_Resources](https://web.archive.org/web/20160926203103/https://wiki.freepascal.org/Assembler_and_ABI_Resources)_
