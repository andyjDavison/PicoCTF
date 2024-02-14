# unpackme

## Description
Can you get the flag? Reverse engineer this binary.

## Challenge Information
**Point Value:** 300  
**Category:** Reverse Engineering

## Hints
1. What is UPX?

## Solution
We are given a binary executable, except we can't execute it. We figure out that it is packed by upx and so we run: ```$ upx -d unpackme-upx -o unpacked```. This gives a runnable executable file, and if we run ```$ ./unpacked```. This asks us for the program's favorite number and we input 1, which is wrong. Let's take a look at the source. We run ```$ gdb unpacked``` and then ```(gdb) disassemble main``` and we get: 
```s
Dump of assembler code for function main:
   0x0000000000401e43 <+0>:     endbr64 
   0x0000000000401e47 <+4>:     push   %rbp
   0x0000000000401e48 <+5>:     mov    %rsp,%rbp
   0x0000000000401e4b <+8>:     sub    $0x50,%rsp
   0x0000000000401e4f <+12>:    mov    %edi,-0x44(%rbp)
   0x0000000000401e52 <+15>:    mov    %rsi,-0x50(%rbp)
   0x0000000000401e56 <+19>:    mov    %fs:0x28,%rax
   0x0000000000401e5f <+28>:    mov    %rax,-0x8(%rbp)
   0x0000000000401e63 <+32>:    xor    %eax,%eax
   0x0000000000401e65 <+34>:    movabs $0x4c75257240343a41,%rax
   0x0000000000401e6f <+44>:    movabs $0x30623e306b6d4146,%rdx
   0x0000000000401e79 <+54>:    mov    %rax,-0x30(%rbp)
   0x0000000000401e7d <+58>:    mov    %rdx,-0x28(%rbp)
   0x0000000000401e81 <+62>:    movabs $0x6865666430486637,%rax
   0x0000000000401e8b <+72>:    mov    %rax,-0x20(%rbp)
   0x0000000000401e8f <+76>:    movl   $0x36636433,-0x18(%rbp)
   0x0000000000401e96 <+83>:    movw   $0x4e,-0x14(%rbp)
   0x0000000000401e9c <+89>:    lea    0xb1161(%rip),%rdi        # 0x4b3004
   0x0000000000401ea3 <+96>:    mov    $0x0,%eax
   0x0000000000401ea8 <+101>:   call   0x410ba0 <printf>
   0x0000000000401ead <+106>:   lea    -0x3c(%rbp),%rax
   0x0000000000401eb1 <+110>:   mov    %rax,%rsi
   0x0000000000401eb4 <+113>:   lea    0xb1165(%rip),%rdi        # 0x4b3020
   0x0000000000401ebb <+120>:   mov    $0x0,%eax
   0x0000000000401ec0 <+125>:   call   0x410d30 <__isoc99_scanf>
   0x0000000000401ec5 <+130>:   mov    -0x3c(%rbp),%eax
   0x0000000000401ec8 <+133>:   cmp    $0xb83cb,%eax
   0x0000000000401ecd <+138>:   jne    0x401f12 <main+207>
   0x0000000000401ecf <+140>:   lea    -0x30(%rbp),%rax
   0x0000000000401ed3 <+144>:   mov    %rax,%rsi
   0x0000000000401ed6 <+147>:   mov    $0x0,%edi
   0x0000000000401edb <+152>:   call   0x401d85 <rotate_encrypt>
   0x0000000000401ee0 <+157>:   mov    %rax,-0x38(%rbp)
   0x0000000000401ee4 <+161>:   mov    0xdd7e5(%rip),%rdx        # 0x4df6d0 <stdout>
   0x0000000000401eeb <+168>:   mov    -0x38(%rbp),%rax
   0x0000000000401eef <+172>:   mov    %rdx,%rsi
   0x0000000000401ef2 <+175>:   mov    %rax,%rdi
   0x0000000000401ef5 <+178>:   call   0x420980 <fputs>
   0x0000000000401efa <+183>:   mov    $0xa,%edi
   0x0000000000401eff <+188>:   call   0x420e20 <putchar>
   0x0000000000401f04 <+193>:   mov    -0x38(%rbp),%rax
   0x0000000000401f08 <+197>:   mov    %rax,%rdi
   0x0000000000401f0b <+200>:   call   0x42ec70 <free>
   0x0000000000401f10 <+205>:   jmp    0x401f1e <main+219>
   0x0000000000401f12 <+207>:   lea    0xb110a(%rip),%rdi        # 0x4b3023
   0x0000000000401f19 <+214>:   call   0x420c40 <puts>
   0x0000000000401f1e <+219>:   mov    $0x0,%eax
   0x0000000000401f23 <+224>:   mov    -0x8(%rbp),%rcx
   0x0000000000401f27 <+228>:   xor    %fs:0x28,%rcx
   0x0000000000401f30 <+237>:   je     0x401f37 <main+244>
   0x0000000000401f32 <+239>:   call   0x45cba0 <__stack_chk_fail_local>
   0x0000000000401f37 <+244>:   leave  
   0x0000000000401f38 <+245>:   ret  
```
Since we know the program was asking us for a number, we can look for a cmp statement, and we notice that we are comparing our number to ```$0xb83cb```. If we convert that to decimal, we get ```754635``` which, when inputed to the program gives us our flag.

## Flag
picoCTF{up><_m3_f7w_5769b54e}