# WebAssembly/Instructions

This page is created because it's annoying to constantly jump around the official WebAssembly documentation. 

The page requires careful review as official specs are updated 

## Instructions

Instruction  | Byte Code  | Execution  | Notes   
---|---|---|---  
**unreachable** | $00  | 

    1\. Trap
|   
**nop** | $01  | 

    1\. Do nothing
|   
**block** | $02 _blocktype_ | 

    1\. Let n be the arity |t?| of the result type t?.
    2\. Let L be the label whose arity is n and whose continuation is the end of the block.
    3\. Enter the block instr∗ with label L.
| **block** must always be closed with **end**  
**loop** | $03  |  |   
**if** | $04  |  |   
**else** | $05  |  | can only be used with **if**. Only single **else** is allowed per **if**  
(reserved)  | $06..$0A  |  |   
**end** | $0B  |  | **end** can only close **block** or **loop** clocks   
**br** _labelidx_ | $0C _labelidx_ | 

    1\. Assert: due to validation, the stack contains at least l+1 labels.
    2\. Let L be the l-th label appearing on the stack, starting from the top and counting from zero.
    3\. Let n be the arity of L.
    4\. Assert: due to validation, there are at least n values on the top of the stack.
    5\. Pop the values valn from the stack.
    6\. Repeat l+1 times: 

    a. While the top of the stack is a value, do: 

    i. Pop the value from the stack.
    b. Assert: due to validation, the top of the stack now is a label.
    c. Pop the label from the stack.
    7\. Push the values valn to the stack.
    8\. Jump to the continuation of L.
| Branching - Unconditional Branching is possible only within a block. The block itself defines labels where the branching can go to.   
**br_if** _labelidx_ | $0D _labelidx_ | 

    1\. Assert: due to validation, a value of value type i32 is on the top of the stack.
    2\. Pop the value i32.const c from the stack.
    3\. If c is non-zero, then: 

    a. Execute the instruction (br l).
    4\. Else: 

    b. Do nothing.
| Branching - Conditional Branching is possible only within a block. The block itself defines labels where the branching can go to.   
**br_table** _vec(labelidxI)_ _labelidx_ | $0E  |  |   
**return** | $0F  |  |   
**call** _funcidx_ | $10  | 

    1\. Let F be the current frame.
    2\. Assert: due to validation, F.module.funcaddrs[x] exists.
    3\. Let a be the function address F.module.funcaddrs[x].
    4\. Invoke the function instance at address a.
|   
**call_indirect** _typeuse_ | $11  | 

    1\. Let F be the current frame.
    2\. Assert: due to validation, F.module.tableaddrs[0] exists.
    3\. Let ta be the table address F.module.tableaddrs[0].
    4\. Assert: due to validation, S.tables[ta] exists.
    5\. Let tab be the table instance S.tables[ta].
    6\. Assert: due to validation, F.module.types[x] exists.
    7\. Let ftexpect be the function type F.module.types[x].
    8\. Assert: due to validation, a value with value type i32 is on the top of the stack.
    9\. Pop the value i32.const i from the stack.
    10\. If i is not smaller than the length of tab.elem, then: 

    a. Trap.
    11\. If tab.elem[i] is uninitialized, then: 

    a. Trap.
    12\. Let a be the function address tab.elem[i].
    13\. Assert: due to validation, S.funcs[a] exists.
    14\. Let f be the function instance S.funcs[a].
    15\. Let ftactual be the function type f.type.
    16\. If ftactual and ftexpect differ, then: 

    a. Trap.
    17\. Invoke the function instance at address a.
| Note that **call_indirect** text syntax is different: 
    
    
    call_indirect (type x)
    

A plain numeric value is treated as table number, not function signature. The signature can also be explicitly specified 
    
    
    call_indirect (param i32)(param f32)(result f64)
    

In this case the assembly would assign the proper type.   
(reserved)  | $12..$19  |  |   
**drop** | $1A  | [t]→[]  |   
**select** | $1B  | [t t i32]→[t]  |   
(reserved)  | $1C..$1F  |  |   
**local.get** x  | $20  |  |   
**local.set** x  | $21  |  |   
**local.tee** x  | $22  |  |   
**global.get** x  | $23  |  |   
**global.set** x  | $24  |  |   
(reserved)  | $25..$27  |  |   
**i32.load** memarg  | $28  |  **t**.load **memarg** and **t**.load**N** _**sx** **memarg**

    1\. Let F be the current frame.
    2\. Assert: due to validation, F.module.memaddrs[0] exists.
    3\. Let a be the memory address F.module.memaddrs[0].
    4\. Assert: due to validation, S.mems[a] exists.
    5\. Let mem be the memory instance S.mems[a].
    6\. Assert: due to validation, a value of value type i32 is on the top of the stack.
    7\. Pop the value i32.const i from the stack.
    8\. Let ea be the integer i+memarg.offset.
    9\. If N is not part of the instruction, then: 

    a. Let N be the bit width |t| of value type t.
    10\. If ea+N/8 is larger than the length of mem.data, then: 

    a. Trap.
    11\. Let b∗ be the byte sequence mem.data[ea:N/8].
    12\. If N and sx are part of the instruction, then: 

    a. Let n be the integer for which bytesiN(n)=b∗.
    b. Let c be the result of computing extend_sxN,|t|(n).
    13\. Else: 

    a. Let c be the constant for which bytest(c)=b∗.
    14\. Push the value t.const c to the stack.
