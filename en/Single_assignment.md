# Single assignment

* * *

**Single assignment** is an optimization technique that breaks the connection between variable names in a high level programming language and their location in the assembler code. 

The basic principle of single assigment is that each location is written only once. After that the location can only be read from. 

Demonstration: 

| 
    
    
    a:=x+y;
    b:=a-1;
    a:=y+b;
    b:=x*4;
    a:=a+b;
      
  
---  
...becomes...  |  | 
    
    
    loc1:=x+y;
    loc2:=loc1-1;
    loc3:=y+loc2;
    loc4:=x*4;
    loc5:=loc3+loc4;
      
  
---  
  
The register allocator or temp allocator will try to coalesce the locations and remove moves between them. 

## Contents

  * 1 Advantages of single assignment
    * 1.1 Very short live ranges
    * 1.2 Better use of special registers
    * 1.3 Optimizing variables away
  * 2 Using single assignment for temps instead of registers
  * 3 Technical obstacles implementing single assigment
  * 4 Specific challenges for Free Pascal to implement single assignment
  * 5 High level or low level



## Advantages of single assignment

Basically single assignment allows advanced optimizations. A few examples. 

### Very short live ranges

The live ranges of single assignment will be very short. For example for the example above: 
    
    
    loc1:=x+y;               Live: x,y
    loc2:=loc1-1;            Live: x,y,loc1
    loc3:=y+loc2;            Live: x,loc2
    loc4:=x*4;               Live: loc3,loc4
    loc5:=loc3+loc4;         Live: loc5
      
  
---  
  
The short live ranges ensure that the computation can be performed with a very low amount of registers. The untransformed code used 4 variables, which were all live at the same time. So therefore we would need 4 registers for the variables alone. For temporary value we would propably need one more register, so we would need 5 registers in total. 

For the single assignment form of the code, the register pressure is greatly reduced because of the short live ranges. We need no temporary registers, because the temporary register becomes the new location. Because only 3 locations are live at the same time, the computation can be peformed with only 3 registers. 

### Better use of special registers

For some code, the variables need to be stored in special registers, for example for: 

    c:=a shl b;
    ...
    e:=c shl d;

... variable b should be placed into ecx, but this is true for variable d as well. Normally, this would cause a conflict, and therefore a spill. This can happen with single assignment as well, but, single assignment allows for allocation of b into another register later on in the, and therefore allows possibilities for both b and d to be allocated into ecx during the shift. 

### Optimizing variables away

| 
    
    
    a:=1;
    b:=a+1;
    c:=b*2;
      
  
---  
...becomes...  |  | 
    
    
    loc1:=1;              Live: []
    loc2:=loc1+1;         Live: Loc1
    loc3:=loc2*2;         Live: Loc2
                          Live: Loc3
      
  
---  
  
Location 1..3 do not conflict and can therefore be stored in the same register. The code might become: 

    mov eax,1
    inc eax
    shl eax,1

It can get even better, because nothing with single assignment prevents you from assigning a variable LOC_CONSTANT. I.e. the because loc1 is only written to once with a constant, you can simply "store" loc1 in LOC_CONSTANT. Because loc1 is constant, loc2 also can become LOC_CONSTANT. The code generator could therefore simply generate: 

    mov eax,4

## Using single assignment for temps instead of registers

The single assignment technique can also very well be used for local variables instead of registers and be used to reduce the amount copying between those variables. For example shortstrings are often copied around unnecessary. By applying single assignment to the code, temporary strings can be coalesced with actual string variables and the copies to them can therefore be optimizes away by the temp allocator. 

An example is the code "c:=c+'abcdefg';". The expression c+'abcdefg" is currently calculated in a temporary string which is later copied into the location of c again. Single assignment would result in different two locations for c, which can be coalesced into a single location, removing the need for an unnecessary string copy, and decrease the size of the stack frame. 

## Technical obstacles implementing single assigment

Program control instructions are an obstacle to the single assignment principle. For example: 
    
    
    1:       a:=0;
    2:       b:=0;
    3:       c:=0;
    4:   label1:
    5:       b:=a+1;
    6:       c:=c+b;
    7:       a:=b*2
    8:       if a<10 then
    9:         goto label1;
    10:      result:=c;
      
  
