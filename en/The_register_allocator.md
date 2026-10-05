# The register allocator

│ **English (en)** │

back to contents [FPC internals](<FPC_internals.md> "FPC internals")

## Contents

  * 1 Introduction
  * 2 How to use the register allocator
    * 2.1 Creating the Register allocator
    * 2.2 Using registers in the code generation
    * 2.3 Generating of real registers
  * 3 Tips
  * 4 Calls hierarchy for the public methods
  * 5 Class TRgObj
    * 5.1 Description
    * 5.2 Public methods
    * 5.3 Protected methods
    * 5.4 Private methods
  * 6 Imaginary registers
    * 6.1 Storing imaginary registers
    * 6.2 Life of imaginary register
  * 7 Interferences
    * 7.1 generate_interference_graph
    * 7.2 add_edges_used
    * 7.3 add_edge
  * 8 Backgrounds
    * 8.1 Grouping the instructions
    * 8.2 Data in the instructions
    * 8.3 Programming using the instructions
    * 8.4 Object Design guidelines



## Introduction

Main unit for the register allocation is rgobj.pas Main class for the register allocation is TRgObj located in rgobj.pas 

Register allocator provides imaginary registers for the assembler instructions during the code generation. Then it calculates the real registers to replace the imaginary ones. 

The FPC register allocator uses Register coloring to determine the real registers. It also uses a register spilling technique - when there is not enough registers it uses memory. 

## How to use the register allocator

This topic describes how to use the register allocator during the code generation. Described like a black box with the public methods you can call to get job done. 

### Creating the Register allocator

Creating register allocator is the first step we make before we can use its functionality. 

The low level code generator creates several instances of the TRgObj class. Normally this happens in: 
    
    
    tcg.init_register_allocators
    

And the instances are normally freed in: 
    
    
    tcg.done_register_allocators
    

Each TRgObj instance allocates registers of a certain type. For example, one register type is Integer registers. Another one is floating point (FPU) registers. That's why we have a few TRgObj instances, one for every type of register that the cpu supports. 

They are created when the code generation of specific routine begins. Code generator works on subroutine level. The register allocator also allocates registers for specific method, procedure, function. 

### Using registers in the code generation

To allocate a register use the following method: 
    
    
    TRgObj.getRegister
    

for the appropriate register allocator instance (depending on the register type) to get register for some assembler instruction. This way an **imaginary** register is allocated and can be used after that in a specific assembler instruction. 

The code generator calls it in the following functions: 
    
    
    tcg.GetIntRegister
    tcg.GetAddressRegister
    tcg.GetFPURegister
    tcg.GetMMRegister
    

  
Additionally, the following methods: 
    
    
    TRgObj.getCpuRegister
    TRgObj.ungetCpuRegister
    TRgObj.allocCpuRegisters
    TRgObj.deallocCpuRegisters
    

allocate or free one or multiple **real** registers (not imaginary ones!) This is usually used for instructions that require a specific register (for example SHL/SHR/SAR on x86, which always uses ECX). This is also used when doing a function call to indicate that certain registers (which depend on the calling convention) may be destroyed by the called function. 

After allocating the register we can use it in some assembler instructions. For every instruction that generates, the code generator notifies the register allocator. It passes the instruction, also the imaginary register as parameters to the following method. 
    
    
             TRgObj.add_reg_instruction(instr, r, cg.executionweight);
    

But for MOV instruction there is specific method that is used 
    
    
             TRgObj.add_move_instruction(instr:Taicpu);
    

Also weight is provided to determine which register should be spilled and which is better to use real cpu register. 

### Generating of real registers

At the end when all the assembler instructions are generated we call 
    
    
           do_register_allocation(list: TAsmList; headerTai: TAi)
    

It calculates real registers for the imaginary ones. 

Some instructions can use only certain types of registers. So, the register allocator can not choose from all the Cpu registers. For these instructions we notify the register allocator by calling the method: 
    
    
    TRgObj.add_Edge
    