|   
**i64.load** memarg  | $29  |   
**f32.load** memarg  | $2A  |   
**f64.load** memarg  | $2B  |   
**i32.load8_s** memarg  | $2C  |   
**i32.load8_u** memarg  | $2D  |   
**i32.load16_s** memarg  | $2E  |   
**i32.load16_u** memarg  | $2F  |   
**i64.load8_s** memarg  | $30  |   
**i64.load8_u** memarg  | $31  |   
**i64.load16_s** memarg  | $32  |   
**i64.load16_u** memarg  | $33  |   
**i64.load32_s** memarg  | $34  |   
**i64.load32_u** memarg  | $35  |   
**i32.store** memarg  | $36  |  **t**.store **memarg** and **t**.store**N** **memarg**

    1\. Let F be the current frame.
    2\. Assert: due to validation, F.module.memaddrs[0] exists.
    3\. Let a be the memory address F.module.memaddrs[0].
    4\. Assert: due to validation, S.mems[a] exists.
    5\. Let mem be the memory instance S.mems[a].
    5\. Assert: due to validation, a value of value type t is on the top of the stack.
    7\. Pop the value t.const c from the stack.
    8\. Assert: due to validation, a value of value type i32 is on the top of the stack.
    9\. Pop the value i32.const i from the stack.
    10\. Let ea be the integer i+memarg.offset.
    11\. If N is not part of the instruction, then: 

    a. Let N be the bit width |t| of value type t.
    12\. If ea+N/8 is larger than the length of mem.data, then: 

    a. Trap.
    13\. If N is part of the instruction, then: 

    a. Let n be the result of computing wrap|t|,N(c).
    b. Let b∗ be the byte sequence bytesiN(n).
    14\. Else: 

    a. Let b∗ be the byte sequence bytest(c).
    15\. Replace the bytes mem.data[ea:N/8] with b∗.