---  
  
Tranforming this program to single assignment form is problematic because the second assignment to "a" would move a in a different register than it is expected at label 1. 

To overcome this problem, "merge" functions are added to the label. Also called "phi" functions. Using a [flow graph](</index.php?title=flow_graph&action=edit&redlink=1> "flow graph \(page does not exist\)") a compiler can determine line 3 is reachable from both line 1 as line 9. 

It therefore known a will be assigned to a different location when coming from line 9 than when we are coming from line 1. Therefore a merge function is inserted for variable a for both locations. For easy understanding the locations are named after the variable they originate from in the following single assigment form of the code: 
    
    
    1:       a1:=0;
    2:       b1:=0;
    3:       c1:=0;
    4:   label1:
    5:       a3:=merge(from_line_1:a1,from_line_11:a2);
    6:       b2:=merge(from_line_1:b1,from_line_11:b3);
    7:       c3:=merge(from_line_1:c1,from_line_11:c2);
    8:       b3:=a3+1;
    9:       c2:=c3+b3;
    10:       a2:=b3*2
    12:      if a2<10 then
    13:        goto label1;
    14:      result:=c2;
      
  
---  
  
The merge function can be explained as follows: For example, when arriving from line 1, the merge function assigns a1 to a3, when arriving from line 11, it assigns a2 to a3. After this process, the code is further transformed during which the merge functions are removed: 
    
    
    1:       a1:=0;
    2:       b1:=0;
    3:       c1:=0;
    4:       a3:=a1;
    5:       b2:=b1;
    6:       c3:=c1;
    7:   label1:
    8:       b3:=a3+1;
    9:       c2:=c3+b3;
    10:       a2:=b3*2
    11:      if a2<10 then
    12:        begin
    13:          a3:=a2;
    14:          b2:=b3;
    15:          c3:=c2;
    16:          goto label1;
    17:        end;
    18:      result:=c2;
      
  
---  
  
This is a correct single assignment form of the code. Note that the merge function for b2 is unnecessary in the above code. If we would add merge functions recklessly, for complex code, our (internal) code would blow up a lot and therefore our register allocator would get a lot of work. Using a [flow graph](</index.php?title=flow_graph&action=edit&redlink=1> "flow graph \(page does not exist\)") we can determine exactly when a merge function is necessary. 

There should be a merge function for a variable "a" at a certain node "z" of the flow graph of the code, when all of the following critria are true: 

  * There exists a code block x containing a definition (a write to) a.
  * There exists a code block y, y<>x, containing a definition (a write to) a.
  * In the flow graph, there exists a non-empty path Pa from x to z.
  * In the flow graph, there exists a non-empty path Pb from y to z.
  * Pa and Pb do not have any node in the flow graph in common other than z
  * Node z does not appear within both Pa and Pb, prior to the end though it may appear in one or the other.



Using the above criteria, we can get a more efficient correct single assignment form: 
    
    
    1:       a1:=0;
    2:       b1:=0;
    3:       c1:=0;
    4:       a3:=a1;
    5:       c3:=c1;
    6:   label1:
    7:       b2:=a3+1;
    8:       c2:=c3+b2;
    9:       a2:=b2*2
    10:      if a2<10 then
    11:        begin
    12:          a3:=a2;
    13:          c3:=c2;
    14:          goto label1;
    15:        end;
    16:      result:=c2;
      
  
---  
  
## Specific challenges for Free Pascal to implement single assignment

  * Free Pascal does not generate a flow graph. Generating a flow graph is not trivial with the current node tree design, because generating it needs traversing a code tree bottom up. This could be solved by turning the blockn into an array or double linked list instead of a binary tree.
  * Free Pascal gives control statements their own nodes instead of converting control statements to goto. This problem is minor and while it will make generating the flow graph more complex, the current approach can be kept.



## High level or low level

Single assignment can be done at high level, converting a node tree to single assignment form, as at low level, generating assembler code in single assignment form. While the examples above have been written in a high level form, it makes more sense for Free Pascal do generate code in single assignment form right away.

---

_Source: [https://wiki.freepascal.org/Single_assignment](https://web.archive.org/web/20250326004704/https://wiki.freepascal.org/Single_assignment)_