These interferences are build during the register allocation. Every descendent that wants to add some interferences overrides the method 
    
    
    procedure TRgObj.add_cpu_interferences(p: TAi); virtual;
    

We have descendents of TRgObj for the different kind of processors. They call add_edge in add_cpu_interferences depending on the specific CPU architecture. It is used for Arm processors, x86 processors etc. 

In method tcgprocinfo.generate_code which is located in unit psub.pas is implemented the creation of register allocators, getting the registers, generation of real registers. This method is central place for code generation which includes the register allocation. Inside it you can find call to hlcg.init_register_allocators to create the Register Allocators call to cg.do_register_allocation(aktproccode,headertai); - to do the actual allocation also to hlcg.done_register_allocators; - to free the allocators 

## Tips

You can compile your project by using the -sr switch. This will leave the immaginary register names in the generated .s file 

## Calls hierarchy for the public methods
    
    
    Public
    
    
    constructor
    
    destructor
    
    do_register_allocation - level 1 is ordered by calling
      insert_regalloc_info_all
      generate_interference_graph
        add_edges_used(1, 2)			get_alias						add_edge
          add_edge												ibitmap.s
      prepare_colouring
        make_work_list				ri_coalesced
          sort_simplify_worklist
      colour_registers
        simplify					ri_coalesced
          decrement_degree				ri_coalesced
        coalesce					get_alias		simplifyworklist.add(v);	ibitmap
          conservative				ri_coalesced
          adjacent_ok				ri_coalesced						ibitmap
          add_worklist							simplifyworklist.add(u);
          combine												ibitmap, add_edge
            enable_moves
            decrement_degree			ri_coalesced		simplifyworklist.add(m)
            add_edge											ibitmap.s
        freeze
          freeze_moves(, 2)				get_alias(3)
        select_spill
          freeze_moves(, 2)				get_alias(3)
        assign_colours(, 2)				get_alias
      epilogue_colouring		Destroys the objects used during the coloring - worklist_moves, active_moves, frozen_moves, coalesced_moves, constrained_moves, reginfo.movelist
      spill_registers
        clear_interferences											ibitmap.s
        instr_spill_register			get_alias
          getregisterinline
            add_edges_used(2, 2)			get_alias
    	  add_edge											ibitmap.s
          ungetregisterinline
          get_spill_subreg
          do_spill_replace
          do_spill_read
          do_spill_written
      translate_registers
        assign_colours(, 2)				get_alias
    
    getregister
    
    add_move_instruction
      add_to_movelist
    
    combine						ri_coalesced(s)
      enable_moves
      decrement_degree				ri_coalesced
    
    -----------------------------------------
    Properties
    
    live_range_direction
      set_live_range_direction
    
    live_start
      get_live_start
      set_live_start
    
    live_end
      get_live_end
      set_live_end
    

## Class TRgObj

### Description

Main class for register allocation in FPC 

### Public methods

### Protected methods

### Private methods

## Imaginary registers

### Storing imaginary registers

TRgObj stores the imaginary registers in list of TRegInfo structure. 

### Life of imaginary register

Life of imaginary register starts when it is first used by assembler instruction. When it first appears in an assembler instruction in the assembler list for the specific routine. And ends when it is last used in an assembler instruction. In the TRegInfo structure for the specific imaginary register there are 2 fields for the life of the register. live_start: TAi; indicates which is the assembler instruction where the register is first used. live_end: TAi; indicates which is the assembler instruction where the register is last used. 

## Interferences

Interferences 

Call hierarchy 
    
    
     generate_interference_graph
       add_edges_used(1, 2)			get_alias						add_edge
         add_edge												ibitmap.s
    

  


### generate_interference_graph

This method iterates the assembler list. Looking for reg_alloc instruction, with the proper regType. 

During the iteration we keep all register that are live in list - live_registers. if its allocating it adds the superregister to the live_registers. if its dealloc it removes it from the live_registers. 