|   
**i64.store** memarg  | $37  |   
**f32.store** memarg  | $38  |   
**f64.store** memarg  | $39  |   
**i32.store8** memarg  | $3A  |   
**i32.store16** memarg  | $3B  |   
**i64.store8** memarg  | $3C  |   
**i64.store16** memarg  | $3D  |   
**i64.store32** memarg  | $3E  |   
**memory.size** | $3F  |  |   
**memory.grow** | $40  |  |   
**i32.const** i32  | $41  |  |   
**i64.const** i64  | $42  |  |   
**f32.const** f32  | $43  |  |   
**f64.const** f64  | $44  |  |   
**i32.eqz** | $45  |  | Return 1 if i is zero, 0 otherwise.   
**i32.eq** | $46  |  | Return 1 if i1 equals i2, 0 otherwise.   
**i32.ne** | $47  |  | Return 1 if i1 does not equal i2, 0 otherwise.   
**i32.lt_s** | $48  |  | Let j1 be the signed interpretation of i1. Let j2 be the signed interpretation of i2. Return 1 if j1 is less than j2, 0 otherwise.   
**i32.lt_u** | $49  |  | Return 1 if i1 is less than i2, 0 otherwise.   
**i32.gt_s** | $4A  |  |   
**i32.gt_u** | $4B  |  |   
**i32.le_s** | $4C  |  |   
**i32.le_u** | $4D  |  |   
**i32.ge_s** | $4E  |  |   
**i32.ge_u** | $4F  |  |   
**i64.eqz** | $50  |  | Return 1 if i is zero, 0 otherwise.   
**i64.eq** | $51  |  | Return 1 if i1 equals i2, 0 otherwise.   
**i64.ne** | $52  |  | Return 1 if i1 does not equal i2, 0 otherwise.   
**i64.lt_s** | $53  |  | Let j1 be the signed interpretation of i1. Let j2 be the signed interpretation of i2. Return 1 if j1 is less than j2, 0 otherwise.   
**i64.lt_u** | $54  |  | Return 1 if i1 is less than i2, 0 otherwise.   
**i64.gt_s** | $55  |  |   
**i64.gt_u** | $56  |  |   
**i64.le_s** | $57  |  |   
**i64.le_u** | $58  |  |   
**i64.ge_s** | $59  |  |   
**i64.ge_u** | $5A  |  |   
**f32.eq** | $5B  |  | Return 1 if i1 equals i2, 0 otherwise.   
**f32.ne** | $5C  |  | Return 1 if i1 does not equal i2, 0 otherwise.   
**f32.lt** | $5D  |  |   
**f32.gt** | $5E  |  |   
**f32.le** | $5F  |  |   
**f32.ge** | $60  |  |   
**f64.eq** | $61  |  | Return 1 if i1 equals i2, 0 otherwise.   
**f64.ne** | $62  |  | Return 1 if i1 does not equal i2, 0 otherwise.   
**f64.lt** | $63  |  |   
**f64.gt** | $64  |  |   
**f64.le** | $65  |  |   
**f64.ge** | $66  |  |   
**i32.clz** | $67  |  |   
**i32.ctz** | $68  |  |   
**i32.popcnt** | $69  |  |   
**i32.add** | $6A  |  |   
**i32.sub** | $6B  |  |   
**i32.mul** | $6C  |  |   
**i32.div_s** | $6D  |  |   
**i32.div_u** | $6E  |  |   
**i32.rem_s** | $6F  |  |   
**i32.rem_u** | $70  |  |   
**i32.and** | $71  |  |   
**i32.or** | $72  |  |   
**i32.xor** | $73  |  |   
**i32.shl** | $74  |  |   
**i32.shr_s** | $75  |  |   
**i32.shr_u** | $76  |  |   
**i32.rotl** | $77  |  |   
**i32.rotr** | $78  |  |   
**i64.clz** | $79  |  |   
**i64.ctz** | $7A  |  |   
**i64.popcnt** | $7B  |  |   
**i64.add** | $7C  |  |   
**i64.sub** | $7D  |  |   
**i64.mul** | $7E  |  |   
**i64.div_s** | $7F  |  |   
**i64.div_u** | $80  |  |   
**i64.rem_s** | $81  |  |   
**i64.rem_u** | $82  |  |   
**i64.and** | $83  |  |   
**i64.or** | $84  |  |   
**i64.xor** | $85  |  |   
**i64.shl** | $86  |  |   
**i64.shr_s** | $87  |  |   
**i64.shr_u** | $88  |  |   
**i64.rotl** | $89  |  |   
**i64.rotr** | $8A  |  |   
**f32.abs** | $8B  |  |   
**f32.neg** | $8C  |  |   
**f32.ceil** | $8D  |  |   
**f32.floor** | $8E  |  |   
**f32.trunc** | $8F  |  |   
**f32.nearest** | $90  |  |   
**f32.sqrt** | $91  |  |   
**f32.add** | $92  |  |   
**f32.sub** | $93  |  |   
**f32.mul** | $94  |  |   
**f32.div** | $95  |  |   
**f32.min** | $96  |  |   
**f32.max** | $97  |  |   
**f32.copysign** | $98  |  |   
**f64.abs** | $99  |  |   
**f64.neg** | $9A  |  |   
**f64.ceil** | $9B  |  |   
**f64.floor** | $9C  |  |   
**f64.trunc** | $9D  |  |   
**f64.nearest** | $9E  |  |   
**f64.sqrt** | $9F  |  |   
**f64.add** | $A0  |  |   
**f64.sub** | $A1  |  |   
**f64.mul** | $A2  |  |   
**f64.div** | $A3  |  |   
**f64.min** | $A4  |  |   
**f64.max** | $A5  |  |   
**f64.copysign** | $A6  |  |   
**i32.wrap_i64** | $A7  |  |   
**i32.trunc_f32_s** | $A8  |  |   
**i32.trunc_f32_u** | $A9  |  |   
**i32.trunc_f64_s** | $AA  |  |   
**i32.trunc_f64_u** | $AB  |  |   
**i64.extend_i32_s** | $AC  |  |   
**i64.extend_i32_u** | $AD  |  |   
**i64.trunc_f32_s** | $AE  |  |   
**i64.trunc_f32_u** | $AF  |  |   
**i64.trunc_f64_s** | $B0  |  |   
**i64.trunc_f64_u** | $B1  |  |   
**f32.convert_i32_s** | $B2  |  |   
**f32.convert_i32_u** | $B3  |  |   
**f32.convert_i64_s** | $B4  |  |   
**f32.convert_i64_u** | $B5  |  |   
**f32.demote_f64** | $B6  |  |   
**f64.convert_i32_s** | $B7  |  |   
**f64.convert_i32_u** | $B8  |  |   
**f64.convert_i64_s** | $B9  |  |   
**f64.convert_i64_u** | $BA  |  |   
**f64.promote_f32** | $BB  |  |   
**i32.reinterpret_f32** | $BC  |  |   
**i64.reinterpret_f64** | $BD  |  |   
**f32.reinterpret_i32** | $BE  |  |   
**f64.reinterpret_i64** | $BF  |  |   
  
## See Also

  * [WebAssembly](<../WebAssembly.md> "WebAssembly")
  * <http://webassembly.github.io/spec/core/intro/index.html> \- official documentation

---

_Source: [https://wiki.freepascal.org/WebAssembly/Instructions](https://web.archive.org/web/20250427104049/https://wiki.freepascal.org/WebAssembly/Instructions)_
