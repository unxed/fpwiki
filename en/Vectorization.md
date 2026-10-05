# Vectorization

│ **English (en)** │ 

    _Editor's note:_ This page has been set up as a collaboration and design specification for the proposal to include vectorization support in the x86 and x86_64 variant of the [FPC](<FPC.md> "FPC") (using SSE or AVX to reduce the number of instructions, and hence the execution speed, required to encode functionality). Eventually, this page can be converted into a guide to the optimizer once vectorization is at least partially supported. —[CuriousKit](</index.php?title=User:CuriousKit&action=edit&redlink=1> "User:CuriousKit \(page does not exist\)") ([talk](</index.php?title=User_talk:CuriousKit&action=edit&redlink=1> "User talk:CuriousKit \(page does not exist\)")) 22:52, 11 December 2017 (CET)

## Vector types

As of 2017-12-12, only static arrays of [singles](<Single.md> "Single") or [doubles](<Double.md> "Double") are considered vector types, but they are unaligned unless enforced by additional compiler directives. [Dynamic arrays](<Dynamic_array.md> "Dynamic array") and [pointers](<Pointer.md> "Pointer") to singles or doubles are currently not considered to be vectors. 

## Loop optimization

Coupled with loop unrolling, it is possible to theoretically reduce the number of cycles by a factor of four or eight, depending on if SSE2 or AVX2 is used. It depends on the code contained within the loop, but if it utilizes only relatively simple arithmetic and pointer movement (e.g. reading and writing sequentially from/to an array), then it might be vectorized with relatively great ease. 

For example: 
    
    
    var
    	input0, input1, output: array of cardinal;
    	x: integer;
    
    begin
    	assert(length(input0) = length(input1));
    	SetLength(output, length(input0));
    	
    	for x := 0 to length(input0)-1 do
    	begin
    		output[x] := input0[x] * input1[x];
    	end;
    end;
    

While the size of the inputs is not immediately known, it can be seen that the array sizes do not change within the loop and hence `length(input0)` is constant. Visualizing how the compiler might evaluate the code, it could be internally changed to the following: 
    
    
    var
    	input0, input1, output: array of cardinal;
    	x, arrayLen: integer;
    
    begin
    	assert(length(input0) = length(input1));
    	SetLength(output, length(input0));
    	
    	arrayLen := length(input0) div 4;
    	
    	for x := 0 to arrayLen - 1 do
    	begin
    		output[4 * x + 0] := input0[4 * x + 0] * input1[4 * x + 0];
    		output[4 * x + 1] := input0[4 * x + 1] * input1[4 * x + 1];
    		output[4 * x + 2] := input0[4 * x + 2] * input1[4 * x + 2];
    		output[4 * x + 3] := input0[4 * x + 3] * input1[4 * x + 3];
    	end;
    	
    	// Handle leftover array entries individually
    	// (count = length(input0) mod 4).
    end;
    

As such, the four statements in the converted for-loop can be easily assembled into SSE2 opcodes (although `pmulld` is SSE4.1), even with potentially unaligned memory. For example: 
    
    
    	lea    r8,  output[0]           ; r8  := @output
    	lea    r9,  input0[0]           ; r9  := @input0
    	lea    r10, input1[0]           ; r10 := @input1
    	
    	mov    ecx, input0_len          ; Calculate length(input0), a function call, beforehand and store in ecx
    	xor    rbx, rbx                 ; rbx := 0 - this is our index array
    	
    	shr    ecx, 2                   ; Divide the length by 4, since we're processing 4 values at once.
    	jz     @loop_exit               ; Don't enter loop if the length is less than 4
    	
    @vectorized_loop:
    	movdqu xmm0, [r9  + rbx * 4]    ; Take 4 DWords from input0 and store in xmm0
    	pmulld xmm0, [r10 + rbx * 4]    ; xmm0 := xmm0[0..3] * (input1)^[0..3]
    	movdqu [r8 + rbx * 4], xmm0     ; Store result in output.
    	
    	inc    rbx                      ; increment index
    	dec    ecx
    	ja     @vectorized_loop         ; if ecx > 0 then repeat loop with incremented index
    	
    @loop_exit:
            ; Handle last few entries separately (count = input0_len mod 4)
    

Further optimizations can be achieved, for example, by performing two reads, multiplies and writes per loop cycle, taking advantage of the fact that modern processors have more than one SIMD port to send instructions to. 

## See also

  * [Free Pascal Programmer's Guide, Chapter 5: “Intel MMX support”](<https://www.freepascal.org/docs-html/prog/progch5.html>)
  * [Unit `mmx`](<https://www.freepascal.org/docs-html/rtl/mmx/index.html>)

---

_Source: [https://wiki.freepascal.org/Vectorization](https://web.archive.org/web/20240226045238/https://wiki.freepascal.org/Vectorization)_