After adding or removing in live_registers it calls add_edges_used 

### add_edges_used

Iterates all live_registers and calls add_edge it gets the alias of the current live_register. and the super register u that is passed to the method. 

### add_edge

When we add edge we keep information on 2 places. One is in the interferenceBitmap And the other is in the adjList of the register information in 
    
    
    reginfo[u].adjList.add(v);
    reginfo[v].adjList.add(u);
    ibitmap[v, u] := true;
    ibitmap[u, v] := true;
    

In the relation between these 2 registers there is no dependant. There is no primary one. They are both equal in it. 

In reginfo you access the information by index. And in bitmap you do the same. This is prity much the same as speed. Why we have this duplicate information on 2 places. 

Interferences work with super registers. 

## Backgrounds

### Grouping the instructions

More abstract look at the CPU instructions shows us that we have mainly few kinds of instructions. \- The first group contains instructions that changes some data. This data remains on the same place but it is changed. Also do not need extra data ot perform the change Some instructions like these are inc, dec, CLC, CLD 

\- Second group are instructions that move date from one place to another. Such as MOV, LAHF, POP, PUSH, LEA 

\- Third group of instructions are those to compare data. They do not change the data. just check it. Such instructions are TEST, COMPARE 

\- Fourth group of instructions are those who change the flow of the execution of the instructions such as: JMP, Jcc, JCXZ, 

\- Fifth group are instructions that changes data based on other data like ADD 

Another grouping of instructions could be by the cpu version where the instruction first appear. 

Another grouping of instructions could be based on the registers that can be used in the instruction. Some instructions can use only specific registers for the address - mov ax, [bx + si + 1000] You can choose between for example only these registers - BP, BX, SI and DI. Some instructions can use only registers, some addresses, some both. Some registers have subregisters some do not. 

### Data in the instructions

Almost all of the instructions operate on some data. Also uses some data to perform operation. The data can be in the cpu registers, memory, cpu flags. 

### Programming using the instructions

The instructions are independant one from other. However when a program is created there are some logical dependencies that programmer introduce. Programmer wants a few modifications to be performed on some data. Which can be performed by few instructions executed one after another. And at the end the needed data will be produced. But data is not exactly Cpu register, or memory address. Data is something the the programmer cares about. For example, we have some starting data that equals 1000. we can put it in cpu register we want to add to it 200. We can use Asm instruction add, but then we can move the value that we reached in another register. And then to increment it by 300 at the end we will have the needed value 1500. But we used two registers for one date. Normally we don't have to use two registers. We can do it using one. Cause we have to move the data to second register somewhere in the middle. And this is extra instruction. However more than one data is parallel processed. And sometimes we can use this extra move. Just to speed up executions on other data. This is normally needed when to datas are produced that will calculate third one. 

This is not very important for the register allocator. It is more dedicated to the code generator. Code generator could decide where to add extra instruction, to speed other executions. It look like register allocator should not care about this. But it's good to thing about it. 

### Object Design guidelines

In the current implementation the getting of the registers and the allocation are mixed together. Also they share same structures, same class. But the data for each of them is used on the completely different phases. Mixing them togather brings more code on one place. Also its getting difficult to separate them when you work on them. They can be easily devided without lost of speed. Also different types of register allocations can be used. The getting of the registers and setting the weights, also life … can be on one place. But at the end different allocators can process this data. It is difficult for further develop the register allocation cause many phases and logic was brought together on one place. 

In the 
    
    
    TRgObj.colour_registers
    

method different approach can be performed. 

Entering different states can be implemented. Instead checking the counts of all lists. It will be faster. Clear code. Easy for debugging. Easy to develop further. 

Improvements can be made in the weights of the instructions. Or in the way the allocator decides which register to spill and which not.

---

_Source: [https://wiki.freepascal.org/The_register_allocator](https://web.archive.org/web/20240302030905/https://wiki.freepascal.org/The_register_allocator)_
