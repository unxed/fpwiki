# ObjC Bool

## Contents

  * 1 Objective-C
    * 1.1 Source code
    * 1.2 Compile
  * 2 Disassembly x86_64
    * 2.1 No optimization
    * 2.2 O3



## Objective-C

### Source code

main.m 
    
    
    #import <Foundation/Foundation.h>
    
    @interface BoolTest: NSObject
      -(void) testcbool: (_Bool) avalue;
      -(void) testobjcbool: (BOOL) avalue;
    @end
    
    @implementation BoolTest
    -(void) testcbool: (_Bool) avalue
    {
        NSLog(@" cbool %s", avalue ? "true" : "false");
    }
    
    -(void) testobjcbool: (BOOL) avalue
    {
        NSLog(@" objcbool %s", avalue ? "true" : "false");
    }
    @end
    
    void simpleC(_Bool avalue)
    {
        NSLog(@" plainc %s", avalue ? "true" : "false");
        return;
    }
    
    int main(int argc, const char * argv[]) {
        @autoreleasepool {
            // insert code here...
            int a = 10;
            int b = 10;
            BoolTest* tt = [[BoolTest alloc] init];
            [tt testcbool:(a==b)];
            [tt testobjcbool:(a==b)];
            simpleC(a==b);
       }
        return 0;
    }
    

### Compile

You need to have Xcode command-line tools installed 
    
    
    clang  -framework Foundation -arch x86_64 main.m -o main
    

produces the executable. (The executable can be debugged and lldb can show the disassebly) 

In order to get assembly file from the compiler the following command-line could be used 
    
    
    clang  -framework Foundation -arch x86_64 main.m --assemble -o main.as
    

## Disassembly x86_64

Boolean parameters, no matter BOOL (signed char) or _Bool are always passed in the entire register 

### No optimization
    
    
        0x100000e5d <+93>:  movl   -0x14(%rbp), %edx
        0x100000e60 <+96>:  cmpl   -0x18(%rbp), %edx
        0x100000e63 <+99>:  sete   %r8b
        0x100000e67 <+103>: movq   0x31a(%rip), %rsi         ; "testcbool:"
        0x100000e6e <+110>: andb   $0x1, %r8b
    
        0x100000e72 <+114>: movzbl %r8b, %edx
    
        0x100000e76 <+118>: movq   0x183(%rip), %rcx         ; (void *)0x00007fff7e936e80: objc_msgSend
        0x100000e7d <+125>: movq   %rax, %rdi
        0x100000e80 <+128>: callq  *%rcx
        0x100000e82 <+130>: movq   -0x20(%rbp), %rax
    
        0x100000e86 <+134>: movl   -0x14(%rbp), %edx
        0x100000e89 <+137>: cmpl   -0x18(%rbp), %edx
        0x100000e8c <+140>: sete   %r8b
        0x100000e90 <+144>: andb   $0x1, %r8b
    
        0x100000e94 <+148>: movzbl %r8b, %edx
    
        0x100000e98 <+152>: movb   %dl, %r8b
        0x100000e9b <+155>: movq   0x2ee(%rip), %rsi         ; "testobjcbool:"
        0x100000ea2 <+162>: movq   %rax, %rdi
        0x100000ea5 <+165>: movsbl %r8b, %edx
        0x100000ea9 <+169>: callq  0x100000ef8               ; symbol stub for: objc_msgSend
    
    
        0x100000eae <+174>: movl   -0x14(%rbp), %edx
        0x100000eb1 <+177>: cmpl   -0x18(%rbp), %edx
        0x100000eb4 <+180>: sete   %r8b
        0x100000eb8 <+184>: andb   $0x1, %r8b
    
        0x100000ebc <+188>: movzbl %r8b, %edi
    
        0x100000ec0 <+192>: callq  0x100000db0               ; simpleC at main.m:29
    

### O3

As expected, O3 evaluates constant expressions and puts the end result into the code. Still boolean parameters are being written as long values into registers. 
    
    
        0x100000e97 <+55>:  movq   %rax, %rbx
        0x100000e9a <+58>:  movq   0x2df(%rip), %rsi         ; "testcbool:"
        0x100000ea1 <+65>:  movl   $0x1, %edx
        0x100000ea6 <+70>:  movq   %rbx, %rdi
        0x100000ea9 <+73>:  callq  *%r15
        0x100000eac <+76>:  movq   0x2d5(%rip), %rsi         ; "testobjcbool:"
        0x100000eb3 <+83>:  movl   $0x1, %edx
        0x100000eb8 <+88>:  movq   %rbx, %rdi
        0x100000ebb <+91>:  callq  *%r15
        0x100000ebe <+94>:  leaq   0x1b3(%rip), %rdi         ; @" plainc %s"
        0x100000ec5 <+101>: leaq   0x70(%rip), %rsi          ; "true"
        0x100000ecc <+108>: xorl   %eax, %eax
        0x100000ece <+110>: callq  0x100000ef2               ; symbol stub for: NSLog
        0x100000ed3 <+115>: movq   %rbx, %rdi

---

_Source: [https://wiki.freepascal.org/ObjC_Bool](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/ObjC_Bool)_
